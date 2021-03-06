Naše cesta Reactem pokračuje k další z velmi důležitých dovedností. Téměř každý webové aplikace zobrazuje nějaký seznam, ať už to je nákupní košík, seznam divadelních představení, jídelní lístek nebo třeba seznam nepřečtených e-mailů. V této lekci si ukážeme, jak v Rectu zařídit zobrazení seznamu uloženého v nějakém JavaScriptovém poli.

## Opakování metody map

Na zobrazování seznamů budeme v Reactu používat metodu `map`, kterou jste si již vyzkoušeli v předchozí kapitole. Určitě se nám však vyplatí si tuto metodu připomenout a zopakovat.

Metoda `map` slouží k tomu, abychom z jednoho JavaScriptového pole vyrobili pole jiné pomocí nějaké tranformační funkce. Takto například vyrobíme e-maily pro seznam uživatelů.

```js
const names = ['petr', 'jana', 'marek', 'eva', 'lenka', 'ondra'];
names.map((name) => {
  return `${name}@mejlik.cz`;
});
```

### Zkracování zápisu funkcí

Už na ternárním operátoru z minulé kapitoly jsme si ukázali, že programátoři se snaží kód zkracovat kde to jen jde, aby dokázali programy psát co nejrychleji. Pro arrow funkce proto existuje speciální zkrácený zápis. Jako příklad vězměme naší funkci pro vytváření e-mailu. Její nezkrácený zápis vypadá takto.

```js
(name) => {
  return `${name}@mejlik.cz`;
};
```

Pokud však funkce nedělá nic jiného, než že rovnou vrací nějako hodnotu, můžeme její zápis zkrátit tím, že vynecháme složené závorky a slovíčko `return`.

```js
(name) => `${name}@mejlik.cz`;
```

Díky tomu můžeme náš kód pro generování e-mailů zapsat takto.

```js
const names = ['petr', 'jana', 'marek', 'eva', 'lenka', 'ondra'];
names.map((name) => `${name}@mejlik.cz`);
```

### Zkrácený zápis se závorkami

Občas se nám stane, že funkce sice nedělá nic jiného, než že vrací hodnotu, ale tato hodnota se nevejde na jeden řádek. Příkladem budiž například Reactová komponenta.

<!-- prettier-ignore -->
```js
const Time = (props) => {
  return (
    <div className="time">
      <span className="time__hours">{props.hours}</span>
      :
      <span className="time__mins">{props.minutes}</span>
    </div>
  );
}
```

Tuto komponent nedělá nic jiného, než že vrací JSX element. Můžeme ji proto převést na zkrácený zápis. Sluší se ale v zápise ponechat kulaté závorky pro lepší čítelnost.

<!-- prettier-ignore -->
```js
const Time = (props) => (
  <div className="time">
    <span className="time__hours">{props.hours}</span>
    :
    <span className="time__mins">{props.minutes}</span>
  </div>
);
```

Pokud je však komponenta malá, klidně se bez závorek obejdeme.

```js
const User = (username) => <div className="user__name">{username}</div>;
```

Všimněte si, že pomocí takovéto komponenty bychom například mohli vyrobit pole JSX elementů z našeho pole uživatelů.

```js
const names = ['petr', 'jana', 'marek', 'eva', 'lenka', 'ondra'];
names.map((name) => <User username={name} />);
```

Z tohoto bodu zbývá už jen malý krůček, abychom takovéto pole JSX komponent dokázali zobrazit na naší stránce. To si však necháme až na druhou část lekce.

[[[ excs Cvičení: Zkracování a map
- zkracovaci-jednohubky
- opakovani-map
]]]

## Seznamy v JSX

Na konci předchozí části už jsme se dotknuli způsobu, jak v Reactu zobrazit nějaký seznam. Celá myšlenka tkví v tom, že React nám velmi přimočaře umožňuje zobrazit pole JSX elementů.

Představme si následující pole obsahující `li` elementy.

```js
const dayElements = [
  <li>pondělí</li>,
  <li>úterý</li>,
  <li>středa</li>,
  <li>čtvrtek</li>,
  <li>pátek</li>,
];
```

Všimněte si, že jde o normální JavaScriptové pole, které jako svoje prvky obsahuje JSX elementy. Každý JSX element je hodnota, takže není žádný problém mít pole takovýchto hodnot.

Pokud takové pole máme, můžeme tyto elementy zobrazit uvnitř nějakého rodiče prostě tak, že proměnnou `dayElements` do tohoto rodiče prostě vložíme.

```js
const App = () => (
  <>
    <h1>Pracovní dny</h1>
    <ol className="days">{dayElements}</ol>
  </>
);
```

Kdyby byl v proměnné `dayElements` uložen řetězec, budeme mít uprostřed `ol` seznamu prostě kousek textu. Jelikož však máme v `dayElements` pole JSX elementů, React je jednoduše zapojí jako děti našeho seznamu.

Když tento kód spustíme, React nám do konzole prohlížeče vypíše varování.

```
Warning: Each child in a list should have a unique "key" prop.
```

