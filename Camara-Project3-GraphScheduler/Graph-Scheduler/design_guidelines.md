# Design Guidelines: Graph Coloring Course Scheduler

## Design Approach
**Selected System**: Material Design 3 with adaptations for data-heavy academic applications
**Justification**: Information-dense interface requiring clear hierarchy, excellent readability for schedules/tables, and strong component patterns for complex interactions.

## Core Design Elements

### Typography
- **Headings**: Inter or Roboto - Bold (600-700 weight)
  - H1: 2.5rem (page titles)
  - H2: 1.875rem (section headers)
  - H3: 1.5rem (subsection headers)
- **Body**: Inter or Roboto - Regular (400 weight), 1rem base size
- **Data/Tables**: Roboto Mono - Medium (500 weight), 0.875rem for course codes, time slots
- **Labels**: Inter - Medium (500 weight), 0.875rem

### Layout System
**Spacing Primitives**: Tailwind units of 2, 4, 6, and 8 for consistency
- Component padding: p-6 or p-8
- Section gaps: gap-6 or gap-8
- Card spacing: p-4 or p-6
- Tight groupings: space-y-2 or space-y-4

**Grid Structure**:
- Main layout: Two-column split on desktop (60/40 for graph/controls)
- Schedule tables: Full-width with responsive overflow
- Course cards: Grid of 2-3 columns (md:grid-cols-2 lg:grid-cols-3)

### Component Library

**Navigation**:
- Top app bar with application title, action buttons (Add Course, Generate Schedule, Export)
- Secondary tabs for switching views (Graph View, Schedule View, Conflicts View)

**Graph Visualization Panel**:
- Large canvas area (min-height: 600px) with zoom/pan controls
- Floating control panel overlay (top-right) with legend showing conflict types
- Node size: Medium (48-64px diameter) with course code centered
- Edge styling: Solid lines (2px width), dashed for different conflict types
- Interactive tooltips on hover showing full course details

**Course Management**:
- Compact form cards with clear field labels
- Input fields for: Course Code, Professor, Room, Students (comma-separated)
- Conflict type selector (checkboxes or multi-select dropdown)
- Action buttons grouped at bottom-right of forms

**Schedule Output**:
- Data table with fixed headers, alternating row backgrounds
- Columns: Course | Time Slot | Room | Professor | Conflicts
- Sortable column headers
- Highlight rows on hover for readability
- Sticky header when scrolling

**Room Allocation Display**:
- Matrix/grid view showing time slots (rows) × rooms (columns)
- Cell content: Course code when assigned
- Empty cells clearly differentiated
- Visual density: Compact but readable (min cell height: 56px)

**Conflict Indicators**:
- Badge components showing conflict count per course
- Warning/error states using elevation and border treatments
- Expandable conflict details (accordion pattern)

**Cards & Containers**:
- Elevated cards (shadow-md) for grouping related controls
- Outlined containers for data displays
- Rounded corners (rounded-lg) for modern feel

### Animations
**Minimal, Purposeful Use Only**:
- Smooth graph node transitions when recoloring (300ms ease)
- Subtle scale on card hover (scale-105, 200ms)
- Fade-in for newly added courses (opacity transition)
- **No**: Excessive scroll animations, loading spinners beyond necessity

### Accessibility
- High contrast ratios for all text (WCAG AA minimum)
- Focus indicators on all interactive elements (2px ring)
- Keyboard navigation for graph (arrow keys to traverse nodes)
- Screen reader labels for graph elements
- Clear error messaging in forms

## Layout Specifications

### Main Application Structure
**Three-Panel Layout**:
1. **Left Sidebar** (20% width, collapsible):
   - Course list with quick actions
   - Conflict statistics summary
   - Filter/search controls

2. **Center Canvas** (55% width):
   - Primary graph visualization
   - Toggleable between graph/schedule table views
   - Zoom controls (bottom-right corner)

3. **Right Panel** (25% width):
   - Course details/editing form
   - Live constraint violations
   - Real-time schedule preview

### Mobile Responsive Behavior
- Stack panels vertically on tablet/mobile
- Graph becomes full-width, collapsible panels
- Schedule table horizontal scroll with sticky first column

## Images
**No hero image required** - This is a utility application, not marketing-focused.

**Icon Usage**:
- Material Icons via CDN for UI actions (add, edit, delete, zoom, etc.)
- Course conflict type icons in legend (professor, room, student, time)
- Graph node icons optional (simple circle shapes preferred)

## Key Interaction Patterns
- **Drag-to-pan** graph canvas
- **Click node** to select course and show details in right panel
- **Double-click edge** to edit/remove conflict
- **Ctrl+click** multiple nodes to batch edit
- **Right-click** context menus on nodes for quick actions
- **Generate button** triggers coloring algorithm with loading state
- **Real-time validation** showing constraint violations as user edits

## Visual Hierarchy Principles
1. Graph visualization is hero element - largest screen real estate
2. Schedule output is secondary - accessed via tab/toggle
3. Course management tertiary - contextual panel
4. Clear separation between input (forms) and output (tables/graphs)
5. Action buttons prominent but not distracting (elevated, end-aligned)