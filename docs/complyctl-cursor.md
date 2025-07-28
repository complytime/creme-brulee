# ComplyCtl Codebase Overview

**ComplyCtl** is a comprehensive compliance automation tool that leverages the **OSCAL (Open Security Controls Assessment Language)** standard to perform compliance assessment activities. It uses a plugin-based architecture to support different compliance scanning tools and frameworks.

## 🏗️ Architecture Overview

The system follows a modular architecture with clear separation of concerns:

### **Main Components:**

1. **CLI Layer** (`cmd/complyctl/cli/`) - User-facing commands
2. **Core Logic** (`internal/complytime/`) - Business logic and OSCAL processing
3. **Plugin System** (`cmd/openscap-plugin/`) - Extensible compliance scanning
4. **Utilities** (`pkg/`, `internal/terminal/`, `internal/version/`) - Supporting functionality

### **Key Dependencies:**
- **OSCAL SDK** - For processing OSCAL documents
- **Compliance-to-Policy (C2P) Framework** - Plugin management and policy generation
- **Bubble Tea** - Rich terminal UI components
- **Cobra** - CLI framework

## 📁 Directory Structure

```
complyctl/
├── cmd/                          # Command-line applications
│   ├── complyctl/               # Main CLI application
│   │   ├── cli/                 # Command implementations
│   │   └── main.go             # Entry point
│   └── openscap-plugin/        # OpenSCAP compliance plugin
├── internal/                    # Private application code
│   ├── complytime/             # Core business logic
│   │   └── plan/               # Assessment plan management
│   ├── terminal/               # UI components
│   └── version/                # Version information
├── pkg/                        # Public library code
├── docs/                       # Documentation
├── scripts/                    # Automation scripts
└── vendor/                     # Vendored dependencies
```

## 🔧 Available Commands

### **Discovery & Information Commands:**

#### `complyctl list`
- **Purpose**: Display all available compliance frameworks
- **What it does**: 
  - Scans the `bundles/` directory for OSCAL Component Definitions
  - Extracts framework information from component implementations
  - Shows framework ID, title, and supported components in a table

#### `complyctl info <framework-id>`
- **Purpose**: Show detailed information about a specific framework
- **What it does**:
  - Displays controls, rules, and plugins for the framework
  - Can focus on specific controls (`--control`) or rules (`--rule`)
  - Shows implementation status and parameter information
  - Uses rich terminal UI for better visualization

### **Planning Commands:**

#### `complyctl plan <framework-id>`
- **Purpose**: Create an OSCAL Assessment Plan for compliance scanning
- **What it does**:
  - Processes Component Definitions to extract implemented requirements
  - Creates an Assessment Plan with all controls in scope by default
  - Saves to `workspace/assessment-plan.json`

#### `complyctl plan --dry-run <framework-id>`
- **Purpose**: Preview the assessment scope configuration
- **What it does**:
  - Generates a YAML configuration showing included controls and rules
  - Displays to stdout for review
  - **NEW**: Now includes control titles retrieved from catalogs

#### `complyctl plan --dry-run --out config.yml <framework-id>`
- **Purpose**: Generate a customizable scope configuration file
- **What it does**:
  - Creates `config.yml` with current scope settings
  - Allows users to modify which controls/rules to include
  - **NEW**: Includes human-readable control titles for easier customization

#### `complyctl plan --scope-config config.yml <framework-id>`
- **Purpose**: Create a customized Assessment Plan
- **What it does**:
  - Reads user-modified `config.yml` 
  - Applies scope customizations to the Assessment Plan
  - Filters controls and activities based on user preferences
  - **FIXED**: No longer crashes with nil pointer errors

### **Execution Commands:**

#### `complyctl generate`
- **Purpose**: Generate compliance policies from the Assessment Plan
- **What it does**:
  - Reads the Assessment Plan from workspace
  - Launches configured plugins via the Plugin Manager
  - Each plugin generates its specific policy artifacts (e.g., SCAP tailoring files)
  - Prepares the environment for compliance scanning

#### `complyctl scan`
- **Purpose**: Execute compliance scanning using generated policies
- **What it does**:
  - Loads the Assessment Plan and policies
  - Executes plugins to perform actual compliance scans
  - Aggregates results from all plugins
  - Generates `assessment-results.json` with findings

#### `complyctl scan --with-md`
- **Purpose**: Generate both JSON and Markdown assessment reports
- **What it does**:
  - Performs the same scan as above
  - Additionally generates `assessment-results.md` for human-readable reports
  - Includes control information from catalogs for better readability

