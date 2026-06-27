Paste this whole thing into Claude Code.

---

There's a bug in the mobile hamburger menu on the PHRF site. The menu does not open when the hamburger button is clicked. I already did a lot of diagnosis live in the browser and want you to pick up from there instead of starting over.

**The relevant code** (this same pattern repeats at the bottom of every page file: PHRF_Full_Site.html, about.html, a-word-from-our-founder.html, contact.html, get-help.html, PHRF-Youth-Program.html, programs.html):

```html
<button type="button" class="phrf-hamburger-btn" id="phrf-hamburger" aria-label="Toggle navigation">
  <span></span>
  <span></span>
  <span></span>
</button>
```

```js
function phrfToggleNav() {
  var menu = document.getElementById('phrf-mobile-menu');
  if (menu) menu.classList.toggle('open');
}
function phrfCloseNav() {
  var menu = document.getElementById('phrf-mobile-menu');
  if (menu) menu.classList.remove('open');
}
document.addEventListener('DOMContentLoaded', function() {
  var btn = document.getElementById('phrf-hamburger');
  if (btn && !btn._phrfBound) { btn._phrfBound = true; btn.addEventListener('click', phrfToggleNav); }
});
```

**What I confirmed on the live staging site (https://6kh.a6e.myftpupload.com), tested both logged into WordPress and logged out as a real anonymous visitor, same result both times:**

- Calling `phrfToggleNav()` directly in the console works perfectly, the menu's class toggles to `phrf-mobile-nav open` as expected.
- Calling `document.getElementById('phrf-hamburger').click()` does NOT trigger the menu to open, even though `btn._phrfBound` is `true`, meaning the binding code believes it successfully attached the listener.
- Dispatching a real `MouseEvent('click', {bubbles:true, cancelable:true})` on the button also does nothing.
- Adding a brand new `addEventListener('click', phrfToggleNav)` on that same button, right then in the console, still does NOT fire on click.
- Replacing the binding entirely with an inline `onclick="phrfToggleNav()"` attribute and clicking still does NOT fire it.
- But calling `btn.onclick.call(btn)` directly (bypassing the browser's event dispatch, just invoking the function) DOES work.
- A capturing listener I added on `document` itself DOES see the click event arrive (`e.target.id === 'phrf-hamburger'` fires), so the click event is dispatching and bubbling/capturing through the DOM somewhere, it's just never reaching this button's own handler.

**What I ruled out:**
- Duplicate IDs: I initially thought there were two `id="phrf-hamburger"` elements, but that was a false read caused by substring overlap with `phrf-hamburger-btn` in my own search. A precise regex confirmed there is exactly one element with that ID.
- `disabled` attribute: false.
- `pointer-events`: computed style is `auto`, not `none`.
- `stopPropagation()` / `stopImmediatePropagation()`: zero occurrences anywhere in this page's own script.
- WordPress admin bar / logged-in-only scripts: tested fully logged out as a real visitor, bug still reproduces identically.

**My working theory, unconfirmed:** something is intercepting or neutralizing the click before it reaches this specific button's own handlers, most likely a capturing-phase listener on some ancestor element (possibly from Elementor's own frontend.js, or some other global script on the page) that calls `stopPropagation()` under certain conditions, but I could not locate it through static code search or page-level JavaScript probing. This needs actual DevTools (Elements panel → select the button → Event Listeners tab) to see everything actually registered on it and its ancestors, which I can't do through a scripted browser session.

**What I need you to do:**
1. Open the live staging site in a real browser with DevTools, inspect the hamburger button, and check the Event Listeners panel for the button itself and its ancestors (`.phrf-nav-right`, `nav.nav`, the Elementor wrapper divs) to find anything calling `stopPropagation` or otherwise intercepting the click.
2. Once found, fix it so the hamburger button's click reliably opens/closes `#phrf-mobile-menu`.
3. As a resilience improvement regardless of root cause: consider replacing the direct binding with event delegation, e.g. a single `document.addEventListener('click', function(e) { if (e.target.closest('#phrf-hamburger')) phrfToggleNav(); })`, which is more robust against whatever timing or interception issue is happening here.
4. Apply the fix consistently across all seven page files, since they all share this exact same script block.
5. After fixing, verify by actually clicking the button in a real mobile viewport (or DevTools device toolbar), not just checking that the code looks right, since that's exactly the gap that let this bug through originally.
