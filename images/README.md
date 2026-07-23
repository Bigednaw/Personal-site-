# Smith Consulting — Personal Brand Website

A single-page, Apple-inspired site for **John Smith | Technology Leader**, covering Home,
About, Experience, Projects, Interests, Work With Me, Case Studies, Resume Request,
Newsletter, Contact, and Book a Meeting — all in one `index.html` file with no build step.

---

## 1. View it right now

You don't need to install anything to look at the site:

1. Download `index.html`.
2. Double-click it (or drag it into a browser window).

That's it — everything (HTML, CSS, and JavaScript) lives in this one file.

---

## 2. Edit every placeholder in one place

All the editable content — your name, bio, projects, experience, certifications,
interests, email address, scheduling link, etc. — lives in **one JavaScript object**
near the top of `index.html`, called `SITE`. Open the file in any text editor and look
for this block:

```html
<script>
const SITE = {
  brand: "Smith Consulting",
  name: "John Smith",
  role: "Cloud Architect & Technology Leader",
  ...
};
</script>
```

Everything on the page is generated from this object, so you never need to hunt through
HTML markup to update text — just edit the values here and refresh your browser.

| Field | What it controls |
|---|---|
| `brand`, `name`, `role`, `tagline` | Nav logo, hero headline, hero subheadline |
| `email`, `phone` | Shown in the Contact section |
| `calendlyUrl` | The "Book a Meeting" button destination |
| `socials` | Footer links (LinkedIn, etc.) |
| `highlights` | The 4 stat cards under the hero |
| `about` | Journey, philosophy (placeholder), education, and certifications |
| `experience` | Career timeline entries (expandable) |
| `projects` | Project cards with tags and outcomes |
| `skills` | Skill bars, grouped as Technical, Project Management, Software, Soft Skills — each item has a `level` (0–100) that sets the bar width |
| `interests` | The interests grid |
| `workWithMe` | The 4 "Core Strengths" cards |

**Note:** `about.philosophy` is currently a placeholder — replace it with your own approach to leadership/coordination whenever you're ready.

**Photo & screenshots:** the hero photo and project screenshots are currently soft
placeholder blocks (labelled "Headshot" / "Project screenshot placeholder"). To replace
them with real images, swap the relevant `<div class="hero-photo">…</div>` or
`<div class="project-shot">…</div>` element for an `<img src="images/your-photo.jpg" />`
tag, and add your image files alongside `index.html` (e.g. in an `images/` folder).

---

## 3. Forms — current behavior

The Case Study, Resume Request, Newsletter, and Contact forms all work in the browser
today: they validate required fields and show a confirmation message. **They don't yet
send real emails or store data anywhere** — that requires a small backend, described
below. This keeps the site free to host and easy to preview while you're still making
content edits.

---

## 4. Connecting the forms to a real backend

The original plan called for Supabase (database), Resend or Amazon SES (email
delivery), and Cloudflare Turnstile (spam protection). Here's the lightest path to get
there once you're ready:

### a. Database — Supabase
1. Create a free project at [supabase.com](https://supabase.com).
2. Create four tables matching the original data model:
   - `resume_requests (id, email, requested_at, sent_at)`
   - `newsletter (id, email, subscribed_at)`
   - `contacts (id, name, email, company, message, submitted_at)`
   - `case_study_requests (id, email, document, requested_at)`
3. Use the Supabase JS client in place of the `handleForm()` demo logic in `index.html`
   to insert a row on each submit.

### b. Email delivery — Resend or Amazon SES
1. Verify your domain (e.g. `smithconsulting.com`) with [Resend](https://resend.com) or
   Amazon SES.
2. Create a tiny serverless function (a Vercel API route works well) that:
   - Receives the form data,
   - Writes it to Supabase,
   - Sends the confirmation/verification email via Resend/SES.
3. Point each form's `fetch()` call at that API route instead of the in-browser
   `handleForm()` stub.

### c. Spam protection — Cloudflare Turnstile
Add the Turnstile widget script to the page and verify the token server-side inside
your API route before writing to the database or sending mail.

### d. Custom email address
Set up **iCloud Custom Domains** in Apple ID settings to receive mail at
`john@smithconsulting.com` in Apple Mail, while Resend/SES handles *outbound* mail sent
by the website.

### e. Scheduling
Replace `SITE.calendlyUrl` with your real Calendly or Microsoft Bookings link — no code
changes needed beyond that one value.

---

## 5. Hosting on GitHub + Vercel (recommended)

1. **Create a GitHub repository** and push these files:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
2. **Import the repo into Vercel:**
   - Go to [vercel.com/new](https://vercel.com/new) and import your GitHub repository.
   - Framework preset: choose "Other" (this is a static HTML site, no build step).
   - Deploy — Vercel will give you a live URL immediately.
3. **Connect your domain:**
   - In Vercel, go to your project → Settings → Domains → add `smithconsulting.com`.
   - Update your domain's DNS records as instructed by Vercel.

### Alternative: GitHub Pages
If you'd rather not use Vercel, GitHub Pages works too since this is a static file:
1. In your repo, go to Settings → Pages.
2. Set the source to the `main` branch, root folder.
3. GitHub will publish it at `https://<your-username>.github.io/<your-repo>/`.

---

## 6. Design reference

| Token | Value |
|---|---|
| White | `#ffffff` |
| Off White | `#f5f5f7` |
| Black | `#1d1d1f` |
| Gray | `#86868b` |
| Light Gray | `#d2d2d7` |
| Accent (interactive blue) | `#0071e3` |
| Font | `-apple-system, "SF Pro Display/Text", Inter, sans-serif` |

All of these are CSS custom properties at the top of the `<style>` block (`:root { ... }`)
— change a value once and it updates everywhere it's used.

---

## 7. Future features (Phase 2)

As noted in the original site plan, these are natural next additions once the core site
is live:
- Blog
- AI chatbot trained on your resume
- Client portal
- Project showcase CMS
- Speaking engagements page
- Testimonials
- Video introductions
- Private downloadable resources

---

## File structure

```
.
├── index.html   ← the entire site (HTML + CSS + JS)
└── README.md    ← this file
```

If your project grows, you can always split `index.html`'s `<style>` and `<script>`
blocks into separate `styles.css` and `main.js` files — the code was written in plain,
dependency-free HTML/CSS/JS specifically so it's easy to reorganize as needed.
