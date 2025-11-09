# OI OS Integration Guide for OI-code-index-mcp

This guide provides complete instructions for AI agents to install, configure, and use the OI-code-index-mcp server in OI OS (Brain Trust 4).

## ⚠️ **IMPORTANT: Python Version Requirement**

**This server requires Python 3.10 or higher.**

- **Current system Python:** 3.9.6 (incompatible)
- **Required:** Python 3.10+

**Solutions:**
1. **Use `uv` to manage Python versions** (recommended):
   ```bash
   uv python install 3.10
   uv run --python 3.10 code-index-mcp
   ```

2. **Install Python 3.10+ system-wide:**
   - macOS: `brew install python@3.10`
   - Linux: Use your distribution's package manager
   - Windows: Download from python.org

3. **Use a Python version manager:**
   - `pyenv` (macOS/Linux)
   - `asdf` (cross-platform)

## 🚀 Installation

### Prerequisites

| Requirement | Version        |
| ----------- | -------------- |
| **Python**  | 3.10+ (required) |
| **uv**      | Latest (recommended) |
| **Git**     | Any            |

### Installation Steps

1. **Clone the repository:**
   ```bash
   git clone https://github.com/OI-OS/OI-code-index-mcp.git
   ```

2. **Navigate to the server directory:**
   ```bash
   cd MCP-servers/OI-code-index-mcp
   ```

3. **Install dependencies using `uv` (recommended):**
   ```bash
   uv sync
   ```

   **Or using `pip`:**
   ```bash
   pip install -e .
   ```

4. **Connect the server to OI OS:**
   ```bash
   cd ../../ # Go back to the OI OS root directory
   ./brain-trust4 connect OI-code-index-mcp uv -- run --directory "$(pwd)/MCP-servers/OI-code-index-mcp" code-index-mcp
   ```

   **Or using Python directly (if Python 3.10+ is available):**
   ```bash
   ./brain-trust4 connect OI-code-index-mcp python3 -- "$(pwd)/MCP-servers/OI-code-index-mcp/run.py"
   ```

## 🔧 Configuration

### Project Path

The server requires a project path to be set before use. You can:

1. **Set via CLI argument:**
   ```bash
   ./brain-trust4 connect OI-code-index-mcp uv -- run --directory "$(pwd)/MCP-servers/OI-code-index-mcp" code-index-mcp --project-path /path/to/your/project
   ```

2. **Set via tool call:**
   ```bash
   ./brain-trust4 call OI-code-index-mcp set_project_path '{"path": "/path/to/your/project"}'
   ```

## 📋 Creating Intent Mappings

Intent mappings connect natural language keywords to specific MCP server tools.

**SQL to create intent mappings:**

```sql
BEGIN TRANSACTION;

-- Intent mappings for OI-code-index-mcp
INSERT OR REPLACE INTO intent_mappings (keyword, server_name, tool_name, priority) VALUES
('set project path', 'OI-code-index-mcp', 'set_project_path', 10),
('initialize project', 'OI-code-index-mcp', 'set_project_path', 10),
('set project', 'OI-code-index-mcp', 'set_project_path', 10),
('index project', 'OI-code-index-mcp', 'set_project_path', 10),
('search code', 'OI-code-index-mcp', 'search_code_advanced', 10),
('search for code', 'OI-code-index-mcp', 'search_code_advanced', 10),
('find code', 'OI-code-index-mcp', 'search_code_advanced', 10),
('code search', 'OI-code-index-mcp', 'search_code_advanced', 10),
('find files', 'OI-code-index-mcp', 'find_files', 10),
('search files', 'OI-code-index-mcp', 'find_files', 10),
('locate files', 'OI-code-index-mcp', 'find_files', 10),
('get file summary', 'OI-code-index-mcp', 'get_file_summary', 10),
('analyze file', 'OI-code-index-mcp', 'get_file_summary', 10),
('file summary', 'OI-code-index-mcp', 'get_file_summary', 10),
('refresh index', 'OI-code-index-mcp', 'refresh_index', 10),
('rebuild index', 'OI-code-index-mcp', 'refresh_index', 10),
('update index', 'OI-code-index-mcp', 'refresh_index', 10),
('build deep index', 'OI-code-index-mcp', 'build_deep_index', 10),
('deep index', 'OI-code-index-mcp', 'build_deep_index', 10),
('get settings', 'OI-code-index-mcp', 'get_settings_info', 10),
('settings info', 'OI-code-index-mcp', 'get_settings_info', 10),
('create temp directory', 'OI-code-index-mcp', 'create_temp_directory', 10),
('check temp directory', 'OI-code-index-mcp', 'check_temp_directory', 10),
('clear settings', 'OI-code-index-mcp', 'clear_settings', 10),
('refresh search tools', 'OI-code-index-mcp', 'refresh_search_tools', 10),
('file watcher status', 'OI-code-index-mcp', 'get_file_watcher_status', 10),
('configure file watcher', 'OI-code-index-mcp', 'configure_file_watcher', 10);

COMMIT;
```

