# Capivora Robotics — Website

Single-page site for Capivora Robotics. Positioning: robotics engineering for the shifts that put humans at risk. YouTube intro plays as visitors enter, followed by mission, capabilities, a live telemetry readout, approach, and FAQ.

## File structure

```
capivora-website/
├── index.html              — the whole site (React + Babel via CDN, all styles inline)
├── assets/
│   ├── logo.png            — paper-off-white logo, used on dark backgrounds
│   └── logo-dark.png       — original black logo, kept for light-background use
└── README.md               — this file
```

## Running locally

**Simplest:** double-click `index.html`. Most modern browsers will open it directly. Some browsers block iframes on `file://` URLs, so if the YouTube intro doesn't render, use one of the options below.

**Local server (recommended):**

```bash
# Python 3
python3 -m http.server 8000

# or Node
npx serve

# or PHP
php -S localhost:8000
```

Then open `http://localhost:8000` in your browser.

## Deploying

The site is static — HTML plus two PNG files. Drop it on any static host.

### Netlify (drag-and-drop, no signup required for previews)

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag the entire `capivora-website` folder onto the page
3. You get a live URL in ~10 seconds

### Vercel

```bash
npm i -g vercel
vercel
```

Follow the prompts. Deploy from any subdirectory; Vercel figures out it's static.

### GitHub Pages

1. Push this folder to a GitHub repo
2. Repo → Settings → Pages → Source: `main` branch, root folder
3. Site publishes at `https://<username>.github.io/<repo>/`

### Any other host

Upload the three files (`index.html` + `assets/`) to any web root. That's it.

## Customizing

### Change the YouTube video

In `index.html`, find:

```js
const YOUTUBE_VIDEO_ID = 'Fh5HVdrcZmo';
```

Replace with the ID from any YouTube video URL (the string after `youtu.be/` or `watch?v=`).

### Change the logo

Replace the two files in `assets/`. Keep the filenames the same and the site picks them up automatically. Both should be PNGs with transparent backgrounds:
- `logo.png` — off-white version (`#EDE9DE` tint), shown on the black header/footer/hero
- `logo-dark.png` — black version, kept in case you add it to light-background sections

### Change colors

In `index.html`, find the `:root` CSS block near the top. Colors are all named tokens:

```css
--black: #050505;         /* pure black bands (nav, hero, footer) */
--paper: #EDE9DE;         /* off-white body */
--red: #C2352A;           /* signal red accent — safety-red, industrial */
--blueprint: #5F7A88;     /* muted cyan for technical annotations */
```

### Change the tagline / mission copy

Search for `Mission` in the JavaScript section (it's a React component). All copy lives in JSX, editable directly. Same for `Hero`, `Capabilities`, `FAQ`, etc.

### Change stats numbers

Find `function Stats()` — the numbers, prefixes, suffixes, and labels are all in a plain JavaScript array. Edit and reload.

### Change LinkedIn URL

Find `const LINKEDIN_URL = ` near the top of the script section.

## Turning this into a real React project

The current build uses **Babel Standalone** to compile JSX in the browser. That's fine for prototyping but slow for production. To upgrade to a real build:

**With Vite (recommended):**

```bash
npm create vite@latest capivora-app -- --template react
cd capivora-app
npm install
```

Then extract each React component from `index.html` into its own file under `src/components/` (Hero.jsx, Mission.jsx, Stats.jsx, Telemetry.jsx, Capabilities.jsx, Approach.jsx, FAQ.jsx, Contact.jsx, Footer.jsx, YouTubeIntro.jsx) and move the CSS out of the `<style>` tag into `src/index.css` or CSS modules. Fonts come from Google Fonts the same way. Ship it with `npm run build`.

**With Astro (if you'll add a blog or case studies later):**

The [AstroWind](https://github.com/arthelokyo/astrowind) template is a good starting point — copy the section markup from here into Astro components.

## The wiring

- **Fonts:** [Fraunces](https://fonts.google.com/specimen/Fraunces) (humanist serif display, uses the SOFT variable axis for warmth) + [Inter](https://fonts.google.com/specimen/Inter) (body) + [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) (technical labels only). Loaded from Google Fonts.
- **React 18** and **Babel Standalone** via cdnjs. No build step.
- **YouTube embed** via the privacy-enhanced `youtube-nocookie.com/embed/` endpoint. Autoplays muted (browser requirement); a "Sound on" button unmutes via YouTube's iframe postMessage API. Detects video-end via the same API and fades the overlay to reveal the site.
- **Session storage** remembers the intro played, so returning visitors in the same session go straight to the site.
- **Contact form** is client-side only — logs to nothing right now. Wire the `submit` handler in `function Contact()` to your backend, Formspree, or a mail API.

## License / credit

Logo is Capivora Robotics' own. The visualisation (schematic robot arm in the hero) is custom SVG built for this site. Everything else is standard web building blocks.