## 🔄 OSCAL Data Flow

The system works with five main OSCAL document types:

### **1. Component Definition** (`bundles/`)
- **Purpose**: Defines what controls a software component implements
- **Contains**: 
  - Software components and their implemented requirements
  - Validation components (plugins) and their capabilities
  - Framework mappings and rule associations
  - Parameter definitions and default values

### **2. Catalog** (`controls/`)
- **Purpose**: Master reference of control definitions
- **Contains**:
  - Complete control definitions with titles and descriptions
  - Control hierarchies and relationships
  - Parameter specifications

### **3. Profile** (`controls/`)
- **Purpose**: Selects and customizes controls from catalogs for specific use cases
- **Contains**:
  - Control selections from one or more catalogs
  - Parameter value customizations
  - Framework-specific control groupings

### **4. Assessment Plan** (`workspace/`)
- **Purpose**: Configuration for compliance assessment execution
- **Contains**:
  - Which controls to assess (scope)
  - Assessment activities and steps
  - Component information for the target system
  - Plugin configurations

### **5. Assessment Results** (`workspace/`)
- **Purpose**: Output from compliance scans
- **Contains**:
  - Findings for each assessed control
  - Observations and evidence
  - Pass/fail status and remediation guidance

## 🔌 Plugin Architecture

ComplyCtl uses the **Compliance-to-Policy (C2P) Framework** for plugin management:

### **Plugin Discovery:**
- Plugins are discovered in the `plugins/` directory
- Each plugin has a binary executable and a JSON manifest
- Manifests define plugin capabilities, configuration options, and dependencies

### **Plugin Communication:**
- Plugins communicate with ComplyCtl via **gRPC**
- Standardized interface allows plugins in any language
- Plugin Manager handles lifecycle (launch, configure, execute, cleanup)

### **Plugin Workflow:**
1. **Generate Phase**: Plugins create policy artifacts (e.g., SCAP tailoring files)
2. **Scan Phase**: Plugins execute compliance scans and return observations
3. **Results**: Plugin findings are aggregated into OSCAL Assessment Results

### **Example: OpenSCAP Plugin**
- **Generate**: Creates SCAP tailoring files from Assessment Plans
- **Scan**: Runs `oscap` command-line tool to scan the system
- **Results**: Converts SCAP results to OSCAL observations

## 🚀 Complete Workflow Example

```bash
# 1. Discover available frameworks
complyctl list

# 2. Explore a specific framework
complyctl info anssi_bp28_minimal

# 3. Preview default scope
complyctl plan --dry-run anssi_bp28_minimal

# 4. Customize scope (optional)
complyctl plan --dry-run --out config.yml anssi_bp28_minimal
# Edit config.yml to customize controls/rules
complyctl plan --scope-config config.yml anssi_bp28_minimal

# 5. Or create default plan
complyctl plan anssi_bp28_minimal

# 6. Generate compliance policies
complyctl generate

# 7. Execute compliance scan
complyctl scan --with-md
```

## 🏗️ Key Design Patterns

### **1. Directory-Based Configuration**
- Application uses XDG Base Directory Specification
- Development mode vs. production mode (system vs. user directories)
- Clear separation of bundles, controls, plugins, and workspace

### **2. Interface-Based Plugin System**
- Clean abstractions for plugin communication
- Standardized policy generation and scanning interfaces
- Language-agnostic plugin development

### **3. OSCAL-Native Processing**
- All compliance data stored in OSCAL format
- Leverages OSCAL SDK for validation and processing
- Enables interoperability with other OSCAL tools

### **4. Modular Command Structure**
- Each command is self-contained with clear responsibilities
- Shared common options and configuration
- Consistent error handling and logging

## 🔧 Recent Enhancements

### **Control Title Integration**
- Assessment scope now includes human-readable control titles
- Titles are retrieved from OSCAL catalogs via profiles
- Fallback to control IDs when titles unavailable
- Improves user experience when customizing scope

### **Robust Error Handling**
- Fixed nil pointer dereferences in scope filtering
- Better validation of OSCAL document structure
- Graceful handling of missing or invalid data

This architecture provides a flexible, extensible platform for compliance automation that can adapt to various compliance frameworks and scanning tools while maintaining consistency through OSCAL standards.

> This content was generated using the Cursor AI IDE prompting for an overview of the `complyctl` codebase.
