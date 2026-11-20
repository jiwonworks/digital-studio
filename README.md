<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>유튜브 주간 성과 및 수익 보고 대시보드</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lz-string/1.5.0/lz-string.min.js"></script>
    <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
    <style id="print-orientation-style">
        @page {
            size: A4 landscape;
            margin: 10mm;
        }
    </style>
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
        .drop-active {
            border-color: #0044CC !important;
            background-color: #EFF6FF !important;
        }
        @media print {
            * {
                -webkit-print-color-adjust: exact !important;
                print-color-adjust: exact !important;
            }
            body {
                background-color: #FFFFFF !important;
                padding-bottom: 0 !important;
            }
            header {
                position: static !important;
                box-shadow: none !important;
                border-radius: 12px !important;
                margin-bottom: 12px !important;
            }
            main {
                max-width: 100% !important;
                padding-left: 0 !important;
                padding-right: 0 !important;
                margin-top: 0 !important;
            }
            .no-print,
            #upload-section,
            #btn-upload-toggle,
            #btn-export,
            #btn-share-link,
            #share-modal,
            #pdf-export-wrapper,
            #content-filter-controls {
                display: none !important;
            }
            #content-table-scroll {
                max-height: none !important;
                overflow: visible !important;
            }
            section,
            .print-avoid-break {
                break-inside: avoid;
                page-break-inside: avoid;
            }
            #content-section {
                break-inside: auto !important;
                page-break-inside: auto !important;
            }
            .chart-container {
                height: 240px !important;
                max-height: 250px !important;
            }
        }
    </style>
