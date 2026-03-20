# Linting & Code Quality

## Overview

This project uses ESLint and Prettier to maintain consistent code quality and formatting across all team members.

## Frontend (ESLint + Prettier)

### Installation

```bash
npm install -D eslint prettier eslint-config-prettier eslint-plugin-prettier
npm install -D @typescript-eslint/eslint-plugin @typescript-eslint/parser
npm install -D eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y
```

### ESLint Configuration (.eslintrc.js)

```javascript
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true,
    },
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
  env: {
    browser: true,
    node: true,
    es2022: true,
    jest: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:jsx-a11y/recommended',
    'prettier',
  ],
  plugins: ['@typescript-eslint', 'react', 'react-hooks', 'jsx-a11y', 'prettier'],
  rules: {
    'prettier/prettier': 'error',
    
    // React
    'react/react-in-jsx-scope': 'off',
    'react/prop-types': 'off',
    
    // TypeScript
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
    '@typescript-eslint/no-non-null-assertion': 'off',
    
    // Hooks
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    
    // Best practices
    'no-console': ['warn', { allow: ['warn', 'error'] }],
    'no-debugger': 'error',
    'no-eval': 'error',
    'no-implicit-coercion': 'error',
    
    // Accessibility
    'jsx-a11y/alt-text': 'error',
    'jsx-a11y/anchor-has-content': 'error',
    'jsx-a11y/click-events-have-key-events': 'warn',
    'jsx-a11y/no-static-element-interactions': 'warn',
  },
};
```

### TypeScript ESLint Config (tsconfig.eslint.json)

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "project": "./tsconfig.json"
  },
  "include": [
    "src/**/*",
    "*.config.js",
    "*.config.ts"
  ]
}
```

### Prettier Configuration (.prettierrc)

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "jsxSingleQuote": false,
  "jsxBracketSameLine": false
}
```

### Prettier Ignore (.prettierignore)

```
# Dependencies
node_modules/

# Build output
dist/
build/

# IDE
.vscode/
.idea/

# Git
.git/

# Cache
.cache/
*.lock

# Test coverage
coverage/

# Misc
*.md
```

### Package.json Scripts

```json
{
  "scripts": {
    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,css,scss}\"",
    "format:check": "prettier --check \"src/**/*.{ts,tsx,css,scss}\"",
    "lint:all": "npm run lint && npm run format:check"
  }
}
```

### VSCode Settings (.vscode/settings.json)

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "eslint.validate": [
    "javascript",
    "typescript"
  ],
  "files.eol": "\n"
}
```

### Recommended VSCode Extensions

```json
// extensions.json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "eamodio.gitlens",
    "formulahendry.auto-rename-tag",
    "christian-kohler.path-intellisense"
  ]
}
```

---

## Backend (Java/Spring Boot)

### Maven Configuration (pom.xml)

```xml
<properties>
    <java.version>17</java.version>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <maven.compiler.target>${java.version}</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    
    <!-- Code Quality -->
    <checkstyle.version>10.12.0</checkstyle.version>
    <spotless.version>6.18.0</spotless.version>
    <pitest.version>1.15.0</pitest.version>
</properties>

<build>
    <plugins>
        <!-- Spotless for formatting -->
        <plugin>
            <groupId>com.diffplug.spotless</groupId>
            <artifactId>spotless-maven-plugin</artifactId>
            <version>${spotless.version}</version>
            <configuration>
                <java>
                    <googleJavaFormat>
                        <version>1.17.0</version>
                        <reflowLongStrings>true</reflowLongStrings>
                    </googleJavaFormat>
                    <removeUnusedImports>true</removeUnusedImports>
                    <trimTrailingWhitespace>true</trimTrailingWhitespace>
                    <endWithNewline>true</endWithNewline>
                </java>
            </configuration>
        </plugin>
        
        <!-- Checkstyle -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>${checkstyle.version}</version>
            <configuration>
                <configLocation>checkstyle.xml</configLocation>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### Checkstyle Configuration (checkstyle.xml)

