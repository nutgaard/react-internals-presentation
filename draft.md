# React under-tha-hood

## Agenda
* ## JSX vs React
* ### JSX - Javascript syntax extension
* ### JSX Pragma
* ### React
* #### React renderers
* #### React pakke arkitektur
* ## Kode kode kode
* ### Workshop 1 - stack reconciler kodeeksempler
* ### Workshop 2 - fiber reconciler kodeeksempler

## Formålet og agendaen


## JSX vs React
Ofte når vi snakker om React så hopper vi automatisk på JSX-bandwagon også.
Men det kan være greit å holde disse to konseptene separert, selvom de ofte går hånd-i-hånd.
Og selvom JSX ble introdusert av react, så er det egentlig ingenting som knytter disse sterkt sammen.

### Hva er hva?

#### JSX - JavaScript syntax extension
Dette utvider egentlig bare javascript/typescript språkene med litt ekstra snasen syntaks, slik at vi kan late som om vi skriver HTML i javascript-koden vår. :hurray:.

Alt den i teorien gjør er å oversette `<div className="menu">Menu content</div>` til `React.createElement('div', { className: 'menu' }, 'Menu content')`.
Dette gjøres basert på noe som kalles for jsx-pragma (pragma er synonymt med "directive"), som forteller transformatoren (f.eks babel) hvordan jsx-koden skal se ut i javascript-kode.

F.eks kan man sette følgende i `.babelrc`:
```json
    ["@babel/plugin-transform-react-jsx", {
      "pragma": "Didact.createElement",
      "pragmaFrag": "DidactDOM.Fragment"
    }]
```
og få babel til å bruke `Didact.createElement` fremfor `React.createElement`.

Vi skal ikke styre på for mye med akkurat dette, men det er kjekt å være klar over at JSX ikke er react.
Og at man har mulighet til å skru på hvordan dette fungerer selv. Sånn som om man bruker ett annet virtuell-dom bibliotek (f.eks mithril.js)

#### React - Frontend library
React er selve frontend-biblioteket som gjør jobben. I en applikasjon består denne ofte av to pakker `react` og `react-XXX`.
`react` er kjerne-pakken for apiet vi forholder oss til, men inneholder egentlig ikke veldig mye funksjonalitet.
Men den eksponerer en del funksjonalitet fra renderer-pakken (`react-dom` f.eks) vi bruker, da gjerne gjennom `react-reconciler` pakken.
F.eks `useState` som er definert i både `react-dom` og `react-reconciler` pakken (mulig slik at det skal andre som lager rendere skal slippe å kopiere mye kode?)


Det finnes renderer-pakker for:
* mobil (`react-native`)
* web (`react-dom`)
* command line (`ink`, `react-blessed`)
* desktop (`react-gtk`)
* tv'er
* hardware-skisser
* generering av filer (`react-pdf`, `redocx`)
* som integrasjon til andre (`react-figma`, `react-unity`)
  [Kilde](https://github.com/chentsulin/awesome-react-renderer)

Med så mange forskjellige platformer og resultat så kan man virkelig se styrken til react-modell hvor de har splittet opp ting i flere pakker.

I tillegg finnes det en tredje komponent (også skilt ut i en egen pakke) reconciler'eren.
De som har vært med lenge nok husker at react gikk fra en stack-basert reconciler til en fiber-basert reconciler. Mer om dette senere.

Måten ting blir satt opp er som følger.
Når man kaller `Renderer.render()` så lager den en `Reconciler` hvor den sender med en hel del konfigurasjon (e.g funksjoner som reconcileren kan kalle),
som beskriver hvordan man skal operere mot rendereren sin platform. Her vil f.eks react-dom sende inn en funksjon som lar reconcileren lage en DOM-node.

React-reconciler README: https://github.com/facebook/react/blob/0e100ed00fb52cfd107db1d1081ef18fe4b9167f/packages/react-reconciler/README.md
Dom renderer eksempel 1: https://blog.atulr.com/react-custom-renderer-1/
Dom renderer eksempel 2: https://agent-hunt.medium.com/hello-world-custom-react-renderer-9a95b7cd04bc
React-devblog fiber: https://engineering.fb.com/2017/09/26/web/react-16-a-look-inside-an-api-compatible-rewrite-of-our-frontend-ui-library/
JSX pragma babel doc: https://babeljs.io/docs/en/babel-plugin-transform-react-jsx/


Workshop 1: https://github.com/nutgaard/build-your-own-react
Workshop 2: https://github.com/nutgaard/didact-ts
Babel-repl: https://babeljs.io/repl/