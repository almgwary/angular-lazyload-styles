# 🌟 Angular Lazyload Styles 😉
Dynamically Load CSS with the Angular CLI


## Configuring the CLI 👏

tell the Angular CLI to NOT include these styles *angular.json*


```
"styles": [
            "src/styles.scss",
            {
              "input": "src/client-a-styles.scss",
              "bundleName": "client-a",
              "inject": false
            },
            {
              "input": "src/client-b-styles.scss",
              "bundleName": "client-b",
              "inject": false
            }
          ]
```


# Lazy load at runtime  👀


```
import { DOCUMENT } from '@angular/common';
...
export class AppComponent {
  title = 'dyncss';

  constructor(@Inject(DOCUMENT) private document: Document) {}

  loadStyle(styleName: string) {
    const head = this.document.getElementsByTagName('head')[0];

    let themeLink = this.document.getElementById(
      'client-theme'
    ) as HTMLLinkElement;
    if (themeLink) {
      themeLink.href = styleName;
    } else {
      const style = this.document.createElement('link');
      style.id = 'client-theme';
      style.rel = 'stylesheet';
      style.href = `${styleName}`;

      head.appendChild(style);
    }
  }
}
```

## Lazy load in development  👨🏻‍💻
If you try to invoke loadStyle('client-a.css') in development (via ng serve) you’ll most likely get the following error:


The reason is that the CSS is served via a JavaScript file (style.js) as it is faster during development. To disable this behavior, you can add the "extractCss": true to the project configuration in the angular.json:

```
"dyncss": {
    ...
    "build": {
        ...
        options: {
          ...
          "extractCss": true,
          "styles": [
            ...
          ]
        }
    },
    ...
},

```


Thanks 🌟 for: Juri Strumpflohner 

https://juristr.com/blog/2019/08/dynamically-load-css-angular-cli/




