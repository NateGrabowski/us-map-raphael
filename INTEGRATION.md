# US Map Component Integration Documentation

## Overview

This document provides comprehensive guidance for integrating the Interactive US Map component into other HTML pages or applications. The component provides regional color-coded states with hover effects and click handlers.

## Component Structure

### Core Files
```
us-map-raphael/
├── labels_us_map.svg     # SVG map data with state boundaries and labels
├── index.html           # Standalone demo (reference implementation)
├── readme.md           # Basic usage documentation
└── us-map-svg.js       # Legacy Raphael.js path data (optional reference)
```

### Component Dependencies
- **No external libraries required** - Pure vanilla JavaScript and SVG
- **Modern browser support** - Requires ES6+ (const/let, arrow functions, fetch API)
- **Responsive design** - Scales with container size

## Integration Methods

### Method 1: Direct HTML Integration (Recommended)

Copy the essential CSS and JavaScript directly into your target HTML file.

#### Required CSS (Minimal)
```css
/* Essential map styling - add to your existing CSS */
.us-map-container {
  max-width: 1000px;
  margin: 0 auto;
}

.boundary {
  stroke: #ffffff;
  stroke-width: 1.25;
  stroke-miterlimit: 4;
  stroke-opacity: 1;
  cursor: pointer;
  transition: fill 0.3s ease;
}

.boundary:hover {
  fill: #999999;
}

text {
  fill: #ffffff;
  font-size: 21px;
  font-family: 'Trebuchet MS', Helvetica, sans-serif;
  pointer-events: none;
  user-select: none;
}

text tspan {
  text-anchor: middle;
  dominant-baseline: central;
}

.label-line {
  fill: none;
  stroke: #b9b9b9;
  stroke-width: 1;
  pointer-events: none;
}

rect {
  fill: rgba(0, 0, 0, 0.8);
  stroke: #b9b9b9;
  stroke-width: 1;
  pointer-events: none;
  rx: 5;
  ry: 5;
}

svg {
  width: 100%;
  height: auto;
  background: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.state-group {
  transition: all 0.3s ease;
}

.state-group:hover text {
  font-weight: bold;
  filter: drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.8));
}
```

#### Required HTML
```html
<!-- Add this container where you want the map to appear -->
<div id="us-map-container" class="us-map-container">
  <!-- Map will be loaded here dynamically -->
</div>
```

