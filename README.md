# Clojure Service Template Generator

> A Babashka-powered generator for creating production-ready Clojure microservices with security-first CI/CD pipelines.

## 🚀 Quick Start

### Prerequisites

- [Babashka](https://babashka.org/) (latest version)
- [Clojure CLI](https://clojure.org/guides/install_clojure) 
- [Git](https://git-scm.com/) for version control

### Generate Your First Service

```bash
# Generate a new service in current directory
./new-service my-awesome-service

# Generate in a specific directory
./new-service --parent-dir /path/to/projects my-awesome-service

# Using Babashka (if bb.edn is configured)
bb new-service my-awesome-service
bb new-service --parent-dir /tmp test-service
```

The generator creates a complete Clojure microservice with:

- ✅ Security-focused CI/CD pipelines
- ✅ Comprehensive testing setup (Kaocha + property-based testing)
- ✅ Multi-language linting (Clojure + JavaScript)
- ✅ Vulnerability scanning with Trivy
- ✅ License compliance enforcement
- ✅ Docker containerization
- ✅ Environment configuration management

## 📖 Usage

### Command Line Options

```bash
./new-service [OPTIONS] <service-name>

Options:
  --parent-dir <path>    Create the service in the specified directory
                        (default: current directory)

Examples:
  ./new-service payment-service
  ./new-service --parent-dir /home/projects user-auth-service
  ./new-service --parent-dir ../microservices inventory-service
```

### Generated Project Structure

Each generated service follows this standard structure:

```
my-service/
├── src/my_service/           # Main application code
│   └── core.clj             # Entry point
├── test/my_service/          # Test files
│   └── core_test.clj        # Unit tests
├── env/                     # Environment configurations
│   └── .env.example         # Environment template
├── .github/workflows/       # CI/CD automation
│   ├── ci.yml              # Main CI pipeline
│   └── trivy.yml           # Security scanning
├── deps.edn                # Clojure dependencies
├── bb.edn                  # Babashka tasks
├── package.json            # JavaScript dependencies
├── shadow-cljs.edn         # ClojureScript build config
└── README.md               # Generated project documentation
```

## 🛠️ Development

### Working on the Generator

```bash
# Clone the repository
git clone <repository-url>
cd clj_service_template

# Test the generator
./new-service test-service-123

# Test with custom directory
./new-service --parent-dir /tmp test-service-456

# Clean up test projects
rm -rf test-service-*
rm -rf /tmp/test-service-*

# Start REPL for development
clojure -M:nrepl
```

### Generator Architecture

The generator is built with Babashka and follows a simple template-based approach:

1. **Argument Parsing** (`parse-args`): Handles CLI options and validation
2. **Name Processing** (`kebab->ns`): Converts service names to namespace-safe format
3. **Template Processing** (`process-template`): Applies variable substitution
4. **File Generation** (`create-from-template`): Outputs processed templates

### Template System

Templates are stored in `templates/` directory:

```
templates/
├── README.md.template       # Project documentation
├── deps.edn.template        # Clojure dependencies 
├── bb.edn.template          # Babashka tasks configuration
├── package.json.template    # NPM dependencies
├── shadow-cljs.edn.template # ClojureScript build config
├── core.clj.template        # Main namespace template
├── core_test.clj.template   # Test template
└── workflows/               # GitHub Actions
    ├── ci.yml.template      # Main CI pipeline
    └── trivy.yml.template   # Security scanning workflow
```

#### Template Variables

- `{{SERVICE_NAME}}` - The service name (e.g., "payment-service")
- `{{NS_NAME}}` - Namespace-safe name (e.g., "payment_service")

#### Adding New Templates

1. Create your template file in `templates/` directory
2. Use `{{SERVICE_NAME}}` and `{{NS_NAME}}` placeholders
3. Add the template to the generation process in `new-service:26-84`

### Testing the Generator

```bash
# Basic functionality test
./new-service test-basic

# Test custom parent directory
mkdir -p /tmp/test-projects
./new-service --parent-dir /tmp/test-projects test-custom-dir

# Test error handling
./new-service  # Should show usage
./new-service --parent-dir  # Should show error
./new-service --invalid-option test  # Should show error

# Verify generated project works
cd test-basic
bb init
bb test
bb lint
bb check

# Clean up
cd ..
rm -rf test-basic /tmp/test-projects
```

### Contributing

#### Development Setup

1. **Fork** the repository
2. **Clone** your fork
3. **Create** a feature branch: `git checkout -b feature/template-enhancement`
4. **Test** your changes thoroughly
5. **Submit** a pull request

#### Testing Your Changes

```bash
# Test basic functionality
./new-service contrib-test
cd contrib-test
bb ci  # Should pass completely
cd ..

# Test edge cases
./new-service --parent-dir /tmp edge-case-test
./new-service service-with-dashes
./new-service service_with_underscores

# Test error conditions
./new-service ""  # Empty name
./new-service --parent-dir /nonexistent test  # Invalid path
```

#### Code Standards

- **Clojure Style**: Follow standard Clojure conventions
- **Template Quality**: Ensure generated projects pass all checks
- **Error Handling**: Provide clear, helpful error messages
- **Documentation**: Update README and templates as needed

### Generated Project Features

#### Security-First Approach

- **Trivy Security Scanning**: Comprehensive vulnerability detection
- **License Compliance**: Blocks GPL/AGPL/LGPL licenses automatically
- **Dependency Auditing**: Both Clojure (antq) and npm audit
- **CI/CD Security**: Automated security checks on every commit

#### Developer Experience

- **Babashka Tasks**: Unified task runner for all operations
- **Multi-language Support**: Clojure + ClojureScript + JavaScript
- **Hot Reloading**: Development server with live updates
- **Comprehensive Testing**: Unit, integration, and property-based testing

#### Generated Project Commands

Every generated service includes these Babashka tasks:

```bash
# Setup & Installation
bb init              # Install all dependencies
bb clean             # Clean up generated files

# Testing & Quality
bb test              # Run all tests
bb test:clj          # Run Clojure tests only
bb coverage          # Generate coverage report
bb lint              # Run all linters
bb check             # Complete quality pipeline

# Security & Auditing  
bb trivy-scan        # Generate security report
bb trivy-check       # Enforce security policies
bb audit:clj         # Check outdated dependencies
bb audit:js          # NPM security audit

# Build & Deploy
bb docker-build      # Build Docker image
bb ci                # Full CI pipeline
```

## 🔧 Advanced Usage

### Custom Template Modifications

```bash
# Modify existing templates
vim templates/deps.edn.template

# Add new dependencies or configurations
# Templates support full variable substitution

# Test your template changes
./new-service template-test
cd template-test
bb ci  # Verify everything works
```

### Integration with Build Systems

```bash
# Generate multiple services for a microservice architecture
for service in user-service payment-service inventory-service; do
  ./new-service --parent-dir ./microservices $service
done

# Generate in existing monorepo structure  
./new-service --parent-dir ./apps/backend auth-service
./new-service --parent-dir ./apps/workers notification-worker
```

### Template Versioning & Traceability

Every generated project includes template version tracking:

```bash
# Each template file includes a version comment at the top:
# Clojure/EDN: ;; Template version: 1.0.0
# JSON: " "//": "-- Template version: 1.0.0 --"  
# YAML: # Template version: 1.0.0
# Markdown: <!-- Template version: 1.0.0 -->
```

Benefits:

- **Template Evolution**: Track which template version was used for each file
- **Support & Debugging**: Identify template-specific issues  
- **Security Updates**: Know which services need template updates
- **Compliance**: Maintain audit records of template versions used

## 🐛 Troubleshooting

### Common Issues

**Generator fails to create files:**

```bash
# Check permissions
ls -la templates/
# Ensure templates directory is readable

# Check parent directory exists
mkdir -p /path/to/parent
./new-service --parent-dir /path/to/parent test-service
```

**Generated project fails CI:**

```bash
cd generated-service
bb init  # Ensure dependencies are installed
bb test  # Run tests individually
bb lint  # Check for linting issues
bb trivy-scan  # Check security scan results
cat trivy-report.json | jq .  # View detailed security report
```

**Template variable substitution issues:**

```bash
# Verify template syntax
grep -r "{{" templates/
# Ensure all variables use correct syntax: {{VARIABLE_NAME}}
```

### Getting Help

1. **Check Error Messages**: The generator provides detailed error messages
2. **Test Generated Projects**: Always run `bb ci` on generated services
3. **Review Templates**: Look at `templates/` for examples
4. **Consult Documentation**: 
   - [Babashka Book](https://book.babashka.org/)
   - [Clojure CLI Guide](https://clojure.org/guides/deps_and_cli)

## 🎯 Design Philosophy

### Template-Driven Architecture

This generator prioritizes:

- **Simplicity**: Pure template-based approach with string substitution
- **Maintainability**: Templates can be modified independently of generator code
- **Security**: Every generated project includes comprehensive security scanning
- **Developer Experience**: Rich tooling and clear documentation out of the box

### Why Babashka?

- **Fast Startup**: Near-instant execution compared to JVM Clojure
- **Rich Library Ecosystem**: Full access to Clojure libraries
- **Cross-Platform**: Works consistently across operating systems
- **Scriptable**: Easy to integrate into build pipelines

## 📚 Resources

### Learning Materials

- [Babashka Book](https://book.babashka.org/) - Complete Babashka guide
- [Clojure Guides](https://clojure.org/guides/getting_started) - Official Clojure documentation
- [Microservice Patterns](https://microservices.io/) - Architecture guidance

### Tools Used

- [Babashka](https://babashka.org/) - Fast Clojure scripting environment
- [Trivy](https://trivy.dev/) - Comprehensive security scanner
- [clj-kondo](https://github.com/clj-kondo/clj-kondo) - Clojure linter
- [Kaocha](https://github.com/lambdaisland/kaocha) - Test runner

## 📄 License

This project is licensed under [MIT License](LICENSE) - see the LICENSE file for details.

---

**🎉 Start Building!** Generate your first Clojure microservice and experience the power of security-first development with comprehensive tooling built-in.

```bash
./new-service my-first-service
cd my-first-service
bb ci
```
