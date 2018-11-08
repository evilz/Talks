---
theme : "white"
customTheme : "custom"
---

 <img height="300" src="fable_logo.png" style="margin: 0;background: none;border: none;box-shadow: none" />

<span style="color:hsl(204, 86%, 53%)" >JavaScript you can be proud of!</span>

<div style="margin-top:60px">
      <img src="https://avatars3.githubusercontent.com/u/2937862?v=4&s=120" style="    float: right;margin: 0 10px 0 40px;border-radius: 50%;" />
      <span style=" padding-top:18px;    text-align: right; display: block;">Vincent Bourdon</span>
      <i style="text-align: right; display: block;
    color: rgba(0,0,0,.54);
    display: block;
    font-size:0.8em">
            <i class="fa fa-github" aria-hidden="true"></i>github.com/evilz
      </i>
      
  </div>

---

> Fable is an F# to JavaScript compiler powered by Babel, designed to produce readable and standard code.

<div style="margin-top:60px">
      <img src="https://pbs.twimg.com/profile_images/662939956952829952/vFCyXjf6_400x400.jpg" style="width:100px;    float: right;margin: 0 10px 0 40px;border-radius: 50%;" />
      <span style=" padding-top:18px;    text-align: right; display: block;">Alfonso Garcia-Caro</span>
      <i style="text-align: right; display: block;
    color: rgba(0,0,0,.54);
    display: block;
    font-size:0.8em">
            <i class="fa fa-tweet" aria-hidden="true"></i>@alfonsogcnunez
      </i>
      
  </div>

---

## Pourquoi fable ???

Pour utiliser la puissance de F# 

- Typage static <!-- .element: class="fragment" -->
- Inférence de type <!-- .element: class="fragment" -->
- Pattern matching <!-- .element: class="fragment" -->
- Immutabilité <!-- .element: class="fragment" -->
- Egalité structurel <!-- .element: class="fragment" -->
- Un vrai language functionnel  <!-- .element: class="fragment" -->

---

## API ??

La pluspart des APIs de la `Core library F#` sont supportées

---

## .NET Base Class Library

.NET                                  | JavaScript
--------------------------------------|----------------------------
Numeric Types                         | number
Arrays                                | Array / Typed Arrays
Events                                | fable-core/Event
System.Boolean                        | boolean
System.Char                           | string
System.String                         | string
System.Guid                           | string
System.TimeSpan                       | number
System.DateTime                       | Date
System.Timers.Timer                   | fable-core/Timer
System.Collections.Generic.List       | Array
System.Collections.Generic.HashSet    | Set
System.Collections.Generic.Dictionary | Map
System.Text.RegularExpressions.Regex  | RegExp
System.Lazy                           | fable-core/Lazy
System.Random                         | {}
System.Math                           | (native JS functions)

---

## FSharp.Core

.NET              | JavaScript
------------------|----------------------------------------------------------
Tuples            | Array
Option            | (erased)
Choice            | fable-core/Choice
String            | fable-core/String (module)
Seq               | [Iterable](http://babeljs.io/docs/learn-es2015/#iterators-for-of)
List              | fable-core/List
Map               | fable-core/Map
Set               | fable-core/Set
Async             | fable-core/Async
Event             | fable-core/Event (module)
Observable        | fable-core/Observable (module)
Arrays            | Array / Typed Arrays
Events            | fable-core/Event
MailboxProcessor  | fable-core/MailboxProcessor (limited support)

---

## Plus d'infos dans la doc :

http://fable.io/docs/compatibility.html

---

# Exemples

---

## Fonctions

```fsharp
let square x = x * x
let sq = square 42
```

⬇

```js
export function square(x) {
    return x * x | 0;
}
export const sq = square(42);
```

---

## Currying

```fsharp
let add2times3 = (+) 2 >> (*) 3
let result = add2times3 5
```

⬇

```js
export const add2times3 = $var1 => 3 * (2 + $var1);
export const result = add2times3(5); 
```

---

## Return statement

```fsharp
let x =
  1
  2
```

⬇

```js
export const x = (() => {
  1;
  return 2;
})();
```

---

## Pattern matching

```fsharp
let booleanProxy x =
  match x with
  | true -> true
  | false -> false

booleanProxy true
booleanProxy false
```

⬇

```js
export function booleanProxy(x) {
  if (x) {
    return true;
  } else {
    return false;
  }
}
booleanProxy(true);
booleanProxy(false);
```

---

# REPL 

http://fable.io/repl/

---

## Comment cela fonctionne ?

- Un daemon (.net) écoute les modifications sur les sources  <!-- .element: class="fragment" -->
- Extrait un AST pour Babel  <!-- .element: class="fragment" -->
- Un loader pour webpack ou rollup créé le bundle avec le js  <!-- .element: class="fragment" -->

---

## Comment cela fonctionne ?

![](fable-babel-js.png) <!-- .element: style="width: 70%;border:none;box-shadow: none" -->

---

## Requirements

* [dotnet SDK](https://www.microsoft.com/net/download/core) 2.0 or higher
* [node.js](https://nodejs.org) 6.11 or higher
* A JS package manager: [yarn](https://yarnpkg.com) or [npm](http://npmjs.com/)
* An F# editor like like VS Code or Atom with [Ionide](http://ionide.io/), [Visual Studio](https://www.visualstudio.com/) for Windows or macOS, Emacs with [fsharp-mode](https://github.com/fsharp/emacs-fsharp-mode) or [Rider](https://www.jetbrains.com/rider/)

---

# DEMO

```sh
dotnet new -i Fable.Template
dotnet new fable -n FableApp

cd FableApp
yarn install
dotnet restore

cd src
dotnet fable yarn-start
```

---

## Tout type de projet est possible !

- Du front => React ou Elmish
- Node JS
- Electron
- Du React natif
- Du partage Server/Client (Suave/Fable)

---

## Elmish


Elmish est une implémentation de l'architecture Elm `Model / View / Update`. 

Elmish peut utiliser React / ReactNative comme moteur de rendu.

---

## Elmish

![](elmish-flow.png)

---

# Demo

## Compteur en Fable-Elmish
[Demo](
https://fable-elmish.github.io/sample-react-counter/)

[Sources](https://github.com/fable-elmish/sample-react-counter/tree/master/src)

---

## MODEL

```fsharp
type Model = int

type Msg =
| Increment
| Decrement

let init() : Model = 0
```

---

## UPDATE

```fsharp
let update (msg:Msg) (model:Model) =
    match msg with
    | Increment -> model + 1
    | Decrement -> model - 1
```

---

## VIEW

```fsharp
let view model dispatch =

  R.div []
      [ R.button [ OnClick (fun _ -> dispatch Decrement) ] [ R.str "-" ]
        R.div [] [ R.str (sprintf "%A" model) ]
        R.button [ OnClick (fun _ -> dispatch Increment) ] [ R.str "+" ] ]
```

---

## PROGRAM

```fsharp
Program.mkProgram init update view
#if DEBUG
|> Program.withDebugger
|> Program.withHMR
#endif
|> Program.withReact "elmish-app"
|> Program.run
```

---

# QUESTIONS ?

---

# Merci

- http://fable.io
- https://fable-elmish.github.io/elmish/
- https://mangelmaxime.github.io/Fulma/
