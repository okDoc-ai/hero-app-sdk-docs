# @okdoc-ai/plugin-sdk

[![License](https://img.shields.io/badge/license-Proprietary-red.svg)](LICENSE)

SDK for building **okDoc AI-powered plugins**. Provides MCP-aligned types, decorators, AI format converters, and a standalone iframe SDK for creating plugins that integrate with the okDoc voice assistant.

## Plugin types

| Type | Runtime | Best for |
|------|---------|----------|
| **Iframe plugin** | Standalone HTML page in an `<iframe>` | Third-party developers, any tech stack, full isolation |
| **Remote plugin** | Angular web component loaded at runtime | First-party / trusted plugins, deep host integration |

## Quick start — Iframe plugin (no npm required)

Add a single `<script>` tag and declare your tools:

```html
<script src="https://cdn.jsdelivr.net/npm/@okdoc-ai/plugin-sdk@1/dist/okdoc-iframe-sdk.js"></script>
<script>
  const sdk = OkDocIframeSDK.create({
    pluginId: 'my-plugin',
    displayName: 'My Plugin',
    tools: [
      {
        name: 'greet',
        description: 'Say hello',
        inputSchema: {
          type: 'object',
          properties: { name: { type: 'string', description: 'Name to greet' } },
          required: ['name'],
        },
        handler: async ({ name }) => ({
          content: [{ type: 'text', text: `Hello, ${name}!` }],
        }),
      },
    ],
  });
</script>
```

> **TypeScript support:** Drop [`okdoc-iframe-sdk-global.d.ts`](https://cdn.jsdelivr.net/npm/@okdoc-ai/plugin-sdk@1/dist/okdoc-iframe-sdk-global.d.ts) into your project for full autocompletion.

See the full guide: [DOCS/IframePluginGuide.md](DOCS/IframePluginGuide.md)

## Quick start — Remote plugin (Angular)

```bash
npm install @okdoc-ai/plugin-sdk
```

```typescript
import { OkDocPlugin, McpTool } from '@okdoc-ai/plugin-sdk';

@OkDocPlugin({
  pluginId: 'weather',
  displayName: 'Weather Plugin',
  version: '1.0.0',
})
class WeatherPlugin {
  @McpTool({
    name: 'get_weather',
    description: 'Get the current weather for a city',
    inputSchema: {
      type: 'object',
      properties: { city: { type: 'string', description: 'City name' } },
      required: ['city'],
    },
  })
  async getWeather({ city }: { city: string }) {
    return { content: [{ type: 'text', text: `Weather in ${city}: Sunny, 25°C` }] };
  }
}
```

See the full guide: [DOCS/RemotePluginGuide.md](DOCS/RemotePluginGuide.md)

## Exports

| Import path | Purpose |
|-------------|---------|
| `@okdoc-ai/plugin-sdk` | Core types, decorators (`@OkDocPlugin`, `@McpTool`), metadata readers, registration |
| `@okdoc-ai/plugin-sdk/angular` | Angular integration (`OkDocNotifier`, `OKDOC_NOTIFIER_TOKEN`) |
| `@okdoc-ai/plugin-sdk/handler` | Host-side AI format converters (`toGeminiFunctionDeclarations`, `toOpenAiFunctions`) |

## CDN / jsdelivr

The standalone iframe SDK is served via [jsdelivr](https://www.jsdelivr.com/):

```
https://cdn.jsdelivr.net/npm/@okdoc-ai/plugin-sdk@<version>/dist/okdoc-iframe-sdk.js
```

Replace `<version>` with a specific tag (e.g. `1.0.0`) or use semver ranges (`@1`, `@1.0`).

## Documentation

| Guide | Description |
|-------|-------------|
| [Iframe Plugin Guide](DOCS/IframePluginGuide.md) | Build iframe plugins (any tech stack, no npm) |
| [Remote Plugin Guide](DOCS/RemotePluginGuide.md) | Build Angular remote plugins |
| [Sample Remote Component Guide](DOCS/SampleRemoteComponentDevelopmentGuide.md) | Step-by-step sample remote component |
| [Ionic Angular Project Setup](DOCS/IonicAngularProjectSetup.md) | Host app project setup reference |

## Telemetry

The iframe SDK reports anonymous plugin metadata (plugin name, version, tool names) to okDoc servers when a plugin is discovered by a host application. This helps us understand how the SDK and plugins are used in the ecosystem.

## License

Proprietary — Copyright © 2025-2026 OkDoc AI. All rights reserved. See [LICENSE](LICENSE) for terms. The Software is licensed for use as a runtime dependency of OkDoc plugins; redistribution, modification, and reverse engineering are prohibited.
