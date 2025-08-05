# US Map Component - Quick Reference

## Minimal Integration

### 1. Required Files
```
your-project/
├── your-page.html
├── assets/
│   ├── labels_us_map.svg    # Copy from us-map-raphael/
│   ├── us-map.css          # Extract CSS from integration docs
│   └── us-map.js           # Extract JS class from integration docs
```

### 2. HTML Setup
```html
<div id="map-container"></div>
<link rel="stylesheet" href="assets/us-map.css">
<script src="assets/us-map.js"></script>
<script>
  new USMapComponent('map-container', 'assets/labels_us_map.svg');
</script>
```

## Common Customizations

### Custom Click Handler
```javascript
class MyMap extends USMapComponent {
  onStateClick(stateId) {
    window.location.href = `/state/${stateId}`;
  }
}
new MyMap('map-container');
```

### Custom Colors
```javascript
USMapComponent.REGIONAL_COLORS.western = '#ff6b6b';
USMapComponent.REGIONAL_HOVER_COLORS.western = '#ff5252';
```

### Remove Hover Effects
```css
.boundary {
  transition: none;
}
.boundary:hover {
  transform: none !important;
  filter: none !important;
}
```

## Integration Checklist

- [ ] Copy `labels_us_map.svg` to accessible location
- [ ] Include required CSS styles
- [ ] Include JavaScript component
- [ ] Create container element with unique ID
- [ ] Initialize component with correct paths
- [ ] Test on target browsers
- [ ] Verify click handlers work as expected
- [ ] Check responsive behavior

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Map not loading | Check SVG file path and CORS headers |
| States not colored | Verify JavaScript runs after DOM ready |
| Click not working | Check for CSS `pointer-events: none` conflicts |
| Poor performance | Enable gzip compression for SVG file |
| Layout issues | Ensure container has defined width/height |

## File Size Impact

- SVG: ~25KB (8KB gzipped)
- CSS: ~1KB (0.5KB gzipped)  
- JS: ~3KB (1KB gzipped)
- **Total: ~29KB (~9.5KB gzipped)**
