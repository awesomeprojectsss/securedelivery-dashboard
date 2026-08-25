---
name: SecureDelivery
colors:
  surface: '#fcf8fa'
  surface-dim: '#dcd9db'
  surface-bright: '#fcf8fa'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f6f3f5'
  surface-container: '#f0edef'
  surface-container-high: '#eae7e9'
  surface-container-highest: '#e4e2e4'
  on-surface: '#1b1b1d'
  on-surface-variant: '#45464d'
  inverse-surface: '#303032'
  inverse-on-surface: '#f3f0f2'
  outline: '#76777d'
  outline-variant: '#c6c6cd'
  surface-tint: '#565e74'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#131b2e'
  on-primary-container: '#7c839b'
  inverse-primary: '#bec6e0'
  secondary: '#006a61'
  on-secondary: '#ffffff'
  secondary-container: '#86f2e4'
  on-secondary-container: '#006f66'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#271901'
  on-tertiary-container: '#98805d'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#dae2fd'
  primary-fixed-dim: '#bec6e0'
  on-primary-fixed: '#131b2e'
  on-primary-fixed-variant: '#3f465c'
  secondary-fixed: '#89f5e7'
  secondary-fixed-dim: '#6bd8cb'
  on-secondary-fixed: '#00201d'
  on-secondary-fixed-variant: '#005049'
  tertiary-fixed: '#fcdeb5'
  tertiary-fixed-dim: '#dec29a'
  on-tertiary-fixed: '#271901'
  on-tertiary-fixed-variant: '#574425'
  background: '#fcf8fa'
  on-background: '#1b1b1d'
  surface-variant: '#e4e2e4'
typography:
  display-lg:
    fontFamily: Inter
    fontSize: 32px
    fontWeight: '700'
    lineHeight: 40px
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.01em
  headline-sm:
    fontFamily: Inter
    fontSize: 18px
    fontWeight: '600'
    lineHeight: 28px
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
  label-caps:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.05em
  data-mono:
    fontFamily: JetBrains Mono
    fontSize: 13px
    fontWeight: '500'
    lineHeight: 18px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  base: 4px
  xs: 8px
  sm: 12px
  md: 16px
  lg: 24px
  xl: 32px
  container-max: 1440px
  gutter: 16px
---

## Brand & Style
The design system is engineered for high-stakes operational oversight in the B2B IoT logistics sector. It prioritizes **Professional Modernism**, balancing the rigorous demands of enterprise data density with a clean, approachable aesthetic. 

The visual narrative is built on "Confidence through Clarity." It utilizes a restrained color palette, systematic spacing, and functional typography to reduce cognitive load for dispatchers and fleet managers. The emotional response should be one of control, reliability, and precision.

**Core Principles:**
- **Information over Decoration:** Visual flourishes are minimized in favor of data legibility.
- **Systematic Order:** Heavy reliance on a structured grid to align complex telemetry data.
- **Operational Status:** Color is used sparingly and purposefully to signal system health and immediate action items.

## Colors
The palette is rooted in a "Deep Navy" primary tone to establish authority and professional trust. The "Teal" accent is used strictly for primary actions and interactive states, ensuring a high-contrast focal point against neutral backgrounds.

**Usage Guidelines:**
- **Primary (#0F172A):** Used for navigation sidebars, headers, and primary text to ground the interface.
- **Accent (#0D9488):** Reserved for "Commit" actions (buttons, toggles, active tabs) and critical path UI.
- **Semantic Colors:** Emerald, Amber, and Rose are reserved exclusively for status indicators and data visualization alerts. Do not use these for decorative purposes.
- **Neutrals:** A range of Slate grays (derived from #F8FAFC) is used to create hierarchical separation between surface containers and backgrounds.

## Typography
This design system utilizes **Inter** as the primary typeface for its exceptional legibility in dense SaaS environments. To assist in technical data monitoring (such as SmartBox IDs or sensor coordinates), **JetBrains Mono** is introduced for specific data-heavy labels.

**Hierarchical Rules:**
- **Display & Headlines:** Use tight letter-spacing and semi-bold weights to maintain a compact, professional look.
- **Body Text:** Standardized at 14px for enterprise density without sacrificing readability.
- **Data Mono:** Used for sensor IDs, timestamps, and coordinate data to ensure character differentiation (e.g., distinguishing 0 from O).

## Layout & Spacing
The layout employs a **12-column fluid grid** for desktop, optimized for a 1440px viewport. The system uses a strict 4px baseline grid to ensure vertical rhythm across compact data tables and dashboard modules.

**Breakpoints:**
- **Desktop (Default):** 12 columns, 24px margins, 16px gutters.
- **Tablet (1024px):** 8 columns, 16px margins, 16px gutters.
- **Mobile (375px):** 4 columns, 16px margins, 12px gutters.

**Density:**
For data-heavy views (Operational Monitoring), use "Compact" spacing (sm/xs) for table rows and list items to maximize information density on a single screen.

## Elevation & Depth
This design system uses a **Tonal Layering** approach combined with subtle shadows to define hierarchy. 

- **Level 0 (Background):** #F8FAFC - The canvas.
- **Level 1 (Cards/Surface):** White (#FFFFFF) with a 1px border (#E2E8F0).
- **Level 2 (Dropdowns/Modals):** White with a soft, diffused shadow: `0px 4px 6px -1px rgba(15, 23, 42, 0.1), 0px 2px 4px -2px rgba(15, 23, 42, 0.05)`.
- **Level 3 (Popovers/Tooltips):** #0F172A (Primary Navy) for high-contrast contextual information.

Shadows should never be "black"; they must be tinted with the Primary Navy color to maintain a cohesive, sophisticated look.

## Shapes
A consistent "Rounded" profile is applied across the system to soften the "industrial" nature of IoT data. 

- **Base Radius (8px):** Applied to buttons, input fields, and small cards.
- **Large Radius (16px):** Applied to main dashboard containers and modal windows.
- **Interactive States:** On hover, clickable areas may use a subtle background fill change rather than a shape change to maintain layout stability.

## Components
Consistent component behavior is vital for rapid operational response.

**Data Tables:**
- **Header:** Light gray fill (#F1F5F9), uppercase label-caps typography.
- **Rows:** 40px height for compact mode; alternating zebra stripes are prohibited; use 1px bottom borders instead.
- **Status Indicators:** A small dot (8px) accompanied by text in the semantic color.

**KPI Cards:**
- Top-aligned labels with large "data-mono" values.
- Trend indicators (up/down arrows) placed bottom-right.
- Border-left accent matching the status color if the KPI is in a warning state.

**Buttons:**
- **Primary:** Solid #0D9488 (Teal), White text.
- **Secondary:** Transparent with #E2E8F0 border, #0F172A text.
- **Tertiary:** Text-only with underline on hover.

**SmartBox Activation:**
- Uses a stepper component with "Teal" for completed states and "Navy" for the active state. High-contrast input fields with 1px #CBD5E1 borders that thicken to 2px #0D9488 on focus.

**Chat Interface:**
- Anchored to the bottom right. Uses a "Glassmorphism" header (backdrop-blur) to provide depth without obscuring underlying dashboard telemetry.