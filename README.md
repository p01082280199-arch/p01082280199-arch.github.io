<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>인재교육팀 관리시스템</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.css">
<style>
  :root{
    --bg:#F4F3FF;--surface:#FFFFFF;--surface-2:#F9F8FF;--surface-accent:#EEF2FF;
    --border:#E0DFFE;--border-strong:#C7C4F8;
    --text:#1E1B4B;--text-muted:#6366F1;--text-faint:#A5B4FC;
    --indigo:#4F46E5;--indigo-dark:#3730A3;--indigo-light:#EEF2FF;
    --coral:#F97316;--coral-light:#FFF4ED;
    --emerald:#059669;--emerald-light:#ECFDF5;
    --red:#EF4444;--red-light:#FEF2F2;
    --amber:#D97706;--amber-light:#FFFBEB;
    --purple:#7C3AED;--purple-light:#F5F3FF;
    /* 역량 그룹 색상 */
    --cat-common-accent:#4F46E5;--cat-common-bg:#EEF2FF;--cat-common-light:#E0DFFE;
    --cat-leader-accent:#F97316;--cat-leader-bg:#FFF4ED;--cat-leader-light:#FED7AA;
    --cat-job-accent:#059669;--cat-job-bg:#ECFDF5;--cat-job-light:#A7F3D0;
    --cat-etc-accent:#EC4899;--cat-etc-bg:#FDF2F8;--cat-etc-light:#FBCFE8;
    --radius:12px;--radius-sm:8px;
    --shadow:0 1px 3px rgba(79,70,229,.06),0 4px 12px rgba(79,70,229,.08);
    --shadow-lg:0 12px 36px rgba(79,70,229,.22);
    --font:'Pretendard',-apple-system,BlinkMacSystemFont,'Apple SD Gothic Neo','Malgun Gothic',sans-serif;
  }
  *{box-sizing:border-box;}html,body{margin:0;padding:0;}
  body{font-family:var(--font);background:var(--bg);color:var(--text);-webkit-font-smoothing:antialiased;line-height:1.5;}
  button,input,select,textarea{font-family:inherit;font-size:14px;color:inherit;}

  /* ---- Password ---- */
  #pw-overlay{position:fixed;inset:0;z-index:9999;background:linear-gradient(135deg,#3730A3 0%,#4F46E5 50%,#7C3AED 100%);display:flex;align-items:center;justify-content:center;padding:20px;}
  .pw-box{background:#fff;border-radius:20px;padding:40px 36px;width:100%;max-width:380px;box-shadow:0 24px 60px rgba(30,27,75,.4);text-align:center;}
  .pw-icon{font-size:44px;margin-bottom:12px;}
  .pw-title{font-size:20px;font-weight:800;color:#3730A3;margin-bottom:4px;}
  .pw-sub{font-size:13px;color:#A5B4FC;margin-bottom:28px;}
  .pw-input-wrap{position:relative;margin-bottom:14px;}
  .pw-input{width:100%;border:2px solid #E0DFFE;border-radius:10px;padding:12px 44px 12px 14px;font-size:15px;transition:border-color .15s,box-shadow .15s;letter-spacing:.1em;font-family:inherit;}
  .pw-input:focus{outline:none;border-color:#4F46E5;box-shadow:0 0 0 3px rgba(79,70,229,.12);}
  .pw-toggle{position:absolute;right:12px;top:50%;transform:translateY(-50%);border:none;background:transparent;cursor:pointer;font-size:18px;color:#A5B4FC;padding:4px;}
  .pw-btn{width:100%;padding:13px;border:none;border-radius:10px;background:linear-gradient(135deg,#4F46E5,#7C3AED);color:#fff;font-size:15px;font-weight:800;cursor:pointer;font-family:inherit;box-shadow:0 4px 14px rgba(79,70,229,.35);transition:all .15s;}
  .pw-btn:hover{transform:translateY(-1px);}
  .pw-error{font-size:13px;color:#EF4444;margin-top:10px;min-height:20px;font-weight:600;}
  .pw-contact{margin-top:14px;font-size:12px;color:#A5B4FC;}

  /* ---- Header ---- */
  .app-header{background:linear-gradient(135deg,#4F46E5 0%,#7C3AED 60%,#A855F7 100%);padding:22px 28px 0;position:sticky;top:0;z-index:20;box-shadow:0 2px 16px rgba(79,70,229,.35);}
  .header-top{display:flex;align-items:center;justify-content:space-between;max-width:1320px;margin:0 auto;flex-wrap:wrap;gap:10px;}
  .brand{display:flex;align-items:center;gap:12px;}
  .brand-icon{width:40px;height:40px;background:rgba(255,255,255,.18);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0;}
  .brand h1{font-size:18px;font-weight:800;margin:0;color:#fff;letter-spacing:-.3px;}
  .badge-share{font-size:11px;color:#fff;background:rgba(255,255,255,.22);padding:3px 10px;border-radius:999px;font-weight:600;white-space:nowrap;}
  .tabs{display:flex;gap:2px;max-width:1320px;margin:14px auto 0;}
  .tab-btn{appearance:none;border:none;background:rgba(255,255,255,.12);padding:11px 22px;font-size:14px;font-weight:700;color:rgba(255,255,255,.75);border-radius:10px 10px 0 0;cursor:pointer;transition:color .15s,background .15s;}
  .tab-btn:hover{color:#fff;background:rgba(255,255,255,.22);}
  .tab-btn.active{color:#1E1B4B;background:var(--bg);font-weight:800;}

  main{max-width:1320px;margin:0 auto;padding:24px 28px 64px;}
  .screen{display:none;}.screen.active{display:block;}

  /* ---- Infographic ---- */
  .infographic-banner{background:linear-gradient(135deg,#fff 0%,#EEF2FF 100%);border:1px solid var(--border);border-radius:var(--radius);padding:20px 24px;margin-bottom:20px;box-shadow:var(--shadow);overflow:hidden;position:relative;}
  .infographic-banner::before{content:'';position:absolute;right:-40px;top:-40px;width:200px;height:200px;border-radius:50%;background:linear-gradient(135deg,rgba(79,70,229,.07),rgba(124,58,237,.07));pointer-events:none;}
  .infographic-inner{display:flex;align-items:center;gap:16px;flex-wrap:wrap;}
  .infographic-svg{flex-shrink:0;}
  .infographic-stats{display:flex;gap:12px;flex:1;flex-wrap:wrap;}
  .infographic-stat{text-align:center;padding:10px 14px;background:#fff;border-radius:10px;border:1px solid var(--border);flex:1;min-width:90px;}
  .i-icon{font-size:20px;margin-bottom:4px;}
  .i-label{font-size:11px;color:var(--text-faint);font-weight:600;margin-bottom:3px;}
  .i-value{font-size:17px;font-weight:800;color:var(--text);font-variant-numeric:tabular-nums;letter-spacing:-.5px;}
  .i-value.v-indigo{color:var(--indigo);}.i-value.v-coral{color:var(--coral);}.i-value.v-emerald{color:var(--emerald);}.i-value.v-purple{color:var(--purple);}.i-value.v-amber{color:var(--amber);}.i-value.v-red{color:var(--red);}

  /* ---- Toolbar ---- */
  .toolbar{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:14px;flex-wrap:wrap;}
  .toolbar-left{display:flex;gap:8px;flex-wrap:wrap;}.toolbar-right{display:flex;gap:8px;flex-wrap:wrap;}
  .btn{display:inline-flex;align-items:center;gap:6px;border-radius:var(--radius-sm);border:1px solid transparent;padding:9px 16px;font-size:13.5px;font-weight:700;cursor:pointer;transition:all .15s;white-space:nowrap;}
  .btn-primary{background:linear-gradient(135deg,var(--indigo),var(--purple));color:#fff;box-shadow:0 2px 8px rgba(79,70,229,.3);}
  .btn-primary:hover{box-shadow:0 4px 14px rgba(79,70,229,.45);transform:translateY(-1px);}
  .btn-outline{background:var(--surface);color:var(--text);border-color:var(--border-strong);}
  .btn-outline:hover{background:var(--indigo-light);border-color:var(--indigo);color:var(--indigo);}

  /* ---- Table ---- */
  .table-wrap{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);overflow-x:auto;box-shadow:var(--shadow);}
  table{border-collapse:collapse;width:100%;min-width:700px;}
  thead th{text-align:left;font-size:11.5px;font-weight:800;color:var(--text-muted);letter-spacing:.5px;text-transform:uppercase;background:linear-gradient(180deg,#FAFAFE 0%,#F4F3FF 100%);padding:12px 16px;border-bottom:2px solid var(--border);white-space:nowrap;}
  tbody td{padding:12px 16px;font-size:13.5px;border-bottom:1px solid var(--border);vertical-align:middle;font-variant-numeric:tabular-nums;word-break:keep-all;overflow-wrap:break-word;}
  tbody tr:last-child td{border-bottom:none;}

  /* ---- Category group header ---- */
  .cat-header td{
    padding:8px 16px;font-size:12px;font-weight:800;letter-spacing:.8px;text-transform:uppercase;
    border-bottom:1px solid rgba(0,0,0,.06);
  }
  .cat-header.cat-common td{background:var(--cat-common-bg);color:var(--cat-common-accent);border-left:4px solid var(--cat-common-accent);}
  .cat-header.cat-leader td{background:var(--cat-leader-bg);color:var(--cat-leader-accent);border-left:4px solid var(--cat-leader-accent);}
  .cat-header.cat-job   td{background:var(--cat-job-bg);color:var(--cat-job-accent);border-left:4px solid var(--cat-job-accent);}
  .cat-header.cat-etc   td{background:var(--cat-etc-bg);color:var(--cat-etc-accent);border-left:4px solid var(--cat-etc-accent);}
  .cat-badge{display:inline-flex;align-items:center;gap:6px;font-size:12px;font-weight:800;}
  .cat-count{font-size:11px;opacity:.7;font-weight:600;}

  /* ---- Course rows ---- */
  #screen1-tbody tr.course-row{cursor:pointer;transition:background .12s;}
  #screen1-tbody tr.course-row:hover{background:var(--surface-accent);}
  td.name-cell{text-align:left;font-weight:800;min-width:200px;}
  td.actions{text-align:center;white-space:nowrap;width:48px;}

  /* ---- Screen 2 ---- */
  .session-row td:not(.course-cell){cursor:pointer;transition:background .12s;}
  .session-row:hover td:not(.course-cell){background:var(--surface-accent);}
  .course-cell{cursor:default;font-weight:800;vertical-align:middle;text-align:center;word-break:keep-all;}

  /* ---- Common cells ---- */
  .icon-btn{border:none;background:transparent;cursor:pointer;color:var(--text-faint);width:30px;height:30px;border-radius:8px;font-size:14px;display:inline-flex;align-items:center;justify-content:center;transition:all .15s;}
  .icon-btn:hover{background:var(--red-light);color:var(--red);}
  .icon-btn.btn-disabled{cursor:not-allowed;opacity:.35;}.icon-btn.btn-disabled:hover{background:transparent;color:var(--text-faint);}
  .empty-row td{text-align:center;padding:60px 20px;color:var(--text-faint);font-size:14px;}
  .empty-icon{font-size:36px;display:block;margin-bottom:10px;}
  .muted{color:var(--text-faint);}
  .note-cell{color:var(--text-muted);font-size:13px;}
  .spent-cell .rate-tag{display:inline-block;margin-top:3px;font-size:11px;font-weight:700;padding:2px 7px;border-radius:999px;}
  .spent-cell .rate-tag.good{background:var(--emerald-light);color:var(--emerald);}
  .spent-cell .rate-tag.low{background:var(--amber-light);color:var(--amber);}
  .spent-cell .rate-tag.over{background:var(--red-light);color:var(--red);}
  .round-badge{display:inline-flex;align-items:center;justify-content:center;background:var(--indigo-light);color:var(--indigo);font-size:12px;font-weight:800;padding:3px 10px;border-radius:999px;}
  .date-range{font-size:12.5px;color:var(--text-muted);}
  .progress-wrap{margin-top:4px;display:flex;align-items:center;gap:6px;}
  .progress-bar-bg{background:var(--border);border-radius:999px;height:4px;width:70px;display:inline-block;overflow:hidden;}
  .progress-bar-fill{height:4px;border-radius:999px;background:linear-gradient(90deg,var(--indigo),var(--purple));}
  .drag-handle{cursor:grab;color:var(--text-faint);font-size:15px;user-select:none;padding:0 4px;}
  .drag-handle:active{cursor:grabbing;}
  tr.dragging{opacity:.35;}
  tr.drag-over td{background:var(--indigo-light) !important;outline:2px dashed var(--indigo);outline-offset:-2px;}

  /* ---- Category chip in table ---- */
  .cat-chip{display:inline-block;font-size:11px;font-weight:700;padding:2px 8px;border-radius:999px;white-space:nowrap;}
  .cat-chip.cat-common{background:var(--cat-common-bg);color:var(--cat-common-accent);}
  .cat-chip.cat-leader{background:var(--cat-leader-bg);color:var(--cat-leader-accent);}
  .cat-chip.cat-job{background:var(--cat-job-bg);color:var(--cat-job-accent);}
  .cat-chip.cat-etc{background:var(--cat-etc-bg);color:var(--cat-etc-accent);}

  /* ---- Modal ---- */
  .modal-overlay{position:fixed;inset:0;background:rgba(30,27,75,.45);display:none;align-items:center;justify-content:center;z-index:100;padding:20px;backdrop-filter:blur(3px);}
  .modal-overlay.open{display:flex;}
  .modal{background:var(--surface);border-radius:16px;box-shadow:var(--shadow-lg);width:100%;max-width:460px;max-height:90vh;overflow-y:auto;}
  .modal-header{padding:18px 22px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;background:linear-gradient(135deg,var(--indigo-light),#fff);}
  .modal-header h2{font-size:16px;margin:0;font-weight:800;color:var(--indigo-dark);}
  .modal-close{border:none;background:transparent;font-size:18px;cursor:pointer;color:var(--text-faint);width:32px;height:32px;border-radius:8px;transition:all .15s;}
  .modal-close:hover{background:var(--red-light);color:var(--red);}
  .modal-body{padding:20px 22px;}.field{margin-bottom:16px;}.field:last-child{margin-bottom:0;}
  .field label{display:block;font-size:12px;font-weight:800;color:var(--text-muted);margin-bottom:6px;letter-spacing:.3px;text-transform:uppercase;}
  .field .static-value{font-size:15px;font-weight:800;padding:9px 0;color:var(--indigo-dark);}
  .field input[type=text],.field input[type=number],.field textarea,.field select{width:100%;border:1.5px solid var(--border);border-radius:var(--radius-sm);padding:9px 12px;font-size:14px;background:var(--surface);transition:border-color .15s,box-shadow .15s;}
  .field input:focus,.field select:focus,.field textarea:focus{outline:none;border-color:var(--indigo);box-shadow:0 0 0 3px rgba(79,70,229,.12);}
  .field textarea{resize:vertical;min-height:58px;}
  .field-row{display:flex;gap:10px;}.field-row .field{flex:1;}
  .date-field{display:flex;gap:6px;}.date-field input[type=text]{flex:1;}
  .date-pick-btn{border:1.5px solid var(--border);background:var(--surface);border-radius:var(--radius-sm);width:40px;cursor:pointer;font-size:16px;flex-shrink:0;transition:background .15s;}
  .date-pick-btn:hover{background:var(--indigo-light);}
  .date-native{position:absolute;opacity:0;width:0;height:0;pointer-events:none;}
  .field-hint{font-size:11.5px;color:var(--text-faint);margin-top:5px;}
  .modal-footer{padding:16px 22px 20px;display:flex;justify-content:flex-end;gap:8px;border-top:1px solid var(--border);}

  .i-rate{font-size:11.5px;font-weight:700;margin-top:3px;}
  .i-rate.r-good{color:var(--emerald);}.i-rate.r-low{color:var(--amber);}.i-rate.r-over{color:var(--red);}

  /* ---- Category radio selector ---- */
  .cat-radio-group{display:grid;grid-template-columns:1fr 1fr;gap:8px;}
  .cat-radio{display:none;}
  .cat-radio-label{
    display:flex;align-items:center;gap:8px;padding:10px 12px;
    border:2px solid var(--border);border-radius:var(--radius-sm);cursor:pointer;
    font-size:13px;font-weight:700;transition:all .15s;
  }
  .cat-radio:checked+.cat-radio-label{border-width:2px;}
  .cat-radio.cat-common:checked+.cat-radio-label{border-color:var(--cat-common-accent);background:var(--cat-common-bg);color:var(--cat-common-accent);}
  .cat-radio.cat-leader:checked+.cat-radio-label{border-color:var(--cat-leader-accent);background:var(--cat-leader-bg);color:var(--cat-leader-accent);}
  .cat-radio.cat-job:checked+.cat-radio-label{border-color:var(--cat-job-accent);background:var(--cat-job-bg);color:var(--cat-job-accent);}
  .cat-radio.cat-etc:checked+.cat-radio-label{border-color:var(--cat-etc-accent);background:var(--cat-etc-bg);color:var(--cat-etc-accent);}

  /* ---- Toast / Loading ---- */
  #toast-container{position:fixed;right:22px;bottom:22px;display:flex;flex-direction:column;gap:8px;z-index:200;}
  .toast{background:var(--indigo-dark);color:#fff;padding:12px 16px;border-radius:10px;font-size:13.5px;box-shadow:var(--shadow-lg);opacity:0;transform:translateY(8px);transition:opacity .2s,transform .2s;max-width:340px;}
  .toast.show{opacity:1;transform:translateY(0);}
  .toast-warn{background:var(--amber);}.toast-error{background:var(--red);}.toast-success{background:var(--emerald);}
  .loading-screen{display:flex;align-items:center;justify-content:center;padding:80px 20px;color:var(--text-faint);font-size:15px;gap:10px;}

  /* ---- Summary rows ---- */
  .total-row td{background:linear-gradient(135deg,#EEF2FF 0%,#E8E5FF 100%) !important;color:var(--indigo-dark) !important;font-weight:800;border-bottom:2px solid var(--border-strong) !important;border-top:2px solid var(--border-strong) !important;}
  .total-row td .muted{color:var(--text-faint) !important;}
  .total-row .total-label{font-size:13px;font-weight:800;display:flex;align-items:center;gap:6px;color:var(--indigo-dark);}
  .subtotal-row td:not(.course-cell){background:var(--surface-2) !important;color:var(--indigo-dark);font-weight:700;border-top:2px dashed var(--border-strong) !important;border-bottom:1px solid var(--border) !important;}
  .subtotal-row .course-cell{border-top:2px dashed var(--border-strong) !important;border-bottom:1px solid var(--border) !important;}
  .subtotal-row .sub-label{font-size:12px;font-weight:800;color:var(--text-muted);}

  @media(max-width:760px){.app-header{padding:16px 16px 0;}main{padding:16px 14px 48px;}.infographic-stats{gap:8px;}.field-row{flex-direction:column;gap:0;}.cat-radio-group{grid-template-columns:1fr 1fr;}}
</style>
</head>
<body>

<!-- PASSWORD -->
<div id="pw-overlay">
  <div class="pw-box">
    <div class="pw-icon">🎓</div>
    <div class="pw-title">인재교육팀 관리시스템</div>
    <div class="pw-sub">접근하려면 비밀번호를 입력하세요</div>
    <div class="pw-input-wrap">
      <input class="pw-input" type="password" id="pw-input" placeholder="비밀번호 입력" autocomplete="off" lang="ko">
      <button type="button" class="pw-toggle" id="pw-toggle">👁</button>
    </div>
    <button class="pw-btn" type="button" id="pw-submit">입력</button>
    <div class="pw-error" id="pw-error"></div>
    <div class="pw-contact">문의 박근아(0124)</div>
  </div>
</div>

<header class="app-header">
  <div class="header-top">
    <div class="brand">
      <div class="brand-icon">🎓</div>
      <h1>인재교육팀 관리시스템</h1>
    </div>
    <span class="badge-share">🔗 Firebase 공유 데이터</span>
  </div>
  <nav class="tabs">
    <button class="tab-btn active" data-tab="screen1" type="button">📊 전체 현황</button>
    <button class="tab-btn" data-tab="screen2" type="button">📋 세부 교육운영현황</button>
  </nav>
</header>

<main>
  <div id="loading-screen" class="loading-screen">⏳ 데이터를 불러오는 중입니다...</div>

  <!-- SCREEN 1 -->
  <section class="screen" id="screen1">
    <div class="infographic-banner">
      <div class="infographic-inner">
        <svg class="infographic-svg" width="84" height="84" viewBox="0 0 84 84" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="8" y="52" width="46" height="8" rx="3" fill="#4F46E5" opacity=".9"/>
          <rect x="11" y="42" width="40" height="12" rx="3" fill="#7C3AED" opacity=".85"/>
          <rect x="14" y="30" width="34" height="14" rx="3" fill="#A855F7" opacity=".8"/>
          <ellipse cx="64" cy="20" rx="13" ry="5" fill="#F97316" opacity=".9"/>
          <polygon points="64,9 76,20 64,20 52,20" fill="#F97316" opacity=".7"/>
          <rect x="74" y="20" width="2.5" height="11" rx="1" fill="#F97316" opacity=".7"/>
          <circle cx="75.25" cy="32" r="3" fill="#F97316" opacity=".7"/>
          <rect x="10" y="66" width="7" height="13" rx="2" fill="#059669" opacity=".8"/>
          <rect x="20" y="60" width="7" height="19" rx="2" fill="#059669" opacity=".85"/>
          <rect x="30" y="54" width="7" height="25" rx="2" fill="#059669" opacity=".9"/>
          <rect x="40" y="59" width="7" height="20" rx="2" fill="#4F46E5" opacity=".8"/>
        </svg>
        <div class="infographic-stats">
          <div class="infographic-stat"><div class="i-icon">📚</div><div class="i-label">전체 교육과정</div><div class="i-value v-indigo" id="inf-courses">0건</div></div>
          <div class="infographic-stat"><div class="i-icon">✅</div><div class="i-label">총 운영과정</div><div class="i-value v-purple" id="inf-active-courses">0건</div></div>
          <div class="infographic-stat"><div class="i-icon">🔄</div><div class="i-label">총 운영회차</div><div class="i-value v-purple" id="inf-sessions">0회</div></div>
          <div class="infographic-stat"><div class="i-icon">🎓</div><div class="i-label">총 수료인원</div><div class="i-value v-emerald" id="inf-completed">0명</div></div>
          <div class="infographic-stat"><div class="i-icon">💰</div><div class="i-label">총 예산</div><div class="i-value v-coral" id="inf-budget">—</div></div>
          <div class="infographic-stat"><div class="i-icon">💳</div><div class="i-label">총 집행액</div><div class="i-value v-amber" id="inf-spent">—</div><div class="i-rate" id="inf-rate-sub"></div></div>
        </div>
      </div>
    </div>

    <div class="toolbar">
      <div class="toolbar-left"><button class="btn btn-primary" id="btn-add-course" type="button">＋ 교육 추가</button></div>
      <div class="toolbar-right"><button class="btn btn-outline" id="btn-export-screen1" type="button">⬇ CSV 다운로드</button></div>
    </div>

    <div class="table-wrap">
      <table>
        <thead><tr>
          <th style="width:36px"></th>
          <th style="min-width:220px">교육명</th>
          <th style="width:80px">운영회차</th>
          <th style="width:90px">수료인원</th>
          <th style="min-width:150px">예산</th>
          <th style="min-width:170px">실제집행액 · 집행률</th>
          <th style="min-width:180px">비고</th>
          <th style="width:48px"></th>
        </tr></thead>
        <tbody id="screen1-tbody"></tbody>
      </table>
    </div>
  </section>

  <!-- SCREEN 2 -->
  <section class="screen" id="screen2">
    <div class="toolbar">
      <div class="toolbar-left"><button class="btn btn-primary" id="btn-add-session" type="button">＋ 세부회차 추가</button></div>
      <div class="toolbar-right"><button class="btn btn-outline" id="btn-export-screen2" type="button">⬇ CSV 다운로드</button></div>
    </div>
    <div class="table-wrap">
      <table>
        <thead><tr>
          <th style="width:36px"></th>
          <th style="min-width:180px">교육명</th>
          <th style="width:110px">세부회차</th>
          <th style="min-width:150px">교육 일정</th>
          <th style="width:80px">계획인원</th>
          <th style="width:110px">이수인원<br>(이수율)</th>
          <th style="min-width:140px">소요비용</th>
          <th style="width:80px">만족도</th>
          <th style="min-width:160px">비고</th>
          <th style="width:48px"></th>
        </tr></thead>
        <tbody id="screen2-tbody"></tbody>
      </table>
    </div>
  </section>
</main>

<!-- Course Modal -->
<div class="modal-overlay" id="course-modal-overlay">
  <div class="modal" role="dialog" aria-modal="true">
    <div class="modal-header">
      <h2 id="course-modal-title">교육 추가</h2>
      <button class="modal-close" type="button" data-close="course">✕</button>
    </div>
    <form id="course-form">
      <div class="modal-body">
        <div class="field">
          <label>역량 구분</label>
          <div class="cat-radio-group">
            <div><input type="radio" class="cat-radio cat-common" name="course-cat" id="cat-common" value="공통역량"><label class="cat-radio-label" for="cat-common">🟣 공통역량</label></div>
            <div><input type="radio" class="cat-radio cat-leader" name="course-cat" id="cat-leader" value="리더십역량"><label class="cat-radio-label" for="cat-leader">🟠 리더십역량</label></div>
            <div><input type="radio" class="cat-radio cat-job"    name="course-cat" id="cat-job"    value="직무역량"><label class="cat-radio-label" for="cat-job">🟢 직무역량</label></div>
            <div><input type="radio" class="cat-radio cat-etc"    name="course-cat" id="cat-etc"    value="기타역량"><label class="cat-radio-label" for="cat-etc">🟤 기타역량</label></div>
          </div>
        </div>
        <div class="field">
          <label for="course-name-input">교육명</label>
          <input type="text" id="course-name-input" placeholder="예: 힐리언즈 입문과정" autocomplete="off" lang="ko" inputmode="text">
        </div>
        <div class="field">
          <label for="course-budget-input">예산</label>
          <input type="number" id="course-budget-input" min="0" step="1" placeholder="예: 5000000">
          <div class="field-hint">숫자만 입력 — 화면에는 1,000,000원 형식으로 표시됩니다.</div>
        </div>
        <div class="field">
          <label for="course-note-input">비고</label>
          <textarea id="course-note-input" placeholder="메모를 남겨주세요 (선택)" lang="ko"></textarea>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-outline" data-close="course">취소</button>
        <button type="submit" class="btn btn-primary">저장</button>
      </div>
    </form>
  </div>
</div>

<!-- Session Modal -->
<div class="modal-overlay" id="session-modal-overlay">
  <div class="modal" role="dialog" aria-modal="true">
    <div class="modal-header">
      <h2 id="session-modal-title">세부회차 추가</h2>
      <button class="modal-close" type="button" data-close="session">✕</button>
    </div>
    <form id="session-form">
      <div class="modal-body">
        <div class="field" id="session-course-select-wrap">
          <label for="session-course-select">교육명</label>
          <select id="session-course-select" lang="ko"></select>
        </div>
        <div class="field" id="session-course-display-wrap" style="display:none;">
          <label>교육명</label>
          <div class="static-value" id="session-course-display"></div>
        </div>
        <div class="field">
          <label for="session-round-input">세부회차</label>
          <input type="text" id="session-round-input" placeholder="예: 1차" autocomplete="off" lang="ko" inputmode="text">
        </div>
        <div class="field-row">
          <div class="field">
            <label>교육시작일</label>
            <div class="date-field">
              <input type="text" id="session-start-text" placeholder="2026-06-28" lang="ko">
              <button type="button" class="date-pick-btn" data-target="session-start-native" title="달력">📅</button>
              <input type="date" id="session-start-native" class="date-native">
            </div>
          </div>
          <div class="field">
            <label>교육종료일</label>
            <div class="date-field">
              <input type="text" id="session-end-text" placeholder="2026-06-28" lang="ko">
              <button type="button" class="date-pick-btn" data-target="session-end-native" title="달력">📅</button>
              <input type="date" id="session-end-native" class="date-native">
            </div>
          </div>
        </div>
        <div class="field-row">
          <div class="field">
            <label for="session-planned-input">계획인원</label>
            <input type="number" id="session-planned-input" min="0" step="1" placeholder="예: 30">
          </div>
          <div class="field">
            <label for="session-completed-input">이수인원</label>
            <input type="number" id="session-completed-input" min="0" step="1" placeholder="미입력 시 —">
          </div>
        </div>
        <div class="field-row">
          <div class="field">
            <label for="session-cost-input">소요비용</label>
            <input type="number" id="session-cost-input" min="0" step="1" placeholder="미입력 시 —">
          </div>
          <div class="field">
            <label for="session-satisfaction-input">만족도</label>
            <input type="number" id="session-satisfaction-input" min="0" step="0.01" placeholder="미입력 시 —">
          </div>
        </div>
        <div class="field">
          <label for="session-note-input">비고</label>
          <textarea id="session-note-input" placeholder="메모를 남겨주세요 (선택)" lang="ko"></textarea>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-outline" data-close="session">취소</button>
        <button type="submit" class="btn btn-primary">저장</button>
      </div>
    </form>
  </div>
</div>

<div id="toast-container"></div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, doc, getDoc, setDoc } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyBZy4K76aCaN8dnhpghYy3E5srJPjZruEM",
  authDomain: "ilsan-hrd.firebaseapp.com",
  projectId: "ilsan-hrd",
  storageBucket: "ilsan-hrd.firebasestorage.app",
  messagingSenderId: "230846100323",
  appId: "1:230846100323:web:add7823b4ce9f19f7b9956"
};
const app = initializeApp(firebaseConfig);
const db  = getFirestore(app);
const COL = "edu-management";

async function fbGet(key){ const s=await getDoc(doc(db,COL,key)); return s.exists()?s.data().value:null; }
async function fbSet(key,val){ await setDoc(doc(db,COL,key),{value:val}); }

/* ---- Constants ---- */
const CATEGORIES = ['공통역량','리더십역량','직무역량','기타역량'];
const CAT_KEYS   = {'공통역량':'common','리더십역량':'leader','직무역량':'job','기타역량':'etc'};
const CAT_ICONS  = {'공통역량':'🟣','리더십역량':'🟠','직무역량':'🟢','기타역량':'🟤'};
  const CAT_ICONS_HTML = {
    '공통역량':'🟣','리더십역량':'🟠','직무역량':'🟢',
    '기타역량':'<svg width="15" height="15" viewBox="0 0 15 15" style="vertical-align:middle;margin-right:1px;flex-shrink:0"><circle cx="7.5" cy="7.5" r="7" fill="#EC4899"/></svg>'
  };

/* ---- State ---- */
let courses=[], sessions=[];
let editingCourseId=null, editingSessionId=null;

/* ---- Utils ---- */
function uid(){ return Date.now().toString(36)+Math.random().toString(36).slice(2,8); }
function esc(s){ if(s==null)return''; return String(s).replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
function escName(s){
  // wrap at ( and , with line break
  if(s==null)return'';
  return esc(s).replace(/([,（(])/g,'<br>$1');
}
function numOrZero(v){ const n=Number(v); return isNaN(n)?0:n; }
function hasNum(v){ return v!==null&&v!==undefined&&v!==''&&!isNaN(Number(v)); }
function fmtNum(n){ return new Intl.NumberFormat('ko-KR').format(n); }
function fmtCur(v){ if(!hasNum(v))return'<span class="muted">—</span>'; return fmtNum(Math.round(Number(v)))+'원'; }
function fmtSat(v){ if(!hasNum(v))return'<span class="muted">—</span>'; return Number(v).toFixed(2); }
function fmtHead(planned,completed){
  if(!hasNum(completed))return'<span class="muted">—</span>';
  if(!hasNum(planned)||Number(planned)===0)return fmtNum(completed)+'명';
  const rate=Number(completed)/Number(planned)*100;
  return fmtNum(completed)+'명<br>('+rate.toFixed(2)+'%)';
}
function today(){ const d=new Date(); return d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0')+'-'+String(d.getDate()).padStart(2,'0'); }
function validDate(s){ if(!s)return true; return /^\d{4}-\d{2}-\d{2}$/.test(s)&&!isNaN(Date.parse(s)); }
function catKey(cat){ return CAT_KEYS[cat]||'etc'; }

/* ---- Toast ---- */
function toast(msg,type){
  const c=document.getElementById('toast-container');
  const el=document.createElement('div');
  el.className='toast'+(type?' toast-'+type:'');
  el.textContent=msg; c.appendChild(el);
  requestAnimationFrame(()=>el.classList.add('show'));
  setTimeout(()=>{ el.classList.remove('show'); setTimeout(()=>el.remove(),300); },3200);
}

/* ---- Firebase ---- */
async function loadData(){
  const loadEl=document.getElementById('loading-screen');
  const timeout=setTimeout(()=>{
    loadEl.style.display='none';
    document.getElementById('screen1').classList.add('active');
    toast('Firebase 연결 시간 초과 — Firestore 규칙을 확인해주세요.','error');
    renderAll();
  },5000);
  try{
    const [cR,sR]=await Promise.all([fbGet('courses'),fbGet('sessions')]);
    courses=cR?JSON.parse(cR):[];
    sessions=sR?JSON.parse(sR):[];
    // migrate: add category default for old data
    courses.forEach(c=>{ if(!c.category) c.category='기타역량'; });
  }catch(e){
    console.error(e); toast('데이터 로드 실패: '+e.message,'error');
    courses=[]; sessions=[];
  }finally{
    clearTimeout(timeout);
    loadEl.style.display='none';
    document.getElementById('screen1').classList.add('active');
    renderAll();
  }
}
async function saveC(){ try{ await fbSet('courses',JSON.stringify(courses)); }catch(e){ toast('저장 실패','error'); } }
async function saveS(){ try{ await fbSet('sessions',JSON.stringify(sessions)); }catch(e){ toast('저장 실패','error'); } }

/* ---- Aggregation ---- */
function sessOf(cid){ return sessions.filter(s=>s.courseId===cid); }
function agg(c){
  const g=sessOf(c.id);
  const opCount=g.length;
  const completedSum=g.reduce((s,x)=>s+numOrZero(x.completedCount),0);
  const actualSpent=g.reduce((s,x)=>s+numOrZero(x.cost),0);
  const budget=numOrZero(c.budget);
  const rate=(opCount>0&&budget>0)?(actualSpent/budget*100):null;
  return{opCount,completedSum,actualSpent,rate};
}

/* ---- Infographic ---- */
function renderInfographic(){
  const totalBudget=courses.reduce((s,c)=>s+numOrZero(c.budget),0);
  const totalActual=sessions.reduce((s,x)=>s+numOrZero(x.cost),0);
  const totalCompleted=sessions.reduce((s,x)=>s+numOrZero(x.completedCount),0);
  const overallRate=totalBudget>0?(totalActual/totalBudget*100):null;
  const activeCourseCount=courses.filter(c=>sessions.some(s=>s.courseId===c.id&&hasNum(s.completedCount)&&Number(s.completedCount)>0)).length;
  document.getElementById('inf-courses').textContent=courses.length+'건';
  document.getElementById('inf-active-courses').textContent=activeCourseCount+'건';
  document.getElementById('inf-sessions').textContent=sessions.length+'회';
  document.getElementById('inf-completed').textContent=fmtNum(totalCompleted)+'명';
  document.getElementById('inf-budget').textContent=totalBudget>0?fmtNum(totalBudget)+'원':'—';
  document.getElementById('inf-spent').textContent=totalActual>0?fmtNum(totalActual)+'원':'—';
  const rateSubEl=document.getElementById('inf-rate-sub');
  if(overallRate===null){ rateSubEl.textContent=''; }
  else{
    rateSubEl.textContent='집행률 : '+overallRate.toFixed(1)+'%';
    rateSubEl.className='i-rate '+(overallRate>100?'r-over':overallRate>=80?'r-good':'r-low');
  }
}

/* ---- Screen 1 ---- */
function renderScreen1(){
  renderInfographic();
  const tbody=document.getElementById('screen1-tbody');
  tbody.innerHTML='';
  if(courses.length===0){
    tbody.innerHTML='<tr class="empty-row"><td colspan="8"><span class="empty-icon">📚</span>등록된 교육과정이 없습니다.<br>[＋ 교육 추가] 버튼으로 시작하세요.</td></tr>';
    return;
  }
  CATEGORIES.forEach(cat=>{
    const group=courses.filter(c=>c.category===cat);
    if(group.length===0)return;
    const ck=catKey(cat);
    // group header row
    const htr=document.createElement('tr');
    htr.className='cat-header cat-'+ck;
    htr.innerHTML=`<td colspan="8"><span class="cat-badge">${CAT_ICONS_HTML[cat]} ${cat} <span class="cat-count">(${group.length}개 과정)</span></span></td>`;
    tbody.appendChild(htr);
    // course rows
    group.forEach(c=>{
      const a=agg(c); const blocked=a.opCount>0;
      let spentHtml='<span class="muted">—</span>';
      if(a.opCount>0){
        let rateTag='';
        if(a.rate!==null){
          const cls=a.rate>100?'over':a.rate>=80?'good':'low';
          rateTag=`<div class="progress-wrap"><div class="progress-bar-bg"><div class="progress-bar-fill" style="width:${Math.min(a.rate,100)}%"></div></div><span class="rate-tag ${cls}">${a.rate.toFixed(2)}%</span></div>`;
        }
        spentHtml=`<div>${fmtNum(Math.round(a.actualSpent))}원</div>${rateTag}`;
      }
      const tr=document.createElement('tr');
      tr.className='course-row';
      tr.draggable=true;
      tr.dataset.courseId=c.id;
      tr.innerHTML=`
        <td style="text-align:center;"><span class="drag-handle">⠿</span></td>
        <td class="name-cell" style="background:var(--cat-${ck}-bg);border-left:3px solid var(--cat-${ck}-accent);">${escName(c.name)}</td>
        <td style="text-align:center;"><span class="round-badge">${a.opCount}회</span></td>
        <td>${a.opCount>0?fmtNum(a.completedSum)+'명':'<span class="muted">—</span>'}</td>
        <td>${hasNum(c.budget)?fmtNum(Math.round(c.budget))+'원':'<span class="muted">—</span>'}</td>
        <td class="spent-cell">${spentHtml}</td>
        <td class="note-cell">${c.note?esc(c.note):'<span class="muted">—</span>'}</td>
        <td class="actions"><button class="icon-btn delete-btn${blocked?' btn-disabled':''}" data-id="${c.id}" title="${blocked?'세부회차를 먼저 모두 삭제':'삭제'}">✕</button></td>
      `;
      tr.addEventListener('click',(e)=>{ if(e.target.closest('.delete-btn')||e.target.closest('.drag-handle'))return; openCourseEdit(c.id); });
      tr.querySelector('.delete-btn').addEventListener('click',(e)=>{ e.stopPropagation(); deleteCourse(c.id); });
      addCourseDrag(tr,c.id);
      tbody.appendChild(tr);
    });
  });
}

/* ---- Screen 2 ---- */
function renderScreen2(){
  const tbody=document.getElementById('screen2-tbody');
  tbody.innerHTML='';
  if(sessions.length===0){
    tbody.innerHTML='<tr class="empty-row"><td colspan="10"><span class="empty-icon">📋</span>등록된 세부회차가 없습니다.<br>[＋ 세부회차 추가]로 시작하세요.</td></tr>';
    return;
  }

  /* ---- 전체 합계 행 ---- */
  const totalCompleted=sessions.reduce((s,x)=>s+numOrZero(x.completedCount),0);
  const totalCost=sessions.reduce((s,x)=>s+numOrZero(x.cost),0);
  const satList=sessions.filter(x=>hasNum(x.satisfaction));
  const avgSat=satList.length?satList.reduce((s,x)=>s+Number(x.satisfaction),0)/satList.length:null;
  const totalTr=document.createElement('tr');
  totalTr.className='total-row';
  totalTr.innerHTML=`
    <td colspan="4" style="padding:12px 16px;"><span class="total-label">📊 전체 합계</span></td>
    <td style="text-align:center;font-size:12px;color:rgba(255,255,255,.65);">—</td>
    <td style="text-align:center;font-size:14px;font-weight:800;">${fmtNum(totalCompleted)}명</td>
    <td style="font-size:14px;font-weight:800;">${totalCost>0?fmtNum(Math.round(totalCost))+'원':'<span class="muted">—</span>'}</td>
    <td style="text-align:center;font-size:14px;font-weight:800;">${avgSat!==null?avgSat.toFixed(2):'<span class="muted">—</span>'}</td>
    <td colspan="2"></td>
  `;
  tbody.appendChild(totalTr);

  /* ---- 그룹별 행 + 소계 ---- */
  const groups={};
  sessions.forEach(s=>{ (groups[s.courseId]=groups[s.courseId]||[]).push(s); });
  const cids=courses.filter(c=>groups[c.id]).map(c=>c.id);
  cids.forEach(cid=>{
    const c=courses.find(x=>x.id===cid);
    const g=groups[cid];
    const ck=catKey(c?c.category:'기타역량');

    /* 소계 행 먼저 — course-cell rowspan 안에 포함, 세부회차 위에 위치 */
    const subCompleted=g.reduce((s,x)=>s+numOrZero(x.completedCount),0);
    const subCost=g.reduce((s,x)=>s+numOrZero(x.cost),0);
    const subSatList=g.filter(x=>hasNum(x.satisfaction));
    const subAvgSat=subSatList.length?subSatList.reduce((s,x)=>s+Number(x.satisfaction),0)/subSatList.length:null;
    const subTr=document.createElement('tr');
    subTr.className='subtotal-row';
    // rowspan = g.length + 1 (소계 행 + 세부회차 행들) — course-cell이 이 행부터 시작
    subTr.innerHTML=`
      <td style="text-align:center;width:36px;"></td>
      <td colspan="3" style="padding:9px 16px;"><span class="sub-label">▶ 소계</span></td>
      <td style="text-align:center;font-weight:800;color:var(--indigo-dark);">${fmtNum(subCompleted)}명</td>
      <td style="font-weight:800;color:var(--indigo-dark);">${subCost>0?fmtNum(Math.round(subCost))+'원':'<span class="muted">—</span>'}</td>
      <td style="text-align:center;font-weight:800;color:var(--indigo-dark);">${subAvgSat!==null?subAvgSat.toFixed(2):'<span class="muted">—</span>'}</td>
      <td colspan="2"></td>
    `;
    // course-cell을 소계 행에 삽입 (rowspan = g.length + 1)
    const courseFirstTd=document.createElement('td');
    courseFirstTd.className='course-cell';
    courseFirstTd.setAttribute('rowspan', g.length+1);
    courseFirstTd.setAttribute('style',`background:var(--cat-${ck}-bg);border-left:3px solid var(--cat-${ck}-accent);`);
    courseFirstTd.innerHTML=escName(c.name);
    subTr.insertBefore(courseFirstTd, subTr.children[1]);
    tbody.appendChild(subTr);

    g.forEach((s)=>{
      const tr=document.createElement('tr');
      tr.className='session-row';
      const dateRange=(s.startDate||s.endDate)?`<span class="date-range">${s.startDate||'?'}<br>~${s.endDate||'?'}</span>`:'<span class="muted">—</span>';
      tr.innerHTML=`
        <td style="text-align:center;"><span class="drag-handle">⠿</span></td>
        <td style="text-align:center;"><span class="round-badge">${s.round?esc(s.round):'—'}</span></td>
        <td>${dateRange}</td>
        <td style="text-align:center;">${hasNum(s.plannedCount)?fmtNum(s.plannedCount)+'명':'<span class="muted">—</span>'}</td>
        <td style="text-align:center;">${fmtHead(s.plannedCount,s.completedCount)}</td>
        <td>${fmtCur(s.cost)}</td>
        <td style="text-align:center;">${fmtSat(s.satisfaction)}</td>
        <td class="note-cell">${s.note?esc(s.note):'<span class="muted">—</span>'}</td>
        <td class="actions"><button class="icon-btn delete-btn" data-id="${s.id}">✕</button></td>
      `;
      tr.addEventListener('click',(e)=>{ if(e.target.closest('.delete-btn')||e.target.closest('.course-cell')||e.target.closest('.drag-handle'))return; openSessionEdit(s.id); });
      tr.querySelector('.delete-btn').addEventListener('click',(e)=>{ e.stopPropagation(); deleteSession(s.id); });
      addSessionDrag(tr,s.id);
      tbody.appendChild(tr);
    });
  });
}

function renderAll(){ renderScreen1(); renderScreen2(); }

/* ---- Drag: Courses ---- */
let dragCourseId=null;
function addCourseDrag(tr,courseId){
  tr.addEventListener('dragstart',(e)=>{ dragCourseId=courseId; setTimeout(()=>tr.classList.add('dragging'),0); e.dataTransfer.effectAllowed='move'; });
  tr.addEventListener('dragend',()=>{ tr.classList.remove('dragging'); dragCourseId=null; });
  tr.addEventListener('dragover',(e)=>{ e.preventDefault(); tr.classList.add('drag-over'); });
  tr.addEventListener('dragleave',()=>tr.classList.remove('drag-over'));
  tr.addEventListener('drop',async(e)=>{
    e.preventDefault(); tr.classList.remove('drag-over');
    if(!dragCourseId||dragCourseId===courseId)return;
    const fi=courses.findIndex(c=>c.id===dragCourseId);
    const ti=courses.findIndex(c=>c.id===courseId);
    if(fi<0||ti<0)return;
    const [m]=courses.splice(fi,1); courses.splice(ti,0,m);
    await saveC(); renderAll();
  });
}

/* ---- Drag: Sessions ---- */
let dragSessionId=null;
function addSessionDrag(tr,sessionId){
  tr.draggable=true;
  tr.addEventListener('dragstart',(e)=>{ dragSessionId=sessionId; setTimeout(()=>tr.classList.add('dragging'),0); e.dataTransfer.effectAllowed='move'; });
  tr.addEventListener('dragend',()=>{ tr.classList.remove('dragging'); dragSessionId=null; });
  tr.addEventListener('dragover',(e)=>{ e.preventDefault(); tr.classList.add('drag-over'); });
  tr.addEventListener('dragleave',()=>tr.classList.remove('drag-over'));
  tr.addEventListener('drop',async(e)=>{
    e.preventDefault(); tr.classList.remove('drag-over');
    if(!dragSessionId||dragSessionId===sessionId)return;
    const fi=sessions.findIndex(s=>s.id===dragSessionId);
    const ti=sessions.findIndex(s=>s.id===sessionId);
    if(fi<0||ti<0)return;
    const [m]=sessions.splice(fi,1); sessions.splice(ti,0,m);
    await saveS(); renderAll();
  });
}

/* ---- Tabs ---- */
document.querySelectorAll('.tab-btn').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.tab-btn').forEach(b=>b.classList.toggle('active',b===btn));
    document.querySelectorAll('.screen').forEach(s=>s.classList.toggle('active',s.id===btn.dataset.tab));
  });
});

/* ---- Course modal ---- */
const courseOverlay=document.getElementById('course-modal-overlay');
function closeCourseModal(){ courseOverlay.classList.remove('open'); }
function setCatRadio(val){
  document.querySelectorAll('input[name="course-cat"]').forEach(r=>{ r.checked=r.value===val; });
}
function getCatRadio(){
  const r=document.querySelector('input[name="course-cat"]:checked');
  return r?r.value:'공통역량';
}
function openCourseAdd(){
  editingCourseId=null;
  document.getElementById('course-modal-title').textContent='교육 추가';
  setCatRadio('공통역량');
  document.getElementById('course-name-input').value='';
  document.getElementById('course-budget-input').value='';
  document.getElementById('course-note-input').value='';
  courseOverlay.classList.add('open');
  setTimeout(()=>document.getElementById('course-name-input').focus(),50);
}
function openCourseEdit(id){
  const c=courses.find(x=>x.id===id); if(!c)return;
  editingCourseId=id;
  document.getElementById('course-modal-title').textContent='교육 수정';
  setCatRadio(c.category||'기타역량');
  document.getElementById('course-name-input').value=c.name;
  document.getElementById('course-budget-input').value=c.budget??'';
  document.getElementById('course-note-input').value=c.note||'';
  courseOverlay.classList.add('open');
  setTimeout(()=>document.getElementById('course-name-input').focus(),50);
}
document.getElementById('btn-add-course').addEventListener('click',openCourseAdd);
courseOverlay.querySelectorAll('[data-close="course"]').forEach(el=>el.addEventListener('click',closeCourseModal));
courseOverlay.addEventListener('click',(e)=>{ if(e.target===courseOverlay)closeCourseModal(); });

document.getElementById('course-form').addEventListener('submit',async(e)=>{
  e.preventDefault();
  const name=document.getElementById('course-name-input').value.trim();
  const budgetVal=document.getElementById('course-budget-input').value;
  const note=document.getElementById('course-note-input').value.trim();
  const category=getCatRadio();
  if(!name){ toast('교육명을 입력해주세요.','warn'); return; }
  if(budgetVal===''||isNaN(Number(budgetVal))||Number(budgetVal)<0){ toast('예산을 올바르게 입력해주세요.','warn'); return; }
  if(editingCourseId===null){
    courses.push({id:uid(),name,budget:Number(budgetVal),note,category});
    await saveC(); toast('교육과정이 추가되었습니다.','success');
  }else{
    const c=courses.find(x=>x.id===editingCourseId); if(!c)return;
    c.name=name; c.budget=Number(budgetVal); c.note=note; c.category=category;
    await saveC(); toast('수정되었습니다.','success');
  }
  closeCourseModal(); renderAll();
});

function deleteCourse(id){
  if(sessOf(id).length>0){ toast('세부회차가 있어 삭제할 수 없습니다. 세부 교육운영현황에서 먼저 삭제해주세요.','warn'); return; }
  const c=courses.find(x=>x.id===id); if(!c)return;
  if(!confirm(`'${c.name}'을(를) 삭제하시겠습니까?`))return;
  courses=courses.filter(x=>x.id!==id); saveC(); renderAll();
}

/* ---- Session modal ---- */
const sessionOverlay=document.getElementById('session-modal-overlay');
function closeSessionModal(){ sessionOverlay.classList.remove('open'); }
function populateCourseSelect(){
  const sel=document.getElementById('session-course-select');
  sel.innerHTML='';
  if(courses.length===0){ sel.innerHTML='<option value="">등록된 교육과정이 없습니다</option>'; return; }
  CATEGORIES.forEach(cat=>{
    const grp=courses.filter(c=>c.category===cat);
    if(grp.length===0)return;
    const og=document.createElement('optgroup'); og.label=CAT_ICONS[cat]+' '+cat;
    grp.forEach(c=>{ const o=document.createElement('option'); o.value=c.id; o.textContent=c.name; og.appendChild(o); });
    sel.appendChild(og);
  });
}
document.getElementById('session-start-native').addEventListener('change',function(){ document.getElementById('session-start-text').value=this.value; });
document.getElementById('session-end-native').addEventListener('change',function(){ document.getElementById('session-end-text').value=this.value; });
document.querySelectorAll('.date-pick-btn').forEach(btn=>{
  btn.addEventListener('click',()=>{
    const n=document.getElementById(btn.dataset.target);
    try{ if(n.showPicker)n.showPicker(); else n.click(); }catch(e){ n.click(); }
  });
});
function clearSessionForm(){
  ['session-round-input','session-start-text','session-end-text',
   'session-planned-input','session-completed-input','session-cost-input',
   'session-satisfaction-input','session-note-input'].forEach(id=>{ document.getElementById(id).value=''; });
}
function openSessionAdd(){
  if(courses.length===0){ toast('전체 현황 화면에서 교육과정을 먼저 추가해주세요.','warn'); return; }
  editingSessionId=null;
  document.getElementById('session-modal-title').textContent='세부회차 추가';
  document.getElementById('session-course-select-wrap').style.display='';
  document.getElementById('session-course-display-wrap').style.display='none';
  populateCourseSelect(); clearSessionForm();
  sessionOverlay.classList.add('open');
  setTimeout(()=>document.getElementById('session-round-input').focus(),50);
}
function openSessionEdit(id){
  const s=sessions.find(x=>x.id===id); if(!s)return;
  const c=courses.find(x=>x.id===s.courseId);
  editingSessionId=id;
  document.getElementById('session-modal-title').textContent='세부회차 수정';
  document.getElementById('session-course-select-wrap').style.display='none';
  document.getElementById('session-course-display-wrap').style.display='';
  document.getElementById('session-course-display').textContent=c?c.name:'(삭제된 교육과정)';
  document.getElementById('session-round-input').value=s.round||'';
  document.getElementById('session-start-text').value=s.startDate||'';
  document.getElementById('session-end-text').value=s.endDate||'';
  document.getElementById('session-planned-input').value=s.plannedCount??'';
  document.getElementById('session-completed-input').value=s.completedCount??'';
  document.getElementById('session-cost-input').value=s.cost??'';
  document.getElementById('session-satisfaction-input').value=s.satisfaction??'';
  document.getElementById('session-note-input').value=s.note||'';
  sessionOverlay.classList.add('open');
}
document.getElementById('btn-add-session').addEventListener('click',openSessionAdd);
sessionOverlay.querySelectorAll('[data-close="session"]').forEach(el=>el.addEventListener('click',closeSessionModal));
sessionOverlay.addEventListener('click',(e)=>{ if(e.target===sessionOverlay)closeSessionModal(); });

document.getElementById('session-form').addEventListener('submit',async(e)=>{
  e.preventDefault();
  const round=document.getElementById('session-round-input').value.trim();
  const startDate=document.getElementById('session-start-text').value.trim();
  const endDate=document.getElementById('session-end-text').value.trim();
  const plannedVal=document.getElementById('session-planned-input').value;
  const completedVal=document.getElementById('session-completed-input').value;
  const costVal=document.getElementById('session-cost-input').value;
  const satisfactionVal=document.getElementById('session-satisfaction-input').value;
  const noteVal=document.getElementById('session-note-input').value.trim();
  if(!round){ toast('세부회차를 입력해주세요.','warn'); return; }
  if(!validDate(startDate)||!validDate(endDate)){ toast('날짜는 2026-06-28 형식으로 입력해주세요.','warn'); return; }
  if(startDate&&endDate&&startDate>endDate){ toast('종료일이 시작일보다 빠를 수 없습니다.','warn'); return; }
  const data={round,startDate:startDate||'',endDate:endDate||'',
    plannedCount:plannedVal===''?null:Number(plannedVal),
    completedCount:completedVal===''?null:Number(completedVal),
    cost:costVal===''?null:Number(costVal),
    satisfaction:satisfactionVal===''?null:Number(satisfactionVal),
    note:noteVal||''};
  if(editingSessionId===null){
    const courseId=document.getElementById('session-course-select').value;
    if(!courseId){ toast('교육명을 선택해주세요.','warn'); return; }
    sessions.push(Object.assign({id:uid(),courseId},data));
    await saveS(); toast('세부회차가 추가되었습니다.','success');
  }else{
    const s=sessions.find(x=>x.id===editingSessionId); if(!s)return;
    Object.assign(s,data); await saveS(); toast('수정되었습니다.','success');
  }
  closeSessionModal(); renderAll();
});

function deleteSession(id){
  const s=sessions.find(x=>x.id===id); if(!s)return;
  if(!confirm(`'${s.round||''}' 세부회차를 삭제하시겠습니까?`))return;
  sessions=sessions.filter(x=>x.id!==id); saveS(); renderAll();
}

/* ---- CSV ---- */
function toCSV(v){ const s=(v==null)?'':String(v); return /[",\n]/.test(s)?'"'+s.replace(/"/g,'""')+'"':s; }
function downloadCSV(filename,header,rows){
  const lines=[header,...rows].map(r=>r.map(toCSV).join(',')).join('\r\n');
  const blob=new Blob(['\uFEFF'+lines],{type:'text/csv;charset=utf-8;'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a'); a.href=url; a.download=filename;
  document.body.appendChild(a); a.click(); document.body.removeChild(a); URL.revokeObjectURL(url);
}
document.getElementById('btn-export-screen1').addEventListener('click',()=>{
  const header=['역량구분','교육명','운영회차','수료인원','예산','실제집행액','집행률','비고'];
  const rows=[];
  CATEGORIES.forEach(cat=>{
    courses.filter(c=>c.category===cat).forEach(c=>{
      const a=agg(c);
      rows.push([cat,c.name,a.opCount,a.opCount>0?a.completedSum:'-',
        hasNum(c.budget)?Math.round(c.budget):'-',
        a.opCount>0?Math.round(a.actualSpent):'-',
        a.rate===null?'-':a.rate.toFixed(2)+'%',c.note||'']);
    });
  });
  downloadCSV('전체현황_'+today()+'.csv',header,rows);
});
document.getElementById('btn-export-screen2').addEventListener('click',()=>{
  const header=['역량구분','교육명','세부회차','교육시작일','교육종료일','계획인원','이수인원','이수율','소요비용','만족도','비고'];
  const sorted=sessions.slice().sort((a,b)=>{
    const ca=courses.find(c=>c.id===a.courseId); const cb=courses.find(c=>c.id===b.courseId);
    return (ca?ca.name:'').localeCompare(cb?cb.name:'') || (a.startDate||'').localeCompare(b.startDate||'');
  });
  const rows=sorted.map(s=>{
    const c=courses.find(x=>x.id===s.courseId);
    const rate=(hasNum(s.plannedCount)&&Number(s.plannedCount)>0&&hasNum(s.completedCount))
      ?(Number(s.completedCount)/Number(s.plannedCount)*100).toFixed(2)+'%':'-';
    return[c?c.category:'',c?c.name:'(삭제됨)',s.round||'',s.startDate||'',s.endDate||'',
      hasNum(s.plannedCount)?s.plannedCount:'-',hasNum(s.completedCount)?s.completedCount:'-',rate,
      hasNum(s.cost)?Math.round(s.cost):'-',hasNum(s.satisfaction)?Number(s.satisfaction).toFixed(2):'-',s.note||''];
  });
  downloadCSV('세부교육운영현황_'+today()+'.csv',header,rows);
});

/* ---- Password ---- */
const PW='hrd1313';
const pwOverlay=document.getElementById('pw-overlay');
const pwInput=document.getElementById('pw-input');
const pwError=document.getElementById('pw-error');
function tryPw(){
  if(pwInput.value===PW){ pwOverlay.style.display='none'; loadData(); }
  else{ pwError.textContent='비밀번호가 틀렸습니다.'; pwInput.value=''; pwInput.focus(); setTimeout(()=>pwError.textContent='',2500); }
}
document.getElementById('pw-submit').addEventListener('click',tryPw);
pwInput.addEventListener('keydown',(e)=>{ if(e.key==='Enter')tryPw(); });
document.getElementById('pw-toggle').addEventListener('click',()=>{ pwInput.type=pwInput.type==='password'?'text':'password'; });

/* ---- Keyboard ---- */
document.addEventListener('keydown',(e)=>{ if(e.key==='Escape'){ closeCourseModal(); closeSessionModal(); } });
</script>
</body>
</html>
