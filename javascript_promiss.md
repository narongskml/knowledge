From :http://marcuscode.com/lang/javascript/promise

Promise ในภาษา JavaScript
13 May 2021

ในบทนี้ คุณจะได้เรียนรู้เกี่ยวกับ Promise ในภาษา JavaScript เราจะแนะนำให้คุณรู้จักกับ Promise การใช้งานและเทคนิคการนำ Promise มาใช้ในการเขียนโปรแกรม นี่เป็นเนื้อหาในบทนี้

- Promise คืออะไร
- การใช้งาน Promise
- Promise rejection
- Promise chaining
- Promisifying
- Async/Await กับ Promise

# Promise คืออะไร

Promise เป็นออบเจ็คที่ส่งค่ากลับเป็นผลสำเร็จ (Resolve) หรือผลล้มเหลว (Reject) ของการทำงานแบบ Asynchronous มันสามารถช่วยลดความซับซ้อนและทำให้การเขียนโปรแกรมในรูปแบบ Asynchronous ทำได้ง่ายขึ้น และ Promise เป็นรูปแบบใหม่สำหรับการเขียนโปรแกรมที่ทำงานแบบ Asynchronous ในภาษา JavaScript

ในภาษา JavaScript ฟังก์ชันที่ทำงานแบบ Asynchronous จะต้องการฟังก์ชัน Callback เมื่อการทำงานเสร็จสิ้น การใช้งาน Promise สามารถช่วยลดการใช้ฟังก์ชัน Callback เพื่อทำให้มันเรียบง่ายขึ้นได้ และมันสามารถใช้ร่วมกับคำสั่ง Async/Await เพื่อทำให้การทำงานเป็นแบบ Synchronous

ในการทำงานของ Promise มันสามารถให้ผลลัพธ์การทำงานได้สองรูปแบบนั่นคือ

- การทำงานที่สำเร็จ (Resolve)
- การทำงานที่ล้มเหลว (Reject)

และเมื่อต้องการทราบผลลัพธ์การทำงานของ Promise เราสามารถตรวจสอบผลลัพธ์จากออบเจ็คของ Promise ได้โดยการใช้ Consumer เมธอดอย่าง then และ catch

# การใช้งาน Promise

มาเริ่มทำความคุ้นเคยกับ Promise ในภาษา JavaScript นี่เป็นตัวอย่างของ Promise ที่มีผลการทำงานเป็นผลสำเร็จ หลังจากที่เวลาผ่านไป 1 วินาที และนี่เป็นโค้ดที่ทำงานในรูปแบบ Asynchronous โดยใช้ฟังก์ชัน setTimeout
promise_resolve.js
```javascript
let promise = new Promise((resolve, reject) => {
   setTimeout(() => {
        resolve('Hello');
   }, 1000); 
});

promise.then(value => {
    console.log(`Resolved: ${value}`);
});

console.log('Main program');
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
Main program
Resolved: Hello
```
จากตัวอย่าง สามารถแบ่งการทำงานของโปรแกรมได้เป็นสองส่วนได้แก่ ส่วนที่ทำงานแบบ Asynchronous เป็นโค้ดที่อยู่ในCallback ของฟังก์ชัน setTimeout และส่วนที่ทำงานแบบ Synchronous จะอยู่โปรแกรมหลัก ได้แก่ การสร้าง Promise ออบเจ็ค การเรียกใช้เมธอด then และการแสดงข้อความ "Main program" ออกทางหน้าจอ

