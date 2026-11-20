<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>유튜브 주간 성과 및 수익 보고 대시보드</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js" crossorigin="anonymous"></script>
    <link rel="stylesheet" as="style" crossorigin="" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css">
    <style>
        body {
            font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
            background-color: #F4F7FA;
            color: #1E293B;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 100%;
            margin-left: auto;
            margin-right: auto;
            height: 280px;
            max-height: 340px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 300px;
            }
        }
        .tab-active {
            background-color: #0044CC;
            color: #FFFFFF;
            box-shadow: 0 4px 6px -1px rgba(0, 68, 204, 0.25);
        }
        .tab-inactive {
            background-color: #FFFFFF;
            color: #475569;
            border: 1px solid #E2E8F0;
        }
        .tab-inactive:hover {
            background-color: #F8FAFC;
            color: #0044CC;
        }
        .subtab-active {
            background-color: #0044CC;
            color: #FFFFFF;
        }
        .subtab-inactive {
            background-color: #F1F5F9;
            color: #475569;
        }
        .subtab-inactive:hover {
            background-color: #E2E8F0;
            color: #0F172A;
        }
        .custom-scrollbar::-webkit-scrollbar {
            height: 8px;
            width: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #F1F5F9;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #CBD5E1;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94A3B8;
        }
    </style>
