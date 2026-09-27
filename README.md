<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#F2F2F7" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)">
<title>习惯打卡</title>
<style>
:root{
  --blue:#007AFF;--red:#FF3B30;--green:#34C759;--orange:#FF9500;
  --purple:#AF52DE;--pink:#FF2D55;--teal:#5AC8FA;--yellow:#FFCC00;--indigo:#5856D6;
  --bg:#F2F2F7;--page-bg:#E6E6EB;--card:#FFFFFF;
  --label:#000000;
  --label-2:rgba(60,60,67,.6);
  --label-3:rgba(60,60,67,.42);
  --label-4:rgba(60,60,67,.28);
  --fill:rgba(120,120,128,.12);
  --fill-2:rgba(120,120,128,.08);
  --sep:rgba(60,60,67,.14);
  --sep-2:rgba(60,60,67,.2);
  --nav-bg:rgba(249,249,249,.82);
  --nav-border:rgba(60,60,67,.16);
  --tabbar-bg:rgba(255,255,255,.62);
  --tabbar-border:rgba(255,255,255,.65);
  --shadow:0 1px 3px rgba(0,0,0,.04), 0 8px 24px -12px rgba(0,0,0,.26);
  --tabbar-shadow:0 -6px 24px -10px rgba(0,0,0,.18), 0 3px 10px rgba(0,0,0,.06);
  --fb-blue:#1877F2;--apple-black:#000000;
}
@media (prefers-color-scheme: dark){
  :root{
    --blue:#0A84FF;--red:#FF453A;--green:#30D158;--orange:#FF9F0A;
    --purple:#BF5AF2;--pink:#FF375F;--teal:#64D2FF;--yellow:#FFD60A;--indigo:#5E5CE6;
    --bg:#000000;--page-bg:#000000;--card:#1C1C1E;
    --label:#FFFFFF;
    --label-2:rgba(235,235,245,.6);
    --label-3:rgba(235,235,245,.4);
    --label-4:rgba(235,235,245,.26);
    --fill:rgba(120,120,128,.24);
    --fill-2:rgba(120,120,128,.16);
    --sep:rgba(84,84,88,.4);
    --sep-2:rgba(84,84,88,.6);
    --nav-bg:rgba(28,28,30,.82);
    --nav-border:rgba(84,84,88,.5);
    --tabbar-bg:rgba(30,30,32,.62);
    --tabbar-border:rgba(255,255,255,.09);
    --shadow:0 1px 3px rgba(0,0,0,.5);
    --tabbar-shadow:0 -6px 24px -10px rgba(0,0,0,.75), 0 3px 10px rgba(0,0,0,.45);
    --apple-black:#FFFFFF;
  }
}

*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;padding:0;height:100%}
body{
  font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","PingFang SC","Helvetica Neue","Microsoft YaHei",sans-serif;
  background:var(--page-bg);color:var(--label);
  -webkit-font-smoothing:antialiased;overscroll-behavior-y:none;
}

.app{
  display:flex;flex-direction:column;
  height:100vh;height:100dvh;
  max-width:440px;margin:0 auto;
  background:var(--bg);overflow:hidden;position:relative;
  border-radius:0 0 28px 28px;
}
@media (min-width:480px){
  .app{box-shadow:0 0 0 1px rgba(0,0,0,.06), 0 30px 80px -30px rgba(0,0,0,.5);border-radius:0 0 34px 34px}
}

/* ---------- 页面容器 ---------- */
.page{
  position:absolute;inset:0;
  padding-top:env(safe-area-inset-top);
  display:none;flex-direction:column;
  background:var(--bg);z-index:10;
}
.page.active{display:flex}

.page.overlay{z-index:40;background:var(--bg)}
.page.overlay.active{
  display:flex;
  animation:pageIn .4s cubic-bezier(.32,.72,0,1) both;
}
@keyframes pageIn{from{transform:translateX(100%)}to{transform:translateX(0)}}
.page.overlay.closing{animation:pageOut .34s cubic-bezier(.32,.72,0,1) both}
@keyframes pageOut{from{transform:translateX(0)}to{transform:translateX(100%)}}

.scroll{
  flex:1 1 auto;min-height:0;
  overflow-y:auto;-webkit-overflow-scrolling:touch;
  padding-bottom:110px;
}
.page.overlay .scroll{padding-bottom:40px}

/* ================= iOS 导航栏 ================= */
.ios-nav{
  flex:0 0 auto;
  height:44px;
  display:flex;align-items:center;
  padding:0 6px;
  background:var(--nav-bg);
  backdrop-filter:blur(20px) saturate(180%);
  -webkit-backdrop-filter:blur(20px) saturate(180%);
  position:relative;z-index:20;
}
.ios-nav::after{
  content:"";position:absolute;left:0;right:0;bottom:0;
  height:0.5px;background:var(--nav-border);
  transform:scaleY(.5);transform-origin:bottom;
}
.nav-back{
  position:relative;
  height:44px;min-width:44px;
  padding:0 10px 0 6px;
  border:0;background:none;
  color:var(--blue);
  font-family:inherit;font-size:17px;font-weight:400;
  display:flex;align-items:center;gap:2px;
  cursor:pointer;letter-spacing:-.2px;
  transition:opacity .15s;
  z-index:2;
}
.nav-back:active{opacity:.35}
.nav-back svg{width:22px;height:22px;stroke:var(--blue);flex:0 0 auto;margin-left:-4px}
.nav-back::before{content:"";position:absolute;left:-8px;top:-8px;bottom:-8px;right:-6px;}
.nav-title{
  position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);
  font-size:17px;font-weight:600;letter-spacing:-.4px;
  color:var(--label);white-space:nowrap;max-width:56%;
  overflow:hidden;text-overflow:ellipsis;z-index:1;
}
.nav-right{
  margin-left:auto;height:44px;padding:0 12px;border:0;background:none;
  color:var(--blue);font-family:inherit;font-size:17px;font-weight:500;
  display:flex;align-items:center;cursor:pointer;
  transition:opacity .15s;z-index:2;
}
.nav-right:active{opacity:.35}

/* 大标题头部 */
.header{flex:0 0 auto;padding-bottom:8px;padding-top:8px}
.header-row{
  margin:0 16px;height:46px;
  display:flex;align-items:center;justify-content:space-between;gap:10px;
}
.header-row h1{margin:0;font-size:30px;font-weight:700;letter-spacing:-.8px;flex:1}
.header-row .sub{font-size:13px;color:var(--label-2);margin-top:2px}

.icon-btn{
  width:36px;height:36px;border:0;border-radius:50%;
  background:var(--fill);color:var(--label);
  display:grid;place-items:center;position:relative;cursor:pointer;flex:0 0 auto;
  transition:transform .18s cubic-bezier(.34,1.56,.64,1), background .2s;
}
.icon-btn:active{transform:scale(.88);background:var(--fill-2)}
.icon-btn svg{width:20px;height:20px}

.chips{display:flex;gap:8px;overflow-x:auto;padding:12px 16px 4px;scrollbar-width:none;-ms-overflow-style:none}
.chips::-webkit-scrollbar{display:none}
.chip{
  flex:0 0 auto;height:32px;padding:0 15px;border:0;border-radius:16px;
  background:var(--fill);color:var(--label);
  font-family:inherit;font-size:14px;font-weight:500;cursor:pointer;
  transition:background .24s, color .24s, transform .15s;
}
.chip:active{transform:scale(.93)}
.chip.active{background:var(--blue);color:#fff}

.block{
  background:var(--card);border-radius:20px;
  margin:12px 16px 0;padding:18px;box-shadow:var(--shadow);
}
.block-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px}
.block-head h3{margin:0;font-size:16px;font-weight:700;letter-spacing:-.35px}
.block-head .more{font-size:13px;color:var(--blue);font-weight:500;cursor:pointer}

.section-head{display:flex;align-items:baseline;justify-content:space-between;padding:24px 16px 0}
.section-head h3{margin:0;font-size:18px;font-weight:700;letter-spacing:-.4px}
.section-head span{font-size:12px;color:var(--label-2)}

