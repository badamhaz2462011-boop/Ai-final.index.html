# Ai-final.index.html

<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>STOP STORE | DYNAMIC SYSTEM</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&family=Orbitron:wght@700;900&display=swap" rel="stylesheet">
    <style>
        :root { --p: #6366f1; --bg: #000; --card: #0d0d0d; }
        
        /* جعل الصفحة مرنة وتمنع السكرول العرضي */
        body { 
            background: var(--bg); color: white; font-family: 'Cairo', sans-serif; 
            margin: 0; padding: 0; overflow-x: hidden; width: 100%;
        }
        .h-font { font-family: 'Orbitron', sans-serif; }

        /* Navbar ديناميكي */
        nav { 
            position: fixed; top: 0; width: 100%; z-index: 1000; 
            background: rgba(0,0,0,0.9); backdrop-filter: blur(20px); 
            border-bottom: 1px solid #111; padding: 12px 5%; 
            display: flex; align-items: center; justify-content: space-between; 
        }
        .nav-links { display: flex; gap: clamp(10px, 3vw, 25px); } /* مسافات تتغير حسب حجم الشاشة */
        .nav-link { font-size: clamp(9px, 2vw, 11px); color: #444; cursor: pointer; font-weight: 900; letter-spacing: 1px; }
        .nav-link.active { color: #fff; text-shadow: 0 0 10px var(--p); }

        /* Sections */
        section { display: none; padding-top: 100px; min-height: 100vh; width: 100%; }
        section.active { display: block; animation: fadeIn 0.5s ease; }

        /* تصميم الكروت الديناميكي (Grid) */
        .grid-container { 
            display: grid; 
            /* السحر هنا: بيحدد عدد الأعمدة تلقائياً بناءً على عرض الشاشة */
            grid-template-columns: repeat(auto-fill, minmax(clamp(250px, 100%, 320px), 1fr)); 
            gap: 20px; padding: 20px; max-width: 1200px; margin: 0 auto; 
        }

        /* حاوية الأدمن (تتأقلم مع الشاشات الصغيرة) */
        .admin-box { 
            background: var(--card); border-radius: 30px; padding: clamp(20px, 5vw, 40px); 
            width: 90%; max-width: 450px; margin: 20px auto; border: 1px solid #1a1a1a; 
        }

        /* المدخلات (Inputs) */
        .luxury-input { 
            width: 100%; background: #050505; border: 1px solid #151515; 
            height: 55px; border-radius: 18px; padding: 0 15px; color: white; 
            margin-bottom: 15px; outline: none; font-size: 14px; font-weight: 700;
        }

        .btn { 
            background: var(--p); color: white; width: 100%; height: 55px; 
            border-radius: 18px; font-weight: 900; cursor: pointer; transition: 0.3s; 
        }

        /*Responsive Image View*/
        .img-view { height: clamp(150px, 30vh, 200px); display: flex; justify-content: center; align-items: center; background: #080808; border-radius: 20px; margin-bottom: 15px; }
        .img-view img { max-width: 80%; max-height: 80%; object-fit: contain; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .hidden { display: none; }
    </style>
</head>
<body onload="renderItems()">

    <nav>
        <div class="logo h-font text-xl italic font-black">.STOP</div>
        <div class="nav-links h-font">
            <span onclick="tab('home')" id="l-home" class="nav-link active">HOME</span>
            <span onclick="tab('shop')" id="l-shop" class="nav-link">SHOP</span>
            <span onclick="tab('dash')" id="l-dash" class="nav-link">DASHBOARD</span>
        </div>
        <div class="hidden md:block" style="width: 40px;"></div>
    </nav>

    <main>
        <section id="home" class="active text-center">
            <div class="mt-20 px-6">
                <h1 class="h-font text-[clamp(40px,10vw,80px)] font-black italic leading-tight">PURE<br><span style="color:var(--p)">STUFF.</span></h1>
                <button onclick="tab('shop')" class="btn w-56 mt-12 h-font uppercase tracking-widest">Explore Shop</button>
            </div>
        </section>

        <section id="shop">
            <div id="shop-grid" class="grid-container"></div>
        </section>

        <section id="checkout">
            <div class="admin-box">
                <h2 class="h-font text-lg mb-6 italic text-center">CHECKOUT</h2>
                <div id="order-summary" class="mb-6 p-4 bg-black rounded-xl border border-zinc-900 flex items-center gap-4"></div>
                <input type="text" placeholder="Full Name" class="luxury-input">
                <input type="text" placeholder="Address" class="luxury-input">
                <button onclick="alert('Order Placed!')" class="btn h-font">CONFIRM</button>
                <button onclick="tab('shop')" class="text-zinc-600 text-[10px] mt-4 w-full text-center uppercase">Cancel</button>
            </div>
        </section>

        <section id="dash">
            <div class="admin-box text-center">
                <h2 class="h-font text-base mb-8 text-zinc-500 italic">GATE ACCESS</h2>
                <input type="password" id="pass" placeholder="Password" class="luxury-input text-center h-font">
                <button onclick="unlock()" class="btn h-font">UNLOCK</button>
            </div>
        </section>

        <section id="add-page">
            <div class="admin-box">
                <h2 class="text-xl font-black mb-6 text-center">ADD PRODUCT</h2>
                <input type="text" id="p-name" placeholder="Name" class="luxury-input">
                <input type="number" id="p-price" placeholder="Price ($)" class="luxury-input">
                <label for="p-file" id="file-label" class="flex items-center justify-center border-2 border-dashed border-zinc-800 rounded-xl h-14 mb-4 text-xs cursor-pointer hover:border-indigo-500 transition-colors">Upload Image</label>
                <input type="file" id="p-file" class="hidden" accept="image/*">
                <input type="text" id="p-url" placeholder="Or URL" class="luxury-input">
                <button onclick="saveItem()" class="btn h-font">PUBLISH</button>
            </div>
        </section>
    </main>

    <script>
        let currentImg = "";
        const baseProducts = [
            {name: 'Apple Ultra', price: '450', img: 'https://i.ibb.co/Q3LvcP2k/image.png'},
            {name: 'Aero Vision', price: '180', img: 'https://i.ibb.co/nMk4vyG0/image.png'}
        ];

        function tab(id) {
            document.querySelectorAll('section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(document.getElementById('l-'+id)) document.getElementById('l-'+id).classList.add('active');
            window.scrollTo(0,0);
        }

        function unlock() {
            if(document.getElementById('pass').value === '44556799') tab('add-page');
            else alert('Wrong Pass');
        }

        function openCheckout(name, price, img) {
            document.getElementById('order-summary').innerHTML = `<img src="${img}" class="w-12 h-12 object-contain bg-zinc-900 rounded-lg"><div><div class="text-xs font-bold uppercase">${name}</div><div class="text-indigo-500 font-bold">$${price}</div></div>`;
            tab('checkout');
        }

        document.getElementById('p-file').addEventListener('change', function(e) {
            const r = new FileReader();
            r.onload = () => { currentImg = r.result; document.getElementById('file-label').innerText = "Ready ✅"; };
            r.readAsDataURL(e.target.files[0]);
        });

        function saveItem() {
            const n = document.getElementById('p-name').value;
            const p = document.getElementById('p-price').value;
            const u = document.getElementById('p-url').value;
            const img = currentImg || u;
            if(!n || !p || !img) return alert('Missing info');
            const db = JSON.parse(localStorage.getItem('stop_db')) || [];
            db.push({name:n, price:p, img:img});
            localStorage.setItem('stop_db', JSON.stringify(db));
            alert('Published!');
            renderItems(); tab('shop');
        }

        function renderItems() {
            const grid = document.getElementById('shop-grid');
            const added = JSON.parse(localStorage.getItem('stop_db')) || [];
            const all = [...baseProducts, ...added];
            grid.innerHTML = all.map(p => `
                <div class="bg-[#0d0d0d] rounded-[30px] p-5 border border-[#111] flex flex-col justify-between">
                    <div class="img-view"><img src="${p.img}"></div>
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="h-font text-sm font-bold uppercase italic">${p.name}</h3>
                        <span class="text-lg font-black text-indigo-500 h-font">$${p.price}</span>
                    </div>
                    <button onclick="openCheckout('${p.name}', '${p.price}', '${p.img}')" class="btn !h-10 text-[10px] uppercase tracking-tighter">Buy Now</button>
                </div>
            `).join('');
        }
    </script>
</body>
</html>
