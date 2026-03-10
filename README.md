# Market survey -The Einstein Kitchen

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quick Market Survey</title>

<style>

body{
font-family:Arial,sans-serif;
max-width:600px;
margin:40px auto;
padding:20px;
background:#f4f4f4;
}

h1{text-align:center;}

.question{
margin-bottom:20px;
background:white;
padding:20px;
border-radius:8px;
box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

label{display:block;margin:10px 0;}

button{
padding:10px 20px;
background:#007bff;
color:white;
border:none;
cursor:pointer;
border-radius:6px;
margin-top:10px;
}

button:hover{background:#0056b3;}

.hidden{display:none;}

/* SURPRISE QUESTION */

#q5-container{
display:none;   /* IMPORTANT FIX */
position:relative;
height:320px;
border:2px dashed #ddd;
border-radius:15px;
margin:20px 0;
overflow:hidden;
background:linear-gradient(135deg,#ffdde1,#ffffff);
flex-direction:column;
align-items:center;
justify-content:center;
text-align:center;
padding:20px;
}

#q5{
font-size:26px;
font-weight:bold;
margin-bottom:20px;
}

.buttons-container{
display:flex;
gap:20px;
justify-content:center;
}

#yesBtn{
font-size:18px;
padding:12px 28px;
border-radius:30px;
background:#ff4081;
animation:pulse 2s infinite;
}

#noBtn{
position:absolute;
width:140px;
height:45px;
background:#ff4757;
color:white;
font-weight:bold;
border-radius:25px;
pointer-events:none;
}

@keyframes pulse{
0%{transform:scale(1);}
50%{transform:scale(1.08);}
100%{transform:scale(1);}
}

#results{
display:none;
margin-top:20px;
padding:20px;
background:white;
border-radius:8px;
box-shadow:0 2px 10px rgba(0,0,0,0.1);
text-align:center;
}

</style>
</head>

<body>

<h1>Quick Market Survey</h1>

<form id="surveyForm">

<div id="q1" class="question">
<h3>1. How often do you shop online?</h3>
<label><input type="radio" name="q1">Daily</label>
<label><input type="radio" name="q1">Weekly</label>
<label><input type="radio" name="q1">Monthly</label>
<label><input type="radio" name="q1">Rarely</label>
<button type="button" onclick="nextQ(1)">Next</button>
</div>

<div id="q2" class="question hidden">
<h3>2. What influences your purchases most?</h3>
<label><input type="radio" name="q2">Price</label>
<label><input type="radio" name="q2">Quality</label>
<label><input type="radio" name="q2">Brand</label>
<label><input type="radio" name="q2">Reviews</label>
<button type="button" onclick="nextQ(2)">Next</button>
</div>

<div id="q3" class="question hidden">
<h3>3. Preferred shopping category?</h3>
<label><input type="radio" name="q3">Electronics</label>
<label><input type="radio" name="q3">Clothing</label>
<label><input type="radio" name="q3">Groceries</label>
<label><input type="radio" name="q3">Other</label>
<button type="button" onclick="nextQ(3)">Next</button>
</div>

<div id="q4" class="question hidden">
<h3>4. Satisfaction with current options (1-5)?</h3>
<label><input type="radio" name="q4">1</label>
<label><input type="radio" name="q4">2</label>
<label><input type="radio" name="q4">3</label>
<label><input type="radio" name="q4">4</label>
<label><input type="radio" name="q4">5</label>
<button type="button" onclick="nextQ(4)">Next</button>
</div>

</form>

<!-- Surprise Question -->
<div id="q5-container">

<div id="q5">Can we go on a date? ❤️</div>

<div class="buttons-container">
<button id="yesBtn">Yes 😍</button>
</div>

<button id="noBtn">Not sure yet 🤔</button>

</div>

<div id="results"></div>

<audio id="loveSound">
<source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_115b9b6d6a.mp3?filename=romantic-ambient-piano-110624.mp3">
</audio>

<script>

const form=document.getElementById("surveyForm")
const q5Container=document.getElementById("q5-container")
const yesBtn=document.getElementById("yesBtn")
const noBtn=document.getElementById("noBtn")
const results=document.getElementById("results")

function nextQ(q){

const selected=form.querySelector(`input[name="q${q}"]:checked`)

if(!selected){
alert("Please select an answer!")
return
}

document.getElementById(`q${q}`).style.display="none"

if(q<4){
document.getElementById(`q${q+1}`).classList.remove("hidden")
}
else{

/* SHOW SURPRISE QUESTION */

q5Container.style.display="flex"

}

}

/* Escape button */

function moveNoButton(){

const rect=q5Container.getBoundingClientRect()

const x=Math.random()*(rect.width-140)
const y=Math.random()*(rect.height-50)

noBtn.style.left=x+"px"
noBtn.style.top=y+"px"

}

q5Container.addEventListener("mousemove",moveNoButton)
q5Container.addEventListener("touchstart",moveNoButton)

/* Heart Explosion */

function createHearts(){

for(let i=0;i<25;i++){

const heart=document.createElement("div")
heart.innerHTML="❤️"
heart.style.position="absolute"
heart.style.left="50%"
heart.style.top="50%"
heart.style.fontSize=(Math.random()*20+20)+"px"
heart.style.transition="transform 1s ease-out,opacity 1s"

q5Container.appendChild(heart)

const dx=(Math.random()-0.5)*300
const dy=(Math.random()-0.5)*300

setTimeout(()=>{
heart.style.transform=`translate(${dx}px,${dy}px)`
heart.style.opacity=0
},10)

setTimeout(()=>heart.remove(),1000)

}

}

/* YES BUTTON */

yesBtn.addEventListener("click",(e)=>{

e.preventDefault()

document.getElementById("loveSound").play()

createHearts()

setTimeout(()=>{

q5Container.style.display="none"

results.style.display="block"

results.innerHTML=`
<h2>Survey Complete 🎉</h2>
<p style="font-size:20px">
Cool – let's discuss about the date and time ❤️
</p>
`

},900)

})

</script>

</body>
</html>