#### Required JavaScript
```javascript
// ==============================================
// US MAP COMPONENT
// ==============================================

class USMapComponent {
  constructor(containerId, svgPath = './labels_us_map.svg') {
    this.containerId = containerId;
    this.svgPath = svgPath;
    this.init();
  }

  // Regional mapping - single source of truth
  static REGIONAL_MAPPING = {
    northeast: ['ME', 'NH', 'VT', 'MA', 'RI', 'CT', 'NY', 'NJ', 'PA', 'DE', 'MD'],
    southeast: ['VA', 'WV', 'KY', 'TN', 'NC', 'SC', 'OH', 'IN', 'IL', 'MI', 'WI'],
    southcentral: ['TX', 'AR', 'LA', 'OK', 'MO', 'AL', 'MS', 'GA', 'FL'],
    western: ['WA', 'OR', 'CA', 'NV', 'ID', 'MT', 'WY', 'CO', 'AZ', 'NM', 'UT', 'AK', 'HI'],
    plains: ['ND', 'SD', 'NE', 'KS', 'IA', 'MN']
  };

  static REGIONAL_COLORS = {
    northeast: '#2c3e50',     // Dark blue/navy
    southeast: '#c0392b',     // Red
    southcentral: '#16a085',  // Teal/dark green
    western: '#27ae60',       // Light green
    plains: '#95a5a6'         // Gray
  };

  static REGIONAL_HOVER_COLORS = {
    northeast: '#1a252f',     // Darker navy
    southeast: '#a93226',     // Darker red
    southcentral: '#138d75',  // Darker teal
    western: '#229954',       // Darker green
    plains: '#7f8c8d'         // Darker gray
  };

  async init() {
    try {
      const response = await fetch(this.svgPath);
      const svgContent = await response.text();
      document.getElementById(this.containerId).innerHTML = svgContent;
      this.addMapInteractivity();
    } catch (error) {
      console.error('Error loading US Map SVG:', error);
      this.loadFallback();
    }
  }

  getRegionForState(stateId) {
    for (const [region, states] of Object.entries(USMapComponent.REGIONAL_MAPPING)) {
      if (states.includes(stateId)) {
        return region;
      }
    }
    return null;
  }

  getRegionalColor(stateId) {
    const region = this.getRegionForState(stateId);
    return region ? USMapComponent.REGIONAL_COLORS[region] : '#d3d3d3';
  }
  
  getHoverColor(stateId) {
    const region = this.getRegionForState(stateId);
    return region ? USMapComponent.REGIONAL_HOVER_COLORS[region] : '#999999';
  }

  addMapInteractivity() {
    const states = document.querySelectorAll('.boundary');
    
    this.enhanceTextCentering();
    
    const stateGroups = document.querySelectorAll('g[id]');
    stateGroups.forEach(group => {
      if (group.id && group.id.length <= 2) {
        group.classList.add('state-group');
      }
    });
    
    states.forEach(state => {
      const stateId = state.closest('g').id;
      const regionalColor = this.getRegionalColor(stateId);
      const hoverColor = this.getHoverColor(stateId);
      
      // Apply regional color immediately
      state.style.fill = regionalColor;
      
      // Add hover effects
      state.addEventListener('mouseenter', () => {
        state.style.fill = hoverColor;
        state.style.transform = 'scale(1.02)';
        state.style.transformOrigin = 'center';
        state.style.filter = 'drop-shadow(2px 2px 4px rgba(0,0,0,0.3))';
      });
      
      state.addEventListener('mouseleave', () => {
        state.style.fill = regionalColor;
        state.style.transform = 'scale(1)';
        state.style.filter = 'none';
      });
      
      // Add click handler - customize this for your needs
      state.addEventListener('click', () => {
        this.onStateClick(stateId);
      });
    });
  }

  // Override this method to customize click behavior
  onStateClick(stateId) {
    alert(`You clicked on ${stateId}`);
  }

  enhanceTextCentering() {
    const textElements = document.querySelectorAll('text');
    
    textElements.forEach(textEl => {
      const parent = textEl.closest('g');
      const rect = parent ? parent.querySelector('rect') : null;
      
      if (rect) {
        const rectX = parseFloat(rect.getAttribute('x'));
        const rectWidth = parseFloat(rect.getAttribute('width'));
        const rectY = parseFloat(rect.getAttribute('y'));
        const rectHeight = parseFloat(rect.getAttribute('height'));
        
        const centerX = rectX + rectWidth / 2;
        const centerY = rectY + rectHeight / 2;
        
        textEl.setAttribute('x', centerX);
        textEl.setAttribute('y', centerY);
        textEl.setAttribute('text-anchor', 'middle');
        textEl.setAttribute('dominant-baseline', 'central');
      } else {
        textEl.setAttribute('text-anchor', 'middle');
        textEl.setAttribute('dominant-baseline', 'central');
      }
      
      const tspans = textEl.querySelectorAll('tspan');
      tspans.forEach(tspan => {
        tspan.setAttribute('text-anchor', 'middle');
        tspan.setAttribute('dominant-baseline', 'central');
      });
    });
  }

  loadFallback() {
    document.getElementById(this.containerId).innerHTML = `
      <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 8px;">
        <p>US Map could not be loaded. Please ensure the SVG file is accessible.</p>
      </div>
    `;
  }
}

// Initialize the map - customize as needed
document.addEventListener('DOMContentLoaded', () => {
  new USMapComponent('us-map-container');
});
```

### Method 2: External Script Integration

Create a separate JavaScript file for reuse across multiple pages.

#### 1. Create `us-map-component.js`
```javascript
// Copy the USMapComponent class from Method 1 into this file
```

