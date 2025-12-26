# 🧪 Slopsquatting Demo — npm Supply-Chain Exploitation PoC

This repository contains an **educational proof-of-concept (PoC)** demonstrating **npm supply-chain risks**, with a specific focus on **slopsquatting** and **implicit code execution** in Node.js.

>[!Warning]
>Repo Docs and PoC are created with LLM's and may contain errors/inconsitencies. Global findings have been checked manualy, but may still be off for edge cases.
>Report any findings via the Issues to help with mitigation.
>
>This is **NOT** a full guide to npm security/hardening

It shows how a dependency can execute code:

- **During installation** (`postinstall`)
    
- **At runtime**, simply by being imported via `require()`

No explicit API usage is required.

---

## 📁 Repository Structure

```
Slopsquatting-Demo/
├── cookie-session-es6/
│   ├── index.js
│   ├── package.json
│   ├── package-lock.json
│   └── README.md
│
├── Exploitation PoC/
│   ├── index.js
│   ├── package.json
│   ├── package-lock.json
│   ├── node_modules/
│   ├── test/
│   └── README.md
│
│── GPT answers slopsquatting package.png
│
│── NPM-check on package name.png
│ 
└── README.md   ← (this file)
```

---

## 🔍 Component Overview

### 🔹 `cookie-session-es6/` — Malicious / Slopsquatted Package

This directory contains a **demo npm package** that appears legitimate but is intentionally crafted to demonstrate attack primitives:

- Code execution via `postinstall`
    
- Immediate execution at runtime through top-level module code
    
- Realistic slopsquatting risk (name similarity, no API usage required)


📄 See: `cookie-session-es6/README.md` for implementation details and publishing notes.

---

### 🔹 `Exploitation PoC/` — Victim Application

This directory represents a **consumer Node.js application** that installs and imports the demo package.

It demonstrates:

- Execution during `npm install`
    
- Execution triggered solely by `require("cookie-session-es6")`
    
- How unused or “harmless” dependencies still execute code
    
- Why install-time mitigations alone are insufficient
    

📄 See: `Exploitation - Slopsquatting PoC.md` for the full threat model, execution paths, and mitigations.

---

## 🎯 Purpose of This Repository

This project exists to:

- Demonstrate real npm supply-chain attack surfaces
    
- Highlight the difference between **install-time** and **runtime** execution
    
- Show why importing a package is a security-relevant action
    
- Encourage stricter dependency hygiene and auditing
    
- Provide a reproducible PoC for defensive security research

---

## ⚠️ Core Security Insight

> **Every dependency is executable code.**
> 
> - `npm install` is a code-execution event
>     
> - `require()` / `import` is a code-execution event
>     
> - Unused dependencies are an attack surface

---

## 📚 How to Navigate

|Goal|Start Here|
|---|---|
|Understand the malicious package|`cookie-session-es6/README.md`|
|See exploitation in practice|`Exploitation PoC/Exploitation - README.md`|
|Understand runtime vs install-time execution|`Exploitation PoC/index.js`|

---

## ⚖️ Disclaimer

This repository is provided **strictly for educational and defensive security research purposes**.

- No destructive payloads are included
    
- No real-world exploitation is performed
    
- Do **not** publish malicious packages to npm outside controlled research contexts
