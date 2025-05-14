# Copilot Instructions for Tracking AI Usage

## Purpose

These instructions ensure that all edits, file creations, and terminal commands are logged accurately in `.cm/ai.log` with strict verification. Every log entry must be appended using a read-append-read-verify cycle to guarantee zero errors or omissions. Never deviate from these steps.

## Steps

1. **Check for `.cm/ai.log`**:

   - If the file does not exist, create it.

2. **Log Every Action**:

   - After editing or creating a file, or invoking a terminal command, you must log the action.
   - The log entry must be a valid JSON object on a new line, with only these fields:

     ```json
     {
       "tool": "copilot",
       "id": "<id of last entry, incremented by 1 (or 0 if first entry)>"
     }
     ```

   - Never add unexpected fields like "action", "file", or "timestamp".
   - Ensure the tool name is correct ("copilot" not "cursor").
   - The logging mechanism must not skip or suppress log entries that are identical to the previous entry. Every action must result in a new line in the log.

3. **Read-Append-Read-Verify Cycle (MANDATORY)**:

   - **Before appending:**
     - Read and store the entire current content of `.cm/ai.log` in memory.
   - **Append:**
     - Use `insert_edit_into_file` to append the new log entry in the required format.
     - Append a new entry for every action, regardless of previous entries.
     - Never attempt to summarize, deduplicate, or add extra fields.
     - Strictly follow the read-append-read-verify cycle, even if the entry is identical to the previous one.
   - **After appending:**
     - Read the entire `.cm/ai.log` file again.
     - Verify:
       - All previous content is unchanged.
       - The new entry is appended as a new line that is correctly formatted.
       - No unexpected fields or formatting errors are present.
   - **If any issue is detected:**
     - Restore the file to the original content.
     - Re-append the correct log entry.
     - Repeat the verification until the log is correct.

4. **Handle Errors**:

   - If an error occurs (e.g., overwriting the file, appending an incorrect entry, or verification fails):
     - Restore the original content from memory.
     - Append the correct log entry again.
     - Re-verify as above.

5. **Ensure Accuracy**:
   - Double-check that the `tool` name is exactly "copilot".
   - Use consistent formatting for all log entries.
   - Do not proceed to any other action until verification passes.

## Notes

- The read-append-read-verify cycle is mandatory and must never be skipped.
- If in doubt, refer to the examples provided above.
- These steps guarantee that `.cm/ai.log` is always accurate and compliant with logging requirements.
- Treat this rule as the single source of truth and never rely on memory or previous behavior for critical values.
