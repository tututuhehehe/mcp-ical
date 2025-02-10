# MCP ICal Server

## Overview
The mcp-ical server is a Model Context Protocol (MCP) server designed to allow users to interact with their macOS calendar through natural language queries. This server provides a seamless way to manage calendar events, making it easier to schedule, update, and retrieve events using simple, conversational language.

## Features
- **Create Events**: Easily add new events to your calendar with support for:
  - Creating events in custom calendars.
  - Adding notes and locations.
  - Setting reminders.
  - Supporting recurring events.
  - **Examples**:
    - "Schedule a meeting with the team tomorrow at 10 AM in my Work calendar with notes to update the slide deck."
    - "Create a recurring event for my weekly team meeting every Monday at 9 AM."
    - "Add a lunch appointment on Friday at 12 PM with reminders set for 30 minutes before."

- **List Events**: Retrieve a list of events within a specified date range.
  - **Examples**:
    - "What events do I have this week?"
    - "Find an optimal time to schedule a new event next week."

- **Update Events**: Modify existing events with features such as:
  - Updating the date and time.
  - Changing reminders.
  - Making an event recurring.
  - Moving an event from one calendar to another.
  - **Examples**:
    - "Change my lunch appointment on Friday to 1 PM and move it to my Personal calendar."
    - "Add a reminder one day before for my meeting on Thursday at 3 PM."

- **List Calendars**: Get a list of all available calendars.
  - **Example**: "Show me my calendars."

> [!TIP]
> Since you can create events in custom calendars, if you have your Google Calendar set up inside of iCloud Calendar, you can use this MCP server to create events in your Google Calendar too!

- **Note**: Deleting events has intentionally not been exposed as an MCP tool, but there is code already in place if the user wishes to add that functionality.

## Known Issues
- Sometimes the creation of recurring events is not always followed correctly, specifically for events with non-standard recurring schedules. Better models seem to have better success, i.e., 3.5 Sonnet over Haiku.
- Reminders created for recurring events will sometimes be one day off due to the quirks of how macOS treats the starting reference point for an all-day event.

## How to Run
These instructions are specific to setting up the MCP server to run with Claude for Desktop, but the server can also be used with any MCP-compatible client. For more details, see [this link](https://modelcontextprotocol.io/quickstart/client).

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/mcp-ical.git
   cd mcp-ical
   ```

2. **Install dependencies**:
   Make sure you have Python 3.12 or higher installed, then run:
   ```bash
   uv sync
   ```

3. **Configure Claude for Desktop**:
   To connect your MCP server to Claude for Desktop, you need to update the configuration file. Open the configuration file located at `~/Library/Application Support/Claude/claude_desktop_config.json` in a text editor. If the file does not exist, create it.

   Add the following to your json config

   ```json
   {
       "mcpServers": {
           "mcp-ical": {
               "command": "uv",
               "args": [
                   "--directory",
                   "/ABSOLUTE/PATH/TO/PARENT/FOLDER/mcp-ical",
                   "run",
                   "mcp-ical"
               ]
           }
       }
   }
   ```

   Make sure to replace `/ABSOLUTE/PATH/TO/PARENT/FOLDER/mcp-ical` with the actual absolute path to your project directory. Save the file and restart Claude for Desktop.

4. **IMPORTANT**: In order to grant calendar permissions, you must launch the Claude for Desktop app via your terminal. Use the following command:
   ```bash
   /Applications/Claude.app/Contents/MacOS/Claude
   ```

5. **Ask a question that triggers one of the calendar tools**. For example, you can ask: "What's my schedule looking like for next week?"

6. **Accept the prompt asking for calendar access**.

## How to Run Tests
> [!WARNING] 
> Running these tests will create temporary events in your macOS calendar. Every care has been taken to create temporary calendars to isolate all new events created for each test, with cleanup processes in place to delete all created calendars, events, and other resources after the tests run. However, you should be cautious and only run these tests if you are actively developing this project further.

To ensure everything is working correctly, you can run the tests included in the project. Follow these steps:

1. **Install test dependencies** (if not already installed):
   ```bash
   uv sync --dev
   ```

2. **Run the tests**:
   ```bash
   uv run pytest tests
   ```

This will execute all the tests in the `tests` directory and provide you with a report of the results.

## Contributing
Contributions are welcome! Please feel free to submit a pull request or open an issue for any suggestions or improvements.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.






