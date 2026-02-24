# Farhan Digital

A unified Astro v6 website combining a professional landing page and a feature-rich blog, deployed as static assets on Cloudflare Workers.

## 🎯 Project Overview

This project merges two previously separate Astro websites:
- **Landing Page**: Professional portfolio and services showcase at `/`
- **Blog**: Full-featured blog with search, tags, and archives at `/blog/`

### Key Features

- ⚡ **Astro v6** - Latest framework features and performance
- 🎨 **Tailwind CSS v4** - Modern CSS-first configuration
- 📝 **Content Collections** - Type-safe blog post management
- 🔍 **Pagefind Search** - Fast client-side search
- 🏷️ **Tag System** - Organize posts by topics
- 📅 **Archives** - Browse posts by date
- 🌓 **Dark Mode** - Automatic theme switching
- 🚀 **Cloudflare Workers** - Zero-cost hosting with global CDN
- 📱 **Fully Responsive** - Mobile-first design

## 🏗️ Architecture

### Clean Natural Routing

This project uses **Astro's natural file-based routing** - no complex workarounds or post-build scripts needed!

```text
src/pages/
├── index.astro              → /
├── blog/
│   ├── index.astro          → /blog/
│   ├── posts/[...page].astro → /blog/posts/1, /blog/posts/2
│   ├── [...slug].astro      → /blog/my-post-title
│   ├── tags/
│   │   ├── index.astro      → /blog/tags/
│   │   └── [tag]/[...page].astro → /blog/tags/javascript/1
│   ├── archives/index.astro → /blog/archives/
│   └── search.astro         → /blog/search/
├── rss.xml.ts               → /rss.xml
└── robots.txt.ts            → /robots.txt
```

### Project Structure

```text
farhandigital/
├── src/
│   ├── pages/              # Routes (landing + blog)
│   ├── content/
│   │   └── blog/           # Blog posts (markdown)
│   ├── layouts/
│   │   ├── LandingLayout.astro
│   │   ├── BlogLayout.astro
│   │   ├── Main.astro
│   │   └── PostDetails.astro
│   ├── components/
│   │   ├── landing/        # Landing page components
│   │   ├── blog/           # Blog components
│   │   └── ui/             # Shared UI components
│   ├── utils/
│   │   ├── blog/           # Blog utilities
│   │   └── shared/         # Shared utilities
│   ├── styles/
│   │   ├── global.css      # Unified Tailwind v4 styles
│   │   └── typography.css  # Blog typography
│   ├── assets/             # Images, icons
│   ├── config.ts           # Site configuration
│   └── constants.ts        # Social links, etc.
├── public/
│   ├── logo.png, banner.png, fonts/
│   └── blog/               # Blog-specific assets
│       ├── favicon.svg
│       └── toggle-theme.js
├── astro.config.mjs
├── package.json
├── tsconfig.json
└── wrangler.toml
```

## 🚀 Getting Started

### Prerequisites

