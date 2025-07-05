<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Payment Flow</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* Existing styles from previous step (Payment Form and Loading Screen) */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #2563eb; /* Darker blue background */
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align to top for initial form */
            min-height: 100vh;
            padding-top: 2rem; /* Add some padding to the top */
            position: relative; /* For absolute positioning of header */
            overflow-x: hidden; /* Prevent horizontal scroll */
        }
        .header {
            background-color: #2563eb; /* Blue header background */
            color: white;
            padding: 1rem 1.5rem;
            display: flex;
            align-items: center;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            width: 100%;
            box-sizing: border-box;
            z-index: 10; /* Ensure header is above other content */
        }
        .back-arrow {
            font-size: 1.5rem;
            margin-right: 1rem;
        }
        .container { /* This applies to paymentForm and bankTransferScreen */
            background-color: #ffffff;
            border-radius: 1rem; /* Rounded corners for the white card */
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            width: 90%; /* Responsive width */
            max-width: 400px; /* Max width for desktop */
            padding: 1.5rem;
            margin-top: 6rem; /* Adjusted margin-top to account for fixed header */
            box-sizing: border-box; /* Include padding in width */
        }
        input[type="text"],
        input[type="email"] {
            width: 100%;
            padding: 0.75rem 1rem;
            border: 1px solid #d1d5db; /* Light gray border */
            border-radius: 0.5rem; /* Rounded corners for inputs */
            font-size: 1rem;
            color: #374151; /* Darker text color */
        }
        input::placeholder {
            color: #9ca3af; /* Placeholder color */
        }
        .label {
            font-size: 0.875rem; /* Small label text */
            font-weight: 500;
            color: #374151; /* Darker text color */
            margin-bottom: 0.5rem;
            display: block;
        }
        .button {
            background-color: #2563eb; /* Blue button */
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            font-weight: 600;
            font-size: 1.125rem; /* Larger font size for button */
            width: 100%;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .button:hover {
            background-color: #1d4ed8; /* Darker blue on hover */
        }

        /* Loading Screen Styles */
        .loading-screen {
            display: none; /* Hidden by default */
            position: fixed; /* Cover the entire screen */
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #2563eb; /* Same blue background as body */
            justify-content: center;
            align-items: center;
            flex-direction: column;
            z-index: 20; /* Above everything else */
        }

        .loading-card {
            background-color: #ffffff;
            border-radius: 1rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            width: 90%;
            max-width: 350px; /* Slightly smaller card for loading */
            padding: 2rem;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .loading-spinner {
            width: 80px;
            height: 80px;
            border: 8px solid #e0e7ff; /* Light blue ring */
            border-top: 8px solid #2563eb; /* Blue spinning part */
            border-radius: 50%;
            animation: spin 1.5s linear infinite;
            margin-bottom: 1.5rem;
            position: relative;
        }

        .loading-spinner::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 30px; /* Size of the inner square */
            height: 30px;
            border: 2px solid #2563eb; /* Blue border for the square */
            border-radius: 0.25rem; /* Slightly rounded corners for the square */
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .loading-text {
            font-size: 1.5rem;
            font-weight: 600;
            color: #1f2937; /* Dark gray text */
            margin-bottom: 0.75rem;
        }

        .loading-subtext {
            font-size: 1rem;
            color: #6b7280; /* Medium gray text */
            line-height: 1.5;
        }

        /* New styles from the user's provided HTML (Bank Transfer Screen and Overlays) */
        /* General Body Styles - already handled by main body style, will ensure consistency */
        body.bank-transfer-bg {
            background-color: #f2f2f7; /* Light grey background from iOS */
            align-items: flex-start; /* Align to the top, as in the screenshot */
            padding-top: 0; /* No padding top for this screen */
        }

        /* Main Bank Transfer Container (Initial Screen) */
        .bank-transfer-container { /* Renamed from .container to avoid conflict */
            max-width: 400px;
            width: 100%;
            margin: 0;
            background-color: #fff;
            border-radius: 0;
            padding: 0;
            box-shadow: none;
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out; /* For smooth transitions */
            z-index: 1; /* Ensure it's below overlays */
            display: none; /* Hidden by default */
            min-height: 100vh; /* Take full height */
            flex-direction: column; /* Make it a flex container for its content */
        }

        /* Styles for the top header section (Bank Transfer Screen) */
        .bt-top-header { /* Renamed from .top-header to avoid conflict */
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background-color: #fff;
            border-bottom: 1px solid #e0e0e0;
            font-size: 17px;
            font-weight: 600;
            color: #333;
        }

        .cancel-button {
            color: #007AFF; /* iOS blue for cancel */
            font-size: 15px;
            cursor: pointer;
            font-weight: 400;
        }

        /* Styles for the main content area (Bank Transfer Screen) */
        .bt-content-area { /* Renamed from .content-area to avoid conflict */
            padding: 20px;
            background-color: #f2f2f7;
            flex-grow: 1; /* Allow content area to grow and push button to bottom */
            display: flex;
            flex-direction: column;
        }

        .amount-display {
            text-align: center;
            margin-top: 15px;
            margin-bottom: 15px;
        }

        .amount-display .main-amount {
            font-size: 28px;
            font-weight: bold;
            color: #333;
        }

        .amount-display .email {
            font-size: 14px;
            color: #666;
            margin-top: 5px;
        }

        .instruction {
            text-align: center;
            font-size: 15px;
            color: #333;
            margin: 20px 0;
        }

        .info-box {
            background-color: #fff;
            border-radius: 12px;
            padding: 15px 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }

        .info-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }

        .info-row:last-child {
            border-bottom: none;
        }

        .info-label {
            font-size: 15px;
            color: #555;
            flex-grow: 1;
        }

        .info-value-group {
            display: flex;
            align-items: center;
        }

        .info-value {
            font-weight: 600;
            font-size: 15px;
            color: #333;
            margin-right: 10px;
        }

        .copy-button {
            background-color: #ff9500;
            color: #fff;
            border: none;
            border-radius: 6px;
            padding: 5px 12px;
            font-size: 13px;
            cursor: pointer;
            font-weight: 500;
            transition: background-color 0.2s ease;
        }

        .copy-button:active {
            background-color: #e08000;
        }

        .note {
            font-size: 14px;
            color: #666;
            line-height: 1.5;
            margin-top: 10px;
            margin-bottom: 25px;
            text-align: left;
            padding: 0 5px;
        }

        .confirm-button {
            background-color: #ff9500;
            color: white;
            font-size: 16px;
            font-weight: 600;
            border: none;
            border-radius: 10px;
            padding: 14px;
            width: calc(100% - 40px);
            margin: auto 20px 20px 20px; /* Push to bottom */
            cursor: pointer;
            transition: background-color 0.2s ease;
            -webkit-appearance: none;
        }

        .confirm-button:active {
            background-color: #e08000;
        }

        /* Adjustments for better mobile experience */
        @media (max-width: 420px) {
            .bt-content-area {
                padding: 15px;
            }

            .info-box {
                padding: 15px;
            }

            .confirm-button {
                width: calc(100% - 30px);
                margin: auto 15px 15px 15px;
            }
        }

        /* --- General Overlay Styles --- */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.4); /* Semi-transparent background */
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
        }

        .overlay.active {
            opacity: 1;
            visibility: visible;
        }

        .modal-card { /* General style for cards within overlays */
            background: white;
            border-radius: 16px;
            padding: 40px 30px;
            width: 100%;
            max-width: 320px;
            text-align: center;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
        }

        /* --- Verifying Payment Loading Overlay Styles --- */
        .spinner {
            width: 60px;
            height: 60px;
            border: 4px solid #e0e0e0; /* Light grey border */
            border-top: 4px solid #007AFF; /* Blue top border for spinning effect */
            border-radius: 50%;
            animation: spin 1s linear infinite; /* Spin animation */
            margin: 0 auto 30px; /* Center and space below spinner */
        }

        .loading-title {
            font-size: 22px;
            font-weight: 600;
            color: #333;
            margin-bottom: 10px;
        }

        .loading-message {
            font-size: 15px;
            color: #666;
            line-height: 1.5;
            margin-bottom: 5px;
        }

        .loading-sub-message {
            font-size: 13px;
            color: #888;
        }

        /* --- Payment Not Successful Overlay Styles --- */
        .error-icon-circle {
            width: 80px; /* Larger icon size */
            height: 80px;
            background-color: #FF3B30; /* Red color for error icon */
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 auto 30px; /* Center and space below icon */
            position: relative; /* For positioning pseudo-elements */
        }

        .error-icon-circle::before,
        .error-icon-circle::after {
            content: '';
            position: absolute;
            width: 4px; /* Thickness of the 'X' lines */
            height: 50px; /* Length of the 'X' lines */
            background-color: #fff; /* White 'X' */
            border-radius: 2px; /* Slightly rounded tips */
        }

        .error-icon-circle::before {
            transform: rotate(45deg);
        }

        .error-icon-circle::after {
            transform: rotate(-45deg);
        }

        /* Style for the "Try Again" button (reusing contact-support-link styles) */
        .try-again-button {
            display: block; /* Make it block to take full width */
            width: 100%;
            padding: 15px;
            background-color: #007AFF; /* iOS blue button */
            color: white;
            font-size: 16px;
            font-weight: 600;
            border: none; /* No border for this style */
            border-radius: 10px;
            cursor: pointer;
            transition: background-color 0.2s ease;
            text-decoration: none; /* Remove underline for links */
            -webkit-appearance: none; /* Remove default button styles for iOS */
            box-sizing: border-box; /* Include padding in width */
        }

        .try-again-button:active,
        .try-again-button:hover { /* Added hover for desktop */
            background-color: #0056b3;
        }

        .error-title {
            font-size: 22px;
            font-weight: 600;
            color: #FF3B30; /* Red for the title */
            margin-bottom: 10px;
        }

        .error-message-detail {
            font-size: 15px;
            color: #333;
            line-height: 1.5;
            margin-bottom: 5px;
        }

        .error-suggestion {
            font-size: 14px;
            color: #666;
            margin-bottom: 30px;
        }

        /* Custom Message Box Styles */
        .custom-message-box {
            display: none; /* Hidden by default */
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }

        .custom-message-content {
            background: white;
            padding: 25px;
            border-radius: 12px;
            text-align: center;
            max-width: 300px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .custom-message-content h4 {
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 15px;
            color: #333;
        }

        .custom-message-content p {
            font-size: 1rem;
            color: #666;
            margin-bottom: 20px;
        }

        .custom-message-content button {
            background-color: #007AFF;
            color: white;
            padding: 10px 20px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: background-color 0.2s ease;
        }

        .custom-message-content button:hover {
            background-color: #0056b3;
        }

        .email-display-text {
            font-weight: 600;
            color: #007AFF; /* Highlight the email address */
            word-break: break-all; /* Break long emails */
        }
    </style>
</head>
<body>
    <!-- Header Section (Common to both initial form and bank transfer screen) -->
    <div class="header" id="mainHeader">
        <span class="back-arrow">&#8592;</span> <!-- Left arrow character -->
        <h1 class="text-xl font-semibold" id="headerTitle">Buy PIN Code</h1> <!-- Changed "Buy" to "Buy PIN Code" -->
    </div>

    <!-- Main Content Card (Payment Form) -->
    <div class="container" id="paymentForm">
        <h2 class="text-xl font-semibold text-gray-800 mb-6">Complete Payment</h2>

        <div class="mb-5">
            <label for="amount" class="label">Amount</label>
            <input type="text" id="amount" value="₦6,500" readonly class="bg-gray-50">
        </div>

        <div class="mb-5">
            <label for="fullName" class="label">Full Name</label>
            <input type="text" id="fullName" placeholder="Your full name">
        </div>

        <div class="mb-8">
            <label for="emailAddress" class="label">Your Email Address</label>
            <input type="email" id="emailAddress" placeholder="email address">
        </div>

        <button class="button" id="payButton">Pay</button>
    </div>

    <!-- Loading Screen (Initial Payment Processing) -->
    <div class="loading-screen" id="initialLoadingScreen">
        <div class="loading-card">
            <div class="loading-spinner"></div>
            <p class="loading-text">Preparing Payment Account</p>
            <p class="loading-subtext">Please wait while we set up your payment details...</p>
        </div>
    </div>

    <!-- Bank Transfer Screen -->
    <div class="bank-transfer-container" id="bankTransferScreen">
        <div class="bt-top-header">
            <div>Bank Transfer</div>
            <div class="cancel-button" id="bankTransferCancelBtn">Cancel</div>
        </div>

        <div class="bt-content-area">
            <div class="amount-display">
                <div class="main-amount">NGN 6,250</div>
                <div class="email">Amount to transfer</div>
            </div>
            <div class="instruction">Complete this bank transfer to proceed do not use Opay bank transfer</div>

            <div class="info-box">
                <div class="info-row">
                    <div class="info-label">Amount</div>
                    <div class="info-value-group">
                        <span class="info-value" id="amountToCopy">₦6,250</span>
                        <button class="copy-button" data-copy-target="amountToCopy">Copy</button>
                    </div>
                </div>

                <div class="info-row">
                    <div class="info-label">Account Number</div>
                    <div class="info-value-group">
                        <span class="info-value" id="accountNumberToCopy">1782299316</span>
                        <button class="copy-button" data-copy-target="accountNumberToCopy">Copy</button>
                    </div>
                </div>

                <div class="info-row">
                    <div class="info-label">Bank Name</div>
                    <div class="info-value-group">
                        <span class="info-value">Access Bank</span>
                    </div>
                </div>

                <div class="info-row">
                    <div class="info-label">Account Name</div>
                    <div class="info-value-group">
                        <span class="info-value">ONYEBUCHI JOSHUA</span>
                    </div>
                </div
 <div class="note">
                Kindly proceed with the payment for your PIN code don't buy from anyone.
            </div>
            <button class="confirm-button" id="iHaveMadeTransferBtn">I have made this bank Transfer</button>
        </div>
    </div>

    <!-- Verifying Payment Loading Overlay -->
    <div class="overlay" id="verifyingPaymentOverlay">
        <div class="modal-card">
            <div class="spinner"></div>
            <h3 class="loading-title">Verifying Payment</h3>
            <p class="loading-message">Please wait while we verify your payment...</p>
            <p class="loading-sub-message">This may take a few moments</p>
        </div>
    </div>

    <!-- Payment Not Successful Overlay -->
    <div class="overlay" id="paymentFailedOverlay">
        <div class="modal-card">
            <div class="error-icon-circle"></div>
            <h3 class="error-title">Payment Not Successful!</h3>
            <p class="error-message-detail">Your payment could not be completed</p>
            <p class="error-suggestion">Copy Account details and try again</p>
            <button class="try-again-button" id="tryAgainButton">Try Again</button>
        </div>
    </div>

    <!-- Custom Message Box -->
    <div class="custom-message-box" id="customMessageBox">
        <div class="custom-message-content">
            <h4 id="messageBoxTitle"></h4>
            <p id="messageBoxText"></p>
            <button id="messageBoxCloseBtn">OK</button>
        </div>
    </div>

    <script>
        // Get references to elements
        const payButton = document.getElementById('payButton');
        const paymentForm = document.getElementById('paymentForm');
        const fullNameInput = document.getElementById('fullName');
        const emailAddressInput = document.getElementById('emailAddress');
        const initialLoadingScreen = document.getElementById('initialLoadingScreen');
        const mainHeader = document.getElementById('mainHeader');
        const headerTitle = document.getElementById('headerTitle');

        const bankTransferScreen = document.getElementById('bankTransferScreen');
        const iHaveMadeTransferBtn = document.getElementById('iHaveMadeTransferBtn');
        const bankTransferCancelBtn = document.getElementById('bankTransferCancelBtn');
        const verifyingPaymentOverlay = document.getElementById('verifyingPaymentOverlay');
        const paymentFailedOverlay = document.getElementById('paymentFailedOverlay');
        const copyButtons = document.querySelectorAll('.copy-button');
        const tryAgainButton = document.getElementById('tryAgainButton');

        const customMessageBox = document.getElementById('customMessageBox');
        const messageBoxTitle = document.getElementById('messageBoxTitle');
        const messageBoxText = document.getElementById('messageBoxText');
        const messageBoxCloseBtn = document.getElementById('messageBoxCloseBtn');

        // Function to show custom message box
        function showCustomMessage(title, message, isHtml = false) {
            messageBoxTitle.textContent = title;
            if (isHtml) {
                messageBoxText.innerHTML = message;
            } else {
                messageBoxText.textContent = message;
            }
            customMessageBox.style.display = 'flex';
        }

        // Function to hide custom message box
        messageBoxCloseBtn.addEventListener('click', () => {
            customMessageBox.style.display = 'none';
        });

        // --- Functions to manage screen visibility ---

        function hideAllContentScreens() {
            paymentForm.style.display = 'none';
            initialLoadingScreen.style.display = 'none';
            bankTransferScreen.style.display = 'none';
            verifyingPaymentOverlay.classList.remove('active');
            paymentFailedOverlay.classList.remove('active');
            document.body.classList.remove('bank-transfer-bg'); // Remove specific background class
            document.body.style.alignItems = 'flex-start'; // Reset body alignment
            mainHeader.style.display = 'flex'; // Ensure header is visible by default
        }

        function showPaymentForm() {
            hideAllContentScreens();
            paymentForm.style.display = 'block';
            headerTitle.textContent = 'Buy PIN Code'; // Changed header title here
            document.body.style.backgroundColor = '#2563eb'; // Ensure correct background
            document.body.style.paddingTop = '2rem';
            // Clear inputs when returning to form
            fullNameInput.value = '';
            emailAddressInput.value = '';
        }

        function showInitialLoadingScreen() {
            hideAllContentScreens();
            initialLoadingScreen.style.display = 'flex';
            mainHeader.style.display = 'none'; // Hide header during initial loading
            document.body.style.backgroundColor = '#2563eb'; // Ensure correct background
            document.body.style.alignItems = 'center'; // Center loading screen vertically
            document.body.style.paddingTop = '0';
        }

        function showBankTransferScreen() {
            hideAllContentScreens();
            bankTransferScreen.style.display = 'flex'; // Use flex to make it a flex container
            mainHeader.style.display = 'none'; // Hide the original header, as bank transfer has its own
            document.body.classList.add('bank-transfer-bg'); // Apply specific background
            document.body.style.backgroundColor = '#f2f2f7'; // Set background for this screen
            document.body.style.alignItems = 'flex-start'; // Align to top for this screen
            document.body.style.paddingTop = '0'; // No padding top for this screen
        }

        function showVerifyingPaymentOverlay() {
            // This overlay is relative to the bankTransferScreen, so bankTransferScreen should be hidden
            bankTransferScreen.style.display = 'none';
            verifyingPaymentOverlay.classList.add('active');
            document.body.style.backgroundColor = '#f2f2f7'; // Keep bank transfer background
            document.body.style.alignItems = 'center'; // Center overlay vertically
            mainHeader.style.display = 'none'; // Ensure header is hidden
        }

        function showPaymentFailedOverlay() {
            // This overlay is relative to the bankTransferScreen, so bankTransferScreen should be hidden
            bankTransferScreen.style.display = 'none';
            paymentFailedOverlay.classList.add('active');
            verifyingPaymentOverlay.classList.remove('active'); // Hide verifying overlay if active
            document.body.style.backgroundColor = '#f2f2f7'; // Keep bank transfer background
            document.body.style.alignItems = 'center'; // Center overlay vertically
            mainHeader.style.display = 'none'; // Ensure header is hidden
        }

        // --- Initial state ---
        showPaymentForm(); // Start with the payment form visible

        // --- Event Listeners ---

        // Handle "Pay" button click on the initial form
        payButton.addEventListener('click', () => {
            const fullName = fullNameInput.value.trim();
            const emailAddress = emailAddressInput.value.trim();

            // Basic validation for Full Name
            if (fullName === '') {
                showCustomMessage('Input Error', 'Please enter your full name.');
                return; // Stop execution if validation fails
            }

            // Basic validation for Email Address
            // A simple regex for email validation
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (emailAddress === '') {
                showCustomMessage('Input Error', 'Please enter your email address.');
                return;
            }
            if (!emailRegex.test(emailAddress)) {
                showCustomMessage('Input Error', 'Please enter a valid email address.');
                return;
            }

            // If all validations pass, proceed to loading screen
            showInitialLoadingScreen();

            // Simulate initial payment processing delay (6 seconds as requested)
            setTimeout(() => {
                showBankTransferScreen(); // Transition to the bank transfer screen
            }, 6000);
        });

        // Handle "I have made this bank Transfer" button click
        iHaveMadeTransferBtn.addEventListener('click', function() {
            showVerifyingPaymentOverlay();

            // Simulate verification process for 5 seconds
            setTimeout(() => {
                showPaymentFailedOverlay(); // Transition to the failed screen after 5 seconds
            }, 5000);
        });

        // Handle "Cancel" button on bank transfer screen
        bankTransferCancelBtn.addEventListener('click', function() {
            showCustomMessage('Bank Transfer', 'Bank Transfer cancelled!');
            showBankTransferScreen(); // Stay on this screen after message
        });

        // Handle "Try Again" button click
        tryAgainButton.addEventListener('click', function() {
            showPaymentForm(); // Go back to the initial payment form
        });

        // --- Copy functionality ---
        copyButtons.forEach(button => {
            button.addEventListener('click', function() {
                const targetId = this.getAttribute('data-copy-target');
                const textElement = document.getElementById(targetId);

                if (textElement) {
                    let textToCopy = textElement.innerText;
                    let success = false;
                    try {
                        // Attempt modern Clipboard API first (might not work in all WebViews)
                        if (navigator.clipboard && navigator.clipboard.writeText) {
                            navigator.clipboard.writeText(textToCopy);
                            success = true;
                        } else {
                            // Fallback to document.execCommand (more compatible in some older WebViews/iframes)
                            const tempInput = document.createElement('textarea');
                            tempInput.value = textToCopy;
                            document.body.appendChild(tempInput);
                            tempInput.select();
                            document.execCommand('copy');
                            document.body.removeChild(tempInput);
                            success = true;
                        }
                    } catch (err) {
                        console.error('Failed to copy text:', err);
                        showCustomMessage('Copy Error', 'Failed to copy text automatically. Please copy manually: ' + textToCopy);
                        success = false;
                    }

                    if (success) {
                        const originalText = this.textContent;
                        this.textContent = 'Copied!';
                        setTimeout(() => {
                            this.textContent = originalText;
                        }, 1500);
                    }
                }
            });
        });
    </script>
</body>
</html>
