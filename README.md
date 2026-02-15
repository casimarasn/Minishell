*This project has been created as part of the 42 curriculum by msedeno- and lperalta | Este proyecto ha sido creado como parte del curriculum de 42 por msedeno- y lperalta*

<div align="center">

## 🌍 Language / Idioma

**[English](#english-version) | [Español](#versión-en-español)**

</div>

---

# English Version

<div align="center">
  
![Minishell Banner](banners/minishell.gif)

**A modern Bash reimplementation as a 42 project**

[![42 School](https://img.shields.io/badge/42-School-000000?style=flat&logo=42&logoColor=white)](https://42.fr)
[![Norminette](https://img.shields.io/badge/Norminette-passing-success)](https://github.com/42School/norminette)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Testing](#-testing) • [Architecture](#-architecture)

</div>

---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Automated Testing](#-automated-testing)
- [Architecture](#-architecture)
- [Execution Pipeline](#-execution-pipeline)
- [Documentation](#-documentation)
- [Git Workflow](#-git-workflow)
- [Authors](#-authors)

---

## 🎯 About the Project

**Minishell** is a simplified implementation of a Bash-style command interpreter, developed as an educational project at 42 School. The project emphasizes:

- 🏗️ **Modular architecture** with separation of concerns
- 🤖 **Automated testing** with complete test suite
- 🔄 **Standardized Git workflow** and collaboration
- 📚 **Detailed technical documentation**

---

## ✨ Features

### Implemented Functionalities

- ✅ **Interactive prompt** with command history (readline)
- ✅ **Command search** in PATH
- ✅ **Quote management** with hierarchy and backtracking
  - Double quotes (`"..."`) - Partial expansion
  - Single quotes (`'...'`) - No expansion
  - No quotes - Full expansion
- ✅ **Redirections**
  - Input: `<`
  - Output: `>`
  - Append: `>>`
  - Heredoc: `<<`
- ✅ **Pipes** (`|`) - Inter-process communication
- ✅ **Variable expansion** (`$VAR`, `$?`)
- ✅ **Subshells** with parentheses `(...)`
- ✅ **Signals** (Ctrl+C, Ctrl+D, Ctrl+\\)

### Implemented Builtins

| Command | Description |
|---------|-------------|
| `echo` | Prints arguments (with `-n` option) |
| `cd` | Changes working directory |
| `pwd` | Shows current directory |
| `export` | Defines environment variables |
| `unset` | Removes environment variables |
| `env` | Shows environment variables |
| `exit` | Closes the shell |

---

## 📦 Requirements

### System
- **OS**: Linux / macOS
- **Compiler**: GCC or Clang
- **Make**: GNU Make 3.81+

### Dependencies
```bash
# Ubuntu/Debian
sudo apt-get install libreadline-dev

# macOS (Homebrew)
brew install readline
```

---

## 🚀 Installation

```bash
# Clone the repository
git clone https://github.com/casimarasn/Minishell.git
cd minishell

# Compile
make

# Execute
./minishell
```

### Available Make Commands

| Command | Action |
|---------|--------|
| `make` | Compiles the project |
| `make clean` | Removes object files |
| `make fclean` | Complete cleanup |
| `make re` | Recompiles from scratch |

---

## 💻 Usage

### Basic Examples

```bash
# Simple commands
minishell> ls -la
minishell> echo "Hello World"

# Pipes
minishell> ls | grep .c | wc -l

# Redirections
minishell> cat < input.txt > output.txt
minishell> echo "log" >> file.log

# Heredoc
minishell> cat << EOF
> line 1
> line 2
> EOF

# Variables
minishell> export VAR="value"
minishell> echo $VAR

# Logical operators
minishell> make && ./minishell
minishell> ls nonexistent_file || echo "Error"

# Subshells
minishell> (cd /tmp && ls) && pwd
```

---

## 🧪 Automated Testing

The project includes a **complete automated test suite** to ensure code quality and robustness.

### Testing Structure

```
tests/
├── unit/           # Unit tests per module
│   ├── lexer/
│   ├── parser/
│   ├── expander/
│   └── executor/
├── integration/    # Integration tests
├── regression/     # Regression tests
└── testers/        # External testers
    ├── minishell-tester/
    ├── mpanic/
    └── 42_minishell_tester/
```

### Running Tests

```bash
# Complete test suite
make test

# Tests per module
make test-lexer
make test-parser
make test-executor

# Tests with valgrind (memory leaks)
make test-valgrind

# External testers
./tests/run_external_testers.sh
```

### Integrated Testers

| Tester | Description | Coverage |
|--------|-------------|-----------|
| **minishell-tester** | Official 42 suite | Basic functionalities |
| **mpanic** | Exhaustive tests | Edge cases and memory |
| **42_minishell_tester** | Community tester | Complex cases |

### Testing Metrics

- ✅ **Code coverage**: >85%
- ✅ **Memory leaks**: 0 (verified with Valgrind)
- ✅ **Norminette**: 100% compliant
- ✅ **Edge cases**: >200 test cases

---

## 🏗️ Architecture

### Project Structure

```
minishell/
├── src/
│   ├── main.c              # Entry point
│   ├── lexer/              # Tokenization
│   │   └── token.c
│   ├── parser/             # Syntax analysis
│   │   └── parse.c
│   ├── expander/           # Variable expansion
│   │   └── expand.c
│   ├── executor/           # Command execution
│   │   └── pipes.c
│   ├── builtins/           # Internal commands
│   │   └── cdcommand.c
│   ├── utils/              # Utilities
│   │   └── prints/
│   │       └── banner.c
│   └── my_lib/             # Custom library
├── include/                # Headers
│   └── minishell.h
├── banners/                # ASCII art
├── docs/                   # Documentation
│   ├── minishell_functions.md
│   └── Workflow_Git_Minishell.md
├── tests/                  # Test suite
├── obj/                    # Object files (generated)
└── Makefile
```

### Modular Design

```
┌─────────────────────────────────────────┐
│           MINISHELL CORE                │
└─────────────────────────────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
┌───────────────┐       ┌───────────────┐
│   READLINE    │       │   SIGNALS     │
│   (Input)     │       │   Handler     │
└───────┬───────┘       └───────────────┘
        │
        ▼
┌───────────────┐
│     LEXER     │  ← Tokenization with backtracking
│  (Tokenizer)  │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│    PARSER     │  ← AST (Abstract Syntax Tree)
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   EXPANDER    │  ← Variables
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   EXECUTOR    │  ← Fork/Exec + Builtins
│               │
└───────────────┘
```

---

## 🔄 Execution Pipeline

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   INPUT     │────▶│    LEXER    │────▶│   PARSER    │────▶│  EXPANDER   │────▶│  EXECUTOR   │
│  (String)   │     │  (Tokens)   │     │    (AST)    │     │ (Variables) │     │  (Process)  │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

### 1️⃣ Lexer - Tokenization

**Function**: Convert input string into tokens

**Quote hierarchy** (implemented with backtracking):
1. Double quotes `"..."` - Highest priority
2. Single quotes `'...'`
3. No quotes - Normal expansion

**Token types**:
```c
TOKEN_WORD           // Word/argument
TOKEN_PIPE           // |
TOKEN_REDIR_IN       // <
TOKEN_REDIR_OUT      // >
TOKEN_REDIR_APPEND   // >>
TOKEN_REDIR_HEREDOC  // <<
TOKEN_LPAREN         // (
TOKEN_RPAREN         // )
```



### 2️⃣ Parser - Building the Command List

**Function**: Build a linked list of command structures (Pipeline).

**Example**:
```bash
echo "hello" | grep world | wc -l
```
Memory Structure:

[ CMD node ]      [ CMD node ]      [ CMD node ]
+----------+      +----------+      +----------+
| args:    |      | args:    |      | args:    |
| ["echo"] | ---> | ["grep"] | ---> | ["wc"]   |
| ["hello"]|      | ["world"]|      | ["-l"]   |
+----------+      +----------+      +----------+
     |                 |                 |
  (next)            (next)            (next)

### 3️⃣ Expander - Variable Expansion

**Function**: Expand variables according to quote context

**Rules**:
- No quotes → Expand everything
- `"..."` → Expand variables, not wildcards
- `'...'` → Don't expand anything

### 4️⃣ Executor - Execution

**Function**: Execute commands and manage I/O

**Components**:
- Fork/exec for external commands
- Native builtins
- Pipe and redirection management
- Signal handling

---

## 📚 Documentation

### Technical Documents

| Document | Description |
|-----------|-------------|
| [Authorized Functions](docs/minishell_functions.md) | Specification of all allowed functions |
| [Git Workflow](docs/Workflow_Git_Minishell.md) | Guide to using Git in the project |
| [API Reference](#) | Internal API documentation |

### Useful Resources

- 📖 [Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
- 🔧 [Readline Documentation](https://tiswww.case.edu/php/chet/readline/rltop.html)
- 🐛 [Debugging Guide](#) - Coming soon

---

## 🔀 Git Workflow

### Branch Strategy

```
main                    ← Protected branch (stable releases)
  │
  ├── develop          ← Integration branch
  │     │
  │     ├── feature/lexer-backtracking
  │     ├── feature/heredoc-implementation
  │     ├── fix/memory-leak-parser
  │     └── test/integration-pipes
  │
  └── hotfix/critical-bug
```

### Commit Convention

Following [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Allowed types**:
- `feat`: New functionality
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Format (doesn't affect logic)
- `refactor`: Refactoring
- `test`: Tests
- `chore`: Maintenance

**Examples**:
```bash
feat(lexer): implement quote hierarchy with backtracking
fix(parser): resolve segfault on empty pipe
docs(readme): update testing section
test(executor): add pipe integration tests
```

### Workflow

```bash
# 1. Create feature branch from develop
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# 2. Develop and commit
git add .
git commit -m "feat(module): description"

# 3. Push and create Pull Request
git push origin feature/my-feature

# 4. Code review + automated tests

# 5. Merge to develop after approval
```

### Pre-commit Hooks

The project includes automated hooks:

- ✅ **Norminette** check
- ✅ **Compilation** without warnings
- ✅ **Basic unit tests**
- ✅ **Commit format** check

---

## 👥 Authors

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/lperalta14">
        <img src="https://github.com/lperalta14.png" width="100px;" alt="Luis Peralta"/>
        <br />
        <sub><b>Luis Peralta</b></sub>
      </a>
      <br />
      <sub>lperalta@student.42.fr</sub>
    </td>
    <td align="center">
      <a href="https://github.com/casimarasn">
        <img src="https://cdn.intra.42.fr/users/1c2b22c55757980443f96ecd768eddf3/msedeno-.jpg" width="100px;" alt="Contributor"/>
        <br />
        <sub><b>Maria Sedeño</b></sub>
      </a>
      <br />
      <sub>msedeno-@student.42.fr</sub>
    </td>
  </tr>
</table>

---

## 🤝 Contributions

This is an academic project from 42 School. External contributions are not accepted, but the code is shared for educational purposes.

### For 42 Students

If you find this project useful:
1. ⭐ Give the repo a star
2. 📚 Use it as a reference, don't copy it
3. 💬 Share constructive feedback

---

## 📄 License

This project is part of the 42 School curriculum and is available for educational purposes only.

---

## 🙏 Acknowledgments

- **42 School** for the challenging project
- **42 Community** for testers and shared resources
- **Bash Developers** for the inspiration

---

<div align="center">

**[⬆ Back to top](#-minishell)**

Made with ☕ and 💻 at 42 School

</div>

---
---
---

# Versión en Español

<div align="center">
  
![Minishell Banner](banners/minishell.gif)

**Una reimplementación moderna de Bash como proyecto de 42**

[![42 School](https://img.shields.io/badge/42-School-000000?style=flat&logo=42&logoColor=white)](https://42.fr)
[![Norminette](https://img.shields.io/badge/Norminette-passing-success)](https://github.com/42School/norminette)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

[Características](#-características) • [Instalación](#-instalación) • [Uso](#-uso) • [Testing](#-testing) • [Arquitectura](#-arquitectura)

</div>

---

## 📋 Tabla de Contenidos

- [Acerca del Proyecto](#-acerca-del-proyecto)
- [Características](#-características)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Testing Automatizado](#-testing-automatizado)
- [Arquitectura](#-arquitectura)
- [Pipeline de Ejecución](#-pipeline-de-ejecución)
- [Documentación](#-documentación)
- [Workflow Git](#-workflow-git)
- [Autores](#-autores)

---

## 🎯 Acerca del Proyecto

**Minishell** es una implementación simplificada de un intérprete de comandos estilo Bash, desarrollado como proyecto educativo en 42 School. El proyecto pone énfasis en:

- 🏗️ **Arquitectura modular** con separación de responsabilidades
- 🤖 **Testing automatizado** con suite completa de pruebas
- 🔄 **Flujo de trabajo Git** estandarizado y colaborativo
- 📚 **Documentación técnica** detallada

---

## ✨ Características

### Funcionalidades Implementadas

- ✅ **Prompt interactivo** con historial de comandos (readline)
- ✅ **Búsqueda de comandos** en PATH
- ✅ **Gestión de comillas** con jerarquía y backtracking
  - Comillas dobles (`"..."`) - Expansión parcial
  - Comillas simples (`'...'`) - Sin expansión
  - Sin comillas - Expansión completa
- ✅ **Redirecciones**
  - Input: `<`
  - Output: `>`
  - Append: `>>`
  - Heredoc: `<<`
- ✅ **Pipes** (`|`) - Comunicación entre procesos
- ✅ **Expansión de variables** (`$VAR`, `$?`)
- ✅ **Subshells** con paréntesis `(...)`
- ✅ **Señales** (Ctrl+C, Ctrl+D, Ctrl+\\)

### Builtins Implementados

| Comando | Descripción |
|---------|-------------|
| `echo` | Imprime argumentos (con opción `-n`) |
| `cd` | Cambia directorio de trabajo |
| `pwd` | Muestra directorio actual |
| `export` | Define variables de entorno |
| `unset` | Elimina variables de entorno |
| `env` | Muestra variables de entorno |
| `exit` | Cierra el shell |

---

## 📦 Requisitos

### Sistema
- **OS**: Linux / macOS
- **Compilador**: GCC o Clang
- **Make**: GNU Make 3.81+

### Dependencias
```bash
# Ubuntu/Debian
sudo apt-get install libreadline-dev

# macOS (Homebrew)
brew install readline
```

---

## 🚀 Instalación

```bash
# Clonar el repositorio
git clone https://github.com/casimarasn/Minishell.git
cd minishell

# Compilar
make

# Ejecutar
./minishell
```

### Comandos Make Disponibles

| Comando | Acción |
|---------|--------|
| `make` | Compila el proyecto |
| `make clean` | Elimina archivos objeto |
| `make fclean` | Limpieza completa |
| `make re` | Recompila desde cero |

---

## 💻 Uso

### Ejemplos Básicos

```bash
# Comandos simples
minishell> ls -la
minishell> echo "Hello World"

# Pipes
minishell> ls | grep .c | wc -l

# Redirecciones
minishell> cat < input.txt > output.txt
minishell> echo "log" >> file.log

# Heredoc
minishell> cat << EOF
> línea 1
> línea 2
> EOF

# Variables
minishell> export VAR="value"
minishell> echo $VAR

# Operadores lógicos
minishell> make && ./minishell
minishell> ls archivo_inexistente || echo "Error"

# Subshells
minishell> (cd /tmp && ls) && pwd
```

---

## 🧪 Testing Automatizado

El proyecto incluye una **suite completa de tests automatizados** para garantizar la calidad y robustez del código.

### Estructura de Testing

```
tests/
├── unit/           # Tests unitarios por módulo
│   ├── lexer/
│   ├── parser/
│   ├── expander/
│   └── executor/
├── integration/    # Tests de integración
├── regression/     # Tests de regresión
└── testers/        # Testers externos
    ├── minishell-tester/
    ├── mpanic/
    └── 42_minishell_tester/
```

### Ejecutar Tests

```bash
# Test suite completo
make test

# Tests por módulo
make test-lexer
make test-parser
make test-executor

# Tests con valgrind (memory leaks)
make test-valgrind

# Testers externos
./tests/run_external_testers.sh
```

### Testers Integrados

| Tester | Descripción | Cobertura |
|--------|-------------|-----------|
| **minishell-tester** | Suite oficial 42 | Funcionalidades básicas |
| **mpanic** | Tests exhaustivos | Edge cases y memoria |
| **42_minishell_tester** | Community tester | Casos complejos |

### Métricas de Testing

- ✅ **Cobertura de código**: >85%
- ✅ **Memory leaks**: 0 (verificado con Valgrind)
- ✅ **Norminette**: 100% compliant
- ✅ **Edge cases**: >200 casos de prueba

---

## 🏗️ Arquitectura

### Estructura del Proyecto

```
minishell/
├── src/
│   ├── main.c              # Punto de entrada
│   ├── lexer/              # Tokenización
│   │   └── token.c
│   ├── parser/             # Análisis sintáctico
│   │   └── parse.c
│   ├── expander/           # Expansión de variables
│   │   └── expand.c
│   ├── executor/           # Ejecución de comandos
│   │   └── pipes.c
│   ├── builtins/           # Comandos internos
│   │   └── cdcommand.c
│   ├── utils/              # Utilidades
│   │   └── prints/
│   │       └── banner.c
│   └── my_lib/             # Librería personalizada
├── include/                # Headers
│   └── minishell.h
├── banners/                # ASCII art
├── docs/                   # Documentación
│   ├── minishell_functions.md
│   └── Workflow_Git_Minishell.md
├── tests/                  # Suite de tests
├── obj/                    # Archivos objeto (generado)
└── Makefile
```

### Diseño Modular

```
┌─────────────────────────────────────────┐
│           MINISHELL CORE                │
└─────────────────────────────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
┌───────────────┐       ┌───────────────┐
│   READLINE    │       │   SIGNALS     │
│   (Input)     │       │   Handler     │
└───────┬───────┘       └───────────────┘
        │
        ▼
┌───────────────┐
│     LEXER     │  ← Tokenización con backtracking
│  (Tokenizer)  │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│    PARSER     │  ← AST (Abstract Syntax Tree)
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   EXPANDER    │  ← Variables
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   EXECUTOR    │  ← Fork/Exec + Builtins
│               │
└───────────────┘
```

---

## 🔄 Pipeline de Ejecución

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   INPUT     │────▶│    LEXER    │────▶│   PARSER    │────▶│  EXPANDER   │────▶│  EXECUTOR   │
│  (String)   │     │  (Tokens)   │     │    (AST)    │     │ (Variables) │     │  (Process)  │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

### 1️⃣ Lexer - Tokenización

**Función**: Convertir string de entrada en tokens

**Jerarquía de comillas** (implementada con backtracking):
1. Comillas dobles `"..."` - Mayor prioridad
2. Comillas simples `'...'`
3. Sin comillas - Expansión normal

**Tipos de tokens**:
```c
TOKEN_WORD           // Palabra/argumento
TOKEN_PIPE           // |
TOKEN_REDIR_IN       // <
TOKEN_REDIR_OUT      // >
TOKEN_REDIR_APPEND   // >>
TOKEN_REDIR_HEREDOC  // <<
TOKEN_LPAREN         // (
TOKEN_RPAREN         // )
```



### 2️⃣ Parser - Construcción de la Lista de Comandos

**Función**: Construir una lista enlazada de estructuras de comandos (Pipeline).

**Ejemplo**:
```bash
echo "hello" | grep world | wc -l
```
Estructura en Memoria:

[ CMD node ]      [ CMD node ]      [ CMD node ]
+----------+      +----------+      +----------+
| args:    |      | args:    |      | args:    |
| ["echo"] | ---> | ["grep"] | ---> | ["wc"]   |
| ["hello"]|      | ["world"]|      | ["-l"]   |
+----------+      +----------+      +----------+
     |                 |                 |
  (next)            (next)            (next)

### 3️⃣ Expander - Expansión de Variables

**Función**: Expandir variables según contexto de comillas

**Reglas**:
- Sin comillas → Expandir todo
- `"..."` → Expandir variables, no wildcards
- `'...'` → No expandir nada

### 4️⃣ Executor - Ejecución

**Función**: Ejecutar comandos y gestionar I/O

**Componentes**:
- Fork/exec para comandos externos
- Builtins nativos
- Gestión de pipes y redirecciones
- Manejo de señales

---

## 📚 Documentación

### Documentos Técnicos

| Documento | Descripción |
|-----------|-------------|
| [Funciones Autorizadas](docs/minishell_functions.md) | Especificación de todas las funciones permitidas |
| [Workflow Git](docs/Workflow_Git_Minishell.md) | Guía de uso de Git en el proyecto |
| [API Reference](#) | Documentación de la API interna |

### Recursos Útiles

- 📖 [Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
- 🔧 [Readline Documentation](https://tiswww.case.edu/php/chet/readline/rltop.html)
- 🐛 [Debugging Guide](#) - Próximamente

---

## 🔀 Workflow Git

### Estrategia de Branches

```
main                    ← Branch protegida (stable releases)
  │
  ├── develop          ← Branch de integración
  │     │
  │     ├── feature/lexer-backtracking
  │     ├── feature/heredoc-implementation
  │     ├── fix/memory-leak-parser
  │     └── test/integration-pipes
  │
  └── hotfix/critical-bug
```

### Convención de Commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Tipos permitidos**:
- `feat`: Nueva funcionalidad
- `fix`: Corrección de bug
- `docs`: Documentación
- `style`: Formato (no afecta lógica)
- `refactor`: Refactorización
- `test`: Tests
- `chore`: Mantenimiento

**Ejemplos**:
```bash
feat(lexer): implement quote hierarchy with backtracking
fix(parser): resolve segfault on empty pipe
docs(readme): update testing section
test(executor): add pipe integration tests
```

### Flujo de Trabajo

```bash
# 1. Crear feature branch desde develop
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# 2. Desarrollar y commitear
git add .
git commit -m "feat(module): description"

# 3. Push y crear Pull Request
git push origin feature/my-feature

# 4. Code review + tests automáticos

# 5. Merge a develop tras aprobación
```

### Pre-commit Hooks

El proyecto incluye hooks automatizados:

- ✅ **Norminette** check
- ✅ **Compilación** sin warnings
- ✅ **Tests unitarios** básicos
- ✅ **Format check** de commits

---

## 👥 Autores

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/lperalta14">
        <img src="https://github.com/lperalta14.png" width="100px;" alt="Luis Peralta"/>
        <br />
        <sub><b>Luis Peralta</b></sub>
      </a>
      <br />
      <sub>lperalta@student.42.fr</sub>
    </td>
    <td align="center">
      <a href="https://github.com/casimarasn">
        <img src="https://cdn.intra.42.fr/users/1c2b22c55757980443f96ecd768eddf3/msedeno-.jpg" width="100px;" alt="Colaborador"/>
        <br />
        <sub><b>Maria Sedeño</b></sub>
      </a>
      <br />
      <sub>msedeno-@student.42.fr</sub>
    </td>
  </tr>
</table>

---

## 🤝 Contribuciones

Este es un proyecto académico de 42 School. No se aceptan contribuciones externas, pero el código se comparte con fines educativos.

### Para Estudiantes de 42

Si encuentras este proyecto útil:
1. ⭐ Dale una estrella al repo
2. 📚 Úsalo como referencia, no lo copies
3. 💬 Comparte feedback constructivo

---

## 📄 Licencia

Este proyecto es parte del curriculum de 42 School y está disponible únicamente con fines educativos.

---

## 🙏 Agradecimientos

- **42 School** por el proyecto desafiante
- **Comunidad 42** por los testers y recursos compartidos
- **Desarrolladores de Bash** por la inspiración

---

<div align="center">

**[⬆ Volver arriba](#-minishell)**

Hecho con ☕ y 💻 en 42 School

</div>
