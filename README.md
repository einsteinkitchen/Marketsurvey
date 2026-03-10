# Marketsurvey
Economic market survey
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quick Market Survey</title>
<style>
body{font-family:Arial,sans-serif;max-width:600px;margin:50px auto;padding:20px;background:#f4f4f4;}
.question{margin-bottom:20px;}
label{display:block;margin:10px 0;}
button{padding:10px 20px;background:#007bff;color:white;border:none;cursor:pointer;margin:5px;border-radius:5px;}
button:hover:not(#noBtn){background:#0056b3;transform:scale(1.05);}
#q5{text-align:center;font-size:24px;margin-top:50px;}
#q5-container{position:relative;height:250px;border:2px dashed #ddd;border-radius:10px;margin:20px 0;overflow:hidden;background:linear-gradient(45deg,#ffebee,#fff);}
#noBtn{position:absolute;transition:all 0.3s cubic-bezier(0.68,-0.55,0.265,1.55);background:#ff4757!important;font-weight:bold;z-index:10;box-shadow:0 4px 8px rgba(0,0,0,0.2);pointer-events:auto;}
#yesBtn{position:relative;z-index:5;animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{transform:scale(1);}50%{transform:scale(1.08);}}
.particles{position:absolute;width:5px;height:5px;background:#ff69b4;border-radius:50%;pointer-events:none;animation:explode 1s ease-out forwards;}
@keyframes explode{0%{transform:scale(1) translate(0,0);opacity:1;}100%{transform:scale(0) translate(var(--dx),var(--dy));opacity:0;}}
.hidden{display:none;}
#results{margin-top:20px;padding:20px;background:white;border-radius:8px;box-shadow:0 2px 10px rgba(0,0,0,0.1);}
</style>
</head>
<body>
<h1>Quick Market Survey</h1>
<form id="surveyForm">
<div id="q1" class="question">
<h3>1. How often do you shop online?</h3>
<label><input type="radio" name="q1" value="Daily"> Daily</label>
<label><input type="radio" name="q1" value="Weekly"> Weekly</label>
<label><input type="radio" name="q1" value="Monthly"> Monthly</label>
<label><input type="radio" name="q1" value="Rarely"> Rarely</label>
<button type="button" onclick="nextQ(1)">Next</button>
</div>
<div id="q2" class="question hidden">
<h3>2. What influences your purchases most?</h3>
<label><input type="radio" name="q2" value="Price"> Price</label>
<label><input type="radio" name="q2" value="Quality"> Quality</label>
<label><input type="radio" name="q2" value="Brand"> Brand</label>
<label><input type="radio" name="q2" value="Reviews"> Reviews</label>
<button type="button" onclick="nextQ(2)">Next</button>
</div>
<div id="q3" class="question hidden">
<h3>3. Preferred shopping category?</h3>
<label><input type="radio" name="q3" value="Electronics"> Electronics</label>
<label><input type="radio" name="q3" value="Clothing"> Clothing</label>
<label><input type="radio" name="q3" value="Groceries"> Groceries</label>
<label><input type="radio" name="q3" value="Other"> Other</label>
<button type="button" onclick="nextQ(3)">Next</button>
</div>
<div id="q4" class="question hidden">
<h3>4. Satisfaction with current options (1-5)?</h3>
<label><input type="radio" name="q4" value="1"> 1 (Very low)</label>
<label><input type="radio" name="q4" value="2"> 2</label>
<label><input type="radio" name="q4" value="3"> 3</label>
<label><input type="radio" name="q4" value="4"> 4</label>
<label><input type="radio" name="q4" value="5"> 5 (Very high)</label>
<button type="button" onclick="nextQ(4)">Next</button>
</div>
<div id="q5-container" class="hidden">
<div id="q5">Do you love me? ❤️</div>
<button id="yesBtn">Yes 😍</button>
<button id="noBtn">No</button>
</div>
<div id="results" class="hidden"></div>
</form>
<script>
let currentQ=1;const form=document.getElementById('surveyForm'),responses={};
function nextQ(q){if(!form.querySelector(`input[name="q${q}"]:checked`)){alert('Please select an answer!');return;}
responses[`q${q}`]=form.querySelector(`input[name="q${q}"]:checked`).value;document.getElementById(`q${q}`).classList.add('hidden');if(q<4){document.getElementById(`q${q+1}`).classList.remove('hidden');currentQ++;}
else{document.getElementById('q5-container').classList.remove('hidden');}}

const noBtn=document.getElementById('noBtn'),yesBtn=document.getElementById('yesBtn'),q5Container=document.getElementById('q5-container'),results=document.getElementById('results');

// Position No button initially
noBtn.style.left = '20px';
noBtn.style.top = '80px';

// Enhanced mouse avoidance for No button
q5Container.addEventListener('mousemove', (e) => {
    const rect = q5Container.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    
    // Calculate avoidance position (opposite direction from cursor)
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;
    const btnWidth = noBtn.offsetWidth;
    const btnHeight = noBtn.offsetHeight;
    
    let avoidX = x + (x > centerX ? -100 - Math.random() * 50 : 100 + Math.random() * 50);
    let avoidY = y + (y > centerY ? -80 - Math.random() * 40 : 80 + Math.random() * 40);
    
    // Keep within bounds
    avoidX = Math.max(btnWidth/2, Math.min(rect.width - btnWidth/2, avoidX));
    avoidY = Math.max(btnHeight/2, Math.min(rect.height - btnHeight/2, avoidY));
    
    // Add rotation and scale for extra effect
    const angle = (Math.random() - 0.5) * 180;
    
    noBtn.style.left = avoidX + 'px';
    noBtn.style.top = avoidY + 'px';
    noBtn.style.transform = `rotate(${angle}deg) scale(1.1)`;
});

// Reset position when mouse leaves
q5Container.addEventListener('mouseleave', () => {
    noBtn.style.transform = 'rotate(0deg) scale(1)';
});

function createParticles(){
    for(let i=0;i<20;i++){
        const particle=document.createElement('div');
        particle.className='particles';
        const angle=(Math.PI*2*i)/20,velocity=100+Math.random()*100;
        particle.style.left='50%';
        particle.style.top='50%';
        particle.style.setProperty('--dx',(Math.cos(angle)*velocity)+'px');
        particle.style.setProperty('--dy',(Math.sin(angle)*velocity)+'px');
        q5Container.appendChild(particle);
        setTimeout(()=>particle.remove(),1000);
    }
}

yesBtn.addEventListener('click',()=>{
    responses.q5='Yes ❤️';
    createParticles();
    setTimeout(()=>{
        q5Container.classList.add('hidden');
        results.innerHTML='<h2>Thank you! Survey complete. 💕</h2><pre>'+JSON.stringify(responses,null,2)+'</pre>';
        results.classList.remove('hidden');
        console.log('Responses:',responses);
    },1000);
});
</script>
</body>
</html>
