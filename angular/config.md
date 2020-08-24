# Angular

Comandi gestione app angular.

Installare node tramite il tool nvm.
L'Angular CLI e le app Angular dipendono da caretteristiche e funzionalità fornite da liberie disponibili come pacchetti npm che possono essere installati e gestiti tramite npm package manager installato contestualmente a node.

### Angula CLI

Questo sturmento ci accompagnerà nella creazione delle app Angular.
Per installarla globalmente eseguire il comando

```
npm install -g @angular/cli
ng add @angular/material
ng add @angular/pwa
```

Dopo aver installato la cli possiamo creare il nostro progetto eseguendo il comando

```
ng new mst --enableIvy=true -p=mst --routing=true --style=scss

ng generate module root/main --routing

ng generate component root/main/index
```

il comando installerà i pacchetti necessari con relative dipendenze
La CLI include un sever per eseguire e buildare facilmente l'app localmente, per eseguire lanciare il comando

```
ng serve --open
npm install bootstrap --save
npm install jquery --save
 npm install popper.js --save
 npm install aos --save

             "styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css",
              "node_modules/aos/dist/aos.css",
              "src/styles.scss"
            ],
            "scripts": [
              "node_modules/jquery/dist/jquery.min.js",
              "node_modules/popper.js/dist/umd/popper.min.js",
              "node_modules/bootstrap/dist/js/bootstrap.min.js"
            ]
```

### Lazy loading

Per ottimizzare lo startup dell'applicazione è utile utilizzare il lazy loading dei moduli, quindi con il comando

```
ng generate module features/main --routing
```

viene generato un nuovo modulo a cui viene associato un modulo per il routing in cui è possibile definire le rotte del modulo attraverso la costante di tipo Routes

```
const routes: Routes = [{
  path: '',
  component: MainComponent,
  children: [{
    path: 'dashboard',
    component: DashboardComponent,
  }],
}];
```

a questo punto nell'app routing è possibile caricare i moduli in modo differenziato attraverso la costante di tipo Routes

```
const routes: Routes = [
   { path: 'main', loadChildren: () => import('./features/main/main.module').then(m => m.MainModule)},

  { path: '**', redirectTo: 'main' }
];
```

### Material

Installare Angular Material, Angular CDK e Angular Animations

```
npm install --save @angular/material @angular/cdk @angular/animations
```

Configurare animations

```
import {BrowserAnimationsModule} from '@angular/platform-browser/animations';

@NgModule({
  ...
  imports: [BrowserAnimationsModule],
  ...
})
```

Importare i moduli material con i rispettivi componenti. Creando un NgModulo separato che importa e esporta tutti i componenti che userai nell'applicazione. Un buon posto dove importare ed esportare il modulo che raccoglie tutti i componenti material è lo shared module

```
import {MatButtonModule, MatCheckboxModule} from '@angular/material';

@NgModule({
  imports: [MatButtonModule, MatCheckboxModule],
  exports: [MatButtonModule, MatCheckboxModule],
})
export class MyOwnCustomMaterialModule { }
```

Includere un tema, vedremo poi come personalizzarli a caldo, per usarne uno precostruito è sufficiente aggiungere

```
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
```

allo style.css
Per aggiungere le gesture

```
npm install --save hammerjs
```

e dopo l'installazione aggiungere in main.ts

```
import 'hammerjs';
```

Per utilizzare le mat-icon è sufficiente aggiungere nell'index.html

