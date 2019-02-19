---
id: introducing-jsx
title: Wprowadzenie do JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

Weźmy na warsztat poniższą instrukcję przypisania:

```js
const element = <h1>Hello, world!</h1>;
```

Nieco zaskakującą może się tutaj wydawać przypisywana wartość,
 która nie jest ani typowym łańcuchem znaków, ani HTML-em.

Taką składnię nazywamy JSX i jest to rozszerzenie składni JavaScriptu 
o możliwość wstawiania znaczników. Zalecamy używanie jej wraz z Reactem do opisywania,
jak powinien wyglądać interfejs graficzny strony.
JSX może przypominać język oparty o szablony, jednakże daje on do dyspozycji
pełnię możliwości JavaScriptu.

JSX jest tłumaczony jeden do jednego na reactowe "elementy". Sposoby ich
wyświetlania na stronie poznamy w [następnym podrozdziale](/docs/rendering-elements.html).
Poniżej znajdziesz podstawy składni JSX, które pozwolą ci szybko zacząć tworzyć kod.


### Dlaczego JSX? {#why-jsx}

React godzi się z faktem, że logika związana z prezentacją danych jest z natury 
rzeczy powiązana z innymi elementami logiki biznesowej UI: sposobami przetwarzania
zdarzeń w aplikacji, tym, jak stan aplikacji zmienia się w czasie, jak również tym,
jak dane są przygotowywane do wyświetlenia. 

