
## Architecture Overview

### US Map Component Structure
```
us-map-raphael/
├── index.html           # Standalone demo implementation
├── labels_us_map.svg    # SVG map data with state boundaries and labels  
├── INTEGRATION.md       # Comprehensive integration documentation
├── readme.md           # Basic usage and feature overview
└── us-map-svg.js       # Legacy Raphael.js path data (reference only)
```

### Component Architecture
- **Pure vanilla JavaScript** - No external dependencies
- **SVG-based rendering** - Scalable and performant
- **Regional color mapping** - 5 distinct geographic regions
- **Event-driven interactions** - Hover effects and click handlers
- **Responsive design** - Adapts to container constraints

## Critical Data Flow Patterns

### Component Lifecycle
1. **Initialization** - Fetch SVG content from file system
2. **DOM Injection** - Insert SVG into designated container
3. **Event Registration** - Attach hover/click handlers to state boundaries
4. **Color Application** - Apply regional colors based on state mapping
5. **Interactive State** - Respond to user interactions with visual feedback

### State Management Flow
```
State ID → Regional Mapping Lookup → Color Assignment → Event Binding → User Interaction → Callback Execution
```

### Data Dependencies
- **REGIONAL_MAPPING**: Object mapping regions to state arrays
- **REGIONAL_COLORS**: Base colors for each geographic region  
- **REGIONAL_HOVER_COLORS**: Darker variants for hover states
- **SVG Content**: External file loaded via fetch API

## Project-Specific Patterns

### Regional Grouping System
- **Northeast** (Dark Blue): New England + Mid-Atlantic states
- **Southeast** (Red): Southern states + Great Lakes region  
- **South Central** (Teal): Texas, Louisiana, Arkansas, Oklahoma + surrounding
- **Western** (Green): Pacific, Mountain, Southwest states + Alaska/Hawaii
- **Plains** (Gray): Upper Midwest agricultural/plains states

### Color Consistency Rules
- Each region has primary color + darker hover variant
- Default fallback: `#d3d3d3` (light gray) for unmapped states
- Hover fallback: `#999999` (medium gray)
- All colors follow accessibility contrast guidelines

### File Organization Principles
- **Single source of truth** for regional mappings
- **Separation of concerns** - CSS, JavaScript, and SVG content isolated
- **Integration flexibility** - Component can be embedded in any HTML page
- **No build process required** - Runs directly in browser


# Development Partnership

We build production code together. I handle implementation details while you guide architecture and catch complexity early.

Always operate under the assumption that I, the user, might be incorrect, misunderstanding concepts, or providing incomplete/flawed information. Even if I state something with confidence, critically evaluate it. If you suspect a misunderstanding on my part, or if my request is ambiguous, unclear, or potentially flawed, you must ask clarifying questions or politely point out the potential error and explain why. Don't just accept my statements at face value. Your goal is to ensure the underlying logic and approach are sound.

## Core Workflow: Research → Plan → Implement → Validate

**Start every feature with:** "Let me research the codebase and create a plan before implementing."

1. **Research** - Understand existing patterns and architecture
2. **Plan** - Propose approach and verify with you
3. **Implement** - Build with tests and error handling
4. **Validate** - ALWAYS run formatters, linters, and tests after implementation

## Code Organization

**Keep functions small and focused:**
- If you need comments to explain sections, split into functions
- Group related functionality into clear packages
- Prefer many small files over few large ones

## Architecture Principles

**This is always a feature branch:**
- Delete old code completely - no deprecation needed
- No versioned names (processV2, handleNew, ClientOld)
- No migration code unless explicitly requested
- No "removed code" comments - just delete it

**Prefer explicit over implicit:**
- Clear function names over clever abstractions
- Obvious data flow over hidden magic
- Direct dependencies over service locators

## Maximize Efficiency

**Parallel operations:** Run multiple searches, reads, and greps in single messages
**Batch similar work:** Group related file edits together

## Go Development Standards

### Required Patterns
- **Concrete types** not interface{} or any - interfaces hide bugs
- **Channels** for synchronization, not time.Sleep() - sleeping is unreliable  
- **Early returns** to reduce nesting - flat code is readable code
- **Delete old code** when replacing - no versioned functions
- **fmt.Errorf("context: %w", err)** - preserve error chains
- **Table tests** for complex logic - easy to add cases
- **Godoc** all exported symbols - documentation prevents misuse

## Problem Solving

**When stuck:** Stop. The simple solution is usually correct.

**When uncertain:** "Let me ultrathink about this architecture."

**When choosing:** "I see approach A (simple) vs B (flexible). Which do you prefer?"

Your redirects prevent over-engineering. When uncertain about implementation, stop and ask for guidance.

## Testing Strategy

**Match testing approach to code complexity:**
- Complex business logic: Write tests first (TDD)
- Simple CRUD operations: Write code first, then tests
- Hot paths: Add benchmarks after implementation

**Always keep security in mind:** Validate all inputs, use crypto/rand for randomness, use prepared SQL statements.

**Performance rule:** Measure before optimizing. No guessing.

## Progress Tracking

- **TodoWrite** for task management
- **Clear naming** in all code

Focus on maintainable solutions over clever abstractions.