#### 2. Include in your HTML
```html
<link rel="stylesheet" href="path/to/us-map-styles.css">
<script src="path/to/us-map-component.js"></script>
<script>
  document.addEventListener('DOMContentLoaded', () => {
    new USMapComponent('us-map-container', 'path/to/labels_us_map.svg');
  });
</script>
```

## Customization Options

### Colors
Modify the regional colors by updating the static properties:
```javascript
USMapComponent.REGIONAL_COLORS.northeast = '#your-color';
USMapComponent.REGIONAL_HOVER_COLORS.northeast = '#your-hover-color';
```

### Regional Groupings
Add or modify state groupings:
```javascript
USMapComponent.REGIONAL_MAPPING.custom = ['CA', 'TX', 'FL'];
USMapComponent.REGIONAL_COLORS.custom = '#custom-color';
USMapComponent.REGIONAL_HOVER_COLORS.custom = '#custom-hover-color';
```

### Click Behavior
Override the `onStateClick` method:
```javascript
class CustomUSMap extends USMapComponent {
  onStateClick(stateId) {
    // Your custom logic here
    console.log(`State clicked: ${stateId}`);
    // Could trigger navigation, show popup, update other components, etc.
  }
}
```

### Styling
Modify CSS variables or override classes:
```css
.us-map-container svg {
  background: transparent; /* Remove background */
  box-shadow: none;       /* Remove shadow */
}

.boundary {
  stroke-width: 2px;      /* Thicker borders */
}
```

## File Path Configuration

### Relative Paths
- From same directory: `'./labels_us_map.svg'`
- From parent directory: `'../us-map/labels_us_map.svg'`
- From root: `'/assets/maps/labels_us_map.svg'`

### Absolute URLs
```javascript
new USMapComponent('container', 'https://yourdomain.com/assets/labels_us_map.svg');
```

## Browser Compatibility

### Minimum Requirements
- **ES6 Support**: const/let, arrow functions, classes
- **Fetch API**: For loading SVG content
- **SVG Support**: Modern SVG rendering

### Supported Browsers
- Chrome 49+
- Firefox 45+
- Safari 10+
- Edge 12+
- IE 11 (with polyfills)

### Polyfills (if supporting older browsers)
```html
<!-- For fetch API support in older browsers -->
<script src="https://polyfill.io/v3/polyfill.min.js?features=fetch"></script>
```

## Performance Considerations

### File Sizes
- `labels_us_map.svg`: ~25KB (gzipped: ~8KB)
- JavaScript component: ~3KB (gzipped: ~1KB)
- CSS styles: ~1KB (gzipped: ~0.5KB)

### Optimization Tips
1. **Serve SVG with gzip compression**
2. **Cache SVG file with appropriate headers**
3. **Consider lazy loading** for below-the-fold maps
4. **Preload SVG** if map is above-the-fold

### Lazy Loading Example
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      new USMapComponent('us-map-container');
      observer.unobserve(entry.target);
    }
  });
});

observer.observe(document.getElementById('us-map-container'));
```

## Error Handling

### Common Issues
1. **SVG not found**: Check file path and server configuration
2. **CORS errors**: Ensure SVG is served from same origin or with proper headers
3. **Container not found**: Ensure container element exists before initialization

### Debug Mode
```javascript
class DebugUSMap extends USMapComponent {
  async init() {
    console.log('Initializing US Map...', this.containerId, this.svgPath);
    await super.init();
    console.log('US Map initialized successfully');
  }
  
  onStateClick(stateId) {
    console.log('State clicked:', stateId, 'Region:', this.getRegionForState(stateId));
  }
}
```

## Examples

### Basic Integration
```html
<!DOCTYPE html>
<html>
<head>
  <title>My App with US Map</title>
  <!-- Include the CSS here -->
</head>
<body>
  <h1>Welcome to My App</h1>
  <div id="us-map-container"></div>
  <!-- Include the JavaScript here -->
</body>
</html>
```

### Integration with React/Vue/Angular
For framework integration, wrap the component initialization in the appropriate lifecycle hooks and ensure proper cleanup of event listeners.

This documentation provides everything needed to successfully integrate the US Map component into any HTML-based application.
