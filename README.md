<!DOCTYPE html>
<html lang="ja">

<head>

  <meta charset="UTF-8">

  <title>商品購入フォーム</title>

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >

  <link rel="stylesheet" href="style.css">

</head>


<body>

<div class="all">

  <h1 id="subtitle">
    商品購入フォーム
  </h1>


  <form
    id="myForm"
    action="https://script.google.com/macros/s/AKfycbxvnuEt100FzrflcktC54rBCuoMQ0Ce8kIFVu0QODpI4Kf3TRWU7Cx-9_Kz9DhnLoOMVA/exec"
    method="POST"
  >
  <input
  type="hidden"
  name="注文ID"
  id="orderId"
>

    <div class="nearlyall">


      <!-- ============================== -->
      <!-- 商品 -->
      <!-- ============================== -->

      <div id="products">

        商品情報を読み込んでいます...

      </div>


      <!-- ============================== -->
      <!-- 合計 -->
      <!-- ============================== -->

      <br>

      合計：
      <span id="out">0</span>
      円

      <input
        type="hidden"
        name="合計"
        id="totalPrice"
      >


      <br>
      <br>


      <!-- ============================== -->
      <!-- 名前 -->
      <!-- ============================== -->

      お名前

      <br>


      <div class="name-row">

        <div class="field">

          <label for="name">
            セイ
          </label>

          <input
            type="text"
            name="セイ"
            id="name"
            placeholder="マツノウ"
            pattern="^[ァ-ヶー]+$"
            required
          >

        </div>


        <div class="field">

          <label for="firstname">
            メイ
          </label>

          <input
            type="text"
            name="メイ"
            id="firstname"
            placeholder="タロウ"
            pattern="^[ァ-ヶー]+$"
            required
          >

        </div>


        <input
          type="hidden"
          name="お名前"
          id="fullName"
        >

      </div>


      <br>
      <br>


      <!-- ============================== -->
      <!-- お渡し時間 -->
      <!-- ============================== -->

      <label for="pickupTime">
        お渡し時間
      </label>

      <br>


      <select
        id="pickupTime"
        name="お渡し時間"
        required
      >

        <option value="">
          お渡し時間を選択してください
        </option>

      </select>


      <br>
      <br>
      <br>


      <!-- ============================== -->
      <!-- メール -->
      <!-- ============================== -->

      <label for="email">
        メールアドレス
      </label>

      <br>

      <input
        type="email"
        id="email"
        name="メールアドレス"
        required
      >


      <br>
      <br>
      <br>


      <!-- ============================== -->
      <!-- 保存データ削除 -->
      <!-- ============================== -->

      <button
        type="button"
        onclick="clearData()"
      >
        保存データ削除
      </button>


      <br>
      <br>


      <!-- ============================== -->
      <!-- プライバシーポリシー -->
      <!-- ============================== -->

      <div class="policy">

        プライバシーポリシー

        <br>

        当サービスでは、商品のお渡しおよび関連する連絡を行うため、
        <br>
        お客様の氏名（フルネーム）およびメールアドレスを取得します。

        <br>

        取得した個人情報は、以下の目的でのみ利用いたします。

        <br>
        <br>

        ・商品のお渡し

        <br>

        ・商品に関する必要なご連絡

        <br>
        <br>

        お客様の個人情報は、法令に基づく場合を除き、
        事前の同意なく第三者へ提供することはありません。

        また、個人情報は利用目的が達成され次第、
        適切な方法で管理・削除いたします。


        <br>

        <input
          type="checkbox"
          id="agree"
          name="agree"
          required
        >

        同意する

      </div>


      <br>


      <!-- ============================== -->
      <!-- 送信 -->
      <!-- ============================== -->

      <button
        type="submit"
        id="submitButton"
      >
        注文する
      </button>


    </div>

  </form>


  <p id="message"></p>


</div>


<script>

  
// ========================================
// 固有注文ID
// ========================================

