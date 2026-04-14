# Astro SEO 🚀

A fully-featured, zero-dependency SEO component for [Astro](https://astro.build) projects.

Covers everything Google, Open Graph, and Twitter/X expect:
- Primary meta tags (`title`, `description`, `robots`, `canonical`)
- Open Graph (`og:title`, `og:image`, `og:type`, `og:locale`, etc.)
- Twitter/X Cards (`summary_large_image` by default)
- Article-specific tags (`article:published_time`, `article:author`, etc.)
- JSON-LD structured data (auto-generated or fully custom)
- Hreflang alternates for multi-language sites
- Pagination links (`prev`/`next`)
- Theme colour, `noindex`/`nofollow` helpers

---

## Installation

```bash
npm install @jessgaspar/astro-seo
# or
pnpm add @jessgaspar/astro-seo
# or
yarn add @jessgaspar/astro-seo
```

---

## Setup

Place the component inside the `<head>` of your base layout:

```astro
---
// src/layouts/Layout.astro
import SEO from "@jessgaspar/astro-seo";

export interface Props {
  title: string;
  description: string;
}

const { title, description, ...rest } = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <SEO
      title={title}
      description={description}
      titleTemplate="%s | My Site"
      siteName="My Site"
      ogImage="/images/og-default.png"
      twitterSite="@mysite"
      {...rest}
    />
  </head>
  <body>
    <slot />
  </body>
</html>
```

Then on each page just pass what you need:

```astro
---
// src/pages/index.astro
import Layout from "@/layouts/Layout.astro";
---
<Layout title="Home" description="Welcome to My Site.">
  <h1>Hello!</h1>
</Layout>
```

---

## All Props

### Required

| Prop | Type | Description |
|---|---|---|
| `title` | `string` | Page title |
| `description` | `string` | Meta description (120–160 chars recommended) |

### Core Meta

| Prop | Type | Default | Description |
|---|---|---|---|
| `titleTemplate` | `string` | — | Wraps title, e.g. `"%s \| My Site"` |
| `canonical` | `string` | current URL | Canonical URL |
| `keywords` | `string` | — | Comma-separated keywords |
| `robots` | `string` | `"index, follow"` | Robots directive |
| `noindex` | `boolean` | `false` | Shorthand for `noindex` |
| `nofollow` | `boolean` | `false` | Shorthand for `nofollow` |
| `themeColor` | `string` | — | `<meta name="theme-color">` |

### Open Graph

| Prop | Type | Default | Description |
|---|---|---|---|
| `ogType` | `string` | `"website"` | OG type (`"article"`, `"product"`, etc.) |
| `ogImage` | `string` | — | Absolute URL to OG image (1200×630 recommended) |
| `ogImageAlt` | `string` | — | Alt text for OG image |
| `ogImageWidth` | `number` | `1200` | OG image width |
| `ogImageHeight` | `number` | `630` | OG image height |
| `siteName` | `string` | — | Site name in social cards |
| `locale` | `string` | `"en_US"` | OG locale |

### Twitter / X

| Prop | Type | Default | Description |
|---|---|---|---|
| `twitterCard` | `string` | `"summary_large_image"` | Card type |
| `twitterSite` | `string` | — | Site handle e.g. `"@mysite"` |
| `twitterCreator` | `string` | — | Author handle |

### Article (use with `ogType="article"`)

| Prop | Type | Description |
|---|---|---|
| `articlePublishedTime` | `string` | ISO 8601 publish date |
| `articleModifiedTime` | `string` | ISO 8601 modified date |
| `articleAuthor` | `string` | Author name |
| `articleSection` | `string` | Section / category |
| `articleTags` | `string[]` | Tag array |

### JSON-LD

| Prop | Type | Default | Description |
|---|---|---|---|
| `jsonLd` | `object \| object[]` | auto-generated | Custom JSON-LD schema |
| `jsonLdEnabled` | `boolean` | `true` | Set to `false` to disable |

### Internationalisation

| Prop | Type | Description |
|---|---|---|
| `alternates` | `{ hreflang: string; href: string }[]` | Hreflang alternate URLs |
| `next` | `string` | Pagination next URL |
| `prev` | `string` | Pagination prev URL |

---

## Examples

### Blog post with full Article schema

```astro
<Layout
  title={post.title}
  description={post.excerpt}
  ogType="article"
  ogImage={post.coverImage}
  ogImageAlt={post.coverImageAlt}
  articlePublishedTime={post.publishedAt}
  articleModifiedTime={post.updatedAt}
  articleAuthor={post.author.name}
  articleSection="Engineering"
  articleTags={post.tags}
  twitterCreator={post.author.twitter}
/>
```

### Multi-language site

```astro
<Layout
  title="Accueil"
  description="Bienvenue sur notre site."
  locale="fr_FR"
  alternates={[
    { hreflang: "en", href: "https://example.com/en/" },
    { hreflang: "fr", href: "https://example.com/fr/" },
  ]}
/>
```

### Prevent indexing (legal pages, dashboards, thank-you pages)

```astro
<Layout
  title="Privacy Policy"
  description="Our privacy policy."
  noindex
  nofollow
/>
```

### Custom JSON-LD (Organisation schema)

```astro
<Layout
  title="About"
  description="Learn about us."
  jsonLd={{
    "@context": "https://schema.org",
    "@type": "Organization",
    name: "My Company",
    url: "https://example.com",
    logo: "https://example.com/logo.png",
    sameAs: [
      "https://twitter.com/mycompany",
      "https://linkedin.com/company/mycompany"
    ]
  }}
/>
```

---

## License

MIT