## 📝 Creating Parameter Rules

Parameter rules define which fields are required and how to extract them from natural language queries.

**SQL to create parameter rules:**

```sql
BEGIN TRANSACTION;

-- Parameter rules for OI-code-index-mcp
INSERT OR REPLACE INTO parameter_rules (server_name, tool_name, tool_signature, required_fields, field_generators, patterns) VALUES
('OI-code-index-mcp', 'set_project_path', 'OI-code-index-mcp::set_project_path', '["path"]',
 '{"path": {"FromQuery": "OI-code-index-mcp::set_project_path.path"}}', '[]'),
('OI-code-index-mcp', 'search_code_advanced', 'OI-code-index-mcp::search_code_advanced', '["pattern"]',
 '{"pattern": {"FromQuery": "OI-code-index-mcp::search_code_advanced.pattern"}, "case_sensitive": {"FromQuery": "OI-code-index-mcp::search_code_advanced.case_sensitive"}, "context_lines": {"FromQuery": "OI-code-index-mcp::search_code_advanced.context_lines"}, "file_pattern": {"FromQuery": "OI-code-index-mcp::search_code_advanced.file_pattern"}, "fuzzy": {"FromQuery": "OI-code-index-mcp::search_code_advanced.fuzzy"}, "regex": {"FromQuery": "OI-code-index-mcp::search_code_advanced.regex"}, "start_index": {"FromQuery": "OI-code-index-mcp::search_code_advanced.start_index"}, "max_results": {"FromQuery": "OI-code-index-mcp::search_code_advanced.max_results"}}', '[]'),
('OI-code-index-mcp', 'find_files', 'OI-code-index-mcp::find_files', '["pattern"]',
 '{"pattern": {"FromQuery": "OI-code-index-mcp::find_files.pattern"}}', '[]'),
('OI-code-index-mcp', 'get_file_summary', 'OI-code-index-mcp::get_file_summary', '["file_path"]',
 '{"file_path": {"FromQuery": "OI-code-index-mcp::get_file_summary.file_path"}}', '[]'),
('OI-code-index-mcp', 'configure_file_watcher', 'OI-code-index-mcp::configure_file_watcher', '[]',
 '{"enabled": {"FromQuery": "OI-code-index-mcp::configure_file_watcher.enabled"}, "debounce_seconds": {"FromQuery": "OI-code-index-mcp::configure_file_watcher.debounce_seconds"}, "additional_exclude_patterns": {"FromQuery": "OI-code-index-mcp::configure_file_watcher.additional_exclude_patterns"}}', '[]');

COMMIT;
```

## 🔍 Parameter Extractors

Add these patterns to `parameter_extractors.toml.default`:

