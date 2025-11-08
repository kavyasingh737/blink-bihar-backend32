# Blink Bihar Design Guidelines

## Design Approach
**Reference-Based**: Drawing inspiration from Blinkit, Zepto, Swiggy Instamart, and Amazon for quick commerce patterns. Focus on speed, clarity, and efficient product discovery.

## Typography System
- **Primary Font**: Inter (Google Fonts) for UI elements and body text
- **Display Font**: Poppins (Google Fonts) for headings and emphasis
- **Hierarchy**:
  - Hero/Page Titles: text-4xl to text-5xl (Poppins, font-semibold)
  - Section Headers: text-2xl to text-3xl (Poppins, font-semibold)
  - Card Titles: text-lg (Inter, font-medium)
  - Body Text: text-base (Inter, font-normal)
  - Captions/Labels: text-sm (Inter, font-medium)
  - Price Tags: text-xl (Poppins, font-bold)

## Layout System
**Spacing Primitives**: Use Tailwind units of 2, 4, 8, 12, 16 (e.g., p-4, gap-8, mt-12)

**Grid Patterns**:
- Product Grids: grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4
- Dashboard Cards: grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6
- Vendor List: grid-cols-1 md:grid-cols-2 gap-4
- Container: max-w-7xl mx-auto px-4

## Component Library

### Navigation
**Top Header** (sticky):
- Left: Logo + Location selector (with icon)
- Center: Search bar (prominent, w-full max-w-2xl)
- Right: Cart icon with badge, Profile dropdown
- Height: h-16, shadow-md

**Category Bar** (horizontal scroll on mobile):
- Pill-style category chips
- gap-2, overflow-x-auto, no-scrollbar
- Active state with different styling

### Product Cards
- **Structure**: Image (aspect-square), Title, Weight/Quantity, Price, Add button
- **Layout**: Rounded-lg, border, p-3, hover:shadow-lg transition
- **Image**: object-cover, rounded-t-lg
- **Add Button**: Positioned at bottom, full-width or icon-based
- **Stock Badge**: Absolute positioned on image when low stock

### Vendor Cards
- **Structure**: Cover image (aspect-video), Logo (absolute, rounded-full), Name, Rating, Delivery time, Categories
- **Layout**: Rounded-xl, overflow-hidden, shadow-md
- **Info Section**: p-4, space-y-2

### Cart & Checkout
- **Cart Sidebar**: Fixed right, h-full, w-96, shadow-2xl, slide-in animation
- **Cart Items**: List with quantity controls, remove button, running total
- **Sticky Footer**: Total + Checkout button

### Dashboards (Vendor/Rider/Admin)
**Sidebar Navigation** (desktop):
- Fixed left, w-64, h-screen
- Logo at top
- Navigation items with icons (Heroicons), text-sm font-medium
- Active state with background highlight
- Mobile: Hamburger menu with slide-out drawer

**Stats Cards**:
- Grid layout (mentioned above)
- Rounded-lg, p-6, shadow
- Icon (text-3xl) + Label (text-sm) + Value (text-2xl font-bold)
- Trend indicators where applicable

**Data Tables**:
- Striped rows, hover states
- Sticky headers
- Action buttons (icon-only or text-icon combination)
- Pagination at bottom
- Responsive: Card view on mobile

### Forms
- **Input Fields**: rounded-lg, border, p-3, focus:ring-2
- **Labels**: text-sm font-medium, mb-2
- **Buttons**: 
  - Primary: rounded-lg, px-6, py-3, font-semibold
  - Secondary: outlined variant
  - Icon buttons: rounded-full, p-2
- **Spacing**: space-y-4 between form groups

### Order Tracking
- **Timeline View**: Vertical stepper with icons
- **Status Cards**: Large, prominent current status
- **Map Integration**: Placeholder for rider location (aspect-video, rounded-lg)
- **Delivery Info**: Card with estimated time, rider details, contact button

### Authentication Pages
- **Layout**: Centered card (max-w-md), minimal surrounding content
- **Structure**: Logo, Title, Form, Social auth buttons (if any), Footer links
- **Role Selector**: Radio group or segmented control for role selection

## Icons
**Library**: Heroicons (via CDN)
- Navigation: outline variant, w-6 h-6
- Actions: solid variant, w-5 h-5
- Category icons: w-8 h-8
- Status indicators: w-4 h-4

## Images
**Home Page Hero**: Full-width banner (aspect-[3/1] md:aspect-[4/1]) showing fresh groceries/delivery theme with overlaid search and location selector. Buttons on hero should have backdrop-blur-md backgrounds.

**Product Images**: Use placeholder images via Unsplash API or Lorem Picsum (e.g., `https://source.unsplash.com/400x400/?grocery,${productName}`)

**Vendor Cover Images**: aspect-video, 800x450px minimum

**Empty States**: Illustrations for empty cart, no orders, no vendors in area

## Responsive Behavior
- **Mobile First**: Stack columns, full-width buttons, bottom nav for dashboards
- **Breakpoints**: 
  - Mobile: < 768px
  - Tablet: 768px - 1024px
  - Desktop: > 1024px
- **Navigation**: Hamburger menu on mobile, full nav on desktop
- **Search**: Full-width on mobile, contained on desktop
- **Product Grid**: 2 cols mobile → 3-4 cols tablet → 4-5 cols desktop

## Interaction Patterns
- **Loading States**: Skeleton screens for product grids, shimmer effect
- **Transitions**: transition-all duration-200 for hover states
- **Modals**: Backdrop blur, centered, slide-up animation on mobile
- **Toasts**: Fixed top-right, slide-in animation, auto-dismiss
- **Infinite Scroll**: For product listings and order history

## Accessibility
- Focus rings on all interactive elements (focus:ring-2)
- ARIA labels for icon-only buttons
- Proper heading hierarchy (h1 → h2 → h3)
- Contrast ratios maintained across all text elements
- Keyboard navigation support for all critical flows