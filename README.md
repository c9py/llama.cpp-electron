# Nodejs-Llama Electron App

An Electron application that integrates with llama.cpp to process text prompts using LLM models. This project demonstrates how to create a Node.js native addon that interfaces with the llama.cpp library.

## Features

- Load LLM models through a user-friendly interface
- Process text prompts asynchronously in a separate thread
- Built with Electron for cross-platform compatibility
- Direct integration with llama.cpp via a Node.js addon

## Prerequisites

- Node.js (v16+)
- npm or yarn
- C++ compiler (GCC, Clang, or MSVC)
- CMake (for building llama.cpp)
- Git

## Installation

1. Clone this repository:
   ```
   git clone <your-repo-url>
   cd nodejs-llama
   ```

2. Install dependencies:
   ```
   npm install
   ```

3. Build llama.cpp (required before building the Node.js addon):
   ```
   cd llama.cpp
   mkdir build
   cd build
   cmake ..
   cmake --build . --config Release
   cd ../..
   ```

4. Build the Node.js addon:
   ```
   npm run build
   ```

5. Start the application:
   ```
   npm start
   ```

## How to Use

1. Launch the application
2. Click "Select Model" to choose a llama.cpp compatible model file (.bin or .gguf)
3. Enter a prompt in the text area
4. Click "Process Prompt" to analyze the text
5. View the results in the results section

## Model Files

You'll need to download LLM model files separately. Compatible models include:

- GGUF format models (recommended)
- Other formats supported by llama.cpp

You can download models from Hugging Face or other repositories. Place them in a location accessible by the application.

## Troubleshooting

- **Model loading errors**: Ensure your model file is compatible with llama.cpp
- **Addon building errors**: Make sure llama.cpp is properly built before building the addon
- **Performance issues**: Large models may require more memory and processing power

### Common Issues

1. **Cannot find llama.h**: Make sure you've built llama.cpp using the steps above
2. **Loading model fails**: Verify the model path is correct and the model is in a supported format
3. **Electron startup errors**: Check the terminal output for detailed error messages

## Project Structure

- `src/` - Main application source code
  - `addon/` - C++ Node.js addon code
  - `main.js` - Electron main process
  - `preload.js` - Preload script for IPC
  - `renderer.js` - Frontend logic
  - `index.html` - Main application UI
  - `styles.css` - Application styling
- `llama.cpp/` - Submodule for llama.cpp library

## License

This project is licensed under the ISC License - see the LICENSE file for details.

## Acknowledgments

- [llama.cpp](https://github.com/ggerganov/llama.cpp) - Inference engine
- [Electron](https://www.electronjs.org/) - Application framework
- [Node.js](https://nodejs.org/) - JavaScript runtime 