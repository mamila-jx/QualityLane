# GearLane: An Android DevOps Toolbox

A portable, reusable automation suite for Android projects using Fastlane and Bash scripts.

## Features

- **Automated Initialization**: Setup keys, runtime secrets, and config files in one go.
- **Build & Install**: Easily build debug/release versions and install them on connected devices.
- **Release Management**: Increment version codes, generate changelogs, and create git tags automatically.
- **Firebase Distribution**: Deploy APKs/AABs to Firebase App Distribution with automated release notes.
- **Secret Management**: Generate `RuntimeSecrets.kt` and other secret files (like `google-services.json`) from environment variables.

## Structure

```text
automation/
├── env/                # Configuration templates
├── fastlane/           # Fastlane lanes and Ruby utilities
├── script/             # Entry point Bash scripts
├── Gemfile             # Ruby dependencies
└── README.md           # This file
```

## Prerequisites

- **Ruby**: Version 2.6 or higher.
- **Bundler**: `gem install bundler`.
- **Fastlane**: Installed via the provided `Gemfile`.
- **Android SDK**: `adb` should be in your PATH.
- **Git Bash / WSL**: Required for Windows users to run `.sh` scripts.

## Installation

### 1. Add to your Project
The best way to use this toolbox is as a **Git Submodule**:

```bash
git submodule add https://github.com/your-username/android-toolbox.git automation
```

### 2. Run Setup
Initialize the environment and install dependencies:

```bash
bash automation/script/setup.sh
```

## Configuration

Customize the toolbox for your project by editing `automation/env/config.properties`.

Key configurations include:
- `ENVIRONMENT_KEYS`: Keys to sync from `local.properties` to `.env`.
- `RUNTIME_KEYS`: Keys to inject into `RuntimeSecrets.kt`.
- `SECRETS`: File-based secrets (e.g., Google Services JSON).
- `DEFAULT_MODULE`: Your main Android module (usually `app`).
- `DEPLOY_TESTERS`: Emails for Firebase distribution.

## Usage

Run the scripts from your **project root**:

| Script | Description |
| :--- | :--- |
| `bash automation/script/init_app.sh` | Full project initialization. |
| `bash automation/script/build.sh` | Build the application (`--debug`, `--release`, or `--flavour`). |
| `bash automation/script/install.sh` | Build and install on a device. |
| `bash automation/script/test.sh` | Run all unit tests. |
| `bash automation/script/release_candidate.sh` | Prepare a new RC build and optionally deploy. |
| `bash automation/script/deploy.sh` | Manual deploy of an existing artifact to Firebase. |

---

This project was created to help developers easily integrate automation into their Android apps, but mainly, it was done for fun. 

Created by [mamila](https://github.com/your-username).

Licensed under the [MIT License](LICENSE).
