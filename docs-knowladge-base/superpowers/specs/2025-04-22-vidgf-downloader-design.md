# VidGF Downloader Chrome Extension - Design Spec

**Date:** 2025-04-22
**Status:** Approved
**Version:** 1.0

## Overview

A simple Chrome extension that automatically detects and downloads videos from vidgf.com at the highest available quality, with browser notifications and automatic retry logic.

## Requirements

### Functional Requirements
- Automatically detect videos on vidgf.com pages
- Download videos immediately at highest quality
- Download all videos on a page simultaneously
- Display browser notifications when download starts and completes
- Automatically retry failed downloads (3 attempts with exponential backoff)
- Keep it simple - no configuration pages or settings

### Technical Requirements
- Chrome extension (Manifest V3)
- Content script injection for vidgf.com domain
- Chrome Downloads API
- Chrome Notifications API
- Support for HTML5 video elements

## Architecture

### Extension Structure
```
vidgf-downloader/
├── manifest.json           # Extension configuration
├── content.js             # Video detection and download logic
├── background.js          # Notification manager
├── notification.js        # Notification helper functions
└── icons/                 # Extension icons
    ├── icon16.png
    ├── icon48.png
    └── icon128.png
```

### Component Responsibilities

#### content.js (Main Detection Logic)
- Initialize MutationObserver on page load
- Detect `<video>` elements appearing in DOM
- Extract source URLs and parse for highest quality
- Trigger download via `chrome.runtime.sendMessage()`
- Handle retry logic with exponential backoff (2s, 4s, 8s)
- Track download state per video (pending, downloading, retrying, completed, failed)
- Report status to background script

#### background.js (Notification Manager)
- Listen for messages from content script
- Create and display browser notifications using `chrome.notifications` API
- Handle notification click events (optional: open download folder)
- Clean up completed notifications
- Manage extension lifecycle

#### notification.js (Notification Helpers)
- Format notification titles and messages
- Handle notification error states
- Manage notification IDs and cleanup

#### manifest.json (Extension Configuration)
- Content script matching: `["*://*.vidgf.com/*"]`
- Permissions: `["downloads", "notifications", "activeTab"]`
- Background service worker registration
- Icon configuration

### Data Flow

```
User visits vidgf.com
    ↓
Content script injects automatically
    ↓
MutationObserver watches for <video> elements
    ↓
Video element detected → Extract source URL → Find highest quality
    ↓
Trigger Chrome Downloads API → Download video file
    ↓
Background script sends notifications:
  - "Downloading [filename]..." (Yellow)
  - "Downloaded [filename]" (Green)
  - "Failed to download [filename]" (Red)
  - "Retrying (X/3): [filename]" (Orange)
```

### Error Handling

**Retry Mechanism:**
- Maximum 3 retry attempts per video download
- Exponential backoff: 2s, 4s, 8s between retries
- After 3 failed attempts: Mark as failed, show error notification, continue with other videos

**Error Scenarios:**

1. **Video URL not accessible**
   - Retry with full URL fetch
   - If still fails after 3 attempts, mark as failed

2. **Download interrupted**
   - Chrome Downloads API may auto-retry
   - Add our own retry logic on top

3. **Network timeout**
   - Retry after backoff period

4. **Invalid video format**
   - Skip and notify, no retry

5. **Browser permissions denied**
   - Show notification to grant permissions

**Notification States:**
- **Pending/Starting:** Yellow/Info icon
- **Success:** Green/Success icon
- **Failed:** Red/Error icon
- **Retrying:** Orange/Warning icon

**Multiple Videos Handling:**
- Array to track all active downloads
- Independent retry counters per video
- Concurrent downloads (no queuing)

## Implementation Details

### File Naming Convention
- Downloaded videos: Keep original filename from source URL if available
- Fallback: `vidgf_{timestamp}_{title}.{ext}`

### Icon Design
- Simple download arrow icon (↓)
- Clean, modern design
- 16px, 48px, 128px sizes
- Color scheme: Blue/white (or vidgf.com brand colors if available)

### Security Considerations
- Content script only injects into vidgf.com domain (Content Security Policy)
- Validate video URLs before downloading
- No external API calls
- No user data collection
- No data sent to external servers

### Performance Optimization
- Minimal impact on page load
- MutationObserver detached after all videos detected
- Cleanup event listeners when done
- No background processes when idle

## Testing Strategy

### Manual Testing Scenarios

1. **Basic Functionality**
   - Visit vidgf.com with single video → Verify automatic download
   - Verify start notification appears
   - Verify complete notification appears

2. **Multiple Videos**
   - Visit page with multiple videos
   - Verify all videos download simultaneously
   - Verify notifications for each video

3. **Error Handling**
   - Simulate network failure (disconnect internet)
   - Verify retry mechanism works (3 attempts)
   - Verify error notification after failed retries

4. **Video Formats**
   - Test with different video formats (mp4, webm, etc.)
   - Verify correct file extension saved

5. **Edge Cases**
   - Video with no source URL → Skip gracefully
   - Video that's already downloaded → Handle duplicate
   - Very large video file (>1GB) → Handle memory efficiently
   - Video with authentication/DRM protection → Show appropriate error

### Browser Compatibility
- Chrome (latest version)
- Edge (Chromium-based)
- Brave (if applicable)

## Success Criteria

- Extension automatically detects and downloads videos on vidgf.com
- User receives clear notifications for all download events
- Failed downloads retry automatically 3 times with backoff
- Multiple videos on the same page download simultaneously
- Extension is simple to install and use (no configuration needed)
- Extension doesn't impact browser performance

## Limitations

- Only works on vidgf.com domain
- May not work with streaming protocols or DRM-protected content
- Requires internet connection for download
- Browser notifications must be enabled

## Future Enhancements (Not in v1.0)

- Support for additional video platforms
- Settings page for user customization
- Download queue management
- Video quality selection
- Download history
- Batch download from list of URLs
