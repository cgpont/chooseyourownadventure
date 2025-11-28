# Generate Documentation Template

Quickly generate a technical design documentation template with proper structure and sections.

## Usage

```
/generate-doc-template [feature-name] [doc-type]
```

**Examples:**
```
/generate-doc-template "User Authentication System" system-design
/generate-doc-template "Product Search API" api-reference  
/generate-doc-template "Deployment Process" development-guide
```

## Parameters

- `feature-name`: Name of the feature/system (will be used as document title)
- `doc-type`: Type of documentation (system-design, api-reference, development-guide)

## What This Command Does

Generates a structured markdown template with:

1. **Proper Header Structure** with feature name
2. **Standard Section Layout** based on doc type
3. **Placeholder Content** with guidance for each section
4. **Code Block Examples** ready for customization
5. **Cross-Reference Placeholders** for linking to related docs
6. **Proper Formatting** following project conventions

## Templates Available

### System Design Template
```markdown
# [Feature Name] - Technical Design Documentation

## 🎯 Overview & Business Context
[Explain why this system was needed, business requirements, strategic value]

## 🏗️ System Design Principles
[Document SOLID principles, design patterns, architectural patterns]

### 1. **[Design Pattern Name]**
```php
// Code example showing the pattern
interface ExampleInterface {
    public function method(): ReturnType;
}
```

**Benefits:**
- [List benefits of this design choice]

## 🔧 Architecture Decisions & Justifications
[Detailed justifications for technical choices]

### **Decision 1: [Decision Name]**
```php
// Implementation example
class ExampleClass implements ExampleInterface {
    // Implementation
}
```

**Justification:**
- [Explain why this approach was chosen]
- [Discuss alternatives considered]

## 🏛️ Framework Integration Architecture
[How this integrates with existing systems]

## 🧪 Testing Architecture
[Testing strategy, coverage, approach]

## 📊 Performance & Scalability Considerations
[Performance characteristics, scalability planning]

## 🔒 Security & Compliance
[Security measures, API key management, compliance]

## 🎯 Business Impact & ROI
[Business benefits, strategic value, development improvements]

## 🚀 Future Extensibility
[How to extend, enhancement hooks, migration strategies]

## 📁 Implementation Details
[File structure, configuration, key components]

## 🔗 Related Documentation
[Links to related docs]

## 🏷️ Tags
#system-design #feature-name #architecture
```

### API Reference Template
```markdown
# [Feature Name] API Reference

## Overview
[Brief description of the API functionality]

## Authentication
[How to authenticate with this API]

## Endpoints

### GET /api/endpoint
[Endpoint description]

**Parameters:**
- `param1` (string, required): Description
- `param2` (integer, optional): Description

**Example Request:**
```bash
curl -X GET "https://api.example.com/endpoint" \
  -H "Authorization: Bearer your-token"
```

**Example Response:**
```json
{
  "data": [],
  "meta": {}
}
```

## Error Handling
[How errors are returned and handled]

## Rate Limiting
[Rate limiting information]

## Related Documentation
[Links to related guides]
```

### Development Guide Template
```markdown
# [Feature Name] Development Guide

## Overview
[What this guide covers]

## Prerequisites
[Required tools, knowledge, setup]

## Setup Instructions
[Step-by-step setup process]

## Development Workflow
[How to work with this feature]

## Testing
[How to run tests, test strategies]

## Common Issues
[FAQ and troubleshooting]

## Related Documentation
[Links to related docs]
```

## Generated File Naming

The command will suggest appropriate file names:
- System Design: `feature-name-system-design.md`
- API Reference: `feature-name-api-reference.md`
- Development Guide: `feature-name-development-guide.md`

## File Location Suggestions

- **System Design**: `docs/system-design/`
- **API Reference**: `docs/api/`
- **Development Guide**: `docs/development/`

## Next Steps After Generation

1. **Fill in Template Sections**: Replace placeholders with actual content
2. **Add Code Examples**: Include relevant implementation examples
3. **Update Documentation Index**: Add references in `docs/index.md`
4. **Cross-Reference**: Link to related documentation
5. **Review for Quality**: Ensure professional standards are met

## Template Customization

You can customize templates by:
- Adding/removing sections based on feature complexity
- Adjusting code examples for specific technologies
- Including feature-specific considerations
- Adding business-specific context

## Integration with add-doc-for-pr

This template generator works well with the `/add-doc-for-pr` command:
1. Use `/generate-doc-template` to create initial structure
2. Fill in the template with comprehensive content
3. Use `/add-doc-for-pr` workflow to finalize and integrate

## Example Usage Flow

```bash
# 1. Generate template
/generate-doc-template "Caching System" system-design

# 2. Fill in template with:
#    - Business context and requirements
#    - Architectural decisions and justifications  
#    - Code examples and implementations
#    - Testing strategy and coverage
#    - Performance and scalability considerations

# 3. Integrate with documentation system
/add-doc-for-pr "Caching System" 25
```