```html
<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Drix - Tienda de Moda y Tecnología</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        drix: {
                            50: '#f5f3ff',
                            100: '#ede9fe',
                            200: '#ddd6fe',
                            300: '#c4b5fd',
                            400: '#a78bfa',
                            500: '#8b5cf6',
                            600: '#7c3aed',
                            700: '#6d28d9',
                            800: '#5b21b6',
                            900: '#4c1d95',
                            950: '#2e1065',
                        }
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #c4b5fd; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #8b5cf6; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 antialiased min-h-screen flex flex-col selection:bg-drix-500 selection:text-white">

    <!-- Notification Toast Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <!-- Top Navigation Bar -->
    <header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-drix-100 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <!-- Left: Menu Button & Logo -->
            <div class="flex items-center gap-3">
                <button id="menu-btn" class="p-2 rounded-xl text-drix-700 hover:bg-drix-50 transition-colors focus:outline-none focus:ring-2 focus:ring-drix-500" aria-label="Abrir menú">
                    <i data-lucide="menu" class="w-6 h-6"></i>
                </button>
                <a href="#" class="flex items-center gap-2">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-drix-600 to-drix-400 flex items-center justify-center text-white font-extrabold text-xl shadow-md shadow-drix-500/30">
                        D
                    </div>
                    <span class="text-2xl font-extrabold bg-gradient-to-r from-drix-700 to-drix-500 bg-clip-text text-transparent tracking-tight">DRIX</span>
                </a>
            </div>

            <!-- Center: Search Bar (Desktop) -->
            <div class="hidden md:flex flex-1 max-w-md mx-8">
                <div class="relative w-full">
                    <span class="absolute inset-y-0 left-0 flex items-center pl-3.5 pointer-events-none text-slate-400">
                        <i data-lucide="search" class="w-4 h-4"></i>
                    </span>
                    <input type="text" id="search-input" placeholder="Buscar ropa, tecnología, audífonos..." class="w-full pl-10 pr-4 py-2 bg-slate-100 border border-transparent rounded-full text-sm focus:bg-white focus:border-drix-400 focus:outline-none focus:ring-2 focus:ring-drix-500/20 transition-all">
                </div>
            </div>

            <!-- Right: Cart & User actions -->
            <div class="flex items-center gap-2">
                <!-- Rewards Points Badge -->
                <button onclick="openRewardsModal()" class="hidden sm:flex items-center gap-1.5 px-3 py-1.5 rounded-xl bg-drix-100 hover:bg-drix-200 text-drix-800 text-xs font-bold transition-all border border-drix-200">
                    <i data-lucide="award" class="w-4 h-4 text-drix-600"></i>
                    <span id="header-points-count">250</span> <span class="text-drix-600">Pts</span>
                </button>

                <button id="cart-btn" class="relative p-2.5 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-600 transition-colors focus:outline-none" aria-label="Ver carrito">
                    <i data-lucide="shopping-bag" class="w-6 h-6"></i>
                    <span id="cart-badge" class="absolute -top-1 -right-1 bg-drix-600 text-white text-xs font-bold w-5 h-5 rounded-full flex items-center justify-center shadow-md shadow-drix-600/40 transform scale-0 transition-transform duration-200">0</span>
                </button>
                <button id="profile-icon-btn" class="hidden sm:flex items-center gap-2 p-1.5 pl-2.5 rounded-xl hover:bg-drix-50 transition-colors text-slate-700">
                    <span class="text-sm font-medium">Mi Cuenta</span>
                    <div class="w-8 h-8 rounded-lg bg-drix-100 text-drix-700 flex items-center justify-center font-bold text-sm">
                        JD
                    </div>
                </button>
            </div>
        </div>
        <!-- Mobile Search Bar -->
        <div class="md:hidden px-4 pb-3 pt-1">
            <div class="relative w-full">
                <span class="absolute inset-y-0 left-0 flex items-center pl-3.5 pointer-events-none text-slate-400">
                    <i data-lucide="search" class="w-4 h-4"></i>
                </span>
                <input type="text" id="mobile-search-input" placeholder="Buscar en Drix..." class="w-full pl-10 pr-4 py-2 bg-slate-100 border border-transparent rounded-full text-sm focus:bg-white focus:border-drix-400 focus:outline-none focus:ring-2 focus:ring-drix-500/20 transition-all">
            </div>
        </div>
    </header>

    <!-- Slide-over Navigation Drawer (Hamburger Menu) -->
    <div id="drawer-backdrop" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden opacity-0 transition-opacity duration-300"></div>
    <aside id="nav-drawer" class="fixed top-0 left-0 bottom-0 w-80 max-w-[85vw] bg-white z-50 shadow-2xl transform -translate-x-full transition-transform duration-300 flex flex-col">
        <!-- Drawer Header -->
        <div class="p-5 border-b border-slate-100 flex items-center justify-between bg-gradient-to-r from-drix-900 to-drix-700 text-white">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-white/10 backdrop-blur-md flex items-center justify-center font-bold text-lg text-white border border-white/20">
                    JD
                </div>
                <div>
                    <h3 class="font-bold text-base leading-tight">Juan Developer</h3>
                    <p class="text-xs text-drix-200">juan@drixstore.com</p>
                </div>
            </div>
            <button id="drawer-close" class="p-2 rounded-lg hover:bg-white/10 text-white/80 hover:text-white transition-colors">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
        </div>

        <!-- Drawer Links -->
        <div class="flex-1 overflow-y-auto py-4 px-3 space-y-1">
            <a href="#" onclick="closeDrawer(); scrollToSection('home'); return false;" class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-700 font-medium transition-colors">
                <i data-lucide="home" class="w-5 h-5 text-drix-500"></i>
                <span>Inicio (Home)</span>
            </a>
            <a href="#" onclick="closeDrawer(); scrollToSection('productos'); return false;" class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-700 font-medium transition-colors">
                <i data-lucide="grid" class="w-5 h-5 text-drix-500"></i>
                <span>Catálogo de Productos</span>
            </a>
            <button onclick="closeDrawer(); openRewardsModal();" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-700 font-medium transition-colors text-left">
                <i data-lucide="award" class="w-5 h-5 text-drix-500"></i>
                <span>Puntos Drix (Fidelidad)</span>
            </button>
            <button onclick="closeDrawer(); openProfileModal();" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-700 font-medium transition-colors text-left">
                <i data-lucide="user" class="w-5 h-5 text-drix-500"></i>
                <span>Perfil</span>
            </button>
            <button onclick="closeDrawer(); openOrdersModal();" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-slate-700 hover:bg-drix-50 hover:text-drix-700 font-medium transition-colors text-left">
                <i data-lucide="clipboard-list" class="w-5 h-5 text-drix-500"></i>
                <span>Historial de Pedidos</span>
            </button>
            <div class="pt-4 my-2 border-t border-slate-100"></div>
            <a href="#" onclick="closeDrawer(); filterCategory('Ropa'); return false;" class="flex items-center gap-3 px-4 py-2.5 rounded-xl text-slate-600 hover:bg-drix-50 hover:text-drix-700 text-sm transition-colors">
                <i data-lucide="shirt" class="w-4 h-4 text-drix-400"></i>
                <span>Moda y Ropa</span>
            </a>
            <a href="#" onclick="closeDrawer(); filterCategory('Tecnologia'); return false;" class="flex items-center gap-3 px-4 py-2.5 rounded-xl text-slate-600 hover:bg-drix-50 hover:text-drix-700 text-sm transition-colors">
                <i data-lucide="laptop" class="w-4 h-4 text-drix-400"></i>
                <span>Tecnología y Gadgets</span>
            </a>
        </div>

        <!-- Drawer Footer -->
        <div class="p-4 border-t border-slate-100 bg-slate-50 text-center">
            <p class="text-xs text-slate-400">Drix E-commerce &bull; Versión 3.0</p>
        </div>
    </aside>

    <!-- Main Content Area -->
    <main id="home" class="flex-1">
        <!-- Hero Carousel Section -->
        <section class="relative bg-slate-900 overflow-hidden text-white">
            <div id="carousel" class="relative h-[420px] sm:h-[500px] lg:h-[550px] w-full">
                <!-- Slide 1: Moda -->
                <div class="carousel-slide absolute inset-0 opacity-100 transition-opacity duration-700 ease-in-out flex items-center">
                    <div class="absolute inset-0 bg-gradient-to-r from-drix-950/90 via-drix-900/60 to-transparent z-10"></div>
                    <img src="https://images.unsplash.com/photo-1490481651871-ab68de25d43d?auto=format&fit=crop&w=1600&q=80" alt="Moda Drix" class="absolute inset-0 w-full h-full object-cover object-center" onerror="this.src='https://placehold.co/1600x800/4c1d95/fff?text=Moda+Drix'">
                    <div class="relative z-20 max-w-7xl mx-auto px-6 sm:px-8 w-full">
                        <div class="max-w-xl space-y-4">
                            <span class="px-3.5 py-1.5 rounded-full bg-drix-500/30 border border-drix-400/30 text-drix-200 text-xs font-semibold tracking-wide uppercase backdrop-blur-md">Nueva Colección 2026</span>
                            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight leading-tight">Moda Urbana & <span class="text-drix-300">Estilo Único</span></h1>
                            <p class="text-slate-300 text-sm sm:text-base leading-relaxed">Descubre las últimas tendencias en chaquetas, hoodies y ropa casual diseñadas para destacar en cualquier lugar.</p>
                            <div class="pt-2 flex flex-wrap gap-4">
                                <button onclick="scrollToSection('productos'); filterCategory('Ropa');" class="px-6.5 py-3 rounded-xl bg-gradient-to-r from-drix-600 to-drix-500 hover:from-drix-500 hover:to-drix-400 text-white font-semibold text-sm shadow-lg shadow-drix-600/40 transition-all transform hover:-translate-y-0.5">
                                    Ver Colección Moda
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Slide 2: Tecnología -->
                <div class="carousel-slide absolute inset-0 opacity-0 transition-opacity duration-700 ease-in-out flex items-center pointer-events-none">
                    <div class="absolute inset-0 bg-gradient-to-r from-purple-950/90 via-drix-900/60 to-transparent z-10"></div>
                    <img src="https://images.unsplash.com/photo-1519389950473-47ba0277781c?auto=format&fit=crop&w=1600&q=80" alt="Tecnología Drix" class="absolute inset-0 w-full h-full object-cover object-center" onerror="this.src='https://placehold.co/1600x800/2e1065/fff?text=Tecnologia+Drix'">
                    <div class="relative z-20 max-w-7xl mx-auto px-6 sm:px-8 w-full">
                        <div class="max-w-xl space-y-4">
                            <span class="px-3.5 py-1.5 rounded-full bg-purple-500/30 border border-purple-400/30 text-purple-200 text-xs font-semibold tracking-wide uppercase backdrop-blur-md">Gadgets de Vanguardia</span>
                            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight leading-tight">Tecnología sin <span class="text-purple-300">Límites</span></h1>
                            <p class="text-slate-300 text-sm sm:text-base leading-relaxed">Potencia tu productividad y entretenimiento con audífonos inalámbricos, smartwatches y accesorios premium.</p>
                            <div class="pt-2 flex flex-wrap gap-4">
                                <button onclick="scrollToSection('productos'); filterCategory('Tecnologia');" class="px-6.5 py-3 rounded-xl bg-gradient-to-r from-purple-600 to-drix-500 hover:from-purple-500 hover:to-drix-400 text-white font-semibold text-sm shadow-lg shadow-purple-600/40 transition-all transform hover:-translate-y-0.5">
                                    Explorar Tecnología
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Slide 3: Ofertas -->
                <div class="carousel-slide absolute inset-0 opacity-0 transition-opacity duration-700 ease-in-out flex items-center pointer-events-none">
                    <div class="absolute inset-0 bg-gradient-to-r from-slate-950/90 via-drix-950/60 to-transparent z-10"></div>
                    <img src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?auto=format&fit=crop&w=1600&q=80" alt="Ofertas Drix" class="absolute inset-0 w-full h-full object-cover object-center" onerror="this.src='https://placehold.co/1600x800/5b21b6/fff?text=Ofertas+Drix'">
                    <div class="relative z-20 max-w-7xl mx-auto px-6 sm:px-8 w-full">
                        <div class="max-w-xl space-y-4">
                            <span class="px-3.5 py-1.5 rounded-full bg-indigo-500/30 border border-indigo-400/30 text-indigo-200 text-xs font-semibold tracking-wide uppercase backdrop-blur-md">Pago Móvil Disponible</span>
                            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight leading-tight">Envío Rápido & <span class="text-indigo-300">Pago Seguro</span></h1>
                            <p class="text-slate-300 text-sm sm:text-base leading-relaxed">Compra fácil desde cualquier lugar de Venezuela con validación instantánea de Pago Móvil.</p>
                            <div class="pt-2 flex flex-wrap gap-4">
                                <button onclick="scrollToSection('productos');" class="px-6.5 py-3 rounded-xl bg-gradient-to-r from-drix-600 to-indigo-600 hover:from-drix-500 hover:to-indigo-500 text-white font-semibold text-sm shadow-lg shadow-drix-600/40 transition-all transform hover:-translate-y-0.5">
                                    Ver Todo el Catálogo
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Carousel Controls -->
            <button id="prev-slide" class="absolute left-4 top-1/2 -translate-y-1/2 z-30 w-11 h-11 rounded-full bg-black/40 hover:bg-drix-600 text-white flex items-center justify-center backdrop-blur-md transition-colors focus:outline-none" aria-label="Anterior">
                <i data-lucide="chevron-left" class="w-6 h-6"></i>
            </button>
            <button id="next-slide" class="absolute right-4 top-1/2 -translate-y-1/2 z-30 w-11 h-11 rounded-full bg-black/40 hover:bg-drix-600 text-white flex items-center justify-center backdrop-blur-md transition-colors focus:outline-none" aria-label="Siguiente">
                <i data-lucide="chevron-right" class="w-6 h-6"></i>
            </button>

            <!-- Carousel Indicators -->
            <div class="absolute bottom-5 left-0 right-0 z-30 flex justify-center gap-2">
                <button onclick="currentSlide(0)" class="carousel-dot w-3 h-3 rounded-full bg-white/50 hover:bg-white transition-all focus:outline-none" aria-label="Slide 1"></button>
                <button onclick="currentSlide(1)" class="carousel-dot w-3 h-3 rounded-full bg-white/50 hover:bg-white transition-all focus:outline-none" aria-label="Slide 2"></button>
                <button onclick="currentSlide(2)" class="carousel-dot w-3 h-3 rounded-full bg-white/50 hover:bg-white transition-all focus:outline-none" aria-label="Slide 3"></button>
            </div>
        </section>

        <!-- Features Banner -->
        <section class="bg-white border-b border-slate-100 py-6">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="flex items-center gap-4 p-4 rounded-2xl bg-drix-50/50 border border-drix-100">
                    <div class="w-12 h-12 rounded-xl bg-drix-600 text-white flex items-center justify-center flex-shrink-0 shadow-md shadow-drix-600/30">
                        <i data-lucide="smartphone" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-900 text-sm">Pago Móvil Integrado</h4>
                        <p class="text-xs text-slate-500">Paga en Bs de forma fácil y segura.</p>
                    </div>
                </div>
                <div class="flex items-center gap-4 p-4 rounded-2xl bg-drix-50/50 border border-drix-100">
                    <div class="w-12 h-12 rounded-xl bg-drix-600 text-white flex items-center justify-center flex-shrink-0 shadow-md shadow-drix-600/30">
                        <i data-lucide="truck" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-900 text-sm">Envíos a Nivel Nacional</h4>
                        <p class="text-xs text-slate-500">Recibe tu pedido en la puerta de tu casa.</p>
                    </div>
                </div>
                <div class="flex items-center gap-4 p-4 rounded-2xl bg-drix-50/50 border border-drix-100">
                    <div class="w-12 h-12 rounded-xl bg-drix-600 text-white flex items-center justify-center flex-shrink-0 shadow-md shadow-drix-600/30">
                        <i data-lucide="shield-check" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-slate-900 text-sm">Garantía Drix</h4>
                        <p class="text-xs text-slate-500">Calidad comprobada en moda y tecnología.</p>
                    </div>
                </div>
            </div>
        </section>

        <
