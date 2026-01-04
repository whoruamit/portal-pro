<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Suman Saurabh | Premium Quote Gallery</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #f8fafc;
        }

        /* Premium Animations */
        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-15px) rotate(2deg); }
        }

        @keyframes blob {
            0% { transform: translate(0px, 0px) scale(1); }
            33% { transform: translate(30px, -50px) scale(1.1); }
            66% { transform: translate(-20px, 20px) scale(0.9); }
            100% { transform: translate(0px, 0px) scale(1); }
        }

        @keyframes cardReveal {
            from { opacity: 0; transform: translateY(30px) scale(0.95); }
            to { opacity: 1; transform: translateY(0) scale(1); }
        }

        .animate-blob { animation: blob 7s infinite alternate; }
        .delay-2000 { animation-delay: 2s; }
        .delay-4000 { animation-delay: 4s; }

        .glass {
            background: rgba(255, 255, 255, 0.75);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.4);
        }

        .quote-card {
            opacity: 0;
            animation: cardReveal 0.6s cubic-bezier(0.23, 1, 0.32, 1) forwards;
        }

        .category-pill.active {
            background-color: #4f46e5;
            color: white;
            box-shadow: 0 10px 15px -3px rgba(79, 70, 229, 0.3);
        }

        /* Modal Background */
        .modal-backdrop {
            display: none;
            background: rgba(15, 23, 42, 0.4);
            backdrop-filter: blur(8px);
        }
        .modal-backdrop.active { display: flex; }

        .admin-controls { display: none; }
        .is-admin .admin-controls { display: flex; }
        .is-admin .admin-grid-btn { display: inline-flex; }

        /* Notification Toast */
        #toast {
            transform: translateY(100px);
            transition: transform 0.3s ease;
        }
        #toast.active { transform: translateY(0); }
    </style>
