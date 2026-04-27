# 🚀 AI-First Framework-Based API

A skeleton project for building AI-first REST APIs using a custom PHP framework.

---

## Getting started

### 1. Clone the Repository

```bash
git clone https://github.com/atrehhoff/framework-ai-skeleton --recurse-submodules
```

### 2. Configure Webserver

Set your document root to:
```
framework/src/
```

### 3. Implementation Setup

Use your preferred AI agent to implement the development plan:

> [!IMPORTANT]
> **Recommended:** Use your agent's planning mode for this step.

**Copilot:**
```bash
DB_HOST="REPLACE_THIS_VALUE" \
DB_NAME="REPLACE_THIS_VALUE" \
DB_USER="REPLACE_THIS_VALUE" \
DB_PASS="REPLACE_THIS_VALUE" \
copilot --prompt "Read @PROMPT.md"
```

**Claude:**
```bash
DB_HOST="REPLACE_THIS_VALUE" \
DB_NAME="REPLACE_THIS_VALUE" \
DB_USER="REPLACE_THIS_VALUE" \
DB_PASS="REPLACE_THIS_VALUE" \
claude "Read @PROMPT.md"
```

---

## Project Structure
During the implementation phase, the following paths will be added/updated in the `framework/` directory:

```
framework/
├── src/
│   ├── Api/              # API endpoints controller directory
│   │   ├── Support/      # Support/Helper classes for API endpoints
│   │   ├──── Key.php       # API key entity class
│   │   └──── Validator.php # Request validation class
│   └── ...
├── tests/
│   └── api/              # API test suite
├── api-client/           # Generated SDK from OpenAPI spec (tool-generated)
├── openapi-generator/    # OpenAPI code generation tool (cloned)
├── AGENTS.md             # Updated with API endpoint agent instructions
└── composer.json         # Updated with api:doc, api:sdk, test:api commands
```

---

## Next Steps

🔗 See [PROMPT.md](./PROMPT.md) for comprehensive development instructions and API endpoint specifications.  