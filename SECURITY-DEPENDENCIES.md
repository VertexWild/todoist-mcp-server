# Dependency security review — 2026-11-11

> [!CAUTION]
> 🔴 **A pinned dependency version is covered by published security advisory ranges.** The current implementation uses stdio, which the two identified transport advisories explicitly exclude. This is a dependency warning, not a claim that this server is exploitable or compromised.

## Evidence and scope

Repository: `VertexWild/todoist-mcp-server`; branch `main`; inspected revision `70802047442604b197c59e3ea0706f173f4e6e48`.

`package.json` pins `@modelcontextprotocol/sdk` to **0.5.0**. No dependency lockfile is committed. The actual installed versions of other direct or transitive dependencies therefore cannot be determined from this repository. No package installation, application execution, exploit test or dependency change was performed.

| Package / declared version | Advisory severity | Advisory | Published affected range | First patched version | Applicability observed |
|---|---|---|---|---|---|
| @modelcontextprotocol/sdk 0.5.0 | High | [GHSA-w48q-cv73-mx4w](https://github.com/advisories/GHSA-w48q-cv73-mx4w) | <1.24.0 | 1.24.0 | DNS rebinding issue concerns HTTP-based local servers without authentication; advisory explicitly excludes stdio. |
| @modelcontextprotocol/sdk 0.5.0 | Medium | [GHSA-6prh-2h8m-c8cw](https://github.com/advisories/GHSA-6prh-2h8m-c8cw) | <1.32.0 | 1.32.0 | Redirect issue concerns HTTP client transports/OAuth; advisory explicitly excludes stdio clients. |

`src/index.ts` imports `StdioServerTransport`, constructs it and connects the server through it. No HTTP MCP transport was observed in this entry point. These versions satisfy the advisory database ranges, but **the current stdio server is not shown to meet the affected transport conditions**. Do not describe these findings as a proven attack path.

## Agent guidance

- Reassess these advisories before introducing any HTTP/SSE transport or SDK OAuth client.
- Treat the correction versions as advisory-specific minima, not a guarantee of overall safety or a drop-in upgrade. A move from SDK 0.5.0 to 1.x needs compatibility review.
- Obtain and scan the actual resolved dependency tree before claiming complete dependency coverage. Do not infer the installed version from a caret constraint.
- Do not remove this note merely because the application builds. Document the resolved versions and transport review when superseding it.
- This review authorizes no automatic dependency upgrade, execution, deployment, credential change or third-party communication.

## Limitations

Public GitHub advisory records were inspected on the date above. Private Dependabot alert state, installed environment, runtime, operating system, published npm package and deployments were not inspected. Other manifest dependencies use version ranges rather than an exact installed version. No claim of exhaustive absence of vulnerabilities is made.
