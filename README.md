# Q-N-A
<!doctype html>
<html lang="bn">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>মজার প্রশ্নোত্তর 😄</title>
  <style>
    :root{ --bg:#f7f9fc; --card:#ffffff; --accent:#0b6cf6; --muted:#6b7280; --radius:12px; }
    *{box-sizing:border-box}
    body{font-family:Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; margin:0; background:var(--bg); color:#0f172a}
    header{background:linear-gradient(90deg,#0b6cf6 0%, #4f46e5 100%); color:white; padding:36px 18px}
    .container{max-width:980px; margin: -28px auto 48px; padding:0 16px}
    .card{background:var(--card); border-radius:var(--radius); box-shadow:0 6px 24px rgba(11,108,246,0.08); padding:20px}
    h1{margin:0 0 8px; font-size:28px}
    p.lead{margin:0; color:rgba(255,255,255,0.9)}

    .search-row{display:flex; gap:12px; margin-top:18px}
    .search-row input{flex:1; padding:12px 14px; border-radius:10px; border:0; outline:none}
    .search-row button{background:#fff; color:var(--accent); border:0; padding:10px 14px; border-radius:10px; font-weight:600; cursor:pointer}

    main{max-width:980px; margin:16px auto; padding:0 16px}
    .faq-grid{display:grid; grid-template-columns:1fr; gap:14px}

    .faq-item{background:linear-gradient(180deg, rgba(255,255,255,0.98), rgba(247,249,252,0.98)); border-radius:12px; overflow:hidden; box-shadow:0 6px 18px rgba(2,6,23,0.06)}
    .faq-q{padding:16px 18px; cursor:pointer; display:flex; justify-content:space-between; align-items:center}
    .faq-q h3{margin:0; font-size:16px}
    .faq-a{padding:0 18px 18px 18px; border-top:1px solid rgba(15,23,42,0.04); display:none}
    .faq-a p{margin:12px 0 0; color:var(--muted)}

    footer{max-width:980px; margin:32px auto; padding:12px 16px; color:var(--muted); font-size:14px}

    @media(min-width:720px){ .faq-grid{grid-template-columns:1fr 1fr} }
  </style>
</head>
<body>
  <header>
    <div style="max-width:980px;margin:0 auto;padding:0 16px;">
      <h1>মজার প্রশ্নোত্তর 😄</h1>
      <p class="lead">হাসির ছলে কিছু মজার সাধারণ প্রশ্ন ও তাদের উত্তর!</p>
      <div class="search-row" style="margin-top:18px;">
        <input id="search" placeholder="প্রশ্ন লিখে খুঁজুন..." />
        <button id="clearBtn">সাফ করুন</button>
      </div>
    </div>
  </header>

  <main>
    <div class="container">
      <section class="faq-grid" id="faqs"></section>
    </div>
  </main>

  <footer>
    © 2025 মজার প্রশ্নোত্তর — হাসি মুখে থাকুন 😄
  </footer>

  <script>
    const faqData = [
      {q: 'ঘুম আসছে না, কী করব?', a: 'চোখ বন্ধ করে ভাবো তুমি ক্লাসে বসে আছো... ঘুম নিজে থেকেই চলে আসবে 😴'},
      {q: 'আমার মোবাইলের ব্যাটারি কেন এত তাড়াতাড়ি শেষ হয়?', a: 'কারণ মোবাইলও তোমার মতো ক্লান্ত হয়ে যায় স্ক্রল করতে করতে 😂'},
      {q: 'আমি কি সুন্দর?', a: 'তুমি আয়নায় তাকাও — যদি আয়নাটা হাসে, তাহলে বুঝে নিও “হ্যাঁ!” 😁'},
      {q: 'টাকা জমাতে পারছি না, কী করব?', a: 'টাকা জমানোর আগে “Add to Cart” বোতামটা থেকে একটু দূরে থাকো 🛒'},
      {q: 'প্রেম করলে কি ঘুম কমে যায়?', a: 'না, ঘুম যায় না — শুধু নোটিফিকেশন চেক করতে করতে রাত চলে যায় ❤️'},
      {q: 'আমার গার্লফ্রেন্ড রাগ করেছে, এখন কী করব?', a: 'প্রথমে চা বানাও, তারপর “Sorry” বলো — চা ঠান্ডা হওয়ার আগেই ক্ষমা পেয়ে যাবে ☕'},
      {q: 'আমি কি একদিন বিখ্যাত হব?', a: 'অবশ্যই! অন্তত মিম পেজে একটা দিন তো হবেই 😎'},
      {q: 'অনলাইনে এত মেম কেন?', a: 'কারণ মানুষ এখন হাসে Wi-Fi দিয়ে, পেট দিয়ে না 😂'},
      {q: 'আমার পেটের মেদ কমছে না কেন?', a: 'কারণ মেদও তোমার মতো জায়গা ছাড়তে চায় না 😜'},
      {q: 'পরীক্ষার আগের রাতে কীভাবে শান্ত থাকা যায়?', a: 'সহজ! বই বন্ধ করে ফ্রিজ খুলে চকলেট খাও 🍫'}
    ];

    const faqsEl = document.getElementById('faqs');

    function renderFaqs(list){
      faqsEl.innerHTML = '';
      list.forEach((item, idx)=>{
        const wrap = document.createElement('article'); wrap.className='faq-item';
        wrap.innerHTML = `
          <div class="faq-q" data-idx="${idx}">
            <h3>${item.q}</h3>
            <div style="font-size:18px;opacity:0.6">+</div>
          </div>
          <div class="faq-a"><p>${item.a}</p></div>
        `;
        faqsEl.appendChild(wrap);
      });
      attachToggle();
    }

    function attachToggle(){
      document.querySelectorAll('.faq-q').forEach(node=>{
        node.onclick = ()=>{
          const a = node.nextElementSibling;
          const sign = node.querySelector('div');
          if(a.style.display === 'block'){ a.style.display='none'; sign.textContent='+';} else { a.style.display='block'; sign.textContent='−'; }
        }
      })
    }

    const search = document.getElementById('search');
    search.addEventListener('input', ()=>{
      const q = search.value.trim().toLowerCase();
      if(!q) return renderFaqs(faqData);
      const filtered = faqData.filter(f=> f.q.toLowerCase().includes(q) || f.a.toLowerCase().includes(q));
      renderFaqs(filtered.length? filtered : [{q: 'কোনো ফলাফল নেই 😅', a: 'এই প্রশ্নের কোনো উত্তর মজার তালিকায় নেই!'}]);
    });

    document.getElementById('clearBtn').addEventListener('click', ()=>{ search.value=''; renderFaqs(faqData); search.focus(); });

    renderFaqs(faqData);
  </script>
</body>
</html>