- [Bun](https://bun.sh/) (recommended) or Node.js 20+
- Git

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd farhandigital

# Install dependencies
bun install

# Start development server
bun run dev
```

The site will be available at `http://localhost:4321`

### Development

```bash
# Start dev server
bun run dev

# Type check
bun run check

# Build for production
bun run build

# Preview production build
bun run preview

# Format code
bun run format

# Lint code
bun run lint
```

## 📝 Content Management

### Adding Blog Posts

1. Create a new markdown file in `src/content/blog/`:

```bash
src/content/blog/my-new-post.md
```

2. Add frontmatter:

```markdown
---
title: "My New Post"
description: "A brief description of the post"
pubDatetime: 2025-01-20T10:00:00Z
author: "Farhan"
tags: ["web-development", "astro"]
featured: false
draft: false
---

Your content here...
```

3. The post will automatically appear at `/blog/my-new-post`

### Frontmatter Options

- `title` (required): Post title
- `description` (required): Post description/excerpt
- `pubDatetime` (required): Publication date
- `modDatetime` (optional): Last modified date
- `author` (optional): Author name (default: "Farhan")
- `tags` (optional): Array of tags (default: ["others"])
- `featured` (optional): Show in featured section
- `draft` (optional): Hide from production
- `ogImage` (optional): Custom OG image
- `canonicalURL` (optional): Canonical URL
- `timezone` (optional): Timezone for date display

## 🎨 Customization

### Site Configuration

Edit `src/config.ts`:

```typescript
export const SITE = {
  website: "https://farhandigital.id",
  author: "Farhan",
  blogTitle: "Farhan Digital | Blog",
  blogDescription: "...",
  postPerPage: 4,
  lightAndDarkMode: true,
  showArchives: true,
  // ... more options
};
```

### Styling

The project uses **Tailwind CSS v4** with CSS-first configuration in `src/styles/global.css`:

```css
@theme {
  /* Landing page colors */
  --color-primary: #F48120;
  --color-dark: #121212;
  
  /* Blog colors (light/dark mode) */
  --color-background: var(--background);
  --color-accent: var(--accent);
}
```

### Social Links

Edit `src/constants.ts` to add/remove social links:

```typescript
export const SOCIALS = [
  {
    name: "GitHub",
    href: "https://github.com/yourusername",
    linkTitle: "GitHub Profile",
    icon: IconGitHub,
  },
  // Add more...
];
```

## 🚢 Deployment

### Cloudflare Workers (Recommended)

1. Install Wrangler CLI:

```bash
bun add -g wrangler
```

2. Login to Cloudflare:

```bash
wrangler login
```

3. Deploy:

```bash
bun run build
wrangler pages deploy dist
```

### Other Platforms

The built `dist/` folder can be deployed to any static hosting:
- Vercel
- Netlify
- GitHub Pages
- AWS S3 + CloudFront

## 🔧 Technical Details

### Eliminated Workarounds

This project **eliminates all the complex `/blog/` subdirectory workarounds** from the original blog:

❌ **OLD (Complex)**:
- Custom `withBase()` utility prepending `/blog/` everywhere
- Custom `getPath()` with complex slug manipulation
- Post-build script moving files and rewriting paths
- Assets in `src/pages/blog/_assets/` with underscore prefixes
- Manual path prefixing in every component

✅ **NEW (Clean)**:
- Natural Astro file-based routing
- Simple `getPostPath()` helper
- No post-build scripts needed
- Assets in standard `public/blog/` directory
- Clean, predictable paths throughout

### Path Generation

```typescript
// Simple and clean!
export function getPostPath(post: CollectionEntry<"blog">): string {
  const slug = post.id.split("/").pop()?.replace(/\.md$/, "") || post.id;
  return `/blog/${slug}`;
}
```

### Build Output

```text
dist/
├── index.html              # Landing page
├── logo.png, banner.png    # Landing assets
├── blog/
│   ├── index.html          # Blog home
│   ├── posts/              # Post listings
│   ├── my-post/index.html  # Individual posts
│   ├── tags/               # Tag pages
│   ├── archives/           # Archives
│   ├── search/             # Search
│   ├── _astro/             # Astro assets
│   ├── pagefind/           # Search index
│   └── favicon.svg         # Blog assets
├── rss.xml
├── robots.txt
└── sitemap-index.xml
```

## 📚 Tech Stack

- **Framework**: [Astro v6](https://astro.build/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Search**: [Pagefind](https://pagefind.app/)
- **Syntax Highlighting**: [Shiki](https://shiki.style/)
- **OG Images**: [Satori](https://github.com/vercel/satori)
- **Package Manager**: [Bun](https://bun.sh/)
- **Deployment**: [Cloudflare Workers](https://workers.cloudflare.com/)

## 📄 License

All rights reserved © 2025 Farhan Digital

## 🤝 Contributing

This is a personal website project. If you find bugs or have suggestions, feel free to open an issue.

---

Built with ❤️ using Astro v6 and Tailwind CSS v4
