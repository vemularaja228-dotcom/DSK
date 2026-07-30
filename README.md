<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DSK ONLINE SERVICES - Calculator</title>

<style>
body{
    font-family:Arial,sans-serif;
    background:#f0f8ff;
    margin:0;
    padding:20px;
}

.container{
    max-width:500px;
    margin:auto;
    background:#fff;
    padding:20px;
    border-radius:12px;
    box-shadow:0 4px 15px rgba(0,0,0,.1);
}

h2{
    text-align:center;
    color:#007bff;
}

label{
    display:block;
    margin-top:15px;
    font-weight:bold;
}

input{
    width:100%;
    padding:10px;
    font-size:16px;
    border-radius:6px;
    border:1px solid #ccc;
    margin-top:5px;
    box-sizing:border-box;
}

input:disabled{
    background:#eee;
}

button{
    width:100%;
    padding:12px;
    margin-top:15px;
    background:#007bff;
    color:white;
    border:none;
    border-radius:6px;
    cursor:pointer;
    font-size:16px;
}

button:hover{
    background:#0056b3;
}

.receipt{
    display:none;
    margin-top:20px;
    background:#fff;
    border:1px dashed #888;
    padding:15px;
    white-space:pre-line;
    font-family:monospace;
    border-radius:10px;
}

.footer{
    text-align:center;
    margin-top:25px;
    color:#777;
    font-size:14px;
}
</style>

</head>
<body>

<div class="container">

<h2>DSK ONLINE SERVICES</h2>

<label>Amount Added (₹)</label>
<input type="number" id="amountAdded">

<label>OR Final Bank Amount (₹)</label>
<input type="number" id="bankAmount">

<label>Platform Charges (%)</label>
<input
type="number"
id="platformRate"
value="1.9"
step="0.1"
min="0"
max="10">

<label>Fixed Server Fee (₹)</label>
<input
type="number"
id="fixedFee"
value="15">

<button onclick="calculate()">
Generate Receipt
</button>

<button onclick="shareWhatsApp()">
Share via WhatsApp
</button>

<div
class="receipt"
id="receiptOutput">
</div>

<div class="footer">
DSK ONLINE SERVICES | 85550 34821
</div>

</div>

<script>

function formatDateTime(){

const d=new Date();

const months=["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"];

const day=String(d.getDate()).padStart(2,"0");

const month=months[d.getMonth()];

const year=d.getFullYear();

let hour=d.getHours();

const minute=String(d.getMinutes()).padStart(2,"0");

const second=String(d.getSeconds()).padStart(2,"0");

const ampm=hour>=12?"PM":"AM";

hour=hour%12||12;

hour=String(hour).padStart(2,"0");

return `${day} ${month} ${year}, ${hour}:${minute}:${second} ${ampm}`;

}

function calculate(){

const amountAdded=document.getElementById("amountAdded").value.trim();

const bankAmount=document.getElementById("bankAmount").value.trim();

const rate=parseFloat(document.getElementById("platformRate").value)/100;

const fixed=parseFloat(document.getElementById("fixedFee").value);

let amount,toBank,platformFee,totalFee;

if(amountAdded){

amount=parseFloat(amountAdded);

platformFee=amount*rate;

totalFee=platformFee+fixed;

toBank=amount-totalFee;

}

else if(bankAmount){

toBank=parseFloat(bankAmount);

amount=(toBank+fixed)/(1-rate);

platformFee=amount*rate;

totalFee=platformFee+fixed;

}

else{

alert("Please enter an amount.");

return;

}

const receipt=
`-------------------------------
DSK ONLINE SERVICES
Transaction Receipt
-------------------------------
Date       : ${formatDateTime()}
Contact    : 85550 34821

Amount     : ₹${amount.toFixed(2)}
Platform % : ${(rate*100).toFixed(2)}%
Platform ₹ : ₹${platformFee.toFixed(2)}
Fixed Fee  : ₹${fixed.toFixed(2)}

-------------------------------
Total Fee  : ₹${totalFee.toFixed(2)}
To Bank    : ₹${toBank.toFixed(2)}
-------------------------------

Thank you for choosing

DSK ONLINE SERVICES

85550 34821
-------------------------------`;

const box=document.getElementById("receiptOutput");

box.style.display="block";

box.textContent=receipt;

}

function shareWhatsApp(){

const txt=document.getElementById("receiptOutput").textContent.trim();

if(!txt){

alert("Generate receipt first.");

return;

}

window.open("https://wa.me/?text="+encodeURIComponent(txt),"_blank");

}

const amountInput=document.getElementById("amountAdded");

const bankInput=document.getElementById("bankAmount");

amountInput.addEventListener("input",function(){

if(this.value.trim()!==""){

bankInput.value="";

bankInput.disabled=true;

}else{

bankInput.disabled=false;

}

});

bankInput.addEventListener("input",function(){

if(this.value.trim()!==""){

amountInput.value="";

amountInput.disabled=true;

}else{

amountInput.disabled=false;

}

});

</script>

</body>
</html>
