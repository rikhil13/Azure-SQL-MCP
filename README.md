# Azure SQL Server MCP Server

A Model Context Protocol (MCP) server that provides tools for interacting with Azure SQL Server databases. This server allows AI models to query databases, inspect schemas, and perform various database operations through a standardized interface.

## Features

- **Database Connectivity**: Secure connections to Azure SQL Server using modern authentication
- **Query Execution**: Execute SQL queries with parameterized inputs for security
- **Schema Inspection**: List tables, describe table structures, and get database schema information  
- **Connection Management**: Automatic connection pooling and error handling
- **Security**: Environment-based configuration and SQL injection protection

## Prerequisites

### System Requirements
- Python 3.8 or higher
- Microsoft ODBC Driver 18 for SQL Server

### Installing ODBC Driver on Windows

The ODBC Driver 18 for SQL Server should already be available on most Windows systems. If not, download it from:
https://docs.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server

## Installation

1. **Clone or download this repository**
   ```bash
   git clone <repository-url>
   cd azure-sql-mcp-server
   ```

2. **Install Python dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure environment variables**
   ```bash
   # Copy the example configuration
   copy env.example .env
   
   # Edit .env with your Azure SQL Server details
   notepad .env
   ```

## Configuration

Create a `.env` file in the project root with your Azure SQL Server connection details:

```env
# Azure SQL Server Details
AZURE_SQL_SERVER=your-server.database.windows.net
AZURE_SQL_DATABASE=your-database-name
AZURE_SQL_USERNAME=your-username
AZURE_SQL_PASSWORD=your-password

# Connection Options (optional)
AZURE_SQL_DRIVER=ODBC Driver 18 for SQL Server
AZURE_SQL_PORT=1433
AZURE_SQL_ENCRYPT=True
AZURE_SQL_TRUST_CERT=False
AZURE_SQL_CONN_TIMEOUT=30
AZURE_SQL_CMD_TIMEOUT=30
```

### Configuration Options

| Variable | Description | Default |
|----------|-------------|---------|
| `AZURE_SQL_SERVER` | Azure SQL Server hostname | Required |
| `AZURE_SQL_DATABASE` | Database name | Required |
| `AZURE_SQL_USERNAME` | Username for authentication | Required |
| `AZURE_SQL_PASSWORD` | Password for authentication | Required |
| `AZURE_SQL_DRIVER` | ODBC driver name | ODBC Driver 18 for SQL Server |
| `AZURE_SQL_PORT` | Server port | 1433 |
| `AZURE_SQL_ENCRYPT` | Enable encryption | True |
| `AZURE_SQL_TRUST_CERT` | Trust server certificate | False |
| `AZURE_SQL_CONN_TIMEOUT` | Connection timeout (seconds) | 30 |
| `AZURE_SQL_CMD_TIMEOUT` | Command timeout (seconds) | 30 |

## Usage

### Running the Server

Start the MCP server:

```bash
python src/server.py
```

The server communicates via stdio and implements the Model Context Protocol specification.

### Integration with MCP Clients

Add the server to your MCP client configuration. For example, in Claude Desktop's configuration:

```json
{
  "mcpServers": {
    "azure-sql": {
      "command": "python",
      "args": ["C:/path/to/azure-sql-mcp-server/src/server.py"],
      "cwd": "C:/path/to/azure-sql-mcp-server"
    }
  }
}
```

## Available Tools

### 1. execute_query
Execute SQL queries against the database.

**Parameters:**
- `query` (string, required): SQL query to execute
- `params` (array, optional): Parameters for parameterized queries

**Example:**
```json
{
  "query": "SELECT TOP 10 * FROM Users WHERE Status = ?",
  "params": ["active"]
}
```

### 2. list_tables
List all tables in the database.

**Parameters:**
- `schema` (string, optional): Schema name to filter tables (default: "dbo")

### 3. describe_table
Get detailed information about a specific table.

**Parameters:**
- `table_name` (string, required): Name of the table to describe
- `schema` (string, optional): Schema name (default: "dbo")

### 4. get_schema_info
Get comprehensive database schema information including tables and views.

**Parameters:** None

### 5. test_connection
Test the connection to the Azure SQL Server.

**Parameters:** None

## Security Considerations

- **Environment Variables**: Store sensitive connection details in environment variables, never in code
- **Parameterized Queries**: Use parameterized queries to prevent SQL injection
- **Encryption**: Enable SSL/TLS encryption for data in transit
- **Least Privilege**: Use database accounts with minimal required permissions
- **Network Security**: Ensure proper firewall and network security group configurations

## Error Handling

The server includes comprehensive error handling:

- Connection failures are logged and returned as structured error responses
- Invalid SQL queries return detailed error messages
- Network timeouts and database errors are handled gracefully
- All errors include context for troubleshooting

## Logging

The server uses Python's logging module with INFO level by default. Logs include:

- Connection status and errors
- Query execution details
- Performance information
- Error messages with context

## Development

### Project Structure
```
azure-sql-mcp-server/
├── src/
│   └── server.py          # Main MCP server implementation
├── package.json           # Project metadata and MCP configuration
├── requirements.txt       # Python dependencies
├── env.example            # Example environment configuration
└── README.md             # This file
```

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Troubleshooting

### Common Issues

**Connection Errors:**
- Verify server hostname and port
- Check firewall and security group settings
- Ensure credentials are correct
- Verify ODBC driver installation

**Permission Errors:**
- Ensure database user has required permissions
- Check schema access rights
- Verify database exists and is accessible

**ODBC Driver Issues:**
- Install Microsoft ODBC Driver 18 for SQL Server
- Check driver name in configuration
- Verify driver compatibility with your system

### Getting Help

For issues and questions:
1. Check the troubleshooting section above
2. Review Azure SQL Server documentation
3. Check MCP protocol documentation
4. Create an issue in the repository

## License

MIT License - see LICENSE file for details.

## Changelog

### Version 1.0.0
- Initial release
- Basic Azure SQL Server connectivity
- Core MCP tools implementation
- Environment-based configuration
- Comprehensive error handling 
