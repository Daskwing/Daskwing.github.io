<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>德语词汇变位练习系统</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:"Microsoft YaHei","PingFang SC",sans-serif;}
body{background:linear-gradient(135deg,#f5f7fc,#e9eef8 60%,#f5f7fc);color:#1e2433;min-height:100vh;}
.hidden{display:none!important;}
button{cursor:pointer;font-family:inherit;}
/* ========== 登录/注册页 ========== */
#loginPage{position:fixed;inset:0;overflow:hidden;}
.danmu-layer{position:absolute;inset:0;overflow:hidden;pointer-events:none;z-index:1;}
.danmu-item{position:absolute;left:100%;white-space:nowrap;opacity:.8;animation-name:danmuFly;animation-timing-function:linear;}
@keyframes danmuFly{from{transform:translateX(0);}to{transform:translateX(calc(-100vw - 100%));}}
.login-card{position:relative;z-index:2;width:390px;max-width:92vw;margin:8vh auto 0;background:rgba(255,255,255,.93);backdrop-filter:blur(10px);border:1px solid #dbe3f0;border-radius:16px;padding:36px 34px;box-shadow:0 20px 50px rgba(80,100,160,.18);}
.login-card h1{font-size:22px;text-align:center;letter-spacing:1px;color:#1e2433;}
.login-card .sub{text-align:center;color:#64748b;font-size:13px;margin:6px 0 20px;}
.login-card input{width:100%;padding:12px 14px;margin:8px 0;background:#f8fafc;border:1px solid #cbd5e1;border-radius:8px;color:#1e2433;font-size:15px;outline:none;}
.login-card input:focus{border-color:#3b5bff;background:#fff;}
.captcha-row{display:flex;gap:10px;align-items:center;}
.captcha-row input{flex:1;}
#captchaCanvas{border-radius:8px;border:1px solid #cbd5e1;cursor:pointer;flex-shrink:0;}
#loginMsg,#regMsg{color:#dc2626;font-size:13px;min-height:20px;margin-top:4px;text-align:center;}
#jsErr{display:none;position:fixed;top:0;left:0;right:0;z-index:99;background:#7f1d1d;color:#fecaca;font-size:13px;padding:8px 14px;text-align:center;}
.btn{display:inline-block;border:none;border-radius:9px;padding:11px 26px;font-size:15px;color:#fff;background:linear-gradient(90deg,#3b5bff,#6a5cff);transition:.2s;}
.btn:hover{filter:brightness(1.1);}
.btn-block{width:100%;margin-top:6px;}
.btn-ghost{background:#fff;border:1px solid #cbd5e1;color:#64748b;}
.btn-ghost:hover{border-color:#6a5cff;color:#1e293b;}
.btn-sm{padding:5px 12px;font-size:13px;border-radius:6px;}
.login-tip{text-align:center;color:#94a3b8;font-size:12px;margin-top:12px;}
.login-tip a{color:#2563eb;text-decoration:none;cursor:pointer;}
.login-tip a:hover{text-decoration:underline;}
code{background:#eef2f7;padding:1px 6px;border-radius:4px;font-size:11px;color:#2563eb;}
/* ========== 顶部导航 ========== */
#topnav{position:sticky;top:0;z-index:20;display:flex;align-items:center;gap:6px;background:rgba(255,255,255,.94);backdrop-filter:blur(8px);padding:0 22px;height:58px;border-bottom:1px solid #e2e8f0;flex-wrap:wrap;}
.logo{font-weight:700;font-size:17px;margin-right:22px;background:linear-gradient(90deg,#0d9488,#4f46e5);-webkit-background-clip:text;background-clip:text;color:transparent;white-space:nowrap;}
.nav-tab{padding:8px 15px;border-radius:8px;cursor:pointer;color:#475569;font-size:14px;white-space:nowrap;}
.nav-tab:hover{color:#1e293b;background:#eef2ff;}
.nav-tab.active{background:#3b5bff;color:#fff;}
.nav-right{margin-left:auto;display:flex;align-items:center;gap:12px;}
.user-badge{font-size:13px;color:#4f46e5;background:#eef2ff;padding:5px 12px;border-radius:14px;}
/* ========== 主区 ========== */
main{padding:22px;max-width:1100px;margin:0 auto;}
.card{background:#ffffff;border:1px solid #e6ebf4;border-radius:14px;padding:22px;margin-bottom:18px;box-shadow:0 2px 10px rgba(80,100,160,.06);}
.card h3{font-size:16px;margin-bottom:14px;color:#334155;}
.welcome{font-size:22px;font-weight:700;}
.welcome-sub{color:#64748b;font-size:13px;margin-top:6px;}
.stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:14px;margin-top:16px;}
.stat-box{background:#f8fafc;border:1px solid #e6ebf4;border-radius:12px;padding:16px 18px;}
.stat-num{font-size:26px;font-weight:700;background:linear-gradient(90deg,#0d9488,#4f46e5);-webkit-background-clip:text;background-clip:text;color:transparent;}
.stat-lab{color:#64748b;font-size:13px;margin-top:4px;}
.home-danmu-wrap{position:relative;height:110px;overflow:hidden;border-radius:12px;background:#f8fafc;border:1px dashed #cbd5e1;}
.quick-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;}
.quick-card{background:#f8fafc;border:1px solid #e6ebf4;border-radius:12px;padding:20px;cursor:pointer;transition:.2s;}
.quick-card:hover{transform:translateY(-3px);border-color:#3b5bff;box-shadow:0 6px 16px rgba(80,100,160,.12);}
.quick-card .qi{font-size:26px;}
.quick-card .qt{font-weight:600;margin:8px 0 4px;}
.quick-card .qd{font-size:12px;color:#64748b;}
/* ========== 词汇库 ========== */
.vocab-layout{display:grid;grid-template-columns:190px 1fr;gap:18px;}
@media(max-width:760px){.vocab-layout{grid-template-columns:1fr;}}
.cat-item{padding:10px 14px;border-radius:8px;cursor:pointer;color:#475569;font-size:14px;margin-bottom:4px;}
.cat-item.active,.cat-item:hover{background:#e0e7ff;color:#1e293b;}
.vocab-toolbar{display:flex;gap:10px;margin-bottom:12px;flex-wrap:wrap;}
.vocab-toolbar input,.vocab-toolbar select,#practiceSetup select,#wordFormModal select,#wordFormModal input{background:#f8fafc;color:#1e2433;border:1px solid #cbd5e1;padding:9px 12px;border-radius:8px;font-size:14px;outline:none;}
.vocab-toolbar input:focus,.vocab-toolbar select:focus{border-color:#3b5bff;}
.tbl{width:100%;border-collapse:collapse;}
.tbl th,.tbl td{padding:10px 12px;text-align:left;border-bottom:1px solid #eef2f7;font-size:14px;}
.tbl th{color:#475569;font-weight:600;background:#f1f5f9;}
.tbl tr.clickable{cursor:pointer;}
.tbl tr.clickable:hover td{background:#f1f5f9;}
.badge{display:inline-block;padding:2px 10px;border-radius:10px;font-size:12px;}
.badge-verb{background:#d1fae5;color:#047857;}
.badge-noun{background:#fce7f3;color:#be185d;}
.badge-adj{background:#fef3c7;color:#b45309;}
.empty-tip{text-align:center;color:#94a3b8;padding:36px 0;font-size:14px;}
/* ========== 练习 ========== */
.setup-grid{display:flex;flex-wrap:wrap;gap:20px;align-items:center;margin:14px 0 18px;}
.seg{display:inline-flex;background:#e8edf5;border-radius:10px;padding:4px;gap:4px;flex-wrap:wrap;}
.seg-item{padding:8px 18px;border-radius:8px;cursor:pointer;color:#64748b;font-size:14px;}
.seg-item.active{background:#3b5bff;color:#fff;}
.hint{font-size:12px;color:#94a3b8;margin-top:10px;line-height:1.7;}
.q-item{background:#f8fafc;border:1px solid #e6ebf4;border-radius:12px;padding:16px 18px;margin-bottom:12px;}
.q-head{display:flex;align-items:center;gap:10px;margin-bottom:8px;}
.q-no{font-size:13px;color:#4f46e5;font-weight:600;}
.q-prompt{font-size:15px;margin-bottom:12px;line-height:1.6;}
.q-input{background:#fff;border:1px solid #cbd5e1;color:#1e2433;padding:9px 12px;border-radius:8px;min-width:220px;font-size:15px;outline:none;}
.q-input:focus{border-color:#3b5bff;}
.opt{display:block;padding:10px 14px;border:1px solid #cbd5e1;border-radius:8px;margin:7px 0;cursor:pointer;font-size:15px;transition:.15s;background:#fff;}
.opt:hover{border-color:#3b5bff;background:#eef2ff;}
.opt input{margin-right:8px;}
.dict-tbl{border-collapse:collapse;}
.dict-tbl td{padding:5px 10px 5px 0;}
.person-lab{color:#4f46e5;font-size:14px;min-width:80px;display:inline-block;}
.score-banner{text-align:center;padding:26px 0 10px;}
.score-num{font-size:52px;font-weight:800;background:linear-gradient(90deg,#0d9488,#4f46e5);-webkit-background-clip:text;background-clip:text;color:transparent;}
.review-item{border-radius:10px;padding:12px 16px;margin-bottom:10px;font-size:14px;line-height:1.7;border:1px solid transparent;}
.review-ok{background:#f0fdf4;border-color:#bbf7d0;}
.review-bad{background:#fef2f2;border-color:#fecaca;}
.rv-ans-ok{color:#16a34a;}
.rv-ans-bad{color:#dc2626;}
.rv-correct{color:#0284c7;}
/* ========== 统计图表 ========== */
.chart{display:flex;align-items:flex-end;gap:14px;height:230px;padding-top:10px;}
.bar-col{flex:1;display:flex;flex-direction:column;align-items:center;height:100%;}
.bar-wrap{flex:1;display:flex;align-items:flex-end;width:100%;justify-content:center;}
.bar{width:58%;max-width:46px;background:linear-gradient(180deg,#2dd4bf,#3b82f6);border-radius:6px 6px 0 0;min-height:2px;transition:height .7s;}
.bar-val{font-size:12px;color:#4f46e5;margin-bottom:6px;}
.bar-lab{font-size:12px;color:#64748b;margin-top:8px;}
.hbar-row{display:flex;align-items:center;gap:12px;margin:13px 0;font-size:14px;}
.hbar-lab{width:90px;color:#475569;flex-shrink:0;}
.hbar-track{flex:1;height:16px;background:#e2e8f0;border-radius:8px;overflow:hidden;}
.hbar-fill{height:100%;background:linear-gradient(90deg,#818cf8,#06b6d4);border-radius:8px;transition:width .7s;}
.hbar-val{width:60px;text-align:right;color:#4f46e5;flex-shrink:0;}
.weak-tag{display:inline-block;background:#fef2f2;color:#dc2626;border:1px solid #fecaca;padding:5px 14px;border-radius:14px;font-size:13px;margin:4px 6px 4px 0;}
/* ========== 模态框 ========== */
#modalMask{position:fixed;inset:0;background:rgba(30,41,59,.45);z-index:50;display:flex;align-items:center;justify-content:center;padding:16px;}
.modal-card{width:min(620px,94vw);max-height:85vh;overflow:auto;background:#ffffff;border:1px solid #e2e8f0;border-radius:14px;padding:26px;position:relative;box-shadow:0 20px 50px rgba(80,100,160,.25);}
.modal-close{position:absolute;top:12px;right:16px;background:none;border:none;color:#94a3b8;font-size:22px;}
.modal-close:hover{color:#1e293b;}
.m-zh{color:#64748b;font-size:14px;margin:8px 0 16px;}
.m-ex{margin-top:16px;padding:12px 14px;background:#f8fafc;border-left:3px solid #3b5bff;border-radius:0 8px 8px 0;font-size:14px;line-height:1.7;color:#334155;}
.wf-label{font-size:13px;color:#64748b;}
footer{text-align:center;color:#94a3b8;font-size:12px;padding:24px 0 30px;}
</style>
</head>
<body>

<div id="jsErr"></div>

<!-- ================= 登录/注册页 ================= -->
<div id="loginPage">
  <div class="danmu-layer" id="loginDanmu"></div>
  <div class="login-card">
    <h1>德语词汇变位练习系统</h1>
    <p class="sub">Deutsch Konjugation Trainer · Web 演示版</p>
    <div id="loginForm">
      <input id="loginUser" placeholder="用户名" autocomplete="off">
      <input id="loginPwd" type="password" placeholder="密码">
      <div class="captcha-row">
        <input id="captchaInput" placeholder="验证码（点击图片刷新）" autocomplete="off" maxlength="4">
        <canvas id="captchaCanvas" width="110" height="42" title="点击刷新验证码"></canvas>
      </div>
      <div id="loginMsg"></div>
      <button class="btn btn-block" id="loginBtn">登 录</button>
      <p class="login-tip"><a id="toRegister">没有账号？注册新账号</a></p>
      <p class="login-tip">演示账号：admin / 123456（管理端）· student / 123456（学习者）</p>
    </div>
    <div id="registerForm" class="hidden">
      <input id="regUser" placeholder="用户名（3-16位字母/数字/下划线）" autocomplete="off" maxlength="16">
      <input id="regPwd" type="password" placeholder="密码（至少6位）">
      <input id="regPwd2" type="password" placeholder="确认密码">
      <input id="regEmail" placeholder="邮箱（选填）" autocomplete="off">
      <div id="regMsg"></div>
      <button class="btn btn-block" id="regBtn">注 册</button>
      <p class="login-tip"><a id="toLogin">已有账号？返回登录</a></p>
      <p class="login-tip">密码经 SHA-256 哈希加密存储，不明文保存</p>
    </div>
  </div>
</div>

<!-- ================= 主页面 ================= -->
<div id="appPage" class="hidden">
  <nav id="topnav">
    <div class="logo">🇩🇪 德语变位练习</div>
    <div class="nav-tab active" data-page="home">首页</div>
    <div class="nav-tab" data-page="vocab">词汇库</div>
    <div class="nav-tab" data-page="practice">练习中心</div>
    <div class="nav-tab" data-page="wrong">错题本</div>
    <div class="nav-tab" data-page="stats">学习统计</div>
    <div class="nav-tab hidden" id="adminTab" data-page="admin">⚙️ 管理端</div>
    <div class="nav-right">
      <span class="user-badge" id="userBadge"></span>
      <button class="btn-ghost btn-sm" id="logoutBtn">退出登录</button>
    </div>
  </nav>

  <main>
    <section id="page-home" class="page">
      <div class="card">
        <div class="welcome" id="welcomeText"></div>
        <div class="welcome-sub" id="dateText"></div>
        <div class="stat-grid">
          <div class="stat-box"><div class="stat-num" id="homeTodayCount">0</div><div class="stat-lab">今日练习次数</div></div>
          <div class="stat-box"><div class="stat-num" id="homeTodayAcc">--</div><div class="stat-lab">今日正确率</div></div>
          <div class="stat-box"><div class="stat-num" id="homeTotalQ">0</div><div class="stat-lab">累计答题数</div></div>
          <div class="stat-box"><div class="stat-num" id="homeWrongN">0</div><div class="stat-lab">待攻克错题</div></div>
        </div>
      </div>
      <div class="card">
        <h3>📚 词汇弹幕 · 边走边记</h3>
        <div class="home-danmu-wrap"><div class="danmu-layer" id="homeDanmu"></div></div>
      </div>
      <div class="card">
        <h3>🚀 快捷入口</h3>
        <div class="quick-grid">
          <div class="quick-card" data-go="practice"><div class="qi">✍️</div><div class="qt">开始练习</div><div class="qd">填空 / 选择 / 默写，随机出题，自动批改</div></div>
          <div class="quick-card" data-go="vocab"><div class="qi">📖</div><div class="qt">浏览词汇库</div><div class="qd">查看完整变位表与例句</div></div>
          <div class="quick-card" data-go="wrong"><div class="qi">🗂️</div><div class="qt">错题复盘</div><div class="qd">自动归档，针对性重练</div></div>
          <div class="quick-card" data-go="stats"><div class="qi">📊</div><div class="qt">学习统计</div><div class="qd">正确率趋势，可导出学习报告</div></div>
        </div>
      </div>
    </section>

    <section id="page-vocab" class="page hidden">
      <div class="vocab-layout">
        <div class="card" style="padding:14px;">
          <div class="cat-item active" data-cat="all">📁 全部词汇</div>
          <div class="cat-item" data-cat="verb">🟢 动词（变位）</div>
          <div class="cat-item" data-cat="noun">🌸 名词（变格）</div>
          <div class="cat-item" data-cat="adj">🟡 形容词（变级）</div>
        </div>
        <div class="card">
          <div class="vocab-toolbar">
            <input id="vocabSearch" placeholder="🔍 搜索词形或中文释义…" style="flex:1;min-width:180px;">
            <select id="vocabDiff">
              <option value="all">全部难度</option><option value="1">★☆☆ 基础</option>
              <option value="2">★★☆ 进阶</option><option value="3">★★★ 困难</option>
            </select>
          </div>
          <div id="vocabListWrap"></div>
        </div>
      </div>
    </section>

    <section id="page-practice" class="page hidden">
      <div class="card" id="practiceSetup">
        <h3>⚙️ 练习设置</h3>
        <div class="setup-grid">
          <div>
            <div style="font-size:13px;color:#64748b;margin-bottom:6px;">题型</div>
            <div class="seg" id="segQtype">
              <div class="seg-item active" data-v="fill">变位填空</div>
              <div class="seg-item" data-v="choice">变位选择</div>
              <div class="seg-item" data-v="dictation">动词默写</div>
            </div>
          </div>
          <div>
            <div style="font-size:13px;color:#64748b;margin-bottom:6px;">词性范围</div>
            <select id="selWtype">
              <option value="all">全部词性</option><option value="verb">仅动词</option>
              <option value="noun">仅名词</option><option value="adj">仅形容词</option>
            </select>
          </div>
          <div>
            <div style="font-size:13px;color:#64748b;margin-bottom:6px;">难度</div>
            <select id="selDiff">
              <option value="all">全部难度</option><option value="1">★☆☆</option>
              <option value="2">★★☆</option><option value="3">★★★</option>
            </select>
          </div>
          <div>
            <div style="font-size:13px;color:#64748b;margin-bottom:6px;">题量</div>
            <select id="selCount"><option value="5">5 题</option><option value="10" selected>10 题</option><option value="20">20 题</option></select>
          </div>
        </div>
        <button class="btn" onclick="startPractice()">开始练习 ▶</button>
        <p class="hint">提示：默写题型仅针对动词；答案忽略大小写，ä/ö/ü/ß 可用 ae/oe/ue/ss 代替。</p>
      </div>
      <div id="quizArea"></div>
      <div id="resultArea"></div>
    </section>

    <section id="page-wrong" class="page hidden">
      <div class="card">
        <h3>🗂️ 错题本 <span style="font-size:12px;color:#94a3b8;font-weight:400;">答错自动归档，重练答对自动标记已掌握</span></h3>
        <div class="vocab-toolbar">
          <select id="wrongTypeFilter">
            <option value="all">全部词性</option><option value="verb">动词</option>
            <option value="noun">名词</option><option value="adj">形容词</option>
          </select>
          <select id="wrongStateFilter">
            <option value="unmastered">仅未掌握</option><option value="all">含已掌握</option>
          </select>
          <button class="btn btn-sm" onclick="startRetest()">🔁 错题重练</button>
          <button class="btn-ghost btn-sm" onclick="clearMastered()">清空已掌握</button>
        </div>
        <div id="wrongListWrap"></div>
      </div>
    </section>

    <section id="page-stats" class="page hidden">
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;margin-bottom:14px;">
          <h3 style="margin:0;">📊 总体数据</h3>
          <div>
            <button class="btn btn-sm" onclick="exportReport()">📄 导出学习报告</button>
            <button class="btn-ghost btn-sm" onclick="exportCSV()">📊 导出 CSV 明细</button>
          </div>
        </div>
        <div class="stat-grid">
          <div class="stat-box"><div class="stat-num" id="stSessions">0</div><div class="stat-lab">累计练习次数</div></div>
          <div class="stat-box"><div class="stat-num" id="stTotalQ">0</div><div class="stat-lab">累计答题数</div></div>
          <div class="stat-box"><div class="stat-num" id="stAcc">--</div><div class="stat-lab">总正确率</div></div>
          <div class="stat-box"><div class="stat-num" id="stWrong">0</div><div class="stat-lab">当前错题数</div></div>
        </div>
        <p class="hint">「导出学习报告」会在新窗口打开排版好的报告页，按 Ctrl+P 选择「另存为 PDF」即可生成 PDF 文件。</p>
      </div>
      <div class="card">
        <h3>📈 近 7 天正确率趋势</h3>
        <div class="chart" id="weekChart"></div>
      </div>
      <div class="card">
        <h3>🧩 各题型正确率</h3>
        <div id="typeChart"></div>
      </div>
      <div class="card">
        <h3>⚠️ 薄弱知识点分布（按错误类型）</h3>
        <div id="weakList"></div>
      </div>
    </section>

    <section id="page-admin" class="page hidden">
      <div class="card">
        <div class="seg" id="segAdmin">
          <div class="seg-item active" data-v="overview">📊 系统概览</div>
          <div class="seg-item" data-v="vocab">📖 词汇管理</div>
          <div class="seg-item" data-v="users">👥 用户管理</div>
          <div class="seg-item" data-v="hot">🔥 高频错题</div>
        </div>
      </div>
      <div id="adminContent"></div>
    </section>

    <footer>基于论文《基于Python的德语词汇变位练习管理系统的设计与实现》· 纯 HTML/CSS/JS 单文件演示版 · 数据保存在浏览器本地</footer>
  </main>
</div>

<div id="modalMask" class="hidden">
  <div class="modal-card" id="wordFormModal">
    <button class="modal-close" onclick="closeModal()">×</button>
    <div id="modalBody"></div>
  </div>
</div>

<script>
/* ==================== 全局错误提示 ==================== */
window.onerror=function(msg,src,line){
  var d=document.getElementById("jsErr");
  if(d){d.textContent="⚠ 脚本错误："+msg+"（第 "+line+" 行附近）——请截图发给我";d.style.display="block";}
};

/* ==================== SHA-256（纯JS实现，离线可用） ==================== */
function sha256(ascii){
  function rr(v,a){return (v>>>a)|(v<<(32-a));}
  var maxWord=Math.pow(2,32),result="";
  var words=[],bitLen=ascii.length*8,i,j;
  var hash=sha256.h=sha256.h||[],k=sha256.k=sha256.k||[];
  var primeCounter=k.length,isComposite={};
  for(var candidate=2;primeCounter<64;candidate++){
    if(!isComposite[candidate]){
      for(i=0;i<313;i+=candidate)isComposite[i]=candidate;
      hash[primeCounter]=(Math.pow(candidate,.5)*maxWord)|0;
      k[primeCounter++]=(Math.pow(candidate,1/3)*maxWord)|0;
    }
  }
  ascii+=String.fromCharCode(128);
  while(ascii.length%64-56)ascii+=String.fromCharCode(0);
  for(i=0;i<ascii.length;i++){
    j=ascii.charCodeAt(i);
    if(j>>8)return "";
    words[i>>2]|=j<<((3-i)%4)*8;
  }
  words[words.length]=(bitLen/maxWord)|0;
  words[words.length]=bitLen;
  for(j=0;j<words.length;){
    var w=words.slice(j,j+=16),oldHash=hash;
    hash=hash.slice(0,8);
    for(i=0;i<64;i++){
      var w15=w[i-15],w2=w[i-2];
      var a=hash[0],e=hash[4];
      var t1=hash[7]+(rr(e,6)^rr(e,11)^rr(e,25))+((e&hash[5])^((~e)&hash[6]))+k[i]
        +(w[i]=(i<16)?w[i]:(w[i-16]+(rr(w15,7)^rr(w15,18)^(w15>>>3))+w[i-7]+(rr(w2,17)^rr(w2,19)^(w2>>>10)))|0);
      var t2=(rr(a,2)^rr(a,13)^rr(a,22))+((a&hash[1])^(a&hash[2])^(hash[1]&hash[2]));
      hash=[(t1+t2)|0].concat(hash);
      hash[4]=(hash[4]+t1)|0;
    }
    for(i=0;i<8;i++)hash[i]=(hash[i]+oldHash[i])|0;
  }
  for(i=0;i<8;i++){
    for(j=3;j+1;j--){
      var b=(hash[i]>>(j*8))&255;
      result+=(b<16?"0":"")+b.toString(16);
    }
  }
  return result;
}
function hashPwd(s){return sha256(unescape(encodeURIComponent(s)));}

/* ==================== 默认词库（50 词） ==================== */
const DEFAULT_VOCAB=[
{id:1,word:"machen",type:"verb",ctype:"规则变化",zh:"做，制作",diff:1,conj:{ich:"mache",du:"machst","er/sie/es":"macht",wir:"machen",ihr:"macht","sie/Sie":"machen"},example:"Ich mache die Hausaufgaben. 我做家庭作业。"},
{id:2,word:"lernen",type:"verb",ctype:"规则变化",zh:"学习",diff:1,conj:{ich:"lerne",du:"lernst","er/sie/es":"lernt",wir:"lernen",ihr:"lernt","sie/Sie":"lernen"},example:"Wir lernen Deutsch. 我们学习德语。"},
{id:3,word:"spielen",type:"verb",ctype:"规则变化",zh:"玩，打球",diff:1,conj:{ich:"spiele",du:"spielst","er/sie/es":"spielt",wir:"spielen",ihr:"spielt","sie/Sie":"spielen"},example:"Das Kind spielt im Garten. 孩子在花园里玩。"},
{id:4,word:"arbeiten",type:"verb",ctype:"规则变化（词干+额外e）",zh:"工作",diff:1,conj:{ich:"arbeite",du:"arbeitest","er/sie/es":"arbeitet",wir:"arbeiten",ihr:"arbeitet","sie/Sie":"arbeiten"},example:"Er arbeitet in Berlin. 他在柏林工作。"},
{id:5,word:"wohnen",type:"verb",ctype:"规则变化",zh:"居住",diff:1,conj:{ich:"wohne",du:"wohnst","er/sie/es":"wohnt",wir:"wohnen",ihr:"wohnt","sie/Sie":"wohnen"},example:"Ich wohne in Shanghai. 我住在上海。"},
{id:6,word:"kaufen",type:"verb",ctype:"规则变化",zh:"买",diff:1,conj:{ich:"kaufe",du:"kaufst","er/sie/es":"kauft",wir:"kaufen",ihr:"kauft","sie/Sie":"kaufen"},example:"Sie kauft ein Buch. 她买一本书。"},
{id:7,word:"fragen",type:"verb",ctype:"规则变化",zh:"问，提问",diff:1,conj:{ich:"frage",du:"fragst","er/sie/es":"fragt",wir:"fragen",ihr:"fragt","sie/Sie":"fragen"},example:"Der Lehrer fragt den Schüler. 老师问学生。"},
{id:8,word:"hören",type:"verb",ctype:"规则变化",zh:"听",diff:1,conj:{ich:"höre",du:"hörst","er/sie/es":"hört",wir:"hören",ihr:"hört","sie/Sie":"hören"},example:"Wir hören Musik. 我们听音乐。"},
{id:9,word:"gehen",type:"verb",ctype:"规则变化",zh:"走，去",diff:1,conj:{ich:"gehe",du:"gehst","er/sie/es":"geht",wir:"gehen",ihr:"geht","sie/Sie":"gehen"},example:"Ich gehe nach Hause. 我回家。"},
{id:10,word:"kommen",type:"verb",ctype:"规则变化",zh:"来",diff:1,conj:{ich:"komme",du:"kommst","er/sie/es":"kommt",wir:"kommen",ihr:"kommt","sie/Sie":"kommen"},example:"Er kommt aus China. 他来自中国。"},
{id:11,word:"fahren",type:"verb",ctype:"强变化（a→ä）",zh:"乘车，驾驶",diff:2,conj:{ich:"fahre",du:"fährst","er/sie/es":"fährt",wir:"fahren",ihr:"fahrt","sie/Sie":"fahren"},example:"Wir fahren nach Berlin. 我们乘车去柏林。"},
{id:12,word:"lesen",type:"verb",ctype:"强变化（e→ie）",zh:"读，阅读",diff:2,conj:{ich:"lese",du:"liest","er/sie/es":"liest",wir:"lesen",ihr:"lest","sie/Sie":"lesen"},example:"Sie liest ein Buch. 她在读书。"},
{id:13,word:"sehen",type:"verb",ctype:"强变化（e→ie）",zh:"看，看见",diff:2,conj:{ich:"sehe",du:"siehst","er/sie/es":"sieht",wir:"sehen",ihr:"seht","sie/Sie":"sehen"},example:"Ich sehe einen Film. 我看一部电影。"},
{id:14,word:"nehmen",type:"verb",ctype:"强变化（e→i）",zh:"拿，取，乘坐",diff:2,conj:{ich:"nehme",du:"nimmst","er/sie/es":"nimmt",wir:"nehmen",ihr:"nehmt","sie/Sie":"nehmen"},example:"Er nimmt den Bus. 他乘公交车。"},
{id:15,word:"sprechen",type:"verb",ctype:"强变化（e→i）",zh:"说话，讲",diff:2,conj:{ich:"spreche",du:"sprichst","er/sie/es":"spricht",wir:"sprechen",ihr:"sprecht","sie/Sie":"sprechen"},example:"Sie spricht Deutsch. 她说德语。"},
{id:16,word:"essen",type:"verb",ctype:"强变化（e→i）",zh:"吃",diff:2,conj:{ich:"esse",du:"isst","er/sie/es":"isst",wir:"essen",ihr:"esst","sie/Sie":"essen"},example:"Das Kind isst einen Apfel. 孩子吃一个苹果。"},
{id:17,word:"schlafen",type:"verb",ctype:"强变化（a→ä）",zh:"睡觉",diff:2,conj:{ich:"schlafe",du:"schläfst","er/sie/es":"schläft",wir:"schlafen",ihr:"schlaft","sie/Sie":"schlafen"},example:"Das Baby schläft. 宝宝在睡觉。"},
{id:18,word:"tragen",type:"verb",ctype:"强变化（a→ä）",zh:"穿，携带",diff:2,conj:{ich:"trage",du:"trägst","er/sie/es":"trägt",wir:"tragen",ihr:"tragt","sie/Sie":"tragen"},example:"Sie trägt ein Kleid. 她穿一条裙子。"},
{id:19,word:"sein",type:"verb",ctype:"特殊变化",zh:"是",diff:2,conj:{ich:"bin",du:"bist","er/sie/es":"ist",wir:"sind",ihr:"seid","sie/Sie":"sind"},example:"Ich bin Student. 我是学生。"},
{id:20,word:"haben",type:"verb",ctype:"特殊变化",zh:"有",diff:2,conj:{ich:"habe",du:"hast","er/sie/es":"hat",wir:"haben",ihr:"habt","sie/Sie":"haben"},example:"Wir haben Zeit. 我们有时间。"},
{id:21,word:"Mann",type:"noun",gender:"der",plural:"Männer",zh:"男人",diff:1,decl:{nom:"der Mann",akk:"den Mann",dat:"dem Mann",gen:"des Mannes"},example:"Der Mann arbeitet heute. 这个男人今天工作。"},
{id:22,word:"Frau",type:"noun",gender:"die",plural:"Frauen",zh:"女人，女士",diff:1,decl:{nom:"die Frau",akk:"die Frau",dat:"der Frau",gen:"der Frau"},example:"Die Frau trinkt Kaffee. 这位女士喝咖啡。"},
{id:23,word:"Kind",type:"noun",gender:"das",plural:"Kinder",zh:"孩子",diff:1,decl:{nom:"das Kind",akk:"das Kind",dat:"dem Kind",gen:"des Kindes"},example:"Das Kind spielt. 孩子在玩。"},
{id:24,word:"Tag",type:"noun",gender:"der",plural:"Tage",zh:"天，日子",diff:1,decl:{nom:"der Tag",akk:"den Tag",dat:"dem Tag",gen:"des Tages"},example:"Der Tag ist schön. 今天天气很好。"},
{id:25,word:"Zeit",type:"noun",gender:"die",plural:"Zeiten",zh:"时间",diff:1,decl:{nom:"die Zeit",akk:"die Zeit",dat:"der Zeit",gen:"der Zeit"},example:"Die Zeit vergeht schnell. 时间过得很快。"},
{id:26,word:"Haus",type:"noun",gender:"das",plural:"Häuser",zh:"房子",diff:1,decl:{nom:"das Haus",akk:"das Haus",dat:"dem Haus",gen:"des Hauses"},example:"Das Haus ist groß. 这房子很大。"},
{id:27,word:"Hund",type:"noun",gender:"der",plural:"Hunde",zh:"狗",diff:1,decl:{nom:"der Hund",akk:"den Hund",dat:"dem Hund",gen:"des Hundes"},example:"Der Hund schläft. 狗在睡觉。"},
{id:28,word:"Katze",type:"noun",gender:"die",plural:"Katzen",zh:"猫",diff:1,decl:{nom:"die Katze",akk:"die Katze",dat:"der Katze",gen:"der Katze"},example:"Die Katze trinkt Milch. 猫在喝牛奶。"},
{id:29,word:"Buch",type:"noun",gender:"das",plural:"Bücher",zh:"书",diff:1,decl:{nom:"das Buch",akk:"das Buch",dat:"dem Buch",gen:"des Buches"},example:"Das Buch ist interessant. 这本书很有趣。"},
{id:30,word:"Tisch",type:"noun",gender:"der",plural:"Tische",zh:"桌子",diff:1,decl:{nom:"der Tisch",akk:"den Tisch",dat:"dem Tisch",gen:"des Tisches"},example:"Der Tisch ist neu. 这张桌子是新的。"},
{id:31,word:"Schule",type:"noun",gender:"die",plural:"Schulen",zh:"学校",diff:1,decl:{nom:"die Schule",akk:"die Schule",dat:"der Schule",gen:"der Schule"},example:"Die Schule ist nah. 学校很近。"},
{id:32,word:"Wasser",type:"noun",gender:"das",plural:"Wasser",zh:"水",diff:2,decl:{nom:"das Wasser",akk:"das Wasser",dat:"dem Wasser",gen:"des Wassers"},example:"Das Wasser ist kalt. 水很凉。"},
{id:33,word:"Baum",type:"noun",gender:"der",plural:"Bäume",zh:"树",diff:1,decl:{nom:"der Baum",akk:"den Baum",dat:"dem Baum",gen:"des Baumes"},example:"Der Baum ist alt. 这棵树很老。"},
{id:34,word:"Blume",type:"noun",gender:"die",plural:"Blumen",zh:"花",diff:1,decl:{nom:"die Blume",akk:"die Blume",dat:"der Blume",gen:"der Blume"},example:"Die Blume ist schön. 这朵花很美。"},
{id:35,word:"Auto",type:"noun",gender:"das",plural:"Autos",zh:"汽车",diff:1,decl:{nom:"das Auto",akk:"das Auto",dat:"dem Auto",gen:"des Autos"},example:"Das Auto ist schnell. 这辆车很快。"},
{id:36,word:"Freund",type:"noun",gender:"der",plural:"Freunde",zh:"朋友（男）",diff:1,decl:{nom:"der Freund",akk:"den Freund",dat:"dem Freund",gen:"des Freundes"},example:"Der Freund hilft mir. 这位朋友帮助我。"},
{id:37,word:"Stadt",type:"noun",gender:"die",plural:"Städte",zh:"城市",diff:2,decl:{nom:"die Stadt",akk:"die Stadt",dat:"der Stadt",gen:"der Stadt"},example:"Die Stadt ist groß. 这座城市很大。"},
{id:38,word:"Fenster",type:"noun",gender:"das",plural:"Fenster",zh:"窗户",diff:2,decl:{nom:"das Fenster",akk:"das Fenster",dat:"dem Fenster",gen:"des Fensters"},example:"Das Fenster ist offen. 窗户开着。"},
{id:39,word:"Apfel",type:"noun",gender:"der",plural:"Äpfel",zh:"苹果",diff:2,decl:{nom:"der Apfel",akk:"den Apfel",dat:"dem Apfel",gen:"des Apfels"},example:"Der Apfel ist rot. 这个苹果是红的。"},
{id:40,word:"Arbeit",type:"noun",gender:"die",plural:"Arbeiten",zh:"工作",diff:2,decl:{nom:"die Arbeit",akk:"die Arbeit",dat:"der Arbeit",gen:"der Arbeit"},example:"Die Arbeit ist fertig. 工作完成了。"},
{id:41,word:"gut",type:"adj",ctype:"不规则变化",zh:"好的",diff:3,comp:"besser",sup:"am besten",example:"Der Apfel ist gut. 这个苹果很好。"},
{id:42,word:"schön",type:"adj",ctype:"规则变化",zh:"美丽的",diff:2,comp:"schöner",sup:"am schönsten",example:"Die Blume ist schön. 这朵花很美。"},
{id:43,word:"schnell",type:"adj",ctype:"规则变化",zh:"快的",diff:2,comp:"schneller",sup:"am schnellsten",example:"Das Auto ist schnell. 这辆车很快。"},
{id:44,word:"groß",type:"adj",ctype:"变音（o→ö）",zh:"大的",diff:3,comp:"größer",sup:"am größten",example:"Das Haus ist groß. 这房子很大。"},
{id:45,word:"klein",type:"adj",ctype:"规则变化",zh:"小的",diff:2,comp:"kleiner",sup:"am kleinsten",example:"Die Katze ist klein. 这只猫很小。"},
{id:46,word:"alt",type:"adj",ctype:"变音（a→ä）",zh:"老的，旧的",diff:3,comp:"älter",sup:"am ältesten",example:"Der Baum ist alt. 这棵树很老。"},
{id:47,word:"jung",type:"adj",ctype:"变音（u→ü）",zh:"年轻的",diff:3,comp:"jünger",sup:"am jüngsten",example:"Der Student ist jung. 这个大学生很年轻。"},
{id:48,word:"neu",type:"adj",ctype:"规则变化",zh:"新的",diff:2,comp:"neuer",sup:"am neuesten",example:"Der Tisch ist neu. 这张桌子很新。"},
{id:49,word:"lang",type:"adj",ctype:"变音（a→ä）",zh:"长的",diff:3,comp:"länger",sup:"am längsten",example:"Der Weg ist lang. 这条路很长。"},
{id:50,word:"hoch",type:"adj",ctype:"不规则变化",zh:"高的",diff:3,comp:"höher",sup:"am höchsten",example:"Der Berg ist hoch. 这座山很高。"}
];
const PERSONS=["ich","du","er/sie/es","wir","ihr","sie/Sie"];
const CASES=[["nom","第一格"],["akk","第四格"],["dat","第三格"],["gen","第二格"]];
const DANMU_COLORS=["#0369a1","#be185d","#b45309","#047857","#6d28d9","#b91c1c","#7c3aed"];
let currentUser=null,currentRole="learner",captchaCode="",curQType="fill",quiz=null,quizStart=0,curAdminSub="overview",editingId=null;

/* ==================== 工具函数 ==================== */
function $(id){return document.getElementById(id);}
function shuffle(a){for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}return a;}
function norm(s){return (s||"").trim().toLowerCase().replace(/ä/g,"ae").replace(/ö/g,"oe").replace(/ü/g,"ue").replace(/ß/g,"ss").replace(/\s+/g," ");}
function todayStr(offset){const d=new Date();if(offset)d.setDate(d.getDate()+offset);return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");}
function nowStr(){const d=new Date();return todayStr()+" "+String(d.getHours()).padStart(2,"0")+":"+String(d.getMinutes()).padStart(2,"0");}
function stars(d){return "★".repeat(d)+"☆".repeat(3-d);}
function typeName(t){return t==="verb"?"动词":t==="noun"?"名词":"形容词";}
function loadWrongs(u){try{const v=JSON.parse(localStorage.getItem("dv_wrong_"+(u||currentUser)));return Array.isArray(v)?v:[];}catch(e){return[];}}
function saveWrongs(l,u){localStorage.setItem("dv_wrong_"+(u||currentUser),JSON.stringify(l));}
function loadRecords(u){try{const v=JSON.parse(localStorage.getItem("dv_rec_"+(u||currentUser)));return Array.isArray(v)?v:[];}catch(e){return[];}}
function saveRecords(l,u){localStorage.setItem("dv_rec_"+(u||currentUser),JSON.stringify(l));}

/* ==================== 用户存储（自动迁移旧版数组格式） ==================== */
function ensureUsers(){
  let users=null;
  try{users=JSON.parse(localStorage.getItem("dv_users"));}catch(e){users=null;}
  if(Array.isArray(users)){
    const o={};
    users.forEach(x=>{
      if(x&&x.username)o[x.username]={hash:x.pwdHash||"",email:x.email||"",role:x.role||"learner",created:(x.regTime||todayStr()).slice(0,10)};
    });
    users=o;localStorage.setItem("dv_users",JSON.stringify(users));
  }
  if(!users||typeof users!=="object"){
    users={
      admin:{hash:hashPwd("123456"),email:"",role:"admin",created:"2025-01-01"},
      student:{hash:hashPwd("123456"),email:"",role:"learner",created:"2025-01-01"}
    };
    localStorage.setItem("dv_users",JSON.stringify(users));
  }
  return users;
}
function saveUsers(u){localStorage.setItem("dv_users",JSON.stringify(u));}

/* ==================== 词库存储 ==================== */
function getVocab(){
  let v=null;
  try{v=JSON.parse(localStorage.getItem("dv_vocab"));}catch(e){v=null;}
  if(!Array.isArray(v)||!v.length){v=JSON.parse(JSON.stringify(DEFAULT_VOCAB));saveVocab(v);}
  return v;
}
function saveVocab(v){localStorage.setItem("dv_vocab",JSON.stringify(v));}

/* ==================== 验证码 ==================== */
function drawCaptcha(){
  const chars="ABCDEFGHJKMNPQRSTUVWXYZ23456789";captchaCode="";
  const cv=$("captchaCanvas");if(!cv)return;
  const ctx=cv.getContext("2d");
  ctx.fillStyle="#eef2f7";ctx.fillRect(0,0,cv.width,cv.height);
  for(let i=0;i<4;i++){
    const c=chars[Math.floor(Math.random()*chars.length)];captchaCode+=c;
    ctx.save();ctx.font="bold "+(22+Math.random()*8)+"px Arial";
    ctx.fillStyle="hsl("+Math.floor(Math.random()*360)+",65%,38%)";
    ctx.translate(16+i*24,28+Math.random()*6-3);ctx.rotate(Math.random()*0.6-0.3);
    ctx.fillText(c,0,0);ctx.restore();
  }
  for(let i=0;i<4;i++){ctx.strokeStyle="hsla("+Math.floor(Math.random()*360)+",50%,55%,.5)";ctx.beginPath();ctx.moveTo(Math.random()*110,Math.random()*42);ctx.lineTo(Math.random()*110,Math.random()*42);ctx.stroke();}
  for(let i=0;i<22;i++){ctx.fillStyle="hsla("+Math.floor(Math.random()*360)+",50%,45%,.5)";ctx.fillRect(Math.random()*110,Math.random()*42,2,2);}
}

/* ==================== 弹幕 ==================== */
function spawnDanmu(layerId){
  const layer=$(layerId);if(!layer)return;
  const vc=getVocab();if(!vc.length)return;
  const w=vc[Math.floor(Math.random()*vc.length)];
  const el=document.createElement("div");
  el.className="danmu-item";
  el.textContent=(w.type==="noun"?w.gender+" "+w.word:w.word)+" · "+w.zh;
  el.style.top=(Math.random()*85)+"%";
  el.style.fontSize=(13+Math.random()*11)+"px";
  el.style.color=DANMU_COLORS[Math.floor(Math.random()*DANMU_COLORS.length)];
  const dur=9+Math.random()*9;
  el.style.animationDuration=dur+"s";
  layer.appendChild(el);
  setTimeout(()=>el.remove(),dur*1000);
}
function danmuBurst(layerId,n){for(let i=0;i<n;i++)setTimeout(()=>spawnDanmu(layerId),i*180);}
let loginDanmuTimer=setInterval(()=>spawnDanmu("loginDanmu"),650);
let homeDanmuTimer=null;

/* ==================== 登录 / 注册 / 退出 ==================== */
$("toRegister").onclick=()=>{$("loginForm").classList.add("hidden");$("registerForm").classList.remove("hidden");};
$("toLogin").onclick=()=>{$("registerForm").classList.add("hidden");$("loginForm").classList.remove("hidden");drawCaptcha();};

function doLogin(){
  const u=$("loginUser").value.trim(),p=$("loginPwd").value,c=$("captchaInput").value.trim();
  const msg=$("loginMsg");msg.style.color="";
  if(!u||!p){msg.textContent="请输入用户名和密码";return;}
  if(c.toUpperCase()!==captchaCode){msg.textContent="验证码错误，请重新输入";$("captchaInput").value="";drawCaptcha();return;}
  const users=ensureUsers(),rec=users[u];
  if(rec&&rec.hash===hashPwd(p)){
    currentUser=u;currentRole=rec.role||"learner";msg.textContent="";
    $("loginPage").classList.add("hidden");$("appPage").classList.remove("hidden");
    $("userBadge").textContent=(currentRole==="admin"?"👑 ":"👤 ")+u;
    $("adminTab").classList.toggle("hidden",currentRole!=="admin");
    clearInterval(loginDanmuTimer);
    danmuBurst("homeDanmu",8);
    homeDanmuTimer=setInterval(()=>spawnDanmu("homeDanmu"),750);
    showPage("home");
  }else{msg.textContent="用户名或密码错误";drawCaptcha();}
}
function doRegister(){
  const u=$("regUser").value.trim(),p1=$("regPwd").value,p2=$("regPwd2").value,em=$("regEmail").value.trim();
  const msg=$("regMsg");
  if(!/^[A-Za-z0-9_]{3,16}$/.test(u)){msg.textContent="用户名需为3-16位字母、数字或下划线";return;}
  if(p1.length<6){msg.textContent="密码至少6位";return;}
  if(p1!==p2){msg.textContent="两次输入的密码不一致";return;}
  if(em&&!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(em)){msg.textContent="邮箱格式不正确";return;}
  const users=ensureUsers();
  if(users[u]){msg.textContent="该用户名已被注册";return;}
  users[u]={hash:hashPwd(p1),email:em,role:"learner",created:todayStr()};
  saveUsers(users);
  $("regUser").value="";$("regPwd").value="";$("regPwd2").value="";$("regEmail").value="";msg.textContent="";
  $("registerForm").classList.add("hidden");$("loginForm").classList.remove("hidden");
  $("loginUser").value=u;$("loginPwd").value="";$("captchaInput").value="";drawCaptcha();
  const lm=$("loginMsg");lm.style.color="#16a34a";lm.textContent="🎉 注册成功！请登录";
}
$("loginBtn").onclick=doLogin;
$("regBtn").onclick=doRegister;
$("captchaCanvas").onclick=drawCaptcha;
["loginUser","loginPwd","captchaInput"].forEach(id=>$(id).addEventListener("keydown",e=>{if(e.key==="Enter")doLogin();}));
["regUser","regPwd","regPwd2","regEmail"].forEach(id=>$(id).addEventListener("keydown",e=>{if(e.key==="Enter")doRegister();}));
$("logoutBtn").onclick=()=>{
  currentUser=null;currentRole="learner";clearInterval(homeDanmuTimer);
  $("adminTab").classList.add("hidden");
  $("appPage").classList.add("hidden");$("loginPage").classList.remove("hidden");
  $("loginPwd").value="";$("captchaInput").value="";drawCaptcha();
  danmuBurst("loginDanmu",8);
  loginDanmuTimer=setInterval(()=>spawnDanmu("loginDanmu"),650);
};

/* ==================== 页面切换 ==================== */
function showPage(name){
  if(name==="admin"&&currentRole!=="admin")return;
  document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
  $("page-"+name).classList.remove("hidden");
  document.querySelectorAll(".nav-tab").forEach(t=>t.classList.toggle("active",t.dataset.page===name));
  if(name==="home")renderHome();
  if(name==="vocab")renderVocabList();
  if(name==="wrong")renderWrong();
  if(name==="stats")renderStats();
  if(name==="admin")renderAdmin();
  window.scrollTo(0,0);
}
document.querySelectorAll(".nav-tab").forEach(t=>t.onclick=()=>showPage(t.dataset.page));
document.querySelectorAll(".quick-card").forEach(c=>c.onclick=()=>showPage(c.dataset.go));

/* ==================== 首页 ==================== */
function renderHome(){
  $("welcomeText").textContent="你好，"+currentUser+(currentRole==="admin"?"（管理员）":"")+" 👋";
  $("dateText").textContent="今天是 "+todayStr()+"，开始今天的德语变位练习吧！";
  const recs=loadRecords(),today=recs.filter(r=>r.date===todayStr());
  const tQ=today.reduce((s,r)=>s+r.total,0),tC=today.reduce((s,r)=>s+r.correct,0);
  $("homeTodayCount").textContent=today.length;
  $("homeTodayAcc").textContent=tQ?Math.round(tC/tQ*100)+"%":"--";
  $("homeTotalQ").textContent=recs.reduce((s,r)=>s+r.total,0);
  $("homeWrongN").textContent=loadWrongs().filter(e=>!e.mastered).length;
}

/* ==================== 词汇库 ==================== */
let vocabCat="all";
document.querySelectorAll(".cat-item").forEach(c=>c.onclick=()=>{
  document.querySelectorAll(".cat-item").forEach(x=>x.classList.remove("active"));
  c.classList.add("active");vocabCat=c.dataset.cat;renderVocabList();
});
$("vocabSearch").oninput=renderVocabList;
$("vocabDiff").onchange=renderVocabList;
function renderVocabList(){
  const kw=$("vocabSearch").value.trim().toLowerCase(),df=$("vocabDiff").value;
  const list=getVocab().filter(w=>(vocabCat==="all"||w.type===vocabCat)&&(df==="all"||w.diff==df)&&(!kw||w.word.toLowerCase().includes(kw)||w.zh.includes(kw)));
  if(!list.length){$("vocabListWrap").innerHTML='<div class="empty-tip">没有符合条件的词汇</div>';return;}
  let h='<table class="tbl"><tr><th>词形</th><th>词性</th><th>变化类型</th><th>中文释义</th><th>难度</th></tr>';
  list.forEach(w=>{
    h+='<tr class="clickable" onclick="showDetail('+w.id+')"><td style="font-weight:600;color:#1e293b;">'+(w.type==="noun"?w.gender+" ":"")+w.word+'</td>'
      +'<td><span class="badge badge-'+w.type+'">'+typeName(w.type)+'</span></td>'
      +'<td style="color:#64748b;">'+(w.ctype||"—")+'</td><td>'+w.zh+'</td><td style="color:#d97706;">'+stars(w.diff)+'</td></tr>';
  });
  $("vocabListWrap").innerHTML=h+"</table>";
}

/* ==================== 词汇详情模态框 ==================== */
function showDetail(id){
  const w=getVocab().find(v=>v.id===id);if(!w)return;
  let h='<h2 style="font-size:24px;">'+(w.type==="noun"?w.gender+" ":"")+w.word+' <span class="badge badge-'+w.type+'">'+typeName(w.type)+'</span></h2>';
  h+='<p class="m-zh">'+w.zh+' · '+(w.ctype||"")+' · 难度 <span style="color:#d97706;">'+stars(w.diff)+'</span></p>';
  if(w.type==="verb"){
    h+='<table class="tbl"><tr><th>人称</th><th>现在时变位</th></tr>';
    PERSONS.forEach(p=>{h+="<tr><td>"+p+"</td><td style='color:#0284c7;font-weight:600;'>"+(w.conj?w.conj[p]:"—")+"</td></tr>";});
    h+="</table>";
  }else if(w.type==="noun"){
    h+='<table class="tbl"><tr><th>格</th><th>单数形式（含定冠词）</th></tr>';
    CASES.forEach(c=>{h+="<tr><td>"+c[1]+"</td><td style='color:#db2777;font-weight:600;'>"+(w.decl?w.decl[c[0]]:"—")+"</td></tr>";});
    h+="<tr><td>复数</td><td style='color:#db2777;font-weight:600;'>die "+(w.plural||"—")+"</td></tr></table>";
  }else{
    h+='<table class="tbl"><tr><th>级别</th><th>形式</th></tr>'
      +"<tr><td>原级</td><td style='color:#b45309;font-weight:600;'>"+w.word+"</td></tr>"
      +"<tr><td>比较级</td><td style='color:#b45309;font-weight:600;'>"+(w.comp||"—")+"</td></tr>"
      +"<tr><td>最高级</td><td style='color:#b45309;font-weight:600;'>"+(w.sup||"—")+"</td></tr></table>";
  }
  if(w.example)h+='<p class="m-ex">💬 例句：'+w.example+"</p>";
  $("modalBody").innerHTML=h;$("modalMask").classList.remove("hidden");
}
function closeModal(){$("modalMask").classList.add("hidden");}
$("modalMask").onclick=e=>{if(e.target===$("modalMask"))closeModal();};

/* ==================== 题目生成 ==================== */
document.querySelectorAll("#segQtype .seg-item").forEach(s=>s.onclick=()=>{
  document.querySelectorAll("#segQtype .seg-item").forEach(x=>x.classList.remove("active"));
  s.classList.add("active");curQType=s.dataset.v;
});
function buildChoice(q){
  const opts=[q.answer],seen=new Set([norm(q.answer)]),vc=getVocab();
  const tryPush=v=>{if(opts.length<4&&v&&!seen.has(norm(v))){seen.add(norm(v));opts.push(v);}};
  if(q.word.type==="verb"&&q.word.conj){
    shuffle(PERSONS.filter(p=>p!==q.sub)).forEach(p=>tryPush(q.word.conj[p]));
    shuffle(vc.filter(v=>v.type==="verb"&&v.id!==q.word.id&&v.conj)).forEach(v=>tryPush(v.conj[q.sub]));
  }else if(q.word.type==="noun"&&q.word.decl){
    shuffle(Object.keys(q.word.decl)).forEach(k=>tryPush(q.word.decl[k]));
    shuffle(vc.filter(v=>v.type==="noun"&&v.id!==q.word.id&&v.decl)).forEach(v=>tryPush(v.decl[q.caseKey]));
  }else if(q.word.type==="adj"){
    if(q.adjForm==="comp"){tryPush("mehr "+q.word.word);tryPush(q.word.sup);}
    else{tryPush(q.word.comp);tryPush("am meisten "+q.word.word);}
    shuffle(vc.filter(v=>v.type==="adj"&&v.id!==q.word.id)).forEach(v=>tryPush(q.adjForm==="comp"?v.comp:v.sup));
  }
  return shuffle(opts);
}
function makeQuestion(w,kind){
  if(w.type==="verb"){
    const sub=PERSONS[Math.floor(Math.random()*PERSONS.length)];
    const q={kind,word:w,sub,prompt:"主语【"+sub+"】，请写出动词「"+w.word+"」（"+w.zh+"）的现在时变位",answer:w.conj[sub]};
    if(kind==="choice")q.options=buildChoice(q);
    return q;
  }else if(w.type==="noun"){
    const c=CASES[Math.floor(Math.random()*CASES.length)];
    const q={kind,word:w,caseKey:c[0],prompt:"请写出「"+w.gender+" "+w.word+"」（"+w.zh+"）的"+c[1]+"单数形式（含定冠词）",answer:w.decl[c[0]]};
    if(kind==="choice")q.options=buildChoice(q);
    return q;
  }else{
    const f=Math.random()<0.5?"comp":"sup";
    const q={kind,word:w,adjForm:f,prompt:f==="comp"?"请写出形容词「"+w.word+"」（"+w.zh+"）的比较级":"请写出形容词「"+w.word+"」（"+w.zh+"）的最高级",answer:f==="comp"?w.comp:w.sup};
    if(kind==="choice")q.options=buildChoice(q);
    return q;
  }
}

/* ==================== 练习流程 ==================== */
function startPractice(){
  const wtype=$("selWtype").value,df=$("selDiff").value,count=parseInt($("selCount").value);
  let pool=getVocab().filter(w=>(wtype==="all"||w.type===wtype)&&(df==="all"||w.diff==df));
  if(curQType==="dictation")pool=pool.filter(w=>w.type==="verb"&&w.conj);
  else pool=pool.filter(w=>(w.type==="verb"&&w.conj)||(w.type==="noun"&&w.decl)||(w.type==="adj"&&w.comp));
  if(!pool.length){alert("没有符合条件的词汇，请调整筛选条件");return;}
  shuffle(pool);
  const qs=[];
  if(curQType==="dictation"){
    pool.slice(0,Math.min(count,pool.length)).forEach(w=>qs.push({
      kind:"dict",word:w,
      prompt:"写出动词「"+w.word+"」（"+w.zh+"）的六人称变位",
      answer:PERSONS.map(p=>p+" "+w.conj[p]).join("；")
    }));
  }else{
    for(let i=0;i<count;i++)qs.push(makeQuestion(pool[i%pool.length],curQType));
  }
  quiz={questions:qs,mode:"normal",ptype:curQType};
  quizStart=Date.now();
  renderQuiz();
}
function startRetest(){
  const tf=$("wrongTypeFilter").value;
  let list=loadWrongs().filter(e=>!e.mastered&&(tf==="all"||e.wtype===tf));
  if(!list.length){alert("当前没有可重练的错题");return;}
  list=shuffle(list).slice(0,20);
  const vc=getVocab();
  const qs=list.map(e=>{
    const w=vc.find(v=>v.id===e.wordId);
    return {kind:"fill",word:w||{id:e.wordId,word:e.word,type:e.wtype,zh:""},prompt:e.prompt,answer:e.correctAnswer,retestKey:e.key};
  });
  quiz={questions:qs,mode:"retest",ptype:"retest"};
  quizStart=Date.now();
  showPage("practice");
  renderQuiz();
}
function renderQuiz(){
  $("practiceSetup").classList.add("hidden");$("resultArea").innerHTML="";
  let h='<div class="card"><h3>'+(quiz.mode==="retest"?"🔁 错题重练":"✍️ 练习进行中")+'（共 '+quiz.questions.length+' 题）</h3>';
  quiz.questions.forEach((q,i)=>{
    h+='<div class="q-item"><div class="q-head"><span class="q-no">第 '+(i+1)+' 题</span><span class="badge badge-'+q.word.type+'">'+typeName(q.word.type)+'</span></div>';
    if(q.kind==="dict"){
      h+='<div class="q-prompt">'+q.prompt+'</div><table class="dict-tbl">';
      PERSONS.forEach(p=>{h+='<tr><td><span class="person-lab">'+p+'</span></td><td><input class="q-input" style="min-width:180px;" data-qi="'+i+'" data-person="'+p+'" autocomplete="off"></td></tr>';});
      h+="</table>";
    }else{
      h+='<div class="q-prompt">'+q.prompt+"</div>";
      if(q.kind==="choice"){
        q.options.forEach(o=>{h+='<label class="opt"><input type="radio" name="q'+i+'" value="'+String(o).replace(/"/g,"&quot;")+'">'+o+"</label>";});
      }else{
        h+='<input class="q-input" data-qi="'+i+'" placeholder="请输入答案…" autocomplete="off">';
      }
    }
    h+="</div>";
  });
  h+='<div style="text-align:center;margin-top:18px;"><button class="btn" onclick="gradeQuiz()">提交批改 ✔</button>&nbsp;&nbsp;<button class="btn-ghost btn" onclick="cancelQuiz()">放弃本次练习</button></div></div>';
  $("quizArea").innerHTML=h;
  window.scrollTo(0,0);
}
function cancelQuiz(){quiz=null;$("quizArea").innerHTML="";$("practiceSetup").classList.remove("hidden");}
function classifyError(q,u){
  if(!u||!u.trim())return "未作答";
  if(q.word.type==="verb"&&q.sub&&q.word.conj){
    if(PERSONS.filter(p=>p!==q.sub).some(p=>norm(q.word.conj[p])===norm(u)))return "人称混淆";
    return "词形错误";
  }
  if(q.word.type==="noun"&&q.caseKey&&q.word.decl){
    if(Object.keys(q.word.decl).some(k=>k!==q.caseKey&&norm(q.word.decl[k])===norm(u)))return "格混淆";
    return "词形错误";
  }
  if(q.word.type==="adj"){
    const other=q.adjForm==="comp"?q.word.sup:q.word.comp;
    if(norm(other)===norm(u))return "级别混淆";
    return "词形错误";
  }
  return "词形错误";
}
function addWrong(q,userAns){
  const list=loadWrongs();
  const key=q.word.id+"_"+(q.sub||q.caseKey||q.adjForm||"dict");
  const ex=list.find(e=>e.key===key&&!e.mastered);
  if(ex){ex.wrongCount++;ex.userAnswer=userAns||"(空)";ex.time=nowStr();}
  else list.push({key,wordId:q.word.id,word:q.word.word,wtype:q.word.type,prompt:q.prompt||"",userAnswer:userAns||"(空)",correctAnswer:q.answer,errorType:classifyError(q,userAns),wrongCount:1,mastered:false,time:nowStr()});
  saveWrongs(list);
}
function gradeQuiz(){
  let correct=0;const reviews=[],wrongs=loadWrongs();
  quiz.questions.forEach((q,i)=>{
    if(q.kind==="dict"){
      let allOk=true,parts=[];
      PERSONS.forEach(p=>{
        const inp=document.querySelector('input[data-qi="'+i+'"][data-person="'+p+'"]');
        const uv=inp?inp.value:"";const ok=norm(uv)===norm(q.word.conj[p]);
        if(!ok)allOk=false;
        parts.push(p+" <b style='color:"+(ok?"#16a34a":"#dc2626")+";'>"+q.word.conj[p]+(ok?" ✓":" ✗（你填："+(uv||"空")+"）")+"</b>");
      });
      if(allOk)correct++;
      else addWrong(q,parts.filter(p=>p.indexOf("✗")>=0).map(p=>p.replace(/<[^>]+>/g,"")).join("；"));
      reviews.push({ok:allOk,html:q.prompt+"<br>"+parts.join(" · ")});
    }else{
      let uv="";
      if(q.kind==="choice"){const r=document.querySelector('input[name="q'+i+'"]:checked');uv=r?r.value:"";}
      else{const inp=document.querySelector('input[data-qi="'+i+'"]');uv=inp?inp.value:"";}
      const ok=norm(uv)===norm(q.answer);
      if(ok){
        correct++;
        if(q.retestKey){const e=wrongs.find(x=>x.key===q.retestKey);if(e){e.mastered=true;e.time=nowStr();}}
      }else{
        if(q.retestKey){
          const e=wrongs.find(x=>x.key===q.retestKey);
          if(e){e.wrongCount++;e.userAnswer=uv||"(空)";e.time=nowStr();}
        }else addWrong(q,uv);
      }
      reviews.push({ok,html:q.prompt+"<br>你的答案：<span class='"+(ok?"rv-ans-ok":"rv-ans-bad")+"'>"+(uv||"（未作答）")+"</span>"+(ok?"":"　正确答案：<span class='rv-correct'>"+q.answer+"</span>")});
    }
  });
  if(quiz.mode==="retest")saveWrongs(wrongs);
  const total=quiz.questions.length,score=Math.round(correct/total*100),dur=Math.round((Date.now()-quizStart)/1000);
  const recs=loadRecords();
  recs.push({date:todayStr(),time:nowStr(),ptype:quiz.ptype,total,correct,duration:dur});
  saveRecords(recs);
  const ptypeCN={fill:"变位填空",choice:"变位选择",dictation:"动词默写",retest:"错题重练"}[quiz.ptype];
  let h='<div class="card"><div class="score-banner"><div style="color:#64748b;font-size:14px;">'+ptypeCN+' · 本次得分</div>'
    +'<div class="score-num">'+score+'</div>'
    +'<div style="color:#475569;">共 '+total+' 题，答对 <b style="color:#16a34a;">'+correct+'</b> 题，答错 <b style="color:#dc2626;">'+(total-correct)+'</b> 题，用时 '+dur+' 秒</div></div><hr style="border:none;border-top:1px solid #e2e8f0;margin:16px 0;">';
  reviews.forEach((r,i)=>{h+='<div class="review-item '+(r.ok?"review-ok":"review-bad")+'"><b>第 '+(i+1)+' 题 '+(r.ok?"✓":"✗")+'</b><br>'+r.html+"</div>";});
  h+='<div style="text-align:center;margin-top:16px;"><button class="btn" onclick="cancelQuiz()">返回练习设置</button>&nbsp;&nbsp;<button class="btn-ghost btn" onclick="showPage(\'wrong\')">查看错题本</button></div></div>';
  $("quizArea").innerHTML="";$("resultArea").innerHTML=h;
  window.scrollTo(0,0);
}

/* ==================== 错题本 ==================== */
$("wrongTypeFilter").onchange=renderWrong;
$("wrongStateFilter").onchange=renderWrong;
function renderWrong(){
  const tf=$("wrongTypeFilter").value,sf=$("wrongStateFilter").value;
  const all=loadWrongs();
  const list=all.filter(e=>(tf==="all"||e.wtype===tf)&&(sf==="all"||!e.mastered)).sort((a,b)=>b.wrongCount-a.wrongCount);
  if(!list.length){$("wrongListWrap").innerHTML='<div class="empty-tip">🎉 暂无错题，继续保持！</div>';return;}
  let h='<div style="overflow-x:auto;"><table class="tbl"><tr><th>词汇</th><th>题干</th><th>我的答案</th><th>正确答案</th><th>错误类型</th><th>次数</th><th>状态</th><th>操作</th></tr>';
  list.forEach(e=>{
    const idx=all.indexOf(e);
    h+='<tr><td style="font-weight:600;">'+(e.word||"")+'</td><td style="color:#64748b;font-size:13px;max-width:220px;">'+(e.prompt||"—")+'</td>'
      +'<td class="rv-ans-bad">'+(e.userAnswer||"—")+'</td><td class="rv-correct">'+(e.correctAnswer||"—")+'</td>'
      +'<td><span class="weak-tag">'+(e.errorType||"词形错误")+'</span></td><td style="color:#d97706;">'+e.wrongCount+'</td>'
      +'<td>'+(e.mastered?'<span style="color:#16a34a;">已掌握</span>':'<span style="color:#dc2626;">未掌握</span>')+'</td>'
      +'<td style="white-space:nowrap;">'+(e.mastered?"":'<button class="btn-ghost btn-sm" onclick="markMastered('+idx+')">标为已掌握</button> ')
      +'<button class="btn-ghost btn-sm" style="border-color:#fca5a5;color:#dc2626;" onclick="removeWrong('+idx+')">移除</button></td></tr>';
  });
  $("wrongListWrap").innerHTML=h+"</table></div>";
}
function markMastered(i){const l=loadWrongs();if(l[i]){l[i].mastered=true;saveWrongs(l);}renderWrong();}
function removeWrong(i){const l=loadWrongs();l.splice(i,1);saveWrongs(l);renderWrong();}
function clearMastered(){saveWrongs(loadWrongs().filter(e=>!e.mastered));renderWrong();}

/* ==================== 学习统计 ==================== */
function renderStats(){
  const recs=loadRecords(),wrongs=loadWrongs();
  const tQ=recs.reduce((s,r)=>s+r.total,0),tC=recs.reduce((s,r)=>s+r.correct,0);
  $("stSessions").textContent=recs.length;
  $("stTotalQ").textContent=tQ;
  $("stAcc").textContent=tQ?Math.round(tC/tQ*100)+"%":"--";
  $("stWrong").textContent=wrongs.filter(e=>!e.mastered).length;
  let ch="";
  for(let i=6;i>=0;i--){
    const d=todayStr(-i),dr=recs.filter(r=>r.date===d);
    const dq=dr.reduce((s,r)=>s+r.total,0),dc=dr.reduce((s,r)=>s+r.correct,0);
    const acc=dq?Math.round(dc/dq*100):null;
    ch+='<div class="bar-col"><div class="bar-val">'+(acc===null?"—":acc+"%")+'</div><div class="bar-wrap"><div class="bar" style="height:'+(acc===null?2:Math.max(acc,2))+'%"></div></div><div class="bar-lab">'+d.slice(5)+'</div></div>';
  }
  $("weekChart").innerHTML=ch;
  const types=[["fill","变位填空"],["choice","变位选择"],["dictation","动词默写"],["retest","错题重练"]];
  let th="";
  types.forEach(t=>{
    const tr=recs.filter(r=>r.ptype===t[0]);
    const q=tr.reduce((s,r)=>s+r.total,0),c=tr.reduce((s,r)=>s+r.correct,0);
    const acc=q?Math.round(c/q*100):null;
    th+='<div class="hbar-row"><div class="hbar-lab">'+t[1]+'</div><div class="hbar-track"><div class="hbar-fill" style="width:'+(acc||0)+'%"></div></div><div class="hbar-val">'+(acc===null?"无数据":acc+"%")+"</div></div>";
  });
  $("typeChart").innerHTML=th;
  const cnt={};
  wrongs.filter(e=>!e.mastered).forEach(e=>{const k=e.errorType||"词形错误";cnt[k]=(cnt[k]||0)+1;});
  const keys=Object.keys(cnt).sort((a,b)=>cnt[b]-cnt[a]);
  $("weakList").innerHTML=keys.length?keys.map(k=>'<span class="weak-tag">'+k+" × "+cnt[k]+"</span>").join(""):'<div class="empty-tip" style="padding:14px;">暂无数据，先去练习吧！</div>';
}

/* ==================== 导出学习报告 / CSV ==================== */
function exportReport(){
  const recs=loadRecords(),unm=loadWrongs().filter(e=>!e.mastered);
  const tQ=recs.reduce((s,r)=>s+r.total,0),tC=recs.reduce((s,r)=>s+r.correct,0);
  const acc=tQ?Math.round(tC/tQ*100):null;
  let rows7="";
  for(let i=6;i>=0;i--){
    const d=todayStr(-i),dr=recs.filter(r=>r.date===d);
    const q=dr.reduce((s,r)=>s+r.total,0),c=dr.reduce((s,r)=>s+r.correct,0);
    rows7+="<tr><td>"+d+"</td><td>"+dr.length+"</td><td>"+q+"</td><td>"+(q?Math.round(c/q*100)+"%":"—")+"</td></tr>";
  }
  const types=[["fill","变位填空"],["choice","变位选择"],["dictation","动词默写"],["retest","错题重练"]];
  let rowsT="";
  types.forEach(t=>{
    const tr=recs.filter(r=>r.ptype===t[0]);
    const q=tr.reduce((s,r)=>s+r.total,0),c=tr.reduce((s,r)=>s+r.correct,0);
    rowsT+="<tr><td>"+t[1]+"</td><td>"+tr.length+"</td><td>"+q+"</td><td>"+(q?Math.round(c/q*100)+"%":"—")+"</td></tr>";
  });
  const top=unm.slice().sort((a,b)=>b.wrongCount-a.wrongCount).slice(0,10);
  const rowsW=top.length?top.map((e,i)=>"<tr><td>"+(i+1)+"</td><td>"+(e.word||"")+"</td><td>"+(e.prompt||"")+"</td><td>"+(e.userAnswer||"")+"</td><td>"+(e.correctAnswer||"")+"</td><td>"+(e.errorType||"")+"</td><td>"+e.wrongCount+"</td></tr>").join("")
    :'<tr><td colspan="7" style="text-align:center;color:#888;">暂无错题，表现优秀！</td></tr>';
  const html='<!DOCTYPE html><html lang="zh-CN"><head><meta charset="UTF-8"><title>学习报告 - '+currentUser+'</title><style>'
    +'body{font-family:"Microsoft YaHei",sans-serif;color:#222;max-width:820px;margin:30px auto;padding:0 20px;}'
    +'h1{font-size:22px;border-bottom:3px solid #3b5bff;padding-bottom:10px;}'
    +'h2{font-size:16px;margin-top:28px;color:#3b5bff;}'
    +'table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px;}'
    +'th,td{border:1px solid #ccc;padding:7px 10px;text-align:left;}'
    +'th{background:#eef1ff;}'
    +'.meta{color:#666;font-size:13px;margin:8px 0 20px;}'
    +'.kpis{display:flex;gap:14px;flex-wrap:wrap;margin-top:10px;}'
    +'.kpi{border:1px solid #ccd;border-radius:8px;padding:12px 22px;text-align:center;}'
    +'.kpi b{display:block;font-size:24px;color:#3b5bff;}'
    +'.kpi span{font-size:12px;color:#777;}'
    +'.printbtn{margin-top:26px;padding:10px 28px;background:#3b5bff;color:#fff;border:none;border-radius:8px;font-size:15px;cursor:pointer;}'
    +'@media print{.printbtn,.noprint{display:none;}}'
    +'</style></head><body>'
    +'<h1>德语词汇变位练习系统 · 个人学习报告</h1>'
    +'<p class="meta">用户：<b>'+currentUser+'</b>　生成时间：'+nowStr()+'</p>'
    +'<h2>一、总体数据</h2><div class="kpis">'
    +'<div class="kpi"><b>'+recs.length+'</b><span>累计练习次数</span></div>'
    +'<div class="kpi"><b>'+tQ+'</b><span>累计答题数</span></div>'
    +'<div class="kpi"><b>'+(acc===null?"--":acc+"%")+'</b><span>总正确率</span></div>'
    +'<div class="kpi"><b>'+unm.length+'</b><span>未掌握错题</span></div></div>'
    +'<h2>二、近 7 天学习情况</h2><table><tr><th>日期</th><th>练习次数</th><th>答题数</th><th>正确率</th></tr>'+rows7+'</table>'
    +'<h2>三、各题型正确率</h2><table><tr><th>题型</th><th>练习次数</th><th>答题数</th><th>正确率</th></tr>'+rowsT+'</table>'
    +'<h2>四、高频错题 TOP 10（未掌握）</h2><table><tr><th>#</th><th>词汇</th><th>题干</th><th>我的答案</th><th>正确答案</th><th>错误类型</th><th>次数</th></tr>'+rowsW+'</table>'
    +'<button class="printbtn" onclick="window.print()">🖨 打印 / 另存为 PDF</button>'
    +'<p class="meta noprint" style="margin-top:14px;">提示：点击按钮（或按 Ctrl+P），目标打印机选择「另存为 PDF」即可生成 PDF 文件。</p>'
    +'</body></html>';
  const w=window.open("","_blank");
  if(!w){alert("浏览器拦截了弹窗，请允许本页面弹出窗口后重试");return;}
  w.document.write(html);w.document.close();
}
function exportCSV(){
  const recs=loadRecords();
  if(!recs.length){alert("暂无练习记录可导出");return;}
  const ptypeCN={fill:"变位填空",choice:"变位选择",dictation:"动词默写",retest:"错题重练"};
  let csv="日期,时间,题型,总题数,答对数,正确率,用时(秒)\n";
  recs.forEach(r=>{csv+=r.date+","+r.time+","+(ptypeCN[r.ptype]||r.ptype)+","+r.total+","+r.correct+","+Math.round(r.correct/r.total*100)+"%,"+r.duration+"\n";});
  const blob=new Blob(["\uFEFF"+csv],{type:"text/csv;charset=utf-8"});
  const a=document.createElement("a");
  a.href=URL.createObjectURL(blob);
  a.download="德语练习记录_"+currentUser+"_"+todayStr()+".csv";
  document.body.appendChild(a);a.click();a.remove();
}

/* ==================== 管理端 ==================== */
document.querySelectorAll("#segAdmin .seg-item").forEach(s=>s.onclick=()=>{
  document.querySelectorAll("#segAdmin .seg-item").forEach(x=>x.classList.remove("active"));
  s.classList.add("active");curAdminSub=s.dataset.v;renderAdmin();
});
function renderAdmin(){
  if(curAdminSub==="overview")renderAdminOverview();
  else if(curAdminSub==="vocab")renderAdminVocab();
  else if(curAdminSub==="users")renderAdminUsers();
  else renderAdminHot();
}
function allUserStats(){
  const users=ensureUsers();
  return Object.keys(users).map(u=>{
    const recs=loadRecords(u),wrongs=loadWrongs(u);
    const tq=recs.reduce((s,r)=>s+r.total,0),tc=recs.reduce((s,r)=>s+r.correct,0);
    return {u,role:users[u].role,created:users[u].created||"—",
      sessions:recs.length,totalQ:tq,acc:tq?Math.round(tc/tq*100):null,
      wrong:wrongs.filter(e=>!e.mastered).length,wrongs};
  });
}
function renderAdminOverview(){
  const stats=allUserStats(),vc=getVocab();
  const sum=f=>stats.reduce((s,x)=>s+f(x),0);
  const cards=[["注册用户数",stats.length],["词汇总量",vc.length],["全站练习次数",sum(s=>s.sessions)],["全站累计答题",sum(s=>s.totalQ)],["全站未掌握错题",sum(s=>s.wrong)]];
  let h='<div class="card"><h3>📊 系统概览</h3><div class="stat-grid">';
  cards.forEach(c=>{h+='<div class="stat-box"><div class="stat-num">'+c[1]+'</div><div class="stat-lab">'+c[0]+'</div></div>';});
  h+='</div><p class="hint" style="margin-top:14px;">说明：演示版数据存储于本机浏览器 localStorage，仅统计当前浏览器内的用户数据。</p></div>';
  $("adminContent").innerHTML=h;
}
function renderAdminVocab(){
  const vc=getVocab();
  let h='<div class="card"><h3>📖 词汇管理（共 '+vc.length+' 个）</h3>'
    +'<div class="vocab-toolbar"><button class="btn btn-sm" onclick="openWordForm()">＋ 新增词汇</button>'
    +'<button class="btn-ghost btn-sm" onclick="openImport()">📥 CSV 批量导入</button>'
    +'<button class="btn-ghost btn-sm" onclick="resetVocab()">♻ 恢复默认词库</button></div>'
    +'<div style="max-height:480px;overflow:auto;"><table class="tbl"><tr><th>词形</th><th>词性</th><th>释义</th><th>难度</th><th>操作</th></tr>';
  vc.forEach(w=>{
    h+='<tr><td style="font-weight:600;">'+(w.type==="noun"?w.gender+" ":"")+w.word+'</td>'
      +'<td><span class="badge badge-'+w.type+'">'+typeName(w.type)+'</span></td><td>'+w.zh+'</td>'
      +'<td style="color:#d97706;">'+stars(w.diff)+'</td>'
      +'<td style="white-space:nowrap;"><button class="btn-ghost btn-sm" onclick="openWordForm('+w.id+')">编辑</button> '
      +'<button class="btn-ghost btn-sm" style="border-color:#fca5a5;color:#dc2626;" onclick="deleteWord('+w.id+')">删除</button></td></tr>';
  });
  $("adminContent").innerHTML=h+"</table></div></div>";
}
function deleteWord(id){
  const vc=getVocab(),w=vc.find(v=>v.id===id);
  if(!w||!confirm("确定删除词汇「"+w.word+"」？"))return;
  saveVocab(vc.filter(v=>v.id!==id));renderAdminVocab();
}
function resetVocab(){
  if(!confirm("恢复默认词库？管理员新增和修改的词汇将全部丢失。"))return;
  saveVocab(JSON.parse(JSON.stringify(DEFAULT_VOCAB)));renderAdminVocab();
}
/* ---- 变位自动生成（规则变化） ---- */
function autoVerb(word){
  let stem=word;
  if(word.endsWith("en"))stem=word.slice(0,-2);
  else if(word.endsWith("n")||word.endsWith("e"))stem=word.slice(0,-1);
  const euph=/[dt]$/.test(stem)||/[cgf]n$/.test(stem)||/tm$/.test(stem);
  const sEnd=/[sßxz]$/.test(stem);
  return {ich:stem+"e",du:stem+(euph?"est":(sEnd?"t":"st")),"er/sie/es":stem+(euph?"et":"t"),wir:word,ihr:stem+(euph?"et":"t"),"sie/Sie":word};
}
function autoNoun(word,gender){
  const genSuf=/[aeiouäöü]$/i.test(word)||/(el|er|en|chen|lein)$/i.test(word)?"s":"es";
  if(gender==="der")return {nom:"der "+word,akk:"den "+word,dat:"dem "+word,gen:"des "+word+genSuf};
  if(gender==="das")return {nom:"das "+word,akk:"das "+word,dat:"dem "+word,gen:"des "+word+genSuf};
  return {nom:"die "+word,akk:"die "+word,dat:"der "+word,gen:"der "+word};
}
function autoAdj(word){
  const comp=/e$/.test(word)?word+"r":word+"er";
  const sup=/(d|t|s|ß|z|x|sch)$/i.test(word)?"am "+word+"esten":"am "+word+"sten";
  return {comp,sup};
}
/* ---- 词汇编辑表单 ---- */
function openWordForm(id){
  editingId=id||null;
  const w=id?getVocab().find(v=>v.id===id):null;
  const type=w?w.type:"verb";
  let h='<h3>'+(id?"✏️ 编辑词汇":"＋ 新增词汇")+'</h3>';
  h+='<div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin:14px 0;">'
    +'<label class="wf-label">词性<br><select id="wfType" style="width:100%;margin-top:4px;" onchange="renderWfForms(null)">'
    +'<option value="verb"'+(type==="verb"?" selected":"")+'>动词</option>'
    +'<option value="noun"'+(type==="noun"?" selected":"")+'>名词</option>'
    +'<option value="adj"'+(type==="adj"?" selected":"")+'>形容词</option></select></label>'
    +'<label class="wf-label">词形<br><input id="wfWord" style="width:100%;margin-top:4px;" value="'+(w?w.word:"")+'" placeholder="如 lernen"></label>'
    +'<label class="wf-label">中文释义<br><input id="wfZh" style="width:100%;margin-top:4px;" value="'+(w?w.zh:"")+'" placeholder="如 学习"></label>'
    +'<label class="wf-label">难度<br><select id="wfDiff" style="width:100%;margin-top:4px;">'
    +[1,2,3].map(d=>'<option value="'+d+'"'+((w?w.diff:1)===d?" selected":"")+'>'+stars(d)+'</option>').join("")+'</select></label></div>'
    +'<div id="wfForms"></div>'
    +'<label class="wf-label" style="display:block;margin-top:12px;">例句（选填）<br><input id="wfExample" style="width:100%;margin-top:4px;" value="'+(w&&w.example?String(w.example).replace(/"/g,"&quot;"):"")+'"></label>'
    +'<div style="margin-top:16px;text-align:right;"><button class="btn-ghost btn-sm" onclick="closeModal()">取消</button> <button class="btn btn-sm" onclick="saveWordForm()">保存</button></div>';
  $("modalBody").innerHTML=h;
  renderWfForms(w);
  $("modalMask").classList.remove("hidden");
}
function renderWfForms(w){
  const t=$("wfType").value;
  let h='<div style="background:#f8fafc;border:1px solid #e6ebf4;border-radius:10px;padding:12px 14px;">'
    +'<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">'
    +'<b style="color:#4f46e5;font-size:13px;">变位数据</b>'
    +'<button class="btn-ghost btn-sm" onclick="wfAutoFill()">⚡ 按规则自动生成</button></div>';
  if(t==="verb"){
    PERSONS.forEach(p=>{h+='<div style="display:flex;align-items:center;gap:8px;margin:5px 0;"><span class="person-lab">'+p+'</span><input id="wf_p_'+p+'" class="q-input" style="min-width:0;flex:1;" value="'+(w&&w.conj?w.conj[p]:"")+'"></div>';});
  }else if(t==="noun"){
    h+='<div style="display:flex;gap:10px;margin:5px 0;flex-wrap:wrap;"><label class="wf-label">冠词 <select id="wfGender">'+["der","die","das"].map(g=>'<option'+(w&&w.gender===g?" selected":"")+'>'+g+'</option>').join("")+'</select></label>'
      +'<label class="wf-label" style="flex:1;min-width:140px;">复数 <input id="wfPlural" class="q-input" style="min-width:0;width:100%;" value="'+(w&&w.plural?w.plural:"")+'" placeholder="如 Bücher"></label></div>';
    CASES.forEach(c=>{h+='<div style="display:flex;align-items:center;gap:8px;margin:5px 0;"><span class="person-lab" style="min-width:80px;">'+c[1]+'</span><input id="wf_c_'+c[0]+'" class="q-input" style="min-width:0;flex:1;" value="'+(w&&w.decl?w.decl[c[0]]:"")+'" placeholder="如 den Apfel"></div>';});
  }else{
    h+='<div style="display:flex;align-items:center;gap:8px;margin:5px 0;"><span class="person-lab">比较级</span><input id="wfComp" class="q-input" style="min-width:0;flex:1;" value="'+(w&&w.comp?w.comp:"")+'"></div>'
      +'<div style="display:flex;align-items:center;gap:8px;margin:5px 0;"><span class="person-lab">最高级</span><input id="wfSup" class="q-input" style="min-width:0;flex:1;" value="'+(w&&w.sup?w.sup:"")+'"></div>';
  }
  h+='<div style="font-size:12px;color:#94a3b8;margin-top:8px;">不规则词（如 sein / haben / gut）请点击自动生成后手动修改对应格子。</div></div>';
  $("wfForms").innerHTML=h;
}
function wfAutoFill(){
  const t=$("wfType").value,word=$("wfWord").value.trim();
  if(!word){alert("请先填写词形");return;}
  if(t==="verb"){const c=autoVerb(word);PERSONS.forEach(p=>$("wf_p_"+p).value=c[p]);}
  else if(t==="noun"){const d=autoNoun(word,$("wfGender").value);CASES.forEach(c=>$("wf_c_"+c[0]).value=d[c[0]]);}
  else{const a=autoAdj(word);$("wfComp").value=a.comp;$("wfSup").value=a.sup;}
}
function saveWordForm(){
  const type=$("wfType").value,word=$("wfWord").value.trim(),zh=$("wfZh").value.trim();
  const diff=parseInt($("wfDiff").value),example=$("wfExample").value.trim();
  if(!word||!zh){alert("词形和中文释义必填");return;}
  const vc=getVocab();
  if(!editingId&&vc.some(v=>v.word.toLowerCase()===word.toLowerCase()&&v.type===type)){alert("该词汇已存在（词形+词性相同）");return;}
  let obj=editingId?vc.find(v=>v.id===editingId):{id:vc.reduce((m,v)=>Math.max(m,v.id),0)+1};
  if(!obj){alert("保存失败：未找到该词汇");return;}
  obj.word=word;obj.type=type;obj.zh=zh;obj.diff=diff;obj.example=example;
  if(type==="verb"){
    const conj={};
    for(const p of PERSONS){const v=$("wf_p_"+p).value.trim();if(!v){alert("请填写完整六个人称（可点「自动生成」按钮）");return;}conj[p]=v;}
    obj.conj=conj;delete obj.decl;delete obj.gender;delete obj.plural;delete obj.comp;delete obj.sup;
    if(!obj.ctype)obj.ctype="手动录入";
  }else if(type==="noun"){
    const decl={};
    for(const c of CASES){const v=$("wf_c_"+c[0]).value.trim();if(!v){alert("请填写完整四个格（可点「自动生成」按钮）");return;}decl[c[0]]=v;}
    obj.decl=decl;obj.gender=$("wfGender").value;obj.plural=$("wfPlural").value.trim()||word;
    delete obj.conj;delete obj.comp;delete obj.sup;obj.ctype="";
  }else{
    const comp=$("wfComp").value.trim(),sup=$("wfSup").value.trim();
    if(!comp||!sup){alert("请填写比较级和最高级（可点「自动生成」按钮）");return;}
    obj.comp=comp;obj.sup=sup;delete obj.conj;delete obj.decl;delete obj.gender;delete obj.plural;
    if(!obj.ctype)obj.ctype="手动录入";
  }
  if(editingId){const i=vc.findIndex(v=>v.id===editingId);vc[i]=obj;}else vc.push(obj);
  saveVocab(vc);closeModal();renderAdminVocab();
}
/* ---- CSV 批量导入 ---- */
function openImport(){
  let h='<h3>📥 CSV 批量导入词汇</h3>'
    +'<div style="font-size:12px;color:#64748b;line-height:2;margin:10px 0;">每行一个词，英文逗号分隔（释义/例句中请勿包含逗号）：<br>'
    +'动词：<code>verb,词形,释义,难度,ich,du,er/sie/es,wir,ihr,sie/Sie,例句</code><br>'
    +'名词：<code>noun,词形,释义,难度,冠词,复数,第一格,第四格,第三格,第二格,例句</code><br>'
    +'形容词：<code>adj,词形,释义,难度,比较级,最高级,例句</code></div>'
    +'<input type="file" id="csvFile" accept=".csv,.txt" style="margin-bottom:8px;color:#475569;font-size:13px;">'
    +'<textarea id="csvText" rows="7" style="width:100%;background:#f8fafc;color:#1e2433;border:1px solid #cbd5e1;border-radius:8px;padding:10px;font-size:13px;font-family:Consolas,monospace;" placeholder="verb,tanzen,跳舞,1,tanze,tanzt,tanzt,tanzen,tanzt,tanzen,Wir tanzen gern."></textarea>'
    +'<div id="importMsg" style="font-size:13px;margin:8px 0;line-height:1.8;"></div>'
    +'<div style="text-align:right;"><button class="btn-ghost btn-sm" onclick="closeModal()">关闭</button> <button class="btn btn-sm" onclick="doImport()">开始导入</button></div>';
  $("modalBody").innerHTML=h;
  $("modalMask").classList.remove("hidden");
  $("csvFile").onchange=e=>{
    const f=e.target.files[0];if(!f)return;
    const r=new FileReader();
    r.onload=()=>{$("csvText").value=r.result;};
    r.readAsText(f,"UTF-8");
  };
}
function doImport(){
  const lines=$("csvText").value.split(/\r?\n/);
  const vc=getVocab();
  let nextId=vc.reduce((m,v)=>Math.max(m,v.id),0)+1,ok=0,skip=0;const errs=[];
  lines.forEach((line,idx)=>{
    const t=line.trim();if(!t)return;
    const c=t.split(",").map(s=>s.trim());
    const type=c[0];
    if(["verb","noun","adj"].indexOf(type)<0){errs.push("第"+(idx+1)+"行：首列必须是 verb / noun / adj");skip++;return;}
    const word=c[1],zh=c[2];
    if(!word||!zh){errs.push("第"+(idx+1)+"行：缺少词形或释义");skip++;return;}
    if(vc.some(v=>v.word.toLowerCase()===word.toLowerCase()&&v.type===type)){errs.push("第"+(idx+1)+"行：「"+word+"」已存在，跳过");skip++;return;}
    const diff=[1,2,3].indexOf(parseInt(c[3]))>=0?parseInt(c[3]):1;
    let obj=null;
    if(type==="verb"){
      if(c.length<10){errs.push("第"+(idx+1)+"行：动词需要至少10列");skip++;return;}
      obj={id:nextId++,word,type,zh,diff,ctype:"CSV导入",conj:{ich:c[4],du:c[5],"er/sie/es":c[6],wir:c[7],ihr:c[8],"sie/Sie":c[9]},example:c[10]||""};
    }else if(type==="noun"){
      if(c.length<10){errs.push("第"+(idx+1)+"行：名词需要至少10列");skip++;return;}
      obj={id:nextId++,word,type,zh,diff,gender:["der","die","das"].indexOf(c[4])>=0?c[4]:"der",plural:c[5]||word,decl:{nom:c[6],akk:c[7],dat:c[8],gen:c[9]},example:c[10]||""};
    }else{
      if(c.length<6){errs.push("第"+(idx+1)+"行：形容词至少需要6列");skip++;return;}
      obj={id:nextId++,word,type,zh,diff,ctype:"CSV导入",comp:c[4],sup:c[5],example:c[6]||""};
    }
    vc.push(obj);ok++;
  });
  saveVocab(vc);
  $("importMsg").innerHTML='<span style="color:#16a34a;">✔ 成功导入 '+ok+' 个</span>，跳过 '+skip+' 个'
    +(errs.length?'<br><span style="color:#dc2626;">'+errs.slice(0,6).join("<br>")+(errs.length>6?"<br>…其余省略":"")+"</span>":"");
  renderAdminVocab();
}
/* ---- 用户管理 ---- */
function renderAdminUsers(){
  const stats=allUserStats();
  let h='<div class="card"><h3>👥 用户管理（共 '+stats.length+' 人）</h3><div style="overflow-x:auto;"><table class="tbl">'
    +'<tr><th>用户名</th><th>角色</th><th>注册时间</th><th>练习次数</th><th>累计答题</th><th>总正确率</th><th>未掌握错题</th><th>操作</th></tr>';
  stats.forEach(s=>{
    h+='<tr><td style="font-weight:600;">'+s.u+'</td>'
      +'<td>'+(s.role==="admin"?'<span style="color:#d97706;">👑 管理员</span>':"学习者")+'</td>'
      +'<td style="color:#64748b;">'+s.created+'</td><td>'+s.sessions+'</td><td>'+s.totalQ+'</td>'
      +'<td>'+(s.acc===null?"--":s.acc+"%")+'</td>'
      +'<td>'+(s.wrong?'<span style="color:#dc2626;">'+s.wrong+'</span>':"0")+'</td>'
      +'<td>'+(s.u==="admin"?'<span style="color:#94a3b8;">—</span>':'<button class="btn-ghost btn-sm" style="border-color:#fca5a5;color:#dc2626;" onclick="deleteUser(\''+s.u+'\')">删除</button>')+'</td></tr>';
  });
  $("adminContent").innerHTML=h+"</table></div></div>";
}
function deleteUser(u){
  if(!confirm("确定删除用户「"+u+"」？其练习记录和错题数据将一并清除。"))return;
  const users=ensureUsers();delete users[u];saveUsers(users);
  localStorage.removeItem("dv_rec_"+u);localStorage.removeItem("dv_wrong_"+u);
  renderAdminUsers();
}
/* ---- 高频错题（全站汇总） ---- */
function renderAdminHot(){
  const stats=allUserStats(),agg={};
  stats.forEach(s=>{
    s.wrongs.forEach(e=>{
      if(!agg[e.key])agg[e.key]={word:e.word,prompt:e.prompt,answer:e.correctAnswer,etype:e.errorType,count:0,users:{}};
      agg[e.key].count+=e.wrongCount;agg[e.key].users[s.u]=1;
    });
  });
  const list=Object.values(agg).sort((a,b)=>b.count-a.count).slice(0,20);
  if(!list.length){$("adminContent").innerHTML='<div class="card"><div class="empty-tip">全站暂无错题数据</div></div>';return;}
  let h='<div class="card"><h3>🔥 全站高频错题 TOP '+list.length+'</h3><div style="overflow-x:auto;"><table class="tbl">'
    +'<tr><th>#</th><th>词汇</th><th>题干</th><th>正确答案</th><th>主要错误类型</th><th>累计错误</th><th>涉及人数</th></tr>';
  list.forEach((e,i)=>{
    h+='<tr><td>'+(i+1)+'</td><td style="font-weight:600;">'+(e.word||"")+'</td>'
      +'<td style="color:#64748b;font-size:13px;max-width:240px;">'+(e.prompt||"—")+'</td>'
      +'<td class="rv-correct">'+(e.answer||"—")+'</td><td><span class="weak-tag">'+(e.etype||"词形错误")+'</span></td>'
      +'<td style="color:#d97706;">'+e.count+'</td><td>'+Object.keys(e.users).length+'</td></tr>';
  });
  $("adminContent").innerHTML=h+"</table></div></div>";
}

/* ==================== 初始化 ==================== */
ensureUsers();
getVocab();
drawCaptcha();
danmuBurst("loginDanmu",10);
</script>
</body>
</html>