function getOrderId() {

  let orderId =
    localStorage.getItem("orderId");

  // 初回アクセス
  if (!orderId) {

    orderId =
      crypto.randomUUID();

    localStorage.setItem(
      "orderId",
      orderId
    );

  }

  return orderId;

}


// ========================================
// 注文IDをフォームに設定
// ========================================

function setOrderId() {

  const orderId =
    getOrderId();

  document.getElementById(
    "orderId"
  ).value =
    orderId;

}


setOrderId();

// ========================================
// GAS URL
// ========================================

const GAS_URL =
  "https://script.google.com/macros/s/AKfycbxvnuEt100FzrflcktC54rBCuoMQ0Ce8kIFVu0QODpI4Kf3TRWU7Cx-9_Kz9DhnLoOMVA/exec";


// ========================================
// 要素取得
// ========================================

const form =
  document.getElementById(
    "myForm"
  );


const productsElement =
  document.getElementById(
    "products"
  );


const result =
  document.getElementById(
    "out"
  );


const totalPrice =
  document.getElementById(
    "totalPrice"
  );


const message =
  document.getElementById(
    "message"
  );


const submitButton =
  document.getElementById(
    "submitButton"
  );


// 商品データ
let products = [];


// 送信中フラグ
let sending = false;


// ========================================
// 商品情報取得
// ========================================

async function loadProducts() {

  try {

    const response =
      await fetch(
        GAS_URL +
        "?mode=products"
      );


    if (!response.ok) {

      throw new Error(
        "商品情報の取得に失敗しました。"
      );

    }


    const data =
      await response.json();


    if (
      !data ||
      !Array.isArray(
        data.products
      )
    ) {

      throw new Error(
        data.error ||
        "商品情報を取得できませんでした。"
      );

    }


    // 商品データ
    products =
      data.products;


    // お渡し時間
    createPickupTimes(
      data.pickupTimes || []
    );


    // 商品一覧
    createProducts();


    // 保存データ復元
    loadProgress();


    // 合計計算
    calc();


    // フルネーム
    updateFullName();

  } catch (error) {

    console.error(
      error
    );


    productsElement.textContent =
      "商品情報の読み込みに失敗しました。";

  }

}


// ========================================
// お渡し時間一覧を作る
// ========================================

function createPickupTimes(
  pickupTimes
) {

  const select =
    document.getElementById(
      "pickupTime"
    );


  if (!select) return;


  // 初期化
  select.innerHTML =
    "";


  // 初期選択肢
  const firstOption =
    document.createElement(
      "option"
    );


  firstOption.value =
    "";


  firstOption.textContent =
    "お渡し時間を選択してください";


  firstOption.selected =
    true;


  select.appendChild(
    firstOption
  );


  // 時間一覧
  pickupTimes.forEach(
    time => {

      const option =
        document.createElement(
          "option"
        );


      option.value =
        time;


      option.textContent =
        time;


      select.appendChild(
        option
      );

    }
  );


  // 保存
  select.addEventListener(
    "change",
    () => {

      saveProgress();

    }
  );

}


// ========================================
// 商品一覧を作る
// ========================================

