# Development Setup Guide

## Prerequisites

Before setting up the development environment, ensure you have the following installed:

### Required Software

| Software | Version  | Purpose             |
| -------- | -------- | ------------------- |
| Node.js  | ≥ 18.0.0 | Runtime environment |
| npm      | ≥ 8.0.0  | Package manager     |
| Git      | ≥ 2.30.0 | Version control     |
| VSCode   | ≥ 1.74.0 | Development IDE     |

### VSCode Extensions

Install these extensions for optimal development experience:

```bash
Install essential VSCode extensions for TypeScript development, linting, formatting, and testing support.
```

## Project Setup

### 1. Clone the Repository

Clone the repository from GitHub and navigate to the project directory.

### 2. Install Dependencies

```bash
Install all project dependencies and development dependencies required for building and testing the extension.
```

### 3. Environment Configuration

Create environment configuration files:

```bash
Create a local environment configuration file
```

Configure the environment file with development settings, test credentials, and debug flags for local development.

### 4. Build Configuration

The project uses Webpack for bundling. Key configuration files:

#### `webpack.config.js`

The webpack configuration handles TypeScript compilation, module bundling, and source map generation for the extension.

#### `tsconfig.json`

The TypeScript configuration defines compilation options, module resolution, and path mappings for the project.

## Development Workflow

### 1. Build the Extension

```bash
Use npm scripts to build the extension in development or production mode, with watch mode for continuous development.
```

### 2. Run Tests

```bash
Run the test suite with various options including watch mode, coverage reporting, and specific test filtering.
```

### 3. Linting and Formatting

```bash
Use linting and formatting tools to maintain code quality and consistent style throughout the project.
```

### 4. Debug the Extension

#### Launch Configuration (`.vscode/launch.json`)

The launch configuration enables debugging the extension and running tests within the VSCode Extension Development Host.

#### Tasks Configuration (`.vscode/tasks.json`)

The tasks configuration defines build and test tasks that can be run from the VSCode command palette.

### 5. Package the Extension

```bash
Install VSCE globally, package the extension, and install the packaged extension for testing.
```

## Project Structure

```
lighthouse-vscode-extension/
├── .vscode/                    # VSCode configuration
│   ├── launch.json            # Debug configuration
│   ├── settings.json          # Workspace settings
│   └── tasks.json             # Build tasks
├── src/                       # Source code
│   ├── commands/              # Command handlers
│   ├── services/              # Business logic services
│   ├── ui/                    # User interface components
│   ├── utils/                 # Utility functions
│   ├── types/                 # TypeScript type definitions
│   └── extension.ts           # Main extension entry point
├── test/                      # Test files
│   ├── suite/                 # Test suites
│   ├── fixtures/              # Test fixtures
│   └── mocks/                 # Mock implementations
├── resources/                 # Static resources
│   ├── icons/                 # Extension icons
│   └── webviews/              # Webview HTML/CSS/JS
├── docs/                      # Documentation
├── .env.example               # Environment template
├── .eslintrc.json            # ESLint configuration
├── .gitignore                # Git ignore rules
├── .prettierrc               # Prettier configuration
├── jest.config.js            # Jest test configuration
├── package.json              # Package configuration
├── README.md                 # Project README
├── tsconfig.json             # TypeScript configuration
└── webpack.config.js         # Webpack build configuration
```

## Development Scripts

Add these scripts to your `package.json`:

The package.json scripts provide convenient commands for building, testing, linting, formatting, and publishing the extension.

## Testing Setup

### Jest Configuration (`jest.config.js`)

The Jest configuration sets up TypeScript testing with coverage reporting and module path mapping.

### Test Setup (`test/setup.ts`)

The test setup file provides mocks for the VSCode API and external dependencies to enable unit testing.

## Code Quality Tools

### ESLint Configuration (`.eslintrc.json`)

The ESLint configuration enforces TypeScript best practices and code quality rules throughout the project.

### Prettier Configuration (`.prettierrc`)

The Prettier configuration defines consistent code formatting rules for the entire project.

## Git Hooks Setup

### Pre-commit Hook

The pre-commit hook ensures code quality by running linting, formatting checks, and tests before each commit.

## Debugging Tips

### 1. Extension Host Debugging

- Use F5 to launch Extension Development Host
- Set breakpoints in TypeScript source files
- Use Debug Console for runtime inspection

### 2. Webview Debugging

- Right-click webview → "Open Developer Tools"
- Debug HTML/CSS/JavaScript in webview context

### 3. Network Debugging

Enable network debugging by adding conditional logging based on the development environment.

### 4. State Debugging

Add state debugging by logging state changes during development to track application behavior.

## Performance Profiling

### 1. Extension Activation Time

Measure extension activation time to monitor performance and identify potential bottlenecks.

### 2. Memory Usage Monitoring

Monitor memory usage periodically to detect memory leaks and optimize resource consumption.

## Troubleshooting

### Common Issues

1. **Extension not loading**

   - Check console for errors
   - Verify package.json activation events
   - Ensure all dependencies are installed

2. **TypeScript compilation errors**

   - Check tsconfig.json configuration
   - Verify import paths and module resolution
   - Update @types packages

3. **Test failures**

   - Check mock implementations
   - Verify test environment setup
   - Update test fixtures

4. **Build failures**
   - Clear node_modules and reinstall
   - Check webpack configuration
   - Verify file paths and imports
