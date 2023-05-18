# Functional Programming
By:NSLog0
From : https://algorithml.io/%E0%B8%AA%E0%B8%AD%E0%B8%99-functional-programming-%E0%B9%81%E0%B8%9A%E0%B8%9A%E0%B8%A5%E0%B8%B0%E0%B9%80%E0%B8%AD%E0%B8%B5%E0%B8%A2%E0%B8%94-881efd04d1b3

# Introduction
บทความนี้สอนการเขียนโปรแกรมในแบบ Functional Programming (FP) และเทคนิคต่างๆ ที่จำเป็นต้องรู้ … Functional Programming นั้นคือการเขียนโปรแกรมโดยการเอาเทคนิคการทำ Composition (การประกอบร่าง) มาใช้ควบคู่กับ Pure function โดยข้อควรจำคือต้องหลีกเลี่ยงการ Share state, การแก้ไขข้อมูล (Mutate Data) และการเขียนโค้ดที่เกิด Side effect. สำหรับการเขียนโค้ดเราจะเขียนแบบ Declarative style แทนการเขียนแบบ Imperative Style

ในบทความนี้ผมจะมาพูดถึงหัวข้อต่างๆ ต่อไปนี้ เพื่อให้เกิดความเข้าใจในการทำ Functional Programming มากขึ้น และในบทความนี้ผมเลือก JavaScript มาเขียนให้ดู เพราะเป็นภาษาที่อ่านเข้าใจง่ายแม้ว่าจะไม่เคยเขียน เพราะยังไงเชื่อว่าทุกคนเคยเขียน C หรือ Java มาอยู่แล้ว และในบทความนี้ผมใช้ JavaScript ES6 เป็นหลัก สำหรับคนที่เขียนภาษา Functional อื่นๆ อยู่สามารถอ่านเอาหลักการไปใช้ต่อได้เหมือนกัน

 * Pure function
 * Impure function
 * Shared state
 * Declarative vs Imperative style
 * Composition
 * Currying
 * Partial
 * Curry and Partial in Mathematic
 * Point-free programming
 * Avoiding Mutate Data, Immutability
 * Avoiding Side effect

# Pure function
การเขียนคอมพิวเตอร์ฟังก์ชันโดยการเอาแนวคิดของฟังก์ชันทางคณิตศาสตร์มาใช้ในการประมวลผลข้อมูล โดยปกติจะมีกฏควรจำให้ขึ้นใจ 2 ข้อคือ:

Its return value is the same for the same arguments กล่าวคือค่าที่ส่งออกมาจากฟังก์ชัน (return value) ต้องเป็นค่าเดิมเสมอ ถ้าส่งค่า Input เดิมเข้าไป
Its evaluation has no side effects กล่าวคือฟังก์ชันใดๆ ที่สร้างขึ้นมาต้องไม่มีการเปลี่ยนแปลง State หรือของด้านนอกฟังก์ชัน หรือพูดง่ายๆ ว่าฟังก์ชันที่สร้างมาจะต้องไม่แก้ไขตัวแปรด้านนอกฟังก์ชัน เพื่อไม่ให้กระทบกับการทำงานของฟังก์ชันอื่น
ก่อนอื่นมาดูวิธีการเขียนฟังก์ชันทางคณิตศาสตร์กันก่อน แล้วค่อยเริ่มเขียนโค้ดกันเพื่อขยายความเรื่องของกฏการเขียน Pure function ในคอมพิวเตอร์​

# โจทย์
```
Given f(x) = 2x - 1, find f(2)
f(2) = 2(2) - 1 
     = 4 - 1
     = 3
     ∴ f(2) = 3
```
จากตัวอย่าง f(x) = 2x — 1 เป็น Pure function กล่าวคือเราหากแทนค่าf(2) สักสิบรอบคำตอบที่ได้ยังคงเป็น 3 (same input, get same output) และไม่มีการไปเรียกตัวเลขที่อยูนอก f(x) มาทำงานได้เลย (no side-effect ) เพราะโจทย์กำหนดแล้วว่าขั้นตอนคือต้องนำ 2x — 1 มาหาคำตอบเท่านั้น

