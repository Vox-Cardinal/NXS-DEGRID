# Enhanced Playwright Integration Plan for OpenClaw 2.13

## Philosophy: Native Integration, Not Just Bundling

Instead of bundling Playwright as-is, we'll create a **native OpenClaw browser extension** that leverages Playwright's power while feeling like a native part of OpenClaw.

## Architecture

```
extensions/browser-automation/
├── package.json                    # OpenClaw extension manifest
├── SKILL.md                        # Documentation
├── src/
│   ├── index.ts                    # Main entry
│   ├── browser.ts                  # Browser controller
│   ├── actions.ts                  # High-level actions
│   ├── proxy.ts                    # Proxy configuration
│   └── utils.ts                    # Helpers
├── bin/
│   └── chromium-headless-shell     # Bundled browser (176MB)
├── scripts/
│   ├── install-browser.sh          # Browser installer
│   ├── optimize-chromium.sh        # Strip unnecessary components
│   └── health-check.sh             # Verify browser health
└── config/
    └── default.json                # Default settings
```

## Enhanced Features

### 1. OpenClaw-Native API

Instead of raw Playwright API, provide OpenClaw-style commands:

```typescript
// OpenClaw-style usage
const browser = require('@openclaw/browser-automation');

// Simple fetch (like Lightpanda)
const html = await browser.fetch('https://example.com', {
  strip: 'full',           // Remove JS/CSS/images
  proxy: true,             // Use system proxy
  timeout: 10000
});

// Interactive automation
const session = await browser.createSession('https://example.com');
await session.click('button#submit');
await session.type('input#username', 'user');
await session.type('input#password', 'pass');
await session.submit();
const result = await session.content();
await session.close();

// Screenshot
await browser.screenshot('https://example.com', {
  path: '/tmp/screenshot.png',
  fullPage: true
});

// PDF generation
await browser.pdf('https://example.com', {
  path: '/tmp/page.pdf'
});
```

### 2. Automatic Proxy Integration

```typescript
// Automatically use OpenClaw's proxy settings
const proxy = require('@openclaw/proxy'); // Use existing proxy

browser.configure({
  proxy: {
    server: proxy.getHttpProxy(),  // http://127.0.0.1:7890
    bypass: proxy.getNoProxy()     // localhost,127.0.0.1
  }
});
```

### 3. Session Persistence

```typescript
// Save and restore browser sessions
const session = await browser.createSession('https://site.com', {
  persist: true,
  name: 'my-session'
});

// Later...
const restored = await browser.restoreSession('my-session');
```

### 4. Smart Element Detection

```typescript
// Multiple selector strategies (like Taiko)
await session.click('Submit button');           // Text match
await session.click('#submit');                 // CSS selector
await session.click('button[type="submit"]');   // Attribute
await session.click({ near: 'Password field' }); // Proximity
```

### 5. Batch Operations

```typescript
// Process multiple URLs efficiently
const results = await browser.batch([
  { url: 'https://site1.com', action: 'fetch' },
  { url: 'https://site2.com', action: 'screenshot' },
  { url: 'https://site3.com', action: 'pdf' }
], {
  concurrency: 3,      // Run 3 in parallel
  reuseBrowser: true   // Don't restart browser between tasks
});
```

### 6. Health Monitoring

```typescript
// Built-in health checks
browser.on('health', (status) => {
  if (status.memory > 500) {
    console.warn('Browser using too much memory, restarting...');
    browser.restart();
  }
});
```

## Custom Scripts

### Script 1: Browser Optimizer

```bash
#!/bin/bash
# scripts/optimize-chromium.sh

CHROMIUM_DIR="$1"

echo "Optimizing Chromium for OpenClaw..."

# Remove unnecessary components
rm -rf "$CHROMIUM_DIR/locales/"*  # Remove all locales (we'll add back en only)
mkdir -p "$CHROMIUM_DIR/locales/"
touch "$CHROMIUM_DIR/locales/en-US.pak"  # Minimal locale

# Remove SwiftShader (software rendering) if GPU available
# Keep for headless compatibility

# Strip debug symbols
strip --strip-all "$CHROMIUM_DIR/chrome-headless-shell"

# Compress resources
for pak in "$CHROMIUM_DIR"/*.pak; do
    if [ -f "$pak" ]; then
        xz -9 "$pak"  # Max compression
    fi
done

echo "Optimization complete"
```

### Script 2: Smart Installer

```bash
#!/bin/bash
# scripts/install-browser.sh

INSTALL_DIR="${HOME}/.openclaw/browsers"
CACHE_DIR="${HOME}/.cache/ms-playwright"

# Check if already installed
if [ -f "$INSTALL_DIR/chromium-headless-shell" ]; then
    echo "Browser already installed"
    exit 0
fi

# Option 1: Use bundled browser (if available)
if [ -f "./bin/chromium-headless-shell" ]; then
    echo "Using bundled browser..."
    mkdir -p "$INSTALL_DIR"
    cp -r ./bin/* "$INSTALL_DIR/"
    exit 0
fi

# Option 2: Download from cache
if [ -f "$CACHE_DIR/chromium_headless_shell-1208/chrome-headless-shell-linux64/chrome-headless-shell" ]; then
    echo "Using cached browser..."
    mkdir -p "$INSTALL_DIR"
    cp -r "$CACHE_DIR/chromium_headless_shell-1208"/* "$INSTALL_DIR/"
    exit 0
fi

# Option 3: Download fresh
echo "Downloading browser..."
npx playwright install chromium

exit 0
```

### Script 3: Health Checker