Promise นี้ให้ผลลัพธ์การทำงานเป็นผลสำเร็จ ที่ส่งค่ากลับเป็น "Hello" หลังจากเวลาผ่านไป 1 วินาที และค่าดังกล่าวถูกแสดงออกทางหน้าจอ และในส่วนของโปรแกรมหลัก ข้อความ "Main program" ถูกแสดงออกทางหน้าจอ
```javascript
let promise = new Promise((resolve, reject) => {
   setTimeout(() => {
        resolve('Hello');
   }, 1000); 
});
```
ในการใช้งาน Promise เราสร้างออบเจ็คจากคลาส Promise โดยคอนสตรัคเตอร์จะรับฟังก์ชัน Callback สำหรับ Promise เพื่อทำงาน ซึ่งฟังก์ชันนี้จะส่งมอบสองพารามิเตอร์คือฟังก์ชัน resolve และ reject ที่เราสามารถใช้สรุปผลการทำงานของ Promise ว่าสำเร็จหรือล้มเหลว
```javascript
resolve('Hello');
```
นั่นหมายความว่าเมื่อเราต้องการให้ Promise ทำงานสำเร็จ เราเรียกใช้ฟังก์ชัน resolve พร้อมกับค่าที่ต้องการส่งค่ากลับ ในกรณีนี้คือข้อความ 'Hello' และในตัวอย่างนี้ ยังไม่ได้มีการใช้งานฟังก์ชัน reject ซึ่งเราจะทำมันในภายหลัง
```javascript
promise.then(value => {
    console.log(`Resolved: ${value}`);
});
```
เพื่อตรวจสอบการทำงานของ Promise เราเรียกใช้เมธอด then บนออบเจ็คของ Promise และมันจะถูกเรียกใช้งานเมื่อ Promise ทำงานเสร็จสิ้นหรือเมธอด resolve ถูกเรียกใช้งาน เรากำหนดฟังก์ชัน Callback ที่รับหนึ่งพารามิเตอร์ value ที่เป็นค่าส่งกลับของ Promise

นั่นหมายความว่าเมื่อเวลาผ่านไป 1 วินาที ฟังก์ชัน Callback ที่กำหนดให้กับเมธอด then จะถูกเรียกใช้งานโดยได้รับข้อความ Hello ที่ส่งมาจากการทำงานสำเร็จของ Promise และผลลัพธ์ที่ได้ถูกนำมาแสดงออกทางหน้าจอ

# Promise rejection

อย่างที่บอกไปแล้วว่า Promise สามารถทำงานสำเร็จหรือล้มเหลวได้ ในกรณีที่สำเร็จ ฟังก์ชัน resolve จะถูกเรียกใช้งานพร้อมกับส่งค่ากลับมา ส่วนในกรณีที่ Promise ทำงานล้มเหลวนั้นสามารถเกิดขึ้นได้สองกรณีดังต่อไปนี้

- เกิดจากเรียกใช้ฟังก์ชัน reject เราสามารถเรียกใช้ฟังก์ชันนี้โดยตรงเพื่อทำให้ Promise ทำงานล้มเหลวได้
- เกิดจากโปรแกรมที่ทำงานผิดพลาดที่เกิดการ throw error ขึ้น ซึ่งนี่เป็นการเกิดข้อผิดพลาดปกติในภาษา JavaScript และเมื่อเกิดใน Promise มันทำให้เกิดจาก Reject

โดยทั้งสองกรณีคือการทำงานล้มเหลวของ Promise หรือ Promise rejection และเราสามารถใช้เมธอด catchเพื่อรับมือกับการทำงานที่ล้มเหลวของ Promise ได้ ในตัวอย่างนี้ แสดงการทำงานของ Promise ที่สามารถทำงานสำเร็จ (Resolve) และล้มเหลว (Reject)
```javascript
rejecting_promise.js

function evenPromise(number) {
    return new Promise((resolve, reject) => {
        if (number % 2 == 0) {
            resolve(`${number} is an even number`);
        } else {
            reject(new Error(`${number} is not an even number`))
        }
    });
}

evenPromise(2).then((value) => {
    console.log(value);
}).catch(err => {
    console.log(err.toString());
});

evenPromise(3).then((value) => {
    console.log(value);
}).catch(err => {
    console.log(err.toString());
});
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
2 is an even number
Error: 3 is not an even number
```
ในตัวอย่างนี้ เป็นโปรแกรมเพื่อตรวจสอบว่าตัวเลขเป็นจำนวนคู่หรือไม่ ถ้าหากตัวเลขที่ต้องการตรวจสอบเป็นจำนวนคู่ Promise จะทำงานสำเร็จ (Resolve) ไม่เช่นนั้น มันจะทำงานล้มเหลว (Reject) แทน
```javascript
function evenPromise(number) {
    return new Promise((resolve, reject) => {
        if (number % 2 == 0) {
            resolve(`${number} is an even number`);
        } else {
            reject(new Error(`${number} is not an even number`))
        }
    });
}
```
ฟังก์ชัน evenPromise ส่งค่ากลับเป็นออบเจ็คของ Promise ที่ใช้ในการตรวจสอบตัวเลขว่าเป็นเลขจำนวนคู่หรือจำนวนคี่
```javascript
if (number % 2 == 0) {
    resolve(`${number} is an even number`);
} else {
    reject(new Error(`${number} is not an even number`))
}
```
จากนั้นในการทำงานของ Promise นำตัวเลขมาตรวจสอบว่ามันเป็นจำนวนคู่หรือไม่ หากใช่ฟังก์ชัน resolve ถูกเรียกใช้งานพร้อมกับข้อความบอกว่าตัวเลขเป็นจำนวนคู่ ส่วนในกรณีที่ไม่ใช้ เราเรียกใช้ฟังก์ชัน reject โดยส่งออบเจ็คของข้อผิดพลาดกลับไปแทน

