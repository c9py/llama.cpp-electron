# Nodejs-Llama Architecture

This document explains the architecture of the Nodejs-Llama Electron application and how the components interact.

## Overview

The application integrates llama.cpp (a C++ implementation of LLM inference) with an Electron user interface through a Node.js native addon. This allows running LLM inference directly in the desktop application without requiring an external API.

## Component Architecture

### 1. Electron Application

- **Main Process** (`src/main.js`): Manages the application lifecycle, creates browser windows, and handles IPC communication with the renderer process.
- **Renderer Process** (`src/index.html`, `src/renderer.js`): Provides the user interface for selecting models, entering prompts, and displaying results.
- **Preload Script** (`src/preload.js`): Exposes a secure bridge between the renderer and main processes.

### 2. Node.js Native Addon

- **Addon Entry Point** (`src/addon/llama_addon.cpp`): Provides a JavaScript interface to the llama.cpp library.
- **Async Worker** (`LlamaWorker` class): Runs the LLM inference in a separate thread to avoid blocking the Node.js event loop.

### 3. llama.cpp Integration

- **Library Integration**: The native addon directly links to the llama.cpp library.
- **Model Loading**: The addon loads model files using llama.cpp's API.
- **Inference**: Text processing happens through llama.cpp's context and evaluation functions.

## Data Flow

1. **User Input**:
   - User selects a model file through the UI
   - User enters a prompt in the text area
   - User clicks "Process Prompt"

2. **Processing**:
   - The renderer process sends the model path and prompt to the main process via IPC
   - The main process calls the Node.js addon with these parameters
   - The addon creates an async worker to handle the operation in a separate thread
   - The worker loads the model and processes the prompt using llama.cpp
   - The result is passed back to the main process, then to the renderer for display

## Threading Model

- **Electron Main Thread**: Handles application logic and communication
- **Renderer Thread**: Manages UI interactions
- **Node.js Thread**: Handles JavaScript execution
- **Worker Thread**: Processes LLM operations via the native addon

## Key Design Decisions

1. **Using Node.js Native Addon**: Direct C++ integration allows for efficient memory management and better performance compared to spawning separate processes.

2. **Asynchronous Processing**: LLM inference can be computationally intensive, so all processing happens asynchronously to avoid UI freezing.

3. **Context Isolation**: The renderer process has no direct access to Node.js APIs for security reasons. All communication happens through the contextBridge.

4. **Standalone Operation**: The application includes all necessary components to run inference locally without external dependencies.

## Compilation Process

1. **llama.cpp**: Built as a static library using CMake
2. **Node.js Addon**: Compiled using node-gyp with direct references to llama.cpp headers
3. **Electron Application**: Packaged using electron-builder

## Extensibility

The architecture allows for several extension points:

- **Additional LLM Features**: The addon can be extended to support more llama.cpp features like token-by-token generation or different evaluation modes.
- **Model Management**: The application could be extended with model downloading, updating, or conversion features.
- **UI Enhancements**: The Electron-based UI can be enhanced with advanced features like chat interfaces, prompt templates, etc. 