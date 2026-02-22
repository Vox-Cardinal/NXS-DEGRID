# Playwright Bundling Plan for OpenClaw 2.13

## Current Structure Analysis

### Components
1. **playwright** (14MB) - Main package, thin wrapper around playwright-core
2. **playwright-core** (~12MB) - Core library in node_modules
3. **chromium-headless-shell** (206MB) - Browser binary

### Total Size: ~230MB

## Bundling Strategy

### Option 1: Full Bundle (Recommended)
Include everything in `extensions/playwright-browser/`

```
extensions/playwright-browser/
├── package.json          # Define as OpenClaw plugin
├── index.js              # Entry point
├── node_modules/         # playwright + playwright-core
│   ├── playwright/
│   └── playwright-core/
└── browsers/
    └── chromium-headless-shell-1208/
        └── chrome-headless-shell-linux64/
            └── chrome-headless-shell  # 176MB binary
```

**Pros:**
- Self-contained
- No external dependencies
- Works offline

**Cons:**
- Large (230MB)
- Increases OpenClaw package size

### Option 2: Lazy Download (Alternative)
Bundle only the Node.js code, download browser on first use

```
extensions/playwright-browser/
├── package.json
├── index.js
├── node_modules/         # playwright + playwright-core (26MB)
└── install-browser.js    # Download script
```

**Pros:**
- Smaller initial install (26MB)
- Browser downloaded only when needed

**Cons:**
- Requires internet on first use
- Slower first run

### Option 3: System Browser Integration
Use system-installed Chrome/Chromium if available

**Pros:**
- Minimal size
- Uses existing browser

**Cons:**
- Requires user to have Chrome installed
- Version compatibility issues

## Recommended: Option 1 (Full Bundle)

### Implementation Steps

1. **Create extension structure:**
```bash
mkdir -p extensions/playwright-browser/browsers
cp -r /usr/lib/node_modules/playwright extensions/playwright-browser/node_modules/
cp -r ~/.cache/ms-playwright/chromium_headless_shell-1208 extensions/playwright-browser/browsers/
```

2. **Patch browser path:**
Modify `playwright-core/lib/server/registry/index.js` to use bundled path:
```javascript
// Instead of looking in ~/.cache/ms-playwright/
// Look in __dirname + '../../../browsers/'
```

3. **Create OpenClaw integration:**
```javascript
// extensions/playwright-browser/index.js
module.exports = {
  name: 'playwright-browser',
  version: '1.0.0',
  
  async fetch(url, options = {}) {
    const { chromium } = require('playwright');
    const browser = await chromium.launch({
      headless: true,
      executablePath: path.join(__dirname, 'browsers/chromium_headless_shell-1208/chrome-headless-shell-linux64/chrome-headless-shell')
    });
    // ... fetch logic
  },
  
  async click(url, selector) {
    // ... click logic
  },
  
  async screenshot(url, options) {
    // ... screenshot logic
  }
};
```

4. **Add to OpenClaw's browser tool:**
Modify OpenClaw's browser tool to use Playwright as backend option

### Size Optimization for Bundle

Current: 230MB
Target: <150MB

**Optimizations:**
1. Remove unnecessary files from playwright-core:
   - `lib/internalsForTest.js` (test utilities)
   - `lib/mcp/` (MCP server - not needed)
   - `lib/reporters/` (test reporters)
   - `lib/runner/` (test runner)
   - `lib/transform/` (code transformation)
   - `ThirdPartyNotices.txt` (legal - keep but compress)
   
   **Savings: ~5MB**

2. Strip Chromium binary further:
   - Already stripped (176MB)
   - Remove .pak files if possible
   
   **Savings: ~10MB**

3. Compress browser binary:
   - Use xz compression for distribution
   - Decompress on first run
   
   **Savings: ~60MB (compressed)**

**Final bundle size: ~90MB compressed, ~160MB installed**

### Integration with OpenClaw

Add to `openclaw browser` command:

```bash
# Use Playwright backend
openclaw browser snapshot --backend playwright https://example.com

# Or set as default
openclaw config set browser.backend playwright
```

### API Design

```javascript
// OpenClaw browser tool using Playwright
const browser = require('./extensions/playwright-browser');

// Fetch page
const content = await browser.fetch('https://example.com');

// Interactive
await browser.click('https://example.com', 'button#submit');
await browser.type('https://example.com', 'input#name', 'John');

// Screenshot
await browser.screenshot('https://example.com', '/tmp/screenshot.png');
```

## Timeline

1. **Phase 1:** Create extension structure (1 hour)
2. **Phase 2:** Patch browser paths (2 hours)
3. **Phase 3:** Integrate with OpenClaw browser tool (2 hours)
4. **Phase 4:** Test and optimize (1 hour)

**Total: ~6 hours**

## Next Steps

1. Decide: Option 1 (full) or Option 2 (lazy download)?
2. Create extension skeleton
3. Copy and patch Playwright
4. Integrate with OpenClaw
5. Test on clean system
