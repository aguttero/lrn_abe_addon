# framework
2026 10
Node lts: v24.22.1
npm v11.19.1
nvm v0.40.4

## requirements 2026 10
- Node + npm / NodeJs v18+
- Adobe Express account

## nvm commands
sh: nvm --help
sh: nvm ls (list available versions)
sh: nvm uninstall <version>
sh: nvm install --lts
sh: nvm use --lts

## config folder
`$NVM_DIR` folder (usually `~/.nvm`)



## YouTube link
https://www.youtube.com/watch?v=kSq4ykQGOdo

## documentation
https://developer.adobe.com/express/add-ons/docs/guides/getting-started/developer-journey
https://developer.adobe.com/express/add-ons/docs/guides/getting-started/hello-world

### Code Playground
https://www.adobe.com/go/addon-playground
https://developer.adobe.com/express/add-ons/docs/guides/getting-started/hello-world

### How to guides and available APIs
https://developer.adobe.com/express/add-ons/docs/guides/learn/how-to/

### Complete project tutorials
https://developer.adobe.com/express/add-ons/docs/guides/learn/how-to/#complete-projects

## distribution
https://developer.adobe.com/express/add-ons/docs/guides/build/distribute/
- submission guidelines
- private or public listing
- publisher profile

## Hello world example
1. Setup local web dev environment:
- sh: npx @adobe/create-ccweb-add-on hello-world
- sh: npx @adobe/create-ccweb-add-on hello-world --template javascript-with-document-sandbox

- ignore warnings
- analytics: no
- start with javascript
- document sandbox: yes
- trusted SSL : Automatcally - Set up for me
- see ### success log
- cd to hello-world folder
- sh: npm run build
- sh: npm run start
- log: Done. Your add-on 'hello-world' is hosted on: https://localhost:5241
- log: Continue testing your add-on in Adobe Express by accessing the link: https://www.adobe.com/go/addon-cli

2. sideload project to Adobe Express
- Enable dev mode in Abe Express:
- Login > Settings > Add-ons or Developer Section > turn-on Add-obn Development

3. Connect your add-on to a project
- create a new project
- click add-ons
- click Your add-ons tab
- on Add-on Development section > turn on Add-on testing
- add dev server url from terminal log > connect
- restart chrome if not working need to accept open apps dialog box
 

### success log:
Success! Created hello-world at /Users/alejandroguttero/code_imac/lrn_abe_addon/hello-world.
Inside this directory, you can run the following commands:

  npm run build > Builds the Add-on.
  npm run start > Starts the development server and hosts the Add-on.

We suggest that you begin by typing:

  cd hello-world
  npm run build
  npm run start

### document sandbox option explanation
Adobe Express uses a secure architecture divided into two parts:
1. The UI (User Interface): This handles your sidebar panel, HTML, buttons, and CSS.
2. The Document Sandbox: This is a separate, highly secure container that has permission to actually change the user's canvas (e.g., adding text, shapes, or images to the document).
By choosing Yes, Adobe's creation tool will automatically set up the link between your sidebar UI and this secure sandbox. Without it, your add-on would just be a pretty sidebar panel that cannot actually touch or edit the Adobe Express document.

### SSL certificate explanation
You can find the SSL certificate in /Users/alejandroguttero/Library/Application Support/Adobe/CCWebAddOn/devcert/domains/localhost.

Adobe Express is a highly secure web platform that runs entirely over https:// (encrypted web traffic). Because your add-on needs to load safely inside the Adobe Express website while you are coding, your computer's local testing server must also use https://localhost.
By choosing Automatically, the Adobe tool will instantly generate a safe, local security certificate for your computer. Doing this manually is a massive headache involving encryption keys, so letting the tool automate it saves you a ton of time.

### bug fix gemini notes:
If you can see a response or a blank page on https://localhost:9000 but Adobe Express still shows that error, it means the browser accepted the connection for that tab, but it is still blocking Adobe Express from loading your local code inside an <iframe> (a window within a window) due to strict security rules. [1] (https://medium.com/adobetech/important-cross-origin-isolation-update-for-adobe-express-add-on-developers-8404940e4d43)
Let's force the connection through using these two reliable fixes.

Fix 1: The Chrome Flags Trick (Highly Recommended)

