Check that all documentation in this project is complete, accurate, and up to date.

## What to Check

### 1. README.md
Verify these required sections exist and are not empty or placeholder text:
- [ ] Project description (what it does, why it exists)
- [ ] Prerequisites
- [ ] Getting Started (clone, install, configure, run)
- [ ] Architecture (overview + diagram or description)
- [ ] Development (local dev workflow, commands)
- [ ] Testing (how to run tests)
- [ ] Deployment (how to deploy)

Cross-check against actual project:
- Does the Getting Started section match the actual setup process?
- Are the listed prerequisites accurate (Node version, tools)?
- Does the architecture section reflect the current code structure?

### 2. Architecture Documentation
- [ ] Architecture diagram or Mermaid diagram exists
- [ ] Key components and their responsibilities are described
- [ ] Data flow between components is documented
- [ ] External dependencies/integrations are listed

### 3. CHANGELOG.md
- [ ] Follows Keep a Changelog format
- [ ] Has an `[Unreleased]` section
- [ ] Recent changes are documented (compare with `git log`)

### 4. API Documentation (if applicable)
- [ ] OpenAPI/Swagger spec exists or is auto-generated
- [ ] All endpoints are documented
- [ ] Request/response examples are provided

### 5. ADRs (Architecture Decision Records)
- [ ] `docs/adr/` directory exists (if architecture decisions have been made)
- [ ] Recent technology/architecture decisions have corresponding ADRs

### 6. Code Documentation
- [ ] Exported functions have JSDoc/TSDoc comments
- [ ] Complex logic has explanatory comments
- [ ] No orphaned TODO comments without issue references

### 7. Environment Documentation
- [ ] `.env.example` exists and lists all required variables
- [ ] Variables have descriptive comments
- [ ] No real secrets in `.env.example`

## Output Format

### Documentation Health: X/10

### Missing (must add)
- ...

### Outdated (must update)
- ...

### Incomplete (should improve)
- ...

### Suggestions
- ...
