# About AI Usage Analyzer

AI Usage Analyzer is a desktop and web application for understanding how
developers use AI tools across software repositories. It combines repository
static analysis with optional telemetry and session-log analysis to estimate
tokens, credits, context usage, and activity.

## What it does

- Scans source repositories without executing scanned code.
- Estimates token usage, provider costs, and GitHub AI Credits.
- Analyzes supported dynamic logs and telemetry.
- Breaks usage down by repository, folder, language, file, provider, and time.
- Identifies large or high-usage files and possible optimization opportunities.
- Generates optimization recommendations with rationale, impact estimates, risk,
  confidence, and priority.
- Exports reports as JSON, CSV, Excel, and PDF.
- Caches repository analysis using a fingerprint to avoid unnecessary rescans.

## Architecture

The application keeps analysis and optimization responsibilities separate:

1. **AI Usage Analyzer** analyzes repository content and usage data and produces
   `AIUsageReport.json`.
2. **AI Optimization Advisor** consumes that report and produces actionable
   optimization recommendations.

Provider integrations are isolated behind provider-specific interfaces so
additional AI tools can be supported without changing the core analysis
engine.

## Supported providers

The current application supports:

- GitHub Copilot
- Claude Code

The provider model is designed to accommodate future integrations such as
Microsoft 365 Copilot, Cursor, Gemini Code Assist, Amazon Q Developer,
OpenAI Codex, JetBrains AI Assistant, Windsurf, and Continue.dev.

## Application areas

The interface includes:

- Dashboard
- Static Analysis
- Dynamic Analysis
- Optimization
- Charts
- Export
- Settings

Progress indicators show the current file, files analyzed, completion
percentage, estimated remaining time where available, and resource usage.

## Privacy and safety

Static analysis does not execute repository code. Analysis data, saved sessions,
credentials, source excerpts, prompts, and model responses may contain private
information and should be protected accordingly. Credentials are stored
locally and are never returned to the interface.

Cloud optimization is optional. When enabled, bounded source excerpts and usage
context may be sent to the selected provider. Failed cloud requests are
reported explicitly and retain the local fallback behavior where supported.

## Running from source

The Python CLI can analyze a repository and then generate recommendations:

```powershell
python .\main.py analyze --repo . --report AIUsageReport.json
python .\main.py optimize --report AIUsageReport.json --output OptimizationReport.json
```

The desktop application is provided as a Windows installer or portable package.
See [README.md](README.md) for installation, configuration, provider
authentication, packaging, and troubleshooting details.
