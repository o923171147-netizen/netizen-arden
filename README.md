<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>永續物流與綠色科技 - 專題報告與數據分析儀表板</title>
  
  <meta name="description" content="本平台提供綠色物流與永續科技專題報告之簡報下載、線上投影片簡報器、關鍵綠色物流數據趨勢圖表，並整合多個 YouTube 綠色物流視評與綠色產業科技影片，符合最高級 UI/UX Pro Max 介面設計規範。">
  <meta name="keywords" content="永續物流, 綠色科技, 碳中和, 供應鏈優化, ESG 儀表板, 智慧物流">
  
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    /* ==========================================================================
       全域樣式與磨砂玻璃 (Glassmorphism) 核心設定
       ========================================================================== */
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Outfit:wght@300;400;500;600;700;800&display=swap');

    :root {
      --bg-base: #080b11;
      --bg-sidebar: #0d121f;
      --bg-surface: rgba(17, 24, 39, 0.6);
      --bg-surface-hover: rgba(31, 41, 55, 0.8);
      --border-color: rgba(255, 255, 255, 0.08);
      
      --text-primary: #f3f4f6;
      --text-secondary: #9ca3af;
      --text-muted: #6b7280;
      
      --color-primary: #10b981;      /* 翡翠綠 (永續環保主調) */
      --color-primary-rgb: 16, 185, 129;
      --color-primary-hover: #059669;
      --color-accent: #06b6d4;       /* 青色 (科技感) */
      --color-accent-rgb: 6, 182, 212;
      
      --sidebar-width: 280px;
      --header-height: 70px;
      
      --font-title: 'Outfit', 'Inter', system-ui, -apple-system, sans-serif;
      --font-body: 'Inter', system-ui, -apple-system, sans-serif;
      
      --shadow-sm: 0 4px 6px -1px rgba(0, 0, 0, 0.2), 0 2px 4px -1px rgba(0, 0, 0, 0.1);
      --shadow-md: 0 10px 15px -3px rgba(0, 0, 0, 0.3), 0 4px 6px -2px rgba(0, 0, 0, 0.15);
      --shadow-lg: 0 20px 25px -5px rgba(0, 0, 0, 0.4), 0 10px 10px -5px rgba(0, 0, 0, 0.2);
      --shadow-glow: 0 0 20px rgba(16, 185, 129, 0.25);
      
      --radius-sm: 8px;
      --radius-md: 12px;
      --radius-lg: 16px;
      --radius-full: 9999px;
      
      --transition-fast: 0.15s ease;
      --transition-normal: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    /* 基礎重設與滾動條樣式 */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      scrollbar-width: thin;
      scrollbar-color: rgba(255, 255, 255, 0.15) transparent;
    }

    body {
      background-color: var(--bg-base);
      color: var(--text-primary);
      font-family: var(--font-body);
      font-size: 15px;
      line-height: 1.6;
      min-height: 100vh;
      overflow-x: hidden;
      display: flex;
    }

    a {
      color: inherit;
      text-decoration: none;
    }

    button {
      background: none;
      border: none;
      cursor: pointer;
      font-family: inherit;
      outline: none;
    }

    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: transparent;
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(255, 255, 255, 0.15);
      border-radius: var(--radius-full);
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(255, 255, 255, 0.3);
    }

    h1, h2, h3, h4, h5, h6 {
      font-family: var(--font-title);
      font-weight: 600;
      letter-spacing: -0.02em;
    }

    /* 主容器版面配置 */
    .app-container {
      display: flex;
      width: 100vw;
      min-height: 100vh;
    }

    /* ==========================================================================
       🖥️ 大螢幕專用：固定式側邊導覽列 (寬度 280px)
       ========================================================================== */
    .sidebar {
      width: var(--sidebar-width);
      background-color: var(--bg-sidebar);
      border-right: 1px solid var(--border-color);
      padding: 24px;
      display: flex;
      flex-direction: column;
      position: fixed;
      top: 0;
      bottom: 0;
      left: 0;
      z-index: 100;
      transition: var(--transition-normal);
    }

    .sidebar-brand {
      display: flex;
      align-items: center;
      gap: 12px;
      padding-bottom: 32px;
      border-bottom: 1px solid var(--border-color);
    }

    .brand-logo {
      width: 36px;
      height: 36px;
      background: linear-gradient(135deg, var(--color-primary), var(--color-accent));
      border-radius: var(--radius-md);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #000;
      font-weight: bold;
      box-shadow: var(--shadow-glow);
    }

    .brand-name {
      font-size: 1.15rem;
      font-weight: 800;
      background: linear-gradient(to right, #ffffff, #9ca3af);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .sidebar-menu {
      list-style: none;
      margin-top: 32px;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .menu-item a {
      display: flex;
      align-items: center;
      gap: 14px;
      padding: 12px 16px;
      border-radius: var(--radius-md);
      color: var(--text-secondary);
      font-weight: 500;
      transition: var(--transition-fast);
    }

    .menu-item a:hover {
      background-color: rgba(255, 255, 255, 0.05);
      color: var(--text-primary);
    }

    .menu-item.active a {
      background-color: rgba(var(--color-primary-rgb), 0.15);
      color: var(--color-primary);
      border-left: 3px solid var(--color-primary);
    }

    .menu-item svg {
      width: 20px;
      height: 20px;
      stroke: currentColor;
    }

    .sidebar-footer {
      margin-top: auto;
      padding-top: 24px;
      border-top: 1px solid var(--border-color);
    }

    .profile-card {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    /* 核心修正：主內容區區塊（預留側邊欄空間，並嚴格貼齊左右側） */
    .main-content {
      flex: 1;
      margin-left: var(--sidebar-width);
      padding: 40px 40px; /* 上下 40px，左右 40px 嚴格對齊 */
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      gap: 40px;
      transition: var(--transition-normal);
    }

    /* 頂部全域大標題樣式（網民-arden） */
    .main-title-bar {
      width: 100%;
      padding-bottom: 20px;
      border-bottom: 1px solid var(--border-color);
      margin-bottom: -10px;
    }

    .main-title-bar h1 {
      font-size: 2.2rem;
      font-weight: 800;
      color: var(--text-primary);
      letter-spacing: -0.03em;
    }

    /* ==========================================================================
       📱 行動端專用：頂部導覽橫條 (大螢幕下預設隱藏)
       ========================================================================== */
    .top-header {
      display: none;
      align-items: center;
      justify-content: space-between;
      height: var(--header-height);
      background-color: rgba(8, 11, 17, 0.8);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border-color);
      padding: 0 24px;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      z-index: 90;
    }

    .menu-toggle-btn {
      color: var(--text-primary);
    }

    .menu-toggle-btn svg {
      width: 24px;
      height: 24px;
      stroke: currentColor;
    }

    /* 多分頁卡片切換動畫 */
    section {
      display: none;
      animation: fadeIn 0.4s ease-out forwards;
    }

    section.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* 頁面子標題樣式 */
    .section-header {
      margin-bottom: 28px;
    }

    .section-category {
      font-size: 0.85rem;
      text-transform: uppercase;
      color: var(--color-primary);
      letter-spacing: 0.1em;
      font-weight: 700;
      margin-bottom: 6px;
    }

    .section-title {
      font-size: 1.85rem;
      font-weight: 700;
      color: var(--text-primary);
    }

    .section-desc {
      color: var(--text-secondary);
      font-size: 1rem;
      margin-top: 4px;
      max-width: 800px;
    }

    /* 儀表板關鍵指標卡片區 */
    .metrics-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 24px;
      margin-bottom: 32px;
    }

    .metric-card {
      background-color: var(--bg-surface);
      border: 1px solid var(--border-color);
      border-radius: var(--radius-lg);
      padding: 24px;
      position: relative;
      overflow: hidden;
      transition: var(--transition-fast);
    }

    .metric-card:hover {
      transform: translateY(-2px);
      border-color: rgba(var(--color-primary-rgb), 0.3);
    }

    .metric-icon {
      width: 40px;
      height: 40px;
      border-radius: var(--radius-md);
      background-color: rgba(var(--color-primary-rgb), 0.1);
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--color-primary);
      margin-bottom: 16px;
    }

    .metric-card.accent .metric-icon {
      background-color: rgba(6, 182, 212, 0.1);
      color: var(--color-accent);
    }

    .metric-icon svg {
      width: 20px;
      height: 20px;
      stroke: currentColor;
    }

    .metric-label {
      font-size: 0.85rem;
      color: var(--text-secondary);
      font-weight: 500;
    }

    .metric-value {
      font-size: 1.75rem;
      font-weight: 700;
      margin: 6px 0;
      font-family: var(--font-title);
    }

    .metric-change {
      font-size: 0.8rem;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .metric-change.positive {
      color: #10b981;
    }

    /* 📊 圖表雙欄網格配置 */
    .dashboard-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 28px;
    }

    .chart-card {
      background-color: var(--bg-surface);
      border: 1px solid var(--border-color);
      border-radius: var(--radius-lg);
      padding: 28px;
      min-height: 380px;
      display: flex;
      flex-direction: column;
    }

    .chart-card h3 {
      font-size: 1.1rem;
      margin-bottom: 4px;
    }

    .chart-subtitle {
      color: var(--text-secondary);
      font-size: 0.85rem;
      margin-bottom: 24px;
    }

    .chart-container {
      position: relative;
      flex: 1;
      width: 100%;
    }

    /* 🧑‍🏫 線上簡報播放器結構 */
    .slides-viewer-card {
      background-color: var(--bg-surface);
      border: 1px solid var(--border-color);
      border-radius: var(--radius-lg);
      padding: 32px;
      display: flex;
      flex-direction: column;
      gap: 24px;
      align-items: center;
    }

    .slide-viewport {
      position: relative;
      width: 100%;
      max-width: 900px;
      aspect-ratio: 16 / 9;
      background-color: #000;
      border-radius: var(--radius-md);
      overflow: hidden;
      border: 1px solid var(--border-color);
      box-shadow: var(--shadow-lg);
    }

    .slide-viewport img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      transition: opacity 0.3s ease;
    }

    .slide-loading {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(8, 11, 17, 0.9);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 16px;
      z-index: 10;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s ease;
    }

    .slide-loading.active {
      opacity: 1;
      pointer-events: auto;
    }

    .spinner {
      width: 40px;
      height: 40px;
      border: 3px solid rgba(16, 185, 129, 0.1);
      border-top-color: var(--color-primary);
      border-radius: var(--radius-full);
      animation: spin 1s linear infinite;
    }

    @keyframes spin { to { transform: rotate(360deg); } }

    .slide-controls {
      display: flex;
      align-items: center;
      gap: 20px;
      width: 100%;
      max-width: 900px;
      justify-content: space-between;
    }

    .slide-nav-btn {
      background-color: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--border-color);
      width: 44px;
      height: 44px;
      border-radius: var(--radius-full);
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--text-primary);
      transition: var(--transition-fast);
    }

    .slide-nav-btn:hover:not(:disabled) {
      background-color: rgba(var(--color-primary-rgb), 0.15);
      color: var(--color-primary);
      border-color: rgba(var(--color-primary-rgb), 0.3);
    }

    .slide-nav-btn:disabled {
      opacity: 0.3;
      cursor: not-allowed;
    }

    .slide-nav-btn svg {
      width: 20px;
      height: 20px;
      stroke: currentColor;
    }

    .slide-progress-container {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      max-width: 400px;
    }

    .slide-progress-bar {
      width: 100%;
      height: 6px;
      background-color: rgba(255, 255, 255, 0.1);
      border-radius: var(--radius-full);
      overflow: hidden;
    }

    .slide-progress-fill {
      height: 100%;
      width: 3.5%;
      background: linear-gradient(to right, var(--color-primary), var(--color-accent));
      border-radius: var(--radius-full);
      transition: width 0.3s ease;
    }

    .slide-index-label {
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--text-secondary);
    }

    .slide-actions { display: flex; gap: 12px; }

    /* 下載卡片網格 */
    .downloads-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 24px;
      margin-top: 16px;
    }

    .download-card {
      background-color: var(--bg-surface);
      border: 1px solid var(--border-color);
      border-radius: var(--radius-lg);
      padding: 24px;
      display: flex;
      flex-direction: column;
      gap: 16px;
      transition: var(--transition-fast);
    }

    .download-card:hover {
      border-color: rgba(var(--color-primary-rgb), 0.3);
      transform: translateY(-2px);
    }

    .download-info { display: flex; align-items: center; gap: 16px; }

    .file-icon {
      width: 48px;
      height: 48px;
      border-radius: var(--radius-md);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
    }

    .file-icon.pptx { background-color: rgba(239, 68, 68, 0.1); color: #ef4444; }
    .file-icon.pdf { background-color: rgba(245, 158, 11, 0.1); color: #f59e0b; }
    .file-icon svg { width: 24px; height: 24px; stroke: currentColor; }

    .file-details h4 { font-size: 1rem; font-weight: 600; margin-bottom: 4px; }
    .file-details p { font-size: 0.8rem; color: var(--text-muted); }

    .btn-download {
      width: 100%;
      background-color: rgba(16, 185, 129, 0.1);
      color: var(--color-primary);
      border: 1px solid rgba(16, 185, 129, 0.2);
      padding: 12px;
      border-radius: var(--radius-md);
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: var(--transition-fast);
    }

    .btn-download:hover {
      background-color: var(--color-primary);
      color: #000;
      border-color: var(--color-primary);
    }

    /* 影音卡片版面 */
    .videos-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 28px;
    }

    .video-card {
      background-color: var(--bg-surface);
      border: 1px solid var(--border-color);
      border-radius: var(--radius-lg);
      overflow: hidden;
      display: flex;
      flex-direction: column;
      transition: var(--transition-fast);
    }

    .video-card:hover {
      border-color: rgba(6, 182, 212, 0.3);
      transform: translateY(-2px);
    }

    .video-container {
      width: 100%;
      aspect-ratio: 16 / 9;
      background-color: #000;
      border-bottom: 1px solid var(--border-color);
    }

    .video-container iframe { width: 100%; height: 100%; border: none; }
    .video-body { padding: 20px; display: flex; flex-direction: column; gap: 10px; }
    .video-tag {
      align-self: flex-start;
      background-color: rgba(6, 182, 212, 0.1);
      color: var(--color-accent);
      font-size: 0.75rem;
      font-weight: 700;
      padding: 4px 10px;
      border-radius: var(--radius-full);
    }

    .video-title { font-size: 1rem; font-weight: 600; line-height: 1.4; }
    .video-desc { font-size: 0.85rem; color: var(--text-secondary); }

    /* 頁尾資訊 */
    .footer-text {
      text-align: center;
      color: var(--text-muted);
      font-size: 0.8rem;
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px solid var(--border-color);
    }

    /* 行動端模糊黑幕遮罩 */
    .sidebar-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      z-index: 95;
      backdrop-filter: blur(4px);
    }

    .sidebar-overlay.active { display: block; }

    /* ==========================================================================
       ⚡ RWD 媒體查詢樣式（電腦、平板、手機自動縮放控盤核心）
       ========================================================================== */
    
    /* 1. 中螢幕（平板電腦級別：1023px 以下） */
    @media (max-width: 1023px) {
      .sidebar {
        transform: translateX(-100%); /* 收起側邊欄 */
      }
      
      .sidebar.mobile-active {
        transform: translateX(0);    /* 按鈕點擊時展開側邊欄 */
      }
      
      .main-content {
        margin-left: 0;               /* 填補左側排版間距 */
        padding: 110px 24px 40px;     /* 為頂部漢堡列預留高度空間，左右保持對齊縮排 */
      }
      
      .top-header {
        display: flex;                /* 喚醒手機版頂部導覽列 */
      }
      
      .dashboard-grid {
        grid-template-columns: 1fr;   /* 降維為單一直立欄位，防止擠壓錯位 */
      }
    }

    /* 2. 小螢幕（智慧型手機級別：480px 以下） */
    @media (max-width: 480px) {
      .main-title-bar h1 {
        font-size: 1.8rem;            /* 縮小最上方主標題 */
      }

      .section-title {
        font-size: 1.5rem;            /* 縮小分頁內部標題，防長字破音 */
      }
      
      .metrics-row {
        grid-template-columns: 1fr;   /* 指標字卡降為純單欄直式排版 */
      }
      
      .slide-controls {
        flex-wrap: wrap;              /* 簡報控制按鈕在緊湊空間自動重組 */
        justify-content: center;
        gap: 12px;
      }
      
      .slide-progress-container {
        order: -1;                    /* 進度條排在第一行 */
        min-width: 100%;
      }
    }
  </style>
