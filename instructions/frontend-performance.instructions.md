---
description: "Frontend performance optimization guidelines for bundle size, lazy loading, web vitals, and runtime performance"
applyTo: "**/*.{js,jsx,ts,tsx,vue,svelte}"
---

# Frontend Performance Optimization

Guidelines for building performant frontend applications with optimal bundle size, fast load times, and smooth runtime performance.

## General Instructions

- Measure before optimizing; use tools like Lighthouse, WebPageTest, and browser DevTools
- Prioritize Core Web Vitals (LCP, FID, CLS) for user experience
- Optimize bundle size through code splitting and tree shaking
- Implement progressive loading strategies
- Monitor performance metrics continuously
- Balance performance with maintainability

## Best Practices

### Bundle Size Optimization

- Use code splitting to break large bundles into smaller chunks
- Implement route-based lazy loading for pages and features
- Tree-shake unused code by using ES modules and proper imports
- Analyze bundle composition with webpack-bundle-analyzer or similar tools
- Remove unused dependencies and dead code
- Use production builds with minification and compression
- Target specific browsers to avoid unnecessary polyfills
- Consider using smaller alternative libraries (date-fns vs moment, preact vs react)

### Code Splitting Strategies

- Split by route using dynamic imports for page components
- Split by feature for large, rarely-used functionality
- Split vendor code separately from application code
- Lazy load components below the fold or on interaction
- Preload critical chunks that are likely to be needed soon
- Prefetch resources for anticipated navigation
- Use framework lazy loading capabilities
- Avoid over-splitting; too many small chunks increase overhead

### Image and Media Optimization

- Use modern formats (WebP, AVIF) with fallbacks
- Implement responsive images with srcset and sizes attributes
- Use framework image components when available
- Lazy load images below the fold
- Set explicit width and height to prevent layout shift
- Compress images without visible quality loss
- Use CDN for image delivery and transformation
- Consider using blur-up or skeleton placeholders
- Defer loading of non-critical videos

### JavaScript Loading Strategies

- Place non-critical scripts at the end of body or use defer/async attributes
- Minimize main thread work by deferring non-essential JavaScript
- Use web workers for expensive computations
- Implement virtual scrolling for long lists
- Debounce or throttle expensive event handlers (scroll, resize, input)
- Use requestIdleCallback for non-urgent work
- Minimize JavaScript execution time in each frame (target 16ms for 60fps)

### CSS Optimization

- Extract and inline critical CSS for above-the-fold content
- Load non-critical CSS asynchronously
- Minimize CSS bundle size by removing unused styles
- Use CSS-in-JS with proper optimization when beneficial
- Avoid CSS @import; use build tools to concatenate
- Minimize reflows and repaints by batching DOM changes
- Use CSS containment to limit layout scope
- Prefer transform and opacity for animations (GPU-accelerated)

### Web Vitals Optimization

**Largest Contentful Paint (LCP) - Target < 2.5s:**

- Optimize server response time (TTFB)
- Eliminate render-blocking resources
- Optimize and compress images
- Preload critical resources
- Use CDN for static assets
- Implement proper caching strategies

**First Input Delay (FID) - Target < 100ms:**

- Minimize JavaScript execution time
- Break up long tasks into smaller chunks
- Use web workers for heavy computations
- Defer non-essential JavaScript
- Optimize event handlers

**Cumulative Layout Shift (CLS) - Target < 0.1:**

- Set explicit dimensions for images and embeds
- Avoid inserting content above existing content
- Use transform for animations instead of layout properties
- Preload fonts to prevent font swap
- Reserve space for dynamic content (ads, embeds)

### Caching Strategies

- Implement proper HTTP caching headers (Cache-Control, ETag)
- Use service workers for offline-first experiences
- Cache static assets with long expiration times
- Implement cache busting with content hashes in filenames
- Use stale-while-revalidate for non-critical resources
- Cache API responses appropriately (client-side or service worker)
- Implement versioned cache invalidation strategies
- Use CDN caching for global distribution

### Network Optimization

- Minimize number of requests through bundling and spriting
- Use HTTP/2 or HTTP/3 for multiplexing
- Implement resource hints (preload, prefetch, preconnect, dns-prefetch)
- Compress responses with gzip or brotli
- Minimize cookie size for frequently accessed domains
- Use domain sharding only if beneficial for HTTP/1.1
- Implement efficient polling or use WebSockets/Server-Sent Events
- Batch API requests when possible

### Runtime Performance

- Use memoization and computed properties to prevent unnecessary re-renders
- Implement proper key attributes in lists
- Avoid inline function definitions in render methods
- Use production builds in production
- Profile with browser DevTools and framework-specific tools
- Minimize DOM manipulation; batch changes when needed
- Use requestAnimationFrame for animations
- Implement efficient state updates (immutable patterns)
- Avoid memory leaks by cleaning up subscriptions and timers

### Font Loading

- Use font-display: swap for custom fonts to prevent FOIT (Flash of Invisible Text)
- Subset fonts to include only needed characters
- Preload critical fonts used above the fold
- Use system fonts when appropriate for instant rendering
- Implement FOUT (Flash of Unstyled Text) mitigation with fallback fonts
- Host fonts locally or use optimized font CDNs
- Limit number of font variants and weights

### Third-Party Scripts

- Load third-party scripts asynchronously
- Defer non-critical third-party scripts
- Evaluate impact of each third-party script on performance
- Use facade pattern for heavy embeds (YouTube, maps)
- Implement resource budgets for third-party code
- Monitor third-party script performance over time
- Consider self-hosting critical third-party resources

### Progressive Enhancement

- Ensure core functionality works without JavaScript
- Load enhancements progressively as resources become available
- Implement server-side rendering or static generation when beneficial
- Use streaming SSR for faster time-to-interactive
- Implement proper loading states and skeleton screens
- Design for slower networks and devices

## Code Standards

### Performance Budget

Define and enforce performance budgets:

- **JavaScript**: Target < 200KB (gzipped) for initial bundle
- **CSS**: Target < 50KB (gzipped)
- **Images**: Optimize to appropriate quality; lazy load below fold
- **LCP**: < 2.5 seconds
- **FID**: < 100 milliseconds
- **CLS**: < 0.1
- **TTI**: < 5 seconds on 3G networks

### Measuring Performance

- Use Lighthouse for audits (target score > 90)
- Monitor Real User Metrics (RUM) in production
- Track Core Web Vitals with tools like web-vitals library
- Profile rendering performance with browser DevTools
- Analyze bundle size after every build
- Set up performance monitoring (Sentry, New Relic, DataDog)
- Test on real devices and network conditions

## Validation and Verification

- Run Lighthouse audits regularly and track scores
- Monitor bundle size with each build (CI integration)
- Test on slow 3G network throttling
- Verify Core Web Vitals meet targets
- Profile with browser DevTools Performance tab
- Test on low-end devices (not just high-end)
- Measure real user metrics in production
- Set up alerts for performance regressions
