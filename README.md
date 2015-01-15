#Running custom node on openshift

> Based on work of [ramr](https://github.com/ramr/nodejs-custom-version-openshift) and [ryanj](https://github.com/ryanj/nodejs-custom-version-openshift)

## Why is the master branch empty?

> Because there are two ways of managing the node version!

1. **read-marker-file branch**
  - will read the marker file which is the "normal" way for openshift
2. **read-package-json**
  - based on ryanj's work, will parse the package.json in your app root and read the 'enginge' key and install the corresponding node version

> It does not matter which branch you select, the point is that developers are used to manage dependencies via package.json file

## Preflight check
- Openshift account
- GIT
- NPM
- RHC Client
- NODE

## Installation

#### Web
1. Sign in to your openshift account, head over to applications and create a new one
2. Select the `node.js 0.10` cartridge
3. Name your application
4. In the source code field put: 'https://github.com/DavidReinberger/openshift-custom-nodejs.git'
5. In the second field put the branch you have chosen to suit your needs: 'read-marker-file' or 'read-package-json'
6. Watch magic happen!
7. Go to http://yourappname-yourdomain.rhcloud.com
8. At the top of the page you should see **v0.10.33**

> If anything goes wrong do: `rhc tail yourapp` this should give you the error log

#### Command line
1. Decide wheter you want your app to scale and what gear to use
2. Install your app
  - If you want to use other gear than small add `-g` at the end **recommended**
  - non-scaling - `rhc app create YOURAPPNAME nodejs --from-code=https://github.com/DavidReinberger/openshift-custom-nodejs.git#branch-name`
  - scaling - `rhc app create YOURAPPNAME nodejs --from-code=https://github.com/DavidReinberger/openshift-custom-nodejs.git#branch-name -s`
6. Watch magic happen!
7. Go to Go to http://yourappname-yourdomain.rhcloud.com
8. At the top of the page you should see **v0.10.33**

## Post install
1. If you installed from web clone your repo
2. Copy your code over to the cloned repo
3. Check `package.json`
  - see if there is the desired node version
  ````
  "engines": {
    "node": "0.10.33",
    "npm": "1.4.8"
  }
  ````
4. git add, commit and push
5. Right now you should see a lot of npm install depending on the size of your app
6. Check http://yourappname-yourdomain.rhcloud.com

> If anything goes wrong do: `rhc tail yourapp` this should give you the error log

## Meteor.js? WHAT? YES!
> YES! This is Meteor ready! (almost)

If you take a look at `server.js`, some might point out that is just a renamed meteorshim.js and YES you are absolutely correct!

### Preflight check
  - [Demeteorizer](https://github.com/onmodulus/demeteorizer)

## Instructions
1. Demetorize your app
  - sudo `demeteorizer -o ../_build/app`
    - use sudo to avoid errors!
2. Copy the demeteorized app to your cloned openshift repo
3. Open package.json and change

  ````
  "main": "main.js",
  "scripts": {
    "start": "node main.js"
  },
  ````
  
  To
  
  ````
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  ````
4. git add, commit and push
5. Right now you should see a lot of npm install depending on the size of your app
6. Check http://yourappname-yourdomain.rhcloud.com

> If anything goes wrong do: `rhc tail yourapp` this should give you the error log
