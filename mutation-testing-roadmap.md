# Roadmap

Our mutation testing tool is constantly evolving. Here's what we're working on:

## Short-Term Goals 🎯

### 1. Extended Test Support
- Integration with Forge's fuzz testing framework
- Support for invariant test execution
- Configurable test type selection

### 2. Detector Configuration
- Ability to selectively ignore specific mutation operators
- Granular control over mutation patterns
- Custom mutation scope definition

## Mid-Term Goals 🌱

### 1. Custom Mutant Pipeline
Currently, new mutant suggestions come through our team via email, where we:
1. Analyze the suggested mutation pattern
2. Evaluate its real-world impact
3. Assess historical vulnerabilities it might have caught
4. Determine false positive potential
5. Make implementation decisions

We're working to streamline this process while maintaining our high standards for mutant quality.
We'll add an easier way to add mutants through a DSL. 

## Mid-to-Long-Term Goals 🚀

### Interpretation Engine Initiative

Mutation testing, while powerful, can be complex to interpret. We're building a comprehensive interpretation engine to make results more actionable and insightful.

#### 1. Smart Mutation Analysis
- Evaluate mutant criticality based on:
  - Location in codebase
  - Proximity to core infrastructure
  - Impact on critical functions
  - Integration points with external systems

#### 2. Context-Aware Analysis
- Leverage Intermediate Representation (IR) data to:
  - Map affected code paths
  - Track user-tainted data flow
  - Identify critical state modifications
  - Analyze cross-contract interactions

#### 3. Enhanced Visualization
- Interactive graph representations
- AST visualization
- Function call chain mapping
- Visual diff analysis
- Step-through debugging of failed mutations

#### 4. Failure Analysis Tools
- Detailed assertion failure tracking
- Root cause analysis
- Impact path visualization
- Test case correlation

#### 5. AI-Powered Insights
- LLM-based classification of mutations
- Natural language explanations of failures
- Pattern recognition across similar mutations
- Vulnerability prediction
- Automated severity assessment

#### 6. UX Improvements
- Interactive result exploration
- Customizable reporting dashboards
- Result filtering and categorization
- Historical trend analysis
- Team collaboration features

### Continuous Operator Evolution

We're constantly monitoring:
- New smart contract exploits
- Novel attack patterns
- Community security research
- Emerging blockchain vulnerabilities

This allows us to regularly introduce new mutation operators that reflect real-world threats.

## Note on Operator Updates

While not explicitly part of the roadmap, we continuously add new mutation operators as we discover interesting patterns from:
- Security incidents
- Community feedback
- Academic research
- Internal research
- Smart contract audits

These updates are pushed regularly and don't follow the roadmap timeline - they're integrated as soon as they're validated and tested.
