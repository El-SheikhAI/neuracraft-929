# Contributing to NeuraCraft

Thank you for your interest in NeuraCraft! We’re excited to collaborate with you to build the future of lightweight AI automation pipelines. This guide will help you contribute effectively.

## 🌈 Our Community Pledge
We adhere to our [Code of Conduct](CODE_OF_CONDUCT.md). Please review it before participating. Our community is built on respect, empathy, and inclusivity.

## 🔍 Quick Links
- **Report Bugs**: [Issue Tracker](https://github.com/yourorg/neuracraft/issues)
- **Discuss Ideas**: [GitHub Discussions](https://github.com/yourorg/neuracraft/discussions)
- **Request Features**: [Feature Request Template](https://github.com/yourorg/neuracraft/issues/new?template=feature_request.md)

---

## 🛠 Your First Contribution
### Prerequisites
- Python 3.10+
- Git
- Pipenv (or `pip`/`venv`)

### Setup Guide
```bash
git clone https://github.com/yourorg/neuracraft.git
cd neuracraft
pip install -r requirements-dev.txt  # Install dev dependencies
python -m pytest tests/  # Run test suite
```

### Good First Issues
New to the project? Start here:
- [Issues labeled "good first issue"](https://github.com/yourorg/neuracraft/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
- Documentation improvements
- Test coverage expansion

---

## 🐛 Reporting Bugs
**Before filing an issue:**
1. Search existing issues
2. Test with the latest `main` branch

**Required in bug reports:**
```markdown
### Environment
- OS: [e.g., Ubuntu 22.04]
- Python Version: [e.g., 3.11.4]
- Commit Hash: [e.g., a1b2c3d]

### Steps to Reproduce
1. 
2. 
3. 

### Expected vs Actual Behavior
[What should happen] vs [What actually happens]

### Relevant Logs
(Attach error logs or stack traces)
```

---

## 🌟 Feature Requests
We love innovative ideas! For feature requests:
1. Explain the **problem** the feature solves
2. Describe your **proposed solution**
3. Include **use cases** or **mockups** (if applicable)

---

## 🚀 Pull Request Process
1. **Branch Naming**: Use `feature/description` or `fix/issue-number`
2. **Scope**: Address one logical change per PR
3. **Validation**:
   - All tests must pass
   - Code coverage must not decrease
   - Type annotations required for new Python code
   - Documentation updates required for interface changes

**Commit Message Guidelines:**
```plaintext
feat: Add TensorFlow plugin support
fix: Resolve pipeline memory leak (#123)
docs: Update plugin development guide
chore: Update CI dependencies
```

**PR Review Checklist:**
- [ ] Unit tests added/updated
- [ ] Documentation updated
- [ ] Type hints validated with `mypy`
- [ ] PEP8 compliance verified with `flake8`

---

## 🧩 Plugin Development
Extend NeuraCraft by creating plugins:
1. Use our plugin template:
   ```bash
   cp plugins/template.py plugins/my_plugin.py
   ```
2. Implement the required interface:
   ```python
   class MyPlugin(NeuraCraftPlugin):
       @property
       def metadata(self):
           return {"name": "My Plugin", "version": "1.0.0"}
    
       def execute(self, input_data: dict) -> dict:
           # Your logic here
   ```
3. Add integration tests in `tests/plugins/`

---

## 📜 Documentation Standards
- API documentation: Google-style docstrings
- User guides: Markdown in `/docs` directory
- Plugin tutorials: Include working examples

---

## 🤝 Need Help?
Join our community channels:
- **Discord**: [NeuraCraft Dev Server](https://discord.gg/your-invite-link)
- **Office Hours**: Every Wednesday 3-4 PM UTC (see Discussions)
- **Emergency Contact**: security@yourorg.com (for vulnerabilities)

---

Thank you for making NeuraCraft better! Your contributions power the future of AI automation. 🚀