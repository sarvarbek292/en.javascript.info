# Type Conversions Yozuv suhbatlari 

Ko'pincha operator va funksiyalar o'zlariga berilgan qiymatlarni to'g'ri turga o'tkazib oladdi. 

Masalan,  `alert`istalgan qiymatni ko'rsatish uchun satr ko'rinishiga o'tkazib oladi. Matematik operatsiyalar esa qiymatlarni raqamlarga aylantirib oladilar. 

There are also cases when we need to explicitly convert a value to the expected type. Ba'zi holatlar borki, qiymatni kutilgan turga aniq ravishda o'tkazishimiz kerak bo'ladi. 

```smart header="Hali obyektlar haqida gapirilayotkani yo'q"
Bu bo'limda obyektlarni qamrab olmaymiz. Hozircha faqat primitive(sodda, o'zgaruvchilar) haqida gaplashamiz. 

Keyinroq, obyektlarni o'rganib bo'lgach, <info:object-toprimitive> bo'limida obyektlar qanday mos kelishini o'rganib chiqamiz. 
```

## Satrli suhbat 

Satr suhbati bizga qiymatning satr shakl mavjud bo'lganda kerak bo'ladi. 

Misol uchun, `alert(value)` qiymatni ko'rsatish uchun buni qiladi. 

Shuningdek, `String(value)` funksiyasini ham qiymatni satrga aylantirishda qo'llasa bo'ladi: 

```js run
let value = true;
alert(typeof value); // boolean

*!*
value = String(value); // hozir qiymat satrda "true" (to'g'ri)
alert(typeof value); // satr
*/!*
```

Satrli suhbat ko'pincha ravshan ko'rinishda bo'ladi. `false`  `"false"` ga o'zgaradi, `null` esa `"null"` ga va boshqalar. 

## Raqamli suhbat 

Raqamli suhbat matematik funksiya va ifodalarda avtomatik ravishda sodir bo'ladi. 

Misol uchun, bo'luv amali `/` raqamsiz ifodalarga foydalanilganda: 

```js run
alert( "6" / "2" ); // 3, satrlar raqamlarga aylantiriladi
```

`Number(value)` funksiyasdan `value`(qiymat)ni raqamga ochiq ravishda aylantirishda foydalana olamiz:

```js run
let str = "123";
alert(typeof str); // satr

let num = Number(str); // 123 raqamiga o'zgaradi

alert(typeof num); // raqam
```

Qiymatni matn shakli kabi satrga asoslangan manbalardan o'qib, lekin raqam kiritilishini kutkanimizda, bizdan aniq va ravshan conversion (aylantirish) talab qilinadi. 

Agar satr haqiqiy raqam bo'lmasa, bunday conversion (aylantirish)ning natijasi `NaN` bo'ladi. Masalan:

```js run
let age = Raqam("raqam o'rniga ixtiyoriy qator");

alert(age); // NaN, konvertatsiya amalga oshmadi
```

Numeric conversion rules: Raqamli konvertatsiya qoidalari:

| Qiymat |  O'zgaradi... |
|-------|-------------|
|`aniqlanmagan`|`NaN`|
|`null`|`0`|
|<code>true&nbsp;and&nbsp;yolg'on</code> | `1` va `0` |
| `string` (satr) | Boshidagi va oxiridagi bo'shliqlar olib tashlanadi. Agar qolgan satr bo'sh bo'lsa, natija `0` bo'ladi. Aks holda, raqam satrdan "o'qiladi". Xatolik `NaN`ni beradi. |

Misollar:

```js run
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN ("z" dagi raqamni o'qishda xatolik)
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```

Shunga e'tibor bering,  `null` va `undefined` bu yerda turlicha harakat qiladi.`null` nolga aylanadi, `undefined` esa`NaN`ga.

Most mathematical operators also perform such conversion, we'll see that in the next chapter. Ko'p matematik operatsiyalar ham bunday knvertatsiyalarni amalga oshiradi, buni keyingi bo'limda ko'rib chiqamiz. 

## Boolean Conversion

Boolean conversion is the simplest one.

It happens in logical operations (later we'll meet condition tests and other similar things) but can also be performed explicitly with a call to `Boolean(value)`.

The conversion rule:

- Values that are intuitively "empty", like `0`, an empty string, `null`, `undefined`, and `NaN`, become `false`.
- Other values become `true`.

For instance:

```js run
alert( Boolean(1) ); // true
alert( Boolean(0) ); // false

alert( Boolean("hello") ); // true
alert( Boolean("") ); // false
```

````warn header="Please note: the string with zero `\"0\"` is `true`"
Some languages (namely PHP) treat `"0"` as `false`. But in JavaScript, a non-empty string is always `true`.

```js run
alert( Boolean("0") ); // true
alert( Boolean(" ") ); // spaces, also true (any non-empty string is true)
```
````

## Summary

The three most widely used type conversions are to string, to number, and to boolean.

**`String Conversion`** -- Occurs when we output something. Can be performed with `String(value)`. The conversion to string is usually obvious for primitive values.

**`Numeric Conversion`** -- Occurs in math operations. Can be performed with `Number(value)`.

The conversion follows the rules:

| Value |  Becomes... |
|-------|-------------|
|`undefined`|`NaN`|
|`null`|`0`|
|<code>true&nbsp;/&nbsp;false</code> | `1 / 0` |
| `string` | The string is read "as is", whitespaces from both sides are ignored. An empty string becomes `0`. An error gives `NaN`. |

**`Boolean Conversion`** -- Occurs in logical operations. Can be performed with `Boolean(value)`.

Follows the rules:

| Value |  Becomes... |
|-------|-------------|
|`0`, `null`, `undefined`, `NaN`, `""` |`false`|
|any other value| `true` |


Most of these rules are easy to understand and memorize. The notable exceptions where people usually make mistakes are:

- `undefined` is `NaN` as a number, not `0`.
- `"0"` and space-only strings like `"   "` are true as a boolean.

Objects aren't covered here. We'll return to them later in the chapter <info:object-toprimitive> that is devoted exclusively to objects after we learn more basic things about JavaScript.
