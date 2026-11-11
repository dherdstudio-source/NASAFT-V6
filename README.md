<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-100">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Sabar Furniture - Mobile App Content Manager</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Chart.js for Analytics -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;1,600&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        forest: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#1e7f55',
                            600: '#1b6b48',
                            700: '#16583b',
                            800: '#134730',
                            900: '#0f3826',
                            950: '#092117',
                        },
                        appPurple: {
                            500: '#5c52d4',
                            600: '#4c42c7',
                            700: '#3e34b3'
                        }
                    },
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        serif: ['Playfair Display', 'serif']
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Scrollbar & Touch Optimization */
        ::-webkit-scrollbar {
            width: 4px;
            height: 4px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        
        body {
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        
        .mobile-card {
            background: #ffffff;
            border-radius: 24px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05);
        }

        .bottom-nav {
            box-shadow: 0 -10px 25px rgba(0, 0, 0, 0.06);
        }

        /* Responsive Layout & Desktop Fix for Long Content Scroll */
        .safe-bottom {
            padding-bottom: 160px !important;
        }

        @media (min-width: 768px) {
            .safe-bottom {
                padding-bottom: 100px !important;
            }
        }

        /* Ensure card action buttons stay clickable and interactive */
        .task-card-action {
            position: relative;
            z-index: 10;
            cursor: pointer;
            touch-action: manipulation;
        }
    </style>
</head>
<body class="bg-slate-100 font-sans text-slate-800 antialiased min-h-screen">

    <!-- Mobile & Desktop Frame Container (Fixed Scaling & Scroll Behavior) -->
    <div class="w-full max-w-md md:max-w-4xl lg:max-w-6xl mx-auto h-screen md:h-[94vh] bg-[#f8fafc] md:my-3 md:rounded-[36px] md:shadow-2xl overflow-hidden border border-slate-200/60 relative flex flex-col">

        <!-- App Header (Inspired by Mobile Image Reference) -->
        <header class="bg-gradient-to-r from-forest-800 to-forest-700 text-white p-4 md:p-5 rounded-b-[28px] md:rounded-b-[32px] shadow-lg sticky top-0 z-30 flex-shrink-0">
            <div class="flex items-center justify-between mb-3 md:mb-4">
                <!-- User Profile Intro -->
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 md:w-11 md:h-11 rounded-full bg-white/20 backdrop-blur border border-white/30 flex items-center justify-center font-extrabold text-sm text-white shadow-inner" id="userAvatar">
                        N
                    </div>
                    <div>
                        <p class="text-[11px] text-emerald-200 font-medium">Selamat Datang 👋</p>
                        <h2 class="font-bold text-sm md:text-base tracking-tight" id="userName">Natan</h2>
                    </div>
                </div>

                <!-- Top Header Actions -->
                <div class="flex items-center space-x-1.5 md:space-x-2">
                    <button onclick="openGroupDeleteModal()" class="w-9 h-9 md:w-10 md:h-10 rounded-full bg-rose-600/90 hover:bg-rose-600 text-white font-bold active:scale-95 flex items-center justify-center shadow-md transition-all" title="Pengelompokan & Hapus Tugas Berdasarkan Nama">
                        <i data-lucide="layers" class="w-4 h-4 md:w-5 md:h-5"></i>
                    </button>
                    <button onclick="openBulkTaskModal()" class="w-9 h-9 md:w-10 md:h-10 rounded-full bg-amber-400 text-slate-900 font-bold active:scale-95 flex items-center justify-center shadow-md transition-all" title="Generate Jadwal Sebulan / Masif">
                        <i data-lucide="zap" class="w-4 h-4 md:w-5 md:h-5"></i>
                    </button>
                    <button onclick="printTodayReport()" class="w-9 h-9 md:w-10 md:h-10 rounded-full bg-white/10 hover:bg-white/20 active:scale-95 flex items-center justify-center text-white transition-all" title="Cetak PDF Hari Ini">
                        <i data-lucide="printer" class="w-4 h-4 md:w-5 md:h-5 text-emerald-300"></i>
                    </button>
                    <button onclick="printMonthlyReport()" class="w-9 h-9 md:w-10 md:h-10 rounded-full bg-sky-500 hover:bg-sky-600 active:scale-95 flex items-center justify-center text-white shadow-md transition-all" title="Cetak Laporan Bulanan & Grafik">
                        <i data-lucide="file-bar-chart" class="w-4 h-4 md:w-5 md:h-5 text-white"></i>
                    </button>
                    <button onclick="openCreateTaskModal()" class="w-9 h-9 md:w-10 md:h-10 rounded-full bg-emerald-400 text-forest-900 font-bold active:scale-95 flex items-center justify-center shadow-md transition-all" title="Tambah Tugas Satuan">
                        <i data-lucide="plus" class="w-4 h-4 md:w-5 md:h-5"></i>
                    </button>
                </div>
            </div>

            <!-- Month/Year Selector & Bulk Tool Bar -->
            <div class="flex flex-wrap items-center justify-between bg-white/10 backdrop-blur border border-white/15 px-3 py-2 rounded-2xl text-xs text-white gap-2">
                <div class="flex items-center space-x-2 font-semibold">
                    <i data-lucide="calendar" class="w-4 h-4 text-emerald-300"></i>
                    <select id="monthSelect" class="bg-transparent text-white font-bold cursor-pointer focus:outline-none">
                        <!-- Populated dynamically -->
                    </select>
                    <select id="yearSelect" class="bg-transparent text-emerald-200 font-bold cursor-pointer focus:outline-none">
                        <!-- Populated dynamically -->
                    </select>
                </div>
                <div class="flex flex-wrap items-center gap-1.5">
                    <button onclick="printMonthlyReport()" class="bg-sky-500 hover:bg-sky-600 text-white px-2.5 py-1 rounded-xl text-[10px] font-bold tracking-wide uppercase flex items-center gap-1 shadow-sm">
                        <i data-lucide="file-bar-chart" class="w-3 h-3"></i>
                        <span>Print Laporan Sebulan</span>
                    </button>
                    <button onclick="openGroupDeleteModal()" class="bg-rose-500/90 hover:bg-rose-600 text-white px-2.5 py-1 rounded-xl text-[10px] font-bold tracking-wide uppercase flex items-center gap-1 shadow-sm">
                        <i data-lucide="list-filter" class="w-3 h-3"></i>
                        <span>Kelola Kelompok Nama</span>
                    </button>
                    <button onclick="openBulkTaskModal()" class="bg-amber-400/90 hover:bg-amber-400 text-slate-900 px-2.5 py-1 rounded-xl text-[10px] font-bold tracking-wide uppercase flex items-center gap-1 shadow-sm">
                        <i data-lucide="zap" class="w-3 h-3"></i>
                        <span>Generate 1 Bulan</span>
                    </button>
                    <button onclick="clearCompletedTasks()" class="bg-white/20 hover:bg-white/30 text-white px-2.5 py-1 rounded-xl text-[10px] font-bold tracking-wide uppercase flex items-center gap-1 shadow-sm" title="Hapus Semua Tugas Yang Sudah Dicentang">
                        <i data-lucide="check-check" class="w-3 h-3"></i>
                        <span>Hapus Yg Dicentang</span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Main Workspace Area (Fixed Desktop & Mobile Scroll Overflow for Long Task Lists) -->
        <main class="flex-1 p-4 md:p-6 space-y-5 safe-bottom overflow-y-auto h-full scroll-smooth">

            <!-- Quick Metrics Header Cards -->
            <div class="grid grid-cols-3 gap-3">
                <div class="bg-gradient-to-br from-forest-600 to-forest-700 text-white p-3.5 rounded-2xl shadow-sm text-center">
                    <p class="text-[10px] text-emerald-100 font-medium">Total Tugas</p>
                    <p id="kpiTotalTarget" class="text-xl font-extrabold mt-0.5">0</p>
                </div>
                <div class="bg-white border border-emerald-100 text-slate-800 p-3.5 rounded-2xl shadow-sm text-center">
                    <p class="text-[10px] text-slate-400 font-medium">Selesai</p>
                    <p id="kpiCompleted" class="text-xl font-extrabold text-forest-600 mt-0.5">0</p>
                </div>
                <div class="bg-white border border-amber-100 text-slate-800 p-3.5 rounded-2xl shadow-sm text-center">
                    <p class="text-[10px] text-slate-400 font-medium">Pending</p>
                    <p id="kpiRemaining" class="text-xl font-extrabold text-amber-500 mt-0.5">0</p>
                </div>
            </div>

            <!-- TAB 1: DASHBOARD MAIN VIEW -->
            <div id="tab-dashboard-content" class="space-y-5">

                <!-- Search Input Bar -->
                <div class="relative">
                    <i data-lucide="search" class="w-4 h-4 absolute left-4 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" id="searchInput" placeholder="Cari tugas atau konten..." class="w-full bg-white border border-slate-200/80 rounded-2xl pl-11 pr-4 py-3 text-xs text-slate-700 focus:outline-none focus:ring-2 focus:ring-forest-500/30 shadow-sm">
                </div>

                <!-- Calendar Strip Card -->
                <div class="mobile-card p-4 space-y-3">
                    <div class="flex items-center justify-between">
                        <h3 id="calendarTitle" class="font-bold text-slate-800 text-xs uppercase tracking-wider">Kalender</h3>
                        <button id="clearDateFilterBtn" class="hidden text-[10px] text-rose-500 font-semibold hover:underline">Reset Filter</button>
                    </div>

                    <!-- Calendar Header -->
                    <div class="grid grid-cols-7 gap-1 text-center text-[10px] font-bold text-slate-400 uppercase">
                        <div class="text-rose-500">Min</div>
                        <div>Sen</div>
                        <div>Sel</div>
                        <div>Rab</div>
                        <div>Kam</div>
                        <div>Jum</div>
                        <div>Sab</div>
                    </div>

                    <!-- Calendar Grid -->
                    <div id="calendarGrid" class="grid grid-cols-7 gap-1">
                        <!-- Generated via JS -->
                    </div>
                </div>

                <!-- Content Tasks List (Card-based like Mobile App) -->
                <div class="space-y-3">
                    <div class="flex items-center justify-between">
                        <h3 class="font-bold text-slate-800 text-sm">Daftar Tugas Konten</h3>
                        <div class="flex items-center gap-2">
                            <button onclick="clearCompletedTasks()" class="text-[11px] font-semibold text-rose-600 bg-rose-50 hover:bg-rose-100 px-2.5 py-1 rounded-full border border-rose-200 transition-all flex items-center gap-1">
                                <i data-lucide="trash-2" class="w-3 h-3"></i>
                                <span>Hapus Tugas Selesai</span>
                            </button>
                            <span class="text-[11px] font-semibold text-forest-700 bg-emerald-50 px-2.5 py-1 rounded-full border border-emerald-100">
                                Checklist Harian
                            </span>
                        </div>
                    </div>

                    <!-- Task Items Container -->
                    <div id="scheduleCardsContainer" class="space-y-3">
                        <!-- Generated dynamically via JS -->
                    </div>
                </div>

            </div>

            <!-- TAB 2: FULL CALENDAR MOBILE VIEW -->
            <div id="tab-calendar-content" class="hidden space-y-4">
                <div class="mobile-card p-4 space-y-4">
                    <h3 id="calendarTitleFull" class="font-bold text-slate-800 text-sm">Kalender Konten</h3>
                    <div class="grid grid-cols-7 gap-1.5 text-center text-[10px] font-bold text-slate-400 uppercase">
                        <div class="text-rose-500">Min</div>
                        <div>Sen</div>
                        <div>Sel</div>
                        <div>Rab</div>
                        <div>Kam</div>
                        <div>Jum</div>
                        <div>Sab</div>
                    </div>
                    <div id="fullCalendarGrid" class="grid grid-cols-7 gap-1.5">
                        <!-- Generated via JS -->
                    </div>
                </div>
            </div>

            <!-- TAB 3: SCHEDULE CHECKLIST ONLY VIEW -->
            <div id="tab-schedule-content" class="hidden space-y-3">
                <div class="flex items-center justify-between">
                    <h3 class="font-bold text-slate-800 text-sm">Semua Daftar Tugas</h3>
                    <button onclick="openCreateTaskModal()" class="text-xs bg-forest-600 text-white font-semibold px-3 py-1.5 rounded-xl">
                        + Tambah
                    </button>
                </div>
                <div id="fullScheduleCardsContainer" class="space-y-3">
                    <!-- Cards rendered dynamically -->
                </div>
            </div>

            <!-- TAB 4: ANALYTICS VIEW -->
            <div id="tab-analytics-content" class="hidden space-y-4">
                <div class="mobile-card p-5 space-y-4">
                    <h3 class="font-bold text-slate-800 text-sm">Target & Performa Konten</h3>
                    <div class="h-48 w-full">
                        <canvas id="analyticsChart"></canvas>
                    </div>
                    <div class="grid grid-cols-2 gap-3 text-center pt-2">
                        <div class="p-3 bg-emerald-50 rounded-xl border border-emerald-100">
                            <p class="text-[10px] text-slate-500">Persentase Selesai</p>
                            <p id="analyticsRateVal" class="text-xl font-bold text-forest-700">0%</p>
                        </div>
                        <div class="p-3 bg-amber-50 rounded-xl border border-amber-100">
                            <p class="text-[10px] text-slate-500">Sisa Pending</p>
                            <p id="analyticsPendingVal" class="text-xl font-bold text-amber-600">0</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 5: PROFILE & SETTINGS VIEW -->
            <div id="tab-settings-content" class="hidden space-y-4">
                <div class="mobile-card p-5 space-y-4">
                    <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                        <i data-lucide="user-check" class="w-4 h-4 text-forest-600"></i>
                        Profil Pelapor & Pengaturan
                    </h3>
                    <div class="space-y-3 text-xs">
                        <div>
                            <label class="block font-semibold text-slate-700 mb-1">Nama Creator / Pelapor</label>
                            <input type="text" id="settingsNameInput" value="Natan" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                        </div>
                        <div>
                            <label class="block font-semibold text-slate-700 mb-1">Jabatan / Divisi</label>
                            <input type="text" id="settingsRoleInput" value="Content Creator & Graphic Design" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                        </div>
                        <button onclick="saveSettings()" class="w-full py-2.5 bg-forest-600 hover:bg-forest-700 text-white font-bold rounded-xl shadow-md transition-all">
                            Simpan Perubahan Profil
                        </button>
                    </div>
                </div>

                <div class="mobile-card p-5 space-y-2">
                    <h4 class="font-bold text-slate-800 text-xs text-rose-600">Area Bahaya</h4>
                    <button onclick="resetData()" class="w-full py-2.5 border border-rose-200 text-rose-600 font-semibold text-xs rounded-xl hover:bg-rose-50">
                        Reset Seluruh Sesi Data Bulan Ini
                    </button>
                </div>
            </div>

        </main>

        <!-- Bottom Mobile/Desktop Navigation Bar -->
        <nav class="bottom-nav fixed md:absolute bottom-0 left-0 right-0 max-w-md md:max-w-4xl lg:max-w-6xl mx-auto bg-white/95 backdrop-blur border-t border-slate-200 px-3 py-2 flex items-center justify-around z-40 rounded-t-2xl">
            <a href="#" onclick="switchTab('dashboard')" id="nav-dashboard" class="flex flex-col items-center justify-center w-14 py-1 text-forest-700 font-bold">
                <i data-lucide="layout-grid" class="w-5 h-5"></i>
                <span class="text-[9px] mt-1">Dashboard</span>
            </a>
            <a href="#" onclick="switchTab('schedule')" id="nav-schedule" class="flex flex-col items-center justify-center w-14 py-1 text-slate-400 font-medium">
                <i data-lucide="check-square" class="w-5 h-5"></i>
                <span class="text-[9px] mt-1">Tugas</span>
            </a>
            <a href="#" onclick="openCreateTaskModal()" class="flex flex-col items-center justify-center -mt-6">
                <div class="w-12 h-12 rounded-full bg-forest-600 text-white flex items-center justify-center shadow-lg active:scale-95 transition-all">
                    <i data-lucide="plus" class="w-6 h-6"></i>
                </div>
            </a>
            <a href="#" onclick="switchTab('calendar')" id="nav-calendar" class="flex flex-col items-center justify-center w-14 py-1 text-slate-400 font-medium">
                <i data-lucide="calendar" class="w-5 h-5"></i>
                <span class="text-[9px] mt-1">Kalender</span>
            </a>
            <a href="#" onclick="switchTab('settings')" id="nav-settings" class="flex flex-col items-center justify-center w-14 py-1 text-slate-400 font-medium">
                <i data-lucide="user" class="w-5 h-5"></i>
                <span class="text-[9px] mt-1">Profil</span>
            </a>
        </nav>

    </div>

    <!-- Modal Pengelompokan & Hapus Tugas Berdasarkan Nama -->
    <div id="groupDeleteModal" class="fixed inset-0 z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 bg-slate-900/60 backdrop-blur-sm hidden">
        <div class="bg-white w-full max-w-lg p-6 rounded-t-[32px] sm:rounded-3xl shadow-2xl space-y-4 max-h-[88vh] flex flex-col">
            <div class="flex items-center justify-between border-b border-slate-100 pb-3 flex-shrink-0">
                <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                    <i data-lucide="layers" class="w-4 h-4 text-rose-600"></i>
                    Kelola / Hapus Tugas Berdasarkan Nama
                </h3>
                <button id="closeGroupDeleteModalBtn" class="text-slate-400 hover:text-slate-600 p-1">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <p class="text-xs text-slate-500 flex-shrink-0">
                Daftar di bawah mengelompokkan tugas berdasarkan <b>nama/detail tugas yang sama</b>. Anda dapat menghapus seluruh duplikat tugas dengan nama tersebut secara sekaligus dalam 1 kali klik.
            </p>

            <div id="groupDeleteListContainer" class="flex-1 overflow-y-auto space-y-2 pr-1 text-xs max-h-[50vh]">
                <!-- Group Items Rendered dynamically -->
            </div>

            <div class="pt-3 border-t border-slate-100 flex justify-end flex-shrink-0">
                <button id="closeGroupDeleteModalFooterBtn" class="px-4 py-2 bg-slate-100 text-slate-600 font-semibold rounded-xl text-xs">
                    Tutup Modal
                </button>
            </div>
        </div>
    </div>

    <!-- Form Modal for Mobile Input/Edit -->
    <div id="addTaskModal" class="fixed inset-0 z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 bg-slate-900/50 backdrop-blur-sm hidden">
        <div class="bg-white w-full max-w-md p-6 rounded-t-[32px] sm:rounded-3xl shadow-2xl space-y-4 animate-in fade-in slide-in-from-bottom duration-200 max-h-[90vh] flex flex-col">
            <div class="flex items-center justify-between border-b border-slate-100 pb-3 flex-shrink-0">
                <h3 id="modalTitle" class="font-bold text-slate-800 text-sm flex items-center gap-2">
                    <i data-lucide="plus-circle" class="w-4 h-4 text-forest-600"></i>
                    Input Tugas Baru
                </h3>
                <button id="closeAddTaskModalBtn" class="text-slate-400 hover:text-slate-600 p-1">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <form id="addTaskForm" class="space-y-3 text-xs flex-1 overflow-y-auto pr-1">
                <input type="hidden" id="editingTaskId" value="">

                <div>
                    <label class="block font-semibold text-slate-700 mb-1">Tanggal Tugas</label>
                    <input type="date" id="formDate" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                </div>

                <div>
                    <label class="block font-semibold text-slate-700 mb-1">Jenis Konten</label>
                    <select id="formContentType" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                        <option value="Foto">Foto Produk</option>
                        <option value="Foto Feed IG & FB">Foto Feed IG & FB</option>
                        <option value="Video Organik">Video Organik (Reels/TikTok)</option>
                        <option value="Video Ads">Video Ads</option>
                        <option value="Foto Katalog Ads">Foto Katalog Ads</option>
                    </select>
                </div>

                <div>
                    <label class="block font-semibold text-slate-700 mb-1">Detail Keterangan Tugas</label>
                    <textarea id="formTaskDetail" rows="3" required placeholder="Tuliskan keterangan detail pekerjaan di sini..." class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30 resize-none"></textarea>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Platform</label>
                        <input type="text" id="formPlatform" value="Instagram & FB" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium">
                    </div>
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">PIC (Pelapor)</label>
                        <input type="text" id="formPic" value="Natan" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium">
                    </div>
                </div>

                <div class="flex items-center justify-end space-x-2 pt-2 border-t border-slate-100 flex-shrink-0">
                    <button type="button" id="cancelFormBtn" class="px-3.5 py-2 rounded-xl border border-slate-200 text-slate-600 font-semibold">
                        Batal
                    </button>
                    <button type="submit" class="px-4 py-2 bg-forest-600 hover:bg-forest-700 text-white font-bold rounded-xl shadow-md">
                        Simpan Tugas
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal Generate Jadwal Masif (Bulk Task Generator) -->
    <div id="bulkTaskModal" class="fixed inset-0 z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 bg-slate-900/60 backdrop-blur-sm hidden">
        <div class="bg-white w-full max-w-lg p-6 rounded-t-[32px] sm:rounded-3xl shadow-2xl space-y-4 animate-in fade-in slide-in-from-bottom duration-200 max-h-[90vh] flex flex-col">
            <div class="flex items-center justify-between border-b border-slate-100 pb-3 flex-shrink-0">
                <h3 class="font-bold text-slate-800 text-sm flex items-center gap-2">
                    <i data-lucide="zap" class="w-4 h-4 text-amber-500"></i>
                    Generate Multi-Tugas Masif (1 Bulan+)
                </h3>
                <button id="closeBulkTaskModalBtn" class="text-slate-400 hover:text-slate-600 p-1">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <form id="bulkTaskForm" class="space-y-4 text-xs flex-1 overflow-y-auto pr-1">
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Tanggal Mulai</label>
                        <input type="date" id="bulkStartDate" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                    </div>
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Tanggal Selesai</label>
                        <input type="date" id="bulkEndDate" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                    </div>
                </div>

                <div>
                    <label class="block font-semibold text-slate-700 mb-1">Pola Hari Kerja</label>
                    <select id="bulkDayPattern" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                        <option value="all">Setiap Hari (Senin - Minggu)</option>
                        <option value="workdays" selected>Hari Kerja (Senin - Sabtu)</option>
                        <option value="weekends">Hari Libur / Minggu Saja</option>
                    </select>
                </div>

                <!-- Multi-Task Template Inputs Header -->
                <div class="space-y-2 pt-2 border-t border-slate-100">
                    <div class="flex items-center justify-between">
                        <label class="font-bold text-slate-800 text-xs flex items-center gap-1.5">
                            <i data-lucide="list-plus" class="w-4 h-4 text-forest-600"></i>
                            Daftar Tugas Yang Akan Di-generate Bersamaan:
                        </label>
                        <button type="button" onclick="addBulkTaskRow()" class="text-[11px] bg-forest-50 text-forest-700 font-bold px-2.5 py-1 rounded-xl border border-forest-200 hover:bg-forest-100 flex items-center gap-1">
                            <i data-lucide="plus" class="w-3 h-3"></i>
                            <span>+ Tambah Template Tugas</span>
                        </button>
                    </div>

                    <!-- Container Dynamic Task Input Rows -->
                    <div id="bulkTaskRowsContainer" class="space-y-3">
                        <!-- Populated by JS with dynamic rows -->
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3 pt-2 border-t border-slate-100">
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">Platform Target</label>
                        <input type="text" id="bulkPlatform" value="Instagram & FB" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium">
                    </div>
                    <div>
                        <label class="block font-semibold text-slate-700 mb-1">PIC (Pelapor)</label>
                        <input type="text" id="bulkPic" value="Natan" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-slate-700 font-medium">
                    </div>
                </div>

                <div class="flex items-center justify-end space-x-2 pt-3 border-t border-slate-100 flex-shrink-0">
                    <button type="button" id="cancelBulkFormBtn" class="px-3.5 py-2 rounded-xl border border-slate-200 text-slate-600 font-semibold">
                        Batal
                    </button>
                    <button type="submit" class="px-4 py-2 bg-gradient-to-r from-amber-500 to-amber-600 text-slate-900 font-bold rounded-xl shadow-md flex items-center gap-1.5">
                        <i data-lucide="zap" class="w-4 h-4"></i>
                        <span>Generate Multi-Tugas Sebulan</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Print Guard CSS -->
    <style>
        @media print {
            body {
                background: white !important;
                padding: 0 !important;
            }
            body > *:not(#printableReport) {
                display: none !important;
            }
            #printableReport {
                display: block !important;
                position: absolute !important;
                left: 0 !important;
                top: 0 !important;
                width: 100% !important;
                padding: 20px !important;
            }
        }
    </style>

    <!-- Application Script -->
    <script>
        const MONTH_NAMES = [
            "Januari", "Februari", "Maret", "April", "Mei", "Juni", 
            "Juli", "Agustus", "September", "Oktober", "November", "Desember"
        ];
        const DAY_NAMES = ["Minggu", "Senin", "Selasa", "Rabu", "Kamis", "Jumat", "Sabtu"];
        const CONTENT_TYPES_ROTATION = [
            "Foto", 
            "Video Organik", 
            "Foto Feed IG & FB", 
            "Video Ads", 
            "Foto Katalog Ads"
        ];

        let currentMonth = 10; // November (0-indexed)
        let currentYear = 2026;
        let selectedFilterDate = null;
        let scheduleData = [];
        let analyticsChartInstance = null;
        let bulkRowCounter = 0;

        document.addEventListener('DOMContentLoaded', () => {
            loadUserProfile();
            setupSelectors();
            loadScheduleForMonth(currentMonth, currentYear);
            renderApp();
            initChart();
            setupEventListeners();
            lucide.createIcons();
        });

        function loadUserProfile() {
            const userNameElem = document.getElementById('userName');
            const userAvatarElem = document.getElementById('userAvatar');
            const settingsNameInput = document.getElementById('settingsNameInput');
            const settingsRoleInput = document.getElementById('settingsRoleInput');

            const savedName = localStorage.getItem('sabar_creator_name') || 'Natan';
            const savedRole = localStorage.getItem('sabar_creator_role') || 'Content Creator & Graphic Design';

            if (userNameElem) userNameElem.innerText = savedName;
            if (userAvatarElem) userAvatarElem.innerText = savedName.charAt(0).toUpperCase();
            if (settingsNameInput) settingsNameInput.value = savedName;
            if (settingsRoleInput) settingsRoleInput.value = savedRole;
        }

        function saveSettings() {
            const nameInput = document.getElementById('settingsNameInput');
            const roleInput = document.getElementById('settingsRoleInput');

            if (nameInput) localStorage.setItem('sabar_creator_name', nameInput.value.trim());
            if (roleInput) localStorage.setItem('sabar_creator_role', roleInput.value.trim());

            loadUserProfile();
            alert('Profil pelapor berhasil diperbarui!');
        }

        function setupSelectors() {
            const monthSelect = document.getElementById('monthSelect');
            const yearSelect = document.getElementById('yearSelect');

            if (monthSelect) {
                monthSelect.innerHTML = MONTH_NAMES.map((m, idx) => 
                    `<option value="${idx}" ${idx === currentMonth ? 'selected' : ''}>${m}</option>`
                ).join('');
            }

            if (yearSelect) {
                const years = [2025, 2026, 2027, 2028];
                yearSelect.innerHTML = years.map(y => 
                    `<option value="${y}" ${y === currentYear ? 'selected' : ''}>${y}</option>`
                ).join('');
            }
        }

        function getStorageKey(m, y) {
            return `sabar_schedule_${y}_${m}`;
        }

        function loadScheduleForMonth(m, y) {
            const raw = localStorage.getItem(getStorageKey(m, y));
            if (raw) {
                try {
                    scheduleData = JSON.parse(raw);
                } catch(e) {
                    scheduleData = [];
                }
            } else {
                scheduleData = [];
            }
        }

        function saveCurrentSchedule() {
            localStorage.setItem(getStorageKey(currentMonth, currentYear), JSON.stringify(scheduleData));
        }

        function renderApp() {
            renderKpi();
            renderCalendar();
            renderCards();
            updateChart();
            lucide.createIcons();
        }

        function renderKpi() {
            const total = scheduleData.length;
            const completed = scheduleData.filter(t => t.completed).length;
            const remaining = total - completed;

            const totalElem = document.getElementById('kpiTotalTarget');
            const completedElem = document.getElementById('kpiCompleted');
            const remainingElem = document.getElementById('kpiRemaining');
            const rateVal = document.getElementById('analyticsRateVal');
            const pendingVal = document.getElementById('analyticsPendingVal');

            if (totalElem) totalElem.innerText = total;
            if (completedElem) completedElem.innerText = completed;
            if (remainingElem) remainingElem.innerText = remaining;

            const ratePct = total > 0 ? Math.round((completed / total) * 100) : 0;
            if (rateVal) rateVal.innerText = `${ratePct}%`;
            if (pendingVal) pendingVal.innerText = remaining;
        }

        function renderCalendar() {
            const grids = [
                document.getElementById('calendarGrid'),
                document.getElementById('fullCalendarGrid')
            ];

            const calendarTitle = document.getElementById('calendarTitle');
            const calendarTitleFull = document.getElementById('calendarTitleFull');
            const clearBtn = document.getElementById('clearDateFilterBtn');

            const titleText = `${MONTH_NAMES[currentMonth]} ${currentYear}`;
            if (calendarTitle) calendarTitle.innerText = titleText;
            if (calendarTitleFull) calendarTitleFull.innerText = `Kalender ${titleText}`;

            if (selectedFilterDate && clearBtn) {
                clearBtn.classList.remove('hidden');
            } else if (clearBtn) {
                clearBtn.classList.add('hidden');
            }

            const firstDayIndex = new Date(currentYear, currentMonth, 1).getDay();
            const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

            grids.forEach(grid => {
                if (!grid) return;
                grid.innerHTML = '';

                // Blank cells for padding
                for (let i = 0; i < firstDayIndex; i++) {
                    grid.innerHTML += `<div class="h-8 rounded-lg bg-slate-50"></div>`;
                }

                // Month days
                for (let day = 1; day <= daysInMonth; day++) {
                    const dateStr = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                    const hasTasks = scheduleData.filter(t => t.dateStr === dateStr);
                    const isCompleted = hasTasks.length > 0 && hasTasks.every(t => t.completed);
                    const isSelected = selectedFilterDate === dateStr;

                    let bgClass = "bg-white border border-slate-200 text-slate-700 hover:bg-slate-50";
                    if (isSelected) {
                        bgClass = "bg-forest-600 text-white font-bold shadow-md scale-105";
                    } else if (hasTasks.length > 0) {
                        bgClass = isCompleted 
                            ? "bg-emerald-100 border border-emerald-300 text-emerald-800 font-bold"
                            : "bg-amber-50 border border-amber-200 text-amber-800 font-bold";
                    }

                    grid.innerHTML += `
                        <button onclick="filterByDate('${dateStr}')" class="h-8 rounded-xl ${bgClass} flex flex-col items-center justify-center text-xs transition-all relative">
                            <span>${day}</span>
                            ${hasTasks.length > 0 && !isSelected ? `<span class="w-1 h-1 rounded-full ${isCompleted ? 'bg-emerald-600' : 'bg-amber-500'} absolute bottom-1"></span>` : ''}
                        </button>
                    `;
                }
            });
        }

        function filterByDate(dateStr) {
            if (selectedFilterDate === dateStr) {
                selectedFilterDate = null;
            } else {
                selectedFilterDate = dateStr;
            }
            renderCalendar();
            renderCards();
        }

        function renderCards() {
            const containers = [
                document.getElementById('scheduleCardsContainer'),
                document.getElementById('fullScheduleCardsContainer')
            ];

            const searchInput = document.getElementById('searchInput');
            const searchVal = searchInput ? searchInput.value.toLowerCase() : '';

            let filtered = scheduleData;

            if (selectedFilterDate) {
                filtered = filtered.filter(t => t.dateStr === selectedFilterDate);
            }

            if (searchVal) {
                filtered = filtered.filter(t => 
                    t.taskDetail.toLowerCase().includes(searchVal) ||
                    t.contentType.toLowerCase().includes(searchVal) ||
                    t.pic.toLowerCase().includes(searchVal)
                );
            }

            // Sort by dayNum ascending
            filtered.sort((a, b) => a.dayNum - b.dayNum);

            containers.forEach(container => {
                if (!container) return;
                if (filtered.length === 0) {
                    container.innerHTML = `
                        <div class="mobile-card p-6 text-center space-y-2">
                            <i data-lucide="inbox" class="w-8 h-8 text-slate-300 mx-auto"></i>
                            <p class="text-xs text-slate-500 font-medium">Belum ada tugas terjadwal</p>
                            <button onclick="openCreateTaskModal()" class="text-xs bg-emerald-50 border border-emerald-200 text-forest-700 px-3 py-1.5 rounded-xl font-semibold">
                                + Tambah Tugas Baru
                            </button>
                        </div>
                    `;
                    return;
                }

                container.innerHTML = filtered.map(t => `
                    <div class="mobile-card p-4 space-y-3 border-l-4 ${t.completed ? 'border-emerald-500 bg-emerald-50/20' : 'border-amber-400'} relative">
                        <div class="flex items-start justify-between">
                            <div class="flex items-center space-x-2">
                                <span class="bg-slate-100 text-slate-700 px-2.5 py-1 rounded-xl text-[10px] font-bold">
                                    ${t.dayName}, ${t.dayNum} ${MONTH_NAMES[currentMonth]}
                                </span>
                                <span class="bg-forest-50 border border-forest-100 text-forest-700 px-2.5 py-1 rounded-xl text-[10px] font-bold">
                                    ${escapeHtml(t.contentType)}
                                </span>
                            </div>
                            <div class="flex items-center space-x-1 relative z-10">
                                <button onclick="editTask('${t.id}')" class="task-card-action p-2 text-slate-400 hover:text-forest-600 active:scale-95 transition-all" title="Edit Tugas">
                                    <i data-lucide="edit-3" class="w-4 h-4"></i>
                                </button>
                                <button onclick="deleteTask('${t.id}')" class="task-card-action p-2 text-slate-400 hover:text-rose-600 active:scale-95 transition-all" title="Hapus Tugas">
                                    <i data-lucide="trash-2" class="w-4 h-4"></i>
                                </button>
                            </div>
                        </div>

                        <p class="text-xs font-semibold text-slate-800 leading-relaxed ${t.completed ? 'line-through text-slate-400' : ''}">
                            ${escapeHtml(t.taskDetail)}
                        </p>

                        <div class="flex items-center justify-between pt-1 border-t border-slate-100 text-[11px]">
                            <div class="flex items-center space-x-2 text-slate-500">
                                <span>📱 ${escapeHtml(t.platform)}</span>
                                <span>•</span>
                                <span>👤 ${escapeHtml(t.pic)}</span>
                            </div>
                            <button onclick="toggleTaskStatus('${t.id}')" class="task-card-action flex items-center space-x-1.5 px-3 py-1.5 rounded-xl text-xs font-bold transition-all active:scale-95 ${t.completed ? 'bg-emerald-600 text-white shadow-sm' : 'bg-slate-100 text-slate-600 hover:bg-emerald-50 hover:text-emerald-700'}">
                                <i data-lucide="${t.completed ? 'check-circle' : 'circle'}" class="w-4 h-4"></i>
                                <span>${t.completed ? 'Selesai' : 'Centang'}</span>
                            </button>
                        </div>
                    </div>
                `).join('');
            });

            lucide.createIcons();
        }

        function toggleTaskStatus(id) {
            const task = scheduleData.find(t => t.id === id);
            if (task) {
                task.completed = !task.completed;
                saveCurrentSchedule();
                renderApp();
            }
        }

        function openCreateTaskModal() {
            const modal = document.getElementById('addTaskModal');
            const title = document.getElementById('modalTitle');
            const editingId = document.getElementById('editingTaskId');
            const formDate = document.getElementById('formDate');
            const formDetail = document.getElementById('formTaskDetail');

            if (title) title.innerHTML = `<i data-lucide="plus-circle" class="w-4 h-4 text-forest-600"></i> Input Tugas Baru`;
            if (editingId) editingId.value = '';
            if (formDetail) formDetail.value = '';

            const today = new Date();
            const y = today.getFullYear();
            const m = String(today.getMonth() + 1).padStart(2, '0');
            const d = String(today.getDate()).padStart(2, '0');
            if (formDate) formDate.value = `${y}-${m}-${d}`;

            if (modal) modal.classList.remove('hidden');
            lucide.createIcons();
        }

        function editTask(id) {
            const task = scheduleData.find(t => t.id === id);
            if (!task) return;

            const modal = document.getElementById('addTaskModal');
            const title = document.getElementById('modalTitle');
            const editingId = document.getElementById('editingTaskId');
            const formDate = document.getElementById('formDate');
            const contentType = document.getElementById('formContentType');
            const taskDetail = document.getElementById('formTaskDetail');
            const platform = document.getElementById('formPlatform');
            const pic = document.getElementById('formPic');

            if (title) title.innerHTML = `<i data-lucide="edit-3" class="w-4 h-4 text-forest-600"></i> Edit Tugas`;
            if (editingId) editingId.value = task.id;
            if (formDate) formDate.value = task.dateStr;
            if (contentType) contentType.value = task.contentType;
            if (taskDetail) taskDetail.value = task.taskDetail;
            if (platform) platform.value = task.platform;
            if (pic) pic.value = task.pic;

            if (modal) modal.classList.remove('hidden');
            lucide.createIcons();
        }

        function deleteTask(id) {
            const taskIndex = scheduleData.findIndex(t => t.id === id);
            if (taskIndex === -1) return;

            const task = scheduleData[taskIndex];
            
            if (confirm(`Apakah Anda yakin ingin menghapus tugas "${task.taskDetail}"?`)) {
                // Remove selected task by ID
                scheduleData.splice(taskIndex, 1);
                
                // Save updated schedule to localStorage & re-render UI
                saveCurrentSchedule();
                renderApp();
            }
        }

        function openBulkTaskModal() {
            const startDateElem = document.getElementById('bulkStartDate');
            const endDateElem = document.getElementById('bulkEndDate');
            const bulkModal = document.getElementById('bulkTaskModal');
            const container = document.getElementById('bulkTaskRowsContainer');

            const lastDayOfM = new Date(currentYear, currentMonth + 1, 0).getDate();
            const startStr = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-01`;
            const endStr = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(lastDayOfM).padStart(2, '0')}`;

            if (startDateElem) startDateElem.value = startStr;
            if (endDateElem) endDateElem.value = endStr;

            // Reset dynamic task rows and populate with 2 initial rows
            if (container) {
                container.innerHTML = '';
                bulkRowCounter = 0;
                addBulkTaskRow('Foto', 'Foto Produk Set Minimalis Sabar Furniture');
                addBulkTaskRow('Video Organik', 'Video Reels & TikTok Process Finishing Kayu Jati');
            }

            if (bulkModal) bulkModal.classList.remove('hidden');
            lucide.createIcons();
        }

        function addBulkTaskRow(defaultType = 'Foto', defaultDetail = '') {
            const container = document.getElementById('bulkTaskRowsContainer');
            if (!container) return;

            bulkRowCounter++;
            const rowId = `bulk_row_${bulkRowCounter}`;

            const rowDiv = document.createElement('div');
            rowDiv.id = rowId;
            rowDiv.className = 'p-3 bg-slate-50 border border-slate-200 rounded-2xl space-y-2 relative group';

            rowDiv.innerHTML = `
                <div class="flex items-center justify-between">
                    <span class="text-[10px] font-bold text-forest-700 bg-emerald-100 px-2 py-0.5 rounded-md">
                        Tugas #${container.children.length + 1}
                    </span>
                    ${container.children.length > 0 ? `
                        <button type="button" onclick="removeBulkTaskRow('${rowId}')" class="text-slate-400 hover:text-rose-600 text-[10px] font-bold flex items-center gap-0.5">
                            <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                            <span>Hapus Baris</span>
                        </button>
                    ` : ''}
                </div>
                <div class="grid grid-cols-3 gap-2">
                    <div class="col-span-1">
                        <select class="bulk-row-type w-full bg-white border border-slate-200 rounded-xl px-2 py-1.5 text-[11px] text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                            <option value="Foto" ${defaultType === 'Foto' ? 'selected' : ''}>Foto Produk</option>
                            <option value="Foto Feed IG & FB" ${defaultType === 'Foto Feed IG & FB' ? 'selected' : ''}>Foto Feed IG & FB</option>
                            <option value="Video Organik" ${defaultType === 'Video Organik' ? 'selected' : ''}>Video Organik (Reels/TikTok)</option>
                            <option value="Video Ads" ${defaultType === 'Video Ads' ? 'selected' : ''}>Video Ads</option>
                            <option value="Foto Katalog Ads" ${defaultType === 'Foto Katalog Ads' ? 'selected' : ''}>Foto Katalog Ads</option>
                        </select>
                    </div>
                    <div class="col-span-2">
                        <input type="text" required placeholder="Detail Keterangan Tugas..." value="${escapeHtml(defaultDetail)}" class="bulk-row-detail w-full bg-white border border-slate-200 rounded-xl px-2.5 py-1.5 text-[11px] text-slate-700 font-medium focus:outline-none focus:ring-2 focus:ring-forest-500/30">
                    </div>
                </div>
            `;

            container.appendChild(rowDiv);
            lucide.createIcons();
        }

        function removeBulkTaskRow(rowId) {
            const rowElem = document.getElementById(rowId);
            if (rowElem) rowElem.remove();
        }

        function setupEventListeners() {
            const modal = document.getElementById('addTaskModal');
            const closeBtn = document.getElementById('closeAddTaskModalBtn');
            const cancelBtn = document.getElementById('cancelFormBtn');
            const addTaskForm = document.getElementById('addTaskForm');
            const monthSelect = document.getElementById('monthSelect');
            const yearSelect = document.getElementById('yearSelect');
            const searchInput = document.getElementById('searchInput');

            const bulkModal = document.getElementById('bulkTaskModal');
            const closeBulkBtn = document.getElementById('closeBulkTaskModalBtn');
            const cancelBulkBtn = document.getElementById('cancelBulkFormBtn');
            const bulkTaskForm = document.getElementById('bulkTaskForm');

            const groupModal = document.getElementById('groupDeleteModal');
            const closeGroupBtn = document.getElementById('closeGroupDeleteModalBtn');
            const closeGroupFooterBtn = document.getElementById('closeGroupDeleteModalFooterBtn');

            if (closeGroupBtn) closeGroupBtn.addEventListener('click', () => groupModal.classList.add('hidden'));
            if (closeGroupFooterBtn) closeGroupFooterBtn.addEventListener('click', () => groupModal.classList.add('hidden'));

            if (closeBulkBtn) closeBulkBtn.addEventListener('click', () => bulkModal.classList.add('hidden'));
            if (cancelBulkBtn) cancelBulkBtn.addEventListener('click', () => bulkModal.classList.add('hidden'));

            if (bulkTaskForm) {
                bulkTaskForm.addEventListener('submit', (e) => {
                    e.preventDefault();
                    const startStr = document.getElementById('bulkStartDate').value;
                    const endStr = document.getElementById('bulkEndDate').value;
                    const pattern = document.getElementById('bulkDayPattern').value;
                    const platform = document.getElementById('bulkPlatform').value || 'Instagram & FB';
                    const pic = document.getElementById('bulkPic').value || 'Natan';

                    // Collect all task templates from dynamic rows
                    const typeElems = document.querySelectorAll('.bulk-row-type');
                    const detailElems = document.querySelectorAll('.bulk-row-detail');

                    const taskTemplates = [];
                    for (let i = 0; i < typeElems.length; i++) {
                        const typeVal = typeElems[i].value;
                        const detailVal = detailElems[i].value.trim();
                        if (detailVal) {
                            taskTemplates.push({ contentType: typeVal, taskDetail: detailVal });
                        }
                    }

                    if (!startStr || !endStr || taskTemplates.length === 0) {
                        alert("Harap lengkapi tanggal dan minimal 1 detail keterangan tugas!");
                        return;
                    }

                    const startDate = new Date(startStr);
                    const endDate = new Date(endStr);

                    if (startDate > endDate) {
                        alert("Tanggal mulai harus lebih awal daripada tanggal selesai!");
                        return;
                    }

                    let addedCount = 0;
                    let curDate = new Date(startDate);

                    while (curDate <= endDate) {
                        const dayOfWeek = curDate.getDay();
                        let isMatch = true;

                        // Workdays = Monday (1) through Saturday (6); Sunday (0) is weekend
                        if (pattern === 'workdays' && dayOfWeek === 0) isMatch = false;
                        if (pattern === 'weekends' && dayOfWeek !== 0) isMatch = false;

                        if (isMatch) {
                            const y = curDate.getFullYear();
                            const m = curDate.getMonth();
                            const d = curDate.getDate();
                            const dateStr = `${y}-${String(m + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`;

                            // Generate all template tasks for this matched day (Cumulative append)
                            taskTemplates.forEach(template => {
                                const newTaskObj = {
                                    id: 'task_' + Date.now() + '_' + Math.random().toString(36).substr(2, 5),
                                    dateStr: dateStr,
                                    dayNum: d,
                                    dayName: DAY_NAMES[dayOfWeek],
                                    contentType: template.contentType,
                                    taskDetail: template.taskDetail,
                                    platform: platform,
                                    pic: pic,
                                    completed: false
                                };
                                scheduleData.push(newTaskObj);
                                addedCount++;
                            });
                        }

                        curDate.setDate(curDate.getDate() + 1);
                    }

                    saveCurrentSchedule();
                    renderApp();
                    if (bulkModal) bulkModal.classList.add('hidden');
                    alert(`Berhasil meng-generate ${addedCount} tugas baru (${taskTemplates.length} jenis tugas per hari) ke dalam jadwal!`);
                });
            }

            if (closeBtn) {
                closeBtn.addEventListener('click', () => {
                    if (modal) modal.classList.add('hidden');
                });
            }

            if (cancelBtn) {
                cancelBtn.addEventListener('click', () => {
                    if (modal) modal.classList.add('hidden');
                });
            }

            if (addTaskForm) {
                addTaskForm.addEventListener('submit', (e) => {
                    e.preventDefault();
                    const editingId = document.getElementById('editingTaskId').value;
                    const dateStr = document.getElementById('formDate').value;
                    const contentType = document.getElementById('formContentType').value;
                    const taskDetail = document.getElementById('formTaskDetail').value;
                    const platform = document.getElementById('formPlatform').value || 'Instagram & FB';
                    const pic = document.getElementById('formPic').value || 'Natan';

                    if (!dateStr || !taskDetail) return;

                    const dateParts = dateStr.split('-');
                    const dayNum = parseInt(dateParts[2]);
                    const dObj = new Date(parseInt(dateParts[0]), parseInt(dateParts[1]) - 1, dayNum);
                    const dayName = DAY_NAMES[dObj.getDay()];

                    if (editingId) {
                        const task = scheduleData.find(t => t.id === editingId);
                        if (task) {
                            task.dateStr = dateStr;
                            task.contentType = contentType;
                            task.taskDetail = taskDetail;
                            task.platform = platform;
                            task.pic = pic;
                            task.dayNum = dayNum;
                            task.dayName = dayName;
                        }
                    } else {
                        const newTask = {
                            id: 'task_' + Date.now(),
                            dateStr,
                            dayNum,
                            dayName,
                            contentType,
                            taskDetail,
                            platform,
                            pic,
                            completed: false
                        };
                        scheduleData.push(newTask);
                    }

                    saveCurrentSchedule();
                    renderApp();
                    if (modal) modal.classList.add('hidden');
                });
            }

            if (monthSelect) {
                monthSelect.addEventListener('change', (e) => {
                    currentMonth = parseInt(e.target.value);
                    selectedFilterDate = null;
                    loadScheduleForMonth(currentMonth, currentYear);
                    renderApp();
                });
            }

            if (yearSelect) {
                yearSelect.addEventListener('change', (e) => {
                    currentYear = parseInt(e.target.value);
                    selectedFilterDate = null;
                    loadScheduleForMonth(currentMonth, currentYear);
                    renderApp();
                });
            }

            if (searchInput) {
                searchInput.addEventListener('input', () => {
                    renderCards();
                });
            }

            const clearBtn = document.getElementById('clearDateFilterBtn');
            if (clearBtn) {
                clearBtn.addEventListener('click', () => {
                    selectedFilterDate = null;
                    renderCalendar();
                    renderCards();
                });
            }
        }

        function switchTab(tabName) {
            const tabs = ['dashboard', 'calendar', 'schedule', 'analytics', 'settings'];
            tabs.forEach(t => {
                const content = document.getElementById(`tab-${t}-content`);
                const nav = document.getElementById(`nav-${t}`);
                if (content) {
                    if (t === tabName) content.classList.remove('hidden');
                    else content.classList.add('hidden');
                }
                if (nav) {
                    if (t === tabName) {
                        nav.classList.add('text-forest-700', 'font-bold');
                        nav.classList.remove('text-slate-400', 'font-medium');
                    } else {
                        nav.classList.remove('text-forest-700', 'font-bold');
                        nav.classList.add('text-slate-400', 'font-medium');
                    }
                }
            });
            lucide.createIcons();
        }

        function initChart() {
            const ctx = document.getElementById('analyticsChart');
            if (!ctx) return;
            
            analyticsChartInstance = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Selesai', 'Pending'],
                    datasets: [{
                        data: [0, 0],
                        backgroundColor: ['#1e7f55', '#f59e0b'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
        }

        function updateChart() {
            if (!analyticsChartInstance) return;
            const completedCount = scheduleData.filter(i => i.completed).length;
            const totalCount = scheduleData.length;
            const pendingCount = totalCount - completedCount;

            analyticsChartInstance.data.datasets[0].data = [completedCount, pendingCount];
            analyticsChartInstance.update();
        }

        // =========================================================
        // GROUP DELETE MANAGER & BULK UTILITIES
        // =========================================================

        function openGroupDeleteModal() {
            const modal = document.getElementById('groupDeleteModal');
            const container = document.getElementById('groupDeleteListContainer');

            if (!container) return;

            if (scheduleData.length === 0) {
                container.innerHTML = `
                    <div class="p-6 text-center text-slate-400 font-medium">
                        <i data-lucide="inbox" class="w-8 h-8 mx-auto mb-2 opacity-50"></i>
                        <p>Belum ada tugas di bulan ini.</p>
                    </div>
                `;
            } else {
                // Group tasks by exact name/detail (case-insensitive trim)
                const groups = {};
                scheduleData.forEach(task => {
                    const nameKey = task.taskDetail.trim();
                    if (!groups[nameKey]) {
                        groups[nameKey] = {
                            name: nameKey,
                            count: 0,
                            completedCount: 0,
                            contentType: task.contentType
                        };
                    }
                    groups[nameKey].count++;
                    if (task.completed) groups[nameKey].completedCount++;
                });

                const groupList = Object.values(groups);

                container.innerHTML = groupList.map(g => `
                    <div class="p-3 bg-slate-50 border border-slate-200 rounded-2xl flex items-center justify-between gap-3">
                        <div class="flex-1 min-w-0">
                            <p class="font-bold text-slate-800 text-xs truncate" title="${escapeHtml(g.name)}">
                                ${escapeHtml(g.name)}
                            </p>
                            <p class="text-[10px] text-slate-500 mt-0.5">
                                Kategori: <span class="font-semibold text-forest-700">${escapeHtml(g.contentType)}</span> • Total: <b>${g.count}x</b> (${g.completedCount} selesai)
                            </p>
                        </div>
                        <button onclick="deleteGroupByName('${escapeHtml(g.name).replace(/'/g, "\\'")}')" class="px-3 py-1.5 bg-rose-600 hover:bg-rose-700 text-white font-bold text-[11px] rounded-xl transition-all flex items-center gap-1 shadow-sm flex-shrink-0">
                            <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                            <span>Hapus (${g.count})</span>
                        </button>
                    </div>
                `).join('');
            }

            if (modal) modal.classList.remove('hidden');
            lucide.createIcons();
        }

        function deleteGroupByName(taskName) {
            const targetName = taskName.trim().toLowerCase();
            const matchingCount = scheduleData.filter(t => t.taskDetail.trim().toLowerCase() === targetName).length;

            if (matchingCount === 0) return;

            if (confirm(`Apakah Anda yakin ingin menghapus SEMUA (${matchingCount}) tugas bernama:\n"${taskName}"?`)) {
                scheduleData = scheduleData.filter(t => t.taskDetail.trim().toLowerCase() !== targetName);
                saveCurrentSchedule();
                renderApp();
                openGroupDeleteModal(); // Refresh modal content
            }
        }

        function clearCompletedTasks() {
            const completedCount = scheduleData.filter(t => t.completed).length;

            if (completedCount === 0) {
                alert("Tidak ada tugas yang berstatus Selesai (dicentang) untuk dihapus.");
                return;
            }

            if (confirm(`Apakah Anda yakin ingin menghapus ${completedCount} tugas yang sudah DICENTANG / SELESAI di bulan ini?`)) {
                scheduleData = scheduleData.filter(t => !t.completed);
                saveCurrentSchedule();
                renderApp();
            }
        }

        function resetData() {
            if (confirm("⚠️ WARN: Apakah Anda yakin ingin MENGHAPUS SEMUA DATA tugas di bulan ini? Data tidak dapat dikembalikan.")) {
                scheduleData = [];
                saveCurrentSchedule();
                renderApp();
                alert("Seluruh data bulan ini telah dikosongkan.");
            }
        }

        function printTodayReport() {
            const today = new Date();
            const y = today.getFullYear();
            const m = String(today.getMonth() + 1).padStart(2, '0');
            const d = String(today.getDate()).padStart(2, '0');
            const todayStr = `${y}-${m}-${d}`;

            const todayTasks = scheduleData.filter(t => t.dateStr === todayStr);
            const savedName = localStorage.getItem('sabar_creator_name') || 'Natan';
            const savedRole = localStorage.getItem('sabar_creator_role') || 'Content Creator';

            let reportDiv = document.getElementById('printableReport');
            if (!reportDiv) {
                reportDiv = document.createElement('div');
                reportDiv.id = 'printableReport';
                document.body.appendChild(reportDiv);
            }

            reportDiv.innerHTML = `
                <div style="font-family: Arial, sans-serif; padding: 20px; color: #1e293b;">
                    <div style="border-bottom: 3px solid #16583b; padding-bottom: 12px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end;">
                        <div>
                            <h1 style="font-size: 20px; font-weight: bold; color: #16583b; margin: 0;">SABAR FURNITURE & INTERIOR DESIGN</h1>
                            <p style="font-size: 12px; color: #64748b; margin: 3px 0 0 0;">Laporan Harian Konten & Produksi Kreatif</p>
                        </div>
                        <div style="text-align: right; font-size: 12px; color: #334155;">
                            <p style="margin: 0;"><b>Tanggal Laporan:</b> ${d} ${MONTH_NAMES[today.getMonth()]} ${y}</p>
                            <p style="margin: 2px 0 0 0;"><b>Pelapor:</b> ${escapeHtml(savedName)} (${escapeHtml(savedRole)})</p>
                        </div>
                    </div>

                    <h3 style="font-size: 14px; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 12px; color: #0f172a;">
                        📋 Daftar Tugas & Status Pekerjaan Hari Ini (${todayTasks.length} Tugas)
                    </h3>

                    ${todayTasks.length === 0 ? `
                        <p style="font-style: italic; color: #94a3b8; padding: 15px; border: 1px dashed #cbd5e1; border-radius: 8px;">
                            Tidak ada jadwal tugas yang di-input untuk hari ini.
                        </p>
                    ` : `
                        <table style="width: 100%; border-collapse: collapse; font-size: 12px; margin-bottom: 30px;">
                            <thead>
                                <tr style="background-color: #f1f5f9; text-align: left; border-bottom: 2px solid #cbd5e1;">
                                    <th style="padding: 8px; border: 1px solid #cbd5e1;">No</th>
                                    <th style="padding: 8px; border: 1px solid #cbd5e1;">Jenis Konten</th>
                                    <th style="padding: 8px; border: 1px solid #cbd5e1;">Keterangan / Detail Tugas</th>
                                    <th style="padding: 8px; border: 1px solid #cbd5e1;">Platform</th>
                                    <th style="padding: 8px; border: 1px solid #cbd5e1;">Status</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${todayTasks.map((t, idx) => `
                                    <tr>
                                        <td style="padding: 8px; border: 1px solid #cbd5e1; text-align: center;">${idx + 1}</td>
                                        <td style="padding: 8px; border: 1px solid #cbd5e1; font-weight: bold; color: #16583b;">${escapeHtml(t.contentType)}</td>
                                        <td style="padding: 8px; border: 1px solid #cbd5e1;">${escapeHtml(t.taskDetail)}</td>
                                        <td style="padding: 8px; border: 1px solid #cbd5e1;">${escapeHtml(t.platform)}</td>
                                        <td style="padding: 8px; border: 1px solid #cbd5e1; font-weight: bold; color: ${t.completed ? '#16a34a' : '#d97706'};">
                                            ${t.completed ? '✅ SELESAI' : '⏳ PENDING'}
                                        </td>
                                    </tr>
                                `).join('')}
                            </tbody>
                        </table>
                    `}

                    <div style="margin-top: 50px; display: flex; justify-content: space-between; padding: 0 40px; font-size: 12px;">
                        <div style="text-align: center;">
                            <p style="margin-bottom: 60px;">Pelapor / Content Creator,</p>
                            <p style="font-weight: bold; text-decoration: underline; margin: 0;">( ${escapeHtml(savedName)} )</p>
                        </div>
                        <div style="text-align: center;">
                            <p style="margin-bottom: 60px;">Mengetahui / Atasan,</p>
                            <p style="font-weight: bold; text-decoration: underline; margin: 0;">( Management Sabar Furniture )</p>
                        </div>
                    </div>
                </div>
            `;

            window.print();
        }

        function printMonthlyReport() {
            const monthName = MONTH_NAMES[currentMonth];
            const savedName = localStorage.getItem('sabar_creator_name') || 'Natan';
            const savedRole = localStorage.getItem('sabar_creator_role') || 'Content Creator & Graphic Design';

            // Sort tasks chronologically
            const monthlyTasks = [...scheduleData].sort((a, b) => a.dayNum - b.dayNum);
            const totalTasks = monthlyTasks.length;
            const completedTasks = monthlyTasks.filter(t => t.completed).length;
            const pendingTasks = totalTasks - completedTasks;
            const completionRate = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;

            // Breakdown by Content Type
            const categoryStats = {};
            CONTENT_TYPES_ROTATION.forEach(type => {
                categoryStats[type] = { total: 0, completed: 0, pending: 0 };
            });

            monthlyTasks.forEach(t => {
                if (!categoryStats[t.contentType]) {
                    categoryStats[t.contentType] = { total: 0, completed: 0, pending: 0 };
                }
                categoryStats[t.contentType].total++;
                if (t.completed) categoryStats[t.contentType].completed++;
                else categoryStats[t.contentType].pending++;
            });

            const categories = Object.keys(categoryStats);
            const completedData = categories.map(c => categoryStats[c].completed);
            const pendingData = categories.map(c => categoryStats[c].pending);

            // Generate high-res static chart image via offscreen canvas for printing
            const offscreenCanvas = document.createElement('canvas');
            offscreenCanvas.width = 750;
            offscreenCanvas.height = 280;
            const ctx = offscreenCanvas.getContext('2d');

            // Fill white background
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(0, 0, offscreenCanvas.width, offscreenCanvas.height);

            const tempChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: categories,
                    datasets: [
                        {
                            label: 'Selesai',
                            data: completedData,
                            backgroundColor: '#16a34a',
                            borderRadius: 4
                        },
                        {
                            label: 'Pending',
                            data: pendingData,
                            backgroundColor: '#f59e0b',
                            borderRadius: 4
                        }
                    ]
                },
                options: {
                    responsive: false,
                    animation: false,
                    plugins: {
                        legend: {
                            position: 'top',
                            labels: { font: { size: 12, weight: 'bold' } }
                        },
                        title: {
                            display: true,
                            text: `Grafik Pencapaian Konten Bulan ${monthName} ${currentYear}`,
                            font: { size: 14, weight: 'bold' },
                            color: '#0f172a'
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: { stepSize: 1, font: { size: 11 } }
                        },
                        x: {
                            ticks: { font: { size: 11, weight: 'bold' } }
                        }
                    }
                }
            });

            const chartImgBase64 = offscreenCanvas.toDataURL('image/png');
            tempChart.destroy();

            let reportDiv = document.getElementById('printableReport');
            if (!reportDiv) {
                reportDiv = document.createElement('div');
                reportDiv.id = 'printableReport';
                document.body.appendChild(reportDiv);
            }

            reportDiv.innerHTML = `
                <div style="font-family: Arial, sans-serif; padding: 20px; color: #1e293b;">
                    <!-- Header Laporan -->
                    <div style="border-bottom: 3px solid #16583b; padding-bottom: 12px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end;">
                        <div>
                            <h1 style="font-size: 20px; font-weight: bold; color: #16583b; margin: 0;">SABAR FURNITURE & INTERIOR DESIGN</h1>
                            <p style="font-size: 13px; color: #475569; margin: 4px 0 0 0; font-weight: bold;">LAPORAN RANGKUMAN PRODUKTIVITAS KONTEN BULANAN</p>
                        </div>
                        <div style="text-align: right; font-size: 12px; color: #334155;">
                            <p style="margin: 0;"><b>Periode Bulan:</b> ${monthName} ${currentYear}</p>
                            <p style="margin: 2px 0 0 0;"><b>Pelapor:</b> ${escapeHtml(savedName)} (${escapeHtml(savedRole)})</p>
                        </div>
                    </div>

                    <!-- Ringkasan KPI Executive -->
                    <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 20px;">
                        <div style="border: 1px solid #cbd5e1; border-radius: 8px; padding: 12px; text-align: center; background: #f8fafc;">
                            <p style="font-size: 10px; color: #64748b; text-transform: uppercase; margin: 0;">Total Target Konten</p>
                            <p style="font-size: 20px; font-weight: bold; color: #0f172a; margin: 4px 0 0 0;">${totalTasks}</p>
                        </div>
                        <div style="border: 1px solid #bbf7d0; border-radius: 8px; padding: 12px; text-align: center; background: #f0fdf4;">
                            <p style="font-size: 10px; color: #15803d; text-transform: uppercase; margin: 0;">Konten Selesai</p>
                            <p style="font-size: 20px; font-weight: bold; color: #16a34a; margin: 4px 0 0 0;">${completedTasks}</p>
                        </div>
                        <div style="border: 1px solid #fde68a; border-radius: 8px; padding: 12px; text-align: center; background: #fffbeb;">
                            <p style="font-size: 10px; color: #b45309; text-transform: uppercase; margin: 0;">Belum Selesai (Pending)</p>
                            <p style="font-size: 20px; font-weight: bold; color: #d97706; margin: 4px 0 0 0;">${pendingTasks}</p>
                        </div>
                        <div style="border: 1px solid #bae6fd; border-radius: 8px; padding: 12px; text-align: center; background: #f0f9ff;">
                            <p style="font-size: 10px; color: #0369a1; text-transform: uppercase; margin: 0;">Persentase Capaian</p>
                            <p style="font-size: 20px; font-weight: bold; color: #0284c7; margin: 4px 0 0 0;">${completionRate}%</p>
                        </div>
                    </div>

                    <!-- Visual Grafik Performa -->
                    <div style="border: 1px solid #cbd5e1; border-radius: 8px; padding: 15px; margin-bottom: 20px; text-align: center; background: #ffffff;">
                        <img src="${chartImgBase64}" style="max-width: 100%; height: auto; display: inline-block;" alt="Grafik Performa Bulanan" />
                    </div>

                    <!-- Tabel Rincian Per Kategori -->
                    <h3 style="font-size: 13px; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 8px; color: #16583b;">
                        📊 Rekapitulasi Per Kategori Konten
                    </h3>
                    <table style="width: 100%; border-collapse: collapse; font-size: 11px; margin-bottom: 24px;">
                        <thead>
                            <tr style="background-color: #f1f5f9; text-align: left;">
                                <th style="padding: 8px; border: 1px solid #cbd5e1;">Kategori Konten</th>
                                <th style="padding: 8px; border: 1px solid #cbd5e1; text-align: center;">Total Terjadwal</th>
                                <th style="padding: 8px; border: 1px solid #cbd5e1; text-align: center;">Selesai</th>
                                <th style="padding: 8px; border: 1px solid #cbd5e1; text-align: center;">Pending</th>
                                <th style="padding: 8px; border: 1px solid #cbd5e1; text-align: center;">Rasio Selesai</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${categories.map(cat => {
                                const st = categoryStats[cat];
                                const catRate = st.total > 0 ? Math.round((st.completed / st.total) * 100) : 0;
                                return `
                                    <tr>
                                        <td style="padding: 7px 8px; border: 1px solid #cbd5e1; font-weight: bold;">${escapeHtml(cat)}</td>
                                        <td style="padding: 7px 8px; border: 1px solid #cbd5e1; text-align: center;">${st.total}</td>
                                        <td style="padding: 7px 8px; border: 1px solid #cbd5e1; text-align: center; color: #16a34a; font-weight: bold;">${st.completed}</td>
                                        <td style="padding: 7px 8px; border: 1px solid #cbd5e1; text-align: center; color: #d97706;">${st.pending}</td>
                                        <td style="padding: 7px 8px; border: 1px solid #cbd5e1; text-align: center; font-weight: bold;">${catRate}%</td>
                                    </tr>
                                `;
                            }).join('')}
                        </tbody>
                    </table>

                    <!-- Tabel Daftar Lengkap Tugas 1 Bulan -->
                    <h3 style="font-size: 13px; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 8px; color: #16583b;">
                        📋 Lampiran Rincian Jadwal Tugas Bulan ${monthName} ${currentYear}
                    </h3>
                    ${monthlyTasks.length === 0 ? `
                        <p style="font-style: italic; color: #94a3b8; padding: 12px; border: 1px dashed #cbd5e1; border-radius: 8px; font-size: 12px;">
                            Belum ada data tugas yang tercatat pada bulan ${monthName} ${currentYear}.
                        </p>
                    ` : `
                        <table style="width: 100%; border-collapse: collapse; font-size: 11px; margin-bottom: 30px;">
                            <thead>
                                <tr style="background-color: #f1f5f9; text-align: left;">
                                    <th style="padding: 7px; border: 1px solid #cbd5e1; text-align: center; width: 35px;">No</th>
                                    <th style="padding: 7px; border: 1px solid #cbd5e1; width: 120px;">Tanggal</th>
                                    <th style="padding: 7px; border: 1px solid #cbd5e1; width: 130px;">Jenis Konten</th>
                                    <th style="padding: 7px; border: 1px solid #cbd5e1;">Keterangan / Detail Tugas</th>
                                    <th style="padding: 7px; border: 1px solid #cbd5e1; width: 100px;">Platform</th>
                                    <th style="padding: 7px; border: 1px solid #cbd5e1; text-align: center; width: 90px;">Status</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${monthlyTasks.map((t, idx) => `
                                    <tr>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1; text-align: center;">${idx + 1}</td>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1;">${t.dayName}, ${t.dayNum} ${monthName}</td>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1; font-weight: bold; color: #16583b;">${escapeHtml(t.contentType)}</td>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1;">${escapeHtml(t.taskDetail)}</td>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1;">${escapeHtml(t.platform)}</td>
                                        <td style="padding: 6px 7px; border: 1px solid #cbd5e1; text-align: center; font-weight: bold; color: ${t.completed ? '#16a34a' : '#d97706'};">
                                            ${t.completed ? 'SELESAI' : 'PENDING'}
                                        </td>
                                    </tr>
                                `).join('')}
                            </tbody>
                        </table>
                    `}

                    <!-- Tanda Tangan -->
                    <div style="margin-top: 40px; display: flex; justify-content: space-between; padding: 0 40px; font-size: 12px; page-break-inside: avoid;">
                        <div style="text-align: center;">
                            <p style="margin-bottom: 60px;">Dibuat Oleh / Content Creator,</p>
                            <p style="font-weight: bold; text-decoration: underline; margin: 0;">( ${escapeHtml(savedName)} )</p>
                        </div>
                        <div style="text-align: center;">
                            <p style="margin-bottom: 60px;">Diperiksa & Disetujui Oleh,</p>
                            <p style="font-weight: bold; text-decoration: underline; margin: 0;">( Management Sabar Furniture )</p>
                        </div>
                    </div>
                </div>
            `;

            // Slight delay to ensure base64 image paints cleanly before opening print dialog
            setTimeout(() => {
                window.print();
            }, 150);
        }

        function escapeHtml(str) {
            return (str || '')
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
