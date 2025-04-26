# MRHB Sahel Wallet API Investigation

## Overview
This document details the investigation of API calls made by the MRHB Sahel Wallet app during launch and account creation.

## Investigation Process

### 1. Network Traffic Capture Process

#### Step 1: Set Up Android Emulator
1. Install Android Studio
   - Download from [Android Studio website](https://developer.android.com/studio)
   - Follow installation instructions for your operating system

2. Create a Virtual Device
   - Open Android Studio
   - Click "More Actions" > "Virtual Device Manager"
   - Click "Create Device"
   - Select "Phone" category
   - Choose "Pixel 6" (or similar modern device)
   - Select a system image (recommended: API 33 or newer)
   - Complete the setup with default settings

3. Configure Emulator
   - Launch the virtual device
   - Complete initial setup
   - Enable developer options (no security key required)
   - Enable USB debugging

#### Step 2: Install Network Inspection Tool
- **Tool**: Charles Proxy (Free trial available)
- **Purpose**: Capture and inspect HTTP/HTTPS traffic
- **Installation**:
  1. Download from [Charles Proxy website](https://www.charlesproxy.com/download/)
  2. Install on your development machine
  3. Launch Charles Proxy

#### Step 3: Configure Emulator Proxy
1. In Charles Proxy:
   - Go to Help > SSL Proxying > Install Charles Root Certificate on Mobile Device
   - Note the IP address and port shown
2. In Android Emulator:
   - Go to Settings > Network & Internet > Wi-Fi
   - Long press the connected network
   - Select "Modify Network"
   - Expand "Advanced Options"
   - Set Proxy to "Manual"
   - Enter the IP and port from Charles

#### Step 4: Install SSL Certificate
1. In Android Emulator:
   - Open Chrome
   - Navigate to chls.pro/ssl
   - Download and install the certificate
   - Go to Settings > Security > Install from Storage
   - Select the downloaded certificate

#### Step 5: Prepare for Capture
1. In Charles Proxy:
   - Clear all existing sessions
   - Enable SSL Proxying (Proxy > SSL Proxying Settings)
   - Add "*" to the SSL Proxying locations
2. In Android Emulator:
   - Clear app data for Sahal Wallet
   - Uninstall Sahal Wallet
   - Install fresh copy from Play Store

#### Step 6: Begin Capture
1. In Charles Proxy:
   - Start recording (File > Start Recording)
2. In Android Emulator:
   - Launch Sahal Wallet
   - Proceed through the account creation process
   - Note the exact sequence of actions

#### Step 7: Document API Calls
For each API call observed:
1. Note the timestamp
2. Record the endpoint URL
3. Document the HTTP method
4. Capture request headers and body
5. Capture response status, headers, and body
6. Note what was happening in the app at that moment

## API Call Documentation

### Template
```json
{
  "timestamp": "ISO-8601 timestamp",
  "endpoint": "API endpoint URL",
  "method": "HTTP method",
  "request": {
    "headers": {},
    "body": {}
  },
  "response": {
    "status": "HTTP status code",
    "headers": {},
    "body": {}
  },
  "context": "What was happening in the app when this call was made"
}
```

### Documented Calls
[API calls will be documented here as they are captured]

## Key Areas of Investigation

### 1. Initial Launch
- App configuration loading
- Feature flag checks
- Initial state setup

### 2. Account Creation
- Passcode creation process
- Wallet generation
- Account registration
- Device registration

### 3. Authentication
- Token management
- Session handling
- Security measures

### 4. Wallet Management
- Wallet creation
- Multiple wallet support
- Wallet synchronization

## Security Analysis
- Encryption methods used
- Sensitive data transmission
- Security headers and tokens
- Authentication flow

## Integration Points
- External service calls
- API dependencies
- Third-party integrations
- Webhook configurations

## Performance Metrics
- Response times
- Payload sizes
- Error rates
- Retry mechanisms

## Notes
- Keep API documentation up to date
- Document any rate limits or restrictions
- Note any API versioning
- Track API changes over time

## References
- [Android Studio](https://developer.android.com/studio)
- [Android Emulator](https://developer.android.com/studio/run/emulator)
- [Charles Proxy Documentation](https://www.charlesproxy.com/documentation/)
- [Android Network Security Config](https://developer.android.com/training/articles/security-config) 