นั่นหมายความว่าขึ้นกับค่าในตัวแปร number ที่ส่งมายังฟังก์ชัน evenPromise ถ้าหากตัวเลขเป็นจำนวนคู่ Promise จะทำงานสำเร็จ ไม่เช่นนั้นมันจะล้มเหลวแทน
```javascript
evenPromise(2).then((value) => {
    console.log(value);
}).catch(err => {
    console.log(err.toString());
});

evenPromise(3).then((value) => {
    console.log(value);
}).catch(err => {
    console.log(err.toString());
});
```
จากนั้นเรียกใช้ฟังก์ชัน evenPromise เพื่อตรวจสอบตัวเลขว่าเป็นจำนวนคู่หรือไม่ โดยการกำหนดฟังก์ชัน Callback ให้กับเมธอด then และ catch เพื่อรอบรับการทำงานของ Promise ขึ้นกับว่ามันทำงานสำเร็จหรือล้มเหลว

นอกจากการตรวจสอบผลลัพธ์การทำงานของ Promise ในทันทีแล้ว เราสามารถสร้างออบเจ็คไว้ก่อน และตรวจสอบผลการทำงานในภัยหลังได้ ยกตัวอย่างเช่น
```javascript
defer_promises.js

let twoPromise = evenPromise(2);

// other bunch of code

twoPromise.then((value) => {
    console.log(value);
}).catch(err => {
    console.log(err.toString());
});
```
ทันทีที่ Promise ถูกสร้างจากการเรียกใช้งานฟังก์ชัน evenPromise มันจะทำงานอย่างเร็วที่สุดเท่าที่จะเป็นไปได้ แต่ในการตรวจสอบผลลัพธ์การทำงานนั้นสามารถทำตอนไหนก็ได้เพียงแค่เรียกใช้เมธอด then และ catch เพื่อเราพร้อม
# Promise chaining

Promise chaining คือการเชื่อมต่อหลาย Promise เข้าด้วยกันเพื่อให้มันทำงานแบบเป็นลำดับ ยกตัวอย่างเช่น เราต้องการให้โปรแกรมทำงานจาก fn1, fn2 และต่อด้วย fn3 โดยที่ฟังก์ชันทั้งหมดทำงานแบบ Asynchronous และเมื่อใช้ Promise chaining เราสามารถหลีกเลี่ยงการใช้งานฟังก์ชัน Callback ที่ซ้อนกันได้ ซึ่งนี่จะทำให้โค้ดอ่านง่ายและเป็นระเบียบมากขึ้น

ก่อนอื่นมาดูตัวอย่าการเขียนโปรแกรมในรูปแบบเดิมที่ส่งฟังก์ชัน Callback เพื่อทำงานเมื่อฟังก์ชัน Asynchronous ทำงานเสร็จสิ้น

