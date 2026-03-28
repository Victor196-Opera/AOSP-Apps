# Email & Calendar Patched APK

## Introduction

This is a patched version of the built-in Email and Calendar applications for Android. The patches fix various crash issues related to Contacts permission and array index out-of-bounds errors.

## Fixed Issues

### 1. Calendar App
- **Error**: `ArrayIndexOutOfBoundsException` in `TimezoneAdapter`
- **Cause**: Mismatch between `timezone_ids` and `timezone_labels` array lengths
- **Fix**: Added index validation before accessing array elements

### 2. Email App
Fixed all SecurityException crashes related to ContactsProvider access:

| Class | Method | Fix Description |
|-------|--------|-----------------|
| `AccountSetupFinal$OwnerNameLoaderCallbacks` | `onCreateLoader` | Return null, set default name "User" |
| `SenderInfoLoader` | `loadInBackground` | Return empty map instead of querying contacts |
| `ContactResolver$ContactResolverTask` | `doInBackground` | Skip contact photo loading |
| `SuggestionsProvider$ContactsCursor` | `query` | Added try-catch, return empty cursor |
| `SuggestionsProvider` | `query` | Added try-catch for entire query |
| `MaterialSearchSuggestionsList$QuerySuggestionsTask` | `doInBackground` | Return empty list on error |

## Features

- Account setup completes successfully
- Read and send emails normally
- Search functionality works without crash
- Calendar event creation works
- Timezone selection works
- No Contacts permission required

## System Requirements

- Android 5.0 (API 21) or higher
- Root access if replacing system apps
- Can be installed as normal APK

## Installation

### Method 1: Install as Normal App
1. Download APK from [Releases](../../releases)
2. Uninstall original app if exists
3. Install the patched APK

### Method 2: Replace System App (requires root)

```bash
adb root
adb remount
adb push Email_patched.apk /system/priv-app/Email/Email.apk
adb push Calendar_patched.apk /system/app/Calendar/Calendar.apk
adb shell chmod 644 /system/priv-app/Email/Email.apk
adb shell chmod 644 /system/app/Calendar/Calendar.apk
adb reboot
```

## Important Notes

· No Contacts permission needed for these patched versions
· Default sender name is set to "User"
· Contact photos will not display (prevents crashes)
· Search suggestions from contacts will not appear

## How to Verify Email is Sent

When composing and sending an email, a toast message "Sending message..." will appear, then fade away. This indicates the email has been sent successfully. If an error occurs, a notification will be displayed.

## Reporting Issues

If you encounter any problems:

· Email: bvaundy1999@gmail.com
· Or create an Issue on GitHub

## Please include:

1. Device model and Android version
2. Logcat output if available
3. Steps to reproduce the issue

## Instructions for Manual Patching

If you want to patch the APK yourself:

1. Decompile the APK using MT Manager or apktool
2. Locate the classes listed in the "Fixed Issues" section
3. Apply the try-catch patterns as shown in the patches folder
4. Recompile and sign the APK
5. Install and test

The patches folder contains the modified smali files for reference.
