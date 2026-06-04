<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VS Extractor Pro</title>
    
    <script src="https://accounts.google.com/gsi/client" async defer></script>

    <style>
        /* CSS Variables for Light and Dark Themes */
        :root {
            --bg-color: #f4f7f6;
            --text-color: #333333;
            --header-bg: #3b5998;
            --header-text: #ffffff;
            --sidebar-bg: #ffffff;
            --card-bg: #ffffff;
            --shadow: rgba(0, 0, 0, 0.1);
            --accent-color: #e74c3c;
            --success-color: #2ecc71;
            --spinner-color: #3498db;
            --folder-bg: #e2e8f0;
            --login-box-bg: #ffffff;
        }

        .dark-mode {
            --bg-color: #1e1e1e;
            --text-color: #f4f7f6;
            --header-bg: #111111;
            --header-text: #ffffff;
            --sidebar-bg: #2d2d2d;
            --card-bg: #2d2d2d;
            --shadow: rgba(0, 0, 0, 0.5);
            --spinner-color: #e74c3c;
            --folder-bg: #3d3d3d;
            --login-box-bg: #252525;
        }

        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            transition: background-color 0.3s, color 0.3s;
            overflow-x: hidden;
        }

        /* Splash Screen */
        #splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: var(--bg-color);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 99999;
            transition: opacity 0.5s ease-out, visibility 0.5s;
        }

        .splash-content {
            position: relative;
            width: 80px;
            height: 80px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .splash-logo {
            font-size: 32px;
            font-weight: bold;
            color: var(--header-bg);
            z-index: 2;
            letter-spacing: 2px;
        }

        .splash-spinner {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: 4px solid transparent;
            border-top: 4px solid var(--spinner-color);
            border-right: 4px solid var(--spinner-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            box-sizing: border-box;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Header Layout Configuration */
        header {
            background-color: var(--header-bg);
            color: var(--header-text);
            padding: 10px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 100;
            box-sizing: border-box;
            height: 60px;
        }

        .header-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .menu-icon {
            font-size: 24px;
            cursor: pointer;
            user-select: none;
        }

        header h1 {
            margin: 0;
            font-size: 20px;
            transition: opacity 0.2s;
        }

        .header-right-normal {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        /* Custom Black Theme Sign-In Button */
        .custom-signin-btn {
            background-color: #000000;
            color: #ffffff;
            border: 1px solid #333333;
            padding: 8px 16px;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            transition: background-color 0.2s;
        }

        .custom-signin-btn:hover {
            background-color: #222222;
        }

        .user-info {
            display: none;
            align-items: center;
            gap: 10px;
            font-weight: bold;
            font-size: 14px;
        }

        .user-info img {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: 2px solid white;
        }

        .header-right-selection {
            display: none; 
            align-items: center;
            position: relative;
        }

        #selectionCount {
            font-size: 18px;
            font-weight: bold;
            margin-right: 15px;
        }

        .three-dot-icon {
            font-size: 28px;
            cursor: pointer;
            user-select: none;
            padding: 5px 10px;
        }

        .dropdown-menu {
            display: none;
            position: absolute;
            top: 45px;
            right: 0;
            background-color: var(--card-bg);
            min-width: 160px;
            box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
            z-index: 200;
            border-radius: 6px;
            overflow: hidden;
            border: 1px solid rgba(0,0,0,0.1);
        }

        .dropdown-menu div {
            color: var(--text-color);
            padding: 12px 16px;
            font-weight: 500;
            cursor: pointer;
            text-align: left;
        }

        .dropdown-menu div:hover {
            background-color: var(--bg-color);
        }

        .dropdown-menu .delete-option {
            color: var(--accent-color);
            border-top: 1px solid rgba(0,0,0,0.05);
        }

        /* Sidebar Dynamic Containers */
        .sidebar {
            height: 100%;
            width: 0;
            position: fixed;
            z-index: 99;
            top: 60px;
            left: 0;
            background-color: var(--sidebar-bg);
            overflow-x: hidden;
            transition: 0.3s;
            padding-top: 20px;
            box-shadow: 2px 0 5px var(--shadow);
            box-sizing: border-box;
        }

        .sidebar.open {
            width: 260px;
            padding: 20px;
        }

        .sidebar h3 {
            margin-top: 0;
            border-bottom: 1px solid #ccc;
            padding-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: pointer;
            user-select: none;
        }

        .sidebar-item {
            margin-bottom: 20px;
        }

        /* DEFAULT CLOSED CSS FOR SECTION WRAPPERS */
        .sidebar-toggle-content {
            display: none;
            margin-top: 10px;
        }

        .sidebar-toggle-content.visible {
            display: block;
        }

        button {
            padding: 8px 12px;
            background-color: var(--header-bg);
            color: var(--header-text);
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
        }

        button:hover {
            opacity: 0.9;
        }

        .album-input {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        .album-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .album-list li {
            padding: 10px;
            background: var(--bg-color);
            margin-bottom: 5px;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .album-list li.active {
            background-color: var(--header-bg);
            color: var(--header-text);
        }

        .toggle-icon {
            cursor: pointer;
            font-size: 14px;
            user-select: none;
        }

        /* Main Workspace Layout */
        main {
            margin-top: 80px;
            padding: 20px;
            transition: margin-left 0.3s;
        }

        .upload-section {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px var(--shadow);
            margin-bottom: 25px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            max-width: 500px;
            position: relative;
        }

        .timer-display {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 14px;
            font-weight: bold;
            color: var(--header-bg);
            background: var(--bg-color);
            padding: 4px 8px;
            border-radius: 4px;
            display: none;
        }

        .character-upload-container {
            display: flex;
            gap: 20px;
            margin-bottom: 10px;
        }

        .upload-box {
            flex: 1;
            height: 120px;
            border: 2px dashed #ccc;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            background-color: var(--bg-color);
            transition: 0.2s;
            width: 100%;
            box-sizing: border-box;
        }

        .upload-box:hover {
            border-color: var(--header-bg);
            background-color: rgba(0,0,0,0.05);
        }

        .upload-box span {
            font-size: 40px;
            color: #888;
            margin-bottom: 5px;
        }

        .upload-box p {
            margin: 0;
            font-size: 13px;
            color: #888;
            font-weight: bold;
        }

        .preview-img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: none;
        }

        .video-overlay-text {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            background: rgba(0,0,0,0.6);
            color: white;
            font-size: 12px;
            padding: 5px 0;
            text-align: center;
            display: none;
        }

        #status, #charStatus {
            font-weight: bold;
            color: var(--success-color);
        }

        /* 16:9 Folder Grid Layout System */
        .folder-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .folder-card {
            background-color: var(--folder-bg);
            border-radius: 12px;
            aspect-ratio: 16 / 9;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 10px var(--shadow);
            cursor: pointer;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 15px;
            box-sizing: border-box;
            border: 2px solid transparent;
            transition: transform 0.2s, border-color 0.2s;
        }

        .folder-card:hover {
            transform: scale(1.02);
            border-color: var(--header-bg);
        }

        .folder-title {
            font-size: 16px;
            font-weight: bold;
            margin: 0;
            text-shadow: 0 1px 2px rgba(0,0,0,0.1);
        }

        .folder-info {
            font-size: 12px;
            color: #666;
            margin: 0;
        }

        .dark-mode .folder-info {
            color: #ccc;
        }

        /* Circular Processing Animation Icon Placement */
        .process-icon-wrap {
            position: absolute;
            right: 15px;
            top: calc(50% - 25px);
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 20;
        }

        .process-circle {
            position: absolute;
            width: 100%;
            height: 100%;
            border: 2px dashed var(--spinner-color);
            border-radius: 50%;
            animation: spin 6s linear infinite;
        }

        .process-arrow {
            font-size: 22px;
            font-weight: bold;
            color: var(--text-color);
            transform: translateY(1px);
        }

        /* Gallery Grid Layout */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 20px;
            margin-top: 10px;
        }

        .image-card {
            position: relative;
            background: var(--card-bg);
            border-radius: 6px;
            overflow: hidden;
            box-shadow: 0 4px 6px var(--shadow);
            border: 3px solid transparent;
            box-sizing: border-box;
            cursor: pointer;
            transition: transform 0.2s, border 0.2s;
        }

        .image-card:hover {
            transform: scale(1.02);
        }

        .image-card.selected {
            border: 3px solid var(--success-color);
        }

        .image-card img {
            width: 100%;
            height: auto;
            display: block;
        }

        .selection-tag {
            position: absolute;
            top: 8px;
            left: 8px;
            background-color: var(--success-color);
            color: white;
            width: 26px;
            height: 26px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            font-weight: bold;
            opacity: 0;
            transition: opacity 0.2s;
            pointer-events: none;
            box-shadow: 0 2px 4px rgba(0,0,0,0.5);
            z-index: 10;
        }

        .selection-tag.active {
            opacity: 1;
        }

        .download-btn {
            position: absolute;
            bottom: 8px;
            right: 8px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            border: none;
            border-radius: 4px;
            padding: 6px 12px;
            font-size: 12px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 2px 4px rgba(0,0,0,0.4);
            z-index: 10;
            transition: background 0.3s;
        }

        .download-btn:hover {
            background-color: var(--success-color);
        }

        /* Full Interactive Custom Multi-Sign-In Page Portal Layout */
        #loginGatewayPage {
            display: none;
            position: fixed;
            top: 60px;
            left: 0;
            width: 100vw;
            height: calc(100vh - 60px);
            background-color: var(--bg-color);
            z-index: 200;
            padding: 40px 20px;
            box-sizing: border-box;
            overflow-y: auto;
        }

        .login-container {
            max-width: 420px;
            margin: 0 auto;
            background-color: var(--login-box-bg);
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 10px 25px var(--shadow);
            text-align: center;
            box-sizing: border-box;
        }

        .login-method-btn {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            border-radius: 6px;
            border: 1px solid #ccc;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            font-size: 15px;
            transition: background 0.2s;
        }

        .btn-google { background-color: #ffffff; color: #444; border: 1px solid #ddd; }
        .btn-google:hover { background-color: #f5f5f5; }
        .btn-phone { background-color: #2ecc71; color: white; border: none; }
        .btn-phone:hover { opacity: 0.9; }
        .btn-email { background-color: #34495e; color: white; border: none; }
        .btn-email:hover { opacity: 0.9; }

        .login-input {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }

        /* Dedicated Views Config */
        #downloadsPage, #archiveViewPage {
            display: none;
            position: fixed;
            top: 60px;
            left: 0;
            width: 100vw;
            height: calc(100vh - 60px);
            background-color: var(--bg-color);
            z-index: 95;
            padding: 20px;
            box-sizing: border-box;
            overflow-y: auto;
        }

        .page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-bottom: 2px solid #ccc;
            padding-bottom: 10px;
        }

        /* Full Screen Black Background Viewer */
        #imageViewer {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: #000000;
            z-index: 1000;
            flex-direction: column;
            justify-content: space-between;
        }

        .viewer-image-container {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            padding: 0 20px;
            margin-top: 20px;
        }

        .viewer-image-container img {
            max-width: 100%;
            max-height: 100%;
            object-fit: contain;
        }

        .viewer-bottom {
            background-color: #111;
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            font-size: 14px;
            position: relative;
        }

        .viewer-action {
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }

        .viewer-action:hover {
            color: var(--spinner-color);
        }

        #editMenu {
            display: none;
            position: absolute;
            bottom: 60px; 
            left: 0;
            width: 100%;
            background-color: #222;
            padding: 15px 0;
            justify-content: space-around;
            color: #ddd;
            border-top: 1px solid #333;
        }

        .edit-option {
            cursor: pointer;
            font-size: 13px;
        }
        .edit-option:hover {
            color: white;
        }

        .view-section-title {
            margin-top: 20px;
            border-bottom: 1px solid #ccc;
            padding-bottom: 5px;
        }

    </style>
</head>
<body>

    <div id="splash-screen">
        <div class="splash-content">
            <div class="splash-spinner"></div>
            <div class="splash-logo">VS</div>
        </div>
    </div>

    <header>
        <div class="header-left">
            <div class="menu-icon" onclick="toggleSidebar()">&#9776;</div>
            <h1 id="appTitle">VS Extractor</h1>
        </div>
        
        <div class="header-right-normal" id="headerNormalView">
            <button class="custom-signin-btn" id="customSignInBtn" onclick="openLoginGateway()">Sign In</button>
            <div id="userInfo" class="user-info"></div>
        </div>

        <div class="header-right-selection" id="headerSelectionView">
            <span id="selectionCount">0</span>
            <div class="three-dot-icon" onclick="toggleDropdownMenu(event)">&#8942;</div>
            <div class="dropdown-menu" id="threeDotDropdown">
                <div onclick="selectAllEngine()">Select All</div>
                <div onclick="clearSelectionEngine()">Deselect All</div>
                <div onclick="deleteSelectedEngine()" class="delete-option">Delete</div>
            </div>
        </div>
    </header>

    <div id="sidebar" class="sidebar">
        <div class="sidebar-item">
            <h3>Settings</h3>
            <button onclick="toggleDarkMode()">Switch Theme (Dark/Light)</button>
        </div>
        
        <div class="sidebar-item">
            <h3 onclick="toggleSidebarSection('albumContainer', 'albumToggleIcon')">
                Manage Albums 
                <span class="toggle-icon" id="albumToggleIcon">▼</span>
            </h3>
            <div id="albumContainer" class="sidebar-toggle-content">
                <input type="text" id="albumInput" class="album-input" placeholder="Enter album name...">
                <button onclick="createNewAlbum()" style="width: 100%;">+ Create Album</button>
                <ul id="albumList" class="album-list" style="margin-top: 15px;"></ul>
            </div>
        </div>

        <div class="sidebar-item">
            <h3 onclick="toggleSidebarSection('downloadContent', 'downloadToggleIcon')">
                Downloads 
                <span class="toggle-icon" id="downloadToggleIcon">▼</span>
            </h3>
            <div id="downloadContent" class="sidebar-toggle-content">
                <button onclick="openDownloadsPage()" style="width: 100%; background-color: var(--success-color);">View Downloads 📥</button>
            </div>
        </div>

        <div class="sidebar-item">
            <h3 onclick="toggleSidebarSection('extractContent', 'extractToggleIcon')">
                Extract Image
                <span class="toggle-icon" id="extractToggleIcon">▼</span>
            </h3>
            <div id="extractContent" class="sidebar-toggle-content">
                <button onclick="openArchiveViewPage()" style="width: 100%; background-color: #f39c12;">All Extract Image 📂</button>
            </div>
        </div>
    </div>

    <main id="mainContent">
        <div class="upload-section">
            <span id="normalTimer" class="timer-display">00:00</span>
            <h3>Upload Video (Normal Extract)</h3>
            
            <div class="upload-box" onclick="document.getElementById('normalVideoFile').click()">
                <span>+</span>
                <p>Upload Video</p>
                <input type="file" id="normalVideoFile" accept="video/*" style="display:none;" onchange="previewNormalVideo(event)">
                <div id="normalVideoOverlay" class="video-overlay-text">Video Added ✓</div>
            </div>

            <button onclick="processVideo()">Extract Scenes</button>
            <div id="status"></div>
        </div>

        <div class="upload-section">
            <h3>Extract Character Scenes (Smart Match)</h3>
            <p style="font-size: 12px; margin-top:-5px; color:#777;">Capture scenes automatically based on character appearance.</p>
            
            <div class="character-upload-container">
                <div class="upload-box" onclick="document.getElementById('charPhoto').click()">
                    <span>+</span>
                    <p>Upload Photo</p>
                    <input type="file" id="charPhoto" accept="image/*" style="display:none;" onchange="previewCharPhoto(event)">
                    <img id="charPhotoPreview" class="preview-img">
                </div>
                
                <div class="upload-box" onclick="document.getElementById('charVideo').click()">
                    <span>+</span>
                    <p>Upload Video</p>
                    <input type="file" id="charVideo" accept="video/*" style="display:none;" onchange="previewCharVideo(event)">
                    <div id="charVideoOverlay" class="video-overlay-text">Video Added ✓</div>
                </div>
            </div>

            <button onclick="processCharacterVideo()">Extract Character Appearance</button>
            <div id="charStatus"></div>
        </div>

        <h3 class="view-section-title">Active Video Folder (16:9)</h3>
        <div id="activeFolderContainer" class="folder-grid">
            </div>

        <h3 id="galleryTitle" class="view-section-title" style="display:none;">Extracted Folder Scenes Grid</h3>
        <div class="gallery-section">
            <div id="galleryGrid" class="gallery-grid">
                </div>
        </div>
    </main>

    <div id="downloadsPage">
        <div class="page-header">
            <h2 style="margin: 0;">My Downloads</h2>
            <button onclick="history.back()">Back to Studio</button>
        </div>
        <div id="downloadsGrid" class="gallery-grid"></div>
    </div>

    <div id="archiveViewPage">
        <div class="page-header">
            <h2 style="margin: 0;">Archived Extract Images</h2>
            <button onclick="history.back()">Back to Studio</button>
        </div>
        <div id="archivedFoldersGrid" class="folder-grid"></div>
        
        <h3 id="archiveGalleryTitle" class="view-section-title" style="display:none; margin-top:30px;">Archived Scenes Grid</h3>
        <div id="archiveGalleryGrid" class="gallery-grid"></div>
    </div>

    <div id="loginGatewayPage">
        <div class="page-header">
            <h2 style="margin: 0;">Account Gateway</h2>
            <button onclick="history.back()">Exit Gateway</button>
        </div>
        
        <div class="login-container">
            <h3 style="margin-top: 0; margin-bottom: 25px;">Sign In to VS Extractor</h3>
            
            <button class="login-method-btn btn-google" onclick="executeMockLogin('Google Account')">
                <span style="font-weight: bold; color: #4285F4;">G</span> Continue with Google
            </button>
            
            <div style="margin: 15px 0; color: gray; font-size: 13px;">OR</div>
            
            <div style="margin-bottom: 20px; text-align: left;">
                <label style="font-size: 12px; font-weight: bold; display: block; margin-bottom: 5px;">MOBILE AUTHENTICATION</label>
                <input type="tel" class="login-input" placeholder="+91 XXXXX XXXXX">
                <button class="login-method-btn btn-phone" onclick="executeMockLogin('Mobile Secure OTP')">Send Secure OTP</button>
            </div>

            <div style="text-align: left; border-top: 1px solid #ddd; padding-top: 15px;">
                <label style="font-size: 12px; font-weight: bold; display: block; margin-bottom: 5px;">EMAIL ACCESS PORTAL</label>
                <input type="email" class="login-input" placeholder="name@domain.com">
                <input type="password" class="login-input" placeholder="Enter password">
                <button class="login-method-btn btn-email" onclick="executeMockLogin('Email Portal')">Login via Email</button>
            </div>
        </div>
    </div>

    <div id="imageViewer">
        <div class="viewer-image-container">
            <img id="viewerImg" src="" alt="Viewed Image">
        </div>

        <div id="editMenu">
            <div class="edit-option" onclick="alert('Crop feature coming soon!')">✂️ Crop</div>
            <div class="edit-option" onclick="alert('Adjust feature coming soon!')">🎚️ Adjust</div>
            <div class="edit-option" onclick="alert('Filter feature coming soon!')">🎨 Filter</div>
            <div class="edit-option" onclick="alert('Text feature coming soon!')">🔤 Text</div>
            <div class="edit-option" onclick="alert('Watermark feature coming soon!')">©️ Watermark</div>
        </div>

        <div class="viewer-bottom">
            <div class="viewer-action" onclick="alert('Shared successfully!')">📤 Share</div>
            <div class="viewer-action" onclick="alert('Added to Favorites!')">❤️ Favorite</div>
            <div class="viewer-action" onclick="toggleEditMenu()">✏️ Edit</div>
            <div class="viewer-action" onclick="deleteFromViewer()">🗑️ Delete</div>
            <div class="viewer-action" onclick="alert('More options menu')">⋮ More</div>
        </div>
    </div>

    <video id="hiddenVideo" style="display: none;"></video>
    <canvas id="hiddenCanvas" style="display: none;"></canvas>
    <canvas id="compareCanvas" width="64" height="64" style="display: none;"></canvas>
    <img id="hiddenCharImage" style="display: none;">

    <script>
        // Execution App States
        let appData = {
            albums: { "Default": [] },
            downloads: [], 
            archivedFolders: [], 
            currentActiveFolder: null, 
            currentAlbum: "Default",
            darkMode: false,
            sidebarOpen: false,
            selectedImages: new Set(),
            characterColorData: null,
            
            // Selection track configuration states
            activeContextMode: "studio", // "studio" or "archive" tracking
            selectedArchiveFolderId: null 
        };

        // Handle Back History Engine for Android / Hardware Controls
        window.addEventListener('popstate', (event) => {
            const viewer = document.getElementById('imageViewer');
            const downloadsPage = document.getElementById('downloadsPage');
            const archivePage = document.getElementById('archiveViewPage');
            const loginPage = document.getElementById('loginGatewayPage');
            const mainContent = document.getElementById('mainContent');

            if (viewer.style.display === 'flex') {
                viewer.style.display = 'none';
                currentViewedIndex = -1;
            } 
            else if (event.state && event.state.page === 'downloads') {
                downloadsPage.style.display = 'block';
                mainContent.style.display = 'none';
                archivePage.style.display = 'none';
                loginPage.style.display = 'none';
            }
            else if (event.state && event.state.page === 'archive') {
                archivePage.style.display = 'block';
                mainContent.style.display = 'none';
                downloadsPage.style.display = 'none';
                loginPage.style.display = 'none';
            }
            else if (event.state && event.state.page === 'login') {
                loginPage.style.display = 'block';
                mainContent.style.display = 'none';
                downloadsPage.style.display = 'none';
                archivePage.style.display = 'none';
            }
            else {
                downloadsPage.style.display = 'none';
                archivePage.style.display = 'none';
                loginPage.style.display = 'none';
                mainContent.style.display = 'block';
            }
        });

        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                splash.style.opacity = '0';
                setTimeout(() => {
                    splash.style.visibility = 'hidden';
                    splash.style.display = 'none';
                }, 500);
            }, 1500);

            renderAlbums();
            refreshFolderDisplayBlock();
        });

        window.onclick = function(event) {
            if (!event.target.matches('.three-dot-icon')) {
                hideDropdownMenu();
            }
        }

        function toggleDropdownMenu(event) {
            event.stopPropagation();
            const menu = document.getElementById('threeDotDropdown');
            menu.style.display = (menu.style.display === 'block') ? 'none' : 'block';
        }

        function hideDropdownMenu() {
            const menu = document.getElementById('threeDotDropdown');
            if (menu) menu.style.display = 'none';
        }

        // Open Multi-Method Login Gateway Interface Page
        function openLoginGateway() {
            document.getElementById('loginGatewayPage').style.display = 'block';
            document.getElementById('mainContent').style.display = 'none';
            document.getElementById('downloadsPage').style.display = 'none';
            document.getElementById('archiveViewPage').style.display = 'none';
            if(appData.sidebarOpen) toggleSidebar();
            history.pushState({ page: 'login' }, "Sign In");
        }

        function executeMockLogin(methodString) {
            alert(`Authenticated successfully via ${methodString}!`);
            document.getElementById('customSignInBtn').style.display = 'none';
            const userUI = document.getElementById('userInfo');
            userUI.innerHTML = `<img src="https://via.placeholder.com/32" alt="User"> <span>Secure User</span>`;
            userUI.style.display = 'flex';
            history.back(); // Returns seamlessly to studio
        }

        // Sidebar Content View Mechanics
        function toggleSidebarSection(containerId, iconId) {
            const content = document.getElementById(containerId);
            const icon = document.getElementById(iconId);
            if(content.classList.contains('visible')) {
                content.classList.remove('visible');
                icon.innerHTML = "▼";
            } else {
                content.classList.add('visible');
                icon.innerHTML = "▲";
            }
        }

        function toggleSidebar() {
            const sidebar = document.getElementById('sidebar');
            const mainContent = document.getElementById('mainContent');
            if (appData.sidebarOpen) {
                sidebar.classList.remove('open');
                mainContent.style.marginLeft = "0";
                appData.sidebarOpen = false;
            } else {
                sidebar.classList.add('open');
                mainContent.style.marginLeft = "260px";
                appData.sidebarOpen = true;
            }
        }

        function toggleDarkMode() {
            appData.darkMode = !appData.darkMode;
            if(appData.darkMode) document.body.classList.add('dark-mode');
            else document.body.classList.remove('dark-mode');
        }

        // Header Actions Count Controller
        function updateHeaderLayoutState() {
            const count = appData.selectedImages.size;
            const appTitle = document.getElementById('appTitle');
            const normalView = document.getElementById('headerNormalView');
            const selectionView = document.getElementById('headerSelectionView');
            const countDisplay = document.getElementById('selectionCount');

            if (count > 0) {
                appTitle.style.opacity = '0';
                setTimeout(() => { if(appData.selectedImages.size > 0) appTitle.style.display = 'none'; }, 200);
                normalView.style.display = 'none';
                selectionView.style.display = 'flex';
                countDisplay.textContent = count;
            } else {
                appTitle.style.display = 'block';
                setTimeout(() => { if(appData.selectedImages.size === 0) appTitle.style.opacity = '1'; }, 10);
                normalView.style.display = 'flex';
                selectionView.style.display = 'none';
                hideDropdownMenu();
            }
        }

        // Album Architecture Mechanics
        function createNewAlbum() {
            const albumInput = document.getElementById('albumInput');
            const name = albumInput.value.trim();
            if(name === "") return alert("Please enter a valid album name!");
            if(appData.albums[name]) return alert("Album already exists!");
            appData.albums[name] = [];
            albumInput.value = "";
            renderAlbums();
        }

        function renderAlbums() {
            const listContainer = document.getElementById('albumList');
            listContainer.innerHTML = "";
            for(let albumName in appData.albums) {
                const li = document.createElement('li');
                li.textContent = albumName;
                if(albumName === appData.currentAlbum) li.classList.add('active');
                li.onclick = () => {
                    appData.currentAlbum = albumName;
                    appData.selectedImages.clear();
                    renderAlbums();
                    updateHeaderLayoutState();
                    refreshFolderDisplayBlock();
                };
                listContainer.appendChild(li);
            }
        }

        // Auto Archive Setup on New Video Additions
        function prepareNewFolderEnvironment(videoName) {
            if(appData.currentActiveFolder && appData.currentActiveFolder.frames.length > 0) {
                appData.archivedFolders.push(appData.currentActiveFolder);
            }
            appData.currentActiveFolder = {
                id: 'folder_' + Date.now(),
                name: videoName || "Video Extractions",
                frames: [],
                isExpanded: false
            };
            appData.selectedImages.clear();
            updateHeaderLayoutState();
            document.getElementById('galleryTitle').style.display = "none";
            document.getElementById('galleryGrid').innerHTML = "";
        }

        function openDownloadsPage() {
            document.getElementById('downloadsPage').style.display = 'block';
            document.getElementById('mainContent').style.display = 'none';
            document.getElementById('archiveViewPage').style.display = 'none';
            if(appData.sidebarOpen) toggleSidebar();
            renderDownloadsGrid();
            history.pushState({ page: 'downloads' }, "Downloads");
        }

        function openArchiveViewPage() {
            document.getElementById('archiveViewPage').style.display = 'block';
            document.getElementById('mainContent').style.display = 'none';
            document.getElementById('downloadsPage').style.display = 'none';
            if(appData.sidebarOpen) toggleSidebar();
            appData.activeContextMode = "archive"; 
            appData.selectedImages.clear();
            updateHeaderLayoutState();
            renderArchivedFoldersView();
            history.pushState({ page: 'archive' }, "Archive");
        }

        // Displays 16:9 Folder view interface inside Studio Section with bidirectional toggles
        function refreshFolderDisplayBlock() {
            const container = document.getElementById('activeFolderContainer');
            container.innerHTML = "";

            if(!appData.currentActiveFolder) {
                container.innerHTML = "<p style='color:gray; font-size:13px;'>No active video folder. Please upload a video to generate a workspace folder.</p>";
                return;
            }

            const folder = appData.currentActiveFolder;
            const card = document.createElement('div');
            card.className = "folder-card";
            
            // Adjust dynamic display arrows based on expansion states
            const arrowSymbol = folder.isExpanded ? "&uarr;" : "&darr;";

            card.innerHTML = `
                <div>
                    <p class="folder-title">📂 ${folder.name}</p>
                    <p class="folder-info" style="margin-top:5px;">Active Session Workspace</p>
                </div>
                <div class="process-icon-wrap" onclick="toggleActiveFolderExpansion(event)">
                    <div class="process-circle"></div>
                    <div class="process-arrow">${arrowSymbol}</div>
                </div>
                <p class="folder-info">${folder.frames.length} frames captured</p>
            `;
            container.appendChild(card);
        }

        // Toggle Expand and Collapse for Active Studio Folders
        function toggleActiveFolderExpansion(event) {
            event.stopPropagation();
            if(!appData.currentActiveFolder || appData.currentActiveFolder.frames.length === 0) {
                return alert("This active folder contains no extracted image scenes yet!");
            }
            
            appData.activeContextMode = "studio";
            appData.currentActiveFolder.isExpanded = !appData.currentActiveFolder.isExpanded;
            
            const titleElement = document.getElementById('galleryTitle');
            if(appData.currentActiveFolder.isExpanded) {
                titleElement.style.display = "block";
                renderGalleryGridEngine('galleryGrid', appData.currentActiveFolder.frames, false);
            } else {
                titleElement.style.display = "none";
                document.getElementById('galleryGrid').innerHTML = "";
                appData.selectedImages.clear();
                updateHeaderLayoutState();
            }
            refreshFolderDisplayBlock();
        }

        // Renders Grid layouts for archived folders inside Dedicated Page
        function renderArchivedFoldersView() {
            const container = document.getElementById('archivedFoldersGrid');
            container.innerHTML = "";

            if(appData.archivedFolders.length === 0) {
                container.innerHTML = "<p style='color:gray; padding:10px;'>No archived video extract folders found.</p>";
                return;
            }

            appData.archivedFolders.forEach((folder) => {
                const card = document.createElement('div');
                card.className = "folder-card";
                const arrowSymbol = folder.isExpanded ? "&uarr;" : "&darr;";

                card.innerHTML = `
                    <div>
                        <p class="folder-title"> Bertram 📁 ${folder.name}</p>
                        <p class="folder-info" style="margin-top:5px;">Archived Track</p>
                    </div>
                    <div class="process-icon-wrap" onclick="toggleArchivedFolderExpansion(event, '${folder.id}')">
                        <div class="process-circle"></div>
                        <div class="process-arrow">${arrowSymbol}</div>
                    </div>
                    <p class="folder-info">${folder.frames.length} images saved</p>
                `;
                container.appendChild(card);
            });
        }

        // Toggle Expand / Collapse with selection working on Archived sections
        function toggleArchivedFolderExpansion(event, folderId) {
            event.stopPropagation();
            const folder = appData.archivedFolders.find(f => f.id === folderId);
            if(!folder) return;

            appData.activeContextMode = "archive";
            appData.selectedArchiveFolderId = folderId;
            folder.isExpanded = !folder.isExpanded;

            // Close all other expanded grid sets to maintain selection scope context cleanly
            appData.archivedFolders.forEach(f => { if(f.id !== folderId) f.isExpanded = false; });

            const titleElement = document.getElementById('archiveGalleryTitle');
            if(folder.isExpanded) {
                titleElement.style.display = "block";
                renderGalleryGridEngine('archiveGalleryGrid', folder.frames, false); // Multi selection active
            } else {
                titleElement.style.display = "none";
                document.getElementById('archiveGalleryGrid').innerHTML = "";
                appData.selectedImages.clear();
                updateHeaderLayoutState();
            }
            renderArchivedFoldersView();
        }

        // Generic Core Structural Selection Engine Mapping 
        function renderGalleryGridEngine(targetGridId, framesArray, isFromArchive = false) {
            const grid = document.getElementById(targetGridId);
            grid.innerHTML = "";

            framesArray.forEach((imgDataUrl, index) => {
                const isSelected = appData.selectedImages.has(index);
                const card = document.createElement('div');
                card.className = 'image-card' + (isSelected ? ' selected' : '');

                const img = document.createElement('img');
                img.src = imgDataUrl;

                const checkTag = document.createElement('div');
                checkTag.className = 'selection-tag' + (isSelected ? ' active' : '');
                checkTag.innerHTML = '&#10004;'; 

                card.onclick = function() { 
                    toggleSelection(index, card, checkTag); 
                };

                const downloadBtn = document.createElement('button');
                downloadBtn.className = 'download-btn';
                downloadBtn.innerHTML = '⬇ Download';
                downloadBtn.onclick = function(e) {
                    e.stopPropagation(); 
                    downloadImage(imgDataUrl, `Scene_${index + 1}.jpg`);
                };

                card.appendChild(img);
                card.appendChild(checkTag);
                card.appendChild(downloadBtn);
                grid.appendChild(card);
            });
        }

        function renderDownloadsGrid() {
            const grid = document.getElementById('downloadsGrid');
            grid.innerHTML = "";
            if(appData.downloads.length === 0) {
                grid.innerHTML = "<p style='grid-column: 1 / -1; color: gray;'>No downloaded images found.</p>";
                return;
            }
            appData.downloads.forEach((imgDataUrl, index) => {
                const card = document.createElement('div');
                card.className = 'image-card';
                card.onclick = () => openImageViewer(imgDataUrl, index);
                const img = document.createElement('img');
                img.src = imgDataUrl;
                card.appendChild(img);
                grid.appendChild(card);
            });
        }

        // Full Screen Viewer Logic Engine
        let currentViewedIndex = -1; 

        function openImageViewer(imgSrc, index) {
            currentViewedIndex = index;
            document.getElementById('viewerImg').src = imgSrc;
            document.getElementById('imageViewer').style.display = 'flex';
            document.getElementById('editMenu').style.display = 'none';
            history.pushState({ page: 'viewer' }, "Image Viewer");
        }

        function toggleEditMenu() {
            const menu = document.getElementById('editMenu');
            const style = window.getComputedStyle(menu);
            menu.style.display = (style.display === 'none') ? 'flex' : 'none';
        }

        function deleteFromViewer() {
            if(currentViewedIndex > -1) {
                if(confirm("Are you sure you want to delete this specific download item?")) {
                    appData.downloads.splice(currentViewedIndex, 1);
                    history.back();
                    renderDownloadsGrid(); 
                }
            }
        }

        function formatTime(seconds) {
            let m = Math.floor(seconds / 60).toString().padStart(2, '0');
            let s = Math.floor(seconds % 60).toString().padStart(2, '0');
            return `${m}:${s}`;
        }

        function previewNormalVideo(event) {
            if(event.target.files[0]) {
                document.getElementById('normalVideoOverlay').style.display = "block";
            }
        }

        function isSceneDifferent(imgData1, imgData2) {
            if (!imgData2) return true;
            let diffCount = 0;
            const data1 = imgData1.data;
            const data2 = imgData2.data;
            const len = data1.length;
            for (let i = 0; i < len; i += 4) {
                const rDiff = Math.abs(data1[i] - data2[i]);
                const gDiff = Math.abs(data1[i+1] - data2[i+1]);
                const bDiff = Math.abs(data1[i+2] - data2[i+2]);
                if (rDiff + gDiff + bDiff > 40) { diffCount++; }
            }
            const totalPixels = len / 4;
            return ((diffCount / totalPixels) * 100) > 5.0;
        }

        async function processVideo() {
            const fileInput = document.getElementById('normalVideoFile');
            const statusDiv = document.getElementById('status');
            const timerDiv = document.getElementById('normalTimer');

            if(fileInput.files.length === 0) return alert("Please select a video file first!");
            const file = fileInput.files[0];
            
            prepareNewFolderEnvironment(file.name);
            refreshFolderDisplayBlock();

            const video = document.getElementById('hiddenVideo');
            const canvas = document.getElementById('hiddenCanvas');
            const ctx = canvas.getContext('2d');
            const compCanvas = document.getElementById('compareCanvas');
            const compCtx = compCanvas.getContext('2d', { willReadFrequently: true });

            video.src = URL.createObjectURL(file);
            await new Promise((resolve) => video.onloadedmetadata = () => resolve());

            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;

            let duration = video.duration;
            let currentTime = 0;
            let captureInterval = 1; 
            let lastImageData = null;
            timerDiv.style.display = "block";

            while(currentTime < duration) {
                video.currentTime = currentTime;
                await new Promise((resolve) => video.onseeked = () => resolve());
                timerDiv.textContent = formatTime(currentTime);

                compCtx.drawImage(video, 0, 0, 64, 64);
                let currentImageData = compCtx.getImageData(0, 0, 64, 64);

                if (isSceneDifferent(currentImageData, lastImageData)) {
                    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
                    let dataURL = canvas.toDataURL('image/jpeg');
                    appData.currentActiveFolder.frames.push(dataURL);
                    lastImageData = currentImageData;
                }
                currentTime += captureInterval;
                statusDiv.textContent = `Captured: ${appData.currentActiveFolder.frames.length} frames`;
                refreshFolderDisplayBlock();
            }
            statusDiv.textContent = `Finished! Extracted frames into active folder.`;
            fileInput.value = "";
            document.getElementById('normalVideoOverlay').style.display = "none";
        }

        // Smart Character Extract Match Optimized Strict Pipeline
        function previewCharPhoto(event) {
            const file = event.target.files[0];
            if(file) {
                const url = URL.createObjectURL(file);
                document.getElementById('charPhotoPreview').src = url;
                document.getElementById('charPhotoPreview').style.display = "block";
                document.getElementById('hiddenCharImage').src = url;
                document.getElementById('hiddenCharImage').onload = function() {
                    appData.characterColorData = extractImageSignature(this);
                };
            }
        }

        function previewCharVideo(event) {
            if(event.target.files[0]) {
                document.getElementById('charVideoOverlay').style.display = "block";
            }
        }

        function extractImageSignature(imgElement) {
            const tempCanvas = document.createElement('canvas');
            const tCtx = tempCanvas.getContext('2d');
            tempCanvas.width = 64; 
            tempCanvas.height = 64;
            tCtx.drawImage(imgElement, 0, 0, 64, 64);
            
            const imageData = tCtx.getImageData(0, 0, 64, 64).data;
            let colorBins = {};
            let totalPixels = 0;
            
            for (let i = 0; i < imageData.length; i += 16) { 
                let r = Math.floor(imageData[i] / 64);
                let g = Math.floor(imageData[i+1] / 64);
                let b = Math.floor(imageData[i+2] / 64);
                let key = `${r},${g},${b}`;
                colorBins[key] = (colorBins[key] || 0) + 1;
                totalPixels++;
            }
            return { bins: colorBins, total: totalPixels };
        }

        // Strict validation engine (checks if signature matching threshold passes high precision limits)
        function isCharacterInFrame(frameCtx, canvasWidth, canvasHeight, charData) {
            if(!charData) return false; 
            const imageData = frameCtx.getImageData(0, 0, canvasWidth, canvasHeight).data;
            let frameBins = {};
            let frameTotal = 0;

            for (let i = 0; i < imageData.length; i += 32) { 
                let r = Math.floor(imageData[i] / 64);
                let g = Math.floor(imageData[i+1] / 64);
                let b = Math.floor(imageData[i+2] / 64);
                let key = `${r},${g},${b}`;
                frameBins[key] = (frameBins[key] || 0) + 1;
                frameTotal++;
            }

            let matchScore = 0;
            for(let key in charData.bins) {
                if(frameBins[key]) {
                    let charRatio = charData.bins[key] / charData.total;
                    let frameRatio = frameBins[key] / frameTotal;
                    matchScore += Math.min(charRatio, frameRatio);
                }
            }
            // Strict high-pass barrier filter. Below 0.18 matching means target character is absent.
            return matchScore >= 0.18; 
        }

        async function processCharacterVideo() {
            const charFile = document.getElementById('charPhoto').files.length;
            const videoInput = document.getElementById('charVideo');
            const statusDiv = document.getElementById('charStatus');

            if(charFile === 0) return alert("Please upload a character signature photo first!");
            if(videoInput.files.length === 0) return alert("Please select a video asset file first!");

            statusDiv.textContent = "Scanning matching filters strict mode... please wait.";
            const file = videoInput.files[0];
            
            prepareNewFolderEnvironment("Smart Match: " + file.name);
            refreshFolderDisplayBlock();

            const video = document.getElementById('hiddenVideo');
            const canvas = document.getElementById('hiddenCanvas');
            const ctx = canvas.getContext('2d', { willReadFrequently: true });
            const compCanvas = document.getElementById('compareCanvas');
            const compCtx = compCanvas.getContext('2d', { willReadFrequently: true });

            video.src = URL.createObjectURL(file);
            await new Promise((resolve) => video.onloadedmetadata = () => resolve());

            canvas.width = 128; 
            canvas.height = 128;
            
            const fullCanvas = document.createElement('canvas');
            fullCanvas.width = video.videoWidth;
            fullCanvas.height = video.videoHeight;
            const fullCtx = fullCanvas.getContext('2d');

            let duration = video.duration;
            let currentTime = 0;
            let captureInterval = 0.5; 
            let lastImageData = null;

            while(currentTime < duration) {
                video.currentTime = currentTime;
                await new Promise((resolve) => video.onseeked = () => resolve());

                ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
                
                // STRICT CHECK ENGINE LAYER
                if(isCharacterInFrame(ctx, canvas.width, canvas.height, appData.characterColorData)) {
                    compCtx.drawImage(video, 0, 0, 64, 64);
                    let currentImageData = compCtx.getImageData(0, 0, 64, 64);
                    
                    if(isSceneDifferent(currentImageData, lastImageData)) {
                        fullCtx.drawImage(video, 0, 0, fullCanvas.width, fullCanvas.height);
                        let dataURL = fullCanvas.toDataURL('image/jpeg');
                        appData.currentActiveFolder.frames.push(dataURL);
                        lastImageData = currentImageData;
                        refreshFolderDisplayBlock();
                    }
                }
                currentTime += captureInterval;
            }
            statusDiv.textContent = `Scan complete! Saved ${appData.currentActiveFolder.frames.length} valid character appearance frames.`;
            videoInput.value = ""; 
            document.getElementById('charVideoOverlay').style.display = "none";
        }

        // Globalized Unified Selection Engine Routing Mechanics
        function toggleSelection(index, cardElement, checkTagElement) {
            if (appData.selectedImages.has(index)) {
                appData.selectedImages.delete(index);
                cardElement.classList.remove('selected');
                checkTagElement.classList.remove('active');
            } else {
                appData.selectedImages.add(index);
                cardElement.classList.add('selected');
                checkTagElement.classList.add('active');
            }
            updateHeaderLayoutState();
        }

        function selectAllEngine() {
            let activeFramesArray = [];
            if(appData.activeContextMode === "studio" && appData.currentActiveFolder) {
                activeFramesArray = appData.currentActiveFolder.frames;
            } else if(appData.activeContextMode === "archive" && appData.selectedArchiveFolderId) {
                let folder = appData.archivedFolders.find(f => f.id === appData.selectedArchiveFolderId);
                if(folder) activeFramesArray = folder.frames;
            }

            for(let i = 0; i < activeFramesArray.length; i++) appData.selectedImages.add(i);
            hideDropdownMenu();
            updateHeaderLayoutState();
            refreshActiveContextGrids();
        }

        function clearSelectionEngine() {
            appData.selectedImages.clear();
            hideDropdownMenu();
            updateHeaderLayoutState(); 
            refreshActiveContextGrids();
        }

        function deleteSelectedEngine() {
            const count = appData.selectedImages.size;
            if(count === 0) return;
            if(!confirm(`Are you sure you want to completely erase ${count} selected frames?`)) return;

            const sortedIndexes = Array.from(appData.selectedImages).sort((a, b) => b - a);

            if(appData.activeContextMode === "studio" && appData.currentActiveFolder) {
                sortedIndexes.forEach(index => appData.currentActiveFolder.frames.splice(index, 1));
            } else if(appData.activeContextMode === "archive" && appData.selectedArchiveFolderId) {
                let folder = appData.archivedFolders.find(f => f.id === appData.selectedArchiveFolderId);
                if(folder) sortedIndexes.forEach(index => folder.frames.splice(index, 1));
            }

            appData.selectedImages.clear();
            hideDropdownMenu();
            updateHeaderLayoutState();
            refreshActiveContextGrids();
        }

        function refreshActiveContextGrids() {
            if(appData.activeContextMode === "studio" && appData.currentActiveFolder) {
                refreshFolderDisplayBlock();
                if(appData.currentActiveFolder.isExpanded) {
                    renderGalleryGridEngine('galleryGrid', appData.currentActiveFolder.frames, false);
                }
            } else if(appData.activeContextMode === "archive" && appData.selectedArchiveFolderId) {
                renderArchivedFoldersView();
                let folder = appData.archivedFolders.find(f => f.id === appData.selectedArchiveFolderId);
                if(folder && folder.isExpanded) {
                    renderGalleryGridEngine('archiveGalleryGrid', folder.frames, false);
                }
            }
        }

        function downloadImage(dataUrl, filename) {
            const link = document.createElement('a');
            link.href = dataUrl;
            link.download = filename;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            appData.downloads.push(dataUrl);
            alert("Downloaded successfully!");
        }
    </script>
</body>
</html>
