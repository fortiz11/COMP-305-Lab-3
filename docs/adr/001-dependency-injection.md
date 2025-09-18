# ADR-001: Use Constructor-Based Dependency Injection
## Status
Accepted
## Context
We need a way to manage dependencies between components while maintaining:
- Testability
- Loose coupling
- Clear dependency relationships
- No external framework dependencies
## Decision
We will use constructor-based dependency injection where all dependencies are passed
as constructor parameters.
## Consequences
### Positive
- Dependencies are explicit and visible
- Works naturally with JavaScript's approach to OOP
- Easy to mock for testing
### Negative
- Manual wiring of dependencies
- Constructor parameter lists can grow
- Dependencies still need to be well considered
## Example
` ``javascript
// Dice roller with injected renderer dependency
class DiceRoller {
constructor(renderer) {
this.renderer = renderer; // Injected display strategy
}
roll(notation) {
const result = /* rolling logic */;
return this.renderer.render(result); // Use injected renderer
}
}
// Usage with different renderers
const textRenderer = new TextRenderer();
const asciiRenderer = new AsciiRenderer();
const textRoller = new DiceRoller(textRenderer);
const visualRoller = new DiceRoller(asciiRenderer);
// Same logic, different output
textRoller.roll("2d6"); // Output: "You rolled 7 (3, 4)"
visualRoller.roll("2d6"); // Output: ASCII art of dice faces
` ``
## Alternatives Considered
1. DI Framework - Additional complexity and dependencies. Always avoid adopting
complexity until we understand basics
2. Factory Pattern - Could be used, but rather keep the concept of DI clear before
introducing more specific coding patterns
Create docs/adr/002-documentation-as-code.md :
# ADR-002: Documentation as Code with JSDoc and Mermaid
## Status
Accepted
## Context
Documentation often becomes outdated as code evolves. We need a way to:
- Keep documentation synchronized with code by making sure it is reviewed and evolved
as code changes
- Generate visual diagrams from code to provide a map of the architecture, flow, and
data structures
- Provide appropriate developer documentation for the code itself
## Decision
We will embed documentation directly in code using:
- JSDoc comments for API documentation
- Mermaid diagrams in JSDoc for visual representations
- Markdown files in the repository for high-level docs
- ADRs for architectural decisions
## Implementation
### JSDoc with Mermaid
` ``javascript
/**
* @fileoverview Die class for simulating dice rolls
* @mermaid
* classDiagram
* class Die {
* -sides: number
* -currentValue: number
* +constructor(sides: number)
* +roll(): number
* +getValue(): number
* }
*/
` ``
### File Structure
` ``
src/
├── domain/
│ ├── Die.js # Single die implementation
│ ├── DiceSet.js # Collection of dice
│ └── DiceNotation.js # Parse dice notation strings
├── application/
│ ├── DiceRoller.js # Main rolling logic
│ └── Statistics.js # Track roll history
└── presentation/
├── renderers/ # Different display strategies
└── cli/ # Command-line interface
` ``
## Consequences
### Positive
- Documentation stays close to code
- Changes are reviewed together
- Visual diagrams from code
- Single source of truth
- Can eventually be used to generate documentation website(s)
### Negative
- Requires discipline to maintain
- JSDoc can make files longer and may obscure code
- Learning curve for Mermaid syntax and concern that maybe it isn't the correct thing
to adopt
- Need tooling for generation
## Tools and Workflow
1. Write JSDoc comments with all public APIs
2. Include @mermaid tags for diagrams
3. Review documentation with code in PRs (future)
4. Run documentation generator (future)
5. Deploy documentation to GitHub Pages or other (future)
## Examples to Follow
- Every class should have a @class tag
- Every public method needs @param and @returns
- Complex logic should have @example
- State machines use @mermaid stateDiagram
- Class relationships use @mermaid classDiagram