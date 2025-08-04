# BrainLoop

### Connect. Create. Collaborate.

BrainLoop is a real-time collaborative text editor built with Node.js and React. This application allows multiple users to edit a single document simultaneously, with changes instantly synchronized across all connected clients.

## Features

- **Real-time Collaboration:** Multiple users can edit the same document at the same time.
- **WebSocket Communication:** Utilizes WebSockets for efficient, low-latency communication between the client and server.
- **User Authentication:** Simple username-based entry to join a session.
- **Active User Display:** See who else is currently editing the document with unique user avatars.
- **Diffing Algorithm:** Sends only the changed lines to the server, optimizing network usage.
- **State Management:** Uses React hooks (`useState`, `useEffect`, `useRef`) for clean and predictable state handling.

## Project Structure

The project is divided into two main parts: a `Backend` and a `Frontend`.

/
├── Backend/
│   ├── node_modules/
│   ├── index.js
│   ├── package.json
│   └── package-lock.json
└── Frontend/
├── node_modules/
├── public/
│   ├── Favicon.jpeg
│   └── index.html
├── src/
│   ├── App.js
│   ├── index.css
│   └── index.js
├── package.json
└── package-lock.json


- **Backend:**
  - `index.js`: The main server file that handles WebSocket connections, user management, and document synchronization.
- **Frontend:**
  - `src/App.js`: The primary React component that contains the application's logic, including state management, WebSocket communication, and UI rendering.
  - `src/index.css`: Contains the styling for the application.
  - `public/index.html`: The main HTML file that serves the React application.

## Getting Started

Follow these instructions to set up and run the project locally.

### Prerequisites

- Node.js (version 14 or higher recommended)
- npm (Node Package Manager)

### Installation

1.  **Clone the repository:**
    ```sh
    git clone <repository_url>
    cd <repository_name>
    ```

2.  **Install backend dependencies:**
    ```sh
    cd Backend
    npm install
    ```

3.  **Install frontend dependencies:**
    ```sh
    cd ../Frontend
    npm install
    ```

### Running the Application

1.  **Start the backend server:**
    Open a terminal and navigate to the `Backend` directory.
    ```sh
    cd Backend
    node index.js
    ```
    The server will start on `http://localhost:8080`.

2.  **Start the frontend development server:**
    Open a new terminal and navigate to the `Frontend` directory.
    ```sh
    cd Frontend
    npm start
    ```
    This will launch the application in your browser, typically at `http://localhost:3000`.

## How it Works

### Frontend (`src/App.js`)

- **State Management:** Uses `useState` hooks to manage the document content, user ID, and active users.
- **WebSocket Connection:** The `useEffect` hook establishes a connection to the backend WebSocket server on component mount. It handles incoming messages for document updates, user list changes, and username approval/rejection.
- **Document Synchronization:** When a user types in the `textarea`, the `handleDocumentChange` function is triggered. It calculates the `diff` (changed lines) between the old and new content and sends this minimal update to the server.
- **User Interface:** The UI conditionally renders either the username entry screen or the main collaborative editor based on the `isUsernameApproved` state.

### Backend (`Backend/index.js`)

- **WebSocket Server:** Creates a WebSocket server that listens for incoming client connections.
- **Message Handling:** The server listens for specific message types:
  - `checkUsername`: Validates if a chosen username is unique.
  - `edit`: Receives document edits from a client and broadcasts the new document content to all connected users.
- **Document State:** The server maintains the single source of truth for the document content and the list of active users.
- **Broadcasting:** When a change occurs, the server broadcasts a `documentUpdate` or `userListUpdate` message to all connected clients, ensuring everyone's view is synchronized.

## Technologies Used

- **Frontend:**
  - React
  - HTML
  - CSS
- **Backend:**
  - Node.js
  - ws (WebSocket library)