function createProducts() {

  productsElement.innerHTML =
    "";


  if (
    products.length === 0
  ) {

    productsElement.textContent =
      "現在、商品がありません。";

    return;

  }


  const title =
    document.createElement(
      "div"
    );


  title.textContent =
    "購入数";


  productsElement.appendChild(
    title
  );


  products.forEach(
    (product, index) => {

      const wrapper =
        document.createElement(
          "div"
        );


      wrapper.className =
        "product";


      // ========================================
      // 商品名
      // ========================================

      const label =
        document.createElement(
          "label"
        );


      label.textContent =
        `${product.name}（${product.price.toLocaleString()}円）`;


      const id =
        "product_" +
        index;


      label.setAttribute(
        "for",
        id
      );


      wrapper.appendChild(
        label
      );


      wrapper.appendChild(
        document.createElement(
          "br"
        )
      );


      // ========================================
      // 注文受付終了
      // ========================================

      if (
        product.soldOut
      ) {

        const soldOut =
          document.createElement(
            "span"
          );


        soldOut.textContent =
          "現在、注文受付終了";


        soldOut.style.fontWeight =
          "bold";


        wrapper.appendChild(
          soldOut
        );

      } else {

        // ========================================
        // セレクトボックス
        // ========================================

        const select =
          document.createElement(
            "select"
          );


        select.id =
          id;


        select.name =
          product.name;


        // ========================================
        // 最大購入可能数
        // ========================================

        let max =
          product.max;


        // 上限あり
        if (
          product.remaining !== null
        ) {

          max =
            Math.min(
              max,
              product.remaining
            );

        }


        // ========================================
        // 0～最大数
        // ========================================

        for (
          let i = 0;
          i <= max;
          i++
        ) {

          const option =
            document.createElement(
              "option"
            );


          option.value =
            i;


          option.textContent =
            i;


          if (
            i === 0
          ) {

            option.selected =
              true;

          }


          select.appendChild(
            option
          );

        }


        // 個
        const unit =
          document.createTextNode(
            " 個"
          );


        // ========================================
        // イベント
        // ========================================

        select.addEventListener(
          "change",
          () => {

            calc();

            saveProgress();

          }
        );


        select.addEventListener(
          "input",
          () => {

            calc();

            saveProgress();

          }
        );


        wrapper.appendChild(
          select
        );


        wrapper.appendChild(
          unit
        );

      }


      productsElement.appendChild(
        wrapper
      );


      productsElement.appendChild(
        document.createElement(
          "br"
        )
      );

    }
  );

}


// ========================================
// 合計計算
// ========================================

function calc() {

  let total = 0;


  products.forEach(
    (product, index) => {

      const select =
        document.getElementById(
          "product_" +
          index
        );


      if (!select) return;


      const quantity =
        Number(
          select.value
        ) || 0;


      total +=
        product.price *
        quantity;

    }
  );


  result.textContent =
    total.toLocaleString();


  totalPrice.value =
    total;

}


// ========================================
// フルネーム更新
// ========================================

function updateFullName() {

  const sei =
    document.getElementById(
      "name"
    ).value;


  const mei =
    document.getElementById(
      "firstname"
    ).value;


  document.getElementById(
    "fullName"
  ).value =
    sei +
    " " +
    mei;

}


// ========================================
// 入力内容保存
// ========================================

function saveProgress() {

  const data = {};


  // 商品
  products.forEach(
    (product, index) => {

      const select =
        document.getElementById(
          "product_" +
          index
        );


      if (select) {

        data[select.id] =
          select.value;

      }

    }
  );


  // セイ
  data.name =
    document.getElementById(
      "name"
    ).value;


  // メイ
  data.firstname =
    document.getElementById(
      "firstname"
    ).value;


  // メール
  data.email =
    document.getElementById(
      "email"
    ).value;


  // お渡し時間
  const pickupTime =
    document.getElementById(
      "pickupTime"
    );


  if (pickupTime) {

    data.pickupTime =
      pickupTime.value;

  }


  // 同意
  data.agree =
    document.getElementById(
      "agree"
    ).checked;


  localStorage.setItem(
    "orderData",
    JSON.stringify(
      data
    )
  );

}


// ========================================
// 保存データ復元
// ========================================