```toml
# ============================================================================
# OI-code-index-mcp
# ============================================================================

# set_project_path
"OI-code-index-mcp::set_project_path.path" = "conditional:if_contains:/|then:regex:(/[^\\s]+|\\S+)|else:regex:(?:path|project[\\s_-]?path|directory|dir|project)[\\s:]+([^\\s]+)"

# search_code_advanced
"OI-code-index-mcp::search_code_advanced.pattern" = "remove:search,find,for,code,in,with,the,a,an"
"OI-code-index-mcp::search_code_advanced.case_sensitive" = "regex:(?:case[\\s_-]?sensitive|case[\\s_-]?insensitive)[\\s:]+(true|false|yes|no)"
"OI-code-index-mcp::search_code_advanced.context_lines" = "regex:(?:context[\\s_-]?lines?|lines?[\\s_-]?of[\\s_-]?context)[\\s:]+(\\d+)"
"OI-code-index-mcp::search_code_advanced.file_pattern" = "regex:(?:file[\\s_-]?pattern|pattern|files?|in[\\s_-]?files?)[\\s:]+([^\\s]+(?:\\s+[^\\s]+)*)"
"OI-code-index-mcp::search_code_advanced.fuzzy" = "regex:(?:fuzzy|fuzzy[\\s_-]?match)[\\s:]+(true|false|yes|no)"
"OI-code-index-mcp::search_code_advanced.regex" = "regex:(?:regex|regular[\\s_-]?expression)[\\s:]+(true|false|yes|no)"
"OI-code-index-mcp::search_code_advanced.start_index" = "regex:(?:start[\\s_-]?index|offset|page)[\\s:]+(\\d+)"
"OI-code-index-mcp::search_code_advanced.max_results" = "regex:(?:max[\\s_-]?results?|limit|count)[\\s:]+(\\d+)"

# find_files
"OI-code-index-mcp::find_files.pattern" = "conditional:if_contains:*|then:regex:([^\\s]+)|else:remove:find,search,locate,files?,for,with,the,a,an"

# get_file_summary
"OI-code-index-mcp::get_file_summary.file_path" = "regex:(?:file[\\s_-]?path|path|file)[\\s:]+([^\\s]+(?:\\s+[^\\s]+)*)"

# configure_file_watcher
"OI-code-index-mcp::configure_file_watcher.enabled" = "regex:(?:enabled?|enable|disable)[\\s:]+(true|false|yes|no|on|off)"
"OI-code-index-mcp::configure_file_watcher.debounce_seconds" = "regex:(?:debounce[\\s_-]?seconds?|debounce)[\\s:]+([\\d.]+)"
"OI-code-index-mcp::configure_file_watcher.additional_exclude_patterns" = "regex:(?:exclude[\\s_-]?patterns?|exclude)[\\s:]+([^\\s]+(?:\\s*,\\s*[^\\s]+)*)"
```

## 🛠️ Available Tools

### Project Management

1. **`set_project_path`** - Initialize indexing for a project directory
   - **Parameters:**
     - `path` (required, string): Absolute path to the project directory

2. **`refresh_index`** - Rebuild the shallow file index after file changes
   - **Parameters:** None

3. **`build_deep_index`** - Generate the full symbol index used by deep analysis
   - **Parameters:** None

4. **`get_settings_info`** - View current project configuration and status
   - **Parameters:** None

### Search & Discovery

5. **`search_code_advanced`** - Smart search with regex, fuzzy matching, file filtering, and pagination
   - **Parameters:**
     - `pattern` (required, string): Search pattern
     - `case_sensitive` (optional, bool, default: true): Case-sensitive search
     - `context_lines` (optional, int, default: 0): Lines of context around matches
     - `file_pattern` (optional, string): Glob pattern to filter files (e.g., `*.py`)
     - `fuzzy` (optional, bool, default: false): Enable fuzzy matching
     - `regex` (optional, bool, default: None): Enable regex pattern matching
     - `start_index` (optional, int, default: 0): Pagination offset
     - `max_results` (optional, int, default: 10): Maximum results per page

6. **`find_files`** - Locate files using glob patterns
   - **Parameters:**
     - `pattern` (required, string): Glob pattern (e.g., `**/*.py`)

7. **`get_file_summary`** - Analyze file structure, functions, imports, and complexity
   - **Parameters:**
     - `file_path` (required, string): Path to the file to analyze

