# InHotel Chat Window

This repository contains the InHotel Chat Widget - a customizable chat interface that communicates with InHotel AI agents using the A2A (Agent-to-Agent) protocol.

## Features

- **A2A Protocol Integration**: Real-time streaming responses via Server-Sent Events
- **Customizable UI**: Colors, titles, avatars, and branding
- **Session Persistence**: Chat history saved in localStorage
- **Mobile Responsive**: Fullscreen mode on mobile devices
- **Contact Form**: Built-in email capture and summarization

## Getting Started

### Prerequisites

- Node.js 16+
- npm

### Installation

```bash
npm install
```

### Development

Run the development server:

```bash
npm run dev
```

The app starts at [http://localhost:3000](http://localhost:3000).

### Testing the Widget

Open `demo.html` in your browser to test the widget with a specific assistant:

```html
<script>
  ASSISTANT_INTERACTION_PROFILE_ID = 'your-assistant-id-here';
</script>
<script
  type="text/javascript"
  async
  defer
  src="http://localhost:3000/widget.js"
></script>
```

## Building

To build the production widget:

```bash
npm run build:widget
```

This generates the bundled `public/widget.js` file.

## Configuration

The widget accepts configuration through `window.InhotelChatWindow.config`:

```javascript
window.InhotelChatWindow = {
  config: {
    title: 'Chat with us',
    subtitle: 'We typically reply within minutes',
    primaryColor: '#73a55f',
    greeting: 'Hello! How can I help you today?',
    newMessagePlaceholder: 'Type your message...',
    position: 'right', // or 'left'
    // ... more options
  },
};
```

## Architecture

- **A2A Client** (`helpers/a2a-client.ts`): Handles JSON-RPC 2.0 over SSE communication
- **Storage** (`helpers/storage.ts`): Manages session persistence in localStorage
- **ChatWindow** (`components/ChatWindow.tsx`): Main chat component
- **Widget** (`components/Widget.tsx`): Widget container and toggle button

## Deployment

`chat.inhotel.io` serves `/widget.js` and the iframe page it opens. It runs on GKE as Deployment `inhotel-chat-window` in namespace `inhtoel-agent-app` on cluster `inhotel-agent` (project `inhotel-prod`, `europe-west6`).

[`.github/workflows/ci-cd.yml`](.github/workflows/ci-cd.yml) builds every pull request and every push to `main`. On `main` it then pushes `gcr.io/inhotel-prod/inhotel-chat-window:<commit SHA>` and rolls the Deployment onto it. It signs in to GCP through Workload Identity Federation, configured by the repository variables `GCP_WIF_PROVIDER` and `GCP_SERVICE_ACCOUNT`; no key is stored. The manifests in `k8s/` were applied once by hand, and the pipeline changes only the image.

## License

Proprietary - InHotel.io
