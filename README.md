# Pear Intelligence Plugin Registry

The official marketplace registry for [Pear Intelligence](https://github.com/pear-intelligence/pear-intelligence) plugins. Plugins listed here appear in the in-app marketplace and can be installed with one tap.

## Available Plugins

| Plugin | Description | Author |
|--------|-------------|--------|
| [weather-briefing](https://github.com/pear-intelligence/plugin-weather-briefing) | Morning weather briefings via OpenWeatherMap | pearintelligence |
| [fitness-tracker](https://github.com/pear-intelligence/plugin-fitness-tracker) | Sync and track fitness data from Garmin Connect and Strava | pearintelligence |
| [stock-data](https://github.com/pear-intelligence/plugin-stock-data) | Live stock quotes, ETF data, company profiles, candles, and market news via Finnhub | pearintelligence |

## Installing Plugins

From the iOS app:
1. Open **Settings** → **Plugins** → tap the **storefront** icon
2. Browse available plugins and tap **Install**

From the API:
```bash
curl -X POST http://localhost:3000/plugins/marketplace/install \
  -H "Content-Type: application/json" \
  -d '{"name": "weather-briefing", "repoUrl": "https://github.com/pear-intelligence/plugin-weather-briefing"}'
```

## Adding Your Plugin

### 1. Create your plugin repo

Your repo must contain a `plugin.json` manifest and an entry file. See the [Plugin Development Guide](https://github.com/pear-intelligence/pear-intelligence/blob/master/plugins/EXTENSION.md) for the full spec.

Minimal structure:
```
your-plugin/
├── plugin.json
├── index.ts
└── package.json    # optional, for dependencies
```

### 2. Add your plugin to the registry

Fork this repo and add an entry to `index.json`:

```json
{
  "name": "your-plugin-name",
  "displayName": "Your Plugin",
  "description": "What your plugin does",
  "author": "your-github-username",
  "icon": "star.fill",
  "version": "1.0.0",
  "repoUrl": "https://github.com/you/your-plugin",
  "tags": ["category1", "category2"]
}
```

### 3. Submit a pull request

Open a PR against `main`. We'll review that your plugin:
- Has a valid `plugin.json` manifest
- Entry point exists and exports `activate(ctx)`
- Doesn't contain malicious code
- Has a clear description and appropriate tags

### Plugin Manifest Reference

```json
{
  "name": "my-plugin",
  "displayName": "My Plugin",
  "version": "1.0.0",
  "description": "What it does",
  "author": "username",
  "icon": "star.fill",
  "main": "index.ts",
  "capabilities": {
    "routes": false,
    "tools": true,
    "webhooks": false,
    "scheduled": false
  },
  "hotReloadable": true,
  "settings": [
    { "key": "apiKey", "label": "API Key", "type": "secret", "default": "" },
    { 
      "key": "oauth_setup", 
      "label": "Connect Account", 
      "type": "link", 
      "default": "",
      "url": "/px/my-plugin/oauth/setup",
      "buttonText": "Open Setup Page"
    }
  ],
  "dependencies": []
}
```

**Setting types:** `string`, `secret`, `number`, `boolean`, `select`, `link`

**New in v1.1:**
- `link` - Clickable button that opens a URL (requires `url` and optional `buttonText` fields)

**Icon:** Any valid [SF Symbol](https://developer.apple.com/sf-symbols/) name

## License

MIT
