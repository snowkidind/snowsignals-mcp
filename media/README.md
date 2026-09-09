# SnowSignals / TrendVane — branding media

Public branding assets for directory listings, logos, favicons, and social/hero images.

**Use the GitHub Pages URLs (below) for anything a third-party service fetches** — GitHub Pages is served
by a CDN built for public assets. `raw.githubusercontent.com` also serves these files, but GitHub
**403s automated/datacenter fetches** of raw (it renders fine in a browser but breaks server-side
consumers like the APIs.guru logo validator), so prefer Pages when handing a URL to another service.

| Asset | Size | Use | Canonical URL (GitHub Pages) |
|---|---|---|---|
| `trendvane-icon.png` | 512×512, square | Logo / favicon / directory icon (e.g. APIs.guru, displayed 100×100) | `https://snowkidind.github.io/snowsignals-mcp/media/trendvane-icon.png` |
| `trendvane-card.png` | 588×798 | The full TrendVane card (wordmark + gauge) — hero / social / docs | `https://snowkidind.github.io/snowsignals-mcp/media/trendvane-card.png` |

The square icon is a centered crop of the gauge dial from the card (the wordmark is dropped because it
goes illegible at small sizes). The `raw.githubusercontent.com/snowkidind/snowsignals-mcp/master/media/<file>`
form works for browser/manual use, but see the raw-403 caveat above before giving it to a service.
