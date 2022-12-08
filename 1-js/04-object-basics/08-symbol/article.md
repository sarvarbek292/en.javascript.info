
# Symbol turi

Spetsifikatsiyaga ko'ra, ob'ekt xossalari kalitlari satr turida yoki Symbollar turida bo'lishi mumkin. Raqamlar emas, mantiqiy emas, faqat satrlar yoki Symbollar, shu ikki turda bo'ladi.

Hozirgacha faqat satrlardan foydalanayotkan edik. Keling endi, Symbollar bizga qanday foyda keltirishi mumkinligini ko'rib chiqamiz.

## Symbollar

"Symbol" noyob aniqlovchini ifodalaydi.

A value of this type can be created using `Symbol()`:

```js
// id yangi Symboldir
let id = Symbol();
```

Yaratilgandan so'ng, Symbolga tavsif berish mumkin(shuningdek Symbol vaqti ham deb ataladi), va ayniqsa debugging maqsadlarida foydali bo'ladi: 

```js
// id - "id" tavsifiga ega Symbol
let id = Symbol("id");
```

Symbol o'ziga hos bo'lishi kafolatlanadi. Xatto bir xil tavsif bilan ko'plab Symbol yaratsak ham, ular har xil qiymatda bo'ladi. Tavsif shunchaki yorliq bo'lib, hech narsaga ta'sir qilmaydi.


Misol uchun, bu yerda bir xil tavsifga ega ikkita symbol bor - ular teng emas:

```js run
let id1 = Symbol("id");
let id2 = Symbol("id");

*!*
alert(id1 == id2); // false
*/!*
```

Agar Ruby yoki qandaydir "symbol"lari bo'lgan boshqa bir til bilan tanish bo'lsangiz -- yanglishmang. JavaScript belgilari boshqacha.

````warn header="Symbol lar avtomatik ravishda string ga aylantirilmaydi"
JavaScript dagi aksariayat qiymatlar string ga yashirin o'tishni qo'llaydi. Misol uchun, biz deyarli barcha qiymatni `alert` qila olamiz, va bu ishlaydi. Symbol lar maxsusdir. Ular avtomatik ravishda o'zgarmaydi. 

Misol uchun, ushbu `alert` xatolikni ko'rsatib beradi:

```js run
let id = Symbol("id");
*!*
alert(id); // TypeError: Symbol qiymatni string ga aylantirib bo‘lmaydi
*/!*
```