ในตัวอย่างนี้ เป็นการนับตัวเลขจาก 1-4 โดยมีระยะห่างระหว่างการนับเป็น 1 วินาที ในการเขียนโปรแกรมรูปแบบเดิม เพียงแค่ใช้ฟังก์ชัน setTimeout กำหนดเวลาในการนับ พร้อมกับระบุฟังก์ชัน Callback ของมันดังนี้
```javascript
callback_hell.js

setTimeout(() => {
    console.log('One');
    setTimeout(() => {
        console.log('Two');
        setTimeout(() => {
            console.log('Three');
            setTimeout(() => {
                console.log('Four');
            }, 1000);
        }, 1000);
    }, 1000);
}, 1000);
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
One
Two
Three
Four
```
โปรแกรมจะทำการนับตัวเลขจาก 1-4 โดยเว้นระเวลาห่างจากกันเป็นเวลา 1 วินาที

ฟังก์ชัน setTimeout ใช้สำหรับหน่วงเวลาเพื่อให้โปรแกรมทำงานในฟังก์ชัน Callback หลังจากที่เวลาผ่านไปตามที่กำหนด เนื่องจากเราต้องการนับตัวเลขถัดไปหลังจากการนับก่อนหน้า จะเห็นว่าการนับครั้งใหม่จะต้องทำให้ฟังก์ชัน Callback ของการนับก่อนหน้า

จะเห็นว่าฟังก์ชัน Callback ซ้อนกันลงไปเรื่อยๆ ตามจำนวนครั้งที่นับ การซ้อนกันของ Callback ในรูปแบบนี้เรียกว่า Callback hell หรือ Pyramid of doom คุณสามารถจินตนาการได้ว่าฟังก์ชัน setTimeout อาจเป็นฟังก์ชันใดๆ ในภาษา JavaScript ที่มีการทำงานแบบ Asynchronous

ดังนั้นเพื่อลดการซ้อนกันของฟังก์ชัน Callback เราสามารถเขียนมันโดยการใช้ Promise chaining เหมือนกับตัวอย่างต่อไปนี้
```javascript
promise_chaining.js

new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('One');
        resolve();
    }, 1000);
}).then(() => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Two');
            resolve();
        }, 1000);
    });
}).then(() => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Three');
            resolve();
        }, 1000);
    });
}).then(() => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('Four');
            resolve();
        }, 1000);
    });
});
```
ในตัวอย่างนี้ แม้ว่าโค้ดจะมีการทำงานที่ซับซ้อนขึ้น เนื่องจากเราต้องสร้างออบเจ็คของ Promise เป็นจำนวนมาก แต่การนับแต่ละครั้งจะรักษาระดับความลึกของฟังก์ชัน Callback ไว้เพียงหนึ่งระดับเท่านั้น ที่ได้กำหนดผ่านเมธอด then

และเพื่อลดจำนวนของการสร้างออบเจ็ค Promise ใหม่ซ้ำๆ เราสามารถนำโค้ดส่วนนี้ไปสร้างเป็นฟังก์ชันเพื่อทำให้มันสามารถนำกลับมาใช้ใหม่ได้เหมือนกับที่เราทำกับฟังก์ชัน evenPromise จากตัวอย่างก่อนหน้า นี่เป็นตัวอย่าง
```javascript
better_promise_chaining.js

function count(number) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(number);
            resolve();
        }, 1000);
    });
}

count('One').then(() => {
    return count('Two');
}).then(() => {
    return count('Three');
}).then(() => {
    return count('Four');
});
```
โค้ดของตัวอย่างนี้ให้ผลลัพธ์การทำงานเหมือนกับสองตัวอย่างก่อนหน้า แต่เราได้ย้ายส่วนของการสร้าง Promise ไปเป็นฟังก์ชัน count และเรียกใช้งานพร้อมกับค่าที่ต้องการนับแทน จะเห็นว่าการใช้ Promise chaining สามารถลดจำนวนการซ้อนกันของฟังก์ชัน Callback และทำให้โค้ดของเราอ่านง่ายมากขึ้น ซึ่งเป็นวิธีที่ดีและแนะนำในการเขียนโปรแกรม

# Promisifying

Promisifying เป็นการเปลี่ยนการทำงานของฟังก์ชัน Asynchronous ให้เป็น Promise เนื่องจาก Promise เป็นรูปแบบใหม่สำหรับการเขียนโปรแกรมแบบ Asynchronous ดังนั้นมันจึงเป็นวิธีที่แนะนำ ในตัวอย่างนี้ เราจะมาทำการ Promisify ฟังก์ชัน Asynchronous ในภาษา JavaScript และฟังก์ชันอ่านไฟล์จากโมดูลมาตรฐานของ Node.js

