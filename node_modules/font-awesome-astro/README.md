# Font Awesome Astro Components

[![npm](https://img.shields.io/npm/v/font-awesome-astro)](https://www.npmjs.com/package/fontawesome-astro)
[![license](https://img.shields.io/npm/l/font-awesome-astro)](LICENSE)

Official Font Awesome integration for Astro. Easily use Font Awesome icons in your Astro projects with optimized SVG rendering.

## ✨ Features

- **Zero client-side JS**: Pure SVG rendering
- **SSR compatible**: Works with server-side rendering
- **TypeScript support**: Full type definitions included
- **Layered icons**: Create complex icon compositions
- **Styling control**: Apply classes directly to SVGs

## 📦 Installation

```bash
npm install font-awesome-astro @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons
```
### Or using yarn/pnpm:
```bash
yarn add font-awesome-astro @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons
pnpm add font-awesome-astro @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons
```

## 🚀 Basic Usage

### Single Icon

```js
---
import { FontAwesomeIcon } from 'font-awesome-astro';
import { faRocket } from '@fortawesome/free-solid-svg-icons';
---

<FontAwesomeIcon 
  icon={faRocket} 
  class="text-purple-500 hover:scale-110 transition-all"
/>
```

### Icon Layers

```js
---
import { FontAwesomeLayer } from 'font-awesome-astro';
import { faCircle, faEnvelope } from '@fortawesome/free-solid-svg-icons';
---

<FontAwesomeLayer class="fa-2x">
  {{
    icons: [
      { icon: faCircle, class: "text-blue-400" },
      { icon: faEnvelope, class: "text-white scale-75" }
    ]
  }}
</FontAwesomeLayer>
```

## 📚 Component API
### `FontAwesomeIcon`
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `icon` | `IconDefinition` | ✅ | - | The Font Awesome icon to render. |
| `class` | `string` | - | - | Additional classes to apply to the SVG. |
### `FontAwesomeLayer`
| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| 'icons' | `Array<{ icon: IconDefinition, class?: string }>` | ✅ | - | An array of icons to layer. Each icon can have its own class. |
| `class` | `string` | - | - | Additional classes to apply to the SVG. |

## ⚙️ Configuration

### Font Awesome Setup

Create src/config/fontawesome.ts:
```ts
import { config } from '@fortawesome/fontawesome-svg-core';
import '@fortawesome/fontawesome-svg-core/styles.css';

// Configure Font Awesome
config.autoAddCss = false;
config.replacementClass = 'fa-svg';
```

### TypeScript Support

Add to your tsconfig.json:
```json
{
  "compilerOptions": {
    "types": ["font-awesome-astro/types"]
  }
}
```

## 🔍 Examples

### Animated Spinner

```js
---
import { FontAwesomeIcon } from 'font-awesome-astro';
import { faSpinner } from '@fortawesome/free-solid-svg-icons';
---

<FontAwesomeIcon 
  icon={faSpinner} 
  class="animate-spin text-2xl text-green-500"
/>
```
### Social Media Stack
```js
---
import { FontAwesomeLayer } from 'font-awesome-astro';
import { 
  faSquare,
  faTwitter,
  faGithub
} from '@fortawesome/free-brands-svg-icons';
---

<FontAwesomeLayer class="flex gap-4">
  {{
    icons: [
      { icon: faSquare, class: "text-blue-400 text-3xl" },
      { icon: faTwitter, class: "text-white text-xl -ml-3" },
      { icon: faSquare, class: "text-gray-800 text-3xl" },
      { icon: faGithub, class: "text-white text-xl -ml-3" }
    ]
  }}
</FontAwesomeLayer>
```

## ❓ FAQ

### Can I use Pro icons?
Yes, you can use Pro icons by installing the `@fortawesome/pro-svg-icons` package and importing the icons you need. Make sure to follow Font Awesome's licensing terms for Pro icons.
### How to handle SSR?
The package handles SSR automatically. Ensure you:
1. Disable auto CSS injection
2. Configure replacement classes
3. Import styles globally
### Can I use multiple icon styles?
Absolutely! Mix different styles in layers:
```js
import { faRegularClock } from '@fortawesome/pro-regular-svg-icons';
import { faSolidUser } from '@fortawesome/pro-solid-svg-icons';
```

### 🤝 Contributing
1. Fork the repository
2. Create feature branch: git checkout -b feature/amazing-feature
3. Commit changes: git commit -m 'Add amazing feature'
4. Push to branch: git push origin feature/amazing-feature
5. Open Pull Request

## 📄 License

MIT License ©Ihorok. Font Awesome icons are licensed under CC BY 4.0.

Made with ❤️ by Ihorok and Astro community.
Font Awesome by Fort Awesome.