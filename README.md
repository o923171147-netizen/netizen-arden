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
       🖥️ 大螢幕專