```bash
#!/bin/bash
# scripts/health-check.sh

BROWSER_PATH="${HOME}/.openclaw/browsers/chrome-headless-shell-linux64/chrome-headless-shell"

# Check binary exists
if [ ! -f "$BROWSER_PATH" ]; then
    echo "ERROR: Browser not found at $BROWSER_PATH"
    exit 1
fi

# Check binary is executable
if [ ! -x "$BROWSER_PATH" ]; then
    echo "ERROR: Browser not executable"
    exit 1
fi

# Test launch
timeout 5 "$BROWSER_PATH" --version > /dev/null 2>&1
if [ $? -ne 0 ]; then
    echo "ERROR: Browser failed to launch"
    exit 1
fi

echo "OK: Browser is healthy"
exit 0
```

## OpenClaw Integration Points

### 1. Extend `openclaw browser` Command

```bash
# Current OpenClaw browser commands:
openclaw browser snapshot https://example.com
openclaw browser click <ref>

# Enhanced with Playwright backend:
openclaw browser fetch https://example.com --strip-mode full
openclaw browser interact https://example.com --script "click('button'); type('input', 'text')"
openclaw browser screenshot https://example.com --full-page
openclaw browser pdf https://example.com --output page.pdf
```

### 2. Add to OpenClaw's Tool System

```typescript
// Register as OpenClaw tool
export default {
  name: 'browser-automation',
  version: '1.0.0',
  
  tools: [
    {
      name: 'browser_fetch',
      description: 'Fetch web page content',
      parameters: {
        url: { type: 'string', required: true },
        strip_mode: { type: 'string', enum: ['none', 'js', 'css', 'full'] },
        timeout: { type: 'number', default: 10000 }
      },
      handler: async ({ url, strip_mode, timeout }) => {
        return await browser.fetch(url, { strip_mode, timeout });
      }
    },
    {
      name: 'browser_click',
      description: 'Click element on page',
      parameters: {
        url: { type: 'string', required: true },
        selector: { type: 'string', required: true }
      },
      handler: async ({ url, selector }) => {
        const session = await browser.createSession(url);
        await session.click(selector);
        const result = await session.content();
        await session.close();
        return result;
      }
    },
    {
      name: 'browser_screenshot',
      description: 'Take screenshot of page',
      parameters: {
        url: { type: 'string', required: true },
        output: { type: 'string', required: true },
        full_page: { type: 'boolean', default: false }
      },
      handler: async ({ url, output, full_page }) => {
        await browser.screenshot(url, { path: output, full_page });
        return { screenshot: output };
      }
    }
  ]
};
```

### 3. Agent SDK Integration

```typescript
// AI Agent can use browser automation
const agent = {
  async research(topic) {
    // Search
    const searchResults = await browser.fetch(
      `https://duckduckgo.com/html/?q=${encodeURIComponent(topic)}`,
      { strip_mode: 'full' }
    );
    
    // Extract links
    const links = extractLinks(searchResults);
    
    // Visit top result
    const session = await browser.createSession(links[0]);
    const content = await session.content();
    await session.close();
    
    return summarize(content);
  }
};
```

## Size Optimization Strategy

### Target: <100MB Total

| Component | Original | Optimized | Method |
|-----------|----------|-----------|--------|
| Playwright core | 26MB | 20MB | Remove test/reporter code |
| Chromium binary | 176MB | 150MB | Strip further, remove locales |
| Resources | 30MB | 15MB | Compress .pak files |
| **Total** | **232MB** | **85MB** | **xz compression** |

### Distribution Strategy

1. **Development:** Full Playwright (npm install)
2. **Production:** Optimized bundle (85MB)
3. **Minimal:** Download on demand (26MB + download)

## Implementation Roadmap

### Phase 1: Core Extension (2 hours)
- Create extension structure
- Implement basic fetch/click/type
- Integrate with OpenClaw proxy

### Phase 2: Advanced Features (3 hours)
- Session persistence
- Batch operations
- Screenshot/PDF
- Smart selectors

### Phase 3: Optimization (2 hours)
- Create optimization scripts
- Compress browser binary
- Remove unnecessary code

### Phase 4: Integration (2 hours)
- Extend `openclaw browser` command
- Register as OpenClaw tool
- Add health monitoring

### Phase 5: Testing (1 hour)
- Test on clean system
- Verify proxy integration
- Performance benchmarks

**Total: 10 hours**

## Key Differentiators

1. **Native OpenClaw Feel:** Uses OpenClaw patterns, not raw Playwright
2. **Automatic Proxy:** Uses OpenClaw's proxy settings automatically
3. **Session Persistence:** Save/restore browser state
4. **Smart Selectors:** Text, CSS, proximity matching
5. **Health Monitoring:** Built-in resource monitoring
6. **Batch Operations:** Efficient parallel processing
7. **Agent SDK:** AI-friendly high-level API

## Decision Points

1. **Bundle size vs Features:**
   - A) Full bundle (85MB) - Everything included
   - B) Minimal bundle (30MB) + download - Smaller, needs network
   - C) System browser (0MB) - Requires Chrome installed

2. **API Style:**
   - A) Playwright-native (power users)
   - B) OpenClaw-native (consistent with other tools)
   - C) Both (configurable)

3. **Browser Backend:**
   - A) Chromium only (recommended)
   - B) Chromium + Firefox + WebKit
   - C) Pluggable (user chooses)

## Recommendation

**Option A for all:**
- Full bundle (85MB compressed)
- OpenClaw-native API
- Chromium only

This provides the best user experience: install and go, consistent API, optimized size.
