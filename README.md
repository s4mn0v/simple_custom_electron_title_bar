# simple_custom_electron_title_bar

A simple custom electron js title bar

## Main.js
```
// IPC event handlers
  ipcMain.handle('minimize', () => {
    mainWindow.minimize();
  });

  ipcMain.handle('maximize', () => {
    if (mainWindow.isMaximized()) {
      mainWindow.unmaximize();
    } else {
      mainWindow.maximize();
    }
  });

  ipcMain.handle('close', () => {
    mainWindow.close();
  });
```

## index.html
```
<div id="title-bar">
        <span>My Custom Title Bar</span>
        <div>
            <button id="minimize-btn">_</button>
            <button id="maximize-btn">[]</button>
            <button id="close-btn">X</button>
        </div>
    </div>
    <div id="content">
        <h1>Welcome to My Electron App!</h1>
        <p>Content goes here...</p>
    </div>
```

## renderer.js
```
document.getElementById('minimize-btn').addEventListener('click', () => {
    window.electron.minimize();
});

document.getElementById('maximize-btn').addEventListener('click', () => {
    window.electron.maximize();
});

document.getElementById('close-btn').addEventListener('click', () => {
    window.electron.close();
});
```

## preload.js
```
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electron', {
  minimize: () => ipcRenderer.invoke('minimize'),
  maximize: () => ipcRenderer.invoke('maximize'),
  close: () => ipcRenderer.invoke('close')
});

```