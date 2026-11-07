[index.html](https://github.com/user-attachments/files/34506197/index.html)
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>�к������èѴ�����кѹ�֡��Ǫ���Ѵ (Lab KPI Multi-Year System)</title>
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
                    <h1 class="font-bold text-lg leading-tight">�к��ѹ�֡��еԴ�����Ǫ���Ѵ�س�Ҿ (Lab KPIs)</h1>
                    <p class="text-xs text-teal-200">������ҹ෤�Ԥ���ᾷ�� � �ͧ�Ѻ��������»է�����ҳ</p>
                </div>
            </div>

            <!-- Global Year Selector & Navigation -->
            <div class="flex flex-wrap items-center gap-3">
                <div class="flex items-center bg-teal-800 rounded-lg px-3 py-1.5 border border-teal-600">
                    <i data-lucide="calendar" class="w-4 h-4 text-teal-200 mr-2"></i>
                    <span class="text-xs font-semibold mr-2 text-teal-100">�է�����ҳ:</span>
                    <select id="globalYearSelect" onchange="changeFiscalYear(this.value)" class="bg-teal-900 text-white text-sm font-bold rounded px-2 py-0.5 focus:outline-none focus:ring-2 focus:ring-teal-400">
                        <!-- Options generated dynamically -->
                    </select>
                </div>

                <nav class="flex bg-teal-800 rounded-lg p-1 text-sm">
                    <button onclick="switchTab('dashboard')" id="btn-dashboard" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 bg-white text-teal-800 shadow-sm">
                        <i data-lucide="layout-dashboard" class="w-4 h-4"></i> �Ҿ���
                    </button>
                    <button onclick="switchTab('data-entry')" id="btn-data-entry" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="file-edit" class="w-4 h-4"></i> ŧ�����������͹
                    </button>
                    <button onclick="switchTab('incidents')" id="btn-incidents" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="alert-triangle" class="w-4 h-4"></i> ���˵ء�û���ʸ
                    </button>
                    <button onclick="switchTab('annual-report')" id="btn-annual-report" class="tab-btn px-3 py-1.5 rounded-md font-medium transition flex items-center gap-1.5 text-teal-100 hover:bg-teal-700">
                        <i data-lucide="table" class="w-4 h-4"></i> ��ػ��Шӻ�
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
                        <span>��ػ�Ҿ�����Ǫ���Ѵ �է�����ҳ</span>
                        <span id="dash-year-label" class="text-teal-600 bg-teal-50 px-3 py-0.5 rounded-full text-lg border border-teal-200">2568</span>
                    </h2>
                    <p class="text-xs text-slate-500 mt-0.5">�����Ťӹǳ����������������ѡ�ͧ������ҹ</p>
                </div>
                <button onclick="exportYearlySummaryCSV()" class="flex items-center gap-1.5 text-xs font-semibold bg-emerald-600 text-white px-3 py-2 rounded-lg hover:bg-emerald-700