```xml
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
    "-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
    "https://checkstyle.org/dtds/configuration_1_3.dtd">

<module name="Checker">
    <property name="charset" value="UTF-8"/>
    <property name="severity" value="warning"/>
    <property name="fileExtensions" value="java"/>

    <!-- Naming conventions -->
    <module name="ConstantName"/>
    <module name="LocalFinalVariableName"/>
    <module name="LocalVariableName"/>
    <module name="MemberName"/>
    <module name="MethodName"/>
    <module name="PackageName"/>
    <module name="ParameterName"/>
    <module name="StaticVariableName"/>
    <module name="TypeName"/>

    <!-- Import checks -->
    <module name="AvoidStarImport"/>
    <module name="IllegalImport"/>
    <module name="RedundantImport"/>
    <module name="UnusedImports">
        <property name="processJavadoc" value="false"/>
    </module>

    <!-- Size -->
    <module name="MethodLength">
        <property name="max" value="150"/>
    </module>
    <module name="ParameterNumber">
        <property name="max" value="7"/>
    </module>

    <!-- Whitespace -->
    <module name="EmptyForIteratorPad"/>
    <module name="GenericWhitespace"/>
    <module name="MethodParamPad"/>
    <module name="NoWhitespaceAfter"/>
    <module name="NoWhitespaceBefore"/>
    <module name="OperatorWrap"/>
    <module name="ParenPad"/>
    <module name="TypecastParenPad"/>
    <module name="WhitespaceAfter"/>
    <module name="WhitespaceAround"/>

    <!-- Modifier checks -->
    <module name="ModifierOrder"/>
    <module name="RedundantModifier"/>

    <!-- Blocks -->
    <module name="AvoidNestedBlocks"/>
    <module name="EmptyBlock"/>
    <module name="LeftCurly"/>
    <module name="NeedBraces"/>
    <module name="RightCurly"/>

    <!-- Coding -->
    <module name="EmptyStatement"/>
    <module name="EqualsHashCode"/>
    <module name="IllegalInstantiation"/>
    <module name="InnerAssignment"/>
    <module name="MissingSwitchDefault"/>
    <module name="SimplifyBooleanExpression"/>
    <module name="SimplifyBooleanReturn"/>

    <!-- Design -->
    <module name="FinalClass"/>
    <module name="HideUtilityClassConstructor"/>
    <module name="InterfaceIsType"/>
    <module name="VisibilityModifier"/>
</module>
```

### Spring Boot Application Properties

```properties
# Code formatting on build
spring-boot-maven-plugin.format.main=true
spring-boot-maven-plugin.format.test=true
```

### Maven Commands

```bash
# Check code formatting
mvn spotless:check

# Apply formatting
mvn spotless:apply

# Run checkstyle
mvn checkstyle:check

# Full check
mvn checkstyle:check spotless:check
```

---

## Git Hooks (Husky + lint-staged)

### Installation

```bash
npm install -D husky lint-staged
npx husky install
```

### Package.json Configuration

```json
{
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### Husky Pre-commit Hook

```bash
# .husky/pre-commit
#!/usr/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

---

## CI/CD Integration

### GitHub Actions Workflow

```yaml
# .github/workflows/lint.yml
name: Lint & Format Check

on:
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run ESLint
        run: npm run lint
      
      - name: Check Prettier formatting
        run: npm run format:check
```

---

## Editor Configuration

### Global Git Ignore (.gitignore)

```
# Dependencies
node_modules/
vendor/

# Build outputs
dist/
build/
target/
out/

# IDE
.vscode/
.idea/
*.iml

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Test coverage
coverage/

# Environment
.env
.env.local

# Cache
.cache/
*.cache
```

---

## Code Quality Checklist

Before committing or opening a PR:

- [ ] `npm run lint` passes with no errors
- [ ] `npm run format` applied (no formatting changes pending)
- [ ] No `console.log` statements (except allowed)
- [ ] No `TODO` comments without issue reference
- [ ] No commented-out code
- [ ] All imports are used
- [ ] No `any` types in TypeScript
- [ ] No hardcoded values (use constants)
