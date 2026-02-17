<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Your Dream House</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">

<style>
body, html {
  margin:0;
  padding:0;
  font-family: 'Roboto', sans-serif;
  height:100%;
  width:100%;
  background: url('https://images.unsplash.com/photo-1568605114967-8130f3a36994') no-repeat center center/cover;
  background-size: cover;
  overflow:hidden;
  position:relative;
}

/* Floating reviews background */
.floating-review {
  position: absolute;
  color: rgba(0,255,149,0.2);
  font-size: 20px;
  font-weight: bold;
  white-space: nowrap;
  pointer-events: none;
  animation: floatReview linear infinite;
}

@keyframes floatReview {
  0% {transform: translateX(100vw);}
  100% {transform: translateX(-100vw);}
}

/* Overlay + blur */
.overlay-bg {
  position: fixed;
  top:0; left:0;
  width:100%; height:100%;
  background: rgba(0,0,0,0.65);
  backdrop-filter: blur(3px);
  display:flex;
  justify-content:center;
  align-items:center;
  z-index: 100;
  animation: fadeIn 0.5s ease;
}

/* Popup */
.popup {
  background: #111;
  padding: 30px 25px;
  border-radius: 15px;
  width: 90%;
  max-width: 400px;
  text-align:center;
  color:white;
  box-shadow: 0 0 25px rgba(0,0,0,0.7);
  animation: fadeIn 0.5s ease;
}

h1 {
  margin-bottom: 5px;
  font-size: 28px;
  color: #00e676;
  text-shadow: 1px 1px 5px rgba(0,0,0,0.6);
}

.tagline {
  font-size:14px;
  color:#aaa;
  margin-bottom:15px;
}

/* Inputs */
input {
  width: 90%;
  padding: 12px;
  margin: 10px 0;
  border-radius: 8px;
  border:none;
  outline:none;
  font-size: 16px;
  box-shadow: 0 0 10px rgba(0,0,0,0.4) inset;
}

/* Buttons */
button {
  background: #00c853;
  color: white;
  padding: 12px;
  width: 95%;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 12px;
  transition: 0.3s;
  box-shadow: 0 5px 15px rgba(0,0,0,0.4);
}
button:hover {
  background: #00b341;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.5);
}

/* Success message */
.success {
  color: #00ff95;
  margin-top: 15px;
  font-weight: bold;
  font-size: 16px;
}

/* Hidden */
.hidden { display:none; }

/* FadeIn Animation */
@keyframes fadeIn {
  0% {opacity:0; transform: scale(0.95);}
  100% {opacity:1; transform: scale(1);}
}

/* Contact Info */
.contact {
  margin-top:15px;
  font-size:14px;
  color:#aaa;
}

/* Confetti effect */
.confetti-piece {
  position: fixed;
  width: 8px;
  height: 8px;
  background-color: #00ff95;
  opacity: 0.8;
  z-index: 9999;
  pointer-events: none;
  animation: confettiFall 3s linear forwards;
}
@keyframes confettiFall {
  0% {transform: translateY(0) rotate(0deg);}
  100% {transform: translateY(600px) rotate(720deg);}
}
</style>
</head>
<body>

<!-- Background Floating Reviews -->
<script>
const reviews = [
  "Ravi K.: Amazing service! Loved it.",
  "Priya S.: Quick registration, very reliable.",
  "Amit P.: Smooth payment, very professional.",
  "Sneha R.: Highly recommend this platform.",
  "Anil M.: Comfortable experience, great support!",
  "Sonal T.: Trusted and fast!"
];
for(let i=0;i<reviews.length;i++){
  let span = document.createElement("div");
  span.classList.add("floating-review");
  span.style.top = Math.random()*80 + "vh";
  span.style.fontSize = 16 + Math.random()*10 + "px";
  span.style.animationDuration = 20 + Math.random()*20 + "s";
  span.innerText = reviews[i];
  document.body.appendChild(span);
}
</script>

