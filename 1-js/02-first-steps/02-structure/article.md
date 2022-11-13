# Kod tuzilishi

Biz o'rganadigan birinchi narsa kodning qurilish bloklari bo'ladi.
## Statements

Statementlar sintaksis tuzilmalar va amallarni bajaradigan buyruqlardir.

Biz  allaqachon `alert('Hello, world!')`, statementini ko'rdik va bu "Salom Dunyo" (Hello, world!) habarini ko'rsatadi.

Biz kodlarda o'zimiz istagancha ko'p statementlar qo'yishimiz mumkin. Statementlarni nuqtali vergul(;) bilan ajratib qo'ysa ham bo'ladi.

Masalan  bu yerda, biz "Salom Dunyo" (Hello World) ni ikkita alertga ajratamiz:

```js run no-beautify
alert('Hello'); alert('World');
```

Odatda statementlar o'qishga oson bo'lishi uchun ikkita alohida qatorlarda yoziladi:

```js run no-beautify
alert('Hello');
alert('World');
```

## Nuqtali vergullar [#semicolon]

Satr uzilishi mavjud bo'lgan ko'p hollarda nuqtali vergul tushirib qoldirilishi ham mumkin.

Bunday holatda ham u ishlay oladi:

```js run no-beautify
alert('Hello')
alert('World')
```

Buyerda JavaScript buzilgan satrni "yashirin" (implicit) nuqtali vergul dep tushunadi. Bu nuqtali vergulni automatik joylashtirish dep ataldi [automatic semicolon insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion).

**Ko'p hollarda yangi qator nuqtali vergul o'rnida bo'la oladi, lekin har doim ham emas!** 

Ba'zi hollar borki, yangi qator nuqtali vergul ma'nosini bildirmaydi, masalan: 

```js run no-beautify
alert(3 +
1
+ 2);
```

Kod `6` raqamini chiqaradi chunki buyerda JavaScript nuqtali vergulni qo'shmaydi. Shu narsa aniqki agar qator plus `"+"` belgisi bilan tugasa, demak bu "tugallanmagan ifoda" (uncompeted expression)ni bildiradi, shuning uchun nuqtali vergul bu yerda xato bo'lar edi. Bunday vaziyatda, bu maqsadga muvofiq ishlaydi. 

**Lekin shunday vaziyatlar bo'ladiki JavaScript juda kerak bo'lgan paytida nuqtali vergulni faraz qilishda muvofaqqiyatsizlikka uchraydi**

Bunday vaziyatda yuzaga keladigan xatolarni topish va tuzatish ancha mushkul.

````smart header="Xatoga misol"
Agar shunaqa xatoning aniq misolini ko'rishga qiziqayotkan bo'lsangiz, ushbu kodni sinab ko'ring:

```js run
alert("Hello");

[1, 2].forEach(alert);
```

Hozircha qavs `[]` va `forEach` ning ma'nolarini o'ylab o'tirishga xojat yo'q. Biz ularni keyinroq o'rganib chiqamiz. Hozircha, kodni ishga tushirishdan keyingi natija yodingizda qolsin: u `Hello`, keyin `1`, keyin `2` ni ko'rsatadi.

Keling endi `alert` dan keyin nuqtali vergulni olib tashlaymiz:   

```js run no-beautify
alert("Hello")

[1, 2].forEach(alert);
```

Yuqoridagi kod bilan bu kodning farqi faqat bittagina belgida: Birinchi qatorning oxiridagi nuqtalivergul hozir g'oyib bo'lgan.
Agar biz bu kodni ishga tushirsak, faqatkina birinchi `Hello` ko'rinadi(va bu yerda xatolik mavjud, buni ko'rish uchun konsolni ochishingiz kerak bo'lishi mumkin). Bu yerda raqamlar endi mavjud emas.


Buning sababi shundaki JavaScript to'rtburchak kavs`[...]`lardan oldin nuqtalivergulni faraz qila olmaydi. Shuning uchun oxirgi misoldagi kod yagona statement dep qaraladi.

Quyidagi, (engine)enjinni qanday ishlashi:

```js run no-beautify
alert("Hello")[1, 2].forEach(alert);
```
G'alati ko'rinyapti to'g'rimi? Ushbu holatdagi birlashib ketish shunchaki xato hisoblanadi. Biz kodni to'g'ri ishlashi uchun  `alert` dan kegin nuqtali vergul qo'yishimiz kerak.


Bunday vaziyat boshqa holatlarda ham kuzatilishi mumkin. 
````

Biz xattoki statementlar alohida yangi qatorlar bilan ajratilgan bo'lsa ham ularning o'rtasiga nuqtali vergul qo'yishni tavsiya qilamiz. Yana bir marta ta'kidlab o'tamiz: -- ko'p holatlarda nuqtali vergullarni tushirib qoldirish *mumkin*. Lekin ulardan foydalanish xavfsizroq -- ayniqsa yangi boshlaganlar uchun.


## Izohlar [#kod-izohlari]

Vaqt o'tishi bilan dasturlar tobora mukammallashib  boradi va kodning ishlash vazifalari va sabablarini ko'rsatib beradigan *izohlar* qo'shish zarur bo'ladi.

Izohlar scriptning istalgan joyiga qo'yilishi mumkin. Ular amallarga ta'sir qilmaydi chunki enjinlar ularni e'tiborga olmaydi.

**Bir qatorli izohlar ikkitalik slash  `//` belgilari bilan boshlanadi.**

Satrning qolgan qismi izohdir. U butun qatorni egallashi yoki statementdan kegin kelishi mumkin. 

Quyidagiga o'xshash:
```js run
// Bu izoh o'zining qatorini band qiladi
alert('Hello');

alert('World'); // Bu izoh esa statementdan keyin keladi
```

**Ko'p qatorli izohlar slash va yulduzcha  <code>/&#42;</code> belgisi bilan boshlanadi va yulduzcha va slash <code>&#42;/</code> belgilari bilan tugaydi .**

Quydagiga o'xshash:

```js run
/*  Bu ikkita habarga ega misol.
Bu ko'p qatorli izoh. 
*/
alert('Hello');
alert('World');
```

Izohlardagi tarkibiy qism e'tiborga olinmaydi, shuning uchun <code>/&#42; ... &#42;/</code> ning ichiga kod yozsangiz u amalga oshmaydi.

Ba'zida kodning bir qismini vaqtinchalik ishdan chiqarish qulay bo'lishi mumkin:

```js run
/* Commenting out the code Kodni sharhlash
alert('Hello');

*/
alert('World');
```

```smart header="Use hotkeys!"
Ko'p muharrirlarda kod qatoriga `key:Ctrl+/` tugmasini bosish orqali izoh bersa bo'ladi (bir qatorli izohga), `key:Ctrl+Shift+/` --ko'p qatorli izohlarga (kodni bir qismini tanlang va tugmalarni bosing). Mac lar uchun `key:Ctrl` ning o'rniga `key:Cmd` ni va `key:Shift` ning o'rniga `key:Option` ni sinab ko'ring. 
```

````Ogohlantiruvchi header = "Ichki izohlar qo'llab quvvatlanmaydi!"
Biror `/*...*/` ning ichida boshqa bir `/*...*/` bo'lishi mumkin emas.

Bunday kod xato bilan ishlamay qoladi:

```js run no-beautify
/*
  /* ichki izoh ?!? */
*/
alert( 'World' );
```
````

Kodingizga izoh berishda hech ikkilanmang.

Izohlar kodlarning umumiy hajmini oshiradi, lekin bu hech ham muammo tug'dirmaydi. Chunki, kodlarni mahsulot server (production server)ga chiqarishdan oldin ularni kichiklashtiradigan ko'plab vositalar mavjud. Ular izohlarni olib tashlaydi, shu sabab ishlab turgan scriptlarda paydo bo'lmaydi. Bu degani, izohlarning mahsulot(production)larda hech qanday salbiy ta'siri yo'q. 

O'quv-qo'llanmaning kelgusi qismlarining birida <info:code-quality> bo'limi bo'ladi, bu ham yaxshiroq izoh yozish xususida tushuntirib beradi. 
