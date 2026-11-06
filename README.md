<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบบริหารจัดการและบันทึกตัวชี้วัด (Lab KPI Multi-Year System)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Sarabun', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col text-slate-800">

    <!-- Header Navigation -->
    <header class="bg-teal-700 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex flex-col md:flex-row md:items-center md:justify-between gap-4">
            <div class="flex items-center space-x-3">
                <div class="p-2 bg-teal-800 rounded-lg">
                    <i data-lucide="activity" class="w-6 h-6 text-teal-200"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg leading-tight">ระบบบันทึกและติดตามตัวชี้วัดคุณภาพ (Lab KPIs)</h1>
                    <p class="text-xs text-teal-200">กลุ่มงานเทคนิคการแพทย์ — รองรับข้อมูลรายปีงบประมาณ</p>
                </div>
            </div>

            <!-- Global Year Selector & Navigation -->
            <div class="flex flex-wrap items-center gap-3">
                <div class="flex items-center bg-teal-800 rounded-lg px-3 py-1.5 border border-teal-600">
                    <i data-lucide="calendar" class="w-4 h-4 text-teal-200 mr-2"></i>
                    <span class="text-xs font-semibold mr-2 text-teal-100">ปีงบประมาณ:</span>
                    <select id="globalYearSelect" onchange="changeFiscalYear(this.value)" class="bg-teal-900 text-white text-sm font-bold rounded px-2 py-0.5 focus:outline-none focus:ring-2 focus:ring-teal-400">
                        <!-- Options generated dynamically -->
                    </select>
                </div>

                <nav class="flex bg-teal-800 rounded-lg p-1 text-sm">
                    <button onclick="switchTab('dashboard')" id="btn-dashboard" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 bg-white text-teal-800 shadow-sm">
                        <i data-lucide="layout-dashboard" class="w-4 h-4"></i> ภาพรวม
                    </button>
                    <button onclick="switchTab('data-entry')" id="btn-data-entry" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="file-edit" class="w-4 h-4"></i> ลงข้อมูลรายเดือน
                    </button>
                    <button onclick="switchTab('incidents')" id="btn-incidents" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="alert-triangle" class="w-4 h-4"></i> สาเหตุการปฏิเสธ
                    </button>
                    <button onclick="switchTab('annual-report')" id="btn-annual-report" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="table" class="w-4 h-4"></i> สรุปประจำปี
                    </button>
                </nav>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 py-6 flex-1 w-full">

        <!-- TAB 1: DASHBOARD -->
        <section id="tab-dashboard" class="tab-content space-y-6">
            <!-- Summary Stats Header -->
            <div class="flex justify-between items-center bg-white p-4 rounded-xl shadow-sm border border-slate-200">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <span>สรุปภาพรวมตัวชี้วัด ปีงบประมาณ</span>
                        <span id="dash-year-label" class="text-teal-600 bg-teal-50 px-3 py-0.5 rounded-full text-lg border border-teal-200">2568</span>
                    </h2>
                    <p class="text-xs text-slate-500 mt-0.5">ข้อมูลคำนวณสะสมตามเป้าหมายหลักของกลุ่มงาน</p>
                </div>
                <button onclick="exportYearlySummaryCSV()" class="flex items-center gap-1.5 text-xs font-semibold bg-emerald-600 text-white px-3 py-2 rounded-lg hover:bg-emerald-700 transition">
                    <i data-lucide="download" class="w-4 h-4"></i> ส่งออกข้อมูลปีนี้ (CSV)
                </button>
            </div>

            <!-- KPI Cards Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4" id="kpi-cards-container">
                <!-- Cards dynamically loaded -->
            </div>

            <!-- Visual Charts Section -->
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Rejection Causes Chart -->
                <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-slate-700 mb-1 flex items-center gap-2">
                        <i data-lucide="pie-chart" class="w-5 h-5 text-teal-600"></i> สัดส่วนสาเหตุการปฏิเสธสิ่งส่งตรวจ (QI Rejections)
                    </h3>
                    <p class="text-xs text-slate-500 mb-4">จำแนกตามประเภทปัญหาหลักที่พบในห้องปฏิบัติการ</p>
                    <div class="relative h-64">
                        <canvas id="rejectionChart"></canvas>
                    </div>
                </div>

                <!-- Turnaround Time & Error Trend Chart -->
                <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-slate-700 mb-1 flex items-center gap-2">
                        <i data-lucide="trending-up" class="w-5 h-5 text-teal-600"></i> แนวโน้มการรายงานผลทันเวลา (TAT) รายเดือน
                    </h3>
                    <p class="text-xs text-slate-500 mb-4">เปรียบเทียบผลตรวจด่วน (Urgent) และผลตรวจปกติ (Routine)</p>
                    <div class="relative h-64">
                        <canvas id="tatChart"></canvas>
                    </div>
                </div>
            </div>

            <!-- Detailed Status Table -->
            <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="px-5 py-4 border-b border-slate-100 flex justify-between items-center">
                    <h3 class="font-bold text-slate-800">สถานะตัวชี้วัดหลัก 12 รายการ</h3>
                    <span class="text-xs text-slate-500">* อ้างอิงตามเกณฑ์เป้าหมายกลุ่มงานเทคนิคการแพทย์</span>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm">
                        <thead class="bg-slate-50 text-slate-600 text-xs uppercase font-semibold">
                            <tr>
                                <th class="py-3 px-4">รหัส</th>
                                <th class="py-3 px-4">ตัวชี้วัด (KPI Metric)</th>
                                <th class="py-3 px-4 text-center">เป้าหมาย</th>
                                <th class="py-3 px-4 text-center">ผลงานสะสม</th>
                                <th class="py-3 px-4 text-center">สถานะ</th>
                                <th class="py-3 px-4">ผู้รับผิดชอบ</th>
                            </tr>
                        </thead>
                        <tbody id="kpi-status-table-body" class="divide-y divide-slate-100 text-slate-700">
                            <!-- Dynamic Table Rows -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 2: MONTHLY DATA ENTRY -->
        <section id="tab-data-entry" class="tab-content hidden space-y-6">
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex flex-col sm:flex-row justify-between sm:items-center gap-4 mb-6 pb-4 border-b">
                    <div>
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i data-lucide="edit-3" class="w-5 h-5 text-teal-600"></i> บันทึกข้อมูลตัวชี้วัดรายเดือน
                        </h2>
                        <p class="text-xs text-slate-500">เลือกระบุข้อมูลตัวตั้งและตัวหารเพื่อคำนวณอัตราส่วนอัตโนมัติ</p>
                    </div>
                    
                    <div class="flex items-center gap-3">
                        <label class="text-sm font-semibold text-slate-700">เลือกเดือน:</label>
                        <select id="entryMonthSelect" onchange="loadMonthlyDataForm()" class="bg-slate-100 border border-slate-300 rounded-lg px-3 py-2 font-bold text-slate-800 focus:outline-none focus:ring-2 focus:ring-teal-500">
                            <option value="oct">ตุลาคม</option>
                            <option value="nov">พฤศจิกายน</option>
                            <option value="dec">ธันวาคม</option>
                            <option value="jan">มกราคม</option>
                            <option value="feb">กุมภาพันธ์</option>
                            <option value="mar">มีนาคม</option>
                            <option value="apr">เมษายน</option>
                            <option value="may">พฤษภาคม</option>
                            <option value="jun">มิถุนายน</option>
                            <option value="jul">กรกฎาคม</option>
                            <option value="aug">สิงหาคม</option>
                            <option value="sep">กันยายน</option>
                        </select>
                    </div>
                </div>

                <form id="monthlyForm" onsubmit="saveMonthlyData(event)" class="space-y-6">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6" id="form-fields-container">
                        <!-- Dynamic Input Forms -->
                    </div>

                    <div class="flex justify-end gap-3 pt-4 border-t">
                        <button type="reset" class="px-4 py-2 border border-slate-300 rounded-lg text-slate-600 hover:bg-slate-50 text-sm font-medium">ล้างค่า</button>
                        <button type="submit" class="px-6 py-2 bg-teal-700 text-white rounded-lg hover:bg-teal-800 text-sm font-semibold shadow flex items-center gap-2">
                            <i data-lucide="save" class="w-4 h-4"></i> บันทึกข้อมูลประจำเดือน
                        </button>
                    </div>
                </form>
            </div>
        </section>

        <!-- TAB 3: INCIDENT LOGS -->
        <section id="tab-incidents" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Incident Input Form -->
                <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-slate-800 mb-1 flex items-center gap-2">
                        <i data-lucide="plus-circle" class="w-5 h-5 text-teal-600"></i> บันทึกเหตุปฏิเสธสิ่งส่งตรวจ
                    </h3>
                    <p class="text-xs text-slate-500 mb-4">บันทึกอุบัติการณ์เพื่อวิเคราะห์สาเหตุ (QI Root Causes)</p>

                    <form id="incidentForm" onsubmit="addIncidentLog(event)" class="space-y-4 text-sm">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">เดือนที่เกิดเหตุ</label>
                            <select id="incMonth" class="w-full bg-slate-50 border border-slate-300 rounded-lg p-2">
                                <option value="oct">ตุลาคม</option>
                                <option value="nov">พฤศจิกายน</option>
                                <option value="dec">ธันวาคม</option>
                                <option value="jan">มกราคม</option>
                                <option value="feb">กุมภาพันธ์</option>
                                <option value="mar">มีนาคม</option>
                                <option value="apr">เมษายน</option>
                                <option value="may">พฤษภาคม</option>
                                <option value="jun">มิถุนายน</option>
                                <option value="jul">กรกฎาคม</option>
                                <option value="aug">สิงหาคม</option>
                                <option value="sep">กันยายน</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">หมวดหมู่ปัญหา (QI Category)</label>
                            <select id="incCategory" class="w-full bg-slate-50 border border-slate-300 rounded-lg p-2">
                                <option value="สิ่งส่งตรวจไม่เหมาะสม">สิ่งส่งตรวจไม่เหมาะสม (Clot / Hemolysis / ปริมาตร)</option>
                                <option value="ขอยกเลิกการตรวจวิเคราะห์">ขอยกเลิกการตรวจวิเคราะห์ (แพทย์ยกเลิก/ชื่อซ้ำ/Refer)</option>
                                <option value="คำสั่งตรวจผิดพลาด">คำสั่งตรวจผิดพลาด (คำสั่งซ้ำ/ผิดในคอม)</option>
                                <option value="ไม่มีสิ่งส่งตรวจ">ไม่มีสิ่งส่งตรวจ</option>
                                <option value="ฉลากติดชื่อไม่ตรงกัน">ฉลากติดชื่อและใบนำส่งไม่ตรงกัน</option>
                                <option value="อุปกรณ์/เครื่องมือไม่พร้อม">อุปกรณ์/เครื่องมือไม่พร้อมใช้งาน</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">สาเหตุเฉพาะ (Specific Cause)</label>
                            <input type="text" id="incCause" placeholder="เช่น เลือด Clot, Hemolysis, ฝาสลับหลอด" required class="w-full bg-slate-50 border border-slate-300 rounded-lg p-2 focus:ring-2 focus:ring-teal-500">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-slate-700 mb-1">จำนวนครั้งที่พบ (Count)</label>
                            <input type="number" id="incCount" min="1" value="1" required class="w-full bg-slate-50 border border-slate-300 rounded-lg p-2">
                        </div>

                        <button type="submit" class="w-full py-2.5 bg-teal-700 text-white font-semibold rounded-lg hover:bg-teal-800 transition shadow flex items-center justify-center gap-2">
                            <i data-lucide="plus" class="w-4 h-4"></i> เพิ่มรายการอุบัติการณ์
                        </button>
                    </form>
                </div>

                <!-- Incident Table -->
                <div class="lg:col-span-2 bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                    <h3 class="font-bold text-slate-800 mb-1">รายการบันทึกอุบัติการณ์การปฏิเสธสิ่งส่งตรวจ</h3>
                    <p class="text-xs text-slate-500 mb-4">รายการทั้งหมดที่ถูกบันทึกในปีงบประมาณนี้</p>

                    <div class="overflow-x-auto max-h-[420px]">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-slate-50 text-slate-600 text-xs uppercase sticky top-0">
                                <tr>
                                    <th class="py-2.5 px-3">เดือน</th>
                                    <th class="py-2.5 px-3">หมวดหมู่</th>
                                    <th class="py-2.5 px-3">สาเหตุเฉพาะ</th>
                                    <th class="py-2.5 px-3 text-center">จำนวน</th>
                                    <th class="py-2.5 px-3 text-center">จัดการ</th>
                                </tr>
                            </thead>
                            <tbody id="incident-table-body" class="divide-y divide-slate-100 text-xs">
                                <!-- Dynamic incident logs -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 4: ANNUAL SUMMARY REPORT -->
        <section id="tab-annual-report" class="tab-content hidden space-y-6">
            <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200">
                <div class="flex justify-between items-center mb-6 pb-4 border-b print:hidden">
                    <div>
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i data-lucide="file-spreadsheet" class="w-5 h-5 text-teal-600"></i> ตารางสรุปตัวชี้วัดหลักประจำปีงบประมาณ 12 เดือน
                        </h2>
                        <p class="text-xs text-slate-500">แสดงผลงานแยกรายเดือน (ต.ค. - ก.ย.) พร้อมยอดรวมสรุปสะสม</p>
                    </div>
                    <div class="flex gap-2">
                        <button onclick="window.print()" class="px-3 py-2 bg-slate-700 text-white rounded-lg text-xs font-semibold flex items-center gap-1.5 hover:bg-slate-800">
                            <i data-lucide="printer" class="w-4 h-4"></i> พิมพ์รายงาน
                        </button>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-xs text-left border-collapse border border-slate-200" id="matrix-report-table">
                        <thead class="bg-slate-100 text-slate-700 font-bold border-b border-slate-300">
                            <tr>
                                <th class="border border-slate-200 p-2 min-w-[200px]">ตัวชี้วัดหลัก / รายการตรวจ</th>
                                <th class="border border-slate-200 p-2 text-center">เป้าหมาย</th>
                                <th class="border border-slate-200 p-1 text-center">ต.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">พ.ย.</th>
                                <th class="border border-slate-200 p-1 text-center">ธ.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">ม.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">ก.พ.</th>
                                <th class="border border-slate-200 p-1 text-center">มี.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">เม.ย.</th>
                                <th class="border border-slate-200 p-1 text-center">พ.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">มิ.ย.</th>
                                <th class="border border-slate-200 p-1 text-center">ก.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">ส.ค.</th>
                                <th class="border border-slate-200 p-1 text-center">ก.ย.</th>
                                <th class="border border-slate-200 p-2 text-center bg-teal-50 text-teal-800">รวมปี</th>
                            </tr>
                        </thead>
                        <tbody id="matrix-table-body" class="divide-y divide-slate-200">
                            <!-- Matrix Table Dynamic Rows -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-4 mt-8">
        <div class="max-w-7xl mx-auto px-4 text-center text-xs text-slate-500">
            กลุ่มงานเทคนิคการแพทย์ — ระบบบันทึกและประมวลผลตัวชี้วัดคุณภาพประจำปีงบประมาณ
        </div>
    </footer>

    <!-- JavaScript Application Logic -->
    <script>
        // Default Master Data Schema
        const KPI_METRICS_LIST = [
            { id: 'kpi1', code: 'KPI-01', name: 'อัตราการปฏิเสธสิ่งส่งตรวจ', targetStr: '< 2%', targetVal: 2, isLowerBetter: true, unit: '%', owner: 'นิตยา' },
            { id: 'kpi2', code: 'KPI-02.1', name: 'อัตราการประกันคุณภาพภายในครบถ้วน (IQC)', targetStr: '100%', targetVal: 100, isLowerBetter: false, unit: '%', owner: 'นิตยา' },
            { id: 'kpi3', code: 'KPI-02.2', name: 'อัตราการประกันคุณภาพภายในถูกต้อง', targetStr: '≥ 90%', targetVal: 90, isLowerBetter: false, unit: '%', owner: 'นิตยา' },
            { id: 'kpi4', code: 'KPI-03', name: 'อัตราการรายงานผล EQA ผ่านเกณฑ์', targetStr: '≥ 90%', targetVal: 90, isLowerBetter: false, unit: '%', owner: 'นิตยา' },
            { id: 'kpi5', code: 'KPI-04', name: 'อัตราการรายงานผลผิดพลาด (Miss)', targetStr: '≤ 0.1%', targetVal: 0.1, isLowerBetter: true, unit: '%', owner: 'นิตยา' },
            { id: 'kpi6', code: 'KPI-05', name: 'อัตราความครอบคลุมการรายงานค่าวิกฤติ (5 นาที)', targetStr: '100%', targetVal: 100, isLowerBetter: false, unit: '%', owner: 'นิตยา/สุรัญญา' },
            { id: 'kpi7', code: 'KPI-06a', name: 'อัตราการรายงานผลด่วนทันเวลา (TAT Urgent)', targetStr: '≥ 80%', targetVal: 80, isLowerBetter: false, unit: '%', owner: 'ทักษพร' },
            { id: 'kpi8', code: 'KPI-06b', name: 'อัตราการรายงานผลปกติทันเวลา (TAT Routine)', targetStr: '≥ 80%', targetVal: 80, isLowerBetter: false, unit: '%', owner: 'ทักษพร' },
            { id: 'kpi9', code: 'KPI-07', name: 'อัตราการจ่ายเลือดผิดพลาด', targetStr: '0%', targetVal: 0, isLowerBetter: true, unit: '%', owner: 'นิตยา' },
            { id: 'kpi10', code: 'KPI-08', name: 'อัตราการเกิดปฏิกิริยาหลังการให้เลือด', targetStr: '< 1%', targetVal: 1, isLowerBetter: true, unit: '%', owner: 'นิตยา' },
            { id: 'kpi11', code: 'KPI-10', name: 'อัตราการเกิดอุบัติเหตุจากการทำงาน (เข็มตำ/ติดเชื้อ)', targetStr: '0 ครั้ง', targetVal: 0, isLowerBetter: true, unit: 'ครั้ง', owner: 'นิตยา' },
            { id: 'kpi12', code: 'KPI-12', name: 'อัตราความพึงพอใจของผู้ใช้บริการภายนอก/ภายใน', targetStr: '> 80%', targetVal: 80, isLowerBetter: false, unit: '%', owner: 'นิตยา' }
        ];

        const MONTHS_KEY = ['oct', 'nov', 'dec', 'jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jul', 'aug', 'sep'];

        // App Global State
        let currentFiscalYear = 2568;
        let db = {};
        let rejectionChartInstance = null;
        let tatChartInstance = null;

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            setupYearSelector();
            loadYearData(currentFiscalYear);
            lucide.createIcons();
        });

        // Year Switcher Handler
        function setupYearSelector() {
            const select = document.getElementById('globalYearSelect');
            select.innerHTML = '';
            const currentYear = 2568;
            for (let y = currentYear - 2; y <= currentYear + 3; y++) {
                const opt = document.createElement('option');
                opt.value = y;
                opt.textContent = `ปีงบประมาณ ${y}`;
                if (y === currentFiscalYear) opt.selected = true;
                select.appendChild(opt);
            }
        }

        function changeFiscalYear(year) {
            currentFiscalYear = parseInt(year);
            document.getElementById('dash-year-label').textContent = currentFiscalYear;
            loadYearData(currentFiscalYear);
        }

        // Data Storage Engine (LocalStorage per Fiscal Year)
        function loadYearData(year) {
            const storageKey = `LAB_KPI_DATA_FY_${year}`;
            const localData = localStorage.getItem(storageKey);

            if (localData) {
                db = JSON.parse(localData);
            } else {
                // Initialize default empty structure or demo data for 2568
                db = generateBaselineData(year);
                saveYearData();
            }

            renderDashboard();
            loadMonthlyDataForm();
            renderIncidentLogs();
            renderMatrixReport();
        }

        function saveYearData() {
            const storageKey = `LAB_KPI_DATA_FY_${currentFiscalYear}`;
            localStorage.setItem(storageKey, JSON.stringify(db));
        }

        // Baseline Mock Generator for FY 2568
        function generateBaselineData(year) {
            const monthly = {};
            MONTHS_KEY.forEach(m => {
                monthly[m] = {
                    totalSpecimens: year === 2568 ? Math.floor(Math.random() * 1000 + 2500) : 0,
                    rejections: year === 2568 ? Math.floor(Math.random() * 15 + 10) : 0,
                    iqcDone: 55, iqcTotal: 55,
                    iqcPassed: 52, iqcMonitored: 55,
                    eqaPassed: 21, eqaTotal: 22,
                    reportingErrors: year === 2568 ? (Math.random() < 0.3 ? 1 : 0) : 0,
                    criticalFound: 60, criticalReported5Min: 60,
                    urgentTotal: 80, urgentOnTime: 72,
                    routineTotal: 1500, routineOnTime: 1400,
                    bloodErrors: 0, bloodTransfused: 10,
                    reactionCount: 0,
                    accidents: 0,
                    satisfactionScore: 85
                };
            });

            const incidents = year === 2568 ? [
                { id: 1, month: 'oct', category: 'สิ่งส่งตรวจไม่เหมาะสม', cause: 'Clot', count: 6 },
                { id: 2, month: 'nov', category: 'สิ่งส่งตรวจไม่เหมาะสม', cause: 'Hemolysis', count: 4 },
                { id: 3, month: 'dec', category: 'คำสั่งตรวจผิดพลาด', cause: 'คำสั่งในคอมฯผิด', count: 3 }
            ] : [];

            return { monthly, incidents };
        }

        // Tab Switcher
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-white', 'text-teal-800', 'shadow-sm');
                btn.classList.add('text-teal-100', 'hover:bg-teal-700');
            });

            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`btn-${tabId}`);
            activeBtn.classList.add('bg-white', 'text-teal-800', 'shadow-sm');
            activeBtn.classList.remove('text-teal-100', 'hover:bg-teal-700');

            if (tabId === 'dashboard') renderDashboard();
            if (tabId === 'annual-report') renderMatrixReport();
        }

        // Calculations Engine
        function calculateKPISummary() {
            let totalSpecimens = 0, totalRejections = 0;
            let totalReportingErrors = 0;
            let urgentTotal = 0, urgentOnTime = 0;
            let routineTotal = 0, routineOnTime = 0;
            let criticalTotal = 0, criticalOnTime = 0;

            MONTHS_KEY.forEach(m => {
                const d = db.monthly[m];
                totalSpecimens += Number(d.totalSpecimens || 0);
                totalRejections += Number(d.rejections || 0);
                totalReportingErrors += Number(d.reportingErrors || 0);
                urgentTotal += Number(d.urgentTotal || 0);
                urgentOnTime += Number(d.urgentOnTime || 0);
                routineTotal += Number(d.routineTotal || 0);
                routineOnTime += Number(d.routineOnTime || 0);
                criticalTotal += Number(d.criticalFound || 0);
                criticalOnTime += Number(d.criticalReported5Min || 0);
            });

            const rejectRate = totalSpecimens > 0 ? ((totalRejections / totalSpecimens) * 100).toFixed(2) : '0.00';
            const errorRate = totalSpecimens > 0 ? ((totalReportingErrors / totalSpecimens) * 100).toFixed(3) : '0.000';
            const urgentTatRate = urgentTotal > 0 ? ((urgentOnTime / urgentTotal) * 100).toFixed(1) : '0.0';
            const routineTatRate = routineTotal > 0 ? ((routineOnTime / routineTotal) * 100).toFixed(1) : '0.0';
            const criticalRate = criticalTotal > 0 ? ((criticalOnTime / criticalTotal) * 100).toFixed(1) : '100.0';

            return {
                rejectRate: parseFloat(rejectRate),
                errorRate: parseFloat(errorRate),
                urgentTatRate: parseFloat(urgentTatRate),
                routineTatRate: parseFloat(routineTatRate),
                criticalRate: parseFloat(criticalRate),
                totalSpecimens,
                totalRejections
            };
        }

        // Render Dashboard Content
        function renderDashboard() {
            const summary = calculateKPISummary();
            const container = document.getElementById('kpi-cards-container');
            container.innerHTML = '';

            const cardsData = [
                { title: 'อัตราการปฏิเสธสิ่งส่งตรวจ', val: `${summary.rejectRate}%`, target: '< 2%', pass: summary.rejectRate < 2, icon: 'shield-alert', color: 'teal' },
                { title: 'อัตราการรายงานผลผิดพลาด', val: `${summary.errorRate}%`, target: '≤ 0.1%', pass: summary.errorRate <= 0.1, icon: 'alert-circle', color: 'rose' },
                { title: 'TAT ผลตรวจด่วน (Urgent)', val: `${summary.urgentTatRate}%`, target: '≥ 80%', pass: summary.urgentTatRate >= 80, icon: 'clock', color: 'sky' },
                { title: 'ความครอบคลุมค่าวิกฤติ (5 นาที)', val: `${summary.criticalRate}%`, target: '100%', pass: summary.criticalRate >= 100, icon: 'check-circle-2', color: 'emerald' }
            ];

            cardsData.forEach(card => {
                const el = document.createElement('div');
                el.className = 'bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex flex-col justify-between';
                el.innerHTML = `
                    <div class="flex justify-between items-start mb-2">
                        <span class="text-xs font-semibold text-slate-500">${card.title}</span>
                        <div class="p-2 bg-${card.color}-50 text-${card.color}-600 rounded-lg">
                            <i data-lucide="${card.icon}" class="w-4 h-4"></i>
                        </div>
                    </div>
                    <div>
                        <div class="text-2xl font-bold text-slate-800">${card.val}</div>
                        <div class="flex items-center gap-2 mt-1">
                            <span class="text-xs font-medium ${card.pass ? 'text-emerald-600 bg-emerald-50' : 'text-rose-600 bg-rose-50'} px-2 py-0.5 rounded">
                                ${card.pass ? 'ผ่านเกณฑ์' : 'ตกเกณฑ์'}
                            </span>
                            <span class="text-xs text-slate-400">เป้าหมาย: ${card.target}</span>
                        </div>
                    </div>
                `;
                container.appendChild(el);
            });

            renderStatusTable(summary);
            renderCharts();
            lucide.createIcons();
        }

        function renderStatusTable(summary) {
            const tbody = document.getElementById('kpi-status-table-body');
            tbody.innerHTML = '';

            KPI_METRICS_LIST.forEach(m => {
                let currentVal = '-';
                let isPass = true;

                if (m.id === 'kpi1') { currentVal = `${summary.rejectRate}%`; isPass = summary.rejectRate < m.targetVal; }
                else if (m.id === 'kpi5') { currentVal = `${summary.errorRate}%`; isPass = summary.errorRate <= m.targetVal; }
                else if (m.id === 'kpi6') { currentVal = `${summary.criticalRate}%`; isPass = summary.criticalRate >= m.targetVal; }
                else if (m.id === 'kpi7') { currentVal = `${summary.urgentTatRate}%`; isPass = summary.urgentTatRate >= m.targetVal; }
                else if (m.id === 'kpi8') { currentVal = `${summary.routineTatRate}%`; isPass = summary.routineTatRate >= m.targetVal; }
                else { currentVal = 'ปกติ'; isPass = true; }

                const row = document.createElement('tr');
                row.className = 'hover:bg-slate-50 transition';
                row.innerHTML = `
                    <td class="py-3 px-4 font-semibold text-xs text-slate-500">${m.code}</td>
                    <td class="py-3 px-4 font-medium">${m.name}</td>
                    <td class="py-3 px-4 text-center text-xs font-medium text-slate-600">${m.targetStr}</td>
                    <td class="py-3 px-4 text-center font-bold text-slate-800">${currentVal}</td>
                    <td class="py-3 px-4 text-center">
                        <span class="inline-block px-2.5 py-0.5 rounded-full text-xs font-semibold ${isPass ? 'bg-emerald-100 text-emerald-800' : 'bg-rose-100 text-rose-800'}">
                            ${isPass ? 'ตามเกณฑ์' : 'ต้องปรับปรุง'}
                        </span>
                    </td>
                    <td class="py-3 px-4 text-slate-500 text-xs">${m.owner}</td>
                `;
                tbody.appendChild(row);
            });
        }

        // Render Canvas Charts
        function renderCharts() {
            // Rejection Doughnut Chart
            const categoryCounts = {};
            db.incidents.forEach(item => {
                categoryCounts[item.category] = (categoryCounts[item.category] || 0) + Number(item.count);
            });

            const rejLabels = Object.keys(categoryCounts).length > 0 ? Object.keys(categoryCounts) : ['สิ่งส่งตรวจไม่เหมาะสม', 'คำสั่งผิดพลาด', 'ขอยกเลิก'];
            const rejData = Object.keys(categoryCounts).length > 0 ? Object.values(categoryCounts) : [12, 5, 3];

            if (rejectionChartInstance) rejectionChartInstance.destroy();
            const ctx1 = document.getElementById('rejectionChart').getContext('2d');
            rejectionChartInstance = new Chart(ctx1, {
                type: 'doughnut',
                data: {
                    labels: rejLabels,
                    datasets: [{
                        data: rejData,
                        backgroundColor: ['#0d9488', '#0284c7', '#f59e0b', '#e11d48', '#8b5cf6']
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });

            // TAT Monthly Line Chart
            const urgentTatData = MONTHS_KEY.map(m => {
                const d = db.monthly[m];
                return d.urgentTotal > 0 ? ((d.urgentOnTime / d.urgentTotal) * 100).toFixed(1) : 85;
            });

            const routineTatData = MONTHS_KEY.map(m => {
                const d = db.monthly[m];
                return d.routineTotal > 0 ? ((d.routineOnTime / d.routineTotal) * 100).toFixed(1) : 92;
            });

            if (tatChartInstance) tatChartInstance.destroy();
            const ctx2 = document.getElementById('tatChart').getContext('2d');
            tatChartInstance = new Chart(ctx2, {
                type: 'line',
                data: {
                    labels: ['ต.ค.', 'พ.ย.', 'ธ.ค.', 'ม.ค.', 'ก.พ.', 'มี.ค.', 'เม.ย.', 'พ.ค.', 'มิ.ย.', 'ก.ค.', 'ส.ค.', 'ก.ย.'],
                    datasets: [
                        { label: 'ผลด่วน (Urgent %)', data: urgentTatData, borderColor: '#0284c7', tension: 0.3, fill: false },
                        { label: 'ผลปกติ (Routine %)', data: routineTatData, borderColor: '#0d9488', tension: 0.3, fill: false }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: { y: { min: 50, max: 100 } }
                }
            });
        }

        // Form Loader for Selected Month
        function loadMonthlyDataForm() {
            const month = document.getElementById('entryMonthSelect').value;
            const data = db.monthly[month] || {};
            const container = document.getElementById('form-fields-container');

            container.innerHTML = `
                <div class="space-y-3 bg-slate-50 p-4 rounded-lg border">
                    <h4 class="font-bold text-teal-800 text-sm">1. ข้อมูลปริมาณสิ่งส่งตรวจและการปฏิเสธ</h4>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700">จำนวนสิ่งส่งตรวจทั้งหมด (Specimens)</label>
                        <input type="number" id="inp_totalSpecimens" value="${data.totalSpecimens || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700">จำนวนการปฏิเสธสิ่งส่งตรวจ (Rejections)</label>
                        <input type="number" id="inp_rejections" value="${data.rejections || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                    </div>
                </div>

                <div class="space-y-3 bg-slate-50 p-4 rounded-lg border">
                    <h4 class="font-bold text-teal-800 text-sm">2. ข้อมูลการรายงานผลตามเวลา (TAT)</h4>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">ผลด่วน (ทั้งหมด)</label>
                            <input type="number" id="inp_urgentTotal" value="${data.urgentTotal || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">ผลด่วน (ทันเวลา)</label>
                            <input type="number" id="inp_urgentOnTime" value="${data.urgentOnTime || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">ผลปกติ (ทั้งหมด)</label>
                            <input type="number" id="inp_routineTotal" value="${data.routineTotal || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">ผลปกติ (ทันเวลา)</label>
                            <input type="number" id="inp_routineOnTime" value="${data.routineOnTime || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                    </div>
                </div>

                <div class="space-y-3 bg-slate-50 p-4 rounded-lg border">
                    <h4 class="font-bold text-teal-800 text-sm">3. การรายงานผลผิดพลาด & ค่าวิกฤติ</h4>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700">จำนวนรายงานผลผิดพลาด (Miss)</label>
                        <input type="number" id="inp_reportingErrors" value="${data.reportingErrors || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">ค่าวิกฤติที่พบ</label>
                            <input type="number" id="inp_criticalFound" value="${data.criticalFound || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-700">รายงานทัน 5 นาที</label>
                            <input type="number" id="inp_criticalReported5Min" value="${data.criticalReported5Min || 0}" class="w-full bg-white border rounded p-2 text-sm mt-1">
                        </div>
                    </div>
                </div>
            `;
        }

        function saveMonthlyData(e) {
            e.preventDefault();
            const month = document.getElementById('entryMonthSelect').value;
            
            db.monthly[month] = {
                ...db.monthly[month],
                totalSpecimens: parseInt(document.getElementById('inp_totalSpecimens').value) || 0,
                rejections: parseInt(document.getElementById('inp_rejections').value) || 0,
                urgentTotal: parseInt(document.getElementById('inp_urgentTotal').value) || 0,
                urgentOnTime: parseInt(document.getElementById('inp_urgentOnTime').value) || 0,
                routineTotal: parseInt(document.getElementById('inp_routineTotal').value) || 0,
                routineOnTime: parseInt(document.getElementById('inp_routineOnTime').value) || 0,
                reportingErrors: parseInt(document.getElementById('inp_reportingErrors').value) || 0,
                criticalFound: parseInt(document.getElementById('inp_criticalFound').value) || 0,
                criticalReported5Min: parseInt(document.getElementById('inp_criticalReported5Min').value) || 0
            };

            saveYearData();
            alert(`บันทึกข้อมูลเดือน ${month.toUpperCase()} สำหรับปีงบประมาณ ${currentFiscalYear} เรียบร้อยแล้ว`);
        }

        // Incident Management Log
        function addIncidentLog(e) {
            e.preventDefault();
            const log = {
                id: Date.now(),
                month: document.getElementById('incMonth').value,
                category: document.getElementById('incCategory').value,
                cause: document.getElementById('incCause').value,
                count: parseInt(document.getElementById('incCount').value) || 1
            };

            db.incidents.push(log);
            saveYearData();
            renderIncidentLogs();
            document.getElementById('incCause').value = '';
        }

        function removeIncidentLog(id) {
            db.incidents = db.incidents.filter(i => i.id !== id);
            saveYearData();
            renderIncidentLogs();
        }

        function renderIncidentLogs() {
            const tbody = document.getElementById('incident-table-body');
            tbody.innerHTML = '';

            db.incidents.forEach(item => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-50';
                tr.innerHTML = `
                    <td class="py-2 px-3 font-semibold uppercase text-teal-700">${item.month}</td>
                    <td class="py-2 px-3">${item.category}</td>
                    <td class="py-2 px-3">${item.cause}</td>
                    <td class="py-2 px-3 text-center font-bold">${item.count}</td>
                    <td class="py-2 px-3 text-center">
                        <button onclick="removeIncidentLog(${item.id})" class="text-rose-600 hover:text-rose-800">
                            <i data-lucide="trash-2" class="w-4 h-4 inline"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
            lucide.createIcons();
        }

        // Render Matrix Yearly Report Table
        function renderMatrixReport() {
            const tbody = document.getElementById('matrix-table-body');
            tbody.innerHTML = '';

            // Metric 1: Specimen Rejection Rate Row
            let rejRow = `<tr class="hover:bg-slate-50 font-medium"><td class="border p-2">อัตราการปฏิเสธสิ่งส่งตรวจ (%)</td><td class="border p-2 text-center text-slate-500">< 2%</td>`;
            let totalSpec = 0, totalRej = 0;
            MONTHS_KEY.forEach(m => {
                const spec = db.monthly[m].totalSpecimens || 0;
                const rej = db.monthly[m].rejections || 0;
                totalSpec += spec; totalRej += rej;
                const rate = spec > 0 ? ((rej / spec) * 100).toFixed(2) : '-';
                rejRow += `<td class="border p-1 text-center">${rate}</td>`;
            });
            const yrRejRate = totalSpec > 0 ? ((totalRej / totalSpec) * 100).toFixed(2) : '0.00';
            rejRow += `<td class="border p-2 text-center font-bold bg-teal-50 text-teal-800">${yrRejRate}%</td></tr>`;

            // Metric 2: TAT Urgent Rate Row
            let tatUrgRow = `<tr class="hover:bg-slate-50 font-medium"><td class="border p-2">TAT รายงานผลด่วนทันเวลา (%)</td><td class="border p-2 text-center text-slate-500">≥ 80%</td>`;
            let totUrg = 0, totUrgTime = 0;
            MONTHS_KEY.forEach(m => {
                const tot = db.monthly[m].urgentTotal || 0;
                const time = db.monthly[m].urgentOnTime || 0;
                totUrg += tot; totUrgTime += time;
                const rate = tot > 0 ? ((time / tot) * 100).toFixed(1) : '-';
                tatUrgRow += `<td class="border p-1 text-center">${rate}</td>`;
            });
            const yrTatUrg = totUrg > 0 ? ((totUrgTime / totUrg) * 100).toFixed(1) : '0.0';
            tatUrgRow += `<td class="border p-2 text-center font-bold bg-teal-50 text-teal-800">${yrTatUrg}%</td></tr>`;

            tbody.innerHTML = rejRow + tatUrgRow;
        }

        // Export Data to CSV
        function exportYearlySummaryCSV() {
            const summary = calculateKPISummary();
            let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
            csvContent += `รายงานสรุปตัวชี้วัดกลุ่มงานเทคนิคการแพทย์ ปีงบประมาณ ${currentFiscalYear}\n\n`;
            csvContent += "Metric,Target,Result,Status\n";
            csvContent += `Specimen Rejection Rate,< 2%,${summary.rejectRate}%,${summary.rejectRate < 2 ? 'PASS' : 'FAIL'}\n`;
            csvContent += `Reporting Error Rate,<= 0.1%,${summary.errorRate}%,${summary.errorRate <= 0.1 ? 'PASS' : 'FAIL'}\n`;
            csvContent += `TAT Urgent Rate,>= 80%,${summary.urgentTatRate}%,${summary.urgentTatRate >= 80 ? 'PASS' : 'FAIL'}\n`;

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Lab_KPI_Summary_FY${currentFiscalYear}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>
</body>
</html>