/* Hero */
.hero{
  position:relative;margin:8px 16px 0;
  border-radius:24px;overflow:hidden;padding:22px 22px 20px;color:#fff;
  background:linear-gradient(135deg,#0A84FF 0%,#5E5CE6 52%,#BF5AF2 100%);
  box-shadow:0 20px 44px -20px rgba(10,132,255,.85), 0 2px 8px rgba(10,132,255,.2);
}
.hero::after{content:"";position:absolute;inset:0;background:linear-gradient(180deg,rgba(255,255,255,.24),rgba(255,255,255,0) 42%);pointer-events:none}
.hero-top{display:flex;justify-content:space-between;align-items:flex-start;position:relative;z-index:1}
.hero-date{font-size:12.5px;opacity:.9;letter-spacing:.2px;font-weight:500}
.hero-progress{margin-top:16px;position:relative;z-index:1;display:flex;align-items:flex-end;gap:10px}
.hero-progress .big{font-size:46px;font-weight:700;letter-spacing:-2px;line-height:.92;font-variant-numeric:tabular-nums}
.hero-progress .unit{font-size:15px;font-weight:600;opacity:.9;padding-bottom:5px}
.hero-bar{margin-top:16px;height:9px;border-radius:5px;background:rgba(255,255,255,.24);overflow:hidden;position:relative;z-index:1}
.hero-bar i{display:block;height:100%;border-radius:5px;background:#fff;transition:width .8s cubic-bezier(.22,1,.36,1);box-shadow:0 0 12px rgba(255,255,255,.55)}
.hero-meta{margin-top:11px;font-size:12.5px;opacity:.92;display:flex;justify-content:space-between;align-items:center;position:relative;z-index:1;font-weight:500}
.deco{position:absolute;border-radius:50%;background:rgba(255,255,255,.13);pointer-events:none}
.deco-1{width:170px;height:170px;right:-56px;top:-66px}
.deco-2{width:104px;height:104px;right:44px;bottom:-58px;background:rgba(255,255,255,.09)}

/* 习惯卡片 */
.habit-list{padding:10px 16px 0;display:flex;flex-direction:column;gap:12px}
.habit{
  position:relative;display:flex;align-items:center;gap:14px;
  background:var(--card);border-radius:20px;
  padding:15px 15px 15px 16px;box-shadow:var(--shadow);
  animation:cardIn .48s cubic-bezier(.22,1,.36,1) backwards;
  transition:transform .18s ease, box-shadow .2s;overflow:hidden;cursor:pointer;
}
.habit:active{transform:scale(.984)}
.habit.removing{animation:cardOut .35s cubic-bezier(.55,0,.68,.5) forwards}
@keyframes cardOut{to{opacity:0;transform:translateX(-100%) scale(.9);margin-bottom:-64px}}
@keyframes cardIn{from{opacity:0;transform:translateY(14px) scale(.96)}to{opacity:1;transform:none}}
.habit::before{content:"";position:absolute;left:0;top:18%;bottom:18%;width:3px;border-radius:0 3px 3px 0;background:var(--habit-color,var(--blue));opacity:.9}
.habit-ico{flex:0 0 48px;width:48px;height:48px;border-radius:15px;display:grid;place-items:center;font-size:23px;position:relative}
.habit-body{flex:1;min-width:0}
.habit-name{font-size:15.5px;font-weight:600;letter-spacing:-.25px;display:flex;align-items:center;gap:7px}
.habit-name .streak{font-size:10.5px;font-weight:700;color:var(--orange);background:rgba(255,149,0,.14);padding:2.5px 8px;border-radius:999px;display:inline-flex;align-items:center;gap:2px}
.habit-sub{margin-top:5px;font-size:12px;color:var(--label-2);display:flex;align-items:center;gap:8px}
.habit-sub .dot{width:3px;height:3px;border-radius:50%;background:var(--label-4)}
.habit-dots{display:flex;gap:3.5px;margin-top:8px}
.habit-dots i{width:6px;height:6px;border-radius:50%;background:var(--fill);transition:background .3s}
.habit-dots i.on{background:var(--green)}
.habit-dots i.today{box-shadow:0 0 0 1.5px var(--blue)}

.check-btn{
  flex:0 0 auto;width:54px;height:54px;border-radius:50%;
  border:2.5px solid var(--fill);background:transparent;color:var(--label-4);
  display:grid;place-items:center;cursor:pointer;
  transition:all .26s cubic-bezier(.34,1.56,.64,1);position:relative;
}
.check-btn svg{width:25px;height:25px;transition:transform .3s cubic-bezier(.34,1.56,.64,1)}
.check-btn:active{transform:scale(.88)}
.check-btn:disabled{opacity:.35;cursor:default}
.check-btn.done{background:var(--green);border-color:var(--green);color:#fff;box-shadow:0 10px 24px -10px rgba(52,199,89,.95)}
.check-btn.done svg{transform:scale(1.12)}
.check-btn.pop{animation:btnPop .55s cubic-bezier(.34,1.56,.64,1)}
@keyframes btnPop{0%{transform:scale(1)}35%{transform:scale(1.25)}65%{transform:scale(.92)}100%{transform:scale(1)}}
.habit-hint{position:absolute;right:78px;top:50%;transform:translateY(-50%) translateX(10px);font-size:11px;font-weight:600;color:var(--label-3);background:var(--fill);padding:4px 9px;border-radius:10px;opacity:0;transition:opacity .3s, transform .3s;pointer-events:none}
.habit.show-hint .habit-hint{opacity:1;transform:translateY(-50%) translateX(0)}

/* 统计 */
.stat-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;padding:10px 16px 0}
.stat-card{background:var(--card);border-radius:20px;padding:17px 16px;box-shadow:var(--shadow);position:relative;overflow:hidden}
.stat-card::after{content:"";position:absolute;right:-30px;top:-30px;width:90px;height:90px;border-radius:50%;background:var(--stat-color,var(--blue));opacity:.08}
.stat-card .ico{width:38px;height:38px;border-radius:12px;display:grid;place-items:center;color:#fff;margin-bottom:13px;background:var(--stat-color,var(--blue))}
.stat-card .ico svg{width:20px;height:20px}
.stat-card .num{font-size:27px;font-weight:700;letter-spacing:-1px;font-variant-numeric:tabular-nums;line-height:1}
.stat-card .num small{font-size:13px;font-weight:600;color:var(--label-2);margin-left:3px}
.stat-card .lbl{font-size:12px;color:var(--label-2);margin-top:5px;font-weight:500}

.chart{display:flex;align-items:flex-end;justify-content:space-between;gap:7px;height:140px;padding:0 2px}
.chart-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:9px;height:100%;justify-content:flex-end}
.chart-bar{width:100%;max-width:28px;border-radius:9px;background:var(--fill);position:relative;transition:height .8s cubic-bezier(.22,1,.36,1);min-height:7px}
.chart-bar.on{background:linear-gradient(180deg,#0A84FF,#5E5CE6)}
.chart-bar.today{box-shadow:0 0 0 2px var(--blue)}
.chart-bar .val{position:absolute;top:-20px;left:50%;transform:translateX(-50%);font-size:10.5px;font-weight:700;color:var(--blue);opacity:0;transition:opacity .3s}
.chart-bar.on .val{opacity:1}
.chart-day{font-size:10.5px;color:var(--label-3);font-weight:600}
.chart-day.on{color:var(--blue)}
.chart-day.today{background:var(--blue);color:#fff;width:20px;height:20px;border-radius:50%;display:grid;place-items:center;font-size:10px}

.ring-wrap{display:flex;align-items:center;gap:22px;padding:4px 0}
.ring{position:relative;width:116px;height:116px;flex:0 0 auto}
.ring svg{transform:rotate(-90deg);display:block}
.ring .center{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center}
.ring .center .p{font-size:26px;font-weight:700;letter-spacing:-1px;font-variant-numeric:tabular-nums;line-height:1}
.ring .center .t{font-size:10.5px;color:var(--label-2);margin-top:3px}
.ring-info{flex:1;min-width:0}
.ring-info .row{display:flex;align-items:center;gap:9px;font-size:13px;margin-bottom:12px}
.ring-info .row:last-child{margin-bottom:0}
.ring-info .row i{width:9px;height:9px;border-radius:50%;flex:0 0 auto}
.ring-info .row span{color:var(--label-2);font-weight:500}
.ring-info .row b{margin-left:auto;font-weight:700;font-variant-numeric:tabular-nums}

/* 我的 */
.profile{padding:10px 16px 0}
.profile-card{
  position:relative;background:var(--card);border-radius:22px;
  padding:20px 18px;display:flex;align-items:center;gap:14px;
  box-shadow:var(--shadow);overflow:hidden;cursor:pointer;
}
.profile-card::after{content:"";position:absolute;right:-40px;top:-60px;width:160px;height:160px;border-radius:50%;background:linear-gradient(135deg,rgba(10,132,255,.14),rgba(175,82,222,.1))}
.avatar{
  flex:0 0 60px;width:60px;height:60px;border-radius:50%;
  background:linear-gradient(135deg,#0A84FF,#BF5AF2);
  display:grid;place-items:center;color:#fff;font-size:23px;font-weight:600;
  position:relative;z-index:1;
}
.avatar::after{content:"";position:absolute;inset:-3px;border-radius:50%;border:2px solid var(--card)}
.profile-info{flex:1;min-width:0;position:relative;z-index:1}
.profile-info h2{margin:0;font-size:19px;font-weight:700;letter-spacing:-.45px}
.profile-info p{margin:5px 0 0;font-size:12px;color:var(--label-2);font-weight:500}
.level-tag{position:absolute;top:0;right:0;font-size:10px;font-weight:700;color:#fff;background:linear-gradient(135deg,#FF9500,#FF2D55);padding:3px 9px;border-radius:999px}

.stats{display:grid;grid-template-columns:repeat(3,1fr);background:var(--card);border-radius:20px;margin-top:12px;padding:18px 0;box-shadow:var(--shadow)}
.stat{text-align:center;position:relative}
.stat + .stat::before{content:"";position:absolute;left:0;top:50%;transform:translateY(-50%);width:.5px;height:28px;background:var(--sep-2)}
.stat .num{font-size:20px;font-weight:700;letter-spacing:-.5px;font-variant-numeric:tabular-nums}
.stat .lbl{font-size:11px;color:var(--label-2);margin-top:4px;font-weight:500}

.group-title{margin:24px 0 10px;font-size:13px;font-weight:600;color:var(--label-2);padding-left:4px}
.list{background:var(--card);border-radius:18px;overflow:hidden;box-shadow:var(--shadow)}
.list-item{display:flex;align-items:center;gap:13px;padding:14px 16px;cursor:pointer;position:relative;transition:background .18s}
.list-item + .list-item::before{content:"";position:absolute;left:56px;right:0;top:0;height:.5px;background:var(--sep)}
.list-item:active{background:var(--fill-2)}
.list-ico{flex:0 0 32px;width:32px;height:32px;border-radius:10px;display:grid;place-items:center;color:#fff}
.list-ico svg{width:18px;height:18px}
.list-ico.b{background:var(--blue)}
.list-ico.g{background:var(--green)}
.list-ico.o{background:var(--orange)}
.list-ico.p{background:var(--purple)}
.list-ico.r{background:var(--red)}
.list-ico.t{background:var(--teal)}
.list-ico.pk{background:var(--pink)}
.list-ico.y{background:var(--yellow)}
.list-text{flex:1;min-width:0;font-size:14.5px;font-weight:500;letter-spacing:-.2px}
.list-val{font-size:13px;color:var(--label-2);font-weight:500}
.chev{color:var(--label-4);flex:0 0 auto;display:grid;place-items:center}
.chev svg{width:16px;height:16px}

/* 空状态 */
.empty{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:60px 40px 80px;text-align:center}
.empty .ico{width:84px;height:84px;border-radius:50%;background:var(--fill);display:grid;place-items:center;margin-bottom:18px}
.empty .ico svg{width:38px;height:38px;color:var(--label-3)}
.empty h4{margin:0 0 7px;font-size:16.5px;font-weight:600}
.empty p{margin:0 0 20px;font-size:13px;color:var(--label-2);line-height:1.55}
.empty button{
  border:0;border-radius:16px;padding:0 24px;height:44px;
  background:var(--blue);color:#fff;font-family:inherit;font-size:14.5px;font-weight:600;
  cursor:pointer;
}

/* 表单 */
.form-wrap{padding:10px 16px 0}
.field{margin-bottom:18px}
.field label{display:block;font-size:12px;font-weight:600;color:var(--label-2);margin-bottom:9px;padding-left:3px}
.field-box{
  display:flex;align-items:center;background:var(--card);border-radius:15px;
  height:52px;padding:0 15px;gap:11px;box-shadow:var(--shadow);
  border:1.5px solid transparent;transition:border-color .22s, box-shadow .22s;
}
.field-box:focus-within{border-color:var(--blue);box-shadow:0 0 0 4px rgba(0,122,255,.12)}
.field-box svg{width:20px;height:20px;color:var(--label-3);flex:0 0 auto}
.field-box input{flex:1;min-width:0;border:0;outline:none;background:transparent;font-family:inherit;font-size:16px;color:var(--label);font-weight:500}
.field-box input::placeholder{color:var(--label-3);font-weight:400}
.field-box .eye{border:0;background:none;color:var(--label-3);padding:6px;cursor:pointer;display:grid;place-items:center;margin-right:-4px}
.field-box .eye svg{width:20px;height:20px}
.field-box .code-btn{border:0;background:none;color:var(--blue);font-family:inherit;font-size:13.5px;font-weight:600;cursor:pointer;white-space:nowrap;padding:6px 0}
.field-box .code-btn:disabled{color:var(--label-4);cursor:default}

.field-row{display:flex;justify-content:space-between;align-items:center;margin:-4px 0 18px;padding:0 2px}
.field-row label{display:flex;align-items:center;gap:7px;font-size:12.5px;color:var(--label-2);cursor:pointer}
.field-row label input{width:16px;height:16px;accent-color:var(--blue);margin:0;cursor:pointer}
.field-row a{font-size:12.5px;color:var(--blue);text-decoration:none;font-weight:500}

.emoji-picker{display:flex;flex-wrap:wrap;gap:9px}
.emoji-pick{width:48px;height:48px;border-radius:15px;border:2px solid transparent;background:var(--fill);display:grid;place-items:center;font-size:23px;cursor:pointer;transition:all .22s}
.emoji-pick.active{border-color:var(--blue);background:rgba(0,122,255,.1);transform:scale(1.06)}

.color-picker{display:flex;gap:11px;flex-wrap:wrap}
.color-pick{width:40px;height:40px;border-radius:50%;border:3px solid transparent;cursor:pointer;transition:transform .2s;position:relative}
.color-pick.active{transform:scale(1.08)}
.color-pick.active::after{content:"";position:absolute;inset:-6px;border-radius:50%;border:2.5px solid currentColor}

.day-picker{display:flex;gap:7px;justify-content:space-between}
.day-pick{flex:1;height:44px;border-radius:13px;border:0;background:var(--fill);color:var(--label-2);font-family:inherit;font-size:13px;font-weight:600;cursor:pointer;transition:all .22s}
.day-pick.active{background:var(--blue);color:#fff}

.btn-primary{
  width:100%;height:52px;border:0;border-radius:16px;
  background:var(--blue);color:#fff;
  font-family:inherit;font-size:16px;font-weight:600;letter-spacing:-.25px;
  cursor:pointer;
  transition:transform .16s ease, opacity .16s;
  display:flex;align-items:center;justify-content:center;gap:8px;
}
.btn-primary:active{transform:scale(.975);opacity:.92}
.btn-primary:disabled{opacity:.5;cursor:default}

/* 详情 */
.detail-hero{position:relative;padding:30px 22px 24px;color:#fff;border-radius:0 0 26px 26px;overflow:hidden}
.detail-hero::after{content:"";position:absolute;inset:0;background:linear-gradient(180deg,rgba(255,255,255,.2),rgba(255,255,255,0) 50%);pointer-events:none}
.detail-hero .emoji{font-size:54px;line-height:1;display:block;margin-bottom:12px;position:relative;z-index:1}
.detail-hero h2{margin:0;font-size:25px;font-weight:700;letter-spacing:-.6px;position:relative;z-index:1}
.detail-hero p{margin:7px 0 0;font-size:13px;opacity:.88;position:relative;z-index:1;font-weight:500}

.calendar{display:grid;grid-template-columns:repeat(7,1fr);gap:6px;margin-top:6px}
.cal-head{font-size:10.5px;color:var(--label-3);text-align:center;font-weight:600;padding-bottom:4px}
.cal-day{aspect-ratio:1/1;border-radius:11px;background:var(--fill);display:grid;place-items:center;font-size:12px;color:var(--label-3);font-weight:500;position:relative}
.cal-day.on{background:var(--green);color:#fff;font-weight:700}
.cal-day.today{box-shadow:0 0 0 2px var(--blue)}
.cal-day.empty-cal{background:transparent}

.timeline{position:relative;padding-left:28px}
.timeline::before{content:"";position:absolute;left:9px;top:8px;bottom:8px;width:1.5px;background:var(--sep-2)}
.tl-item{position:relative;padding-bottom:20px}
.tl-item:last-child{padding-bottom:0}
.tl-item::before{content:"";position:absolute;left:-24px;top:5px;width:10px;height:10px;border-radius:50%;background:var(--green);box-shadow:0 0 0 3.5px var(--card)}
.tl-item .time{font-size:12px;color:var(--label-2);font-weight:500}
.tl-item .text{font-size:13.5px;font-weight:600;margin-top:3px}

.danger-btn{
  width:100%;height:52px;border:0;border-radius:16px;
  background:var(--card);color:var(--red);
  font-family:inherit;font-size:15.5px;font-weight:600;
  cursor:pointer;box-shadow:var(--shadow);
  margin-top:24px;
}

/* 设置 */
.setting-item{display:flex;align-items:center;gap:13px;padding:14px 16px;position:relative;cursor:pointer;transition:background .18s}
.setting-item + .setting-item::before{content:"";position:absolute;left:16px;right:0;top:0;height:.5px;background:var(--sep)}
.setting-item:active{background:var(--fill-2)}
.setting-text{flex:1;font-size:14.5px;font-weight:500}
.setting-val{font-size:13.5px;color:var(--label-2);font-weight:500}

.switch{width:51px;height:31px;border-radius:16px;background:var(--fill);position:relative;flex:0 0 auto;transition:background .3s;cursor:pointer}
.switch::after{content:"";position:absolute;top:2px;left:2px;width:27px;height:27px;border-radius:50%;background:#fff;box-shadow:0 3px 8px rgba(0,0,0,.15);transition:transform .3s}
.switch.on{background:var(--green)}
.switch.on::after{transform:translateX(20px)}

/* TabBar */
.tabbar{
  position:absolute;left:14px;right:14px;
  bottom:calc(12px + env(safe-area-inset-bottom));
  z-index:60;
  display:flex;align-items:center;
  padding:8px 10px;border-radius:40px;
  background:var(--tabbar-bg);
  border:0.5px solid var(--tabbar-border);
  backdrop-filter:blur(28px) saturate(200%);
  -webkit-backdrop-filter:blur(28px) saturate(200%);
  box-shadow:var(--tabbar-shadow);overflow:hidden;
  transition:transform .35s cubic-bezier(.32,.72,0,1), opacity .3s;
}
.tabbar.hidden{transform:translateY(120%);opacity:0;pointer-events:none}
.tabbar::before{content:"";position:absolute;inset:0;border-radius:inherit;background:linear-gradient(180deg, rgba(255,255,255,.6), rgba(255,255,255,.1) 46%, rgba(255,255,255,0) 72%);pointer-events:none;mix-blend-mode:overlay}
.tab{
  flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;
  padding:6px 0 5px;border:0;background:none;
  color:var(--label-3);font-family:inherit;font-size:10px;font-weight:600;
  cursor:pointer;position:relative;border-radius:24px;
  transition:color .24s, background .3s, transform .18s;
}
.tab:active{transform:scale(.9)}
.tab.active{color:var(--blue);background:rgba(0,122,255,.11)}
.tab svg{width:25px;height:25px;display:block;position:relative;z-index:1}
.tab span:not(.badge){position:relative;z-index:1}

.tab-add{
  flex:0 0 auto;width:54px;height:54px;border-radius:50%;
  border:0;background:linear-gradient(135deg,#0A84FF,#5E5CE6);
  color:#fff;display:grid;place-items:center;cursor:pointer;margin:0 5px;
  box-shadow:0 12px 24px -10px rgba(10,132,255,.95);
  transition:transform .24s;
}
.tab-add:active{transform:scale(.88)}
.tab-add svg{width:26px;height:26px}

/* Toast */
.toast{
  position:fixed;left:50%;
  bottom:calc(122px + env(safe-area-inset-bottom));
  transform:translate(-50%,16px);
  padding:11px 22px;border-radius:26px;
  background:rgba(0,0,0,.85);color:#fff;
  font-size:14px;font-weight:600;white-space:nowrap;
  opacity:0;pointer-events:none;z-index:999;
  transition:opacity .3s ease, transform .3s;
  backdrop-filter:blur(16px);
}
.toast.show{opacity:1;transform:translate(-50%,0)}

/* 撒花 */
.confetti{position:fixed;inset:0;pointer-events:none;z-index:998;overflow:hidden}
.confetti i{position:absolute;width:9px;height:9px;border-radius:2px;animation:fall 1.6s cubic-bezier(.3,.7,.5,1) forwards}
@keyframes fall{0%{transform:translateY(0) rotate(0) scale(1);opacity:1}100%{transform:translateY(105vh) rotate(620deg) scale(.6);opacity:0}}

/* 庆祝 */
.celebrate{position:fixed;inset:0;z-index:997;display:flex;align-items:center;justify-content:center;pointer-events:none;opacity:0;transition:opacity .35s}
.celebrate.show{opacity:1}
.celebrate .card{background:var(--card);border-radius:26px;padding:28px 30px;box-shadow:0 24px 60px -20px rgba(0,0,0,.5);text-align:center;transform:scale(.85);transition:transform .45s cubic-bezier(.34,1.56,.64,1)}
.celebrate.show .card{transform:scale(1)}
.celebrate .icon{font-size:52px;margin-bottom:14px;animation:bounce 1s ease infinite}
@keyframes bounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
.celebrate h3{margin:0 0 8px;font-size:20px;font-weight:700}
.celebrate p{margin:0;font-size:13.5px;color:var(--label-2)}

/* 边缘提示 */
.edge-hint{position:fixed;left:0;top:50%;transform:translateY(-50%);width:4px;height:60px;border-radius:0 4px 4px 0;background:var(--blue);opacity:0;z-index:996;pointer-events:none;transition:opacity .15s}
.edge-hint.show{opacity:.7}

/* 登录页 */
#page-login{background:var(--bg);z-index:80}
#page-login.active{display:flex;animation:loginIn .5s cubic-bezier(.22,1,.36,1) both}
@keyframes loginIn{from{opacity:0;transform:scale(.98)}to{opacity:1;transform:scale(1)}}

.login-wrap{
  flex:1;display:flex;flex-direction:column;
  padding:20px 24px calc(30px + env(safe-area-inset-bottom));
  overflow-y:auto;-webkit-overflow-scrolling:touch;
}
.login-brand{
  text-align:center;
  padding:24px 0 28px;
  display:flex;flex-direction:column;align-items:center;gap:12px;
}
.login-logo{
  width:76px;height:76px;border-radius:22px;
  background:linear-gradient(135deg,#0A84FF,#5E5CE6);
  display:grid;place-items:center;color:#fff;
  box-shadow:0 18px 38px -16px rgba(10,132,255,.95);
}
.login-logo svg{width:40px;height:40px}
.login-brand h1{margin:0;font-size:24px;font-weight:700;letter-spacing:-.6px}
.login-brand p{margin:0;font-size:13.5px;color:var(--label-2)}

.seg{display:flex;background:var(--fill);border-radius:12px;padding:3px;margin-bottom:20px}
.seg button{
  flex:1;border:0;background:none;border-radius:9px;
  height:34px;font-family:inherit;font-size:13.5px;font-weight:600;
  color:var(--label-2);cursor:pointer;transition:all .24s;
}
.seg button.active{background:var(--card);color:var(--label);box-shadow:0 1px 4px rgba(0,0,0,.1)}

.divider{display:flex;align-items:center;gap:12px;margin:24px 0 18px;color:var(--label-3);font-size:12px}
.divider::before,.divider::after{content:"";flex:1;height:.5px;background:var(--sep-2)}

.social{display:flex;flex-direction:column;gap:12px}
.social-btn{
  position:relative;width:100%;height:52px;
  border:0;border-radius:16px;
  display:flex;align-items:center;justify-content:center;gap:10px;
  font-family:inherit;font-size:15.5px;font-weight:600;
  cursor:pointer;
  transition:transform .16s ease, opacity .16s;
  overflow:hidden;
}
.social-btn:active{transform:scale(.975)}
.social-btn .logo{width:22px;height:22px;flex:0 0 auto;display:grid;place-items:center}
.social-btn .logo svg{width:22px;height:22px;display:block}

.social-btn.google{background:var(--card);color:var(--label);box-shadow:var(--shadow);border:0.5px solid var(--sep-2)}
.social-btn.google .logo{width:20px;height:20px;border-radius:5px;background:#fff;padding:1px}
.social-btn.google .logo svg{width:18px;height:18px}

.social-btn.facebook{background:var(--fb-blue);color:#fff}
.social-btn.facebook .logo{width:24px;height:24px;border-radius:50%;background:#fff;display:grid;place-items:center}
.social-btn.facebook .logo svg{width:16px;height:16px}

.social-btn.apple{background:var(--apple-black);color:var(--bg)}
@media (prefers-color-scheme: dark){
  .social-btn.apple{background:#FFFFFF;color:#000000}
  .social-btn.apple .logo svg path{fill:#000000}
}
.social-btn.apple .logo svg{width:19px;height:19px}

.social-btn.loading,.btn-primary.loading{pointer-events:none;opacity:.7}
.spinner{width:18px;height:18px;border-radius:50%;border:2px solid rgba(255,255,255,.35);border-top-color:#fff;animation:spin .7s linear infinite}
.social-btn.google .spinner{border-color:rgba(0,0,0,.15);border-top-color:#333}
@keyframes spin{to{transform:rotate(360deg)}}

.terms{display:flex;align-items:flex-start;gap:8px;margin-top:18px;padding:0 2px;font-size:12px;color:var(--label-2);line-height:1.5}
.terms input{width:16px;height:16px;accent-color:var(--blue);margin:1px 0 0;flex:0 0 auto;cursor:pointer}
.terms a{color:var(--blue);text-decoration:none}

.login-foot{margin-top:auto;padding-top:24px;text-align:center}
.login-foot p{margin:0;font-size:12px;color:var(--label-3);line-height:1.6}

/* 弹层 */
.sheet-mask{
  position:absolute;inset:0;z-index:120;
  background:rgba(0,0,0,.4);
  opacity:0;pointer-events:none;transition:opacity .3s;
  backdrop-filter:blur(2px);
}
.sheet-mask.show{opacity:1;pointer-events:auto}
.sheet{
  position:absolute;left:0;right:0;bottom:0;z-index:130;
  background:var(--card);
  border-radius:24px 24px 0 0;
  padding:10px 20px calc(24px + env(safe-area-inset-bottom));
  transform:translateY(100%);
  transition:transform .36s cubic-bezier(.22,1,.36,1);
  max-height:80%;overflow-y:auto;
}
.sheet.show{transform:translateY(0)}
.sheet-bar{width:38px;height:5px;border-radius:3px;background:var(--fill);margin:0 auto 16px}
.sheet h3{margin:0 0 12px;font-size:18px;font-weight:700}
.sheet p{margin:0 0 12px;font-size:13.5px;color:var(--label-2);line-height:1.65}
.sheet .sheet-actions{display:flex;flex-direction:column;gap:10px;margin-top:16px}
.sheet .sheet-actions button{
  width:100%;height:50px;border:0;border-radius:14px;
  font-family:inherit;font-size:16px;font-weight:600;cursor:pointer;
  transition:transform .16s;
}
.sheet .sheet-actions button:active{transform:scale(.975)}
.sheet .sheet-actions .confirm{background:var(--red);color:#fff}
.sheet .sheet-actions .cancel{background:var(--fill);color:var(--label)}
</style>
</head>
<body>

<div class="app" id="app">

  <!-- ============ 登录页 ============ -->
  <section class="page" id="page-login">
    <div class="login-wrap">
      <div class="login-brand">
        <div class="login-logo">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 6 9 17l-5-5"/>
          </svg>
        </div>
        <h1>欢迎回来</h1>
        <p>坚持打卡 · 遇见更好的自己</p>
      </div>

      <div class="seg" id="loginSeg">
        <button class="active" data-mode="pwd">密码登录</button>
        <button data-mode="code">验证码登录</button>
      </div>

      <div id="formPwd">
        <div class="field">
          <label>手机号 / 邮箱</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3" y="5.5" width="18" height="13" rx="3"/>
              <path d="m3.6 7.4 7.5 5.2a1.6 1.6 0 0 0 1.8 0l7.5-5.2"/>
            </svg>
            <input type="text" id="pwdAccount" placeholder="请输入手机号或邮箱" autocomplete="username">
          </div>
        </div>

        <div class="field">
          <label>密码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="4.5" y="10.5" width="15" height="10" rx="2.6"/>
              <path d="M8 10.5V8a4 4 0 0 1 8 0v2.5"/>
            </svg>
            <input type="password" id="pwdValue" placeholder="请输入密码" autocomplete="current-password">
            <button class="eye" data-eye="pwdValue" aria-label="显示密码">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
                <path d="M2.5 12S6 5.8 12 5.8 21.5 12 21.5 12 18 18.2 12 18.2 2.5 12 2.5 12Z"/>
                <circle cx="12" cy="12" r="3"/>
              </svg>
            </button>
          </div>
        </div>

        <div class="field-row">
          <label><input type="checkbox" id="rememberMe">记住我</label>
          <a href="#" data-forgot>忘记密码？</a>
        </div>

        <button class="btn-primary" id="btnPwdLogin">登 录</button>
      </div>

      <div id="formCode" style="display:none">
        <div class="field">
          <label>手机号</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="6.5" y="2.5" width="11" height="19" rx="3"/>
              <path d="M10.5 18.5h3"/>
            </svg>
            <input type="tel" id="codePhone" placeholder="请输入手机号" maxlength="11">
          </div>
        </div>

        <div class="field">
          <label>验证码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3.5" y="5" width="17" height="14" rx="3"/>
              <path d="M8 12h8M8 9h5M8 15h6"/>
            </svg>
            <input type="tel" id="codeValue" placeholder="6 位验证码" maxlength="6">
            <button class="code-btn" id="btnSendCode">获取验证码</button>
          </div>
        </div>

        <div style="height:8px"></div>
        <button class="btn-primary" id="btnCodeLogin">登 录</button>
      </div>

      <div class="divider">或使用以下方式登录</div>

      <div class="social">
        <button class="social-btn google" data-social="google">
          <span class="logo">
            <svg viewBox="0 0 48 48" xmlns="http://www.w3.org/2000/svg">
              <path fill="#4285F4" d="M45.12 24.5c0-1.56-.14-3.06-.4-4.5H24v8.51h11.84c-.51 2.75-2.06 5.08-4.39 6.64v5.52h7.11c4.16-3.83 6.56-9.47 6.56-16.17z"/>
              <path fill="#34A853" d="M24 46c5.94 0 10.92-1.97 14.56-5.33l-7.11-5.52c-1.97 1.32-4.49 2.1-7.45 2.1-5.73 0-10.58-3.87-12.31-9.07H4.34v5.7C7.96 41.07 15.4 46 24 46z"/>
              <path fill="#FBBC05" d="M11.69 28.18C11.25 26.86 11 25.45 11 24s.25-2.86.69-4.18v-5.7H4.34C2.85 17.09 2 20.45 2 24s.85 6.91 2.34 9.88l7.35-5.7z"/>
              <path fill="#EA4335" d="M24 10.75c3.23 0 6.13 1.11 8.41 3.29l6.31-6.31C34.91 4.18 29.93 2 24 2 15.4 2 7.96 6.93 4.34 14.12l7.35 5.7c1.73-5.2 6.58-9.07 12.31-9.07z"/>
            </svg>
          </span>
          <span class="label-text">使用 Google 登录</span>
        </button>

        <button class="social-btn facebook" data-social="facebook">
          <span class="logo">
            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path fill="#1877F2" d="M24 12c0-6.63-5.37-12-12-12S0 5.37 0 12c0 5.99 4.39 10.95 10.13 11.85v-8.38H7.08V12h3.05V9.36c0-3.01 1.79-4.67 4.53-4.67 1.31 0 2.69.23 2.69.23v2.96h-1.52c-1.49 0-1.96.93-1.96 1.88V12h3.33l-.53 3.47h-2.8v8.38C19.61 22.95 24 17.99 24 12z"/>
            </svg>
          </span>
          <span class="label-text">使用 Facebook 登录</span>
        </button>

        <button class="social-btn apple" data-social="apple">
          <span class="logo">
            <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
              <path fill="#FFFFFF" d="M16.37 12.72c-.02-2.42 1.98-3.58 2.07-3.64-1.13-1.65-2.89-1.88-3.52-1.9-1.5-.15-2.92.88-3.68.88-.76 0-1.93-.86-3.17-.83-1.63.02-3.13.95-3.97 2.4-1.69 2.93-.43 7.27 1.22 9.65.81 1.17 1.77 2.48 3.04 2.43 1.22-.05 1.68-.79 3.16-.79 1.48 0 1.89.79 3.18.77 1.31-.02 2.14-1.19 2.94-2.36.93-1.35 1.31-2.66 1.33-2.73-.03-.01-2.55-.98-2.6-3.88zM13.99 5.62c.67-.81 1.12-1.94.99-3.06-.96.04-2.12.64-2.81 1.45-.62.72-1.16 1.87-1.01 2.97 1.07.08 2.16-.55 2.83-1.36z"/>
            </svg>
          </span>
          <span class="label-text">使用 Apple 登录</span>
        </button>
      </div>

      <div class="terms">
        <input type="checkbox" id="agreeTerms" checked>
        <span>我已阅读并同意 <a href="#" data-terms="user">《用户协议》</a> 和 <a href="#" data-terms="privacy">《隐私政策》</a></span>
      </div>

      <div style="text-align:center;margin-top:22px">
        <span style="font-size:13.5px;color:var(--label-2)">还没有账号？</span>
        <a href="#" id="btnToRegister" style="font-size:13.5px;color:var(--blue);font-weight:600;text-decoration:none;margin-left:4px">立即注册</a>
      </div>

      <div class="login-foot">
        <p>登录即代表你已满 18 周岁</p>
      </div>
    </div>
  </section>

  <!-- ============ 注册页 ============ -->
  <section class="page overlay" id="page-register">
    <nav class="ios-nav">
      <button class="nav-back" data-back>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="m15 5-7 7 7 7"/>
        </svg>
        <span>返回</span>
      </button>
      <div class="nav-title">创建账号</div>
      <div style="width:60px"></div>
    </nav>

    <div class="scroll">
      <div class="form-wrap">
        <div class="field">
          <label>手机号</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="6.5" y="2.5" width="11" height="19" rx="3"/>
              <path d="M10.5 18.5h3"/>
            </svg>
            <input type="tel" id="regPhone" placeholder="请输入手机号" maxlength="11">
          </div>
        </div>

        <div class="field">
          <label>验证码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3.5" y="5" width="17" height="14" rx="3"/>
              <path d="M8 12h8M8 9h5M8 15h6"/>
            </svg>
            <input type="tel" id="regCode" placeholder="6 位验证码" maxlength="6">
            <button class="code-btn" id="btnRegCode">获取验证码</button>
          </div>
        </div>

        <div class="field">
          <label>设置密码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="4.5" y="10.5" width="15" height="10" rx="2.6"/>
              <path d="M8 10.5V8a4 4 0 0 1 8 0v2.5"/>
            </svg>
            <input type="password" id="regPwd" placeholder="8-20 位，含字母和数字">
          </div>
        </div>

        <div class="field">
          <label>确认密码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="4.5" y="10.5" width="15" height="10" rx="2.6"/>
              <path d="M8 10.5V8a4 4 0 0 1 8 0v2.5"/>
            </svg>
            <input type="password" id="regPwd2" placeholder="请再次输入密码">
          </div>
        </div>

        <div class="terms" style="margin-top:6px">
          <input type="checkbox" id="regAgree" checked>
          <span>我已阅读并同意 <a href="#" data-terms="user">《用户协议》</a> 和 <a href="#" data-terms="privacy">《隐私政策》</a></span>
        </div>

        <div style="height:20px"></div>
        <button class="btn-primary" id="btnRegister">注 册</button>
      </div>
    </div>
  </section>

  <!-- ============ 忘记密码页 ============ -->
  <section class="page overlay" id="page-forgot">
    <nav class="ios-nav">
      <button class="nav-back" data-back>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="m15 5-7 7 7 7"/>
        </svg>
        <span>返回</span>
      </button>
      <div class="nav-title">重置密码</div>
      <div style="width:60px"></div>
    </nav>

    <div class="scroll">
      <div class="form-wrap">
        <div class="field">
          <label>手机号</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="6.5" y="2.5" width="11" height="19" rx="3"/>
              <path d="M10.5 18.5h3"/>
            </svg>
            <input type="tel" id="forgotPhone" placeholder="请输入手机号" maxlength="11">
          </div>
        </div>

        <div class="field">
          <label>验证码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="3.5" y="5" width="17" height="14" rx="3"/>
              <path d="M8 12h8M8 9h5M8 15h6"/>
            </svg>
            <input type="tel" id="forgotCode" placeholder="6 位验证码" maxlength="6">
            <button class="code-btn" id="btnForgotCode">获取验证码</button>
          </div>
        </div>

        <div class="field">
          <label>新密码</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <rect x="4.5" y="10.5" width="15" height="10" rx="2.6"/>
              <path d="M8 10.5V8a4 4 0 0 1 8 0v2.5"/>
            </svg>
            <input type="password" id="forgotPwd" placeholder="8-20 位，含字母和数字">
          </div>
        </div>

        <div style="height:20px"></div>
        <button class="btn-primary" id="btnReset">确认重置</button>
      </div>
    </div>
  </section>

  <!-- ============ 今日 ============ -->
  <section class="page" id="page-home">
    <header class="header">
      <div class="header-row">
        <div style="flex:1">
          <h1 id="greetTitle">早上好</h1>
          <div class="sub" id="todayDate"></div>
        </div>
        <button class="icon-btn" data-toast="通知中心" aria-label="通知">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
            <path d="M18 8.5a6 6 0 1 0-12 0c0 6-2.5 7.5-2.5 7.5h17S18 14.5 18 8.5Z"/>
            <path d="M10.3 19.5a2 2 0 0 0 3.4 0"/>
          </svg>
        </button>
      </div>
    </header>

    <div class="scroll">
      <section class="hero">
        <div class="hero-top">
          <div class="hero-date" id="heroDate"></div>
          <div class="hero-date" id="heroWeek"></div>
        </div>
        <div class="hero-progress">
          <span class="big" id="heroDone">0</span>
          <span class="unit">/ <span id="heroTotal">0</span> 已完成</span>
        </div>
        <div class="hero-bar"><i id="heroBar" style="width:0%"></i></div>
        <div class="hero-meta">
          <span id="heroTip">开始你的第一个打卡吧</span>
          <span id="heroPct">0%</span>
        </div>
        <div class="deco deco-1"></div>
        <div class="deco deco-2"></div>
      </section>

      <div class="section-head">
        <h3>今日习惯</h3>
        <span id="habitCount"></span>
      </div>

      <div class="habit-list" id="habitList"></div>
    </div>
  </section>

  <!-- ============ 统计 ============ -->
  <section class="page" id="page-stats">
    <header class="header">
      <div class="header-row"><h1>统计</h1></div>
      <div class="chips" id="statChips"></div>
    </header>

    <div class="scroll">
      <div class="block" style="margin-top:12px">
        <div class="block-head"><h3>完成率</h3><span class="more" id="rangeLabel"></span></div>
        <div class="ring-wrap">
          <div class="ring">
            <svg width="116" height="116" viewBox="0 0 116 116">
              <circle cx="58" cy="58" r="49" fill="none" stroke="var(--fill)" stroke-width="12"/>
              <circle id="ringArc" cx="58" cy="58" r="49" fill="none" stroke="url(#ringGrad)" stroke-width="12"
                stroke-linecap="round" stroke-dasharray="307.9" stroke-dashoffset="307.9"/>
              <defs>
                <linearGradient id="ringGrad" x1="0" y1="0" x2="1" y2="1">
                  <stop offset="0%" stop-color="#0A84FF"/>
                  <stop offset="100%" stop-color="#5E5CE6"/>
                </linearGradient>
              </defs>
            </svg>
            <div class="center">
              <div class="p" id="ringPct">0%</div>
              <div class="t">完成率</div>
            </div>
          </div>
          <div class="ring-info">
            <div class="row"><i style="background:var(--green)"></i><span>已完成</span><b id="ringDone">0 次</b></div>
            <div class="row"><i style="background:var(--fill)"></i><span>未完成</span><b id="ringMiss">0 次</b></div>
            <div class="row"><i style="background:var(--orange)"></i><span>连续天数</span><b id="ringStreak">0 天</b></div>
          </div>
        </div>
      </div>

      <div class="block">
        <div class="block-head"><h3>近 7 天打卡</h3><span class="more" id="chartTotal"></span></div>
        <div class="chart" id="chart"></div>
      </div>

      <div class="stat-grid">
        <div class="stat-card" style="--stat-color:var(--blue)">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg>
          </div>
          <div class="num" id="sTotal">0<small>次</small></div>
          <div class="lbl">累计打卡</div>
        </div>
        <div class="stat-card" style="--stat-color:var(--orange)">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M12 2.5c1.8 3.2 4.5 5 4.5 8.5a4.5 4.5 0 1 1-9 0c0-1.6.6-2.9 1.6-4.2"/>
            </svg>
          </div>
          <div class="num" id="sStreak">0<small>天</small></div>
          <div class="lbl">当前连续</div>
        </div>
        <div class="stat-card" style="--stat-color:var(--purple)">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 3 4 7v6c0 5 3.4 8.3 8 9 4.6-.7 8-4 8-9V7l-8-4Z"/></svg>
          </div>
          <div class="num" id="sBest">0<small>天</small></div>
          <div class="lbl">最长连续</div>
        </div>
        <div class="stat-card" style="--stat-color:var(--green)">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="8.5"/><path d="M12 7.5V12l3 1.8"/>
            </svg>
          </div>
          <div class="num" id="sRate">0<small>%</small></div>
          <div class="lbl">平均完成率</div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ 我的 ============ -->
  <section class="page" id="page-me">
    <div class="scroll">
      <header class="header">
        <div class="header-row">
          <h1>我的</h1>
          <button class="icon-btn" data-open="settings" aria-label="设置">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="3.2"/>
              <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 1 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 1 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 1 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9c.14.36.4.66.74.85.3.17.64.26.98.26H21a2 2 0 1 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1Z"/>
            </svg>
          </button>
        </div>
      </header>

      <div class="profile">
        <div class="profile-card" data-toast="编辑资料">
          <div class="avatar" id="meAvatar">早</div>
          <div class="profile-info">
            <h2 id="meName">早起达人</h2>
            <p id="meSub">坚持打卡 · 遇见更好的自己</p>
          </div>
          <span class="level-tag" id="levelTag">Lv.6</span>
        </div>

        <div class="stats">
          <div class="stat"><div class="num" id="meTotal">0</div><div class="lbl">累计打卡</div></div>
          <div class="stat"><div class="num" id="meStreak">0</div><div class="lbl">连续天数</div></div>
          <div class="stat"><div class="num" id="meHabits">0</div><div class="lbl">习惯数</div></div>
        </div>

        <div class="group-title">我的习惯</div>
        <div class="list" id="meHabitList"></div>

        <div class="group-title">更多</div>
        <div class="list">
          <div class="list-item" data-toast="成就徽章">
            <span class="list-ico y">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="9" r="5.5"/><path d="m8.5 13.5-2 8L12 19l5.5 2.5-2-8"/>
              </svg>
            </span>
            <span class="list-text">成就徽章</span>
            <span class="list-val">8 / 24</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-toast="提醒设置">
            <span class="list-ico o">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="13" r="7.5"/><path d="M12 10v3.5l2.5 1.5M9 3h6"/>
              </svg>
            </span>
            <span class="list-text">提醒设置</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-open="settings">
            <span class="list-ico pk">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M4 6h16M4 12h16M4 18h10"/>
              </svg>
            </span>
            <span class="list-text">设置</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-logout>
            <span class="list-ico r">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M15 4.5H6.5a2 2 0 0 0-2 2v11a2 2 0 0 0 2 2H15"/>
                <path d="M19 12H9.5M16 8.5 19.5 12 16 15.5"/>
              </svg>
            </span>
            <span class="list-text" style="color:var(--red)">退出登录</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ 新建 ============ -->
  <section class="page overlay" id="page-new">
    <nav class="ios-nav">
      <button class="nav-back" data-back>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="m15 5-7 7 7 7"/>
        </svg>
        <span>返回</span>
      </button>
      <div class="nav-title">新建习惯</div>
      <button class="nav-right" id="navCreate">完成</button>
    </nav>

    <div class="scroll">
      <div class="form-wrap">
        <div class="field">
          <label>习惯名称</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
              <path d="M4 6.5h16M4 12h10M4 17.5h7"/>
            </svg>
            <input type="text" id="newName" placeholder="例如：喝水、阅读、跑步" maxlength="12">
          </div>
        </div>

        <div class="field">
          <label>选择图标</label>
          <div class="emoji-picker" id="emojiPicker"></div>
        </div>

        <div class="field">
          <label>选择颜色</label>
          <div class="color-picker" id="colorPicker"></div>
        </div>

        <div class="field">
          <label>重复周期</label>
          <div class="day-picker" id="dayPicker"></div>
        </div>

        <div class="field">
          <label>每日目标次数</label>
          <div class="field-box">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="8.5"/><path d="M12 8v4l3 2"/>
            </svg>
            <input type="tel" id="newGoal" value="1" maxlength="2">
          </div>
        </div>

        <button class="btn-primary" id="btnCreate" style="margin-top:28px">创建习惯</button>
      </div>
    </div>
  </section>

  <!-- ============ 详情 ============ -->
  <section class="page overlay" id="page-habit">
    <nav class="ios-nav">
      <button class="nav-back" data-back>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="m15 5-7 7 7 7"/>
        </svg>
        <span>返回</span>
      </button>
      <div class="nav-title" id="habitNavTitle">习惯详情</div>
      <button class="nav-right" id="navHabitEdit">编辑</button>
    </nav>
    <div class="scroll" id="habitScroll"></div>
  </section>

  <!-- ============ 设置 ============ -->
  <section class="page overlay" id="page-settings">
    <nav class="ios-nav">
      <button class="nav-back" data-back>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="m15 5-7 7 7 7"/>
        </svg>
        <span>返回</span>
      </button>
      <div class="nav-title">设置</div>
      <div style="width:60px"></div>
    </nav>
    <div class="scroll">
      <div style="padding:0 16px">
        <div class="group-title">账号</div>
        <div class="list">
          <div class="setting-item" data-toast="个人资料"><span class="setting-text">个人资料</span><span class="setting-val" id="setAccountName">早起达人</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item" data-toast="已绑定 2 个第三方账号"><span class="setting-text">第三方账号</span><span class="setting-val" id="setProvider">密码登录</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
        </div>

        <div class="group-title">通用</div>
        <div class="list">
          <div class="setting-item"><span class="setting-text">每日提醒</span><div class="switch on" data-switch></div></div>
          <div class="setting-item"><span class="setting-text">提醒时间</span><span class="setting-val">08:00</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item"><span class="setting-text">打卡音效</span><div class="switch on" data-switch></div></div>
          <div class="setting-item"><span class="setting-text">振动反馈</span><div class="switch on" data-switch></div></div>
        </div>

        <div class="group-title">关于</div>
        <div class="list">
          <div class="setting-item" data-toast="版本 1.0.0"><span class="setting-text">版本</span><span class="setting-val">1.0.0</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
        </div>

        <button class="danger-btn" data-logout style="margin-top:22px">退出登录</button>
      </div>
    </div>
  </section>

  <!-- 玻璃质感悬浮 Tab -->
  <nav class="tabbar hidden" id="tabbar">
    <button class="tab active" data-tab="home">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
        <path d="M3.2 10.6 12 3.4l8.8 7.2"/>
        <path d="M5.6 9.4V20a1 1 0 0 0 1 1h10.8a1 1 0 0 0 1-1V9.4"/>
      </svg>
      <span>今日</span>
    </button>
    <button class="tab" data-tab="stats">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
        <path d="M4 20V10M10 20V4M16 20v-7M22 20H2"/>
      </svg>
      <span>统计</span>
    </button>

    <button class="tab-add" data-open="new" aria-label="新建习惯">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round">
        <path d="M12 5v14M5 12h14"/>
      </svg>
    </button>

    <button class="tab" data-tab="me">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round">
        <circle cx="12" cy="8.4" r="3.6"/>
        <path d="M4.8 20a7.2 7.2 0 0 1 14.4 0"/>
      </svg>
      <span>我的</span>
    </button>
  </nav>

  <div class="toast" id="toast"></div>
  <div class="confetti" id="confetti"></div>
  <div class="celebrate" id="celebrate">
    <div class="card">
      <div class="icon" id="celIcon">🎉</div>
      <h3 id="celTitle">全部完成</h3>
      <p id="celText">今天的所有习惯都已打卡</p>
    </div>
  </div>
  <div class="edge-hint" id="edgeHint"></div>

  <div class="sheet-mask" id="sheetMask"></div>
  <div class="sheet" id="sheet">
    <div class="sheet-bar"></div>
    <h3 id="sheetTitle">用户协议</h3>
    <div id="sheetBody"></div>
    <div class="sheet-actions">
      <button class="cancel" id="sheetClose">我知道了</button>
    </div>
  </div>

  <div class="sheet-mask" id="logoutMask"></div>
  <div class="sheet" id="logoutSheet">
    <div class="sheet-bar"></div>
    <h3>确认退出登录？</h3>
    <p>退出后需要重新登录才能继续打卡，你的打卡数据会保留。</p>
    <div class="sheet-actions">
      <button class="confirm" id="confirmLogout">退出登录</button>
      <button class="cancel" id="cancelLogout">取消</button>
    </div>
  </div>
</div>

<script>
(function () {
  'use strict';

  var $ = function(id){ return document.getElementById(id); };
  var app = $('app');
  var tabbar = $('tabbar'), toastEl = $('toast');

  var currentUser = null;

  var COLORS = [
    { id: 'blue',   val: '#0A84FF' },
    { id: 'green',  val: '#34C759' },
    { id: 'orange', val: '#FF9500' },
    { id: 'purple', val: '#AF52DE' },
    { id: 'pink',   val: '#FF2D55' },
    { id: 'teal',   val: '#5AC8FA' },
    { id: 'indigo', val: '#5856D6' },
    { id: 'yellow', val: '#FFCC00' }
  ];
  var EMOJIS = ['💧','📖','🏃','🧘','🥗','😴','✍️','🎯','🚴','🎸','🧹','💊','🌱','☀️','🏋️','🎨'];
  var DAY_LABELS = ['日','一','二','三','四','五','六'];

  function pad(n){ return String(n).padStart(2,'0'); }
  function fmtDate(d){ return d.getFullYear()+'-'+pad(d.getMonth()+1)+'-'+pad(d.getDate()); }
  var TODAY = fmtDate(new Date());

  function dateAdd(dateStr, n){
    var d = new Date(dateStr+'T00:00:00');
    d.setDate(d.getDate()+n);
    return fmtDate(d);
  }
  function weekdayOf(dateStr){ return new Date(dateStr+'T00:00:00').getDay(); }

  function pastDays(n){
    var arr = [];
    for (var k=n-1; k>=0; k--) arr.push(dateAdd(TODAY,-k));
    return arr;
  }
  var LAST_30 = pastDays(30);
  function seedRecords(rate){ return LAST_30.filter(function(){ return Math.random()<rate; }); }

  var habits = [
    { id:1, name:'早起', emoji:'☀️', color:'#FF9500', goal:1, days:[1,2,3,4,5], createdAt:dateAdd(TODAY,-40) },
    { id:2, name:'喝水', emoji:'💧', color:'#5AC8FA', goal:8, days:[0,1,2,3,4,5,6], createdAt:dateAdd(TODAY,-40) },
    { id:3, name:'阅读', emoji:'📖', color:'#5856D6', goal:1, days:[0,1,2,3,4,5,6], createdAt:dateAdd(TODAY,-35) },
    { id:4, name:'运动', emoji:'🏃', color:'#34C759', goal:1, days:[1,3,5], createdAt:dateAdd(TODAY,-28) },
    { id:5, name:'冥想', emoji:'🧘', color:'#AF52DE', goal:1, days:[0,1,2,3,4,5,6], createdAt:dateAdd(TODAY,-20) }
  ];

  var records = {
    1: seedRecords(0.82),
    2: seedRecords(0.9),
    3: seedRecords(0.75),
    4: seedRecords(0.6),
    5: seedRecords(0.88)
  };

  ['1','3','4'].forEach(function(id){
    var arr = records[id];
    var i = arr.indexOf(TODAY);
    if (i>=0) arr.splice(i,1);
  });

  var nextId = 6;
  var statRange = 7;

  function isScheduled(habit, dateStr){ return habit.days.indexOf(weekdayOf(dateStr)) >= 0; }
  function isChecked(habitId, dateStr){ return (records[habitId]||[]).indexOf(dateStr) >= 0; }

  function currentStreak(habitId){
    var habit = habits.filter(function(h){ return h.id===habitId; })[0];
    if (!habit) return 0;
    var streak=0, d=TODAY, guard=0;
    if (!isChecked(habitId,d)) d=dateAdd(d,-1);
    while (guard++<400) {
      if (!isScheduled(habit,d)) { d=dateAdd(d,-1); continue; }
      if (isChecked(habitId,d)) { streak++; d=dateAdd(d,-1); }
      else break;
    }
    return streak;
  }

  function bestStreak(habitId){
    var list = (records[habitId]||[]).slice().sort();
    if (!list.length) return 0;
    var best=0, cur=0, prev=null;
    for (var i=0;i<list.length;i++){
      var d = list[i];
      if (prev && dateAdd(prev,1)===d) cur++;
      else cur=1;
      if (cur>best) best=cur;
      prev=d;
    }
    return best;
  }

  function totalChecks(){
    var s=0;
    for (var k in records) s += records[k].length;
    return s;
  }

  function globalStreak(){
    var streak=0, d=TODAY, guard=0;
    function anyChecked(ds){
      return habits.some(function(h){ return isScheduled(h,ds) && isChecked(h.id,ds); });
    }
    if (!anyChecked(d)) d=dateAdd(d,-1);
    while (anyChecked(d) && guard++<400) { streak++; d=dateAdd(d,-1); }
    return streak;
  }

  function todayStats(){
    var total=0, done=0;
    habits.forEach(function(h){
      if (!isScheduled(h,TODAY)) return;
      total++;
      if (isChecked(h.id,TODAY)) done++;
    });
    return { total:total, done:done };
  }

  var toastTimer;
  function showToast(msg){
    toastEl.textContent = msg;
    toastEl.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function(){ toastEl.classList.remove('show'); }, 1600);
  }

  function confetti(count){
    var box = $('confetti');
    var colors = ['#0A84FF','#34C759','#FF9500','#AF52DE','#FF2D55','#FFCC00','#5AC8FA','#00C7BE'];
    for (var i=0; i<(count||30); i++){
      var el = document.createElement('i');
      el.style.left = (5 + Math.random()*90) + '%';
      el.style.top = '-14px';
      el.style.background = colors[Math.floor(Math.random()*colors.length)];
      el.style.animationDelay = (Math.random()*0.28) + 's';
      el.style.animationDuration = (1.2 + Math.random()*0.7) + 's';
      if (Math.random()>0.5) el.style.borderRadius='50%';
      if (Math.random()>0.7) { el.style.width='6px'; el.style.height='12px'; }
      box.appendChild(el);
      (function(e){ setTimeout(function(){ e.remove(); }, 2600); })(el);
    }
  }

  var celTimer;
  function celebrate(icon, title, text){
    var box = $('celebrate');
    $('celIcon').textContent = icon;
    $('celTitle').textContent = title;
    $('celText').textContent = text;
    box.classList.add('show');
    clearTimeout(celTimer);
    celTimer = setTimeout(function(){ box.classList.remove('show'); }, 2000);
  }

  function renderHome(){
    var h = new Date().getHours();
    $('greetTitle').textContent = h<6?'夜深了':h<12?'早上好':h<18?'下午好':'晚上好';
    var now = new Date();
    var weekCn = ['日','一','二','三','四','五','六'];
    $('todayDate').textContent = (now.getMonth()+1)+' 月 '+now.getDate()+' 日 · 星期'+weekCn[now.getDay()];

    var st = todayStats();
    $('heroDate').textContent = (now.getMonth()+1)+'月'+now.getDate()+'日';
    $('heroWeek').textContent = '星期'+weekCn[now.getDay()];
    $('heroDone').textContent = st.done;
    $('heroTotal').textContent = st.total;
    var pct = st.total ? Math.round(st.done/st.total*100) : 0;
    $('heroBar').style.width = pct+'%';
    $('heroPct').textContent = pct+'%';
    $('heroTip').textContent = st.total===0 ? '今天没有安排打卡'
      : st.done===st.total ? '🎉 今日全部完成，太棒了！'
      : st.done===0 ? '开始你的第一个打卡吧'
      : '还差 '+(st.total-st.done)+' 个就全部完成';

    var list = $('habitList');
    if (!habits.length){
      list.innerHTML = '<div class="empty" style="padding:40px 30px 60px">'
        +'<div class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg></div>'
        +'<h4>还没有习惯</h4><p>点击下方加号，创建你的第一个打卡习惯</p></div>';
      $('habitCount').textContent = '';
      return;
    }
    $('habitCount').textContent = habits.length+' 个习惯';

    list.innerHTML = habits.map(function(h,i){
      var scheduled = isScheduled(h, TODAY);
      var checked = isChecked(h.id, TODAY);
      var streak = currentStreak(h.id);
      var dots = '';
      for (var k=6;k>=0;k--){
        var d = dateAdd(TODAY,-k);
        var on = isChecked(h.id,d);
        var sch = isScheduled(h,d);
        var isToday = d===TODAY;
        dots += '<i class="'+(on?'on':'')+' '+(isToday?'today':'')+'" style="'+(sch?'':'opacity:.3')+'"></i>';
      }
      return '<div class="habit" style="animation-delay:'+(Math.min(i,6)*50)+'ms;--habit-color:'+h.color+'" data-habit="'+h.id+'">'
        +'<div class="habit-ico" style="background:'+h.color+'1f;color:'+h.color+'"><span>'+h.emoji+'</span></div>'
        +'<div class="habit-body">'
          +'<div class="habit-name">'+h.name+(streak>1?'<span class="streak">🔥 '+streak+' 天</span>':'')+'</div>'
          +'<div class="habit-sub"><span>'+(h.days.length===7?'每天':h.days.map(function(d){return DAY_LABELS[d];}).join(' '))+'</span>'
            +(h.goal>1?'<span class="dot"></span><span>目标 '+h.goal+' 次</span>':'')
          +'</div>'
          +'<div class="habit-dots">'+dots+'</div>'
        +'</div>'
        +'<button class="check-btn '+(checked?'done':'')+'" data-check="'+h.id+'" '+(!scheduled?'disabled':'')+' aria-label="打卡">'
          +'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg>'
        +'</button>'
        +'<span class="habit-hint">长按删除</span>'
      +'</div>';
    }).join('');
  }

  function renderStatChips(){
    $('statChips').innerHTML = [7,14,30].map(function(n){
      return '<button class="chip'+(n===statRange?' active':'')+'" data-range="'+n+'">近 '+n+' 天</button>';
    }).join('');
    $('rangeLabel').textContent = '近 '+statRange+' 天';
  }

  function renderStats(){
    renderStatChips();
    var days = [];
    for (var k=statRange-1;k>=0;k--) days.push(dateAdd(TODAY,-k));

    var should=0, done=0;
    days.forEach(function(d){
      habits.forEach(function(h){
        if (!isScheduled(h,d) || h.createdAt>d) return;
        should++;
        if (isChecked(h.id,d)) done++;
      });
    });
    var pct = should ? Math.round(done/should*100) : 0;

    var C = 2*Math.PI*49;
    $('ringArc').style.strokeDashoffset = C - C*pct/100;
    $('ringPct').textContent = pct+'%';
    $('ringDone').textContent = done+' 次';
    $('ringMiss').textContent = (should-done)+' 次';
    $('ringStreak').textContent = globalStreak()+' 天';

    var weekDays = [];
    for (var k2=6;k2>=0;k2--) weekDays.push(dateAdd(TODAY,-k2));

    var weekTotal = 0;
    $('chart').innerHTML = weekDays.map(function(d){
      var t=0, dn=0;
      habits.forEach(function(h){
        if (!isScheduled(h,d) || h.createdAt>d) return;
        t++;
        if (isChecked(h.id,d)) dn++;
      });
      weekTotal += dn;
      var pctD = t ? dn/t : 0;
      var isToday = d===TODAY;
      var on = dn>0;
      return '<div class="chart-col">'
        +'<div class="chart-bar '+(on?'on':'')+' '+(isToday?'today':'')+'" style="height:'+Math.max(7,pctD*100)+'%">'+(on?'<span class="val">'+dn+'</span>':'')+'</div>'
        +'<div class="chart-day '+(on?'on':'')+' '+(isToday?'today':'')+'">'+DAY_LABELS[weekdayOf(d)]+'</div>'
      +'</div>';
    }).join('');
    $('chartTotal').textContent = '共 '+weekTotal+' 次';

    $('sTotal').innerHTML = totalChecks()+'<small>次</small>';
    $('sStreak').innerHTML = globalStreak()+'<small>天</small>';
    var best=0;
    habits.forEach(function(h){ best = Math.max(best, bestStreak(h.id)); });
    $('sBest').innerHTML = best+'<small>天</small>';
    $('sRate').innerHTML = pct+'<small>%</small>';
  }

  function renderMe(){
    $('meTotal').textContent = totalChecks();
    $('meStreak').textContent = globalStreak();
    $('meHabits').textContent = habits.length;
    $('levelTag').textContent = 'Lv.'+Math.max(1, Math.min(99, Math.floor(totalChecks()/12)+1));

    if (currentUser){
      $('meName').textContent = currentUser.name;
      $('meAvatar').textContent = currentUser.avatar;
      $('meSub').textContent = '通过 '+currentUser.provider+' 登录';
      $('setAccountName').textContent = currentUser.name;
      $('setProvider').textContent = currentUser.provider;
    }

    var list = $('meHabitList');
    if (!habits.length){
      list.innerHTML = '<div class="list-item" style="justify-content:center;color:var(--label-3)">还没有习惯</div>';
      return;
    }
    list.innerHTML = habits.map(function(h){
      var streak = currentStreak(h.id);
      return '<div class="list-item" data-habit="'+h.id+'">'
        +'<span class="list-ico" style="background:'+h.color+'22;color:'+h.color+';font-size:15px">'+h.emoji+'</span>'
        +'<span class="list-text">'+h.name+'</span>'
        +'<span class="list-val">'+(streak>0?'🔥 '+streak+' 天':'未开始')+'</span>'
        +'<span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>'
      +'</div>';
    }).join('');
  }

  function checkIn(habitId, btn){
    if (!currentUser){ showLogin(); return; }
    var habit = habits.filter(function(h){ return h.id===habitId; })[0];
    if (!habit) return;
    if (!isScheduled(habit,TODAY)){ showToast('今天不需要打卡'); return; }

    if (!records[habitId]) records[habitId]=[];
    var idx = records[habitId].indexOf(TODAY);
    var before = todayStats();

    if (idx>=0){
      records[habitId].splice(idx,1);
      showToast('已取消打卡');
    } else {
      records[habitId].push(TODAY);
      if (btn){
        btn.classList.remove('pop');
        void btn.offsetWidth;
        btn.classList.add('pop');
      }
      confetti(28);
      var after = todayStats();
      var streak = currentStreak(habitId);

      if (streak===7) celebrate('🏆','达成 7 天','连续打卡一周，继续保持！');
      else if (streak===21) celebrate('🌟','达成 21 天','习惯正在养成！');
      else if (streak===30) celebrate('👑','达成 30 天','连续一个月，太厉害了！');
      else if (before.total>0 && after.done===after.total){
        celebrate('🎉','全部完成','今天的所有习惯都已打卡');
        confetti(50);
      } else {
        showToast(streak>1?'打卡成功 · 连续 '+streak+' 天 🔥':'打卡成功 🎉');
      }
    }

    renderHome(); renderStats(); renderMe();
  }

  var newEmoji = EMOJIS[0];
  var newColor = COLORS[0].val;
  var newDays = [0,1,2,3,4,5,6];

  function renderNewForm(){
    $('emojiPicker').innerHTML = EMOJIS.map(function(e){
      return '<button class="emoji-pick'+(e===newEmoji?' active':'')+'" data-emoji="'+e+'">'+e+'</button>';
    }).join('');
    $('colorPicker').innerHTML = COLORS.map(function(c){
      return '<button class="color-pick'+(c.val===newColor?' active':'')+'" data-color="'+c.val+'" style="background:'+c.val+';color:'+c.val+'"></button>';
    }).join('');
    $('dayPicker').innerHTML = DAY_LABELS.map(function(label,i){
      return '<button class="day-pick'+(newDays.indexOf(i)>=0?' active':'')+'" data-day="'+i+'">'+label+'</button>';
    }).join('');
  }

  function createHabit(){
    if (!currentUser){ showLogin(); return; }
    var name = $('newName').value.trim();
    if (!name){ showToast('请输入习惯名称'); $('newName').focus(); return; }
    if (!newDays.length){ showToast('请至少选择一天'); return; }
    var goal = Math.max(1, Math.min(99, parseInt($('newGoal').value,10)||1));

    var id = nextId++;
    habits.push({
      id:id, name:name, emoji:newEmoji, color:newColor, goal:goal,
      days:newDays.slice().sort(function(a,b){return a-b;}),
      createdAt:TODAY
    });
    records[id] = [];

    showToast('习惯创建成功');
    setTimeout(function(){
      closePage();
      renderHome(); renderStats(); renderMe();
    }, 400);
  }

  function openHabitDetail(id){
    var h = habits.filter(function(x){ return x.id===id; })[0];
    if (!h) return;
    $('habitNavTitle').textContent = h.name;

    var streak = currentStreak(h.id);
    var best = bestStreak(h.id);
    var total = (records[h.id]||[]).length;

    var now = new Date();
    var y = now.getFullYear(), m = now.getMonth();
    var firstDay = new Date(y, m, 1).getDay();
    var daysInMonth = new Date(y, m+1, 0).getDate();

    var cal = '';
    for (var i=0;i<firstDay;i++) cal += '<div class="cal-day empty-cal"></div>';
    for (var d=1; d<=daysInMonth; d++){
      var ds = y+'-'+pad(m+1)+'-'+pad(d);
      var on = isChecked(h.id, ds);
      var isToday = ds===TODAY;
      var sch = isScheduled(h, ds);
      cal += '<div class="cal-day '+(on?'on':'')+' '+(isToday?'today':'')+'" style="'+((!sch&&!on)?'opacity:.35':'')+'">'+d+'</div>';
    }

    var recent = (records[h.id]||[]).slice().sort().reverse().slice(0,8);
    var timeline = recent.length ? recent.map(function(d){
      var dateObj = new Date(d+'T00:00:00');
      return '<div class="tl-item">'
        +'<div class="time">'+(dateObj.getMonth()+1)+'月'+dateObj.getDate()+'日 · 星期'+DAY_LABELS[dateObj.getDay()]+'</div>'
        +'<div class="text">完成打卡'+(h.goal>1?' · 目标 '+h.goal+' 次':'')+'</div>'
      +'</div>';
    }).join('') : '<div style="color:var(--label-3);font-size:13px">还没有打卡记录</div>';

    $('habitScroll').innerHTML = ''
      +'<div class="detail-hero" style="background:linear-gradient(135deg, '+h.color+', '+h.color+'cc)">'
        +'<span class="emoji">'+h.emoji+'</span>'
        +'<h2>'+h.name+'</h2>'
        +'<p>'+(h.days.length===7?'每天':'每周 '+h.days.map(function(d){return DAY_LABELS[d];}).join(' '))+' · 目标 '+h.goal+' 次</p>'
      +'</div>'

      +'<div class="block" style="margin-top:16px">'
        +'<div class="block-head"><h3>数据概览</h3></div>'
        +'<div class="stats" style="box-shadow:none;margin:0;padding:0;background:transparent">'
          +'<div class="stat"><div class="num">'+total+'</div><div class="lbl">累计</div></div>'
          +'<div class="stat"><div class="num">🔥 '+streak+'</div><div class="lbl">连续</div></div>'
          +'<div class="stat"><div class="num">'+best+'</div><div class="lbl">最长</div></div>'
        +'</div>'
      +'</div>'

      +'<div class="block">'
        +'<div class="block-head"><h3>本月打卡</h3><span class="more">'+(now.getMonth()+1)+' 月</span></div>'
        +'<div class="calendar">'
          + DAY_LABELS.map(function(d){ return '<div class="cal-head">'+d+'</div>'; }).join('')
          + cal
        +'</div>'
      +'</div>'

      +'<div class="block">'
        +'<div class="block-head"><h3>最近打卡</h3></div>'
        +'<div class="timeline">'+timeline+'</div>'
      +'</div>'

      +'<div style="padding:0 16px 40px">'
        +'<button class="danger-btn" data-del="'+h.id+'">删除该习惯</button>'
      +'</div>';

    showPage('habit');
  }

  var mainPages = ['home','stats','me'];
  var pageStack = [];

  function showPage(name){
    var isOverlay = mainPages.indexOf(name)<0 && name!=='login';
    if (isOverlay){
      pageStack = [name];
    } else {
      pageStack = [];
    }

    document.querySelectorAll('.page').forEach(function(p){
      p.classList.toggle('active', p.id==='page-'+name);
    });
    document.querySelectorAll('.page.overlay').forEach(function(p){
      if (p.id !== 'page-'+name) p.classList.remove('closing');
    });

    tabbar.classList.toggle('hidden', isOverlay || name==='login');
  }

  function closePage(){
    var name = pageStack.pop();
    if (!name) return;
    var el = document.getElementById('page-'+name);
    if (!el) return;
    el.classList.add('closing');
    el.classList.remove('active');

    var prev = pageStack.length ? pageStack[pageStack.length-1] : null;
    setTimeout(function(){
      el.classList.remove('closing');
      if (prev){
        var prevEl = document.getElementById('page-'+prev);
        if (prevEl) prevEl.classList.add('active');
      } else {
        var cur = document.querySelector('.tab.active');
        var curName = cur ? cur.dataset.tab : 'home';
        var curEl = document.getElementById('page-'+curName);
        if (curEl) curEl.classList.add('active');
        tabbar.classList.remove('hidden');
      }
    }, 320);
  }

  function showLogin(){
    pageStack = [];
    document.querySelectorAll('.page').forEach(function(p){
      p.classList.toggle('active', p.id==='page-login');
    });
    tabbar.classList.add('hidden');
  }

  var edgeStartX=0, edgeStartY=0, edgeTracking=false;
  var EDGE_ZONE = 22;

  app.addEventListener('touchstart', function(e){
    if (!pageStack.length) return;
    var t = e.touches[0];
    if (t.clientX <= EDGE_ZONE){
      edgeStartX = t.clientX;
      edgeStartY = t.clientY;
      edgeTracking = true;
      $('edgeHint').classList.add('show');
    }
  }, {passive:true});

  app.addEventListener('touchmove', function(e){
    if (!edgeTracking) return;
    var t = e.touches[0];
    var dx = t.clientX - edgeStartX;
    var dy = Math.abs(t.clientY - edgeStartY);
    if (dy>40){ edgeTracking=false; $('edgeHint').classList.remove('show'); return; }
    if (dx>60){
      edgeTracking=false;
      $('edgeHint').classList.remove('show');
      closePage();
    }
  }, {passive:true});

  app.addEventListener('touchend', function(){
    edgeTracking=false;
    $('edgeHint').classList.remove('show');
  });

  var pressTimer=null, pressEl=null;

  function startPress(el){
    pressEl = el;
    el.classList.add('show-hint');
    pressTimer = setTimeout(function(){
      var id = +el.dataset.habit;
      var h = habits.filter(function(x){ return x.id===id; })[0];
      if (!h) return;
      if (confirm('删除「'+h.name+'」？')){
        el.classList.add('removing');
        setTimeout(function(){
          habits = habits.filter(function(x){ return x.id!==id; });
          delete records[id];
          renderHome(); renderStats(); renderMe();
        }, 340);
      }
      el.classList.remove('show-hint');
    }, 700);
  }
  function endPress(){
    clearTimeout(pressTimer);
    if (pressEl) pressEl.classList.remove('show-hint');
    pressEl = null;
  }

  app.addEventListener('touchstart', function(e){
    var el = e.target.closest('.habit');
    if (!el || e.target.closest('.check-btn')) return;
    startPress(el);
  }, {passive:true});
  app.addEventListener('touchend', endPress);
  app.addEventListener('touchcancel', endPress);
  app.addEventListener('touchmove', endPress, {passive:true});

  var mouseDown=false;
  app.addEventListener('mousedown', function(e){
    var el = e.target.closest('.habit');
    if (!el || e.target.closest('.check-btn')) return;
    mouseDown=true;
    startPress(el);
  });
  app.addEventListener('mouseup', function(){ if (mouseDown){ endPress(); mouseDown=false; } });

  var SHEETS = {
    user: { title:'用户协议', body:'欢迎使用本习惯打卡应用。使用本服务即表示你同意本协议全部条款。<br><br>1. 账号注册与安全：你需对账号下的一切行为负责。<br>2. 服务内容：本应用提供习惯创建、打卡记录、数据统计等服务。<br>3. 用户行为规范：不得发布违法、侵权内容。<br>4. 免责声明：因不可抗力导致的服务中断，本应用不承担责任。' },
    privacy: { title:'隐私政策', body:'本应用重视你的隐私。本政策说明我们如何收集、使用、存储和保护你的个人信息。<br><br>1. 收集范围：手机号、邮箱、打卡记录、设备信息。<br>2. 使用目的：账号登录、数据同步、客服支持。<br>3. 第三方共享：仅在必要范围内与云服务商共享。<br>4. 你的权利：可随时查询、更正、删除个人信息。' }
  };

  function openSheet(key){
    var data = SHEETS[key];
    if (!data) return;
    $('sheetTitle').textContent = data.title;
    $('sheetBody').innerHTML = '<p>'+data.body+'</p>';
    $('sheetMask').classList.add('show');
    $('sheet').classList.add('show');
  }

  $('sheetMask').addEventListener('click', function(){
    $('sheetMask').classList.remove('show');
    $('sheet').classList.remove('show');
  });
  $('sheetClose').addEventListener('click', function(){
    $('sheetMask').classList.remove('show');
    $('sheet').classList.remove('show');
  });

  document.querySelectorAll('[data-eye]').forEach(function(btn){
    btn.addEventListener('click', function(){
      var input = document.getElementById(btn.dataset.eye);
      if (!input) return;
      input.type = input.type==='password' ? 'text' : 'password';
      btn.style.color = input.type==='text' ? 'var(--blue)' : '';
    });
  });

  $('loginSeg').addEventListener('click', function(e){
    var btn = e.target.closest('button[data-mode]');
    if (!btn) return;
    $('loginSeg').querySelectorAll('button').forEach(function(b){ b.classList.toggle('active', b===btn); });
    var isPwd = btn.dataset.mode==='pwd';
    $('formPwd').style.display = isPwd ? '' : 'none';
    $('formCode').style.display = isPwd ? 'none' : '';
  });

  function startCountdown(btn){
    var n=60;
    btn.disabled = true;
    var raw = btn.textContent;
    btn.textContent = n+'s 后重发';
    var timer = setInterval(function(){
      n--;
      if (n<=0){ clearInterval(timer); btn.disabled=false; btn.textContent=raw; }
      else btn.textContent = n+'s 后重发';
    }, 1000);
  }

  function bindSendCode(btnId, phoneId){
    var btn = $(btnId);
    if (!btn) return;
    btn.addEventListener('click', function(){
      var phone = (($(phoneId)||{}).value || '').trim();
      if (!/^1[3-9]\d{9}$/.test(phone)){ showToast('请输入正确的手机号'); return; }
      startCountdown(btn);
      showToast('验证码已发送（演示：123456）');
    });
  }
  bindSendCode('btnSendCode','codePhone');
  bindSendCode('btnRegCode','regPhone');
  bindSendCode('btnForgotCode','forgotPhone');

  var SOCIAL = {
    google:   { name:'Google User',   email:'user@gmail.com',    label:'Google'   },
    facebook: { name:'Facebook User', email:'user@facebook.com', label:'Facebook' },
    apple:    { name:'Apple User',    email:'user@icloud.com',   label:'Apple'    }
  };

  document.querySelectorAll('[data-social]').forEach(function(btn){
    btn.addEventListener('click', function(){
      if (!$('agreeTerms').checked){ showToast('请先同意用户协议和隐私政策'); return; }
      var provider = btn.dataset.social;
      var info = SOCIAL[provider];
      var original = btn.innerHTML;
      btn.classList.add('loading');
      btn.innerHTML = '<span class="spinner"></span><span class="label-text">正在跳转 '+info.label+'…</span>';
      setTimeout(function(){
        btn.classList.remove('loading');
        btn.innerHTML = original;
        loginSuccess({
          name: info.name,
          email: info.email,
          provider: info.label,
          avatar: info.name.charAt(0).toUpperCase()
        });
      }, 1200);
    });
  });

  $('btnPwdLogin').addEventListener('click', function(){
    var account = $('pwdAccount').value.trim();
    var pwd = $('pwdValue').value;
    if (!account){ showToast('请输入手机号或邮箱'); $('pwdAccount').focus(); return; }
    if (!pwd){ showToast('请输入密码'); $('pwdValue').focus(); return; }
    if (pwd.length<6){ showToast('密码至少 6 位'); return; }
    if (!$('agreeTerms').checked){ showToast('请先同意用户协议和隐私政策'); return; }

    var btn = $('btnPwdLogin');
    btn.classList.add('loading');
    var raw = btn.textContent;
    btn.innerHTML = '<span class="spinner"></span>登录中…';
    setTimeout(function(){
      btn.classList.remove('loading');
      btn.textContent = raw;
      var isPhone = /^1[3-9]\d{9}$/.test(account);
      var name = isPhone ? '用户'+account.slice(-4) : account.split('@')[0];
      loginSuccess({
        name: name,
        email: isPhone ? '' : account,
        provider: '账号密码',
        avatar: name.charAt(0).toUpperCase()
      });
    }, 900);
  });

  $('btnCodeLogin').addEventListener('click', function(){
    var phone = $('codePhone').value.trim();
    var code = $('codeValue').value.trim();
    if (!/^1[3-9]\d{9}$/.test(phone)){ showToast('请输入正确的手机号'); return; }
    if (!/^\d{6}$/.test(code)){ showToast('请输入 6 位验证码'); return; }
    if (!$('agreeTerms').checked){ showToast('请先同意用户协议和隐私政策'); return; }

    var btn = $('btnCodeLogin');
    btn.classList.add('loading');
    var raw = btn.textContent;
    btn.innerHTML = '<span class="spinner"></span>登录中…';
    setTimeout(function(){
      btn.classList.remove('loading');
      btn.textContent = raw;
      loginSuccess({
        name: '用户'+phone.slice(-4),
        email: '',
        provider: '验证码',
        avatar: phone.slice(-1)
      });
    }, 900);
  });

  $('btnToRegister').addEventListener('click', function(e){
    e.preventDefault();
    showPage('register');
  });

  $('btnRegister').addEventListener('click', function(){
    var phone = $('regPhone').value.trim();
    var code = $('regCode').value.trim();
    var pwd = $('regPwd').value;
    var pwd2 = $('regPwd2').value;

    if (!/^1[3-9]\d{9}$/.test(phone)){ showToast('请输入正确的手机号'); return; }
    if (!/^\d{6}$/.test(code)){ showToast('请输入 6 位验证码'); return; }
    if (pwd.length<8 || pwd.length>20){ showToast('密码需 8-20 位'); return; }
    if (!/[a-zA-Z]/.test(pwd) || !/\d/.test(pwd)){ showToast('密码需含字母和数字'); return; }
    if (pwd!==pwd2){ showToast('两次密码不一致'); return; }
    if (!$('regAgree').checked){ showToast('请先同意用户协议和隐私政策'); return; }

    var btn = $('btnRegister');
    btn.classList.add('loading');
    var raw = btn.textContent;
    btn.innerHTML = '<span class="spinner"></span>注册中…';
    setTimeout(function(){
      btn.classList.remove('loading');
      btn.textContent = raw;
      showToast('注册成功，正在登录…');
      setTimeout(function(){
        loginSuccess({
          name: '用户'+phone.slice(-4),
          email: '',
          provider: '手机号注册',
          avatar: phone.slice(-1)
        });
      }, 600);
    }, 1000);
  });

  document.addEventListener('click', function(e){
    if (e.target.closest('[data-forgot]')){
      e.preventDefault();
      showPage('forgot');
    }
  });

  $('btnReset').addEventListener('click', function(){
    var phone = $('forgotPhone').value.trim();
    var code = $('forgotCode').value.trim();
    var pwd = $('forgotPwd').value;

    if (!/^1[3-9]\d{9}$/.test(phone)){ showToast('请输入正确的手机号'); return; }
    if (!/^\d{6}$/.test(code)){ showToast('请输入 6 位验证码'); return; }
    if (pwd.length<8 || pwd.length>20){ showToast('密码需 8-20 位'); return; }
    if (!/[a-zA-Z]/.test(pwd) || !/\d/.test(pwd)){ showToast('密码需含字母和数字'); return; }

    showToast('密码重置成功，请重新登录');
    setTimeout(function(){ showPage('login'); }, 900);
  });

  function loginSuccess(user){
    currentUser = user;
    renderMe();
    showToast('欢迎回来，'+user.name);
    setTimeout(function(){ switchTab('home'); }, 400);
  }

  document.addEventListener('click', function(e){
    if (e.target.closest('[data-logout]')){
      $('logoutMask').classList.add('show');
      $('logoutSheet').classList.add('show');
    }
  });

  $('cancelLogout').addEventListener('click', function(){
    $('logoutMask').classList.remove('show');
    $('logoutSheet').classList.remove('show');
  });

  $('confirmLogout').addEventListener('click', function(){
    $('logoutMask').classList.remove('show');
    $('logoutSheet').classList.remove('show');
    currentUser = null;
    showToast('已退出登录');
    setTimeout(function(){
      showLogin();
      $('pwdAccount').value = '';
      $('pwdValue').value = '';
      $('codePhone').value = '';
      $('codeValue').value = '';
    }, 300);
  });

  document.addEventListener('click', function(e){
    var t = e.target;

    if (t.closest('[data-back]')){ closePage(); return; }

    var termsEl = t.closest('[data-terms]');
    if (termsEl){ e.preventDefault(); openSheet(termsEl.dataset.terms); return; }

    var opener = t.closest('[data-open]');
    if (opener){
      if (!currentUser){ showLogin(); return; }
      var name = opener.dataset.open;
      if (name==='new'){ renderNewForm(); $('newName').value=''; $('newGoal').value='1'; }
      showPage(name);
      return;
    }

    var checkBtn = t.closest('[data-check]');
    if (checkBtn){
      e.stopPropagation();
      checkIn(+checkBtn.dataset.check, checkBtn);
      return;
    }

    var habitEl = t.closest('[data-habit]');
    if (habitEl){
      if (!currentUser){ showLogin(); return; }
      openHabitDetail(+habitEl.dataset.habit);
      return;
    }

    var delBtn = t.closest('[data-del]');
    if (delBtn){
      var id = +delBtn.dataset.del;
      var h = habits.filter(function(x){ return x.id===id; })[0];
      if (h && confirm('删除「'+h.name+'」？')){
        habits = habits.filter(function(x){ return x.id!==id; });
        delete records[id];
        showToast('已删除');
        setTimeout(function(){
          closePage();
          renderHome(); renderStats(); renderMe();
        }, 400);
      }
      return;
    }

    var toastTrigger = t.closest('[data-toast]');
    if (toastTrigger){ showToast(toastTrigger.dataset.toast); return; }
  });

  document.addEventListener('click', function(e){
    var chip = e.target.closest('#statChips .chip');
    if (chip){ statRange = +chip.dataset.range; renderStats(); }
  });

  document.addEventListener('click', function(e){
    var em = e.target.closest('.emoji-pick');
    if (em){
      newEmoji = em.dataset.emoji;
      $('emojiPicker').querySelectorAll('.emoji-pick').forEach(function(x){ x.classList.toggle('active', x===em); });
      return;
    }
    var co = e.target.closest('.color-pick');
    if (co){
      newColor = co.dataset.color;
      $('colorPicker').querySelectorAll('.color-pick').forEach(function(x){ x.classList.toggle('active', x===co); });
      return;
    }
    var dy = e.target.closest('.day-pick');
    if (dy){
      var d = +dy.dataset.day;
      var idx = newDays.indexOf(d);
      if (idx>=0) newDays.splice(idx,1); else newDays.push(d);
      dy.classList.toggle('active');
      return;
    }
  });

  $('btnCreate').addEventListener('click', createHabit);
  $('navCreate').addEventListener('click', createHabit);
  $('navHabitEdit').addEventListener('click', function(){ showToast('编辑功能开发中'); });

  document.addEventListener('click', function(e){
    var sw = e.target.closest('[data-switch]');
    if (sw) sw.classList.toggle('on');
  });

  function switchTab(name){
    if (!currentUser){ showLogin(); return; }
    pageStack = [];
    document.querySelectorAll('.page.overlay').forEach(function(p){ p.classList.remove('active','closing'); });
    tabbar.classList.remove('hidden');

    tabbar.querySelectorAll('.tab').forEach(function(t){ t.classList.toggle('active', t.dataset.tab===name); });
    document.querySelectorAll('.page').forEach(function(p){ p.classList.toggle('active', p.id==='page-'+name); });

    if (name==='home') renderHome();
    if (name==='stats') renderStats();
    if (name==='me') renderMe();
  }

  tabbar.addEventListener('click', function(e){
    var tab = e.target.closest('.tab');
    if (!tab) return;
    switchTab(tab.dataset.tab);
  });

  renderHome();
  renderStats();
  renderMe();
  renderNewForm();
  showLogin();

})();
</script>
</body>
</html>
