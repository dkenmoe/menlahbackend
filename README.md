https://github.com/stemmlerjs/simple-typescript-starter/blob/master/package.json

# How to Setup a TypeScript + Node.js Project

## Initial Setup

mkdir menlahbackend
cd menlahbackend

## Setup Node.js package.json
npm init -y

## Add TypeScript as a dev dependency
npm install typescript --save-dev

## Install ambient Node.js types for TypeScript
npm install @types/node --save-dev

## Create a tsconfig.json

npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true

## Create the src folder and create our first TypeScript file

mkdir src
touch src/index.ts

## Compiling our TypeScript

npx tsc

# Useful configurations & scripts

## Cold reloading

Cold reloading is nice for local development. In order to do this, we'll need to rely on a couple more packages: ts-node for running TypeScript code directly without having to wait for it be compiled, and nodemon, to watch for changes to our code and automatically restart when a file is changed.

npm install --save-dev ts-node nodemon

Add a nodemon.json config.

{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "npx ts-node ./src/index.ts"
}

And then to run the project, all we have to do is run nodemon. Let's add a script for that.
"start:dev": "npx nodemon",
By running npm run start:dev, npx nodemon will start our app using npx ts-node ./src/index.ts, watching for changes to .ts and .js files from within /src.

## Creating production builds

In order to clean and compile the project for production, we can add a build script.
Install rimraf, a cross-platform tool that acts like the rm -rf command (just obliterates whatever you tell it to).

npm install --save-dev rimraf

And then, add this to your package.json.
"build": "rimraf ./build && tsc",

Now, when we run npm run build, rimraf will remove our old build folder before the TypeScript compiler emits new code to dist.

## Production startup script

In order to start the app in production, all we need to do is run the build command first, and then execute the compiled JavaScript at build/index.js.

The startup script looks like this.
"start": "npm run build && node build/index.js"