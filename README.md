# Electron

## [Quick Start](https://www.electronjs.org/docs/latest/tutorial/quick-start#open-a-window-if-none-are-open-macos)

1. Check `Node.js` Installed `node -v && npm -v`
2. Create Application : Creating a folder and initialzing an npm package

```bash
    $ mkdir my-app && cd my-app
    $ npm init

    # entry point should be main.js

    $ npm install --save-dev electron
```

- App Root -> Create `index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.
  </body>
</html>
```

- App Root -> Create `main.js`
```js
// main.js

// Modules to control application life and create native browser window
const { app, BrowserWindow } = require('electron')
const path = require('path')

const createWindow = () => {
  // Create the browser window.
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  // and load the index.html of the app.
  mainWindow.loadFile('index.html')

  // Open the DevTools.
  // mainWindow.webContents.openDevTools()
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.whenReady().then(() => {
  createWindow()

  app.on('activate', () => {
    // On macOS it's common to re-create a window in the app when the
    // dock icon is clicked and there are no other windows open.
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})

// Quit when all windows are closed, except on macOS. There, it's common
// for applications and their menu bar to stay active until the user quits
// explicitly with Cmd + Q.
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit()
})

// In this file you can include the rest of your app's specific main process
// code. You can also put them in separate files and require them here.
```

- Package and distribute my application

```bash
    $ npm install --save-dev @electron-forge/cli
    $ npx electron-forge import

    $ my-electron-app@1.0.0 make /my-electron-app
    $ electron-forge make
```

- `package.json` should look something like

```json
{
  "name": "demo-app",
  "version": "1.0.0",
  "description": "Demo",
  "main": "main.js",
  "author": "Kim Bum Jun",
  "license": "MIT",
  "devDependencies": {
    "@electron-forge/cli": "^6.1.1",
    "@electron-forge/maker-deb": "^6.1.1",
    "@electron-forge/maker-rpm": "^6.1.1",
    "@electron-forge/maker-squirrel": "^6.1.1",
    "@electron-forge/maker-zip": "^6.1.1",
    "electron": "^24.3.1"
  },
  "scripts": {
    "start": "electron-forge start",
    "package": "electron-forge package",
    "make": "electron-forge make"
  },
  "dependencies": {
    "electron-squirrel-startup": "^1.0.0"
  }
}
```

3. Finally Open App Development mode Start-> `$ npm start`

---

