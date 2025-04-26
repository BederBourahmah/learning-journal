# MRHB Sahel Wallet Account Creation Process

## Research Details
- **Date**: April 26th, 2025
- **Researcher**: Beder Bourahmah
- **Purpose**: Document the account creation process for the MRHB Sahel wallet to understand the user experience and identify potential integration points.

## Process Documentation

### 1. Download and Installation
- **Source**: [Sahal Wallet](https://play.google.com/store/apps/details?id=sahal.wallet.app&pcampaignid=web_share)
- **Device**: Samsung Galaxy S25+
- **Operating System**: Android 15
- **App Rating**: Teen (USA Play Store)
- **Content Information**: 
  - Users Interact
  - In-App Purchases
- **Installation Requirements**:
  - Google Play Store account
  - Internet connection
  - Storage space: ~60MB
  - Android 6.0 or higher
- **Compatibility Notes**:
  - App runs smoothly on Samsung Galaxy S25+
  - No device-specific issues encountered
  - App supports both light and dark mode (pending confirmation)
  - Responsive to different screen orientations (pending confirmation)

### 2. Account Creation Flow
1. Initial Permission Request
   - **Step**: App requests notification permission
   - **Options**: 
     - Allow
     - Don't Allow
   - **Choice Made**: Allow
   - **Observations**: 
     - Standard Android notification permission dialog
     - App requires this permission to send transaction notifications and alerts
   - **Questions**: 
     - Can this permission be changed later in settings?
     - What types of notifications will be sent?

2. Landing Screen
   - **Visual Elements**:
     - Sahal logo at the top
     - 3D rendered "MRHB" coin in the center
     - Dynamic text: "Your <red word> Wallet" where the red word cycles between:
       - NFT
       - Finance
       - Crypto
   - **Interactive Elements**:
     - Primary button: "Let's Go"
     - Secondary login options:
       - Text: "or login with"
       - Social login buttons:
         - Google (disabled, shows "Coming Soon..." notification)
         - X (Twitter)
         - Apple
   - **Choice Made**: Clicked "Let's Go" button
   - **Observations**:
     - Clean, modern interface
     - Animated text transitions
     - Social login options indicate future integration plans
   - **Questions**:
     - When will Google login be available?
     - Are there any differences in features between social login and direct registration?

3. Passcode Creation Screen
   - **Loading State**:
     - Loading modal appeared
     - Duration: ~4 seconds
   - **Screen Elements**:
     - Header:
       - "Settings" in small red text
       - "New Passcode" in larger white text
     - Background: Black
     - Center:
       - 6 dark reddish-gray circles
       - Text: "Create your passcode" in white
     - Navigation:
       - Left arrow in top left corner
   - **Interaction**:
     - Clicked left arrow
   - **Observations**:
     - Dark theme interface
     - Clear visual hierarchy
     - Intuitive navigation
   - **Questions**:
     - What is the purpose of the left arrow? (Back navigation?)
     - What are the passcode requirements?
     - Can the passcode be changed later?

4. Passcode Input Behavior
   - **Navigation**:
     - Back arrow returned to landing page
     - Clicked "Let's Go" again to return to passcode screen
   - **Input Testing**:
     - Native numeric pad appears on most screen touches except for bottom quarter of the screen
     - Input Validation:
       - Multiple "." not allowed
       - Accepted input: one "." followed by 5 ","
   - **Observations**:
     - Flexible input validation
     - Native Android numeric input
     - Back navigation works as expected
   - **Questions**:
     - What is the purpose of allowing "," in the passcode?
     - Is there a maximum length for the passcode?
     - Are there any security implications of the flexible input validation?

5. Dashboard Navigation
   - **Bottom Navigation Bar**:
     - 5 white icons with white text labels
     - First icon: White circle with three white dots in upward triangle
     - Label: "Dash"
     - Current page indicated by gray filled circle
   - **Top Navigation**:
     - Left: Pink circle with white bidirectional vertical arrows
     - Right: Pink circle with white bell icon
   - **Observations**:
     - Consistent navigation elements across pages
     - Clear visual hierarchy
     - Intuitive iconography
   - **Questions**:
     - What are the other navigation options?
     - What is the purpose of the bidirectional arrows?
     - What notifications are available through the bell icon?

6. Wallet Activity Page
   - **Header**:
     - "Wallet" in small pink text
     - "Activity" in larger white text
     - "Wallet 2" in smaller white text
   - **Navigation**:
     - Same top navigation as Dashboard
   - **Observations**:
     - Multiple wallet support
     - Consistent navigation pattern
   - **Questions**:
     - Is "Wallet 2" a new wallet created after reinstallation?
     - Is account creation happening server-side or locally?
     - How can we inspect the API calls to understand the account creation process?
   - **Technical Investigation Needed**:
     - API call inspection
     - Network traffic analysis
     - Account creation flow reverse engineering

### 3. Verification Process
- **Required Documents**:
  - [List required documents]
- **Verification Steps**:
  1. [Step 1]
  2. [Step 2]
  [etc...]
- **Time to Complete**: [How long did it take?]
- **Success Rate**: [Did it work first try?]

### 4. Initial Setup
- **Required Actions**:
  - [List any required setup steps]
- **Optional Features**:
  - [List any optional features]
- **Security Setup**:
  - [Document security features]

## Findings

### Positive Aspects
- [List positive aspects of the process]
- [Note any particularly good UX elements]

### Challenges
- [List any challenges encountered]
- [Note any confusing or problematic steps]

### Integration Considerations
- [Note any points relevant to integration]
- [Document any API or technical requirements observed]

## Questions and Assumptions
- [List any questions that need to be answered]
- [Document any assumptions made during the process]

## Next Steps
- [List any follow-up research needed]
- [Note any areas requiring further investigation]

## References
- [Link to any relevant documentation]
- [Link to any related tasks]

## Technical Investigation
For detailed API investigation and network traffic capture process, see [API Investigation Documentation](api-investigation.md).

## Notes
- Keep documentation up to date
- Document any changes in the process
- Note any issues or challenges encountered

## References
- [Link to any relevant documentation]
- [Link to any related tasks] 