# MQTT Experimental Project

This project demonstrates real-time communication between two websites using MQTT, Vite.js, Svelte, TypeScript, and Bun.

## Features

- Real-time messaging between two websites using MQTT.
- Modern UI with chat bubbles for sent and received messages.
- Built with Vite.js, Svelte, TypeScript, and Bun.

## Prerequisites

- [Bun](https://bun.sh/) (v1.0 or higher)
- [Node.js](https://nodejs.org/) (v18 or higher)

## Setup

1. Clone the repository:

```bash
git clone https://github.com/nanonymoussu/mqtt-experimental.git
cd mqtt-experimental
```

2. Install dependencies for both websites:

```bash
cd mqtt-1
bun install

cd ../mqtt-2
bun install
```

3. Run the development servers:

- For Website 1:

```bash
cd ../mqtt-1
bun run dev
```

- For Website 2:

```bash
cd ../mqtt-2
bun run dev
```

4. Open the websites in your browser

- Website 1: `http://localhost:5173`
- Website 2: `http://localhost:5174`

## Usage

- Send messages from Website 1 or Website 2.
- Messages will appear in real-time on both websites.
- "You" messages are displayed on the right side, and "Other" messages are displayed on the left side.

## License

This project is licensed under the MIT License. See the [LICENSE ](https://github.com/nanonymoussu/mqtt-experimental/blob/main/LICENSE)file for details.
