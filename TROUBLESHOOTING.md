# Troubleshooting OI-code-index-mcp

## ✅ FIXED: Tool Discovery Timeout/Crash

### Problem (RESOLVED)
The server was crashing with "unhandled errors in a TaskGroup" when trying to list tools or call tools. This was a bug in the standard FastMCP library's async handling.

### Solution
**Updated to use OI-OS/OI-fastmcp fork** which fixes the async TaskGroup issue.

### Changes Made

1. ✅ **Updated Dependency**: Changed from `mcp>=0.3.0` to `fastmcp @ git+https://github.com/OI-OS/OI-fastmcp.git` in `pyproject.toml`
2. ✅ **Updated Imports**: Changed all imports from `mcp.server.fastmcp` to `fastmcp`
3. ✅ **Removed Dependencies Parameter**: Removed `dependencies=["pathlib"]` from FastMCP initialization (not supported in OI-fastmcp)

### Files Modified

- `pyproject.toml`: Updated dependency to use OI-fastmcp fork
- `src/code_index_mcp/server.py`: Updated imports and FastMCP initialization
- `src/code_index_mcp/utils/context_helper.py`: Updated import
- `src/code_index_mcp/services/base_service.py`: Updated import

### Current Status

**✅ Server is now fully functional!** 
- Tool discovery works
- Tool calls work correctly
- Server initializes and responds properly
- Tested successfully with `set_project_path` tool

### Testing

The server has been tested and confirmed working:
```bash
oi call OI-code-index-mcp set_project_path '{"path": "/path/to/project"}'
# ✅ Successfully indexed 431 files
```

### References

- OI-fastmcp fork: https://github.com/OI-OS/OI-fastmcp
- Original FastMCP: https://github.com/jlowin/fastmcp
