[dashboard.html](https://github.com/user-attachments/files/32936772/dashboard.html)
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
                <option value="มกราคม69">มกราคม 2569</option>
                <option value="กุมภาพันธ์ 69">กุมภาพันธ์ 2569</option>
                <option value="มีนาคม 69">มีนาคม 2569</option>
                <option value="เมษายน 69">เมษายน 2569</option>
                <option value="พฤษภาคม69">พฤษภาคม 2569</option>
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
        // ลิงก์ Web App URL จาก Google Apps Script ของคุณ
        const GOOGLE_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzKKqhF6MapYv50JbAMctZr_0V4CjakHbriJTOwb9ctXt4PJx1C5MVPE8wjy6bEe8BM/exec"; 

        let rawData = [];
        let chartPareto, chartDept, chartTrend, chartMachine;

        // ดึงข้อมูลอัตโนมัติเมื่อเปิดหน้าเว็บ
        window.addEventListener('DOMContentLoaded', () => {
            fetchDataFromGoogleSheet();
        });

        function fetchDataFromGoogleSheet() {
            document.getElementById('recordCount').innerText = "กำลังดึงข้อมูลจาก Google Sheets...";

            fetch(GOOGLE_SCRIPT_URL)
                .then(response => response.json())
                .then(data => {
                    // แปลงข้อมูลจาก Google Sheet / Form เข้าโครงสร้าง Dashboard
                    rawData = data.map((item, index) => ({
                        id: index + 1,
                        date: item["ประทับเวลา"] ? item["ประทับเวลา"].toString().slice(0, 10) : (item["วันที่"] || item["วันที่พบปัญหา"] || "-"),
                        dept_norm: item["แผนก"] || "ไม่ระบุ",
                        machine: item["หมายเลขเครื่องจักร"] || item["เครื่อง"] || "ไม่ระบุ",
                        shift: item["กะการทำงาน"] || item["กะ"] || "-",
                        operator: item["หัวหน้าเครื่อง / ผู้ควบคุม"] || item["ผู้ควบคุม"] || "-",
                        job: item["เลข JOB"] || item["JOB"] || "-",
                        problem: item["ปัญหาที่พบ"] || item["ปัญหา"] || "-",
                        qty_sheets: Number(item["จำนวนของเสีย (แผ่น)"] || item["จำนวนแผ่น"] || item["จำนวนของเสีย แผ่น"]) || 0,
                        qty_pcs: Number(item["จำนวนของเสีย (ชิ้น)"] || item["จำนวนชิ้น"] || item["จำนวนของเสีย ชิ้น"]) || 0,
                        detail: item["รายละเอียดปัญหา / ช่วงที่พบ"] || item["รายละเอียด"] || "-",
                        action: item["มาตรการแก้ไข"] || item["การแก้ไข"] || "-",
                        reporter: item["ผู้แจ้งปัญหา"] || item["ผู้แจ้ง"] || "-",
                        month: item["เดือน"] || "มกราคม69"
                    }));

                    // ประมวลผลและสร้างกราฟ
                    populateDropdowns();
                    renderCharts();
                    applyFilters();
                })
                .catch(error => {
                    console.error("Error fetching data:", error);
                    alert("ไม่สามารถดึงข้อมูลได้ กรุณาตรวจสอบสิทธิ์การแชร์ของ Apps Script หรือรีเฟรชอีกครั้งค่ะ");
                });
        }

        function populateDropdowns() {
            const depts = [...new Set(rawData.map(item => item.dept_norm))].sort();
            const deptSelect = document.getElementById('filterDept');
            deptSelect.innerHTML = '<option value="ALL">ทุกแผนก</option>';
            depts.forEach(d => {
                const opt = document.createElement('option');
                opt.value = d;
                opt.textContent = d;
                deptSelect.appendChild(opt);
            });

            const machines = [...new Set(rawData.map(item => item.machine))].filter(m => m !== 'ไม่ระบุ').sort();
            const mcSelect = document.getElementById('filterMachine');
            mcSelect.innerHTML = '<option value="ALL">ทุกเครื่องจักร</option>';
            machines.forEach(m => {
                const opt = document.createElement('option');
                opt.value = m;
                opt.textContent = `เครื่อง ${m}`;
                mcSelect.appendChild(opt);
            });
        }

        function getFilteredData() {
            const month = document.getElementById('filterMonth').value;
            const dept = document.getElementById('filterDept').value;
            const mc = document.getElementById('filterMachine').value;
            const shift = document.getElementById('filterShift').value;
            const search = document.getElementById('filterSearch').value.toLowerCase().trim();

            return rawData.filter(item => {
                if (month !== 'ALL' && item.month !== month) return false;
                if (dept !== 'ALL' && item.dept_norm !== dept) return false;
                if (mc !== 'ALL' && item.machine !== mc) return false;
                if (shift !== 'ALL' && item.shift !== shift) return false;
                if (search !== '') {
                    const matchProb = item.problem.toLowerCase().includes(search);
                    const matchJob = item.job.toLowerCase().includes(search);
                    const matchDetail = item.detail.toLowerCase().includes(search);
                    const matchOp = item.operator.toLowerCase().includes(search);
                    if (!matchProb && !matchJob && !matchDetail && !matchOp) return false;
                }
                return true;
            });
        }

        function applyFilters() {
            const filtered = getFilteredData();
            updateKPIs(filtered);
            updateTable(filtered);
            updateCharts(filtered);
        }

        function resetFilters() {
            document.getElementById('filterMonth').value = 'ALL';
            document.getElementById('filterDept').value = 'ALL';
            document.getElementById('filterMachine').value = 'ALL';
            document.getElementById('filterShift').value = 'ALL';
            document.getElementById('filterSearch').value = '';
            applyFilters();
        }

        function updateKPIs(data) {
            document.getElementById('kpiTotalNC').innerText = data.length.toLocaleString();
            
            const totalSheets = data.reduce((acc, curr) => acc + (curr.qty_sheets || 0), 0);
            document.getElementById('kpiTotalSheets').innerText = totalSheets.toLocaleString();

            const totalPcs = data.reduce((acc, curr) => acc + (curr.qty_pcs || 0), 0);
            document.getElementById('kpiTotalPcs').innerText = totalPcs.toLocaleString();

            const deptCounts = {};
            data.forEach(d => { deptCounts[d.dept_norm] = (deptCounts[d.dept_norm] || 0) + 1; });
            let topDept = '-';
            let maxCount = 0;
            Object.keys(deptCounts).forEach(dept => {
                if (deptCounts[dept] > maxCount) {
                    maxCount = deptCounts[dept];
                    topDept = dept;
                }
            });
            document.getElementById('kpiTopDept').innerText = topDept;
            document.getElementById('kpiTopDeptSub').innerText = `${maxCount} เคส (${data.length ? Math.round((maxCount/data.length)*100) : 0}%)`;
        }

        function updateTable(data) {
            const tbody = document.getElementById('ncTableBody');
            document.getElementById('recordCount').innerText = data.length;
            tbody.innerHTML = '';

            if (data.length === 0) {
                tbody.innerHTML = `<tr><td colspan="9" class="p-6 text-center text-slate-400">ไม่พบข้อมูลตรงตามเงื่อนไขที่เลือก</td></tr>`;
                return;
            }

            data.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-purple-50/60 transition border-b border-purple-50';
                
                const qtyText = item.qty_sheets > 0 ? `${item.qty_sheets.toLocaleString()} แผ่น` : 
                               (item.qty_pcs > 0 ? `${item.qty_pcs.toLocaleString()} ชิ้น` : '-');

                tr.innerHTML = `
                    <td class="p-3 font-mono text-slate-500 whitespace-nowrap">${item.date || '-'}</td>
                    <td class="p-3"><span class="px-2.5 py-1 rounded-lg bg-purple-100 text-purple-700 text-[11px] font-medium">${item.dept_norm}</span></td>
                    <td class="p-3 font-semibold text-purple-600">${item.machine !== 'ไม่ระบุ' ? 'ม.' + item.machine : '-'}</td>
                    <td class="p-3"><span class="text-amber-600 font-semibold">กะ ${item.shift}</span> <span class="text-slate-400">(${item.operator})</span></td>
                    <td class="p-3 font-mono text-slate-600">${item.job}</td>
                    <td class="p-3 font-medium text-pink-500">${item.problem}</td>
                    <td class="p-3 text-right font-mono font-bold text-amber-600">${qtyText}</td>
                    <td class="p-3 max-w-xs">
                        <div class="text-slate-700 truncate" title="${item.detail}">${item.detail || '-'}</div>
                        <div class="text-[11px] text-emerald-600 font-medium truncate" title="${item.action}"><i class="fa-solid fa-wrench mr-1"></i>${item.action || '-'}</div>
                    </td>
                    <td class="p-3 text-slate-500">${item.reporter}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function renderCharts() {
            Chart.defaults.color = '#64748b';
            Chart.defaults.font.family = 'Prompt';

            if(chartPareto) chartPareto.destroy();
            if(chartDept) chartDept.destroy();
            if(chartTrend) chartTrend.destroy();
            if(chartMachine) chartMachine.destroy();

            const ctxPareto = document.getElementById('paretoChart').getContext('2d');
            chartPareto = new Chart(ctxPareto, {
                type: 'bar',
                data: { 
                    labels: [], 
                    datasets: [{ 
                        label: 'จำนวนเคส', 
                        data: [], 
                        backgroundColor: ['#c084fc', '#f472b6', '#fb923c', '#fbbf24', '#34d399', '#38bdf8', '#818cf8'], 
                        borderRadius: 8 
                    }] 
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { grid: { color: '#f1f5f9' } }, x: { grid: { display: false } } } }
            });

            const ctxDept = document.getElementById('deptChart').getContext('2d');
            chartDept = new Chart(ctxDept, {
                type: 'doughnut',
                data: { labels: [], datasets: [{ data: [], backgroundColor: ['#c084fc', '#f472b6', '#fb923c', '#34d399', '#38bdf8', '#a78bfa'] }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'right', labels: { boxWidth: 12, font: { size: 11 } } } } }
            });

            const ctxTrend = document.getElementById('trendChart').getContext('2d');
            chartTrend = new Chart(ctxTrend, {
                type: 'line',
                data: { 
                    labels: ['ม.ค.', 'ก.พ.', 'มี.ค.', 'เม.ย.', 'พ.ค.', 'มิ.ย.', 'ก.ค.', 'ส.ค.', 'ก.ย.', 'ต.ค.', 'พ.ย.', 'ธ.ค.'], 
                    datasets: [{ 
                        label: 'จำนวน NC (เคส)', 
                        data: [], 
                        borderColor: '#c084fc', 
                        backgroundColor: 'rgba(192, 132, 252, 0.15)', 
                        fill: true, 
                        tension: 0.35, 
                        pointRadius: 5, 
                        pointBackgroundColor: '#c084fc' 
                    }] 
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { grid: { color: '#f1f5f9' } }, x: { grid: { display: false } } } }
            });

            const ctxMachine = document.getElementById('machineChart').getContext('2d');
            chartMachine = new Chart(ctxMachine, {
                type: 'bar',
                data: { labels: [], datasets: [{ label: 'จำนวนเคส', data: [], backgroundColor: '#fb923c', borderRadius: 8 }] },
                options: { indexAxis: 'y', responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { x: { grid: { color: '#f1f5f9' } }, y: { grid: { display: false } } } }
            });
        }

        function updateCharts(data) {
            const probCounts = {};
            data.forEach(d => { probCounts[d.problem] = (probCounts[d.problem] || 0) + 1; });
            const sortedProbs = Object.entries(probCounts).sort((a,b) => b[1] - a[1]).slice(0, 10);
            chartPareto.data.labels = sortedProbs.map(d => d[0]);
            chartPareto.data.datasets[0].data = sortedProbs.map(d => d[1]);
            chartPareto.update();

            const deptCounts = {};
            data.forEach(d => { deptCounts[d.dept_norm] = (deptCounts[d.dept_norm] || 0) + 1; });
            chartDept.data.labels = Object.keys(deptCounts);
            chartDept.data.datasets[0].data = Object.values(deptCounts);
            chartDept.update();

            const monthsOrder = ['มกราคม69', 'กุมภาพันธ์ 69', 'มีนาคม 69', 'เมษายน 69', 'พฤษภาคม69', 'มิถุนายน 69', 'กรกฎาคม 69', 'สิงหาคม 69', 'กันยายน 69', 'ตุลาคม 69', 'พฤศจิกายน 69', 'ธันวาคม 69'];
            const trendData = monthsOrder.map(m => data.filter(d => d.month === m).length);
            chartTrend.data.datasets[0].data = trendData;
            chartTrend.update();

            const mcCounts = {};
            data.forEach(d => { if(d.machine !== 'ไม่ระบุ') mcCounts['เครื่อง ' + d.machine] = (mcCounts['เครื่อง ' + d.machine] || 0) + 1; });
            const sortedMcs = Object.entries(mcCounts).sort((a,b) => b[1] - a[1]).slice(0, 8);
            chartMachine.data.labels = sortedMcs.map(d => d[0]);
            chartMachine.data.datasets[0].data = sortedMcs.map(d => d[1]);
            chartMachine.update();
        }

        function exportCSV() {
            const filtered = getFilteredData();
            if(!filtered.length) { alert('ไม่มีข้อมูลสำหรับส่งออก'); return; }

            let csvContent = "\uFEFFวันที่,แผนก,เครื่อง,กะ,ผู้ควบคุม,JOB,ปัญหา,จำนวนแผ่น,จำนวนชิ้น,รายละเอียด,การแก้ไข,ผู้แจ้ง\n";
            filtered.forEach(r => {
                csvContent += `"${r.date}","${r.dept_norm}","${r.machine}","${r.shift}","${r.operator}","${r.job}","${r.problem}","${r.qty_sheets}","${r.qty_pcs}","${r.detail.replace(/"/g, '""')}","${r.action.replace(/"/g, '""')}","${r.reporter}"\n`;
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = `NC_Report_WG6_${new Date().toISOString().slice(0,10)}.csv`;
            link.click();
        }
    </script>
</body>
</html>
