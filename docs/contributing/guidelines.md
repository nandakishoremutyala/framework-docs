# Contributing Guidelines

Thank you for your interest in contributing to the framework documentation! This guide outlines the process and standards for contributing.

## Code of Conduct

We are committed to providing a welcoming and inclusive environment for all contributors. Please be respectful and professional in all interactions.

## How to Contribute

### Types of Contributions

We welcome various types of contributions:

- **Documentation improvements**: Fix typos, clarify explanations, add examples
- **New content**: Add new guides, tutorials, or API documentation
- **Bug reports**: Report issues with existing documentation
- **Feature requests**: Suggest new documentation features
- **Translations**: Help translate documentation to other languages

### Getting Started

1. **Read the documentation** to understand the current state
2. **Check existing issues** to avoid duplicate work
3. **Set up development environment** following the [Development Setup](development-setup.md) guide
4. **Start small** with minor improvements before tackling major changes

## Contribution Process

### 1. Planning Your Contribution

Before starting work:

- **Open an issue** to discuss major changes
- **Check with maintainers** for significant additions
- **Review existing content** to maintain consistency
- **Consider the audience** - who will benefit from your contribution?

### 2. Making Changes

#### Documentation Standards

**Writing Style:**
- Use clear, concise language
- Write in active voice when possible
- Use consistent terminology throughout
- Include practical examples
- Structure content logically

**Formatting:**
- Use proper markdown syntax
- Follow heading hierarchy (H1 → H2 → H3)
- Include code syntax highlighting
- Use consistent indentation
- Add blank lines between sections

**Code Examples:**
- Provide complete, runnable examples
- Include necessary imports
- Add comments for complex code
- Test all code examples
- Use realistic variable names

#### File Organization

- Place files in appropriate directories
- Use descriptive filenames
- Update navigation in `mkdocs.yml`
- Follow existing naming conventions
- Keep related content together

### 3. Quality Assurance

#### Before Submitting

**Content Review:**
- [ ] Spelling and grammar check
- [ ] Technical accuracy verification
- [ ] Code examples tested
- [ ] Links work correctly
- [ ] Images display properly

**Technical Review:**
- [ ] MkDocs builds without errors
- [ ] Navigation works correctly
- [ ] Responsive design maintained
- [ ] Search functionality works
- [ ] Performance not degraded

#### Testing Checklist

```bash
# Build and test locally
mkdocs serve
mkdocs build --clean

# Check for broken links
linkchecker site/

# Validate markdown
markdownlint docs/
```

### 4. Submitting Your Contribution

#### Pull Request Guidelines

**PR Title:**
- Use descriptive, concise titles
- Start with action verb (Add, Fix, Update, etc.)
- Reference issue numbers when applicable

**PR Description:**
- Explain what changes were made
- Describe why the changes are needed
- Include screenshots for visual changes
- List any breaking changes
- Add testing instructions

**Example PR Template:**
```markdown
## Description
Brief description of changes made.

## Changes Made
- Added new section on advanced configuration
- Updated code examples to use latest API
- Fixed broken links in navigation

## Testing
- [ ] Local build successful
- [ ] All links work
- [ ] Code examples tested
- [ ] Navigation updated

## Screenshots
(Include if applicable)

Closes #123
```

#### Review Process

1. **Automated checks** run on all PRs
2. **Maintainer review** for content and technical accuracy
3. **Community feedback** may be requested
4. **Revisions** may be needed based on feedback
5. **Merge** once approved

## Documentation Standards

### Content Guidelines

#### Structure
- Start with overview/introduction
- Provide step-by-step instructions
- Include practical examples
- End with next steps or related links

#### Language
- Use inclusive language
- Avoid jargon without explanation
- Write for international audience
- Be consistent with terminology

#### Examples
- Make examples realistic and practical
- Include error handling where appropriate
- Show both basic and advanced usage
- Provide context for when to use features

### Technical Standards

#### Markdown
- Use ATX-style headers (`#` not `===`)
- Include alt text for images
- Use reference-style links for readability
- Indent code blocks consistently

#### Code
- Follow language-specific style guides
- Include necessary imports and setup
- Use meaningful variable names
- Add comments for complex logic

#### Links
- Use relative links for internal pages
- Include descriptive link text
- Verify all links work
- Update links when moving content

## Review Criteria

### Content Quality
- **Accuracy**: Information is correct and up-to-date
- **Clarity**: Content is easy to understand
- **Completeness**: All necessary information is included
- **Relevance**: Content serves the target audience

### Technical Quality
- **Functionality**: All code examples work
- **Performance**: Changes don't slow down the site
- **Compatibility**: Works across different devices/browsers
- **Accessibility**: Content is accessible to all users

### Style Consistency
- **Voice and tone**: Matches existing documentation
- **Formatting**: Follows established patterns
- **Organization**: Logical structure and flow
- **Navigation**: Easy to find and follow

## Common Mistakes to Avoid

### Content Issues
- **Outdated information**: Always verify current accuracy
- **Missing context**: Provide background for complex topics
- **Inconsistent terminology**: Use the same terms throughout
- **Poor examples**: Ensure examples are realistic and complete

### Technical Issues
- **Broken builds**: Test locally before submitting
- **Missing dependencies**: Include all required imports
- **Broken links**: Verify all internal and external links
- **Poor formatting**: Follow markdown best practices

### Process Issues
- **Large PRs**: Break big changes into smaller PRs
- **Missing tests**: Verify all changes work correctly
- **Unclear descriptions**: Explain what and why clearly
- **Ignoring feedback**: Address review comments promptly

## Recognition

Contributors are recognized in several ways:

- **Contributors list** in the repository
- **Changelog mentions** for significant contributions
- **Community recognition** in discussions and social media
- **Maintainer consideration** for regular contributors

## Getting Help

If you need assistance:

1. **Check existing documentation** and issues
2. **Ask in GitHub Discussions** for general questions
3. **Open an issue** for specific problems
4. **Contact maintainers** for urgent matters

### Resources

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material Theme Guide](https://squidfunk.github.io/mkdocs-material/)
- [Markdown Guide](https://www.markdownguide.org/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

## Thank You

Your contributions help make this documentation better for everyone. We appreciate your time and effort in improving the framework documentation!

## Next Steps

- Set up your [Development Environment](development-setup.md)
- Review [Best Practices](../user-guide/best-practices.md)
- Check out [Examples](../examples/basic-examples.md) for inspiration