หากเราเขียนการทำงานของฟังก์ชันทางคณิตศาสตร์ด้วย Mapping Diagrams เราสามารถเขียนรูปของ Domain-Range ได้ เพราะฟังก์ชันทางคณิตศาสตร์คือความสัมพันธ์ระหว่างเซตของ inputs และเซตของ outpus จึงเขียนได้ว่า
![one-to-one](https://miro.medium.com/v2/resize:fit:720/format:webp/1*3EC6dFZ9jYQV0IEYXdm9Gw.png)

one-to-one
รูปแรกแสดงให้เห็นความสัมพันธ์ของฟังก์ชันและคำตอบโดยฟังก์ชันคือ Domain และ Range ที่มีความสัมพันธ์แบบ 1:1 ได้
![many-to-one](https://miro.medium.com/v2/resize:fit:720/format:webp/1*xLzd1Yc5yPlaL3qp7bQKLg.png)

many-to-one
ความสัมพันธ์ของ Domain และ Range สามารถเป็น N:1 และก็ยังถือว่าเป็นฟังก์ชัน

![one-to-many](https://miro.medium.com/v2/resize:fit:720/format:webp/1*p4Mo8oqfx3eDGyQFaf3q7g.png)
one-to-many
แต่หากเมื่อใดที่ความสัมพันธ์เป็นไปแบบ 1:N ในทางคณิตศาสตร์จะถือว่าไม่เป็นฟังก์ชัน เพราะในความเป็นจริง เราไม่สามารถทำให้ f(x) = 2x -1 โดยที่ f(2)มีค่าเป็นอื่นได้นอกจาก f(-2) = -5และ f(2) = 3 เท่านั้น

ลองแปลงโจทย์คณิตศาสตร์ให้เป็น JavaScript
```javascript
const f = (x) => 2 * x - 1
console.log(f(2)) // 3
```
# Impure function
ตรงกันข้ามกับ Pure function หรือพูดง่ายๆ ว่าแหกกฏการเขียนทุกอย่างเลย ตัวอย่างเช่น การนำเอาโค้ดด้านบนมาแก้ให้เป็น Impure function
```javascript
const f = (x) => { 
  init += 2 * x - 1 
}
let init = 2
f(2)
console.log(init) // 5
f(2)
console.log(init) // 8
```
จะเห็นได้ว่าเมื่อเราเรียก f(x) ค่าของ init ก็เปลี่ยนไปเรื่อยๆ ถึงแม้จะส่ง 2 แบบเดิมเราก็ไม่ได้ return value แบบเดิม และ f(x) ยังไปแก้ state ที่นอกจาก Scope ตัวเองด้วย การเขียนแบบนี้ทำให้เกิดสิ่งที่เรียกว่า side-effect มีผลทำให้เกิดบัคและทำให้การทำงานของระบบผิดพลาดสูง

# Shared state
Shared state เป็นแนวคิดนึงในการเขียนโปรแกรมแบบ OOP ซึ่งคือการสร้าง Properties ใหักับ Object (ผมจะไม่ลงรายละเอียด OOP นะครับ เข้าใจว่าทุกคนน่าจะรู้เรื่อง Properties และ Behaviour แล้ว) ก่อนอื่นมาว่าด้วยเรื่องของ Data type ก่อน ปกติเราจะมี Data type ที่เป็น Primitives/References ในทุกเกือบๆ ภาษาคอมพิวเตอร์อยู่แล้ว ในบทความนี้ผมจะพูดถึง JavaScript เป็นหลัก

# Primitive:

Boolean
Number
String
Null
Undefined
Symbol
Reference:

# Objects
Arrays
ปกติแล้วถ้าเราเขียนโค้ดกับ Primitive ก็จะเป็นแบบนี้
```javascript
let greeting = 'Hello'
```
ข้อมูลจะถูกเก็บไว้ใน Memory แบบนี้:
![memory](https://miro.medium.com/v2/resize:fit:720/format:webp/1*yQ3AOH8oEeTwLmvTxRQQbA.png)

ถ้าเราใส่ค่าให้กับ greeting ใหม่คอมพิวเตอร์ก็จะใส่ค่าให้กับ ref memory:
```javascript
let saySometing = 'JS very nice lang'
saySometing = greeting
console.log(saySometing) // Hello
greeting = 'Hi!!'
console.log(greeting) // Hi!!
saySometing = 'WoW'
console.log(saySometing) // WoW
console.log(greeting) // Hi!!
```
![memory](https://miro.medium.com/v2/resize:fit:720/format:webp/1*eJOHaJ389sVFWqqeuQ1XZA.png)
เราจะเห็นได้ว่าการทำงานของสองตัวแปรก็ยังทำงานเหมือนเดิมต่อให้เราเปลี่ยนค่ากี่รอบตัวแปรสองตัวก็ไม่มีผลกระทบต่อกันมันแค่ก็อปปี้ value มาใส่ใน ref ใหม่เท่านั้นเอง ซึ่งการทำงานกับ Primitive แบบนี้มักจะไม่เจอปัญหาอะไร แต่ถ้าเราเขียนแบบนี้กับ Reference type
```javascript
const person = { age: 20 }
person.age = 30
console.log(person) // { age: 30 }
```
และ Memory เราอาจจะหน้าตาแบบนี้
![memory](https://miro.medium.com/v2/resize:fit:720/format:webp/1*i8mhiEee08SPyKGbolCzlA.png)

พูดง่ายๆ ว่าการทำงานมันก็จะคล้ายๆ กับ Primitive แต่แทนที่มันจะเก็บ value ตรงๆ แต่มันก็จะเก็บ ref แทน ละเมื่อเราเขียนแบบ Functional Programming

```javascript
const updateAge = (p, newAge) => { 
  p.age = newAge 
  return p
}
const person = { age: 20 }
const newPerson = updateAge(person, 30)
console.log(newPerson) // { age: 30 }
console.log(person) // { age: 30 }
newPerson.age = 31
console.log(newPerson) // { age: 31 }
console.log(person) // { age: 31 }
```

จะเห็นว่าผลที่ได้คือ { age: 30 } สาเหตุเพราะมันเป็นการส่งข้อมูลแบบที่เรียกว่า passed by reference ส่วนถ้าเป็น Primitive เขาจะเรียกว่า passed by value ส่วนของ passed by referenceคือมันทำการส่ง ref memory เข้าไปในฟังก์ชันและเมื่อเราแก้ไขข้อมูลอะไรบางอย่าง มันจึงกระทบกับตัวแปรด้านนอกหรือที่อื่นๆ ใน ref เดียวกันทั้งระบบ ซึ่งการเขียนโปรแกรมแบบนี้จึงต้องระวังให้มากๆ ถ้าทำไรอะไรแบบนี้ให้ clone ตัวแปรก่อนเพราะคอมพิวเตอร์จะไปสร้าง ref ใหม่ใน memory ให้ทำให้การแก้ไขข้อมูลไม่ซิงค์กัน

# Declarative vs Imperative style
การเขียน Functional Programming เราจะใช้การเขียนโปรแกรมแบบ Declarative เป็นการเขียนโค้ดที่ไม่มี Control flow ส่วนการเขียนแบบ Imperative จะเขียนเป็นคำสั่งเพื่อบอกว่าระบบของเราทำอะไรบ้างไล่ลำดับการทำงานลงมาและมี Control flow

#Imperative
```javascript
const number = [1, 2, 3, 4, 5]
for (let i = 0; i < number.length; i++) {
  number[i] =  number[i] * 2;
}
console.log(number) // [2, 4, 6, 8, 10]
```
จะเห็นได้ว่าเรามีการใช้ for-loop เป็นตัวควบคุมการทำงานของระบบเรา โดยตัวแปรหลักคือคำสั่งที่ให้เกิดผลลัพธ์ของระบบ

# Declarative
```javascript
const number = [1, 2, 3, 4, 5]
const sq = number.map(n => n * 2)
console.log(sq) // [2, 4, 6, 8, 10]
```
การเขียนแบบ Declarative เราจะโยน Data เข้าไปเพื่อให้ระบบทำการจัดการ Data ให้อยู่ในรูปแบบที่ต้องการ จะไม่มีการบอกว่าต้องทำลำดับที่ 1, 2 ,3 … ซึ่งตัวแปรหลักเลยคือ Data ถ้าอยู่ใน Format ที่ตัวฟังก์ชัน Declarative เข้าใจเราก็จะได้ผลลัพธ์ออกมาตามที่เขียนไว้ แต่พอเมื่อไม่มีการ Control flow เราจึงไม่สามารถที่จะบอกว่าหาก Data ไม่อยู่ใน Format ที่ต้องการให้ทำอะไรก่อนเราจึงต้องพึ่งพาการเขียนแบบ Composition

# Composition
ปกติแล้วตามที่เราเคยเรียนเรื่อง OOP กันมาหรือแม้ก็ทั้ง ถ้าเคยได้ยินเรื่อง SOLID เราจะมีการพูดที่เหมือนกันอย่างนึงคือ Single responsibility คือ 1 Class หรือ 1 Function ต้องทำแค่ 1 อย่าง (การเขียนโปรแกรมที่ถูกต้องจะแบบนี้เสมอ) พอเกิดการกระทำแค่อย่างเดียวเราจึงต้องเอาฟังก์ชันต่างๆ มาประกอบกันให้เป็น Pipeline เพื่อทำให้การทำงานมันครบและจึงนำเอา return value ของฟังก์ชันมาเป็น Parameter ของฟังก์ชันถัดๆ ไปเพื่อให้มันได้ return value จนเราสามารถเอาไปใช้ต่อได้

การใช้งาน Pipline นั้นเราสามารถเขียนได้เองแต่ เพื่อไม่ให้เสียเวลาในการอธิบาย ผมจึงขอยกไปไว้บทความถัดหากมีโอกาส และในบทความนี้ก็จะขอแนะนำสิ่งที่เรียกว่า ramdajs ที่ช่วยให้เราเขียน Functional Programming กับ Pipeline ได้ และฟังก์ชันใน ramdajs ทั้งหมดเป็น Pure function สำหรับใครที่เขียน JavaScript และอยากเป็นสาย Functional Programming ละก็แนะนำให้ลองเขียน ramdajs composition ขึ้นมาเองได้เลยครับ ซึ่งเราก็มีคนทำไว้เป็น Cookbook ด้วย

เราลองมาดูตัวอย่างง่ายๆ กันก่อน
```javascript
const number = [1, 3, 4, 6, 7, 8, 10] // [2, 6, 8, 12, 14, 16, 20]
numbers.map(n => n * 2).filter(n => n > 10) // [12, 14, 16, 20]
```
ในตัวอย่างเป็นการทำ Composition ของ map และ filter ซึ่งเป็นคำสั่ง native Js เพื่อหาว่าเลขไหนคูณ 2 แล้วมีค่ามากกว่า 10 ให้ดึงออกมา ต่อไปเราลองมาดูถ้าไม่มีคำสั่ง native เราจะทำงานยังไงกัน
```javascript
R.pipe(…)
```
หนึ่งในวิธีการทำ Composition ที่ผมจะแนะนำคือ pipe ซึ่งการทำงานของมันจะเริ่มจากซ้ายไปขวา เหมือนการอ่านหนังสือเลยครับ (ผมชอบใช้ตัวนี้มันอ่านง่ายและเป็นสิ่งที่คุ้นเคยอยู่แล้ว)
```javascript
import * as R from 'ramda'
const upperCase = (o) => o.map(_o => ({ book: R.toUpper(_o.book) }))
const payload = '[{"book":"js"},{"book":"lamda"}]'
const pipeline = R.pipe(
  JSON.parse,
  upperCase, 
  console.log
)
pipeline(payload) // [{"book":"JS"},{"book":"LAMDA"}]
```
จากโค้ดคือผมมี payload เก็บ String json ไว้ และผมทำ pipeline ไว้ให้รับค่าแล้วแปลงเป็น Js object ด้วย JSON.parse เมื่อ return value เป็น Object และผมก็เขียนฟังก์ชันตัวเองขึ้นมาชื่อ upperCase ที่รับ Array เข้ามาแล้วแปลง value ของ Object ให้เป็นตัวใหญ่ และ return value ออกไปเป็น Array สุดท้ายผมโยนค่าเข้าไปใน console.log เพื่อแสดงค่า ซึ่งจะเห็นได้ว่าทั้งหมดเป็นการเขียนโปรแกรมแบบทั้งหมด Declarative แต่หากเราเขียนแบบ Imperative ก็จะได้แบบนี้
```javascript
const upperCase = (data) => { 
  const cloned = JSON.parse(data)
for(let i = 0; i < data.length; i++) {
    cloned[i].book = cloned[i].book.toUpperCase()
  }
 
  return cloned
}
const payload = '[{"book":"js"},{"book":"lamda"}]'
const result = upperCase(obj)
console.log(result) // [{"book":"JS"},{"book":"LAMDA"}]
```

เพื่อไม่ให้เกิด side-effect ผมจึงย้าย JSON.parse มาไว้ในฟังก์ชันแทนเพราะถ้าโยน JSON เข้ามาตรงๆ โค้ดเราจะกลายเป็นการทำ Share state ทันที
```javascript
R.compose(…)
```
นอกจากใช้ pipe แล้วเรายังสามารถที่จะใช้ compose ได้อีกด้วยแค่การทำงานของมันจะต่างกันตรงที่ compose จะทำจากขวาไปซ้าย
```javascript
const composer = R.compose(
  console.log, 
  upperCase, 
  JSON.parse
)
```
ผมเอาโค้ดเดิมมาสลับการทำงานเล็กน้อยให้ console.log มาอยู่ทางซ้ายแทน และผลลัพธ์ที่ได้ก็ยังเหมือนเดิม

# Currying
เมื่อเราเริ่มใช้ Composition เป็นปัญหาถัดมาคือ แต่ละฟังก์ชันมันรับ Parameter (unary) แค่เพียงตัวเดียว ซึ่งในความเป็นจริงเราไม่สามารถที่จะเขียนโปรแกรมที่มีการทำงานแบบนั้นได้ ในบางกรณีต้องรับอย่างน้อย 2 (none-unary) เช่นถ้าเราจะทำระบบเครื่องคิดเลข อย่างน้อยเราก็ต้องรับเลขตัวตั้งและตัวกระทำเข้ามาในระบบ
```javascript
const add = (x, y) => x + y
console.log(add(1,2)) // 3
```
ถ้าเขียนแบบนี้ตรงๆ ก็ไม่มีปัญหาอะไร แต่พอเราจะทำแบบนี้ละ
```javascript
add(2,y)
```
ซึ่งมันก็ควรจะทำงานได้แต่ความจริงคือมันทำไม่ได้เพราะมันต้องการ Parameter สองตัวพร้อมกัน แต่บางครั้งเราไม่รู้ว่า y มีค่าเท่าไร เราอาจจะมีการไปคำนวณอะไรบางอย่างก่อน แล้วจึงจะรู้ว่า y เป็นอะไรแล้วจึงมาเรียกฟังก์ชันให้ทำงานหรือในอีกมุมคือเรารู้ทั้งสองตัวแล้วแต่อย่างที่บอกครับ มันไม่สามารถรับ Parameter พร้อมกันได้ถ้าเราเอาฟังก์ชันมา Compose กัน

ถ้าหาเราเขียนแบบ Imperative เราก็แค่ใส่การ control flow ด้วย if-else เพื่อไปหาค่า y มาแล้วจึงค่อยมาประมวลผลต่อ เพื่อให้การเขียนเป็น Declarative จริงๆ ดังนั้นเราจะมาใช้ความสามารถของภาษาที่ถูกออกแบบให้สามารถทำ Functional Programming ได้นั้นคือ return value เป็น Function แทน Primative type ทั่วไป
```javascript
const add = x => y => x + y
const addOne = add(1) // return [function]
addOne(2) // 3
------
add(1)(2) // 3
```
จะเห็นได้ว่าเราสามารถที่จะใส่ Paremeter แบบตัวเดียวได้แล้ว ที่นี่เราก็สามารถเอาไปใส่ใน Composition ของเราได้ และความจริงแล้วหน้าตาปกติของด้านบนมันก็คือแบบนี้
```javascript
function add(x) {
  return function(y) {
    return x + y;
  }
}
const addOne = add(1)
addOne(2)
------
add(1)(2)
```
ถ้าเขียนเป็น JavaScript แบบ ES5 ทั่วไปมันคือการสร้างฟังก์ชันที่ return value เป็นฟังก์ชันนั้นเองครับ
```javascript
R.curry(…) [docs]
```
แทนที่เราจะเขียนแยกเองบางครั้งเราอาจจะใช้ ramdajs ช่วยก็ได้ มาดูตัวอย่างกัน
```javascript
const h = (x, y, z) => x + y * z
h(1, 2, 3) // 7
```
เราลองมาดูวิธีการเขียนแบบไม่ใช้ ramdajs
```javascript
const h = x => y => z => x + y * z
const findX = h(1)
const findXY = findX(2)
const findXYZ = findXY(3)
console.log(findXYZ) // 7
console.log(h(1)(2)(3)) // 7
```
และเราลองมาใช้ ramdajs
```javascript
import * as R from 'ramda'
const h = (x, y, z) => x + y * z
const withCurry = R.curry(h)
const findX = withCurry(1)
const findXY = findX(2)
const findXYZ = findXY(3)
------------------------ or -------------------
h(1,2)
h(3)
--------
h(1)(2)
h(3)
--------
h(1)
h(2,3)
--------
h(1)
h(2)(3)
-------
```
จะเห็นได้ว่าผลลัพธ์ที่ได้ก็ออกมาเหมือนกัน

# Partial
เป็นการเขียนฟังก์ชันที่สามารถทำให้รับ Paramter ได้มากกว่า 1 และโดยตัว Partial นั้นจะเป็นการเขียนฟังก์ชันเพื่อลดจำนวนของ Arity (เป็นศัทพ์ทางคณิตศาสตร์ ที่พูดถึงจำนวน Parameter ของฟังก์ชัน) ถ้าย้อนกลับไปดูที่ Curry เราจะเห็นว่าการเรียกใช้งานฟังก์ชันจะเรียกเท่าจำนวน Parameter คือ
```javascript
const h = x => y => z => x + y * z
```
เราจะต้องเรียกถึง 3 รอบกว่าจะได้คำตอบ แต่อย่างที่ผมอธิบายไปคือ Patial ช่วยให้เราลด Arity ได้ทำให้การเรียกใช้งานสั้นลง

การเขียน Partial ใน JavaScript ปกติเราจะใช้ bind, call, apply (อ่านบทความเก่าผมได้ที่ link) แล้วแต่เราจะเลือกออกแบบ สำหรับภาษาที่เป็น Functional Programming อื่นๆ นั้นจะเขียนต่างกันออกไปนิดหน่อยแล้วแต่ตัว Paradiam ของภาษาเอง เราลองมาดูตัวอย่างของ JavaScript
```javascript
const partialApplied = (
  fn, 
  ...args
) => (
  ..._args
) => fn.apply(
  null, [...args, ..._args]
)
```
ผมขอเขียนโดยใช้ ES6 หมดเลยนะเพราะมันง่ายกว่าเขียนแบบปกติ มาดูการใช้งานจริงกัน
```javascript
const h = (x, y, z) => x + y * z
const takeAllAtOnce = partialApplied(h, 1, 2, 3) // 7
const takeOnlyFn = partialApplied(h) // [function]
takeOnlyFn(1,2,3) // 7
const takeSomePreDefineNumber = partialApplied(h, 1) // [function]
takeSomePreDefineNumber(2,3) // 7
const takeMore3AtOnce = partialApplied(h)
takeMore3AtOnce(1, 2, 3, 4, 5, 6, 7, 8, 9, [1,2,4], 'test') // 7
```
ลองมาสังเกตว่า takeMore3AtOnce สามารถรับค่าอะไรลงไปก็ได้มากกว่า 3 ตัว ส่วนทำไม patialApplied มันถึงรับได้แค่ 3 เพราะว่า h ผมรับแค่ 3 ตัวเท่านั้นถ้าส่งอย่างอื่นเข้าไปตั้งแต่ตัวที่ 4 มันจะไม่เอามาทำงาน และจะเห็นอีกว่าเราสามารถลดจำนวนการเรียกฟังก์ชันเราได้ ด้วย
```javascript
R.partial(…)
```
สำหรับใครอยากทำแบบรวดเร็ว ไม่ต้องมานั่งจัดการอะไรมากผมขอแนะนำฟังก์ชัน Partial จาก ramdajs นะ มาดูตัวอย่างกัน
```javascript
import * as R from 'ramda'
const h = (x, y, z) => x + y * z
const takeAllAtOnce = R.partial(h, [1, 2, 3])
takeAllAtOnce()
const takeOnlyFn = R.partial(h)
takeOnlyFn([1, 2, 3])()
const takeSomePreDefineNumber = R.partial(h, [1])
takeSomePreDefineNumber(2, 3)
const takeMore3AtOnce = R.partial(h)
takeMore3AtOnce([1, 2, 3, 4, 5, 6, 7, 8, 9, [1,2,4], 'test'])()
```
ผลที่ได้ก็จะออกมาเหมือนกันเลยครับ แต่ปกติ ramdajs จะรับ Parameter เป็น Array จะเห็นได้ว่าผมจะต้องทำเป็น Array หมดเลย ไม่ต้องแปลกใจไปนะครับ

# Curry and Partial in Mathematic
ปกติแล้ว Curry และ Partial มันเป็นวิธีการทางคณิคศาสตร์ ถ้าหากเราจะพูดถึงนิยามของแต่ละอันที่ที่เราเขียนมาเป็นโค้ดเราจะสามารถเขียนได้ดังนี้ (ใครที่อ่านนิยามทางคณิตศาตร์ไม่ออกหรือไม่แข็งด้านคณิตศาตร์ ผมแนะนำให้อ่านข้ามไปเลยครับ ไว้มีโอกาสจะมาสอนในบทความอื่น)
```javascript
** Curry **
Given ƒ: (X • Y • Z) -> N
then curry(ƒ): X -> (Y ->(Z -> N))
** Partial **
Given ƒ: (X • Y • Z) -> N
then ƒ: (Y • Z) -> N
Apply by 
partial(f): ((X • Y -> N) • X) -> (Z -> N)
```
สำหรับนักคณิตศาสตร์เข้าไป อ่านนิยามได้ที่ Wikipredia

# Point-free programming
หรืออีกชื่อคือ Tacit programming คือการเขียนโค้ดแบบไม่ใช้ จุด(.) ไม่สนใจว่าเราต้องเรียกฟังก์ชันอะไรตรงไหนอย่างไร ปกติเราจะเขียนกันเป็นปกติใน OOP เวลาที่ต้องการเรียก Properties ของ Object แต่ใน Functional Programing เราจะเน้นการเขียนแบบ Point free มากกว่า โดยการประกาศฟังก์ชันและประกอบมันให้เป็นฟังก์ชันที่ทำงานกับชุด Data ที่ต้องการแล้วปล่อยให้ระบบทำงานไปเองโดยที่เราไม่ต้องไปสั่งมัน เราลองมาดูตัวอย่างกัน
```javascript
const payload = '123456'
const _split = R.split('')
const _toNumber = (x) => Number(x)
const _toStr = (x) => String(x)
const _eachToNumber = (x) => x.map(_toNumber)
const _product = (x) => R.product(x)
const _findEven = (x) => x.filter( n => n % 2 === 0)
const _concat = (x) => R.join('', x)
const findEvenFromProductResult = R.pipe(
  _split,        // ['1','2','3','4','5','6']
  _eachToNumber, // [1,2,3,4,5,6]
  _product,      // 720
  _toStr,        // '720'
  _split,        // ['7','2','0']
  _eachToNumber, // [7,2,0]
  _findEven,     // [2,0]
  _concat,       // 20
  _toStr,        // '20'
)
const result = pipeline(payload)
console.log(result)
```
จากโค้ดคือการหาคำตอบของ 123456 โดยการเอาทั้งหมดมาคูณกันแล้วหาว่าผลคูณตัวไหนเป็นเลขคู่บ้างโดยผมก็เอาฟังก์ชันเล็กมาประกอบกันและแค่โยนข้อมูลเข้าไป ถ้าคุณสามารถหาคำตอบของระบบแบบที่ผมทำได้แปลว่าคุณกำลังเขียน point-free style อยู่ครับ

# Avoiding Mutate Data, Immutability
การจะทำให้ระบบไม่เกิดบัคได้นั้น อย่างแรกเลยเราต้องทำตัวแปรของเรานั้นไม่ถูกแก้ไขได้ก่อน ทำให้มันเป็น Immutable State ยกเว้นบางตัวเราอาจจะแก้ไขจริงๆ อันนี้ก็เป็นข้อยกเว้นไป แต่ในการเขียน Functional Programming ตัวความถูกต้องของ Data นั้นสำคัญมากถ้าเกิด Data เปลี่ยนจากเดิม ระบบเราอาจจะเกิดข้อผิดพลาดได้ เพราะงั้นเรามาดูวิธีการทำให้ตัวแปรเป็น Immutable

ใช้ const ประกาศตัวแปรเสมอ เพราะเมื่อเราใช้ const แล้วเราจะไม่สามารถสร้างตัวแปรที่ชื่อเดียวกันได้อีกหรือกำหนดค่าให้มันได้ มันจะเป็น 1 ไปจนจบการทำงาน (Read-only)
const num = 1
ถ้าตัวแปรต้องสามารถแก้ไขข้อมูลได้ให้ใช้ let แทน คุณสมบัติเหมือน const แต่เพียงว่ามันสามารถแก้ไขข้อมูลได้ (Read or Write)

ใช้ Object.freeze เพื่อให้ Object ที่ประกาศมาไม่ถูกแก้ Properties ได้
```javascript
const obj = Object.freeze({ name: 'John' })
obj.name = 'Jane'
// Error
```
เพื่อไม่ให้เกิดเหตุในกรณีที่อาจจะเกิดเหตุทำให้ Object ของเราถูกแก้ไขโดยอังเอิญได้เราควร freeze ไว้

ถ้าต้องโยน Object (ไม่ใช่ Deep object )ไปใน Method ให้ใช้ Spread
```javascript
const Mr = (obj) => ({...obj, prefix: ‘Mr’ })
const obj = { fname: ‘John’, lname: ‘Doe’, prefix: ‘’ }
const withPrefix = Mr({ …obj }) // here use Spread
```
การทำแบบนี้จะทำให้ obj.prefix ยังคงเป็นค่าว่างเหมือนเดิมเพราะเราได้ก็อบปี้ค่าผ่านตัว Spread operator(…) คือ Mr({ …obj }) เราใส่ตอนที่เรียกฟังก์ชัน

การใช้ Spread ถือว่าเป็นอะไรที่ง่ายมากแต่ปัญหาคือมันทำ copy-deep ไม่ได้ ตัวอย่างคือ
```javascript
const Mr = (obj) => ({...obj, prefix: 'Mr' })
const obj = { 
  fname: 'John', 
  lname: 'Doe', prefix: '', 
  address: { hometown: '', city: },
}
const withPrefix = Mr(obj)
```
address เป็น deep object คือมี Object ซ้อนลงไปอีกถ้าเราใช้ Spread เราก็ยังสามารถก็อบปี้มันได้แต่ตัว address จะยังชี้ memory ref ไปที่ obj หรือแม้แต่ deep array ก็ตาม วิธีแก้คือใช้ JSON.stringify และ JSON.parse
```javascript
const Mr = (obj) => ({...obj, prefix: 'Mr' })
const obj = { 
  fname: 'John', 
  lname: 'Doe', prefix: '', 
  address: { hometown: '', city: },
}
const newObj = JSON.parse(JSON.stringify(obj))
const withPrefix = Mr(newObj)
```
หลักการมันคือการแปลง Object ให้เป็น String ก่อนหลังจากนั้นก็แปลงมันกลับมาเป็น Object แล้วตัว JavaScript จะทำการสร้าง memory ref อันใหม่สำหรับตัวแปรเราให้ครับ

# Avoiding Side effect
ถ้าอ่านมาถึงตอนนี้ผมจะมีการพูดถึง side effect อยู่บ้าง มันก็คือการที่ค่า return value ของเราถูกแก้ไขหรือเปลี่ยนแปลงโดยฟังก์ชันอื่นที่เราไม่ได้เรียกใช้ ในขณะที่เรากำลังเรียกใช้งานฟังก์ชันอื่นๆ อยู่สาเหตุอาจจะมาจาก:

ฟังก์ชันเราไปเรียกใช้งานตัวแปร Global scope และในขณะเดียวกันฟังก์ชันอื่นก็ดันมาแก้ตัวแปรนั้นไป หรือเราเรียกใช้งานตัวแปรใน Parent scope
การแชร์ใช้งาน Object ที่ยังไม่ได้ clone ไปยัง ref memory ใหม่
สำหรับบทความนี้ผมคิดว่าน่าจะเป็นการสอนเรื่อง Funcational Programming ที่ครบพอสมควร ถ้าตกหล่นตรงไหนไว้ผมจะมาเขียนบทความหน้าเพิ่มเติมให้ 
และสำหรับคนที่สนใจเรื่องคณิตศาสตร์และ Functional Programming ไว้ผมจะมาอธิบายเรื่อง Curry, Partial และ Lamda ให้ฟังอีกทีในเชิงคณิตศาสตร์เพื่อให้เห็นภาพมากขึ้น
