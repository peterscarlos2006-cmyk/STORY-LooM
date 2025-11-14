# Storyloom Branding Guidelines

This document outlines the branding and UI guidelines for the Storyloom application. The goal is to create a consistent, calming, and inspiring user experience.

## 1. Logo and Name

The official name of the application is **Storyloom**. All references to the previous name, "ChronoMuse," must be removed from the codebase, documentation, and user-facing content.

The logo should be a minimalist and elegant design that reflects the creative and narrative nature of the application.

## 2. Color Palette

The primary color palette is a soft gradient that transitions from a deep indigo to a warm sand color. This gradient should be used sparingly, primarily for headlines, buttons, and other key UI elements.

- **Primary Gradient**: `#4B0082` (Indigo) to `#F4A460` (Sandy Brown)
- **Primary Text**: `#333333` (Dark Gray)
- **Secondary Text**: `#666666` (Medium Gray)
- **Background**: `#FFFFFF` (White) or a very light off-white.

## 3. Typography

- **Headlines**: A serif font with a classic and literary feel.
  - **Recommended Fonts**: Playfair Display, Lora, or Merriweather.
- **Body Text**: A clean and readable sans-serif font.
  - **Recommended Fonts**: Open Sans, Lato, or Nunito Sans.

## 4. Icons

Icons should be minimalist and line-based. Avoid heavy, filled-in icons. The icon set should be consistent throughout the application.

- **Recommended Icon Sets**: Feather Icons, Heroicons, or a custom-designed set.

## 5. "Calm Mode"

"Calm Mode" is an optional UI theme that users can enable to create a more muted and focused writing environment.

- **Color Palette**:
  - **Background**: A soft, light gray or beige.
  - **Text**: A darker gray for reduced contrast.
  - **Accent Colors**: Desaturated and muted versions of the primary color palette.
- **Motion**:
  - Reduce or disable animations and transitions.
  - UI elements should have a more static and stable feel.
- **Implementation**:
  - "Calm Mode" can be implemented using a CSS class that is applied to the root element of the application. This class will override the default styles with the "Calm Mode" styles.
