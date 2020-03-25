# GreenhousePlusPlus.App

GreenhousePlusPlus.WebAPI

[![Build Status](https://jenkins.sedrad.com/buildStatus/icon?job=GreenhousePlusPlus.WebApi)](https://jenkins.sedrad.com/job/GreenhousePlusPlus.WebApi/)

This project consumes the web API provided by: https://github.com/srad/GreenhousePlusPlus

## Quickstart

```bash
git clone https://github.com/srad/GreenhousePlusPlus.App.git
npm install
cp .env.default .env.local
```

Now replace the `#{API_URL}#` in `.env.local` which the endpoint address, i.e.: `http://localhost:5000`

Start the development server:

```bash
npm run serve
```

## Screenshots

![](https://raw.githubusercontent.com/srad/GreenhousePlusPlus.App/master/docs/screenshot0.jpg)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your unit tests
```
npm run test:unit
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
