Table of contents:

[TOC]

# Conecting Firebase with Angular project

1)  create a project in Firebase, and an app, and grab the config like this:

````typescript
//environment.ts && environment.prod

export const environment = {
	production: false (or true),
	
	firebaseConfig :{
    apiKey: "AIzaSyCOkAGnXf6HqUNVwnQtVB7YTtHTZhslIIY",
    authDomain: "portfolio-a05f5.firebaseapp.com",
    databaseURL: "https://portfolio-a05f5.firebaseio.com",
    projectId: "portfolio-a05f5",
    storageBucket: "portfolio-a05f5.appspot.com",
    messagingSenderId: "225295559611",
    appId: "1:225295559611:web:8df55a85cb64312c7cb29e",
    measurementId: "G-TDCP4LKQYL"
  }
}


````



2) install angular fire

````bash
ng add @angular/fire
````



3) configure `app.module.ts`:

````typescript
//app.module.ts

import { AngularFireModule } from '@angular/fire';
import { AngularFireFunctionsModule } from '@angular/fire/functions';
import { environment } from '../../src/environments/environment';



@NgModule({
    declarations: [
        //some components
    ],
    imports: [
        BrowserAnimationsModule,
        NgbModule,
        FormsModule,
        RouterModule,
        AppRoutingModule,
        
        //important part here!
        AngularFireModule.initializeApp(environment.firebaseConfig),
        AngularFireFunctionsModule,

    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule { }
````

4) make sure the `firebase` node_module package is installed, because angular fire uses it



# Adding cloud functions

1)  Initialize the functions folder

````bash
firebase init
select functions
follow instructions
````

2) Adding npm packages: a `/functions` folder is gonna be created. Make sure to go cd into that folder before installing packages with npm

````bash
cd functions

npm i whatever
````

3) Testing functions locally: 

````bash
firebase emulators:start --only functions
````

Test them with local emulator docs:

https://firebase.google.com/docs/functions/local-emulator?authuser=0