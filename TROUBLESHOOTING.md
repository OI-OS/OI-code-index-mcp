# Troubleshooting OI-code-index-mcp

## Known Issue: Tool Discovery Timeout

### Problem
The server initializes successfully but tool discovery times out after 30 seconds. When tested directly, the server crashes with "unhandled errors in a TaskGroup" when trying to list tools.

### Error Details
```
exceptiongroup.ExceptionGroup: unhandled errors in a TaskGroup (1 sub-exception)
```

This occurs in the MCP SDK's async handling when processing tool list requests.

### Investigation Results

1. **Python Version**: ✅ Python 3.10.19 installed and working
2. **Server Initialization**: ✅ Server initializes successfully
3. **Tool Registration**: ❌ Tool discovery fails/times out
4. **Direct Testing**: ❌ Server crashes when handling tool list requests

### Attempted Solutions

1. ✅ **Python 3.10 Installation**: Installed via `uv python install 3.10`
2. ✅ **Simple Project Path**: Tested with `/tmp/test-code-index` - same issue
3. ✅ **Project Path at Startup**: Used `--project-path` flag - same issue
4. ❌ **Tool Discovery**: Still times out regardless of configuration

### Root Cause Analysis

The issue appears to be in the MCP SDK's async handling (`anyio.create_task_group`). The server:
- Initializes correctly
- Responds to `initialize` requests
- Crashes when handling `tools/list` requests
- Shows async TaskGroup errors

### Workarounds

**Current Status**: Server is not functional for tool discovery. The following workarounds have been attempted:

1. **Setting project path first**: No effect
2. **Using simpler paths**: No effect  
3. **Different transport modes**: Same issue
4. **Direct JSON-RPC testing**: Confirms server crash

### Recommendations

1. **Check for MCP SDK Updates**: The server uses `mcp>=0.3.0`. Consider:
   - Updating to latest MCP SDK version
   - Checking for known issues in MCP SDK async handling

2. **Report to Maintainers**: 
   - GitHub: https://github.com/OI-OS/OI-code-index-mcp
   - Original: https://github.com/johnhuang316/code-index-mcp

3. **Alternative Approaches**:
   - Use HTTP/SSE transport instead of stdio
   - Check if there's a newer version of the server
   - Consider using a different code indexing solution

### Current Configuration

```bash
# Connection command
uv run --python 3.10 --directory "$(pwd)/MCP-servers/OI-code-index-mcp" code-index-mcp --transport stdio

# With project path
uv run --python 3.10 --directory "$(pwd)/MCP-servers/OI-code-index-mcp" code-index-mcp --transport stdio --project-path /path/to/project
```

### Status

**⚠️ Server is currently non-functional for tool discovery**

- Python 3.10: ✅ Installed and working
- Server startup: ✅ Works
- Tool discovery: ❌ Times out/crashes
- Tool execution: ❌ Cannot test (tools not discovered)

### Next Steps

1. Monitor GitHub issues for fixes
2. Try updating MCP SDK to latest version
3. Test with HTTP/SSE transport if available
4. Consider alternative code indexing solutions