<!-- Step 1: Name + Phone -->
<div class="overlay-bg" id="step1">
  <div class="popup">
    <h1>Your Dream House</h1>
    <div class="tagline">Made in 2026 – Your trusted home booking platform</div>
    <p>Register & Confirm Your Stay</p>
    <input type="text" id="name" placeholder="Enter Your Name">
    <input type="number" id="phone" placeholder="Enter Mobile Number">
    <button onclick="goToStep2()">Continue</button>
    <div class="contact">Contact: yourdreamhouse12@gmail.com</div>
  </div>
</div>

<!-- Step 2: Payment + Transaction ID -->
<div class="overlay-bg hidden" id="step2">
  <div class="popup">
    <h1>Payment</h1>
    <p>Click below to pay via UPI</p>
    <a href="upi://pay?pa=nileshverma5001@ybl&pn=Your Dream House&am=100&cu=INR">
      <button>Pay ₹1 via UPI</button>
    </a>
    <p>Enter Transaction ID after payment</p>
    <input type="text" id="txnId" placeholder="Transaction ID">
    <button onclick="finishPayment()">Confirm Payment</button>
    <div class="contact">Contact: yourdreamhouse12@gmail.com</div>
  </div>
</div>

<!-- Step 3: Success -->
<div class="overlay-bg hidden" id="step3">
  <div class="popup">
    <h1>🎉 Registration Successful</h1>
    <div class="success" id="finalMessage"></div>
    <div class="contact">Contact: yourdreamhouse12@gmail.com</div>
  </div>
</div>

<!-- Admin Panel -->
<div class="overlay-bg hidden" id="adminPanel">
  <div class="popup">
    <h1>Admin Panel</h1>
    <div id="adminData" style="text-align:left; max-height:300px; overflow:auto;"></div>
    <button onclick="closeAdmin()">Close</button>
  </div>
</div>

<script>
// Tickets storage
let tickets = JSON.parse(localStorage.getItem("tickets")) || [];

// Step 1 → Step 2
function goToStep2(){
  let name = document.getElementById("name").value.trim();
  let phone = document.getElementById("phone").value.trim();
  if(name===""||phone===""){alert("Please fill all details"); return;}
  document.getElementById("step1").classList.add("hidden");
  document.getElementById("step2").classList.remove("hidden");
}

// Step 2 → Step 3
function finishPayment(){
  let name = document.getElementById("name").value.trim();
  let phone = document.getElementById("phone").value.trim();
  let txn = document.getElementById("txnId").value.trim();
  if(txn===""){alert("Please enter Transaction ID"); return;}

  let ticketNumber = Math.floor(Math.random()*20000)+1;
  tickets.push({name, phone, txnId:txn, ticketNumber, date:new Date().toLocaleString()});
  localStorage.setItem("tickets", JSON.stringify(tickets));

  document.getElementById("step2").classList.add("hidden");
  document.getElementById("step3").classList.remove("hidden");
  document.getElementById("finalMessage").innerHTML =
    "Your Ticket Number is <b>"+ticketNumber+"</b><br>" +
    "Transaction ID: <b>"+txn+"</b><br>" +
    "Please take a screenshot.";

  launchConfetti();
}

// Admin panel
function openAdmin(){
  document.getElementById("adminPanel").classList.remove("hidden");
  let dataDiv = document.getElementById("adminData");
  dataDiv.innerHTML="";
  tickets.forEach((t,i)=>{
    dataDiv.innerHTML += (i+1)+". Name: "+t.name+", Phone: "+t.phone+", Ticket: "+t.ticketNumber+", TxnID: "+t.txnId+", Date: "+t.date+"<br>";
  });
}
function closeAdmin(){document.getElementById("adminPanel").classList.add("hidden");}
document.addEventListener("keydown", function(e){if(e.key==='A'||e.key==='a') openAdmin();});

// Confetti
function launchConfetti(){
  for(let i=0;i<50;i++){
    let confetti = document.createElement("div");
    confetti.classList.add("confetti-piece");
    confetti.style.left=Math.random()*100+"%";
    confetti.style.backgroundColor=`hsl(${Math.random()*360},100%,50%)`;
    document.body.appendChild(confetti);
    setTimeout(()=>{confetti.remove();},3000);
  }
}
</script>
</body>
</html>