มาเริ่มต้นจากการทำฟังก์ชัน setTimeout ให้สามารถใช้งานได้ในรูปแบบของ Promise คุณได้เห็นว่าเราทำแบบนี้ไปแล้วในตัวอย่างที่ผ่านมา แต่ในครั้งนี้ เราจะมาพูดถึงรายละเอียดกันว่าทำไมถึงต้องทำเช่นนี้
```javascript
promisifying.js

function setTimeoutPromise(delay) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve();
        }, delay);
    });
}

setTimeoutPromise(1000).then(() => {
    console.log('Hello after 1 second');
});
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
Hello after 1 second
```
ในตัวอย่าง เราได้สร้างฟังก์ชัน setTimeoutPromise สำหรับแปลงการทำงานของฟังก์ชัน setTimeout ให้เป็น Promise โดยการห่อหุ้มการทำงานของมันด้วย Promise ที่จะ Resolve เมื่อเวลาผ่านไปตามพารามิเตอร์ที่ส่งเข้ามายังฟังก์ชัน

และอย่างที่เราทราบ การใช้งาน Promise สามารถช่วยลดการใช้ฟังก์ชัน Callback ที่ซ้อนกันได้ ดังนั้นการเชื่อมต่อ Promise ก็สามารถทำได้ง่ายๆ เมื่อการทำงานของ Promise ก่อนหน้าเสร็จสิ้น เพียงแค่ส่งค่ากลับเป็นออบเจ็คของ Promise ใหม่จากเมธอด then เราก็จะสามารถเชื่อมต่อ Promise ด้วยเมธอด then ได้เรื่อยๆ นี่เป็นตัวอย่าง
```javascript
promisifying2.js

setTimeoutPromise(500).then(() => {
    console.log('Task 1 done');
    return setTimeoutPromise(1000);
}).then(() => {
    console.log('Task 2 done');
    return setTimeoutPromise(2000);
}).then(() => {
    console.log('All tasks done');
});
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
Task 1 done
Task 2 done
All tasks done
```
คุณได้เห็นตัวอย่างการใช้งานฟังก์ชัน setTimeout กับ Promise เป็นจำนวนมาก นั่นเป็นเพราะว่ามันเป็นฟังก์ชันพื้นฐานที่ทำงานแบบ Asynchronous ในภาษา JavaScript อย่างไรก็ตาม มีฟังก์ชันเป็นจำนวนมากที่ทำงานในรูปแบบนี้

ในตัวอย่างนี้ เราจะมาแปลงฟังก์ชันอ่านไฟล์ซึ่งเป็นฟังก์ชันจากโมดูล fs ของ Node.js ที่ทำงานแบบ Asynchronous ให้เป็น Promise และก่อนที่จะเริ่มมาดูการใช้งานของฟังก์ชันในรูปแบบปกติ
```
readfile_callback.js

const fs = require('fs');

fs.readFile('./myfile.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return
    }
    console.log(data);
});
```
ฟังก์ชัน readFile เป็นฟังก์ชันจากโมดูล fs ของ Node.js สำหรับอ่านข้อมูลจากไฟล์ที่ทำงานแบบ Asynchronous จะเห็นว่าเราต้องกำหนดฟังก์ชัน Callback เพื่อทำงานเมื่อการอ่านไฟล์เสร็จสิ้น ฟังก์ชันรับพารามิเตอร์ err และ data เพื่อบอกว่าการอ่านไฟล์มีข้อผิดพลาดหรือไม่ และข้อมูลที่อ่านได้ ตามลำดับ

