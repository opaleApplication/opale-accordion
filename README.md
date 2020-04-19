# Installation
```
$ npm i opale-accordion
```
# Preview

![Alt Text](https://www.jrichardapp.com/images/gif-opale-accordion.gif)

# ionic Angular
Using a Stencil built web component collection within an Angular CLI project is a two-step process. We need to:
1. Include the ``` CUSTOM_ELEMENTS_SCHEMA ``` in the modules that use the components.

2. Call ``` defineCustomElements(window)``` from ``` main.ts``` (or some other appropriate place).

### Including the Custom Elements Schema

Including the ``` CUSTOM_ELEMENTS_SCHEMA ``` in the module allows the use of the web components in the HTML markup without the compiler producing errors. This code should be added into the ```AppModule``` and in every other modules that use your custom elements.

Here is an example of adding it to ```AppModule```:

```javascript
import { BrowserModule } from '@angular/platform-browser';
import { CUSTOM_ELEMENTS_SCHEMA, NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, FormsModule],
  bootstrap: [AppComponent],
  schemas: [CUSTOM_ELEMENTS_SCHEMA], // add this
})
export class AppModule {}
```

The ```CUSTOM_ELEMENTS_SCHEMA``` needs to be included in any module that uses custom elements.

### Calling defineCustomElements
A component collection built with Stencil includes a main function that is used to load the components in the collection. That function is called ```defineCustomElements()``` and it needs to be called once during the bootstrapping of your application. One convenient place to do this is in ```main.ts``` as such:


```javascript
src/maint.ts
...
import { defineCustomElements } from 'opale-accordion/loader';

...
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
defineCustomElements(window);// add this

```

### Edge and IE11 polyfills
If you want your custom elements to be able to work on older browsers, you should add the ```applyPolyfills()``` that surround the ```defineCustomElements()``` function.

```javascript
import { applyPolyfills, defineCustomElements } from 'opale-accordion/loader';
...
applyPolyfills().then(() => {
  defineCustomElements(window)
})
```

### Accessing components using ViewChild and ViewChildren
Once included, components could be referenced in your code using ```ViewChild``` and ```ViewChildren``` as in the following example:
```javascript
import {Component, ElementRef, ViewChild} from '@angular/core';

import 'opale-accordion';

@Component({
    selector: 'app-home',
    template: `<opale-accordion #opale></opale-accordion>`,
    styleUrls: ['./home.component.scss'],
})
export class HomeComponent {

    @ViewChild('opale') opaleComponent: ElementRef<HTMLOpaleComponentElement>;

    async onAction() {
        await this.opaleComponent.nativeElement.OpaleComponentMethod();
    }
}
```
### Usage
Call the component like this

```
   <opale-accordion color="#868686" width="100%" label="Lorem Ipsum"
        description="<b>Lorem</b> ipsum dolor sit amet, <i>consectetur</i> adipiscing elit.<br> Integer rhoncus facilisis urna, ut auctor lacus.">
      </opale-accordion>
```
# React

With an application built using the ```create-react-app``` script the easiest way to include the component library is to call ```defineCustomElements(window)``` from the ```index.js``` file.
Note that in this scenario applyPolyfills is needed if you are targeting Edge or IE11

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

// opale-accordion published to npm:
import { applyPolyfills, defineCustomElements } from 'opale-accordion/loader';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();

applyPolyfills().then(() => {
  defineCustomElements(window);
});

```
Following the steps above will enable your web components to be used in React, however there are some additional complexities that must also be considered. https://custom-elements-everywhere.com/ contains a synopsis of the current issues.

# Vue.js

# License MIT

Free component, Hell Yeah!
