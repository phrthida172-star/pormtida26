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
                        <span>แผนกหลังพิมพ์ (WG6) | สรุปช่วงเวลา: <span class="font-semibold text-slate-700">มกราคม ถึง ธันวาคม 2569</span></span>
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
    <section class="rainbow-card p-5 mb-6 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-4">
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-regular fa-calendar text-pink-400 mr-1"></i> เดือน</label>
            <select id="filterMonth" onchange="applyFilters()" class="w-full bg-slate-50/80 border border-pink-200 rounded-xl px-3 py-2 text-sm text-slate-700 focus:outline-none focus:ring-2 focus:ring-pink-300">
                <option value="ALL">ทุกเดือนตามข้อมูล (ม.ค. - ธ.ค.)</option>
                <option value="มกราคม 69">มกราคม 2569</option>
                <option value="กุมภาพันธ์ 69">กุมภาพันธ์ 2569</option>
                <option value="มีนาคม 69">มีนาคม 2569</option>
                <option value="เมษายน 69">เมษายน 2569</option>
                <option value="พฤษภาคม 69">พฤษภาคม 2569</option>
                <option value="มิถุนายน 69">มิถุนายน 2569</option>
                <option value="กรกฎาคม 69">กรกฎาคม 2569</option>
                <option value="สิงหาคม 69">สิงหาคม 2569</option>
                <option value="กันยายน 69">กันยายน 2569</option>
                <option value="ตุลาคม 69">ตุลาคม 2569</option>
                <option value="พฤศจิกายน 69">พฤศจิกายน 2569</option>
                <option value="ธันวาคม 69">ธันวาคม 2569</option>
            </select>
        </div>
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
                <option value="ALL">ทุกกะ (A / B)</option>
                <option value="A">กะ A</option>
                <option value="B">กะ B</option>
            </select>
        </div>
        <div>
            <label class="block text-xs font-semibold text-slate-500 mb-1.5"><i class="fa-solid fa-magnifying-glass text-purple-400 mr-1"></i> ค้นหาปัญหา/JOB</label>
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

    <!-- Charts Grid Section 1 -->
    <section class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
        <!-- Pareto Chart (Top Defects) -->
        <div class="rainbow-card p-5 lg:col-span-2">
            <div class="flex justify-between items-center mb-4">
                <div>
                    <h2 class="text-base font-bold text-slate-700 flex items-center gap-2">
                        <i class="fa-solid fa-chart-bar text-purple-400"></i> Top 10 ประเภทปัญหาที่พบบ่อยสุด (Pareto Defects)
                    </h2>
                    <p class="text-xs text-slate-400">จำแนกตามจำนวนเคสที่ได้รับแจ้ง</p>
                </div>
            </div>
            <div class="h-72">
                <canvas id="paretoChart"></canvas>
            </div>
        </div>

        <!-- Department Breakdown (Donut) -->
        <div class="rainbow-card p-5">
            <div class="flex justify-between items-center mb-4">
                <div>
                    <h2 class="text-base font-bold text-slate-700 flex items-center gap-2">
                        <i class="fa-solid fa-chart-pie text-pink-400"></i> สัดส่วน NC แยกตามแผนก
                    </h2>
                    <p class="text-xs text-slate-400">เปอร์เซ็นต์ปัญหาในแต่ละกระบวนการ</p>
                </div>
            </div>
            <div class="h-72 flex items-center justify-center">
                <canvas id="deptChart"></canvas>
            </div>
        </div>
    </section>

    <!-- Charts Grid Section 2 -->
    <section class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
        <!-- Monthly Trend Chart -->
        <div class="rainbow-card p-5">
            <div class="flex justify-between items-center mb-4">
                <div>
                    <h2 class="text-base font-bold text-slate-700 flex items-center gap-2">
                        <i class="fa-solid fa-chart-line text-sky-400"></i> แนวโน้มการเกิด NC รายเดือน (Trend Analysis)
                    </h2>
                    <p class="text-xs text-slate-400">เปรียบเทียบจำนวนเคสรายเดือน (ม.ค. - ธ.ค. 2569)</p>
                </div>
            </div>
            <div class="h-72">
                <canvas id="trendChart"></canvas>
            </div>
        </div>

        <!-- Top Machines Chart -->
        <div class="rainbow-card p-5">
            <div class="flex justify-between items-center mb-4">
                <div>
                    <h2 class="text-base font-semibold text-slate-700 flex items-center gap-2">
                        <i class="fa-solid fa-screwdriver-wrench text-amber-400"></i> Top 8 เครื่องจักรที่เกิดปัญหาสูงสุด
                    </h2>
                    <p class="text-xs text-slate-400">จำนวนเคสแบ่งตามหมายเลขเครื่องจักร</p>
                </div>
            </div>
            <div class="h-72">
                <canvas id="machineChart"></canvas>
            </div>
        </div>
    </section>

    <!-- Action Tracker Table -->
    <section class="rainbow-card p-5">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-4">
            <div>
                <h2 class="text-base font-bold text-slate-700 flex items-center gap-2">
                    <i class="fa-solid fa-table-list text-purple-400"></i> ตารางบันทึกการรายงานปัญหาและการแก้ไข (NC Log)
                </h2>
                <p class="text-xs text-slate-400">แสดงรายการทั้งหมดตามเงื่อนไขตัวกรอง (<span id="recordCount" class="text-purple-500 font-bold">0</span> รายการ)</p>
            </div>
        </div>

        <div class="overflow-x-auto custom-scrollbar max-h-96 rounded-xl border border-purple-100 shadow-inner">
            <table class="w-full text-left text-xs text-slate-600">
                <thead class="bg-purple-50/80 text-purple-800 uppercase text-[11px] font-bold sticky top-0 backdrop-blur-md z-10 border-b border-purple-100">
                    <tr>
                        <th class="p-3">วันที่</th>
                        <th class="p-3">แผนก</th>
                        <th class="p-3">เครื่อง</th>
                        <th class="p-3">กะ/ผู้ควบคุม</th>
                        <th class="p-3">เลข JOB</th>
                        <th class="p-3">ปัญหาที่พบ</th>
                        <th class="p-3 text-right">จำนวน (แผ่น/ชิ้น)</th>
                        <th class="p-3">รายละเอียด / การแก้ไข</th>
                        <th class="p-3">ผู้แจ้ง</th>
                    </tr>
                </thead>
                <tbody id="ncTableBody" class="divide-y divide-purple-50 bg-white">
                    <!-- Dynamic Rows -->
                </tbody>
            </table>
        </div>
    </section>

    <!-- Data Injection & Application Logic -->
    <script>
        const GOOGLE_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzKKqhF6MapYv50JbAMctZr_0V4CjakHbriJTOwb9ctXt4PJx1C5MVPE8wjy6bEe8BM/exec"; 
        let rawData = [];
        let chartPareto, chartDept, chartTrend, chartMachine;

        window.addEventListener('
