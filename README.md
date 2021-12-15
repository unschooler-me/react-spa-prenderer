# react-spa-prerender

## Warning!
react-spa-prerender processes only page with flag page-loaded(<div id="page-loaded" />)

## Install
packege.json: 
```
    "devDependencies": {
        ...
         "react-spa-prerender": "https://github.com/unschooler-me/react-spa-prenderer.git"
        ...
    }
```
yarn install

## Add as postbuild script
In your package.json add the following in the scripts section:
```
"scripts": {
    "postbuild": "react-spa-prerender",
}
```

## Add .rsp.json file
__.rsp.json__ is the configuration file for `react-spa-prerender`. Create this file in your __application root folder__.
The minimum configuration requires the __routes__ you want to be parsed.
Example:
```
{
    routes: [
        "/",
        "/about",
        "/services"
        "/blog/article1"
        ...
    ]
}
```
From example above:
Your "/" route will transform into "index.html" page.  
"/about" -> "about.html"  
"/services" -> "services.html"  
"/blog/article1" -> create blog directory with file "article1.html" ("/blog/article1.html")
and so on...

__The rest of the .rsp.json options described below__

## Add ReactDOM.hydrate in your index.js 
In your index.js(main app file) change the ReactDOM.render logic:
```
import ReactDOM from 'react-dom';

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
Into following:
```
import ReactDOM from 'react-dom';

const rootElement = document.getElementById("root");

if (rootElement.hasChildNodes()) {
  ReactDOM.hydrate(<App />, rootElement);
} else {
  ReactDOM.render(<App />, rootElement);
}
```

## Voila!!!
That's it. After accomplishing all the steps above, run you build command and your prerendered files will be in your build directory.

## .rsp.json Options

|option | type | default | description |
|-----|--------|------|---------|
| routes(Required) | Array | - | An array of routes you want to parse and prerender into static html|
| port | Number | 3000 | port where prerendering server will be starting |
| buildDirectory | String | './build' | a relative path to your build folder
|engine | Object | {} | params for Puppeteer engine, list of available params described below
| delay | Number | 0  | Timeout load page


### Engine options:
- launchOptions - object, containing properties to control **puppeteer.launch()** command. The whole list of available properties available here: [https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-puppeteerlaunchoptions](https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-puppeteerlaunchoptions)

#### Example of .rsp.json with engine options:
```
{
  "port": 3000,
  "buildDirectory": "./build",
  "delay": 5000,
  "engine": {
    "launchOptions": {
      "args": ["--no-sandbox", "--disable-setuid-sandbox"],
      "product": "chrome",
      "headless": true,
      "ignoreHTTPSErrors": true
    }
  },
  "routes": [
    "/",
    "/about",
    "/services",
    "/blog/article1",
    "/blog/article2"
  ]
}
```
