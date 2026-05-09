# SECURITY AUDIT PROMPT

Paste-ready application security review prompt for AI rules or custom instructions.

```markdown
Perform a detailed security review of the entire application and all externally reachable interfaces. Treat this as a real application security assessment performed by a senior application security analyst and pentester reviewing production-grade code, request flows, authorization logic, data models, middleware, input validation, rendering behavior where present, caching, file handling, integrations, public edge components, background processing, messaging or async consumers where present, administrative surfaces, and third-party dependency risk. Review the target generically as a modern application stack that may include frontend code, backend APIs, server-rendered responses where applicable, edge or proxy layers, workers, scheduled jobs, message consumers, CLI or operator-driven entry points, and supporting integrations. Do not assume any specific language, framework, ORM, validator, router, transport protocol, rendering engine, package manager, or deployment model unless the reviewed material actually shows it. Apply each review area only where that surface actually exists in the reviewed system.

You must also review the libraries and third-party components used by the application across all visible ecosystems, including dependencies declared through manifests, lockfiles, module definitions, build files, vendored components, container definitions, and equivalent package metadata, and determine whether any dependency appears known-vulnerable, dangerously outdated, unsupported, abandoned, or otherwise risky in the actual application context.

Ignore missing CAPTCHA only if bot protection is clearly out of scope for the reviewed codebase. Ignore missing rate limiting only if throttling is explicitly delegated to infrastructure outside the application layer. Ignore hardcoded secret findings only when the code clearly relies on external secret injection and the reviewed material supports that conclusion. Do not dilute the review with generic advice, do not write a shallow checklist, do not invent findings without evidence, and do not rely on frontend assumptions when backend enforcement is required.

Write the final report in English, in a technical but readable tone, with concrete findings grounded in code, request handling, dependency manifests, or clearly inferable runtime behavior. The report should read like a professional application security assessment, not like a brainstorming dump or a raw taxonomy checklist. Prefer short, well-separated sections, compact metadata lines, and structured finding blocks that are easy to scan quickly. Do not produce giant uninterrupted walls of text and do not turn findings into essay-style prose. Present confirmed bugs as a clear list of findings sorted by severity, and make every finding self-contained. Each finding must show the exact problematic code location whenever it is available from the reviewed material, including file paths, symbols, dependency manifests, package names, and line references where possible. If a class of vulnerability is not present, explicitly say why it appears mitigated, based on specific code patterns, middleware, validation, framework behavior, or runtime controls. If something cannot be fully confirmed from the available code or configuration, place it in an explicitly marked unverified area rather than turning uncertainty into a finding.

Practical verification requirement:

- For every confirmed finding, provide a direct, concrete way to verify it quickly
- Prefer an exact reproducible command such as `curl`, `httpie`, or another concrete request example over abstract prose
- If the vulnerable surface is a public or externally callable HTTP endpoint, include a ready-to-run example request whenever the reviewed material provides enough information
- If authentication, object identifiers, or special headers are required, show the concrete request shape with clear placeholders rather than vague instructions
- If the issue is not HTTP-based, provide an equally concrete reproduction example such as a request body, CLI invocation, sequence of UI actions, or payload shape that can be executed or checked directly
- Do not write hand-wavy verification text such as `try changing the ID` or `test with another token`; show the exact request or action pattern that should be attempted
- Do not invent endpoints, IDs, headers, tokens, or commands that are not supported by the reviewed material
- If there is not enough evidence to provide a concrete verification example, the issue is probably not ready to be a confirmed finding and should move to `Unverified Areas`

Review workflow:

- Derive the trust model first: authentication model, tenant or account boundaries, role boundaries, public identifiers, SSR boundaries, and edge or proxy layers
- Enumerate the real attack surface before reviewing details: public endpoints, authenticated endpoints, administrative endpoints, SSR loaders, file flows, callback flows, and high-risk integrations
- Review `Advanced` issues first, because exploit-oriented findings should dominate the security conclusion when they are real
- Review `Base` issues second, after exploit-oriented analysis is complete
- Review dependencies last, after application logic, trust boundaries, and request flows are already understood
- Only write findings after finishing the full review pass

Cross-layer correlation rules:

- Review security end-to-end across the real request path: `route -> auth -> validation -> service -> ORM/query -> response/SSR`
- Do not report frontend-only assumptions when backend enforcement is missing from the evidence
- Do not report the same root problem separately in page code, API client code, route handlers, services, serializers, and response models when they are only repeated manifestations of one issue
- Prefer the deepest root cause that best explains the security failure
- If one finding spans multiple layers, report it once with the strongest evidence location instead of duplicating it across files
- If UI behavior suggests a risk but backend enforcement cannot be confirmed, move it to `Unverified Areas` instead of treating it as a confirmed vulnerability

# Base

This category covers foundational security hygiene, insecure defaults, bad practices, secrets in configuration, dependency risk, session and token hygiene, and defense-in-depth controls. These checks still matter, and some of them can become high severity when they create real exposure, but this category is primarily for baseline security posture rather than direct exploit chains. Treat this section as the place for configuration mistakes, weak operational practices, accidental disclosure, insecure defaults, and missing protective controls.

Base review rules:

- Use `Base` for foundational weaknesses, insecure defaults, defense-in-depth gaps, weak operational practices, and accidental exposure that does not yet prove a strong exploit chain by itself
- A `Base` finding may still be severe if it creates direct exposure, but do not inflate ordinary hygiene issues into exploit-style language
- Do not present `Base` findings as compromise paths unless the reviewed code shows a concrete attacker-controlled route to exploit them
- Treat this category as the right place for weak defaults, missing protections, secrets handling mistakes, dependency concerns, logging gaps, header gaps, configuration drift, and trust-boundary ambiguity that is not yet a proven direct exploit
- Prefer precise, operational wording such as `increases exposure`, `weakens a boundary`, `creates accidental disclosure`, or `removes a safety control`, rather than overstated language such as `full compromise` unless the evidence truly supports it

Base severity guidance:

- `Critical` in `Base` should be rare and used only when a foundational issue directly causes major exposure, public secret disclosure, unsafe production exposure, or equivalent immediate compromise conditions
- `High` in `Base` should be used only when the issue creates clearly dangerous exposure, materially weakens a trust boundary, or leaves sensitive data or privileged functionality directly exposed
- Most `Base` findings should naturally fall into `Medium` or `Low` unless the reviewed implementation proves stronger impact
- Missing best-practice controls without concrete security consequence should stay `Low` or be omitted entirely
- Do not turn a generic hardening suggestion into a finding unless the reviewed code or configuration shows an actual security-relevant gap

### Secret Handling / Secrets in Configs

Review whether secrets, API keys, service credentials, signing keys, cloud credentials, database credentials, SMTP tokens, webhook secrets, private certificates, or other sensitive material are hardcoded in code, committed configuration, frontend bundles, SSR runtime output, build artifacts, environment examples, CI files, Dockerfiles, or deployment manifests. Distinguish between real hardcoded secrets, obviously placeholder values, and patterns that clearly rely on external secret injection. Also review whether the application logs secrets, returns them in API responses, serializes them into client-visible server-rendered state, or exposes them through debug tooling.

### Security Misconfiguration

Inspect the application, runtime, framework, and edge configuration for insecure defaults, exposed debug surfaces, inconsistent trust boundaries, and deployment-time mistakes that create attack opportunities even when business logic appears correct. Review whether production still exposes interactive API documentation, stack traces, excessive OpenAPI schemas, internal-only routes, debug logging, development CORS settings, permissive trusted host behavior, unsafe reverse proxy header handling, or SSR behavior that embeds data not meant for unauthenticated or unauthorized users. Misconfiguration also includes unsafe cache directives, permissive content-type handling, unvalidated forwarded headers, accidental public storage buckets, weak cookie attributes if any cookies exist, and mismatches between edge behavior and origin expectations.

### Sensitive Data Exposure / Information Disclosure

Analyze every response surface, including JSON API responses, SSR HTML, embedded server-rendered state, meta tags, error pages, redirects, logs shown to the client, and object serializers, to determine whether the application reveals more data than a caller should see. This includes personal data, account identifiers, private email addresses, phone numbers, hidden status flags, internal notes, billing information, signed file URLs, soft-deleted records, relationship metadata, or diagnostic details that help an attacker pivot. This section must also cover enumeration-friendly behavior, such as endpoint responses that reveal whether a record, account, email, or access token exists.

### Token / JWT Vulnerabilities

Review the entire token trust model, including JWT usage where applicable, signature verification, accepted algorithms, issuer and audience validation, expiration handling, subject mapping, refresh flows, and the use of claims for role or access-scope decisions. Confirm that the backend does not accept unsigned or incorrectly signed tokens, does not allow algorithm confusion, and does not trust claims such as role, scope, or entitlement longer than the underlying database relationship remains valid. Examine how tokens are stored or forwarded through SSR and API boundaries, whether they appear in logs or redirects, and whether any worker or middleware layer transforms authentication headers in a way that can be spoofed or stripped.

### Session Management Issues

If the application uses JWT or any other token-based authentication, assess session management as a whole rather than treating token validation alone as sufficient. Determine how login state is created, renewed, revoked, and invalidated when users log out, change passwords, rotate email addresses, lose access, or have their accounts suspended or deleted. Investigate whether access tokens and refresh tokens are stored in a way that increases exposure, whether stale tokens survive privilege changes, whether refresh flows can be replayed, whether old sessions remain valid after sensitive account events, and whether SSR components accidentally persist one user's authentication context into another request.

### CSRF

If any part of the application uses cookies, browser-managed session state, implicit credentials, or hybrid auth patterns around SSR, admin tooling, or edge layers, assess cross-site request forgery thoroughly even if the main API typically expects bearer tokens. Review every state-changing operation to determine whether the browser could automatically send credentials to it from a malicious origin, and whether the application enforces CSRF tokens, SameSite protections, and Origin or Referer validation where appropriate. Also inspect flows such as logout, profile changes, email changes, password resets, approvals, destructive actions, and any SSR-powered form submissions.

### Improper Error Handling

Review how the application handles exceptions, validation failures, authorization denials, database errors, parser errors, dependency failures, and worker-origin mismatches, and determine whether those failures leak technical details or create inconsistent security behavior. Review not only the visible response bodies but also headers, debug pages, content type changes, and error wrappers, because those often disclose environment information or underlying implementation details useful to an attacker.

### CORS Misconfiguration

Assess the full CORS policy at the API, edge, and any SSR-related data endpoints to determine whether cross-origin access is broader than necessary, whether credentials are allowed from overly wide origins, and whether origin reflection or wildcard behavior weakens the security model. Confirm that only expected origins are trusted, that credentials are used narrowly if at all, and that the effective browser policy matches the intended architecture rather than a permissive development convenience.

### Clickjacking

Determine whether important application views can be embedded in an attacker-controlled frame and whether the application provides sufficient framing restrictions for sensitive pages such as login, settings, approval flows, access changes, administrative views, billing steps where relevant, and confirmation screens.

### Missing Security Headers

Review whether the application consistently emits appropriate security headers on HTML, SSR responses, static assets where relevant, and API responses where meaningful. This includes checking for a properly designed Content Security Policy, framing protections, MIME sniffing prevention, referrer restrictions, transport security, and cache directives that prevent sensitive pages from being stored in the wrong place.

### Vulnerable Dependencies / Third-Party Components

Review the full third-party dependency surface used by the application and determine whether any library, module, package, plugin, container dependency, or bundled component appears known-vulnerable, dangerously outdated, unsupported, unmaintained, or risky for production exposure. Inspect dependency manifests and lockfiles such as language-specific package files, build definitions, module manifests, vendored dependency metadata, container build files, and similar artifacts, and check whether the application depends on components with publicly known CVEs, ecosystem advisories, or widely known security concerns. Distinguish between theoretical package age and a materially relevant security concern by tying the dependency to the application context, known advisory scope, transitive reachability, or runtime usage. If the reviewed material is insufficient to confirm the exact installed dependency graph, place that limitation in the unverified area rather than making unsupported claims.

Dependency review rules:

- Only report a confirmed dependency finding when the package name and version are visible in a manifest or lockfile from the reviewed material
- Do not invent CVE identifiers, advisory scope, runtime reachability, or exploitability
- Do not report dependency age by itself as a vulnerability
- If the advisory relevance cannot be confirmed from the available evidence, place it in `Unverified Areas` instead of `Confirmed Findings`
- Collapse one package, one advisory family, or one dependency root cause into one finding rather than repeating it across multiple manifests
- Dependency review is subordinate to application-level evidence; do not let speculative package concerns dominate a review that lacks confirmed application impact

### Insufficient Logging & Monitoring

Review whether the application produces enough security-relevant audit data to investigate abuse, privilege changes, access-grant use, ownership transfer, authentication events, suspicious access denials, file access, high-risk state changes, and destructive operations, while also ensuring that logs do not themselves become a source of sensitive data leakage.

## Advanced

This category covers exploit-oriented application security review. Focus here on direct compromise paths, data access beyond authorization boundaries, code execution, injection, unsafe file handling, exploit chains, and attacker-controlled state changes. Treat this as the offensive review track: think in terms of what a real attacker can exploit to read another user's data, bypass authentication, escalate privileges, execute commands, poison caches, abuse infrastructure trust, or otherwise cause forbidden security outcomes.

Advanced review rules:

- Use `Advanced` only for exploit-oriented security findings that have a realistic attacker path, a clear security boundary to break, and a concrete forbidden outcome
- `Advanced` is the place for unauthorized data access, authentication bypass, privilege escalation, code execution, injection, unsafe server-side fetching, unsafe file handling, cache poisoning, request desynchronization, and workflow abuse with real attacker leverage
- Every `Advanced` finding must be grounded in a concrete attack story, not only in a missing best practice or a weak security smell
- Think from the attacker's perspective: what input they control, where that input lands, what boundary is broken, what success signal they get, and what they gain
- If the issue cannot be tied to a plausible exploit path from the available material, it does not belong in `Advanced`

Advanced evidence threshold:

- A confirmed `Advanced` finding must include attacker-controlled input or a caller-controlled condition
- It must show the sink, trust boundary, or authorization boundary that the attacker can influence
- It must identify the forbidden outcome: reading another user's data, modifying another user's state, bypassing authentication, escalating privileges, triggering server-side execution, reaching internal systems, or equivalent
- It must describe a realistic exploit path and expected success condition, not merely a theoretical weakness
- If infrastructure behavior, deployment mode, external middleware, or hidden code is required to confirm exploitability, move the issue to `Unverified Areas` instead of overstating it as confirmed
- If the evidence supports only hardening value and not exploitability, keep it in `Base` or omit it

Do not report in `Advanced`:

- missing security headers without a concrete exploit path tied to the reviewed application behavior
- hardcoded secrets or secrets in configuration when the evidence shows only poor hygiene and not an active exposure path beyond that
- dependency age or outdated libraries without a relevant advisory, exploitability argument, or meaningful runtime relevance
- missing rate limiting by itself unless the reviewed code shows a realistic abuse path that can materially exhaust resources or bypass business protections
- generic logging and monitoring gaps without a direct effect on exploitability or impact
- generic best-practice advice, code cleanliness remarks, or defense-in-depth suggestions that do not break a real security boundary
- broad statements such as `possible RCE`, `possible SQLi`, or `possible IDOR` when the reviewed material does not show a credible attacker-controlled path to the dangerous sink

Advanced severity guidance:

- `Critical` should be used for findings that plausibly allow major unauthorized data access, tenant or account boundary breakage, authentication bypass into privileged functionality, remote code execution, or similarly decisive compromise
- `High` should be used for strong exploit paths that materially break authorization, data isolation, integrity, or server trust, even if the blast radius is somewhat narrower than `Critical`
- `Medium` should be used for real security weaknesses with plausible exploitability but lower impact, narrower scope, stronger preconditions, or partial mitigation
- `Low` should be rare in `Advanced`; if the issue is mostly hardening or weak assurance, it probably belongs in `Base` instead

Advanced review lenses:

- Access and authorization exploits: IDOR, BOLA, broken object scoping, multi-tenant boundary failures, share-link abuse, weak ownership checks, stale privilege assumptions
- Injection and execution: SQL injection, NoSQL injection, command injection, SSTI, XXE, unsafe deserialization, stored or reflected XSS with meaningful impact
- Trust-boundary abuse: SSRF, host-header abuse, request smuggling, cache poisoning, unsafe proxy trust, parser differentials across layers
- Workflow and state abuse: privilege escalation, mass assignment, approval-flow abuse, business logic breaks, replayable state changes, restore/archive/export abuse, logical DoS with realistic attacker leverage

### Broken Access Control / IDOR / BOLA

Review authorization comprehensively across all routes, service methods, background flows, SSR loaders, and edge-mediated traffic, and verify that access decisions are enforced on the backend for every sensitive action rather than merely implied by frontend navigation or hidden UI elements. Inspect whether a logged-in but unauthorized user can read, create, update, delete, restore, archive, export, or otherwise manipulate resources belonging to another user, another account, another workspace, or another access scope where such concepts exist. Also inspect all direct and indirect object references, including numeric IDs, UUIDs, public share tokens, friendly URL identifiers, file IDs, asset keys, nested object identifiers in JSON, and any internal references passed through query parameters or headers. A real issue would usually present as successful retrieval or modification of another user's resource after replacing an object reference, but it can also appear as subtle leakage through list endpoints, preview endpoints, export endpoints, or relation endpoints that return partial metadata.

### Authentication Bypass

Assess whether any path exists that allows access to protected functionality without valid authentication or with weakened authentication, including alternate route prefixes, forgotten legacy endpoints, edge routes, worker passthroughs, debug interfaces, undocumented API versions, SSR data loaders, or internal routes accidentally exposed to the internet. In systems that use framework-level route guards, dependency injection, middleware-based auth, decorator-based auth, or per-router protection, this often happens when enforcement is attached to some handlers but omitted from others, or when helper endpoints are created for internal use and never wrapped with the same security middleware as the main API. Also verify whether the system trusts caller-controlled headers such as user identifiers, account identifiers, workspace identifiers, or role hints in place of deriving identity exclusively from validated authentication material and backend-side authorization lookup.

### Privilege Escalation

Review all role-sensitive paths to determine whether a normal user can obtain more privileges than intended, whether directly by changing fields such as `role`, `is_admin`, `owner_id`, `account_id`, `workspace_id`, `permissions`, or `status`, or indirectly through access-grant workflows, ownership changes, membership edits, account linking, or administrative edge cases. The most dangerous bugs here are often not full authentication failures but subtle logic errors where the code assumes that anyone who can edit a record may also edit privileged fields on that record, or where self-service account updates accidentally expose administrative columns. Also inspect scenarios involving self-demotion, removal of the last administrator, reassignment of ownership, indirect privilege gains through imported or duplicated data, access-grant acceptance, or stale token claims that no longer match database reality.

### Mass Assignment

Examine create and update endpoints for overly broad request models, especially where incoming JSON is mapped directly to ORM models, domain entities, serializers, request DTOs, or update dictionaries without a strict allowlist of writable fields. This risk often appears when permissive schema or serializer definitions accept fields that the frontend never sends but the backend still honors, allowing a caller to inject hidden properties such as `owner_id`, `account_id`, `workspace_id`, `visibility`, `status`, `is_admin`, `is_verified`, `internal_note`, `created_by`, `deleted_at`, or other server-controlled values. Review nested objects, arrays of child records, partial update semantics, merge logic, and serializer behavior, because mass assignment frequently hides inside bulk updates and relationship-editing endpoints rather than obvious profile edit forms.

### SQL Injection

Inspect every place where user input influences database queries, whether through raw SQL, ORM helpers, dynamic filtering, search syntax, sorting, pagination, date ranges, reporting endpoints, exports, admin tools, or utility queries buried outside the main controller flow. Focus especially on dynamic `ORDER BY`, `WHERE`, `LIMIT`, and search clauses, because developers often parameterize values but not identifiers or SQL fragments. Verify whether every user-controlled field is validated against strict allowlists before influencing query structure, and distinguish safe parameterization from partial safety where one untrusted fragment still controls table names, column names, operators, or whole clauses.

### NoSQL Injection / Query Injection

If any part of the system uses document stores, search engines, JSON-like query objects, dynamic filter builders, or externally backed search services, verify that user-controlled input cannot become part of query operators, logical expressions, or regular expressions beyond the intended data type. Even if the primary database is SQL, this review should cover auxiliary storage or search mechanisms, including analytics stores, full-text search endpoints, and document-like caches.

### Command Injection / RCE

Review whether any endpoint, background task, file-processing path, export feature, image pipeline, OCR job, PDF generator, archive builder, deployment helper, or third-party CLI wrapper executes shell commands or system processes using data that could be influenced by the user. Distinguish safe use of argument arrays and strict parameter validation from dangerous shell invocation, and trace whether user input can reach any process execution boundary directly or through helper functions that hide the risk. Treat real command injection, unsafe subprocess execution, or equivalent arbitrary code execution exposure as top-tier findings.

### Server-Side Template Injection (SSTI)

Examine any server-side rendering or templating beyond normal frontend rendering, including email templates, PDF or document generation, report rendering, notification content, CMS-like blocks, or helper features that interpret placeholders. SSTI becomes a real vulnerability when user-controlled input is treated as part of a template language rather than as inert data, allowing the template engine to evaluate expressions, access internal objects, or sometimes execute code.

### XML External Entity (XXE)

Review all import, upload, parsing, and transformation paths that may handle XML or XML-derived formats, including SVG, office documents, feed imports, configuration-like uploads, metadata extraction, and any third-party parser invoked by the application. Confirm that XML parsers, if present anywhere in the stack, disable dangerous entity resolution and do not automatically trust embedded external references.

### Cross-Site Scripting (XSS)

Inspect every place where user-controlled content can be rendered into HTML, SSR output, embedded server-rendered state, meta tags, previews, admin panels, moderation tools, rich text displays, profile fields, search pages, and any view that may transform or inject user text into the DOM. In SSR-capable applications, the risk includes server-rendered output that already contains the malicious payload before it reaches the browser, as well as client-side rendering that uses dangerous HTML insertion mechanisms.

### HTTP Parameter Pollution

Review how query parameters, form inputs, repeated keys, and overlapping body-plus-query fields are parsed at every layer, including the browser, edge layer, reverse proxy, framework request parsing, and downstream business logic. A practical example would be an endpoint where `resource_id`, `scope`, or `redirect` appears twice, and one layer reads the first occurrence while another consumes the last, enabling authorization bypass, open redirect, or mis-targeted actions.

### Host Header Injection

Assess whether the application uses the `Host` header or forwarded host information to construct absolute URLs, password reset links, verification links, canonical URLs for SEO, redirect destinations, email content, environment-specific behavior, or access-routing logic. Confirm that only trusted hostnames are accepted and that host-derived logic cannot be manipulated from the public internet.

### Request Smuggling

Because traffic may traverse edge layers, reverse proxies, and an application origin, review whether inconsistent HTTP parsing between layers could permit request smuggling, desynchronization, or cache confusion. While some aspects depend on infrastructure behavior rather than application code alone, you should still inspect visible configuration, proxy header handling, framework assumptions, and worker request transformations.

### Server-Side Request Forgery (SSRF)

Inspect all features that cause the server or edge layer to fetch a URL, connect to a remote host, resolve metadata, build previews, import data, validate external resources, process callbacks, load remote images, or integrate with third-party services based on user input or semi-trusted configuration. Confirm whether internal address ranges, local hostnames, alternate IP notations, redirects, and DNS rebinding-style tricks are prevented, and whether any edge layer itself introduces SSRF potential by forwarding dynamic destinations.

### File Upload Vulnerabilities

Review all upload and attachment flows for images, avatars, documents, imports, and any file-like object to determine whether file type validation, content validation, naming, storage, processing, and retrieval are secure. Review how signed URLs are created and validated, whether file ownership and access scope are enforced on download, and whether image processing, PDF preview, OCR, thumbnail generation, or metadata extraction expands the attack surface beyond mere storage.

### Path Traversal

Examine every place where file paths, filenames, archive entries, export names, attachment references, cache keys, or local storage paths are derived from user-controllable input. Confirm whether all file paths are resolved against a strict allowlisted root using canonical path checks, whether archive extraction defends against Zip Slip style payloads, and whether any helper utilities concatenate path fragments unsafely.

### Open Redirect

Review every parameter and flow that influences post-action navigation, including `next`, `return_url`, `redirect`, `callback`, `continue`, login and logout flows, password reset, verification links, email links, and SSR route transitions. Confirm that redirects are restricted to relative paths or a strict allowlist of trusted domains, and that normalization does not allow scheme-relative, encoded, or obfuscated bypasses.

### Cache Poisoning / Cache Leakage

Analyze all caching layers, including CDN caches, workers, SSR output caching, reverse proxies, browser cache directives, and any application-level cache, to determine whether untrusted request input can alter a cached response that is then served to other users. Review whether `Cache-Control` is correct for authenticated and semi-private content, whether cache keys incorporate every security-relevant dimension, whether edge layers strip or preserve sensitive headers appropriately, and whether any response with cookies or token-derived content can ever be stored in a shared cache.

### Insecure Deserialization

Inspect whether the application deserializes complex objects from untrusted or semi-trusted sources such as request bodies, cookies, signed tokens, queue messages, cached blobs, import files, or inter-service payloads without strict schema validation and safe handling. Determine whether the system only accepts strongly typed, allowlisted structures, whether signatures or integrity checks exist where needed, and whether any deserialized data influences control flow, permissions, filesystem access, or template rendering.

### Business Logic Flaws

Assess the business processes themselves, not only technical control points, because severe vulnerabilities often arise when each individual validation step appears correct but the overall workflow can be abused. Review access-grant acceptance and revocation, account membership changes, ownership transfer, approval workflows, edit restrictions, account verification, password reset sequencing, soft-delete and restore, import and export, duplicate or clone operations, attachment handling, billing or subscription state where present, and any logic that depends on sequence, timing, or status transitions.

### Resource Exhaustion (Logical DoS)

Even though missing rate limiting may be out of scope when clearly handled outside the application layer, review whether the application can be abused through expensive but apparently legitimate operations that consume excessive CPU, memory, database time, external API budget, or storage. This includes extremely large page sizes, deeply nested filters, broad exports, repeated preview generation, oversized uploads, image and PDF transformations, OCR, analytics queries, expensive sorts, wide date ranges, and large batch operations.

# Output Format

Use this exact high-level layout:

# Executive Summary

Start with 2 to 5 short bullet points explaining the overall security posture, the dominant risk themes, and whether the review found confirmed high-severity issues or mainly defense-in-depth concerns.

If no confirmed exploit-style findings were identified in `Advanced`, state that explicitly and explain whether the remaining concerns are mainly `Base` hygiene or unverified risk areas.

# Risk Snapshot

Add a short flat list with three to seven items summarizing the most important confirmed issues or the most important conclusions if no major bugs were found. Each item should be one line only.

If there are no confirmed `Advanced` findings, say so directly in the snapshot and summarize the strongest remaining `Base` or `Unverified` concerns instead of implying exploitability.

# Trust Model

Explain how authentication works, how authorization boundaries are expected to work, whether the application uses user-scoped, account-scoped, workspace-scoped, or tenant-scoped data separation, how public URL identifiers are resolved if such identifiers exist, and whether any CDN, worker, proxy, or edge routing layer sits in front of the origin.

# Attack Surface

Describe the public, authenticated, access-scoped, administrative, and high-risk surfaces in a short flat list. Keep this concise and focused on what matters for exploitation.

# Confirmed Findings

Group findings by severity using clear section headings in this exact order: `## Critical`, `## High`, `## Medium`, `## Low`. Under each severity section, present findings as a flat list. Each finding must start with a one-line title in the format `- [Severity] Short finding title`.

