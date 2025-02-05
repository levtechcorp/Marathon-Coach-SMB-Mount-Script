# Marathon Coach SMB Mount Script

**Version:** Alpha 4.0.8

A robust AppleScript solution for mounting SMB network drives on macOS custom tailored for Marathon Coach. This script automates the mounting of your personal directory along with a user-selected list of SMB shares. It securely stores your credentials in the macOS Keychain, offers configurable share lists, and supports a timeout-based reconfiguration dialog with an option to update your credentials.

> **Note:**  
> For most users, it is recommended to download and use the provided **.app release**. This release is fully packaged and ready to run at boot. Only download the source if you need to customize the list of shares or other configuration options.

---

## Overview

The Marathon Coach SMB Mount Script:

- **Automatically mounts network drives:**  
  Combines your personal directory (`smb://nostromo/persdir/<username>`) with additional SMB shares.
  
- **Secure Credential Storage:**  
  Retrieves or creates keychain entries for your SMB username and password.
  
- **User Configuration:**  
  Provides a default list of known SMB shares (including names with spaces like `"marketing & paint"`) and allows you to add custom shares.
  
- **Preferences Management:**  
  Saves your share configuration in `~/Library/Preferences/com.marathoncoach.mountscript.shares.txt` so that your settings persist between runs.
  
- **Timeout Behavior:**  
  If the reconfiguration dialog times out after 10 seconds, it defaults to using the existing share configuration without prompting further.
  
- **Credential Updates:**  
  Includes an option to update your stored SMB credentials directly from the reconfigure dialog.

---

## Features

- **Keychain Integration:**  
  Uses the macOS Keychain to securely store and retrieve your SMB credentials.

- **Customizable Share List:**  
  Choose from a default list or add custom SMB shares. Your configuration is saved for future runs.

- **Timeout and Default Actions:**  
  The reconfiguration dialog will automatically default to "Use Existing" after 10 seconds if no action is taken.

- **Clear Error Handling:**  
  The script provides options to skip or abort mounting if errors occur during drive mounting.

---

## Requirements

- **macOS**  
- **AppleScript** (can be run via Script Editor or compiled as an application)
- **SMB Network Access** to the desired shares

---

## Installation

### Using the Pre-Built .app Release