If you are using Google Chrome or a Chromium-based browser (like Edge or Brave), you can tell the browser to permanently trust localhost certificates for development purposes.
1. Open a brand new tab and paste this into your address bar:text
chrome://flags/#allow-insecure-localhost
Usa el código con precaución.
2. Look for the highlighted setting called "Allow invalid certificates for resources loaded from localhost."
3. Change the dropdown menu from Disabled to Enabled.
4. Click the Relaunch button that appears at the bottom right of your browser window to restart Chrome.
Once Chrome restarts, go back to Adobe Express and click connect. It should bypass the security block instantly!

Fix 2: Disable Strict Tracking Protection / Ad Blockers

Adobe Express relies on cross-origin requests to load your add-on. Sometimes, aggressive privacy tools treat localhost traffic as a tracking script and block it completely. [1] (https://community.adobe.com/announcements-327/fixed-adobe-express-outage-my-stuff-not-loading-something-went-wrong-march-27th-2026-1555383), [2] (https://www.reddit.com/r/AdobeExpress/comments/1lgvvch/addon_development_problem/), [3] (https://medium.com/adobetech/important-cross-origin-isolation-update-for-adobe-express-add-on-developers-8404940e4d43), [4] (https://community.adobe.com/questions-329/express-fails-to-load-or-create-project-is-it-cors-issues-1225876)
1. Look at your browser address bar while on the Adobe Express tab.
2. If you use Brave, turn off the Brave Shields (the lion icon) for the Adobe Express site.
3. If you use Chrome/Firefox, look for an ad blocker (like uBlock Origin) or tracking blocker. Click it and pause it or turn it off for Adobe Express.
4. Refresh your Adobe Express page and try connecting again.

Fix 3: Test via Incognito Mode

If the steps above still don't clear it, an aggressive browser extension might be interfering with your network requests. [1] (https://community.adobe.com/questions-329/adobe-express-not-loading-2024-solved-1221789)
1. Open a new Incognito / Private Window.
2. First, go to https://localhost:9000 and click "Advanced -> Proceed" if it asks you to.
3. In that same Incognito window, open express.adobe.com, log in, and try adding your add-on. [1] (https://community.adobe.com/questions-329/adobe-express-not-loading-2024-solved-1221789), [2] (https://community.adobe.com/announcements-327/fixed-adobe-express-outage-my-stuff-not-loading-something-went-wrong-march-27th-2026-1555383), [3] (https://www.reddit.com/r/AdobeExpress/comments/1lgvvch/addon_development_problem/)
Give Fix 1 a try first, as it solves this problem for 95% of developers!
Let me know:
• Did enabling that Chrome flag get the add-on to load?
• Which web browser are you currently using?

## Basic add-on structure
Every Adobe Express add-on has these essential files:

* manifest.json - Tells Adobe Express about your add-on
* index.html - Your add-on's user interface
* index.js - Your add-on's logic

* The simplest add-on
Plain JavaScript Template (No Build)
This is the absolute simplest structure - no build tools, no document manipulation:

my-addon/
├── src/
│   ├── index.html       # Your UI
│   ├── index.js         # Your UI logic
│   └── manifest.json    # Configuration
├── tsconfig.json        # For IDE support only
└── README.md


* The Adobe Express Add-on Architecture

Unlike a traditional desktop UXP plugin, an Adobe Express add-on splits its architecture into two distinct layers to protect the cloud-based web application:
┌─────────────────────────────────────────────────────────────┐
│                      Adobe Express Host                     │
│                                                             │
│   ┌────────────────────────┐       ┌────────────────────┐   │
│   │       iframe UI        │◄─────►│  Document Sandbox  │   │
│   │                        │       │                    │   │
│   │ (HTML / React / CSS)   │ Bridge│ (Express Doc SDK)  │   │
│   │ Complete Web APIs      │       │ No DOM access      │   │
│   └────────────────────────┘       └────────────────────┘   │
└─────────────────────────────────────────────────────────────┘

## creating a project

1. Las dependencias base (Automáticas)

No necesitas adivinarlas ni escribirlas a mano. Adobe provee una herramienta de terminal que genera el archivo package.json con todo lo necesario para arrancar. Solo debes ejecutar en tu terminal:

sh: npm create @adobe/ccweb-add-on@latest

## Grid add-on Tutorial
https://developer.adobe.com/express/add-ons/docs/guides/learn/how-to/tutorials/grids-addon
https://github.com/AdobeDocs/express-add-on-samples/tree/main/document-sandbox-samples/express-grids-addon/

1. git clone https://github.com/AdobeDocs/express-add-on-samples.git
2. cd to grids-design-start
3. sh: npm install && npm run build && npm run start
4.