จากโค้ดตัวอย่างก่อนหน้าที่ใช้ฟังก์ชัน Callback เราสามารถแปลงมันให้อยู่ในรูปแบบของ Promise ได้ดังนี้
```javascript
readfile_promise.js

const fs = require('fs');

function readFilePromise(fileName, encoding) {
    return new Promise((resolve, reject) => {
        fs.readFile(fileName, encoding, (err, data) => {
            if (err) {
                reject(err)
            } else {
                resolve(data)
            }
        });
    });
}

readFilePromise('./myfile.txt', 'utf8').then(data => {
    console.log(data);
}).catch(err => {
    console.error(err)
});
```
และกำหนดให้เรามีไฟล์ myfile.txt ที่มีข้อมูลดังต่อไปนี้
```
myfile.txt

first
second
third
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
first
second
third
```
โปรแกรมนี้ให้ผลลัพธ์การทำงานเช่นเดิมกับตัวอย่างก่อนหน้า แต่เราได้เปลี่ยนรูปแบบของการเขียนมาใช้ Promise แทน คุณสามารถลองเปลี่ยนชื่อไฟล์เป็นไฟล์ที่ไม่มีอยู่ นั่นจะทำให้ Promise เกิดการ Reject ขึ้น และโปรแกรมจะทำงานในบล็อคของเมธอด catch ที่ใช้สำหรับจัดการกับข้อผิดพลาดที่เกิดจาก Promise แทน
```javascript
readFilePromise('./nofile.txt', 'utf8').then(data => {
    console.log(data);
}).catch(err => {
    console.error(err)
});
```
และนี่เป็นผลลัพธ์การทำงานที่ล้มเหลว
```
[Error: ENOENT: no such file or directory, open 'C:\marcuscode\nodejs\promise\nofile.txt'] {
  errno: -4058,
  code: 'ENOENT',
  syscall: 'open',
  path: 'C:\\marcuscode\\nodejs\\promise\\nofile.txt'
}
```
จะเห็นว่าการใช้งาน Promise สามารถช่วยให้โปรแกรมเป็นระเบียบและอ่านง่ายขึ้นอย่างชัดเจน แม้ว่าการเขียนโปรแกรมในรูปแบบทั้งสองจะให้ผลลัพธ์การทำงานที่เหมือนกัน แต่ Promise เป็นรูปแบบใหม่ที่เราควรใช้นับจากนี้ไป สำหรับการเขียนโปรแกรมที่ทำงานแบบ Asynchronous

    Note: ภาษา JavaScript เป็นภาษาที่ทำงานแบบ Synchronous ตามธรรมชาติของมัน ฟังก์ชัน Asynchronous ที่คุณเห็นโดยส่วนมากแล้วไม่ใช่ฟังก์ชันของภาษา JavaScript เอง แต่เป็นฟังก์ชันจาก Host environment ที่ภาษา JavaScript ทำงานอยู่เช่น เว็บบราวน์เซอร์ หรือ Node.js เป็นต้น แต่เราสามารถบอกว่ามันเป็นเพื่อความเรียบง่ายในการอธิบาย

# Async/Await กับ Promise

Promise สามารถเปลี่ยนแปลงการทำงานเป็นรูปแบบ Synchronous ได้โดยใช้งานร่วมกับคำสั่ง Async/Await; การทำงานแบบ Synchronous คือการที่โปรแกรมรอจนกว่า Promise จะทำงานเสร็จสิ้น และที่สำคัญคือการใช้งานคำสั่ง Async/Await ทำให้เราสามารถละเว้นการใช้ฟังก์ชัน Callback ได้อย่างสิ้นเชิง

เราจะใช้สองตัวอย่างก่อนหน้าในเรื่องการ Promisifying สำหรับแสดงการใช้งานคำสั่ง Async/Await ร่วมกับ Promise ในตัวอย่างแรก มาเปลียนการทำงานของฟังก์ชัน setTimeout ให้เป็นแบบ Synchronous กัน นี่เป็นตัวอย่าง
```javascript
settimeout_sync.js

function setTimeoutPromise(delay) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve();
        }, delay);
    });
}

async function run() {
    await setTimeoutPromise(500);
    console.log('Task 1 done');

    await setTimeoutPromise(1000);
    console.log('Task 2 done');

    await setTimeoutPromise(2000);
    console.log('All tasks done');
}

run();
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม ผลลัพธ์นั้นเหมือนกับสองตัวอย่างก่อนหน้า แต่รูปแบบการเขียนนั้นแตกต่างออกไป
```
Task 1 done
Task 2 done
All tasks done
```
เพื่อทำให้ Promise บล็อคการทำงานของโปรแกรม เราเพิ่มคำสั่ง await ที่หน้าออบเจ็คของ Promise นี่จะทำให้โปรแกรมรอการทำงานจนกว่า Promise จะทำงานเสร็จสิ้น นั่นคือเมื่อมัน Resolve หรือ Reject
```javascript
await setTimeoutPromise(500);
console.log('Task 1 done');

