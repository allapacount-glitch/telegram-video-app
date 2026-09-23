<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Earn & Refer Mini App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body {
            background: linear-gradient(135deg, #090d16, #111827);
            color: #f8fafc;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 15px;
            overflow-x: hidden;
        }

        /* ফোর্স জয়েন মোডাল (শুরুতে চ্যানেল চেক করার জন্য) */
        #forceJoinModal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(9, 13, 22, 0.95); backdrop-filter: blur(15px);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 9999; padding: 25px; text-align: center;
        }

        .container { width: 100%; max-width: 380px; padding-bottom: 70px; display: none; }
        
        /* প্রিমিয়াম কার্ড ডিজাইন */
        .card {
            background: rgba(15, 23, 42, 0.92);
            backdrop-filter: blur(20px);
            padding: 25px 20px;
            border-radius: 24px;
            text-align: center;
            margin-bottom: 15px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
            border: 2px solid transparent;
            background-image: linear-gradient(rgba(15, 23, 42, 0.95), rgba(15, 23, 42, 0.95)), linear-gradient(90deg, #38bdf8, #ec4899, #8b5cf6, #38bdf8);
            background-origin: border-box;
            background-clip: padding-box, border-box;
            background-size: 400% 400%;
            animation: liveBorder 4s linear infinite;
        }

        @keyframes liveBorder {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 400% 50%; }
        }

        h2 { font-size: 22px; color: #ffffff; margin-bottom: 10px; }
        p { font-size: 13.5px; color: #94a3b8; line-height: 1.5; }

        /* বাটন ও ইন্টারঅ্যাকশন */
        .btn {
            display: block; width: 100%; background: linear-gradient(90deg, #38bdf8, #6366f1);
            color: white; border: none; padding: 14px; border-radius: 14px;
            font-size: 15px; font-weight: bold; cursor: pointer; margin-top: 15px;
            box-shadow: 0 5px 15px rgba(56, 189, 248, 0.3);
            transition: transform 0.3s ease;
            text-decoration: none;
        }
        .btn:active { transform: scale(0.97); }

        /* ট্যাব কন্টেন্ট */
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        /* নেভিগেশন বার */
        .nav-bar {
            position: fixed; bottom: 0; left: 0; width: 100%;
            background: rgba(30, 41, 59, 0.95); backdrop-filter: blur(10px);
            display: flex; justify-content: space-around; padding: 12px 0;
            border-top: 1px solid #334155; z-index: 1000;
        }
        .nav-item { color: #94a3b8; text-align: center; cursor: pointer; font-size: 13px; font-weight: 600; }
        .nav-item.active { color: #38bdf8; }
        .nav-item i { font-size: 18px; display: block; margin-bottom: 3px; }

        /* ইনপুট ফিল্ড */
        .input-group { margin-bottom: 12px; text-align: left; }
        .input-group label { font-size: 12.5px; color: #94a3b8; display: block; margin-bottom: 5px; }
        .input-group select, .input-group input {
            width: 100%; padding: 12px; border-radius: 10px;
            background: rgba(30, 41, 59, 0.8); border: 1px solid #475569;
            color: white; font-size: 14px; outline: none;
        }
    </style>
</head>
<body>

    <!-- ফোর্স জয়েন পপআপ (চ্যানেলে জয়েন বাধ্যতামূলক) -->
    <div id="forceJoinModal">
        <div class="card" style="max-width: 320px; width: 100%;">
            <i class="fa-brands fa-telegram" style="font-size: 45px; color: #38bdf8; margin-bottom: 15px;"></i>
            <h2>চ্যানেলে জয়েন করুন</h2>
            <p>বট ব্যবহার করতে হলে অবশ্যই আমাদের অফিশিয়াল টেলিগ্রাম চ্যানেলে জয়েন করতে হবে।</p>
            <a href="https://t.me/your_channel_link" class="btn" target="_blank" onclick="checkJoined()">চ্যানেলে জয়েন করুন</a>
            <button class="btn" style="background: #1e293b; border: 1px solid #38bdf8; margin-top: 8px;" onclick="verifyJoin()">জয়েন করেছি, চেক করুন</button>
        </div>
    </div>

    <!-- মূল মিনি অ্যাপ কনটেইনার -->
    <div class="container" id="mainAppContainer">
        
        <!-- হোম ট্যাব -->
        <div id="homeTab" class="tab-content active">
            <div class="card">
                <h3>আপনার ব্যালেন্স</h3>
                <h2 style="color: #38bdf8; font-size: 28px; margin-top: 5px;" id="userBalance">৳ 0.00</h2>
            </div>
            
            <div class="card">
                <h3>রেফারেল প্রোগ্রাম</h3>
                <p style="margin: 10px 0;">প্রতি রেফারে পাবেন **১০ টাকা**! বন্ধুদের সাথে আপনার রেফার লিংক শেয়ার করুন।</p>
                <button class="btn" onclick="triggerAd(); copyReferLink()">রেফার লিংক কপি করুন</button>
            </div>
        </div>

        <!-- প্রোফাইল ও উইথড্র ট্যাব -->
        <div id="profileTab" class="tab-content">
            <div class="card" style="text-align: left;">
                <h2>প্রোফাইল ও উইথড্র</h2>
                <p style="font-size: 12px; margin-bottom: 15px;">ইউজার আইডি: <span id="tgId">Loading...</span></p>
                
                <div class="input-group">
                    <label>পেমেন্ট মেথড</label>
                    <select id="payMethod">
                        <option value="Bkash">বিকাশ (Bkash)</option>
                        <option value="Nagad">নগদ (Nagad)</option>
                        <option value="Binance">বাইনান্স (Binance USDT)</option>
                    </select>
                </div>

                <div class="input-group">
                    <label>অ্যাকাউন্ট নম্বর / ওয়ালেট অ্যাড্রেস</label>
                    <input type="text" id="accountNo" placeholder="নম্বর বা অ্যাড্রেস দিন">
                </div>

                <div class="input-group">
                    <label>টাকার পরিমাণ</label>
                    <input type="number" id="withdrawAmt" placeholder="ন্যূনতম ৳১০০">
                </div>

                <button class="btn" onclick="triggerAd(); requestWithdraw()">উইথড্র রিকোয়েস্ট পাঠান</button>
            </div>
        </div>
    </div>

    <!-- বট নেভিগেশন বার -->
    <div class="nav-bar" id="bottomNav" style="display: none;">
        <div class="nav-item active" onclick="switchTab('home')">
            <i class="fa-solid fa-house"></i> হোম
        </div>
        <div class="nav-item" onclick="switchTab('profile')">
            <i class="fa-solid fa-user"></i> প্রোফাইল & উইথড্র
        </div>
    </div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.expand();
        document.getElementById('tgId').innerText = tg.initDataUnsafe?.user?.id || "987654321";

        // ফোর্স জয়েন চেক ফাংশন
        function verifyJoin() {
            // এখানে ব্যাকএন্ডে চেক করা হবে ইউজার চ্যানেলে জয়েন করেছে কি না
            // সাময়িকভাবে এটি পাস করে দিচ্ছি:
            document.getElementById('forceJoinModal').style.display = 'none';
            document.getElementById('mainAppContainer').style.display = 'block';
            document.getElementById('bottomNav').style.display = 'flex';
        }

        // ট্যাব সুইচ করার ফাংশন
        function switchTab(tab) {
            triggerAd(); // অপশনে ক্লিক করলেই অ্যাড পপআপ ট্রিগার হবে
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
            
            if(tab === 'home') {
                document.getElementById('homeTab').classList.add('active');
                event.currentTarget.classList.add('active');
            } else {
                document.getElementById('profileTab').classList.add('active');
                event.currentTarget.classList.add('active');
            }
        }

        // যেকোনো বাটনে ক্লিক করলেই অ্যাড শো করার সিস্টেম (এখানে আপনার অ্যাড নেটওয়ার্ক কোড বসাবেন)
        function triggerAd() {
            console.log("Ad Triggered: User clicked an option.");
            // উদাহরণস্বরূপ: Monetag বা Adsterra এর Popunder / Interstitial Ad কোড এখানে দিতে পারেন
        }

        function copyReferLink() {
            let uid = tg.initDataUnsafe?.user?.id || "12345";
            let link = `https://t.me/YourBotName?start=ref_${uid}`;
            navigator.clipboard.writeText(link);
            alert("রেফার লিংক কপি হয়েছে!");
        }

        function requestWithdraw() {
            let amt = document.getElementById('withdrawAmt').value;
            let acc = document.getElementById('accountNo').value;
            if(!amt || !acc) {
                alert("সব তথ্য সঠিকভাবে পূরণ করুন!");
                return;
            }
            alert("আপনার উইথড্র রিকোয়েস্ট সফলভাবে অ্যাডমিন প্যানেলে পাঠানো হয়েছে!");
        }
    </script>
</body>
</html>

