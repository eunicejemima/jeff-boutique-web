# Jeff Boutique Website

A portfolio and enquiry website for Jeff Boutique, a boutique for women and kids offering ready designs and customized dresses. It is not an e-commerce site: there is no cart, checkout, login, or fixed pricing.

## Pages

- **Collections (Home):** Hero, About section, and footer.
- **Frocks & Gowns:** Hero, category tiles (Frocks, Gowns, Chudis), flared silhouette details, and customization cards.
- **Customization:** Bespoke process, Bring Your Own Design, measurement options, and the enquiry form.
- **Our Story:** Hero and image bento grid.
- **Kids Wear:** Hero for children's collection.

Categories: Gowns, Frocks, Chudis, Customized Designs. Kurtis is not included.

## Tech Stack

- React 18
- Plain CSS (no Tailwind)
- esbuild (bundles the app into a single classic script so it runs from `file://`)
- Google Apps Script (stores enquiries in Google Sheets)


## Getting Started

### Requirements

- Node.js 18 or newer
- npm

### Install and Build

```bash
npm install
./build.sh
```

The build output goes to `dist/`. Open `dist/index.html` in a browser. No web server is needed.

## Adding Images

Place the PNG files in `public/assets/` (or `dist/assets/` after building) using these exact names:

| File | Used for |
|---|---|
| `home-hero.png` | Home hero |
| `home-about.png` | Home about section |
| `frocks-hero.png` | Frocks & Gowns hero |
| `frocks-tile-1.png` | Daily & Party Frocks tile |
| `frocks-tile-2.png` | Statement Indo-Western Gowns tile |
| `frocks-tile-3.png` | Occasion & Bridal Gowns tile |
| `frocks-tile-4.png` | Chudis tile |
| `frocks-detail.png` | Flared silhouette detail |
| `custom-hero.png` | Customization hero |
| `custom-byod.png` | Bring Your Own Design collage |
| `story-large.png` | Our Story large photo |
| `story-small-1.png` | Our Story small photo |
| `kids-hero.png` | Kids Wear hero |

Missing images show as a soft gradient placeholder.

## Enquiry Form and Google Sheets

The enquiry form collects four fields: Full Name, Email Address, Target Date, and Reference Links. Full Name and Email are required.

Submissions are sent with a GET request to a Google Apps Script Web App and appended to a sheet named **Responses** with the columns: Timestamp, Full Name, Email Address, Target Date, Reference Links.

### Setting Up the Apps Script

1. Open your Google Sheet and go to **Extensions → Apps Script**.
2. Paste the contents of `Code.gs` (included in the project) and save.
3. Click **Deploy → New deployment → Web app**.
4. Set **Execute as: Me** and **Who has access: Anyone**, then deploy.
5. Copy the Web App URL.

### Connecting the Form

Set the URL in `src/Enquiry.jsx`:

```javascript
const SCRIPT_URL = "https://script.google.com/macros/s/.../exec";
```

Rebuild with `./build.sh`.

### Testing the Script

Open this in a browser (replace the URL):

```
<SCRIPT_URL>?fullName=Test&email=test@example.com&targetDate=2026-12-01&referenceLinks=none
```

You should see `{"success":true}` and a new row in the Responses sheet.

> **Note:** The form uses `mode: "no-cors"`, so the site shows the success message once the request is sent. It cannot confirm that the row was saved. Check the sheet after testing.

When you change `Code.gs`, redeploy with **Deploy → Manage deployments → Edit → New version**. If the Web App URL changes, update `SCRIPT_URL`.

## Configuration

- **WhatsApp number:** Replace the placeholder `910000000000` in `src/App.jsx` and `src/Enquiry.jsx` with your number.
- **Navigation:** Routing uses URL hashes (`#home`, `#frocks`, `#customization`, `#story`, `#kids`), so it works without a server.

## Responsive Design

Layouts adapt at 1024px, 860px (mobile navigation menu), and 560px.

## Notes

- Design source: Figma file "Jeff-Boutique-Website".
- Fonts: Playfair Display and Montserrat (loaded from Google Fonts).
