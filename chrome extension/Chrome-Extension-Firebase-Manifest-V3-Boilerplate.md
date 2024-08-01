---
title: "Chrome Extension Firebase Manifest V3 boilerplate"
coverImage: "https://qiy.blob.core.windows.net/blogs/firebase.png"
---

# Chrome Extension Firebase Manifest V3 Template 

## Features

- **Firebase Authentication**: Seamlessly integrate Firebase authentication into your Chrome extension.
- **Background Script ID Token Fetching**: Efficiently fetch ID tokens in the background script.
- **Comprehensive Login Options**: Support for all Firebase login methods, ensuring flexible user authentication.
- **Website Integration**: Easily link the extension with your website for a cohesive user experience.
- **API Calls in Background Script**: Make API calls directly from the background script for enhanced functionality.
- **Tailwind CSS Enabled**: Utilize Tailwind CSS for streamlined and responsive design.
- **Minimal Permissions**: No additional permissions required to enable Firebase, ensuring user privacy and security.

---

let's get started 
![minion gif](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMDI2czNjbXM4Yml3cjFqYmVwaTZwd3JiZzJhdW1kd2NtYjNiYmx6OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/tSdEYGwXB0ZDlfUM0K/giphy.gif)

### How to Use Firebase in a Chrome Extension (MV3)

In this blog, we will walk you through the steps to use Firebase in a Chrome extension using a boilerplate that includes Firebase, Tailwind CSS, and TypeScript.

#### Step 1: Clone the Chrome Extension Boilerplate

First, clone the Chrome extension boilerplate repository. This boilerplate has Firebase already set up, allowing you to get the ID token and make API calls to the backend server from the background script.

```bash
git clone https://github.com/muhammadasadullahtariq/Chrome-Extension-Firebase-Manifest-V3-Boilerplate
cd Chrome-Extension-Firebase-Manifest-V3-Boilerplate
```


#### Step 2: Set Up Chrome Extension

- `npm i` to install dependancies
- Add the firebase credentials it utils in `firebase_config.ts` file
- `npm start` to start running the fast development mode Webpack build process that bundle files into the `dist` folder
- `npm i --save-dev <package_name>` to install new packages

##### Loading The Chrome Extension

- Open Chrome and navigate to `chrome://extensions/`
- Toggle on `Developer mode` in the top right corner
- Click `Load unpacked`
- Select the entire `dist` folder

#### Step 3: Set Up Your Website

Next, set up your website to interact with the Chrome extension. Create a route, for example, `linkapp/service/extension`.

When the user clicks the login button on the Chrome extension, it should open this URL on your website. On the website, implement the logic to check if the user is already logged in. If not, prompt the user to log in or create an account using any authentication method you are using.

You can use a custom hook to check if the user is logged in or not. Here’s an example using Firebase Authentication in a React component:

```javascript
import { useEffect, useState } from 'react';
import auth from '@react-native-firebase/auth';

const useFirebaseAuthentication = () => {
    const [authUser, setAuthUser] = useState(undefined);

    useEffect(() => {
        const unListen = auth().onAuthStateChanged(authUser => {
            authUser ? setAuthUser(authUser) : setAuthUser(null);
        });
        return () => {
            unListen();
        };
    }, []);

    return authUser;
};

export default useFirebaseAuthentication;
```

When the user is logged in (`auth().currentUser != null`), proceed to create a custom token for the user on the backend.

#### Step 4: Create a Custom Token on the Backend

To create a custom token, obtain the service account key from Firebase settings and use it to initialize Firebase Admin SDK:

```javascript
const admin = require('firebase-admin');
const serviceAccount = require('path/to/serviceAccountKey.json');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
});

const customToken = await admin.auth().createCustomToken(result.uid);
```

#### Step 5: Embed the Custom Token in the URL

Once the custom token is generated, embed it into the URL. Here’s an example using Next.js:

```javascript
const current = new URLSearchParams(Array.from(searchParams.entries()));
current.set('idToken', token.data);
const search = current.toString();
const query = search ? `?${search}` : '';
router.push(`${pathname}${query}`);
```

#### Step 6: Read the Token in the Chrome Extension

The Chrome extension will automatically read the token from the URL, logging the user in.

By following these steps, you can integrate Firebase authentication into your Chrome extension, allowing seamless user login and interaction with your website.

Good job solder now return to your base
![Minion GIF](https://media.giphy.com/media/dDFPsSRtmyW0SYQSGS/giphy.gif)



# Steps to change and push your extension to production

1. `git init` to start a new git repo for tracking your changes, do an initial base commit with all the default files
2. Update `package.json`, important fields include `author`, `version`, `name` and `description`
3. Update `manifest.json`, important fields include `version`, `name` and `description`
4. Update `webpack.commmon.js`, the title in the `getHtmlPlugins` function should be your extension name

# Production Build

1. `npm run build` to generate a minimized production build in the `dist` folder
2. ZIP the entire `dist` folder (e.g. `dist.zip`)
3. Publish the ZIP file on the Chrome Web Store Developer Dashboard!

## Important Default Boilerplate Notes

- Folders get flattened, static references to images from HTML do not need to be relative (i.e. `icon.png` instead of `../static/icon.png`)
- Importing local ts/tsx/css files should be relative, since Webpack will build a dependancy graph using these paths
- Update the manifest file as per usual for chrome related permissions, references to files in here should also be flattened and not be relative


---

This guide should help you get started with using Firebase in your Chrome extension. If you have any questions or need further assistance, feel free to reach out!
