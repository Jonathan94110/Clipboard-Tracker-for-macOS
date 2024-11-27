# Clipboard-Tracker-for-macOS
A Python script for monitoring clipboard content on macOS, saving the last 30 entries in real-time to a text file.
# Clipboard Tracker for Mac

import pyperclip
import time
from collections import deque

# Define the maximum number of clipboard items to track
MAX_CLIPBOARD_HISTORY = 30

# Use a deque to store clipboard history
clipboard_history = deque(maxlen=MAX_CLIPBOARD_HISTORY)

# Function to save the clipboard history to a file
def save_history_to_file():
    with open("clipboard_history.txt", "w") as file:
        file.write("\n".join(clipboard_history))

# Function to check clipboard and update history
def monitor_clipboard():
    last_clipboard_content = ""
    while True:
        try:
            # Get current clipboard content
            clipboard_content = pyperclip.paste()
            
            # If the clipboard content is new, add it to history
            if clipboard_content != last_clipboard_content:
                clipboard_history.append(clipboard_content)
                last_clipboard_content = clipboard_content

                # Save updated history to a file
                save_history_to_file()

            # Wait before checking the clipboard again
            time.sleep(1)
        except KeyboardInterrupt:
            print("\nClipboard monitoring stopped.")
            break
        except Exception as e:
            print(f"Error: {e}")

if __name__ == "__main__":
    print("Starting clipboard monitoring. Press Ctrl+C to stop.")
    monitor_clipboard()