Zamiast sztucznie rozdzielać *technologie*, umiejscowiając znaczniki oraz logikę 
aplikacji w osobnych plikach, React wprowadza [podział odpowiedzialności](https://pl.wikipedia.org/wiki/Zasada_jednej_odpowiedzialno%C5%9Bci) 
poprzez wprowadzenie luźno powiązanych jednostek, nazywanych "komponentami", które
zawierają zarówno znaczniki HTML, jak i związaną z nimi logikę.

React [nie wymaga](/docs/react-without-jsx.html) używania JSX, jednakże większość
programistów uważa go za przydatne narzędzie unaoczniające to, co dzieje się w kodzie
javascriptowym operującym na interfejsach graficznych. Pomaga on również Reactowi 
wyświetlać bardziej użyteczne informacje o błędach i ostrzeżenia. 

Pora sprawdzić, jak to działa - do dzieła!

### Osadzanie wyrażeń w JSX {#embedding-expressions-in-jsx}

W poniższym przykładzie deklarujemy zmienną `imie`, a następnie używamy jej 
wewnątrz JSX, po prostu otaczając ją nawiasami klamrowymi:

```js{1,2}
const imie = 'Gracjan';
const element = <h1>Witaj, {imie}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

Zauważ, że dowolne [wyrażenie JavaScriptowe (ang.)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)
możesz osadzić w JSX poprzez otoczenie go klamrami. Przykładowo, możesz użyć 
`2 + 2`, `uzytkownik.imie` czy `pokoloruj(uzytkownik)`, jako że są to prawidłowe
wyrażenia w języku JavaScript. 

W poniższym przykładzie umiejscawiamy wynik wywołania funkcji javascriptowej 
`formatuj(uzytkownik)` wewnątrz znacznika `<h1>`:


```js{12}
function formatuj(uzytkownik) {
  return uzytkownik.imie + ' ' + uzytkownik.nazwisko;
}

const uzytkownik = {
  imie: 'Gracjan',
  nazwisko: 'Brzęczyszczykiewicz',
};

const element = (
  <h1>
    Witaj, {formatuj(uzytkownik)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://introducing-jsx)

Dla czytelności, podzieliliśmy kod JSX na kilka linii. Nie jest to wprawdzie 
wymagane, ale polecamy również otaczanie go w nawiasy. Pozwoli to uniknąć pułapek
związanych z [automatycznym wstawianiem średników (ang.)](http://stackoverflow.com/q/2846283) w JavaScripcie.

### JSX również jest wyrażeniem {#jsx-is-an-expression-too}

Po kompilacji, wyrażenia JSX-owe stają się zwykłymi wywołaniami funkcji w JavaScripcie
i są ewaluowane do obiektów JavaScriptowych.

Oznacza to, że możesz używać znaczników JSX również wewnątrz instrukcji warunkowych `if`,
pętli `for`, przypisywać je do zmiennych, oczekiwać przekazania jako argumenty
do funkcji, zwracać z funkcji i w wielu innych miejscach.

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Podawanie atrybutów w JSX {#specifying-attributes-with-jsx}

Możesz przekazać literał znakowy jako atrybut, używając poniższej składni:

```js
const element = <div tabIndex="0"></div>;
```

Możesz także przypisać wartość dowolnego wyrażenia javascriptowego do atrybutu, używając nawiasów
klamrowych:

```js
const element = <img src={user.avatarUrl}></img>;
```

Nie otaczaj jednak klamr cudzysłowami, gdy chcesz przekazać wartość jakiegoś 
wyrażenia do atrybutu. Zalecamy użycie albo cudzysłowów (dla statycznych ciągów 
znaków), albo klamr (dla wartości wyrażeń), ale nie obydwu naraz (dla tego samego
atrybutu). 


>**Uwaga:**
>
> Jako że składni JSX jest bliżej do JavaScriptu niż do HTML-a, React DOM używa do nazywania 
> argumentów notacji `camelCase` zamiast nazw atrybutów HTML-owych.
>
> Przykładowo, w JSX `class` staje się [`className`](https://developer.mozilla.org/pl/docs/Web/API/Element/className),
> zaś zamiast `tabindex` używamy `tabIndex`. 
>

### Specyfikowanie elementów potomnych w JSX {#specifying-children-with-jsx}

Jeśli znacznik jest pusty, możesz zakończyć go bezpośrednio przy pomocy `/>`,
podobnie jak ma to miejsce w XML-u:

```js
const element = <img src={user.avatarUrl} />;
```

Znaczniki JSX mogą jednak również zawierać elementy potomne:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### JSX jest odporny na ataki przez wstrzyknięcie kodu {#jsx-prevents-injection-attacks}

W JSX można bezpiecznie osadzać dane wprowadzone przez użytkownika:

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

Domyślnie, React DOM stosuje [znaki ucieczki](https://pl.wikipedia.org/wiki/Znak_modyfikacji)
w wartościach używanych w JSX, zanim je wyświetli. W ten sposób upewniamy się,
że nic, co nie zostało bezpośrednio napisane w kodzie twojej aplikacji, nie zostanie
wstrzyknięte w kod strony. Wszystko, co wyświetlamy, zamieniane jest na statyczne
łańcuchy znaków. Pozwala to uniknąć ataków typu [XSS (Cross-site scripting)](https://pl.wikipedia.org/wiki/Cross-site_scripting). 


### JSX reprezentuje obiekty {#jsx-represents-objects}

Zanim twój kod JSX będzie mógł zostać wyświetlony w przeglądarce, musi zostać 
odpowiednio przetworzony przez narzędzie takie jak _Babel_. Babel transpiluje 
znaczniki JSX do wywołań funkcji `React.createElement`. 

Poniższe dwa fragmenty kodu są sobie równoważne:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

Funkcja `React.createElement` wykonuje kilka sprawdzeń, które ułatwiają pisanie
kodu wolnego od błędów. W swej istocie jednak, zwraca ona po prostu obiekt 
podobny do poniższego:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

Takie obiekty nazywamy "_elementami reactowymi_". Możesz o nich myśleć jak o sposobie
opisania tego, co chcesz zobaczyć na ekranie. `ReactDOM` odczytuje te obiekty 
i używa ich do skonstruowania drzewa DOM twojej strony, jak również do późniejszego
odświeżania go. 

Dowiesz się więcej o wyświetlaniu elementów reactowych w następnym rozdziale.

>**Wskazówka:**
>
> Polecamy używanie ["Babela" jako definicji języka (ang.)](http://babeljs.io/docs/editors)
> w twoim ulubionym edytorze. Pozwoli to na poprawne podświetlanie składni ES6 i JSX.
> Niniejsza strona używa schematu kolorów [Oceanic Next](https://labs.voronianski.com/oceanic-next-color-scheme/),
> który jest z nią kompatybilny.
>
