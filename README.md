# Exodia.World

A decentralized digital gaming marketplace and platform that enables game studios to sell their games directly to players all around the world without a middleman in a secure and transparent way.

---

# Exodia.World Desktop Client

A desktop client for Exodia.World's marketplace, digital wallet, crowdfunding, etc. This project is built on top of Electron (Muon) + Metamask.

## Overview

exodia.world-desktop-client wraps [exodia.world-web-client](https://bitbucket.org/exodia-world/exodia.world-web-client) as a *submodule* so that we may build the same code for both web and desktop users.

## How it works

This project is built on top of Muon, which is a security-focused fork from Electron with support for Chrome extensions. It sets up a Muon application and injects the MetaMask extension. 

1. The npm scripts download the MetaMask code from the Chrome Store to the local folder.
2. The Muon application renders local HTML and injects the MetaMask content scripts.
3. The frontend code should use `chrome.ipcRenderer.send('message')` to communicate with Muon's main process and trigger the MetaMask popups.
4. MetaMask handles all the wallet management side of the application. So we don't have to deal directly with user's private keys.
5. Electron Builder packs and generates installers for Linux, Windows and MacOS.

## How to use

Because exodia.world-web-client was added as a submodule to this project, the first time you clone you have to run `git submodule init` to initialize the submodule.

Subsequently, run `git submodule update` to pull any changes to the submodule.

Commit changes to exodia.world-web-client just as usual (by going into the `app` directory first), but remember to add unstaged changes of `app` from this project's base directory as well.

If you don't get what I mean, check this [basic explanation about Git Submodules](https://gist.github.com/gitaarik/8735255).

### Setting up your project

Usually the frontend code that is rendered in the browser resides within a `dist` folder. If for some reason you decide to change the location, edit `main.js` to properly read `index.html` from your app's folder.

In development mode Muon will read the extensions file from the parent folder at `/extensions/metamask`, but when bundling to production, Electron Builder will expect the same folder at `app/dist/extensions/metamask` (this can be changed in `extensions.js`).

There are npm scripts to download MetaMask from Chrome Store. When in development run `npm run download.metamask.dev`. In production `npm run download.metamask.prod`.

Once you're ready run `npm start` for a development server.

### Managing MetaMask's popups

Unfortunately, the built-in communication between the MetaMask scripts to trigger the UI doesn't work on Muon. In order to make the popups work, you'll need to intercept the web3 calls and manually send a message to Muon.

Check out [this function](https://github.com/SwapyNetwork/electron-metamask-boilerplate/blob/master/your-app/index.js#L33).

### Building the installers

Electron Builder depends on both `package.json`s to generate the installers. For more info, check the [docs](https://www.electron.build/).

## Resources

[swapy-exchange-electron-wrapper](https://github.com/SwapyNetwork/swapy-exchange-electron-wrapper)

[Swapy's article on how we did the Electron + MetaMask integration](https://medium.com/@SwapyNetwork/integrating-metamask-with-electron-a-simple-secure-and-non-intrusive-approach-517a04da1656)

[Aragon's implementation](https://blog.aragon.one/electron-metamask-secure-easy-to-use-dapps-5a9987d21034)

