# Interactive United States Map

An interactive US map with regional color coding and hover effects. Built with modern vanilla JavaScript and SVG.

## Features

- **Regional Color Coding**: States grouped by geographic regions with distinct colors
- **Interactive Hover Effects**: States scale and change color on hover
- **Click Handlers**: Click states to get their information
- **Responsive Design**: Scales appropriately across devices
- **Modern SVG**: No dependencies - uses native SVG and JavaScript

## Regional Groups

- **Northeast** (Dark Blue): ME, NH, VT, MA, RI, CT, NY, NJ, PA, DE, MD
- **Southeast/Great Lakes** (Red): VA, WV, KY, TN, NC, SC, OH, IN, IL, MI, WI
- **South Central** (Teal): TX, AR, LA, OK, MO, AL, MS, GA, FL
- **Western** (Green): WA, OR, CA, NV, ID, MT, WY, CO, AZ, NM, UT, AK, HI
- **Plains/Central** (Gray): ND, SD, NE, KS, IA, MN

## Usage

Simply open `index.html` in a web browser. The map will load with regional colors applied and interactive features enabled.

## Integration

To add this map to another HTML file or application:

- **📖 [INTEGRATION.md](INTEGRATION.md)** - Complete integration guide with code examples
- **⚡ [QUICK-REFERENCE.md](QUICK-REFERENCE.md)** - Minimal setup and common patterns
- **🎯 [index.html](index.html)** - Reference implementation

### Quick Start
```html
<div id="map-container"></div>
<script src="us-map-component.js"></script>
<script>
  new USMapComponent('map-container', 'path/to/labels_us_map.svg');
</script>
```

See the integration documentation for complete setup instructions.

## Files

- `index.html` - Main interactive map application
- `labels_us_map.svg` - SVG map data with state boundaries and labels
- `us-map-svg.js` - Legacy Raphael.js path data (for reference)

## Credits

- [SVG Map of the United States](http://commons.wikimedia.org/wiki/File:Blank_US_Map.svg)
- Original Raphael.js implementation concept

## License

The map coordinates and code are both public domain.












