<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>طوفان چت ⚡</title>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;500;600;700;800&display=swap');
  * { margin:0; padding:0; box-sizing:border-box; font-family:'Vazirmatn',Tahoma,sans-serif; -webkit-tap-highlight-color:transparent; }
  body { background:#050812; min-height:100vh; color:white; overflow-x:hidden; }
  .storm-bg { position:fixed; inset:0; z-index:-1; background:radial-gradient(ellipse at top, #1a3a6e 0%, #0a1128 50%, #050812 100%); }
  .storm-bg::before { content:''; position:absolute; inset:0; background:radial-gradient(circle at 20% 30%, rgba(59,130,246,0.18) 0%, transparent 50%), radial-gradient(circle at 80% 70%, rgba(147,51,234,0.15) 0%, transparent 50%); animation:pulse 6s ease-in-out infinite; }
  @keyframes pulse { 0%,100%{opacity:0.6;} 50%{opacity:1;} }
  .lightning { position:fixed; inset:0; pointer-events:none; z-index:1; opacity:0; }
  .lightning.flash { animation:flash 0.5s; }
  @keyframes flash { 0%,100% { opacity:0; } 20%,40% { opacity:0.85; background:rgba(147,197,253,0.3); } 30% { opacity:0.2; } }
  .bolt-icon { position:fixed; top:18px; right:18px; font-size:30px; z-index:2; filter:drop-shadow(0 0 15px #60a5fa); animation:boltFloat 3s ease-in-out infinite; }
  @keyframes boltFloat { 0%,100% { transform:translateY(0) rotate(-5deg); } 50% { transform:translateY(-8px) rotate(5deg); } }
  .app { max-width:430px; margin:0 auto; min-height:100vh; position:relative; }
  .screen { padding:24px 20px; min-height:100vh; display:flex; flex-direction:column; justify-content:center; }
  .screen.hidden { display:none; }
  .logo { text-align:center; margin-bottom:28px; }
  .logo-icon { font-size:70px; line-height:1; filter:drop-shadow(0 0 25px #3b82f6); animation:boltFloat 2.5s ease-in-out infinite; }
  .logo-title { font-size:32px; font-weight:800; margin-top:10px; background:linear-gradient(135deg,#60a5fa,#a78bfa,#60a5fa); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; letter-spacing:1px; }
  .logo-sub { font-size:13px; color:#93c5fd; margin-top:6px; opacity:0.8; }
  .card { background:rgba(15,23,42,0.78); backdrop-filter:blur(20px); border:1px solid rgba(96,165,250,0.25); border-radius:24px; padding:26px 20px; box-shadow:0 20px 60px rgba(0,0,0,0.5), 0 0 40px rgba(59,130,246,0.12); }
  .card h2 { font-size:20px; margin-bottom:8px; color:#e0e7ff; text-align:center; }
  .card p { font-size:13px; color:#94a3b8; text-align:center; margin-bottom:20px; line-height:1.7; }
  .input-group { margin-bottom:14px; }
  .input-group label { display:block; font-size:12px; color:#93c5fd; margin-bottom:6px; font-weight:600; }
  .input { width:100%; padding:14px 16px; background:rgba(30,41,59,0.7); border:2px solid rgba(96,165,250,0.2); border-radius:14px; color:white; font-size:15px; outline:none; transition:all 0.2s; text-align:right; }
  .input:focus { border-color:#60a5fa; background:rgba(30,41,59,0.95); box-shadow:0 0 0 4px rgba(96,165,250,0.1); }
  .input::placeholder { color:#64748b; }
  .input.num { text-align:center; font-size:22px; letter-spacing:10px; font-weight:700; }
  .btn { width:100%; padding:15px; background:linear-gradient(135deg,#3b82f6,#6366f1); color:white; border:none; border-radius:16px; font-size:16px; font-weight:700; cursor:pointer; margin-top:8px; box-shadow:0 8px 0 #1e40af, 0 10px 30px rgba(59,130,246,0.35); transition:all 0.1s; display:flex; align-items:center; justify-content:center; gap:8px; text-decoration:none; }
  .btn:active { transform:translateY(4px); box-shadow:0 4px 0 #1e40af; }
  .btn:disabled { opacity:0.6; }
  .btn.ghost { background:transparent; box-shadow:none; border:2px solid rgba(96,165,250,0.3); color:#93c5fd; }
  .btn.ghost:active { background:rgba(96,165,250,0.1); transform:none; box-shadow:none; }
  .btn.danger { background:linear-gradient(135deg,#ef4444,#dc2626); box-shadow:0 8px 0 #991b1b; }
  .btn.warn { background:linear-gradient(135deg,#f59e0b,#d97706); box-shadow:0 8px 0 #92400e; }
  .demo-box { background:linear-gradient(135deg,rgba(251,191,36,0.15),rgba(245,158,11,0.15)); border:2px dashed #fbbf24; border-radius:14px; padding:14px; margin:16px 0; text-align:center; }
  .demo-box .label { font-size:12px; color:#fbbf24; margin-bottom:6px; }
  .demo-box .code { font-size:30px; font-weight:800; color:#fcd34d; letter-spacing:10px; text-shadow:0 0 20px rgba(251,191,36,0.5); }
  .app-header { display:flex; align-items:center; justify-content:space-between; padding:16px 18px; background:rgba(15,23,42,0.9); backdrop-filter:blur(20px); border-bottom:1px solid rgba(96,165,250,0.2); position:sticky; top:0; z-index:50; }
  .app-header .title { font-size:19px; font-weight:800; background:linear-gradient(135deg,#60a5fa,#a78bfa); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .app-header .back { width:42px; height:42px; border-radius:50%; background:rgba(96,165,250,0.15); display:flex; align-items:center; justify-content:center; font-size:20px; cursor:pointer; color:#93c5fd; }
  .tab-bar { position:fixed; bottom:0; left:0; right:0; max-width:430px; margin:0 auto; background:rgba(15,23,42,0.97); backdrop-filter:blur(20px); border-top:1px solid rgba(96,165,250,0.2); display:flex; padding:8px 0 12px; z-index:60; }
  .tab { flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; cursor:pointer; padding:8px 0; color:#64748b; transition:color 0.2s; }
  .tab.active { color:#60a5fa; }
  .tab .icon { font-size:22px; }
  .tab .lbl { font-size:11px; font-weight:600; }
  .users-list { padding:12px 16px 100px; }
  .user-item { display:flex; align-items:center; gap:12px; background:rgba(15,23,42,0.7); border:1px solid rgba(96,165,250,0.15); border-radius:18px; padding:14px; margin-bottom:10px; cursor:pointer; transition:all 0.2s; }
  .user-item:active { background:rgba(59,130,246,0.15); transform:scale(0.98); }
  .avatar { width:52px; height:52px; border-radius:50%; background:linear-gradient(135deg,#3b82f6,#8b5cf6); display:flex; align-items:center; justify-content:center; font-size:22px; font-weight:800; color:white; flex-shrink:0; box-shadow:0 0 20px rgba(59,130,246,0.4); }
  .avatar.sm { width:40px; height:40px; font-size:16px; }
  .user-info { flex:1; overflow:hidden; }
  .user-info .name { font-size:15px; font-weight:700; color:#e0e7ff; }
  .user-info .username { font-size:12px; color:#94a3b8; margin-top:2px; }
  .chat-area { padding:16px; padding-bottom:90px; display:flex; flex-direction:column; gap:10px; min-height:calc(100vh - 75px); }
  .msg { max-width:78%; padding:10px 14px; border-radius:18px; font-size:14px; line-height:1.5; word-break:break-word; animation:msgIn 0.25s ease; }
  @keyframes msgIn { from{opacity:0; transform:translateY(8px);} to{opacity:1; transform:translateY(0);} }
  .msg.me { align-self:flex-start; background:linear-gradient(135deg,#3b82f6,#6366f1); color:white; border-bottom-right-radius:4px; }
  .msg.other { align-self:flex-end; background:rgba(30,41,59,0.9); color:#e0e7ff; border:1px solid rgba(96,165,250,0.2); border-bottom-left-radius:4px; }
  .msg .time { font-size:10px; opacity:0.7; margin-top:4px; }
  .msg.me .time { text-align:left; }
  .msg.other .time { text-align:right; }
  .chat-input { position:fixed; bottom:0; left:0; right:0; max-width:430px; margin:0 auto; background:rgba(15,23,42,0.97); backdrop-filter:blur(20px); border-top:1px solid rgba(96,165,250,0.2); padding:10px 12px 16px; display:flex; gap:8px; z-index:60; }
  .chat-input input { flex:1; padding:12px 16px; background:rgba(30,41,59,0.9); border:2px solid rgba(96,165,250,0.2); border-radius:24px; color:white; font-size:14px; outline:none; }
  .chat-input input:focus { border-color:#60a5fa; }
  .chat-input button { width:46px; height:46px; border-radius:50%; background:linear-gradient(135deg,#3b82f6,#6366f1); color:white; border:none; cursor:pointer; font-size:20px; box-shadow:0 0 20px rgba(59,130,246,0.4); display:flex; align-items:center; justify-content:center; }
  .profile-wrap { padding:20px 16px 100px; }
  .profile-header { text-align:center; padding:24px 20px; background:rgba(15,23,42,0.7); border:1px solid rgba(96,165,250,0.2); border-radius:24px; margin-bottom:16px; }
  .profile-header .avatar { width:80px; height:80px; font-size:34px; margin:0 auto 12px; }
  .profile-header .name { font-size:20px; font-weight:800; color:#e0e7ff; }
  .profile-header .username { font-size:14px; color:#60a5fa; margin-top:4px; }
  .profile-header .phone { font-size:13px; color:#94a3b8; margin-top:8px; direction:ltr; }
  .rules-box { background:rgba(15,23,42,0.7); border:1px solid rgba(96,165,250,0.2); border-radius:20px; padding:20px; margin-bottom:16px; }
  .rules-box h3 { font-size:16px; color:#60a5fa; margin-bottom:12px; display:flex; align-items:center; gap:8px; }
  .rules-box ol { padding-right:20px; color:#cbd5e1; font-size:13px; line-height:2; }
  .rules-box li { margin-bottom:6px; }
  .loader { display:flex; align-items:center; justify-content:center; padding:40px 0; color:#60a5fa; font-size:14px; }
  .loader::after { content:''; display:inline-block; width:20px; height:20px; border:3px solid rgba(96,165,250,0.3); border-top-color:#60a5fa; border-radius:50%; margin-right:10px; animation:spin 0.8s linear infinite; }
  @keyframes spin { to{transform:rotate(360deg);} }
  .empty { text-align:center; padding:40px 20px; color:#64748b; font-size:14px; line-height:2; }
  .modal { position:fixed; inset:0; background:rgba(0,0,0,0.78); display:none; justify-content:center; align-items:center; z-index:200; padding:20px; }
  .modal.active { display:flex; }
  .modal-content { background:linear-gradient(135deg,#0f172a,#1e293b); border:1px solid rgba(96,165,250,0.3); border-radius:24px; padding:24px; width:100%; max-width:360px; text-align:center; box-shadow:0 20px 60px rgba(0,0,0,0.7); max-height:85vh; overflow-y:auto; }
  .modal-content h2 { color:#60a5fa; margin-bottom:12px; font-size:20px; }
  .modal-content p { color:#cbd5e1; font-size:14px; line-height:1.7; margin-bottom:16px; }

  /* پنل مدیریت */
  .admin-panel {
    position:fixed; inset:0; background:rgba(0,0,0,0.9);
    display:none; justify-content:center; align-items:flex-start;
    z-index:300; overflow-y:auto; padding:16px;
  }
  .admin-panel.active { display:flex; }
  .admin-inner {
    background:linear-gradient(135deg,#0f172a,#1e293b);
    border:1px solid rgba(96,165,250,0.3);
    border-radius:24px;
    padding:20px; width:100%; max-width:440px;
    margin-top:20px; margin-bottom:20px;
  }
  .admin-header {
    display:flex; justify-content:space-between; align-items:center;
    margin-bottom:16px; padding-bottom:14px;
    border-bottom:1px solid rgba(96,165,250,0.2);
  }
  .admin-header h2 { color:#60a5fa; font-size:18px; }
  .admin-close {
    width:36px; height:36px; border-radius:50%;
    background:rgba(239,68,68,0.2); color:#f87171;
    border:none; font-size:18px; cursor:pointer;
    display:flex; align-items:center; justify-content:center;
  }
  .stats-row {
    display:grid; grid-template-columns:1fr 1fr; gap:10px;
    margin-bottom:16px;
  }
  .stat-box {
    background:rgba(30,41,59,0.6);
    border:1px solid rgba(96,165,250,0.2);
    border-radius:14px; padding:14px; text-align:center;
  }
  .stat-box .num { font-size:24px; font-weight:800; color:#60a5fa; }
  .stat-box .lbl { font-size:11px; color:#94a3b8; margin-top:4px; }
  .admin-search {
    width:100%; padding:11px 14px;
    background:rgba(30,41,59,0.9);
    border:2px solid rgba(96,165,250,0.2);
    border-radius:12px; color:white; font-size:14px;
    outline:none; margin-bottom:14px;
  }
  .admin-search:focus { border-color:#60a5fa; }
  .admin-user-card {
    background:rgba(15,23,42,0.7);
    border:1px solid rgba(96,165,250,0.15);
    border-radius:16px;
    padding:12px; margin-bottom:10px;
  }
  .admin-user-row {
    display:flex; align-items:center; gap:12px;
    margin-bottom:8px;
  }
  .admin-user-info { flex:1; }
  .admin-user-info .n { font-size:14px; font-weight:700; color:#e0e7ff; }
  .admin-user-info .u { font-size:12px; color:#60a5fa; margin-top:2px; }
  .admin-user-info .p { font-size:12px; color:#94a3b8; direction:ltr; margin-top:2px; }
  .admin-user-info .d { font-size:10px; color:#64748b; margin-top:2px; }
  .admin-actions {
    display:flex; gap:8px; margin-top:8px;
  }
  .admin-actions button {
    flex:1; padding:8px;
    border:none; border-radius:10px;
    font-size:12px; font-weight:700;
    cursor:pointer;
  }
  .btn-chat-admin { background:linear-gradient(135deg,#3b82f6,#2563eb); color:white; }
  .btn-del-admin { background:linear-gradient(135deg,#ef4444,#dc2626); color:white; }
  .hidden { display:none !important; }
</style>
</head>
<body>

<div class="storm-bg"></div>
<div class="lightning" id="lightning"></div>
<div class="bolt-icon">⚡</div>

<div class="app">

<!-- صفحه ۱: شماره تلفن -->
<div class="screen" id="screen-phone">
  <div class="logo">
    <div class="logo-icon">🌩️</div>
    <div class="logo-title">طوفان چت</div>
    <div class="logo-sub">⚡ سریع، امن، رعدآسا ⚡</div>
  </div>
  <div class="card">
    <h2>ورود / ثبت‌نام</h2>
    <p>شماره موبایلت رو وارد کن</p>
    <div class="input-group">
      <label>شماره موبایل</label>
      <input type="tel" class="input" id="phone-input" placeholder="09xxxxxxxxx" maxlength="11" inputmode="numeric" style="direction:ltr;text-align:center;">
    </div>
    <button class="btn" onclick="sendCode()">📩 دریافت کد تأیید</button>
  </div>
</div>

<!-- صفحه ۲: کد تأیید -->
<div class="screen hidden" id="screen-code">
  <div class="logo">
    <div class="logo-icon">📩</div>
    <div class="logo-title">تأیید شماره</div>
  </div>
  <div class="card">
    <p>کد ۵ رقمی به شماره <b id="phone-show" style="color:#60a5fa;direction:ltr;display:inline-block;"></b></p>
    <div class="demo-box">
      <div class="label">🎁 حالت دمو - کد پیامک:</div>
      <div class="code" id="demo-code">-----</div>
    </div>
    <input type="tel" class="input num" id="code-input" placeholder="-----" maxlength="5" inputmode="numeric" style="direction:ltr;">
    <button class="btn" onclick="verifyCode()" style="margin-top:16px;">✅ تأیید کد</button>
    <button class="btn ghost" onclick="backToPhone()" style="margin-top:10px;">← ویرایش شماره</button>
  </div>
</div>

<!-- صفحه ۳: تکمیل پروفایل -->
<div class="screen hidden" id="screen-profile-setup">
  <div class="logo">
    <div class="logo-icon">👤</div>
    <div class="logo-title">تکمیل پروفایل</div>
  </div>
  <div class="card">
    <p>یه اسم و نام کاربری برای خودت انتخاب کن</p>
    <div class="input-group">
      <label>نام و نام خانوادگی</label>
      <input type="text" class="input" id="name-input" placeholder="مثلاً: محمد رسول‌زاده" maxlength="30">
    </div>
    <div class="input-group">
      <label>نام کاربری (انگلیسی، بدون فاصله)</label>
      <input type="text" class="input" id="username-input" placeholder="mmd_r" maxlength="20" style="direction:ltr;text-align:left;">
    </div>
    <button class="btn" onclick="completeProfile()">🚀 ورود به طوفان چت</button>
  </div>
</div>

<!-- اپ اصلی -->
<div id="main-app" class="hidden">
  <div class="app-header">
    <div style="width:42px;"></div>
    <div class="title">⚡ طوفان چت</div>
    <div style="width:42px;"></div>
  </div>

  <div id="tab-users-view">
    <div class="users-list" id="users-list">
      <div class="loader">در حال بارگذاری کاربران</div>
    </div>
  </div>

  <div id="tab-profile-view" class="hidden">
    <div class="profile-wrap">
      <div class="profile-header">
        <div class="avatar" id="my-avatar">؟</div>
        <div class="name" id="my-name">-</div>
        <div class="username" id="my-username">@-</div>
        <div class="phone" id="my-phone">-</div>
      </div>

      <div class="rules-box">
        <h3>📜 قوانین و مقررات</h3>
        <ol>
          <li>احترام به همه کاربران الزامی است.</li>
          <li>ارسال پیام‌های تبلیغاتی، توهین‌آمیز یا نامناسب ممنوع است.</li>
          <li>استفاده از نام کاربری جعلی یا توهین‌آمیز ممنوع است.</li>
          <li>هر کاربر فقط با یک شماره می‌تواند ثبت‌نام کند.</li>
          <li>مسئولیت محتوای پیام‌ها بر عهده فرستنده است.</li>
          <li>در صورت تخلف، حساب کاربر مسدود خواهد شد.</li>
          <li>استفاده از ربات یا اسپم ممنوع است.</li>
          <li>حریم خصوصی دیگران باید حفظ شود.</li>
          <li>پشتیبانی: @Mohamad55954 در تلگرام</li>
          <li>ادامه استفاده به معنای پذیرش قوانین است.</li>
        </ol>
      </div>

      <a href="https://t.me/Mohamad55954" target="_blank" class="btn" style="background:linear-gradient(135deg,#0088cc,#00a2e8);box-shadow:0 8px 0 #005f8f;">
        ✈️ پشتیبانی در تلگرام
      </a>

      <!-- دکمه پنل مدیریت (کوچیک) -->
      <button class="btn warn" onclick="openAdminLogin()" style="margin-top:12px;font-size:13px;padding:11px;">
        🔐 پنل مدیریت
      </button>

      <button class="btn danger" onclick="logout()" style="margin-top:12px;">
        🚪 خروج از حساب
      </button>
    </div>
  </div>

  <div class="tab-bar" id="main-tab-bar">
    <div class="tab active" id="tab-users" onclick="switchTab('users')">
      <div class="icon">👥</div>
      <div class="lbl">کاربران</div>
    </div>
    <div class="tab" id="tab-profile" onclick="switchTab('profile')">
      <div class="icon">👤</div>
      <div class="lbl">پروفایل</div>
    </div>
  </div>
</div>

<!-- صفحه چت -->
<div id="chat-screen" class="hidden">
  <div class="app-header">
    <div class="back" onclick="closeChat()">←</div>
    <div class="title" id="chat-title">چت</div>
    <div style="width:42px;"></div>
  </div>
  <div class="chat-area" id="chat-area">
    <div class="loader">در حال بارگذاری پیام‌ها</div>
  </div>
  <div class="chat-input">
    <input type="text" id="msg-input" placeholder="پیامت رو بنویس..." onkeypress="if(event.key==='Enter')sendMessage()">
    <button onclick="sendMessage()">➤</button>
  </div>
</div>

</div>

<!-- مودال عمومی -->
<div class="modal" id="msg-modal">
  <div class="modal-content">
    <h2 id="modal-title">پیام</h2>
    <p id="modal-text"></p>
    <button class="btn" onclick="closeModal()">باشه</button>
  </div>
</div>

<!-- مودال ورود مدیر -->
<div class="modal" id="admin-login-modal">
  <div class="modal-content">
    <h2>🔐 ورود مدیر</h2>
    <p>رمز مدیریت رو وارد کن</p>
    <input type="password" class="input" id="admin-pass-input" placeholder="رمز..." style="text-align:center;margin-bottom:14px;">
    <button class="btn" onclick="checkAdminPass()">ورود</button>
    <button class="btn ghost" onclick="closeModalById('admin-login-modal')" style="margin-top:10px;">انصراف</button>
  </div>
</div>

<!-- پنل مدیریت -->
<div class="admin-panel" id="admin-panel">
  <div class="admin-inner">
    <div class="admin-header">
      <h2>🔐 پنل مدیریت</h2>
      <button class="admin-close" onclick="closeAdminPanel()">✕</button>
    </div>

    <div class="stats-row">
      <div class="stat-box">
        <div class="num" id="total-users">0</div>
        <div class="lbl">👥 کل کاربران</div>
      </div>
      <div class="stat-box">
        <div class="num" id="total-messages">0</div>
        <div class="lbl">💬 کل پیام‌ها</div>
      </div>
    </div>

    <input type="text" class="admin-search" id="admin-search" placeholder="🔍 جستجوی نام، یوزرنیم یا شماره..." oninput="filterAdminUsers()">

    <div id="admin-users-list">
      <div class="loader">در حال بارگذاری...</div>
    </div>

    <button class="btn danger" onclick="closeAdminPanel()" style="margin-top:16px;">
      بستن پنل
    </button>
  </div>
</div>

<script>
// ============ Firebase ============
const firebaseConfig = {
  apiKey: "AIzaSyCRctzP-Z0TIGd27nOQRCXvEhQEd16UQ1M",
  authDomain: "mmd-hosh-ad601.firebaseapp.com",
  projectId: "mmd-hosh-ad601",
  storageBucket: "mmd-hosh-ad601.firebasestorage.app",
  messagingSenderId: "974087697030",
  appId: "1:974087697030:web:fde065030e6286d1559707"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// ============ متغیرها ============
let currentUser = null;
let currentChatUser = null;
let currentCode = '';
let tempPhone = '';
let unsubscribeMessages = null;
let unsubscribeUsers = null;
let allUsersCache = [];
let isAdmin = false;
const ADMIN_PASSWORD = 'Mohammad_1392';

// ============ رعد و برق ============
function triggerLightning() {
  const el = document.getElementById('lightning');
  el.classList.add('flash');
  setTimeout(() => el.classList.remove('flash'), 500);
  setTimeout(triggerLightning, 4000 + Math.random() * 7000);
}
setTimeout(triggerLightning, 1500);

// ============ توابع کمکی ============
function showModal(title, text) {
  document.getElementById('modal-title').textContent = title;
  document.getElementById('modal-text').textContent = text;
  document.getElementById('msg-modal').classList.add('active');
}
function closeModal() {
  document.getElementById('msg-modal').classList.remove('active');
}
function closeModalById(id) {
  document.getElementById(id).classList.remove('active');
}
function showScreen(id) {
  ['screen-phone','screen-code','screen-profile-setup'].forEach(s => {
    document.getElementById(s).classList.add('hidden');
  });
  if (id) document.getElementById(id).classList.remove('hidden');
}
function timeAgo(ts) {
  if (!ts) return '';
  const d = ts.toDate ? ts.toDate() : new Date(ts);
  const diff = (Date.now() - d.getTime()) / 1000;
  if (diff < 60) return 'الان';
  if (diff < 3600) return Math.floor(diff/60) + ' د';
  if (diff < 86400) return Math.floor(diff/3600) + ' س';
  return Math.floor(diff/86400) + ' روز';
}
function fullDate(ts) {
  if (!ts) return '-';
  const d = ts.toDate ? ts.toDate() : new Date(ts);
  return d.toLocaleDateString('fa-IR') + ' ' + d.toLocaleTimeString('fa-IR', {hour:'2-digit',minute:'2-digit'});
}
function getInitial(name) {
  return (name || '?').trim().charAt(0).toUpperCase();
}
function escapeHtml(s) {
  return String(s||'').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

// ============ مرحله ۱ ============
function sendCode() {
  const phone = document.getElementById('phone-input').value.trim();
  if (!/^09\d{9}$/.test(phone)) {
    showModal('❌ خطا', 'شماره موبایل معتبر نیست.\nباید با ۰۹ شروع شود و ۱۱ رقم باشد.');
    return;
  }
  tempPhone = phone;
  currentCode = String(Math.floor(10000 + Math.random() * 90000));
  document.getElementById('phone-show').textContent = phone;
  document.getElementById('demo-code').textContent = currentCode;
  document.getElementById('code-input').value = '';
  showScreen('screen-code');
  document.getElementById('lightning').classList.add('flash');
  setTimeout(() => document.getElementById('lightning').classList.remove('flash'), 500);
}
function backToPhone() { showScreen('screen-phone'); }

// ============ مرحله ۲ ============
async function verifyCode() {
  const code = document.getElementById('code-input').value.trim();
  if (code !== currentCode) { showModal('❌ خطا', 'کد وارد شده صحیح نیست.'); return; }
  try {
    const doc = await db.collection('users').doc(tempPhone).get();
    if (doc.exists) {
      currentUser = doc.data();
      currentUser.phone = tempPhone;
      localStorage.setItem('storm_user', tempPhone);
      enterApp();
    } else {
      showScreen('screen-profile-setup');
    }
  } catch(e) { showModal('❌ خطا', 'مشکل در اتصال: ' + e.message); }
}

// ============ مرحله ۳ ============
async function completeProfile() {
  const name = document.getElementById('name-input').value.trim();
  const username = document.getElementById('username-input').value.trim().toLowerCase().replace(/\s+/g,'_');
  if (name.length < 2) { showModal('❌ خطا', 'نام باید حداقل ۲ حرف باشد.'); return; }
  if (username.length < 3) { showModal('❌ خطا', 'نام کاربری باید حداقل ۳ حرف باشد.'); return; }
  if (!/^[a-z0-9_]+$/.test(username)) { showModal('❌ خطا', 'نام کاربری فقط حروف انگلیسی، عدد و _ باشد.'); return; }
  try {
    const check = await db.collection('users').where('username', '==', username).get();
    if (!check.empty) { showModal('❌ خطا', 'این نام کاربری قبلاً استفاده شده.'); return; }
    const userData = {
      phone: tempPhone, name: name, username: username,
      createdAt: firebase.firestore.FieldValue.serverTimestamp(),
      lastSeen: firebase.firestore.FieldValue.serverTimestamp()
    };
    await db.collection('users').doc(tempPhone).set(userData);
    currentUser = userData;
    localStorage.setItem('storm_user', tempPhone);
    enterApp();
  } catch(e) { showModal('❌ خطا', 'مشکل در ثبت‌نام: ' + e.message); }
}

// ============ ورود به اپ ============
function enterApp() {
  showScreen(null);
  document.getElementById('main-app').classList.remove('hidden');
  document.getElementById('my-avatar').textContent = getInitial(currentUser.name);
  document.getElementById('my-name').textContent = currentUser.name;
  document.getElementById('my-username').textContent = '@' + currentUser.username;
  document.getElementById('my-phone').textContent = currentUser.phone;
  loadUsers();
  db.collection('users').doc(currentUser.phone).update({
    lastSeen: firebase.firestore.FieldValue.serverTimestamp()
  }).catch(()=>{});
}

// ============ تب‌ها ============
function switchTab(tab) {
  const usersView = document.getElementById('tab-users-view');
  const profileView = document.getElementById('tab-profile-view');
  const tabUsers = document.getElementById('tab-users');
  const tabProfile = document.getElementById('tab-profile');
  if (tab === 'users') {
    usersView.classList.remove('hidden');
    profileView.classList.add('hidden');
    tabUsers.classList.add('active');
    tabProfile.classList.remove('active');
  } else {
    usersView.classList.add('hidden');
    profileView.classList.remove('hidden');
    tabUsers.classList.remove('active');
    tabProfile.classList.add('active');
  }
}

// ============ بارگذاری کاربران ============
function loadUsers() {
  if (unsubscribeUsers) unsubscribeUsers();
  unsubscribeUsers = db.collection('users').orderBy('createdAt', 'desc')
    .onSnapshot(snapshot => {
      allUsersCache = [];
      snapshot.forEach(doc => {
        const u = doc.data();
        u.phone = doc.id;
        allUsersCache.push(u);
      });
      renderUsersList();
    }, err => {
      document.getElementById('users-list').innerHTML = '<div class="empty">❌ خطا: ' + err.message + '</div>';
    });
}

function renderUsersList() {
  const list = document.getElementById('users-list');
  let html = '';
  let count = 0;
  allUsersCache.forEach(u => {
    if (u.phone === currentUser.phone) return;
    count++;
    html += `
      <div class="user-item" onclick="openChat('${u.phone}')">
        <div class="avatar">${getInitial(u.name)}</div>
        <div class="user-info">
          <div class="name">${escapeHtml(u.name)}</div>
          <div class="username">@${escapeHtml(u.username)}</div>
        </div>
        <div style="color:#60a5fa;font-size:20px;">💬</div>
      </div>
    `;
  });
  if (count === 0) {
    list.innerHTML = '<div class="empty">😴<br>هنوز کسی عضو نشده<br><br>لینک رو برای دوستات بفرست!</div>';
  } else {
    list.innerHTML = html;
  }
}

// ============ چت ============
function openChat(otherPhone) {
  const u = allUsersCache.find(x => x.phone === otherPhone);
  if (!u) return;
  currentChatUser = u;
  document.getElementById('chat-title').textContent = '💬 ' + u.name;
  document.getElementById('main-app').classList.add('hidden');
  document.getElementById('chat-screen').classList.remove('hidden');
  document.getElementById('chat-area').innerHTML = '<div class="loader">در حال بارگذاری پیام‌ها</div>';
  loadMessages();
}
function closeChat() {
  if (unsubscribeMessages) { unsubscribeMessages(); unsubscribeMessages = null; }
  currentChatUser = null;
  document.getElementById('chat-screen').classList.add('hidden');
  document.getElementById('main-app').classList.remove('hidden');
}
function getChatId(p1, p2) {
  const s = [p1, p2].sort();
  return s[0] + '_' + s[1];
}
function loadMessages() {
  if (unsubscribeMessages) unsubscribeMessages();
  const chatId = getChatId(currentUser.phone, currentChatUser.phone);
  const area = document.getElementById('chat-area');
  unsubscribeMessages = db.collection('chats').doc(chatId).collection('messages')
    .orderBy('createdAt', 'asc')
    .onSnapshot(snapshot => {
      if (snapshot.empty) {
        area.innerHTML = '<div class="empty">👋<br>اولین پیام رو تو بفرست!</div>';
        return;
      }
      let html = '';
      snapshot.forEach(doc => {
        const m = doc.data();
        const isMe = m.from === currentUser.phone;
        html += `
          <div class="msg ${isMe ? 'me' : 'other'}">
            <div>${escapeHtml(m.text)}</div>
            <div class="time">${m.createdAt ? timeAgo(m.createdAt) : ''}</div>
          </div>
        `;
      });
      area.innerHTML = html;
      setTimeout(() => window.scrollTo(0, document.body.scrollHeight), 50);
    }, err => {
      area.innerHTML = '<div class="empty">❌ خطا: ' + err.message + '</div>';
    });
}
async function sendMessage() {
  const input = document.getElementById('msg-input');
  const text = input.value.trim();
  if (!text) return;
  input.value = '';
  const chatId = getChatId(currentUser.phone, currentChatUser.phone);
  try {
    await db.collection('chats').doc(chatId).collection('messages').add({
      from: currentUser.phone,
      fromName: currentUser.name,
      to: currentChatUser.phone,
      text: text,
      createdAt: firebase.firestore.FieldValue.serverTimestamp()
    });
  } catch(e) { showModal('❌ خطا', 'پیام ارسال نشد: ' + e.message); }
}

// ============ خروج ============
function logout() {
  if (!confirm('از حساب خارج می‌شی؟')) return;
  localStorage.removeItem('storm_user');
  location.reload();
}

// ============ پنل مدیریت ============
function openAdminLogin() {
  document.getElementById('admin-pass-input').value = '';
  document.getElementById('admin-login-modal').classList.add('active');
}

function checkAdminPass() {
  const pass = document.getElementById('admin-pass-input').value;
  if (pass === ADMIN_PASSWORD) {
    isAdmin = true;
    closeModalById('admin-login-modal');
    openAdminPanel();
  } else {
    showModal('❌ خطا', 'رمز اشتباه است!');
  }
}

function openAdminPanel() {
  document.getElementById('admin-panel').classList.add('active');
  renderAdminUsers();
  loadStats();
}

function closeAdminPanel() {
  document.getElementById('admin-panel').classList.remove('active');
}

function renderAdminUsers(filter = '') {
  const container = document.getElementById('admin-users-list');
  let users = allUsersCache;

  if (filter.trim()) {
    const q = filter.toLowerCase().trim();
    users = users.filter(u =>
      (u.name||'').toLowerCase().includes(q) ||
      (u.username||'').toLowerCase().includes(q) ||
      (u.phone||'').includes(q)
    );
  }

  if (!users.length) {
    container.innerHTML = '<div class="empty">کاربری پیدا نشد</div>';
    return;
  }

  let html = '';
  users.forEach(u => {
    const isMe = u.phone === currentUser.phone;
    html += `
      <div class="admin-user-card">
        <div class="admin-user-row">
          <div class="avatar sm">${getInitial(u.name)}</div>
          <div class="admin-user-info">
            <div class="n">${escapeHtml(u.name)} ${isMe ? '(خودت)' : ''}</div>
            <div class="u">@${escapeHtml(u.username)}</div>
            <div class="p">📞 ${escapeHtml(u.phone)}</div>
            <div class="d">📅 عضویت: ${fullDate(u.createdAt)}</div>
            <div class="d">🕐 آخرین بازدید: ${u.lastSeen ? timeAgo(u.lastSeen) : 'نامشخص'}</div>
          </div>
        </div>
        <div class="admin-actions">
          ${!isMe ? `<button class="btn-chat-admin" onclick="adminOpenChat('${u.phone}')">💬 چت</button>` : ''}
          ${!isMe ? `<button class="btn-del-admin" onclick="adminDeleteUser('${u.phone}', '${escapeHtml(u.name)}')">🗑️ حذف</button>` : ''}
        </div>
      </div>
    `;
  });
  container.innerHTML = html;
}

function filterAdminUsers() {
  const q = document.getElementById('admin-search').value;
  renderAdminUsers(q);
}

async function loadStats() {
  try {
    const usersSnap = await db.collection('users').get();
    document.getElementById('total-users').textContent = usersSnap.size;

    // تعداد پیام‌ها - شمارش از کالکشن chats
    let msgCount = 0;
    const chatsSnap = await db.collection('chats').get();
    for (const chatDoc of chatsSnap.docs) {
      const msgsSnap = await chatDoc.ref.collection('messages').get();
      msgCount += msgsSnap.size;
    }
    document.getElementById('total-messages').textContent = msgCount;
  } catch(e) {
    document.getElementById('total-messages').textContent = '?';
  }
}

async function adminDeleteUser(phone, name) {
  if (!confirm(`مطمئنی می‌خوای "${name}" رو حذف کنی؟\nتمام پیام‌هاش هم پاک می‌شن!`)) return;

  try {
    // حذف کاربر
    await db.collection('users').doc(phone).delete();

    // حذف چت‌های مربوط به این کاربر
    const chatsSnap = await db.collection('chats').get();
    const batch = db.batch();
    chatsSnap.forEach(chatDoc => {
      if (chatDoc.id.includes(phone)) {
        batch.delete(chatDoc.ref);
      }
    });
    await batch.commit();

    showModal('✅ انجام شد', `کاربر "${name}" حذف شد.`);
    renderAdminUsers(document.getElementById('admin-search').value);
    loadStats();
  } catch(e) {
    showModal('❌ خطا', 'حذف نشد: ' + e.message);
  }
}

function adminOpenChat(phone) {
  closeAdminPanel();
  switchTab('users');
  openChat(phone);
}

// ============ راه‌اندازی ============
window.addEventListener('load', async () => {
  const saved = localStorage.getItem('storm_user');
  if (saved) {
    try {
      const doc = await db.collection('users').doc(saved).get();
      if (doc.exists) {
        currentUser = doc.data();
        currentUser.phone = saved;
        enterApp();
        return;
      }
    } catch(e) {}
    localStorage.removeItem('storm_user');
  }
  showScreen('screen-phone');
});

document.getElementById('phone-input').addEventListener('input', e => {
  e.target.value = e.target.value.replace(/\D/g,'');
});
document.getElementById('code-input').addEventListener('input', e => {
  e.target.value = e.target.value.replace(/\D/g,'');
});
</script>
</body>
</html>
