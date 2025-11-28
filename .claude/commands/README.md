# Claude Code Slash Commands

This directory contains custom Claude Code slash commands for the project.

## Available Commands

### 📝 Documentation Commands

| Command | Purpose | Usage |
|---------|---------|--------|
| `/add-doc-for-pr` | Create comprehensive technical documentation for completed features | `/add-doc-for-pr "Feature Name" PR#` |
| `/doc-workflow-example` | Show the proven workflow used for AI Provider Integration System | `/doc-workflow-example` |
| `/generate-doc-template` | Generate structured documentation templates | `/generate-doc-template "Feature" doc-type` |

## Command Details

### `/add-doc-for-pr`
**Purpose**: Automate creation of professional-grade technical design documentation

**Key Features**:
- System architecture analysis and documentation
- Design pattern identification and justification  
- Business context and ROI analysis
- Integration with existing documentation structure
- Interview-quality system design explanations

**Output**: Complete technical design document + updated documentation index

---

### `/doc-workflow-example`  
**Purpose**: Demonstrate the exact process used to document the AI Provider Integration System

**Key Features**:
- Real implementation example with 10KB+ documentation
- Quality checklist and success metrics
- Step-by-step workflow demonstration
- Tips for creating system design documentation

**Output**: Complete workflow understanding and implementation guide

---

### `/generate-doc-template`
**Purpose**: Quickly scaffold documentation with proper structure

**Supported Types**:
- `system-design`: Technical architecture documentation
- `api-reference`: API endpoint documentation  
- `development-guide`: Development process documentation

**Output**: Structured markdown template ready for content

## Documentation Standards

These commands enforce:
- ✅ **Professional Quality**: Suitable for job interviews and technical reviews
- ✅ **System Design Focus**: Emphasizes architectural thinking and patterns
- ✅ **Business Context**: Includes strategic value and ROI considerations
- ✅ **Team Ready**: Creates documentation for onboarding and reference
- ✅ **Integration Compliant**: Proper integration with existing docs structure

## Example Workflow

```bash
# 1. Generate initial template
/generate-doc-template "Authentication System" system-design

# 2. Fill template with comprehensive content
# [Manual editing of generated template]

# 3. Create final documentation and integrate  
/add-doc-for-pr "Authentication System" 28
```

## File Locations

Generated documentation follows these conventions:
- **System Design**: `docs/system-design/feature-name-system-design.md`
- **API Reference**: `docs/api/feature-name-api-reference.md`  
- **Development Guides**: `docs/development/feature-name-guide.md`

## Quality Standards

All documentation created by these commands should meet:
- **Technical Depth**: Suitable for senior developers and architects
- **Business Value**: Clear connection to business objectives and ROI
- **Architectural Focus**: Emphasis on design patterns, scalability, reliability
- **Cross-Referenced**: Proper integration with existing documentation
- **Future-Proof**: Extensibility and maintenance considerations included

## Contributing

To add new slash commands:
1. Create `.md` file in this directory
2. Follow the established format and structure
3. Include comprehensive usage examples
4. Document the command's purpose and output
5. Test the command workflow thoroughly

## Related Documentation

- [Project Documentation Index](../../docs/index.md)
- [AI Provider Integration System Design](../../docs/system-design/ai-provider-integration-system-design.md)
- [Internationalization System Design](../../docs/system-design/internationalization-system-design.md)