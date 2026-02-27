<!DOCTYPE html>

<html lang="ru">

<head>

&nbsp;   <meta charset="UTF-8">

&nbsp;   <meta name="viewport" content="width=device-width, initial-scale=1.0">

&nbsp;   <title>Приключения Фьюмика и Фантика</title>

&nbsp;   <style>

&nbsp;       body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #b3e5fc; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }

&nbsp;       #game-box { width: 450px; background: white; border-radius: 30px; padding: 25px; box-shadow: 0 15px 35px rgba(0,0,0,0.2); text-align: center; border: 4px solid #0288d1; }

&nbsp;       

&nbsp;       /\* Анимация неба \*/

&nbsp;       .runway { width: 100%; height: 150px; background: #90a4ae; border-radius: 20px; position: relative; overflow: hidden; border-bottom: 8px dashed white; display: flex; align-items: flex-end; justify-content: space-between; padding-bottom: 10px; }

&nbsp;       .cloud { position: absolute; color: white; font-size: 30px; animation: fly 12s linear infinite; opacity: 0.6; }

&nbsp;       @keyframes fly { from { left: -50px; } to { left: 500px; } }

&nbsp;       

&nbsp;       #hero { font-size: 50px; position: relative; transition: 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); z-index: 5; }

&nbsp;       #target { font-size: 50px; margin-right: 10px; }

&nbsp;       

&nbsp;       .fuel-container { background: #e0e0e0; height: 20px; border-radius: 10px; margin: 15px 0; overflow: hidden; border: 2px solid #546e7a; }

&nbsp;       #fuel-level { height: 100%; width: 100%; background: linear-gradient(90deg, #ff8f00, #ffca28); transition: 0.4s; }

&nbsp;       

&nbsp;       .math-card { background: #f1f8e9; border: 3px solid #8bc34a; border-radius: 20px; padding: 20px; margin: 20px 0; }

&nbsp;       #question { font-size: 32px; color: #2e7d32; font-weight: bold; }

&nbsp;       

&nbsp;       .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }

&nbsp;       button { background: #03a9f4; color: white; border: none; padding: 15px; font-size: 20px; border-radius: 15px; cursor: pointer; box-shadow: 0 4px #0288d1; transition: 0.1s; }

&nbsp;       button:hover { background: #29b6f6; }

&nbsp;       button:active { box-shadow: 0 0; transform: translateY(4px); }

&nbsp;       

&nbsp;       .header h2 { margin: 0; color: #01579b; font-size: 22px; }

&nbsp;       .header p { margin: 5px 0; color: #546e7a; font-size: 14px; }

&nbsp;   </style>

</head>

<body>



<div id="game-box">

&nbsp;   <div class="header">

&nbsp;       <h2>✈️ Рейс Фьюмика и Фантика</h2>

&nbsp;       <p>Помоги друзьям долететь до цели!</p>

&nbsp;   </div>



&nbsp;   <div class="runway">

&nbsp;       <div class="cloud" style="top:10%; animation-duration: 15s;">☁️</div>

&nbsp;       <div class="cloud" style="top:30%; animation-duration: 10s;">☁️</div>

&nbsp;       <div id="hero"><div id="hero"><img src="ВАША\_ССЫЛКА\_НА\_ФОТО" height="70"></div>.

</div> 

&nbsp;       <div id="target">🏁</div>

&nbsp;   </div>



&nbsp;   <div class="fuel-container"><div id="fuel-level"></div></div>

&nbsp;   

&nbsp;   <div class="math-card">

&nbsp;       <div id="question">Загрузка...</div>

&nbsp;   </div>



&nbsp;   <div class="grid" id="answers">

&nbsp;       <button onclick="check(0)"></button>

&nbsp;       <button onclick="check(1)"></button>

&nbsp;       <button onclick="check(2)"></button>

&nbsp;       <button onclick="check(3)"></button>

&nbsp;   </div>

</div>



<script>

&nbsp;   let step = 0;

&nbsp;   let fuel = 100;

&nbsp;   let correctValue;

&nbsp;   const goal = 10;



&nbsp;   function next() {

&nbsp;       if (step >= goal) {

&nbsp;           document.getElementById('game-box').innerHTML = "<h1>🎉 УРА!</h1><p>Фьюмик и Фантик успешно приземлились!</p><button onclick='location.reload()'>Новый полет</button>";

&nbsp;           return;

&nbsp;       }



&nbsp;       const isMult = Math.random() > 0.5;

&nbsp;       if (isMult) {

&nbsp;           let a = Math.floor(Math.random() \* 8) + 2;

&nbsp;           let b = Math.floor(Math.random() \* 8) + 2;

&nbsp;           correctValue = a \* b;

&nbsp;           document.getElementById('question').innerText = ${a} × ${b} = ?;

&nbsp;       } else {

&nbsp;           let a = Math.floor(Math.random() \* 500) + 100;

&nbsp;           let b = Math.floor(Math.random() \* 400) + 100;

&nbsp;           correctValue = a + b;

&nbsp;           document.getElementById('question').innerText = ${a} + ${b} = ?;

&nbsp;       }

let options = \[correctValue];

&nbsp;       while(options.length < 4) {

&nbsp;           let fake = correctValue + (Math.floor(Math.random() \* 20) - 10);

&nbsp;           if (!options.includes(fake) \&\& fake > 0) options.push(fake);

&nbsp;       }

&nbsp;       options.sort(() => Math.random() - 0.5);



&nbsp;       const btns = document.querySelectorAll('#answers button');

&nbsp;       options.forEach((val, i) => {

&nbsp;           btns\[i].innerText = val;

&nbsp;           btns\[i].onclick = () => {

&nbsp;               if(val === correctValue) {

&nbsp;                   step++;

&nbsp;                   fuel = Math.min(100, fuel + 10);

&nbsp;                   document.getElementById('hero').style.left = (step \* 35) + 'px';

&nbsp;                   next();

&nbsp;               } else {

&nbsp;                   fuel -= 25;

&nbsp;                   alert("Турбулентность! Фьюмик просит пересчитать.");

&nbsp;                   if(fuel <= 0) {

&nbsp;                       alert("Топливо кончилось! Садимся на запасной аэродром.");

&nbsp;                       location.reload();

&nbsp;                   }

&nbsp;               }

&nbsp;               document.getElementById('fuel-level').style.width = fuel + '%';

&nbsp;           }

&nbsp;       });

&nbsp;   }

&nbsp;   next();

</script>

</body>

</html>