function loadProgress() {

  const saved =
    localStorage.getItem(
      "orderData"
    );


  if (!saved) return;


  try {

    const data =
      JSON.parse(
        saved
      );


    // ========================================
    // 商品
    // ========================================

    products.forEach(
      (product, index) => {

        const select =
          document.getElementById(
            "product_" +
            index
          );


        if (
          select &&
          data[select.id] !==
          undefined
        ) {

          const value =
            Number(
              data[select.id]
            );


          if (
            value >= 0 &&
            value <=
            Number(
              select.options[
                select.options.length - 1
              ].value
            )
          ) {

            select.value =
              value;

          }

        }

      }
    );


    // ========================================
    // 名前
    // ========================================

    if (
      data.name !==
      undefined
    ) {

      document.getElementById(
        "name"
      ).value =
        data.name;

    }


    // ========================================
    // メイ
    // ========================================

    if (
      data.firstname !==
      undefined
    ) {

      document.getElementById(
        "firstname"
      ).value =
        data.firstname;

    }


    // ========================================
    // メール
    // ========================================

    if (
      data.email !==
      undefined
    ) {

      document.getElementById(
        "email"
      ).value =
        data.email;

    }


    // ========================================
    // お渡し時間
    // ========================================

    if (
      data.pickupTime !==
      undefined
    ) {

      const pickupTime =
        document.getElementById(
          "pickupTime"
        );


      if (
        pickupTime
      ) {

        pickupTime.value =
          data.pickupTime;

      }

    }


    // ========================================
    // 同意
    // ========================================

    if (
      data.agree !==
      undefined
    ) {

      document.getElementById(
        "agree"
      ).checked =
        data.agree;

    }

  } catch (error) {

    console.error(
      "保存データの読み込みに失敗しました。",
      error
    );

  }

}


// ========================================
// テキスト入力イベント
// ========================================

document
  .getElementById(
    "name"
  )
  .addEventListener(
    "input",
    () => {

      updateFullName();

      saveProgress();

    }
  );


document
  .getElementById(
    "firstname"
  )
  .addEventListener(
    "input",
    () => {

      updateFullName();

      saveProgress();

    }
  );


document
  .getElementById(
    "email"
  )
  .addEventListener(
    "input",
    () => {

      saveProgress();

    }
  );


document
  .getElementById(
    "agree"
  )
  .addEventListener(
    "change",
    () => {

      saveProgress();

    }
  );


// ========================================
// 注文送信
// ========================================

form.addEventListener(
  "submit",
  async function(e) {

    e.preventDefault();


    // すでに送信中なら終了
    if (sending) return;


    // 送信前に更新
    updateFullName();

    calc();


    // 入力チェック
    if (
      !form.checkValidity()
    ) {

      form.reportValidity();

      return;

    }


    // 送信開始
    sending = true;


    submitButton.disabled =
      true;


    submitButton.textContent =
      "送信中...";


    message.textContent =
      "注文を送信しています...";


    try {

      const formData =
        new FormData(
          form
        );


      const response =
        await fetch(
          GAS_URL,
          {
            method:
              "POST",

            body:
              formData

          }
        );


      const data =
        await response.json();


      if (
        data.result !==
        "success"
      ) {

        throw new Error(
          data.error ||
          "注文に失敗しました。"
        );

      }


      // 注文済み
      localStorage.setItem(
        "ordered",
        "true"
      );


      // 入力データ削除
      localStorage.removeItem(
        "orderData"
      );


      message.textContent =
        "注文が完了しました。ありがとうございます。";


      // 送信後はボタン無効
      submitButton.disabled =
        true;

    } catch (error) {

      console.error(
        error
      );


      alert(
        "送信に失敗しました。\n" +
        "時間をおいてもう一度お試しください。"
      );


      sending = false;


      submitButton.disabled =
        false;


      submitButton.textContent =
        "注文する";


      message.textContent =
        "";

    }

  }
);


// ========================================
// 注文済みチェック
// ========================================

window.addEventListener(
  "DOMContentLoaded",
  () => {

    if (
      localStorage.getItem(
        "ordered"
      )
    ) {

      submitButton.disabled =
        true;


      submitButton.textContent =
        "注文済み";


      message.textContent =
        "この端末ではすでに注文済みです。";

    }

  }
);


// ========================================
// 保存データ削除
// ========================================

function clearData() {

  localStorage.removeItem(
    "orderData"
  );


  localStorage.removeItem(
    "ordered"
  );


  location.reload();

}


// ========================================
// 商品読み込み開始
// ========================================

loadProducts();

</script>


</body>

</html>
