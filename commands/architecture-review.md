Review the architecture documentation and code architecture of this project.

## What to Check

### 1. Architecture Documentation Exists

- [ ] README.md has an "Architecture" section
- [ ] Architecture diagram exists (Mermaid, image, or other visual)
- [ ] Key components and their responsibilities are documented
- [ ] Data flow between components is described
- [ ] External dependencies and integrations are listed

### 2. Architecture Matches Code

Read the architecture documentation, then compare with the actual code:
- [ ] Documented components exist in code
- [ ] No undocumented major components in code
- [ ] Data flow matches actual implementation
- [ ] Documented integrations are present

### 3. Architecture Principles

Check if the codebase follows the standard architecture principles:

#### Frontend/Backend Separation
- [ ] Frontend and backend are separate projects/packages
- [ ] Communication only via defined APIs
- [ ] No backend logic in frontend code

#### Layered Architecture (Backend)
- [ ] Route Handlers → Services → Repositories pattern
- [ ] No business logic in route handlers
- [ ] No HTTP concepts (req, res) in services
- [ ] No framework code in domain layer

#### Dependency Direction
- [ ] Dependencies point downward only (UI → API → Domain → Infrastructure)
- [ ] No circular dependencies between layers

### 4. ADRs (Architecture Decision Records)

- [ ] `docs/adr/` directory exists
- [ ] Major technology choices have ADRs
- [ ] ADRs have proper status (Proposed/Accepted/Deprecated)
- [ ] Recent architecture changes have corresponding ADRs

Check for decisions that should have ADRs:
- Framework/library choices
- Database selection
- Authentication approach
- API design decisions
- Infrastructure architecture

### 5. Improvement Suggestions

Based on the review, suggest:
- Missing documentation that should be added
- Architecture improvements
- Components that should be split or consolidated
- ADRs that should be written

## Output Format

### Architecture Review

**Documentation Completeness: X/10**
**Code-Documentation Alignment: X/10**

### Missing Documentation
- ...

### Documentation-Code Mismatches
- ...

### Architecture Issues
- ...

### Suggested ADRs
- ...

### Suggested Improvements
- ...