### Monitoring & Auto-refresh

8. **`get_file_watcher_status`** - Check file watcher status and configuration
   - **Parameters:** None

9. **`configure_file_watcher`** - Enable/disable auto-refresh and configure settings
   - **Parameters:**
     - `enabled` (optional, bool): Enable/disable file watcher
     - `debounce_seconds` (optional, float): Debounce timing
     - `additional_exclude_patterns` (optional, list): Additional exclude patterns

### System & Maintenance

10. **`create_temp_directory`** - Set up storage directory for index data
    - **Parameters:** None

11. **`check_temp_directory`** - Verify index storage location and permissions
    - **Parameters:** None

12. **`clear_settings`** - Reset all cached data and configurations
    - **Parameters:** None

13. **`refresh_search_tools`** - Re-detect available search tools (ugrep, ripgrep, etc.)
    - **Parameters:** None

## 💡 Usage Examples

### Natural Language Queries

```bash
# Set project path
./oi "set project path to /Users/dev/my-react-app"

# Search for code
./oi "search code for authentication"
./oi "find code matching getUserData in Python files"
./oi "search for ERROR|WARN using regex in *.py files"

# Find files
./oi "find all TypeScript files in src/components"
./oi "locate files matching test_*.js"

# Analyze files
./oi "get file summary for src/api/userService.ts"
./oi "analyze file src/components/App.tsx"

# Index management
./oi "refresh index"
./oi "build deep index"
```

### Direct Calls

```bash
# Set project path
./brain-trust4 call OI-code-index-mcp set_project_path '{"path": "/Users/dev/my-react-app"}'

# Search code
./brain-trust4 call OI-code-index-mcp search_code_advanced '{
  "pattern": "authentication",
  "file_pattern": "*.py",
  "max_results": 20
}'

# Find files
./brain-trust4 call OI-code-index-mcp find_files '{"pattern": "**/*.tsx"}'

# Get file summary
./brain-trust4 call OI-code-index-mcp get_file_summary '{"file_path": "src/api/userService.ts"}'
```

## 🚨 Troubleshooting

### Python Version Issues

**Error:** "Server closed connection" or "Initialization failed"

**Solution:** Ensure Python 3.10+ is available:
```bash
python3 --version  # Should show 3.10 or higher
```

If not, use `uv` to manage Python versions:
```bash
uv python install 3.10
uv run --python 3.10 code-index-mcp
```

### Server Timeout

**Error:** "Tool discovery timed out" or "Server response timed out"

**Solution:**
1. Check Python version compatibility
2. Ensure all dependencies are installed: `uv sync` or `pip install -e .`
3. Try running the server directly to check for errors:
   ```bash
   cd MCP-servers/OI-code-index-mcp
   uv run code-index-mcp
   ```

### Project Path Not Set

**Error:** "Project path not set" or similar

**Solution:** Set the project path first:
```bash
./brain-trust4 call OI-code-index-mcp set_project_path '{"path": "/absolute/path/to/project"}'
```

### File Watcher Not Working

**Error:** Auto-refresh not working when files change

**Solution:**
1. Check file watcher status:
   ```bash
   ./brain-trust4 call OI-code-index-mcp get_file_watcher_status
   ```
2. Install `watchdog` if missing:
   ```bash
   pip install watchdog
   ```
3. Manually refresh index:
   ```bash
   ./brain-trust4 call OI-code-index-mcp refresh_index
   ```

## 📖 Additional Resources

- **GitHub Repository:** https://github.com/OI-OS/OI-code-index-mcp
- **Original Repository:** https://github.com/johnhuang316/code-index-mcp
- **MCP Documentation:** https://modelcontextprotocol.io

## ✅ Verification

After installation, verify the server is working:

```bash
# List tools
./brain-trust4 tools OI-code-index-mcp

# Set project path
./oi "set project path to /path/to/your/project"

# Test search
./oi "find files matching *.py"
```

If all commands work, the server is successfully installed and configured!