</head>
<body class="antialiased pb-16">

    <header class="bg-gradient-to-r from-[#002B80] via-[#0044CC] to-[#0077FF] text-white shadow-lg sticky top-0 z-30">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4">
                <div>
                    <div class="flex items-center gap-2 text-xs font-semibold tracking-wider uppercase text-blue-200 mb-1">
                        <span>WEEKLY EXECUTIVE REPORT</span>
                        <span>•</span>
                        <span id="report-period-label">분석 기간: 2026.10.20 ~ 2026.11.17 (28일)</span>
                    </div>
                    <h1 class="text-xl sm:text-2xl font-extrabold tracking-tight">유튜브 채널 수익 및 콘텐츠 성과 대시보드</h1>
                </div>
                
                <div class="flex flex-wrap items-center gap-2.5">
                    <div id="fx-control-box" class="flex items-center gap-1.5 bg-white/15 px-3 py-1.5 rounded-xl border border-white/25 text-xs">
                        <span class="text-blue-100 font-semibold">적용 환율 1 USD =</span>
                        <input type="number" id="usd-krw-rate" value="1400" step="1" min="500" max="3000" oninput="onExchangeRateChange()" class="w-16 bg-white text-slate-900 font-extrabold text-right px-1.5 py-0.5 rounded focus:outline-none">
                        <span class="text-blue-100 font-semibold">원</span>
                    </div>

                    <div class="flex flex-wrap items-center gap-1.5 bg-white/10 p-1.5 rounded-xl backdrop-blur-sm border border-white/20">
                        <button onclick="switchChannel('all')" id="btn-all" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-inactive">
                            전체 통합
                        </button>
                        <button onclick="switchChannel('whatever')" id="btn-whatever" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-inactive">
                            뭐든하기루
                        </button>
                        <button onclick="switchChannel('banjiha')" id="btn-banjiha" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-active">
                            반지하 유정수
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6 space-y-8">
        <section>
            <div class="mb-4 flex flex-col sm:flex-row sm:items-center justify-between gap-2">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <span class="w-2 h-6 bg-[#0044CC] rounded-full inline-block"></span>
                        1. 핵심 성과 요약 (28일 전체 합계 · 원화 KRW 기준)
                    </h2>
                    <p class="text-sm text-slate-600 mt-1">
                        달러(USD) 기준 데이터는 상단 적용 환율을 곱해 원화(₩)로 자동 변환 후 합산한 최근 28일 전체 성과입니다.
                    </p>
                </div>
                <div id="currency-notice-badge" class="px-3 py-1.5 rounded-xl bg-amber-50 border border-amber-200 text-amber-800 text-xs font-bold">💱 USD 수익 채널 자동 원화 환산 적용 중 (1 USD = 1,400원)</div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-blue-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>추정 총 수익 (28일)</span>
                            <span class="text-lg">💰</span>
                        </div>
                        <div id="kpi-revenue" class="text-2xl sm:text-3xl font-extrabold text-[#0044CC] tracking-tight mb-3">₩1,404,708</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-rev-sub" class="text-slate-500 font-medium">총 172개 콘텐츠 합산</span>
                            <span id="kpi-rev-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">▼ -88.1% WoW</span>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-cyan-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>총 조회수 (28일)</span>
                            <span class="text-lg">👁️</span>
                        </div>
                        <div id="kpi-views" class="text-2xl sm:text-3xl font-extrabold text-slate-800 tracking-tight mb-3">941,937회</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-views-sub" class="text-slate-500 font-medium">영상당 평균 5,476회</span>
                            <span id="kpi-views-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">▼ -85.2% WoW</span>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-emerald-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>구독자 증감 (28일)</span>
                            <span class="text-lg">👥</span>
                        </div>
                        <div id="kpi-subs" class="text-2xl sm:text-3xl font-extrabold text-emerald-600 tracking-tight mb-3">+2,122명</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-subs-sub" class="text-slate-500 font-medium">주간 평균 531명 유입</span>
                            <span id="kpi-subs-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">▼ -91.6% WoW</span>
                        </div>
                    </div>
                </div>

                <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-indigo-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>총 시청 시간 (28일)</span>
                            <span class="text-lg">⏱️</span>
                        </div>
                        <div id="kpi-watch" class="text-2xl sm:text-3xl font-extrabold text-slate-800 tracking-tight mb-3">43,841시간</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-watch-sub" class="text-slate-500 font-medium">주간 평균 10,960시간</span>
                            <span id="kpi-watch-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">▼ -89.4% WoW</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="flex flex-col lg:flex-row lg:items-center justify-between mb-6 gap-4">
                <div>
                    <div class="flex flex-wrap items-center gap-2">
                        <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2 h-6 bg-[#0077FF] rounded-full inline-block"></span>
                            2. 주간별(WoW) 채널 수익 및 성과 비교표
                        </h2>
                        <span class="px-2.5 py-0.5 rounded-md text-xs font-extrabold bg-blue-100 text-[#0044CC]">
                            단순 주간별 실적 (비누적 · 7일 단위)
                        </span>
                    </div>
                    <p id="weekly-table-desc" class="text-sm text-slate-600 mt-1.5">
                        누적이 아닌 <strong>각 주차(7일) 동안 발생한 단독 실적</strong>과 해당 주차 신규 업로드 썸네일입니다. 우측 버튼으로 <strong>전체 / 롱폼 / 쇼츠</strong> 기준을 전환할 수 있습니다.
                    </p>
                </div>

                <div class="flex items-center gap-1.5 bg-slate-100 p-1 rounded-xl border border-slate-200 self-start lg:self-auto shrink-0">
                    <button onclick="switchWeeklyFormat('all')" id="wfmt-all" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all subtab-inactive">
                        전체 (롱폼+쇼츠)
                    </button>
                    <button onclick="switchWeeklyFormat('롱폼')" id="wfmt-long" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all subtab-active">
                        롱폼 기준
                    </button>
                    <button onclick="switchWeeklyFormat('쇼츠')" id="wfmt-shorts" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all subtab-inactive">
                        쇼츠 기준
                    </button>
                </div>
            </div>

            <div class="overflow-x-auto custom-scrollbar rounded-xl border border-slate-200">
                <table class="w-full text-sm text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-800 text-white text-xs uppercase tracking-wider">
                            <th class="py-3.5 px-4 font-bold whitespace-nowrap">보고 주차</th>
                            <th class="py-3.5 px-4 font-bold whitespace-nowrap">대상 기간 (7일)</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">신규 업로드</th>
                            <th class="py-3.5 px-4 font-bold min-w-[180px]">업로드 썸네일</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">주간 조회수</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">주간 구독자 증감</th>
                            <th class="py-3.5 px-4 font-bold text-right bg-[#0044CC] whitespace-nowrap">주간 추정 수익 (원)</th>
                            <th class="py-3.5 px-4 font-bold text-center whitespace-nowrap">전주 대비(수익)</th>
                        </tr>
                    </thead>
                    <tbody id="weekly-table-body" class="divide-y divide-slate-200 font-medium"></tbody>
                </table>
            </div>
        </section>

        <section class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="mb-4">
                <h3 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                    <span class="w-2 h-6 bg-[#00BBFF] rounded-full inline-block"></span>
                    3. 주차별 단독 수익 및 조회수 추이 비교
                </h3>
                <p class="text-xs sm:text-sm text-slate-600 mt-1">
                    막대 그래프(좌측 축)는 해당 주차의 단독 추정 수익(원)을, 꺾은선 그래프(우측 축)는 주간 조회수를 나타냅니다.
                </p>
            </div>
            <div class="chart-container" id="weeklyTrendChartBox"><canvas id="weeklyTrendChart"></canvas></div>
        </section>

        <section class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="flex flex-col md:flex-row md:items-center justify-between mb-6 gap-4">
                <div>
                    <div class="flex flex-wrap items-center gap-2">
                        <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2 h-6 bg-[#FF9900] rounded-full inline-block"></span>
                            4. 시청자 연령 및 성별 분포 분석
                        </h2>
                        <span id="demo-status-pill" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-emerald-100 text-emerald-700">실제 시청자 연령/성별 데이터 반영됨</span>
                    </div>
                    <p class="text-sm text-slate-600 mt-1">
                        유튜브 스튜디오 [분석 &gt; 시청자]에서 내보낸 <strong>시청자 연령(.zip)</strong> 및 <strong>시청자 성별(.zip)</strong> 데이터를 기반으로 채널의 핵심 타깃 시청층을 분석합니다.
                    </p>
                </div>

                <div id="demo-summary-badges" class="flex flex-wrap items-center gap-2">
                    <span class="px-3 py-1.5 rounded-xl bg-blue-50 border border-blue-200 text-[#0044CC] text-xs font-bold">
                        핵심 연령층: <strong id="top-age-label">만 35–44세 (32.9%)</strong>
                    </span>
                    <span class="px-3 py-1.5 rounded-xl bg-amber-50 border border-amber-200 text-amber-800 text-xs font-bold">
                        성별 비율: <strong id="gender-ratio-label">여성 39.5% / 남성 60.4% / 사용자가 지정 0.1%</strong>
                    </span>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <div class="lg:col-span-7 bg-slate-50/70 rounded-2xl p-5 border border-slate-200/80 flex flex-col justify-between">
                    <div class="mb-3">
                        <h3 class="text-base font-bold text-slate-800">연령대별 시청자 비중 (%)</h3>
                        <p class="text-xs text-slate-500 mt-0.5">만 13세 ~ 65세 이상 연령 구간별 시청 점유율</p>
                    </div>
                    <div class="chart-container" id="ageDistributionChartBox"><canvas id="ageDistributionChart"></canvas></div>
                </div>

                <div class="lg:col-span-5 bg-slate-50/70 rounded-2xl p-5 border border-slate-200/80 flex flex-col justify-between">
                    <div class="mb-3">
                        <h3 class="text-base font-bold text-slate-800">성별 시청자 비중 (%)</h3>
                        <p class="text-xs text-slate-500 mt-0.5">여성 / 남성 / 사용자 지정 비율 구성</p>
                    </div>
                    <div class="chart-container" id="genderDistributionChartBox"><canvas id="genderDistributionChart"></canvas></div>
                </div>
            </div>
        </section>

        <section class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="flex flex-col md:flex-row md:items-center justify-between mb-6 gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <span class="w-2 h-6 bg-[#0044CC] rounded-full inline-block"></span>
                        5. 콘텐츠별 상세 성과 및 수익 분석표 (<span id="content-count-label">172</span>건)
                    </h2>
                    <p class="text-sm text-slate-600 mt-1">
                        `표 데이터.csv`에 포함된 모든 개별 동영상의 썸네일, 조회수, 클릭률, 원화 환산 추정 수익 내역입니다.
                    </p>
                </div>

                <div class="flex flex-wrap items-center gap-2">
                    <input type="text" id="search-input" oninput="renderContentTable()" placeholder="영상 제목 검색..." class="bg-slate-50 border border-slate-300 text-slate-800 text-xs font-medium rounded-lg px-3 py-2 focus:outline-none focus:border-[#0044CC]">
                    <select id="format-filter" onchange="renderContentTable()" class="bg-slate-50 border border-slate-300 text-slate-800 text-xs font-bold rounded-lg px-3 py-2 focus:outline-none focus:border-[#0044CC]">
                        <option value="all">포맷: 전체</option>
                        <option value="롱폼">포맷: 롱폼</option>
                        <option value="쇼츠">포맷: 쇼츠</option>
                    </select>
                    <select id="sort-filter" onchange="renderContentTable()" class="bg-slate-50 border border-slate-300 text-slate-800 text-xs font-bold rounded-lg px-3 py-2 focus:outline-none focus:border-[#0044CC]">
                        <option value="revenue">정렬: 추정 수익순</option>
                        <option value="views">정렬: 조회수순</option>
                        <option value="ctr">정렬: 클릭률(CTR)순</option>
                        <option value="date">정렬: 최신 게시일순</option>
                    </select>
                </div>
            </div>

            <div class="overflow-x-auto custom-scrollbar rounded-xl border border-slate-200 max-h-[650px]">
                <table class="w-full text-sm text-left border-collapse">
                    <thead class="sticky top-0 z-10">
                        <tr class="bg-slate-100 text-slate-700 text-xs uppercase tracking-wider border-b border-slate-200">
                            <th class="py-3.5 px-3 font-bold whitespace-nowrap">채널</th>
                            <th class="py-3.5 px-3 font-bold whitespace-nowrap">구분</th>
                            <th class="py-3.5 px-3 font-bold whitespace-nowrap">썸네일</th>
                            <th class="py-3.5 px-4 font-bold min-w-[240px]">동영상 제목</th>
                            <th class="py-3.5 px-3 font-bold whitespace-nowrap">게시 시간</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">조회수</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">시청 시간(h)</th>
                            <th class="py-3.5 px-4 font-bold text-right whitespace-nowrap">클릭률(CTR)</th>
                            <th class="py-3.5 px-4 font-bold text-right text-[#0044CC] whitespace-nowrap">추정 수익 (원)</th>
                        </tr>
                    </thead>
                    <tbody id="content-table-body" class="divide-y divide-slate-200"></tbody>
                </table>
            </div>
        </section>
    </main>

    <script id="app-data-script">
        let STORE = {"whatever":{"channelKey":"whatever","channelName":"뭐든하기루","isUSD":true,"rawTotals":{"views":1527013,"watchHours":124164.3533,"subs":233,"rawRevenue":2924.698},"videos":[{"channel":"whatever","channelName":"뭐든하기루","id":"ES3U2OUHtDU","videoId":"ES3U2OUHtDU","format":"롱폼","title":"나..너 좋아하냐??❤️떡밍아웃(현선이네 떡볶이)ㅣ낮술하기루 EP110","dateObj":"2026-11-03T15:00:00.000Z","dateStr":"2026-11-04","views":231532,"watchHours":18004.727,"subs":93,"rawRevenue":561.626,"impressions":1207285,"ctr":6.75},{"channel":"whatever","channelName":"뭐든하기루","id":"tp3DwFClg3E","videoId":"tp3DwFClg3E","format":"롱폼","title":"먹고 웃고 사랑하며 살기루(with 성현주 & 곱창 전골) | 낮술하기루 EP.109","dateObj":"2026-10-27T15:00:00.000Z","dateStr":"2026-10-28","views":209119,"watchHours":21141.8355,"subs":96,"rawRevenue":510.645,"impressions":1026106,"ctr":6.89},{"channel":"whatever","channelName":"뭐든하기루","id":"KwrFwE-QE1M","videoId":"KwrFwE-QE1M","format":"롱폼","title":"신당 중앙시장에 홀라당 반했기루(육회.갑오징어 외 7종 처리) | 낮술하기루 EP.111","dateObj":"2026-11-10T15:00:00.000Z","dateStr":"2026-11-11","views":208285,"watchHours":24324.5953,"subs":88,"rawRevenue":563.918,"impressions":951203,"ctr":7.81},{"channel":"whatever","channelName":"뭐든하기루","id":"KlMgJ7X-Pek","videoId":"KlMgJ7X-Pek","format":"쇼츠","title":"찐친대화","dateObj":"2026-10-28T15:00:00.000Z","dateStr":"2026-10-29","views":188103,"watchHours":467.79,"subs":16,"rawRevenue":29.736,"impressions":26700,"ctr":4.89},{"channel":"whatever","channelName":"뭐든하기루","id":"vHW7Lvfut3U","videoId":"vHW7Lvfut3U","format":"롱폼","title":"[미공개] 김민석&신기루 그대안의 블루 듀엣 무대 풀버전 | 뭐든하기루","dateObj":"2026-11-08T15:00:00.000Z","dateStr":"2026-11-09","views":37325,"watchHours":277.5248,"subs":0,"rawRevenue":0.449,"impressions":153148,"ctr":5.66},{"channel":"whatever","channelName":"뭐든하기루","id":"EgBfjiW6iZw","videoId":"EgBfjiW6iZw","format":"롱폼","title":"무당계의 신기루님께 신 들린 조롱 당하고 왔기루 | 뭐든하기루 EP.14","dateObj":"2024-12-03T15:00:00.000Z","dateStr":"2024-12-04","views":33932,"watchHours":2513.6574,"subs":27,"rawRevenue":38.037,"impressions":170750,"ctr":7.1},{"channel":"whatever","channelName":"뭐든하기루","id":"7s1-TngIHEs","videoId":"7s1-TngIHEs","format":"롱폼","title":"손님 보다 주인이 더 많이 먹는 오뎅바가 있기루 (with.홍윤화.홍뎅) | 낮술하기루 EP.75","dateObj":"2025-08-12T15:00:00.000Z","dateStr":"2025-08-13","views":28422,"watchHours":3399.1755,"subs":14,"rawRevenue":67.129,"impressions":141094,"ctr":7},{"channel":"whatever","channelName":"뭐든하기루","id":"zZ1Y4rd5lmo","videoId":"zZ1Y4rd5lmo","format":"롱폼","title":"[노콘쇼 미공개] 내가 너의 벗 되리라","dateObj":"2026-11-18T15:00:00.000Z","dateStr":"2026-11-19","views":21683,"watchHours":1127.9091,"subs":1,"rawRevenue":24.129,"impressions":78713,"ctr":8.11},{"channel":"whatever","channelName":"뭐든하기루","id":"DYtqXMOOxQY","videoId":"DYtqXMOOxQY","format":"롱폼","title":"찍먹삼겹살과 먹먹 공연후기(여의도 서글렁탕) | 낮술하기루 EP.108","dateObj":"2026-10-06T15:00:00.000Z","dateStr":"2026-10-07","views":20745,"watchHours":1491.4944,"subs":5,"rawRevenue":44.099,"impressions":125238,"ctr":4.54},{"channel":"whatever","channelName":"뭐든하기루","id":"fM1usK5xfS8","videoId":"fM1usK5xfS8","format":"쇼츠","title":"탄수화물 중독","dateObj":"2026-11-04T15:00:00.000Z","dateStr":"2026-11-05","views":17449,"watchHours":84.6486,"subs":3,"rawRevenue":2.46,"impressions":21139,"ctr":5.85},{"channel":"whatever","channelName":"뭐든하기루","id":"XcNFjQQndiE","videoId":"XcNFjQQndiE","format":"롱폼","title":"막창엔 소주! 토크엔 신기루 이용진이 일등이기루 | 낮술하기루 EP.22","dateObj":"2024-04-02T15:00:00.000Z","dateStr":"2024-04-03","views":16704,"watchHours":1609.4999,"subs":18,"rawRevenue":22.185,"impressions":115505,"ctr":5.12},{"channel":"whatever","channelName":"뭐든하기루","id":"9jgEexIB-Lc","videoId":"9jgEexIB-Lc","format":"롱폼","title":"부대시설 정보 없음. 부대식사 정보뿐인 호텔 부위로그(콘래드서울) | 부위로그 EP.10","dateObj":"2026-10-13T15:00:00.000Z","dateStr":"2026-10-14","views":15575,"watchHours":1814.0868,"subs":7,"rawRevenue":49.356,"impressions":103951,"ctr":5.36},{"channel":"whatever","channelName":"뭐든하기루","id":"iK06DDHtUq4","videoId":"iK06DDHtUq4","format":"롱폼","title":"살치 외 11가지 메뉴 먹으면서 살찐 외 11가지 토크하기루(With.샵뚱) | 낮술하기루 EP.63","dateObj":"2025-05-06T15:00:00.000Z","dateStr":"2025-05-07","views":14940,"watchHours":2529.866,"subs":18,"rawRevenue":48.583,"impressions":107680,"ctr":5.28},{"channel":"whatever","channelName":"뭐든하기루","id":"NwswhRp4xXk","videoId":"NwswhRp4xXk","format":"롱폼","title":"박준면과 신기루는 같은 혈통이기루 (용리단길 꺼거) | 낮술하기루 EP.105","dateObj":"2026-08-11T15:00:00.000Z","dateStr":"2026-08-12","views":14571,"watchHours":1857.1308,"subs":12,"rawRevenue":31.592,"impressions":100444,"ctr":4.51},{"channel":"whatever","channelName":"뭐든하기루","id":"sz9mP7xiZ5k","videoId":"sz9mP7xiZ5k","format":"롱폼","title":"북한산에서 등산말고 쏘맥말고 먹기루 | 낮술하기루 EP.01","dateObj":"2023-10-24T15:00:00.000Z","dateStr":"2023-10-25","views":14411,"watchHours":234.8292,"subs":4,"rawRevenue":5.807,"impressions":74572,"ctr":2.83},{"channel":"whatever","channelName":"뭐든하기루","id":"X08CmBrW9mI","videoId":"X08CmBrW9mI","format":"쇼츠","title":"루루식 슬픔 위로","dateObj":"2026-10-28T15:00:00.000Z","dateStr":"2026-10-29","views":13373,"watchHours":84.331,"subs":0,"rawRevenue":1.881,"impressions":16034,"ctr":5.67},{"channel":"whatever","channelName":"뭐든하기루","id":"Z9-Uu86quO8","videoId":"Z9-Uu86quO8","format":"롱폼","title":"세상은 아직 따뜻하다","dateObj":"2026-11-10T15:00:00.000Z","dateStr":"2026-11-11","views":12969,"watchHours":102.3458,"subs":1,"rawRevenue":2.454,"impressions":24445,"ctr":5.61},{"channel":"whatever","channelName":"뭐든하기루","id":"Roy59iuwBgI","videoId":"Roy59iuwBgI","format":"롱폼","title":"모란시장에서 도란도란 먹기루ㅣ낮술하기루 EP.65","dateObj":"2025-05-20T15:00:00.000Z","dateStr":"2025-05-21","views":12860,"watchHours":1071.5479,"subs":23,"rawRevenue":23.644,"impressions":81176,"ctr":5.76},{"channel":"whatever","channelName":"뭐든하기루","id":"S1-ejYwhTqw","videoId":"S1-ejYwhTqw","format":"쇼츠","title":"'노콘쇼'지만 가랭이는 좀... #신기루","dateObj":"2026-09-22T15:00:00.000Z","dateStr":"2026-09-23","views":12751,"watchHours":37.5452,"subs":6,"rawRevenue":1.561,"impressions":49644,"ctr":7.83},{"channel":"whatever","channelName":"뭐든하기루","id":"DsGFcpnOMI0","videoId":"DsGFcpnOMI0","format":"롱폼","title":"통인시장 기름떡볶이에 반한 신기름씨 (닭꼬치.통닭.만두.비빔국수) | 낮술하기루 EP.92","dateObj":"2026-02-03T15:00:00.000Z","dateStr":"2026-02-04","views":11109,"watchHours":1605.4406,"subs":3,"rawRevenue":36.873,"impressions":75892,"ctr":3.91},{"channel":"whatever","channelName":"뭐든하기루","id":"iFyEyG50JTo","videoId":"iFyEyG50JTo","format":"롱폼","title":"구 썸남들과 추억 곱씹으면서 안주 씹기루 (Feat.김철민.양배차+이상준) | 낮술하기루 EP.41","dateObj":"2024-09-10T15:00:00.000Z","dateStr":"2024-09-11","views":10554,"watchHours":1851.4459,"subs":10,"rawRevenue":33.413,"impressions":67154,"ctr":6.21},{"channel":"whatever","channelName":"뭐든하기루","id":"0pJqGi_KfRQ","videoId":"0pJqGi_KfRQ","format":"롱폼","title":"연신내 연서시장에서 연신 먹기루 (꼼장어, 김밥, 떡볶이 외 7메뉴)  | 낮술하기루 EP.86","dateObj":"2025-11-19T15:00:00.000Z","dateStr":"2025-11-20","views":9858,"watchHours":1174.5347,"subs":5,"rawRevenue":26.803,"impressions":63497,"ctr":5.21},{"channel":"whatever","channelName":"뭐든하기루","id":"ymk2tRPk39o","videoId":"ymk2tRPk39o","format":"롱폼","title":"샵뚱vs루뚱 우니 조지면서 우정 다지기루 | 낮술하기루 EP.33","dateObj":"2024-07-09T15:00:00.000Z","dateStr":"2024-07-10","views":8668,"watchHours":1354.6508,"subs":7,"rawRevenue":23.723,"impressions":62834,"ctr":5.15},{"channel":"whatever","channelName":"뭐든하기루","id":"XDlBMZXpHyY","videoId":"XDlBMZXpHyY","format":"롱폼","title":"식탁이랑 화면 꽉 채우기루 (feat.홍윤화 김민경) | 낮술하기루 EP.13","dateObj":"2024-01-30T15:00:00.000Z","dateStr":"2024-01-31","views":8041,"watchHours":1047.5272,"subs":8,"rawRevenue":18.876,"impressions":63363,"ctr":4.64},{"channel":"whatever","channelName":"뭐든하기루","id":"MgTFuH2u4Do","videoId":"MgTFuH2u4Do","format":"롱폼","title":"낭만 채우러 가서 배만 채우고 왔기루 (홋카이도 하코다테) | 부위로그 EP.7","dateObj":"2026-05-26T15:00:00.000Z","dateStr":"2026-05-27","views":7601,"watchHours":949.4455,"subs":7,"rawRevenue":18.894,"impressions":49792,"ctr":5.53},{"channel":"whatever","channelName":"뭐든하기루","id":"iqqQq4EULaY","videoId":"iqqQq4EULaY","format":"롱폼","title":"헛뚱뚱이 아우랑 중식한상 차려놓고 착상썰 들었기루 | 낮술하기루 EP.99","dateObj":"2026-06-16T15:00:00.000Z","dateStr":"2026-06-17","views":6963,"watchHours":985.9623,"subs":6,"rawRevenue":16.687,"impressions":57898,"ctr":4.86},{"channel":"whatever","channelName":"뭐든하기루","id":"hhdThtpQN78","videoId":"hhdThtpQN78","format":"롱폼","title":"곱창 대창 왕창 먹으면서 주구장창 떠들기루 | 낮술하기루 EP.84","dateObj":"2025-11-04T15:00:00.000Z","dateStr":"2025-11-05","views":6943,"watchHours":825.7392,"subs":1,"rawRevenue":18.549,"impressions":43322,"ctr":5.32},{"channel":"whatever","channelName":"뭐든하기루","id":"6XXHL1kz96I","videoId":"6XXHL1kz96I","format":"롱폼","title":"풍자랑 대게 되게 많이먹기루(feat.백두혈통) | 낮술하기루 EP.15","dateObj":"2024-02-13T15:00:00.000Z","dateStr":"2024-02-14","views":6773,"watchHours":916.9503,"subs":6,"rawRevenue":16.621,"impressions":61390,"ctr":4.73},{"channel":"whatever","channelName":"뭐든하기루","id":"mJDNrTUNLzs","videoId":"mJDNrTUNLzs","format":"쇼츠","title":"똑똑하기루","dateObj":"2026-10-28T15:00:00.000Z","dateStr":"2026-10-29","views":6660,"watchHours":25.2066,"subs":2,"rawRevenue":0.805,"impressions":12449,"ctr":2.64},{"channel":"whatever","channelName":"뭐든하기루","id":"OzFlaoqEDFg","videoId":"OzFlaoqEDFg","format":"쇼츠","title":"60대 어머니 '노콘쇼'에는 어떻게 오셨어요..?","dateObj":"2026-09-25T15:00:00.000Z","dateStr":"2026-09-26","views":6253,"watchHours":39.1217,"subs":1,"rawRevenue":0.788,"impressions":21532,"ctr":7.56},{"channel":"whatever","channelName":"뭐든하기루","id":"e1zawvd4-9s","videoId":"e1zawvd4-9s","format":"쇼츠","title":"족보 브레이커","dateObj":"2026-11-04T15:00:00.000Z","dateStr":"2026-11-05","views":5894,"watchHours":19.6556,"subs":1,"rawRevenue":0.714,"impressions":10265,"ctr":2.27},{"channel":"whatever","channelName":"뭐든하기루","id":"jrdEkmWimxA","videoId":"jrdEkmWimxA","format":"롱폼","title":"비빔국수.김밥.잔치국수 먹으면서 잔치걱정 하기루 (찐단골집) | 뭐든하기루 EP.17","dateObj":"2026-09-15T15:00:00.000Z","dateStr":"2026-09-16","views":5689,"watchHours":382.3042,"subs":3,"rawRevenue":10.343,"impressions":39039,"ctr":4.39},{"channel":"whatever","channelName":"뭐든하기루","id":"YzrUiRi8_4U","videoId":"YzrUiRi8_4U","format":"롱폼","title":"뚝도시장 에서 뚝심 있게 4차 까지 처리했기루 | 낮술하기루 EP.96","dateObj":"2026-04-14T15:00:00.000Z","dateStr":"2026-04-15","views":5667,"watchHours":1161.2672,"subs":2,"rawRevenue":22.927,"impressions":38621,"ctr":4.88},{"channel":"whatever","channelName":"뭐든하기루","id":"R-9UHKCKygg","videoId":"R-9UHKCKygg","format":"쇼츠","title":"속마음 토크","dateObj":"2026-10-28T15:00:00.000Z","dateStr":"2026-10-29","views":5589,"watchHours":16.4369,"subs":0,"rawRevenue":0.677,"impressions":11585,"ctr":4.01},{"channel":"whatever","channelName":"뭐든하기루","id":"Lfq-BOYBT0I","videoId":"Lfq-BOYBT0I","format":"롱폼","title":"간단하게 가맥집으로 2차가서 순차적으로 먹었기루 | 낮술하기루 EP.85","dateObj":"2025-11-11T15:00:00.000Z","dateStr":"2025-11-12","views":5541,"watchHours":642.5364,"subs":7,"rawRevenue":8.794,"impressions":33656,"ctr":5.93},{"channel":"whatever","channelName":"뭐든하기루","id":"DtqRxhTgs6w","videoId":"DtqRxhTgs6w","format":"롱폼","title":"서울중앙시장 코스","dateObj":"2026-11-10T15:00:00.000Z","dateStr":"2026-11-11","views":5516,"watchHours":36.8361,"subs":0,"rawRevenue":0.826,"impressions":16707,"ctr":4.8},{"channel":"whatever","channelName":"뭐든하기루","id":"8HYiIBEhuT4","videoId":"8HYiIBEhuT4","format":"쇼츠","title":"조롱잔치 아님","dateObj":"2026-09-22T15:00:00.000Z","dateStr":"2026-09-23","views":5472,"watchHours":21.7508,"subs":1,"rawRevenue":0.694,"impressions":21532,"ctr":7.66},{"channel":"whatever","channelName":"뭐든하기루","id":"qxZ20RdcrrA","videoId":"qxZ20RdcrrA","format":"롱폼","title":"야장에서 치킨 뜯으면서 파란만장 근황 뜯어보기루 | 낮술하기루 EP.83","dateObj":"2025-10-28T15:00:00.000Z","dateStr":"2025-10-29","views":5347,"watchHours":711.8194,"subs":1,"rawRevenue":12.864,"impressions":33673,"ctr":4.77},{"channel":"whatever","channelName":"뭐든하기루","id":"A9i9MEF6xuk","videoId":"A9i9MEF6xuk","format":"롱폼","title":"30년 전통 돈까스 썰면서 20년 방송 초심썰기루 | 낮술하기루 EP.78","dateObj":"2025-09-10T15:00:00.000Z","dateStr":"2025-09-11","views":5290,"watchHours":456.4201,"subs":2,"rawRevenue":8.811,"impressions":31824,"ctr":5.64},{"channel":"whatever","channelName":"뭐든하기루","id":"mMt6EYCJnt8","videoId":"mMt6EYCJnt8","format":"롱폼","title":"김치삼겹살 1티어에서 기름 튀어 가면서 먹기루 | 낮술하기루 EP.54","dateObj":"2025-01-21T15:00:00.000Z","dateStr":"2025-01-22","views":5168,"watchHours":651.5693,"subs":2,"rawRevenue":15.057,"impressions":30663,"ctr":6.34},{"channel":"whatever","channelName":"뭐든하기루","id":"JaZDekgoUW0","videoId":"JaZDekgoUW0","format":"롱폼","title":"진한 곱창전골 국물과 진한 눈물😭 찐친 술자리 (with.하하) | 낮술하기루 EP.93","dateObj":"2026-02-17T15:00:00.000Z","dateStr":"2026-02-18","views":4861,"watchHours":1016.8573,"subs":3,"rawRevenue":17.697,"impressions":33868,"ctr":5.47},{"channel":"whatever","channelName":"뭐든하기루","id":"8qVjUZi0ndY","videoId":"8qVjUZi0ndY","format":"롱폼","title":"치맥잔치 하면서 조롱잔치 하기루 (Feat.민기.윤화.재준.깡총) | 낮술하기루 EP.44","dateObj":"2024-10-08T15:00:00.000Z","dateStr":"2024-10-09","views":4760,"watchHours":603.2389,"subs":4,"rawRevenue":10.128,"impressions":35881,"ctr":4.71},{"channel":"whatever","channelName":"뭐든하기루","id":"NGXEZB7AfME","videoId":"NGXEZB7AfME","format":"롱폼","title":"광장시장에서 굉장히 먹었기루 (Feat.육회.꽈배기.빈대떡.김밥...) l 낮술하기루 EP.61","dateObj":"2025-04-15T15:00:00.000Z","dateStr":"2025-04-16","views":4760,"watchHours":537.5512,"subs":5,"rawRevenue":11.438,"impressions":33228,"ctr":5.19},{"channel":"whatever","channelName":"뭐든하기루","id":"IHusU1mV218","videoId":"IHusU1mV218","format":"롱폼","title":"상준이랑 곱창먹으면서 토크곱씹기루 (feat.이상준) | 낮술하기루 EP.10","dateObj":"2024-01-09T15:00:00.000Z","dateStr":"2024-01-10","views":4682,"watchHours":487.4082,"subs":4,"rawRevenue":9.85,"impressions":28672,"ctr":5.58},{"channel":"whatever","channelName":"뭐든하기루","id":"brXi_Bc9ePA","videoId":"brXi_Bc9ePA","format":"롱폼","title":"비도 오고 그래서 전 생각이 났어 🎵 (Feat.고추장찌개.막걸리) | 낮술하기루 EP.102","dateObj":"2026-07-21T15:00:00.000Z","dateStr":"2026-07-22","views":4641,"watchHours":719.9802,"subs":5,"rawRevenue":18.147,"impressions":29912,"ctr":6.32},{"channel":"whatever","channelName":"뭐든하기루","id":"eps7OpdaVpM","videoId":"eps7OpdaVpM","format":"롱폼","title":"간장게장에 입맛 돌아버리고 조회수에 눈 돌아버렸기루 | 낮술하기루 EP.55","dateObj":"2025-02-25T15:00:00.000Z","dateStr":"2025-02-26","views":4487,"watchHours":590.6105,"subs":2,"rawRevenue":13.331,"impressions":27063,"ctr":6.14},{"channel":"whatever","channelName":"뭐든하기루","id":"p84XHcy7beE","videoId":"p84XHcy7beE","format":"롱폼","title":"히밥의 끝없는 굴보쌈추가에 루루의 토크추가하기루 (feat.히밥) | 낮술하기루 EP.09","dateObj":"2023-12-26T15:00:00.000Z","dateStr":"2023-12-27","views":4390,"watchHours":313.5434,"subs":0,"rawRevenue":7.533,"impressions":23655,"ctr":5.9},{"channel":"whatever","channelName":"뭐든하기루","id":"gPeLnGtg8Bk","videoId":"gPeLnGtg8Bk","format":"롱폼","title":"호텔 조식 3차전","dateObj":"2026-10-13T15:00:00.000Z","dateStr":"2026-10-14","views":4272,"watchHours":20.8542,"subs":1,"rawRevenue":0.545,"impressions":15568,"ctr":5.33},{"channel":"whatever","channelName":"뭐든하기루","id":"xzGxVutKa5E","videoId":"xzGxVutKa5E","format":"롱폼","title":"은마상가는 어마어마한 맛집이기루 (튀김아저씨.미러칼국수.조이쿡..) | 낮술하기루 EP.76","dateObj":"2025-08-19T15:00:00.000Z","dateStr":"2025-08-20","views":4055,"watchHours":517.0757,"subs":4,"rawRevenue":10.923,"impressions":19872,"ctr":6.34},{"channel":"whatever","channelName":"뭐든하기루","id":"JMNtSCsKMyE","videoId":"JMNtSCsKMyE","format":"롱폼","title":"미경산 삼겹살 먹으면서 미정산 토크 (with.이은형.강재준) (망원 꽃삼정) | 낮술하기루 EP.106","dateObj":"2026-08-25T15:00:00.000Z","dateStr":"2026-08-26","views":4053,"watchHours":832.4497,"subs":3,"rawRevenue":14.059,"impressions":31262,"ctr":5.72},{"channel":"whatever","channelName":"뭐든하기루","id":"D_sGCNrhZLY","videoId":"D_sGCNrhZLY","format":"롱폼","title":"부대찌개 끓이면서 댓글 낋여왔기루 (50년 전통 남영동 털보집) | 낮술하기루 EP.103","dateObj":"2026-07-29T15:00:00.000Z","dateStr":"2026-07-30","views":3823,"watchHours":557.3197,"subs":1,"rawRevenue":12.455,"impressions":28438,"ctr":5},{"channel":"whatever","channelName":"뭐든하기루","id":"6QHeSHkMFG0","videoId":"6QHeSHkMFG0","format":"롱폼","title":"부산역 주변 맛집 부산스럽게 찾아먹기루 (영도포차.신발원.주렁박) | 부위로그 EP.9","dateObj":"2026-07-16T15:00:00.000Z","dateStr":"2026-07-17","views":3748,"watchHours":347.199,"subs":1,"rawRevenue":8.345,"impressions":26846,"ctr":5.25},{"channel":"whatever","channelName":"뭐든하기루","id":"qc1YhWcY32g","videoId":"qc1YhWcY32g","format":"쇼츠","title":"낮맥에 즉떡","dateObj":"2026-11-04T15:00:00.000Z","dateStr":"2026-11-05","views":3689,"watchHours":14.665,"subs":1,"rawRevenue":0.409,"impressions":7173,"ctr":3.57},{"channel":"whatever","channelName":"뭐든하기루","id":"EJinY2oRuWE","videoId":"EJinY2oRuWE","format":"롱폼","title":"근본 삼겹살 먹으면서 기본 겹겹이 토크하기루 (With.해리포터) | 낮술하기루 EP.67","dateObj":"2025-06-10T15:00:00.000Z","dateStr":"2025-06-11","views":3665,"watchHours":731.6244,"subs":1,"rawRevenue":10.849,"impressions":19319,"ctr":5.83},{"channel":"whatever","channelName":"뭐든하기루","id":"4fWM2ucDTLY","videoId":"4fWM2ucDTLY","format":"롱폼","title":"망원시장 포차에서 가차 없이 먹었기루 (빈대떡.호떡.떡꼬치 트리플떡악셀 달성) | 낮술하기루 EP.82","dateObj":"2025-10-21T15:00:00.000Z","dateStr":"2025-10-22","views":3610,"watchHours":684.3877,"subs":1,"rawRevenue":15.512,"impressions":19248,"ctr":5.75},{"channel":"whatever","channelName":"뭐든하기루","id":"ZEkHeUPoNtU","videoId":"ZEkHeUPoNtU","format":"롱폼","title":"흑백요리사의 중심에서 골목식당을 외치다 (포방터시장) | 낮술하기루 EP.91","dateObj":"2026-01-27T15:00:00.000Z","dateStr":"2026-01-28","views":3598,"watchHours":541.2541,"subs":2,"rawRevenue":10.99,"impressions":23448,"ctr":5.19},{"channel":"whatever","channelName":"뭐든하기루","id":"wHsuotqYFb4","videoId":"wHsuotqYFb4","format":"롱폼","title":"재준이랑 야구 응원하면서 응어리풀기루(with.강재준) (상암 연화주점) | 낮술하기루 EP.107","dateObj":"2026-09-08T15:00:00.000Z","dateStr":"2026-09-09","views":3554,"watchHours":464.7895,"subs":2,"rawRevenue":10.716,"impressions":32632,"ctr":3.71},{"channel":"whatever","channelName":"뭐든하기루","id":"O9KW2-kfDxM","videoId":"O9KW2-kfDxM","format":"롱폼","title":"마츠다의 정체","dateObj":"2025-07-12T15:00:00.000Z","dateStr":"2025-07-13","views":3476,"watchHours":18.5123,"subs":1,"rawRevenue":0.293,"impressions":19750,"ctr":4.89},{"channel":"whatever","channelName":"뭐든하기루","id":"C1dbHpmO_CE","videoId":"C1dbHpmO_CE","format":"롱폼","title":"굴보쌈 먹으면서 팬들이랑 눈치쌈 했기루 (합정 각시보쌈) | 낮술하기루 EP.88","dateObj":"2025-12-16T15:00:00.000Z","dateStr":"2025-12-17","views":3421,"watchHours":474.625,"subs":0,"rawRevenue":11.407,"impressions":19649,"ctr":6.01},{"channel":"whatever","channelName":"뭐든하기루","id":"vhIXVUlxG7U","videoId":"vhIXVUlxG7U","format":"쇼츠","title":"신기루 남편이 부르는 애칭?","dateObj":"2023-12-06T15:00:00.000Z","dateStr":"2023-12-07","views":3400,"watchHours":7.9405,"subs":0,"rawRevenue":0.336,"impressions":11453,"ctr":7.95},{"channel":"whatever","channelName":"뭐든하기루","id":"RN5LKRDBYd8","videoId":"RN5LKRDBYd8","format":"롱폼","title":"요즘 핫한 LA갈비집에서 팬들의 핫한 사랑 자랑하기루(심경희 라라랜드) | 낮술하기루 EP.87","dateObj":"2025-12-02T15:00:00.000Z","dateStr":"2025-12-03","views":3327,"watchHours":407.5378,"
