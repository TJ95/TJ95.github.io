# Personal Website

A single-page personal portfolio with scroll-driven animations, dark mode theme, and minimalist+retro aesthetic. Built with Astro and deployed to GitHub Pages.

## Features

- **Scroll-driven animations** using CSS `animation-timeline: scroll()` with Intersection Observer fallback
- **Dark theme** with CSS custom properties
- **Responsive design** for all screen sizes
- **Sections**: Hero/About, Projects, Gallery, Valorant Rank Card
- **Component-based architecture** for easy customization
- **Static site** - fast loading, SEO-friendly

## Tech Stack

- [Astro 5.x](https://astro.build/) - Static site generator
- CSS custom properties + vanilla CSS - Styling
- CSS Scroll-driven animations + Intersection Observer - Animations
- GitHub Pages - Hosting

## Project Structure

```
my-site/
├── .github/workflows/deploy.yml    # GitHub Actions deployment
├── public/                          # Static assets
│   └── images/                      # Gallery and project images
├── src/
│   ├── components/                  # Reusable Astro components
│   │   ├── GalleryItem.astro
│   │   ├── Navigation.astro
│   │   ├── ProjectCard.astro
│   │   ├── ScrollReveal.astro
│   │   ├── Section.astro
│   │   └── ValorantCard.astro
│   ├── data/                        # Content data (easy to edit)
│   │   ├── gallery.json
│   │   └── projects.json
│   ├── layouts/
│   │   └── Layout.astro             # Root layout with dark theme
│   ├── pages/
│   │   └── index.astro              # Main page with all sections
│   └── styles/
│       ├── animations.css           # Scroll animation styles
│       └── global.css               # CSS variables, reset, base
├── astro.config.mjs                 # Astro configuration
└── package.json
```

## Getting Started

### Prerequisites

- Node.js 22+
- npm

### Installation

```bash
# Clone the repository
git clone https://github.com/username/username.github.io.git
cd username.github.io

# Install dependencies
npm install

# Start development server
npm run dev
```

Open http://localhost:4321 to view the site.

### Build

```bash
npm run build
```

The static site will be generated in the `dist/` directory.

## Customization

### 1. Update Personal Information

Edit `src/pages/index.astro` to update:
- Name in the hero section
- Bio/description
- Social links (GitHub, Twitter, LinkedIn)
- Stats (years of experience, projects count)
- Tech stack tags

### 2. Update Projects

Edit `src/data/projects.json` to add/remove projects:

```json
{
  "title": "Project Name",
  "description": "Brief description of the project",
  "tech": ["React", "TypeScript", "Node.js"],
  "link": "https://live-demo.com",
  "github": "https://github.com/username/repo",
  "image": "/images/project-name.jpg"
}
```

Add project images to `public/images/`.

### 3. Update Gallery

Edit `src/data/gallery.json`:

```json
{
  "src": "/images/photo-name.jpg",
  "alt": "Descriptive alt text",
  "caption": "Optional caption"
}
```

Add photos to `public/images/`.

### 4. Update Valorant Rank

Edit the `ValorantCard` component props in `src/pages/index.astro`:

```astro
<ValorantCard
  rank="Immortal 2"
  rr={78}
  winRate={55}
  matchesPlayed={150}
/>
```

Or add a rank image to `public/images/valorant-rank.png`.

### 5. Customize Theme Colors

Edit CSS variables in `src/styles/global.css`:

```css
:root {
  --bg-primary: #0a0a0f;        /* Deep dark background */
  --bg-secondary: #12121a;      /* Card backgrounds */
  --text-primary: #e2e2e2;      /* Main text */
  --accent: #7c3aed;            /* Primary accent (purple) */
  --accent-secondary: #06b6d4;  /* Secondary accent (cyan) */
}
```

## Deployment

### GitHub Pages Setup

1. Create a GitHub repository named `username.github.io` (replace `username` with your GitHub username)

2. Update `astro.config.mjs` with your GitHub username:

```javascript
export default defineConfig({
  site: 'https://username.github.io',
  base: '/',
  output: 'static',
});
```

3. Push to GitHub:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/username.github.io.git
git push -u origin main
```

4. Enable GitHub Pages:
   - Go to repository Settings > Pages
   - Set Source to "GitHub Actions"

5. The site will automatically deploy on every push to `main`

## Animation Strategy

The site uses a progressive enhancement approach for scroll animations:

1. **Modern browsers** (Chrome/Edge): Use CSS `animation-timeline: view()` for smooth, performant scroll-driven animations
2. **Fallback browsers** (Firefox/Safari): Intersection Observer adds `.is-visible` class to trigger CSS animations
3. **Reduced motion**: Respects `prefers-reduced-motion` media query

## Browser Support

- Chrome 115+ (full scroll-driven animations)
- Edge 115+ (full scroll-driven animations)
- Firefox (Intersection Observer fallback)
- Safari (Intersection Observer fallback)

## License

MIT License - feel free to use this template for your own portfolio!
