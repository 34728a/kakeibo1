[index.html](https://github.com/user-attachments/files/27567210/index.html)
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>家計簿アプリ</title>

<style>
body {
  font-family: sans-serif;
  background: #f5f6fa;
  margin: 0;
  padding: 10px;
}

h1 {
  text-align: center;
}

 .card{
  background: #fff0f5;
  padding: 10px;
  border-bottom: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0,1);
}

input, select {
  width: 100%;
  padding: 10px;
  border: none;
  background: #fffff0;
  border-radius: 8px;
  border: 1px solid #ccc;
}

button {
  width: 100%;
  padding: 10px;
  border: none;
  background: #FFCCE5;
  color: white;
  border-radius: 10px;
  font-size: 13px;
}

li {
  list-style: none;
  padding: 8px;
  text-align: center;
}
</style>

</head>

<h1>💰　家計簿</h1>

<div class="card">
  <input id="item" placeholder="項目">

  <select id="category">
    <option>食費</option>
    <option>交通費</option>
    <option>娯楽</option>
    <option>その他</option>
  </select>

  <input id="price" placeholder="金額">
  <button onclick="add()">追加</button>
</div>

<div class="card">
  <h3>履歴</h3>
  <ul id="list"></ul>
</div>

<div class="card total">
  合計:<span id="total">0</span>円
</div>

<script>
let data = [];
let total = 0;

window.onload = () =>{
  const saved = localStorage.getItem("kakeibo");
  if (saved){
   data = JSON.parse(saved);
   render();
  }
};

function add(){
  const item = document.getElementById("item").value;
  const price = Number(document.getElementById("price").value);
  const category = document.getElementById("category").value;

  const date = new Date().toLocaleDateString("ja-JP");

  data.push({ item, price, category, date });

  localStorage.setItem("kakeibo",JSON.stringify(data));

  render();
}

function render(){
  const list = document.getElementById("list");
  list.innerHTML = "";
  total = 0;

  data.forEach((d, index) => {
   const li = document.createElement("li");
   li.textContent = `${d.date} [${d.category}]${d.item}:${d.price}円`;

   if(d.category === "食費"){
    li.style.background = "#ffd1e8";
   }else if(d.category === "交通費"){
    li.style.background = "#ffd1d1";
   }else if(d.category ==="娯楽"){
    li.style.background = "#ffd1ff";
   }else {
    li.style.background = "#ff93c9";
   }

   const del = document.createElement("button");
   del.textContent = "削除";
   del.style.marginLeft = "10px"
   del.style.background = "#e74c3c";
   del.style.color = "white";
   del.style.border = "none";
   del.style.borderRadius = "5px";
   del.style.cursor = "pointer";

   del.onclick = () => {
    data.splice(index, 1);
    localStorage.setItem("kakeibo", JSON.stringify(data));
    render();
   };

   const edit = document.createElement("button");
   edit.textContent = "編集";
   edit.style.marginLeft = "10px";
   edit.style.background = "#8989ff";
   edit.style.color = "white";
   edit.style.border = "none";
   edit.style.borderRadius = "5px";
   edit.style.cursor = "pointer";
   

   edit.onclick = () => {
     document.getElementById("item").value = d.item;
     document.getElementById("price").value = d.price;
     document.getElementById("category").value = d.category;

     data.splice(index, 1);
     localStorage.setItem("kakeibo", JSON.stringify(data));
     render();
   };

   li.appendChild(del);
   li.appendChild(edit);
   list.appendChild(li);

   total += d.price;
  });

  document.getElementById("total").textContent = total;
}
</script>

</body>
</html>
