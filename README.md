<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>إنجاز ⚡</title>

    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/785/785116.png">
    <link rel="icon" href="https://cdn-icons-png.flaticon.com/512/785/785116.png">

    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body { font-family: 'Tajawal', sans-serif; background-color: #f1f5f9; color: #1e293b; user-select: none; -webkit-tap-highlight-color: transparent; }
        .tablet-container { display: grid; grid-template-columns: 280px 1fr; height: 100vh; }
        .sidebar { background: white; border-left: 1px solid #e2e8f0; display: flex; flex-direction: column; padding: 1.5rem 1rem; z-index: 50; }
        .content-area { overflow-y: auto; padding: 2.5rem; background: #f8fafc; }
        .font-luxury { font-weight: 900; letter-spacing: -0.5px; }
        .target-card { background: linear-gradient(135deg, #6366f1 0%, #4338ca 100%); border-radius: 2rem; padding: 1.5rem; color: white; position: relative; overflow: hidden; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); }
        .calendar-bg-icon { position: absolute; left: -10px; bottom: -20px; width: 120px; height: 120px; opacity: 0.15; transform: rotate(-15deg); color: white; pointer-events: none; }
        .subject-page { display: none; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 1.5rem; }
        .subject-page.active { display: grid; animation: slideUp 0.4s ease-out; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .nav-link { display: flex; align-items: center; gap: 1rem; padding: 0.8rem 1.2rem; border-radius: 1.2rem; color: #64748b; font-weight: 800; margin-bottom: 0.3rem; transition: 0.3s; }
        .nav-link.active.math { background: #eff6ff; color: #2563eb; }
        .nav-link.active.physics { background: #fffbeb; color: #d97706; }
        .nav-link.active.chem { background: #fff7ed; color: #ea580c; }
        .nav-link.active.bio { background: #f0fdf4; color: #16a34a; }
        .day-card { background: white; border-radius: 1.8rem; padding: 1.5rem; border: 1px solid #e2e8f0; }
        .day-card.completed { border-color: #10b981; background: #f0fdf4; }
        
        /* خاصية منع تأخير اللمس */
        .action-btn { touch-action: manipulation; transition: all 0.1s; }
        .action-btn:active { transform: scale(0.9); }
        
        .ai-box { background: #0f172a; border-radius: 1.5rem; padding: 1.2rem; color: white; margin-bottom: 1.2rem; border: 1px solid rgba(255,255,255,0.1); }
        .date-input { position: absolute; inset: 0; opacity: 0; cursor: pointer; z-index: 20; }
        .reset-btn { margin-top: 1rem; padding: 0.7rem; border-radius: 1rem; color: #ef4444; font-size: 0.75rem; font-weight: 800; display: flex; align-items: center; justify-content: center; gap: 0.5rem; border: 1px solid #fee2e2; }
    </style>
</head>
<body>

    <div class="tablet-container">
        <aside class="sidebar">
            <div class="flex items-center gap-3 px-4 mb-6">
                <div class="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center shadow-lg shadow-indigo-200">
                    <i data-lucide="zap" class="text-white w-6 h-6 fill-current"></i>
                </div>
                <h1 class="text-xl font-luxury text-slate-800 tracking-tighter">إنجاز <span class="text-indigo-600">35</span></h1>
            </div>

            <div class="mb-5 p-5 bg-slate-900 rounded-[2rem] text-white text-center shadow-xl relative overflow-hidden">
                <p class="text-[9px] font-black opacity-40 uppercase tracking-widest mb-1">الإنجاز الإجمالي</p>
                <div class="flex items-center justify-center gap-2">
                    <span id="total-done-pages" class="text-4xl font-luxury italic">0</span>
                    <div class="text-right">
                        <p class="text-[10px] opacity-40 font-bold">/ 338 ص</p>
                        <p id="total-pct-label" class="text-indigo-400 text-[11px] font-black">0%</p>
                    </div>
                </div>
            </div>

            <div id="ai-container" class="ai-box">
                <div class="flex items-center gap-2 mb-2">
                    <i data-lucide="sparkles" class="w-4 h-4 text-indigo-400"></i>
                    <span class="text-[9px] font-black uppercase text-indigo-400">الذكاء الاصطناعي</span>
                </div>
                <p id="ai-message" class="text-[10px] font-bold leading-relaxed opacity-90">جاهز لتحليل خطة الـ 35 يوماً..</p>
            </div>

            <nav class="flex-1 px-1 overflow-y-auto">
                <button onclick="tab('math')" id="b-math" class="nav-link active math w-full text-right"><i data-lucide="layout-grid"></i><span>الرياضيات</span></button>
                <button onclick="tab('physics')" id="b-physics" class="nav-link physics w-full text-right"><i data-lucide="zap"></i><span>الفيزياء</span></button>
                <button onclick="tab('chem')" id="b-chem" class="nav-link chem w-full text-right"><i data-lucide="beaker"></i><span>الكيمياء</span></button>
                <button onclick="tab('bio')" id="b-bio" class="nav-link bio w-full text-right"><i data-lucide="dna"></i><span>الأحياء</span></button>
            </nav>

            <div class="mt-auto space-y-2 pt-4 border-t">
                <div class="relative bg-orange-50/50 p-2 rounded-xl border border-orange-100 text-center">
                    <input type="date" id="exam1-date" onchange="updateUI()" class="date-input">
                    <p class="text-[8px] font-black text-orange-400 uppercase">الاختبار 1</p>
                    <p id="cd1" class="text-lg font-luxury text-orange-600">--</p>
                </div>
                <div class="relative bg-blue-50/50 p-2 rounded-xl border border-blue-100 text-center">
                    <input type="date" id="exam2-date" onchange="updateUI()" class="date-input">
                    <p class="text-[8px] font-black text-blue-400 uppercase">الاختبار 2</p>
                    <p id="cd2" class="text-lg font-luxury text-blue-600">--</p>
                </div>
                <button onclick="resetData()" class="reset-btn w-full">
                    <i data-lucide="refresh-cw" class="w-4 h-4"></i> تصفير الخطة
                </button>
            </div>
        </aside>

        <main class="content-area">
            <div class="grid grid-cols-3 gap-6 mb-10">
                <div class="target-card">
                    <i data-lucide="calendar" class="calendar-bg-icon"></i>
                    <input type="date" id="target-finish" onchange="updateUI()" class="date-input">
                    <div class="relative z-10 text-right">
                        <div class="flex items-center gap-2 mb-2 opacity-80">
                            <i data-lucide="target" class="w-3 h-3"></i>
                            <span class="text-[10px] font-black uppercase tracking-widest">هدف الختم</span>
                        </div>
                        <p id="target-display" class="text-2xl font-luxury italic">--</p>
                    </div>
                </div>

                <div class="bg-white rounded-[2rem] p-6 shadow-sm border border-slate-200 text-right">
                    <span class="text-[10px] font-black text-slate-400 uppercase mb-2">الحالة الراهنة</span>
                    <p id="status-title" class="text-2xl font-luxury text-slate-800 italic">--</p>
                </div>

                <div class="bg-white rounded-[2rem] p-6 shadow-sm border border-slate-200 text-right">
                    <span class="text-[10px] font-black text-slate-400 uppercase">أيام الأمان</span>
                    <div class="flex justify-between mt-3">
                        <div class="text-center flex-1 border-l">
                            <p id="safety-1" class="text-3xl font-luxury text-slate-300">--</p>
                            <p class="text-[8px] font-black text-orange-500">للاختبار 1</p>
                        </div>
                        <div class="text-center flex-1">
                            <p id="safety-2" class="text-3xl font-luxury text-slate-300">--</p>
                            <p class="text-[8px] font-black text-blue-500">للاختبار 2</p>
                        </div>
                    </div>
                </div>
            </div>

            <div id="p-math" class="subject-page active"></div>
            <div id="p-physics" class="subject-page"></div>
            <div id="p-chem" class="subject-page"></div>
            <div id="p-bio" class="subject-page"></div>
        </main>
    </div>

    <script>
        const plan = {
            math: { start: 1, color: 'indigo', ranges: ["92-99", "100-107", "108-115", "116-123", "124-131", "132-139", "140-147", "148-155", "156-163", "164-171", "172-177"] },
            physics: { start: 12, color: 'amber', ranges: ["8-17", "18-27", "28-37", "38-47", "48-57", "58-67", "68-77", "78-89"] },
            chem: { start: 20, color: 'orange', ranges: ["180-189", "190-199", "200-209", "210-219", "220-229", "230-239", "240-250", "251-262"] },
            bio: { start: 28, color: 'emerald', ranges: ["266-276", "277-287", "288-298", "299-309", "310-320", "321-331", "332-342", "343-352"] }
        };

        let storage = JSON.parse(localStorage.getItem('study_v35_final')) || { prog: {}, targetDate: '', exam1Date: '', exam2Date: '' };
        
        function getPages(rng) { const p = rng.split('-'); return parseInt(p[1]) - parseInt(p[0]) + 1; }

        function init() {
            lucide.createIcons();
            document.getElementById('target-finish').value = storage.targetDate || '';
            document.getElementById('exam1-date').value = storage.exam1Date || '';
            document.getElementById('exam2-date').value = storage.exam2Date || '';
            Object.keys(plan).forEach(sub => {
                const container = document.getElementById(`p-${sub}`);
                container.innerHTML = '';
                plan[sub].ranges.forEach((rng, i) => {
                    const dId = plan[sub].start + i;
                    const total = getPages(rng);
                    const cur = storage.prog[dId] || 0;
                    container.innerHTML += `
                        <div class="day-card ${cur >= total ? 'completed' : ''}" id="card-${dId}">
                            <div class="flex justify-between items-start mb-6 text-right">
                                <span class="bg-slate-100 w-9 h-9 rounded-xl flex items-center justify-center font-black text-slate-400 text-[10px]">يوم ${dId}</span>
                                <div>
                                    <p class="text-xl font-luxury text-slate-800">صـ ${rng}</p>
                                    <p id="stats-${dId}" class="text-[10px] font-bold text-slate-400 italic text-right">${cur} / ${total}</p>
                                </div>
                            </div>
                            <div class="flex items-center justify-between gap-4">
                                <button onclick="changeVal(${dId},-1,${total})" class="action-btn w-12 h-12 rounded-xl bg-slate-100 flex items-center justify-center text-slate-400"><i data-lucide="minus"></i></button>
                                <span class="text-3xl font-luxury text-slate-700" id="count-${dId}">${cur}</span>
                                <button onclick="changeVal(${dId},1,${total})" class="action-btn w-12 h-12 rounded-xl bg-${plan[sub].color}-500 flex items-center justify-center text-white shadow-lg"><i data-lucide="plus"></i></button>
                            </div>
                        </div>`;
                });
            });
            lucide.createIcons();
            updateUI();
        }

        function changeVal(id, delta, total) {
            let val = (storage.prog[id] || 0) + delta;
            if (val < 0) val = 0; if (val > total) val = total;
            storage.prog[id] = val;
            document.getElementById(`count-${id}`).innerText = val;
            document.getElementById(`stats-${id}`).innerText = `${val} / ${total}`;
            const card = document.getElementById(`card-${id}`);
            if (val >= total) card.classList.add('completed'); else card.classList.remove('completed');
            updateUI();
        }

        function tab(s) { document.querySelectorAll('.subject-page, .nav-link').forEach(el => el.classList.remove('active')); document.getElementById(`p-${s}`).classList.add('active'); document.getElementById(`b-${s}`).classList.add('active'); }

        function updateUI() {
            let totalDone = 0, daysRemaining = 0;
            Object.keys(plan).forEach(sub => {
                plan[sub].ranges.forEach((rng, i) => {
                    const dId = plan[sub].start + i;
                    const val = storage.prog[dId] || 0;
                    totalDone += val;
                    if (val < getPages(rng)) daysRemaining++;
                });
            });

            document.getElementById('total-done-pages').innerText = totalDone;
            document.getElementById('total-pct-label').innerText = Math.round((totalDone / 338) * 100) + '%';
            
            storage.targetDate = document.getElementById('target-finish').value;
            storage.exam1Date = document.getElementById('exam1-date').value;
            storage.exam2Date = document.getElementById('exam2-date').value;

            const now = new Date(); now.setHours(0,0,0,0);
            const estFinish = new Date(now); estFinish.setDate(now.getDate() + daysRemaining);

            const updateCounter = (id, dateStr) => {
                const el = document.getElementById(id);
                if (!dateStr) { el.innerText = '--'; return null; }
                const diff = Math.ceil((new Date(dateStr) - estFinish) / 864e5);
                el.innerText = diff;
                el.className = `text-3xl font-luxury ${diff > 0 ? 'text-emerald-500' : (diff < 0 ? 'text-rose-500' : 'text-blue-500')}`;
                return diff;
            };

            updateCounter('safety-1', storage.exam1Date);
            updateCounter('safety-2', storage.exam2Date);

            if (storage.targetDate) {
                const diffT = Math.ceil((new Date(storage.targetDate) - estFinish) / 864e5);
                document.getElementById('target-display').innerText = new Date(storage.targetDate).toLocaleDateString('ar-SA', { month: 'short', day: 'numeric' });
                const st = document.getElementById('status-title');
                st.innerText = diffT >= 0 ? `سابق بـ ${diffT} يوم` : `متأخر بـ ${Math.abs(diffT)} يوم`;
                st.className = `text-2xl font-luxury italic ${diffT >= 0 ? 'text-emerald-600' : 'text-rose-600'}`;
            }

            const setCD = (id, date) => {
                const el = document.getElementById(id);
                if(!date) { el.innerText = '--'; return; }
                el.innerText = Math.max(0, Math.ceil((new Date(date) - now) / 864e5)) + " يوم";
            };
            setCD('cd1', storage.exam1Date);
            setCD('cd2', storage.exam2Date);

            localStorage.setItem('study_v35_final', JSON.stringify(storage));
        }

        function resetData() { if (confirm('تصفير الخطة؟')) { localStorage.clear(); location.reload(); } }
        init();
    </script>
</body>
</html>        .nav-link.active.bio { background: #f0fdf4; color: #16a34a; }
        .day-card { background: white; border-radius: 1.8rem; padding: 1.5rem; border: 1px solid #e2e8f0; }
        .day-card.completed { border-color: #10b981; background: #f0fdf4; }
        .action-btn { touch-action: manipulation; transition: all 0.1s; }
        .action-btn:active { transform: scale(0.9); }
        .ai-box { background: #0f172a; border-radius: 1.5rem; padding: 1.2rem; color: white; margin-bottom: 1.2rem; border: 1px solid rgba(255,255,255,0.1); }
        .date-input { position: absolute; inset: 0; opacity: 0; cursor: pointer; z-index: 20; }
        .reset-btn { margin-top: 1rem; padding: 0.7rem; border-radius: 1rem; color: #ef4444; font-size: 0.75rem; font-weight: 800; display: flex; align-items: center; justify-content: center; gap: 0.5rem; border: 1px solid #fee2e2; }
    </style>
</head>
<body>

    <div class="tablet-container">
        <aside class="sidebar">
            <div class="flex items-center gap-3 px-4 mb-6">
                <div class="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center shadow-lg shadow-indigo-200">
                    <i data-lucide="zap" class="text-white w-6 h-6 fill-current"></i>
                </div>
                <h1 class="text-xl font-luxury text-slate-800 tracking-tighter">إنجاز <span class="text-indigo-600">35</span></h1>
            </div>

            <div class="mb-5 p-5 bg-slate-900 rounded-[2rem] text-white text-center shadow-xl relative overflow-hidden">
                <p class="text-[9px] font-black opacity-40 uppercase tracking-widest mb-1">الإنجاز الإجمالي</p>
                <div class="flex items-center justify-center gap-2">
                    <span id="total-done-pages" class="text-4xl font-luxury italic">0</span>
                    <div class="text-right">
                        <p class="text-[10px] opacity-40 font-bold">/ 338 ص</p>
                        <p id="total-pct-label" class="text-indigo-400 text-[11px] font-black">0%</p>
                    </div>
                </div>
            </div>

            <div id="ai-container" class="ai-box">
                <div class="flex items-center gap-2 mb-2">
                    <i data-lucide="sparkles" class="w-4 h-4 text-indigo-400"></i>
                    <span class="text-[9px] font-black uppercase text-indigo-400">الذكاء الاصطناعي</span>
                </div>
                <p id="ai-message" class="text-[10px] font-bold leading-relaxed opacity-90">جاهز لتحليل خطة الـ 35 يوماً..</p>
            </div>

            <nav class="flex-1 px-1 overflow-y-auto">
                <button onclick="tab('math')" id="b-math" class="nav-link active math w-full"><i data-lucide="layout-grid"></i><span>الرياضيات</span></button>
                <button onclick="tab('physics')" id="b-physics" class="nav-link physics w-full"><i data-lucide="zap"></i><span>الفيزياء</span></button>
                <button onclick="tab('chem')" id="b-chem" class="nav-link chem w-full"><i data-lucide="beaker"></i><span>الكيمياء</span></button>
                <button onclick="tab('bio')" id="b-bio" class="nav-link bio w-full"><i data-lucide="dna"></i><span>الأحياء</span></button>
            </nav>

            <div class="mt-auto space-y-2 pt-4 border-t">
                <div class="relative bg-orange-50/50 p-2 rounded-xl border border-orange-100 text-center">
                    <input type="date" id="exam1-date" onchange="updateUI()" class="date-input">
                    <p class="text-[8px] font-black text-orange-400 uppercase">الاختبار 1</p>
                    <p id="cd1" class="text-lg font-luxury text-orange-600">--</p>
                </div>
                <div class="relative bg-blue-50/50 p-2 rounded-xl border border-blue-100 text-center">
                    <input type="date" id="exam2-date" onchange="updateUI()" class="date-input">
                    <p class="text-[8px] font-black text-blue-400 uppercase">الاختبار 2</p>
                    <p id="cd2" class="text-lg font-luxury text-blue-600">--</p>
                </div>
                <button onclick="resetData()" class="reset-btn w-full">
                    <i data-lucide="refresh-cw" class="w-4 h-4"></i> تصفير الخطة
                </button>
            </div>
        </aside>

        <main class="content-area">
            <div class="grid grid-cols-3 gap-6 mb-10">
                <div class="target-card">
                    <i data-lucide="calendar" class="calendar-bg-icon"></i>
                    <input type="date" id="target-finish" onchange="updateUI()" class="date-input">
                    <div class="relative z-10 text-right">
                        <div class="flex items-center gap-2 mb-2 opacity-80">
                            <i data-lucide="target" class="w-3 h-3"></i>
                            <span class="text-[10px] font-black uppercase tracking-widest">هدف الختم</span>
                        </div>
                        <p id="target-display" class="text-2xl font-luxury italic">--</p>
                    </div>
                </div>

                <div class="bg-white rounded-[2rem] p-6 shadow-sm border border-slate-200 text-right">
                    <span class="text-[10px] font-black text-slate-400 uppercase mb-2">الحالة الراهنة</span>
                    <p id="status-title" class="text-2xl font-luxury text-slate-800 italic">--</p>
                </div>

                <div class="bg-white rounded-[2rem] p-6 shadow-sm border border-slate-200 text-right">
                    <span class="text-[10px] font-black text-slate-400 uppercase">أيام الأمان</span>
                    <div class="flex justify-between mt-3">
                        <div class="text-center flex-1 border-l">
                            <p id="safety-1" class="text-3xl font-luxury text-slate-300">--</p>
                            <p class="text-[8px] font-black text-orange-500">للاختبار 1</p>
                        </div>
                        <div class="text-center flex-1">
                            <p id="safety-2" class="text-3xl font-luxury text-slate-300">--</p>
                            <p class="text-[8px] font-black text-blue-500">للاختبار 2</p>
                        </div>
                    </div>
                </div>
            </div>

            <div id="p-math" class="subject-page active"></div>
            <div id="p-physics" class="subject-page"></div>
            <div id="p-chem" class="subject-page"></div>
            <div id="p-bio" class="subject-page"></div>
        </main>
    </div>

    <script>
        // الخطة الموزعة على 35 يوماً
        const plan = {
            math: { start: 1, color: 'indigo', ranges: ["92-99", "100-107", "108-115", "116-123", "124-131", "132-139", "140-147", "148-155", "156-163", "164-171", "172-177"] },
            physics: { start: 12, color: 'amber', ranges: ["8-17", "18-27", "28-37", "38-47", "48-57", "58-67", "68-77", "78-89"] },
            chem: { start: 20, color: 'orange', ranges: ["180-189", "190-199", "200-209", "210-219", "220-229", "230-239", "240-250", "251-262"] },
            bio: { start: 28, color: 'emerald', ranges: ["266-276", "277-287", "288-298", "299-309", "310-320", "321-331", "332-342", "343-352"] }
        };

        const DEFAULTS = { prog: {}, targetDate: '', exam1Date: '', exam2Date: '' };
        let storage = JSON.parse(localStorage.getItem('study_v35_final')) || JSON.parse(JSON.stringify(DEFAULTS));
        
        let holdTimeout, holdInterval;
        let vSpeed = 200;

        function startHold(id, delta, total) { changeVal(id, delta, total); vSpeed = 200; holdTimeout = setTimeout(() => { holdInterval = setInterval(() => { changeVal(id, delta, total); if (vSpeed > 60) { clearInterval(holdInterval); vSpeed -= 15; holdInterval = setInterval(() => changeVal(id, delta, total), vSpeed); } }, vSpeed); }, 400); }
        function stopHold() { clearTimeout(holdTimeout); clearInterval(holdInterval); }
        function getPages(rng) { const p = rng.split('-'); return parseInt(p[1]) - parseInt(p[0]) + 1; }

        function init() {
            lucide.createIcons();
            document.getElementById('target-finish').value = storage.targetDate || '';
            document.getElementById('exam1-date').value = storage.exam1Date || '';
            document.getElementById('exam2-date').value = storage.exam2Date || '';
            Object.keys(plan).forEach(sub => {
                const container = document.getElementById(`p-${sub}`);
                container.innerHTML = '';
                plan[sub].ranges.forEach((rng, i) => {
                    const dId = plan[sub].start + i;
                    const total = getPages(rng);
                    const cur = storage.prog[dId] || 0;
                    container.innerHTML += `
                        <div class="day-card ${cur >= total ? 'completed' : ''}" id="card-${dId}">
                            <div class="flex justify-between items-start mb-6">
                                <span class="bg-slate-100 w-9 h-9 rounded-xl flex items-center justify-center font-black text-slate-400 text-[10px]">يوم ${dId}</span>
                                <div class="text-left">
                                    <p class="text-xl font-luxury text-slate-800">صـ ${rng}</p>
                                    <p id="stats-${dId}" class="text-[10px] font-bold text-slate-400 text-right italic">${cur} / ${total}</p>
                                </div>
                            </div>
                            <div class="flex items-center justify-between gap-4">
                                <button onmousedown="startHold(${dId},-1,${total})" onmouseup="stopHold()" ontouchstart="startHold(${dId},-1,${total})" ontouchend="stopHold()" class="action-btn w-12 h-12 rounded-xl bg-slate-100 flex items-center justify-center text-slate-400"><i data-lucide="minus"></i></button>
                                <span class="text-3xl font-luxury text-slate-700" id="count-${dId}">${cur}</span>
                                <button onmousedown="startHold(${dId},1,${total})" onmouseup="stopHold()" ontouchstart="startHold(${dId},1,${total})" ontouchend="stopHold()" class="action-btn w-12 h-12 rounded-xl bg-${plan[sub].color}-500 flex items-center justify-center text-white shadow-lg"><i data-lucide="plus"></i></button>
                            </div>
                        </div>`;
                });
            });
            updateUI();
        }

        function changeVal(id, delta, total) {
            let val = (storage.prog[id] || 0) + delta;
            if (val < 0) val = 0; if (val > total) val = total;
            storage.prog[id] = val;
            document.getElementById(`count-${id}`).innerText = val;
            document.getElementById(`stats-${id}`).innerText = `${val} / ${total}`;
            const card = document.getElementById(`card-${id}`);
            if (val >= total) card.classList.add('completed'); else card.classList.remove('completed');
            updateUI();
        }

        function tab(s) { document.querySelectorAll('.subject-page, .nav-link').forEach(el => el.classList.remove('active')); document.getElementById(`p-${s}`).classList.add('active'); document.getElementById(`b-${s}`).classList.add('active'); }

        function updateUI() {
            let totalDone = 0, daysRemaining = 0;
            Object.keys(plan).forEach(sub => {
                plan[sub].ranges.forEach((rng, i) => {
                    const dId = plan[sub].start + i;
                    const val = storage.prog[dId] || 0;
                    totalDone += val;
                    if (val < getPages(rng)) daysRemaining++;
                });
            });

            document.getElementById('total-done-pages').innerText = totalDone;
            document.getElementById('total-pct-label').innerText = Math.round((totalDone / 338) * 100) + '%';

            storage.targetDate = document.getElementById('target-finish').value;
            storage.exam1Date = document.getElementById('exam1-date').value;
            storage.exam2Date = document.getElementById('exam2-date').value;

            const now = new Date(); now.setHours(0,0,0,0);
            const estFinish = new Date(now); estFinish.setDate(now.getDate() + daysRemaining);

            const aiMsg = document.getElementById('ai-message');
            const updateCounter = (id, dateStr) => {
                const el = document.getElementById(id);
                if (!dateStr) { el.innerText = '--'; el.className = "text-3xl font-luxury text-slate-300"; return null; }
                const diff = Math.ceil((new Date(dateStr) - estFinish) / 864e5);
                el.innerText = diff;
                el.className = diff > 0 ? "text-3xl font-luxury text-emerald-500" : (diff < 0 ? "text-3xl font-luxury text-rose-500" : "text-3xl font-luxury text-blue-500");
                return diff;
            };

            const s1 = updateCounter('safety-1', storage.exam1Date);
            const s2 = updateCounter('safety-2', storage.exam2Date);

            if (!storage.exam1Date) {
                aiMsg.innerText = "أهلاً بك في تحدي الـ 35 يوماً. حدد مواعيد اختباراتك لنبدأ الحساب الفعلي.";
            } else {
                aiMsg.innerText = s1 < 0 ? "🚨 الخطة الحالية تتجاوز موعد الاختبار! تحتاج لزيادة الصفحات يومياً." : "✨ خطة الـ 35 يوماً تسير بشكل مثالي مع مواعيدك.";
            }

            const targetDisp = document.getElementById('target-display');
            const statusTitle = document.getElementById('status-title');
            if (storage.targetDate) {
                const diffT = Math.ceil((new Date(storage.targetDate) - estFinish) / 864e5);
                targetDisp.innerText = new Date(storage.targetDate).toLocaleDateString('ar-SA', { month: 'short', day: 'numeric' });
                statusTitle.innerText = diffT >= 0 ? `سابق بـ ${diffT} يوم` : `متأخر بـ ${Math.abs(diffT)} يوم`;
                statusTitle.className = diffT >= 0 ? "text-2xl font-luxury text-emerald-600 italic" : "text-2xl font-luxury text-rose-600 italic";
            }

            const setCD = (id, date) => {
                const el = document.getElementById(id);
                if(!date) { el.innerText = '--'; return; }
                const cd = Math.ceil((new Date(date) - now) / 864e5);
                el.innerText = Math.max(0, cd) + " يوم";
            };
            setCD('cd1', storage.exam1Date);
            setCD('cd2', storage.exam2Date);

            localStorage.setItem('study_v35_final', JSON.stringify(storage));
        }

        function resetData() { if (confirm('⚠️ هل تريد إعادة تصفير خطة الـ 35 يوماً؟')) { localStorage.removeItem('study_v35_final'); location.reload(); } }
        init();
    </script>
</body>
</html>
