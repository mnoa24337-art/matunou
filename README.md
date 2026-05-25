<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <title>商品購入フォーム</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>  
  
  <div class="all">
    <h1 id="subtitle">商品購入フォーム</h1>
      <form id="myForm">
         <div class="nearlyall">
        購入数
        <br> <br>大根の酢漬（３５０円）<br>
        <select name="RadishPickled" id="RadishPickled">
          <option value="0" selected>0</option>
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select> 
        個
        <br> <br>大根の醤油漬（３５０円）<br>
        <select name="RadishSoySauce" id="RadishSoySauce"> 
          <option value="0" selected>0</option>
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select> 
        個
        <br> <br>大根のピリ辛醤油漬（３５０円）<br> 
         <select name="RadishSpicySoySauce" id="RadishSpicySoySauce"> 
          <option value="0" selected>0</option>
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br> <br>いちごジャム（４５０円）
           <br>
         <select name="StrawberryJam" id="StrawberryJam"> 
          <option value="0" selected>0</option>
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br><br>いちじくジャム（４５０円）
           <br>
        <select name="FigJam" id="FigJam"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option> 
        </select> 
        個
        <br><br>さつまいもジャム（４５０円）<br> 
        <select name="SweetpotatoJam" id="SweetpotatoJam"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br><br>マーマレード（４５０円）<br> 
         <select name="marmalade" id="marmalade"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br><br>味噌（５００円）<br> 
          <select name="miso" id="miso"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br><br>キムチ（５００円）<br> 
         <select name="kimchi" id="kimchi"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select> 
        個
        <br><br>カクテキキムチ（５００円）<br> 
        <select name="kakuteki" id="kakuteki"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
        <br><br>たけのこの水煮（１０００円）<br> 
        <select name="bamboo" id="bamboo"> 
          <option value="0" selected>0</option> 
          <option value="1">1</option> 
          <option value="2">2</option> 
          <option value="3">3</option> 
        </select>  
        個
<br>合計：<span id="out">0</span> 円
       <br><br> お名前<br>

<div class="name-row">

  <div class="field">
    <span>セイ</span>
    <input type="text"
      style="width:80px;"
      id="name"
      placeholder="マツノウ"
      pattern="^[ァ-ヶー]+$"
      required>
     </div>

  <div class="field">
    <span>メイ</span>
    <input type="text"
      style="width:80px;"
      id="firstname"
      placeholder="タロウ"
      pattern="^[ァ-ヶー]+$"
      required>
  </div>

</div>
        
           
<br><br>
        メールアドレス<br> 
        <input type="email" id="email" required><br><br>
       
<br><br>

<button type="button" onclick="clearData()">保存データ削除</button>

 <br><br> 
    <div class="policy">
　プライバシーポリシー <br>
　当サービスでは、商品のお渡しおよび関連する連絡を行うため、
  <br>お客様の氏名（フルネーム）およびメールアドレスを取得します。 
  <br>取得した個人情報は、以下の目的でのみ利用いたします。 
　<br><br>・商品のお渡し 
  <br>・商品に関する必要なご連絡 
　<br><br>お客様の個人情報は、法令に基づく場合を除き、事前の同意なく第三者へ提供することはありません。 
また、個人情報は利用目的が達成され次第、適切な方法で管理・削除いたします。 
   </div>
  </div><br><input type="checkbox" id="agree" name="agree" required>

    同意する
        <br><button type="submit">注文する</button> 
  </form> 
  <p id="message"></p>
     </div> 
       <script>
// ===== 要素取得 =====
const form = document.getElementById("myForm");
const msg = document.getElementById("message");
const result = document.getElementById("out");
const fields = document.querySelectorAll("input, select");

// ===== 商品価格 =====
const prices = {
  StrawberryJam: 450,
  FigJam: 450,
  SweetpotatoJam: 450,
  marmalade: 450,
  miso: 500,
  kimchi: 500,
  kakuteki: 500,
  bamboo: 1000,
  RadishPickled: 350,
  RadishSoySauce: 350,
  RadishSpicySoySauce: 350
};

// ===== 合計計算 =====
function calc() {
  let total = 0;

  Object.keys(prices).forEach(id => {
    const el = document.getElementById(id);
    if (!el) return;

    const qty = Number(el.value) || 0;
    total += prices[id] * qty;
  });

  result.textContent = total.toLocaleString();
}

// ===== 保存 =====
function saveProgress() {
  const data = {};

  fields.forEach(el => {
    if (!el.id) return;

    if (el.type === "checkbox") {
      data[el.id] = el.checked;
    } else {
      data[el.id] = el.value;
    }
  });

  localStorage.setItem("orderData", JSON.stringify(data));
}

// ===== 復元 =====
function loadProgress() {
  const saved = localStorage.getItem("orderData");
  if (!saved) return;

  const data = JSON.parse(saved);

  fields.forEach(el => {
    if (!el.id) return;
    if (data[el.id] === undefined) return;

    if (el.type === "checkbox") {
      el.checked = data[el.id];
    } else {
      el.value = data[el.id];
    }
  });
}

// ===== 注文済みチェック =====
function checkSubmitted() {
  if (localStorage.getItem("ordered")) {
    form.querySelectorAll("input, select, button").forEach(el => {
      el.disabled = true;
    });
    msg.textContent = "すでに注文済みです";
  }
}

// ===== リセット =====
function clearData() {
  localStorage.removeItem("orderData");
  localStorage.removeItem("ordered");

  fields.forEach(el => {
    if (el.type === "checkbox") {
      el.checked = false;
    } else if (el.tagName === "SELECT") {
      el.value = "0";
    } else {
      el.value = "";
    }

    el.disabled = false;
  });

  msg.textContent = "";
  calc();
}

// ===== イベント設定 =====
fields.forEach(el => {
  el.addEventListener("change", () => {
    calc();
    saveProgress();
  });

  el.addEventListener("input", saveProgress);
});

// ===== 送信処理 =====
form.addEventListener("submit", function(e) {
  e.preventDefault();

  localStorage.setItem("ordered", "true");

  msg.textContent = "注文が完了しました";
  checkSubmitted();
});

// ===== 初期化 =====
window.addEventListener("DOMContentLoaded", () => {
  loadProgress();
  calc();
  checkSubmitted();
});
</script>

    </html>
