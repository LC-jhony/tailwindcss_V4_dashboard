# AGENTS.md - Themeplate Project Guidelines

## Project Overview

This is a static HTML dashboard template using Tailwind CSS v4 (via Vite) and Alpine.js for interactivity. It includes a dark/light mode toggle and a responsive layout with header, main content area, and sidebar.

## Build/Lint/Test Commands

### Installation

```bash
npm install
```

### Development

```bash
npm run dev
```

Then open http://localhost:5173 in your browser.

### Build for Production

```bash
npm run build
```

### Preview Production Build

```bash
npm run preview
```

## Code Style Guidelines

### Project Structure

```
themeplate/
├── index.html          # Main HTML file
├── package.json        # Dependencies and scripts
├── vite.config.js     # Vite configuration
├── src/
│   └── style.css      # Tailwind CSS imports
└── AGENTS.md          # This file
```

### Tailwind CSS v4 Setup

In Tailwind v4 with Vite, configure dark mode in CSS:

```css
@import "tailwindcss";

@custom-variant dark (&:where(.dark, .dark *));
```

### HTML Structure

```html
<html x-data="theme()" class="dark">
```

### Tailwind CSS Dark Mode Variants

Use native `dark:` variants:

```html
<!-- Light mode (default) -->
class="bg-slate-50"

<!-- Dark mode -->
class="bg-slate-50 dark:bg-slate-900"
class="text-slate-900 dark:text-white"
class="bg-white dark:bg-slate-800"
```

### Color Palette

Current color scheme using indigo as primary:

- **Header Light**: `bg-indigo-600`
- **Header Dark**: `bg-indigo-900`
- **Primary accent**: `indigo-500`, `indigo-600`
- **Secondary accent**: `emerald-500` (success), `amber-500` (warning)
- **Neutral Light**: `slate-50`, `white` for backgrounds
- **Neutral Dark**: `slate-800`, `slate-900` for backgrounds

### Dark Mode Implementation

The template uses Alpine.js for dark mode toggle with system preference detection:

```javascript
function theme() {
    return {
        dark: true,
        init() {
            this.updateFromStorage();
            window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
                if (!localStorage.getItem('theme')) {
                    this.dark = e.matches;
                    this.updateClass();
                }
            });
        },
        updateFromStorage() {
            const stored = localStorage.getItem('theme');
            if (stored) {
                this.dark = stored === 'dark';
            } else {
                this.dark = window.matchMedia('(prefers-color-scheme: dark)').matches;
            }
            this.updateClass();
        },
        updateClass() {
            if (this.dark) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        },
        toggle() {
            this.dark = !this.dark;
            localStorage.setItem('theme', this.dark ? 'dark' : 'light');
            this.updateClass();
        }
    }
}
```

### Responsive Header

The header uses Alpine.js for mobile menu toggle:

```html
<header x-data="{ menuOpen: false }">
    <button @click="menuOpen = !menuOpen" class="md:hidden">
        <svg x-show="!menuOpen">...</svg>
        <svg x-show="menuOpen">...</svg>
    </button>
    <div x-show="menuOpen" x-transition class="md:hidden">...</div>
</header>
```

### Layout Structure

- **Header**: Full-width with indigo background, fixed navigation
- **Main**: Grid layout with `lg:grid-cols-3` (2 columns content, 1 column sidebar)
- **Sidebar cards**: Profile, Activity, Quick Stats, Tags

### Best Practices

- Keep HTML indented consistently (4 spaces)
- Group related elements with blank lines
- Use comments to mark major sections (`<!-- HEADER -->`, `<!-- MAIN -->`, `<!-- SIDEBAR -->`)
- Use Tailwind utility classes
- Use `x-show` for conditional rendering with Alpine.js
- Use `dark:` variants for dark mode styling

## Notes for AI Agents

- Use Tailwind v4 with Vite (not CDN)
- Run `npm install` before starting development
- Use native `dark:` variants for dark mode
- Keep changes minimal and focused on the specific request
- Always test dark mode when making color changes
- Preserve existing patterns and class ordering
