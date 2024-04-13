# Welcome to babel

The purpose of this project is to add support for creating a React interface for a Visual Basic 6 application. To achieve this, we utilize Ultralight as the HTML engine. However, since the engine operates exclusively in 64-bit, and VB6 only runs in 32-bit, we have developed a 32-bit API for VB6. This API spawns a 64-bit background process to execute the HTML and employs shared memory to handle communication.

### React.js interface
The interface used in Argentum Online is in the following repository: https://github.com/ao-org/ao-ui

# The solution is composed of 2 binaries:

### BabelUI.dll
This VB6 API module handles the spawning of child processes and converts all VB6 calls into events that can be processed by the background process.

### BabelSlave.exe
The main function of this executable is to render the HTML content. Additionally, it renders the current HTML state to a buffer and sends it to the API for display within the VB6 application. This process also manages the loading of game resources, allowing the HTML to display game content seamlessly.

![babelFlow](https://github.com/matiascalegaris/BabelUI/assets/28540132/1231cb3f-d6a0-4e4f-a9a9-469b3548e7f2)

1) VB6 uses the API interface to send any user interactions such as mouse movements, key presses, or any other actions required by the client logic from the UI, such as changing menus.
2) Since VB6 is single-threaded, we cannot directly call UI events. Instead, BabelApi stores all pending events from the UI (such as button presses). Then, VB6 requests an update from the main thread, and all callbacks are executed.
3) BabelApi converts all API calls to events and stores them in shared memory.
4) BabelSlave retrieves all events from the shared memory and calls the required functions in JavaScript, handle any js function callback and send it back to the api, it also load any resource required by the game like icons for the inventory


## Requirements:

1- Install Visual Studio 2019

Dependencies
![image](https://user-images.githubusercontent.com/5874806/236651340-da26785e-aa71-4d89-80d8-120182a1e410.png)


## Build:

"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe" BabelUi.sln -property:Configuration=Release
