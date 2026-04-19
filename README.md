```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Elite Digital Shop | Mobile Money Payment</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');
        
        :root {
            --gold: #f59e0b;
            --dark-bg: #050505;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: var(--dark-bg);
            color: white;
            overflow-x: hidden;
        }

        .gold-gradient {
            background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
        }

        .card-glass {
            background: rgba(13, 13, 13, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.4s ease;
        }

        .card-glass:hover {
            border-color: var(--gold);
            transform: translateY(-5px);
        }

        .urgency-badge {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.05); opacity: 0.8; }
            100% { transform: scale(1); opacity: 1; }
        }

        .hidden { display: none; }
        
        input, textarea {
            background: #000 !important;
            border: 1px solid rgba(255, 255, 255, 0.1) !important;
            color: white !important;
        }

        input:focus { border-color: var(--gold) !important; outline: none; }
    </style>
</head>
<body>

    <!-- Barre de Navigation -->
    <nav class="fixed top-0 w-full z-50 bg-black/80 backdrop-blur-md border-b border-white/5">
        <div class="max-w-7xl mx-auto px-6 h-20 flex items-center justify-between">
            <div class="flex items-center gap-2">
                <i class="fas fa-gem text-amber-500 text-2xl"></i>
                <span class="font-extrabold text-xl tracking-tighter italic uppercase">ZENOVA<span class="text-amber-500">SHOP</span></span>
            </div>
            <div class="flex gap-4">
                <button onclick="switchPage('shop')" id="nav-shop" class="text-amber-500 font-bold text-xs uppercase tracking-widest border-b-2 border-amber-500 pb-1">Boutique</button>
                <button onclick="switchPage('admin')" id="nav-admin" class="text-gray-400 hover:text-white font-bold text-xs uppercase tracking-widest pb-1">Gestionnaire</button>
            </div>
        </div>
    </nav>

    <!-- Page Boutique -->
    <section id="page-shop" class="pt-32 pb-20 px-6 max-w-7xl mx-auto">
        <div class="text-center mb-16">
            <div class="urgency-badge inline-block bg-red-600/10 border border-red-600/20 text-red-500 px-4 py-2 rounded-full text-[10px] font-black uppercase tracking-[0.2em] mb-6">
                🔥 OFFRE LIMITÉE : Les prix augmentent dans <span id="countdown">05:00:00</span>
            </div>
            <h1 class="text-5xl md:text-7xl font-black mb-6 uppercase italic tracking-tighter">
                Accélérez votre <span class="text-amber-500">Liberté</span>
            </h1>
            <p class="text-gray-400 max-w-2xl mx-auto">Téléchargement immédiat de vos guides PDF après paiement Mobile Money sécurisé.</p>
        </div>

        <div id="grid-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <!-- Injecté dynamiquement -->
        </div>
    </section>

    <!-- Page Admin -->
    <section id="page-admin" class="pt-32 pb-20 px-6 max-w-3xl mx-auto hidden">
        <div class="card-glass p-8 md:p-12 rounded-[40px]">
            <h2 class="text-3xl font-black mb-8 italic uppercase tracking-tighter text-amber-500">Ajouter un produit</h2>
            <form id="form-product" class="space-y-6">
                <input type="text" id="in-title" placeholder="Titre du livre PDF" class="w-full p-4 rounded-2xl" required>
                <input type="text" id="in-price" placeholder="Prix (ex: 15.000 FCFA)" class="w-full p-4 rounded-2xl" required>
                <textarea id="in-desc" placeholder="Description du produit..." class="w-full p-4 rounded-2xl h-24" required></textarea>
                <input type="text" id="in-img" placeholder="URL Image Couverture (Unsplash)" class="w-full p-4 rounded-2xl">
                <input type="text" id="in-wa" placeholder="Lien WhatsApp (https://wa.me/votre-numero)" class="w-full p-4 rounded-2xl" required>
                <button type="submit" class="w-full gold-gradient text-black font-black py-5 rounded-2xl uppercase tracking-widest hover:scale-105 transition-all">Publier Immédiatement</button>
            </form>
            
            <div class="mt-12 pt-8 border-t border-white/10">
                <h3 class="text-xs font-bold text-gray-500 uppercase tracking-widest mb-4">Gestion du catalogue</h3>
                <div id="admin-list" class="space-y-2"></div>
            </div>
        </div>
    </section>

    <!-- Modal Paiement -->
    <div id="modal-payment" class="fixed inset-0 z-[100] bg-black/95 backdrop-blur-md hidden flex items-center justify-center p-6">
        <div class="bg-[#0d0d0d] border border-amber-500/30 p-8 rounded-[40px] max-w-md w-full">
            <h2 class="text-2xl font-black mb-6 text-center uppercase italic">Paiement Mobile Money</h2>
            <div class="space-y-3">
                <button onclick="confirmPay('Orange Money')" class="w-full p-4 rounded-2xl bg-[#FF6600]/10 border border-[#FF6600]/20 flex items-center justify-between font-bold hover:bg-[#FF6600]/20 transition-all">
                    <span>Orange Money</span><i class="fas fa-chevron-right"></i>
                </button>
                <button onclick="confirmPay('MTN MoMo')" class="w-full p-4 rounded-2xl bg-[#FFCC00]/10 border border-[#FFCC00]/20 flex items-center justify-between font-bold hover:bg-[#FFCC00]/20 transition-all">
                    <span class="text-white">MTN MoMo</span><i class="fas fa-chevron-right"></i>
                </button>
                <button onclick="confirmPay('Wave')" class="w-full p-4 rounded-2xl bg-[#1E90FF]/10 border border-[#1E90FF]/20 flex items-center justify-between font-bold hover:bg-[#1E90FF]/20 transition-all">
                    <span>Wave</span><i class="fas fa-chevron-right"></i>
                </button>
                <button onclick="confirmPay('Moov Money')" class="w-full p-4 rounded-2xl bg-[#004A99]/10 border border-[#004A99]/20 flex items-center justify-between font-bold hover:bg-[#004A99]/20 transition-all">
                    <span>Moov Money</span><i class="fas fa-chevron-right"></i>
                </button>
            </div>
            <button onclick="closeModal()" class="mt-8 w-full text-gray-500 font-bold uppercase text-xs tracking-widest">Annuler</button>
        </div>
    </div>

    <script>
        let db = [
            { id: 1, title: "L'EMPIRE DU DIGITAL", desc: "Le secret des 1% pour bâtir un business automatisé en Afrique.", price: "25.000 FCFA", img: "https://images.unsplash.com/photo-1544947950-fa07a98d237f?w=500", wa: "https://wa.me/225XXXXXXXX" },
            { id: 2, title: "STRATÉGIE WAVE & ADS", desc: "Utilisez la puissance des réseaux sociaux pour vos ventes locales.", price: "15.000 FCFA", img: "https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=500", wa: "https://wa.me/225XXXXXXXX" }
        ];

        let selected = null;

        function switchPage(page) {
            document.getElementById('page-shop').classList.toggle('hidden', page !== 'shop');
            document.getElementById('page-admin').classList.toggle('hidden', page !== 'admin');
            
            document.getElementById('nav-shop').className = page === 'shop' ? 'text-amber-500 font-bold text-xs uppercase tracking-widest border-b-2 border-amber-500 pb-1' : 'text-gray-400 hover:text-white font-bold text-xs uppercase tracking-widest pb-1';
            document.getElementById('nav-admin').className = page === 'admin' ? 'text-amber-500 font-bold text-xs uppercase tracking-widest border-b-2 border-amber-500 pb-1' : 'text-gray-400 hover:text-white font-bold text-xs uppercase tracking-widest pb-1';
            
            render();
        }

        function render() {
            const grid = document.getElementById('grid-container');
            const list = document.getElementById('admin-list');
            grid.innerHTML = '';
            list.innerHTML = '';

            db.forEach(item => {
                grid.innerHTML += `
                    <div class="card-glass rounded-[35px] overflow-hidden flex flex-col">
                        <div class="h-64 relative overflow-hidden">
                            <img src="${item.img}" class="w-full h-full object-cover">
                            <div class="absolute top-4 left-4 bg-amber-500 text-black px-3 py-1 rounded-full text-[10px] font-black uppercase italic">Best Seller</div>
                        </div>
                        <div class="p-8 flex flex-col flex-grow">
                            <h3 class="text-2xl font-black italic mb-3 uppercase tracking-tighter">${item.title}</h3>
                            <p class="text-gray-500 text-xs mb-8 leading-relaxed">${item.desc}</p>
                            <div class="mt-auto pt-6 border-t border-white/5 flex items-center justify-between">
                                <div><p class="text-[9px] text-amber-500 font-bold uppercase tracking-widest">Prix</p><p class="text-xl font-black italic">${item.price}</p></div>
                                <button onclick="openPay(${item.id})" class="gold-gradient text-black h-12 w-12 rounded-2xl flex items-center justify-center shadow-lg hover:rotate-6 transition-all"><i class="fas fa-shopping-bag"></i></button>
                            </div>
                        </div>
                    </div>
                `;
                list.innerHTML += `
                    <div class="flex justify-between items-center bg-black/50 p-3 rounded-xl border border-white/5">
                        <span class="text-[11px] font-bold uppercase italic">${item.title}</span>
                        <button onclick="remove(${item.id})" class="text-red-500 hover:text-red-400 p-2"><i class="fas fa-trash"></i></button>
                    </div>
                `;
            });
        }

        function openPay(id) {
            selected = db.find(x => x.id === id);
            document.getElementById('modal-payment').classList.remove('hidden');
        }

        function closeModal() { document.getElementById('modal-payment').classList.add('hidden'); }

        function confirmPay(method) {
            const text = `Bonjour ! Je souhaite commander "${selected.title}" (${selected.price}) via ${method}. Merci de m'envoyer le numéro de paiement.`;
            window.open(`${selected.wa}?text=${encodeURIComponent(text)}`, '_blank');
            closeModal();
        }

        document.getElementById('form-product').onsubmit = function(e) {
            e.preventDefault();
            db.unshift({
                id: Date.now(),
                title: document.getElementById('in-title').value,
                price: document.getElementById('in-price').value,
                desc: document.getElementById('in-desc').value,
                img: document.getElementById('in-img').value || 'https://images.unsplash.com/photo-1589998059171-988d887df646?w=500',
                wa: document.getElementById('in-wa').value
            });
            this.reset();
            switchPage('shop');
        };

        function remove(id) { db = db.filter(x => x.id !== id); render(); }

        // Compteur factice
        function startTimer() {
            let h=4, m=59, s=59;
            setInterval(() => {
                s--; if(s<0){s=59;m--;} if(m<0){m=59;h--;}
                document.getElementById('countdown').innerText = `${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
            }, 1000);
        }

        render();
        startTimer();
    </script>
</body>
</html>

```