</head>
<body>

  <div class="app-container">
    <div class="sidebar-overlay" id="sidebarOverlay"></div>

    <aside class="sidebar" id="sidebar">
      <div class="sidebar-brand">
        <div class="brand-logo">G</div>
        <span class="brand-name">綠羅技</span>
      </div>
      
      <nav class="sidebar-nav">
        <ul class="sidebar-menu">
          <li class="menu-item active" data-target="dashboard-section">
            <a href="#">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="7" height="9" x="3" y="3" rx="1"/><rect width="7" height="5" x="14" y="3" rx="1"/><rect width="7" height="9" x="14" y="10" rx="1"/><rect width="7" height="5" x="3" y="16" rx="1"/></svg>
              <span>趨勢儀表板</span>
            </a>
          </li>
          <li class="menu-item" data-target="ppt-section">
            <a href="#">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h20"/><path d="M21 3v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V3"/><path d="m7 21 5-5 5 5"/></svg>
              <span>專題投影片</span>
            </a>
          </li>
          <li class="menu-item" data-target="videos-section">
            <a href="#">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2.5 17a24.12 24.12 0 0 1 0-10 2 2 0 0 1 1.4-1.4 49.56 49.56 0 0 1 16.2 0A2 2 0 0 1 21.5 7a24.12 24.12 0 0 1 0 10 2 2 0 0 1-1.4 1.4 49.55 49.55 0 0 1-16.2 0A2 2 0 0 1 2.5 17"/><path d="m10 15 5-3-5-3z"/></svg>
              <span>精選影音影評</span>
            </a>
          </li>
          <li class="menu-item" data-target="downloads-section">
            <a href="#">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 14.899A7 7 0 1 1 15.71 8h1.79a4.5 4.5 0 0 1 2.5 8.242"/><path d="M12 12v9"/><path d="m8 17 4 4 4-4"/></svg>
              <span>相關附件下載</span>
            </a>
          </li>
        </ul>
      </nav>

      <div class="sidebar-footer">
        <div class="profile-card">
          <div class="avatar">ESG</div>
          <div class="profile-info">
            <h4>綠色物流專案小組</h4>
            <p>永續科技研究員</p>
          </div>
        </div>
      </div>
    </aside>

    <header class="top-header">
      <button class="menu-toggle-btn" id="menuToggle" aria-label="切換選單">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
      </button>
      <div class="brand-name">GreenLogiTech</div>
      <div class="avatar" style="width: 32px; height: 32px; font-size: 0.75rem;">ESG</div>
    </header>

    <main class="main-content">
      
      <div class="main-title-bar">
        <h1>網民-arden</h1>
      </div>

      <section id="dashboard-section" class="active">
        <div class="section-header">
          <span class="section-category">Sustainable Technology & ESG Overview</span>
          <h2 class="section-title">永續物流與綠色科技</h2>
          <p class="section-desc">監控全球與企業內部的綠色物流指標。以下為運用人工智慧路徑優化、電動車隊與物聯網倉儲所帶來的碳排放降幅與趨勢分析。</p>
        </div>
        
        <div class="metrics-row">
          <div class="metric-card">
            <div class="metric-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 20A7 7 0 0 1 9.8 6.1C15.5 5 17 4.48 19 2c1 2 2 3.58 0 8a7 7 0 0 1-8 10Z"/><path d="M19 2c-2.26 4.33-5.27 7.14-8 10"/></svg>
            </div>
            <div class="metric-label">累計碳排放減量</div>
            <div class="metric-value">124,850 噸</div>
            <div class="metric-change positive">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="width: 14px; height: 14px;"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>
              <span>+14.8% 比上季</span>
            </div>
          </div>
          
          <div class="metric-card accent">
            <div class="metric-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 18V6a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2v11a1 1 0 0 0 1 1h2"/><path d="M19 18h2a1 1 0 0 0 1-1v-5.14a1 1 0 0 0-.294-.707l-2.856-2.856A1 1 0 0 0 18.14 8.3H14"/><circle cx="7.5" cy="18.5" r="2.5"/><circle cx="17.5" cy="18.5" r="2.5"/></svg>
            </div>
            <div class="metric-label">新能源電動車佔比</div>
            <div class="metric-value">42.5 %</div>
            <div class="metric-change positive">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="width: 14px; height: 14px;"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>
              <span>+8.2% 比去年</span>
            </div>
          </div>

          <div class="metric-card">
            <div class="metric-icon" style="background-color: rgba(59, 130, 246, 0.1); color: #3b82f6;">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"/></svg>
            </div>
            <div class="metric-label">智慧倉儲綠電自給率</div>
            <div class="metric-value">68.0 %</div>
            <div class="metric-change positive">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="width: 14px; height: 14px;"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>
              <span>+12.4% 比上月</span>
            </div>
          </div>
        </div>

        <div class="dashboard-grid">
          <div class="chart-card">
            <h3>綠色物流市場產值趨勢</h3>
            <span class="chart-subtitle">全球綠色供應鏈市場的穩定擴張與潛在投資預估 (2020 - 2026)</span>
            <div class="chart-container">
              <canvas id="marketTrendChart"></canvas>
            </div>
          </div>
          
          <div class="chart-card">
            <h3>減碳關鍵技術貢獻佔比</h3>
            <span class="chart-subtitle">依據各項綠色物流核心技術模組對年度碳排放降低的貢獻比重</span>
            <div class="chart-container">
              <canvas id="reductionContributionChart"></canvas>
            </div>
          </div>
        </div>
      </section>

      <section id="ppt-section">
        <div class="section-header">
          <span class="section-category">Interactive Slide Deck</span>
          <h2 class="section-title">永續物流與綠色科技投影片</h2>
          <p class="section-desc">線上查閱綠色物流簡報，瞭解最新永續商業模式與科技運用策略。支援鍵盤左右方向鍵翻頁。</p>
        </div>

        <div class="slides-viewer-card">
          <div class="slide-viewport">
            <div class="slide-loading" id="slideLoading">
              <div class="spinner"></div>
              <span>載入簡報投影片中...</span>
            </div>
            <img src="slides_images/slide1.png" alt="投影片" id="slideImage">
          </div>

          <div class="slide-controls">
            <button class="slide-nav-btn" id="prevSlide" aria-label="上一頁" disabled>
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m15 18-6-6 6-6"/></svg>
            </button>
            
            <div class="slide-progress-container">
              <div class="slide-progress-bar">
                <div class="slide-progress-fill" id="slideProgressFill"></div>
              </div>
              <span class="slide-index-label" id="slideIndexLabel">第 1 / 28 頁</span>
            </div>

            <button class="slide-nav-btn" id="nextSlide" aria-label="下一頁">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m9 18 6-6-6-6"/></svg>
            </button>
          </div>

          <div class="slide-actions">
            <a href="永續物流與綠色科技.pptx" download class="btn-secondary">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z"/><path d="M14 2v4a2 2 0 0 0 2 2h4"/><path d="M10 9H8"/><path d="M16 13H8"/><path d="M16 17H8"/></svg>
              <span>下載 PPTX 簡報檔 (61.8 MB)</span>
            </a>
            <a href="永續物流與綠色技術.pptx.pdf" target="_blank" class="btn-secondary">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z"/><path d="M14 2v4a2 2 0 0 0 2 2h4"/></svg>
              <span>線上預覽/下載 PDF 檔 (354 KB)</span>
            </a>
          </div>
        </div>
      </section>

      <section id="videos-section">
        <div class="section-header">
          <span class="section-category">Multimedia Library</span>
          <h2 class="section-title">精選影音與產業視評</h2>
          <p class="section-desc">精選 YouTube 上關於綠色科技、永續物流實務與全球供應鏈低碳轉型的影音資料，加深對綠色生態圈的理解。</p>
        </div>

        <div class="videos-grid">
          <div class="video-card">
            <div class="video-container">
              <iframe src="https://www.youtube.com/embed/iOg_y3-FcRY" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
            </div>
            <div class="video-body">
              <span class="video-tag">永續物流思維</span>
              <h3 class="video-title">綠色物流未來趨勢：碳中和與減碳關鍵路徑</h3>
              <p class="video-desc">深入分析現代供應鏈在面臨歐盟 CBAM 等碳關稅壓力下，如何透過全面綠色化轉型提升核心競爭力。</p>
            </div>
          </div>
          
          <div class="video-card">
            <div class="video-container">
              <iframe src="https://www.youtube.com/embed/biGSdycqWZ8" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
            </div>
            <div class="video-body">
              <span class="video-tag">智慧科技應用</span>
              <h3 class="video-title">AI 技術在物流行業的低碳革新與配送路徑優化</h3>
              <p class="video-desc">介紹如何以人工智慧、機器學習演算法，在每日數百萬件包裹的配送任務中，精準削減行駛里程與耗油量。</p>
            </div>
          </div>

          <div class="video-card">
            <div class="video-container">
              <iframe src="https://www.youtube.com/embed/YsmFThCkhWc" title="YouTube video player" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
            </div>
            <div class="video-body">
              <span class="video-tag">產業實務觀點</span>
              <h3 class="video-title">綠色智慧工廠與自動化零排放倉儲管理實戰</h3>
              <p class="video-desc">實地參訪當前全球先進倉儲如何結合屋頂太陽能板、儲能系統及 AGV 無人搬運車，實現運營階段近乎零排放目標。</p>
            </div>
          </div>
        </div>
      </section>

      <section id="downloads-section">
        <div class="section-header">
          <span class="section-category">Resource Center</span>
          <h2 class="section-title">專案附件與參考資料下載</h2>
          <p class="section-desc">在此處可直接獲取專題相關的原始報告簡報檔與 PDF 彙整文件，便於在其他平台上簡報或進行學術/商務使用。</p>
        </div>

        <div class="downloads-grid">
          <div class="download-card">
            <div class="download-info">
              <div class="file-icon pptx">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 2H6a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h9"/><path d="M20 8v11a2 2 0 0 1-2 2h-2"/><path d="M14 2v4a2 2 0 0 0 2 2h4"/><path d="M10 9H8"/><path d="M10 13H8"/></svg>
              </div>
              <div class="file-details">
                <h4>永續物流與綠色科技.pptx</h4>
                <p>PowerPoint 簡報原檔 • 61.8 MB</p>
              </div>
            </div>
            <a href="永續物流與綠色科技.pptx" download class="btn-download">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" x2="12" y1="15" y2="3"/></svg>
              <span>下載 PPTX 檔案</span>
            </a>
          </div>

          <div class="download-card">
            <div class="download-info">
              <div class="file-icon pdf">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z"/><path d="M14 2v4a2 2 0 0 0 2 2h4"/><path d="M10 9H8"/><path d="M16 13H8"/><path d="M16 17H8"/></svg>
              </div>
              <div class="file-details">
                <h4>永續物流與綠色技術.pptx.pdf</h4>
                <p>PDF 文件格式 • 353 KB</p>
              </div>
            </div>
            <a href="永續物流與綠色技術.pptx.pdf" download class="btn-download">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" x2="12" y1="15" y2="3"/></svg>
              <span>下載 PDF 檔案</span>
            </a>
          </div>
        </div>
      </section>

      <footer class="footer-text">
        <p>© 2026 GreenLogiTech 永續物流與綠色技術專案研究小組. All Rights Reserved.</p>
        <p style="margin-top: 4px; color: var(--text-muted);">本平台由 UI/UX Pro Max 高級設計引導實現</p>
      </footer>

    </main>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      // 1. 手機漢堡選單滑出切換功能
      const sidebar = document.getElementById('sidebar');
      const menuToggle = document.getElementById('menuToggle');
      const sidebarOverlay = document.getElementById('sidebarOverlay');
      
      if (menuToggle && sidebar && sidebarOverlay) {
        const toggleSidebar = () => {
          sidebar.classList.toggle('mobile-active');
          sidebarOverlay.classList.toggle('active');
        };
        
        menuToggle.addEventListener('click', toggleSidebar);
        sidebarOverlay.addEventListener('click', toggleSidebar);
        
        // 切換按鈕點擊後，自動收起手機版側欄選單
        const menuLinks = document.querySelectorAll('.sidebar-menu a');
        menuLinks.forEach(link => {
          link.addEventListener('click', () => {
            if (sidebar.classList.contains('mobile-active')) {
              toggleSidebar();
            }
          });
        });
      }

      // 2. 側邊欄切換主頁面分頁結構
      const menuItems = document.querySelectorAll('.menu-item');
      const sections = document.querySelectorAll('section');
      
      menuItems.forEach(item => {
        item.addEventListener('click', (e) => {
          e.preventDefault();
          
          menuItems.forEach(mi => mi.classList.remove('active'));
          item.classList.add('active');
          sections.forEach(sec => sec.classList.remove('active'));
          
          const targetId = item.getAttribute('data-target');
          const targetSection = document.getElementById(targetId);
          if (targetSection) {
            targetSection.classList.add('active');
            document.querySelector('.main-content').scrollTop = 0;
          }
        });
      });

      // 3. 投影片播放器核心邏輯與圖片緩存預載
      const totalSlides = 28;
      let currentSlide = 1;
      
      const slideImage = document.getElementById('slideImage');
      const prevBtn = document.getElementById('prevSlide');
      const nextBtn = document.getElementById('nextSlide');
      const progressFill = document.getElementById('slideProgressFill');
      const indexLabel = document.getElementById('slideIndexLabel');
      const slideLoading = document.getElementById('slideLoading');
      
      const getSlideSrc = (index) => {
        if (index === 11) {
          return `slides_images/slide11.jpeg`; // 兼顧特殊的副檔名格式
        }
        return `slides_images/slide${index}.png`;
      };

      const preloadAdjacentSlides = (currentIndex) => {
        if (currentIndex < totalSlides) {
          const nextImg = new Image();
          nextImg.src = getSlideSrc(currentIndex + 1);
        }
        if (currentIndex > 1) {
          const prevImg = new Image();
          prevImg.src = getSlideSrc(currentIndex - 1);
        }
      };
      
      const updateSlide = (index) => {
        if (index < 1 || index > totalSlides) return;
        currentSlide = index;
        
        slideLoading.classList.add('active');
        
        const tempImg = new Image();
        tempImg.src = getSlideSrc(currentSlide);
        tempImg.onload = () => {
          slideImage.src = tempImg.src;
          slideLoading.classList.remove('active');
          preloadAdjacentSlides(currentSlide);
        };
        
        tempImg.onerror = () => {
          slideLoading.classList.remove('active');
        };
        
        prevBtn.disabled = currentSlide === 1;
        nextBtn.disabled = currentSlide === totalSlides;
        
        const progressPercent = (currentSlide / totalSlides) * 100;
        progressFill.style.width = `${progressPercent}%`;
        indexLabel.textContent = `第 ${currentSlide} / ${totalSlides} 頁`;
      };
      
      if (prevBtn && nextBtn && slideImage) {
        prevBtn.addEventListener('click', () => {
          if (currentSlide > 1) updateSlide(currentSlide - 1);
        });
        
        nextBtn.addEventListener('click', () => {
          if (currentSlide < totalSlides) updateSlide(currentSlide + 1);
        });
        
        // 支援鍵盤左右方向鍵、空白鍵翻頁
        document.addEventListener('keydown', (e) => {
          const pptSection = document.getElementById('ppt-section');
          if (pptSection && pptSection.classList.contains('active')) {
            if (e.key === 'ArrowRight' || e.key === ' ') {
              e.preventDefault();
              if (currentSlide < totalSlides) updateSlide(currentSlide + 1);
            } else if (e.key === 'ArrowLeft') {
              e.preventDefault();
              if (currentSlide > 1) updateSlide(currentSlide - 1);
            }
          }
        });

        updateSlide(1);
      }

      // 4. Chart.js 儀表板圖表宣告
      // 圖表一：產值折線圖
      const trendCtx = document.getElementById('marketTrendChart');
      if (trendCtx) {
        new Chart(trendCtx, {
          type: 'line',
          data: {
            labels: ['2020', '2021', '2022', '2023', '2024', '2025', '2026 (預估)'],
            datasets: [{
              label: '全球綠色物流市場規模 (十億美元)',
              data: [1008, 1075, 1148, 1230, 1318, 1420, 1535],
              borderColor: '#10b981',
              backgroundColor: 'rgba(16, 185, 129, 0.1)',
              fill: true,
              tension: 0.4,
              borderWidth: 3,
              pointBackgroundColor: '#10b981',
              pointBorderColor: '#080b11',
              pointBorderWidth: 2,
              pointRadius: 6,
              pointHoverRadius: 8
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
              legend: { display: false },
              tooltip: {
                padding: 12,
                backgroundColor: '#0d121f',
                titleFont: { family: 'Outfit', size: 14, weight: 'bold' },
                bodyFont: { family: 'Inter', size: 13 },
                borderColor: 'rgba(255, 255, 255, 0.1)',
                borderWidth: 1,
                displayColors: false,
                callbacks: {
                  label: function(context) { return `產值: $${context.raw} 十億美元`; }
                }
              }
            },
            scales: {
              x: {
                grid: { color: 'rgba(255, 255, 255, 0.05)', drawTicks: false },
                ticks: { color: '#9ca3af', font: { family: 'Inter', size: 12 }, padding: 10 }
              },
              y: {
                grid: { color: 'rgba(255, 255, 255, 0.05)', drawTicks: false },
                ticks: { color: '#9ca3af', font: { family: 'Inter', size: 12 }, padding: 10 }
              }
            }
          }
        });
      }

      // 圖表二：技術比重甜甜圈圖
      const contribCtx = document.getElementById('reductionContributionChart');
      if (contribCtx) {
        new Chart(contribCtx, {
          type: 'doughnut',
          data: {
            labels: ['AI 智慧路線優化', '新能源電動車隊', 'IoT 節能倉庫', '包裝循環與回收'],
            datasets: [{
              data: [35, 30, 20, 15],
              backgroundColor: ['#10b981', '#06b6d4', '#3b82f6', '#f59e0b'],
              borderWidth: 0,
              hoverOffset: 4
            }]
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '75%',
            plugins: {
              legend: {
                position: 'bottom',
                labels: {
                  color: '#9ca3af',
                  font: { family: 'Inter', size: 12 },
                  padding: 18,
                  usePointStyle: true,
                  pointStyle: 'circle'
                }
              },
              tooltip: {
                padding: 12,
                backgroundColor: '#0d121f',
                titleFont: { family: 'Outfit', size: 14, weight: 'bold' },
                bodyFont: { family: 'Inter', size: 13 },
                borderColor: 'rgba(255, 255, 255, 0.1)',
                borderWidth: 1,
                callbacks: {
                  label: function(context) { return ` 減碳貢獻比重: ${context.raw}%`; }
                }
              }
            }
          }
        });
      }
    });
  </script>
</body>
</html>
