# CSS Modules Cheatsheet

*A comprehensive reference for CSS Modules - locally scoped CSS*

📚 **Official Repository:** <a href="https://github.com/css-modules/css-modules" target="_blank">CSS Modules GitHub</a>  
📖 **ICSS Specification:** <a href="https://github.com/css-modules/icss" target="_blank">Interoperable CSS</a>

## Table of Contents

### 🚀 Getting Started
- [What are CSS Modules?](#what-are-css-modules)
- [Basic Usage](#basic-usage)
- [Class Name Composition](#class-name-composition)

### 🎯 Core Features
- [Global Styles](#global-styles)
- [Values and Variables](#values-and-variables)
- [Advanced Patterns](#advanced-patterns)

### 🔧 Framework Integration
- [React](#react)
- [Next.js](#nextjs)
- [Vite](#vite)
- [Webpack](#webpack)

### 🛠️ Development & Tools
- [Build Tool Configuration](#build-tool-configuration)
- [TypeScript Support](#typescript-support)
- [Debugging and Development](#debugging-and-development)
- [Testing CSS Modules](#testing-css-modules)

### 📋 Best Practices & Patterns
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)

### 📚 Resources
- [Quick Reference Links](#quick-reference-links)

---

## What are CSS Modules?

CSS Modules are CSS files where all class names and animation names are **scoped locally by default**. They compile to ICSS (Interoperable CSS) format and solve the global scope problem in CSS by making styles modular and component-scoped.

**Key Benefits:**
- 🔒 **Local Scope** - Prevents style clashes across components
- 📝 **Clear Dependencies** - Explicit style imports show which styles affect which components  
- 🚫 **No Global Conflicts** - Styles are isolated to specific modules
- 🔄 **Reusability** - Same class names can be used across different modules

## Basic Usage

📖 <a href="https://github.com/css-modules/css-modules#specification" target="_blank">CSS Modules Specification</a>

```css
/* Button.module.css */
.button {
  background-color: #007bff;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.primary {
  background-color: #007bff;
}

.secondary {
  background-color: #6c757d;
}

.large {
  padding: 12px 24px;
  font-size: 18px;
}
```

```javascript
// Button.js
import styles from './Button.module.css';

function Button({ children, variant = 'primary', size }) {
  const buttonClass = [
    styles.button,
    styles[variant],
    size && styles[size]
  ].filter(Boolean).join(' ');

  return <button className={buttonClass}>{children}</button>;
}

// Generated class names might look like:
// "Button_button__2D1aX Button_primary__3kF9s"
```

## Class Name Composition

📖 <a href="https://github.com/css-modules/css-modules#composition" target="_blank">Composition Documentation</a>

```css
/* styles.module.css */
.base {
  padding: 10px;
  margin: 5px;
  border-radius: 4px;
}

.primary {
  composes: base;
  background-color: #007bff;
  color: white;
}

.secondary {
  composes: base;
  background-color: #6c757d;
  color: white;
}

/* Compose from external files */
.button {
  composes: button from './common.module.css';
  border: 2px solid #333;
}

/* Multiple compositions */
.submitButton {
  composes: base primary;
  font-weight: bold;
}
```

```javascript
import styles from './styles.module.css';

// styles.primary includes both 'base' and 'primary' styles
<button className={styles.primary}>Primary Button</button>
```

## Global Styles

📖 <a href="https://github.com/css-modules/css-modules#exceptions" target="_blank">Global Scope Documentation</a>

```css
/* styles.module.css */

/* Local by default */
.header {
  font-size: 24px;
}

/* Explicit global scope */
:global(.app-container) {
  max-width: 1200px;
  margin: 0 auto;
}

/* Global class within local context */
.navigation :global(.active) {
  font-weight: bold;
}

/* Switch to global scope for multiple rules */
:global {
  .reset {
    margin: 0;
    padding: 0;
  }
  
  .clearfix::after {
    content: "";
    display: table;
    clear: both;
  }
}

/* Local animations */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.fadeInElement {
  animation: fadeIn 0.3s ease-in;
}

/* Global animations */
:global {
  @keyframes globalPulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
  }
}
```

## Values and Variables

📖 <a href="https://github.com/css-modules/css-modules#values-variables" target="_blank">Values Documentation</a>

```css
/* values.module.css */
@value primary: #007bff;
@value secondary: #6c757d;
@value success: #28a745;
@value danger: #dc3545;
@value warning: #ffc107;

@value small: 8px;
@value medium: 16px;
@value large: 24px;

/* Export values for other modules */
@value primary, secondary from './colors.module.css';
@value small as smallPadding, medium as mediumPadding from './spacing.module.css';
```

```css
/* component.module.css */
@value colors: './values.module.css';
@value primary, danger from colors;

.button {
  background-color: primary;
  padding: smallPadding mediumPadding;
}

.errorButton {
  background-color: danger;
  color: white;
}
```

## Advanced Patterns

### Conditional Classes

```javascript
import styles from './Component.module.css';
import classNames from 'classnames'; // Popular utility library

function Component({ isActive, variant, disabled }) {
  return (
    <div 
      className={classNames(
        styles.base,
        styles[variant],
        {
          [styles.active]: isActive,
          [styles.disabled]: disabled
        }
      )}
    >
      Content
    </div>
  );
}

// Or with template strings
function Component({ isActive, variant }) {
  const className = `${styles.base} ${styles[variant]} ${isActive ? styles.active : ''}`.trim();
  return <div className={className}>Content</div>;
}
```

### Dynamic Class Names

```css
/* theme.module.css */
.light {
  background: white;
  color: black;
}

.dark {
  background: #333;
  color: white;
}

.blue {
  background: #007bff;
  color: white;
}

.red {
  background: #dc3545;
  color: white;
}
```

```javascript
import styles from './theme.module.css';

function ThemedComponent({ theme = 'light' }) {
  return (
    <div className={styles[theme]}>
      Themed content
    </div>
  );
}

// With fallback
function SafeThemedComponent({ theme }) {
  const themeClass = styles[theme] || styles.light;
  return <div className={themeClass}>Content</div>;
}
```

### CSS Custom Properties with Modules

```css
/* component.module.css */
.container {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --border-radius: 4px;
}

.button {
  background-color: var(--primary-color);
  border-radius: var(--border-radius);
  color: white;
}

.card {
  border: 1px solid var(--secondary-color);
  border-radius: var(--border-radius);
}
```

```javascript
import styles from './component.module.css';

function Component({ primaryColor = '#007bff' }) {
  return (
    <div 
      className={styles.container}
      style={{ '--primary-color': primaryColor }}
    >
      <button className={styles.button}>Button</button>
      <div className={styles.card}>Card</div>
    </div>
  );
}
```

## Framework Integration

### React

```javascript
// Basic usage
import styles from './Component.module.css';

function Component() {
  return <div className={styles.container}>Content</div>;
}

// With TypeScript
interface Props {
  variant: 'primary' | 'secondary';
}

const Component: React.FC<Props> = ({ variant }) => {
  return <div className={styles[variant]}>Content</div>;
};
```

### Next.js

📖 <a href="https://nextjs.org/docs/app/building-your-application/styling/css-modules" target="_blank">Next.js CSS Modules</a>

```javascript
// pages/index.js or app/page.js
import styles from './page.module.css';

export default function Page() {
  return <div className={styles.container}>Hello Next.js</div>;
}

// Next.js automatically handles CSS Modules for files ending in .module.css
```

### Vite

📖 <a href="https://vitejs.dev/guide/features.html#css-modules" target="_blank">Vite CSS Modules</a>

```javascript
// vite.config.js
export default {
  css: {
    modules: {
      localsConvention: 'camelCase', // Convert kebab-case to camelCase
      generateScopedName: '[name]__[local]___[hash:base64:5]'
    }
  }
}

// Usage with camelCase conversion
import styles from './component.module.css';
// .my-class becomes styles.myClass
```

### Webpack

📖 <a href="https://webpack.js.org/loaders/css-loader/#modules" target="_blank">Webpack CSS Modules</a>

```javascript
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.module\.css$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                localIdentName: '[path][name]__[local]--[hash:base64:5]'
              }
            }
          }
        ]
      }
    ]
  }
};
```

## Build Tool Configuration

### PostCSS

```javascript
// postcss.config.js
module.exports = {
  plugins: [
    require('postcss-modules')({
      generateScopedName: '[name]__[local]___[hash:base64:5]',
      hashPrefix: 'prefix',
      localsConvention: 'camelCaseOnly'
    }),
    require('autoprefixer')
  ]
};
```

### CSS Modules with Sass

```scss
/* component.module.scss */
$primary-color: #007bff;
$border-radius: 4px;

.container {
  background: $primary-color;
  border-radius: $border-radius;
  
  .header {
    font-weight: bold;
    margin-bottom: 1rem;
  }
  
  &.large {
    padding: 2rem;
  }
}

// Nested composition
.button {
  composes: button from './base.module.scss';
  
  &:hover {
    opacity: 0.8;
  }
}
```

## TypeScript Support

📖 <a href="https://github.com/css-modules/css-modules#typescript" target="_blank">TypeScript CSS Modules</a>

```typescript
// types/css-modules.d.ts
declare module '*.module.css' {
  const classes: { [key: string]: string };
  export default classes;
}

declare module '*.module.scss' {
  const classes: { [key: string]: string };
  export default classes;
}
```

```typescript
// Generate type definitions automatically
// Use typescript-plugin-css-modules or similar tools

// component.module.css.d.ts (auto-generated)
export interface Styles {
  container: string;
  button: string;
  primary: string;
}

export type ClassNames = keyof Styles;
declare const styles: Styles;
export default styles;
```

```typescript
// Component.tsx
import styles from './component.module.css';

interface Props {
  variant: keyof typeof styles; // Type-safe class names
}

const Component: React.FC<Props> = ({ variant }) => {
  return <div className={styles[variant]}>Content</div>;
};
```

## Best Practices

### File Organization

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.module.css
│   │   └── index.js
│   └── Card/
│       ├── Card.jsx
│       ├── Card.module.css
│       └── index.js
├── styles/
│   ├── globals.css
│   ├── variables.module.css
│   └── mixins.module.css
└── utils/
    └── classNames.js
```

### Naming Conventions

```css
/* Good: Descriptive, component-scoped names */
.navigationHeader { }
.primaryButton { }
.userAvatar { }
.formFieldError { }

/* Avoid: Generic names that might conflict */
.header { } /* Better: .componentHeader */
.button { } /* Better: .actionButton */
.text { }   /* Better: .bodyText */
```

### Class Name Utility

```javascript
// utils/classNames.js
export function cn(...classes) {
  return classes.filter(Boolean).join(' ');
}

// Usage
import { cn } from '../utils/classNames';
import styles from './Component.module.css';

function Component({ isActive, variant }) {
  return (
    <div className={cn(
      styles.base,
      styles[variant],
      isActive && styles.active
    )}>
      Content
    </div>
  );
}
```

### Performance Considerations

```css
/* Efficient: Avoid deep nesting */
.card { }
.cardHeader { }
.cardContent { }

/* Less efficient: Deep nesting */
.card .header .title .text { }

/* Good: Use composition for variants */
.baseButton {
  padding: 8px 16px;
  border: none;
  cursor: pointer;
}

.primaryButton {
  composes: baseButton;
  background: #007bff;
  color: white;
}
```

## Debugging and Development

### Class Name Debugging

```javascript
// Development helper to see generated class names
if (process.env.NODE_ENV === 'development') {
  console.log('Available styles:', Object.keys(styles));
}

// Runtime class name inspection
function Component() {
  const className = styles.button;
  console.log('Generated class name:', className);
  
  return <button className={className}>Button</button>;
}
```

### Browser DevTools

```css
/* Use meaningful local names for easier debugging */
.userProfileHeader { /* Better than .a1b2c3 */ }
.navigationMenuItem { /* Better than .x9y8z7 */ }

/* Add comments for complex compositions */
.complexButton {
  composes: baseButton;
  composes: primaryVariant;
  /* Additional styles for special use case */
  border: 2px solid transparent;
}
```

## Common Patterns

### Theme Implementation

```css
/* themes/light.module.css */
.theme {
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #212529;
  --text-secondary: #6c757d;
}

/* themes/dark.module.css */
.theme {
  --bg-primary: #212529;
  --bg-secondary: #343a40;
  --text-primary: #ffffff;
  --text-secondary: #adb5bd;
}
```

```javascript
import lightTheme from './themes/light.module.css';
import darkTheme from './themes/dark.module.css';
import styles from './Component.module.css';

function ThemedApp({ isDark }) {
  const theme = isDark ? darkTheme : lightTheme;
  
  return (
    <div className={`${theme.theme} ${styles.app}`}>
      <header className={styles.header}>Header</header>
      <main className={styles.content}>Content</main>
    </div>
  );
}
```

### Responsive Design

```css
/* responsive.module.css */
.container {
  padding: 1rem;
}

@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}

@media (min-width: 1024px) {
  .container {
    padding: 3rem;
    max-width: 1200px;
    margin: 0 auto;
  }
}

.responsiveGrid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 768px) {
  .responsiveGrid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .responsiveGrid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## Testing CSS Modules

### Jest Configuration

```javascript
// jest.config.js
module.exports = {
  moduleNameMapping: {
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy'
  },
  // Or use a custom mock
  '\\.(css|less|scss|sass)$': '<rootDir>/__mocks__/styleMock.js'
};

// __mocks__/styleMock.js
module.exports = {};

// Or for more realistic testing
module.exports = new Proxy({}, {
  get: function getter(target, key) {
    if (key === '__esModule') {
      return false;
    }
    return key;
  }
});
```

### Testing Components with CSS Modules

```javascript
// Component.test.js
import { render, screen } from '@testing-library/react';
import Component from './Component';

test('applies correct CSS classes', () => {
  render(<Component variant="primary" />);
  
  const element = screen.getByRole('button');
  
  // Test that classes are applied (will be hashed in production)
  expect(element).toHaveClass(expect.stringMatching(/button/));
  expect(element).toHaveClass(expect.stringMatching(/primary/));
});
```

---

## Quick Reference Links

### Essential Resources
- 📚 <a href="https://github.com/css-modules/css-modules" target="_blank">CSS Modules Repository</a> - Main documentation
- 🔧 <a href="https://github.com/webpack-contrib/css-loader" target="_blank">CSS Modules Loader</a> - Webpack integration
- 📖 <a href="https://github.com/css-modules/icss" target="_blank">ICSS Specification</a> - Low-level format
- 🎯 <a href="https://github.com/madyankin/postcss-modules" target="_blank">PostCSS Modules</a> - PostCSS plugin

### Framework Documentation
- ⚛️ <a href="https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/" target="_blank">React CSS Modules</a> - Create React App
- 🚀 <a href="https://nextjs.org/docs/app/building-your-application/styling/css-modules" target="_blank">Next.js CSS Modules</a> - Next.js integration
- ⚡ <a href="https://vitejs.dev/guide/features.html#css-modules" target="_blank">Vite CSS Modules</a> - Vite configuration
- 🔥 <a href="https://angular.io/guide/component-styles" target="_blank">Angular CSS Modules</a> - Angular integration

### Tools & Utilities
- 🎨 <a href="https://github.com/JedWatson/classnames" target="_blank">classnames</a> - Conditional class utility
- 📝 <a href="https://github.com/Quramy/typescript-plugin-css-modules" target="_blank">TypeScript CSS Modules</a> - TypeScript support
- 🔍 <a href="https://github.com/css-modules/css-modules-examples" target="_blank">CSS Modules Examples</a> - Example implementations
- 🧪 <a href="https://github.com/css-modules/postcss-modules-values" target="_blank">CSS Modules Values</a> - Values plugin

### Learning Resources
- 📚 <a href="https://glenmaddern.com/articles/css-modules" target="_blank">CSS Modules Introduction</a> - Glen Maddern's intro article
- 🎓 <a href="https://css-tricks.com/css-modules-part-1-need/" target="_blank">CSS Modules Tutorial</a> - CSS-Tricks series
- 💡 <a href="https://github.com/css-modules/css-modules/blob/master/docs/css-modules-vs-styled-components.md" target="_blank">CSS Modules vs Styled Components</a> - Comparison guide
- 🔄 <a href="https://github.com/css-modules/css-modules/blob/master/docs/migration.md" target="_blank">Migration Guide</a> - From global CSS

---

*This cheatsheet covers CSS Modules core features and modern tooling. For the latest updates, refer to the <a href="https://github.com/css-modules/css-modules" target="_blank">official repository</a>.*