Pro tutu chvíli jej můžeme ignorovat. Později si vysvětlíme co přesně znamená a jak se k němu postavit.

### Použití map

V předchozím případě jsme pole JSX elementů měli připravené dopředu. V praxi jej však chceme vyrobit z nějakých dat. Máme například názvy dní v týdnu jako řetězce v poli.

```js
const days = ['pondělí', 'úterý', 'středa', 'čtvrtek', 'pátek'];
```

Z tohoto pole chceme vyrobit pole JSX elementů pro náš seznam. K tomu nám stačí poučít funkci `map`, kterou jsme v první části lekce tak poctivě trénovali.

```js
const days = ['pondělí', 'úterý', 'středa', 'čtvrtek', 'pátek'];

const dayElements = days.map((day) => <li>{day}</li>);

const App = () => (
  <>
    <h1>Pracovní dny</h1>
    <ol className="days">{dayElements}</ol>
  </>
);
```

Tento kód bude hezky fungovat. Z hlediska profesionáních vývojářů je však zbytečně ukecaný. Proměnnou `dayElements` používáme pouze jednou, takže můžeme na místě jejího použití rovnou zavolat náš mapovací kód.

<!-- prettier-ignore -->
```js
const days = ['pondělí', 'úterý', 'středa', 'čtvrtek', 'pátek'];

const App = () => (
  <>
    <h1>Pracovní dny</h1>
    <ol className="days">
      {
        days.map((day) => <li>{day}</li>)
      }
    </ol>
  </>
);
```

Pokud vám kód výše stále přijde srozumitelný, je zde příležitost jej udělat ještě malinko kompaktnější. V praxi se často setkáte s takovýmo formátováním.

```js
const days = ['pondělí', 'úterý', 'středa', 'čtvrtek', 'pátek'];

const App = () => (
  <>
    <h1>Pracovní dny</h1>
    <ol className="days">
      {days.map((day) => (
        <li>{day}</li>
      ))}
    </ol>
  </>
);
```

Tady už je potřeba si dát zatraceně dobrý pozor na jednotlivé závorky a dobře se orientovat v tom, co která znamená.

### Složitější komponenty

Z praktického hlediska je náš ilustrativní příklad výše stále hodně jednoduchý. V praxi míváme mnohem obsáhlejší data i mnohem složitejší komponenty. Vezměme například data pro nám již známý nákupní seznam.

```js
const list = [
  { name: 'mrkev', amount: '3ks', bought: false },
  { name: 'paprika', amount: '2ks', bought: true },
  { name: 'cibule', amount: '2ks', bought: false },
  { name: 'čínské zelí', amount: '1ks', bought: true },
  { name: 'arašídy', amount: '250g', bought: false },
  { name: 'sojová omáčka', amount: '1ks', bought: false },
];
```

Pokud tento seznam budeme chtít zobrazit, budeme potřebovat složitější JSX strukturu. Například jako v následující ukázce.

```js
const App = () => (
  <>
    <h1>Nákupní sezname</h1>
    <div className="shopping-list">
      {list.map((item) => {
        const itemClass = item.bought ? 'item item--selected' : 'item';
        return (
          <div className={itemClass}>
            <span className="item__name">{item.name}</span>
            <span className="item__amount">{item.amount}</span>
          </div>
        );
      })}
    </div>
  </>
);
```

Všimněte si, že funkce, která vyrábí JSX pro jednotlivé položky našeho seznamu ja už o kus obsáhlejší. Dokonce o tolik, že už nejde napsat zkráceným způsobem, a musíme použít složené závorky a `return`. Tato ukázka opět poslouží jako dobré cvičení na pozornost ohledně závorek.

V praxi však často nastane situace, že funkce použitá uvnitř `map` už je tak složítá, že je těžké se v kódu zorientovat. Náš poslední příklad už je také trochu na hraně. V takovém případě se nám rozhodně vyplatí vytvořit si pro zobrazování jednotlivých prvků seznamu komponentu, například takto.

```js
const ShoppingItem = (props) => {
  const itemClass = props.bought ? 'item item-selected' : 'item';
  return (
    <div className={itemClass}>
      <span className="item__name">{props.name}</span>
      <span className="item__amount">{props.amount}</span>
    </div>
  );
};

const App = () => (
  <>
    <h1>Nákupní sezname</h1>
    <div className="shopping-list">
      {list.map((item) => (
        <ShoppingList
          name={item.name}
          amount={item.amount}
          bought={item.bought}
        />
      ))}
    </div>
  </>
);
```

Takto je náš kód mnohem čitelnější. Toto je jeden z hlavních důvodů, proč rozdělujeme naše aplikace na komponenty. Lépe se tak dokážeme v kódu orientovat nejen my, ale také naší kolegové, kteří pracují na stejném projektu a musí číst náš kód.

[[[ excs Cvičení: Zobrazování seznamů
- ceska-mesta
- ceska-mesta-2
]]]

[[[ excs Doporučené úložky na doma
- ceska-mesta-3
- podcasty
]]]
