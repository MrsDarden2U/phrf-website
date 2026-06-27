# PHRF Website — Bulk QA Fix Prompt for Claude Code

You are working on 7 standalone HTML files for the PHRF Charities (The Potter's House Relief Fund) website. The files are:

- `PHRF_Full_Site.html` (homepage)
- `about.html`
- `a-word-from-our-founder.html`
- `contact.html`
- `get-help.html`
- `PHRF-Youth-Program.html`
- `programs.html`

Apply ALL of the following fixes. Work through them one file at a time. Do not change any content, copy, colors, fonts, layout, or styling beyond what is explicitly specified below.

---

## FIX 1 — Staging domain replacement (ALL 7 files)

Find every instance of `6kh.a6e.myftpupload.com` and replace it with `phrfcharities.org`.

This includes occurrences in:
- `href` attributes on `<a>` tags
- `src` attributes on `<img>` tags
- `url(...)` values inside inline CSS (hero background images)
- Any hidden input `value` attributes

After replacing, do a final check on each file to confirm zero remaining instances of `6kh.a6e.myftpupload.com`.

---

## FIX 2 — Standardize the footer across all 7 files

Every page's footer has two link columns: **Programs** and **Organization**. Replace both columns on all 7 files with the following identical markup. Do not change the footer brand block (org name, tagline, 501c3 line, EIN line) or the footer-bottom copyright line — only replace the two `<div class="footer-col">` blocks.

**Programs column (use this exact markup on all 7 files):**
```html
<div class="footer-col">
  <h4>Programs</h4>
  <a href="https://phrfcharities.org/programs">Financial assistance</a>
  <a href="https://phrfcharities.org/programs">Job training</a>
  <a href="https://phrfcharities.org/programs">Mentoring</a>
  <a href="https://phrfcharities.org/programs">Academic tutoring</a>
  <a href="https://phrfcharities.org/programs">College grant support</a>
  <a href="https://phrfcharities.org/phrf-youth-program">Youth Program</a>
</div>
```

**Organization column (use this exact markup on all 7 files):**
```html
<div class="footer-col">
  <h4>Organization</h4>
  <a href="https://phrfcharities.org/about">About</a>
  <a href="https://phrfcharities.org/a-word-from-our-founder">Founder's story</a>
  <a href="https://phrfcharities.org/get-help">Get help</a>
  <a href="https://givebutter.com/phrf-campaign-1-o5jbbu" target="_blank" rel="noopener">Donate</a>
  <a href="https://phrfcharities.org/contact">Contact</a>
  <a href="mailto:contact@phrfcharities.org">contact@phrfcharities.org</a>
</div>
```

---

## FIX 3 — Add EIN line to footer brand block on about.html and PHRF-Youth-Program.html

In `about.html` and `PHRF-Youth-Program.html` only, find the footer brand block. It will contain a `<p>` tag with the 501(c)(3) description. Immediately after that paragraph, add:

```html
<p style="margin-top:8px;font-size:11px;color:#5a8fa0;">EIN furnished upon request.</p>
```

Do not add this line to the other 5 files — it is already present there.

---

## FIX 4 — Add guardian email field to youth enrollment form (PHRF-Youth-Program.html only)

In `PHRF-Youth-Program.html`, find the enrollment form (`id="youthForm"`). Locate the parent/guardian information section. After the guardian phone `<div class="form-group">` block, add the following new field:

```html
<div class="form-group">
  <label>Parent / guardian email</label>
  <input type="email" name="guardian_email" placeholder="your@email.com" required>
</div>
```

---

## FIX 5 — Replace state text input with a select dropdown (PHRF-Youth-Program.html only)

In `PHRF-Youth-Program.html`, find the address state input:
```html
<input type="text" name="address_state" placeholder="OH / KY / GA">
```

Replace it with:
```html
<select name="address_state" required>
  <option value="" disabled selected>Select state</option>
  <option value="OH">Ohio</option>
  <option value="KY">Kentucky</option>
  <option value="GA">Georgia</option>
</select>
```

---

## FIX 6 — Add for/id pairs to enrollment form labels (PHRF-Youth-Program.html only)

In `PHRF-Youth-Program.html`, the enrollment form labels are not linked to their inputs via `for`/`id`. Add `id` attributes to each input and matching `for` attributes to each label. Use these id values:

- Youth's full name → `id="youth_name"`, label `for="youth_name"`
- Youth's phone number → `id="youth_phone"`, label `for="youth_phone"`
- Parent / guardian name → `id="guardian_name"`, label `for="guardian_name"`
- Parent / guardian phone → `id="guardian_phone"`, label `for="guardian_phone"`
- Parent / guardian email (new field from Fix 4) → `id="guardian_email"`, label `for="guardian_email"`
- Street address → `id="address_street"`, label `for="address_street"`
- City → `id="address_city"`, label `for="address_city"`
- State → `id="address_state"`, label `for="address_state"` (this is now a select from Fix 5)
- Message → `id="youth_message"`, label `for="youth_message"` (also add `id="youth_message"` to the textarea)

---

## FIX 7 — Add redirect fields to the two forms that are missing them

**PHRF-Youth-Program.html** — In the youthForm, add this hidden input immediately after the existing `access_key` and `subject` hidden inputs:
```html
<input type="hidden" name="redirect" value="https://phrfcharities.org/phrf-youth-program">
```

**PHRF_Full_Site.html** — Find the claim form (`id="claimForm"`). Add this hidden input immediately after the existing `access_key` and `subject` hidden inputs:
```html
<input type="hidden" name="redirect" value="https://phrfcharities.org/#claim-reward">
```

Do not modify the redirect field already present in `contact.html`.

---

## FIX 8 — Fix nav dropdown to work on focus and touch (ALL 7 files)

In every file, find the nav dropdown CSS. It currently shows the dropdown only on `:hover`. Add `:focus-within` support so keyboard users and touch devices can access the "A Word from Our Founder" submenu.

Find this CSS rule (or its equivalent):
```css
.nav-dropdown:hover .nav-dropdown-menu { display: block; }
```

Replace it with:
```css
.nav-dropdown:hover .nav-dropdown-menu,
.nav-dropdown:focus-within .nav-dropdown-menu { display: block; }
```

Also add `tabindex="0"` to the dropdown trigger anchor tag so it can receive keyboard focus:
Find: `<a href="https://phrfcharities.org/about">About</a>` inside the `.nav-dropdown` div
Replace with: `<a href="https://phrfcharities.org/about" tabindex="0">About</a>`

---

## FIX 9 — Standardize response time to "1 to 2 business days" (PHRF-Youth-Program.html only)

In `PHRF-Youth-Program.html`, find both instances of "2–3 business days" and replace each with "1 to 2 business days".

---

## FIX 10 — Update Ohio city list (about.html and contact.html)

The correct Ohio service cities are: **Cincinnati, Dayton, Columbus**. Cleveland is incorrect and must be removed.

**about.html** — Find the "Where we show up" section. The Ohio list currently reads: Cincinnati, Dayton, Columbus. This is already correct — confirm it and leave it unchanged.

**contact.html** — Find the "Regional service areas" section. The Ohio list currently reads: Cincinnati, Columbus, Cleveland, Dayton. Remove Cleveland so it reads: Cincinnati, Columbus, Dayton. Match the order used in about.html: Cincinnati, Dayton, Columbus.

---

## FIX 11 — Update Georgia city list (about.html and contact.html)

The correct Georgia service cities are: **Atlanta, South Fulton, East Point, College Park**.

**about.html** — Find the "Where we show up" Georgia section. It currently lists: Atlanta, Athens, Macon, Savannah. Replace the entire Georgia city list with: Atlanta, South Fulton, East Point, College Park.

**contact.html** — Find the "Regional service areas" Georgia section. It currently lists: Atlanta, Athens, Macon, Savannah. Replace the entire Georgia city list with: Atlanta, South Fulton, East Point, College Park.

---

## FIX 12 — Update homepage stat bar city count (PHRF_Full_Site.html only)

In `PHRF_Full_Site.html`, find the stats bar on the homepage. It currently displays "9 cities" (or a number near 9). Update it to read **"10 cities"**.

The correct total is:
- Ohio: Cincinnati, Dayton, Columbus (3)
- Kentucky: Covington, Lexington, Louisville (3)
- Georgia: Atlanta, South Fulton, East Point, College Park (4)
- Total: 10 cities

---

## FOLLOW-UP FIXES — Round 2 (apply after Round 1 is confirmed)

### FIX 13 — Move EIN line back into the footer brand block (PHRF_Full_Site.html only)

The EIN disclosure line ("EIN furnished upon request.") is currently placed in the Sweet Rewards donate section instead of the footer brand block. This is incorrect — Fix 3 only specified adding it to about.html and PHRF-Youth-Program.html's footer brand block because it was already present in the footer brand block on the other 5 files, including PHRF_Full_Site.html. It should not have moved from there.

1. Remove the EIN line from wherever it currently appears in the Sweet Rewards / claim-reward section of PHRF_Full_Site.html.
2. Confirm the EIN line is present in the footer brand block of PHRF_Full_Site.html, in the same position and with the same styling used in contact.html, get-help.html, programs.html, and a-word-from-our-founder.html.
3. Confirm the footer brand block (org name, tagline, 501(c)(3) description, EIN line) is now identical across all 7 files.

### FIX 14 — Change goody bag fulfillment from coordinator distribution to direct-to-donor shipping (PHRF_Full_Site.html only)

Per Wayne's decision, goody bags will now ship directly to the donor rather than through program coordinators. Update the shipping note in the Sweet Rewards section.

Find this line:
```html
<p class="sr-shipping-note">Goody bags are assembled in Atlanta, GA and shipped to program coordinators in Ohio, Kentucky, and Georgia for distribution. Allow 2 to 3 weeks after donation confirmation for delivery.</p>
```

Replace it with:
```html
<p class="sr-shipping-note">Goody bags are shipped directly to your address by our fulfillment partner. Allow 2 to 3 weeks after donation confirmation for delivery.</p>
```

Note: this language is intentionally generic ("our fulfillment partner") rather than naming a specific distributor, since the vendor arrangement is still being finalized. Do not add a vendor name unless explicitly instructed to in a future round.

Also check the "Ready to choose your bag?" message block just above the donate button — it currently says:
```html
<p>Select your goody bag above then donate $20 or more using the button below. After your donation is confirmed we will reach out to collect your bag choice and shipping information.</p>
```
This line is still accurate for direct-to-donor shipping (it already says PHRF will collect shipping info from the donor), so leave it unchanged unless it references coordinators elsewhere — confirm it does not before leaving it as is.

Also check the claim form (`id="claimForm"`) for a shipping address field. If the form does not currently collect a mailing address, add one — direct-to-donor shipping requires it:
```html
<div class="form-group">
  <label>Shipping address *</label>
  <input type="text" name="shipping_address" placeholder="Street address, city, state, ZIP" required>
</div>
```

---

## ROUND 2 VERIFICATION CHECKLIST

1. EIN line appears in the footer brand block of PHRF_Full_Site.html and nowhere else on that page
2. Footer brand block is byte-for-byte identical across all 7 files
3. Shipping note on PHRF_Full_Site.html reads "shipped directly to your address" — no mention of program coordinators or distribution
4. Claim form on PHRF_Full_Site.html includes a shipping address field
5. No other reference to coordinator-based goody bag distribution remains anywhere in PHRF_Full_Site.html

---

## FOLLOW-UP FIXES — Round 3

### FIX 15 — Standardize the 4th program's name to "Academic Tutoring and College Grant Support" (ALL 7 files)

The 4th program currently has multiple different names across the site. Replace every variant with the exact canonical name: **"Academic Tutoring and College Grant Support"**

Find and replace each of the following with the canonical name, matching the capitalization of the surrounding context (sentence case if it's mid-sentence, title case if it's a heading or standalone label):

- `College Grant Support` → `Academic Tutoring and College Grant Support`
- `College grant assistance` → `Academic Tutoring and College Grant Support`
- `College grant support` → `Academic Tutoring and College Grant Support`
- `College grants` → `Academic Tutoring and College Grant Support`

Specifically check these known locations:
- `programs.html` — program heading (already correct, confirm it reads exactly "Academic Tutoring and College Grant Support")
- `get-help.html` — step 4 label
- `PHRF-Youth-Program.html` — offering card label
- Footer "Programs" column on all 7 files — the footer link currently reads "College grant support" per the standardized footer from Fix 2; update it to the full canonical name

After this fix, search all 7 files for the strings `College Grant`, `College grant`, and `College grants` to confirm every instance now reads the full canonical name with no shortened variants remaining.

---

## ROUND 3 VERIFICATION CHECKLIST

1. Every reference to the 4th program across all 7 files reads exactly "Academic Tutoring and College Grant Support" (case-adjusted for sentence vs. heading context only)
2. No shortened variant ("College Grant Support," "College grant assistance," "College grants," etc.) remains anywhere
3. Footer Programs column link text updated to match on all 7 files

Before finishing, confirm the following on each file:

1. Zero instances of `6kh.a6e.myftpupload.com` remain
2. Both footer columns are identical across all 7 files
3. EIN line is present in footer brand block on all 7 files
4. PHRF-Youth-Program.html enrollment form has: guardian email field, state dropdown, for/id label pairs, redirect hidden input
5. PHRF_Full_Site.html claim form has a redirect hidden input
6. Nav dropdown CSS includes `:focus-within` on all 7 files
7. Ohio city list is Cincinnati, Dayton, Columbus on both about.html and contact.html — no Cleveland
8. Georgia city list is Atlanta, South Fulton, East Point, College Park on both about.html and contact.html
9. PHRF_Full_Site.html stat bar shows "10 cities"
10. No copy, colors, layout, or styling was changed beyond what was specified above

---

## FINAL STEP — Run these verification commands and report the output

After all fixes are applied, run each of the following commands and show me the results:

```bash
# 1. Confirm no staging domain remains in any file
grep -r "6kh.a6e" .

# 2. Confirm Ohio city list in about.html
grep -i "cincinnati\|dayton\|columbus\|cleveland" about.html

# 3. Confirm Ohio city list in contact.html
grep -i "cincinnati\|dayton\|columbus\|cleveland" contact.html

# 4. Confirm Georgia cities in about.html
grep -i "atlanta\|athens\|macon\|savannah\|south fulton\|east point\|college park" about.html

# 5. Confirm Georgia cities in contact.html
grep -i "atlanta\|athens\|macon\|savannah\|south fulton\|east point\|college park" contact.html

# 6. Confirm stat bar city count in homepage
grep -i "cities" PHRF_Full_Site.html

# 7. Confirm redirect fields exist on both forms
grep -i "redirect" PHRF-Youth-Program.html
grep -i "redirect" PHRF_Full_Site.html

# 8. Confirm EIN line exists on all 7 files
grep -l "EIN furnished" about.html a-word-from-our-founder.html contact.html get-help.html PHRF-Youth-Program.html programs.html PHRF_Full_Site.html

# 9. Confirm focus-within was added to all 7 files
grep -l "focus-within" about.html a-word-from-our-founder.html contact.html get-help.html PHRF-Youth-Program.html programs.html PHRF_Full_Site.html

# 10. Confirm guardian email field exists on enrollment form
grep -i "guardian_email" PHRF-Youth-Program.html
```

If any command returns unexpected results, fix the issue before considering the task complete.
