# Personal Website

A single-page personal portfolio with scroll-driven animations, dark mode theme, and minimalist+retro aesthetic. Built with Astro and deployed to GitHub Pages.

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
