<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#F2F2F7" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)">
<title>商城</title>
<style>
:root{
  --blue:#007AFF;
  --red:#FF3B30;
  --green:#34C759;
  --orange:#FF9500;
  --purple:#AF52DE;
  --pink:#FF2D55;
  --teal:#5AC8FA;
  --bg:#F2F2F7;
  --page-bg:#E6E6EB;
  --card:#FFFFFF;
  --label:#000000;
  --label-2:rgba(60,60,67,.6);
  --label-3:rgba(60,60,67,.42);
  --fill:rgba(120,120,128,.12);
  --fill-2:rgba(120,120,128,.08);
  --sep:rgba(60,60,67,.2);
  --tabbar-bg:rgba(255,255,255,.58);
  --tabbar-border:rgba(255,255,255,.6);
  --shadow:0 1px 3px rgba(0,0,0,.04), 0 8px 20px -12px rgba(0,0,0,.28);
  --shadow-lg:0 14px 28px -14px rgba(10,132,255,.75);
  --tabbar-shadow:0 -6px 24px -10px rgba(0,0,0,.2), 0 3px 10px rgba(0,0,0,.07);
}
@media (prefers-color-scheme: dark){
  :root{
    --blue:#0A84FF;--red:#FF453A;--green:#30D158;--orange:#FF9F0A;
    --purple:#BF5AF2;--pink:#FF375F;--teal:#64D2FF;
    --bg:#000000;--page-bg:#000000;--card:#1C1C1E;
    --label:#FFFFFF;
    --label-2:rgba(235,235,245,.6);
    --label-3:rgba(235,235,245,.4);
    --fill:rgba(120,120,128,.24);
    --fill-2:rgba(120,120,128,.16);
    --sep:rgba(84,84,88,.6);
    --tabbar-bg:rgba(30,30,32,.6);
    --tabbar-border:rgba(255,255,255,.09);
    --shadow:0 1px 3px rgba(0,0,0,.5);
    --shadow-lg:0 14px 28px -14px rgba(0,0,0,.8);
    --tabbar-shadow:0 -6px 24px -10px rgba(0,0,0,.75), 0 3px 10px rgba(0,0,0,.45);
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
  .app{box-shadow:0 0 0 1px rgba(0,0,0,.08), 0 24px 70px -24px rgba(0,0,0,.45);border-radius:0 0 34px 34px}
}

/* ---------- 状态栏 ---------- */
.statusbar{
  flex:0 0 auto;height:44px;
  padding:env(safe-area-inset-top) 26px 0 30px;
  display:flex;align-items:center;justify-content:space-between;
  font-size:15px;font-weight:600;letter-spacing:-.2px;color:var(--label);
  box-sizing:content-box;position:relative;z-index:30;
}
.status-icons{display:flex;align-items:center;gap:5px}
.status-icons svg{display:block}

/* ---------- 页面容器 ---------- */
.page{
  position:absolute;
  inset:0;
  padding-top:calc(44px + env(safe-area-inset-top));
  display:none;flex-direction:column;
  background:var(--bg);
  z-index:10;
}
.page.active{display:flex}
.page.overlay{z-index:40}
.page.overlay.active{
  display:flex;
  animation:pageIn .34s cubic-bezier(.32,.72,0,1);
}
@keyframes pageIn{from{transform:translateX(100%)}to{transform:translateX(0)}}

.scroll{
  flex:1 1 auto;min-height:0;
  overflow-y:auto;-webkit-overflow-scrolling:touch;
  padding-bottom:108px;
}

/* 头部 */
.header{flex:0 0 auto;padding-bottom:8px}
.header-row{
  margin:0 16px;height:46px;
  display:flex;align-items:center;justify-content:space-between;gap:10px;
}
.header-row h1{margin:0;font-size:28px;font-weight:700;letter-spacing:-.6px;flex:1}
.header-row .sub{font-size:13px;color:var(--label-2);margin-top:2px}

.icon-btn{
  width:38px;height:38px;border:0;border-radius:50%;
  background:var(--fill);color:var(--label);
  display:grid;place-items:center;position:relative;cursor:pointer;flex:0 0 auto;
  transition:transform .16s ease, background .2s;
}
.icon-btn:active{transform:scale(.9)}
.icon-btn svg{width:21px;height:21px}

.badge{
  position:absolute;top:-3px;right:-3px;
  min-width:18px;height:18px;padding:0 5px;border-radius:9px;
  background:var(--red);color:#fff;font-size:11px;font-weight:600;
  line-height:18px;text-align:center;
  box-shadow:0 0 0 2.5px var(--bg);
  transform:scale(0);
  transition:transform .3s cubic-bezier(.34,1.56,.64,1);
}
.badge.show{transform:scale(1)}

/* 搜索框 */
.searchbar{
  margin:2px 16px 0;height:36px;padding:0 8px;
  display:flex;align-items:center;gap:6px;
  background:var(--fill);border-radius:10px;color:var(--label-3);
}
.searchbar svg{width:16px;height:16px;flex:0 0 auto}
.searchbar input{
  flex:1;min-width:0;border:0;outline:none;background:transparent;
  font-family:inherit;font-size:16px;color:var(--label);padding:0;
}
.searchbar input::placeholder{color:var(--label-3)}

/* 分类 chips */
.chips{display:flex;gap:8px;overflow-x:auto;padding:12px 16px 4px;scrollbar-width:none;-ms-overflow-style:none}
.chips::-webkit-scrollbar{display:none}
.chip{
  flex:0 0 auto;height:32px;padding:0 15px;border:0;border-radius:16px;
  background:var(--fill);color:var(--label);
  font-family:inherit;font-size:14px;font-weight:500;cursor:pointer;
  transition:background .22s, color .22s, transform .15s;
}
.chip:active{transform:scale(.94)}
.chip.active{background:var(--blue);color:#fff}

/* Banner */
.banner{
  position:relative;margin:8px 16px 0;padding:20px;border-radius:20px;overflow:hidden;
  color:#fff;background:linear-gradient(135deg,#0A84FF 0%,#5E5CE6 55%,#BF5AF2 100%);
  box-shadow:var(--shadow-lg);
}
.banner-tag{
  display:inline-block;font-size:11px;font-weight:600;letter-spacing:.3px;
  padding:4px 10px;border-radius:999px;background:rgba(255,255,255,.24);
  margin-bottom:10px;backdrop-filter:blur(4px);
}
.banner h2{margin:0;font-size:22px;line-height:1.28;font-weight:700;letter-spacing:-.4px}
.banner p{margin:8px 0 0;font-size:12.5px;opacity:.85}
.deco{position:absolute;border-radius:50%;background:rgba(255,255,255,.16);pointer-events:none}
.deco-1{width:150px;height:150px;right:-46px;top:-58px}
.deco-2{width:92px;height:92px;right:46px;bottom:-52px;background:rgba(255,255,255,.1)}

.section-head{
  display:flex;align-items:baseline;justify-content:space-between;
  padding:22px 16px 0;
}
.section-head h3{margin:0;font-size:18px;font-weight:700;letter-spacing:-.3px}
.section-head span{font-size:12px;color:var(--label-2)}
.section-head .more{
  font-size:13px;color:var(--blue);font-weight:500;cursor:pointer;
  display:flex;align-items:center;gap:2px;
}

/* 商品网格 */
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;padding:12px 16px 24px}

.card{
  background:var(--card);border-radius:16px;overflow:hidden;
  box-shadow:var(--shadow);cursor:pointer;
  animation:cardIn .42s cubic-bezier(.22,1,.36,1) backwards;
  transition:transform .18s ease;
}
.card:active{transform:scale(.972)}
@keyframes cardIn{from{opacity:0;transform:translateY(14px) scale(.96)}to{opacity:1;transform:none}}

.thumb{position:relative;aspect-ratio:1/1;display:grid;place-items:center}
.thumb .emoji{font-size:54px;line-height:1;filter:drop-shadow(0 6px 10px rgba(0,0,0,.12))}
.tag{
  position:absolute;top:8px;left:8px;font-size:10.5px;font-weight:600;color:#fff;
  background:var(--blue);padding:3px 8px;border-radius:999px;letter-spacing:.2px;
}
.info{padding:10px 12px 12px}
.name{
  font-size:14px;font-weight:600;line-height:1.32;letter-spacing:-.2px;
  display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;
  overflow:hidden;min-height:37px;
}
.desc{margin-top:3px;font-size:11px;color:var(--label-2);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.row{display:flex;align-items:center;justify-content:space-between;margin-top:9px}
.price{color:var(--red);font-size:17px;font-weight:700;letter-spacing:-.4px}
.price .cur{font-size:12px;font-weight:600;margin-right:1px}
.add{
  width:28px;height:28px;flex:0 0 auto;border:0;border-radius:50%;
  background:var(--blue);color:#fff;display:grid;place-items:center;
  cursor:pointer;transition:transform .15s, opacity .15s;
}
.add:active{transform:scale(.85);opacity:.85}
.add.pop{animation:btnPop .38s ease}
@keyframes btnPop{0%{transform:scale(1)}40%{transform:scale(1.35)}70%{transform:scale(.94)}100%{transform:scale(1)}}

/* ================= 分类页 ================= */
.cat-layout{display:flex;flex:1;min-height:0;padding-top:8px}

.cat-side{
  flex:0 0 96px;overflow-y:auto;
  padding:0 0 120px;scrollbar-width:none;
}
.cat-side::-webkit-scrollbar{display:none}
.cat-item{
  position:relative;width:100%;border:0;background:none;
  padding:14px 8px 14px 16px;
  font-family:inherit;font-size:13.5px;font-weight:500;
  color:var(--label-2);text-align:left;cursor:pointer;
  transition:color .2s, background .2s;
}
.cat-item.active{color:var(--label);font-weight:600;background:var(--card)}
.cat-item.active::before{
  content:"";position:absolute;left:0;top:50%;transform:translateY(-50%);
  width:3px;height:18px;border-radius:0 3px 3px 0;background:var(--blue);
}

.cat-main{
  flex:1;min-width:0;overflow-y:auto;
  background:var(--card);border-radius:18px 0 0 0;
  padding:16px 16px 120px;scrollbar-width:none;
}
.cat-main::-webkit-scrollbar{display:none}
.cat-title{
  font-size:17px;font-weight:700;letter-spacing:-.3px;
  margin:0 0 12px;display:flex;align-items:center;gap:8px;
}
.cat-title small{font-size:11px;font-weight:500;color:var(--label-2)}
.cat-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.cat-card{
  background:var(--bg);border-radius:14px;overflow:hidden;
  cursor:pointer;transition:transform .18s ease;
}
.cat-card:active{transform:scale(.96)}
.cat-card .thumb{aspect-ratio:1/1;border-radius:14px 14px 0 0}
.cat-card .thumb .emoji{font-size:38px}
.cat-card .info{padding:8px 10px 10px}
.cat-card .name{font-size:12.5px;min-height:33px;-webkit-line-clamp:2}
.cat-card .price{font-size:14px;margin-top:6px;display:block}
.cat-card .price .cur{font-size:10.5px}

/* ================= 购物车页 ================= */
.cart-list{padding:8px 16px 0;display:flex;flex-direction:column;gap:12px}
.cart-item{
  display:flex;gap:12px;background:var(--card);border-radius:16px;
  padding:12px;box-shadow:var(--shadow);
  animation:cardIn .36s cubic-bezier(.22,1,.36,1) backwards;
}
.cart-thumb{
  flex:0 0 76px;width:76px;height:76px;border-radius:12px;
  display:grid;place-items:center;position:relative;
}
.cart-thumb .emoji{font-size:34px;line-height:1}
.cart-body{flex:1;min-width:0;display:flex;flex-direction:column}
.cart-name{
  font-size:13.5px;font-weight:600;line-height:1.34;letter-spacing:-.2px;
  display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden;
}
.cart-desc{margin-top:2px;font-size:11px;color:var(--label-2);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.cart-foot{display:flex;align-items:center;justify-content:space-between;margin-top:auto;padding-top:8px}
.cart-price{color:var(--red);font-size:16px;font-weight:700;letter-spacing:-.3px}
.cart-price .cur{font-size:11px;font-weight:600;margin-right:1px}

.stepper{
  display:flex;align-items:center;
  background:var(--fill);border-radius:999px;padding:3px;
}
.stepper button{
  width:26px;height:26px;border:0;border-radius:50%;
  background:var(--card);color:var(--blue);
  display:grid;place-items:center;cursor:pointer;
  font-size:16px;font-weight:600;line-height:1;
  transition:transform .14s ease;
  box-shadow:0 1px 3px rgba(0,0,0,.1);
}
.stepper button:active{transform:scale(.86)}
.stepper .qty{
  min-width:28px;text-align:center;
  font-size:14px;font-weight:600;font-variant-numeric:tabular-nums;
}

/* 结算栏 */
.checkout-bar{
  position:absolute;left:12px;right:12px;
  bottom:calc(86px + env(safe-area-inset-bottom));
  z-index:15;
  display:flex;align-items:center;justify-content:space-between;gap:12px;
  padding:12px 14px 12px 18px;border-radius:26px;
  background:var(--tabbar-bg);
  border:0.5px solid var(--tabbar-border);
  backdrop-filter:blur(24px) saturate(200%);
  -webkit-backdrop-filter:blur(24px) saturate(200%);
  box-shadow:var(--tabbar-shadow);
  transform:translateY(140%);opacity:0;
  transition:transform .38s cubic-bezier(.22,1,.36,1), opacity .3s ease;
  pointer-events:none;
}
.checkout-bar.show{transform:translateY(0);opacity:1;pointer-events:auto}
.checkout-total{display:flex;flex-direction:column;gap:1px}
.checkout-total .lbl{font-size:10.5px;color:var(--label-2)}
.checkout-total .amt{color:var(--red);font-size:20px;font-weight:700;letter-spacing:-.5px}
.checkout-total .amt .cur{font-size:12px;font-weight:600;margin-right:1px}
.pay-btn{
  border:0;border-radius:18px;padding:0 22px;height:44px;
  background:var(--blue);color:#fff;
  font-family:inherit;font-size:15px;font-weight:600;letter-spacing:-.2px;
  cursor:pointer;
  box-shadow:0 8px 20px -8px rgba(0,122,255,.85);
  transition:transform .16s ease, opacity .16s;
}
.pay-btn:active{transform:scale(.95);opacity:.9}

/* 空状态 */
.empty{
  flex:1;display:flex;flex-direction:column;
  align-items:center;justify-content:center;
  padding:60px 40px 80px;text-align:center;
}
.empty .ico{
  width:78px;height:78px;border-radius:50%;
  background:var(--fill);display:grid;place-items:center;margin-bottom:16px;
}
.empty .ico svg{width:36px;height:36px;color:var(--label-3)}
.empty h4{margin:0 0 6px;font-size:16px;font-weight:600}
.empty p{margin:0 0 18px;font-size:13px;color:var(--label-2);line-height:1.5}
.empty button{
  border:0;border-radius:16px;padding:0 22px;height:42px;
  background:var(--blue);color:#fff;
  font-family:inherit;font-size:14px;font-weight:600;cursor:pointer;
  transition:transform .16s ease;
}
.empty button:active{transform:scale(.95)}

/* ================= 我的页 ================= */
.profile{padding:8px 16px 0}
.profile-card{
  position:relative;background:var(--card);border-radius:20px;
  padding:20px 18px;display:flex;align-items:center;gap:14px;
  box-shadow:var(--shadow);overflow:hidden;cursor:pointer;
}
.profile-card:active{transform:scale(.99)}
.profile-card::after{
  content:"";position:absolute;right:-40px;top:-60px;
  width:150px;height:150px;border-radius:50%;
  background:linear-gradient(135deg,rgba(10,132,255,.16),rgba(175,82,222,.12));
}
.avatar{
  flex:0 0 58px;width:58px;height:58px;border-radius:50%;
  background:linear-gradient(135deg,#0A84FF,#BF5AF2);
  display:grid;place-items:center;color:#fff;font-size:22px;font-weight:600;
  box-shadow:0 8px 18px -8px rgba(10,132,255,.9);
}
.profile-info{flex:1;min-width:0;position:relative}
.profile-info h2{margin:0;font-size:19px;font-weight:700;letter-spacing:-.4px}
.profile-info p{margin:4px 0 0;font-size:12px;color:var(--label-2)}

.stats{
  display:grid;grid-template-columns:repeat(3,1fr);
  background:var(--card);border-radius:20px;
  margin-top:12px;padding:16px 0;box-shadow:var(--shadow);
}
.stat{text-align:center;position:relative;cursor:pointer}
.stat + .stat::before{
  content:"";position:absolute;left:0;top:50%;transform:translateY(-50%);
  width:.5px;height:26px;background:var(--sep);
}
.stat .num{font-size:19px;font-weight:700;letter-spacing:-.4px;font-variant-numeric:tabular-nums}
.stat .lbl{font-size:11px;color:var(--label-2);margin-top:3px}

.group-title{
  margin:22px 0 8px;font-size:13px;font-weight:600;
  color:var(--label-2);letter-spacing:-.1px;
}

.list{background:var(--card);border-radius:16px;overflow:hidden;box-shadow:var(--shadow)}
.list-item{
  display:flex;align-items:center;gap:12px;
  padding:13px 16px;cursor:pointer;position:relative;
  transition:background .18s;
}
.list-item + .list-item::before{
  content:"";position:absolute;left:54px;right:0;top:0;
  height:.5px;background:var(--sep);
}
.list-item:active{background:var(--fill)}
.list-ico{
  flex:0 0 30px;width:30px;height:30px;border-radius:9px;
  display:grid;place-items:center;color:#fff;
}
.list-ico svg{width:17px;height:17px}
.list-ico.b{background:var(--blue)}
.list-ico.g{background:var(--green)}
.list-ico.o{background:var(--orange)}
.list-ico.p{background:var(--purple)}
.list-ico.r{background:var(--red)}
.list-ico.t{background:var(--teal)}
.list-ico.pk{background:var(--pink)}
.list-text{flex:1;min-width:0;font-size:14.5px;font-weight:500;letter-spacing:-.2px}
.list-val{font-size:13px;color:var(--label-2)}
.chev{color:var(--label-3);flex:0 0 auto;display:grid;place-items:center}
.chev svg{width:16px;height:16px}

/* ================= 商品详情页 ================= */
.detail-hero{
  position:relative;
  aspect-ratio:1/1;
  display:grid;place-items:center;
  margin:0;
}
.detail-hero .emoji{font-size:130px;line-height:1;filter:drop-shadow(0 12px 24px rgba(0,0,0,.16))}
.detail-hero .tag{
  top:14px;left:16px;font-size:12px;padding:5px 12px;
}

.detail-body{
  background:var(--bg);
  border-radius:24px 24px 0 0;
  margin-top:-24px;
  position:relative;
  padding:20px 16px 0;
}
.detail-price{color:var(--red);font-size:28px;font-weight:700;letter-spacing:-.8px}
.detail-price .cur{font-size:16px;font-weight:600;margin-right:2px}
.detail-name{margin:8px 0 0;font-size:20px;font-weight:700;letter-spacing:-.5px;line-height:1.35}
.detail-desc{margin:8px 0 0;font-size:13.5px;color:var(--label-2);line-height:1.5}

.detail-meta{
  display:flex;gap:8px;margin-top:14px;flex-wrap:wrap;
}
.meta-pill{
  font-size:11.5px;font-weight:500;color:var(--label-2);
  background:var(--fill);padding:6px 12px;border-radius:999px;
}

.detail-block{
  background:var(--card);border-radius:16px;
  margin-top:16px;padding:16px;box-shadow:var(--shadow);
}
.detail-block h4{
  margin:0 0 10px;font-size:14px;font-weight:600;letter-spacing:-.2px;
}
.detail-block p{margin:0;font-size:13px;color:var(--label-2);line-height:1.6}

.spec-row{display:flex;gap:10px;margin-top:10px;flex-wrap:wrap}
.spec{
  border:1.5px solid var(--fill);
  background:var(--bg);
  color:var(--label);
  border-radius:12px;padding:9px 16px;
  font-family:inherit;font-size:13.5px;font-weight:500;
  cursor:pointer;transition:all .2s;
}
.spec:active{transform:scale(.95)}
.spec.active{
  border-color:var(--blue);background:rgba(0,122,255,.1);color:var(--blue);font-weight:600;
}

.detail-bar{
  position:absolute;left:12px;right:12px;
  bottom:calc(10px + env(safe-area-inset-bottom));
  z-index:15;
  display:flex;align-items:center;gap:10px;
  padding:10px;border-radius:28px;
  background:var(--tabbar-bg);
  border:0.5px solid var(--tabbar-border);
  backdrop-filter:blur(24px) saturate(200%);
  -webkit-backdrop-filter:blur(24px) saturate(200%);
  box-shadow:var(--tabbar-shadow);
}
.detail-bar .icon-btn{background:var(--fill)}
.detail-bar .pay-btn{flex:1;height:46px;border-radius:20px;font-size:15.5px}

/* 详情页返回按钮 */
.nav-back{
  position:absolute;
  top:calc(env(safe-area-inset-top) + 6px);
  left:14px;
  z-index:50;
  width:38px;height:38px;border:0;border-radius:50%;
  background:var(--tabbar-bg);
  border:0.5px solid var(--tabbar-border);
  backdrop-filter:blur(20px) saturate(180%);
  -webkit-backdrop-filter:blur(20px) saturate(180%);
  color:var(--label);
  display:grid;place-items:center;cursor:pointer;
  box-shadow:0 2px 10px rgba(0,0,0,.12);
  transition:transform .16s;
}
.nav-back:active{transform:scale(.9)}
.nav-back svg{width:20px;height:20px}

/* ================= 搜索页 ================= */
.search-head{
  display:flex;align-items:center;gap:10px;
  padding:6px 16px 0;
}
.search-head .searchbar{flex:1;margin:0;height:40px;border-radius:12px}
.search-head .cancel{
  border:0;background:none;color:var(--blue);
  font-family:inherit;font-size:15px;font-weight:500;
  cursor:pointer;padding:0;white-space:nowrap;
}
.search-tags{display:flex;flex-wrap:wrap;gap:8px;padding:14px 16px 0}
.search-tag{
  border:0;background:var(--fill);color:var(--label);
  font-family:inherit;font-size:13.5px;font-weight:500;
  padding:8px 15px;border-radius:999px;cursor:pointer;
  transition:transform .15s, background .2s;
}
.search-tag:active{transform:scale(.94)}
.search-tag.hot{color:var(--red);background:rgba(255,59,48,.1)}

/* ================= 订单页 ================= */
.order-tabs{
  display:flex;gap:6px;overflow-x:auto;
  padding:10px 16px 4px;scrollbar-width:none;
}
.order-tabs::-webkit-scrollbar{display:none}
.order-tab{
  flex:0 0 auto;height:30px;padding:0 14px;border:0;border-radius:15px;
  background:var(--fill);color:var(--label-2);
  font-family:inherit;font-size:13px;font-weight:500;cursor:pointer;
  transition:all .2s;
}
.order-tab.active{background:var(--blue);color:#fff;font-weight:600}

.order-card{
  background:var(--card);border-radius:16px;
  margin:12px 16px 0;padding:14px;
  box-shadow:var(--shadow);
}
.order-top{
  display:flex;align-items:center;justify-content:space-between;
  margin-bottom:12px;
}
.order-no{font-size:11.5px;color:var(--label-3)}
.order-status{font-size:12.5px;font-weight:600;color:var(--blue)}
.order-status.done{color:var(--green)}
.order-status.wait{color:var(--orange)}

.order-goods{display:flex;gap:12px;align-items:center}
.order-thumb{
  flex:0 0 62px;width:62px;height:62px;border-radius:12px;
  display:grid;place-items:center;
}
.order-thumb .emoji{font-size:28px}
.order-info{flex:1;min-width:0}
.order-info .name{
  font-size:13.5px;font-weight:600;min-height:auto;
  -webkit-line-clamp:1;line-height:1.35;
}
.order-info .sub{font-size:11.5px;color:var(--label-2);margin-top:4px}
.order-info .amt{
  color:var(--red);font-size:15px;font-weight:700;margin-top:6px;
}
.order-info .amt .cur{font-size:11px;margin-right:1px}

.order-actions{
  display:flex;gap:8px;justify-content:flex-end;
  margin-top:12px;padding-top:12px;
  border-top:.5px solid var(--sep);
}
.order-btn{
  border:0;border-radius:14px;padding:0 16px;height:34px;
  font-family:inherit;font-size:13px;font-weight:500;cursor:pointer;
  transition:transform .15s;
}
.order-btn:active{transform:scale(.94)}
.order-btn.ghost{background:var(--fill);color:var(--label)}
.order-btn.primary{background:var(--blue);color:#fff;font-weight:600}

/* ================= 地址页 ================= */
.addr-card{
  background:var(--card);border-radius:16px;
  margin:12px 16px 0;padding:16px;
  box-shadow:var(--shadow);position:relative;
}
.addr-head{
  display:flex;align-items:center;gap:8px;margin-bottom:8px;
}
.addr-name{font-size:15.5px;font-weight:600;letter-spacing:-.2px}
.addr-phone{font-size:13px;color:var(--label-2)}
.addr-default{
  font-size:10px;font-weight:600;color:#fff;background:var(--blue);
  padding:2px 7px;border-radius:6px;margin-left:auto;
}
.addr-text{font-size:13px;color:var(--label-2);line-height:1.55}
.addr-edit{
  position:absolute;right:14px;bottom:14px;
  border:0;background:none;color:var(--blue);
  font-family:inherit;font-size:13.5px;font-weight:500;
  cursor:pointer;display:flex;align-items:center;gap:3px;
}
.addr-add{
  display:flex;align-items:center;justify-content:center;gap:8px;
  margin:16px;padding:16px;border-radius:16px;
  border:1.5px dashed var(--sep);
  background:transparent;color:var(--blue);
  font-family:inherit;font-size:15px;font-weight:600;cursor:pointer;
  width:calc(100% - 32px);
  transition:background .2s;
}
.addr-add:active{background:var(--fill)}

/* ================= 设置页 ================= */
.setting-item{
  display:flex;align-items:center;gap:12px;
  padding:13px 16px;position:relative;cursor:pointer;
  transition:background .18s;
}
.setting-item + .setting-item::before{
  content:"";position:absolute;left:16px;right:0;top:0;
  height:.5px;background:var(--sep);
}
.setting-item:active{background:var(--fill)}
.setting-text{flex:1;font-size:14.5px;font-weight:500}
.setting-val{font-size:13.5px;color:var(--label-2)}

.switch{
  width:51px;height:31px;border-radius:16px;
  background:var(--fill);
  position:relative;flex:0 0 auto;
  transition:background .28s cubic-bezier(.22,1,.36,1);
  cursor:pointer;
}
.switch::after{
  content:"";position:absolute;top:2px;left:2px;
  width:27px;height:27px;border-radius:50%;
  background:#fff;
  box-shadow:0 3px 8px rgba(0,0,0,.15);
  transition:transform .28s cubic-bezier(.22,1,.36,1);
}
.switch.on{background:var(--green)}
.switch.on::after{transform:translateX(20px)}

/* ---------- 玻璃质感悬浮 TabBar（更圆） ---------- */
.tabbar{
  position:absolute;
  left:14px;right:14px;
  bottom:calc(12px + env(safe-area-inset-bottom));
  z-index:60;
  display:flex;align-items:center;
  padding:8px 10px;
  border-radius:40px;             /* 圆角拉满，接近胶囊 */
  background:var(--tabbar-bg);
  border:0.5px solid var(--tabbar-border);
  backdrop-filter:blur(28px) saturate(200%);
  -webkit-backdrop-filter:blur(28px) saturate(200%);
  box-shadow:var(--tabbar-shadow);
  overflow:hidden;
}
.tabbar::before{
  content:"";position:absolute;inset:0;border-radius:inherit;
  background:linear-gradient(180deg, rgba(255,255,255,.6), rgba(255,255,255,.1) 46%, rgba(255,255,255,0) 72%);
  pointer-events:none;mix-blend-mode:overlay;
}
@media (prefers-color-scheme: dark){
  .tabbar::before{background:linear-gradient(180deg, rgba(255,255,255,.18), rgba(255,255,255,.04) 48%, rgba(255,255,255,0) 72%)}
}

.tab{
  flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;
  padding:6px 0 5px;border:0;background:none;
  color:var(--label-3);font-family:inherit;font-size:10px;font-weight:500;
  cursor:pointer;position:relative;
  border-radius:24px;
  transition:color .22s, background .28s, transform .18s;
}
.tab:active{transform:scale(.92)}
.tab.active{color:var(--blue);background:rgba(0,122,255,.11)}
@media (prefers-color-scheme: dark){
  .tab.active{background:rgba(10,132,255,.2)}
}
.tab svg{width:25px;height:25px;display:block;position:relative;z-index:1}
.tab span:not(.badge){position:relative;z-index:1}
.tab .badge{top:2px;right:calc(50% - 22px);box-shadow:0 0 0 2.5px var(--tabbar-bg)}

/* ---------- Toast ---------- */
.toast{
  position:fixed;left:50%;
  bottom:calc(120px + env(safe-area-inset-bottom));
  transform:translate(-50%,16px);
  padding:10px 20px;border-radius:24px;
  background:rgba(0,0,0,.82);color:#fff;
  font-size:14px;font-weight:500;letter-spacing:-.1px;white-space:nowrap;
  opacity:0;pointer-events:none;z-index:999;
  transition:opacity .26s ease, transform .26s cubic-bezier(.22,1,.36,1);
  backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);
}
.toast.show{opacity:1;transform:translate(-50%,0)}
</style>
</head>
<body>

<div class="app">

  <!-- 状态栏（始终在最上层） -->
  <div class="statusbar">
    <span>9:41</span>
    <span class="status-icons">
      <svg width="18" height="12" viewBox="0 0 18 12" fill="currentColor">
        <rect x="0" y="8" width="3" height="4" rx="1"/>
        <rect x="5" y="5.5" width="3" height="6.5" rx="1"/>
        <rect x="10" y="3" width="3" height="9" rx="1"/>
        <rect x="15" y="0.5" width="3" height="11.5" rx="1" opacity=".35"/>
      </svg>
      <svg width="16" height="12" viewBox="0 0 16 12" fill="none">
        <path d="M8 11.2 5.7 8.7a3.4 3.4 0 0 1 4.6 0L8 11.2Z" fill="currentColor"/>
        <path d="M3.5 6.4a6.5 6.5 0 0 1 9 0" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
        <path d="M1.1 3.9a9.9 9.9 0 0 1 13.8 0" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/>
      </svg>
      <svg width="25" height="12" viewBox="0 0 25 12" fill="none">
        <rect x="0.6" y="0.6" width="21" height="10.8" rx="3.4" stroke="currentColor" stroke-opacity=".35"/>
        <rect x="2.2" y="2.2" width="16" height="7.6" rx="2" fill="currentColor"/>
        <path d="M23.2 4.1v3.8a2.4 2.4 0 0 0 0-3.8Z" fill="currentColor" fill-opacity=".4"/>
      </svg>
    </span>
  </div>

  <!-- ============ 首页 ============ -->
  <section class="page active" id="page-home">
    <header class="header">
      <div class="header-row">
        <h1>商城</h1>
        <button class="icon-btn" data-open="search" aria-label="搜索">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round">
            <circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/>
          </svg>
        </button>
        <button class="icon-btn" data-goto="cart" aria-label="购物车">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
            <path d="M6 8h12l.9 11.1a1.5 1.5 0 0 1-1.5 1.6H6.6a1.5 1.5 0 0 1-1.5-1.6L6 8Z"/>
            <path d="M9 8V6.5a3 3 0 0 1 6 0V8"/>
          </svg>
          <span class="badge" id="badgeHead">0</span>
        </button>
      </div>

      <div class="searchbar" data-open="search">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round">
          <circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/>
        </svg>
        <input type="search" placeholder="搜索商品、品牌" readonly>
      </div>

      <div class="chips" id="chips"></div>
    </header>

    <div class="scroll">
      <section class="banner">
        <span class="banner-tag">限时特惠</span>
        <h2>秋季新品<br>低至 5 折</h2>
        <p>满 ¥299 免运费 · 会员额外 9 折</p>
        <div class="deco deco-1"></div>
        <div class="deco deco-2"></div>
      </section>

      <div class="section-head">
        <h3>为你推荐</h3>
        <span id="count"></span>
      </div>

      <div class="grid" id="grid"></div>
    </div>
  </section>

  <!-- ============ 分类页 ============ -->
  <section class="page" id="page-category">
    <header class="header">
      <div class="header-row">
        <h1>分类</h1>
      </div>
    </header>

    <div class="cat-layout">
      <div class="cat-side" id="catSide"></div>
      <div class="cat-main" id="catMain"></div>
    </div>
  </section>

  <!-- ============ 购物车页 ============ -->
  <section class="page" id="page-cart">
    <header class="header">
      <div class="header-row">
        <h1>购物车</h1>
        <span class="sub" id="cartSummary"></span>
      </div>
    </header>

    <div class="scroll" id="cartScroll"></div>

    <div class="checkout-bar" id="checkoutBar">
      <div class="checkout-total">
        <span class="lbl">合计（含运费）</span>
        <span class="amt"><span class="cur">¥</span><span id="totalAmt">0</span></span>
      </div>
      <button class="pay-btn" id="payBtn">去结算</button>
    </div>
  </section>

  <!-- ============ 我的页 ============ -->
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
        <div class="profile-card" data-open="settings">
          <div class="avatar">L</div>
          <div class="profile-info">
            <h2>Lisa Wong</h2>
            <p>黄金会员 · 积分 2,480</p>
          </div>
          <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
        </div>

        <div class="stats">
          <div class="stat" data-open="orders"><div class="num">12</div><div class="lbl">待付款</div></div>
          <div class="stat" data-open="orders"><div class="num">3</div><div class="lbl">待收货</div></div>
          <div class="stat" data-toast="优惠券"><div class="num">28</div><div class="lbl">优惠券</div></div>
        </div>

        <div class="group-title">我的订单</div>
        <div class="list">
          <div class="list-item" data-open="orders">
            <span class="list-ico b">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <path d="M5 3.5h14a1 1 0 0 1 1 1V21l-3-2-2 2-2-2-2 2-2-2-3 2V4.5a1 1 0 0 1 1-1Z"/>
                <path d="M9 8.5h6M9 12.5h6"/>
              </svg>
            </span>
            <span class="list-text">全部订单</span>
            <span class="list-val">12</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-toast="退款 / 售后">
            <span class="list-ico o">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <path d="M3 12a9 9 0 1 0 3-6.7"/><path d="M3 4.5V10h5.5"/>
              </svg>
            </span>
            <span class="list-text">退款 / 售后</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-open="address">
            <span class="list-ico g">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <path d="M12 21s7-5.6 7-11a7 7 0 1 0-14 0c0 5.4 7 11 7 11Z"/>
                <circle cx="12" cy="10" r="2.6"/>
              </svg>
            </span>
            <span class="list-text">收货地址</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
        </div>

        <div class="group-title">更多服务</div>
        <div class="list">
          <div class="list-item" data-toast="我的收藏">
            <span class="list-ico r">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <path d="M12 20.3 4.8 13a4.7 4.7 0 0 1 6.6-6.7l.6.6.6-.6a4.7 4.7 0 0 1 6.6 6.7L12 20.3Z"/>
              </svg>
            </span>
            <span class="list-text">我的收藏</span>
            <span class="list-val">36</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-toast="浏览历史">
            <span class="list-ico p">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="8.5"/><path d="M12 7.5V12l3 1.8"/>
              </svg>
            </span>
            <span class="list-text">浏览历史</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-toast="帮助与客服">
            <span class="list-ico t">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="8.5"/>
                <path d="M9.6 9.4a2.5 2.5 0 1 1 3.6 2.2c-.7.4-1.2 1-1.2 1.8v.3"/>
                <circle cx="12" cy="16.6" r=".9" fill="currentColor" stroke="none"/>
              </svg>
            </span>
            <span class="list-text">帮助与客服</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
          <div class="list-item" data-open="settings">
            <span class="list-ico pk">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round">
                <path d="M4 6h16M4 12h16M4 18h10"/>
              </svg>
            </span>
            <span class="list-text">设置</span>
            <span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ 商品详情页 ============ -->
  <section class="page overlay" id="page-detail">
    <button class="nav-back" data-back aria-label="返回">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
        <path d="m15 5-7 7 7 7"/>
      </svg>
    </button>
    <div class="scroll" id="detailScroll" style="padding-bottom:120px"></div>
    <div class="detail-bar" id="detailBar">
      <button class="icon-btn" data-toast="已收藏" aria-label="收藏">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 20.3 4.8 13a4.7 4.7 0 0 1 6.6-6.7l.6.6.6-.6a4.7 4.7 0 0 1 6.6 6.7L12 20.3Z"/>
        </svg>
      </button>
      <button class="icon-btn" data-goto="cart" aria-label="购物车">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
          <path d="M6 8h12l.9 11.1a1.5 1.5 0 0 1-1.5 1.6H6.6a1.5 1.5 0 0 1-1.5-1.6L6 8Z"/>
          <path d="M9 8V6.5a3 3 0 0 1 6 0V8"/>
        </svg>
      </button>
      <button class="pay-btn" id="detailAdd">加入购物车</button>
    </div>
  </section>

  <!-- ============ 搜索页 ============ -->
  <section class="page overlay" id="page-search">
    <div class="search-head">
      <div class="searchbar">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round">
          <circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/>
        </svg>
        <input type="search" id="searchInput" placeholder="搜索商品、品牌" enterkeyhint="search" autocomplete="off">
      </div>
      <button class="cancel" data-back>取消</button>
    </div>

    <div class="scroll" id="searchScroll">
      <div class="section-head" style="padding-top:18px">
        <h3 style="font-size:15px">热门搜索</h3>
      </div>
      <div class="search-tags">
        <button class="search-tag hot" data-q="AirPods">AirPods</button>
        <button class="search-tag hot" data-q="口红">口红</button>
        <button class="search-tag" data-q="咖啡">咖啡</button>
        <button class="search-tag" data-q="T恤">T恤</button>
        <button class="search-tag" data-q="手表">手表</button>
        <button class="search-tag" data-q="香薰">香薰</button>
        <button class="search-tag" data-q="精华">精华</button>
        <button class="search-tag" data-q="马克杯">马克杯</button>
      </div>

      <div id="searchResult"></div>
    </div>
  </section>

  <!-- ============ 订单页 ============ -->
  <section class="page overlay" id="page-orders">
    <button class="nav-back" data-back aria-label="返回">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
        <path d="m15 5-7 7 7 7"/>
      </svg>
    </button>
    <header class="header" style="padding-top:calc(env(safe-area-inset-top) + 44px)">
      <div class="header-row"><h1 style="font-size:22px">我的订单</h1></div>
      <div class="order-tabs" id="orderTabs">
        <button class="order-tab active" data-status="all">全部</button>
        <button class="order-tab" data-status="wait">待付款</button>
        <button class="order-tab" data-status="ship">待发货</button>
        <button class="order-tab" data-status="recv">待收货</button>
        <button class="order-tab" data-status="done">已完成</button>
      </div>
    </header>
    <div class="scroll" id="orderList"></div>
  </section>

  <!-- ============ 地址页 ============ -->
  <section class="page overlay" id="page-address">
    <button class="nav-back" data-back aria-label="返回">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
        <path d="m15 5-7 7 7 7"/>
      </svg>
    </button>
    <header class="header" style="padding-top:calc(env(safe-area-inset-top) + 44px)">
      <div class="header-row"><h1 style="font-size:22px">收货地址</h1></div>
    </header>
    <div class="scroll" id="addrList"></div>
    <button class="addr-add" data-toast="新建地址">＋ 新建收货地址</button>
  </section>

  <!-- ============ 设置页 ============ -->
  <section class="page overlay" id="page-settings">
    <button class="nav-back" data-back aria-label="返回">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
        <path d="m15 5-7 7 7 7"/>
      </svg>
    </button>
    <header class="header" style="padding-top:calc(env(safe-area-inset-top) + 44px)">
      <div class="header-row"><h1 style="font-size:22px">设置</h1></div>
    </header>
    <div class="scroll">
      <div style="padding:0 16px">
        <div class="group-title">账号</div>
        <div class="list">
          <div class="setting-item" data-toast="个人资料"><span class="setting-text">个人资料</span><span class="setting-val">Lisa Wong</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item" data-toast="修改密码"><span class="setting-text">修改密码</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item" data-toast="绑定手机"><span class="setting-text">绑定手机</span><span class="setting-val">138****8888</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
        </div>

        <div class="group-title">通用</div>
        <div class="list">
          <div class="setting-item"><span class="setting-text">消息推送</span><div class="switch on" data-switch></div></div>
          <div class="setting-item"><span class="setting-text">Wi-Fi 下自动加载图片</span><div class="switch on" data-switch></div></div>
          <div class="setting-item"><span class="setting-text">深色模式跟随系统</span><div class="switch on" data-switch></div></div>
          <div class="setting-item" data-toast="清除缓存"><span class="setting-text">清除缓存</span><span class="setting-val">32.6 MB</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
        </div>

        <div class="group-title">关于</div>
        <div class="list">
          <div class="setting-item" data-toast="版本 1.0.0"><span class="setting-text">版本</span><span class="setting-val">1.0.0</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item" data-toast="用户协议"><span class="setting-text">用户协议</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
          <div class="setting-item" data-toast="隐私政策"><span class="setting-text">隐私政策</span><span class="chev"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg></span></div>
        </div>

        <button class="addr-add" style="border:none;background:var(--card);color:var(--red);margin-top:20px;border-radius:16px;box-shadow:var(--shadow)" data-toast="已退出登录">退出登录</button>
      </div>
    </div>
  </section>

  <!-- 玻璃质感悬浮 Tab -->
  <nav class="tabbar" id="tabbar">
    <button class="tab active" data-tab="home">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
        <path d="M3.2 10.6 12 3.4l8.8 7.2"/>
        <path d="M5.6 9.4V20a1 1 0 0 0 1 1h10.8a1 1 0 0 0 1-1V9.4"/>
      </svg>
      <span>首页</span>
    </button>
    <button class="tab" data-tab="category">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7">
        <rect x="3.6" y="3.6" width="7" height="7" rx="2.2"/>
        <rect x="13.4" y="3.6" width="7" height="7" rx="2.2"/>
        <rect x="3.6" y="13.4" width="7" height="7" rx="2.2"/>
        <rect x="13.4" y="13.4" width="7" height="7" rx="2.2"/>
      </svg>
      <span>分类</span>
    </button>
    <button class="tab" data-tab="cart">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round">
        <path d="M6 8h12l.9 11.1a1.5 1.5 0 0 1-1.5 1.6H6.6a1.5 1.5 0 0 1-1.5-1.6L6 8Z"/>
        <path d="M9 8V6.5a3 3 0 0 1 6 0V8"/>
      </svg>
      <span>购物车</span>
      <span class="badge" id="badgeCart">0</span>
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
</div>

<script>
(function () {
  'use strict';

  /* ---------------- 数据 ---------------- */
  const CATS = ['全部', '数码', '服饰', '家居', '美妆', '食品'];

  const PRODUCTS = [
    { id: 1,  cat: '数码', name: 'AirPods Pro 2 无线降噪耳机', desc: '主动降噪 · 无线充电', price: 1899, emoji: '🎧', tag: '热卖', g: ['#E9F1FF', '#CFE0FF'], specs: ['标准版', '刻字版'], rating: '4.9', sold: '2.3万' },
    { id: 2,  cat: '数码', name: 'MagSafe 磁吸无线充电器',     desc: '15W 快充 · 兼容全系', price: 329,  emoji: '🔌', tag: '',     g: ['#F1ECFF', '#DCD2FF'], specs: ['白色', '黑色'], rating: '4.8', sold: '8,912' },
    { id: 3,  cat: '服饰', name: '纯棉基础款圆领 T 恤',         desc: '多色可选 · 亲肤透气', price: 129,  emoji: '👕', tag: '新品', g: ['#EAF7F0', '#CDEEDC'], specs: ['S', 'M', 'L', 'XL'], rating: '4.7', sold: '1.6万' },
    { id: 4,  cat: '家居', name: '北欧香薰蜡烛礼盒',             desc: '雪松与柑橘调 · 200g', price: 168,  emoji: '🕯️', tag: '',     g: ['#FFF4E6', '#FFE2C4'], specs: ['单盒装', '双盒装'], rating: '4.9', sold: '5,231' },
    { id: 5,  cat: '美妆', name: '玻尿酸保湿精华液 30ml',        desc: '深层补水 · 敏感肌可用', price: 299,  emoji: '🧴', tag: '爆款', g: ['#FFEAF2', '#FFD2E2'], specs: ['30ml', '50ml'], rating: '4.8', sold: '9,876' },
    { id: 6,  cat: '食品', name: '手冲挂耳咖啡 10 包装',         desc: '中深烘 · 坚果可可调', price: 89,   emoji: '☕', tag: '',     g: ['#F6EEE6', '#E7D6C3'], specs: ['中深烘', '浅烘'], rating: '4.9', sold: '3.2万' },
    { id: 7,  cat: '数码', name: '智能手表 Series 9 GPS 版',     desc: '血氧监测 · 全天续航', price: 2999, emoji: '⌚', tag: '',     g: ['#E9F1FF', '#CFE0FF'], specs: ['41mm', '45mm'], rating: '4.9', sold: '6,120' },
    { id: 8,  cat: '家居', name: '哑光釉面陶瓷马克杯',           desc: '380ml · 微波炉可用', price: 79,   emoji: '🥛', tag: '',     g: ['#EFF4F8', '#D8E4EE'], specs: ['白', '灰', '蓝'], rating: '4.7', sold: '1.1万' },
    { id: 9,  cat: '服饰', name: '轻量防风冲锋外套',             desc: '防泼水 · 可收纳',     price: 459,  emoji: '🧥', tag: '',     g: ['#EDF1F6', '#D3DDE9'], specs: ['M', 'L', 'XL'], rating: '4.8', sold: '4,320' },
    { id: 10, cat: '美妆', name: '丝绒哑光唇釉 #06',             desc: '不沾杯 · 持妆 8 小时', price: 159,  emoji: '💄', tag: '',     g: ['#FFEAF2', '#FFD2E2'], specs: ['#06 干枯玫瑰', '#09 焦糖红棕'], rating: '4.8', sold: '7,654' }
  ];

  const $ = id => document.getElementById(id);
  const grid = $('grid'), chips = $('chips'), tabbar = $('tabbar'), toastEl = $('toast');
  const badgeHead = $('badgeHead'), badgeCart = $('badgeCart');
  const cartScroll = $('cartScroll'), checkoutBar = $('checkoutBar'), totalAmt = $('totalAmt');
  const cartSummary = $('cartSummary');
  const catSide = $('catSide'), catMain = $('catMain');

  const fmt = n => n.toLocaleString('zh-CN');

  /* ---------------- 页面栈 ---------------- */
  const pageStack = [];
  const mainPages = ['home', 'category', 'cart', 'me'];

  function showPage(name) {
    document.querySelectorAll('.page').forEach(p => p.classList.toggle('active', p.id === 'page-' + name));
    const isOverlay = !mainPages.includes(name);
    tabbar.style.display = isOverlay ? 'none' : 'flex';
    if (isOverlay) pageStack.push(name);
  }

  function closePage() {
    const name = pageStack.pop();
    if (!name) return;
    document.getElementById('page-' + name).classList.remove('active');
    const prev = pageStack.length ? pageStack[pageStack.length - 1] : null;
    // 主页面栈底永远保留
    if (mainPages.includes(name)) {
      showPage(name);
      return;
    }
    if (prev && !mainPages.includes(prev)) {
      document.getElementById('page-' + prev).classList.add('active');
    } else {
      // 回到当前主 Tab
      const cur = document.querySelector('.tab.active')?.dataset.tab || 'home';
      document.getElementById('page-' + cur).classList.add('active');
      tabbar.style.display = 'flex';
    }
  }

  /* ---------------- 首页 ---------------- */
  chips.innerHTML = CATS
    .map((c, i) => `<button class="chip${i === 0 ? ' active' : ''}" data-cat="${c}">${c}</button>`)
    .join('');

  function renderHome(list) {
    grid.innerHTML = list.map((p, i) => `
      <article class="card" data-id="${p.id}" style="animation-delay:${Math.min(i, 8) * 45}ms">
        <div class="thumb" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
          ${p.tag ? `<span class="tag">${p.tag}</span>` : ''}
          <span class="emoji">${p.emoji}</span>
        </div>
        <div class="info">
          <div class="name">${p.name}</div>
          <div class="desc">${p.desc}</div>
          <div class="row">
            <div class="price"><span class="cur">¥</span>${fmt(p.price)}</div>
            <button class="add" data-add="${p.id}" aria-label="加入购物车">
              <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
                <path d="M7 1.6v10.8M1.6 7h10.8" stroke="currentColor" stroke-width="2.1" stroke-linecap="round"/>
              </svg>
            </button>
          </div>
        </div>
      </article>
    `).join('');
    $('count').textContent = list.length + ' 件商品';
  }
  renderHome(PRODUCTS);

  /* ---------------- 分类页 ---------------- */
  let curCat = CATS[1];

  function renderCatSide() {
    catSide.innerHTML = CATS.slice(1).map(c =>
      `<button class="cat-item${c === curCat ? ' active' : ''}" data-cat="${c}">${c}</button>`
    ).join('');
  }

  function renderCatMain() {
    const list = PRODUCTS.filter(p => p.cat === curCat);
    catMain.innerHTML = `
      <h3 class="cat-title">${curCat}<small>${list.length} 件</small></h3>
      <div class="cat-grid">
        ${list.map(p => `
          <article class="cat-card" data-id="${p.id}">
            <div class="thumb" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
              <span class="emoji">${p.emoji}</span>
            </div>
            <div class="info">
              <div class="name">${p.name}</div>
              <div class="price"><span class="cur">¥</span>${fmt(p.price)}</div>
            </div>
          </article>
        `).join('')}
      </div>
    `;
  }
  renderCatSide();
  renderCatMain();

  /* ---------------- 购物车 ---------------- */
  let cart = [];

  const cartCount = () => cart.reduce((s, i) => s + i.qty, 0);
  const cartTotal = () => cart.reduce((s, i) => {
    const p = PRODUCTS.find(x => x.id === i.id);
    return s + (p ? p.price * i.qty : 0);
  }, 0);

  function renderCart() {
    const count = cartCount();
    cartSummary.textContent = count ? `共 ${count} 件` : '';

    if (!count) {
      cartScroll.innerHTML = `
        <div class="empty">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
              <path d="M6 8h12l.9 11.1a1.5 1.5 0 0 1-1.5 1.6H6.6a1.5 1.5 0 0 1-1.5-1.6L6 8Z"/>
              <path d="M9 8V6.5a3 3 0 0 1 6 0V8"/>
            </svg>
          </div>
          <h4>购物车还是空的</h4>
          <p>快去挑选心仪的商品<br>满 ¥299 免运费</p>
          <button data-goto="home">去逛逛</button>
        </div>`;
      checkoutBar.classList.remove('show');
      return;
    }

    cartScroll.innerHTML = `
      <div class="cart-list">
        ${cart.map((item, i) => {
          const p = PRODUCTS.find(x => x.id === item.id);
          if (!p) return '';
          return `
            <div class="cart-item" style="animation-delay:${Math.min(i, 6) * 40}ms">
              <div class="cart-thumb" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
                <span class="emoji">${p.emoji}</span>
              </div>
              <div class="cart-body">
                <div class="cart-name">${p.name}</div>
                <div class="cart-desc">${p.desc}</div>
                <div class="cart-foot">
                  <div class="cart-price"><span class="cur">¥</span>${fmt(p.price * item.qty)}</div>
                  <div class="stepper">
                    <button data-step="-1" data-id="${p.id}" aria-label="减少">−</button>
                    <span class="qty">${item.qty}</span>
                    <button data-step="1" data-id="${p.id}" aria-label="增加">+</button>
                  </div>
                </div>
              </div>
            </div>`;
        }).join('')}
      </div>`;

    totalAmt.textContent = fmt(cartTotal());
    checkoutBar.classList.add('show');
  }

  function addToCart(id, btn) {
    const item = cart.find(i => i.id === id);
    if (item) item.qty++;
    else cart.push({ id, qty: 1 });

    updateBadges();
    renderCart();

    if (btn) {
      btn.classList.remove('pop');
      void btn.offsetWidth;
      btn.classList.add('pop');
    }
    showToast('已加入购物车');
  }

  function updateBadges() {
    const n = cartCount();
    const show = n > 0;
    [badgeHead, badgeCart].forEach(b => {
      if (!b) return;
      b.textContent = n > 99 ? '99+' : n;
      b.classList.toggle('show', show);
    });
  }

  /* ---------------- 商品详情 ---------------- */
  let detailProduct = null;
  let detailSpec = 0;

  function openDetail(id) {
    const p = PRODUCTS.find(x => x.id === id);
    if (!p) return;
    detailProduct = p;
    detailSpec = 0;

    $('detailScroll').innerHTML = `
      <div class="detail-hero" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
        ${p.tag ? `<span class="tag">${p.tag}</span>` : ''}
        <span class="emoji">${p.emoji}</span>
      </div>
      <div class="detail-body">
        <div class="detail-price"><span class="cur">¥</span>${fmt(p.price)}</div>
        <h2 class="detail-name">${p.name}</h2>
        <p class="detail-desc">${p.desc}</p>
        <div class="detail-meta">
          <span class="meta-pill">⭐ ${p.rating} 分</span>
          <span class="meta-pill">已售 ${p.sold}</span>
          <span class="meta-pill">24h 发货</span>
          <span class="meta-pill">7 天无理由</span>
        </div>

        <div class="detail-block">
          <h4>选择规格</h4>
          <div class="spec-row" id="specRow">
            ${p.specs.map((s, i) => `<button class="spec${i === 0 ? ' active' : ''}" data-spec="${i}">${s}</button>`).join('')}
          </div>
        </div>

        <div class="detail-block">
          <h4>商品详情</h4>
          <p>${p.name}，${p.desc}。精选优质材料，做工精细，品质可靠。支持 7 天无理由退换货，全国联保，让您购物无忧。</p>
        </div>

        <div class="detail-block">
          <h4>服务保障</h4>
          <p>· 正品保证，假一赔十<br>· 满 ¥299 免运费<br>· 会员额外 9 折</p>
        </div>
      </div>
    `;
    showPage('detail');
  }

  /* ---------------- 搜索 ---------------- */
  function renderSearch(q) {
    const query = (q || '').trim().toLowerCase();
    const result = $('searchResult');
    if (!query) { result.innerHTML = ''; return; }

    const list = PRODUCTS.filter(p =>
      p.name.toLowerCase().includes(query) || p.desc.toLowerCase().includes(query) || p.cat.includes(query)
    );

    if (!list.length) {
      result.innerHTML = `
        <div class="empty" style="padding:50px 40px">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round">
              <circle cx="11" cy="11" r="7"/><path d="m20 20-3.5-3.5"/>
            </svg>
          </div>
          <h4>没有找到相关商品</h4>
          <p>换个关键词试试</p>
        </div>`;
      return;
    }

    result.innerHTML = `
      <div class="section-head" style="padding-top:18px">
        <h3 style="font-size:15px">搜索结果</h3>
        <span>${list.length} 件</span>
      </div>
      <div class="grid" style="padding-top:12px">
        ${list.map(p => `
          <article class="card" data-id="${p.id}">
            <div class="thumb" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
              ${p.tag ? `<span class="tag">${p.tag}</span>` : ''}
              <span class="emoji">${p.emoji}</span>
            </div>
            <div class="info">
              <div class="name">${p.name}</div>
              <div class="desc">${p.desc}</div>
              <div class="row">
                <div class="price"><span class="cur">¥</span>${fmt(p.price)}</div>
                <button class="add" data-add="${p.id}" aria-label="加入购物车">
                  <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
                    <path d="M7 1.6v10.8M1.6 7h10.8" stroke="currentColor" stroke-width="2.1" stroke-linecap="round"/>
                  </svg>
                </button>
              </div>
            </div>
          </article>
        `).join('')}
      </div>`;
  }

  /* ---------------- 订单 ---------------- */
  const ORDERS = [
    { no: '202409260001', status: 'wait', stText: '待付款', cls: 'wait', pid: 1, qty: 1, time: '2024-09-26 10:23' },
    { no: '202409250012', status: 'ship', stText: '待发货', cls: '', pid: 5, qty: 2, time: '2024-09-25 18:40' },
    { no: '202409240088', status: 'recv', stText: '待收货', cls: '', pid: 6, qty: 3, time: '2024-09-24 09:12' },
    { no: '202409200156', status: 'done', stText: '已完成', cls: 'done', pid: 7, qty: 1, time: '2024-09-20 15:35' },
    { no: '202409180023', status: 'done', stText: '已完成', cls: 'done', pid: 4, qty: 1, time: '2024-09-18 11:02' },
    { no: '202409150077', status: 'done', stText: '已完成', cls: 'done', pid: 3, qty: 2, time: '2024-09-15 20:18' }
  ];

  let orderFilter = 'all';

  function renderOrders() {
    const list = orderFilter === 'all' ? ORDERS : ORDERS.filter(o => o.status === orderFilter);
    const box = $('orderList');

    if (!list.length) {
      box.innerHTML = `
        <div class="empty">
          <div class="ico">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round">
              <path d="M5 3.5h14a1 1 0 0 1 1 1V21l-3-2-2 2-2-2-2 2-2-2-3 2V4.5a1 1 0 0 1 1-1Z"/>
            </svg>
          </div>
          <h4>暂无相关订单</h4>
          <p>去逛逛，看看有什么想买的</p>
          <button data-goto="home">去逛逛</button>
        </div>`;
      return;
    }

    box.innerHTML = list.map(o => {
      const p = PRODUCTS.find(x => x.id === o.pid);
      if (!p) return '';
      return `
        <div class="order-card">
          <div class="order-top">
            <span class="order-no">订单号 ${o.no}</span>
            <span class="order-status ${o.cls}">${o.stText}</span>
          </div>
          <div class="order-goods">
            <div class="order-thumb" style="background:linear-gradient(135deg,${p.g[0]},${p.g[1]})">
              <span class="emoji">${p.emoji}</span>
            </div>
            <div class="order-info">
              <div class="name">${p.name}</div>
              <div class="sub">×${o.qty} · ${o.time}</div>
              <div class="amt"><span class="cur">¥</span>${fmt(p.price * o.qty)}</div>
            </div>
          </div>
          <div class="order-actions">
            ${o.status === 'wait'
              ? `<button class="order-btn ghost" data-toast="已取消订单">取消订单</button>
                 <button class="order-btn primary" data-toast="去付款">去付款</button>`
              : o.status === 'ship'
              ? `<button class="order-btn ghost" data-toast="提醒发货">提醒发货</button>`
              : o.status === 'recv'
              ? `<button class="order-btn ghost" data-toast="查看物流">查看物流</button>
                 <button class="order-btn primary" data-toast="确认收货">确认收货</button>`
              : `<button class="order-btn ghost" data-toast="再次购买">再次购买</button>
                 <button class="order-btn primary" data-toast="评价晒单">评价晒单</button>`}
          </div>
        </div>`;
    }).join('');
  }

  /* ---------------- 地址 ---------------- */
  const ADDRESSES = [
    { name: 'Lisa Wong', phone: '138****8888', text: '上海市浦东新区陆家嘴街道世纪大道 100 号 环球金融中心 88 层', def: true },
    { name: 'Lisa Wong', phone: '138****8888', text: '北京市朝阳区建国路 88 号 SOHO 现代城 B 座 1801', def: false }
  ];

  function renderAddress() {
    $('addrList').innerHTML = ADDRESSES.map(a => `
      <div class="addr-card">
        <div class="addr-head">
          <span class="addr-name">${a.name}</span>
          <span class="addr-phone">${a.phone}</span>
          ${a.def ? '<span class="addr-default">默认</span>' : ''}
        </div>
        <div class="addr-text">${a.text}</div>
        <button class="addr-edit" data-toast="编辑地址">
          编辑
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 5 7 7-7 7"/></svg>
        </button>
      </div>
    `).join('');
  }

  /* ---------------- Toast ---------------- */
  let toastTimer;
  function showToast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(() => toastEl.classList.remove('show'), 1400);
  }

  /* ---------------- 事件委托 ---------------- */
  document.addEventListener('click', e => {
    const t = e.target;

    // 返回
    if (t.closest('[data-back]')) { closePage(); return; }

    // 打开浮层
    const opener = t.closest('[data-open]');
    if (opener) {
      const name = opener.dataset.open;
      if (name === 'orders') { orderFilter = 'all'; renderOrders(); syncOrderTabs(); }
      if (name === 'address') renderAddress();
      if (name === 'search') {
        renderSearch('');
        $('searchInput').value = '';
        setTimeout(() => $('searchInput').focus(), 320);
      }
      showPage(name);
      return;
    }

    // 跳主 Tab
    const goto = t.closest('[data-goto]');
    if (goto) { switchTab(goto.dataset.goto); return; }

    // 加购
    const addBtn = t.closest('[data-add]');
    if (addBtn) {
      e.stopPropagation();
      addToCart(+addBtn.dataset.add, addBtn);
      return;
    }

    // 打开详情
    const card = t.closest('.card, .cat-card');
    if (card && card.dataset.id) { openDetail(+card.dataset.id); return; }

    // 通用 Toast
    const toastElTrigger = t.closest('[data-toast]');
    if (toastElTrigger) { showToast(toastElTrigger.dataset.toast); return; }
  });

  // 首页分类 chips
  chips.addEventListener('click', e => {
    const chip = e.target.closest('.chip');
    if (!chip) return;
    chips.querySelectorAll('.chip').forEach(c => c.classList.toggle('active', c === chip));
    const cat = chip.dataset.cat;
    renderHome(cat === '全部' ? PRODUCTS : PRODUCTS.filter(p => p.cat === cat));
  });

  // 分类侧栏
  catSide.addEventListener('click', e => {
    const btn = e.target.closest('.cat-item');
    if (!btn) return;
    curCat = btn.dataset.cat;
    catSide.querySelectorAll('.cat-item').forEach(b => b.classList.toggle('active', b === btn));
    renderCatMain();
    catMain.scrollTop = 0;
  });

  // 购物车增减
  cartScroll.addEventListener('click', e => {
    const stepBtn = e.target.closest('[data-step]');
    if (stepBtn) {
      const id = +stepBtn.dataset.id;
      const step = +stepBtn.dataset.step;
      const item = cart.find(i => i.id === id);
      if (!item) return;
      item.qty += step;
      if (item.qty <= 0) cart = cart.filter(i => i.id !== id);
      updateBadges();
      renderCart();
      return;
    }
    const goto = e.target.closest('[data-goto]');
    if (goto) switchTab(goto.dataset.goto);
  });

  // 详情规格
  $('detailScroll').addEventListener('click', e => {
    const spec = e.target.closest('.spec');
    if (!spec) return;
    detailSpec = +spec.dataset.spec;
    $('detailScroll').querySelectorAll('.spec').forEach(s => s.classList.toggle('active', s === spec));
  });

  // 详情加购
  $('detailAdd').addEventListener('click', () => {
    if (!detailProduct) return;
    addToCart(detailProduct.id, null);
  });

  // 结算
  $('payBtn').addEventListener('click', () => {
    if (!cartCount()) return;
    showToast(`已提交订单 ¥${fmt(cartTotal())}`);
  });

  // 搜索
  $('searchInput').addEventListener('input', e => renderSearch(e.target.value));
  $('searchInput').addEventListener('keydown', e => {
    if (e.key === 'Enter') { e.preventDefault(); e.target.blur(); renderSearch(e.target.value); }
  });
  $('searchScroll').addEventListener('click', e => {
    const tag = e.target.closest('.search-tag');
    if (tag) {
      $('searchInput').value = tag.dataset.q;
      renderSearch(tag.dataset.q);
    }
  });

  // 订单筛选
  $('orderTabs').addEventListener('click', e => {
    const tab = e.target.closest('.order-tab');
    if (!tab) return;
    orderFilter = tab.dataset.status;
    syncOrderTabs();
    renderOrders();
  });
  function syncOrderTabs() {
    $('orderTabs').querySelectorAll('.order-tab').forEach(t =>
      t.classList.toggle('active', t.dataset.status === orderFilter)
    );
  }

  // 设置开关
  document.addEventListener('click', e => {
    const sw = e.target.closest('[data-switch]');
    if (sw) sw.classList.toggle('on');
  });

  /* ---------------- Tab 切换 ---------------- */
  function switchTab(name) {
    // 关闭所有浮层
    pageStack.length = 0;
    document.querySelectorAll('.page.overlay').forEach(p => p.classList.remove('active'));
    tabbar.style.display = 'flex';

    tabbar.querySelectorAll('.tab').forEach(t => t.classList.toggle('active', t.dataset.tab === name));
    document.querySelectorAll('.page').forEach(p => p.classList.toggle('active', p.id === 'page-' + name));

    if (name === 'cart') renderCart();
    if (name === 'category') { renderCatSide(); renderCatMain(); }
  }

  tabbar.addEventListener('click', e => {
    const tab = e.target.closest('.tab');
    if (!tab) return;
    switchTab(tab.dataset.tab);
  });

  /* ---------------- 初始化 ---------------- */
  updateBadges();
  renderCart();

})();
</script>
</body>
</html>
