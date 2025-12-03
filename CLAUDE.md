# CLAUDE.md - Doom Timer Project Guide

## Project Overview

**Doom Timer** is a single-page countdown timer application that tracks multiple critical IT-related deadlines and end-of-support dates. The application displays real-time countdowns with visual urgency indicators and business day calculations.

**Repository**: PNelsonFTP/doom-timer
**Primary File**: `index.html` (single self-contained HTML file)
**Technology**: Pure HTML5, CSS3, and vanilla JavaScript (no dependencies, frameworks, or build tools)
**Deployment**: Static HTML file (can be hosted on GitHub Pages or any static hosting)

## Project Purpose

This tool serves as a visual countdown for important IT deadlines, including:
- Software end-of-support dates (Windows 10, Exchange 2019, etc.)
- Fiscal year deadlines (Cisco End of Year)
- Personal milestones (New Years Eve)

The countdown timer helps teams track urgency and plan transitions for critical IT infrastructure changes.

## Codebase Structure

```
doom-timer/
├── .github/
│   └── workflows/
│       └── blank.yml          # Basic GitHub Actions workflow (placeholder)
├── .git/                      # Git repository
└── index.html                 # Main application (self-contained)
```

### Architecture

The entire application is contained in a **single HTML file** (`index.html`) with three main sections:

1. **HTML Structure** (lines 1-184):
   - Responsive container layout
   - Header with title and subtitle
   - Countdown container (dynamically populated)
   - Current time display footer

2. **CSS Styling** (lines 10-168):
   - Mobile-first responsive design
   - Dark gradient theme with red accents
   - Color-coded urgency states
   - Pulse animation for critical countdowns
   - iOS/mobile-optimized viewport settings

3. **JavaScript Logic** (lines 186-439):
   - `MobileDoomTimer` class handles all countdown logic
   - Event data array with deadline configurations
   - Time calculation and display updates (every 1 second)
   - Business days calculation (excludes weekends)
   - Dynamic color coding based on urgency
   - Comprehensive error handling for mobile devices

## Key Components

### Event Configuration (lines 198-218)

Events are defined in the `this.events` array within the `MobileDoomTimer` constructor:

```javascript
{
    name: 'Event Name',
    date: new Date('YYYY-MM-DDTHH:MM:SS'),
    description: 'Event description'
}
```

Events are automatically sorted by date (earliest first).

### Urgency Color Coding (lines 377-423)

Visual urgency is indicated by color-coded borders and backgrounds:

- **RED**: ≤7 days remaining (critical, pulsing animation)
- **ORANGE**: ≤30 days remaining (high urgency)
- **YELLOW**: ≤90 days remaining (medium urgency)
- **GREEN**: >90 days remaining (low urgency)

### Business Days Calculation (lines 302-320)

The `calculateBusinessDays()` function counts weekdays (Monday-Friday) between the current date and the deadline, **including the deadline date itself** (fixed in commit 06311a2).

## Development Workflow

### Making Changes

Since this is a single-file application:

1. **Direct Editing**: All changes are made to `index.html`
2. **No Build Process**: The file is ready to deploy as-is
3. **Testing**: Open `index.html` in a browser to test changes
4. **Version Control**: Use git for tracking changes

### Adding New Events

To add a new countdown event:

1. Locate the `this.events` array in `index.html` (around line 197)
2. Add a new object with `name`, `date`, and `description`
3. The date format must be: `new Date('YYYY-MM-DDTHH:MM:SS')`
4. Events will automatically sort by date

Example:
```javascript
{
    name: 'Server Migration Deadline',
    date: new Date('2026-03-15T17:00:00'),
    description: 'Final day for data center migration'
}
```

### Removing Events

To remove an event:

1. Find the event in the `this.events` array
2. Delete the entire object (including commas)
3. Test to ensure no JavaScript errors occur

### Modifying Event Dates

To update an existing deadline:

1. Locate the event in the `this.events` array
2. Update the `date` property with the new date/time
3. Optionally update the `description` to reflect changes

## Git Conventions

### Branch Naming

- Feature branches: `claude/claude-md-*` or `codex/*` (for AI-assisted changes)
- Main branch: `main` (protected, requires PRs)

### Commit Message Patterns

Based on recent history:
- `"Remove [feature] and update [feature]"` - For combined removals and updates
- `"Include [feature] in [context]"` - For bug fixes or enhancements
- `"Add files via upload"` - For initial file additions
- Clear, descriptive messages that explain the "why" not just the "what"

### Recent Changes Reference

- **a078a55**: Removed Symantec support event, updated Cisco countdown date
- **06311a2**: Fixed business day calculation to include the deadline date
- **43b2608**: Initial project upload

## Testing Considerations

### Manual Testing Checklist

When making changes, verify:

1. **Display**: Open `index.html` in multiple browsers (Chrome, Firefox, Safari)
2. **Mobile**: Test on actual mobile devices or browser dev tools (iOS/Android)
3. **Calculations**: Verify countdown accuracy and business days count
4. **Edge Cases**:
   - Events that have already passed (should show "---")
   - Events occurring on weekends
   - Leap years and month boundaries
   - Different timezones (uses client's local time)
5. **Responsiveness**: Test various screen sizes (320px to 1920px+)
6. **Color Transitions**: Verify color coding changes at 7, 30, and 90-day thresholds

### Error Handling

The application includes comprehensive error handling:
- Window-level error handler (line 188)
- Try-catch blocks around initialization and updates
- Graceful degradation for missing DOM elements
- Error messages display in the countdown container

## Code Quality Standards

### JavaScript Patterns

1. **Class-Based Architecture**: Use the `MobileDoomTimer` class pattern
2. **Pure Functions**: Calculation methods should be pure (deterministic)
3. **Error Handling**: Wrap DOM operations in try-catch blocks
4. **Mobile-First**: Always consider mobile performance and UX
5. **No Dependencies**: Keep the application dependency-free

### CSS Best Practices

1. **Mobile-First**: Base styles for mobile, scale up with media queries
2. **Flexbox/Grid**: Use modern layout techniques
3. **Naming**: Use descriptive class names (`.event-card`, `.time-unit`)
4. **Consistency**: Follow existing color scheme and spacing patterns

### HTML Standards

1. **Semantic HTML**: Use appropriate semantic tags
2. **Accessibility**: Include proper meta tags and ARIA labels if needed
3. **Mobile Optimization**: Maintain viewport and iOS-specific meta tags
4. **Minimal Markup**: Keep DOM structure simple for performance

## Common Tasks for AI Assistants

### Task: Add a New Event

```javascript
// Locate this.events array (line ~197) and add:
{
    name: 'Your Event Name',
    date: new Date('2026-01-01T00:00:00'),
    description: 'Brief description of the event'
}
```

### Task: Change Urgency Thresholds

Modify the `applyColors()` method (line 377):
- Change day thresholds: `days <= 7`, `days <= 30`, `days <= 90`
- Adjust colors: Update hex values in style assignments

### Task: Adjust Update Frequency

Modify the `setInterval` call (line 232):
```javascript
setInterval(() => { ... }, 1000);  // 1000ms = 1 second
```

### Task: Modify Visual Theme

Update CSS variables in the `<style>` section:
- Background gradient: `body` selector (line 18)
- Primary color: `#ff4444` (red accent color)
- Card backgrounds: `.event-card` and `.time-unit` selectors

### Task: Fix Business Days Calculation

The business days calculation (lines 302-320) includes the deadline:
```javascript
while (current <= end) {  // Note: <= includes the end date
```

Ensure any modifications maintain this behavior or update intentionally.

## Deployment

### GitHub Pages Setup

1. Go to repository Settings → Pages
2. Set source to "Deploy from a branch"
3. Select `main` branch and `/ (root)` folder
4. The site will be available at: `https://pnelsonftp.github.io/doom-timer/`

### Alternative Hosting

Since this is a static HTML file, it can be deployed to:
- Netlify (drag-and-drop)
- Vercel (import repository)
- AWS S3 + CloudFront
- Any web server (Apache, Nginx, IIS)

Simply upload `index.html` - no build process required.

## Troubleshooting

### Common Issues

1. **Countdown Not Updating**:
   - Check browser console for JavaScript errors
   - Verify date format: `new Date('YYYY-MM-DDTHH:MM:SS')`
   - Ensure no syntax errors in events array

2. **Wrong Business Days Count**:
   - Verify timezone handling (uses client's local timezone)
   - Check that calculation includes the deadline date (line 311)
   - Test with dates spanning weekends

3. **Mobile Display Issues**:
   - Verify viewport meta tag is present (line 5)
   - Test in mobile browser dev tools
   - Check for overflow or fixed widths in CSS

4. **Colors Not Changing**:
   - Verify day calculations are correct
   - Check `applyColors()` method logic (line 377)
   - Inspect element styles in browser dev tools

## Dependencies

**None** - This is a zero-dependency application using only web platform APIs:
- HTML5
- CSS3 (Flexbox, Grid, Gradients, Animations)
- Vanilla JavaScript (ES6+ classes, Date API)

## Browser Compatibility

Tested and working on:
- Chrome/Edge (latest)
- Firefox (latest)
- Safari (iOS and macOS)
- Mobile browsers (iOS Safari, Chrome Mobile, Samsung Internet)

Minimum browser versions:
- Chrome 49+ (2016)
- Firefox 52+ (2017)
- Safari 10+ (2016)
- Edge 79+ (Chromium-based)

## Performance Considerations

- **Update Frequency**: Countdown updates every 1 second (configurable)
- **DOM Operations**: Minimal DOM manipulation (only text content updates)
- **Memory**: Single instance of MobileDoomTimer class
- **Mobile**: Optimized for low-power mobile devices
- **File Size**: ~18KB (entire application in one file)

## Future Enhancement Ideas

If requested by users, consider:
- Local storage for custom events
- Timezone selection
- Dark/light theme toggle
- Sound alerts for critical thresholds
- Export countdown data
- Keyboard shortcuts
- Accessibility improvements (ARIA labels, screen reader support)

## AI Assistant Guidelines

When working on this codebase:

1. **Read First**: Always read `index.html` before making changes
2. **Test Calculations**: Verify date/time logic with multiple test cases
3. **Preserve Structure**: Maintain the single-file architecture
4. **Mobile Focus**: Consider mobile UX in all changes
5. **No Dependencies**: Never introduce external libraries or frameworks
6. **Validate Dates**: Ensure date formats are correct and timezone-aware
7. **Error Handling**: Maintain existing error handling patterns
8. **Comment Sparingly**: Code should be self-documenting; add comments only for complex logic
9. **Commit Carefully**: Use clear commit messages that explain the reasoning
10. **Branch Naming**: Follow the `claude/*` or `codex/*` pattern for AI-assisted changes

## Questions or Issues

For questions about this documentation or the codebase, refer to:
- Recent commit history (`git log`) for context on changes
- Inline code comments in `index.html`
- GitHub Issues for the repository

---

**Last Updated**: 2025-12-03
**Version**: 1.0.0
**Maintained By**: AI assistants working with PNelsonFTP
