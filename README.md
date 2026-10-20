<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🎉 Chúc Mừng 20/10 - Lớp 6A8 💐</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; font-family:'Segoe UI',Tahoma,sans-serif; }
  body {
    min-height:100vh; display:flex; flex-direction:column;
    justify-content:center; align-items:center;
    background:linear-gradient(135deg,#ff9a9e 0%,#fecfef 50%,#fbc2eb 100%);
    overflow:hidden; position:relative; padding:20px;
  }
  canvas#fireworks { position:fixed; inset:0; pointer-events:none; z-index:1; }
  .card {
    background:rgba(255,255,255,0.92); padding:40px 30px; border-radius:25px;
    box-shadow:0 15px 50px rgba(255,105,180,0.5); text-align:center;
    max-width:480px; width:100%; z-index:10; animation:floatUp 1s ease-out;
    border:3px solid #ffb6c1; position:relative;
  }
  @keyframes floatUp { from{opacity:0;transform:translateY(40px);} to{opacity:1;transform:translateY(0);} }
  .flowers { font-size:2.2rem; margin-bottom:15px; animation:swing 2s infinite ease-in-out; }
  @keyframes swing { 0%,100%{transform:rotate(-6deg);} 50%{transform:rotate(6deg);} }
  h1 {
    font-size:2.6rem; margin-bottom:5px;
    background:linear-gradient(90deg,#e91e63,#ff6b9d,#e91e63);
    background-size:200% auto; -webkit-background-clip:text; background-clip:text;
    -webkit-text-fill-color:transparent; animation:shine 3s linear infinite;
  }
  @keyframes shine { to{background-position:200% center;} }
  h2 { color:#c2185b; font-size:1.6rem; margin-bottom:20px; }
  .message { color:#555; line-height:1.8; margin-bottom:15px; font-size:1.05rem; }
  .wish { color:#d81b60; font-style:italic; line-height:1.7; margin-bottom:25px; font-size:1.05rem; }
  .btn-group { display:flex; gap:10px; justify-content:center; flex-wrap:wrap; }
  button {
    background:linear-gradient(135deg,#ff6b9d,#e91e63); color:white; border:none;
    padding:12px 26px; border-radius:50px; font-size:1rem; cursor:pointer;
    box-shadow:0 5px 15px rgba(233,30,99,0.4); transition:all 0.3s;
  }
  button:hover { transform:translateY(-3px) scale(1.05); box-shadow:0 8px 22px rgba(233,30,99,0.6); }
  button.secondary { background:linear-gradient(135deg,#ba68c8,#8e24aa); }
  .surprise { margin-top:20px; font-size:1.1rem; color:#e91e63; font-weight:bold; min-height:30px; animation:pop 0.5s ease; }
  @keyframes pop { 0%{transform:scale(0);} 80%{transform:scale(1.2);} 100%{transform:scale(1);} }
  footer { margin-top:20px; color:#fff; font-size:0.9rem; text-shadow:1px 1px 2px rgba(0,0,0,0.25); z-index:10; }
  #musicBtn {
    position:fixed; top:15px; right:15px; width:50px; height:50px; border-radius:50%;
    background:rgba(255,255,255,0.9); color:#e91e63; font-size:1.4rem;
    border:2px solid #ffb6c1; cursor:pointer; z-index:9999;
    box-shadow:0 4px 12px rgba(233,30,99,0.3);
    display:flex; align-items:center; justify-content:center; transition:transform 0.2s;
  }
  #musicBtn:hover { transform:scale(1.1); }
  #musicBtn.playing { animation:spin 3s linear infinite; }
  @keyframes spin { to{transform:rotate(360deg);} }
  @media (max-width:500px) { h1{font-size:2rem;} h2{font-size:1.2rem;} .card{padding:30px 20px;} }
</style>
</head>
<body>

<canvas id="fireworks"></canvas>
<button id="musicBtn" title="Bật/Tắt nhạc">🔇</button>

<div class="card">
  <div class="flowers">🌸 🌷 🌹 🌺 🌼</div>
  <h1>Happy 20/10</h1>
  <h2>Lớp 6A8 💖</h2>
  <p class="message">
    Nhân ngày Phụ nữ Việt Nam 20/10,<br>
    tập thể lớp <strong>6A8</strong> xin gửi đến<br>
    các cô giáo và các bạn nữ<br>
    những lời chúc tốt đẹp nhất! 💕
  </p>
  <p class="wish">Chúc mọi người luôn xinh đẹp,<br>vui vẻ, hạnh phúc và thành công! 🌷</p>
  <div class="btn-group">
    <button onclick="showLove()">💌 Lời chúc bất ngờ</button>
    <button class="secondary" onclick="boomFireworks()">🎆 Bắn pháo hoa</button>
  </div>
  <div id="surprise" class="surprise"></div>
</div>
<footer>Made with ❤️ by Lớp 6A8</footer>

<audio id="bgMusic" loop>
  <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_c8c8a73467.mp3?filename=happy-birthday-114173.mp3" type="audio/mpeg">
</audio>

<script>
(function () {
  "use strict";

  const wishes = [
    "🌷 Chúc các bạn nữ lớp 6A8 luôn xinh đẹp và rạng rỡ!",
    "💐 Chúc các cô giáo thật nhiều sức khỏe và hạnh phúc!",
    "🌸 Mong tập thể 6A8 mãi đoàn kết và yêu thương nhau!",
    "💖 Chúc mừng ngày Phụ nữ Việt Nam 20/10 vui vẻ!",
    "🌹 Chúc mọi điều tốt đẹp nhất sẽ đến với các bạn nữ 6A8!",
    "💕 Chúc một nửa thế giới của 6A8 luôn tỏa sáng!",
    "🎀 Chúc các bạn nữ học giỏi, chăm ngoan, xinh tươi!",
    "🌺 Chúc 6A8 luôn là một tập thể tuyệt vời nhất!"
  ];

  /* ----- TRÁI TIM ----- */
  const heartEmojis = ["💖","💕","🌸","💗","🌷","💐","❤️","🌺","🌹"];
  function createHeart() {
    const heart = document.createElement("div");
    heart.textContent = heartEmojis[Math.floor(Math.random()*heartEmojis.length)];
    heart.style.cssText = `position:fixed;left:${Math.random()*100}%;bottom:-40px;
      font-size:${Math.random()*20+15}px;pointer-events:none;z-index:999;
      animation:heartRise ${Math.random()*4+4}s linear forwards;`;
    document.body.appendChild(heart);
    setTimeout(()=>heart.remove(),8000);
  }
  const hs = document.createElement("style");
  hs.textContent = `@keyframes heartRise{0%{transform:translateY(0) rotate(0) scale(1);opacity:1;}100%{transform:translateY(-110vh) rotate(360deg) scale(.6);opacity:0;}}`;
  document.head.appendChild(hs);
  setInterval(createHeart, 450);

  /* ----- PHÁO HOA ----- */
  const canvas = document.getElementById("fireworks");
  const ctx = canvas.getContext("2d");
  function resize(){ canvas.width=innerWidth; canvas.height=innerHeight; }
  resize(); addEventListener("resize", resize);
  const colors = ["#e91e63","#ff6b9d","#fbc2eb","#ffd54f","#64b5f6","#ba68c8","#ff8a65","#4dd0e1"];
  let particles = [];
  class Particle {
    constructor(x,y,c){
      this.x=x; this.y=y;
      const a=Math.random()*Math.PI*2, s=Math.random()*6+2;
      this.vx=Math.cos(a)*s; this.vy=Math.sin(a)*s;
      this.color=c; this.life=1; this.decay=Math.random()*0.015+0.01;
      this.size=Math.random()*3+2;
    }
    update(){ this.x+=this.vx; this.y+=this.vy; this.vy+=0.05; this.vx*=0.99; this.vy*=0.99; this.life-=this.decay; }
    draw(){ ctx.globalAlpha=this.life; ctx.fillStyle=this.color;
      ctx.beginPath(); ctx.arc(this.x,this.y,this.size,0,Math.PI*2); ctx.fill(); }
  }
  function explode(x,y){
    const c=colors[Math.floor(Math.random()*colors.length)];
    for(let i=0;i<80;i++) particles.push(new Particle(x,y,c));
  }
  function animate(){
    ctx.fillStyle="rgba(0,0,0,0.08)"; ctx.fillRect(0,0,canvas.width,canvas.height);
    particles=particles.filter(p=>p.life>0);
    particles.forEach(p=>{p.update();p.draw();});
    ctx.globalAlpha=1;
    requestAnimationFrame(animate);
  }
  animate();
  function autoFireworks(){ explode(Math.random()*canvas.width, Math.random()*canvas.height*0.6); }
  setInterval(autoFireworks, 1500);
  setTimeout(autoFireworks, 300);
  window.boomFireworks = function(){
    for(let i=0;i<5;i++) setTimeout(()=>explode(
      Math.random()*canvas.width, Math.random()*canvas.height*0.6), i*200);
  };

  /* ----- CONFETTI ----- */
  function createConfetti(){
    const cs=["#e91e63","#ff6b9d","#ffd54f","#64b5f6","#ba68c8","#4dd0e1","#81c784"];
    for(let i=0;i<60;i++){
      const p=document.createElement("div");
      const s=Math.random()*8+4;
      p.style.cssText=`position:fixed;top:-20px;left:${Math.random()*100}%;
        width:${s}px;height:${s}px;background:${cs[Math.floor(Math.random()*cs.length)]};
        border-radius:${Math.random()>0.5?'50%':'2px'};pointer-events:none;z-index:9998;
        animation:confettiFall ${Math.random()*2+3}s linear forwards;
        transform:rotate(${Math.random()*360}deg);`;
      document.body.appendChild(p);
      setTimeout(()=>p.remove(),5000);
    }
  }
  const cfs = document.createElement("style");
  cfs.textContent = `@keyframes confettiFall{0%{transform:translateY(0) rotate(0);opacity:1;}100%{transform:translateY(110vh) rotate(720deg);opacity:0;}}`;
  document.head.appendChild(cfs);

  /* ----- THIỆP POPUP ----- */
  function showGreeting(){
    const ov=document.createElement("div");
    ov.style.cssText=`position:fixed;inset:0;background:rgba(0,0,0,.6);
      backdrop-filter:blur(5px);display:flex;align-items:center;justify-content:center;
      z-index:10000;opacity:0;transition:opacity .4s ease;padding:20px;`;
    const card=document.createElement("div");
    card.style.cssText=`background:linear-gradient(135deg,#fff0f6,#ffe4ec);
      padding:35px 28px;border-radius:25px;max-width:90vw;width:440px;
      text-align:center;box-shadow:0 20px 60px rgba(233,30,99,.5);
      border:3px solid #ffb6c1;transform:scale(.7);
      transition:transform .4s cubic-bezier(.34,1.56,.64,1);`;
    const rw = wishes[Math.floor(Math.random()*wishes.length)];
    card.innerHTML = `
      <div style="font-size:2.8rem;margin-bottom:10px;">🌸💐🌷</div>
      <h1 style="color:#e91e63;font-size:1.9rem;margin:0 0 8px;">Happy 20/10</h1>
      <h2 style="color:#c2185b;font-size:1.3rem;margin:0 0 18px;">Lớp 6A8 💖</h2>
      <p style="color:#555;line-height:1.7;font-size:1rem;margin-bottom:22px;">${rw}</p>
      <button id="tt-close" style="background:linear-gradient(135deg,#ff6b9d,#e91e63);
        color:#fff;border:none;padding:12px 32px;border-radius:50px;
        font-size:1rem;cursor:pointer;box-shadow:0 5px 15px rgba(233,30,99,.4);">
        💌 Cảm ơn!</button>`;
    ov.appendChild(card);
    document.body.appendChild(ov);
    requestAnimationFrame(()=>{ ov.style.opacity="1"; card.style.transform="scale(1)"; });
    for(let i=0;i<25;i++) setTimeout(createHeart, i*60);
    boomFireworks(); createConfetti();
    card.querySelector("#tt-close").addEventListener("click",()=>{
      ov.style.opacity="0"; card.style.transform="scale(.7)";
      setTimeout(()=>ov.remove(),400);
    });
    ov.addEventListener("click",e=>{
      if(e.target===ov) card.querySelector("#tt-close").click();
    });
  }

  /* ----- LỜI CHÚC BẤT NGỜ ----- */
  window.showLove = function(){
    const s=document.getElementById("surprise");
    s.textContent = wishes[Math.floor(Math.random()*wishes.length)];
    s.style.animation="none"; void s.offsetWidth; s.style.animation="pop 0.5s ease";
    for(let i=0;i<20;i++) setTimeout(createHeart,i*50);
    createConfetti();
  };

  /* ----- NHẠC ----- */
  const music = document.getElementById("bgMusic");
  const musicBtn = document.getElementById("musicBtn");
  let musicOn = false;

  function playMusic(){
    if(musicOn) return;
    music.volume = 0.4;
    music.play().then(()=>{
      musicOn = true;
      musicBtn.textContent = "🎵";
      musicBtn.classList.add("playing");
    }).catch(()=>{});
  }

  function pauseMusic(){
    music.pause();
    musicOn = false;
    musicBtn.textContent = "🔇";
    musicBtn.classList.remove("playing");
  }

  musicBtn.addEventListener("click",(e)=>{
    e.stopPropagation();
    musicOn ? pauseMusic() : playMusic();
  });

  /* ============================================
     ⚡ TỰ ĐỘNG HIỆN NGAY KHI MỞ LINK
     ============================================ */

  // Kiểm tra URL có ?auto=1 hoặc ?auto không
  const isAuto = /[?&]auto(=1)?(&|$)/.test(location.search);

  // Hiện thiệp NGAY LẬP TỨC (100ms)
  setTimeout(showGreeting, isAuto ? 100 : 900);

  // Nếu ?auto=1 → bắn thêm pháo hoa + confetti ngay
  if (isAuto) {
    setTimeout(boomFireworks, 200);
    setTimeout(createConfetti, 350);
  }

  // 🎵 TỰ PHÁT NHẠC KHI USER CHẠM LẦN ĐẦU (browser bắt buộc)
  const autoMusic = () => { playMusic(); };
  document.addEventListener("click", autoMusic, { once: true });
  document.addEventListener("touchstart", autoMusic, { once: true });
  document.addEventListener("keydown", autoMusic, { once: true });

  // Cho phép gọi lại từ console
  window.showTwentyTen = showGreeting;

  console.log("💐 Chúc mừng 20/10 - Lớp 6A8! 💖");
  console.log("💡 Gõ showTwentyTen() để mở lại thiệp");
})();
</script>
</body>
</html>