</head>
<body class="antialiased pb-16">

    <header class="bg-gradient-to-r from-[#002B80] via-[#0044CC] to-[#0077FF] text-white shadow-lg sticky top-0 z-30">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <div class="flex flex-col xl:flex-row xl:items-center xl:justify-between gap-4">
                <div>
                    <div class="flex items-center gap-2 text-xs font-semibold tracking-wider uppercase text-blue-200 mb-1">
                        <span>WEEKLY EXECUTIVE REPORT</span>
                        <span>•</span>
                        <span id="report-period-label">분석 기간: 2026.10.20 ~ 2026.11.17 (28일)</span>
                    </div>
                    <h1 class="text-xl sm:text-2xl font-extrabold tracking-tight">유튜브 채널 수익 및 콘텐츠 성과 대시보드</h1>
                </div>
                
                <div class="flex flex-wrap items-center gap-2">
                    <div id="fx-control-box" class="flex items-center gap-1.5 bg-white/15 px-3 py-1.5 rounded-xl border border-white/25 text-xs">
                        <span class="text-blue-100 font-semibold">적용 환율 1 USD =</span>
                        <input type="number" id="usd-krw-rate" value="1400" step="1" min="500" max="3000" oninput="onExchangeRateChange()" class="w-16 bg-white text-slate-900 font-extrabold text-right px-1.5 py-0.5 rounded focus:outline-none">
                        <span class="text-blue-100 font-semibold">원</span>
                    </div>

                    <div class="flex flex-wrap items-center gap-1.5 bg-white/10 p-1.5 rounded-xl backdrop-blur-sm border border-white/20">
                        <button onclick="switchChannel('all')" id="btn-all" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-active">
                            전체 통합
                        </button>
                        <button onclick="switchChannel('whatever')" id="btn-whatever" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-inactive">
                            뭐든하기루
                        </button>
                        <button onclick="switchChannel('banjiha')" id="btn-banjiha" class="px-3.5 py-2 rounded-lg text-xs sm:text-sm font-bold transition-all duration-200 tab-inactive">
                            반지하 유정수
                        </button>
                    </div>

                    <button type="button" onclick="toggleUploadBox()" id="btn-upload-toggle" class="px-3.5 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-emerald-400 hover:bg-emerald-300 text-slate-950 shadow-md transition-all cursor-pointer flex items-center gap-1.5">
                        <span>📂 ZIP 업로드 칸 열기/닫기</span>
                    </button>

                    <button type="button" onclick="openShareModal()" id="btn-share-link" class="px-3.5 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-[#00BBFF] hover:bg-sky-300 text-slate-950 shadow-md transition-all cursor-pointer flex items-center gap-1.5">
                        <span>🔗 구글 시트 URL 복사</span>
                    </button>

                    <div id="pdf-export-wrapper" class="relative">
                        <button type="button" onclick="togglePdfMenu(event)" class="px-3.5 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-white text-[#0044CC] hover:bg-blue-50 shadow-md transition-all cursor-pointer flex items-center gap-1.5">
                            <span>📄 PDF 저장</span>
                            <span class="text-[10px]">▼</span>
                        </button>
                        <div id="pdf-menu" class="hidden absolute right-0 mt-2 w-60 bg-white rounded-xl shadow-xl border border-slate-200 py-1.5 z-50 text-slate-800">
                            <div class="px-3.5 py-1.5 text-[11px] font-bold text-slate-400 uppercase tracking-wider border-b border-slate-100">
                                PDF 용지 방향 선택
                            </div>
                            <button type="button" onclick="exportToPDF('landscape')" class="w-full text-left px-3.5 py-2.5 hover:bg-blue-50 transition-colors flex items-start gap-2.5 cursor-pointer">
                                <span class="text-base leading-none mt-0.5">🖥️</span>
                                <div>
                                    <div class="text-xs font-extrabold text-slate-800">가로형 PDF (Landscape)</div>
                                    <div class="text-[11px] text-slate-500 mt-0.5">프레젠테이션 · 넓은 표 보고 추천</div>
                                </div>
                            </button>
                            <button type="button" onclick="exportToPDF('portrait')" class="w-full text-left px-3.5 py-2.5 hover:bg-blue-50 transition-colors flex items-start gap-2.5 border-t border-slate-100 cursor-pointer">
                                <span class="text-base leading-none mt-0.5">📄</span>
                                <div>
                                    <div class="text-xs font-extrabold text-slate-800">세로형 PDF (Portrait)</div>
                                    <div class="text-[11px] text-slate-500 mt-0.5">A4 세로 출력 · 문서 결재용</div>
                                </div>
                            </button>
                        </div>
                    </div>

                    <button onclick="exportPopulatedHTML()" id="btn-export" class="px-3.5 py-2.5 rounded-xl text-xs sm:text-sm font-bold bg-[#FF9900] hover:bg-amber-500 text-slate-950 shadow-md transition-all cursor-pointer">
                        💾 데이터 포함 HTML 저장
                    </button>
                </div>
            </div>
        </div>
    </header>

    <div id="share-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 backdrop-blur-xs p-4" onclick="closeShareModal(event)">
        <div class="bg-white rounded-2xl max-w-2xl w-full shadow-2xl border border-slate-200 overflow-hidden" onclick="event.stopPropagation()">
            <div class="bg-gradient-to-r from-[#002B80] to-[#0044CC] px-6 py-4 text-white flex items-center justify-between">
                <div>
                    <span class="text-[11px] font-bold uppercase tracking-wider text-blue-200">GOOGLE SHEETS & WEB PUBLISHING</span>
                    <h3 class="text-lg font-extrabold">ZIP 데이터가 반영된 구글 시트용 URL 복사</h3>
                </div>
                <button type="button" onclick="closeShareModal()" class="w-8 h-8 rounded-lg bg-white/15 hover:bg-white/25 text-white font-bold flex items-center justify-center cursor-pointer">✕</button>
            </div>

            <div class="p-6 space-y-5 text-sm text-slate-700 max-h-[82vh] overflow-y-auto custom-scrollbar">
                <div id="modal-no-data-alert" class="hidden bg-rose-50 border border-rose-300 rounded-xl p-4 space-y-1">
                    <div class="text-xs font-extrabold text-rose-900">⚠️ 아직 업로드된 ZIP 파일 데이터가 없습니다!</div>
                    <p class="text-xs text-rose-800">먼저 화면 상단의 <strong>ZIP 업로드 칸</strong>에 유튜브 스튜디오 <code class="bg-white px-1 rounded">.zip</code> 파일을 끌어다 놓으신 뒤 이 버튼을 눌러주세요.</p>
                </div>

                <div class="bg-emerald-50 border border-emerald-200 rounded-xl p-4 space-y-3">
                    <div class="flex flex-wrap items-center justify-between gap-2">
                        <span class="px-2.5 py-0.5 rounded-full text-xs font-extrabold bg-emerald-700 text-white">방법 A · 현재 웹 URL에 ZIP 데이터 즉시 포함해서 복사</span>
                        <span id="share-url-status" class="text-xs font-bold text-emerald-700">구글 시트 규격 검사 중...</span>
                    </div>
                    <p class="text-xs text-slate-700 leading-relaxed">
                        지금 이 페이지에 올리신 <strong>ZIP 파일의 모든 핵심 지표·주간 추이·연령/성별·상위 영상 데이터</strong>가 URL 주소(<code class="bg-white px-1 rounded">#report=...</code>) 안에 압축되어 들어갑니다. <strong>웹사이트에 올린 상태에서 ZIP을 업로드한 뒤 아래 링크만 복사해서 구글 시트에 붙여넣으시면 됩니다.</strong>
                    </p>
                    <div class="flex items-center gap-2">
                        <input type="text" id="generated-share-url" readonly class="flex-1 bg-white border border-slate-300 rounded-lg px-3 py-2 text-xs text-slate-600 font-mono focus:outline-none">
                        <button type="button" onclick="copyShareUrl()" id="btn-copy-share-url" class="px-4 py-2 rounded-lg bg-slate-900 hover:bg-slate-800 text-white text-xs font-extrabold whitespace-nowrap cursor-pointer">
                            📋 데이터 URL 복사
                        </button>
                    </div>
                    <p id="local-file-warning" class="hidden text-[11px] text-rose-700 font-bold bg-rose-50 border border-rose-200 rounded-lg px-3 py-2">
                        🚫 현재 주소가 file:// (내 PC 로컬 파일)이므로 구글 시트에 바로 넣으면 "유효한 URL을 기입하세요" 오류가 발생합니다. 아래 방법 B로 배포해 https:// 주소를 먼저 만든 뒤, 그 웹페이지에서 ZIP을 올리고 링크를 복사하세요!
                    </p>
                </div>

                <div class="bg-blue-50/80 border border-blue-200 rounded-xl p-4 space-y-3">
                    <div class="flex items-center justify-between">
                        <span class="px-2.5 py-0.5 rounded-full text-xs font-extrabold bg-[#0044CC] text-white">방법 B · ZIP 데이터가 영구 내장된 index.html 배포</span>
                        <span class="text-xs font-bold text-slate-500">회원가입 불필요 · 무료</span>
                    </div>
                    <p class="text-xs text-slate-600 leading-relaxed">
                        먼저 ZIP 파일을 업로드한 상태에서 아래 <strong>[데이터 포함 index.html 다운로드]</strong>를 누르면, ZIP 데이터가 내부에 저장되고 <strong>ZIP 업로드 칸도 그대로 유지된</strong> <code class="bg-white px-1 rounded font-bold text-[#0044CC]">index.html</code>이 저장됩니다. 이를 Netlify Drop 등에 올리면 추가 작업 없이 바로 데이터가 뜹니다.
                    </p>
                    <div class="flex flex-wrap items-center gap-2 pt-1">
                        <button type="button" onclick="exportPopulatedHTML('index.html')" class="px-4 py-2.5 rounded-xl bg-[#0044CC] hover:bg-blue-800 text-white text-xs font-extrabold shadow-sm transition-colors cursor-pointer">
                            ⬇️ 데이터 포함 index.html 다운로드
                        </button>
                        <a href="https://app.netlify.com/drop" target="_blank" rel="noopener noreferrer" class="px-4 py-2.5 rounded-xl bg-white hover:bg-slate-50 text-[#0044CC] border border-blue-300 text-xs font-extrabold shadow-xs transition-colors">
                            🌐 Netlify Drop 열기 ↗
                        </a>
                        <a href="https://tiiny.host" target="_blank" rel="noopener noreferrer" class="px-3.5 py-2.5 rounded-xl bg-white hover:bg-slate-50 text-slate-700 border border-slate-300 text-xs font-bold transition-colors">
                            🌐 Tiiny.host 열기 ↗
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6 space-y-8">

        <section id="upload-section" class="bg-white rounded-2xl p-6 shadow-sm border-2 border-dashed border-blue-400 transition-all">
            <div id="dropzone" class="flex flex-col lg:flex-row items-start lg:items-center justify-between gap-6 cursor-pointer" onclick="document.getElementById('file-input').click()">
                <div class="flex items-start gap-4">
                    <div class="w-12 h-12 rounded-2xl bg-blue-100 text-[#0044CC] flex items-center justify-center text-2xl shrink-0 font-bold">
                        📂
                    </div>
                    <div>
                        <div class="flex flex-wrap items-center gap-2">
                            <h2 class="text-base sm:text-lg font-extrabold text-slate-800">유튜브 스튜디오 내보내기 파일(.zip 또는 .csv) 업로드</h2>
                            <span id="data-source-pill" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-rose-100 text-rose-700">현재 데이터 없음 · ZIP을 올려주세요</span>
                        </div>
                        <div class="flex flex-wrap items-center gap-1.5 mt-2">
                            <span id="status-whatever" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-slate-100 text-slate-500">뭐든하기루 콘텐츠: 미등록</span>
                            <span id="status-banjiha" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-slate-100 text-slate-500">반지하 유정수 콘텐츠: 미등록</span>
                            <span id="status-demo-whatever" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-slate-100 text-slate-500">뭐든하기루 연령/성별: 미등록</span>
                            <span id="status-demo-banjiha" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-slate-100 text-slate-500">반지하 유정수 연령/성별: 미등록</span>
                        </div>
                        <p class="text-xs sm:text-sm text-slate-600 mt-2">
                            여기에 <strong>콘텐츠 성과(.zip)</strong>, <strong>시청자 연령(.zip)</strong>, <strong>시청자 성별(.zip)</strong> 파일을 한 번에 드래그하거나 선택하세요. 업로드 즉시 아래 모든 차트와 표에 반영되며, 상단 <strong>[구글 시트 URL 복사]</strong>로 데이터가 담긴 링크를 바로 복사할 수 있습니다.
                        </p>
                    </div>
                </div>
                <div class="shrink-0 flex flex-col sm:flex-row items-stretch sm:items-center gap-2" onclick="event.stopPropagation()">
                    <div class="flex items-center gap-1.5 bg-slate-50 border border-slate-200 px-3 py-2 rounded-xl text-xs">
                        <span class="text-slate-500 font-semibold">파일명 미표기 시 대상:</span>
                        <select id="fallback-channel-select" class="bg-white border border-slate-300 rounded px-2 py-1 font-bold text-slate-800 focus:outline-none">
                            <option value="auto">자동 감지 (현재 탭 우선)</option>
                            <option value="whatever">뭐든하기루</option>
                            <option value="banjiha">반지하 유정수</option>
                        </select>
                    </div>
                    <button type="button" onclick="document.getElementById('file-input').click()" class="px-5 py-3 rounded-xl bg-[#0044CC] hover:bg-blue-800 text-white text-sm font-bold shadow-sm transition-colors cursor-pointer">
                        ZIP / CSV 선택하기
                    </button>
                    <input type="file" id="file-input" multiple accept=".zip,.csv" class="hidden" onchange="handleFiles(this.files)">
                </div>
            </div>

            <div id="quick-share-bar" class="hidden mt-4 pt-4 border-t border-slate-200 flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-3 bg-emerald-50/80 px-4 py-3 rounded-xl">
                <div class="text-xs text-emerald-900 font-bold flex items-center gap-2">
                    <span>✅ ZIP 데이터 반영 완료! 이 데이터가 포함된 구글 시트용 웹 링크:</span>
                </div>
                <div class="flex items-center gap-2 flex-1 max-w-xl">
                    <input type="text" id="inline-share-url" readonly class="flex-1 bg-white border border-emerald-300 rounded-lg px-2.5 py-1.5 text-xs font-mono text-slate-700 focus:outline-none">
                    <button type="button" onclick="copyInlineShareUrl()" id="btn-inline-copy" class="px-3.5 py-1.5 rounded-lg bg-emerald-700 hover:bg-emerald-800 text-white text-xs font-extrabold whitespace-nowrap cursor-pointer">
                        📋 URL 복사
                    </button>
                </div>
            </div>
        </section>

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
                <div id="currency-notice-badge" class="hidden px-3 py-1.5 rounded-xl bg-amber-50 border border-amber-200 text-amber-800 text-xs font-bold">
                    💱 USD 수익 채널 자동 원화 환산 적용 중
                </div>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                <div class="print-avoid-break bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-blue-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>추정 총 수익 (28일)</span>
                            <span class="text-lg">💰</span>
                        </div>
                        <div id="kpi-revenue" class="text-2xl sm:text-3xl font-extrabold text-[#0044CC] tracking-tight mb-3">-</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-rev-sub" class="text-slate-500 font-medium">ZIP 업로드 대기 중</span>
                            <span id="kpi-rev-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">-</span>
                        </div>
                    </div>
                </div>

                <div class="print-avoid-break bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-cyan-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>총 조회수 (28일)</span>
                            <span class="text-lg">👁️</span>
                        </div>
                        <div id="kpi-views" class="text-2xl sm:text-3xl font-extrabold text-slate-800 tracking-tight mb-3">-</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-views-sub" class="text-slate-500 font-medium">ZIP 업로드 대기 중</span>
                            <span id="kpi-views-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">-</span>
                        </div>
                    </div>
                </div>

                <div class="print-avoid-break bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-emerald-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>구독자 증감 (28일)</span>
                            <span class="text-lg">👥</span>
                        </div>
                        <div id="kpi-subs" class="text-2xl sm:text-3xl font-extrabold text-emerald-600 tracking-tight mb-3">-</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-subs-sub" class="text-slate-500 font-medium">ZIP 업로드 대기 중</span>
                            <span id="kpi-subs-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">-</span>
                        </div>
                    </div>
                </div>

                <div class="print-avoid-break bg-white rounded-2xl p-6 shadow-sm border border-slate-200/80 relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-indigo-50 rounded-bl-full -mr-4 -mt-4 z-0"></div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between text-slate-500 text-sm font-semibold mb-2">
                            <span>총 시청 시간 (28일)</span>
                            <span class="text-lg">⏱️</span>
                        </div>
                        <div id="kpi-watch" class="text-2xl sm:text-3xl font-extrabold text-slate-800 tracking-tight mb-3">-</div>
                        <div class="flex items-center justify-between text-xs">
                            <span id="kpi-watch-sub" class="text-slate-500 font-medium">ZIP 업로드 대기 중</span>
                            <span id="kpi-watch-badge" class="px-2 py-0.5 rounded-full font-bold bg-slate-100 text-slate-600">-</span>
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

                <div class="no-print flex items-center gap-1.5 bg-slate-100 p-1 rounded-xl border border-slate-200 self-start lg:self-auto shrink-0">
                    <button onclick="switchWeeklyFormat('all')" id="wfmt-all" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all subtab-active">
                        전체 (롱폼+쇼츠)
                    </button>
                    <button onclick="switchWeeklyFormat('롱폼')" id="wfmt-long" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all subtab-inactive">
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
                    <tbody id="weekly-table-body" class="divide-y divide-slate-200 font-medium">
                        <tr>
                            <td colspan="8" class="py-10 text-center text-slate-400">상단 영역에 유튜브 스튜디오 .zip 파일을 업로드하면 단순 주간별 실제 수치가 표에 정리됩니다.</td>
                        </tr>
                    </tbody>
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
            <div class="chart-container" id="weeklyTrendChartBox">
                <canvas id="weeklyTrendChart"></canvas>
            </div>
        </section>

        <section class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="flex flex-col md:flex-row md:items-center justify-between mb-6 gap-4">
                <div>
                    <div class="flex flex-wrap items-center gap-2">
                        <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                            <span class="w-2 h-6 bg-[#FF9900] rounded-full inline-block"></span>
                            4. 시청자 연령 및 성별 분포 분석
                        </h2>
                        <span id="demo-status-pill" class="px-2.5 py-0.5 rounded-full text-xs font-bold bg-slate-100 text-slate-500">
                            연령/성별 파일 업로드 대기 중
                        </span>
                    </div>
                    <p class="text-sm text-slate-600 mt-1">
                        유튜브 스튜디오 [분석 > 시청자]에서 내보낸 <strong>시청자 연령(.zip)</strong> 및 <strong>시청자 성별(.zip)</strong> 데이터를 기반으로 채널의 핵심 타깃 시청층을 분석합니다.
                    </p>
                </div>

                <div id="demo-summary-badges" class="flex flex-wrap items-center gap-2">
                    <span class="px-3 py-1.5 rounded-xl bg-blue-50 border border-blue-200 text-[#0044CC] text-xs font-bold">
                        핵심 연령층: <strong id="top-age-label">-</strong>
                    </span>
                    <span class="px-3 py-1.5 rounded-xl bg-amber-50 border border-amber-200 text-amber-800 text-xs font-bold">
                        성별 비율: <strong id="gender-ratio-label">-</strong>
                    </span>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <div class="print-avoid-break lg:col-span-7 bg-slate-50/70 rounded-2xl p-5 border border-slate-200/80 flex flex-col justify-between">
                    <div class="mb-3">
                        <h3 class="text-base font-bold text-slate-800">연령대별 시청자 비중 (%)</h3>
                        <p class="text-xs text-slate-500 mt-0.5">만 13세 ~ 65세 이상 연령 구간별 시청 점유율</p>
                    </div>
                    <div class="chart-container" id="ageDistributionChartBox">
                        <canvas id="ageDistributionChart"></canvas>
                    </div>
                </div>

                <div class="print-avoid-break lg:col-span-5 bg-slate-50/70 rounded-2xl p-5 border border-slate-200/80 flex flex-col justify-between">
                    <div class="mb-3">
                        <h3 class="text-base font-bold text-slate-800">성별 시청자 비중 (%)</h3>
                        <p class="text-xs text-slate-500 mt-0.5">여성 / 남성 / 사용자 지정 비율 구성</p>
                    </div>
                    <div class="chart-container" id="genderDistributionChartBox">
                        <canvas id="genderDistributionChart"></canvas>
                    </div>
                </div>
            </div>
        </section>

        <section id="content-section" class="bg-white rounded-2xl p-6 sm:p-8 shadow-sm border border-slate-200/80">
            <div class="flex flex-col md:flex-row md:items-center justify-between mb-6 gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                        <span class="w-2 h-6 bg-[#0044CC] rounded-full inline-block"></span>
                        5. 콘텐츠별 상세 성과 및 수익 분석표 (<span id="content-count-label">0</span>건)
                    </h2>
                    <p class="text-sm text-slate-600 mt-1">
                        `표 데이터.csv`에 포함된 모든 개별 동영상의 썸네일, 조회수, 클릭률, 원화 환산 추정 수익 내역입니다.
                    </p>
                </div>

                <div id="content-filter-controls" class="flex flex-wrap items-center gap-2">
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

            <div id="content-table-scroll" class="overflow-x-auto custom-scrollbar rounded-xl border border-slate-200 max-h-[650px]">
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
                    <tbody id="content-table-body" class="divide-y divide-slate-200">
                        <tr>
                            <td colspan="9" class="py-10 text-center text-slate-400">파일을 업로드하면 전체 콘텐츠 목록이 표시됩니다.</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

    </main>

    <script id="app-data-script">
        let STORE = {
            whatever: null,
            banjiha: null
        };

        let DEMOGRAPHICS = {
            whatever: { age: null, gender: null },
            banjiha: { age: null, gender: null }
        };

        let USD_TO_KRW = 1400;
        let currentChannel = 'all';
        let weeklyFormat = 'all';
    </script>

    <script id="app-logic-script">
        let weeklyChartInstance = null;
        let ageChartInstance = null;
        let genderChartInstance = null;

        function hasAnyLoadedData() {
            return Boolean(
                STORE.whatever ||
                STORE.banjiha ||
                (DEMOGRAPHICS.whatever && (DEMOGRAPHICS.whatever.age || DEMOGRAPHICS.whatever.gender)) ||
                (DEMOGRAPHICS.banjiha && (DEMOGRAPHICS.banjiha.age || DEMOGRAPHICS.banjiha.gender))
            );
        }

        function buildCompactPayload(maxVideosPerChannel) {
            const limit = typeof maxVideosPerChannel === 'number' ? maxVideosPerChannel : 60;
            const packStore = (st) => {
                if (!st) return null;
                const sortedVideos = [...(st.videos || [])]
                    .sort((a, b) => b.rawRevenue - a.rawRevenue || b.views - a.views)
                    .slice(0, limit);
                return {
                    k: st.channelKey,
                    u: st.isUSD ? 1 : 0,
                    t: {
                        views: Math.round(st.rawTotals.views),
                        watchHours: Number(st.rawTotals.watchHours.toFixed(1)),
                        subs: Math.round(st.rawTotals.subs),
                        rawRevenue: Number(st.rawTotals.rawRevenue.toFixed(2))
                    },
                    v: sortedVideos.map(v => [
                        v.videoId || extractVideoId(v.id),
                        v.format === '쇼츠' ? 1 : 0,
                        v.title.slice(0, 48),
                        v.dateStr,
                        Math.round(v.views),
                        Number(v.watchHours.toFixed(1)),
                        Math.round(v.subs),
                        Number(v.rawRevenue.toFixed(2)),
                        Number(v.ctr.toFixed(1))
                    ]),
                    w: st.rawWeeklyByFormat
                };
            };
            return {
                r: USD_TO_KRW,
                c: currentChannel,
                wf: weeklyFormat,
                s: {
                    whatever: packStore(STORE.whatever),
                    banjiha: packStore(STORE.banjiha)
                },
                d: DEMOGRAPHICS
            };
        }

        function restoreFromCompactPayload(payload) {
            if (!payload || !payload.s) return false;
            const unpackStore = (p) => {
                if (!p) return null;
                const chKey = p.k;
                const chName = chKey === 'whatever' ? '뭐든하기루' : '반지하 유정수';
                const videos = (p.v || []).map(v => {
                    if (Array.isArray(v)) {
                        return {
                            channel: chKey,
                            channelName: chName,
                            id: v[0],
                            videoId: v[0],
                            format: v[1] === 1 ? '쇼츠' : '롱폼',
                            title: v[2],
                            dateObj: parseKoreanDate(v[3]),
                            dateStr: v[3],
                            views: v[4],
                            watchHours: v[5],
                            subs: v[6],
                            rawRevenue: v[7],
                            ctr: v[8]
                        };
                    }
                    return {
                        channel: v.c || chKey,
                        channelName: v.cn || chName,
                        id: v.i,
                        videoId: v.i,
                        format: v.f,
                        title: v.t,
                        dateObj: parseKoreanDate(v.d),
                        dateStr: v.d,
                        views: v.vw,
                        watchHours: v.wh,
                        subs: v.sb,
                        rawRevenue: v.rv,
                        ctr: v.ct
                    };
                });
                return {
                    channelKey: chKey,
                    channelName: p.n || chName,
                    isUSD: Boolean(p.u),
                    rawTotals: p.t,
                    videos,
                    rawWeeklyByFormat: p.w
                };
            };
            STORE.whatever = unpackStore(payload.s.whatever);
            STORE.banjiha = unpackStore(payload.s.banjiha);
            if (payload.d) DEMOGRAPHICS = payload.d;
            if (payload.r) {
                USD_TO_KRW = payload.r;
                const rateInput = document.getElementById('usd-krw-rate');
                if (rateInput) rateInput.value = String(USD_TO_KRW);
            }
            if (payload.c) currentChannel = payload.c;
            if (payload.wf) weeklyFormat = payload.wf;
            return hasAnyLoadedData();
        }

        function saveStateToLocalStorage() {
            try {
                if (!hasAnyLoadedData()) return;
                const fullState = {
                    STORE,
                    DEMOGRAPHICS,
                    USD_TO_KRW,
                    currentChannel,
                    weeklyFormat
                };
                localStorage.setItem('yt_weekly_dashboard_state_v1', JSON.stringify(fullState));
            } catch (e) {}
        }

        function loadStateFromLocalStorage() {
            try {
                const raw = localStorage.getItem('yt_weekly_dashboard_state_v1');
                if (!raw) return false;
                const parsed = JSON.parse(raw);
                if (!parsed) return false;
                if (parsed.STORE) {
                    ['whatever', 'banjiha'].forEach(k => {
                        if (parsed.STORE[k]) {
                            parsed.STORE[k].videos = (parsed.STORE[k].videos || []).map(v => ({
                                ...v,
                                dateObj: parseKoreanDate(v.dateStr)
                            }));
                        }
                    });
                    STORE = parsed.STORE;
                }
                if (parsed.DEMOGRAPHICS) DEMOGRAPHICS = parsed.DEMOGRAPHICS;
                if (parsed.USD_TO_KRW) {
                    USD_TO_KRW = parsed.USD_TO_KRW;
                    const rateInput = document.getElementById('usd-krw-rate');
                    if (rateInput) rateInput.value = String(USD_TO_KRW);
                }
                if (parsed.currentChannel) currentChannel = parsed.currentChannel;
                if (parsed.weeklyFormat) weeklyFormat = parsed.weeklyFormat;
                return hasAnyLoadedData();
            } catch (e) {
                return false;
            }
        }

        function loadStateFromUrlHash() {
            try {
                const hash = window.location.hash || '';
                if (!hash.startsWith('#report=')) return false;
                const encoded = hash.slice('#report='.length);
                if (!encoded || typeof LZString === 'undefined') return false;
                const jsonStr = LZString.decompressFromEncodedURIComponent(encoded);
                if (!jsonStr) return false;
                const payload = JSON.parse(jsonStr);
                return restoreFromCompactPayload(payload);
            } catch (e) {
                return false;
            }
        }

        function generateShareUrlString() {
            const baseUrl = window.location.href.split('#')[0];
            if (!hasAnyLoadedData() || typeof LZString === 'undefined') return baseUrl;

            const limits = [60, 30, 15, 8, 4, 0];
            let bestUrl = '';
            for (const lim of limits) {
                const payload = buildCompactPayload(lim);
                const compressed = LZString.compressToEncodedURIComponent(JSON.stringify(payload));
                bestUrl = `${baseUrl}#report=${compressed}`;
                if (bestUrl.length <= 1950) break;
            }
            return bestUrl;
        }

        function syncUrlAndShareBar() {
            if (!hasAnyLoadedData()) return;
            const shareUrl = generateShareUrlString();
            try {
                if (shareUrl.includes('#report=')) {
                    window.history.replaceState(null, '', '#' + shareUrl.split('#')[1]);
                }
            } catch (e) {}

            const quickBar = document.getElementById('quick-share-bar');
            const inlineInput = document.getElementById('inline-share-url');
            if (quickBar && inlineInput) {
                quickBar.classList.remove('hidden');
                inlineInput.value = shareUrl;
            }
        }

        function copyInlineShareUrl() {
            const input = document.getElementById('inline-share-url');
            const btn = document.getElementById('btn-inline-copy');
            if (!input) return;
            input.select();
            input.setSelectionRange(0, 999999);
            if (navigator.clipboard && navigator.clipboard.writeText) {
                navigator.clipboard.writeText(input.value);
            } else {
                document.execCommand('copy');
            }
            if (btn) {
                const orig = btn.textContent;
                btn.textContent = '✅ 복사됨!';
                setTimeout(() => {
                    btn.textContent = orig;
                }, 2000);
            }
        }

        function openShareModal() {
            const modal = document.getElementById('share-modal');
            const input = document.getElementById('generated-share-url');
            const warn = document.getElementById('local-file-warning');
            const status = document.getElementById('share-url-status');
            const noDataAlert = document.getElementById('modal-no-data-alert');
            if (!modal || !input) return;

            const hasData = hasAnyLoadedData();
            if (noDataAlert) {
                if (!hasData) {
                    noDataAlert.classList.remove('hidden');
                } else {
                    noDataAlert.classList.add('hidden');
                }
            }

            const shareUrl = generateShareUrlString();
            input.value = shareUrl;

            const isHttp = window.location.protocol === 'http:' || window.location.protocol === 'https:';
            const isShortEnough = shareUrl.length <= 2000;

            if (status) {
                if (!hasData) {
                    status.className = "text-xs font-extrabold text-rose-600";
                    status.textContent = "⚠️ 먼저 ZIP 파일을 업로드해 주세요";
                } else if (isHttp && isShortEnough) {
                    status.className = "text-xs font-extrabold text-emerald-700";
                    status.textContent = `✅ ZIP 데이터 포함 완료 · 구글 시트 삽입 가능 (${shareUrl.length.toLocaleString()}자)`;
                } else if (!isHttp) {
                    status.className = "text-xs font-extrabold text-rose-600";
                    status.textContent = `⚠️ 로컬 경로(file://) · 방법 B로 웹 배포 필요`;
                } else {
                    status.className = "text-xs font-extrabold text-amber-700";
                    status.textContent = `⚠️ 길이 ${shareUrl.length.toLocaleString()}자 · 방법 B index.html 배포 권장`;
                }
            }
            if (warn) {
                if (!isHttp) {
                    warn.classList.remove('hidden');
                } else {
                    warn.classList.add('hidden');
                }
            }
            modal.classList.remove('hidden');
        }

        function closeShareModal(event) {
            if (event && event.target !== event.currentTarget) return;
            const modal = document.getElementById('share-modal');
            if (modal) modal.classList.add('hidden');
        }

        function copyShareUrl() {
            const input = document.getElementById('generated-share-url');
            const btn = document.getElementById('btn-copy-share-url');
            if (!input) return;
            input.select();
            input.setSelectionRange(0, 999999);
            if (navigator.clipboard && navigator.clipboard.writeText) {
                navigator.clipboard.writeText(input.value);
            } else {
                document.execCommand('copy');
            }
            if (btn) {
                const orig = btn.textContent;
                btn.textContent = '✅ 복사 완료!';
                btn.classList.replace('bg-slate-900', 'bg-emerald-600');
                setTimeout(() => {
                    btn.textContent = orig;
                    btn.classList.replace('bg-emerald-600', 'bg-slate-900');
                }, 2000);
            }
        }

        function toggleUploadBox() {
            const sec = document.getElementById('upload-section');
            if (sec) sec.classList.toggle('hidden');
        }

        function togglePdfMenu(event) {
            if (event) event.stopPropagation();
            const menu = document.getElementById('pdf-menu');
            if (menu) menu.classList.toggle('hidden');
        }

        document.addEventListener('click', function(e) {
            const wrapper = document.getElementById('pdf-export-wrapper');
            const menu = document.getElementById('pdf-menu');
            if (wrapper && menu && !wrapper.contains(e.target)) {
                menu.classList.add('hidden');
            }
        });

        function exportToPDF(orientation) {
            const menu = document.getElementById('pdf-menu');
            if (menu) menu.classList.add('hidden');

            const styleEl = document.getElementById('print-orientation-style');
            if (styleEl) {
                styleEl.textContent = orientation === 'portrait'
                    ? '@page { size: A4 portrait; margin: 10mm; }'
                    : '@page { size: A4 landscape; margin: 10mm; }';
            }

            if (weeklyChartInstance) weeklyChartInstance.resize();
            if (ageChartInstance) ageChartInstance.resize();
            if (genderChartInstance) genderChartInstance.resize();

            setTimeout(() => {
                window.print();
            }, 150);
        }

        window.addEventListener('afterprint', () => {
            if (weeklyChartInstance) weeklyChartInstance.resize();
            if (ageChartInstance) ageChartInstance.resize();
            if (genderChartInstance) genderChartInstance.resize();
        });

        function norm(str) {
            return String(str || '').normalize('NFC').trim();
        }

        function escapeAttr(str) {
            return String(str || '').replace(/&/g, '&amp;').replace(/"/g, '&quot;').replace(/</g, '&lt;').replace(/>/g, '&gt;');
        }

        function extractVideoId(rawId) {
            const s = norm(rawId);
            if (/^[a-zA-Z0-9_-]{11}$/.test(s)) return s;
            const match = s.match(/(?:v=|\/shorts\/|youtu\.be\/)([a-zA-Z0-9_-]{11})/);
            return match ? match[1] : '';
        }

        function getThumbUrl(videoId) {
            return videoId ? `https://i.ytimg.com/vi/${videoId}/mqdefault.jpg` : '';
        }

        function onExchangeRateChange() {
            const inputEl = document.getElementById('usd-krw-rate');
            if (!inputEl) return;
            const val = parseFloat(inputEl.value);
            if (!isNaN(val) && val > 0) {
                USD_TO_KRW = val;
                updateStatusBadges();
                refreshDashboard();
                saveStateToLocalStorage();
                syncUrlAndShareBar();
            }
        }

        function bindDropzoneEvents() {
            const dropzone = document.getElementById('dropzone');
            const uploadSec = document.getElementById('upload-section');
            if (!dropzone || !uploadSec) return;

            ['dragenter', 'dragover'].forEach(evt => {
                dropzone.addEventListener(evt, e => {
                    e.preventDefault();
                    e.stopPropagation();
                    uploadSec.classList.add('drop-active');
                });
            });
            ['dragleave', 'drop'].forEach(evt => {
                dropzone.addEventListener(evt, e => {
                    e.preventDefault();
                    e.stopPropagation();
                    uploadSec.classList.remove('drop-active');
                });
            });
            dropzone.addEventListener('drop', e => {
                if (e.dataTransfer && e.dataTransfer.files) {
                    handleFiles(e.dataTransfer.files);
                }
            });
        }

        function parseNum(val) {
            if (val === null || val === undefined || val === '' || val === '-') return 0;
            if (typeof val === 'number') return val;
            const cleaned = String(val).replace(/[^0-9.-]/g, '');
            const num = parseFloat(cleaned);
            return isNaN(num) ? 0 : num;
        }

        function parseKoreanDate(str) {
            if (!str) return null;
            const s = norm(str);
            const krMatch = s.match(/(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})/);
            if (krMatch) {
                return new Date(parseInt(krMatch[1]), parseInt(krMatch[2]) - 1, parseInt(krMatch[3]));
            }
            const krWordMatch = s.match(/(\d{4})년\s*(\d{1,2})월\s*(\d{1,2})일/);
            if (krWordMatch) {
                return new Date(parseInt(krWordMatch[1]), parseInt(krWordMatch[2]) - 1, parseInt(krWordMatch[3]));
            }
            const isoMatch = s.match(/(\d{4})-(\d{1,2})-(\d{1,2})/);
            if (isoMatch) {
                return new Date(parseInt(isoMatch[1]), parseInt(isoMatch[2]) - 1, parseInt(isoMatch[3]));
            }
            const d = new Date(s);
            return isNaN(d.getTime()) ? null : d;
        }

        function formatDateStr(d) {
            if (!d) return '-';
            const y = d.getFullYear();
            const m = String(d.getMonth() + 1).padStart(2, '0');
            const day = String(d.getDate()).padStart(2, '0');
            return `${y}-${m}-${day}`;
        }

        function hasColumn(rows, keywords) {
            if (!rows || rows.length === 0) return false;
            const keys = Object.keys(rows[0]).map(k => norm(k));
            return keywords.some(kw => keys.some(k => k.includes(norm(kw))));
        }

        function getField(row, keywords) {
            const keys = Object.keys(row);
            for (const kw of keywords) {
                const nkw = norm(kw);
                const exact = keys.find(k => norm(k) === nkw);
                if (exact !== undefined) return row[exact];
            }
            for (const kw of keywords) {
                const nkw = norm(kw);
                const partial = keys.find(k => norm(k).includes(nkw));
                if (partial !== undefined) return row[partial];
            }
            return '';
        }

        function getPctField(row) {
            const keys = Object.keys(row);
            const pctKey = keys.find(k => {
                const nk = norm(k);
                return nk.includes('%') && (nk.includes('조회수') || nk.includes('시청') || nk.includes('Views') || nk.includes('Watch'));
            }) || keys.find(k => norm(k).includes('%'));
            if (pctKey) {
                return { val: parseNum(row[pctKey]), isRaw: false };
            }
            const rawVal = parseNum(getField(row, ['조회수', '시청 시간', 'Views', 'Watch time']));
            return { val: rawVal, isRaw: true };
        }

        function detectIsUSD(rows, channelKey) {
            if (!rows || rows.length === 0) return channelKey === 'whatever';
            const headers = Object.keys(rows[0]).map(h => norm(h).toUpperCase());
            const revHeader = headers.find(h => h.includes('수익') || h.includes('REVENUE'));
            if (revHeader) {
                if (revHeader.includes('USD') || revHeader.includes('$')) return true;
                if (revHeader.includes('KRW') || revHeader.includes('₩')) return false;
            }
            for (let i = 0; i < Math.min(rows.length, 15); i++) {
                const rawRev = String(getField(rows[i], ['예상 수익', '예상 총 수익', '수익', 'Estimated revenue']));
                if (rawRev.includes('$')) return true;
                if (rawRev.includes('₩')) return false;
            }
            return channelKey === 'whatever';
        }

        function detectChannelFromFileName(name) {
            const n = norm(name).toLowerCase();
            if (n.includes('뭐든') || n.includes('whatever')) return 'whatever';
            if (n.includes('반지하') || n.includes('banjiha')) return 'banjiha';
            const fallbackEl = document.getElementById('fallback-channel-select');
            const fallback = fallbackEl ? fallbackEl.value : 'auto';
            if (fallback === 'whatever' || fallback === 'banjiha') return fallback;
            if (currentChannel === 'whatever' || currentChannel === 'banjiha') return currentChannel;
            return null;
        }

        async function handleFiles(fileList) {
            const files = Array.from(fileList);
            for (const file of files) {
                const name = norm(file.name);
                const detectedChannel = detectChannelFromFileName(name);

                if (name.toLowerCase().endsWith('.zip')) {
                    const zip = await JSZip.loadAsync(file);
                    const csvFiles = {};
                    for (const [relativePath, zipEntry] of Object.entries(zip.files)) {
                        const normPath = norm(relativePath);
                        if (!zipEntry.dir && !normPath.startsWith('__MACOSX') && normPath.toLowerCase().endsWith('.csv')) {
                            const text = await zipEntry.async('string');
                            const baseName = norm(normPath.split('/').pop());
                            csvFiles[baseName] = text;
                        }
                    }
                    routeCSVSet(detectedChannel, name, csvFiles);
                } else if (name.toLowerCase().endsWith('.csv')) {
                    const text = await file.text();
                    const obj = {};
                    obj[name] = text;
                    routeCSVSet(detectedChannel, name, obj);
                }
            }
            updateStatusBadges();
            refreshDashboard();
            saveStateToLocalStorage();
            syncUrlAndShareBar();
        }

        function parseCSVText(text) {
            const cleanText = text.replace(/^\uFEFF/, '');
            const res = Papa.parse(cleanText, {
                header: true,
                skipEmptyLines: true,
                transformHeader: h => norm(h)
            });
            return res.data || [];
        }

        function routeCSVSet(detectedChannel, fileName, csvFiles) {
            let tableRows = [];
            let chartRows = [];
            const allParsedList = [];

            for (const [rawName, content] of Object.entries(csvFiles)) {
                const fname = norm(rawName);
                const parsed = parseCSVText(content);
                if (parsed.length === 0) continue;
                allParsedList.push(parsed);
                if (fname.includes('표') || fname.toLowerCase().includes('table')) {
                    tableRows = parsed;
                } else if (fname.includes('차트') || fname.toLowerCase().includes('chart')) {
                    chartRows = parsed;
                }
            }

            if (tableRows.length === 0) {
                tableRows = allParsedList.find(p => !hasColumn(p, ['날짜', 'Date'])) || [];
            }
            if (chartRows.length === 0) {
                chartRows = allParsedList.find(p => hasColumn(p, ['날짜', 'Date'])) || [];
            }

            const fnLower = norm(fileName).toLowerCase();
            const isAgeData = fnLower.includes('연령') || fnLower.includes('age')
                || hasColumn(tableRows, ['시청자 연령', '연령', 'Viewer age'])
                || hasColumn(chartRows, ['시청자 연령', '연령', 'Viewer age']);
            const isGenderData = fnLower.includes('성별') || fnLower.includes('gender')
                || hasColumn(tableRows, ['시청자 성별', '성별', 'Viewer gender'])
                || hasColumn(chartRows, ['시청자 성별', '성별', 'Viewer gender']);

            if (isAgeData) {
                const ch = detectedChannel || (!DEMOGRAPHICS.whatever.age ? 'whatever' : 'banjiha');
                processAgeCSV(ch, tableRows, chartRows);
                return;
            }
            if (isGenderData) {
                const ch = detectedChannel || (!DEMOGRAPHICS.whatever.gender ? 'whatever' : 'banjiha');
                processGenderCSV(ch, tableRows, chartRows);
                return;
            }

            const ch = detectedChannel || (!STORE.whatever ? 'whatever' : 'banjiha');
            processChannelCSVSet(ch, csvFiles);
        }

        function normalizePctItems(rawItems) {
            if (rawItems.length === 0) return [];
            const hasRawOnly = rawItems.some(it => it.isRaw);
            if (hasRawOnly) {
                const total = rawItems.reduce((s, it) => s + it.val, 0);
                return rawItems.map(it => ({
                    label: it.label,
                    pct: total > 0 ? Number(((it.val / total) * 100).toFixed(1)) : 0
                }));
            }
            return rawItems.map(it => ({
                label: it.label,
                pct: Number(it.val.toFixed(1))
            }));
        }

        function processAgeCSV(channelKey, tableRows, chartRows) {
            const rawItems = [];
            if (tableRows && tableRows.length > 0) {
                tableRows.forEach(row => {
                    const label = norm(getField(row, ['시청자 연령', '연령', 'Viewer age']));
                    if (!label || label === '합계' || label === 'Total') return;
                    const { val, isRaw } = getPctField(row);
                    rawItems.push({ label, val, isRaw });
                });
            } else if (chartRows && chartRows.length > 0) {
                const map = new Map();
                chartRows.forEach(row => {
                    const label = norm(getField(row, ['시청자 연령', '연령', 'Viewer age']));
                    if (!label || label === '합계' || label === 'Total') return;
                    const val = parseNum(getField(row, ['조회수', '시청 시간', 'Views', 'Watch time']));
                    map.set(label, (map.get(label) || 0) + val);
                });
                map.forEach((val, label) => rawItems.push({ label, val, isRaw: true }));
            }

            const items = normalizePctItems(rawItems);
            if (items.length > 0) {
                DEMOGRAPHICS[channelKey].age = items;
            }
        }

        function processGenderCSV(channelKey, tableRows, chartRows) {
            const rawItems = [];
            if (tableRows && tableRows.length > 0) {
                tableRows.forEach(row => {
                    const label = norm(getField(row, ['시청자 성별', '성별', 'Viewer gender']));
                    if (!label || label === '합계' || label === 'Total') return;
                    const { val, isRaw } = getPctField(row);
                    rawItems.push({ label, val, isRaw });
                });
            }

            if (rawItems.length === 0 && chartRows && chartRows.length > 0) {
                const aggFromChart = new Map();
                chartRows.forEach(row => {
                    const genderLabel = norm(getField(row, ['시청자 성별', '성별', 'Viewer gender']));
                    const val = parseNum(getField(row, ['조회수', '시청 시간', 'Views', 'Watch time']));
                    if (!genderLabel || genderLabel === '합계' || genderLabel === 'Total') return;
                    aggFromChart.set(genderLabel, (aggFromChart.get(genderLabel) || 0) + val);
                });
                aggFromChart.forEach((val, label) => rawItems.push({ label, val, isRaw: true }));
            }

            const items = normalizePctItems(rawItems);
            if (items.length > 0) {
                DEMOGRAPHICS[channelKey].gender = items;
            }
        }

        function processChannelCSVSet(channelKey, csvFiles) {
            const channelName = channelKey === 'whatever' ? '뭐든하기루' : '반지하 유정수';
            let tableRows = [];
            let chartRows = [];
            let totalCsvRows = [];

            for (const [rawName, content] of Object.entries(csvFiles)) {
                const fname = norm(rawName);
                const parsed = parseCSVText(content);
                if (fname.includes('표') || fname.toLowerCase().includes('table')) {
                    tableRows = parsed;
                } else if (fname.includes('차트') || fname.toLowerCase().includes('chart')) {
                    chartRows = parsed;
                } else if (fname.includes('총계') || fname.toLowerCase().includes('total')) {
                    totalCsvRows = parsed;
                } else if (parsed.length > 0 && hasColumn(parsed, ['동영상 제목', '콘텐츠'])) {
                    tableRows = parsed;
                }
            }

            if (tableRows.length === 0) {
                fo
