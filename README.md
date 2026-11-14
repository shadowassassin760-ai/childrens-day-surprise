<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#ffd6ea">
<title>Her Little Funfair 🎡</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Pacifico&display=swap" rel="stylesheet">
<style>
:root{--pink:#ffb7d9;--pink2:#ff7fbc;--lav:#d7c3ff;--lav2:#a98bff;--blue:#bfe6ff;--blue2:#63b8ff;--peach:#ffd3b0;--yel:#fff1a6;--yel2:#ffd45e;--mint:#c8f5df;--ink:#5b3a7a;--ink2:#8c6aa8;--sh:0 10px 26px rgba(150,100,200,.28)}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden;background:#e9ddff}
body{font-family:'Fredoka',system-ui,sans-serif;color:var(--ink);-webkit-user-select:none;user-select:none;touch-action:manipulation;overscroll-behavior:none}
button{font-family:inherit;color:inherit;border:0;background:none;cursor:pointer;touch-action:manipulation}
.script{font-family:'Pacifico',cursive;font-weight:400}
#stage{position:fixed;inset:0;overflow:hidden}
.screen{position:absolute;inset:0;display:none;flex-direction:column;align-items:center;padding:76px 14px calc(20px + env(safe-area-inset-bottom));overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch}
.screen.active{display:flex;animation:scrIn .5s both}
@keyframes scrIn{from{opacity:0;transform:scale(1.04)}to{opacity:1;transform:none}}
@keyframes pulse{50%{scale:1.06}}
@keyframes wob{0%,100%{rotate:-7deg}50%{rotate:7deg}}
@keyframes bob{0%,100%{translate:0 0}50%{translate:0 -6px}}
@keyframes drift{from{translate:-14px 0}to{translate:22px 0}}
@keyframes spinS{to{transform:rotate(360deg)}}
@keyframes spinR{to{transform:rotate(-360deg)}}
@keyframes blink{50%{opacity:.25}}
@keyframes pop{0%{opacity:0;transform:scale(.6) translateY(20px)}100%{opacity:1;transform:none}}
@keyframes rise{from{translate:0 0}to{translate:0 -130vh}}
@keyframes swayx{from{transform:translateX(-12px)}to{transform:translateX(12px)}}
@keyframes twinkle{0%,100%{opacity:.25;scale:.7}50%{opacity:1;scale:1.2}}

/* buttons & shared bits */
.btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:13px 26px;border-radius:999px;font-weight:700;font-size:1.05rem;color:#fff;background:linear-gradient(180deg,#ff9fcf,#ff6fb0);box-shadow:0 6px 0 #d94f93,0 14px 24px rgba(255,111,176,.4);transition:transform .12s,box-shadow .12s;position:relative;z-index:2}
.btn:active{transform:translateY(5px);box-shadow:0 1px 0 #d94f93,0 6px 12px rgba(255,111,176,.4)}
.btn.blue{background:linear-gradient(180deg,#8fd0ff,#52a8f5);box-shadow:0 6px 0 #2f82cc,0 14px 24px rgba(82,168,245,.4)}
.btn.blue:active{box-shadow:0 1px 0 #2f82cc}
.btn.yel{background:linear-gradient(180deg,#ffe27a,#ffbf3d);color:#7a4a00;box-shadow:0 6px 0 #d6951a,0 14px 24px rgba(255,191,61,.45)}
.btn.yel:active{box-shadow:0 1px 0 #d6951a}
.btn.lav{background:linear-gradient(180deg,#c9b2ff,#9a78f5);box-shadow:0 6px 0 #6f4fd0,0 14px 24px rgba(154,120,245,.4)}
.btn.lav:active{box-shadow:0 1px 0 #6f4fd0}
.btn.big{font-size:1.3rem;padding:16px 38px;animation:pulse 1.6s ease-in-out infinite}
.btn.locked{filter:grayscale(.7);opacity:.75;animation:none}
.ttl{font-size:2rem;color:var(--ink);text-shadow:0 2px 0 #fff;position:relative;z-index:2;text-align:center;line-height:1.1}
.sub{font-size:.95rem;color:var(--ink2);max-width:330px;position:relative;z-index:2;text-align:center;margin:4px 0 8px}
.sub.light{color:#fff}
.say{background:#fff;border-radius:18px;padding:8px 14px;font-weight:600;font-size:.92rem;box-shadow:var(--sh);margin:6px 0;z-index:2;text-align:center;max-width:94%}
.pbar{height:8px;background:#f0e2ff;border-radius:99px;overflow:hidden;margin-top:6px;min-width:200px}
.pbar i{display:block;height:100%;width:0;background:linear-gradient(90deg,#ff8fc0,#ffd45e,#7fe0b8,#7cc4ff);border-radius:99px;transition:width .6s}
.cloud{position:absolute;width:90px;height:28px;background:#fff;border-radius:99px;opacity:.92;animation:drift 14s ease-in-out infinite alternate;pointer-events:none}
.cloud::before,.cloud::after{content:"";position:absolute;background:#fff;border-radius:50%}
.cloud::before{width:42px;height:42px;left:14px;top:-20px}.cloud::after{width:32px;height:32px;left:46px;top:-13px}
.fb{position:absolute;bottom:-90px;width:34px;height:44px;border-radius:50% 50% 48% 48%;opacity:.8;animation:rise linear infinite,swayx 3s ease-in-out infinite alternate;pointer-events:none}
.fb::after{content:"";position:absolute;left:50%;top:100%;width:2px;height:34px;background:rgba(255,255,255,.85);margin-left:-1px}
.fb::before{content:"";position:absolute;left:22%;top:14%;width:8px;height:12px;border-radius:50%;background:rgba(255,255,255,.6);rotate:25deg}

/* bunting + HUD */
#bunting{position:fixed;top:0;left:0;right:0;height:26px;z-index:40;pointer-events:none;display:flex;justify-content:space-between;padding:0 2px}
#bunting::before{content:"";position:absolute;left:0;right:0;top:0;height:3px;background:#fff;opacity:.85}
#bunting i{width:0;height:0;border-left:12px solid transparent;border-right:12px solid transparent;border-top:21px solid var(--c);transform-origin:50% 0;animation:sway 3s ease-in-out infinite alternate}
@keyframes sway{from{transform:rotate(-6deg)}to{transform:rotate(6deg)}}
#hud{position:fixed;top:28px;left:0;right:0;padding:4px 10px;display:flex;justify-content:space-between;align-items:center;z-index:50;pointer-events:none}
#hud>*{pointer-events:auto}
.pill{background:rgba(255,255,255,.92);border-radius:999px;box-shadow:var(--sh);padding:6px 14px;font-weight:600;display:flex;gap:12px;align-items:center;font-size:.95rem;border:2px solid #fff}
.pill.bump{animation:pulse .35s}
.rbtn{width:42px;height:42px;border-radius:50%;background:#fff;box-shadow:var(--sh);font-size:1.25rem;display:grid;place-items:center;border:2px solid #fff}
body[data-screen="intro"] #homeBtn,body[data-screen="intro"] #counts,body[data-screen="hub"] #homeBtn{visibility:hidden}

/* sheet, toast, wipe, fx */
#sheet{position:fixed;inset:0;z-index:80;background:rgba(91,58,122,.42);display:none;align-items:flex-end;justify-content:center;padding:16px}
#sheet.on{display:flex}
.sh-card{width:100%;max-width:420px;background:#fff;border-radius:30px;padding:22px 20px;text-align:center;border:4px solid #ffd1ea;box-shadow:0 -10px 40px rgba(0,0,0,.2);animation:pop .45s cubic-bezier(.2,1.3,.4,1) both;margin-bottom:max(6px,env(safe-area-inset-bottom))}
.sh-card .e{font-size:3.6rem;line-height:1.1;animation:bob 2s ease-in-out infinite}
.sh-card h3{font-size:1.4rem;margin:4px 0 6px}
.sh-card p{font-size:1rem;color:var(--ink2);margin-bottom:14px;line-height:1.4}
.sh-card .row{display:flex;gap:10px;justify-content:center;flex-wrap:wrap}
#toast{position:fixed;top:82px;left:50%;transform:translate(-50%,-20px);background:#fff;border-radius:999px;padding:10px 18px;font-weight:600;font-size:.92rem;box-shadow:var(--sh);z-index:95;opacity:0;pointer-events:none;transition:.3s;max-width:92vw;text-align:center;border:2px solid #ffd1ea}
#toast.on{opacity:1;transform:translate(-50%,0)}
#wipe{position:fixed;inset:0;z-index:90;pointer-events:none;background:radial-gradient(circle,#ffe3f1,#d7c3ff);clip-path:circle(0% at 50% 50%);transition:clip-path .42s ease-in-out;display:grid;place-items:center}
#wipe.on{clip-path:circle(150% at 50% 50%);pointer-events:auto}
#wipe span{font-size:4rem;animation:spinS 1.2s linear infinite}
#fx{position:fixed;inset:0;width:100%;height:100%;z-index:60;pointer-events:none}
.ft{position:fixed;z-index:75;font-weight:700;font-size:1.25rem;color:#fff;text-shadow:0 2px 0 #ff5aa5,0 0 8px #ff5aa5;pointer-events:none;white-space:nowrap}
.flyer{position:fixed;left:0;top:0;font-size:32px;z-index:70;pointer-events:none}
.bit{position:fixed;left:0;top:0;z-index:70;pointer-events:none;line-height:1}

/* ferris + carousel */
.fw svg{width:100%;height:auto;display:block;overflow:visible}
.fwh{transform-origin:100px 100px;animation:spinS 40s linear infinite}
.gon{transform-origin:0 0;animation:spinR 40s linear infinite}
.bulb{animation:blink 1s infinite}
.carousel{width:120px;text-align:center}
.carousel .cstar{height:0;font-size:1rem;position:relative;top:-14px}
.carousel .ct{height:46px;margin:0 6px;background:repeating-linear-gradient(90deg,#ff9fcf 0 12px,#fff 12px 24px);clip-path:polygon(50% 0,100% 100%,0 100%)}
.carousel .cs{display:flex;justify-content:space-around;height:46px;padding:0 10px}
.carousel .cs i{font-style:normal;font-size:1.7rem;position:relative;animation:horse 1.6s ease-in-out infinite alternate}
.carousel .cs i::before{content:"";position:absolute;left:50%;top:-6px;bottom:-4px;width:3px;margin-left:-1.5px;background:#ffd45e;z-index:-1;border-radius:2px}
.carousel .cs i:nth-child(2){animation-delay:-.5s}.carousel .cs i:nth-child(3){animation-delay:-1s}
@keyframes horse{from{translate:0 -6px}to{translate:0 8px}}
.carousel .cb{height:14px;border-radius:8px;background:linear-gradient(#ffc0e0,#ff8fc0);box-shadow:0 5px 0 #d46ea5}

/* 1 intro */
#s-intro{background:linear-gradient(180deg,#ffd6ea 0%,#ffe8d0 42%,#e2d6ff 100%);justify-content:flex-start;gap:12px;overflow:hidden;text-align:center}
.rays{position:absolute;left:50%;top:72%;width:170vmax;height:170vmax;margin:-85vmax 0 0 -85vmax;background:repeating-conic-gradient(rgba(255,255,255,.4) 0 7deg,transparent 7deg 14deg);animation:spinS 90s linear infinite;pointer-events:none}
.sign{position:relative;z-index:2;padding:10px 24px 12px;background:#fff;border-radius:26px;border:7px dotted #ffd45e;animation:bulbs 1.2s steps(1) infinite;box-shadow:var(--sh);line-height:1}
@keyframes bulbs{50%{border-color:#ff9fcf}}
.sign .script{display:block;color:var(--pink2);font-size:1.3rem;margin-bottom:2px}
.sign b{display:block;font-size:2.3rem;letter-spacing:2px;background:linear-gradient(90deg,#ff7fbc,#ffb84d,#52c8a0,#63b8ff,#a98bff);-webkit-background-clip:text;background-clip:text;color:transparent;font-weight:700}
.intro-title{font-size:clamp(1.45rem,6.8vw,2.05rem);font-weight:700;line-height:1.15;max-width:350px;position:relative;z-index:2;text-shadow:0 2px 0 #fff,0 0 14px #fff}
.scene{position:absolute;left:0;right:0;bottom:0;height:46%;pointer-events:none}
.scene>*{position:absolute}
#introFw{left:50%;bottom:6%;width:min(72vw,320px);translate:-50% 0}
.hill{left:-20%;right:-20%;bottom:-16%;height:42%;border-radius:50%;background:linear-gradient(#cdf3dc,#9fe0be)}
.carousel.ci{left:2%;bottom:3%;scale:.78;transform-origin:0 100%;pointer-events:auto}
.tbooth{right:5%;bottom:2%;font-size:3.4rem;pointer-events:auto;animation:wob 2.4s ease-in-out infinite;transform-origin:50% 100%}
.spk{position:absolute;animation:twinkle 2.4s ease-in-out infinite;pointer-events:none;font-size:1.1rem}

/* 2 hub */
#s-hub{background:linear-gradient(180deg,#bfe6ff 0%,#e4dcff 38%,#ffe3f1 70%,#d9f6dc 100%);justify-content:flex-start;gap:6px}
.tip{background:#fff;border-radius:22px;padding:8px 16px;font-weight:600;font-size:.9rem;box-shadow:var(--sh);text-align:center;z-index:3;max-width:96%}
.map{position:relative;width:100%;max-width:460px;height:660px;flex:none}
.map .path{position:absolute;inset:0;width:100%;height:100%;pointer-events:none}
.gate{position:absolute;left:50%;top:0;width:min(60%,210px);translate:-50% 0;text-align:center;cursor:pointer}
.gate .gtag{display:inline-block;margin-top:-6px;background:#fff;border-radius:999px;padding:5px 12px;font-size:.8rem;font-weight:600;box-shadow:var(--sh);position:relative}
.gate.open .fw{filter:drop-shadow(0 0 14px #ffd45e)}
.gate.open .gtag{background:linear-gradient(#ffe27a,#ffbf3d);animation:pulse 1s infinite}
.gate.shake{animation:shk .4s}
@keyframes shk{25%{translate:calc(-50% - 8px) 0}75%{translate:calc(-50% + 8px) 0}}
.stall{position:absolute;left:var(--x);top:var(--y);width:36%;text-align:center;animation:bob 3.4s ease-in-out infinite;animation-delay:var(--d,0s);filter:drop-shadow(0 10px 8px rgba(120,80,170,.25))}
.stall:active{transform:scale(.92)}
.stall .aw{display:block;height:32px;border-radius:14px 14px 40% 40%/14px 14px 12px 12px;background:repeating-linear-gradient(90deg,var(--c) 0 16px,#fff 16px 32px)}
.stall .bd{display:block;background:#fff;border-radius:0 0 18px 18px;padding:6px 4px 10px;margin:0 7px;box-shadow:inset 0 -6px 0 var(--c)}
.stall .ico{font-size:1.9rem;display:block;line-height:1.1}
.stall b{display:block;font-size:.84rem;line-height:1.1;margin-top:2px}
.stall .chk{position:absolute;right:0;top:-8px;width:26px;height:26px;border-radius:50%;background:#7fe3b0;color:#fff;font-style:normal;display:none;place-items:center;font-size:.9rem;font-weight:700;border:2px solid #fff;z-index:2}
.stall.done .chk{display:grid}
.mascot{position:absolute;right:5%;top:84%;font-size:2.8rem;animation:wob 2.6s ease-in-out infinite;transform-origin:50% 100%}
.carousel.ch{position:absolute;left:0;top:82%;scale:.66;transform-origin:0 0}

/* 3 wheel */
#s-wheel{background:radial-gradient(ellipse at 50% 0,#fff8c9 0,transparent 55%),linear-gradient(180deg,#c9b5ff 0%,#ffc3e1 100%);justify-content:flex-start}
.beam{position:absolute;top:-10%;width:40%;height:90%;background:linear-gradient(180deg,rgba(255,255,255,.55),transparent);clip-path:polygon(40% 0,60% 0,100% 100%,0 100%);transform-origin:50% 0;animation:beam 5s ease-in-out infinite alternate;pointer-events:none}
.beam.b1{left:4%}.beam.b2{right:4%;animation-delay:-2.5s}
@keyframes beam{from{rotate:-14deg}to{rotate:14deg}}
.wheel-box{position:relative;width:min(80vw,44vh,350px);aspect-ratio:1;margin:20px auto 18px;flex:none;z-index:2}
.wheel{position:absolute;inset:0;border-radius:50%;border:8px solid #fff;box-shadow:0 0 0 7px #ff9fcf,0 16px 30px rgba(120,60,160,.4);will-change:transform;overflow:hidden}
.wline{position:absolute;left:50%;top:0;height:50%;width:3px;margin-left:-1.5px;background:#fff;transform-origin:50% 100%}
.wseg{position:absolute;inset:0}
.wseg span{position:absolute;left:50%;top:7%;translate:-50% 0;text-align:center;line-height:1}
.wseg em{display:block;font-style:normal;font-size:clamp(1.6rem,8vw,2.3rem)}
.wseg small{display:block;font-size:.62rem;font-weight:700;margin-top:3px;color:var(--ink)}
.wb{position:absolute;left:50%;top:50%;width:11px;height:11px;margin:-5.5px;border-radius:50%;background:#fff6b0;box-shadow:0 0 8px #ffd45e;animation:blink 1s infinite;z-index:3}
.ptr{position:absolute;left:50%;top:-7%;translate:-50% 0;z-index:5;width:0;height:0;border-left:16px solid transparent;border-right:16px solid transparent;border-top:34px solid #ff4f9c;filter:drop-shadow(0 3px 2px rgba(0,0,0,.3));transform-origin:50% 0}
.whub{position:absolute;left:50%;top:50%;width:23%;aspect-ratio:1;transform:translate(-50%,-50%);border-radius:50%;background:radial-gradient(circle at 35% 30%,#fff,#ffd45e);font-weight:700;font-size:.85rem;box-shadow:0 0 0 5px #fff,0 6px 12px rgba(0,0,0,.25);z-index:4;color:#b8460f}

/* 4 candy */
#s-candy{background:repeating-linear-gradient(90deg,#ffe0ef 0 26px,#fff4f9 26px 52px);justify-content:flex-start}
.awn{position:absolute;left:0;right:0;top:0;height:92px;background:repeating-linear-gradient(90deg,#ff9fcf 0 28px,#fff 28px 56px);border-radius:0 0 50% 50%/0 0 22px 22px;box-shadow:0 8px 0 rgba(255,127,188,.25);z-index:0}
.keeper{display:flex;align-items:center;gap:8px;z-index:2;margin:2px 0 4px;max-width:420px;width:100%}
.keeper .kp{font-size:3rem;animation:wob 2.6s ease-in-out infinite;transform-origin:50% 100%}
.keeper .say{margin:0;flex:1;text-align:left;border-radius:18px 18px 18px 4px}
.shelf{width:100%;max-width:420px;margin-top:10px;position:relative;z-index:2}
.shelf .items{display:flex;justify-content:space-around;align-items:flex-end;padding:0 4px}
.shelf::after{content:"";display:block;height:14px;border-radius:8px;background:linear-gradient(#e7b08a,#cf8f66);box-shadow:0 6px 0 #b97a52}
.item{position:relative;text-align:center;transition:transform .15s;margin-bottom:-2px}
.item:active{transform:scale(.92)}
.item em{display:block;font-style:normal;font-size:.68rem;font-weight:600;margin-top:2px;color:var(--ink);background:rgba(255,255,255,.85);border-radius:8px;padding:1px 4px}
.jar{width:84px}
.jar .lid{display:block;width:54px;height:15px;margin:0 auto;border-radius:8px 8px 3px 3px;background:linear-gradient(#ffe27a,#ffbf3d);transition:transform .35s;position:relative;z-index:2}
.jar .gl{display:block;position:relative;width:72px;height:80px;margin:-2px auto 0;border-radius:14px 14px 24px 24px;background:rgba(255,255,255,.6);border:3px solid #fff;box-shadow:inset 0 0 12px rgba(255,255,255,.9),0 4px 8px rgba(150,100,200,.2);overflow:hidden}
.jar .gl i{position:absolute;width:15px;height:15px;border-radius:50%;background:radial-gradient(circle at 30% 30%,#fff,var(--c1) 55%,var(--c2))}
.jar .gl i:nth-child(1){left:5px;bottom:3px}.jar .gl i:nth-child(2){left:22px;bottom:5px}.jar .gl i:nth-child(3){left:40px;bottom:3px}.jar .gl i:nth-child(4){left:12px;bottom:19px}.jar .gl i:nth-child(5){left:31px;bottom:20px}.jar .gl i:nth-child(6){left:47px;bottom:18px}.jar .gl i:nth-child(7){left:24px;bottom:36px}
.jar.open .lid{transform:translateY(-24px) rotate(-28deg)}
.jar.open .gl{animation:shk2 .5s}
@keyframes shk2{25%{rotate:-6deg}75%{rotate:6deg}}
.jar.got::before{content:"✓";position:absolute;right:2px;top:12px;width:22px;height:22px;border-radius:50%;background:#7fe3b0;color:#fff;font-weight:700;font-size:.85rem;display:grid;place-items:center;z-index:3;border:2px solid #fff}
.toy{width:84px}
.toy .bow{display:block;font-size:1.5rem;height:0;position:relative;top:-6px;z-index:2;transition:transform .5s}
.toy .bx{display:grid;place-items:center;width:70px;height:70px;margin:12px auto 0;border-radius:16px;background:linear-gradient(135deg,#d7c3ff,#a98bff);position:relative;box-shadow:0 5px 0 #7d5fe0;overflow:hidden}
.toy .bx::before{content:"";position:absolute;left:50%;top:0;bottom:0;width:12px;margin-left:-6px;background:#ffe27a}
.toy .bx::after{content:"";position:absolute;top:50%;left:0;right:0;height:12px;margin-top:-6px;background:#ffe27a}
.toy .q{position:relative;z-index:2;font-size:1.6rem;color:#fff;text-shadow:0 2px 0 #7d5fe0}
.toy .rv{display:none;font-size:2.4rem;position:relative;z-index:2}
.toy.got .bx{background:#fff3c8;box-shadow:0 5px 0 #f0cf7a}.toy.got .bx::before,.toy.got .bx::after{display:none}
.toy.got .q{display:none}.toy.got .rv{display:block;animation:pop .5s both}
.toy.open .bow{transform:translateY(-26px) rotate(30deg)}
.tray{width:100%;max-width:420px;margin-top:22px;background:#fff;border-radius:20px;padding:10px 12px;box-shadow:var(--sh);z-index:2;text-align:center;border:3px dashed #ffb7d9}
.tray .trh{font-weight:600;font-size:.92rem;margin-bottom:6px}
.slots{display:flex;gap:6px;justify-content:center;flex-wrap:wrap}
.sl{width:28px;height:28px;border-radius:9px;background:rgba(240,228,255,.8);display:grid;place-items:center;font-size:1.05rem;color:#b9a4ee}
.sl.f{background:#fff6c4}
.sl.big{width:44px;height:44px;font-size:1.6rem;border-radius:14px}

/* 5 arcade */
#s-arcade{background:linear-gradient(180deg,#2c2266 0%,#5a3fa8 55%,#8b5fd0 100%);justify-content:flex-start;text-align:center}
#s-arcade::before{content:"";position:absolute;left:-50%;right:-50%;bottom:0;height:38%;background:repeating-linear-gradient(90deg,rgba(255,150,220,.4) 0 2px,transparent 2px 46px),repeating-linear-gradient(0deg,rgba(255,150,220,.4) 0 2px,transparent 2px 36px);transform:perspective(300px) rotateX(55deg);transform-origin:50% 100%;pointer-events:none}
.neon{font-size:2rem;font-weight:700;color:#fff;text-shadow:0 0 6px #ff8fd0,0 0 16px #ff5fb8,0 0 30px #ff5fb8;animation:flick 3s infinite;letter-spacing:2px;position:relative;z-index:2}
@keyframes flick{0%,92%,100%{opacity:1}94%{opacity:.5}96%{opacity:1}98%{opacity:.6}}
.cabs{display:flex;align-items:flex-end;justify-content:center;gap:10px;margin-top:22px;width:100%;max-width:440px;position:relative;z-index:2}
.cab{flex:1;max-width:136px;border-radius:20px 20px 8px 8px;padding:8px 6px 10px;color:#fff;box-shadow:0 0 0 4px rgba(255,255,255,.35),0 12px 24px rgba(0,0,0,.35);display:flex;flex-direction:column;align-items:center;gap:6px;transition:transform .2s}
.cab:active{transform:scale(.94)}
.cab.c1{background:linear-gradient(#ff8fc8,#d9519c);height:240px}
.cab.c2{background:linear-gradient(#8fd0ff,#4a8de0);height:285px}
.cab.c3{background:linear-gradient(#ffe27a,#f2a93a);height:215px;color:#6b3d00}
.cab .mq{font-weight:700;font-size:.76rem;line-height:1.05;padding:5px 6px;border-radius:8px;background:rgba(0,0,0,.28);animation:blink 1.4s steps(1) infinite}
.cab .scr{flex:1;width:100%;border-radius:10px;background:#1b1446;display:grid;place-items:center;font-size:2.5rem;box-shadow:inset 0 0 14px #000,0 0 12px rgba(255,255,255,.4)}
.cab .scr span{animation:pulse 1.4s infinite}
.cab .ctl{display:flex;gap:7px;align-items:center;height:24px}
.cab .ctl i{width:14px;height:14px;border-radius:50%;background:#fff;box-shadow:inset 0 -3px 0 rgba(0,0,0,.25)}
.cab .ctl i.js{width:8px;height:22px;border-radius:5px;background:#333}
.cab small{font-size:.72rem;font-weight:600}
.legend{position:relative;z-index:2;margin-top:18px;color:#fff;font-size:.88rem;opacity:.9;text-align:center}

/* games shared */
.gbar{display:flex;gap:8px;align-items:center;justify-content:space-between;width:100%;max-width:520px;margin-bottom:8px;z-index:2}
.gbar .back{background:#fff;border-radius:999px;padding:8px 14px;font-weight:600;box-shadow:var(--sh);font-size:.9rem}
.gstat{background:#fff;border-radius:999px;padding:8px 14px;font-weight:600;box-shadow:var(--sh)}
.field{position:relative;flex:1;min-height:320px;width:100%;max-width:520px;border-radius:26px;overflow:hidden;touch-action:none;border:4px solid #fff;box-shadow:var(--sh)}
.gready{position:absolute;inset:0;z-index:5;background:rgba(255,255,255,.82);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:12px;text-align:center;padding:20px}
.gready h3{font-size:1.7rem}.gready p{color:var(--ink2);max-width:260px}
#s-balloon{background:linear-gradient(180deg,#a8dcff,#e8f6ff 70%,#ffeaf4);justify-content:flex-start}
#bField{background:linear-gradient(180deg,#8fd3ff,#d8f1ff)}
.bl{position:absolute;left:0;top:0;width:58px;height:72px;border-radius:50% 50% 47% 47%;background:radial-gradient(circle at 32% 28%,rgba(255,255,255,.7) 0 8%,transparent 9%),linear-gradient(160deg,var(--a),var(--b));box-shadow:inset -6px -8px 0 rgba(0,0,0,.07);display:grid;place-items:center;font-size:1.6rem;will-change:transform}
.bl::before{content:"";position:absolute;inset:-14px -10px -6px}
.bl::after{content:"";position:absolute;left:50%;top:100%;width:2px;height:34px;background:rgba(255,255,255,.85);margin-left:-1px}
.bl.gold{box-shadow:0 0 18px #ffd45e,inset -6px -8px 0 rgba(0,0,0,.07)}
#s-stars{background:linear-gradient(180deg,#2b2566,#6a4cbf 55%,#e784bd);justify-content:flex-start}
#cField{background:radial-gradient(circle at 20% 20%,rgba(255,255,255,.5) 0 1.5px,transparent 2px) 0 0/60px 70px,radial-gradient(circle at 70% 60%,rgba(255,255,255,.4) 0 1.5px,transparent 2px) 0 0/80px 60px,linear-gradient(180deg,#352b7a,#7a58c9)}
.fo{position:absolute;left:0;top:0;font-size:34px;line-height:1;will-change:transform}
#basket{position:absolute;left:0;top:0;font-size:60px;line-height:1;will-change:transform;z-index:3}
#basket.bump{animation:bmp .2s}
@keyframes bmp{50%{scale:1.2 .85}}
#s-teddy{background:radial-gradient(#fff 3px,transparent 4px) 0 0/34px 34px,linear-gradient(180deg,#fff6c2,#ffdcb0);justify-content:flex-start}
#tField{background:linear-gradient(180deg,#fffbe6,#fff0c8)}
.ti{position:absolute;left:0;top:0;line-height:1;animation:wob 3s ease-in-out infinite;animation-delay:var(--d)}
.ti.hint{animation:pulse .5s infinite;filter:drop-shadow(0 0 10px #ff5aa5) drop-shadow(0 0 18px #ffd45e);z-index:3}

/* photo */
#s-photo{background:radial-gradient(#fff 2.5px,transparent 3px) 0 0/28px 28px,linear-gradient(180deg,#fff0e0,#ffe0ef);justify-content:flex-start}
.curt{position:absolute;top:0;bottom:0;width:15%;background:repeating-linear-gradient(90deg,#ff9fcf 0 14px,#ff86c0 14px 28px);box-shadow:0 0 20px rgba(0,0,0,.2);z-index:0}
.curt.l{left:0;border-radius:0 0 40px 0}.curt.r{right:0;border-radius:0 0 0 40px}
.rope{width:80%;height:0;border-top:3px dashed #b58cff;margin-top:6px;position:relative;z-index:1}
.polwrap{position:relative;width:min(68vw,270px);margin-top:12px;z-index:2}
.clip{position:absolute;top:-16px;width:12px;height:28px;background:#ffd45e;border-radius:4px;z-index:5;box-shadow:0 2px 4px rgba(0,0,0,.2)}
.clip.a{left:18%}.clip.b{right:18%}
.pol{background:#fff;padding:12px 12px 14px;border-radius:10px;transform-origin:50% 0;animation:swing 4s ease-in-out infinite alternate;box-shadow:0 0 0 4px #fff,0 0 26px 8px rgba(255,170,210,.85),0 18px 30px rgba(90,40,120,.3)}
@keyframes swing{from{rotate:-2.5deg}to{rotate:2.5deg}}
.pbox{position:relative;aspect-ratio:1/1.1;border-radius:6px;overflow:hidden;background:linear-gradient(135deg,#ffe1f0,#e1e9ff)}
.pbox img{width:100%;height:100%;object-fit:cover;display:block}
.pmiss{position:absolute;inset:0;display:none;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:14px;font-size:.85rem;color:var(--ink2)}
.pmiss b{font-size:2.4rem}
.pbox.no .pmiss{display:flex}
.pcap{margin-top:10px;font-size:1rem;color:var(--ink);text-align:center;line-height:1.3}
#stickLayer{position:absolute;inset:0;pointer-events:none;z-index:6}
.stk{position:absolute;font-size:34px;line-height:1;pointer-events:auto;touch-action:none;rotate:var(--r);cursor:grab;filter:drop-shadow(0 3px 3px rgba(0,0,0,.2));animation:bob 3s ease-in-out infinite}
.stk.drag{scale:1.25;animation:none}
.stray{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;max-width:340px;margin:14px 0 6px;z-index:2}
.stray button{width:42px;height:42px;border-radius:50%;background:#fff;font-size:1.4rem;box-shadow:var(--sh);transition:transform .15s}
.stray button:active{transform:scale(.85)}
.linkb{font-size:.8rem;color:var(--ink2);text-decoration:underline;margin-bottom:8px;z-index:2}
/* NEW: gallery picker */
.fileinp{position:absolute;left:-9999px;top:0;width:1px;height:1px;opacity:0;overflow:hidden}
.gbtn{margin-top:20px;cursor:pointer;text-align:center;-webkit-user-select:none;user-select:none}
.ghint{font-size:.78rem;color:var(--ink2);margin:10px 0 0;z-index:2;text-align:center;max-width:290px;line-height:1.3}

/* rewards */
#s-rewards{background:radial-gradient(circle at 20% 8%,#fff3b8,transparent 40%),linear-gradient(180deg,#ffe9f4,#e8dcff 60%,#d6ecff);justify-content:flex-start;text-align:center;gap:10px}
.ringw{position:relative;width:128px;height:128px;flex:none}
.ringw svg{width:100%;height:100%}
.rbg{fill:#fff;stroke:#f0e2ff;stroke-width:12}
.rfg{fill:none;stroke:url(#gr);stroke-width:12;stroke-linecap:round;stroke-dasharray:326.7;stroke-dashoffset:326.7;transition:stroke-dashoffset 1s ease}
.rtxt{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;line-height:1}
.rtxt b{font-size:1.7rem}.rtxt small{font-size:.65rem;color:var(--ink2);margin-top:2px}
.ticket{width:100%;max-width:400px;background:#fff7d6;padding:6px 18px;position:relative;border-radius:18px;box-shadow:var(--sh);-webkit-mask:radial-gradient(circle 11px at 0 50%,transparent 97%,#000) left/51% 100% no-repeat,radial-gradient(circle 11px at 100% 50%,transparent 97%,#000) right/51% 100% no-repeat;mask:radial-gradient(circle 11px at 0 50%,transparent 97%,#000) left/51% 100% no-repeat,radial-gradient(circle 11px at 100% 50%,transparent 97%,#000) right/51% 100% no-repeat}
.trow{display:flex;gap:10px;align-items:center;padding:9px 0;border-bottom:2px dashed #f0d9a0;text-align:left}
.trow:last-child{border:0}
.medal{width:46px;height:46px;border-radius:50%;display:grid;place-items:center;font-size:1.5rem;background:#eee;filter:grayscale(1);opacity:.55;flex:none;border:3px solid #ddd}
.trow.ok .medal{filter:none;opacity:1;background:radial-gradient(circle at 30% 30%,#fff,#ffe27a);border-color:#ffbf3d;box-shadow:0 0 12px rgba(255,191,61,.7)}
.trow .tx{flex:1}.trow .tx b{display:block;font-size:.95rem}.trow .tx small{font-size:.75rem;color:var(--ink2);display:block}
.mb{height:6px;background:#f2e6bd;border-radius:9px;margin-top:4px;overflow:hidden}.mb i{display:block;height:100%;background:linear-gradient(90deg,#ff8fc0,#ffbf3d);border-radius:9px}
.tv{font-weight:700;font-size:.85rem;color:var(--ink2);min-width:38px;text-align:right}.trow.ok .tv{color:#2fb37a;font-size:1.2rem}
.shelfbox{width:100%;max-width:400px;background:rgba(255,255,255,.75);border-radius:20px;padding:10px 12px;box-shadow:var(--sh)}
.shelfbox h4{font-size:.92rem;margin-bottom:8px}

/* finale */
#s-finale{background:linear-gradient(180deg,#2a1d63 0%,#5b3aa5 45%,#c95fa8 100%);justify-content:flex-start;text-align:center;overflow-x:hidden}
.tw{position:absolute;animation:twinkle 2s ease-in-out infinite;pointer-events:none;font-size:.9rem}
.fttl{font-size:clamp(1.8rem,8.5vw,2.6rem);color:#fff;text-shadow:0 0 12px #ff9fdc,0 0 28px #ff6fc0;line-height:1.25;max-width:340px;position:relative;z-index:3}
.fttl span{display:inline-block;animation:drop .7s cubic-bezier(.2,1.5,.4,1) both}
@keyframes drop{from{opacity:0;transform:translateY(-40px) scale(.6)}to{opacity:1;transform:none}}
.fsub{color:#ffe9f7;margin:8px 0 4px;font-size:.95rem;position:relative;z-index:3}
.cake{position:relative;width:230px;height:235px;margin:10px auto 6px;cursor:pointer;flex:none;z-index:3}
.cake .plate{position:absolute;bottom:0;left:50%;width:230px;height:22px;margin-left:-115px;border-radius:50%;background:linear-gradient(#fff,#e6dcff);box-shadow:0 8px 0 #b9a4ee}
.tier{position:absolute;left:50%;translate:-50% 0;border-radius:14px 14px 8px 8px;box-shadow:inset 0 -8px 0 rgba(0,0,0,.08)}
.tier::before{content:"";position:absolute;left:0;right:0;top:0;height:20px;border-radius:14px 14px 0 0;background:linear-gradient(#fff,#fff) top/100% 8px no-repeat,radial-gradient(circle at 12px 0,#fff 9px,transparent 10px) 0 6px/24px 14px repeat-x}
.tier::after{content:"";position:absolute;inset:22px 0 6px;background:radial-gradient(circle,#fff 2px,transparent 3px) 6px 2px/22px 18px,radial-gradient(circle,#ffd45e 2px,transparent 3px) 17px 10px/22px 18px;opacity:.9}
.t1{width:210px;height:60px;bottom:14px;background:linear-gradient(#ffe0b8,#ffc58e)}
.t2{width:154px;height:54px;bottom:70px;background:linear-gradient(#e4d4ff,#c9b0ff)}
.t3{width:104px;height:48px;bottom:120px;background:linear-gradient(#ffc2e0,#ff9fcd)}
.candles{position:absolute;left:50%;bottom:166px;width:90px;margin-left:-45px;display:flex;justify-content:space-around}
.cd{width:8px;height:30px;border-radius:3px;background:repeating-linear-gradient(45deg,#fff 0 5px,#ff7fbc 5px 10px);position:relative}
.fl{position:absolute;left:50%;bottom:100%;width:12px;height:18px;margin-left:-6px;border-radius:50% 50% 50% 50%/65% 65% 35% 35%;background:radial-gradient(circle at 50% 70%,#fff6a8,#ffb629 70%);box-shadow:0 0 14px 6px rgba(255,200,80,.6);animation:flame .35s ease-in-out infinite alternate;transform-origin:50% 100%}
@keyframes flame{from{transform:scale(1,1) rotate(-4deg)}to{transform:scale(.88,1.12) rotate(4deg)}}
.cake.out .fl{display:none}
.cake.out .cd::after{content:"💨";position:absolute;bottom:100%;left:50%;margin-left:-8px;font-size:14px;animation:smoke 1.2s forwards}
@keyframes smoke{to{transform:translateY(-34px);opacity:0}}
#fmsg{display:none;background:#fff;border-radius:28px;padding:20px 18px;max-width:380px;box-shadow:0 0 30px rgba(255,150,220,.6);margin:12px 0 6px;z-index:3;border:4px solid #ffd1ea}
#fmsg.on{display:block;animation:pop .7s both}
#fmsg h3{font-size:1.4rem;color:var(--pink2);margin-bottom:8px}
#fmsg p{font-size:1.02rem;line-height:1.45;margin-bottom:10px}
#fmsg .stat{font-weight:600;color:var(--ink2);margin-bottom:14px;font-size:.95rem}
#fmsg .row{display:flex;flex-direction:column;gap:12px;align-items:center}
@media(prefers-reduced-motion:reduce){*{animation-duration:.01ms!important;animation-iteration-count:1!important}}
</style>
</head>
<body data-screen="intro">

<div id="stage">

<!-- 1 ENTRANCE -->
<section class="screen active" id="s-intro">
  <div class="rays"></div>
  <div class="sign"><span class="script">🎡 Her Little 🎠</span><b>FUNFAIR</b></div>
  <h1 class="intro-title">Welcome to Your Little World of Happiness!</h1>
  <button class="btn big" id="startBtn">LET'S PLAY! 🎪</button>
  <div class="scene">
    <div id="introFw" class="fw"></div>
    <div class="hill"></div>
    <div class="carousel ci" id="introCar"></div>
    <div class="tbooth" id="introTeddy">🧸</div>
  </div>
</section>

<!-- HUB -->
<section class="screen" id="s-hub">
  <div class="tip"><span id="tipTxt">Tap a stall to play! 🎪</span><div class="pbar"><i id="hubBar"></i></div></div>
  <div class="map" id="map">
    <div class="cloud" style="top:2%;left:3%"></div>
    <div class="cloud" style="top:14%;right:2%;scale:.7;animation-delay:-6s"></div>
    <svg class="path" viewBox="0 0 100 100" preserveAspectRatio="none"><path d="M50 40 C 20 44, 18 52, 24 58 S 50 54, 76 58 S 80 70, 50 76 S 40 84, 50 92" fill="none" stroke="#fff" stroke-width="4" stroke-dasharray="1 9" stroke-linecap="round" vector-effect="non-scaling-stroke"/></svg>
    <div class="gate" id="gate" role="button" aria-label="Grand finale"><div class="fw" id="gateFw"></div><div class="gtag" id="gtag">🔒 Grand Finale</div></div>
    <button class="stall" id="stWheel" data-go="wheel" style="--x:5%;--y:42%;--c:#ffb7d9"><i class="chk">✓</i><span class="aw"></span><span class="bd"><span class="ico">🎡</span><b>Happiness<br>Wheel</b></span></button>
    <button class="stall" id="stCandy" data-go="candy" style="--x:59%;--y:44%;--c:#bfe6ff;--d:-1s"><i class="chk">✓</i><span class="aw"></span><span class="bd"><span class="ico">🍭</span><b>Candy<br>Shop</b></span></button>
    <button class="stall" id="stArcade" data-go="arcade" style="--x:5%;--y:63%;--c:#d7c3ff;--d:-2s"><i class="chk">✓</i><span class="aw"></span><span class="bd"><span class="ico">🕹️</span><b>Mini<br>Arcade</b></span></button>
    <button class="stall" id="stPhoto" data-go="photo" style="--x:59%;--y:65%;--c:#ffd3b0;--d:-1.5s"><i class="chk">✓</i><span class="aw"></span><span class="bd"><span class="ico">📸</span><b>Photo<br>Corner</b></span></button>
    <button class="stall" id="stRewards" data-go="rewards" style="--x:32%;--y:83%;--c:#fff1a6;--d:-.5s"><i class="chk">✓</i><span class="aw"></span><span class="bd"><span class="ico">🏆</span><b>Carnival<br>Rewards</b></span></button>
    <div class="carousel ch" id="hubCar"></div>
    <div class="mascot" id="hubTeddy">🧸</div>
  </div>
</section>

<!-- WHEEL -->
<section class="screen" id="s-wheel">
  <div class="beam b1"></div><div class="beam b2"></div>
  <h2 class="ttl script">Happiness Wheel</h2>
  <p class="sub">Spin for a surprise: a silly challenge or a sweet secret!</p>
  <div class="wheel-box" id="wbox">
    <div class="ptr" id="ptr"></div>
    <div class="wheel" id="wheel"></div>
    <button class="whub" id="spinHub">SPIN</button>
  </div>
  <button class="btn big" id="spinBtn">SPIN IT! 🎡</button>
  <p class="sub" id="spinInfo">Spins so far: 0</p>
</section>

<!-- CANDY -->
<section class="screen" id="s-candy">
  <div class="awn"></div>
  <h2 class="ttl script" style="margin-top:2px">Candy Shop</h2>
  <div class="keeper"><div class="kp">🧸</div><div class="say" id="keeperSay">Welcome! Tap a jar or a mystery box!</div></div>
  <div class="shelf"><div class="items" id="shelf1"></div></div>
  <div class="shelf"><div class="items" id="shelf2"></div></div>
  <div class="tray"><div class="trh">My Collection <b id="trayCount">0/6</b></div><div class="slots" id="slots"></div></div>
</section>

<!-- ARCADE -->
<section class="screen" id="s-arcade">
  <h2 class="neon">MINI ARCADE</h2>
  <p class="sub light">Pick a machine and play!</p>
  <div class="cabs">
    <button class="cab c1" data-go="balloon"><div class="mq">BALLOON<br>POP</div><div class="scr"><span>🎈</span></div><div class="ctl"><i class="js"></i><i></i><i></i></div><small id="bestB">Best: 0</small></button>
    <button class="cab c2" data-go="stars"><div class="mq">CATCH THE<br>STARS</div><div class="scr"><span>⭐</span></div><div class="ctl"><i class="js"></i><i></i><i></i></div><small id="bestS">Best: 0</small></button>
    <button class="cab c3" data-go="teddy"><div class="mq">FIND THE<br>TEDDY</div><div class="scr"><span>🧸</span></div><div class="ctl"><i class="js"></i><i></i><i></i></div><small id="bestT">Hide & seek</small></button>
  </div>
  <p class="legend">Win badges to unlock the Grand Finale 🎆</p>
</section>

<!-- BALLOON POP -->
<section class="screen" id="s-balloon">
  <div class="gbar"><button class="back" data-go="arcade">← Arcade</button><div class="gstat">🎈 <b id="bScore">0</b></div><div class="gstat">⏱ <b id="bTime">30</b></div></div>
  <div class="field" id="bField">
    <div class="cloud" style="top:10%;left:10%"></div><div class="cloud" style="top:40%;right:6%;scale:.8;animation-delay:-5s"></div>
    <div class="gready" id="bReady"><h3>Balloon Pop! 🎈</h3><p>Tap the balloons to pop them. Golden ones are worth 5!</p><button class="btn" id="bStart">START</button></div>
  </div>
</section>

<!-- CATCH STARS -->
<section class="screen" id="s-stars">
  <div class="gbar"><button class="back" data-go="arcade">← Arcade</button><div class="gstat">⭐ <b id="cScore">0</b></div><div class="gstat">⏱ <b id="cTime">30</b></div></div>
  <div class="field" id="cField">
    <div id="basket">🧺</div>
    <div class="gready" id="cReady"><h3>Catch the Stars! ⭐</h3><p>Slide your finger to move the basket. Stars +1, candy +2, golden +3. Avoid the chilli 🌶️!</p><button class="btn blue" id="cStart">START</button></div>
  </div>
</section>

<!-- FIND TEDDY -->
<section class="screen" id="s-teddy">
  <div class="gbar"><button class="back" data-go="arcade">← Arcade</button><div class="gstat" id="tRound">Round 1/3</div><button class="gstat" id="tHint">💡 Hint</button></div>
  <div class="say" id="tSay">Find the hidden teddy! 🧸</div>
  <div class="field" id="tField">
    <div class="gready" id="tReady"><h3>Find the Teddy! 🧸</h3><p>A teddy is hiding among the cuties. Find him in 3 rounds!</p><button class="btn yel" id="tStart">START</button></div>
  </div>
</section>

<!-- PHOTO -->
<section class="screen" id="s-photo">
  <div class="curt l"></div><div class="curt r"></div>
  <h2 class="ttl script">Photo Corner</h2>
  <div class="rope"></div>
  <div class="polwrap" id="polwrap">
    <div class="clip a"></div><div class="clip b"></div>
    <div class="pol" id="polBox"><div class="pbox" id="pbox"><img id="herImg" alt="Her photo"><div class="pmiss"><b>📸</b><span>Tap the button below<br>to choose her photo<br>from your gallery!</span></div></div><p class="pcap script">The Star of This Carnival ⭐</p></div>
    <div id="stickLayer"></div>
  </div>

  <!-- NEW: gallery picker (label triggers the Android image picker) -->
  <input type="file" id="galleryInput" class="fileinp" accept="image/*">
  <label for="galleryInput" class="btn blue gbtn" id="galleryBtn">Choose Photo From Gallery 📸</label>
  <p class="ghint">🔒 The photo stays on your phone. Nothing is uploaded anywhere.</p>

  <div class="stray" id="stray"></div>
  <button class="linkb" id="clrStk">🧹 reset stickers</button>
  <button class="btn yel" id="flashBtn">📸 Say cheese!</button>
</section>

<!-- REWARDS -->
<section class="screen" id="s-rewards">
  <h2 class="ttl script">Carnival Rewards</h2>
  <div class="ringw"><svg viewBox="0 0 120 120"><defs><linearGradient id="gr"><stop offset="0" stop-color="#ff7fbc"/><stop offset="1" stop-color="#ffbf3d"/></linearGradient></defs><circle class="rbg" cx="60" cy="60" r="52"/><circle class="rfg" id="rfg" cx="60" cy="60" r="52" transform="rotate(-90 60 60)"/></svg><div class="rtxt"><b id="rpct">0%</b><small>Funfair<br>progress</small></div></div>
  <div class="ticket" id="ticket"></div>
  <div class="shelfbox"><h4>⭐ Star Collection <span id="starsN"></span></h4><div class="slots" id="starSlots"></div></div>
  <div class="shelfbox"><h4>🍬 Sweet Treats</h4><div class="slots" id="treatSlots"></div></div>
  <button class="btn big lav" id="finBtn">🎡 Ride the Ferris Wheel!</button>
</section>

<!-- FINALE -->
<section class="screen" id="s-finale">
  <h1 class="fttl script" id="fTitle">Happy Children's Day, My Little Sunshine! 💗</h1>
  <p class="fsub" id="fSub">Make a wish and blow out the candles! 🕯️</p>
  <div class="cake" id="cake" role="button" aria-label="Cake">
    <div class="candles"><span class="cd"><i class="fl"></i></span><span class="cd"><i class="fl"></i></span><span class="cd"><i class="fl"></i></span><span class="cd"><i class="fl"></i></span><span class="cd"><i class="fl"></i></span></div>
    <div class="tier t3"></div><div class="tier t2"></div><div class="tier t1"></div><div class="plate"></div>
  </div>
  <button class="btn big" id="blowBtn">Blow the candles 🌬️</button>
  <div id="fmsg">
    <h3 class="script">For my little sunshine 💗</h3>
    <p>You played every game, popped every balloon and found every teddy, but the best part of this carnival was always you. Stay silly, stay curious and never stop being the happiest kid at heart. I love you to the moon, the stars and every candy in the shop! 🍭</p>
    <div class="stat" id="fStat"></div>
    <div class="row"><button class="btn yel" id="replayBtn">Replay the Funfair 🎠</button><button class="btn blue" id="backBtn" data-go="hub">Back to the map 🎪</button></div>
  </div>
</section>

</div>

<div id="bunting"></div>
<div id="hud">
  <button class="rbtn" id="homeBtn" aria-label="Funfair map">🎪</button>
  <div class="pill" id="counts"><span>⭐ <b id="cStars">0</b></span><span>🍬 <b id="cTreats">0</b></span><span>🏅 <b id="cBadges">0</b></span></div>
  <button class="rbtn" id="muteBtn" aria-label="Sound on/off">🔊</button>
</div>
<div id="sheet"><div class="sh-card"><div class="e" id="shE">🎉</div><h3 id="shT"></h3><p id="shM"></p><div class="row"><button class="btn" id="shB">Yay!</button><button class="btn blue" id="shB2" style="display:none"></button></div></div></div>
<div id="toast"></div>
<div id="wipe"><span>🎡</span></div>
<canvas id="fx"></canvas>

<script>
(()=>{
"use strict";
/* ====== EASY SETTINGS ====== */
const PHOTO_PATH="her-photo.jpg";   // optional fallback file (only used if no gallery photo was chosen)
const UNLOCK_AT=6;                  // badges needed to open the Grand Finale (max 6)
/* =========================== */

const $=(s,r=document)=>r.querySelector(s),$$=(s,r=document)=>[...r.querySelectorAll(s)];
const rnd=(a,b)=>Math.random()*(b-a)+a,pick=a=>a[Math.floor(Math.random()*a.length)],clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
const COL=["#ff8fc0","#ffd45e","#7fe0b8","#7cc4ff","#c3a2ff","#ffb07a","#fff08a"];
const PASTEL=["#ffb7d9","#ffe27a","#c8f5df","#bfe6ff","#d7c3ff","#ffd3b0"];

/* ---------- state ---------- */
const KEY="herLittleFunfair_v1";
const PHOTO_KEY="herLittleFunfair_photo";
const DEF=()=>({stars:0,spins:0,treats:[],bestB:0,bestS:0,teddyFound:0,flash:0,badges:[]});
let st=DEF();
try{const s=JSON.parse(localStorage.getItem(KEY)||"null");if(s)st=Object.assign(DEF(),s);}catch(e){}
const save=()=>{try{localStorage.setItem(KEY,JSON.stringify(st))}catch(e){}};
const TASKS=[
 {id:"wheel",ico:"🎡",name:"Wheel Whiz",desc:"Spin the Happiness Wheel",val:()=>st.spins,goal:1},
 {id:"candy",ico:"🍬",name:"Sweet Tooth",desc:"Open 3 treats in the Candy Shop",val:()=>st.treats.length,goal:3},
 {id:"balloon",ico:"🎈",name:"Pop Star",desc:"Pop 12 balloons in one game",val:()=>st.bestB,goal:12},
 {id:"stars",ico:"⭐",name:"Star Catcher",desc:"Score 10 in Catch the Stars",val:()=>st.bestS,goal:10},
 {id:"teddy",ico:"🧸",name:"Teddy Detective",desc:"Find the hidden teddy 3 times",val:()=>st.teddyFound,goal:3},
 {id:"photo",ico:"📸",name:"Camera Queen",desc:"Take a photo in the Photo Corner",val:()=>st.flash,goal:1}
];
const NEED=Math.min(UNLOCK_AT,TASKS.length);

/* ---------- sound ---------- */
let AC=null,muted=false,mt=0,musicWanted=false,mi=0,lastBoom=0;
try{muted=localStorage.getItem("ff_mute")==="1"}catch(e){}
function ac(){if(!AC){try{AC=new(window.AudioContext||window.webkitAudioContext)()}catch(e){}}if(AC&&AC.state==="suspended")AC.resume();return AC}
function tone(f,d=.12,type="sine",v=.1,slide=0,when=0){
 if(muted)return;const a=ac();if(!a)return;const t=a.currentTime+when,o=a.createOscillator(),g=a.createGain();
 o.type=type;o.frequency.setValueAtTime(f,t);if(slide)o.frequency.exponentialRampToValueAtTime(Math.max(30,f+slide),t+d);
 g.gain.setValueAtTime(.0001,t);g.gain.exponentialRampToValueAtTime(v,t+.01);g.gain.exponentialRampToValueAtTime(.0001,t+d);
 o.connect(g);g.connect(a.destination);o.start(t);o.stop(t+d+.03);
}
function noise(d,v){if(muted)return;const a=ac();if(!a)return;const len=Math.floor(a.sampleRate*d),buf=a.createBuffer(1,len,a.sampleRate),ch=buf.getChannelData(0);
 for(let i=0;i<len;i++)ch[i]=(Math.random()*2-1)*(1-i/len);const s=a.createBufferSource();s.buffer=buf;const g=a.createGain();g.gain.value=v;s.connect(g);g.connect(a.destination);s.start();}
const sfx={
 tap:()=>tone(660,.07,"triangle",.09),
 pop:()=>{tone(rnd(380,520),.09,"square",.05,-250);tone(1100,.05,"sine",.05)},
 good:()=>[523,659,784,1047].forEach((f,i)=>tone(f,.16,"triangle",.09,0,i*.08)),
 win:()=>[523,659,784,1047,1319,1568].forEach((f,i)=>tone(f,.2,"triangle",.09,0,i*.09)),
 bad:()=>tone(220,.22,"sawtooth",.06,-90),
 tick:()=>tone(1400,.025,"square",.03),
 star:()=>{tone(1200,.08,"sine",.07,400);tone(1800,.1,"sine",.05,0,.06)},
 flash:()=>{tone(2000,.05,"square",.04);tone(900,.12,"square",.04,0,.05)},
 blow:()=>noise(.5,.12),
 boom:()=>noise(.45,.12)
};
const MEL=[0,2,4,2,3,1,2,0,0,2,4,5,4,2,1,-1],SC=[523.25,587.33,659.25,783.99,880,1046.5];
function startMusic(){musicWanted=true;if(mt||muted)return;mt=setInterval(()=>{const n=MEL[mi++%MEL.length];if(n>=0){tone(SC[n],.7,"sine",.03);tone(SC[n]*2,.35,"triangle",.01)}if(mi%8===1)tone(261.6,1.2,"sine",.025)},400)}
function stopMusic(){clearInterval(mt);mt=0}
document.addEventListener("visibilitychange",()=>{if(document.hidden)stopMusic();else if(musicWanted&&!muted)startMusic()});
const muteBtn=$("#muteBtn");
function muteUI(){muteBtn.textContent=muted?"🔇":"🔊"}
muteBtn.onclick=()=>{muted=!muted;try{localStorage.setItem("ff_mute",muted?"1":"0")}catch(e){}muteUI();if(muted)stopMusic();else{ac();if(musicWanted)startMusic();sfx.tap()}};
muteUI();

/* ---------- helpers: effects ---------- */
function floatText(t,x,y){const d=document.createElement("div");d.className="ft";d.textContent=t;d.style.left=x+"px";d.style.top=y+"px";document.body.appendChild(d);
 d.animate([{transform:"translate(-50%,0)",opacity:1},{transform:"translate(-50%,-64px)",opacity:0}],{duration:900,easing:"ease-out"}).onfinish=()=>d.remove()}
function emo(x,y,list,n){for(let i=0;i<n;i++){const s=document.createElement("div");s.className="bit";s.textContent=pick(list);s.style.fontSize=rnd(16,28)+"px";document.body.appendChild(s);
 const dx=rnd(-70,70),dy=rnd(-90,-10);s.animate([{transform:`translate(${x}px,${y}px) scale(.5)`,opacity:1},{transform:`translate(${x+dx}px,${y+dy}px) scale(1.2) rotate(${rnd(-90,90)}deg)`,opacity:0}],{duration:rnd(600,950),easing:"ease-out"}).onfinish=()=>s.remove()}}
function bits(x,y,color,n=8){for(let i=0;i<n;i++){const s=document.createElement("div");s.className="bit";const z=rnd(6,12);s.style.cssText=`width:${z}px;height:${z}px;border-radius:50%;background:${color}`;document.body.appendChild(s);
 const a=Math.random()*6.28,d=rnd(24,60);s.animate([{transform:`translate(${x}px,${y}px)`,opacity:1},{transform:`translate(${x+Math.cos(a)*d}px,${y+Math.sin(a)*d}px) scale(.3)`,opacity:0}],{duration:500}).onfinish=()=>s.remove()}}
function fly(emoji,fromEl,toEl,cb){const a=fromEl.getBoundingClientRect(),b=toEl.getBoundingClientRect(),s=document.createElement("div");s.className="flyer";s.textContent=emoji;document.body.appendChild(s);
 const x0=a.left+a.width/2-16,y0=a.top+a.height/2-16,x1=b.left+b.width/2-16,y1=b.top+b.height/2-16;
 s.animate([{transform:`translate(${x0}px,${y0}px) scale(.6)`},{transform:`translate(${(x0+x1)/2}px,${Math.min(y0,y1)-70}px) scale(1.6)`,offset:.5},{transform:`translate(${x1}px,${y1}px) scale(1)`}],{duration:800,easing:"ease-in-out"}).onfinish=()=>{s.remove();cb&&cb()}}
let tq=[],tb=false;
function toast(m){tq.push(m);if(!tb)nextToast()}
function nextToast(){const m=tq.shift();if(!m){tb=false;return}tb=true;const t=$("#toast");t.textContent=m;t.classList.add("on");setTimeout(()=>{t.classList.remove("on");setTimeout(nextToast,300)},2300)}
let shCb=null,shCb2=null;
function openSheet(o){$("#shE").textContent=o.e;$("#shT").textContent=o.t;$("#shM").textContent=o.m;$("#shB").textContent=o.b||"Yay!";const b2=$("#shB2");b2.style.display=o.b2?"inline-flex":"none";if(o.b2)b2.textContent=o.b2;shCb=o.on||null;shCb2=o.on2||null;$("#sheet").classList.add("on")}
const closeSheet=()=>$("#sheet").classList.remove("on");
$("#shB").onclick=()=>{closeSheet();sfx.tap();const f=shCb;shCb=null;f&&f()};
$("#shB2").onclick=()=>{closeSheet();sfx.tap();const f=shCb2;shCb2=null;f&&f()};

/* ---------- canvas: confetti + fireworks ---------- */
const fx=$("#fx"),fc=fx.getContext("2d");let P=[],fxRun=false,fireTimer=0;
const DPR=Math.min(window.devicePixelRatio||1,2);
function fxSize(){fx.width=innerWidth*DPR;fx.height=innerHeight*DPR;fc.setTransform(DPR,0,0,DPR,0,0)}fxSize();addEventListener("resize",fxSize);
function kick(){if(!fxRun){fxRun=true;requestAnimationFrame(fxTick)}}
function confetti(x,y,n=60){for(let i=0;i<n;i++)P.push({t:"c",x,y,vx:rnd(-6,6),vy:rnd(-12,-3),g:.28,life:0,max:rnd(70,130),col:pick(COL),sz:rnd(5,10),rot:rnd(0,6.28),vr:rnd(-.3,.3),sh:Math.random()<.4});if(P.length>700)P.splice(0,P.length-700);kick()}
function rocket(){P.push({t:"r",x:rnd(.15,.85)*innerWidth,y:innerHeight,vx:rnd(-.6,.6),vy:-rnd(10,13),g:.12,col:pick(COL),life:0,max:100,ty:rnd(.12,.42)*innerHeight});kick()}
function burstFW(x,y,col){const n=44;for(let i=0;i<n;i++){const a=i/n*6.28,sp=rnd(2,5.5);P.push({t:"s",x,y,vx:Math.cos(a)*sp,vy:Math.sin(a)*sp,g:.05,life:0,max:rnd(50,80),col:Math.random()<.7?col:pick(COL)})}
 const now=performance.now();if(now-lastBoom>180){lastBoom=now;sfx.boom()}}
function fxTick(){
 fc.clearRect(0,0,innerWidth,innerHeight);const np=[];
 for(const p of P){p.life++;
  if(p.t==="r"){p.x+=p.vx;p.y+=p.vy;p.vy+=p.g*.15;if(p.y<=p.ty||p.life>p.max){burstFW(p.x,p.y,p.col);continue}fc.globalAlpha=1;fc.fillStyle=p.col;fc.fillRect(p.x-1.5,p.y,3,9);np.push(p);continue}
  p.vy+=p.g;p.x+=p.vx;p.y+=p.vy;
  if(p.t==="c"){p.vx*=.99;p.rot+=p.vr;fc.save();fc.translate(p.x,p.y);fc.rotate(p.rot);fc.globalAlpha=Math.min(1,(p.max-p.life)/30);fc.fillStyle=p.col;
   if(p.sh){fc.beginPath();fc.arc(0,0,p.sz/2,0,6.28);fc.fill()}else fc.fillRect(-p.sz/2,-p.sz/3,p.sz,p.sz*.66);fc.restore()}
  else{p.vx*=.985;p.vy*=.985;fc.globalAlpha=Math.max(0,1-p.life/p.max);fc.strokeStyle=p.col;fc.lineWidth=2.4;fc.beginPath();fc.moveTo(p.x-p.vx*2,p.y-p.vy*2);fc.lineTo(p.x,p.y);fc.stroke()}
  if(p.life<p.max&&p.y<innerHeight+40)np.push(p)}
 fc.globalAlpha=1;P=np;
 if(P.length||fireTimer)requestAnimationFrame(fxTick);else{fxRun=false;fc.clearRect(0,0,innerWidth,innerHeight)}}

/* ---------- SVG / HTML builders ---------- */
function ferris(){
 const gc=["#ff9fcf","#ffd45e","#7fe0b8","#7cc4ff","#c3a2ff","#ffb07a"];
 let s='<svg viewBox="0 0 200 232" aria-hidden="true"><path d="M100 100 62 222M100 100 138 222" stroke="#c9a6ff" stroke-width="7" stroke-linecap="round" fill="none"/><path d="M48 224H152" stroke="#b58cff" stroke-width="8" stroke-linecap="round"/><g class="fwh"><circle cx="100" cy="100" r="80" fill="none" stroke="#ff9fcf" stroke-width="5"/><circle cx="100" cy="100" r="52" fill="none" stroke="#ffd45e" stroke-width="3"/>';
 for(let i=0;i<8;i++){const a=i*Math.PI/4;s+=`<line x1="100" y1="100" x2="${(100+80*Math.sin(a)).toFixed(1)}" y2="${(100-80*Math.cos(a)).toFixed(1)}" stroke="#ffc2e2" stroke-width="3"/>`}
 for(let i=0;i<16;i++){const a=i*Math.PI/8;s+=`<circle class="bulb" cx="${(100+80*Math.sin(a)).toFixed(1)}" cy="${(100-80*Math.cos(a)).toFixed(1)}" r="3" fill="#fff6a8" style="animation-delay:${(i%4)*.25}s"/>`}
 for(let i=0;i<8;i++){const a=i*Math.PI/4,x=(100+80*Math.sin(a)).toFixed(1),y=(100-80*Math.cos(a)).toFixed(1);
  s+=`<g transform="translate(${x} ${y})"><g class="gon"><line x1="0" y1="0" x2="0" y2="8" stroke="#8a6aa8" stroke-width="2"/><rect x="-9" y="8" width="18" height="16" rx="5" fill="${gc[i%6]}"/><path d="M-11 9H11" stroke="#fff" stroke-width="3" stroke-linecap="round"/></g></g>`}
 return s+'<circle cx="100" cy="100" r="10" fill="#ffd45e" stroke="#fff" stroke-width="3"/></g></svg>'}
const carHTML='<div class="cstar">⭐</div><div class="ct"></div><div class="cs"><i>🦄</i><i>🐴</i><i>🐻</i></div><div class="cb"></div>';
$("#introFw").innerHTML=ferris();$("#gateFw").innerHTML=ferris();$("#introCar").innerHTML=carHTML;$("#hubCar").innerHTML=carHTML;
(function(){const b=$("#bunting"),n=Math.max(10,Math.floor(innerWidth/28));for(let i=0;i<n;i++){const t=document.createElement("i");t.style.setProperty("--c",PASTEL[i%6]);t.style.animationDelay=(-i*.3)+"s";b.appendChild(t)}})();
function floaters(sec,n,cols){const c=cols||PASTEL;for(let i=0;i<n;i++){const b=document.createElement("div");b.className="fb";b.style.left=rnd(2,94)+"%";b.style.background=pick(c);const s=rnd(.8,1.3);b.style.scale=s;b.style.animationDuration=rnd(16,28)+"s,"+rnd(2.5,4)+"s";b.style.animationDelay=(-rnd(0,24))+"s,0s";sec.appendChild(b)}}
const small=innerWidth<600;
floaters($("#s-intro"),small?6:10);floaters($("#map"),small?4:6);
(function(){const s=$("#s-intro");for(let i=0;i<12;i++){const d=document.createElement("div");d.className="spk";d.textContent=pick(["⭐","✨","💫","🍬","🌸"]);d.style.left=rnd(2,94)+"%";d.style.top=rnd(2,52)+"%";d.style.animationDelay=(-rnd(0,2))+"s";s.appendChild(d)}})();

/* ---------- HUD, progress ---------- */
function hudUpdate(){$("#cStars").textContent=st.stars;$("#cTreats").textContent=st.treats.length;$("#cBadges").textContent=st.badges.length}
function addStars(n){st.stars+=n;hudUpdate();save();const p=$("#cStars").getBoundingClientRect();floatText("+"+n+" ⭐",p.left+10,p.bottom+6);const c=$("#counts");c.classList.remove("bump");void c.offsetWidth;c.classList.add("bump")}
function refreshHub(){
 const has=id=>st.badges.includes(id),n=st.badges.length;
 $("#stWheel").classList.toggle("done",has("wheel"));$("#stCandy").classList.toggle("done",has("candy"));
 $("#stArcade").classList.toggle("done",has("balloon")&&has("stars")&&has("teddy"));$("#stPhoto").classList.toggle("done",has("photo"));
 $("#stRewards").classList.toggle("done",n>=NEED);
 const all=n>=NEED;$("#hubBar").style.width=(Math.min(n,NEED)/NEED*100)+"%";
 $("#tipTxt").textContent=all?"All badges earned! Tap the glowing Ferris Wheel 🎡":`Badges ${n}/${NEED}. Tap a stall to play! 🎪`;
 $("#gate").classList.toggle("open",all);$("#gtag").textContent=all?"🎆 Tap to ride!":"🔒 Grand Finale";hudUpdate()}
function checkBadges(){
 const nw=[];TASKS.forEach(t=>{if(!st.badges.includes(t.id)&&t.val()>=t.goal){st.badges.push(t.id);nw.push(t)}});
 if(!nw.length)return;nw.forEach(t=>toast(`🏅 Badge unlocked: ${t.name}!`));sfx.good();confetti(innerWidth/2,innerHeight*.35,70);
 if(st.badges.length>=NEED)setTimeout(()=>toast("🎡 The Ferris Wheel finale is unlocked!"),700);save();refreshHub()}
function tryFinale(){if(st.badges.length>=NEED)go("finale");else{sfx.bad();toast(`🔒 Earn ${NEED} badges to unlock the Ferris Wheel finale!`);const g=$("#gate");g.classList.remove("shake");void g.offsetWidth;g.classList.add("shake")}}
$("#gate").onclick=tryFinale;$("#finBtn").onclick=tryFinale;

/* ---------- navigation ---------- */
const screens={};$$(".screen").forEach(s=>screens[s.id.slice(2)]=s);
let cur="intro",busy=false;const G={};
function stopGames(){if(G.raf)cancelAnimationFrame(G.raf);G.raf=0;G.run=false;clearInterval(G.tm);clearInterval(fireTimer);fireTimer=0}
function go(name){
 if(busy||name===cur)return;busy=true;closeSheet();$("#wipe").classList.add("on");
 setTimeout(()=>{stopGames();screens[cur].classList.remove("active");cur=name;const s=screens[name];s.classList.add("active");document.body.dataset.screen=name;s.scrollTop=0;
  onEnter[name]&&onEnter[name]();$("#wipe").classList.remove("on");setTimeout(()=>busy=false,420)},440)}
document.addEventListener("click",e=>{const b=e.target.closest("[data-go]");if(b){sfx.tap();go(b.dataset.go)}});
$("#homeBtn").onclick=()=>{sfx.tap();go("hub")};

/* ---------- intro ---------- */
$("#startBtn").onclick=()=>{ac();startMusic();sfx.good();confetti(innerWidth/2,innerHeight*.5,80);go("hub")};
function teddyTap(e,el){sfx.tap();const r=el.getBoundingClientRect();emo(r.left+r.width/2,r.top,["💗","💖","🍬","⭐"],6);el.animate([{scale:1},{scale:1.3,offset:.4},{scale:1}],{duration:400})}
$("#introTeddy").onclick=e=>teddyTap(e,e.currentTarget);$("#hubTeddy").onclick=e=>teddyTap(e,e.currentTarget);
$$(".carousel").forEach(c=>c.addEventListener("click",()=>{sfx.good();const r=c.getBoundingClientRect();emo(r.left+r.width/2,r.top,["🎠","⭐","🎶","💖"],6);toast("Wheeee! 🎠")}));

/* ---------- wheel ---------- */
const WH=[
 {e:"🤪",lab:"Funny face",t:"Funny Face Challenge!",m:"Make your silliest face and hold it for 5 whole seconds. Judge says: 10/10, adorable as always. 🤪"},
 {e:"💌",lab:"Sweet note",t:"A Sweet Secret 💌",m:"You turn the most ordinary days into festivals. I hope you always know how special you are."},
 {e:"🍫",lab:"Choco time",t:"Chocolate Mission 🍫",m:"Challenge: eat one chocolate right now and say “mmmm” very dramatically. Bonus points for a tiny happy wiggle."},
 {e:"💃",lab:"Happy dance",t:"Happy Dance Time!",m:"Do a 10-second happy dance. No audience needed (but I would absolutely buy a ticket). 💃"},
 {e:"🧸",lab:"Teddy hug",t:"Teddy Hug Delivery 🧸",m:"Hug a teddy (or the nearest human) for 10 seconds. The teddy says thank you and wants a snack."},
 {e:"🌟",lab:"Make a wish",t:"Make a Wish 🌟",m:"Of all the stars in the sky, you are the one I would wish on. Close your eyes and make a wish. ✨"}
];
const WC=["#ffb7d9","#ffe9a0","#c8f5df","#bfe6ff","#d7c3ff","#ffd3b0"];
const wheel=$("#wheel");
wheel.style.background=`conic-gradient(${WC.map((c,i)=>`${c} ${i*60}deg ${i*60+60}deg`).join(",")})`;
wheel.innerHTML=WH.map((w,i)=>`<div class="wline" style="transform:rotate(${i*60}deg)"></div>`).join("")+WH.map((w,i)=>`<div class="wseg" style="transform:rotate(${i*60+30}deg)"><span><em>${w.e}</em><small>${w.lab}</small></span></div>`).join("");
function buildBulbs(){$$(".wb",$("#wbox")).forEach(b=>b.remove());const R=$("#wbox").clientWidth/2+1;
 for(let i=0;i<20;i++){const b=document.createElement("i");b.className="wb";b.style.transform=`rotate(${i*18}deg) translateY(${-R}px)`;b.style.animationDelay=(i%2?.5:0)+"s";$("#wbox").appendChild(b)}
 $("#spinInfo").textContent="Spins so far: "+st.spins}
let rot=0,spinning=false,lastSeg=-1;
function spin(){
 if(spinning)return;spinning=true;sfx.tap();
 const idx=Math.floor(Math.random()*6),target=360-(idx*60+30)+rnd(-22,22),base=Math.ceil(rot/360)*360;
 rot=base+360*(5+Math.floor(Math.random()*3))+target;
 wheel.style.transition="transform 5s cubic-bezier(.12,.62,.08,1)";wheel.style.transform=`rotate(${rot}deg)`;
 lastSeg=-1;(function loop(){if(!spinning)return;try{const m=new DOMMatrix(getComputedStyle(wheel).transform);let a=Math.atan2(m.b,m.a)*180/Math.PI;a=(a+360)%360;const sg=Math.floor(a/60);
  if(sg!==lastSeg){if(lastSeg!==-1){sfx.tick();$("#ptr").animate([{transform:"rotate(-20deg)"},{transform:"rotate(0)"}],{duration:140})}lastSeg=sg}}catch(e){}requestAnimationFrame(loop)})();
 setTimeout(()=>{spinning=false;wheelResult(idx)},5150)}
function wheelResult(i){const w=WH[i];st.spins++;$("#spinInfo").textContent="Spins so far: "+st.spins;addStars(1);sfx.good();confetti(innerWidth/2,innerHeight*.4,50);openSheet({e:w.e,t:w.t,m:w.m,b:"Spin again 🎡"});checkBadges()}
$("#spinBtn").onclick=spin;$("#spinHub").onclick=spin;

/* ---------- candy shop ---------- */
const TR=[
 {e:"🍓",n:"Strawberry Drops",k:"jar",c1:"#ff7fa8",c2:"#ffc0d3",m:"Strawberry Drops! Sweet like you, but they never steal my fries. 🍓"},
 {e:"🌈",n:"Rainbow Gummies",k:"jar",c1:"#7cc4ff",c2:"#ffd45e",m:"Rainbow Gummies! Side effect: uncontrollable giggling. Just like you. 🌈"},
 {e:"☁️",n:"Cotton Cloud",k:"jar",c1:"#e6d8ff",c2:"#ffd1ea",m:"Cotton Cloud! Soft, fluffy and sweet. Basically a hug in candy form. ☁️"},
 {e:"🦄",n:"Mystery toy",k:"toy",m:"A unicorn! It says you are the real magical one around here. ✨"},
 {e:"🚂",n:"Mystery toy",k:"toy",m:"Choo choo! This little train goes straight to Cuddle Town. All aboard! 🚂"},
 {e:"🦆",n:"Mystery toy",k:"toy",m:"Squeak! The duck says you are the cutest. Also, he wants a snack. 🦆"}
];
function renderCandy(){
 const mk=(t,i)=>t.k==="jar"
  ?`<button class="item jar ${st.treats.includes(i)?"got":""}" data-i="${i}" style="--c1:${t.c1};--c2:${t.c2}"><span class="lid"></span><span class="gl">${"<i></i>".repeat(7)}</span><em>${t.n}</em></button>`
  :`<button class="item toy ${st.treats.includes(i)?"got":""}" data-i="${i}"><span class="bow">🎀</span><span class="bx"><b class="q">?</b><b class="rv">${t.e}</b></span><em>${t.n}</em></button>`;
 $("#shelf1").innerHTML=TR.slice(0,3).map((t,i)=>mk(t,i)).join("");$("#shelf2").innerHTML=TR.slice(3).map((t,i)=>mk(t,i+3)).join("");
 $("#slots").innerHTML=TR.map((t,i)=>`<span class="sl big ${st.treats.includes(i)?"f":""}" id="slot${i}">${st.treats.includes(i)?t.e:"?"}</span>`).join("");
 $("#trayCount").textContent=st.treats.length+"/6";$("#keeperSay").textContent="Welcome! Tap a jar or a mystery box!";
 $$(".item").forEach(b=>b.onclick=()=>openTreat(+b.dataset.i,b))}
function openTreat(i,el){
 const t=TR[i];sfx.pop();$("#keeperSay").textContent=t.m;el.classList.add("open");
 const r=el.getBoundingClientRect();emo(r.left+r.width/2,r.top+20,["🍬","⭐","💖","🍭"],6);
 setTimeout(()=>el.classList.remove("open"),600);
 if(!st.treats.includes(i)){st.treats.push(i);el.classList.add("got");sfx.star();
  fly(t.e,el,$("#slot"+i),()=>{const s=$("#slot"+i);s.textContent=t.e;s.classList.add("f");$("#trayCount").textContent=st.treats.length+"/6";addStars(1);checkBadges()});save();hudUpdate()}}

/* ---------- arcade games ---------- */
const BL=[["#ff9fcf","#ff6fb0"],["#ffd45e","#ffb02e"],["#7fe0b8","#3fc794"],["#7cc4ff","#4a9df0"],["#c3a2ff","#9a78f5"],["#ffb07a","#ff8a4a"]];
function gameReward(score,div){return Math.min(5,1+Math.floor(score/div))}

/* Balloon Pop */
function startBalloon(){
 stopGames();const f=$("#bField");$$(".bl",f).forEach(e=>e.remove());$("#bReady").style.display="none";
 const W=f.clientWidth,H=f.clientHeight;let score=0,time=30,list=[],spawn=0,last=performance.now();G.run=true;
 $("#bScore").textContent=0;$("#bTime").textContent=time;
 G.tm=setInterval(()=>{time--;$("#bTime").textContent=time;if(time<=0)end()},1000);
 function mk(){const gold=Math.random()<.1,el=document.createElement("div");el.className="bl"+(gold?" gold":"");const c=gold?["#ffe27a","#ffb800"]:pick(BL);
  el.style.setProperty("--a",c[0]);el.style.setProperty("--b",c[1]);if(gold)el.textContent="⭐";
  const o={el,x:rnd(8,W-66),y:H+90,sp:rnd(70,120)+(30-time)*2.5,ph:rnd(0,6.28),amp:rnd(6,18),gold,dead:false};
  el.addEventListener("pointerdown",ev=>{ev.preventDefault();popB(o)});f.appendChild(el);list.push(o)}
 function popB(o){if(o.dead||!G.run)return;o.dead=true;const r=o.el.getBoundingClientRect(),cx=r.left+r.width/2,cy=r.top+r.height/2;o.el.remove();
  const v=o.gold?5:1;score+=v;$("#bScore").textContent=score;sfx.pop();bits(cx,cy,pick(COL),8);floatText("+"+v,cx,cy);if(navigator.vibrate)try{navigator.vibrate(10)}catch(e){}
  if(score>0&&score%10===0)toast(pick(["Pop-tastic! 🎈","Balloon boss! 👑","Pop pop hooray! 🎉"]))}
 function end(){stopGames();$$(".bl",f).forEach(e=>e.remove());const old=st.bestB;st.bestB=Math.max(st.bestB,score);const rw=gameReward(score,6);addStars(rw);
  const msg=score>=20?"Legendary! The balloons are officially terrified of you. 😂":score>=12?"Great popping! The balloons never stood a chance. 🎈":"Cute popping! Balloons everywhere are giggling. Try again?";
  openSheet({e:score>=12?"🏆":"🎈",t:`You popped ${score} points!`,m:msg+`  (+${rw} ⭐)`,b:"Play again",b2:"Arcade",on:startBalloon,on2:()=>go("arcade")});
  if(score>old)sfx.win();checkBadges()}
 function loop(now){if(!G.run)return;const dt=Math.min(.05,(now-last)/1000);last=now;spawn-=dt;
  if(spawn<=0){mk();spawn=Math.max(.32,.75-(30-time)*.015)}
  for(const o of list){o.y-=o.sp*dt;o.ph+=dt*2;o.el.style.transform=`translate3d(${o.x+Math.sin(o.ph)*o.amp}px,${o.y}px,0)`}
  list=list.filter(o=>{if(o.dead)return false;if(o.y<-110){o.el.remove();return false}return true});G.raf=requestAnimationFrame(loop)}
 G.raf=requestAnimationFrame(loop)}
$("#bStart").onclick=()=>{sfx.tap();startBalloon()};

/* Catch the Stars */
const PLACE=()=>{const f=$("#cField");const W=f.clientWidth,H=f.clientHeight;$("#basket").style.transform=`translate(${W/2-30}px,${H-76}px)`};
function startStars(){
 stopGames();const f=$("#cField");$$(".fo",f).forEach(e=>e.remove());$("#cReady").style.display="none";
 const W=f.clientWidth,H=f.clientHeight,bk=$("#basket");let score=0,time=30,list=[],spawn=0,last=performance.now(),bx=W/2,cx=bx;G.run=true;
 $("#cScore").textContent=0;$("#cTime").textContent=time;
 const mv=e=>{const r=f.getBoundingClientRect();bx=clamp(e.clientX-r.left,34,W-34)};f.onpointermove=mv;f.onpointerdown=mv;
 G.tm=setInterval(()=>{time--;$("#cTime").textContent=time;if(time<=0)end()},1000);
 const TY=[["⭐",1,56],["🌟",3,12],["🍬",2,18],["🌶️",-2,14]];
 const tp=()=>{let r=Math.random()*100,a=0;for(const t of TY){a+=t[2];if(r<a)return t}return TY[0]};
 function mk(){const t=tp(),el=document.createElement("div");el.className="fo";el.textContent=t[0];f.appendChild(el);list.push({el,x:rnd(6,W-42),y:-40,v:t[1],sp:rnd(110,170)+(30-time)*3,dead:false})}
 function end(){stopGames();$$(".fo",f).forEach(e=>e.remove());const old=st.bestS;st.bestS=Math.max(st.bestS,score);const rw=gameReward(score,5);addStars(rw);
  const msg=score>=18?"Star master! The sky is jealous of your basket. 🌌":score>=10?"Nice catching! Your basket is officially magical. ⭐":"Cute try! The stars are playing hard to get. Go again?";
  openSheet({e:score>=10?"🌟":"⭐",t:`You scored ${score}!`,m:msg+`  (+${rw} ⭐)`,b:"Play again",b2:"Arcade",on:startStars,on2:()=>go("arcade")});
  if(score>old)sfx.win();checkBadges()}
 function loop(now){if(!G.run)return;const dt=Math.min(.05,(now-last)/1000);last=now;cx+=(bx-cx)*Math.min(1,dt*18);bk.style.transform=`translate(${cx-30}px,${H-76}px)`;
  spawn-=dt;if(spawn<=0){mk();spawn=Math.max(.3,.6-(30-time)*.01)}
  for(const o of list){o.y+=o.sp*dt;o.el.style.transform=`translate(${o.x}px,${o.y}px)`;
   if(o.y>H-100&&o.y<H-42&&Math.abs(o.x+17-cx)<46){o.dead=true;o.el.remove();score=Math.max(0,score+o.v);$("#cScore").textContent=score;
    const r=bk.getBoundingClientRect();floatText((o.v>0?"+":"")+o.v,r.left+30,r.top);bk.classList.remove("bump");void bk.offsetWidth;bk.classList.add("bump");if(o.v>0)sfx.star();else{sfx.bad();toast("Spicy! 🌶️ -2")}}
   else if(o.y>H+10){o.dead=true;o.el.remove()}}
  list=list.filter(o=>!o.dead);G.raf=requestAnimationFrame(loop)}
 G.raf=requestAnimationFrame(loop)}
$("#cStart").onclick=()=>{sfx.tap();startStars()};
addEventListener("keydown",e=>{if(cur!=="stars"||!G.run)return;const f=$("#cField");if(e.key==="ArrowLeft"||e.key==="ArrowRight"){const b=$("#basket").getBoundingClientRect();const x=b.left+30+(e.key==="ArrowLeft"?-40:40);f.onpointermove({clientX:x})}});

/* Find the Teddy */
const TE=["🐰","🐱","🐶","🦊","🐼","🐨","🐸","🐷","🐥","🦄","🍭","🍬","🎈","🌸","🍩","🎀","🦋","🍓","🎁","🐻"];
const TN={"🐰":"bunny","🐱":"kitty","🐶":"puppy","🦊":"fox","🐼":"panda","🐨":"koala","🐸":"froggy","🐷":"piggy","🐥":"chick","🦄":"unicorn","🍭":"lollipop","🍬":"candy","🎈":"balloon","🌸":"flower","🍩":"donut","🎀":"ribbon","🦋":"butterfly","🍓":"strawberry","🎁":"present","🐻":"big bear"};
const ROUNDS=[12,20,30];let tRound=0,tMiss=0,tEl=null,tLock=false;
function startTeddy(){stopGames();tRound=0;tMiss=0;$("#tReady").style.display="none";buildRound()}
function buildRound(){
 const f=$("#tField");$$(".ti",f).forEach(e=>e.remove());tLock=false;const n=ROUNDS[tRound],W=f.clientWidth,H=f.clientHeight;
 const cols=Math.max(3,Math.round(Math.sqrt(n*W/H))),rows=Math.ceil(n/cols),cw=W/cols,ch=H/rows,fs=Math.min(46,Math.min(cw,ch)*.7);
 const cells=[...Array(cols*rows).keys()].sort(()=>Math.random()-.5).slice(0,n),ti=Math.floor(Math.random()*n);
 $("#tRound").textContent=`Round ${tRound+1}/3`;$("#tSay").textContent=["Find the hidden teddy! 🧸","Harder now! He's sneaky 🕵️","Last round! Find him! 🔍"][tRound];
 cells.forEach((c,i)=>{const r=Math.floor(c/cols),cc=c%cols,e=i===ti?"🧸":pick(TE),el=document.createElement("div");el.className="ti";el.textContent=e;
  el.style.fontSize=fs+"px";el.style.setProperty("--d",(-rnd(0,3))+"s");
  el.style.left=clamp(cc*cw+cw/2-fs/2+rnd(-.16,.16)*cw,2,W-fs-2)+"px";el.style.top=clamp(r*ch+ch/2-fs/2+rnd(-.16,.16)*ch,2,H-fs-2)+"px";
  if(i===ti){tEl=el;el.dataset.t="1"}
  el.addEventListener("pointerdown",ev=>{ev.preventDefault();if(tLock)return;
   if(el.dataset.t)found(el);else{tMiss++;sfx.bad();$("#tSay").textContent=`Nope! That's just a ${TN[e]||"cutie"} in disguise 🕵️`;el.animate([{transform:"translateX(0)"},{transform:"translateX(-8px)"},{transform:"translateX(8px)"},{transform:"translateX(0)"}],{duration:300})}});
  f.appendChild(el)})}
function found(el){
 tLock=true;const r=el.getBoundingClientRect();sfx.good();emo(r.left+r.width/2,r.top,["🧸","💖","⭐","🎉"],10);confetti(r.left+r.width/2,r.top,40);
 st.teddyFound++;addStars(1);tRound++;$("#tSay").textContent="Found him! He was hiding so well 🧸💗";
 if(tRound<ROUNDS.length)setTimeout(buildRound,1000);
 else setTimeout(()=>{addStars(2);openSheet({e:"🕵️",t:"Teddy Detective!",m:`You found all 3 hiding teddies with ${tMiss} oops${tMiss===1?"":"es"}. Detective level: adorable. (+2 ⭐)`,b:"Play again",b2:"Arcade",on:startTeddy,on2:()=>go("arcade")});checkBadges()},900);
 checkBadges()}
$("#tHint").onclick=()=>{if(!tEl||tLock)return;if($("#tReady").style.display!=="none")return;sfx.star();tEl.classList.add("hint");setTimeout(()=>tEl&&tEl.classList.remove("hint"),1600)};
$("#tStart").onclick=()=>{sfx.tap();startTeddy()};

/* ---------- photo corner ---------- */
const img=$("#herImg"),pbox=$("#pbox");
img.onerror=()=>{pbox.classList.add("no");img.style.display="none"};
img.onload=()=>{pbox.classList.remove("no");img.style.display="block"};

/* load: saved gallery photo first, otherwise optional her-photo.jpg, otherwise placeholder */
(function(){
 let saved=null;try{saved=localStorage.getItem(PHOTO_KEY)}catch(e){}
 if(saved){img.src=saved}
 else if(PHOTO_PATH){img.src=PHOTO_PATH}
 else{pbox.classList.add("no");img.style.display="none"}
})();

/* NEW: choose a photo from the phone gallery (FileReader, all local) */
const gInp=$("#galleryInput");
function shrinkImage(dataUrl,cb){
 const im=new Image();
 im.onload=()=>{
  try{
   const MAX=1100;let w=im.naturalWidth||im.width,h=im.naturalHeight||im.height;
   const sc=Math.min(1,MAX/Math.max(w,h));w=Math.max(1,Math.round(w*sc));h=Math.max(1,Math.round(h*sc));
   const c=document.createElement("canvas");c.width=w;c.height=h;
   const x=c.getContext("2d");x.fillStyle="#fff";x.fillRect(0,0,w,h);x.drawImage(im,0,0,w,h);
   cb(c.toDataURL("image/jpeg",.86));
  }catch(e){cb(dataUrl)}
 };
 im.onerror=()=>{toast("That photo type isn't supported. Try a JPG or PNG 📷")};
 im.src=dataUrl;
}
function applyGalleryPhoto(url){
 img.style.display="block";img.src=url;pbox.classList.remove("no");
 try{localStorage.setItem(PHOTO_KEY,url)}catch(e){}  /* remember it if there is room; otherwise just show it */
 sfx.good();
 const pb=$("#polBox");pb.animate([{transform:"scale(.85) rotate(-6deg)",opacity:.4},{transform:"scale(1.07) rotate(3deg)",offset:.6,opacity:1},{transform:"scale(1) rotate(0)"}],{duration:650,easing:"ease-out"});
 const r=$("#polwrap").getBoundingClientRect();
 emo(r.left+r.width/2,r.top+r.height/2,["💖","⭐","🌸","✨","📸"],14);
 confetti(r.left+r.width/2,r.top+r.height*.4,50);
 toast(pick(["Beautiful photo! 💗","Lovely! Look at that smile 😍","Perfect pick! 🌟"]));
}
gInp.addEventListener("change",()=>{
 const f=gInp.files&&gInp.files[0];
 if(!f)return;
 if(f.type&&f.type.indexOf("image/")!==0){toast("Please choose an image 📷");gInp.value="";return}
 const rd=new FileReader();
 rd.onload=()=>{shrinkImage(rd.result,applyGalleryPhoto)};
 rd.onerror=()=>{toast("Couldn't open that photo 😢 Try another one")};
 rd.onloadend=()=>{try{gInp.value=""}catch(e){}};  /* lets you pick the same photo again later */
 rd.readAsDataURL(f);
});
$("#galleryBtn").addEventListener("click",()=>{ac();sfx.tap()});

const STK=["🌸","⭐","💖","🦋","🍭","🌈","🎈","🧸","🌼","✨"];
$("#stray").innerHTML=STK.map(s=>`<button data-s="${s}">${s}</button>`).join("");
function mkSticker(e,x,y){
 const lay=$("#stickLayer"),w=$("#polwrap"),d=document.createElement("div");d.className="stk";d.textContent=e;d.style.left=x+"px";d.style.top=y+"px";d.style.setProperty("--r",rnd(-20,20)+"deg");d.style.animationDelay=(-rnd(0,3))+"s";
 let sx,sy,ox,oy,drag=false;
 d.addEventListener("pointerdown",ev=>{drag=true;d.setPointerCapture(ev.pointerId);sx=ev.clientX;sy=ev.clientY;ox=parseFloat(d.style.left);oy=parseFloat(d.style.top);d.classList.add("drag");ev.preventDefault()});
 d.addEventListener("pointermove",ev=>{if(!drag)return;d.style.left=clamp(ox+ev.clientX-sx,-30,w.clientWidth-20)+"px";d.style.top=clamp(oy+ev.clientY-sy,-30,w.clientHeight-20)+"px"});
 const up=()=>{drag=false;d.classList.remove("drag")};d.addEventListener("pointerup",up);d.addEventListener("pointercancel",up);
 lay.appendChild(d);if(lay.children.length>18)lay.firstChild.remove()}
function initStickers(){const w=$("#polwrap"),W=w.clientWidth,H=w.clientHeight;$("#stickLayer").innerHTML="";
 [["🌸",-16,-10],["⭐",W-30,-14],["💖",-20,H*.42],["🦋",W-26,H*.38],["🌼",-12,H-40],["🍭",W-32,H-34]].forEach(a=>mkSticker(a[0],a[1],a[2]))}
$("#stray").addEventListener("click",e=>{const b=e.target.closest("button");if(!b)return;sfx.pop();const w=$("#polwrap");mkSticker(b.dataset.s,rnd(10,w.clientWidth-40),rnd(10,w.clientHeight-60))});
$("#clrStk").onclick=()=>{sfx.tap();initStickers()};
$("#flashBtn").onclick=()=>{
 sfx.flash();const fl=document.createElement("div");fl.style.cssText="position:fixed;inset:0;background:#fff;z-index:85;pointer-events:none";document.body.appendChild(fl);
 fl.animate([{opacity:0},{opacity:1,offset:.15},{opacity:0}],{duration:600}).onfinish=()=>fl.remove();
 const r=$("#polwrap").getBoundingClientRect();emo(r.left+r.width/2,r.top+r.height/2,["⭐","💖","✨","🌸","📸"],14);
 setTimeout(()=>{sfx.good();toast(pick(["Gorgeous! 📸","That smile though! 😍","Photo of the year! 🌟"]))},300);
 if(!st.flash){st.flash=1;addStars(2);checkBadges()}};

/* ---------- rewards ---------- */
function renderRewards(){
 const n=st.badges.length,pct=Math.round(Math.min(n,TASKS.length)/TASKS.length*100);
 $("#rpct").textContent=pct+"%";setTimeout(()=>$("#rfg").style.strokeDashoffset=326.7*(1-n/TASKS.length),50);
 $("#ticket").innerHTML=TASKS.map(t=>{const v=Math.min(t.val(),t.goal),ok=st.badges.includes(t.id);
  return `<div class="trow ${ok?"ok":""}"><div class="medal">${t.ico}</div><div class="tx"><b>${t.name}</b><small>${t.desc}</small><div class="mb"><i style="width:${v/t.goal*100}%"></i></div></div><div class="tv">${ok?"✓":v+"/"+t.goal}</div></div>`}).join("");
 const k=Math.min(st.stars,24);$("#starsN").textContent="("+st.stars+")";
 $("#starSlots").innerHTML=Array.from({length:24},(_,i)=>`<span class="sl ${i<k?"f":""}">${i<k?"⭐":""}</span>`).join("");
 $("#treatSlots").innerHTML=TR.map((t,i)=>`<span class="sl big ${st.treats.includes(i)?"f":""}">${st.treats.includes(i)?t.e:"?"}</span>`).join("");
 const ok=n>=NEED,b=$("#finBtn");b.classList.toggle("locked",!ok);b.textContent=ok?"🎡 Ride the Ferris Wheel!":`🔒 Earn ${NEED} badges to unlock the finale`}

/* ---------- finale ---------- */
let twDone=false;
function startFinale(){
 const s=$("#s-finale");
 if(!twDone){twDone=true;for(let i=0;i<30;i++){const d=document.createElement("div");d.className="tw";d.textContent=pick(["⭐","✨","💫"]);d.style.left=rnd(2,96)+"%";d.style.top=rnd(2,96)+"%";d.style.animationDelay=(-rnd(0,2))+"s";s.insertBefore(d,s.firstChild)}floaters(s,small?7:12)}
 $("#cake").classList.remove("out");$("#fmsg").classList.remove("on");$("#blowBtn").style.display="inline-flex";$("#fSub").textContent="Make a wish and blow out the candles! 🕯️";
 const words="Happy Children's Day, My Little Sunshine! 💗".split(" ");$("#fTitle").innerHTML=words.map((w,i)=>`<span style="animation-delay:${i*.14}s">${w}</span>`).join(" ");
 $("#fStat").textContent=`⭐ ${st.stars} stars · 🍬 ${st.treats.length} treats · 🏅 ${st.badges.length} badges`;
 confetti(innerWidth/2,innerHeight*.3,90);sfx.win();
 clearInterval(fireTimer);rocket();fireTimer=setInterval(rocket,1100)}
function blow(){
 const c=$("#cake");if(c.classList.contains("out"))return;c.classList.add("out");sfx.blow();$("#blowBtn").style.display="none";
 setTimeout(()=>{confetti(innerWidth/2,innerHeight*.45,120);sfx.win();for(let i=0;i<4;i++)setTimeout(rocket,i*300);
  $("#fSub").textContent="Your wish is on its way! ✨";$("#fmsg").classList.add("on");
  setTimeout(()=>$("#fmsg").scrollIntoView({behavior:"smooth",block:"center"}),500)},500)}
$("#cake").onclick=blow;$("#blowBtn").onclick=blow;
$("#replayBtn").onclick=()=>{sfx.tap();st=DEF();save();hudUpdate();refreshHub();go("intro")};

/* ---------- screen enter hooks ---------- */
const onEnter={
 hub:refreshHub,wheel:buildBulbs,candy:renderCandy,
 arcade:()=>{$("#bestB").textContent="Best: "+st.bestB;$("#bestS").textContent="Best: "+st.bestS;$("#bestT").textContent=st.teddyFound>=3?"Solved ✓":"Hide & seek"},
 balloon:()=>{$("#bReady").style.display="flex";$$("#bField .bl").forEach(e=>e.remove());$("#bScore").textContent=0;$("#bTime").textContent=30},
 stars:()=>{$("#cReady").style.display="flex";$$("#cField .fo").forEach(e=>e.remove());$("#cScore").textContent=0;$("#cTime").textContent=30;PLACE()},
 teddy:()=>{$("#tReady").style.display="flex";$$("#tField .ti").forEach(e=>e.remove());$("#tSay").textContent="Find the hidden teddy! 🧸";$("#tRound").textContent="Round 1/3";tEl=null},
 photo:()=>{if(!$("#stickLayer").children.length)initStickers()},
 rewards:renderRewards,finale:startFinale,intro:()=>{}
};
hudUpdate();refreshHub();
})();
</script>
</body>
</html>