```
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

### NGRX

Pattern per fornire uno state container basato su tre principi:
un singola sorgente dati
uno stato in sola lettura
i cambiamenti allo stato vengono effettuati tramite pure functions
Poichè lo stato in un app redux è immutabile, quando un reducer cambia qualcosa nello stato questo restituisce un nuovo stato

- Installation of the library

```
npm install @ngrx/core @ngrx/store @ngrx/effects @ngrx/store-devtools @ngrx/router-store --save
```

- Folder structure for the store

creare la struttura di cartelle sotto la cartella store: “actions”, “effects”, “reducers”, “selectors” and “state”.

- Create State and initial values

creo il models e lo stato iniziale e generare l'app state che conterrà tutto lo stato dell'app

- Create Actions

le azioni descrivono cosa succede allo stato quindi get add remove update
creao delle azioni per ogni oggetto dello stato.
Viene esportato un Enum contenente le definizioni del tipo di azioni.
Viene creata ed esportata una classe per ogni azione che implemente un'interfaccia Action di ngrx.
Settiamo il tipo con uno dei valori enums e se c'è un payload per l'azione bisogna aggiungerlo al costruttore della classe.
Infine esportiamo un type contente le nostre classi di azione

- Create Reducers

avremo dei reducer corrispondenti ad alcune azioni, altre vengono prese in carico dagli effect.
La dichiarazione di un reducer riceve lo stato di un determinato tipo insieme all'azione e restituisce un I...State.
Usando uno switch case generiamo dei casi per ogni singola azione.
Ogni caso restituisce un nuovo oggetto che è il risultato del merge del vecchio stato e il nuvo.
Ogni reducer ha un risultato di default che restituisce lo stato senza nessun cambiamento.
Come ultimo passaggio aggiungiamo tutti i reducer ad un'app action reducer map

- Create Effects

Gli effects stanno in ascolto se un'azione viene dipatchata, se è una delle azioni per cui ha un caso allora genere un side effect per poi emettere una nuova azione che farà entrare in azione il reducer.
Creiamo il config effect usando l'injectable decorator,
dichiariamo l'effect usando l'effect decorator fornito da ngrx,
usando le azioni fornite da ngrx possiamo iniziare facendo una pipe del nostro operatore per questo effect.
Settiamo l'effect action type usando l'ofType operatore,
usiamo degli operatorei rxjs per avere ciò di cui abbiamo bisogno,
infine facciamo un dispatch della nuova azione,

- Create Selectors

Poichè non avrebbe senso ripetere le selezioni di pezzi del nostro stato creeiamo dei selettori che potremo riutilizzare.
Il primo parametro è il pezzo di store che andiamo ad utilizzare per avere i dati, il secondo è una funzione anonima che risolve cosa il selettore restituirà.

- Setting everything up

Importiamo i reducer e facciamo il provide alla forRoot dello store module,
importiamo gli efferri e facciamo il provide dentro un array alla forRoot dell'effect module,
settiamo la configurazione per il router state modulde facciando il provide alla forRoot di un oggetto statekey
aggiungiamo lo store dev tool se siamo nell'ambiente di sviluppo

- Using the store in some components

injettiamo lo store nel componente,
settiamo una proprieta nel componente
nell'init facciamo il dispatch della action get...

Creare il model
Creare lo state iniziale e aggiungerlo ad app state
Create Actions
Create Reducer e aggiungerlo all'app reducer
Create Effect and add to Effect module
Create selector

### Error

Per catturare l'errore senza interrompere il flusso usiamo un servizio di log in cui creiamo il metodo handleError che prenderà in ingresso il nome dell'operazione andata in errore e il valore che sarà restituito come observable e lo usiamo nell'operatore catchError che intercetta un observable non andato a buon fine.

### Update

```
ng update
```

Analizza package.json e cerca aggiornamenti nel progetto suggerendo quali pacchetti sono da aggiornare

```
npm outdate
```

Controlla nel registro per controllare se ci sono pacchetti non aggiornati.

```
npm update
```

Aggiorna pacchetti installati

```
npm list -g --depth 0
```

Lista globale pacchetti npm installati.
nvm tool per gestire le versioni di node.

### Controllare bundle

```
npm install --save-dev webpack-bundle-analyzer
```

Pacchetto npm per analizzare la grandezza dei pacchetti webpack, una volta installato modificare il gile package json aggiungendo l'istruzione per creare le statistiche

```
"bundle-report": "ng build --prod --stats-json && webpack-bundle-analyzer dist/stats.json"
```

controllare la cartella e il nome del file stats.json e poi eseguire il comando appena creato

```
npm run bundle-report
```

### Aggiungere lingue

```
npm i @ngx-translate/core --save
npm i @ngx-translate/http-loader --save
```

The @ngx-translate/core package includes the essential services, pipe, and directives, to convert the content in various languages.

The @ngx-translate/http-loader service helps in fetching the translation files from a webserver.

```
    HttpClientModule,
    TranslateModule.forRoot({
      loader: {
        provide: TranslateLoader,
        useFactory: httpTranslateLoader,
        deps: [HttpClient]
      }
    })
  ],
  providers: [],
  bootstrap: [AppComponent]
})

export class AppModule { }

// AOT compilation support
export function httpTranslateLoader(http: HttpClient) {
  return new TranslateHttpLoader(http);
}
```
