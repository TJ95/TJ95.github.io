# Gallery Section Implementation Plan

## Overview
Add a masonry-style gallery section between Experiences and Misc sections with a preview grid that fades out at the bottom, and a full-screen gallery modal with smooth transitions.

---

## Phase 1: Project Structure & Components

### New Files to Create

1. **`/src/components/GalleryGrid.astro`**
   - Masonry grid layout component
   - Displays 5-6 images with varied sizes
   - Configurable number of images and layout
   - Props: `images[]`, `columns`, `class`

2. **`/src/components/GalleryModal.astro`**
   - Full-screen gallery overlay
   - Exit button and click-outside-to-close
   - Smooth open/close animations
   - Props: `images[]`, `isOpen`, `onClose`

3. **`/src/components/GallerySection.astro`**
   - Main section wrapper combining preview + modal
   - "View Full Gallery" button with hover effects
   - Fade-out gradient overlay at bottom
   - Uses ScrollReveal for entrance animation

4. **`/src/data/gallery.json`**
   - Image metadata (src, alt, title, aspect ratio hints)
   - Pre-configured selection of 6 images for preview
   - All 14 images for full gallery

### Modified Files

1. **`/src/pages/index.astro`**
   - Import and add GallerySection between Experiences and Misc
   - Add gallery section styles

2. **`/src/components/Navigation.astro`**
   - Uncomment/update Gallery nav item (already exists in navItems array)

3. **`/src/styles/animations.css`**
   - Add modal open/close animations
   - Add image hover scale effects

---

## Phase 2: Technical Implementation Details

### 1. Masonry Grid Layout Strategy

**CSS Columns Approach (Recommended)**
```css
.masonry-grid {
  columns: 3;
  column-gap: 1rem;
}

.masonry-item {
  break-inside: avoid;
  margin-bottom: 1rem;
}

/* Varied sizes via aspect ratios */
.masonry-item:nth-child(1) { aspect-ratio: 3/4; }   /* Portrait */
.masonry-item:nth-child(2) { aspect-ratio: 4/3; }   /* Landscape */
.masonry-item:nth-child(3) { aspect-ratio: 1/1; }   /* Square */
.masonry-item:nth-child(4) { aspect-ratio: 16/9; }  /* Wide */
.masonry-item:nth-child(5) { aspect-ratio: 3/4; }   /* Portrait */
.masonry-item:nth-child(6) { aspect-ratio: 4/5; }   /* Portrait */
```

**Preview Grid Constraints:**
- Max height: ~500px with `overflow: hidden`
- Bottom fade: gradient overlay from transparent to `--bg-primary`
- 6 images selected from the 14 available

### 2. Fade-Out Effect at Bottom

```css
.gallery-preview {
  position: relative;
  max-height: 500px;
  overflow: hidden;
}

.gallery-preview::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 150px;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    var(--bg-primary) 100%
  );
  pointer-events: none;
}
```

### 3. Full Gallery Modal

**State Management (Vanilla JS in Astro)**
```javascript
// GallerySection.astro script
let isModalOpen = false;

function openGallery() {
  isModalOpen = true;
  document.body.style.overflow = 'hidden'; // Prevent background scroll
}

function closeGallery() {
  isModalOpen = false;
  document.body.style.overflow = '';
}

// Close on ESC key
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') closeGallery();
});
```

**Modal Animation CSS**
```css
.gallery-modal {
  position: fixed;
  inset: 0;
  z-index: 2000;
  background: rgba(10, 10, 15, 0.95);
  backdrop-filter: blur(20px);

  opacity: 0;
  visibility: hidden;
  transition: opacity 0.4s ease, visibility 0.4s ease;
}

.gallery-modal.is-open {
  opacity: 1;
  visibility: visible;
}

.gallery-modal-content {
  transform: scale(0.95);
  transition: transform 0.4s ease;
}

.gallery-modal.is-open .gallery-modal-content {
  transform: scale(1);
}
```

### 4. Full Gallery Grid Layout

- 4-column masonry on desktop
- 3-column on tablet
- 2-column on mobile
- All 14 images displayed
- Lightbox-style image viewing on click (optional enhancement)

### 5. GitHub.io Path Considerations

Since this will be hosted on GitHub Pages:
```astro
<!-- Use root-relative paths -->
<img src={`${import.meta.env.BASE_URL}/images/gallery/${image.src}`} />

<!-- Or configure astro.config.mjs base -->
export default defineConfig({
  base: '/your-repo-name', // for GitHub project pages
  // base: '/', // for GitHub user/org pages
});
```

---

## Phase 3: Image Selection & Sizing

