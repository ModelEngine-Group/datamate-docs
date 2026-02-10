---
title: Contribution Guide
description: Welcome to the DataMate project. We welcome all forms of contributions including documentation, code, testing, translation, etc.
weight: 10
---

{{% pageinfo %}}

DataMate is an enterprise-level open source data processing project dedicated to providing efficient data solutions for model training, AI applications, and data flywheel scenarios. We welcome all developers, document creators, and test engineers to participate through code commits, documentation optimization, issue feedback, and community support.

If this is your first time contributing to an open source project, we recommend reading [Open Source Contribution Newbie Guide](https://opensource.guide/how-to-contribute/) first, then proceed with this guide. All contributions must follow the [DataMate Code of Conduct]().
{{% /pageinfo %}}

## Contribution Scope and Methods

DataMate open source project contributions cover the following core scenarios. You can choose your participation based on your expertise:

| Contribution Type | Specific Content | Suitable For |
|-----------------|-----------------|---------------|
| **Code Contribution** | Core feature development, bug fixes, performance optimization, new feature proposals | Backend/frontend developers, data engineers |
| **Documentation Contribution** | User manual updates, API documentation improvements, tutorial writing, contribution guide optimization | Technical document creators, experienced users |
| **Testing Contribution** | Write unit/integration tests, feedback test issues, participate in compatibility testing | Test engineers, QA personnel |
| **Community Contribution** | Answer GitHub Issues, participate in community discussions, share use cases | All users, tech enthusiasts |
| **Design Contribution** | UI/UX optimization, logo/icon design, documentation visual upgrade | UI/UX designers, visual designers |

Thank you for choosing to participate in the DataMate open source project! Whether it's code, documentation, or community support, every contribution helps the project grow and advances enterprise-level data processing technology. If you encounter any issues during the contribution process, feel free to seek help through community channels.

## Getting Started

### Development Environment

Before contributing, please set up your development environment:

1. **Clone Repository**

```bash
git clone https://github.com/ModelEngine-Group/DataMate.git
cd DataMate
```

2. **Install Dependencies**

```bash
# Backend dependencies
cd backend
mvn clean install

# Frontend dependencies
cd frontend
pnpm install

# Python dependencies
cd runtime
pip install -r requirements.txt
```

3. **Start Services**

```bash
# Start basic services
make install dev=true
```

For detailed setup instructions, see:
- [Development Environment Setup](/docs/getting-started/development/) - Local development configuration
- [Backend Architecture](/docs/developer-guide/backend-architecture/) - Backend architecture
- [Frontend Architecture](/docs/developer-guide/frontend-architecture/) - Frontend architecture

## Code Contribution

### Code Standards

#### Java Code Standards

- **Naming Conventions**:
  - Class name: PascalCase `UserService`
  - Method name: camelCase `getUserById`
  - Constants: UPPER_CASE `MAX_SIZE`
  - Variables: camelCase `userName`

- **Documentation**: Add Javadoc comments for public APIs

```java
/**
 * User service
 *
 * @author Your Name
 * @since 1.0.0
 */
public class UserService {
    /**
     * Get user by ID
     *
     * @param userId user ID
     * @return user information
     */
    public User getUserById(Long userId) {
        // ...
    }
}
```

#### TypeScript Code Standards

- **Naming Conventions**:
  - Components: PascalCase `UserProfile`
  - Types/Interfaces: PascalCase `UserData`
  - Functions: camelCase `getUserData`
  - Constants: UPPER_CASE `API_BASE_URL`

#### Python Code Standards

Follow PEP 8:

```python
def get_user(user_id: int) -> dict:
    """
    Get user information

    Args:
        user_id: User ID

    Returns:
        User information dictionary
    """
    # ...
```

### Submitting Code

#### 1. Create Branch

```bash
git checkout -b feature/your-feature-name
```

Branch naming convention:
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `refactor/` - Refactoring

#### 2. Make Changes

Follow the code standards mentioned above.

#### 3. Write Tests

```bash
# Backend tests
mvn test

# Frontend tests
pnpm test

# Python tests
pytest
```

#### 4. Commit Changes

```bash
git add .
git commit -m "feat: add new feature description"
```

Commit message format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes
- `refactor:` - Refactoring
- `test:` - Adding tests
- `chore:` - Other changes

#### 5. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then create a Pull Request on GitHub.

## Documentation Contribution

### Documentation Structure

Documentation is located in the `/docs` directory:

```
docs/
├── getting-started/     # Quick start
├── user-guide/          # User guide
├── api-reference/       # API reference
├── developer-guide/     # Developer guide
└── appendix/            # Appendix
```

### Writing Documentation

#### 1. Choose Language

Documents support bilingual (Chinese and English). When updating documentation, please update both language versions.

#### 2. Follow Format

Use Markdown format with Hugo front matter:

```markdown
---
title: Page Title
description: Page description
weight: 1
---

Content here...
```

#### 3. Add Examples

Include code examples, commands, and use cases to help users understand.

#### 4. Cross-Reference

Add links to related documentation:

```markdown
See [Data Management](/docs/user-guide/data-management/) for details.
```

## Testing Contribution

### Test Coverage

We aim for comprehensive test coverage:

- **Unit Tests**: Test individual functions and classes
- **Integration Tests**: Test service interactions
- **E2E Tests**: Test complete workflows

### Writing Tests

#### Backend Tests (JUnit)

```java
@Test
public void testGetDataset() {
    // Arrange
    String datasetId = "test-dataset";

    // Act
    Dataset result = datasetService.getDataset(datasetId);

    // Assert
    assertNotNull(result);
    assertEquals("test-dataset", result.getId());
}
```

#### Frontend Tests (Jest + React Testing Library)

```typescript
test('renders data management page', () => {
  render(<DataManagement />);
  expect(screen.getByText('Data Management')).toBeInTheDocument();
});
```

### Reporting Issues

When finding bugs:

1. Search existing [GitHub Issues](https://github.com/ModelEngine-Group/DataMate/issues)
2. If not found, create new issue with:
   - Clear title
   - Detailed description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment info

## Design Contribution

### UI/UX Guidelines

We use **Ant Design** as the UI component library. When contributing design changes:

1. Follow Ant Design principles
2. Ensure consistency with existing design
3. Consider accessibility
4. Test on different screen sizes

### Design Assets

Design assets should be placed in:
- Frontend assets: `frontend/src/assets/`
- Documentation images: `content/en/docs/images/`

## Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers and help them learn
- Focus on constructive feedback
- Collaborate openly

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: General discussions
- **Pull Requests**: Code and documentation contributions

## Getting Help

If you need help:

1. Check existing [documentation](/docs/)
2. Search [GitHub Issues](https://github.com/ModelEngine-Group/DataMate/issues)
3. Start a [GitHub Discussion](https://github.com/ModelEngine-Group/DataMate/discussions)

## Recognition

Contributors will be recognized in:
- **Contributors List**: In the documentation
- **Release Notes**: For significant contributions
- **Community Highlights**: For outstanding contributions

## License

By contributing to DataMate, you agree that your contributions will be licensed under the [MIT License](https://github.com/ModelEngine-Group/DataMate/blob/main/LICENSE).

---

Thank you for contributing to DataMate! Your contributions help make DataMate better for everyone. 🎉