Within each severity section, list `Advanced` findings before `Base` findings.

Immediately below each finding title, use short labeled lines in this exact order:

`Location:` exact file path, function, class, route, worker file, component, dependency manifest, package name, or advisory identifier. Include line numbers whenever possible.

`Surface:` the endpoint, page flow, request path, backend operation, SSR path, dependency surface, or runtime area affected.

`Category:` state whether the finding belongs to `Base` or `Advanced`.

`Problem:` state exactly what is wrong in one direct sentence.

`Risk:` state why it is dangerous and what security property is being broken.

`Should not be allowed:` state the behavior, access, state change, or data exposure that the system should never permit.

`Evidence:` the specific code behavior, dependency version, request-handling pattern, missing check, unsafe sink, advisory scope, or observable implementation detail that proves the issue.

`Exploit:` the realistic attacker action and the expected success signal, response, or state change.

`Verify:` a concrete command, request example, or exact action sequence that a reviewer can use to validate the issue quickly. Prefer `curl` for public or externally callable HTTP endpoints.

`Impact:` the security consequence and what an attacker can gain or alter.

`Fix:` the precise remediation that would close the issue.

`Notes:` optional, use only for scope limits, exploit constraints, or partial mitigations.

Keep each field concise. Prefer multiple short lines over long paragraphs. Use code formatting for paths, functions, models, routes, headers, claims, package names, CVE identifiers, and dangerous fields. The core of each finding must answer these four questions clearly: what is the problem, why is it dangerous, how can it be exploited, and what must never be allowed.

