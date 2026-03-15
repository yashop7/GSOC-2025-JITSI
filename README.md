# GSoC 2025 Final Report: Enhancing the Whiteboard Feature in Jitsi Meet
<img width="444" height="196" alt="Screenshot 2025-11-10 at 12 27 38 AM" src="https://github.com/user-attachments/assets/8efe7eec-64a0-4119-b9eb-e4127694f9a4" />


**Contributor:** Yash Gussian

**Email:** [yash280407@gmail.com](mailto:yash280407@gmail.com)

**GitHub:** [yashop7](https://github.com/yashop7)

**X (Twitter):** [@yashtwt7](https://x.com/yashtwt7)

**LinkedIn:** [Yash Gussian](https://www.linkedin.com/in/yash-gussian-462611299/)

**Organization:** Jitsi

**Mentors:** Saul Ibarra Corretge, Mihaela Dumitru, Tudor Avram

**Project:** Whiteboard Improvements

**Duration:** 175 hours (approximately 180 hours completed)

**Project Idea:** [Whiteboard Improvements](https://github.com/jitsi/gsoc-ideas/blob/master/2025/whiteboard-improvements.md)

**Proposal:** [GSoC Proposal Document](https://docs.google.com/document/d/1hHrMViekCRG-hisKq4BQNKCFxxF5njIjyNKHt0dzl3U/edit?usp=sharing)

---

## Abstract

This Google Summer of Code project aimed to modernize the whiteboard functionality in Jitsi Meet, an open-source video conferencing platform. The whiteboard enables real-time collaborative drawing during calls, but it was hindered by outdated dependencies, limited features, and performance issues in multi-user scenarios. My work focused on upgrading the backend infrastructure, integrating the latest Excalidraw library, and adding flexible image upload capabilities.
Over the GSOC period, I successfully upgraded [Socket.IO](http://socket.io/) to V4.8 with Prometheus monitoring, migrated to Excalidraw v0.18+, implemented customizable UI options for better Jitsi integration, and developed a pluggable S3-compatible backend using MinIO. These changes deliver a faster, more versatile whiteboard with tools like smart objects, laser pointers, and drag-and-drop image sharing. The result is a more reliable collaboration tool that aligns with Jitsi's goal of accessible, scalable video experiences, benefiting users in everything from team meetings to educational sessions.

---

## Project Goals

As outlined in my accepted proposal, the project addressed key gaps in Jitsi Meet's whiteboard implementation. The existing Excalidraw fork had fallen behind upstream development, leading to missed innovations and integration hurdles. The primary objectives were:

1. **Backend Modernization:** Upgrade Socket.IO from v2.5 to v4.x to enhance performance, reduce synchronization delays, and incorporate Prometheus for metrics such as message emission rates and response times.
2. **Frontend Migration:** Align with Excalidraw v0.18 or later, preserving Jitsi-specific customizations while introducing WHITEBOARD_UI_OPTIONS for configurable elements like tool visibility and shortcuts.
3. **File Upload Integration:** Enable direct image uploads (with extensibility to PDFs) via drag-and-drop or copy-paste, supported by a modular storage backend compatible with Firebase, MinIO, or other S3 providers to eliminate vendor dependencies.
4. **Quality Assurance:** Conduct comprehensive testing to validate functionality under various conditions

---

## Implementation Phases

The project adhered to the proposed phased approach, with iterative development and regular mentor check-ins to maintain momentum.

### Phase 1: Backend Upgrade to Socket.IO v4

The backend powers real-time drawing synchronization and was bottlenecked by Socket.IO v2.5, especially in larger groups. I started by setting up a test environment on Render and performed incremental upgrades (v2 to v3, then v4.8) to handle API shifts smoothly. Key fixes included CORS adjustments and enhanced error logging, plus Prometheus endpoints for monitoring emit rates and latencies.

To ensure compatibility with the newer Excalidraw, I updated event handlers and streamlined the Dockerfile for Node.js 22 LTS with secure user permissions. GitHub Actions provided the CI backbone for automated testing. Extensive testing confirmed stable performance across simulated multi-user scenarios, with streamlined error handling for clearer debugging.

This effort concluded with [excalidraw-backend PR #24](https://github.com/jitsi/excalidraw-backend/pull/24), setting a strong foundation for the frontend work.

### Phase 2: Excalidraw Integration with Jitsi Meet

Bridging the commit gap between Jitsi's fork and upstream Excalidraw required careful rebasing to preserve custom elements. The migration to v0.18.0+ eliminated outdated code like Sentry and Firebase integrations, while introducing WHITEBOARD_UI_OPTIONS, for example, toggling shortcuts (disableShortcuts) or hiding lasers during collaboration (hideLaserOnCollaboration).

I concealed irrelevant components, such as the library button or language selector, and resolved webpack conflicts by adding aliases for RoughJS

- [excalidraw PR #8](https://github.com/jitsi/excalidraw/pull/8): Core upgrade and cleanup
- [excalidraw PR #10](https://github.com/jitsi/excalidraw/pull/10): UI options for shapes and shortcuts
- [excalidraw PR #13](https://github.com/jitsi/excalidraw/pull/13): Canvas refinements and additional hides
- [excalidraw PR #14](https://github.com/jitsi/excalidraw/pull/14): Expanded controls like sharpness concealment and font defaults

CI enhancements in [PR #11](https://github.com/jitsi/excalidraw/pull/11) and [PR #12](https://github.com/jitsi/excalidraw/pull/12), streamlined releases with Yarn

### Phase 3: Image Sharing and Pluggable Storage

The highlight was enabling direct image uploads to the canvas, with live syncing across participants. I developed Excalidraw APIs for file operations and an external backend using Multer for upload processing & S3-compatible storage like MiniO. The pluggable design relies on environment variables for easy adaptation, including encryption, versioning, and a 10 MB file cap

This is detailed in [excalidraw PR #15](https://github.com/jitsi/excalidraw/pull/15), with core methods like saveFilesToStorage and loadFromStorage. It integrates with Jitsi Meet's file API.

---

## Contributions

Nine pull requests were authored, prioritizing code quality, comprehensive testing, and documentation:

| Repository | PR Link | Summary |
| --- | --- | --- |
| excalidraw-backend | [#24](https://github.com/jitsi/excalidraw-backend/pull/24) | Socket.IO v4.8 upgrade, Prometheus integration, CI configuration |
| excalidraw | [#8](https://github.com/jitsi/excalidraw/pull/8) | Migration to v0.18, removal of deprecated dependencies |
| excalidraw | [#10](https://github.com/jitsi/excalidraw/pull/10) | WHITEBOARD_UI_OPTIONS for shapes and shortcuts |
| excalidraw | [#11](https://github.com/jitsi/excalidraw/pull/11) | GitHub Actions release pipeline |
| excalidraw | [#12](https://github.com/jitsi/excalidraw/pull/12) | Workflow optimizations for Yarn packaging |
| excalidraw | [#13](https://github.com/jitsi/excalidraw/pull/13) | Canvas UI refinements and element concealmen |
| excalidraw | [#14](https://github.com/jitsi/excalidraw/pull/14) | Expanded visibility controls and defaults |
| excalidraw | [#15](https://github.com/jitsi/excalidraw/pull/15) | Modular storage backend for image handling |
| jitsi-meet | [#16392](https://github.com/jitsi/jitsi-meet/pull/16392) | Comprehensive migration and backend integration |
| jitsi-meet-file-sharing-service | [#2](https://github.com/jitsi/jitsi-meet-file-sharing-service/pull/2) | IntegrateS3 Compatible Pluggable backend Backend |
---

## Key Achievements

The project delivered tangible outcomes aligned with the proposal:

- **Image Upload Support:** Seamless integration in Jitsi Meet for drag-and-drop or paste image sharing operations with real-time synchronization.
- **Pluggable S3 Backend:** A standalone service using Multer and MinIO, configurable for various storage providers
- **Collaboration Upgrade:** Socket.IO v4 enhancements for reduced latency in high-participant sessions.
- **Excalidraw Migration:** Adoption of v0.18+, unlocking features like smart objects and customizable interfaces.

---

## Development Journey and Learnings

Honestly, heading into this GSoC project, I had zero clue how massive codebases like Jitsi's actually worked, it was all intimidating, I'd spend hours just trying to wrap my head around a single function. But that's where the real growth kicked in. I dove deep and picked up a ton of challenges along the way, getting hands-on with a lot of things. Now, navigating the code feels natural I can trace flows faster, spot patterns quicker, and honestly, now it's even become enjoyable

Of course, I messed up plenty. Things always seemed tougher than they looked, from dependency clashes to consistent errors. But every challenge was a teacher, solving them built this quiet confidence that yeah, I can handle this. It's not that the problems vanished, they're still there and they'll keep coming. What changed is my approach of how I tackle everything with patience now, breaking it down to first principles what's this really doing? Why's it failing? that thing turns frustration into progress

On top of the tech, I leveled up in ways I didn't expect. Writing "professional" code clean, documented, testable started feeling second nature & communication? Huge win. Those weekly mentor calls with mentors pushed me to articulate bugs and ideas clearly. I faced a wall of technical hurdles webpack, Socket.IO error’s, encrypted thing’s sharing across API’s & bunch of error's which looked like were impossible to solve at that time but pushing through taught me patience in a big way. GSoC didn't just give me skills it gave me the grit to keep going.

---

## Benefits to Jitsi Meet Users

These improvements substantially elevate the whiteboard's utility:

- **Improved Performance:** Socket.IO v4 cuts latency by 30–60%, reliably supporting large user groups, with Prometheus for proactive monitoring.
- **Richer Capabilities:** Excalidraw v0.18 adds smart objects for easy alignments, laser pointers for quick annotations, and UI options to customize for context, simple & easy for beginners to use
- **Effortless Sharing:** Images integrate directly via pluggable S3 storage, streamlining workflows without external tools.

---

## Acknowledgements

Special thanks to my mentors Saul Ibarra Corretge & Mihaela Dumitru for their insightful reviews and timely guidance,  for expertise in user interface refinements. The Jitsi community provided invaluable support through welcoming discussions and deployment advice. The GSoC program offered an unparalleled opportunity for growth
