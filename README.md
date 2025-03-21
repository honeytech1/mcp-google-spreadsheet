# MCP Google Spreadsheet

A Google Spreadsheet and Google Drive operation tool implemented as an MCP (Model Context Protocol) server. This tool enables AI assistants to manipulate Google Spreadsheet and Google Drive files.

## Features

### Google Drive Operations

- **list_files**: Get a list of files in Google Drive
- **copy_file**: Copy a file in Google Drive
- **rename_file**: Rename a file in Google Drive

### Google Spreadsheet Operations

- **list_sheets**: Get a list of sheets in a spreadsheet
- **copy_sheet**: Copy a sheet within a spreadsheet
- **rename_sheet**: Rename a sheet within a spreadsheet
- **get_sheet_data**: Retrieve data from a sheet
- **add_rows**: Add rows to a sheet
- **add_columns**: Add columns to a sheet
- **update_cells**: Update cells in a single range
- **batch_update_cells**: Batch update cells in multiple ranges

## Prerequisites

- Go 1.24 or higher
- Google Cloud Platform project with enabled APIs:
  - Google Drive API
  - Google Sheets API

## Installation

```bash
go install github.com/kazz187/mcp-google-spreadsheet@latest
```

This will install the `mcp-google-spreadsheet` binary in your `$GOPATH/bin` directory.

## Configuration

The following environment variables need to be set:

- `MCPGS_CLIENT_SECRET_PATH`: Path to the Google API client secret file (https://developers.google.com/identity/protocols/oauth2/native-app)
- `MCPGS_TOKEN_PATH`: Path to the Google API token file (will be created automatically if it doesn't exist)
- `MCPGS_FOLDER_ID`: ID of the Google Drive folder to operate on

### Google API Setup Steps

1. Access the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a project
3. Enable the Google Drive API and Google Sheets API
4. Create credentials (OAuth client ID)
5. Download the client secret

## Usage

### Starting the Server

```bash
export MCPGS_CLIENT_SECRET_PATH=/path/to/client_secret.json
export MCPGS_TOKEN_PATH=/path/to/token.json
export MCPGS_FOLDER_ID=your_folder_id
mcp-google-spreadsheet
```

If you installed using `go install`, ensure that `$GOPATH/bin` is included in your PATH.

On first launch, authentication is required. A browser will automatically open with the Google account authentication screen. After successful authentication, you will be automatically returned to the application. If the browser doesn't open automatically, open the URL displayed in the console.

### MCP Configuration

To use with AI assistants like Claude or ChatGPT, add the following to your MCP configuration file:

```json
{
  "mcpServers": {
    "mcp_google_spreadsheet": {
      "command": "mcp-google-spreadsheet",
      "args": [],
      "env": {
        "MCPGS_CLIENT_SECRET_PATH": "/path/to/client_secret.json",
        "MCPGS_TOKEN_PATH": "/path/to/token.json",
        "MCPGS_FOLDER_ID": "your_folder_id"
      }
    }
  }
}
```

If you installed using `go install`, you can specify just the executable name in the `command` field as shown above, instead of an absolute path. In that case, make sure `$GOPATH/bin` is included in the PATH of the user running the MCP server.

## Security

- Access is restricted to files only within the specified folder ID
- Directory traversal attacks (using `../` in paths) are prevented
- Files specified by users are verified to exist within the specified folder
