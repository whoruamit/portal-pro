<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Suman Saurabh | Digital Visionary Gallery</title>
    <!-- Tailwind CSS for Responsive Design -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        
        :root {
            --primary: #6366f1;
            --secondary: #a855f7;
            --accent: #f43f5e;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #fcfcfd;
            overflow-x: hidden;
        }

        /* Responsive Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }

        /* Animations */
        @keyframes blob {
            0% { transform: translate(0px, 0px) scale(1); }
            33% { transform: translate(30px, -50px) scale(1.1); }
            66% { transform: translate(-20px, 20px) scale(0.9); }
            100% { transform: translate(0px, 0px) scale(1); }
        }

        @keyframes cardReveal {
            from { opacity: 0; transform: translateY(30px); filter: blur(5px); }
            to { opacity: 1; transform: translateY(0); filter: blur(0px); }
        }

        .animate-blob { animation: blob 10s infinite alternate cubic-bezier(0.45, 0, 0.55, 1); }
        
        .glass {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.5);
        }

        .quote-card {
            opacity: 0;
            animation: cardReveal 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }

        .category-pill.active {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            box-shadow: 0 8px 15px -3px rgba(99, 102, 241, 0.3);
            border-color: transparent;
        }

        /* Modal styling */
        .modal-backdrop {
            display: none;
            background: rgba(15, 23, 42, 0.4);
            backdrop-filter: blur(10px);
            z-index: 100;
        }
        .modal-backdrop.active { display: flex; }

        .admin-controls { display: none; }
        .is-admin .admin-controls { display: flex; }

        .post-image {
            width: 100%;
            height: 240px;
            object-fit: cover;
            border-radius: 1.5rem;
            margin-bottom: 1.25rem;
        }

        /* Notification Toast */
        #toast {
            transform: translate(-50%, 120%);
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        #toast.active { transform: translate(-50%, 0); }

        .upload-area {
            border: 2px dashed #e2e8f0;
            transition: all 0.3s;
        }
        .upload-area:hover {
            border-color: #6366f1;
            background-color: #f8fafc;
        }
    </style>
