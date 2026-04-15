# Gemma Reasoning Pipeline Filter

Middleware for Open WebUI that normalizes Gemma 4's proprietary reasoning tokens into standard UI-compatible tags.

## Functionality

Gemma 4 emits reasoning via `<|channel>thought` and `<channel|>` sequences. This filter intercepts the model's output stream to map these to `<think>` and `</think>`, enabling the native reasoning UI in Open WebUI.

### Core Components

* **Real-time Interception**: Modifies Server-Sent Events (SSE) chunks on the fly for immediate UI rendering.
* **Stateful Buffering**: Implements a look-ahead buffer to handle control tokens fragmented across multiple network packets.
* **Sanitization**: Automatically strips extraneous control artifacts including `<|turn|>`, `<bos>`, and `<eos>`.
* **Persistence Cleanup**: The `outlet` hook ensures the final message stored in the database is sanitized and formatted.

## Configuration

The filter is controlled via Open WebUI's "Valves" system.

| Variable | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `enabled_for_models` | `string` | `"gemma"` | Comma-separated list of model ID substrings to trigger the filter. |

## Installation

1.  Copy the `gemma_thinking_fix.json` content.
2.  Import the JSON into the **Workspace > Filters** section of your Open WebUI instance.
3.  Enable the filter globally or for specific model sets.

## Technical Notes

The script uses module-level regex compilation for $O(1)$ initialization and maintains a minimal memory footprint, making it suitable for deployment on low-spec edge hardware.
