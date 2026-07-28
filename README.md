> **Note:** Still editing
# Immich Memories Blueprint

![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Blueprint-blue)
![Immich](https://img.shields.io/badge/Immich-v3.0_Compatible-5B00FF)
![AI Assisted](https://img.shields.io/badge/Developed%20with-AI-brightgreen)

An enhanced Home Assistant automation blueprint that sends daily push notifications featuring memories from your Immich photo server on the same calendar day in past years.

> **Credit & AI Disclosure:**  
> This project is a feature-rich fork based on the original blueprint created by @zolakt.  
> The rewrite, advanced Jinja2 templating, multi-language support, error handling, and overall logic were almost entirely developed with the assistance of AI.

---

## Key Enhancements in v3.0

Compared to the original release, this version brings significant stability fixes and new features:

- **Multi-Device Support:** Send notifications to multiple phones simultaneously with dynamic target resolution.
- **Random Delay:** Introduces a configurable random time delay before sending notifications to make reminders feel more natural.
- **Multi-Language Support (i18n):** Native message templates for 10 languages (PT, EN, ES, DE, FR, NL, PL, IT, JA, ZH) with local grammar rules. Includes a dedicated `immich_memories_pt.yaml` file pre-configured for Portuguese users.
- **iOS & Android Image Compatibility:** Thumbnail URLs are automatically converted to JPEG with size=preview, ensuring high-resolution rendering across all platforms (including iOS HEIC images).
- **Robust Error Handling:** Advanced JSON parsing prevents crashes (e.g., fixing KeyError: 0) if the API returns unexpected data or authentication errors.
- **Optional Empty Day Notifications:** Toggleable notifications to let you know when no memories exist for today.

---

## Compatibility & Requirements

1. Tested on Immich v3.0 and Home Assistant 2024.x+.
2. Immich API Key with the following permissions:
   - `memory.read`
   - `asset.view`
3. Manual addition of `rest_command` in Home Assistant.

---

## Installation

### Step 1: Add REST Command

Add the exact snippet below to your `configuration.yaml` file. Do not change `{{ api_url }}` or `{{ api_key }}`—these are variables filled dynamically by the blueprint during execution.

```yaml
rest_command:
  immich_get_memories:
    url: "{{ api_url }}/api/memories?for={{ now().strftime('%Y-%m-%dT00:00:00.000Z') }}"
    method: GET
    headers:
      Accept: "application/json"
      x-api-key: "{{ api_key }}"
```

> **Note:** Save the file and restart Home Assistant (or reload REST Commands from Developer Tools -> YAML).

### Step 2: Import the Blueprint

Click one of the buttons below to import your preferred version directly into Home Assistant:

**Default (English / Multi-language UI):**

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fsodpt%2Fimmich-memories-ha-blueprint%2Fblob%2Fmain%2Fimmich-memories.yaml)

**Versão em Português (`immich_memories_pt.yaml`):**

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fsodpt%2Fimmich-memories-ha-blueprint%2Fblob%2Fmain%2Fimmich-memories_pt.yaml)

Alternatively, manually copy and paste one of the URLs below in **Settings -> Automations & Scenes -> Blueprints -> Import Blueprint**:
- `https://github.com/sodpt/immich-memories-ha-blueprint/blob/main/immich-memories.yaml`
- `https://github.com/sodpt/immich-memories-ha-blueprint/blob/main/immich-memories_pt.yaml`

---

## Configuration Reference

| Input | Description | Default |
|---|---|---|
| **Immich API URL** | Base URL of your Immich server (e.g., `http://192.168.1.50:2283`) | Required |
| **Immich API Key** | Immich API Key with proper permissions | Required |
| **Immich UI URL** | Choose between Android App, iOS App, custom URL, or default API URL | Android App |
| **Notify Targets** | Select one or multiple mobile devices to receive notifications | Required |
| **Time of Day** | Base time to trigger the daily check | `09:00:00` |
| **Max Random Delay** | Maximum delay (in minutes) after trigger time to send notification | `0` (Disabled) |
| **Language** | Select your preferred notification language | English |
| **Include Thumbnail** | Attach image preview to push notification | `true` |
| **Memory Selection** | Mode to pick memory: `random`, `newest`, or `oldest` | `random` |
| **Asset Selection** | Mode to pick image within memory: `random`, `newest`, or `oldest` | `random` |
| **Notify on No Memories** | Send a fallback message if no memories exist for today | `false` |

---

## Supported Languages (i18n)

The blueprint includes built-in localized templates for:
- Portuguese (also available as standalone `immich_memories_pt.yaml`)
- English
- Spanish
- German
- French
- Dutch
- Polish
- Italian
- Japanese
- Chinese

---

## Troubleshooting

- **KeyError / Crashes:** Ensure your server is running Immich v3.0+. If you receive authentication errors, verify that your API key has both `memory.read` and `asset.view` scopes.
- **No Notifications Received:** Verify that the `rest_command` runs without errors in **Developer Tools -> Actions**, ensure your target mobile app devices are properly selected, and check your Home Assistant system logs (`home-assistant.log`).
- **Thumbnails Missing on iOS:** The blueprint automatically forces JPEG preview conversion (`size=preview`). If thumbnails fail to load, ensure your Home Assistant mobile app has network access to your Immich URL.

---

## Security & Privacy

- **API Keys:** Never hardcode your API key in public configuration files. When configuring the automation via the Home Assistant UI, the key is saved safely in your local automation setup.
- **Permissions:** The API key only requires read-only scopes (`memory.read`, `asset.view`). No write or delete permissions are required.

---

## Contributing & License

Contributions, translations, and bug reports are welcome! Feel free to open an issue or submit a Pull Request on GitHub.

This project is licensed under the MIT License.

---

## Credits

- Special thanks to @zolakt for creating the original Immich memories concept and initial blueprint layout.
- Designed and built with AI collaboration.
