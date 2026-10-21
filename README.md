<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NC Quality Dashboard - WG6 (ส่วนหลังพิมพ์)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Prompt', sans-serif; 
            background: linear-gradient(120deg, #fef2f2 0%, #fef3c7 25%, #ecfdf5 50%, #e0f2fe 75%, #f3e8ff 100%); 
            color: #334155; 
            min-height: 100vh; 
        }
        .rainbow-card { 
            background: rgba(255, 255, 255, 0.85); 
            backdrop-filter: blur(12px); 
            border: 1px solid rgba(255, 255, 255, 0.9); 
            border-radius: 1.25rem; 
            box-shadow: 0 10px 25px -5px rgba(203, 213, 225, 0.4), 0 8px 10px -6px rgba(203, 213, 225, 0.2); 
        }
        .rainbow-card:hover { 
            border-color: #c084fc; 
            box-shadow: 0 14px 28px -4px rgba(192, 132, 252, 0.25); 
            transition: all 0.3s ease; 
        }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; height: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #f1f5f9; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 3px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="p-4 md:p-6 custom-scrollbar">
    <!-- Header Section -->
    <header class="mb-6 flex flex-col md:flex-row md:items-center md:justify-between gap-4 rainbow-card p-6 border-l-8 border-l-purple-400">
        <div>
            <div class="flex items-center gap-3">
                <div class="p-3 bg-purple-100 text-purple-600 rounded-2xl shadow-sm">
                    <i class="fa-solid fa-chart-line text-2xl"></i>
                </div>
                <div>
                    <h1 class="text-2xl md:text-3xl font-bold tracking-tight text-slate-800">รายงานสถิติผลิตภัณฑ์ที่ไม่เป็นไปตามข้อกำหนด (NC)</h1>
                    <p class="text-purple-600 font-medium text-sm mt-0.5 flex items-center gap-1.5">
                        <i class="fa-regular fa-clock text-pink-400"></i>
                        <span>แผนกหลังพิมพ์ (WG6) | สรุปรายงานสถิติคุณภาพ</span>
                    </p>
                </div>
            </div>
        </div>
        <div class="flex items-center gap-3 self-end md:self-auto">
            <button onclick="resetFilters()" class="px-4 py-2.5 bg-white hover:bg-slate-50 text-slate-600 rounded-xl text-sm font-medium transition flex items-center gap-2 border border-purple-200 shadow-sm">
                <i class="fa-solid fa-rotate-left text-purple-400"></i> ล้างตัวกรอง
            </button>
            <button onclick="exportCSV()" class="px-4 py-2.5 bg-purple-400 hover:bg-purple-500 text-white rounded-xl text-sm font-medium transition flex items-center gap-2 shadow-md shadow-purple-200">
                <i class="fa-solid fa-file-csv"></i> ส่งออก CSV
            </button>
        </div>
    </header>

    <!-- Filters Bar -->
    <section class="rainbow-card p-5 mb-6 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-solid fa-sitemap text-amber-400 mr-1"></i> แผนก</label>
            <select id="filterDept" onchange="applyFilters()" class="w-full bg-slate-50/80 border border-amber-200 rounded-xl px-3 py-2 text-sm text-slate-700 focus:outline-none focus:ring-2 focus:ring-amber-300">
                <option value="ALL">ทุกแผนก</option>
            </select>
        </div>
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-solid fa-gears text-emerald-400 mr-1"></i> เครื่องจักร</label>
            <select id="filterMachine" onchange="applyFilters()" class="w-full bg-slate-50/80 border border-emerald-200 rounded-xl px-3 py-2 text-sm text-slate-700 focus:outline-none focus:ring-2 focus:ring-emerald-300">
                <option value="ALL">ทุกเครื่องจักร</option>
            </select>
        </div>
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-solid fa-clock text-sky-400 mr-1"></i> กะการทำงาน</label>
            <select id="filterShift" onchange="applyFilters()" class="w-full bg-slate-50/80 border border-sky-200 rounded-xl px-3 py-2 text-sm text-slate-700 focus:outline-none focus:ring-2 focus:ring-sky-300">
                <option value="ALL">ทุกกะ</option>
            </select>
        </div>
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-solid fa-magnifying-glass text-purple-400 mr-1"></i> ค้นหาปัญหา/JOB/ผู้ควบคุม</label>
            <input type="text" id="filterSearch" onkeyup="applyFilters()" placeholder="พิมพ์คำค้นหา..." class="w-full bg-slate-50/80 border border-purple-200 rounded-xl px-3 py-2 text-sm text-slate-700 focus:outline-none focus:ring-2 focus:ring-purple-300">
        </div>
    </section>

    <!-- Executive KPI Cards -->
    <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5 mb-6">
        <div class="rainbow-card p-5 relative overflow-hidden bg-gradient-to-br from-white to-purple-50">
            <div class="flex justify-between items-start">
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">จำนวนเคส NC ทั้งหมด</p>
                    <h3 id="kpiTotalNC" class="text-3xl font-extrabold text-purple-600 mt-2">0</h3>
                    <p class="text-xs text-purple-400 font-medium mt-1"><i class="fa-solid fa-triangle-exclamation mr-1"></i> รายการที่ได้รับแจ้ง</p>
                </div>
                <div class="p-3 bg-purple-100 text-purple-500 rounded-2xl">
                    <i class="fa-solid fa-clipboard-list text-2xl"></i>
                </div>
            </div>
            <div class="absolute bottom-0 left-0 right-0 h-1.5 bg-purple-300"></div>
        </div>

        <div class="rainbow-card p-5 relative overflow-hidden bg-gradient-to-br from-white to-amber-50">
            <div class="flex justify-between items-start">
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">จำนวนของเสียสะสม (แผ่น)</p>
                    <h3 id="kpiTotalSheets" class="text-3xl font-extrabold text-amber-500 mt-2">0</h3>
                    <p class="text-xs text-amber-400 font-medium mt-1"><i class="fa-solid fa-copy mr-1"></i> รวมปริมาณแผ่นที่กระทบ</p>
                </div>
                <div class="p-3 bg-amber-100 text-amber-500 rounded-2xl">
                    <i class="fa-solid fa-layer-group text-2xl"></i>
                </div>
            </div>
            <div class="absolute bottom-0 left-0 right-0 h-1.5 bg-amber-300"></div>
        </div>

        <div class="rainbow-card p-5 relative overflow-hidden bg-gradient-to-br from-white to-pink-50">
            <div class="flex justify-between items-start">
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">จำนวนของเสียสะสม (ชิ้น)</p>
                    <h3 id="kpiTotalPcs" class="text-3xl font-extrabold text-pink-500 mt-2">0</h3>
                    <p class="text-xs text-pink-400 font-medium mt-1"><i class="fa-solid fa-box-open mr-1"></i> รวมปริมาณชิ้นงานที่กระทบ</p>
                </div>
                <div class="p-3 bg-pink-100 text-pink-400 rounded-2xl">
                    <i class="fa-solid fa-boxes-stacked text-2xl"></i>
                </div>
            </div>
            <div class="absolute bottom-0 left-0 right-0 h-1.5 bg-pink-300"></div>
        </div>

        <div class="rainbow-card p-5 relative overflow-hidden bg-gradient-to-br from-white to-emerald-50">
            <div class="flex justify-between items-start">
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">แผนกที่พบปัญหามากสุด</p>
                    <h3 id="kpiTopDept" class="text-xl font-extrabold text-emerald-600 mt-2 truncate max-w-[180px]">-</h3>
                    <p id="kpiTopDeptSub" class="text-xs text-emerald-400 font-medium mt-1">0 เคส</p>
                </div>
                <div class="p-3 bg-emerald-100 text-emerald-500 rounded-2xl">
                    <i class="fa-solid fa-industry text-2xl"></i>
                </div>
            </div>
            <div class="absolute bottom-0 left-0 right-0 h-1.5 bg-emerald-300"></div>
        </div>
    </section>

    <!-- Charts Grid Section -->
    <section class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb
