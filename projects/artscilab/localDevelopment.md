# Setting up local development for ArtSciLab

Local development has till now only involved the frontend. We have been making changes to the backend at [dev.atec.io](https://dev.atec.io) as needed, without a corresponding local installation, and just having the local frontend querying the same live API.

To get set up, 
- Install [Node.js and NPM](https://nodejs.org)
- Install [Yarn](https://classic.yarnpkg.com/en/) using `npm install --global yarn` 
- Clone the [ArtSciLab repo](https://github.com/artscilab/artscilab).
- In `package.json`, remove `node-sass` from dependecies.
- Run `yarn install` to download all the dependencies. 
- Install `node-sass` using `yarn add node-sass`
- **DO NOT PUSH updated `package.json` or `yarn.lock` file to git EVER. This will break the hosted website.**
- Modify Node options
  -  Windows: `set NODE_OPTIONS=--openssl-legacy-provider`
  -  Unix: `export NODE_OPTIONS=--openssl-legacy-provider`

You should now be ready to go, just run `yarn dev` to get started with development. Peruse the [Next.js docs](https://nextjs.org/docs/getting-started) to get started. They have a pretty comprehensive [tutorial](https://nextjs.org/learn/basics/create-nextjs-app) that is worth going through as well.  

--- 

Contributors: Al Madireddy, Omkar Ajnadkar
