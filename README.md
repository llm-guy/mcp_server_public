# Basic MCP Server for Data File Analysis

A Model Context Protocol (MCP) server that provides tools for analyzing CSV and Parquet files.

## Features

- **CSV File Analysis**: Summarize CSV files by reporting row and column counts
- **Parquet File Analysis**: Summarize Parquet files by reporting row and column counts
- **Sample Data**: Includes sample user data in both CSV and Parquet formats

## Project Structure

```
mix_server/
│
├── data/                 # Sample CSV and Parquet files
│   ├── sample.csv
│   └── sample.parquet
│
├── tools/                # MCP tool definitions
│   ├── __init__.py
│   ├── csv_tools.py
│   └── parquet_tools.py
│
├── utils/                # Reusable file reading logic
│   ├── __init__.py
│   └── file_reader.py
│
├── server.py             # MCP server instance
├── main.py              # Entry point for the MCP server
├── generate_parquet.py  # Script to convert CSV to Parquet
└── README.md            # This file
```

## Installation

1. **Install uv** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Create and activate virtual environment**:
   ```bash
   uv venv
   source .venv/bin/activate
   ```

3. **Install dependencies**:
   ```bash
   uv add "mcp[cli]" pandas pyarrow
   ```

## Usage

### Running the Server 

Start the MCP server:
```bash
uv run main.py
```

### Using with LM Studio 

To use this MCP server with LM Studio, edit your `mcp.json` file and add:

This starts the MCP server for you.

```json
{
  "mcpServers": {
    "mix_server": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/your/mix_server",
        "run",
        "main.py"
      ]
    }
  }
}
```

**Note**: Replace `/path/to/your/mix_server` with the actual path to your mix_server directory.

Once loaded you should see all available tools for your local LLM to use.

### Available Tools

1. **summarize_csv_file(filename: str)**
   - Summarizes a CSV file by reporting its number of rows and columns
   - Example: `summarize_csv_file("sample.csv")`

2. **summarize_parquet_file(filename: str)**
   - Summarizes a Parquet file by reporting its number of rows and columns
   - Example: `summarize_parquet_file("sample.parquet")`

### Sample Data

The server includes sample user data with the following structure:
- **id**: Unique identifier
- **name**: User's full name
- **email**: User's email address
- **signup_date**: Date when the user signed up

## Development

### Adding New Tools

1. Create a new file in the `tools/` directory
2. Import the MCP server instance: `from server import mcp`
3. Define your tool function with the `@mcp.tool()` decorator
4. Import the new tool module in `main.py`

### Adding New File Formats

1. Add utility functions in `utils/file_reader.py`
2. Create corresponding tools in the `tools/` directory
3. Import the new tools in `main.py`

## Dependencies

- **mcp[cli]**: Official MCP SDK and command-line tools
- **pandas**: For reading CSV and Parquet files
- **pyarrow**: Adds support for reading Parquet files via Pandas

## License

This project is open source and available under the MIT License.