</head>
<body class="overflow-x-hidden selection:bg-indigo-100 selection:text-indigo-900">

    <!-- Background Orbs -->
    <div class="fixed inset-0 -z-10 overflow-hidden pointer-events-none">
        <div class="absolute top-[-10%] left-[-5%] w-[300px] md:w-[600px] h-[300px] md:h-[600px] bg-indigo-200/30 rounded-full filter blur-[80px] md:blur-[120px] animate-blob"></div>
        <div class="absolute bottom-[10%] right-[-5%] w-[250px] md:w-[500px] h-[250px] md:h-[500px] bg-purple-200/30 rounded-full filter blur-[80px] md:blur-[120px] animate-blob" style="animation-delay: 2s"></div>
    </div>

    <!-- Navigation -->
    <nav class="sticky top-0 z-50 glass border-b border-white/40">
        <div class="max-w-7xl mx-auto px-4 md:px-6 h-16 md:h-20 flex items-center justify-between">
            <div class="flex items-center gap-3 group cursor-pointer" onclick="window.scrollTo({top: 0, behavior: 'smooth'})">
                <div class="w-10 h-10 md:w-12 md:h-12 bg-indigo-600 rounded-xl md:rounded-2xl flex items-center justify-center text-white shadow-lg shadow-indigo-100 group-hover:rotate-[-5deg] transition-all">
                    <i data-lucide="sparkles" class="w-5 h-5 md:w-6 md:h-6"></i>
                </div>
                <div class="flex flex-col">
                    <span class="text-lg md:text-2xl font-black tracking-tighter text-slate-900 leading-none uppercase">Suman Saurabh</span>
                    <span class="text-[8px] md:text-[10px] font-bold text-indigo-500 uppercase tracking-[0.2em] md:tracking-[0.4em] mt-0.5 md:mt-1">Gallery</span>
                </div>
            </div>

            <div class="flex items-center gap-2 md:gap-4">
                <div id="admin-nav" class="admin-controls items-center gap-2 md:gap-3">
                    <button onclick="openQuoteModal()" class="hidden sm:flex bg-slate-900 text-white px-4 md:px-6 py-2 md:py-2.5 rounded-xl md:rounded-2xl font-bold items-center gap-2 hover:bg-indigo-600 transition-all text-sm">
                        <i data-lucide="plus" class="w-4 h-4"></i> <span>Add</span>
                    </button>
                    <button onclick="logoutAdmin()" class="p-2 md:p-3 text-slate-400 hover:text-red-500 hover:bg-red-50 rounded-xl transition-all">
                        <i data-lucide="log-out" class="w-5 h-5 md:w-6 md:h-6"></i>
                    </button>
                </div>
                <button id="lock-btn" onclick="openLoginModal()" class="w-10 h-10 md:w-12 md:h-12 rounded-xl md:rounded-2xl flex items-center justify-center text-slate-400 hover:text-indigo-600 hover:bg-white transition-all border border-slate-100 shadow-sm">
                    <i data-lucide="lock" class="w-5 h-5 md:w-6 md:h-6"></i>
                </button>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <header class="max-w-7xl mx-auto px-4 md:px-6 pt-12 md:pt-20 pb-10 md:pb-16 text-center">
        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-indigo-50 text-indigo-600 text-[10px] font-black uppercase tracking-widest mb-6 md:mb-8">
            <i data-lucide="star" class="w-3 h-3"></i> Exclusive Thoughts
        </div>
        <h2 class="text-4xl md:text-7xl lg:text-8xl font-black text-slate-900 tracking-tightest mb-8 md:mb-10 leading-[1.1] md:leading-[1]">
            Visions in <br class="hidden md:block"> <span class="text-indigo-600">Pure Quality.</span>
        </h2>
        
        <div class="max-w-xl mx-auto glass p-1.5 rounded-2xl md:rounded-[2rem] shadow-xl shadow-indigo-100/40 mb-10 md:mb-12 flex items-center">
            <div class="pl-4 md:pl-6 text-slate-400">
                <i data-lucide="search" class="w-5 h-5 md:w-6 md:h-6"></i>
            </div>
            <input type="text" id="search-input" onkeyup="filterQuotes()" placeholder="Dhoondhein vichar..." class="w-full px-3 md:px-4 py-3 md:py-4 bg-transparent outline-none font-medium text-base md:text-lg text-slate-700">
        </div>

        <div id="category-filters" class="flex items-center justify-start md:justify-center gap-2 overflow-x-auto pb-4 md:pb-0 scrollbar-hide px-2">
            <button onclick="setCategory('All')" class="category-pill active whitespace-nowrap px-5 md:px-8 py-2 md:py-3 rounded-full font-bold text-xs md:text-sm transition-all border border-slate-200 bg-white">All</button>
            <button onclick="setCategory('Wisdom')" class="category-pill whitespace-nowrap px-5 md:px-8 py-2 md:py-3 rounded-full font-bold text-sm transition-all border border-slate-200 bg-white">Wisdom</button>
            <button onclick="setCategory('Success')" class="category-pill whitespace-nowrap px-5 md:px-8 py-2 md:py-3 rounded-full font-bold text-sm transition-all border border-slate-200 bg-white">Success</button>
            <button onclick="setCategory('Motivation')" class="category-pill whitespace-nowrap px-5 md:px-8 py-2 md:py-3 rounded-full font-bold text-sm transition-all border border-slate-200 bg-white">Motivation</button>
            <button onclick="setCategory('Life')" class="category-pill whitespace-nowrap px-5 md:px-8 py-2 md:py-3 rounded-full font-bold text-sm transition-all border border-slate-200 bg-white">Life</button>
        </div>
    </header>

    <!-- Content Area -->
    <main class="max-w-7xl mx-auto px-4 md:px-6 mb-32">
        <div id="loader" class="flex flex-col items-center justify-center py-24 md:py-40">
            <div class="w-12 h-12 md:w-16 md:h-16 border-4 md:border-8 border-indigo-100 border-t-indigo-600 rounded-full animate-spin mb-4 md:mb-6"></div>
            <p class="font-bold text-slate-400 uppercase text-[10px] tracking-widest">Loading Gallery</p>
        </div>

        <div id="grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 md:gap-10 hidden"></div>

        <div id="empty-state" class="hidden text-center py-20 md:py-40 glass rounded-[2rem] md:rounded-[4rem] px-6">
            <i data-lucide="search-x" class="w-12 h-12 md:w-16 md:h-16 mx-auto text-slate-300 mb-4 md:mb-6"></i>
            <h3 class="text-xl md:text-2xl font-bold text-slate-800">Kuch nahi mila</h3>
        </div>
    </main>

    <!-- Mobile Admin Button -->
    <button onclick="openQuoteModal()" class="admin-controls fixed bottom-6 right-6 w-14 h-14 bg-slate-900 text-white rounded-2xl shadow-2xl z-50 flex items-center justify-center sm:hidden active:scale-90 transition-transform">
        <i data-lucide="plus" class="w-6 h-6"></i>
    </button>

    <!-- Admin Login Modal -->
    <div id="modal-login" class="modal-backdrop fixed inset-0 items-center justify-center p-4 md:p-6">
        <div class="bg-white w-full max-w-sm md:max-w-md rounded-[2.5rem] p-8 md:p-10 shadow-2xl">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-2xl font-black text-slate-900">Admin Login</h3>
                <button onclick="closeModals()"><i data-lucide="x" class="w-6 h-6"></i></button>
            </div>
            <div class="space-y-6">
                <input type="password" id="pass-input" placeholder="Password (delhi)" class="w-full px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-indigo-500 outline-none font-bold text-center">
                <button onclick="verifyPass()" class="w-full bg-indigo-600 text-white py-4 rounded-2xl font-black text-lg shadow-lg shadow-indigo-100 active:scale-95 transition-all">
                    Unlock Panel
                </button>
            </div>
        </div>
    </div>

    <!-- Edit/Add Modal -->
    <div id="modal-quote" class="modal-backdrop fixed inset-0 items-center justify-center p-4 md:p-6">
        <div class="bg-white w-full max-w-2xl rounded-[2.5rem] md:rounded-[3.5rem] overflow-hidden shadow-2xl">
            <div class="bg-slate-50 px-6 md:px-10 py-6 md:py-8 border-b flex justify-between items-center">
                <h3 id="quote-modal-title" class="text-xl md:text-2xl font-black text-slate-800">Post Insight</h3>
                <button onclick="closeModals()"><i data-lucide="x" class="w-6 h-6"></i></button>
            </div>
            <div class="p-6 md:p-10 space-y-4 md:space-y-6 overflow-y-auto max-h-[85vh]">
                <input type="hidden" id="edit-id">
                
                <div class="space-y-2">
                    <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Quote</label>
                    <textarea id="form-text" required placeholder="Kya vichar hai aapka?" class="w-full h-32 px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-indigo-500 outline-none font-medium text-lg resize-none"></textarea>
                </div>

                <div class="space-y-2">
                    <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Photo Upload</label>
                    <div id="upload-preview-container" class="hidden mb-2">
                        <img id="upload-preview" class="w-full h-32 object-cover rounded-xl">
                    </div>
                    <label class="upload-area flex flex-col items-center justify-center py-6 rounded-2xl cursor-pointer">
                        <i data-lucide="image-plus" class="text-slate-400 mb-2"></i>
                        <span class="text-sm font-bold text-slate-500">Photo Select Karein</span>
                        <input type="file" id="form-file" accept="image/*" class="hidden" onchange="previewFile(this)">
                    </label>
                    <p class="text-[9px] text-slate-400 mt-1 italic text-center">Note: Photo 1MB se chhoti honi chahiye.</p>
                </div>
                
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Category</label>
                        <select id="form-cat" class="w-full px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none font-bold">
                            <option>Wisdom</option>
                            <option>Life</option>
                            <option>Motivation</option>
                            <option>Success</option>
                        </select>
                    </div>
                    <div class="space-y-2">
                        <label class="text-[10px] font-black uppercase text-slate-400 ml-2">Author</label>
                        <input type="text" id="form-author" value="Suman Saurabh" class="w-full px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none font-bold">
                    </div>
                </div>
                
                <button onclick="submitQuote()" id="submit-btn" class="w-full bg-indigo-600 text-white py-4 md:py-5 rounded-[2rem] font-black text-lg md:text-xl shadow-xl shadow-indigo-100 active:scale-95 transition-all mt-4">
                    Confirm Publication
                </button>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-6 md:bottom-10 left-1/2 z-[200] glass px-6 md:px-8 py-3 md:py-4 rounded-2xl shadow-2xl border-l-4 border-l-indigo-600 flex items-center gap-3">
        <i data-lucide="check" class="w-5 h-5 text-indigo-600"></i>
        <span id="toast-msg" class="font-bold text-slate-800 text-sm">Action Success</span>
    </div>

    <footer class="py-12 md:py-20 text-center border-t border-slate-100 bg-white px-6">
        <p class="text-[10px] font-black uppercase tracking-[0.4em] text-slate-400">Curated by Suman Saurabh</p>
    </footer>

    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, doc, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-auth.js";

        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'suman-saurabh-final-upload';

        let user = null, isAdmin = false, quotes = [], currentFilter = 'All', base64Image = "";

        window.showToast = (msg) => {
            const t = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            t.classList.add('active');
            setTimeout(() => t.classList.remove('active'), 3000);
        };

        window.closeModals = () => {
            document.querySelectorAll('.modal-backdrop').forEach(m => m.classList.remove('active'));
            base64Image = "";
            document.getElementById('upload-preview-container').classList.add('hidden');
        };

        window.openLoginModal = () => document.getElementById('modal-login').classList.add('active');
        
        window.openQuoteModal = (q = null) => {
            const m = document.getElementById('modal-quote');
            if (q) {
                document.getElementById('edit-id').value = q.id;
                document.getElementById('form-text').value = q.text;
                document.getElementById('form-cat').value = q.category;
                document.getElementById('form-author').value = q.author;
                base64Image = q.imageUrl || "";
                if (base64Image) {
                    document.getElementById('upload-preview').src = base64Image;
                    document.getElementById('upload-preview-container').classList.remove('hidden');
                }
                document.getElementById('quote-modal-title').innerText = "Edit Post";
            } else {
                document.getElementById('edit-id').value = "";
                document.getElementById('form-text').value = "";
                document.getElementById('form-cat').value = "Wisdom";
                document.getElementById('form-author').value = "Suman Saurabh";
                base64Image = "";
                document.getElementById('upload-preview-container').classList.add('hidden');
                document.getElementById('quote-modal-title').innerText = "New Post";
            }
            m.classList.add('active');
        };

        window.previewFile = (input) => {
            const file = input.files[0];
            if (file) {
                if (file.size > 1024 * 1024) { // 1MB Limit
                    alert("File bahut badi hai! Kripya 1MB se chhoti photo select karein.");
                    input.value = "";
                    return;
                }
                const reader = new FileReader();
                reader.onload = (e) => {
                    base64Image = e.target.result;
                    document.getElementById('upload-preview').src = base64Image;
                    document.getElementById('upload-preview-container').classList.remove('hidden');
                };
                reader.readAsDataURL(file);
            }
        };

        window.verifyPass = () => {
            if (document.getElementById('pass-input').value === 'delhi') {
                isAdmin = true;
                document.body.classList.add('is-admin');
                document.getElementById('lock-btn').classList.add('hidden');
                showToast("Admin Mode Active");
                closeModals();
            } else {
                alert("Incorrect Key!");
            }
        };

        window.logoutAdmin = () => {
            isAdmin = false;
            document.body.classList.remove('is-admin');
            document.getElementById('lock-btn').classList.remove('hidden');
            showToast("Logged Out");
        };

        window.copyQuote = (txt) => {
            const area = document.createElement('textarea');
            area.value = txt; document.body.appendChild(area);
            area.select(); document.execCommand('copy');
            document.body.removeChild(area);
            showToast("Copied!");
        };

        window.setCategory = (c) => {
            currentFilter = c;
            document.querySelectorAll('.category-pill').forEach(b => b.classList.toggle('active', b.innerText.includes(c)));
            renderQuotes();
        };

        window.filterQuotes = () => renderQuotes();

        const quotesRef = collection(db, 'artifacts', appId, 'public', 'data', 'quotes');

        window.submitQuote = async () => {
            if (!user) return;
            const text = document.getElementById('form-text').value;
            const id = document.getElementById('edit-id').value;
            const btn = document.getElementById('submit-btn');
            if (!text.trim()) return;

            btn.disabled = true;
            btn.innerText = "Processing...";

            const data = {
                text,
                imageUrl: base64Image,
                author: document.getElementById('form-author').value,
                category: document.getElementById('form-cat').value,
                updatedAt: Date.now()
            };

            try {
                if (id) {
                    await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'quotes', id), data);
                    showToast("Updated!");
                } else {
                    await addDoc(quotesRef, { ...data, createdAt: Date.now() });
                    showToast("Published!");
                }
                closeModals();
            } catch (e) { 
                console.error(e);
                alert("Galti hui! Photo shayad bahut badi hai.");
            } finally {
                btn.disabled = false;
                btn.innerText = id ? "Update" : "Confirm Publication";
            }
        };

        window.deleteQuote = async (id) => {
            if (confirm("Delete this?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'quotes', id));
                showToast("Deleted");
            }
        };

        async function run() {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (e) { console.error(e); }

            onAuthStateChanged(auth, (u) => {
                user = u;
                if (u) {
                    onSnapshot(quotesRef, (snap) => {
                        quotes = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                        renderQuotes();
                    }, (err) => console.error(err));
                }
            });
        }

        function renderQuotes() {
            const search = document.getElementById('search-input').value.toLowerCase();
            const grid = document.getElementById('grid');
            const loader = document.getElementById('loader');
            const empty = document.getElementById('empty-state');
            
            loader.classList.add('hidden');
            grid.innerHTML = "";

            const filtered = quotes
                .filter(q => (currentFilter === 'All' || q.category === currentFilter))
                .filter(q => (q.text.toLowerCase().includes(search) || q.author.toLowerCase().includes(search)))
                .sort((a,b) => (b.createdAt || 0) - (a.createdAt || 0));

            if (filtered.length === 0) {
                grid.classList.add('hidden');
                empty.classList.remove('hidden');
            } else {
                grid.classList.remove('hidden');
                empty.classList.add('hidden');
            }

            filtered.forEach((q, i) => {
                const card = document.createElement('div');
                card.className = "glass p-6 md:p-8 rounded-[2.5rem] hover:shadow-2xl transition-all quote-card relative overflow-hidden group border border-slate-100 flex flex-col justify-between";
                card.style.animationDelay = `${i * 0.05}s`;
                
                let imgPart = q.imageUrl ? `<img src="${q.imageUrl}" class="post-image" onerror="this.style.display='none'">` : '';

                card.innerHTML = `
                    <div>
                        <div class="flex justify-between items-center mb-6">
                            <span class="text-[9px] font-black tracking-widest text-indigo-500 bg-white/50 px-4 py-1.5 rounded-full border border-indigo-50">${q.category || 'Vision'}</span>
                            <i data-lucide="quote" class="w-5 h-5 text-indigo-100"></i>
                        </div>
                        ${imgPart}
                        <p class="text-lg font-bold text-slate-800 leading-relaxed mb-8 italic">"${q.text}"</p>
                    </div>
                    <div class="flex items-center justify-between mt-auto pt-4 border-t border-slate-50">
                        <span class="text-[10px] font-black text-slate-400 uppercase tracking-widest">${q.author}</span>
                        <div class="flex items-center gap-1">
                            <button onclick="copyQuote(\`${q.text}\`)" class="p-2 text-slate-300 hover:text-indigo-600 transition-all"><i data-lucide="copy" class="w-4 h-4"></i></button>
                            <div class="admin-controls gap-1">
                                <button class="edit-btn p-2 text-blue-400 hover:bg-blue-50 rounded-lg"><i data-lucide="edit-3" class="w-4 h-4"></i></button>
                                <button class="delete-btn p-2 text-red-400 hover:bg-red-50 rounded-lg"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                            </div>
                        </div>
                    </div>
                `;
                
                card.querySelector('.edit-btn')?.addEventListener('click', () => openQuoteModal(q));
                card.querySelector('.delete-btn')?.addEventListener('click', () => deleteQuote(q.id));
                grid.appendChild(card);
            });
            lucide.createIcons();
        }

        run();
        lucide.createIcons();
    </script>
</body>
</html>
