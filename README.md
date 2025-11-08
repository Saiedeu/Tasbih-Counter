Tasbih Counter - Digital Islamic Prayer Counter
Overview
Tasbih Counter is a progressive web application (PWA) designed to help Muslims perform dhikr (remembrance of Allah) by counting repetitive prayers like "Subhanallah," "Alhamdulillah," and "Allahu Akbar." The application provides a beautiful, mobile-friendly interface optimized for Bengali and English-speaking users, with a focus on accessibility and offline functionality.

The application is built as a single-page web app with no backend dependencies, making it lightweight and fast. It targets users primarily in Bangladesh and other Bengali-speaking regions, with multi-language support and a responsive design that works seamlessly on mobile devices.

Recent Changes
November 8, 2025 - Real-Time Counting Accuracy Fix
Issue: Users reported that rapid tapping (8-9 taps) only registered 2-3 counts, especially at group boundaries (33rd and 66th counts).

Root Cause:

isAnimating boolean throttle was blocking rapid taps during 400ms animation
600ms setTimeout delay at group boundaries (33→34, 66→67) was dropping tap inputs
Counter returned early when hitting group max, ignoring taps until timeout completed
Solution Implemented:

Removed isAnimating throttle mechanism completely from incrementCounter()
Replaced 600ms blocking setTimeout with immediate synchronous group transitions
Made group switching instant - when count reaches 33/66, next group activates immediately
Changed guard condition from count >= groupMaxes[currentTasbih] to totalCount >= 99 to allow continuous counting
Animation now runs in background without blocking new tap inputs
Original design and all features preserved - only JavaScript counting logic modified
Result: Counter now achieves perfect 1:1 tap-to-count accuracy at any speed, with no dropped inputs across all 99 counts.

User Preferences
Preferred communication style: Simple, everyday language.

System Architecture
Frontend Architecture
Single-Page Application (SPA)

The application is built as a standalone HTML/CSS/JavaScript application without framework dependencies
Pure vanilla JavaScript is used for all interactive functionality to minimize bundle size and maximize performance
The architecture follows a component-based mental model even without using a framework
Responsive Mobile-First Design

The UI is optimized for mobile devices with touch interactions
Uses viewport meta tags with user-scalable=no to prevent accidental zoom during counting
Implements touch-action: manipulation for better touch responsiveness
Disables text selection (user-select: none) to prevent interference with rapid tapping
Visual Design System

Custom CSS variable system for theming with light/dark mode support
Glass morphism effects using rgba transparency and backdrop filters
Gradient-based color scheme with primary colors (#43cea2 to #185a9d)
Poppins font family from Google Fonts for clean, modern typography
Font Awesome icons (v6.4.0) via CDN for UI elements
PWA Capabilities

Configured as a Progressive Web App with apple-mobile-web-app-capable and mobile-web-app-capable meta tags
Custom status bar styling for iOS devices with black-translucent theme
Installable on home screen with custom app icons and splash screens
Data Persistence
Client-Side Storage

Local Storage for persisting counter values, user preferences, and theme settings
No server-side database or backend required
All data remains on the user's device for privacy
Internationalization
Multi-Language Support

Primary language: Bengali (bn)
Secondary language: English
Content localized for Bangladesh (geo.region: BD, locale: bn_BD)
Bengali script throughout the UI with English fallbacks
SEO and Social Sharing
Enhanced Meta Tag System

Comprehensive Open Graph tags for Facebook/social media sharing
Twitter Card meta tags for rich previews
Structured data with multi-locale support (bn_BD, en_US)
Semantic HTML for search engine optimization
Descriptive keywords targeting Islamic prayer/dhikr use cases
Content Security Policy

CSP headers to allow CDN resources while maintaining security
Whitelisted domains: cdnjs.cloudflare.com, cdn.jsdelivr.net, code.jquery.com, Google Fonts
External Dependencies
CDN Resources
Font Awesome v6.4.0

Purpose: Icon library for UI elements (buttons, settings, counters)
Source: cdnjs.cloudflare.com
Rationale: Provides professional icons without local asset management
Google Fonts - Poppins

Purpose: Primary typeface for clean, modern typography
Weights: 300, 400, 500, 600, 700
Rationale: Web-safe font with excellent Bengali script support and readability
Hosting Environment
Static File Hosting

Hosted on rf.gd (free hosting platform)
No server-side processing required
Assets served from relative paths (../tasbeeh/assets/)
Browser APIs
Web APIs Used

Local Storage API for data persistence
Service Worker API (implied by PWA setup) for offline functionality
Touch Events API for mobile interaction
Viewport API for responsive behavior
Future Considerations
The application is designed to be fully client-side with no backend dependencies. If future features require server-side functionality (user accounts, cloud sync, analytics), the architecture would need to be extended with:

Backend API (Node.js/Express recommended for JavaScript consistency)
Database layer (could use Drizzle ORM with appropriate database)
Authentication service (OAuth or JWT-based)
However, the current design philosophy prioritizes simplicity, privacy, and offline-first functionality without external service dependencies.
