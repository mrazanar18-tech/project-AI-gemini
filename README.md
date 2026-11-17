# project-AI-gemini
```html
<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Analisis Burnout Siswa — SMA Fullday Al-Muhajirin</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Font Awesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        /* Pengaturan khusus untuk mode cetak (Print) */
        @media print {
            body {
                background-color: #ffffff !important;
                color: #0f172a !important;
            }
            .no-print {
                display: none !important;
            }
            .print-shadow-none {
                box-shadow: none !important;
                border: 1px solid #e2e8f0 !important;
            }
            .print-break-inside {
                break-inside: avoid;
            }
            canvas {
                max-width: 100% !important;
                height: auto !important;
            }
        }
        /* Custom scrollbar untuk tabel responsif */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans antialiased selection:bg-emerald-200 selection:text-emerald-900">

    <!-- 1. Header Profesional -->
    <header class="bg-gradient-to-r from-emerald-900 via-teal-800 to-emerald-800 text-white shadow-lg border-b border-emerald-700/50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 md:py-8">
            <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-6">
                <!-- Identitas Sekolah & Judul Laporan -->
                <div class="flex items-start sm:items-center gap-4">
                    <div class="bg-white/15 backdrop-blur-sm p-3.5 rounded-2xl border border-white/20 shrink-0 shadow-inner">
                        <i class="fa-solid fa-graduation-cap text-2xl sm:text-3xl text-emerald-200"></i>
                    </div>
                    <div>
                        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-emerald-950/60 border border-emerald-500/30 text-emerald-200 text-xs font-semibold tracking-wide uppercase mb-2">
                            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                            Interactive Analytics Dashboard
                        </div>
                        <h1 class="text-xl sm:text-2xl md:text-3xl font-extrabold tracking-tight text-white leading-snug">
                            Dashboard Analisis Burnout Siswa — SMA Fullday Al-Muhajirin
                        </h1>
                        <p id="headerSubtitle" class="mt-1.5 text-sm text-emerald-100/95 font-medium">
                            Survei 7 Indikator Aspek Penilaian • N = 60 Responden • Periode 12–13 November 2026
                        </p>
                    </div>
                </div>

                <!-- Tombol Aksi -->
                <div class="flex flex-wrap items-center gap-2.5 no-print shrink-0">
                    <!-- Unggah CSV Baru -->
                    <input type="file" id="csvFileInput" accept=".csv" class="hidden" onchange="handleCSVUpload(event)">
                    <button onclick="document.getElementById('csvFileInput').click()" class="inline-flex items-center justify-center gap-2 px-3.5 py-2.5 rounded-xl bg-white/10 hover:bg-white/20 active:bg-white/25 text-white font-medium text-xs sm:text-sm border border-white/25 transition-all duration-200 cursor-pointer shadow-sm" title="Muat file CSV baru">
                        <i class="fa-solid fa-upload text-emerald-300"></i>
                        <span>Unggah CSV</span>
                    </button>

                    <!-- Ekspor Data CSV -->
                    <button onclick="exportFilteredCSV()" class="inline-flex items-center justify-center gap-2 px-3.5 py-2.5 rounded-xl bg-white/10 hover:bg-white/20 active:bg-white/25 text-white font-medium text-xs sm:text-sm border border-white/25 transition-all duration-200 cursor-pointer shadow-sm" title="Unduh data sesuai filter aktif">
                        <i class="fa-solid fa-file-csv text-emerald-300"></i>
                        <span>Ekspor CSV</span>
                    </button>

                    <!-- Cetak / Simpan PDF -->
                    <button onclick="window.print()" class="inline-flex items-center justify-center gap-2 px-4 py-2.5 rounded-xl bg-emerald-400 hover:bg-emerald-300 active:bg-emerald-500 text-slate-950 font-bold text-xs sm:text-sm transition-all duration-200 cursor-pointer shadow-md hover:shadow-lg">
                        <i class="fa-solid fa-print"></i>
                        <span>Cetak / PDF</span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8 space-y-6 sm:space-y-8">

        <!-- 2. Filter Global Interaktif -->
        <section aria-label="Filter Global Dashboard" class="bg-white rounded-2xl p-4 sm:p-5 shadow-sm border border-slate-200/90 no-print sticky top-3 z-20 backdrop-blur-md bg-white/95">
            <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4">
                <div class="flex items-center gap-2.5">
                    <div class="w-8 h-8 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center border border-emerald-200/70 shrink-0">
                        <i class="fa-solid fa-sliders text-sm"></i>
                    </div>
                    <div>
                        <h2 class="text-sm font-bold text-slate-900">Filter Global Real-Time</h2>
                        <p class="text-[11px] text-slate-500">Semua KPI, grafik, dan tabel otomatis menyesuaikan filter</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 lg:flex lg:items-center gap-3 flex-1 lg:justify-end">
                    <!-- Filter Tanggal -->
                    <div class="min-w-[175px]">
                        <label for="globalDateFilter" class="sr-only">Filter Tanggal</label>
                        <select id="globalDateFilter" class="w-full px-3 py-2 text-xs sm:text-sm font-medium bg-slate-50 border border-slate-300 rounded-xl text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:bg-white transition cursor-pointer">
                            <option value="ALL">Semua Tanggal</option>
                            <option value="2026-11-12">2026-11-12</option>
                            <option value="2026-11-13">2026-11-13</option>
                        </select>
                    </div>

                    <!-- Filter Kategori Skor -->
                    <div class="min-w-[205px]">
                        <label for="globalCategoryFilter" class="sr-only">Filter Kategori Skor</label>
                        <select id="globalCategoryFilter" class="w-full px-3 py-2 text-xs sm:text-sm font-medium bg-slate-50 border border-slate-300 rounded-xl text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:bg-white transition cursor-pointer">
                            <option value="ALL">Semua Kategori</option>
                            <option value="Tinggi">Tinggi (≥ 3.00)</option>
                            <option value="Sedang">Sedang (2.00 – 2.99)</option>
                            <option value="Rendah">Rendah (&lt; 2.00)</option>
                        </select>
                    </div>

                    <!-- Pencarian ID Responden -->
                    <div class="relative min-w-[180px]">
                        <label for="globalSearchInput" class="sr-only">Pencarian ID Responden</label>
                        <i class="fa-solid fa-magnifying-glass text-slate-400 absolute left-3 top-1/2 -translate-y-1/2 text-xs"></i>
                        <input type="text" id="globalSearchInput" placeholder="Cari ID (misal R23)..." class="w-full pl-8 pr-3 py-2 text-xs sm:text-sm bg-slate-50 border border-slate-300 rounded-xl text-slate-800 focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:bg-white transition">
                    </div>

                    <!-- Tombol Reset Filter -->
                    <button onclick="resetAllFilters()" id="resetGlobalBtn" class="inline-flex items-center justify-center gap-1.5 px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-200 transition cursor-pointer shrink-0">
                        <i class="fa-solid fa-rotate-left text-xs"></i>
                        <span>Reset Filter</span>
                    </button>
                </div>
            </div>
        </section>

        <!-- 3. 4 Kartu KPI (Statistik Utama Real-Time) -->
        <section aria-label="Statistik Utama">
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                <!-- KPI 1: Total Responden -->
                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 hover:border-emerald-300 transition-all duration-200 print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-500">Total Responden</span>
                            <div class="w-10 h-10 rounded-xl bg-teal-50 text-teal-600 flex items-center justify-center border border-teal-100">
                                <i class="fa-solid fa-users text-lg"></i>
                            </div>
                        </div>
                        <div class="flex items-baseline gap-2">
                            <h3 id="kpiTotalValue" class="text-3xl font-extrabold text-slate-900">60</h3>
                            <span id="kpiTotalPct" class="text-xs font-bold text-teal-700 bg-teal-50 px-2 py-0.5 rounded-md border border-teal-100">Siswa (100%)</span>
                        </div>
                    </div>
                    <div id="kpiDateBreakdown" class="mt-4 pt-3 border-t border-slate-100 flex items-center justify-between text-xs text-slate-600">
                        <span>12 Nov: <strong class="text-slate-800">40</strong></span>
                        <span class="text-slate-300">|</span>
                        <span>13 Nov: <strong class="text-slate-800">20</strong></span>
                    </div>
                </div>

                <!-- KPI 2: Rata-Rata Skor Burnout -->
                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 hover:border-emerald-300 transition-all duration-200 print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-500">Rata-Rata Skor</span>
                            <div class="w-10 h-10 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center border border-blue-100">
                                <i class="fa-solid fa-chart-simple text-lg"></i>
                            </div>
                        </div>
                        <div class="flex items-baseline gap-2">
                            <h3 id="kpiAvgValue" class="text-3xl font-extrabold text-slate-900">2.72</h3>
                            <span class="text-xs font-medium text-slate-500">/ 4.00 skala</span>
                        </div>
                    </div>
                    <div class="mt-4 pt-3 border-t border-slate-100 flex items-center justify-between text-xs text-slate-600">
                        <span>SD: <strong id="kpiStdDev" class="text-slate-800">0.36</strong></span>
                        <span class="text-slate-300">•</span>
                        <span>Median: <strong id="kpiMedian" class="text-slate-800">2.71</strong></span>
                    </div>
                </div>

                <!-- KPI 3: Skor Tertinggi & Terendah -->
                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 hover:border-emerald-300 transition-all duration-200 print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-500">Skor Ekstrem</span>
                            <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center border border-indigo-100">
                                <i class="fa-solid fa-arrows-up-down text-lg"></i>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-2.5">
                            <div id="kpiMaxCard" onclick="openRespondentModalById(this.dataset.respId)" data-resp-id="R23" class="bg-rose-50/80 hover:bg-rose-100/80 border border-rose-100 rounded-xl p-2.5 cursor-pointer transition" title="Klik untuk detail responden">
                                <span class="text-[11px] font-semibold text-rose-600 block">Tertinggi (Max)</span>
                                <div class="flex items-baseline justify-between mt-0.5">
                                    <span id="kpiMaxScore" class="text-lg font-extrabold text-rose-700">3.43</span>
                                    <span id="kpiMaxId" class="text-[11px] font-bold bg-rose-200/80 text-rose-900 px-1.5 py-0.5 rounded">R23</span>
                                </div>
                            </div>
                            <div id="kpiMinCard" onclick="openRespondentModalById(this.dataset.respId)" data-resp-id="R25" class="bg-emerald-50/80 hover:bg-emerald-100/80 border border-emerald-100 rounded-xl p-2.5 cursor-pointer transition" title="Klik untuk detail responden">
                                <span class="text-[11px] font-semibold text-emerald-700 block">Terendah (Min)</span>
                                <div class="flex items-baseline justify-between mt-0.5">
                                    <span id="kpiMinScore" class="text-lg font-extrabold text-emerald-800">1.86</span>
                                    <span id="kpiMinId" class="text-[11px] font-bold bg-emerald-200/80 text-emerald-900 px-1.5 py-0.5 rounded">R25</span>
                                </div>
                            </div>
                        </div>
                    </div>
                    <p class="text-[11px] text-slate-500 mt-2.5">Klik kartu Max/Min untuk melihat profil jawaban</p>
                </div>

                <!-- KPI 4: Proporsi Kategori Risiko -->
                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 hover:border-emerald-300 transition-all duration-200 print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-500">Kategori Risiko</span>
                            <div class="w-10 h-10 rounded-xl bg-amber-50 text-amber-600 flex items-center justify-center border border-amber-100">
                                <i class="fa-solid fa-layer-group text-lg"></i>
                            </div>
                        </div>
                        <div class="space-y-2 text-xs">
                            <div class="flex items-center justify-between">
                                <span class="flex items-center gap-1.5 font-medium text-slate-700">
                                    <span class="w-2.5 h-2.5 rounded-full bg-red-500"></span> Tinggi (&ge;3.00)
                                </span>
                                <span id="kpiCatHigh" class="font-bold text-slate-900">19 siswa (31.7%)</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="flex items-center gap-1.5 font-medium text-slate-700">
                                    <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span> Sedang (2.00–2.99)
                                </span>
                                <span id="kpiCatMed" class="font-bold text-slate-900">40 siswa (66.7%)</span>
                            </div>
                            <div class="flex items-center justify-between">
                                <span class="flex items-center gap-1.5 font-medium text-slate-700">
                                    <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span> Rendah (&lt;2.00)
                                </span>
                                <span id="kpiCatLow" class="font-bold text-slate-900">1 siswa (1.7%)</span>
                            </div>
                        </div>
                    </div>
                    <!-- Mini stacked progress bar -->
                    <div class="w-full h-2 rounded-full bg-slate-100 overflow-hidden flex mt-3">
                        <div id="barHigh" class="bg-red-500 transition-all duration-300" style="width: 31.7%"></div>
                        <div id="barMed" class="bg-amber-500 transition-all duration-300" style="width: 66.7%"></div>
                        <div id="barLow" class="bg-emerald-500 transition-all duration-300" style="width: 1.7%"></div>
                    </div>
                </div>
            </div>
        </section>

        <!-- 4. Visualisasi Grafik Interaktif (Chart.js Grid 12 Kolom) -->
        <section aria-label="Visualisasi Grafik Interaktif" class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <!-- Grafik 1 (Lebar 8 kolom): Distribusi Jawaban per Indikator (Q1 - Q7) -->
            <div class="lg:col-span-8 bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 print-shadow-none print-break-inside flex flex-col justify-between">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-5">
                    <div>
                        <h3 class="text-base sm:text-lg font-bold text-slate-900">Distribusi Jawaban per Indikator (Q1 – Q7)</h3>
                        <p class="text-xs text-slate-500 mt-0.5">Sebaran skala STS, TS, S, dan SS pada setiap aspek penilaian</p>
                    </div>
                    <!-- Controls Toggle: Stacked vs Grouped & Jumlah vs Persentase -->
                    <div class="flex flex-wrap items-center gap-2 no-print">
                        <!-- Toggle Layout Bar -->
                        <div class="inline-flex rounded-xl bg-slate-100 p-1 border border-slate-200/60">
                            <button id="btnBarStacked" onclick="setBarLayout(true)" class="px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-slate-900 shadow-sm transition cursor-pointer">
                                Stacked Bar
                            </button>
                            <button id="btnBarGrouped" onclick="setBarLayout(false)" class="px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer">
                                Grouped Bar
                            </button>
                        </div>

                        <!-- Toggle Unit Nilai -->
                        <div class="inline-flex rounded-xl bg-slate-100 p-1 border border-slate-200/60">
                            <button id="btnUnitCount" onclick="setBarUnit('count')" class="px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-emerald-700 shadow-sm transition cursor-pointer">
                                Jumlah (N)
                            </button>
                            <button id="btnUnitPercent" onclick="setBarUnit('percent')" class="px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer">
                                Persentase (%)
                            </button>
                        </div>
                    </div>
                </div>
                <div class="relative w-full h-72 sm:h-80">
                    <canvas id="indicatorChart"></canvas>
                </div>
            </div>

            <!-- Grafik 2 (Lebar 4 kolom): Proporsi Kategori Burnout (Doughnut Chart) -->
            <div class="lg:col-span-4 bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 print-shadow-none print-break-inside flex flex-col justify-between">
                <div>
                    <h3 class="text-base sm:text-lg font-bold text-slate-900">Proporsi Kategori Burnout</h3>
                    <p class="text-xs text-slate-500 mt-0.5">Klasifikasi skor rata-rata responden aktif</p>
                </div>
                <div class="relative w-full h-64 my-3 flex items-center justify-center">
                    <canvas id="categoryDoughnutChart"></canvas>
                </div>
                <div id="doughnutNote" class="bg-slate-50 rounded-xl p-3 border border-slate-100 text-xs text-slate-600 text-center">
                    Arahkan kursor pada grafik untuk melihat persentase detail.
                </div>
            </div>

            <!-- Grafik 3 (Lebar 12 kolom): Profil & Tren Skor Rata-Rata Seluruh Responden (R1 - R60) -->
            <div class="lg:col-span-12 bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 print-shadow-none print-break-inside">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3 mb-5">
                    <div>
                        <h3 class="text-base sm:text-lg font-bold text-slate-900">Profil &amp; Tren Skor Rata-Rata Responden</h3>
                        <p class="text-xs text-slate-500 mt-0.5">
                            <span class="inline-flex items-center gap-1 text-emerald-700 font-semibold"><i class="fa-solid fa-hand-pointer"></i> Klik titik mana saja pada grafik</span> untuk membuka modal detail jawaban responden tersebut.
                        </p>
                    </div>
                    <div class="flex flex-wrap items-center gap-3 text-xs">
                        <span class="inline-flex items-center gap-1.5 text-slate-600">
                            <span class="w-3 h-3 rounded-full bg-red-500 inline-block"></span> Tinggi (&ge; 3.00)
                        </span>
                        <span class="inline-flex items-center gap-1.5 text-slate-600">
                            <span class="w-3 h-3 rounded-full bg-amber-500 inline-block"></span> Sedang (2.00–2.99)
                        </span>
                        <span class="inline-flex items-center gap-1.5 text-slate-600">
                            <span class="w-3 h-3 rounded-full bg-emerald-500 inline-block"></span> Rendah (&lt; 2.00)
                        </span>
                    </div>
                </div>
                <div class="relative w-full h-72 sm:h-80">
                    <canvas id="respondentTrendChart"></canvas>
                </div>
            </div>
        </section>

        <!-- 5. Ringkasan Temuan Utama (3 Insight Cards) -->
        <section aria-label="Ringkasan Temuan Utama">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Insight 1: Persetujuan Tertinggi Q4 -->
                <div class="bg-gradient-to-br from-emerald-50 to-teal-50/60 rounded-2xl p-5 border border-emerald-200/90 shadow-sm print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between gap-2 mb-3">
                            <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-[11px] font-bold bg-emerald-600 text-white">
                                <i class="fa-solid fa-circle-check"></i> Persetujuan Tertinggi
                            </span>
                            <span class="text-xs font-bold text-emerald-900 bg-emerald-200/70 px-2 py-0.5 rounded-md">Q4</span>
                        </div>
                        <h3 class="text-base font-bold text-slate-900 mb-1.5">
                            Indikator 4 (Q4) — 91.7% Setuju
                        </h3>
                        <p class="text-xs sm:text-sm text-slate-700 leading-relaxed">
                            Sebanyak <strong>55 dari 60 siswa (91.7%)</strong> menyatakan setuju (<strong>50 S + 5 SS</strong>) pada Indikator 4, dengan <strong>0% (0 responden)</strong> memilih STS. Indikator ini merupakan aspek yang paling konsisten dirasakan siswa.
                        </p>
                    </div>
                    <div class="mt-4 pt-2.5 border-t border-emerald-200/70 flex items-center justify-between text-[11px] font-semibold text-emerald-900">
                        <span>S + SS: 55 (91.7%)</span>
                        <span>TS: 5 (8.3%)</span>
                        <span>STS: 0 (0%)</span>
                    </div>
                </div>

                <!-- Insight 2: Penolakan Tertinggi Q2 -->
                <div class="bg-gradient-to-br from-amber-50 to-orange-50/60 rounded-2xl p-5 border border-amber-200/90 shadow-sm print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between gap-2 mb-3">
                            <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-[11px] font-bold bg-amber-600 text-white">
                                <i class="fa-solid fa-triangle-exclamation"></i> Penolakan Tertinggi
                            </span>
                            <span class="text-xs font-bold text-amber-950 bg-amber-200/70 px-2 py-0.5 rounded-md">Q2</span>
                        </div>
                        <h3 class="text-base font-bold text-slate-900 mb-1.5">
                            Indikator 2 (Q2) — 71.7% Tidak Setuju
                        </h3>
                        <p class="text-xs sm:text-sm text-slate-700 leading-relaxed">
                            Sebanyak <strong>43 dari 60 siswa (71.7%)</strong> menolak pernyataan pada Indikator 2 (<strong>18 STS + 25 TS</strong>). Mayoritas siswa tidak mengalami gejala atau kendala spesifik pada aspek penilaian ini.
                        </p>
                    </div>
                    <div class="mt-4 pt-2.5 border-t border-amber-200/70 flex items-center justify-between text-[11px] font-semibold text-amber-950">
                        <span>STS + TS: 43 (71.7%)</span>
                        <span>S: 11 (18.3%)</span>
                        <span>SS: 6 (10.0%)</span>
                    </div>
                </div>

                <!-- Insight 3: Perbandingan Waktu Pengisian -->
                <div class="bg-gradient-to-br from-blue-50 to-indigo-50/60 rounded-2xl p-5 border border-blue-200/90 shadow-sm print-shadow-none print-break-inside flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between gap-2 mb-3">
                            <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-[11px] font-bold bg-blue-600 text-white">
                                <i class="fa-regular fa-clock"></i> Distribusi Waktu Survei
                            </span>
                            <span class="text-xs font-bold text-blue-950 bg-blue-200/70 px-2 py-0.5 rounded-md">2 Gelombang</span>
                        </div>
                        <h3 class="text-base font-bold text-slate-900 mb-1.5">
                            Malam 12 Nov vs Pagi–Sore 13 Nov
                        </h3>
                        <p class="text-xs sm:text-sm text-slate-700 leading-relaxed">
                            Gelombang 1 pada <strong>Malam 12 Nov 2026</strong> mencakup <strong>40 responden (66.7%)</strong> dengan rata-rata skor <strong>2.66</strong>, sedangkan Gelombang 2 pada <strong>Pagi–Sore 13 Nov 2026</strong> mencakup <strong>20 responden (33.3%)</strong> dengan rata-rata <strong>2.83</strong>.
                        </p>
                    </div>
                    <div class="mt-4 pt-2.5 border-t border-blue-200/70 flex items-center justify-between text-[11px] font-semibold text-blue-950">
                        <span>12 Nov: 40 Siswa (Avg 2.66)</span>
                        <span>13 Nov: 20 Siswa (Avg 2.83)</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- 6. Tabel Data Responden Interaktif -->
        <section aria-label="Tabel Data Responden Interaktif" class="bg-white rounded-2xl shadow-sm border border-slate-200/80 print-shadow-none overflow-hidden">
            <!-- Header Tabel -->
            <div class="p-5 sm:p-6 border-b border-slate-200/80 bg-slate-50/70 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <div>
                    <h3 class="text-base sm:text-lg font-bold text-slate-900 flex items-center gap-2">
                        <i class="fa-solid fa-table-list text-emerald-600"></i>
                        Tabel Data Responden Interaktif
                    </h3>
                    <p class="text-xs text-slate-500 mt-1">
                        Klik header kolom untuk mengurutkan data, atau <strong class="text-slate-700">klik baris responden</strong> untuk membuka modal profil detail.
                    </p>
                </div>

                <!-- Legenda Skala & Status Jumlah Data -->
                <div class="flex flex-col sm:items-end gap-2">
                    <div id="tableCountSummary" class="text-xs font-medium text-slate-600">
                        Menampilkan <strong class="text-slate-900">60</strong> dari <strong class="text-slate-900">60</strong> responden
                    </div>
                    <div class="flex flex-wrap items-center gap-1.5 text-[11px]">
                        <span class="px-2 py-0.5 rounded font-bold bg-red-100 text-red-800 border border-red-200">STS = 1</span>
                        <span class="px-2 py-0.5 rounded font-bold bg-amber-100 text-amber-800 border border-amber-200">TS = 2</span>
                        <span class="px-2 py-0.5 rounded font-bold bg-blue-100 text-blue-800 border border-blue-200">S = 3</span>
                        <span class="px-2 py-0.5 rounded font-bold bg-emerald-100 text-emerald-800 border border-emerald-200">SS = 4</span>
                    </div>
                </div>
            </div>

            <!-- Responsive Table Wrapper -->
            <div class="overflow-x-auto max-h-[580px]">
                <table class="w-full text-left border-collapse">
                    <thead class="bg-slate-100 text-slate-700 text-xs font-bold uppercase tracking-wider sticky top-0 z-10 shadow-sm">
                        <tr>
                            <th onclick="sortTable('Responden')" class="py-3.5 px-4 border-b border-slate-200 cursor-pointer hover:bg-slate-200/80 transition select-none whitespace-nowrap">
                                <div class="flex items-center gap-1.5">
                                    <span>Responden</span>
                                    <i id="sort-Responden" class="fa-solid fa-sort-up text-emerald-600"></i>
                                </div>
                            </th>
                            <th class="py-3.5 px-4 border-b border-slate-200 whitespace-nowrap">Tanggal</th>
                            <th onclick="sortTable('Waktu')" class="py-3.5 px-4 border-b border-slate-200 cursor-pointer hover:bg-slate-200/80 transition select-none whitespace-nowrap">
                                <div class="flex items-center gap-1.5">
                                    <span>Waktu</span>
                                    <i id="sort-Waktu" class="fa-solid fa-sort text-slate-400"></i>
                                </div>
                            </th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q1</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q2</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q3</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q4</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q5</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q6</th>
                            <th class="py-3.5 px-2.5 border-b border-slate-200 text-center">Q7</th>
                            <th onclick="sortTable('Skor_Rata_Rata')" class="py-3.5 px-4 border-b border-slate-200 text-right cursor-pointer hover:bg-slate-200/80 transition select-none whitespace-nowrap">
                                <div class="flex items-center justify-end gap-1.5">
                                    <span>Skor Rata-Rata</span>
                                    <i id="sort-Skor_Rata_Rata" class="fa-solid fa-sort text-slate-400"></i>
                                </div>
                            </th>
                            <th class="py-3.5 px-4 border-b border-slate-200 text-center whitespace-nowrap">Status Kategori</th>
                        </tr>
                    </thead>
                    <tbody id="tableBody" class="divide-y divide-slate-100 text-sm">
                        <!-- Baris data dirender secara dinamis melalui JavaScript -->
                    </tbody>
                </table>
            </div>

            <!-- Empty State jika filter menghasilkan 0 data -->
            <div id="emptyState" class="hidden py-12 px-4 text-center">
                <div class="w-12 h-12 rounded-full bg-slate-100 text-slate-400 flex items-center justify-center mx-auto mb-3">
                    <i class="fa-solid fa-filter-circle-xmark text-xl"></i>
                </div>
                <h4 class="text-base font-bold text-slate-800">Tidak ada data responden ditemukan</h4>
                <p class="text-xs text-slate-500 mt-1">Ubah kriteria filter tanggal, kategori, atau kata kunci pencarian ID responden.</p>
                <button onclick="resetAllFilters()" class="mt-4 px-4 py-2 rounded-xl bg-emerald-600 text-white text-xs font-semibold hover:bg-emerald-700 transition cursor-pointer">
                    Reset Semua Filter
                </button>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 mt-4 border-t border-slate-200/80 text-center sm:flex sm:items-center sm:justify-between text-xs text-slate-500">
        <p>&copy; 2026 SMA Fullday Al-Muhajirin — Dashboard Analisis Burnout Siswa.</p>
        <p id="footerDataSource" class="mt-1 sm:mt-0">Sumber Data: Dataset Survei Burnout (N = 60 Responden)</p>
    </footer>

    <!-- Modal Detail Responden -->
    <div id="respondentModal" class="fixed inset-0 z-50 hidden items-center justify-center p-4 bg-slate-900/60 backdrop-blur-xs no-print" onclick="closeModalOnBackdrop(event)">
        <div class="bg-white rounded-2xl max-w-2xl w-full shadow-2xl border border-slate-200 overflow-hidden transform transition-all">
            <!-- Modal Header -->
            <div class="bg-gradient-to-r from-emerald-900 to-teal-800 px-6 py-4 text-white flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-white/15 flex items-center justify-center font-extrabold text-base border border-white/20" id="modalBadgeId">
                        R1
                    </div>
                    <div>
                        <h3 class="text-lg font-bold leading-tight" id="modalTitle">Profil Detail Responden R1</h3>
                        <p class="text-xs text-emerald-100 mt-0.5" id="modalTimestamp">Tanggal: 2026-11-12 • Waktu: 20:41:10</p>
                    </div>
                </div>
                <button onclick="closeRespondentModal()" class="w-8 h-8 rounded-lg bg-white/10 hover:bg-white/20 flex items-center justify-center text-white transition cursor-pointer" aria-label="Tutup Modal">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Modal Body -->
            <div class="p-6 space-y-5 max-h-[80vh] overflow-y-auto">
                <!-- Ringkasan Skor Responden -->
                <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 bg-slate-50 p-4 rounded-xl border border-slate-200/80">
                    <div>
                        <span class="text-[11px] font-semibold uppercase text-slate-500 block">Skor Rata-Rata</span>
                        <span id="modalScore" class="text-2xl font-extrabold text-slate-900">2.57</span>
                        <span class="text-xs text-slate-500">/ 4.00</span>
                    </div>
                    <div>
                        <span class="text-[11px] font-semibold uppercase text-slate-500 block">Status Kategori</span>
                        <div id="modalCategoryBadge" class="mt-1"></div>
                    </div>
                    <div class="col-span-2 sm:col-span-1">
                        <span class="text-[11px] font-semibold uppercase text-slate-500 block">Perbandingan Rata-Rata</span>
                        <span id="modalComparison" class="text-xs font-semibold text-slate-700 mt-1 block">-0.15 di bawah rata-rata</span>
                    </div>
                </div>

                <!-- Rincian 7 Indikator -->
                <div>
                    <h4 class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-3">Rincian Jawaban 7 Indikator Aspek Penilaian</h4>
                    <div id="modalIndicatorsList" class="space-y-2.5">
                        <!-- Diisi melalui JavaScript -->
                    </div>
                </div>
            </div>

            <!-- Modal Footer -->
            <div class="px-6 py-3.5 bg-slate-50 border-t border-slate-200 flex items-center justify-between">
                <div class="flex items-center gap-2">
                    <button id="modalPrevBtn" onclick="navigateModalRespondent(-1)" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-white border border-slate-300 hover:bg-slate-100 text-slate-700 transition cursor-pointer">
                        <i class="fa-solid fa-chevron-left mr-1"></i> Sebelumnya
                    </button>
                    <button id="modalNextBtn" onclick="navigateModalRespondent(1)" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-white border border-slate-300 hover:bg-slate-100 text-slate-700 transition cursor-pointer">
                        Berikutnya <i class="fa-solid fa-chevron-right ml-1"></i>
                    </button>
                </div>
                <button onclick="closeRespondentModal()" class="px-4 py-1.5 rounded-lg text-xs font-bold bg-emerald-600 hover:bg-emerald-700 text-white transition cursor-pointer">
                    Tutup
                </button>
            </div>
        </div>
    </div>

    <!-- Notifikasi Toast Non-Blocking -->
    <div id="toastNotification" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none no-print">
        <div class="bg-slate-900 text-white px-4 py-3 rounded-xl shadow-xl border border-slate-700 flex items-center gap-3 text-sm">
            <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400"></i>
            <span id="toastMessage">Aksi berhasil dilakukan.</span>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        // Dataset JSON 60 Responden Default (Persis sesuai spesifikasi)
        const DEFAULT_DATASET = [
            {"Responden": "R1", "Tanggal": "2026-11-12", "Waktu": "20:41:10", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.57},
            {"Responden": "R2", "Tanggal": "2026-11-12", "Waktu": "20:42:15", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R3", "Tanggal": "2026-11-12", "Waktu": "20:43:05", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "STS", "Indikator 4 (Q4) — Aspek Penilaian 4": "TS", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.0},
            {"Responden": "R4", "Tanggal": "2026-11-12", "Waktu": "20:44:12", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "TS", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "TS", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.29},
            {"Responden": "R5", "Tanggal": "2026-11-12", "Waktu": "20:45:30", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R6", "Tanggal": "2026-11-12", "Waktu": "20:46:18", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "TS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "STS", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.0},
            {"Responden": "R7", "Tanggal": "2026-11-12", "Waktu": "20:47:05", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R8", "Tanggal": "2026-11-12", "Waktu": "20:48:40", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.57},
            {"Responden": "R9", "Tanggal": "2026-11-12", "Waktu": "20:49:11", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R10", "Tanggal": "2026-11-12", "Waktu": "20:50:02", "Indikator 1 (Q1) — Aspek Penilaian 1": "STS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "STS", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 2.29},
            {"Responden": "R11", "Tanggal": "2026-11-12", "Waktu": "20:51:19", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.29},
            {"Responden": "R12", "Tanggal": "2026-11-12", "Waktu": "20:52:08", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "STS", "Indikator 6 (Q6) — Aspek Penilaian 6": "SS", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 2.86},
            {"Responden": "R13", "Tanggal": "2026-11-12", "Waktu": "20:52:45", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "TS", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.29},
            {"Responden": "R14", "Tanggal": "2026-11-12", "Waktu": "20:53:01", "Indikator 1 (Q1) — Aspek Penilaian 1": "STS", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.29},
            {"Responden": "R15", "Tanggal": "2026-11-12", "Waktu": "20:53:30", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R16", "Tanggal": "2026-11-12", "Waktu": "20:54:12", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "TS", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.57},
            {"Responden": "R17", "Tanggal": "2026-11-12", "Waktu": "20:55:04", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "TS", "Indikator 7 (Q7) — Aspek Penilaian 7": "TS", "Skor_Rata_Rata": 2.71},
            {"Responden": "R18", "Tanggal": "2026-11-12", "Waktu": "20:56:19", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R19", "Tanggal": "2026-11-12", "Waktu": "20:57:00", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "SS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.14},
            {"Responden": "R20", "Tanggal": "2026-11-12", "Waktu": "20:58:22", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R21", "Tanggal": "2026-11-12", "Waktu": "20:59:10", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "SS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "STS", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.14},
            {"Responden": "R22", "Tanggal": "2026-11-12", "Waktu": "21:02:14", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 3.0},
            {"Responden": "R23", "Tanggal": "2026-11-12", "Waktu": "21:05:33", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "SS", "Indikator 6 (Q6) — Aspek Penilaian 6": "SS", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 3.43},
            {"Responden": "R24", "Tanggal": "2026-11-12", "Waktu": "21:10:45", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 3.14},
            {"Responden": "R25", "Tanggal": "2026-11-12", "Waktu": "21:15:20", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "STS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "STS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 1.86},
            {"Responden": "R26", "Tanggal": "2026-11-12", "Waktu": "21:18:09", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.43},
            {"Responden": "R27", "Tanggal": "2026-11-12", "Waktu": "21:20:41", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R28", "Tanggal": "2026-11-12", "Waktu": "21:22:15", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 3.14},
            {"Responden": "R29", "Tanggal": "2026-11-12", "Waktu": "21:25:50", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "TS", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 2.86},
            {"Responden": "R30", "Tanggal": "2026-11-12", "Waktu": "21:28:12", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R31", "Tanggal": "2026-11-12", "Waktu": "21:31:05", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.29},
            {"Responden": "R32", "Tanggal": "2026-11-12", "Waktu": "21:35:40", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.57},
            {"Responden": "R33", "Tanggal": "2026-11-12", "Waktu": "21:42:18", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "SS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R34", "Tanggal": "2026-11-12", "Waktu": "21:50:03", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R35", "Tanggal": "2026-11-12", "Waktu": "21:58:44", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.43},
            {"Responden": "R36", "Tanggal": "2026-11-12", "Waktu": "22:04:19", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "TS", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.29},
            {"Responden": "R37", "Tanggal": "2026-11-12", "Waktu": "22:09:51", "Indikator 1 (Q1) — Aspek Penilaian 1": "SS", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.29},
            {"Responden": "R38", "Tanggal": "2026-11-12", "Waktu": "22:16:30", "Indikator 1 (Q1) — Aspek Penilaian 1": "STS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.14},
            {"Responden": "R39", "Tanggal": "2026-11-12", "Waktu": "22:28:11", "Indikator 1 (Q1) — Aspek Penilaian 1": "SS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "SS", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R40", "Tanggal": "2026-11-12", "Waktu": "23:51:08", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R41", "Tanggal": "2026-11-13", "Waktu": "06:45:12", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "SS", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R42", "Tanggal": "2026-11-13", "Waktu": "06:52:30", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "STS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "STS", "Skor_Rata_Rata": 2.0},
            {"Responden": "R43", "Tanggal": "2026-11-13", "Waktu": "06:58:19", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.14},
            {"Responden": "R44", "Tanggal": "2026-11-13", "Waktu": "07:01:05", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R45", "Tanggal": "2026-11-13", "Waktu": "07:03:42", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "SS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R46", "Tanggal": "2026-11-13", "Waktu": "07:05:18", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R47", "Tanggal": "2026-11-13", "Waktu": "07:07:50", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "SS", "Indikator 5 (Q5) — Aspek Penilaian 5": "TS", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R48", "Tanggal": "2026-11-13", "Waktu": "07:09:14", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "TS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "SS", "Skor_Rata_Rata": 2.86},
            {"Responden": "R49", "Tanggal": "2026-11-13", "Waktu": "07:12:33", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "SS", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.14},
            {"Responden": "R50", "Tanggal": "2026-11-13", "Waktu": "07:15:09", "Indikator 1 (Q1) — Aspek Penilaian 1": "TS", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.86},
            {"Responden": "R51", "Tanggal": "2026-11-13", "Waktu": "07:18:45", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R52", "Tanggal": "2026-11-13", "Waktu": "07:22:10", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "STS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R53", "Tanggal": "2026-11-13", "Waktu": "07:26:55", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R54", "Tanggal": "2026-11-13", "Waktu": "07:31:20", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R55", "Tanggal": "2026-11-13", "Waktu": "07:38:04", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.14},
            {"Responden": "R56", "Tanggal": "2026-11-13", "Waktu": "07:45:19", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R57", "Tanggal": "2026-11-13", "Waktu": "08:12:40", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "TS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 2.71},
            {"Responden": "R58", "Tanggal": "2026-11-13", "Waktu": "09:30:15", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "S", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R59", "Tanggal": "2026-11-13", "Waktu": "12:15:02", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "TS", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.0},
            {"Responden": "R60", "Tanggal": "2026-11-13", "Waktu": "16:11:51", "Indikator 1 (Q1) — Aspek Penilaian 1": "S", "Indikator 2 (Q2) — Aspek Penilaian 2": "SS", "Indikator 3 (Q3) — Aspek Penilaian 3": "S", "Indikator 4 (Q4) — Aspek Penilaian 4": "S", "Indikator 5 (Q5) — Aspek Penilaian 5": "S", "Indikator 6 (Q6) — Aspek Penilaian 6": "S", "Indikator 7 (Q7) — Aspek Penilaian 7": "S", "Skor_Rata_Rata": 3.14}
        ];

        // Daftar Label Indikator Lengkap
        const INDICATOR_FULL_LABELS = [
            "Indikator 1 (Q1) — Aspek Penilaian 1",
            "Indikator 2 (Q2) — Aspek Penilaian 2",
            "Indikator 3 (Q3) — Aspek Penilaian 3",
            "Indikator 4 (Q4) — Aspek Penilaian 4",
            "Indikator 5 (Q5) — Aspek Penilaian 5",
            "Indikator 6 (Q6) — Aspek Penilaian 6",
            "Indikator 7 (Q7) — Aspek Penilaian 7"
        ];

        // State Global Aplikasi
        let masterData = [];
        let filteredData = [];
        let sortState = { column: 'Responden', direction: 'asc' };
        let barLayoutStacked = true;
        let barUnitMode = 'count'; // 'count' | 'percent'
        let activeModalRespondentId = null;

        // Chart.js Instances
        let indicatorChart = null;
        let categoryDoughnutChart = null;
        let respondentTrendChart = null;

        Chart.defaults.font.family = "'Inter', sans-serif";
        Chart.defaults.color = '#475569';

        // Normalisasi dataset mentah ke struktur internal yang konsisten
        function normalizeDataset(rawList) {
            return rawList.map((row, index) => {
                const q1 = row["Indikator 1 (Q1) — Aspek Penilaian 1"] || row["Q1"] || "S";
                const q2 = row["Indikator 2 (Q2) — Aspek Penilaian 2"] || row["Q2"] || "S";
                const q3 = row["Indikator 3 (Q3) — Aspek Penilaian 3"] || row["Q3"] || "S";
                const q4 = row["Indikator 4 (Q4) — Aspek Penilaian 4"] || row["Q4"] || "S";
                const q5 = row["Indikator 5 (Q5) — Aspek Penilaian 5"] || row["Q5"] || "S";
                const q6 = row["Indikator 6 (Q6) — Aspek Penilaian 6"] || row["Q6"] || "S";
                const q7 = row["Indikator 7 (Q7) — Aspek Penilaian 7"] || row["Q7"] || "S";

                const skor = parseFloat(row["Skor_Rata_Rata"] ?? row["Skor"] ?? 0);
                let kategori = "Sedang";
                if (skor >= 3.0) kategori = "Tinggi";
                else if (skor < 2.0) kategori = "Rendah";

                const respId = String(row["Responden"] || `R${index + 1}`).trim();
                const numMatch = respId.match(/\d+/);
                const idNum = numMatch ? parseInt(numMatch[0], 10) : index + 1;

                return {
                    Responden: respId,
                    idNum: idNum,
                    Tanggal: String(row["Tanggal"] || "").trim(),
                    Waktu: String(row["Waktu"] || "").trim(),
                    Q1: q1.trim(),
                    Q2: q2.trim(),
                    Q3: q3.trim(),
                    Q4: q4.trim(),
                    Q5: q5.trim(),
                    Q6: q6.trim(),
                    Q7: q7.trim(),
                    Skor_Rata_Rata: skor,
                    Kategori: kategori
                };
            });
        }

        // Perbarui dropdown filter tanggal berdasarkan masterData
        function populateDateFilterOptions() {
            const dateSelect = document.getElementById('globalDateFilter');
            const uniqueDates = [...new Set(masterData.map(d => d.Tanggal))].sort();
            dateSelect.innerHTML = `<option value="ALL">Semua Tanggal (${masterData.length})</option>`;
            uniqueDates.forEach(dateStr => {
                const count = masterData.filter(d => d.Tanggal === dateStr).length;
                const opt = document.createElement('option');
                opt.value = dateStr;
                opt.textContent = `${dateStr} (${count})`;
                dateSelect.appendChild(opt);
            });
        }

        // Fungsi Utama Reaktivitas Real-Time saat Filter Berubah
        function applyGlobalFilters() {
            const selectedDate = document.getElementById('globalDateFilter').value;
            const selectedCat = document.getElementById('globalCategoryFilter').value;
            const searchQuery = document.getElementById('globalSearchInput').value.trim().toLowerCase();

            // 1. Filter data sesuai kontrol global
            filteredData = masterData.filter(item => {
                const matchDate = (selectedDate === 'ALL') || (item.Tanggal === selectedDate);
                const matchCat = (selectedCat === 'ALL') || (item.Kategori === selectedCat);
                const matchSearch = (searchQuery === '') || item.Responden.toLowerCase().includes(searchQuery);
                return matchDate && matchCat && matchSearch;
            });

            // 2. Perbarui semua komponen dashboard secara sinkron
            updateKPICards(filteredData);
            updateIndicatorChart(filteredData);
            updateCategoryDoughnutChart(filteredData);
            updateRespondentTrendChart(filteredData);
            renderDataTable(filteredData);
        }

        // Perhitungan & Update 4 Kartu KPI
        function updateKPICards(data) {
            const n = data.length;
            const totalMaster = masterData.length;
            const pctOfTotal = totalMaster > 0 ? ((n / totalMaster) * 100).toFixed(1) : '0.0';

            // KPI 1: Total Responden
            document.getElementById('kpiTotalValue').textContent = n;
            document.getElementById('kpiTotalPct').textContent = `Siswa (${pctOfTotal}%)`;

            const dateCounts = {};
            data.forEach(d => {
                dateCounts[d.Tanggal] = (dateCounts[d.Tanggal] || 0) + 1;
            });
            const dateEntries = Object.entries(dateCounts).sort((a, b) => a[0].localeCompare(b[0]));
            const dateBreakdownEl = document.getElementById('kpiDateBreakdown');
            if (dateEntries.length === 0) {
                dateBreakdownEl.innerHTML = `<span class="text-slate-400">Tidak ada data terpilih</span>`;
            } else {
                dateBreakdownEl.innerHTML = dateEntries.map(([tgl, cnt]) => {
                    const shortTgl = tgl.slice(5); // MM-DD
                    return `<span>${shortTgl}: <strong class="text-slate-800">${cnt}</strong></span>`;
                }).join('<span class="text-slate-300">|</span>');
            }

            // KPI 2: Rata-rata, Standar Deviasi, Median
            if (n === 0) {
                document.getElementById('kpiAvgValue').textContent = '0.00';
                document.getElementById('kpiStdDev').textContent = '0.00';
                document.getElementById('kpiMedian').textContent = '0.00';
            } else {
                const scores = data.map(d => d.Skor_Rata_Rata);
                const sum = scores.reduce((acc, v) => acc + v, 0);
                const mean = sum / n;

                // Sample/Population Standard Deviation (menggunakan N untuk deskriptif sampel penuh)
                const variance = scores.reduce((acc, v) => acc + Math.pow(v - mean, 2), 0) / (n > 1 ? n - 1 : 1);
                const stdDev = Math.sqrt(variance);

                const sortedScores = [...scores].sort((a, b) => a - b);
                const mid = Math.floor(n / 2);
                const median = (n % 2 !== 0) ? sortedScores[mid] : (sortedScores[mid - 1] + sortedScores[mid]) / 2;

                document.getElementById('kpiAvgValue').textContent = mean.toFixed(2);
                document.getElementById('kpiStdDev').textContent = stdDev.toFixed(2);
                document.getElementById('kpiMedian').textContent = median.toFixed(2);
            }

            // KPI 3: Skor Tertinggi & Terendah
            const maxCard = document.getElementById('kpiMaxCard');
            const minCard = document.getElementById('kpiMinCard');
            if (n === 0) {
                document.getElementById('kpiMaxScore').textContent = '-';
                document.getElementById('kpiMaxId').textContent = '-';
                document.getElementById('kpiMinScore').textContent = '-';
                document.getElementById('kpiMinId').textContent = '-';
                maxCard.dataset.respId = '';
                minCard.dataset.respId = '';
            } else {
                let maxItem = data[0];
                let minItem = data[0];
                data.forEach(item => {
                    if (item.Skor_Rata_Rata > maxItem.Skor_Rata_Rata) maxItem = item;
                    if (item.Skor_Rata_Rata < minItem.Skor_Rata_Rata) minItem = item;
                });
                document.getElementById('kpiMaxScore').textContent = maxItem.Skor_Rata_Rata.toFixed(2);
                document.getElementById('kpiMaxId').textContent = maxItem.Responden;
                maxCard.dataset.respId = maxItem.Responden;

                document.getElementById('kpiMinScore').textContent = minItem.Skor_Rata_Rata.toFixed(2);
                document.getElementById('kpiMinId').textContent = minItem.Responden;
                minCard.dataset.respId = minItem.Responden;
            }

            // KPI 4: Proporsi Kategori Risiko
            const highCount = data.filter(d => d.Kategori === 'Tinggi').length;
            const medCount = data.filter(d => d.Kategori === 'Sedang').length;
            const lowCount = data.filter(d => d.Kategori === 'Rendah').length;

            const highPct = n > 0 ? ((highCount / n) * 100).toFixed(1) : '0.0';
            const medPct = n > 0 ? ((medCount / n) * 100).toFixed(1) : '0.0';
            const lowPct = n > 0 ? ((lowCount / n) * 100).toFixed(1) : '0.0';

            document.getElementById('kpiCatHigh').textContent = `${highCount} siswa (${highPct}%)`;
            document.getElementById('kpiCatMed').textContent = `${medCount} siswa (${medPct}%)`;
            document.getElementById('kpiCatLow').textContent = `${lowCount} siswa (${lowPct}%)`;

            document.getElementById('barHigh').style.width = `${highPct}%`;
            document.getElementById('barMed').style.width = `${medPct}%`;
            document.getElementById('barLow').style.width = `${lowPct}%`;
        }

        // Grafik 1: Distribusi Jawaban per Indikator (Q1 - Q7)
        function updateIndicatorChart(data) {
            const ctx = document.getElementById('indicatorChart').getContext('2d');
            if (indicatorChart) {
                indicatorChart.destroy();
            }

            const qKeys = ['Q1', 'Q2', 'Q3', 'Q4', 'Q5', 'Q6', 'Q7'];
            const labels = ['Q1', 'Q2', 'Q3', 'Q4', 'Q5', 'Q6', 'Q7'];
            const n = data.length;

            const countsSTS = qKeys.map(k => data.filter(d => d[k] === 'STS').length);
            const countsTS = qKeys.map(k => data.filter(d => d[k] === 'TS').length);
            const countsS = qKeys.map(k => data.filter(d => d[k] === 'S').length);
            const countsSS = qKeys.map(k => data.filter(d => d[k] === 'SS').length);

            const formatSeries = (arr) => {
                if (barUnitMode === 'percent') {
                    return arr.map(val => n > 0 ? parseFloat(((val / n) * 100).toFixed(1)) : 0);
                }
                return arr;
            };

            indicatorChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'STS (Sangat Tidak Setuju)',
                            data: formatSeries(countsSTS),
                            rawCounts: countsSTS,
                            backgroundColor: '#ef4444', // Merah #ef4444
                            borderRadius: barLayoutStacked ? 0 : 4
                        },
                        {
                            label: 'TS (Tidak Setuju)',
                            data: formatSeries(countsTS),
                            rawCounts: countsTS,
                            backgroundColor: '#f59e0b', // Oranye #f59e0b
                            borderRadius: barLayoutStacked ? 0 : 4
                        },
                        {
                            label: 'S (Setuju)',
                            data: formatSeries(countsS),
                            rawCounts: countsS,
                            backgroundColor: '#3b82f6', // Biru #3b82f6
                            borderRadius: barLayoutStacked ? 0 : 4
                        },
                        {
                            label: 'SS (Sangat Setuju)',
                            data: formatSeries(countsSS),
                            rawCounts: countsSS,
                            backgroundColor: '#10b981', // Hijau Emerald #10b981
                            borderRadius: barLayoutStacked ? { topLeft: 4, topRight: 4 } : 4
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    interaction: {
                        mode: 'index',
                        intersect: false
                    },
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                usePointStyle: true,
                                pointStyle: 'circle',
                                padding: 14,
                                font: { size: 11, weight: '600' }
                            }
                        },
                        tooltip: {
                            backgroundColor: '#0f172a',
                            padding: 12,
                            cornerRadius: 8,
                            callbacks: {
                                title: function(items) {
                                    const idx = items[0].dataIndex;
                                    return INDICATOR_FULL_LABELS[idx];
                                },
                                label: function(context) {
                                    const rawCount = context.dataset.rawCounts[context.dataIndex];
                                    const pct = n > 0 ? ((rawCount / n) * 100).toFixed(1) : '0.0';
                                    return ` ${context.dataset.label}: ${rawCount} siswa (${pct}%)`;
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            stacked: barLayoutStacked,
                            grid: { display: false },
                            ticks: { font: { weight: '700', size: 12 } }
                        },
                        y: {
                            stacked: barLayoutStacked,
                            beginAtZero: true,
                            max: barUnitMode === 'percent' ? (barLayoutStacked ? 100 : undefined) : undefined,
                            grid: { color: '#f1f5f9' },
                            title: {
                                display: true,
                                text: barUnitMode === 'percent' ? 'Persentase Responden (%)' : 'Jumlah Responden (Siswa)',
                                font: { size: 11, weight: '600' }
                            },
                            ticks: {
                                callback: function(value) {
                                    return barUnitMode === 'percent' ? `${value}%` : value;
                                }
                            }
                        }
                    }
                }
            });
        }

        function setBarLayout(isStacked) {
            barLayoutStacked = isStacked;
            const btnStacked = document.getElementById('btnBarStacked');
            const btnGrouped = document.getElementById('btnBarGrouped');
            if (isStacked) {
                btnStacked.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-slate-900 shadow-sm transition cursor-pointer';
                btnGrouped.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer';
            } else {
                btnGrouped.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-slate-900 shadow-sm transition cursor-pointer';
                btnStacked.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer';
            }
            updateIndicatorChart(filteredData);
        }

        function setBarUnit(mode) {
            barUnitMode = mode;
            const btnCount = document.getElementById('btnUnitCount');
            const btnPercent = document.getElementById('btnUnitPercent');
            if (mode === 'count') {
                btnCount.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-emerald-700 shadow-sm transition cursor-pointer';
                btnPercent.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer';
            } else {
                btnPercent.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg bg-white text-emerald-700 shadow-sm transition cursor-pointer';
                btnCount.className = 'px-2.5 py-1 text-xs font-semibold rounded-lg text-slate-600 hover:text-slate-900 transition cursor-pointer';
            }
            updateIndicatorChart(filteredData);
        }

        // Grafik 2: Proporsi Kategori Burnout (Doughnut Chart)
        function updateCategoryDoughnutChart(data) {
            const ctx = document.getElementById('categoryDoughnutChart').getContext('2d');
            if (categoryDoughnutChart) {
                categoryDoughnutChart.destroy();
            }

            const n = data.length;
            const highCount = data.filter(d => d.Kategori === 'Tinggi').length;
            const medCount = data.filter(d => d.Kategori === 'Sedang').length;
            const lowCount = data.filter(d => d.Kategori === 'Rendah').length;

            categoryDoughnutChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['Tinggi (≥ 3.00)', 'Sedang (2.00 – 2.99)', 'Rendah (< 2.00)'],
                    datasets: [{
                        data: [highCount, medCount, lowCount],
                        backgroundColor: ['#ef4444', '#f59e0b', '#10b981'],
                        borderWidth: 3,
                        borderColor: '#ffffff',
                        hoverOffset: 6
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '64%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                usePointStyle: true,
                                pointStyle: 'circle',
                                padding: 12,
                                font: { size: 11, weight: '600' }
                            }
                        },
                        tooltip: {
                            backgroundColor: '#0f172a',
                            padding: 12,
                            cornerRadius: 8,
                            callbacks: {
                                label: function(context) {
                                    const val = context.parsed;
                                    const pct = n > 0 ? ((val / n) * 100).toFixed(1) : '0.0';
                                    return ` ${context.label}: ${val} siswa (${pct}%)`;
                                }
                            }
                        }
                    }
                }
            });
        }

        // Grafik 3: Profil & Tren Skor Rata-Rata Responden (Line/Area Chart + Click Point)
        function updateRespondentTrendChart(data) {
            const ctx = document.getElementById('respondentTrendChart').getContext('2d');
            if (respondentTrendChart) {
                respondentTrendChart.destroy();
            }

            // Urutkan secara kronologis berdasarkan nomor responden untuk grafik tren
            const orderedData = [...data].sort((a, b) => a.idNum - b.idNum);
            const labels = orderedData.map(d => d.Responden);
            const scores = orderedData.map(d => d.Skor_Rata_Rata);

            const n = orderedData.length;
            const mean = n > 0 ? parseFloat((scores.reduce((a, b) => a + b, 0) / n).toFixed(2)) : 2.72;
            const avgLine = new Array(n).fill(mean);
            const highLimitLine = new Array(n).fill(3.00);

            const pointColors = orderedData.map(d => {
                if (d.Skor_Rata_Rata >= 3.0) return '#ef4444';
                if (d.Skor_Rata_Rata < 2.0) return '#10b981';
                return '#f59e0b';
            });

            respondentTrendChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Skor Rata-Rata Individu',
                            data: scores,
                            borderColor: '#0d9488',
                            backgroundColor: 'rgba(13, 148, 136, 0.12)',
                            borderWidth: 2,
                            pointBackgroundColor: pointColors,
                            pointBorderColor: '#ffffff',
                            pointBorderWidth: 1.5,
                            pointRadius: 5,
                            pointHoverRadius: 8,
                            fill: true,
                            tension: 0.25
                        },
                        {
                            label: `Rata-Rata Aktif (${mean.toFixed(2)})`,
                            data: avgLine,
                            borderColor: '#3b82f6',
                            borderWidth: 2,
                            borderDash: [6, 6],
                            pointRadius: 0,
                            pointHoverRadius: 0,
                            fill: false
                        },
                        {
                            label: 'Batas Risiko Tinggi (3.00)',
                            data: highLimitLine,
                            borderColor: '#ef4444',
                            borderWidth: 1.5,
                            borderDash: [3, 4],
                            pointRadius: 0,
                            pointHoverRadius: 0,
                            fill: false
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    interaction: {
                        mode: 'index',
                        intersect: false
                    },
                    onClick: (event, elements) => {
                        if (elements && elements.length > 0) {
                            const idx = elements[0].index;
                            const clickedResp = orderedData[idx];
                            if (clickedResp) {
                                openRespondentModalById(clickedResp.Responden);
                            }
                        }
                    },
                    onHover: (event, chartElement) => {
                        event.native.target.style.cursor = chartElement[0] ? 'pointer' : 'default';
                    },
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                usePointStyle: true,
                                padding: 14,
                                font: { size: 11, weight: '600' }
                            }
                        },
                        tooltip: {
                            backgroundColor: '#0f172a',
                            padding: 12,
                            cornerRadius: 8,
                            callbacks: {
                                afterBody: function(context) {
                                    const idx = context[0].dataIndex;
                                    const item = orderedData[idx];
                                    if (!item) return [];
                                    return [
                                        `Status Kategori: ${item.Kategori}`,
                                        `Tanggal & Waktu: ${item.Tanggal} (${item.Waktu})`,
                                        `Klik titik untuk detail 7 indikator`
                                    ];
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            grid: { display: false },
                            ticks: {
                                maxRotation: 0,
                                autoSkip: true,
                                maxTicksLimit: 20,
                                font: { size: 11 }
                            }
                        },
                        y: {
                            min: 1.5,
                            max: 3.75,
                            grid: { color: '#f1f5f9' },
                            title: {
                                display: true,
                                text: 'Skor Rata-Rata (1.00 – 4.00)',
                                font: { size: 11, weight: '600' }
                            }
                        }
                    }
                }
            });
        }

        // Badge Helper untuk Skala Jawaban Q1-Q7
        function renderScaleBadge(val) {
            const styles = {
                'STS': 'bg-red-100 text-red-800 border-red-200',
                'TS': 'bg-amber-100 text-amber-800 border-amber-200',
                'S': 'bg-blue-100 text-blue-800 border-blue-200',
                'SS': 'bg-emerald-100 text-emerald-800 border-emerald-200'
            };
            const cls = styles[val] || 'bg-slate-100 text-slate-700 border-slate-200';
            return `<span class="inline-flex items-center justify-center w-10 py-0.5 rounded-md text-xs font-bold border ${cls}">${val}</span>`;
        }

        // Badge Helper untuk Status Kategori
        function renderCategoryBadge(kategori) {
            if (kategori === 'Tinggi') {
                return `<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-bold bg-red-100 text-red-800 border border-red-200"><span class="w-1.5 h-1.5 rounded-full bg-red-500"></span>Tinggi</span>`;
            }
            if (kategori === 'Rendah') {
                return `<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-bold bg-emerald-100 text-emerald-800 border border-emerald-200"><span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span>Rendah</span>`;
            }
            return `<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-xs font-bold bg-amber-100 text-amber-800 border border-amber-200"><span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span>Sedang</span>`;
        }

        // Render Tabel Data Responden Interaktif
        function renderDataTable(data) {
            const tbody = document.getElementById('tableBody');
            const emptyState = document.getElementById('emptyState');
            const summaryEl = document.getElementById('tableCountSummary');

            summaryEl.innerHTML = `Menampilkan <strong class="text-slate-900">${data.length}</strong> dari <strong class="text-slate-900">${masterData.length}</strong> responden`;

            if (data.length === 0) {
                tbody.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');

            // Terapkan pengurutan (Sorting)
            const sorted = [...data].sort((a, b) => {
                let valA, valB;
                if (sortState.column === 'Responden') {
                    valA = a.idNum;
                    valB = b.idNum;
                } else {
                    valA = a[sortState.column];
                    valB = b[sortState.column];
                }
                if (valA < valB) return sortState.direction === 'asc' ? -1 : 1;
                if (valA > valB) return sortState.direction === 'asc' ? 1 : -1;
                return 0;
            });

            tbody.innerHTML = sorted.map(row => {
                const isMax = row.Responden === 'R23';
                const isMin = row.Responden === 'R25';
                const rowHighlight = (isMax || isMin) ? 'bg-slate-50/80' : '';
                return `
                    <tr onclick="openRespondentModalById('${row.Responden}')" class="${rowHighlight} hover:bg-emerald-50/70 transition-colors cursor-pointer group">
                        <td class="py-3 px-4 font-bold text-slate-900 whitespace-nowrap group-hover:text-emerald-700">
                            ${row.Responden}
                            ${isMax ? '<span class="ml-1.5 text-[10px] font-bold uppercase px-1.5 py-0.5 rounded bg-red-600 text-white">Max</span>' : ''}
                            ${isMin ? '<span class="ml-1.5 text-[10px] font-bold uppercase px-1.5 py-0.5 rounded bg-emerald-600 text-white">Min</span>' : ''}
                        </td>
                        <td class="py-3 px-4 text-slate-600 whitespace-nowrap text-xs font-medium">${row.Tanggal}</td>
                        <td class="py-3 px-4 text-slate-500 whitespace-nowrap text-xs font-mono">${row.Waktu}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q1)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q2)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q3)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q4)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q5)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q6)}</td>
                        <td class="py-3 px-2.5 text-center">${renderScaleBadge(row.Q7)}</td>
                        <td class="py-3 px-4 text-right font-extrabold text-slate-900 whitespace-nowrap">${row.Skor_Rata_Rata.toFixed(2)}</td>
                        <td class="py-3 px-4 text-center whitespace-nowrap">${renderCategoryBadge(row.Kategori)}</td>
                    </tr>
                `;
            }).join('');
        }

        // Sorting Kolom Tabel
        function sortTable(column) {
            if (sortState.column === column) {
                sortState.direction = sortState.direction === 'asc' ? 'desc' : 'asc';
            } else {
                sortState.column = column;
                sortState.direction = column === 'Skor_Rata_Rata' ? 'desc' : 'asc';
            }

            ['Responden', 'Waktu', 'Skor_Rata_Rata'].forEach(col => {
                const icon = document.getElementById(`sort-${col}`);
                if (icon) icon.className = 'fa-solid fa-sort text-slate-400';
            });

            const activeIcon = document.getElementById(`sort-${column}`);
            if (activeIcon) {
                activeIcon.className = sortState.direction === 'asc'
                    ? 'fa-solid fa-sort-up text-emerald-600'
                    : 'fa-solid fa-sort-down text-emerald-600';
            }

            renderDataTable(filteredData);
        }

        // Reset Semua Filter Global
        function resetAllFilters() {
            document.getElementById('globalDateFilter').value = 'ALL';
            document.getElementById('globalCategoryFilter').value = 'ALL';
            document.getElementById('globalSearchInput').value = '';
            sortState = { column: 'Responden', direction: 'asc' };

            ['Responden', 'Waktu', 'Skor_Rata_Rata'].forEach(col => {
                const icon = document.getElementById(`sort-${col}`);
                if (icon) icon.className = col === 'Responden' ? 'fa-solid fa-sort-up text-emerald-600' : 'fa-solid fa-sort text-slate-400';
            });

            applyGlobalFilters();
            showToast('Semua filter telah dikembalikan ke pengaturan awal.');
        }

        // Modal Detail Responden
        function openRespondentModalById(respId) {
            if (!respId) return;
            const respondent = masterData.find(d => d.Responden === respId);
            if (!respondent) return;

            activeModalRespondentId = respId;

            // Hitung rata-rata master untuk perbandingan
            const overallAvg = masterData.reduce((acc, d) => acc + d.Skor_Rata_Rata, 0) / masterData.length;
            const diff = respondent.Skor_Rata_Rata - overallAvg;
            const diffText = diff >= 0
                ? `+${diff.toFixed(2)} di atas rata-rata (${overallAvg.toFixed(2)})`
                : `${diff.toFixed(2)} di bawah rata-rata (${overallAvg.toFixed(2)})`;

            document.getElementById('modalBadgeId').textContent = respondent.Responden;
            document.getElementById('modalTitle').textContent = `Profil Detail Responden ${respondent.Responden}`;