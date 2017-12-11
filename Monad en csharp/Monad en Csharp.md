---
theme: "white"
customTheme: "evilzslide"
---
<!-- background: #2d2422-->
<!-- color: #fff4e3 -->
<!-- font: univers -->

#Monad en Csharp

---


## A propos

Typewriter etsy messenger bag [fingerstache](), aesthetic vinyl semiotics twee **DIY** forage chillwave. Thundercats ennui messenger bag, squid carles chillwave shoreditch pickled cliche **letterpress**. DIY beard locavore occupy salvia, whatever single-origin ==coffee== fanny pack 3 wolf moon typewriter gastropub1 kale H20 chips. Ennui keffiyeh thundercats jean shorts biodiesel. Terry richardson, swag blog locavore umami vegan helvetica. Fingerstache kale chips.

<footer>
<p>DIY beard locavore <i>occupy</i> salvia, whatever single-origin <code>coffee</code> fanny pack 3 wolf moon <a href="">typewriter</a> gastropub<sup>1</sup> kale H<sub>2</sub>0 chips. Ennui <strong>keffiyeh</strong> thundercats jean <em>shorts</em> biodiesel. Terry richardson, swag blog locavore umami <b>vegan</b> helvetica. Fingerstache kale chips.</p>
</footer>

---

## Design pattern

Les **design pattern** sont des repetition de modeles, certain sont integre directment dans le coeur du langage : **functions, variables, types ...**

Le **pattern Monad** est un autre pattern que l'on utilise sur des types.
Le **Monad** est un type sur lequel le pattern est utilise

---

## Monad en CSharp

Il existe deja plusieurs Monad dans le framework .net :

`Nullable<T>`
`Func<T>`
`Lazy<T>`
`Task<T>`
`IEnumerable<T>`

---

## Generic

Tous ces Monad fonctionnent sur de la meme facon sur le type generique utilise*.

La monad va **amplifier** le type generique, c'est a dire ajouter de nouvelles fonctionnalites

> Le type `Nullable<T>` ne permet pas les T de type classe.

---

## Cree une instance de Monad

### Wrapper une valeur

```
static Nullable<T> CreateSimpleNullable<T>(T item)
{
    return new Nullable<T>(item);
}

static OnDemand<T> CreateSimpleOnDemand<T>(T item)
{
   return ()=>item;
}

static IEnumerable<T> CreateSimpleSequence<T>(T item)
{
    yield return item;
}

```

---

## Unwrapper

la monad permet contient un getter permettant de recuperer la valeur d'origine ou de la creer.

```
new Nullable<T>(5).Value;
```

On peut ensuite rewrapper facilement cette valeur.

---

## Regle 1

Le pattern Monad defini toujours un moyen simple de cree a partir d une valeur du type generic qui est augmente la monad.

```
static M<T> CreateSimpleM<T>(T value)
```

---

## Regle 2

Lors de l'utilisation d'une methode modifiant la valeur wrappe, cette methode doit renvoyer la nouvelle valeur dans la monad du meme type.

```
static M<R> ApplySpecialFunction<A, R>(
  M<A> wrapped,
  Func<A, M<R>> function)
```

---

## Regle 3

La composition doit pouvoir s'appliquer :

```
Func<X, M<Y>> f = whatever;
Func<Y, M<Z>> g = whatever;
M<X> mx = whatever;
M<Y> my = ApplySpecialFunction(mx, f);
M<Z> mz1 = ApplySpecialFunction(my, g);
Func<X, M<Z>> h = ComposeSpecial(f, g);
M<Z> mz2 = ApplySpecialFunction(mx, h);
```


## Resume

Une monad est un type generic  `M<T>`
Il peut etre construit a partir d'une valeur du type wrapper 
 `T -> M<T>`

Il est possible de modifier la valeur d'origine en utili
Also there is some way of applying a function that takes the underlying type to a monad of that type. We’ve been characterizing this as a method with signature:
static M<R> ApplySpecialFunction<A, R>(
  M<A> monad, Func<A, M<R>> function)
Finally, both these methods must obey the monad laws, which are:
Applying the construction function to a given instance of the monad produces a logically identical instance of the monad.
Applying a function to the result of the construction function on a value, and applying that function to the value directly, produces two logically identical instances of the monad.
Applying to a value a first function followed by applying to the result a second function, and applying to the original value a third function that is the composition of the first and second functions, produces two logically identical instances of the monad.


---
