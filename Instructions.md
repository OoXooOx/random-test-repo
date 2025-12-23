# AWS MFA Token Generator

A simple Node.js script that generates time-based one-time passwords (TOTP) for AWS Multi-Factor Authentication (MFA).

## Description

This script provides a programmatic way to generate 6-digit MFA codes for AWS authentication, eliminating the need to manually check a mobile authenticator app. It uses the `speakeasy` library to create TOTP codes that are compatible with AWS MFA requirements.

## Prerequisites

- Node.js (v14 or higher)
- npm or yarn package manager

## Installation

1. Install the required dependency:

```bash
npm install speakeasy
```

or

```bash
yarn add speakeasy
```

## Usage

Run the script using Node.js:

```bash
node MFA.js
```

The script will output:
```
AWS MFA code: 123456
```

## Configuration

The script requires a secret key that matches your AWS MFA configuration:

```javascript
const secret = "6OCD";
```

**Important:** Replace `"6OCD"` with your actual MFA secret key from AWS.

### How to Find Your Secret Key

1. When setting up MFA in AWS, you'll be shown a QR code
2. Click "Show secret key" to reveal the base32-encoded secret
3. Copy this secret and replace the value in the script

## How It Works

The script generates TOTP codes using these standard parameters:

- **Secret**: Your MFA secret key (base32 encoded)
- **Digits**: 6-digit codes
- **Time Step**: 30 seconds (codes refresh every 30 seconds)
- **Algorithm**: SHA1 (standard for AWS MFA)

Each generated code is valid for 30 seconds and synchronized with AWS's authentication servers based on the current time.

## Code Explanation

1. Import the `speakeasy` library for TOTP generation
2. Define the secret key shared with AWS
3. Call `speakeasy.totp()` with AWS-compatible parameters
4. Output the generated 6-digit code to the console

The generated token changes every 30 seconds, matching the behavior of mobile authenticator apps like Google Authenticator or Authy.

## Security Notes

⚠️ **Important Security Considerations:**

- **Never commit your actual secret key to version control**
- Store the secret in environment variables or a secure configuration file
- Keep your secret key confidential - anyone with it can generate valid MFA codes
- Consider using `.gitignore` to exclude files containing secrets

### Recommended: Use Environment Variables

```javascript
import speakeasy from "speakeasy";
const secret = process.env.AWS_MFA_SECRET || "YOUR_SECRET_HERE";
const token = speakeasy.totp({
  secret,
  encoding: "base32",
  digits: 6,
  step: 30,
  algorithm: "sha1",
});
console.log("AWS MFA code:", token);
```

Then run:
```bash
AWS_MFA_SECRET="your_secret_here" node MFA.js
```

## Use Cases

- Automated AWS CLI authentication scripts
- CI/CD pipelines requiring MFA
- Development workflows needing frequent AWS access
- Quick MFA code generation without mobile device access

## License

This script is provided as-is for personal use.

