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
