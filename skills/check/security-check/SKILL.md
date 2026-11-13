---
name: security-check
description: Use when code needs a security check or touches untrusted input.
---

# Security check

Trace the target, not the whole application. Find each **trust boundary** the target crosses, then follow untrusted data from its **source** to its **sink**. Untrusted data includes requests, files, webhooks, third-party responses and model output.

`review` runs this trace inside its security dimension and gives the findings its own shape. Run on its own, it reports in the conversation.

## 1. Pin the target

Settle what you are checking, such as a ref, a path or the working tree, and what it is compared against. With an empty diff, say so and stop.

## 2. Trace and sweep

1. **Trace**: For each trust boundary, follow the data through [Untrusted data reaching a sink](#untrusted-data-reaching-a-sink) and [Who is acting](#who-is-acting). Follow a control that lives outside the target, such as a gateway, when you can read it.
2. **Sweep**: Walk the other sections wherever the target touches them, with or without a boundary, such as a new dependency or a CI change.

This step is done when every boundary has been traced and every section that applies has been walked.

## 3. Report

Present the report in the conversation:

1. **Target**: What was checked, and what it was compared against.
2. **Boundaries**: Each trust boundary traced, in one line each, so the user can see what was covered.
3. **Findings**: Each one as a finding block, ordered Act on, Consider, Noted. Cite a secret by its location, never its value. With none, write "No findings."
4. **Closing line**: Any control outside the target you couldn't read, and anything else you couldn't assess.

Each finding takes this block:

```markdown
### <title: name the problem, not the category>

**Recommendation:** Act on / Consider / Noted · **Category:** Security · **Where:** `path/file.cs:42` · **Evidence rung:** <prove-it rung>

Why it matters, then the current shape and the suggested one, with code snippets only where prose is less precise.
```

1. **Act on**: Fix before this ships.
2. **Consider**: Worth doing, and it can wait.
3. **Noted**: An observation, with no action expected.

**Where** lists every implicated `file:line`. A problem the change neither caused nor worsened is pre-existing: start its title with `Pre-existing:`, and recommend **Noted** or **Consider**.

## Untrusted data reaching a sink

1. **Queries**: Is data bound as parameters, including in raw ORM calls and dynamic `ORDER BY` clauses?
2. **Commands**: Does the code pass an argument array rather than a shell string?
3. **HTML**: Is output contextually encoded? Check raw-HTML paths such as `innerHTML` and `dangerouslySetInnerHTML`.
4. **Paths**: Is the path canonicalized, then confirmed to sit inside the permitted root?
5. **Outbound URLs**: Can the caller steer the destination, including through redirects? This is SSRF.
6. **Redirects**: Is the target limited to known destinations?
7. **Deserialization**: Can untrusted data reach `pickle`, unsafe YAML loading, Java object deserialization, or an XML parser with external entities enabled?
8. **Code execution**: Can it reach `eval`, `exec`, `Function`, dynamic imports, or templates compiled from input?
9. **Regular expressions**: Can input trigger catastrophic backtracking?
10. **Object mapping**: Is input mapped to allowed fields only? Check mass assignment and prototype pollution.
11. **Prompts and model output**: Can untrusted text steer an agent into using its tools? Is model output treated as untrusted before it reaches a sink?

## Who is acting

1. **Object access**: Does each read and write confirm that this caller may touch this specific record, including tenant scope?
2. **Function access**: Is authorization enforced server-side on every new route, job and action?
3. **Sessions and tokens**: Are cookies `Secure`, `HttpOnly` and `SameSite`, and is the session rotated at login? Are a JWT's signature, algorithm, expiry and audience verified?
4. **Passwords**: Are they hashed with Argon2, scrypt or bcrypt?
5. **CSRF**: Do cookie-authenticated, state-changing requests carry a CSRF defence?
6. **Webhooks**: Is the signature verified against the raw body?
7. **Races**: Can a check-then-act sequence be raced, such as a balance check or a single-use token?

## Secrets and sensitive data

1. **Secrets**: Are they kept out of code and config? Treat a committed secret as compromised, and recommend rotating it.
2. **Logs and errors**: Do secrets, tokens and personal data stay out of logs, traces and error responses?
3. **Responses**: Does each response return only the fields this caller may see?
4. **Personal data**: Is new collection or sharing necessary, and is the data protected at rest?

## Cryptography

1. **Libraries**: Does the code use established libraries and authenticated encryption, such as AES-GCM or ChaCha20-Poly1305?
2. **Randomness**: Do tokens, keys and nonces come from a secure random source, with nonces never reused?
3. **Hashes**: Are MD5 and SHA-1 absent from security-sensitive use?
4. **Comparisons**: Are secrets compared in constant time?

## Files and resource limits

1. **Uploads**: Are size and content type checked, with files stored outside the web root under non-guessable names?
2. **Limits**: Do parsing, pagination, and new expensive or authentication endpoints have limits?

## Supply chain and configuration

1. **Dependencies**: Is each new package needed, maintained, spelled correctly and locked? Do lockfile changes match the manifest?
2. **CI**: Does untrusted pull request data stay out of `run:` steps? Check `pull_request_target`, actions pinned to a commit SHA, and token permissions.
3. **Configuration**: Check changed CORS rules, security headers, debug flags, TLS settings and default credentials.

## C and C++

1. **Copies**: Are they bounded, using `snprintf`, `std::string` or `std::span`?
2. **Format strings**: Are they constant?
3. **Memory**: Check object lifetimes, use-after-free, and integer overflow before allocation.
