<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>تطبيق RNF التعليمي</title>
    <!-- Font Awesome للايقونات -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Google Fonts للخط العربي -->
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@300;400;500;700;800&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Tajawal', sans-serif;
            transition: background-color 0.3s ease;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 15px;
        }

        /* الوضع النهاري */
        body.day-mode {
            background: linear-gradient(145deg, #fff9e6, #fff0d4);
        }

        /* الوضع الليلي */
        body.night-mode {
            background: linear-gradient(145deg, #1a2634, #0f1a2f);
        }

        .app-container {
            max-width: 400px;
            width: 100%;
            border-radius: 45px 45px 30px 30px;
            overflow: hidden;
            padding: 20px 18px 25px 18px;
            transition: all 0.3s ease;
        }

        /* الوضع النهاري للتطبيق */
        .day-mode .app-container {
            background: rgba(255, 248, 235, 0.85);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            box-shadow: 0 25px 45px -15px rgba(200, 120, 30, 0.3),
                        0 0 0 1px rgba(255, 215, 150, 0.8) inset;
            border: 1px solid rgba(255, 200, 100, 0.5);
        }

        /* الوضع الليلي للتطبيق */
        .night-mode .app-container {
            background: rgba(10, 20, 30, 0.85);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            box-shadow: 0 25px 45px -15px #00000080,
                        0 0 0 1px rgba(100, 150, 255, 0.3) inset;
            border: 1px solid #2a3f5a;
        }

        /* زر تبديل الوضع الليلي/النهاري */
        .theme-toggle {
            position: absolute;
            top: 15px;
            left: 15px;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: #ffd966;
            border: none;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            z-index: 100;
            font-size: 22px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: 0.2s;
        }

        .night-mode .theme-toggle {
            background: #2c3e50;
            color: #ffd700;
        }

        /* رأس التطبيق */
        .app-header {
            text-align: center;
            margin-bottom: 20px;
            position: relative;
        }

        .secondary-title {
            font-size: 15px;
            font-weight: 400;
            padding: 8px 0;
            border-radius: 50px;
            margin-bottom: 5px;
        }

        .day-mode .secondary-title {
            color: #b65c00;
            background: linear-gradient(90deg, transparent, rgba(255, 165, 0, 0.3), transparent);
        }

        .night-mode .secondary-title {
            color: #ffb347;
            background: linear-gradient(90deg, transparent, rgba(255, 165, 0, 0.2), transparent);
        }

        .supervisor {
            font-size: 13px;
            font-weight: 500;
            display: inline-block;
            padding: 5px 18px;
            border-radius: 30px;
            margin-bottom: 12px;
            backdrop-filter: blur(4px);
        }

        .day-mode .supervisor {
            color: #8b5a2b;
            background: rgba(255, 215, 150, 0.8);
            border: 1px solid #ffb347;
        }

        .night-mode .supervisor {
            color: #ffd966;
            background: rgba(50, 70, 100, 0.8);
            border: 1px solid #5f9ea0;
        }

        .main-title {
            padding: 15px 10px;
            border-radius: 60px;
            font-size: 32px;
            font-weight: 800;
            letter-spacing: 2px;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .day-mode .main-title {
            background: linear-gradient(135deg, #ff8c00, #ffaa33);
            color: white;
            box-shadow: 0 10px 20px -8px #cc7700, 0 0 0 3px #ffe68f inset;
        }

        .night-mode .main-title {
            background: linear-gradient(135deg, #1e3c5f, #2a5f7a);
            color: #e6f3ff;
            box-shadow: 0 10px 20px -8px #0a1a2a, 0 0 0 3px #5f9ea0 inset;
        }

        /* تصغير أسماء رغد - نور - فاطمة */
        .names {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 8px;
            font-size: 14px;
            font-weight: 600;
            padding: 5px 15px;
            border-radius: 40px;
            backdrop-filter: blur(4px);
            width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }

        .day-mode .names {
            color: #b45f06;
            background: rgba(255, 220, 150, 0.7);
            border: 1px solid #ffaa33;
        }

        .night-mode .names {
            color: #a0d0ff;
            background: rgba(30, 50, 70, 0.7);
            border: 1px solid #4682b4;
        }

        .names i {
            margin: 0 2px;
            font-size: 12px;
        }

        .day-mode .names i { color: #ff8c00; }
        .night-mode .names i { color: #87ceeb; }

        /* الشاشات */
        #mainScreen, #sectionScreen {
            transition: all 0.3s ease;
        }

        #sectionScreen {
            display: none;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .section-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 20px;
        }

        .back-button {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: #ffaa33;
            border: none;
            color: white;
            font-size: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .night-mode .back-button {
            background: #4682b4;
        }

        .section-title {
            font-size: 24px;
            font-weight: 700;
            margin: 0;
        }

        .day-mode .section-title { color: #cc5500; }
        .night-mode .section-title { color: #87CEEB; }

        /* أقسام الفقاعات */
        .sections-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            margin: 25px 0 20px;
        }

        .bubble {
            flex: 1 0 calc(33.33% - 12px);
            min-width: 100px;
            padding: 18px 5px;
            border-radius: 35px 35px 35px 12px;
            text-align: center;
            font-weight: 800;
            font-size: 15px;
            box-shadow: 0 10px 20px -5px rgba(0,0,0,0.2), 0 0 0 2px rgba(255,255,255,0.9) inset;
            transition: all 0.25s ease;
            cursor: pointer;
            backdrop-filter: blur(5px);
            position: relative;
            overflow: hidden;
            text-shadow: 0 1px 2px rgba(0,0,0,0.1);
            border: none;
        }

        .bubble::after {
            content: '';
            position: absolute;
            top: -30%;
            left: -30%;
            width: 80px;
            height: 80px;
            background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, transparent 70%);
            opacity: 0.5;
            transform: rotate(25deg);
            pointer-events: none;
        }

        .bubble:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 20px 25px -8px rgba(0,0,0,0.3), 0 0 0 3px white inset;
        }

        /* ألوان الفقاعات */
        .bubble[data-section="intro"] { background: linear-gradient(145deg, #FF5733, #FF2E00); color: white; }
        .bubble[data-section="naissance"] { background: linear-gradient(145deg, #33FF57, #00CC33); color: black; }
        .bubble[data-section="principe"] { background: linear-gradient(145deg, #3357FF, #001BCC); color: white; }
        .bubble[data-section="proscons"] { background: linear-gradient(145deg, #FF33F5, #CC00C1); color: white; }
        .bubble[data-section="importance"] { background: linear-gradient(145deg, #FFD733, #FFAA00); color: black; }
        .bubble[data-section="acteurs-opportunites"] { background: linear-gradient(145deg, #33FFF5, #00CCCC); color: black; }
        .bubble[data-section="conclusion-doaa"] { background: linear-gradient(145deg, #F533FF, #B200CC); color: white; }

        /* معاينة سريعة */
        .preview-area {
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            border-radius: 35px 35px 25px 25px;
            padding: 22px 18px;
            margin: 15px 0 20px;
            max-height: 250px;
            overflow-y: auto;
            transition: all 0.3s ease;
        }

        .day-mode .preview-area {
            background: rgba(255, 250, 240, 0.9);
            border: 1px solid #ffcc99;
            box-shadow: 0 5px 15px rgba(255, 140, 0, 0.1);
        }

        .night-mode .preview-area {
            background: rgba(20, 35, 50, 0.9);
            border: 1px solid #3a6a8a;
            color: #e0e0e0;
        }

        .preview-text {
            font-size: 14px;
            line-height: 1.6;
            opacity: 0.9;
        }

        /* محتوى القسم */
        .full-content {
            font-size: 15px;
            line-height: 1.7;
            text-align: justify;
            max-height: 70vh;
            overflow-y: auto;
            padding: 5px 5px 20px;
            scrollbar-width: thin;
        }

        .day-mode .full-content { color: #2f4156; }
        .night-mode .full-content { color: #e0e0e0; }

        .full-content h2 {
            font-size: 22px;
            font-weight: 700;
            margin-bottom: 15px;
            border-right: 5px solid;
            padding-right: 12px;
        }

        .day-mode .full-content h2 { color: #cc5500; border-right-color: #ffaa33; }
        .night-mode .full-content h2 { color: #87CEEB; border-right-color: #4682b4; }

        .full-content h3 {
            font-size: 18px;
            margin: 15px 0 8px;
            font-weight: 600;
        }

        .day-mode .full-content h3 { color: #b45f06; }
        .night-mode .full-content h3 { color: #b0c4de; }

        /* تلوين الكلمات المهمة في مبدأ العمل */
        .highlight-blue {
            color: #0066cc;
            font-weight: 700;
            background: rgba(0, 102, 204, 0.1);
            padding: 2px 5px;
            border-radius: 8px;
        }

        .night-mode .highlight-blue {
            color: #66b0ff;
            background: rgba(102, 176, 255, 0.15);
        }

        .highlight-green {
            color: #00aa00;
            font-weight: 700;
            background: rgba(0, 170, 0, 0.1);
            padding: 2px 5px;
            border-radius: 8px;
        }

        .night-mode .highlight-green {
            color: #88dd88;
            background: rgba(136, 221, 136, 0.15);
        }

        .highlight-orange {
            color: #ff6600;
            font-weight: 700;
            background: rgba(255, 102, 0, 0.1);
            padding: 2px 5px;
            border-radius: 8px;
        }

        .night-mode .highlight-orange {
            color: #ffaa66;
            background: rgba(255, 170, 102, 0.15);
        }

        .highlight-purple {
            color: #9933cc;
            font-weight: 700;
            background: rgba(153, 51, 204, 0.1);
            padding: 2px 5px;
            border-radius: 8px;
        }

        .night-mode .highlight-purple {
            color: #cc99ff;
            background: rgba(204, 153, 255, 0.15);
        }

        /* تلوين أسماء العلماء والتواريخ */
        .scientist-name {
            color: #c45a32;
            font-weight: 700;
            border-bottom: 2px dotted #c45a32;
        }

        .night-mode .scientist-name {
            color: #ffa07a;
            border-bottom-color: #ffa07a;
        }

        .date-highlight {
            color: #2e5c8a;
            font-weight: 700;
            background: rgba(46, 92, 138, 0.15);
            padding: 0 3px;
            border-radius: 5px;
        }

        .night-mode .date-highlight {
            color: #9fc5e8;
            background: rgba(159, 197, 232, 0.15);
        }

        /* مربعات المراحل */
        .stages-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin: 25px 0 15px;
        }

        .stage-box {
            padding: 20px 15px;
            border-radius: 25px;
            text-align: center;
            font-weight: 600;
            font-size: 16px;
            box-shadow: 0 8px 15px rgba(0,0,0,0.1);
            border: 2px solid white;
            transition: transform 0.2s;
        }

        .stage-box:hover {
            transform: scale(1.02);
        }

        .stage-box .stage-number {
            display: inline-block;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            background: rgba(255,255,255,0.3);
            line-height: 35px;
            margin-bottom: 10px;
            font-size: 18px;
        }

        .stage-box .stage-title {
            font-size: 18px;
            margin-bottom: 8px;
            font-weight: 700;
        }

        .stage-1 {
            background: linear-gradient(135deg, #FFD966, #FFB347);
            color: #5a3e1b;
        }

        .stage-2 {
            background: linear-gradient(135deg, #6EC4B8, #4A9B9B);
            color: #063b3b;
        }

        .stage-3 {
            background: linear-gradient(135deg, #F4A261, #E76F51);
            color: #3a1e0b;
        }

        .night-mode .stage-1 { background: linear-gradient(135deg, #B8860B, #8B6914); color: #FFE4B5; }
        .night-mode .stage-2 { background: linear-gradient(135deg, #2E8B57, #1E6B5E); color: #E0F2E0; }
        .night-mode .stage-3 { background: linear-gradient(135deg, #CD5C5C, #8B4513); color: #FFDAB9; }

        /* مربع الإيجابيات */
        .pros-box {
            background: rgba(76, 175, 80, 0.25);
            border: 2px solid #4CAF50;
            border-radius: 25px;
            padding: 18px 15px;
            margin: 20px 0;
            box-shadow: 0 5px 15px rgba(76, 175, 80, 0.2);
            backdrop-filter: blur(5px);
        }

        .pros-box p {
            margin: 0;
            font-weight: 500;
        }

        .pros-box strong {
            color: #2E7D32;
            font-size: 18px;
            display: block;
            margin-bottom: 10px;
        }

        /* مربع السلبيات */
        .cons-box {
            background: rgba(244, 67, 54, 0.25);
            border: 2px solid #F44336;
            border-radius: 25px;
            padding: 18px 15px;
            margin: 20px 0;
            box-shadow: 0 5px 15px rgba(244, 67, 54, 0.2);
            backdrop-filter: blur(5px);
        }

        .cons-box p {
            margin: 0;
            font-weight: 500;
        }

        .cons-box strong {
            color: #C62828;
            font-size: 18px;
            display: block;
            margin-bottom: 10px;
        }

        /* الذكاء الاصطناعي - نصف الشاشة */
        .ai-section {
            position: relative;
            margin-top: 15px;
        }

        .ai-button {
            width: 60px;
            height: 60px;
            background: linear-gradient(145deg, #ff7700, #ff5500);
            border-radius: 30px 30px 30px 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 28px;
            box-shadow: 0 10px 15px -3px #cc4400, 0 0 0 3px #ffd700 inset;
            cursor: pointer;
            transition: 0.2s;
            border: none;
            outline: none;
            margin-right: auto;
            z-index: 10;
        }

        .ai-button:hover {
            transform: scale(1.05) rotate(3deg);
        }

        /* نافذة الذكاء الاصطناعي - نصف الشاشة */
        .ai-expanded {
            position: fixed;
            top: auto;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(5px);
            display: none;
            justify-content: center;
            align-items: flex-end;
            z-index: 1000;
            padding: 10px;
            height: auto;
            animation: slideUp 0.3s ease;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .ai-expanded.show {
            display: flex;
        }

        .ai-card {
            max-width: 400px;
            width: 100%;
            background: white;
            border-radius: 35px 35px 25px 25px;
            padding: 20px;
            box-shadow: 0 -10px 30px rgba(0,0,0,0.2);
            position: relative;
            max-height: 50vh;
            overflow-y: auto;
        }

        .night-mode .ai-card {
            background: #1e2e3e;
            border: 1px solid #4682b4;
        }

        .ai-close {
            position: absolute;
            top: 10px;
            left: 10px;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: #ff4444;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .ai-header {
            text-align: center;
            margin-bottom: 15px;
        }

        .ai-header i {
            font-size: 35px;
            color: #ff7700;
            margin-bottom: 5px;
        }

        .ai-header h3 {
            font-size: 18px;
            font-weight: 700;
            color: #333;
        }

        .night-mode .ai-header h3 {
            color: #ffd966;
        }

        .ai-features {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }

        .ai-feature {
            background: rgba(255, 165, 0, 0.1);
            border-radius: 20px;
            padding: 12px 5px;
            text-align: center;
            cursor: pointer;
            transition: 0.2s;
            border: 1px solid #ffaa33;
        }

        .ai-feature:hover {
            transform: translateY(-2px);
            background: rgba(255, 165, 0, 0.2);
        }

        .ai-feature i {
            font-size: 22px;
            color: #ff7700;
            margin-bottom: 4px;
        }

        .ai-feature span {
            display: block;
            font-weight: 600;
            font-size: 12px;
            color: #333;
        }

        .night-mode .ai-feature span {
            color: #ffd966;
        }

        .ai-chat-box {
            background: rgba(255, 235, 200, 0.3);
            border-radius: 25px;
            padding: 12px;
        }

        .ai-message {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 8px;
            border-radius: 15px;
            background: white;
            margin-bottom: 10px;
            font-size: 13px;
        }

        .night-mode .ai-message {
            background: #2a4050;
        }

        .ai-message i {
            font-size: 22px;
            color: #ff7700;
        }

        .ai-input-area {
            display: flex;
            gap: 5px;
        }

        .ai-input {
            flex: 1;
            padding: 10px 12px;
            border-radius: 50px;
            border: 1px solid #ffaa33;
            font-family: 'Tajawal', sans-serif;
            font-size: 13px;
            background: white;
        }

        .ai-send {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #ffaa33;
            border: none;
            color: white;
            font-size: 16px;
            cursor: pointer;
        }

        .ai-response {
            padding: 10px;
            border-radius: 15px;
            font-size: 13px;
            min-height: 40px;
            background: white;
            margin-top: 8px;
        }

        .night-mode .ai-response {
            background: #2a4050;
            color: #d0e0ff;
        }
    </style>
</head>
<body class="day-mode">
    <button class="theme-toggle" id="themeToggle">
        <i class="fas fa-moon"></i>
    </button>

    <div class="app-container">
        <!-- الشاشة الرئيسية -->
        <div id="mainScreen">
            <!-- الرأس -->
            <div class="app-header">
                <div class="secondary-title">الشهيد قرينات بن حرز الله</div>
                <div class="supervisor"><i class="fas fa-chalkboard-teacher"></i> إشراف الأستاذة فاطمة قرينات</div>
                <div class="main-title">
                    RNF
                </div>
                <div class="names">
                    <span><i class="fas fa-star"></i> رغد</span>
                    <span><i class="fas fa-moon"></i> نور</span>
                    <span><i class="fas fa-heart"></i> فاطمة</span>
                </div>
            </div>

            <!-- فقاعات الأقسام -->
            <div class="sections-grid" id="sectionsGrid">
                <div class="bubble" data-section="intro">مقدمة</div>
                <div class="bubble" data-section="naissance">نشأة</div>
                <div class="bubble" data-section="principe">مبدأ عمل</div>
                <div class="bubble" data-section="proscons">إيجابيات وسلبيات</div>
                <div class="bubble" data-section="importance">أهمية في الجزائر</div>
                <div class="bubble" data-section="acteurs-opportunites">دور الفاعلين + فرص</div>
                <div class="bubble" data-section="conclusion-doaa">الخاتمة + الدعاء</div>
            </div>

            <!-- معاينة سريعة -->
            <div class="preview-area" id="previewArea">
                <div class="preview-text" id="previewText">
                    👆 اضغط على أي قسم لقراءة المحتوى كاملاً
                </div>
            </div>
        </div>

        <!-- شاشة عرض القسم -->
        <div id="sectionScreen">
            <div class="section-header">
                <button class="back-button" id="backButton">
                    <i class="fas fa-arrow-right"></i>
                </button>
                <h2 class="section-title" id="sectionTitle">مقدمة</h2>
            </div>
            
            <div class="full-content" id="fullContent">
                <!-- المحتوى الكامل سيتم تحميله هنا ديناميكياً -->
            </div>
        </div>

        <!-- زر الذكاء الاصطناعي -->
        <div class="ai-section">
            <button class="ai-button" id="aiButton">
                <i class="fas fa-robot"></i>
            </button>
        </div>
    </div>

    <!-- نافذة الذكاء الاصطناعي - نصف الشاشة -->
    <div class="ai-expanded" id="aiExpanded">
        <div class="ai-card">
            <button class="ai-close" id="aiClose">
                <i class="fas fa-times"></i>
            </button>
            
            <div class="ai-header">
                <i class="fas fa-robot"></i>
                <h3>مساعد RNF الذكي</h3>
            </div>

            <div class="ai-features">
                <div class="ai-feature" data-question="ما هي الطاقة الشمسية؟">
                    <i class="fas fa-sun"></i>
                    <span>الطاقة الشمسية</span>
                </div>
                <div class="ai-feature" data-question="كيف تعمل الخلايا الشمسية؟">
                    <i class="fas fa-microchip"></i>
                    <span>كيف تعمل؟</span>
                </div>
                <div class="ai-feature" data-question="ما هي فوائد الطاقة الشمسية؟">
                    <i class="fas fa-leaf"></i>
                    <span>الفوائد</span>
                </div>
                <div class="ai-feature" data-question="الطاقة الشمسية في الجزائر">
                    <i class="fas fa-map-marker-alt"></i>
                    <span>في الجزائر</span>
                </div>
                <div class="ai-feature" data-question="مراحل تحويل الطاقة الشمسية">
                    <i class="fas fa-chart-line"></i>
                    <span>المراحل</span>
                </div>
                <div class="ai-feature" data-question="تاريخ الطاقة الشمسية">
                    <i class="fas fa-history"></i>
                    <span>التاريخ</span>
                </div>
            </div>

            <div class="ai-chat-box">
                <div class="ai-message">
                    <i class="fas fa-robot"></i>
                    <span>مرحباً! اسألني عن أي شيء في التطبيق 🌞</span>
                </div>
                
                <div class="ai-response" id="aiResponse">
                    ✨ أنا هنا لمساعدتك
                </div>
                
                <div class="ai-input-area">
                    <input type="text" class="ai-input" id="aiInput" placeholder="اكتب سؤالك هنا...">
                    <button class="ai-send" id="aiSend"><i class="fas fa-paper-plane"></i></button>
                </div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            // عناصر التحكم
            const mainScreen = document.getElementById('mainScreen');
            const sectionScreen = document.getElementById('sectionScreen');
            const backButton = document.getElementById('backButton');
            const sectionTitle = document.getElementById('sectionTitle');
            const fullContent = document.getElementById('fullContent');
            const previewText = document.getElementById('previewText');
            
            // عناصر الذكاء الاصطناعي
            const aiButton = document.getElementById('aiButton');
            const aiExpanded = document.getElementById('aiExpanded');
            const aiClose = document.getElementById('aiClose');
            const aiFeatures = document.querySelectorAll('.ai-feature');
            const aiInput = document.getElementById('aiInput');
            const aiSend = document.getElementById('aiSend');
            const aiResponse = document.getElementById('aiResponse');

            // محتويات الأقسام مع تلوين الكلمات المهمة
            const sectionsContent = {
                intro: `
                    <h2>📖 مقدمة</h2>
                    <p>الحمد لله الذي جعل الشمس سراجًا وهاجًا، وبسط نورها رحمةً للعباد، وسخّر ما في السماوات وما في الأرض ليكون آيةً لأولي الألباب، والصلاة والسلام على من دعا إلى عمارة الأرض وحسن استثمار نعم الله، سيدنا محمد ﷺ، وعلى آله وصحبه أجمعين.<br><br>أما بعد، فإنّ من نعم الله العظيمة التي قلّ من يتدبّرها حق التدبّر نعمة الشمس، ذلك النور الدائم الذي لا ينقطع، والذي جعله الله مصدر حياةٍ وطاقة، ومنه انطلقت فكرة الطاقة الشمسية لتكون سبيلًا من سبل الاستخلاف الرشيد، وحلًا من حلول العصر لمواجهة حاجات الإنسان المتزايدة للطاقة دون إفسادٍ في الأرض.</p>
                `,
                naissance: `
                    <h2>🌅 نشأة فكرة الطاقة الشمسية</h2>
                    <p>بدأ المسار العلمي لفكرة الطاقة الشمسية في <span class="date-highlight">القرن التاسع عشر</span>، حين اكتشف العالم الفرنسي <span class="scientist-name">إدمون بيكريل</span> سنة <span class="date-highlight">1839</span> ظاهرة <span class="highlight-blue">التأثير الكهروضوئي</span>، حيث لاحظ تولّد تيار كهربائي عند تعرّض بعض المواد للضوء، وكان هذا الاكتشاف الأساس النظري لتحويل ضوء الشمس إلى طاقة كهربائية. ثم جاء العالم <span class="scientist-name">ألبرت أينشتاين</span> سنة <span class="date-highlight">1905</span> ليُفسّر هذه الظاهرة تفسيرًا علميًا دقيقًا، موضحًا طبيعة الضوء ودوره في تحرير الإلكترونات، وهو ما أهّله لاحقًا لنيل <span class="highlight-green">جائزة نوبل في الفيزياء</span>. وفي أواخر القرن التاسع عشر، قام المخترع الأمريكي <span class="scientist-name">تشارلز فريتس</span> بتصنيع أول <span class="highlight-orange">خلية شمسية بدائية</span> من السيلينيوم، لتكون أول تطبيق عملي للفكرة. أما الانطلاقة الصناعية الحقيقية للطاقة الشمسية، فقد تحققت سنة <span class="date-highlight">1954</span> عندما طوّرت <span class="highlight-purple">مختبرات بِل (Bell Labs)</span> أول <span class="highlight-blue">خلية شمسية فعّالة</span> وقابلة للاستعمال التجاري، لتبدأ بذلك مرحلة جديدة في تاريخ استغلال الطاقة الشمسية.</p>
                `,
                principe: `
                    <h2>⚙️ مبدأ عمل الطاقة الشمسية</h2>
                    <p>يقوم مبدأ عمل الطاقة الشمسية على استغلال <span class="highlight-orange">إشعاع الشمس</span> وتحويله إلى طاقة نافعة، وذلك أساسًا عبر <span class="highlight-blue">الخلايا الشمسية</span> المصنوعة من مواد شبه موصلة مثل <span class="highlight-green">السيليكون</span>. فعند سقوط أشعة الشمس على هذه الخلايا، تمتص <span class="highlight-purple">الفوتونات</span> طاقة الضوء، مما يؤدي إلى تحرير <span class="highlight-blue">الإلكترونات</span> داخل المادة، فتتولد حركة إلكترونية تُنتج <span class="highlight-orange">تيارًا كهربائيًا مستمرًا</span> يُعرف <span class="highlight-green">بالتأثير الكهروضوئي</span>. ثم يُحوَّل هذا التيار إلى <span class="highlight-purple">تيار متناوب</span> صالح للاستعمال بواسطة <span class="highlight-blue">العاكس الكهربائي</span>، ليُستخدم في تشغيل الأجهزة أو يُخزَّن في <span class="highlight-green">البطاريات</span>. كما يمكن استغلال حرارة الشمس مباشرة في <span class="highlight-orange">الأنظمة الحرارية</span> لتسخين المياه أو إنتاج البخار، وبذلك تُعدّ الشمس مصدرًا يجمع بين الضوء والحرارة لخدمة الإنسان.</p>
                    
                    <div class="stages-container">
                        <div class="stage-box stage-1">
                            <div class="stage-number">①</div>
                            <div class="stage-title">المرحلة الأولى: الامتصاص</div>
                            <div class="stage-desc">تمتص <span class="highlight-blue">الخلايا الشمسية</span> المصنوعة من <span class="highlight-green">السيليكون</span> أشعة الشمس، حيث تلتقط <span class="highlight-purple">الفوتونات</span> حاملات الطاقة الضوئية.</div>
                        </div>
                        
                        <div class="stage-box stage-2">
                            <div class="stage-number">②</div>
                            <div class="stage-title">المرحلة الثانية: التأثير الكهروضوئي</div>
                            <div class="stage-desc">تحرر <span class="highlight-orange">الفوتونات</span> <span class="highlight-blue">الإلكترونات</span> داخل المادة شبه الموصلة، مما يولد <span class="highlight-green">تياراً كهربائياً مستمراً</span>.</div>
                        </div>
                        
                        <div class="stage-box stage-3">
                            <div class="stage-number">③</div>
                            <div class="stage-title">المرحلة الثالثة: التحويل والتخزين</div>
                            <div class="stage-desc">يحول <span class="highlight-purple">العاكس الكهربائي</span> التيار المستمر إلى <span class="highlight-orange">تيار متردد</span> للاستخدام المنزلي، مع إمكانية تخزين الطاقة في <span class="highlight-green">البطاريات</span>.</div>
                        </div>
                    </div>
                `,
                proscons: `
                    <h2>🌟 إيجابيات وسلبيات الطاقة الشمسية</h2>
                    
                    <div class="pros-box">
                        <strong>✅ الإيجابيات:</strong>
                        <p>تتميّز الطاقة الشمسية بكونها <span class="highlight-green">مصدرًا متجددًا ونظيفًا لا ينفد</span>، إذ تساهم في تقليل <span class="highlight-blue">التلوث البيئي والانبعاثات الضارة</span>، وتحافظ على صحة الإنسان والتوازن البيئي. كما تساعد على التقليل من الاعتماد على <span class="highlight-orange">الوقود الأحفوري</span>، وتوفّر <span class="highlight-purple">طاقة مجانية</span> بعد تركيب المنظومة، إضافة إلى ملاءمتها <span class="highlight-green">للمناطق النائية</span> التي يصعب ربطها بشبكات الكهرباء التقليدية.</p>
                    </div>
                    
                    <div class="cons-box">
                        <strong>❌ السلبيات:</strong>
                        <p>غير أنّ للطاقة الشمسية بعض السلبيات، من أبرزها ارتفاع <span class="highlight-red">تكلفة التركيب الأولية</span>، واعتماد إنتاجها على <span class="highlight-orange">شدة الإشعاع الشمسي والظروف المناخية</span>، مما يجعلها أقل استقرارًا في بعض الأوقات. كما تتطلب <span class="highlight-blue">مساحات واسعة</span> لتركيب الألواح في المشاريع الكبرى، إضافة إلى الحاجة إلى <span class="highlight-purple">وسائل تخزين فعّالة</span> لضمان استمرار التزوّد بالطاقة عند غياب الشمس.</p>
                    </div>
                `,
                importance: `
                    <h2>🇩🇿 أهمية الطاقة الشمسية في الجزائر</h2>
                    <p>تكتسب الطاقة الشمسية أهمية بالغة في الجزائر نظرًا لموقعها الجغرافي المميّز، حيث تتمتع البلاد بنسبة عالية من <span class="highlight-orange">الإشعاع الشمسي</span> طوال معظم أيام السنة، خاصة في المناطق <span class="highlight-blue">الجنوبية والصحراوية</span>. ويسمح هذا الامتياز الطبيعي باستغلال الشمس كمصدر طاقة فعّال ومستدام، يساهم في تلبية الطلب المتزايد على الكهرباء وتقليل الضغط على الموارد الطاقوية التقليدية، ودعم <span class="highlight-green">التنمية المستدامة</span>.</p>
                `,
                'acteurs-opportunites': `
                    <div class="inner-section">
                        <h2>🔋 دور الجزائر والفاعلين</h2>
                        <h3>أولاً: أبرز الفاعلين في برنامج الطاقة الشمسية (<span class="highlight-orange">3,200 ميجاواط</span>)</h3>
                        <ul>
                            <li><strong><span class="highlight-blue">مجمع سونلغاز:</span></strong> المالك والمشرف الرئيسي على جميع المشاريع، والمسؤول عن ربط المحطات بالشبكة الكهربائية الوطنية.</li>
                            <li><strong><span class="highlight-green">شركة أوزغون (Özgün):</span></strong> تشرف على محطة حاسي الدلاعة بقدرة <span class="highlight-orange">362 ميجاواط</span>، وتتولى التصميم والبناء والتشغيل.</li>
                            <li><strong><span class="highlight-purple">مجمع كوزيدار (Cosider):</span></strong> ينجز محطات تاندلة (<span class="highlight-orange">200 ميجاواط</span>) ونخلة (<span class="highlight-orange">80 ميجاواط</span>)، مع خبرة في الهندسة المدنية والتركيب.</li>
                            <li><strong><span class="highlight-blue">شركة شمس (SHAEMS):</span></strong> شركة مشتركة بين سونلغاز وسوناطراك، مسؤولة عن تنظيم المشاريع، هندسة المناقصات، وجذب المستثمرين.</li>
                            <li><strong><span class="highlight-green">التحالفات الصينية (CWE، HXCC وغيرها):</span></strong> تساهم في إنجاز محطات باتنة (<span class="highlight-orange">220 ميجاواط</span>)، المسيلة (<span class="highlight-orange">200 ميجاواط</span>)، وأولاد جلال (<span class="highlight-orange">80 ميجاواط</span>)، مع نقل التكنولوجيا وتسريع الإنجاز.</li>
                        </ul>
                    </div>
                    
                    <div class="inner-section">
                        <h2>💡 فرص المشاريع الشمسية في الجزائر</h2>
                        <div class="items-grid">
                            <div class="item-card">🌾 <span class="highlight-green">الفلاحة الذكية</span> (الضخ الشمسي)</div>
                            <div class="item-card">🧼 <span class="highlight-blue">صيانة المحطات</span> والتنظيف الآلي</div>
                            <div class="item-card">💡 <span class="highlight-orange">الإنارة العمومية</span> بالطاقة الشمسية</div>
                            <div class="item-card">🏭 <span class="highlight-purple">الأنظمة الهجينة</span> للمصانع</div>
                            <div class="item-card">🔋 <span class="highlight-green">تجميع بطاريات الليثيوم</span> محلياً</div>
                        </div>
                        <p style="margin-top:10px">هذه الفرص تساهم في تقليل التكاليف وتحسين الاستدامة ودعم الاقتصاد الوطني.</p>
                    </div>
                `,
                'conclusion-doaa': `
                    <div class="inner-section">
                        <h2>🔆 الخاتمة</h2>
                        <p>في الختام، تُعدّ الطاقة الشمسية <span class="highlight-green">نعمة من نعم الله</span>، ومصدرًا واعدًا من مصادر الطاقة المتجددة، يجمع بين الحفاظ على البيئة وتلبية حاجات الإنسان المتزايدة للطاقة. ويُمثّل الاستثمار فيها خطوة أساسية نحو تحقيق <span class="highlight-blue">التنمية المستدامة</span> وبناء مستقبل أفضل للأجيال القادمة، خاصة في بلدٍ يتمتع بإمكانات شمسية كبيرة كالجزائر.</p>
                    </div>
                    
                    <div class="inner-section">
                        <h2>🤲 الدعاء</h2>
                        <p>اللهم كما سخّرت لنا الشمس نورًا ودفئًا، سخّر لنا من نعمك ما يعيننا على <span class="highlight-green">عمارة الأرض</span>، واحفظ بلادنا من كل سوء، ووفّقنا لحسن استثمار نعمك، واجعل أعمالنا خالصة لوجهك الكريم، وبارك في علمنا وجهدنا، إنك على كل شيء قدير.</p>
                    </div>
                `
            };

            // عناوين الأقسام
            const sectionTitles = {
                intro: 'مقدمة',
                naissance: 'نشأة الفكرة',
                principe: 'مبدأ العمل',
                proscons: 'الإيجابيات والسلبيات',
                importance: 'أهمية في الجزائر',
                'acteurs-opportunites': 'الفاعلون والفرص',
                'conclusion-doaa': 'الخاتمة والدعاء'
            };

            // نصوص المعاينة
            const previews = {
                intro: 'الحمد لله الذي جعل الشمس سراجًا وهاجًا...',
                naissance: 'بدأ المسار العلمي لفكرة الطاقة الشمسية في القرن التاسع عشر...',
                principe: 'يقوم مبدأ عمل الطاقة الشمسية على استغلال إشعاع الشمس...',
                proscons: 'تتميّز الطاقة الشمسية بكونها مصدرًا متجددًا ونظيفًا...',
                importance: 'تكتسب الطاقة الشمسية أهمية بالغة في الجزائر...',
                'acteurs-opportunites': 'أبرز الفاعلين: سونلغاز، أوزغون، كوزيدار...',
                'conclusion-doaa': 'الطاقة الشمسية نعمة من نعم الله... اللهم كما سخّرت لنا الشمس...'
            };

            // الفقاعات
            const bubbles = document.querySelectorAll('.bubble');
            
            bubbles.forEach(bubble => {
                bubble.addEventListener('click', function() {
                    const section = this.dataset.section;
                    
                    sectionTitle.textContent = sectionTitles[section];
                    fullContent.innerHTML = sectionsContent[section];
                    previewText.textContent = '🔄 ' + previews[section];
                    
                    mainScreen.style.display = 'none';
                    sectionScreen.style.display = 'block';
                });
            });

            backButton.addEventListener('click', function() {
                sectionScreen.style.display = 'none';
                mainScreen.style.display = 'block';
            });

            // الذكاء الاصطناعي - فتح النافذة (نصف الشاشة)
            aiButton.addEventListener('click', function() {
                aiExpanded.classList.add('show');
            });

            aiClose.addEventListener('click', function() {
                aiExpanded.classList.remove('show');
            });

            // النقر على الأيقونات
            aiFeatures.forEach(feature => {
                feature.addEventListener('click', function() {
                    const question = this.dataset.question;
                    aiInput.value = question;
                    sendQuestion();
                });
            });

            // قاعدة المعرفة الموسعة للذكاء الاصطناعي
            function getAIResponse(question) {
                question = question.toLowerCase();
                
                // تحية
                if (question.includes('السلام') || question.includes('مرحبا') || question.includes('اهلاً')) {
                    return 'وعليكم السلام! 👋 أنا مساعد RNF، كيف يمكنني مساعدتك في فهم الطاقة الشمسية أو محتوى التطبيق؟';
                }
                
                // الطاقة الشمسية بشكل عام
                else if (question.includes('ما هي الطاقة الشمسية') || question.includes('تعريف الطاقة الشمسية')) {
                    return 'الطاقة الشمسية هي تحويل ضوء الشمس إلى كهرباء باستخدام الخلايا الكهروضوئية. إنها مصدر نظيف ومتجدد للطاقة! ☀️ تعتمد على امتصاص الفوتونات من أشعة الشمس وتحويلها إلى تيار كهربائي.';
                }
                
                // كيفية العمل
                else if (question.includes('كيف تعمل') || question.includes('مبدأ عمل') || question.includes('آلية')) {
                    return 'تعمل الطاقة الشمسية عبر 3 مراحل رئيسية:\n1️⃣ تمتص الخلايا الشمسية (السيليكون) أشعة الشمس.\n2️⃣ تحرر الفوتونات الإلكترونات مسببة تياراً كهربائياً مستمراً.\n3️⃣ يحول العاكس الكهربائي التيار إلى تيار متردد للاستخدام المنزلي.';
                }
                
                // الإيجابيات
                else if (question.includes('إيجابيات') || question.includes('فوائد') || question.includes('مميزات')) {
                    return '✅ من إيجابيات الطاقة الشمسية:\n• مصدر متجدد ونظيف\n• يقلل التلوث والانبعاثات\n• طاقة مجانية بعد التركيب\n• مثالية للمناطق النائية\n• تقلل الاعتماد على الوقود الأحفوري';
                }
                
                // السلبيات
                else if (question.includes('سلبيات') || question.includes('عيوب')) {
                    return '❌ من سلبيات الطاقة الشمسية:\n• تكلفة تركيب أولية مرتفعة\n• تعتمد على الظروف الجوية\n• تحتاج مساحات واسعة\n• تحتاج بطاريات للتخزين\n• أقل كفاءة في الليل';
                }
                
                // النشأة والتاريخ
                else if (question.includes('نشأة') || question.includes('تاريخ') || question.includes('بداية')) {
                    return '📚 تاريخ الطاقة الشمسية:\n• 1839: إدمون بيكريل يكتشف التأثير الكهروضوئي\n• 1905: أينشتاين يفسر الظاهرة\n• 1954: أول خلية شمسية عملية في مختبرات بيل';
                }
                
                // العلماء
                else if (question.includes('إدمون بيكريل') || question.includes('أينشتاين') || question.includes('تشارلز فريتس')) {
                    return '👨‍🔬 العلماء المساهمون:\n• إدمون بيكريل (1839): اكتشف التأثير الكهروضوئي\n• ألبرت أينشتاين (1905): فسر الظاهرة وحصل على نوبل\n• تشارلز فريتس: صنع أول خلية شمسية بدائية';
                }
                
                // الجزائر
                else if (question.includes('الجزائر') || question.includes('جزائر')) {
                    return '🇩🇿 الطاقة الشمسية في الجزائر:\n• إشعاع شمسي عالٍ جداً (أكثر من 3000 ساعة/سنة)\n• مشروع 3200 ميجاواط\n• فاعلون: سونلغاز، أوزغون، كوزيدار\n• فرص: فلاحة ذكية، إنارة عمومية، بطاريات محلية';
                }
                
                // الفاعلون
                else if (question.includes('فاعلين') || question.includes('سونلغاز') || question.includes('أوزغون') || question.includes('كوزيدار')) {
                    return '🏭 أبرز الفاعلين:\n• سونلغاز: المشرف الرئيسي\n• أوزغون: محطة حاسي الدلاعة (362 ميجاواط)\n• كوزيدار: محطات تاندلة ونخلة\n• شمس: تنظيم المشاريع\n• تحالفات صينية: باتنة، المسيلة';
                }
                
                // فرص المشاريع
                else if (question.includes('فرص') || question.includes('مشاريع') || question.includes('استثمار')) {
                    return '💡 فرص المشاريع:\n• الفلاحة الذكية (الضخ الشمسي)\n• صيانة وتنظيف المحطات\n• الإنارة العمومية\n• أنظمة هجينة للمصانع\n• تجميع بطاريات الليثيوم محلياً';
                }
                
                // مراحل العمل
                else if (question.includes('مراحل') || question.includes('خطوات')) {
                    return '🔬 المراحل الثلاث:\n① الامتصاص: الخلايا تمتص الفوتونات\n② التأثير الكهروضوئي: تحرير الإلكترونات\n③ التحويل: العاكس يحول التيار';
                }
                
                // الخلايا الشمسية
                else if (question.includes('خلايا') || question.includes('ألواح') || question.includes('سيليكون')) {
                    return '🔋 الخلايا الشمسية:\n• تصنع من السيليكون (مادة شبه موصلة)\n• تمتص الفوتونات من الضوء\n• تحول الطاقة الضوئية إلى كهرباء\n• تحتاج لعاكس لتحويل التيار';
                }
                
                // التخزين
                else if (question.includes('تخزين') || question.includes('بطاريات')) {
                    return '🔋 تخزين الطاقة:\n• تستخدم بطاريات لتخزين الطاقة ليلاً\n• تحتاج لأنظمة تخزين فعالة\n• فرص لتجميع البطاريات محلياً في الجزائر';
                }
                
                // الليل
                else if (question.includes('ليل') || question.includes('غروب') || question.includes('مشكلة')) {
                    return '🌙 مشكلة الليل:\n• لا تنتج الطاقة الشمسية كهرباء ليلاً\n• الحل: استخدام بطاريات تخزين\n• أو ربطها بالشبكة الكهربائية';
                }
                
                // التكلفة
                else if (question.includes('تكلفة') || question.includes('سعر') || question.includes('ثمن')) {
                    return '💰 التكلفة:\n• التركيب الأولي مرتفع\n• لكنها مجانية بعد ذلك\n• توفر فواتير الكهرباء\n• تحتاج صيانة دورية';
                }
                
                // الرد الافتراضي
                else {
                    return 'شكراً لسؤالك! يمكنك:\n• استخدام الأيقونات أعلاه\n• الاطلاع على أقسام التطبيق\n• إعادة صياغة السؤال\nأنا هنا لمساعدتك في فهم الطاقة الشمسية 💡';
                }
            }

            // إرسال السؤال
            function sendQuestion() {
                const question = aiInput.value.trim();
                if (question) {
                    aiResponse.innerHTML = '🤔 جاري التفكير...';
                    
                    setTimeout(() => {
                        const answer = getAIResponse(question);
                        aiResponse.innerHTML = answer.replace(/\n/g, '<br>');
                    }, 300);
                }
            }

            aiSend.addEventListener('click', sendQuestion);
            aiInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendQuestion();
                }
            });

            // إغلاق النافذة بالضغط خارجها
            aiExpanded.addEventListener('click', function(e) {
                if (e.target === aiExpanded) {
                    aiExpanded.classList.remove('show');
                }
            });

            // تبديل الوضع الليلي/النهاري
            const themeToggle = document.getElementById('themeToggle');
            const body = document.body;

            themeToggle.addEventListener('click', function() {
                if (body.classList.contains('day-mode')) {
                    body.classList.remove('day-mode');
                    body.classList.add('night-mode');
                    themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
                } else {
                    body.classList.remove('night-mode');
                    body.classList.add('day-mode');
                    themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
                }
            });
        })();
    </script>
</body>
</html>