Bu chalkashlikllarni oldini olish uchun "language guard" (til qo'riqchisi) dir, chunki string lar va symbol lar tubdan farq qiladi va ular tasodifan bir-biriga aylantirilmasligi kerak.

Agar symbol ni ko'rsatmoqchi bo'lsak, unda `.toString()` ni ochiq tarzda chaqirishimiz kerak bo'ladi, xuddi shunga o'xshab:
```js run
let id = Symbol("id");
*!*
alert(id.toString()); // Symbol(id), endi u ishlaydi
*/!*
```

Yoki faqat tavsifni ko'rsatish uchun "symbol.description" xususiyatini olinadi:
```js run
let id = Symbol("id");
*!*
alert(id.description); // id
*/!*
```

````

## "Hidden" (Yashirin) xususiyatlar

Symbol lar bizga ob'ektning kodning boshqa bir qismi tasodifan kira olmaydigan va ustiga  yoza olmaydigan "yashirin" xususiyatlar yaratish imkonini beradi. 

Misol uchun, agar `user` ob'ektlari bilan foydalanayotkan bo'lsak, ular uchinchi qism kodiga tegishli. Biz ularga aniqlovchialrni qo'shmoqchimiz.

Buning uchun symbol kalitidan foydalanamiz:

```js run
let user = { // boshqa kodga tegishli
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // Ma'lumotlarga symbol dan kalit sifatida foydalanib kirish mumkin
```

`Symbol("id")` ni `"id"` stringi ustidan foydalanishning foydasi nima?

`user` ob'ektlari boshqa kodga tegishli bo'lgani uchun va bu kod ular bilan ham ishlaganligi uchun, unga shunchaki maydon qo'shmasligimiz kerak bo'ladi. Bu xavfli ish. Ammo symbolga tasodifan kirish mumkin emas, uchinchi tomon kodi uni hatto ko'rmasligi ham mumkin, shuning uchun buni qilish to'g'ri bo'lishi ham mumkin.

Bundan tashqari, tasavvur qiling, boshqa skript o'z maqsadlari uchun `user` ichida o'z identifikatoriga ega bo'lishni xohlayapti. Bu boshqa bir JavaScript kutubxonasi bo'lishi mumkin, shuning uchun skriptlar bir-biridan mutlaqo bexabar holatda.

Keyin bu skript o'zining "Symbol("id")' ni yaratishi mumkin, masalan:

```js
// ...
let id = Symbol("id");

user[id] = "Their id value";
```

Bizning va ularning identifikatorlari o'rtasida hech qanday ziddiyat bo'lmaydi, chunki symbol lar bir xil nomga ega bo'lsa ham, har doim farq qiladi.

...Ammo bir xil maqsad uchun symbol o'rniga `"id"` string idan foydalansak, U holda u yerda ziddiyat *bo'lardi*:
```js
    
let user = { name: "John" };

// Bizning skriptimiz "id" xususiyatidan foydalanadi
user.id = "Our id value";

// ...Boshqa skript ham o'zining maqsadlari uchun "id" dan foydalanishni xohlaydi... 

user.id = "Their id value"   
// Bom! boshqa skript tomonidan yozilgan!
```

### Ob'ekt harfidagi symbol lar

Agar biz `{...}` ob'ektida symboldan foydalanmoqchi bo'lsak, uning atrofida kvadrat qavslar kerak.

Shunga o'xshash:

```js
let id = Symbol("id");

let user = {
  name: "John",
*!*
  [id]: 123 // "id" emas: 123
*/!*
};
```
Buning sababi, kalit sifatida bizga "id" string emas, balki "id" o'zgaruvchisi qiymati kerak.

### Symbols for..in tomonidan tashlab ketiladi 

Symbolic xususiyatlar `for..in` siklida qatnashmaydi.

Misol uchun:
```js run
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

*!*
for (let key in user) alert(key); // ism, yosh (symbol larsiz)
*/!*

// symbol orqali to'g'ridan to'g'ri kirish ishlaydi
alert( "Direct: " + user[id] );
```

[Object.keys(user)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) ham ularga e'tibor bermaydi. Bu umumiy "symbolic xususiyatlarni yashirish" tamoyilining bir qismidir. Agar boshqa skript yoki kutubxona ob'ektimiz ustidan aylansa, u kutilmaganda symbolic xususiyatga kira olmaydi.

Bundan farqli ravishda, [Object.assign](mdn:js/Object/assign) ham string, ham symbol xususiyatlarini nusxalaydi:
```js run
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

Bu yerda hech qanday paradoks yo'q, bu shunchki dizayn bo'yicha. G'oya shundan iboratki, biz ob'ektni klonlash yoki ob'ektlarni birlashtirganda, biz odatda *barcha* xususiyatlarni (jumladan, `id` kabi symbol larni) ham nusxalashni xohlaymiz.

## Global symbol lar

Ko'rib turganimizdek, odatda barcha belgilar bir xil nomga ega bo'lsa ham, har xil bo'ladi. But sometimes we want same-named symbols to be same entities. For instance, different parts of our application want to access symbol `"id"` meaning exactly the same property.

Bunga erishish uchun *global symbol registr*i mavjud. Unda symbol lar yaratib, ularga keyinroq kirsa bo'ladi va u bir xil nom bilan qayta-qayta kirirish  ayni bir xil symbolga qaytishni kafolatlaydi. 

Ro'yxatga olish kitobidan belgini o'qish (agar yo'q bo'lsa yaratialdi) uchun `Symbol.for(key)` dan foydalaning.

Ushbu chaqiruv global registrni tekshiradi va agar `key` sifatida tasvirlangan symbol bo'lsa, uni qaytaradi, aks holda yangi `Symbol(key)` symbolni yaratadi va uni berilgan `kalit` bo'yicha registrda saqlaydi.

Masalan:

```js run
// global registrdan o'qing
let id = Symbol.for("id"); // agar symbol mavjud bo'lmasa, u yaratiladi

// uni yana o'qing (kodning boshqa qismidan bo'lishi mumkin)
let idAgain = Symbol.for("id");

// bir xil symbol
alert( id === idAgain ); // to'g'ri
```

Ro'yxatga olish ichidagi symbol lar *global belgilar* deb ataladi. Agar biz kodning hamma joyidan foydalanish mumkin bo'lgan keng ko'lamli dastur symbol ga ega bo'lishni istasak - ular aynan shu maqsadda.

```smart header="Bu Rubyga o'xshab ketadi"
In some programming languages, like Ruby, there's a single symbol per name.

In JavaScript, as we can see, that's right for global symbols.
```

### Symbol.keyFor

For global symbols, not only `Symbol.for(key)` returns a symbol by name, but there's a reverse call: `Symbol.keyFor(sym)`, that does the reverse: returns a name by a global symbol.

For instance:

```js run
// get symbol by name
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// get name by symbol
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

The `Symbol.keyFor` internally uses the global symbol registry to look up the key for the symbol. So it doesn't work for non-global symbols. If the symbol is not global, it won't be able to find it and returns `undefined`.

That said, any symbols have `description` property.

For instance:

```js run
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, global symbol
alert( Symbol.keyFor(localSymbol) ); // undefined, not global

alert( localSymbol.description ); // name
```

## System symbols

There exist many "system" symbols that JavaScript uses internally, and we can use them to fine-tune various aspects of our objects.

They are listed in the specification in the [Well-known symbols](https://tc39.github.io/ecma262/#sec-well-known-symbols) table:

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- ...and so on.

For instance, `Symbol.toPrimitive` allows us to describe object to primitive conversion. We'll see its use very soon.

Other symbols will also become familiar when we study the corresponding language features.

## Summary

`Symbol` is a primitive type for unique identifiers.

Symbols are created with `Symbol()` call with an optional description (name).

Symbols are always different values, even if they have the same name. If we want same-named symbols to be equal, then we should use the global registry: `Symbol.for(key)` returns (creates if needed) a global symbol with `key` as the name. Multiple calls of `Symbol.for` with the same `key` return exactly the same symbol.

Symbols have two main use cases:

1. "Hidden" object properties.
    If we want to add a property into an object that "belongs" to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in `for..in`, so it won't be accidentally processed together with other properties. Also it won't be accessed directly, because another script does not have our symbol. So the property will be protected from accidental use or overwrite.

    So we can "covertly" hide something into objects that we need, but others should not see, using symbolic properties.

2. There are many system symbols used by JavaScript which are accessible as `Symbol.*`. We can use them to alter some built-in behaviors. For instance, later in the tutorial we'll use `Symbol.iterator` for [iterables](info:iterable), `Symbol.toPrimitive` to setup [object-to-primitive conversion](info:object-toprimitive) and so on.

Technically, symbols are not 100% hidden. There is a built-in method [Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols) that allows us to get all symbols. Also there is a method named [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys) that returns *all* keys of an object including symbolic ones. So they are not really hidden. But most libraries, built-in functions and syntax constructs don't use these methods.
