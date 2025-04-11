# Astro Component Library

This documentation provides an overview of our Astro component library, including usage examples, available props, and best practices.

## Table of Contents

- [Layout Components](#layout-components)
  - [MainLayout](#mainlayout)
  - [DashboardLayout](#dashboardlayout)
  - [BlogLayout](#bloglayout)
  - [AdminLayout](#adminlayout)
- [UI Components](#ui-components)
  - [Card](#card)
  - [Alert](#alert)
  - [Button](#button)
  - [ThemeToggle](#themetoggle)
  - [Link](#link)
- [Dashboard Components](#dashboard-components)
  - [AdminDashboard](#admindashboard)
  - [SecurityDashboard](#securitydashboard)
- [Utility Components](#utility-components)
  - [ErrorBoundary](#errorboundary)

## Layout Components

Layout components provide the structure for pages and handle common elements like headers, footers, and navigation.

### MainLayout

The primary layout for public-facing pages.

**File:** `src/layouts/MainLayout.astro`

**Props:**

```typescript
interface Props {
  title: string;
  description?: string;
  image?: string;
  canonicalURL?: string;
}
```

**Usage:**

```astro
---
import MainLayout from '@/layouts/MainLayout.astro';
---

<MainLayout
  title="Home Page"
  description="Welcome to our application"
>
  <div>
    <h1>Welcome</h1>
    <p>This is the home page content.</p>
  </div>
</MainLayout>
```

**Features:**

- Includes Header and Footer components
- Handles meta tags for SEO
- Integrates ViewTransitions for smooth page transitions
- Manages theme handling with CSS variables
- Sets up responsive viewports

### DashboardLayout

Layout for authenticated user dashboard pages.

**File:** `src/layouts/DashboardLayout.astro`

**Props:**

```typescript
interface Props {
  title: string;
  description?: string;
  requiresAuth?: boolean;
  showSidebar?: boolean;
}
```

**Usage:**

```astro
---
import DashboardLayout from '@/layouts/DashboardLayout.astro';
---

<DashboardLayout
  title="User Dashboard"
  requiresAuth={true}
  showSidebar={true}
>
  <div>
    <h1>Dashboard</h1>
    <p>Your dashboard content goes here.</p>
  </div>
</DashboardLayout>
```

**Features:**

- Includes ClientRouter for navigation without full page reloads
- Contains ErrorBoundary for graceful error handling
- Responsive design adapting to different screen sizes
- Proper meta tags for SEO and sharing
- Theme handling with CSS variables
- Smooth transitions between pages
- Enhanced accessibility features

### BlogLayout

Layout for blog posts and article pages.

**File:** `src/layouts/BlogLayout.astro`

**Props:**

```typescript
interface Props {
  title: string;
  description?: string;
  publishDate?: Date;
  author?: string;
  heroImage?: string;
  tags?: string[];
}
```

**Usage:**

```astro
---
import BlogLayout from '@/layouts/BlogLayout.astro';
---

<BlogLayout
  title="Understanding Mental Health"
  description="A comprehensive guide to mental health awareness"
  publishDate={new Date("2025-03-15")}
  author="Dr. Jane Smith"
  tags={["mental health", "wellness", "therapy"]}
>
  <article>
    <p>Blog content goes here...</p>
  </article>
</BlogLayout>
```

**Features:**

- Structured data for SEO and social sharing
- Tag display and filtering
- Author information
- Reading time estimation
- Related posts suggestions
- Integrates with content collections

### AdminLayout

Layout for admin dashboard and management pages.

**File:** `src/layouts/AdminLayout.astro`

**Props:**

```typescript
interface Props {
  title: string;
  description?: string;
  requiredPermission?: string;
}
```

**Usage:**

```astro
---
import AdminLayout from '@/layouts/AdminLayout.astro';
---

<AdminLayout
  title="User Management"
  requiredPermission="manage_users"
>
  <div>
    <h1>User Management</h1>
    <p>Admin content goes here...</p>
  </div>
</AdminLayout>
```

**Features:**

- Role-based access control
- Admin-specific navigation
- System status indicators
- Quick action buttons
- Enhanced security features

## UI Components

Reusable UI components for building interfaces.

### Card

Container component with a clean, bordered appearance.

**File:** `src/components/ui/Card.astro`

**Props:**

```typescript
interface Props extends HTMLAttributes<'div'> {
  class?: string;
}
```

**Usage:**

```astro
---
import Card from '@/components/ui/Card.astro';
import CardHeader from '@/components/ui/CardHeader.astro';
import CardTitle from '@/components/ui/CardTitle.astro';
import CardDescription from '@/components/ui/CardDescription.astro';
import CardContent from '@/components/ui/CardContent.astro';
import CardFooter from '@/components/ui/CardFooter.astro';
---

<Card>
  <CardHeader>
    <CardTitle>Card Title</CardTitle>
    <CardDescription>Card description goes here</CardDescription>
  </CardHeader>
  <CardContent>
    <p>Main content area</p>
  </CardContent>
  <CardFooter>
    <p>Footer content</p>
  </CardFooter>
</Card>
```

**Subcomponents:**

- `CardHeader`: Container for the card's header area
- `CardTitle`: Title element for the card
- `CardDescription`: Description text below the title
- `CardContent`: Main content area of the card
- `CardFooter`: Footer area, typically for actions or additional info
- `CardAction`: Action element positioned in the top-right corner

### Alert

Feedback component for displaying notifications.

**File:** `src/components/ui/Alert.astro`

**Props:**

```typescript
interface Props {
  variant?: 'info' | 'success' | 'warning' | 'error';
  title?: string;
  description?: string;
  icon?: string;
  actions?: string;
  dismissible?: boolean;
  class?: string;
}
```

**Usage:**

```astro
---
import Alert from '@/components/ui/Alert.astro';
---

<Alert
  variant="warning"
  title="Connection Issue"
  description="Your internet connection appears to be unstable."
  dismissible={true}
/>
```

**Features:**

- Four variants: info (default), success, warning, error
- Optional icon
- Dismissible option
- Action buttons support
- Accessible design with proper ARIA attributes

### Button

Interactive button component with multiple variants.

**File:** `src/components/ui/Button.astro`

**Props:**

```typescript
interface Props {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'link' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  type?: 'button' | 'submit' | 'reset';
  class?: string;
}
```

**Usage:**

```astro
---
import Button from '@/components/ui/Button.astro';
---

<Button variant="primary" size="md">
  Click Me
</Button>

<Button variant="outline" disabled={true}>
  Disabled Button
</Button>

<Button variant="danger" loading={true}>
  Delete
</Button>
```

**Features:**

- Multiple variants for different visual styles
- Three size options
- Loading state with spinner
- Disabled state
- Accessible focus states
- Custom class support for additional styling

### ThemeToggle

Button for switching between light, dark, and system themes.

**File:** `src/components/ui/ThemeToggle.astro`

**Props:**

```typescript
interface Props {
  class?: string;
}
```

**Usage:**

```astro
---
import ThemeToggle from '@/components/ui/ThemeToggle.astro';
---

<ThemeToggle />
```

**Features:**

- Cycles between light, dark, and system theme preferences
- Persists preference in localStorage
- Automatically applies theme on page load
- ARIA-labelled for accessibility
- Smooth transition between themes

### Link

Enhanced anchor component with external link handling.

**File:** `src/components/base/Link.astro`

**Props:**

```typescript
interface Props {
  href?: string;
  title?: string;
  rel?: string;
  external?: boolean;
  enableNewTabWarning?: boolean;
  class?: string;
}
```

**Usage:**

```astro
---
import Link from '@/components/base/Link.astro';
---

<Link href="/about">About Us</Link>

<Link 
  href="https://example.com" 
  external={true} 
  enableNewTabWarning={true}
>
  External Link
</Link>
```

**Features:**

- Automatic external link detection
- Optional warning icon for links opening in new tabs
- Title and accessibility attributes for better UX
- Support for named slots (title and end)
- Proper security attributes for external links

## Dashboard Components

Components specifically designed for dashboard and admin views.

### AdminDashboard

Main component for the admin dashboard.

**File:** `src/components/admin/AdminDashboard.astro`

**Props:**

None. This component fetches and displays system metrics automatically.

**Usage:**

```astro
---
import AdminDashboard from '@/components/admin/AdminDashboard.astro';
---

<AdminDashboard />
```

**Features:**

- Displays system metrics (active users, sessions, response time, etc.)
- Error handling for API failures
- Visual indicators for status
- Auto-updates metrics periodically
- Progress bars for capacity indicators

### SecurityDashboard

Dashboard for monitoring security events.

**File:** `src/components/security/SecurityDashboard.astro`

**Props:**

None. This component fetches and displays security events automatically.

**Usage:**

```astro
---
import SecurityDashboard from '@/components/security/SecurityDashboard.astro';
---

<SecurityDashboard />
```

**Features:**

- Real-time security event monitoring
- Filtering by event type and severity
- Statistical overview of events
- Color-coded severity indicators
- Auto-refreshes every 30 seconds
- Cleans up event listeners when component unmounts

## Utility Components

Helper components that provide additional functionality.

### ErrorBoundary

Catches and displays errors in a user-friendly way.

**File:** `src/components/base/ErrorBoundary.astro`

**Props:**

```typescript
interface Props {
  fallback?: string;
}
```

**Usage:**

```astro
---
import ErrorBoundary from '@/components/base/ErrorBoundary.astro';
---

<ErrorBoundary fallback="Something went wrong. Please try again later.">
  <ComponentThatMightError />
</ErrorBoundary>
```

**Features:**

- Catches JavaScript errors in child components
- Displays a friendly fallback message
- Prevents the entire page from crashing
- Logs errors to the console
- Support for custom fallback content

## Best Practices

### Component Composition

Combine components to create more complex interfaces:

```astro
<Card>
  <CardHeader>
    <CardTitle>User Statistics</CardTitle>
    <CardDescription>Overview of user activity</CardDescription>
    <CardAction>
      <ThemeToggle />
    </CardAction>
  </CardHeader>
  <CardContent>
    <Alert variant="info" title="Note">
      Data is updated every 5 minutes.
    </Alert>
    <!-- Content goes here -->
  </CardContent>
  <CardFooter>
    <Button variant="outline">Refresh</Button>
  </CardFooter>
</Card>
```

### Conditional Rendering

Use Astro's template syntax for conditional rendering:

```astro
---
const { showHeader = true, user } = Astro.props;
---

{showHeader && <header>Site Header</header>}

{user ? (
  <p>Welcome back, {user.name}!</p>
) : (
  <p>Please <Link href="/login">log in</Link></p>
)}
```

### Using Slots

Leverage Astro's slot system for flexible content placement:

```astro
---
// Component definition
---

<div class="container">
  <header>
    <slot name="header">Default Header</slot>
  </header>
  <main>
    <slot />
  </main>
  <footer>
    <slot name="footer">Default Footer</slot>
  </footer>
</div>
```

Usage:

```astro
<MyComponent>
  <h1 slot="header">Custom Header</h1>
  <p>Main content goes in the default slot</p>
  <div slot="footer">Custom Footer</div>
</MyComponent>
```

### Client Directives

When integrating React components, use the appropriate client directive:

```astro
---
import ReactComponent from './ReactComponent.jsx';
---

<ReactComponent client:load /> <!-- Load immediately -->
<ReactComponent client:visible /> <!-- Load when visible -->
<ReactComponent client:idle /> <!-- Load during browser idle time -->
```

## Testing Components

All components in the library should have corresponding tests. See the [Component Testing Guide](./component-testing.md) for details on how to test these components.

## Contributing New Components

When creating new components:

1. Follow the established folder structure
2. Include proper TypeScript types for props
3. Document props and usage
4. Create tests using Vitest
5. Follow the [React to Astro Conversion Guide](./react-to-astro-conversion.md) if converting from React

## See Also

- [Component Testing Guide](./component-testing.md)
- [React to Astro Conversion Guide](./react-to-astro-conversion.md)
- [API Documentation](./api.md)
- [Deployment Guide](./deployment.md)
