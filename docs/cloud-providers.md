# Cloud Providers

maestro-runner runs Maestro YAML flows on cloud device providers via the Appium driver. You provide the provider's hub URL and a capabilities JSON file — no code changes needed.

## How it works

```
maestro-runner --driver appium --appium-url <HUB_URL> --caps caps.json test flow.yaml
```

- `--driver appium` — use the Appium driver
- `--appium-url` — the cloud provider's Appium hub endpoint
- `--caps` — JSON file with W3C capabilities (platform, device, app, provider credentials)

CLI flags (`--platform`, `--app-file`, `--device`) override values in the caps file.

## TestingBot

### Android

Create `testingbot-android.json`:

```json
{
  "platformName": "Android",
  "appium:deviceName": "Pixel 8",
  "appium:platformVersion": "14.0",
  "appium:app": "tb://app-id",
  "appium:automationName": "UiAutomator2",
  "tb:options": {
    "key": "YOUR_TESTINGBOT_KEY",
    "secret": "YOUR_TESTINGBOT_SECRET",
    "name": "Maestro Android test"
  }
}
```

Run:

```bash
maestro-runner --driver appium \
  --appium-url "https://hub.testingbot.com/wd/hub" \
  --caps testingbot-android.json \
  test flows/
```

### iOS

Create `testingbot-ios.json`:

```json
{
  "platformName": "iOS",
  "appium:deviceName": "iPhone 16",
  "appium:platformVersion": "18.0",
  "appium:app": "tb://app-id",
  "appium:automationName": "XCUITest",
  "tb:options": {
    "key": "YOUR_TESTINGBOT_KEY",
    "secret": "YOUR_TESTINGBOT_SECRET",
    "name": "Maestro iOS test",
    "realDevice": true
  }
}
```

Run:

```bash
maestro-runner --driver appium \
  --appium-url "https://hub.testingbot.com/wd/hub" \
  --caps testingbot-ios.json \
  test flows/
```

### Uploading your app

Upload your `.apk`, `.ipa`, or `.zip` to TestingBot before running tests:

```bash
curl -X POST "https://api.testingbot.com/v1/storage" \
  -u "YOUR_TESTINGBOT_KEY:YOUR_TESTINGBOT_SECRET" \
  -F "file=@/path/to/app.apk"
```

The response returns an `app_url` (e.g., `tb://app-id`) — use that as the `appium:app` value in your caps file.