For most users, it is recommended to use the pre-built **.app release**. This version is packaged and ready to run without further modification. Simply download the latest release from the [Releases](https://github.com/Levtechcorp/your-repository/releases) page.

### Setting Up the Application to Run on Boot

To ensure the application runs automatically at login (i.e., on boot):

1. **Download and Install the .app File:**  
   Place the downloaded `.app` file in your Applications folder or a location of your choice.

2. **Open System Preferences:**  
   Click on the Apple menu () at the top-left of your screen, then choose **System Preferences**.

3. **Go to Users & Groups:**  
   In the System Preferences window, click on **Users & Groups**.

4. **Select Your User Account:**  
   Make sure your account is selected on the left-hand side.

5. **Open Login Items:**  
   Click the **Login Items** tab.

6. **Add the Application:**  
   Click the **+** button and navigate to the location where you saved the `.app` file. Select the app and click **Add**.

7. **Verify the Application is Listed:**  
   Ensure that the application appears in the list of Login Items. This means it will launch automatically every time you log in.

> **Tip:**  
> If you need to change the shares later, run the source version of the script. Otherwise, the release version is sufficient for daily use.

### Using the Source Version (For Customization)

If you require custom changes (for example, altering the default share list), download the source, open it in the AppleScript Editor, make your modifications, and then save it as an application. Once saved, follow the same steps above to add it to your Login Items.

---

## Usage

1. **Run the Application:**  
   If using the .app release, it will launch automatically on login (if set up as described above) or can be run manually by double-clicking the application.

2. **Enter Credentials:**  
   If credentials are not found in the Keychain, you will be prompted for your SMB username and password.

3. **Reconfiguration Dialog:**  
   If a previous share configuration exists, a dialog will be displayed with three options:
   - **Use Existing:**  
     Immediately mount the previously configured shares.
   - **Reconfigure:**  
     Choose from a list of default SMB shares or add custom shares.
   - **Change Credentials:**  
     Update your stored SMB username and password.
     
   _Note:_ This dialog will automatically timeout after 10 seconds (auto-selecting **Use Existing**) if no action is taken.

4. **Mounting:**  
   The script combines your personal directory with the chosen SMB shares and mounts them using Finder.

---

## Configuration

- **Personal Directory Prefix:**  
  Default is `smb://nostromo/persdir/` (appended with your username).

- **Preferences File:**  
  Stored at `~/Library/Preferences/com.marathoncoach.mountscript.shares.txt`.

- **Keychain Items:**  
  - Username stored under service `MountAllSharesUser`
  - Password stored under service `MountAllSharesPass`

---

## Changelog

### Alpha 4.0.8
- Fixed the timeout behavior in the reconfigure dialog so that if it times out, it automatically selects "Use Existing" and uses the found mount list.
- Added an extra check to ensure that if no button value is returned, it defaults to "Use Existing" and proceeds with mounting.

### Alpha 4.0.7
- Introduced a timeout for the reconfigure dialog with a clear message indicating auto-selection after 10 seconds.
- Added a "Change Credentials" option to update keychain entries.

### Alpha 4.0.6
- Removed the flashing countdown to improve user experience.
- Retained the "Change Credentials" option.

### Alpha 4.0.5
- Fixed type conversion issues by coercing list items to text.
- Removed problematic checks in the reconfigure dialog.

### Alpha 4.0.4
- Added a countdown timer and a "Change Credentials" option.
- Improved error handling and display dialog usage.

### Alpha 4.0.2
- Fixed a parsing issue with the `choose from list` handler where multiple lines were incorrectly split, causing “Can’t make {item 1 of {...}} into type text.”
- Improved line continuation usage in display dialog statements (using ¬) to avoid “Expected expression” errors.
- Refined variable references in `loadSharePrefs` (removed the `linesLisSt` typo).
- Retained the “marketing & paint” share folder reference without renaming.

### Alpha 4.0.1
- Introduced an explicit `listToString` handler to convert multi-item selections into displayable strings.
- Addressed potential “Can’t make {…} into text” errors by coercing lists in the mount logic.
- Updated Keychain retrieval calls to handle missing credentials more gracefully.

### Alpha 4.0.0
- Added a reconfigure flow with a 10-second timeout on the “Use Existing” vs. “Reconfigure” dialog to prevent the script from hanging on boot.
- Incorporated user-defined custom SMB shares saved to preferences in `~/Library/Preferences/<filename>`.
- Enhanced error handling in `mountServerVolumes`, allowing the user to Skip or Abort when shares fail to mount.

### Alpha 3.9.9
- Migrated from storing credentials in script properties to storing them in the macOS Keychain.
- Implemented a new preferences system to remember selected shares across runs.
- Streamlined the personal directory logic with `smb://nostromo/persdir/<username>`.

### Alpha 3.9.8
- Refactored AppleScript handlers for modularity:
  - `getOrCreateKeychainItem`
  - `mountServerVolumes`
  - `loadSharePrefs` / `saveSharesToFile`
- Improved multiline display dialogs for user prompts (initial pass).

### Version 3.9.7 (2025-02-05)
- Adjusted dynamic SMB path concatenation in the drive list to ensure correct string assembly. [【1†source】]
- Enhanced error logging in the mount volume loop to include more detailed messages. [【2†source】]
- Refactored the username retrieval and storage code using the `defaults` command for clarity. [【3†source】]

### Version 3.9.6
- Renamed the local variable for the preference domain to `userPrefDomain` for better readability. [【4†source】]
- Updated the prompt dialog text to be more user-friendly. [【5†source】]
- Slight adjustment to the try–on error block for username retrieval, ensuring smoother fallback behavior. [【6†source】]

### Version 3.9.5
- Separated static drive paths from user-specific paths by creating distinct lists, then merging them—improving maintainability. [【7†source】]
- Inserted additional inline comments to clarify code sections and intended functionality. [【8†source】]
- Made minor modifications to the log message formatting to standardize output. [【9†source】]

### Version 3.0.4
- Reorganized the overall script structure to more closely adhere to AppleScript best practices as outlined in the language guide. [【10†source】]
- Modified the order of drive mounting so that static drives are processed before the dynamic ones. [【11†source】]
- Fixed small syntax inconsistencies (e.g., spacing and string literal formatting) flagged during testing. [【12†source】]

### Version 2
- Initial implementation of dynamic SMB path concatenation that appends the username into specific drive paths. [【13†source】]
- Added basic error handling around the mount volume commands to prevent the script from hanging. [【14†source】]
- Implemented the username prompt for first-run configuration, storing the result using the `defaults` command. [【15†source】]

---

## Developer Information

**Author:** Michelle Levenhagen  
**GitHub:** [Levtechcorp](https://github.com/Levtechcorp)  
**Email:** [levtechcorp@gmail.com](mailto:levtechcorp@gmail.com)

---

## License

This project is licensed under the [GNU General Public License v3 (GPLv3)](https://www.gnu.org/licenses/gpl-3.0.html).

---

## Contributing

Contributions, bug reports, and feature requests are welcome!  
Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to contribute to this project.

---

## Support

For issues, suggestions, or further assistance, please feel free to [open an issue on GitHub](https://github.com/Levtechcorp/your-repository/issues) or contact the developer directly.

---

Thank you for using the Marathon Coach SMB Mount Script!
