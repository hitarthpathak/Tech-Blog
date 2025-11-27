A **Browser Extension** is a software module for customizing a web browser. Browsers typically allow users to install a variety of extensions, including **user interface modifications**, **cookie management**, **ad blocking**, and the custom **scripting and styling of web pages**.
As of August 2025, the **Chrome Web Store** hosts approximately 111,933 extensions. This number fluctuates as Google adds new extensions and removes inactive or policy-violating ones.

-------------------------------------------------

My chrome extension is called **WebSnap**. It allows users to take **Screenshot** and **Screen Recording** on a web page. In this article, I will guide you on how you can make your own chrome extension through the reference of WebSnap.

-------------------------------------------------

To start building your Chrome Extension, make sure you have basic knowledge of **HTML**, **CSS**, and **JavaScript**. Also, have **Google Chrome** installed along with a code editor like **Visual Studio Code**.

-------------------------------------------------

**UNDERSTAND THE CORE FILES :-**

<!DOCTYPE>

    WebSnap/
    ├── popup.html
    ├── popup.css
    ├── popup.js
    ├── manifest.json
    └── icons/
        ├── Icon.png

</html>

- **popup.html :** The popup shown when clicking the extension icon.
- **popup.css :** Styling for your popup interface.
- **popup.js :** The logic controlling the popup’s behavior, such as taking screenshots or starting screen recording.
- **manifest.json :** This file describes your extension’s metadata such as its name, version, permissions, and which files it uses.

**popup.html :-**

<!DOCTYPE html>

    <html>

        <head>

            <title>WebSnap</title>

            <link rel="stylesheet" href="popup.css">

        </head>

        <body>

            <h1>WebSnap</h1>

            <hr>

            <div class="button-box">

                <button id="screenshot-button">Take Screenshot</button>

                <button id="screenrecord-button">Start Screen Recording</button>

            </div>

            <hr>

            <a href="https://hitarthpathak.github.io/" target="_blank">

                <p>Hitarth Pathak</p>

            </a>

        </body>

        <script src="popup.js"></script>

    </html>

</html>

**popup.css :-**

<!DOCTYPE html>

        * {
            font-family: 'Times New Roman';
            text-align: center;
        }
        
        body {
            height: 50rem;
            width: 50rem;
            padding: 1rem;
        }
        
        .button-box {
            height: auto;
            width: auto;
        }
        
        button {
            border: none;
            height: auto;
            width: auto;
            padding: 1rem;
            background-color: rgb(50, 150, 50);
            color: white;
            cursor: pointer;
        }
        
        button:hover {
            background-color: green;
        }
        
</html>

**popup.js :-**

<!DOCTYPE html>

    let screenshot_button = document.getElementById("screenshot-button");
    let screenrecording_button = document.getElementById("screenrecording-button");

    // ------------------------------------------------------------------------------------------------

    screenshot_button.addEventListener("click", () => {

        chrome.tabs.captureVisibleTab(null, { format: 'png' }, function (dataUrl) {

            let link = document.createElement('a');
            link.href = dataUrl;
            link.download = 'screenshot.png';
            link.click();

        });

    });

    // ------------------------------------------------------------------------------------------------

    let media_recorder;
    let recorded_chunks = [];
    let screen_stream;

    screenrecording_button.addEventListener("click", async () => {

        if (media_recorder && media_recorder.state === "recording") {

            media_recorder.stop();
            screen_stream.getTracks().forEach((track) => track.stop());

            screenrecording_button.textContent = "Start Screen Recording";
            return;
        }

        try {

            screen_stream = await navigator.mediaDevices.getDisplayMedia({ video: true, audio: true });

            media_recorder = new MediaRecorder(screen_stream);

            media_recorder.ondataavailable = (event) => {

                recorded_chunks.push(event.data);

            };

            media_recorder.onstop = () => {

                let blob = new Blob(recorded_chunks, { type: "video/webm" });
                let video_URL = URL.createObjectURL(blob);

                recorded_chunks = [];

                let download_link = document.createElement("a");
                download_link.href = video_URL;
                download_link.download = "screen_recording.webm";
                download_link.click();

            };

            media_recorder.start();
            screenrecording_button.textContent = "Stop Recording";

        } catch (err) {
    
            console.error("Error starting screen recording:", err.name || err.message);
    
        }

    });

</html>

**manifest.json :-**

<!DOCTYPE html>

    {
    
        "manifest_version": 3,
        "name": "WebSnap",
        "version": "1.0",
        "description": "A Chrome Extension To Take Screenshots",
        "permissions": [
            "activeTab",
            "storage",
            "tabs",
            "desktopCapture"
        ],
        "action": {
            "default_popup": "popup.html",
            "default_icon": {
                "16": "icons/Icon.png",
                "48": "icons/Icon.png",
                "128": "icons/Icon.png"
            }
        },
        "icons": {
            "16": "icons/Icon.png",
            "48": "icons/Icon.png",
            "128": "icons/Icon.png"
        },
        "host_permissions": [
            "http://*/*",
            "https://*/*"
        ]
        
    }

</html>

Your JavaScript uses the **Chrome API** to capture screenshots via chrome.tabs.captureVisibleTab and manages screen recordings with the MediaRecorder API. When a user stops recording, the extension handles media streams and saves the recording as a downloadable file.

-------------------------------------------------

**TESTING THE EXTENSION :-**

To test your extension, load it in Chrome via the **Extensions Page** (chrome://extensions). First enable **Developer Mode** (top right) and then upload your project folder by selecting **Load Unpacked** (top left).
To make it work in Incognito Mode, go to **Details** and then check **Allow In Incognito**.

-------------------------------------------------

**PUBLISHING THE EXTENSION :-**

To publish your extension, visit chrome web store (chromewebstore.google.com). Follow this link for more guidance — [https://developer.chrome.com/docs/webstore/publish](https://developer.chrome.com/docs/webstore/publish)

-------------------------------------------------

**BEST PRACTICES :-**

- Only request permissions that your extension truly needs.
- Keep **User Interface (UI)** simple.
- Ensure error handling for a smooth **User Experience (UX)**.
- Always test thoroughly using **Chrome DevTools** and reload after updates.