</head>
<body class="overflow-x-hidden selection:bg-indigo-100 selection:text-indigo-900">

    <!-- Background Decoration -->
    <div class="fixed inset-0 -z-10 overflow-hidden pointer-events-none">
        <div class="absolute top-[-10%] left-[-10%] w-[500px] h-[500px] bg-indigo-200/30 rounded-full mix-blend-multiply filter blur-[100px] animate-blob"></div>
        <div class="absolute top-[20%] right-[-5%] w-[400px] h-[400px] bg-purple-200/30 rounded-full mix-blend-multiply filter blur-[100px] animate-blob delay-2000"></div>
        <div class="absolute bottom-[-10%] left-[20%] w-[600px] h-[600px] bg-blue-200/30 rounded-full mix-blend-multiply filter blur-[100px] animate-blob delay-4000"></div>
    </div>

    <!-- Navigation -->
    <nav class="sticky top-0 z-40 glass border-b border-slate-200/60">
        <div class="max-w-7xl mx-auto px-6 h-20 flex items-center justify-between">
            <div class="flex items-center gap-4 group cursor-default">
                <div class="w-12 h-12 bg-gradient-to-br from-indigo-600 to-violet-600 rounded-2xl flex items-center justify-center text-white shadow-xl shadow-indigo-200 group-hover:rotate-12 transition-transform duration-500">
                    <i data-lucide="sparkles" class="w-6 h-6"></i>
                </div>
                <div class="flex flex-col">
                    <span class="text-2xl font-black tracking-tighter text-slate-900 leading-none">SUMAN SAURABH</span>
                    <span class="text-[10px] font-bold text-indigo-500 uppercase tracking-[0.4em] mt-1">Thought Gallery</span>
                </div>
            </div>

            <div class="flex items-center gap-4">
                <div id="admin-nav" class="admin-controls items-center gap-4">
                    <button onclick="openQuoteModal()" class="hidden md:flex bg-slate-900 text-white px-6 py-2.5 rounded-2xl font-bold items-center gap-2 hover:bg-indigo-600 transition-all hover:shadow-2xl active:scale-95">
                        <i data-lucide="plus-circle" class="w-5 h-5"></i> <span>Add New Post</span>
                    </button>
                    <button onclick="logoutAdmin()" class="p-3 text-slate-400 hover:text-red-500 hover:bg-red-50 rounded-2xl transition-all" title="Logout">
                        <i data-lucide="power" class="w-6 h-6"></i>
                    </button>
                </div>
                <button id="lock-btn" onclick="openLoginModal()" class="w-12 h-12 rounded-2xl flex items-center justify-center text-slate-400 hover:text-indigo-600 hover:bg-indigo-50 transition-all border border-transparent hover:border-indigo-100">
                    <i data-lucide="lock" class="w-6 h-6"></i>
                </button>
            </div>
        </div>
    </nav>

    <!-- Header & Search -->
    <section class="max-w-7xl mx-auto px-6 pt-16 pb-12 text-center">
        <h2 class="text-6xl md:text-8xl font-black text-slate-900 tracking-tightest mb-8 leading-tight">
            Design Your <span class="bg-clip-text text-transparent bg-gradient-to-r from-indigo-600 via-violet-600 to-purple-600">Reality.</span>
        </h2>
        
        <div class="max-w-2xl mx-auto glass p-2 rounded-[2.5rem] shadow-2xl shadow-indigo-100/50 mb-12 flex items-center">
            <div class="pl-6 text-slate-400">
                <i data-lucide="search" class="w-6 h-6"></i>
            </div>
            <input type="text" id="search-input" onkeyup="filterQuotes()" placeholder="Duniya ko badalne wala koi vichar dhundhein..." class="w-full px-4 py-4 bg-transparent outline-none font-medium text-lg text-slate-700">
        </div>

        <div id="category-filters" class="flex flex-wrap justify-center gap-3 mb-8">
            <button onclick="setCategory('All')" class="category-pill active px-6 py-2.5 rounded-full font-bold text-sm transition-all border border-slate-200/60 hover:border-indigo-300">All Stories</button>
            <button onclick="setCategory('Wisdom')" class="category-pill px-6 py-2.5 rounded-full font-bold text-sm transition-all border border-slate-200/60 hover:border-indigo-300">Wisdom</button>
            <button onclick="setCategory('Success')" class="category-pill px-6 py-2.5 rounded-full font-bold text-sm transition-all border border-slate-200/60 hover:border-indigo-300">Success</button>
            <button onclick="setCategory('Motivation')" class="category-pill px-6 py-2.5 rounded-full font-bold text-sm transition-all border border-slate-200/60 hover:border-indigo-300">Motivation</button>
            <button onclick="setCategory('Life')" class="category-pill px-6 py-2.5 rounded-full font-bold text-sm transition-all border border-slate-200/60 hover:border-indigo-300">Life</button>
        </div>
    </section>

    <!-- Content Grid -->
    <main class="max-w-7xl mx-auto px-6 mb-32">
        <div id="loader" class="flex flex-col items-center justify-center py-32 opacity-50">
            <div class="w-16 h-16 border-4 border-indigo-100 border-t-indigo-600 rounded-full animate-spin mb-4"></div>
            <p class="font-bold text-indigo-900/40 tracking-widest uppercase text-xs">Loading Insights...</p>
        </div>

        <div id="grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 hidden">
            <!-- Cards go here -->
        </div>

        <div id="empty-state" class="hidden text-center py-32 glass rounded-[3rem]">
            <i data-lucide="search-x" class="w-20 h-20 mx-auto text-slate-300 mb-6"></i>
            <h3 class="text-2xl font-bold text-slate-800">Kuch nahi mila</h3>
            <p class="text-slate-400">Apne search keywords badal kar dekhein.</p>
        </div>
    </main>

    <!-- Floating Add Button for Mobile -->
    <button onclick="openQuoteModal()" class="admin-controls fixed bottom-8 right-8 w-16 h-16 bg-indigo-600 text-white rounded-2xl shadow-2xl shadow-indigo-300 z-50 flex items-center justify-center hover:scale-110 active:scale-90 transition-all md:hidden">
        <i data-lucide="plus" class="w-8 h-8"></i>
    </button>

    <!-- Login Modal -->
    <div id="modal-login" class="modal-backdrop fixed inset-0 z-[100] items-center justify-center p-6">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-3xl animate-[float_0.5s_ease-out]">
            <div class="flex justify-between items-center mb-8">
                <h3 class="text-3xl font-black tracking-tight">Admin Key</h3>
                <button onclick="closeModals()" class="w-10 h-10 flex items-center justify-center rounded-xl hover:bg-slate-50 text-slate-400">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>
            <div class="space-y-6">
                <div class="relative">
                    <input type="password" id="pass-input" placeholder="Security Password..." class="w-full px-6 py-5 bg-slate-50 border-2 border-slate-100 rounded-3xl focus:border-indigo-500 focus:bg-white outline-none transition-all font-bold text-lg">
                </div>
                <button onclick="verifyPass()" class="w-full bg-gradient-to-r from-indigo-600 to-violet-600 text-white py-5 rounded-3xl font-black text-xl shadow-xl shadow-indigo-200 hover:scale-[1.02] active:scale-95 transition-all">
                    Unlock Gallery
                </button>
            </div>
        </div>
    </div>

    <!-- Quote Form Modal -->
    <div id="modal-quote" class="modal-backdrop fixed inset-0 z-[100] items-center justify-center p-6">
        <div class="bg-white w-full max-w-2xl rounded-[3.5rem] overflow-hidden shadow-3xl">
            <div class="bg-slate-50 p-8 border-b border-slate-200/50 flex justify-between items-center">
                <h3 id="quote-modal-title" class="text-2xl font-black text-slate-800">Post New Thought</h3>
                <button onclick="closeModals()" class="w-10 h-10 flex items-center justify-center rounded-xl hover:bg-white text-slate-400 border border-transparent hover:border-slate-200">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>
            <div class="p-10 space-y-8">
                <input type="hidden" id="edit-id">
                <div class="space-y-2">
                    <label class="text-[10px] font-black uppercase tracking-widest text-slate-400 ml-2">Quote Content</label>
                    <textarea id="form-text" required placeholder="Duniya ko kya batana chahte hain?" class="w-full h-48 px-8 py-6 bg-slate-50 border-2 border-slate-100 rounded-[2.5rem] focus:border-indigo-500 focus:bg-white outline-none font-medium text-xl resize-none transition-all"></textarea>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="space-y-2">
                        <label class="text-[10px] font-black uppercase tracking-widest text-slate-400 ml-2">Gallery Category</label>
                        <select id="form-cat" class="w-full px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none focus:border-indigo-500 font-bold appearance-none cursor-pointer">
                            <option>Wisdom</option>
                            <option>Life</option>
                            <option>Motivation</option>
                            <option>Success</option>
                        </select>
                    </div>
                    <div class="space-y-2">
                        <label class="text-[10px] font-black uppercase tracking-widest text-slate-400 ml-2">Signature</label>
                        <input type="text" id="form-author" value="Suman Saurabh" class="w-full px-6 py-4 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none focus:border-indigo-500 font-bold">
                    </div>
                </div>
                
                <button onclick="submitQuote()" class="w-full bg-slate-900 text-white py-6 rounded-[2rem] font-black text-xl shadow-2xl shadow-slate-200 hover:bg-indigo-600 hover:scale-[1.01] transition-all">
                    Confirm Post
                </button>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-8 left-1/2 -translate-x-1/2 glass px-8 py-4 rounded-2xl shadow-2xl z-[200] border-l-4 border-l-indigo-600 flex items-center gap-3">
        <i data-lucide="check-circle" class="w-5 h-5 text-indigo-600"></i>
        <span id="toast-msg" class="font-bold text-slate-700">Action Successful</span>
    </div>

    <!-- Footer -->
    <footer class="py-20 text-center opacity-40">
        <div class="flex flex-col items-center gap-4">
            <div class="flex items-center gap-2">
                <div class="w-2 h-2 bg-indigo-600 rounded-full animate-ping"></div>
                <span class="text-[10px] font-black uppercase tracking-[0.6em] text-slate-600">Curated by Suman Saurabh</span>
            </div>
            <p class="text-xs font-bold">&copy; 2024 Gallery Vault. No Limits.</p>
        </div>
    </footer>

    <!-- Firebase -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, doc, updateDoc, deleteDoc, query } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-auth.js";

        // State
        let db, auth, user, currentQuotes = [], activeCategory = 'All';
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'suman-saurabh-premium-v2';
        const firebaseConfig = JSON.parse(__firebase_config);

        // Init
        const app = initializeApp(firebaseConfig);
        db = getFirestore(app);
        auth = getAuth(app);

        // UI Helpers
        window.showToast = (msg) => {
            const toast = document.getElementById('toast');
            document.getElementById('toast-msg').innerText = msg;
            toast.classList.add('active');
            setTimeout(() => toast.classList.remove('active'), 3000);
        };

        window.closeModals = () => {
            document.querySelectorAll('.modal-backdrop').forEach(m => m.classList.remove('active'));
        };

        window.openLoginModal = () => document.getElementById('modal-login').classList.add('active');
        
        window.openQuoteModal = (quote = null) => {
            const modal = document.getElementById('modal-quote');
            const title = document.getElementById('quote-modal-title');
            
            if (quote) {
                title.innerText = "Refine Your Thought";
                document.getElementById('edit-id').value = quote.id;
                document.getElementById('form-text').value = quote.text;
                document.getElementById('form-cat').value = quote.category;
                document.getElementById('form-author').value = quote.author;
            } else {
                title.innerText = "New Vision";
                document.getElementById('edit-id').value = "";
                document.getElementById('form-text').value = "";
                document.getElementById('form-cat').value = "Wisdom";
                document.getElementById('form-author').value = "Suman Saurabh";
            }
            modal.classList.add('active');
        };

        window.verifyPass = () => {
            const p = document.getElementById('pass-input').value;
            if (p === 'delhi') {
                document.body.classList.add('is-admin');
                document.getElementById('lock-btn').classList.add('hidden');
                showToast("Admin Mode Activated");
                closeModals();
            } else {
                alert("Incorrect Access Key!");
            }
        };

        window.logoutAdmin = () => {
            document.body.classList.remove('is-admin');
            document.getElementById('lock-btn').classList.remove('hidden');
            showToast("Logged out from Admin");
        };

        window.copyQuote = (text) => {
            const el = document.createElement('textarea');
            el.value = text;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);
            showToast("Copied to clipboard!");
        };

        // Quote Filtering
        window.setCategory = (cat) => {
            activeCategory = cat;
            document.querySelectorAll('.category-pill').forEach(btn => {
                btn.classList.toggle('active', btn.innerText.includes(cat));
            });
            renderQuotes();
        };

        window.filterQuotes = () => renderQuotes();

        // Firestore Setup
        const quotesRef = collection(db, 'artifacts', appId, 'public', 'data', 'quotes');

        window.submitQuote = async () => {
            if (!user) return;
            const text = document.getElementById('form-text').value;
            if (!text.trim()) return;

            const id = document.getElementById('edit-id').value;
            const data = {
                text,
                author: document.getElementById('form-author').value || 'Suman Saurabh',
                category: document.getElementById('form-cat').value,
                updatedAt: Date.now()
            };

            try {
                if (id) {
                    await updateDoc(doc(db, 'artifacts', appId, 'public', 'data', 'quotes', id), data);
                    showToast("Insight Updated");
                } else {
                    await addDoc(quotesRef, { ...data, createdAt: Date.now() });
                    showToast("Insight Shared");
                }
                closeModals();
            } catch (e) { console.error(e); }
        };

        window.removeQuote = async (id) => {
            if (!user) return;
            if (confirm("Permanently delete this insight?")) {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'quotes', id));
                showToast("Post Deleted");
            }
        };

        // Rule 3: Authentication FIRST, then setup listeners
        async function run() {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (e) { 
                console.error("Auth initialization failed:", e); 
            }

            onAuthStateChanged(auth, (u) => {
                user = u;
                if (u) {
                    // Set up listener ONLY after confirmed auth
                    onSnapshot(quotesRef, (snap) => {
                        currentQuotes = snap.docs.map(d => ({ id: d.id, ...d.data() }));
                        renderQuotes();
                    }, (err) => {
                        console.error("Firestore listener permission error:", err);
                    });
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

            const filtered = currentQuotes
                .filter(q => (activeCategory === 'All' || q.category === activeCategory))
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
                card.className = "glass group p-10 rounded-[3rem] hover:shadow-3xl hover:shadow-indigo-200/40 transition-all duration-700 flex flex-col justify-between border-transparent hover:border-indigo-100 quote-card relative overflow-hidden";
                card.style.animationDelay = `${i * 0.05}s`;
                
                card.innerHTML = `
                    <div class="absolute top-0 right-0 w-32 h-32 bg-indigo-50/50 rounded-full -translate-y-16 translate-x-16 group-hover:scale-150 transition-transform duration-1000"></div>
                    
                    <div class="relative">
                        <div class="flex justify-between items-center mb-10">
                            <span class="text-[10px] font-black tracking-widest text-indigo-500 bg-white/80 px-5 py-2 rounded-full border border-indigo-50 shadow-sm">
                                ${q.category || 'Vision'}
                            </span>
                            <div class="p-3 bg-indigo-50/50 rounded-2xl text-indigo-600 opacity-20 group-hover:opacity-100 transition-opacity">
                                <i data-lucide="quote" class="w-6 h-6"></i>
                            </div>
                        </div>
                        <p class="text-2xl font-bold text-slate-800 leading-[1.6] mb-10 tracking-tight italic">
                            "${q.text}"
                        </p>
                    </div>
                    
                    <div class="relative flex items-center justify-between mt-auto">
                        <div class="flex items-center gap-4">
                            <div class="w-10 h-[2px] bg-indigo-600 rounded-full group-hover:w-16 transition-all duration-500"></div>
                            <span class="text-xs font-black text-slate-400 group-hover:text-indigo-600 transition-colors uppercase tracking-widest">
                                ${q.author}
                            </span>
                        </div>
                        
                        <div class="flex items-center gap-1">
                            <button onclick="copyQuote(\`${q.text}\`)" class="p-3 text-slate-300 hover:text-indigo-600 hover:bg-white rounded-2xl transition-all" title="Copy text">
                                <i data-lucide="copy" class="w-5 h-5"></i>
                            </button>
                            <div class="admin-controls gap-1">
                                <button class="edit-btn p-3 text-blue-400 hover:bg-blue-50 rounded-2xl transition-all"><i data-lucide="edit-3" class="w-5 h-5"></i></button>
                                <button class="delete-btn p-3 text-red-400 hover:bg-red-50 rounded-2xl transition-all"><i data-lucide="trash-2" class="w-5 h-5"></i></button>
                            </div>
                        </div>
                    </div>
                `;
                
                // Add event listeners for admin buttons
                card.querySelector('.edit-btn')?.addEventListener('click', () => openQuoteModal(q));
                card.querySelector('.delete-btn')?.addEventListener('click', () => removeQuote(q.id));
                
                grid.appendChild(card);
            });
            lucide.createIcons();
        }

        run();
        lucide.createIcons();
    </script>
</body>
</html>