await setTimeoutPromise(1000);
console.log('Task 2 done');

await setTimeoutPromise(2000);
console.log('All tasks done');
```
เนื่องจากโปรแกรมทำงานแบบตามลำดับ นั่นหมายความว่าเราไม่จำเป็นต้องใช้ฟังก์ชัน Callback อีกต่อไป นั่นทำให้คำสั่งทั้งหมดอยู่ในระดับเดียวกันและทำให้โปรแกรมอ่านง่ายมากขึ้น เมื่อไหร่ก็ตามที่ Promse ทำงานสำเร็จด้วยการ Resolve มันจะทำงานในคำสั่งต่อไปทีละบรรทัด
```javascript
async function run() {
    ...
}

run();
```
นอกจากนี้ อีกสิ่งหนึ่งที่คุณจะสังเกตเห็นก็คือเราได้สร้างฟังก์ชัน run สำหรับการรันโปรแกรมนี้ นี่เป็นสิ่งที่จำเป็นเนื่องจากคำสั่ง await จะต้องถูกใช้งานในฟังก์ชันที่เป็น Asynchronous เท่านั้น นั่นเป็นเหตุผลว่าทำไมเราเพิ่มคำสั่ง async ในการประกาศฟังก์ชัน เพื่อให้เราสามารถใช้คำสั่ง await ได้นั่นเอง

สำหรับตัวอย่างต่อไปเป็นการแปลงฟังก์ชันสำหรับอ่านจากไฟล์ที่เป็น Promise ให้มีการทำงานเป็นแบบ Synchronous
```javascript
readfile_sync.js

const fs = require('fs');

function readFilePromise(fileName, encoding) {
    return new Promise((resolve, reject) => {
        fs.readFile(fileName, encoding, (err, data) => {
            if (err) {
                reject(err)
            } else {
                resolve(data)
            }
        });
    });
}

async function run() {
    try {
        let data = await readFilePromise('./myfile.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

run();
```
นี่เป็นผลลัพธ์การทำงานของโปรแกรม
```
first
second
third
```
เหมือนกับตัวอย่างก่อนหน้า เราเพียงแค่เพิ่มคำสั่ง await หน้าฟังก์ชัน readFilePromise ที่ส่งค่ากลับเป็นออบเจ็ค Promise ซึ่งนี่จะทำให้โปรแกรมรอจนกว่าการอ่านไฟล์จะเสร็จสิ้น
```javascript
try {
    let data = await readFilePromise('./myfile.txt', 'utf8');
    console.log(data);
} catch (err) {
    console.error(err);
}
```
สำหรับในตัวอย่างนี้มีสิ่งที่แตกต่างจากตัวอย่างก่อนหน้าคือ Promise จะมีการ Resolve ในกรณีที่การอ่านไฟล์สำเร็จ และจะมีการ Reject ในกรณีที่มีการเกิดข้อผิดพลาดขึ้น

ในกรณีที่ Promise ทำงานสำเร็จหรือ Resolve ค่าจะถูกส่งกลับมาโดยตรงจากคำสั่ง await readFilePromise() ส่วนการตรวจสอบข้อผิดพลาดที่เกิดจาก Promise เราสามารถใช้คำสั่ง try catch ได้ซึ่งเป็นวิธีจัดการข้อผิดพลาดแบบปกติในภาษา JavaScript

ในบทนี้ คุณได้เรียนรู้เกี่ยวกับ Promise ในภาษา JavaScript เราได้แนะนำให้คุณรู้จักกับ Promise และการใช้งาน Promise นั้นเป็นรูปแบบใหม่ของการเขียนโปรแกรมทีทำงานแบบ Asynchronous และคุณควรใช้มัน