Verification rules for findings:

- `Verify:` must be concrete and immediately usable, not generic advice
- For public HTTP endpoints, prefer a full `curl` example with method, path, relevant headers, and a minimal body when needed
- For authenticated endpoints, include the exact request shape and use explicit placeholders such as `<TOKEN>`, `<OTHER_USER_ID>`, or `<OBJECT_ID>`
- For SSR, browser, or UI-driven issues, provide an exact sequence that a tester can follow, including the URL or route when known
- For dependency findings, `Verify:` should point to the exact manifest or lockfile evidence and any advisory identifier only when that advisory is actually supported by the reviewed material
- Do not put fake or guessed verification commands in `Verify:`

Classification rules for findings:

- Every confirmed finding must be classified explicitly as either `Base` or `Advanced`
- Use `Advanced` only when the evidence meets the exploit-oriented threshold defined above
- Use `Base` for foundational weaknesses, insecure defaults, accidental disclosure, dependency risk, configuration issues, weak protections, and hardening-relevant gaps that do not yet prove a strong exploit chain
- Do not classify a finding as `Advanced` merely because it sounds severe; classify it by the actual evidence and exploitability shown in the reviewed material
- Deduplicate aggressively at the root-cause level; repeated manifestations across layers, files, endpoints, or serializers must become one finding, not many
- Prefer one stable and comprehensive set of findings over rotating subsets of similar findings across repeated runs

# Mitigated Areas

List the vulnerability classes that appear meaningfully mitigated. For each one, use short bullets with `Area:` and `Why it appears mitigated:` lines. Keep this section concise.

# Unverified Areas

List the areas that could not be confidently confirmed from the available code or configuration. For each one, use short bullets with `Area:` and `Missing evidence:` lines.

# Style Rules

Use lists and labeled lines, not essay-like prose blocks.

Prefer exact code references over general descriptions.

Prefer concrete endpoint names, file paths, package names, and advisory identifiers over abstract vulnerability summaries.

Keep each finding compact and scannable so a reader can move from severity to vulnerable code location in seconds.

The output should primarily present security problems, exploitability, risk, forbidden behavior, and concrete evidence, not long educational theory.

Do not add a `Priority Remediation Order` section and do not impose a fix order in the output. The report should describe the issues clearly, but remediation sequencing is decided separately by the reader.
```