### Selected Images for Preview (6 images)

| Position | Image | Aspect Ratio | Size Class |
|----------|-------|--------------|------------|
| 1 | DSC03646.JPG | 3:4 | portrait-lg |
| 2 | DSC04109.JPG | 4:3 | landscape |
| 3 | DSC04255.JPG | 1:1 | square |
| 4 | DSC04286.JPG | 16:9 | wide |
| 5 | DSC04310.JPG | 3:4 | portrait-sm |
| 6 | bamboo.png | 4:5 | portrait |

### Full Gallery (14 images)
All images in `/public/images/gallery/` displayed in masonry grid.

---

## Phase 4: Styling Specifications

### Color Scheme (Matches existing)
- Background: `--bg-primary: #0a0a0f` (modal overlay)
- Border: `--border-color: rgba(255, 255, 255, 0.1)`
- Accent: `--accent: #2563eb`
- Text: `--text-primary: #e2e2e2`

### Typography
- Section number: "03" (mono, accent color)
- Title: "Gallery" (display font)
- Button: "View Full Gallery →" (mono font with arrow)

### Spacing
- Section padding: `var(--space-3xl)` (6rem)
- Grid gap: `1rem`
- Modal padding: `2rem`

### Hover Effects
```css
.masonry-item img {
  transition: transform 0.4s ease, filter 0.4s ease;
}

.masonry-item:hover img {
  transform: scale(1.05);
  filter: brightness(1.1);
}
```

---

## Phase 5: Accessibility & UX

### Accessibility Checklist
- [ ] Modal trap focus when open
- [ ] ESC key closes modal
- [ ] Click outside modal closes it
- [ ] Proper ARIA labels on buttons
- [ ] Reduced motion support
- [ ] Alt text for all images
- [ ] Keyboard navigation in gallery

### UX Enhancements
- Loading state for images (blur placeholder)
- Smooth scroll to gallery on nav click
- Exit button fixed in modal header
- Counter showing "Viewing X of Y images" (optional)

---

## Phase 6: File-by-File Implementation Order

### Step 1: Data File
Create `/src/data/gallery.json` with image metadata

### Step 2: GalleryGrid Component
Build reusable masonry grid with CSS columns

### Step 3: GalleryModal Component
Build full-screen modal with animations

### Step 4: GallerySection Component
Combine preview grid + modal + fade effect

### Step 5: Update index.astro
Add GallerySection between Experiences and Misc

### Step 6: Update Navigation
Ensure Gallery link is active

### Step 7: Add Animations
Add modal animations to animations.css

### Step 8: Test & Refine
- Test on different screen sizes
- Verify GitHub.io paths work
- Check reduced motion preference

---

## Phase 7: Code Architecture

### Component Hierarchy
```
index.astro
└── GallerySection.astro (section wrapper)
    ├── Section.astro (existing, for layout)
    ├── ScrollReveal.astro (existing, for entrance)
    ├── GalleryGrid.astro (preview, 6 images)
    │   └── masonry-item (x6)
    └── GalleryModal.astro (full gallery, 14 images)
        ├── close button
        └── GalleryGrid.astro (full, 14 images)
```

### State Flow
1. User clicks "View Full Gallery" → `openModal()` called
2. Modal gets `.is-open` class → CSS animation plays
3. User clicks exit/ESC/overlay → `closeModal()` called
4. Modal loses `.is-open` class → fade out animation

---

## Implementation Notes

### Performance Considerations
- Images are large (10-20MB each) - consider optimization for production
- Use `loading="lazy"` on gallery images
- Consider `decoding="async"` for better performance
- Modal images load only when modal opens

### Browser Support
- Masonry: All modern browsers (CSS columns)
- Modal animations: All modern browsers
- Backdrop-filter: Fallback to solid color for older browsers

### Mobile Considerations
- Modal becomes full-screen on mobile
- 2-column grid on mobile (full gallery)
- Touch gestures for closing (tap outside)
- Swipe to close (optional enhancement)

---

## Final File Checklist

- [ ] `/src/data/gallery.json`
- [ ] `/src/components/GalleryGrid.astro`
- [ ] `/src/components/GalleryModal.astro`
- [ ] `/src/components/GallerySection.astro`
- [ ] Modified: `/src/pages/index.astro`
- [ ] Modified: `/src/components/Navigation.astro`
- [ ] Modified: `/src/styles/animations.css`

---

## Estimated Implementation Time
- Phase 1-2 (Planning): Complete
- Phase 3-4 (Components): ~45 min
- Phase 5-6 (Integration): ~20 min
- Phase 7 (Testing/Polish): ~15 min
- Total: ~80 minutes
