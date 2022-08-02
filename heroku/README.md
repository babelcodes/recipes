# Heroku

[[_TOC_]]

## Setup Heroku CLI

- Download and install [the Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line)
   - `brew link heroku`
- Connect to the CLI to allow following commands:
   - `heroku login`
- See also: https://devcenter.heroku.com/articles/heroku-cli


## Create app on Heroku

- https://medium.com/better-programming/how-to-deploy-your-angular-9-app-to-heroku-in-minutes-51d171c2f0d
    - https://itnext.io/how-to-deploy-angular-application-to-heroku-1d56e09c5147

Procedure:

- https://dashboard.heroku.com
    - https://dashboard.heroku.com/apps/korali-my/deploy
- Create App:
    - Name
    - Region
- Settings tab
    - Add buildpack, nodejs
    - Domains => find the public URL, eg https://korali-api-2.herokuapp.com/


## Angular app

- https://betterprogramming.pub/how-to-deploy-your-angular-9-app-to-heroku-in-minutes-51d171c2f0d

1. Install Express JS: `npm i express -E`
1. Create `/index.js`

```js
// index.js
//
// REPLACE PACKAGE_JSON_NAME BY THE NAME OF THE PROJECT

function requireHTTPS(req, res, next) {
    // The 'x-forwarded-proto' check is for Heroku
    if (!req.secure && req.get('x-forwarded-proto') !== 'https') {
        return res.redirect('https://' + req.get('host') + req.url);
    }
    next();
}
const express = require('express');
const app = express();
app.use(requireHTTPS);
app.use(express.static('./dist/PACKAGE_JSON_NAME'));
app.get('/*', (req, res) =>
  res.sendFile('index.html', {root: 'dist/PACKAGE_JSON_NAME/'}),
);
app.listen(process.env.PORT || 8080);
```
1. Update `/package.json`
```json
{
  "scripts": {
    "start": "node index.js",
    "build": "ng build --prod",
}}    
```


## Node Express App

Ensure that your app has a `start` script in the `package.json`:

```json
// /package.json
{
  "scripts": {
    "start": "node server.js"
  },
}
```


## Deploy

### Common part

- Setup Heroku CLI (see above)
- Create app on Heroku (see above)


### Automatically, by pulling github [DEPRECATED, July 2022]

- Deploy tab >
    - Connect to Github
    - Enable Automatic Deploys
- https://stackoverflow.com/questions/71892543/heroku-and-github-items-could-not-be-retrieved-internal-server-error
    1. Create app on Heroku
    1. Install Heroku CLI: https://devcenter.heroku.com/articles/heroku-cli
    1. `heroku login`

### Another remote, to push explicitly

- Add heroku remote:
    - `heroku git:remote -a korali-my`
- Push to heroku:
    - `git push heroku main`


### Domain

OVH

- Pour un domaine racine, supprimer tous les champs existants deja sur `www.mondomain.com.` (`www.`)
    - https://community.ovh.com/t/redirection-vers-app-heroku/6392/49
- Zone DNS > Ajoutée une entrée > `CNAME`
- Sous-domaine: `api` .wawa.sh (`www` pour un domaine racine)
- TTL         : Par défaut
- Cible       : `wawash-api.herokuapp.com.` (final dot)
- Cible réelle : « wawash-api.herokuapp.com.wawa.sh. »
- Le champ CNAME actuellement généré est le suivant : "api IN CNAME wawash-api.herokuapp.com."

Heroku

- App > Settings > Domain, Add domain
- Domain name: `api.wawa.sh`

!!! PROBLEM WITH HTTPS !!!


### Commands

- To get logs: `heroku logs --tail --app wiwish-web`

