<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ImageBook - Share Your Moments</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-blue: #1e90ff;
            --dark-blue: #0066cc;
            --light-blue: #e6f3ff;
            --accent-blue: #4da6ff;
            --gradient-start: #667eea;
            --gradient-end: #764ba2;
            --bg-dark: #0a1628;
            --bg-card: #1a2744;
            --text-primary: #ffffff;
            --text-secondary: #a0aec0;
            --success: #48bb78;
            --danger: #f56565;
            --warning: #ed8936;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, var(--bg-dark) 0%, #1a365d 100%);
            min-height: 100vh;
            color: var(--text-primary);
        }

        .animated-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: -1; overflow: hidden;
        }

        .animated-bg::before {
            content: ''; position: absolute; width: 200%; height: 200%;
            background: radial-gradient(circle at 20% 80%, rgba(30, 144, 255, 0.1) 0%, transparent 50%),
                        radial-gradient(circle at 80% 20%, rgba(102, 126, 234, 0.1) 0%, transparent 50%);
            animation: bgMove 20s ease-in-out infinite;
        }

        @keyframes bgMove {
            0%, 100% { transform: translate(0, 0); }
            50% { transform: translate(-5%, -5%); }
        }

        .particles { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: -1; }

        .particle {
            position: absolute; width: 4px; height: 4px;
            background: var(--primary-blue); border-radius: 50%; opacity: 0.3;
            animation: float 15s infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(100vh); opacity: 0; }
            10% { opacity: 0.3; }
            90% { opacity: 0.3; }
            100% { transform: translateY(-100vh); opacity: 0; }
        }

        .auth-container {
            display: flex; justify-content: center; align-items: center;
            min-height: 100vh; padding: 20px;
        }

        .auth-box {
            background: rgba(26, 39, 68, 0.95); backdrop-filter: blur(20px);
            border-radius: 24px; padding: 40px; width: 100%; max-width: 480px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
            animation: slideUp 0.6s ease-out;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .logo { text-align: center; margin-bottom: 30px; }

        .logo h1 {
            font-size: 2.5rem; font-weight: 800;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
            display: flex; align-items: center; justify-content: center; gap: 10px;
        }

        .logo p { color: var(--text-secondary); margin-top: 8px; font-size: 0.9rem; }

        .form-group { margin-bottom: 20px; position: relative; }

        .form-group label {
            display: block; margin-bottom: 8px; font-weight: 500;
            color: var(--text-secondary); font-size: 0.9rem;
        }

        .form-group input {
            width: 100%; padding: 14px 16px; padding-left: 45px;
            background: rgba(255, 255, 255, 0.05);
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px; color: var(--text-primary); font-size: 1rem;
            transition: all 0.3s ease;
        }

        .form-group input:focus {
            outline: none; border-color: var(--primary-blue);
            background: rgba(30, 144, 255, 0.1);
        }

        .form-group .icon {
            position: absolute; left: 16px; top: 43px;
            color: var(--text-secondary); pointer-events: none;
        }

        .avatar-upload {
            display: flex; align-items: center; gap: 20px; padding: 15px;
            background: rgba(255, 255, 255, 0.05); border-radius: 12px;
            border: 2px dashed rgba(255, 255, 255, 0.2); cursor: pointer;
        }

        .avatar-upload:hover { border-color: var(--primary-blue); }

        .avatar-preview {
            width: 70px; height: 70px; border-radius: 50%;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            display: flex; align-items: center; justify-content: center; overflow: hidden;
        }

        .avatar-preview img { width: 100%; height: 100%; object-fit: cover; }
        .avatar-preview i { font-size: 1.8rem; color: white; }
        .avatar-text h4 { font-weight: 600; margin-bottom: 4px; }
        .avatar-text p { font-size: 0.8rem; color: var(--text-secondary); }

        .btn {
            width: 100%; padding: 16px; border: none; border-radius: 12px;
            font-size: 1rem; font-weight: 600; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 10px;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary-blue), var(--dark-blue));
            color: white; box-shadow: 0 4px 15px rgba(30, 144, 255, 0.4);
        }

        .btn-primary:hover { transform: translateY(-2px); }

        .btn-secondary {
            background: transparent; color: var(--primary-blue);
            border: 2px solid var(--primary-blue);
        }

        .btn-secondary:hover { background: rgba(30, 144, 255, 0.1); }

        .divider {
            display: flex; align-items: center; margin: 25px 0; color: var(--text-secondary);
        }

        .divider::before, .divider::after {
            content: ''; flex: 1; height: 1px; background: rgba(255, 255, 255, 0.1);
        }

        .divider span { padding: 0 15px; font-size: 0.85rem; }

        .switch-auth { text-align: center; margin-top: 25px; color: var(--text-secondary); }

        .switch-auth a {
            color: var(--primary-blue); text-decoration: none; font-weight: 600; cursor: pointer;
        }

        .app-container { display: none; min-height: 100vh; }
        .app-container.active { display: block; }

        .header {
            background: rgba(26, 39, 68, 0.95); backdrop-filter: blur(20px);
            padding: 15px 30px; position: sticky; top: 0; z-index: 100;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header-content {
            max-width: 1400px; margin: 0 auto; display: flex;
            align-items: center; justify-content: space-between;
        }

        .header-logo h1 {
            font-size: 1.8rem; font-weight: 800;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
        }

        .header-user { display: flex; align-items: center; gap: 15px; }

        .user-info {
            display: flex; align-items: center; gap: 12px;
            padding: 8px 16px; background: rgba(255, 255, 255, 0.05); border-radius: 50px;
        }

        .user-avatar {
            width: 40px; height: 40px; border-radius: 50%;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            display: flex; align-items: center; justify-content: center; overflow: hidden;
        }

        .user-avatar img { width: 100%; height: 100%; object-fit: cover; }
        .user-avatar i { color: white; }
        .user-name { font-weight: 600; }

        .logout-btn {
            padding: 10px 20px; background: rgba(245, 101, 101, 0.2);
            color: var(--danger); border: none; border-radius: 10px;
            cursor: pointer; font-weight: 600;
        }

        .logout-btn:hover { background: var(--danger); color: white; }

        .nav-tabs {
            display: flex; justify-content: center; gap: 10px;
            padding: 20px; flex-wrap: wrap;
        }

        .nav-tab {
            padding: 12px 28px; background: rgba(255, 255, 255, 0.05);
            border: 2px solid transparent; border-radius: 50px;
            color: var(--text-secondary); font-weight: 600; cursor: pointer;
            display: flex; align-items: center; gap: 8px; transition: all 0.3s ease;
        }

        .nav-tab:hover { background: rgba(30, 144, 255, 0.1); color: var(--primary-blue); }

        .nav-tab.active {
            background: linear-gradient(135deg, var(--primary-blue), var(--dark-blue));
            color: white; box-shadow: 0 4px 15px rgba(30, 144, 255, 0.4);
        }

        .nav-tab .badge {
            background: var(--danger); color: white;
            padding: 2px 8px; border-radius: 20px; font-size: 0.75rem;
        }

        .main-content { max-width: 1200px; margin: 0 auto; padding: 20px; }
        .content-section { display: none; }
        .content-section.active { display: block; animation: fadeIn 0.4s ease; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .create-post-btn {
            display: flex; align-items: center; justify-content: center; gap: 10px;
            width: 100%; max-width: 600px; margin: 0 auto 30px; padding: 20px;
            background: linear-gradient(135deg, rgba(30, 144, 255, 0.2), rgba(102, 126, 234, 0.2));
            border: 2px dashed var(--primary-blue); border-radius: 16px;
            color: var(--primary-blue); font-size: 1.1rem; font-weight: 600; cursor: pointer;
        }

        .create-post-btn:hover { transform: scale(1.02); }

        .posts-grid {
            display: grid; grid-template-columns: repeat(auto-fill, minmax(350px, 1fr)); gap: 25px;
        }

        .post-card {
            background: rgba(26, 39, 68, 0.95); border-radius: 20px; overflow: hidden;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1); transition: all 0.3s ease;
        }

        .post-card:hover { transform: translateY(-5px); }

        .post-header {
            display: flex; align-items: center; gap: 12px;
            padding: 15px 20px; border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .post-author-avatar {
            width: 45px; height: 45px; border-radius: 50%;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            display: flex; align-items: center; justify-content: center; overflow: hidden;
        }

        .post-author-avatar img { width: 100%; height: 100%; object-fit: cover; }
        .post-author-info h4 { font-weight: 600; font-size: 1rem; }
        .post-author-info span { font-size: 0.8rem; color: var(--text-secondary); }

        .post-privacy {
            margin-left: auto; padding: 5px 12px; border-radius: 20px;
            font-size: 0.75rem; font-weight: 600;
        }

        .post-privacy.public { background: rgba(72, 187, 120, 0.2); color: var(--success); }
        .post-privacy.private { background: rgba(237, 137, 54, 0.2); color: var(--warning); }

        .post-title { padding: 15px 20px 10px; font-size: 1.2rem; font-weight: 600; }

        .post-media { position: relative; width: 100%; max-height: 400px; overflow: hidden; }

        .post-media img, .post-media video {
            width: 100%; height: auto; max-height: 400px; object-fit: cover;
        }

        .post-audio { padding: 15px 20px; background: rgba(0, 0, 0, 0.2); }
        .post-audio audio { width: 100%; }

        .post-actions {
            display: flex; gap: 10px; padding: 15px 20px; flex-wrap: wrap;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .post-action-btn {
            display: flex; align-items: center; gap: 6px;
            padding: 8px 15px; background: rgba(255, 255, 255, 0.05);
            border: none; border-radius: 20px; color: var(--text-secondary);
            cursor: pointer; transition: all 0.3s ease; font-size: 0.9rem;
        }

        .post-action-btn:hover { background: rgba(30, 144, 255, 0.2); color: var(--primary-blue); }
        .post-action-btn.liked { background: rgba(245, 101, 101, 0.2); color: var(--danger); }

        .people-grid {
            display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px;
        }

        .person-card {
            background: rgba(26, 39, 68, 0.95); border-radius: 16px;
            padding: 25px; text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .person-card:hover { transform: translateY(-5px); }

        .person-avatar {
            width: 90px; height: 90px; border-radius: 50%; margin: 0 auto 15px;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            display: flex; align-items: center; justify-content: center; overflow: hidden;
        }

        .person-avatar img { width: 100%; height: 100%; object-fit: cover; }
        .person-avatar i { font-size: 2.5rem; color: white; }
        .person-name { font-size: 1.2rem; font-weight: 600; margin-bottom: 5px; }
        .person-email { color: var(--text-secondary); font-size: 0.85rem; margin-bottom: 15px; }

        .person-actions { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }

        .action-btn {
            padding: 10px 20px; border-radius: 25px; font-weight: 600;
            font-size: 0.85rem; cursor: pointer; border: none;
            display: flex; align-items: center; gap: 6px; transition: all 0.3s ease;
        }

        .add-friend-btn {
            background: linear-gradient(135deg, var(--primary-blue), var(--dark-blue)); color: white;
        }

        .add-friend-btn:hover { transform: scale(1.05); }
        .add-friend-btn.added { background: var(--success); }

        .remove-friend-btn {
            background: rgba(245, 101, 101, 0.2); color: var(--danger);
        }

        .remove-friend-btn:hover { background: var(--danger); color: white; }

        .follow-btn {
            background: rgba(255, 255, 255, 0.1); color: var(--text-primary);
            border: 2px solid var(--primary-blue);
        }

        .follow-btn:hover { background: var(--primary-blue); }
        .follow-btn.following { background: var(--gradient-end); border-color: var(--gradient-end); }

        .modal-overlay {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.8); backdrop-filter: blur(10px);
            z-index: 1000; align-items: center; justify-content: center; padding: 20px;
        }

        .modal-overlay.active { display: flex; }

        .modal {
            background: var(--bg-card); border-radius: 24px;
            width: 100%; max-width: 600px; max-height: 90vh; overflow-y: auto;
            animation: modalSlide 0.3s ease;
        }

        @keyframes modalSlide {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        .modal-header {
            display: flex; align-items: center; justify-content: space-between;
            padding: 20px 25px; border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .modal-header h2 { font-size: 1.4rem; font-weight: 700; }

        .modal-close {
            width: 40px; height: 40px; border-radius: 50%;
            background: rgba(255, 255, 255, 0.1); border: none;
            color: var(--text-primary); font-size: 1.2rem; cursor: pointer;
        }

        .modal-close:hover { background: var(--danger); }

        .modal-body { padding: 25px; }

        .upload-area {
            padding: 30px; border: 2px dashed rgba(255, 255, 255, 0.2);
            border-radius: 16px; text-align: center; cursor: pointer;
            margin-bottom: 20px; transition: all 0.3s ease;
        }

        .upload-area:hover { border-color: var(--primary-blue); }
        .upload-area i { font-size: 3rem; color: var(--primary-blue); margin-bottom: 15px; }
        .upload-area h4 { font-weight: 600; margin-bottom: 5px; }
        .upload-area p { color: var(--text-secondary); font-size: 0.85rem; }

        .preview-area { margin-bottom: 20px; border-radius: 12px; overflow: hidden; }

        .preview-area img, .preview-area video {
            width: 100%; max-height: 300px; object-fit: contain; background: rgba(0, 0, 0, 0.3);
        }

        .privacy-toggle { display: flex; gap: 15px; margin-bottom: 20px; }

        .privacy-option {
            flex: 1; padding: 15px; border-radius: 12px;
            background: rgba(255, 255, 255, 0.05); border: 2px solid transparent;
            cursor: pointer; text-align: center;
        }

        .privacy-option:hover { background: rgba(255, 255, 255, 0.1); }
        .privacy-option.selected { border-color: var(--primary-blue); background: rgba(30, 144, 255, 0.2); }
        .privacy-option i { font-size: 1.5rem; margin-bottom: 8px; display: block; }
        .privacy-option.public i { color: var(--success); }
        .privacy-option.private i { color: var(--warning); }

        .section-header {
            display: flex; align-items: center; justify-content: space-between; margin-bottom: 25px;
        }

        .section-header h2 {
            font-size: 1.5rem; font-weight: 700;
            display: flex; align-items: center; gap: 10px;
        }

        .section-header h2 .count {
            background: var(--primary-blue); padding: 4px 12px;
            border-radius: 20px; font-size: 0.9rem;
        }

        .empty-state { text-align: center; padding: 60px 20px; color: var(--text-secondary); }
        .empty-state i { font-size: 4rem; margin-bottom: 20px; opacity: 0.3; }
        .empty-state h3 { font-size: 1.3rem; margin-bottom: 10px; color: var(--text-primary); }

        .reset-section {
            padding: 40px 20px; text-align: center;
            border-top: 1px solid rgba(255, 255, 255, 0.1); margin-top: 40px;
        }

        .reset-btn {
            padding: 15px 40px;
            background: linear-gradient(135deg, var(--danger), #c53030);
            color: white; border: none; border-radius: 12px;
            font-size: 1rem; font-weight: 600; cursor: pointer;
        }

        .reset-btn:hover { transform: scale(1.05); }

        .toast-container { position: fixed; bottom: 30px; right: 30px; z-index: 2000; }

        .toast {
            background: var(--bg-card); padding: 15px 25px; border-radius: 12px;
            margin-top: 10px; display: flex; align-items: center; gap: 12px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.4); animation: slideIn 0.3s ease;
            border-left: 4px solid var(--primary-blue);
        }

        .toast.success { border-left-color: var(--success); }
        .toast.error { border-left-color: var(--danger); }

        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        /* Comments Section */
        .comments-section {
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            padding: 15px 20px; display: none;
        }

        .comments-section.active { display: block; }

        .comments-list { max-height: 300px; overflow-y: auto; margin-bottom: 15px; }

        .comment-item {
            display: flex; gap: 12px; padding: 12px;
            background: rgba(255, 255, 255, 0.03); border-radius: 12px;
            margin-bottom: 10px;
        }

        .comment-avatar {
            width: 36px; height: 36px; border-radius: 50%; flex-shrink: 0;
            background: linear-gradient(135deg, var(--primary-blue), var(--gradient-end));
            display: flex; align-items: center; justify-content: center; overflow: hidden;
        }

        .comment-avatar img { width: 100%; height: 100%; object-fit: cover; }
        .comment-avatar i { color: white; font-size: 0.9rem; }

        .comment-content { flex: 1; }

        .comment-header {
            display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px;
        }

        .comment-author { font-weight: 600; font-size: 0.9rem; }
        .comment-time { font-size: 0.75rem; color: var(--text-secondary); }
        .comment-text { font-size: 0.9rem; line-height: 1.4; word-break: break-word; }

        .comment-input-area {
            display: flex; gap: 10px; align-items: flex-end;
        }

        .comment-input-wrapper {
            flex: 1; display: flex; flex-direction: column; gap: 8px;
        }

        .comment-input {
            width: 100%; padding: 12px 15px; background: rgba(255, 255, 255, 0.05);
            border: 2px solid rgba(255, 255, 255, 0.1); border-radius: 12px;
            color: var(--text-primary); font-size: 0.95rem; resize: none;
            font-family: inherit;
        }

        .comment-input:focus { outline: none; border-color: var(--primary-blue); }

        .emoji-picker-btn {
            padding: 8px 12px; background: rgba(255, 255, 255, 0.05);
            border: none; border-radius: 8px; color: var(--text-secondary);
            cursor: pointer; font-size: 1.2rem;
        }

        .emoji-picker-btn:hover { background: rgba(30, 144, 255, 0.2); }

        .emoji-panel {
            display: none; padding: 10px; background: var(--bg-card);
            border-radius: 12px; border: 1px solid rgba(255, 255, 255, 0.1);
            flex-wrap: wrap; gap: 5px; max-width: 300px;
        }

        .emoji-panel.active { display: flex; }

        .emoji-btn {
            padding: 8px; background: transparent; border: none;
            font-size: 1.4rem; cursor: pointer; border-radius: 8px;
        }

        .emoji-btn:hover { background: rgba(255, 255, 255, 0.1); }

        .send-comment-btn {
            padding: 12px 20px;
            background: linear-gradient(135deg, var(--primary-blue), var(--dark-blue));
            border: none; border-radius: 12px; color: white;
            font-weight: 600; cursor: pointer;
        }

        .send-comment-btn:hover { transform: scale(1.05); }

        .no-comments {
            text-align: center; padding: 20px; color: var(--text-secondary);
            font-style: italic;
        }

        @media (max-width: 768px) {
            .auth-box { padding: 25px; }
            .nav-tabs { gap: 8px; }
            .nav-tab { padding: 10px 16px; font-size: 0.85rem; }
            .posts-grid { grid-template-columns: 1fr; }
            .people-grid { grid-template-columns: 1fr; }
            .header-content { flex-direction: column; gap: 15px; }
            .person-actions { flex-direction: column; }
        }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: rgba(255, 255, 255, 0.05); }
        ::-webkit-scrollbar-thumb { background: var(--primary-blue); border-radius: 4px; }

        .hidden { display: none !important; }
    </style>
</head>
<body>
    <div class="animated-bg"></div>
    <div class="particles" id="particles"></div>
    <div class="toast-container" id="toastContainer"></div>

    <!-- Auth Container -->
    <div class="auth-container" id="authContainer">
        <div class="auth-box" id="loginBox">
            <div class="logo">
                <h1><i class="fas fa-images"></i> ImageBook</h1>
                <p>Share your moments with the world</p>
            </div>
            <form id="loginForm">
                <div class="form-group">
                    <label>Email Address</label>
                    <input type="email" id="loginEmail" placeholder="Enter your email" required>
                    <i class="fas fa-envelope icon"></i>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" id="loginPassword" placeholder="Enter your password" required>
                    <i class="fas fa-lock icon"></i>
                </div>
                <button type="submit" class="btn btn-primary">
                    <i class="fas fa-sign-in-alt"></i> Sign In
                </button>
            </form>
            <div class="divider"><span>Don't have an account?</span></div>
            <button type="button" class="btn btn-secondary" id="showRegisterBtn">
                <i class="fas fa-user-plus"></i> Create Account
            </button>
        </div>

        <div class="auth-box hidden" id="registerBox">
            <div class="logo">
                <h1><i class="fas fa-images"></i> ImageBook</h1>
                <p>Join our creative community</p>
            </div>
            <form id="registerForm">
                <div class="form-group">
                    <label>Full Name</label>
                    <input type="text" id="registerName" placeholder="Enter your full name" required>
                    <i class="fas fa-user icon"></i>
                </div>
                <div class="form-group">
                    <label>Email Address</label>
                    <input type="email" id="registerEmail" placeholder="Enter your email" required>
                    <i class="fas fa-envelope icon"></i>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <input type="password" id="registerPassword" placeholder="Create a password" required>
                    <i class="fas fa-lock icon"></i>
                </div>
                <div class="form-group">
                    <label>Confirm Password</label>
                    <input type="password" id="confirmPassword" placeholder="Confirm your password" required>
                    <i class="fas fa-lock icon"></i>
                </div>
                <div class="form-group">
                    <label>Date of Birth</label>
                    <input type="date" id="registerDob" required style="padding-left: 16px;">
                </div>
                <div class="form-group">
                    <label>Profile Avatar (Optional)</label>
                    <div class="avatar-upload" id="avatarUploadArea">
                        <div class="avatar-preview" id="avatarPreview">
                            <i class="fas fa-camera"></i>
                        </div>
                        <div class="avatar-text">
                            <h4>Upload Avatar</h4>
                            <p>Click to choose an image</p>
                        </div>
                    </div>
                    <input type="file" id="avatarInput" accept="image/*" style="display: none;">
                </div>
                <button type="submit" class="btn btn-primary">
                    <i class="fas fa-user-plus"></i> Create Account
                </button>
            </form>
            <div class="switch-auth">
                Already have an account? <a id="showLoginBtn">Sign In</a>
            </div>
        </div>
    </div>

    <!-- Main App Container -->
    <div class="app-container" id="appContainer">
        <header class="header">
            <div class="header-content">
                <div class="header-logo">
                    <h1><i class="fas fa-images"></i> ImageBook</h1>
                </div>
                <div class="header-user">
                    <div class="user-info">
                        <div class="user-avatar" id="headerAvatar">
                            <i class="fas fa-user"></i>
                        </div>
                        <span class="user-name" id="headerName">User</span>
                    </div>
                    <button class="logout-btn" id="logoutBtn">
                        <i class="fas fa-sign-out-alt"></i> Logout
                    </button>
                </div>
            </div>
        </header>

        <nav class="nav-tabs" id="navTabs">
            <button class="nav-tab active" data-tab="posts">
                <i class="fas fa-newspaper"></i> Posts
            </button>
            <button class="nav-tab" data-tab="people">
                <i class="fas fa-users"></i> People
            </button>
            <button class="nav-tab" data-tab="friends">
                <i class="fas fa-user-friends"></i> Friends
                <span class="badge hidden" id="friendsBadge">0</span>
            </button>
            <button class="nav-tab" data-tab="following">
                <i class="fas fa-user-plus"></i> Following
            </button>
            <button class="nav-tab" data-tab="followers">
                <i class="fas fa-heart"></i> Followers
            </button>
        </nav>

        <main class="main-content">
            <section class="content-section active" id="posts">
                <button class="create-post-btn" id="createPostBtn">
                    <i class="fas fa-plus-circle"></i> Create a New Post
                </button>
                <div class="posts-grid" id="postsGrid"></div>
                <div class="empty-state hidden" id="emptyPosts">
                    <i class="fas fa-camera"></i>
                    <h3>No posts yet</h3>
                    <p>Be the first to share something amazing!</p>
                </div>
            </section>

            <section class="content-section" id="people">
                <div class="section-header">
                    <h2><i class="fas fa-users"></i> Discover People</h2>
                </div>
                <div class="people-grid" id="peopleGrid"></div>
                <div class="empty-state hidden" id="emptyPeople">
                    <i class="fas fa-user-slash"></i>
                    <h3>No other users yet</h3>
                    <p>Invite your friends to join ImageBook!</p>
                </div>
            </section>

            <section class="content-section" id="friends">
                <div class="section-header">
                    <h2><i class="fas fa-user-friends"></i> My Friends <span class="count" id="friendsCount">0</span></h2>
                </div>
                <div class="people-grid" id="friendsGrid"></div>
                <div class="empty-state hidden" id="emptyFriends">
                    <i class="fas fa-user-friends"></i>
                    <h3>No friends yet</h3>
                    <p>Add some friends from the People section!</p>
                </div>
            </section>

            <section class="content-section" id="following">
                <div class="section-header">
                    <h2><i class="fas fa-user-plus"></i> Following <span class="count" id="followingCount">0</span></h2>
                </div>
                <div class="people-grid" id="followingGrid"></div>
                <div class="empty-state hidden" id="emptyFollowing">
                    <i class="fas fa-user-plus"></i>
                    <h3>Not following anyone</h3>
                    <p>Follow people to see their content!</p>
                </div>
            </section>

            <section class="content-section" id="followers">
                <div class="section-header">
                    <h2><i class="fas fa-heart"></i> Followers <span class="count" id="followersCount">0</span></h2>
                </div>
                <div class="people-grid" id="followersGrid"></div>
                <div class="empty-state hidden" id="emptyFollowers">
                    <i class="fas fa-heart"></i>
                    <h3>No followers yet</h3>
                    <p>Share great content to attract followers!</p>
                </div>
            </section>

            <div class="reset-section">
                <p style="margin-bottom: 15px; color: var(--text-secondary);">
                    <i class="fas fa-exclamation-triangle"></i> Warning: This will delete all data
                </p>
                <button class="reset-btn" id="resetBtn">
                    <i class="fas fa-trash-alt"></i> Reset All Data
                </button>
            </div>
        </main>
    </div>

    <!-- Create Post Modal -->
    <div class="modal-overlay" id="createPostModal">
        <div class="modal">
            <div class="modal-header">
                <h2><i class="fas fa-plus-circle"></i> Create New Post</h2>
                <button class="modal-close" id="closeModalBtn">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <div class="modal-body">
                <form id="createPostForm">
                    <div class="form-group">
                        <label>Post Title</label>
                        <input type="text" id="postTitle" placeholder="Give your post a title..." required style="padding-left: 16px;">
                    </div>
                    <div class="upload-area" id="mediaUploadArea">
                        <i class="fas fa-cloud-upload-alt"></i>
                        <h4>Upload Image or Video</h4>
                        <p>Click to select from your device</p>
                    </div>
                    <input type="file" id="mediaInput" accept="image/*,video/*" style="display: none;">
                    <div class="preview-area hidden" id="mediaPreview"></div>
                    <div class="upload-area hidden" id="audioUploadArea">
                        <i class="fas fa-music"></i>
                        <h4>Add Background Music (Optional)</h4>
                        <p>Only available for image posts</p>
                    </div>
                    <input type="file" id="audioInput" accept="audio/*" style="display: none;">
                    <div id="audioPreview" class="hidden" style="margin-bottom: 20px;">
                        <audio controls style="width: 100%;"></audio>
                    </div>
                    <label style="display: block; margin-bottom: 10px; font-weight: 500;">Post Visibility</label>
                    <div class="privacy-toggle">
                        <div class="privacy-option public selected" data-value="public">
                            <i class="fas fa-globe"></i>
                            <strong>Public</strong>
                            <p style="font-size: 0.8rem; color: var(--text-secondary); margin-top: 5px;">Everyone can see</p>
                        </div>
                        <div class="privacy-option private" data-value="private">
                            <i class="fas fa-lock"></i>
                            <strong>Private</strong>
                            <p style="font-size: 0.8rem; color: var(--text-secondary); margin-top: 5px;">Friends only</p>
                        </div>
                    </div>
                    <button type="submit" class="btn btn-primary">
                        <i class="fas fa-paper-plane"></i> Publish Post
                    </button>
                </form>
            </div>
        </div>
    </div>

    <script>
        let db;
        let currentUser = null;
        const DB_NAME = 'ImageBookDB';
        const DB_VERSION = 2;

        const EMOJIS = ['😀','😂','😍','🥰','😎','🤩','😢','😡','👍','👎','❤️','🔥','💯','🎉','👏','🙌','💪','🤔','😴','🥳','😇','🤗','😋','🤤','😏','😈','💀','👻','🎃','🦄'];

        function initDB() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open(DB_NAME, DB_VERSION);
                request.onerror = () => reject(request.error);
                request.onsuccess = () => { db = request.result; resolve(db); };
                request.onupgradeneeded = (event) => {
                    const database = event.target.result;
                    if (!database.objectStoreNames.contains('users')) {
                        const usersStore = database.createObjectStore('users', { keyPath: 'id', autoIncrement: true });
                        usersStore.createIndex('email', 'email', { unique: true });
                    }
                    if (!database.objectStoreNames.contains('posts')) {
                        const postsStore = database.createObjectStore('posts', { keyPath: 'id', autoIncrement: true });
                        postsStore.createIndex('userId', 'userId', { unique: false });
                    }
                    if (!database.objectStoreNames.contains('friends')) {
                        database.createObjectStore('friends', { keyPath: 'id', autoIncrement: true });
                    }
                    if (!database.objectStoreNames.contains('follows')) {
                        database.createObjectStore('follows', { keyPath: 'id', autoIncrement: true });
                    }
                    if (!database.objectStoreNames.contains('likes')) {
                        database.createObjectStore('likes', { keyPath: 'id', autoIncrement: true });
                    }
                    if (!database.objectStoreNames.contains('comments')) {
                        const commentsStore = database.createObjectStore('comments', { keyPath: 'id', autoIncrement: true });
                        commentsStore.createIndex('postId', 'postId', { unique: false });
                    }
                };
            });
        }

        function dbAdd(storeName, data) {
            return new Promise((resolve, reject) => {
                const tx = db.transaction([storeName], 'readwrite');
                const store = tx.objectStore(storeName);
                const req = store.add(data);
                req.onsuccess = () => resolve(req.result);
                req.onerror = () => reject(req.error);
            });
        }

        function dbGet(storeName, key) {
            return new Promise((resolve, reject) => {
                const tx = db.transaction([storeName], 'readonly');
                const req = tx.objectStore(storeName).get(key);
                req.onsuccess = () => resolve(req.result);
                req.onerror = () => reject(req.error);
            });
        }

        function dbGetAll(storeName) {
            return new Promise((resolve, reject) => {
                const tx = db.transaction([storeName], 'readonly');
                const req = tx.objectStore(storeName).getAll();
                req.onsuccess = () => resolve(req.result);
                req.onerror = () => reject(req.error);
            });
        }

        function dbDelete(storeName, key) {
            return new Promise((resolve, reject) => {
                const tx = db.transaction([storeName], 'readwrite');
                const req = tx.objectStore(storeName).delete(key);
                req.onsuccess = () => resolve();
                req.onerror = () => reject(req.error);
            });
        }

        function dbClear(storeName) {
            return new Promise((resolve, reject) => {
                const tx = db.transaction([storeName], 'readwrite');
                const req = tx.objectStore(storeName).clear();
                req.onsuccess = () => resolve();
                req.onerror = () => reject(req.error);
            });
        }

        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        function showToast(message, type = 'success') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            toast.className = `toast ${type}`;
            toast.innerHTML = `<i class="fas ${type === 'success' ? 'fa-check-circle' : 'fa-exclamation-circle'}"></i><span>${message}</span>`;
            container.appendChild(toast);
            setTimeout(() => { toast.remove(); }, 3000);
        }

        function createParticles() {
            const container = document.getElementById('particles');
            for (let i = 0; i < 20; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 15 + 's';
                particle.style.animationDuration = (Math.random() * 10 + 10) + 's';
                container.appendChild(particle);
            }
        }

        function showLogin() {
            document.getElementById('loginBox').classList.remove('hidden');
            document.getElementById('registerBox').classList.add('hidden');
        }

        function showRegister() {
            document.getElementById('loginBox').classList.add('hidden');
            document.getElementById('registerBox').classList.remove('hidden');
        }

        function logout() {
            currentUser = null;
            localStorage.removeItem('currentUserId');
            document.getElementById('authContainer').style.display = 'flex';
            document.getElementById('appContainer').classList.remove('active');
            document.getElementById('loginForm').reset();
            showToast('Logged out!', 'success');
        }

        async function showApp() {
            document.getElementById('authContainer').style.display = 'none';
            document.getElementById('appContainer').classList.add('active');
            document.getElementById('headerName').textContent = currentUser.name;
            if (currentUser.avatar) {
                document.getElementById('headerAvatar').innerHTML = `<img src="${currentUser.avatar}" alt="Avatar">`;
            }
            await loadPosts();
            await loadPeople();
            await loadFriends();
            await loadFollowing();
            await loadFollowers();
        }

        let selectedMedia = null, selectedAudio = null, mediaType = null;

        function openCreatePostModal() {
            document.getElementById('createPostModal').classList.add('active');
            resetPostForm();
        }

        function closeCreatePostModal() {
            document.getElementById('createPostModal').classList.remove('active');
            resetPostForm();
        }

        function resetPostForm() {
            document.getElementById('createPostForm').reset();
            document.getElementById('mediaPreview').classList.add('hidden');
            document.getElementById('mediaPreview').innerHTML = '';
            document.getElementById('audioUploadArea').classList.add('hidden');
            document.getElementById('audioPreview').classList.add('hidden');
            selectedMedia = null; selectedAudio = null; mediaType = null;
            document.querySelectorAll('.privacy-option').forEach(o => o.classList.remove('selected'));
            document.querySelector('.privacy-option.public').classList.add('selected');
            document.getElementById('mediaUploadArea').innerHTML = `<i class="fas fa-cloud-upload-alt"></i><h4>Upload Image or Video</h4><p>Click to select from your device</p>`;
        }

        function formatTime(dateString) {
            const date = new Date(dateString);
            const now = new Date();
            const diff = Math.floor((now - date) / 1000);
            if (diff < 60) return 'Just now';
            if (diff < 3600) return `${Math.floor(diff / 60)}m ago`;
            if (diff < 86400) return `${Math.floor(diff / 3600)}h ago`;
            return date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' });
        }

        async function loadPosts() {
            const posts = await dbGetAll('posts');
            const users = await dbGetAll('users');
            const friends = await dbGetAll('friends');
            const likes = await dbGetAll('likes');
            const comments = await dbGetAll('comments');

            const myFriends = friends.filter(f => f.userId === currentUser.id || f.friendId === currentUser.id)
                .map(f => f.userId === currentUser.id ? f.friendId : f.userId);

            const visiblePosts = posts.filter(post => {
                if (post.userId === currentUser.id) return true;
                if (post.privacy === 'public') return true;
                if (post.privacy === 'private' && myFriends.includes(post.userId)) return true;
                return false;
            }).sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));

            const grid = document.getElementById('postsGrid');
            const emptyState = document.getElementById('emptyPosts');

            if (visiblePosts.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = visiblePosts.map(post => {
                const author = users.find(u => u.id === post.userId) || { name: 'Unknown', avatar: null };
                const postLikes = likes.filter(l => l.postId === post.id);
                const postComments = comments.filter(c => c.postId === post.id);
                const userLiked = postLikes.some(l => l.userId === currentUser.id);
                const date = formatTime(post.createdAt);
                const isOwner = post.userId === currentUser.id;

                return `
                    <div class="post-card" data-post-id="${post.id}">
                        <div class="post-header">
                            <div class="post-author-avatar">
                                ${author.avatar ? `<img src="${author.avatar}" alt="${author.name}">` : '<i class="fas fa-user"></i>'}
                            </div>
                            <div class="post-author-info">
                                <h4>${author.name}</h4>
                                <span>${date}</span>
                            </div>
                            <span class="post-privacy ${post.privacy}">${post.privacy === 'public' ? '<i class="fas fa-globe"></i> Public' : '<i class="fas fa-lock"></i> Private'}</span>
                        </div>
                        <h3 class="post-title">${post.title}</h3>
                        <div class="post-media">
                            ${post.mediaType === 'video' ? `<video src="${post.media}" controls></video>` : `<img src="${post.media}" alt="${post.title}">`}
                        </div>
                        ${post.audio ? `<div class="post-audio"><audio src="${post.audio}" controls></audio></div>` : ''}
                        <div class="post-actions">
                            <button class="post-action-btn ${userLiked ? 'liked' : ''}" data-action="like" data-post-id="${post.id}">
                                <i class="fas fa-heart"></i> ${postLikes.length}
                            </button>
                            <button class="post-action-btn" data-action="toggle-comments" data-post-id="${post.id}">
                                <i class="fas fa-comment"></i> ${postComments.length} Comments
                            </button>
                        </div>
                        <div class="comments-section" id="comments-${post.id}">
                            <div class="comments-list" id="comments-list-${post.id}">
                                ${postComments.length === 0 ? '<p class="no-comments">No comments yet. Be the first!</p>' :
                                postComments.sort((a, b) => new Date(a.createdAt) - new Date(b.createdAt)).map(c => {
                                    const commenter = users.find(u => u.id === c.userId) || { name: 'Unknown', avatar: null };
                                    return `
                                        <div class="comment-item">
                                            <div class="comment-avatar">
                                                ${commenter.avatar ? `<img src="${commenter.avatar}" alt="">` : '<i class="fas fa-user"></i>'}
                                            </div>
                                            <div class="comment-content">
                                                <div class="comment-header">
                                                    <span class="comment-author">${commenter.name}</span>
                                                    <span class="comment-time">${formatTime(c.createdAt)}</span>
                                                </div>
                                                <p class="comment-text">${c.text}</p>
                                            </div>
                                        </div>
                                    `;
                                }).join('')}
                            </div>
                            <div class="comment-input-area">
                                <div class="comment-input-wrapper">
                                    <div class="emoji-panel" id="emoji-panel-${post.id}">
                                        ${EMOJIS.map(e => `<button type="button" class="emoji-btn" data-emoji="${e}">${e}</button>`).join('')}
                                    </div>
                                    <div style="display: flex; gap: 8px;">
                                        <button type="button" class="emoji-picker-btn" data-post-id="${post.id}">😀</button>
                                        <textarea class="comment-input" id="comment-input-${post.id}" placeholder="Write a comment..." rows="1"></textarea>
                                    </div>
                                </div>
                                <button class="send-comment-btn" data-post-id="${post.id}">
                                    <i class="fas fa-paper-plane"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');

            // Event listeners
            document.querySelectorAll('[data-action="like"]').forEach(btn => {
                btn.addEventListener('click', async () => {
                    const postId = parseInt(btn.dataset.postId);
                    await toggleLike(postId);
                });
            });

            document.querySelectorAll('[data-action="toggle-comments"]').forEach(btn => {
                btn.addEventListener('click', () => {
                    const postId = btn.dataset.postId;
                    const section = document.getElementById(`comments-${postId}`);
                    section.classList.toggle('active');
                });
            });

            document.querySelectorAll('.emoji-picker-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    const postId = btn.dataset.postId;
                    const panel = document.getElementById(`emoji-panel-${postId}`);
                    panel.classList.toggle('active');
                });
            });

            document.querySelectorAll('.emoji-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    e.preventDefault();
                    const emoji = btn.dataset.emoji;
                    const panel = btn.closest('.emoji-panel');
                    const postId = panel.id.replace('emoji-panel-', '');
                    const input = document.getElementById(`comment-input-${postId}`);
                    input.value += emoji;
                    panel.classList.remove('active');
                    input.focus();
                });
            });

            document.querySelectorAll('.send-comment-btn').forEach(btn => {
                btn.addEventListener('click', async () => {
                    const postId = parseInt(btn.dataset.postId);
                    const input = document.getElementById(`comment-input-${postId}`);
                    const text = input.value.trim();
                    if (text) {
                        await addComment(postId, text);
                        input.value = '';
                    }
                });
            });

            document.querySelectorAll('.comment-input').forEach(input => {
                input.addEventListener('keypress', async (e) => {
                    if (e.key === 'Enter' && !e.shiftKey) {
                        e.preventDefault();
                        const postId = parseInt(input.id.replace('comment-input-', ''));
                        const text = input.value.trim();
                        if (text) {
                            await addComment(postId, text);
                            input.value = '';
                        }
                    }
                });
            });
        }

        async function toggleLike(postId) {
            const likes = await dbGetAll('likes');
            const existing = likes.find(l => l.postId === postId && l.userId === currentUser.id);
            if (existing) {
                await dbDelete('likes', existing.id);
            } else {
                await dbAdd('likes', { postId, userId: currentUser.id });
            }
            await loadPosts();
        }

        async function addComment(postId, text) {
            await dbAdd('comments', {
                postId,
                userId: currentUser.id,
                text,
                createdAt: new Date().toISOString()
            });
            showToast('Comment added!', 'success');
            await loadPosts();
        }

        async function loadPeople() {
            const users = await dbGetAll('users');
            const friends = await dbGetAll('friends');
            const follows = await dbGetAll('follows');

            const otherUsers = users.filter(u => u.id !== currentUser.id);
            const grid = document.getElementById('peopleGrid');
            const emptyState = document.getElementById('emptyPeople');

            if (otherUsers.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = otherUsers.map(user => {
                const isFriend = friends.some(f =>
                    (f.userId === currentUser.id && f.friendId === user.id) ||
                    (f.friendId === currentUser.id && f.userId === user.id)
                );
                const isFollowing = follows.some(f => f.followerId === currentUser.id && f.followingId === user.id);

                return `
                    <div class="person-card">
                        <div class="person-avatar">
                            ${user.avatar ? `<img src="${user.avatar}" alt="${user.name}">` : '<i class="fas fa-user"></i>'}
                        </div>
                        <h3 class="person-name">${user.name}</h3>
                        <p class="person-email">${user.email}</p>
                        <div class="person-actions">
                            ${isFriend ? `
                                <button class="action-btn remove-friend-btn" data-user-id="${user.id}" data-action="unfriend">
                                    <i class="fas fa-user-minus"></i> Unfriend
                                </button>
                            ` : `
                                <button class="action-btn add-friend-btn" data-user-id="${user.id}" data-action="friend">
                                    <i class="fas fa-user-plus"></i> Add Friend
                                </button>
                            `}
                            <button class="action-btn follow-btn ${isFollowing ? 'following' : ''}" data-user-id="${user.id}" data-action="follow">
                                <i class="fas ${isFollowing ? 'fa-check' : 'fa-rss'}"></i>
                                ${isFollowing ? 'Following' : 'Follow'}
                            </button>
                        </div>
                    </div>
                `;
            }).join('');

            document.querySelectorAll('#peopleGrid .action-btn').forEach(btn => {
                btn.addEventListener('click', async () => {
                    const userId = parseInt(btn.dataset.userId);
                    const action = btn.dataset.action;
                    if (action === 'friend') await toggleFriend(userId);
                    else if (action === 'unfriend') await removeFriend(userId);
                    else if (action === 'follow') await toggleFollow(userId);
                });
            });
        }

        async function toggleFriend(userId) {
            const friends = await dbGetAll('friends');
            const existing = friends.find(f =>
                (f.userId === currentUser.id && f.friendId === userId) ||
                (f.friendId === currentUser.id && f.userId === userId)
            );
            if (!existing) {
                await dbAdd('friends', { userId: currentUser.id, friendId: userId });
                showToast('Friend added!', 'success');
            }
            await loadPeople();
            await loadFriends();
            await loadPosts();
        }

        async function removeFriend(userId) {
            const friends = await dbGetAll('friends');
            const existing = friends.find(f =>
                (f.userId === currentUser.id && f.friendId === userId) ||
                (f.friendId === currentUser.id && f.userId === userId)
            );
            if (existing) {
                await dbDelete('friends', existing.id);
                showToast('Friend removed!', 'success');
            }
            await loadPeople();
            await loadFriends();
            await loadPosts();
        }

        async function toggleFollow(userId) {
            const follows = await dbGetAll('follows');
            const existing = follows.find(f => f.followerId === currentUser.id && f.followingId === userId);
            if (existing) {
                await dbDelete('follows', existing.id);
                showToast('Unfollowed!', 'success');
            } else {
                await dbAdd('follows', { followerId: currentUser.id, followingId: userId });
                showToast('Following!', 'success');
            }
            await loadPeople();
            await loadFollowing();
            await loadFollowers();
        }

        async function loadFriends() {
            const users = await dbGetAll('users');
            const friends = await dbGetAll('friends');

            const myFriendIds = friends
                .filter(f => f.userId === currentUser.id || f.friendId === currentUser.id)
                .map(f => f.userId === currentUser.id ? f.friendId : f.userId);

            const uniqueFriendIds = [...new Set(myFriendIds)];
            const myFriends = users.filter(u => uniqueFriendIds.includes(u.id));

            document.getElementById('friendsCount').textContent = myFriends.length;
            const badge = document.getElementById('friendsBadge');
            if (myFriends.length > 0) {
                badge.classList.remove('hidden');
                badge.textContent = myFriends.length;
            } else {
                badge.classList.add('hidden');
            }

            const grid = document.getElementById('friendsGrid');
            const emptyState = document.getElementById('emptyFriends');

            if (myFriends.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = myFriends.map(user => `
                <div class="person-card">
                    <div class="person-avatar">
                        ${user.avatar ? `<img src="${user.avatar}" alt="${user.name}">` : '<i class="fas fa-user"></i>'}
                    </div>
                    <h3 class="person-name">${user.name}</h3>
                    <p class="person-email">${user.email}</p>
                    <div class="person-actions">
                        <button class="action-btn remove-friend-btn" data-user-id="${user.id}">
                            <i class="fas fa-user-minus"></i> Unfriend
                        </button>
                    </div>
                </div>
            `).join('');

            document.querySelectorAll('#friendsGrid .remove-friend-btn').forEach(btn => {
                btn.addEventListener('click', async () => {
                    const userId = parseInt(btn.dataset.userId);
                    await removeFriend(userId);
                });
            });
        }

        async function loadFollowing() {
            const users = await dbGetAll('users');
            const follows = await dbGetAll('follows');

            const followingIds = follows.filter(f => f.followerId === currentUser.id).map(f => f.followingId);
            const following = users.filter(u => followingIds.includes(u.id));

            document.getElementById('followingCount').textContent = following.length;
            const grid = document.getElementById('followingGrid');
            const emptyState = document.getElementById('emptyFollowing');

            if (following.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = following.map(user => `
                <div class="person-card">
                    <div class="person-avatar">
                        ${user.avatar ? `<img src="${user.avatar}" alt="${user.name}">` : '<i class="fas fa-user"></i>'}
                    </div>
                    <h3 class="person-name">${user.name}</h3>
                    <p class="person-email">${user.email}</p>
                    <div class="person-actions">
                        <button class="action-btn follow-btn following" data-user-id="${user.id}">
                            <i class="fas fa-check"></i> Following
                        </button>
                    </div>
                </div>
            `).join('');

            document.querySelectorAll('#followingGrid .follow-btn').forEach(btn => {
                btn.addEventListener('click', async () => {
                    await toggleFollow(parseInt(btn.dataset.userId));
                });
            });
        }

        async function loadFollowers() {
            const users = await dbGetAll('users');
            const follows = await dbGetAll('follows');

            const followerIds = follows.filter(f => f.followingId === currentUser.id).map(f => f.followerId);
            const followers = users.filter(u => followerIds.includes(u.id));

            document.getElementById('followersCount').textContent = followers.length;
            const grid = document.getElementById('followersGrid');
            const emptyState = document.getElementById('emptyFollowers');

            if (followers.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = followers.map(user => {
                const isFollowingBack = follows.some(f => f.followerId === currentUser.id && f.followingId === user.id);
                return `
                    <div class="person-card">
                        <div class="person-avatar">
                            ${user.avatar ? `<img src="${user.avatar}" alt="${user.name}">` : '<i class="fas fa-user"></i>'}
                        </div>
                        <h3 class="person-name">${user.name}</h3>
                        <p class="person-email">${user.email}</p>
                        <div class="person-actions">
                            <button class="action-btn follow-btn ${isFollowingBack ? 'following' : ''}" data-user-id="${user.id}">
                                <i class="fas ${isFollowingBack ? 'fa-check' : 'fa-rss'}"></i>
                                ${isFollowingBack ? 'Following' : 'Follow Back'}
                            </button>
                        </div>
                    </div>
                `;
            }).join('');

            document.querySelectorAll('#followersGrid .follow-btn').forEach(btn => {
                btn.addEventListener('click', async () => {
                    await toggleFollow(parseInt(btn.dataset.userId));
                });
            });
        }

        async function resetAllData() {
            if (confirm('Are you sure you want to delete ALL data? This cannot be undone!')) {
                try {
                    await dbClear('users');
                    await dbClear('posts');
                    await dbClear('friends');
                    await dbClear('follows');
                    await dbClear('likes');
                    await dbClear('comments');
                    localStorage.removeItem('currentUserId');
                    currentUser = null;
                    showToast('All data has been reset!', 'success');
                    document.getElementById('authContainer').style.display = 'flex';
                    document.getElementById('appContainer').classList.remove('active');
                    showLogin();
                } catch (error) {
                    showToast('Error resetting data.', 'error');
                }
            }
        }

        document.addEventListener('DOMContentLoaded', async function() {
            createParticles();
            await initDB();

            document.getElementById('showRegisterBtn').addEventListener('click', showRegister);
            document.getElementById('showLoginBtn').addEventListener('click', showLogin);
            document.getElementById('logoutBtn').addEventListener('click', logout);

            document.getElementById('avatarUploadArea').addEventListener('click', () => {
                document.getElementById('avatarInput').click();
            });

            document.getElementById('avatarInput').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        document.getElementById('avatarPreview').innerHTML = `<img src="${e.target.result}" alt="Avatar">`;
                    };
                    reader.readAsDataURL(file);
                }
            });

            document.getElementById('registerForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                const name = document.getElementById('registerName').value.trim();
                const email = document.getElementById('registerEmail').value.trim();
                const password = document.getElementById('registerPassword').value;
                const confirmPass = document.getElementById('confirmPassword').value;
                const dob = document.getElementById('registerDob').value;
                const avatarFile = document.getElementById('avatarInput').files[0];

                if (password !== confirmPass) { showToast('Passwords do not match!', 'error'); return; }
                if (password.length < 6) { showToast('Password must be at least 6 characters!', 'error'); return; }

                const users = await dbGetAll('users');
                if (users.find(u => u.email === email)) { showToast('Email already registered!', 'error'); return; }

                let avatar = null;
                if (avatarFile) avatar = await fileToBase64(avatarFile);

                try {
                    await dbAdd('users', { name, email, password, dob, avatar, createdAt: new Date().toISOString() });
                    showToast('Account created! Please sign in.', 'success');
                    showLogin();
                    document.getElementById('registerForm').reset();
                    document.getElementById('avatarPreview').innerHTML = '<i class="fas fa-camera"></i>';
                } catch (error) {
                    showToast('Error creating account.', 'error');
                }
            });

            document.getElementById('loginForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                const email = document.getElementById('loginEmail').value.trim();
                const password = document.getElementById('loginPassword').value;

                const users = await dbGetAll('users');
                const user = users.find(u => u.email === email && u.password === password);

                if (user) {
                    currentUser = user;
                    localStorage.setItem('currentUserId', user.id);
                    showToast(`Welcome, ${user.name}!`, 'success');
                    showApp();
                } else {
                    showToast('Invalid email or password!', 'error');
                }
            });

            document.querySelectorAll('.nav-tab').forEach(tab => {
                tab.addEventListener('click', function() {
                    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
                    this.classList.add('active');
                    document.getElementById(this.dataset.tab).classList.add('active');
                });
            });

            document.getElementById('createPostBtn').addEventListener('click', openCreatePostModal);
            document.getElementById('closeModalBtn').addEventListener('click', closeCreatePostModal);

            document.getElementById('createPostModal').addEventListener('click', function(e) {
                if (e.target === this) closeCreatePostModal();
            });

            document.getElementById('mediaUploadArea').addEventListener('click', () => {
                document.getElementById('mediaInput').click();
            });

            document.getElementById('mediaInput').addEventListener('change', async function(e) {
                const file = e.target.files[0];
                if (file) {
                    selectedMedia = await fileToBase64(file);
                    mediaType = file.type.startsWith('video') ? 'video' : 'image';
                    const preview = document.getElementById('mediaPreview');
                    preview.classList.remove('hidden');
                    if (mediaType === 'video') {
                        preview.innerHTML = `<video src="${selectedMedia}" controls></video>`;
                        document.getElementById('audioUploadArea').classList.add('hidden');
                    } else {
                        preview.innerHTML = `<img src="${selectedMedia}" alt="Preview">`;
                        document.getElementById('audioUploadArea').classList.remove('hidden');
                    }
                    document.getElementById('mediaUploadArea').innerHTML = `<i class="fas fa-check-circle" style="color: var(--success);"></i><h4>Media Selected</h4><p>Click to change</p>`;
                }
            });

            document.getElementById('audioUploadArea').addEventListener('click', () => {
                document.getElementById('audioInput').click();
            });

            document.getElementById('audioInput').addEventListener('change', async function(e) {
                const file = e.target.files[0];
                if (file) {
                    selectedAudio = await fileToBase64(file);
                    const audioPreview = document.getElementById('audioPreview');
                    audioPreview.classList.remove('hidden');
                    audioPreview.querySelector('audio').src = selectedAudio;
                    document.getElementById('audioUploadArea').innerHTML = `<i class="fas fa-check-circle" style="color: var(--success);"></i><h4>Music Selected</h4><p>Click to change</p>`;
                }
            });

            document.querySelectorAll('.privacy-option').forEach(option => {
                option.addEventListener('click', function() {
                    document.querySelectorAll('.privacy-option').forEach(o => o.classList.remove('selected'));
                    this.classList.add('selected');
                });
            });

            document.getElementById('createPostForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                if (!selectedMedia) { showToast('Please select an image or video!', 'error'); return; }

                const title = document.getElementById('postTitle').value.trim();
                const privacy = document.querySelector('.privacy-option.selected').dataset.value;

                try {
                    await dbAdd('posts', {
                        userId: currentUser.id,
                        title,
                        media: selectedMedia,
                        mediaType,
                        audio: mediaType === 'image' ? selectedAudio : null,
                        privacy,
                        createdAt: new Date().toISOString()
                    });
                    showToast('Post published!', 'success');
                    closeCreatePostModal();
                    await loadPosts();
                } catch (error) {
                    showToast('Error publishing post.', 'error');
                }
            });

            document.getElementById('resetBtn').addEventListener('click', resetAllData);

            const savedUserId = localStorage.getItem('currentUserId');
            if (savedUserId) {
                const user = await dbGet('users', parseInt(savedUserId));
                if (user) { currentUser = user; showApp(); }
            }
        });
    </script>

<script>
/**
 * Iframe 元素高亮注入脚本
 * 需要在目标网站中引入此脚本来支持跨域 iframe 高亮功能
 *
 * 使用方法：
 * 1. 将此脚本添加到目标网站的 HTML 中
 * 2. 或通过浏览器扩展、用户脚本等方式注入
 */

(function () {
  "use strict";

  // 检查是否在 iframe 中
  if (window.self === window.top) {
    return; // 不在 iframe 中，不执行
  }

  // 检查是否已经初始化过
  if (window.__iframeHighlightInitialized) {
    return;
  }
  window.__iframeHighlightInitialized = true;
  console.log("Iframe 高亮脚本已加载");

  // 创建高亮覆盖层
  var overlay = document.createElement("div");
  overlay.id = "iframe-highlight-overlay";
  overlay.style.cssText = "\n    position: fixed;\n    top: 0;\n    left: 0;\n    width: 100vw;\n    height: 100vh;\n    pointer-events: none;\n    z-index: 999999;\n    overflow: hidden;\n  ";

  // 创建悬停高亮框（虚线边框）
  var highlightBox = document.createElement("div");
  highlightBox.id = "iframe-highlight-box";
  highlightBox.style.cssText = "\n    position: absolute;\n    border: 2px dashed #007AFF;\n    background: rgba(0, 122, 255, 0.08);\n    pointer-events: none;\n    display: none;\n    transition: all 0.1s ease;\n    box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.8);\n    border-radius: 2px;\n  ";

  // 创建选中节点的常驻高亮框（实线边框）
  var selectedBox = document.createElement("div");
  selectedBox.id = "iframe-selected-box";
  selectedBox.style.cssText = "\n    position: absolute;\n    border: 2px solid #007AFF;\n    pointer-events: none;\n    display: none;\n    box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.9), 0 0 8px rgba(255, 107, 53, 0.4);\n    border-radius: 2px;\n    z-index: 1000000;\n  ";

  // 创建悬停标签显示
  var tagLabel = document.createElement("div");
  tagLabel.id = "iframe-tag-label";
  tagLabel.style.cssText = "\n    position: absolute;\n    background: #007AFF;\n    color: white;\n    padding: 2px 6px;\n    font-size: 11px;\n    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;\n    border-radius: 2px;\n    pointer-events: none;\n    display: none;\n    white-space: nowrap;\n    z-index: 1000001;\n    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);\n    font-weight: 500;\n  ";

  // 创建选中节点标签
  var selectedLabel = document.createElement("div");
  selectedLabel.id = "iframe-selected-label";
  selectedLabel.style.cssText = "\n    position: absolute;\n    background: #007AFF;\n    color: white;\n    padding: 3px 8px;\n    font-size: 11px;\n    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;\n    border-radius: 3px;\n    pointer-events: none;\n    display: none;\n    white-space: nowrap;\n    z-index: 1000002;\n    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.4);\n    font-weight: 600;\n  ";
  overlay.appendChild(highlightBox);
  overlay.appendChild(selectedBox);
  overlay.appendChild(tagLabel);
  overlay.appendChild(selectedLabel);
  document.body.appendChild(overlay);

  // 存储当前选中的元素
  var selectedElement = null;
  var highlightEnabled = false;

  // 更新选中元素的高亮显示
  function updateSelectedHighlight(element) {
    console.log("updateSelectedHighlight called with:", element);
    if (!element) {
      selectedBox.style.display = "none";
      selectedLabel.style.display = "none";
      selectedElement = null;
      console.log("Cleared selected highlight");
      return;
    }
    selectedElement = element;
    var rect = element.getBoundingClientRect();
    console.log("Selected element rect:", rect);

    // 更新选中高亮框位置
    selectedBox.style.display = "block";
    selectedBox.style.left = "".concat(rect.left - 2, "px");
    selectedBox.style.top = "".concat(rect.top - 2, "px");
    selectedBox.style.width = "".concat(rect.width + 4, "px");
    selectedBox.style.height = "".concat(rect.height + 4, "px");

    // 更新选中标签位置和内容
    selectedLabel.style.display = "block";
    selectedLabel.textContent = "\u2713 <".concat(element.tagName.toLowerCase(), ">");

    // 计算标签位置，确保不超出视窗
    var labelTop = rect.top - 28;
    var labelLeft = rect.left;

    // 如果标签会超出顶部，显示在元素下方
    if (labelTop < 5) {
      labelTop = rect.bottom + 5;
    }

    // 如果标签会超出右侧，向左调整
    var labelWidth = selectedLabel.offsetWidth || 100; // 预估宽度
    if (labelLeft + labelWidth > window.innerWidth - 10) {
      labelLeft = window.innerWidth - labelWidth - 10;
    }
    selectedLabel.style.left = "".concat(Math.max(5, labelLeft), "px");
    selectedLabel.style.top = "".concat(labelTop, "px");
    console.log("Selected highlight positioned at:", {
      left: selectedBox.style.left,
      top: selectedBox.style.top,
      width: selectedBox.style.width,
      height: selectedBox.style.height
    });
  }
  function getElementSelector(element) {
    if (!(element instanceof Element)) throw new Error('Argument must be a DOM element');
    var segments = [];
    var current = element;
    while (current !== document.documentElement) {
      var selector = '';
      // 优先检查唯一ID
      if (current.id && document.querySelectorAll("#".concat(current.id)).length === 1) {
        segments.unshift("#".concat(current.id));
        break; // ID唯一，无需继续向上
      }

      // 生成类名选择器（取第一个有效类名）
      var classes = Array.from(current.classList).filter(function (c) {
        return !c.startsWith('js-');
      });
      var className = classes.length > 0 ? ".".concat(classes[0]) : '';

      // 生成位置索引（nth-child）
      var tag = current.tagName.toLowerCase();
      if (!className) {
        var siblings = Array.from(current.parentNode.children);
        var index = siblings.findIndex(function (el) {
          return el === current;
        }) + 1;
        selector = "".concat(tag, ":nth-child(").concat(index, ")");
      } else {
        selector = className;
      }
      segments.unshift(selector);
      current = current.parentElement;
    }

    // 处理根元素
    if (current === document.documentElement) {
      segments.unshift('html');
    }
    return segments.join(' > ');
  }

  // 获取元素文本内容
  function getElementText(element) {
    var _element$textContent;
    if (element.tagName === "INPUT") {
      return element.value || element.placeholder || "";
    }
    if (element.tagName === "TEXTAREA") {
      return element.value || element.placeholder || "";
    }
    var text = ((_element$textContent = element.textContent) === null || _element$textContent === void 0 ? void 0 : _element$textContent.trim()) || "";
    return text.length > 50 ? text.substring(0, 50) + "..." : text;
  }

  // 获取元素属性信息
  function getElementAttributes(element) {
    var attrs = {};
    for (var i = 0; i < element.attributes.length; i++) {
      var attr = element.attributes[i];
      attrs[attr.name] = attr.value;
    }
    return attrs;
  }

  // 鼠标悬停事件处理
  function handleMouseOver(e) {
    if (!highlightEnabled) return;
    var target = e.target;
    if (!target || target === overlay || target === highlightBox || target === tagLabel || target === selectedBox || target === selectedLabel) {
      return;
    }

    // 避免高亮 html 和 body 元素
    if (target === document.documentElement || target === document.body) {
      return;
    }

    // 如果是已选中的元素，不显示悬停高亮
    if (target === selectedElement) {
      highlightBox.style.display = "none";
      tagLabel.style.display = "none";
      return;
    }
    var rect = target.getBoundingClientRect();
    var selector = getElementSelector(target);
    var text = getElementText(target);
    var attributes = getElementAttributes(target);

    // 更新悬停高亮框位置
    highlightBox.style.display = "block";
    highlightBox.style.left = "".concat(rect.left - 2, "px");
    highlightBox.style.top = "".concat(rect.top - 2, "px");
    highlightBox.style.width = "".concat(rect.width + 4, "px");
    highlightBox.style.height = "".concat(rect.height + 4, "px");

    // 更新标签位置和内容
    tagLabel.style.display = "block";
    tagLabel.textContent = "<".concat(target.tagName.toLowerCase(), ">");

    // 计算标签位置，确保不超出视窗
    var labelTop = rect.top - 22;
    var labelLeft = rect.left;

    // 如果标签会超出顶部，显示在元素下方
    if (labelTop < 0) {
      labelTop = rect.bottom + 5;
    }

    // 如果标签会超出右侧，向左调整
    if (labelLeft + tagLabel.offsetWidth > window.innerWidth) {
      labelLeft = window.innerWidth - tagLabel.offsetWidth - 5;
    }
    tagLabel.style.left = "".concat(Math.max(0, labelLeft), "px");
    tagLabel.style.top = "".concat(labelTop, "px");

    // 发送消息到父窗口
    var elementInfo = {
      tagName: target.tagName.toLowerCase(),
      rect: {
        left: rect.left,
        top: rect.top,
        right: rect.right,
        bottom: rect.bottom,
        width: rect.width,
        height: rect.height,
        x: rect.x,
        y: rect.y
      },
      selector: selector,
      text: text,
      attributes: attributes,
      url: window.location.href,
      path: window.location.pathname,
      timestamp: Date.now()
    };
    try {
      window.parent.postMessage({
        type: "iframe-element-hover",
        data: elementInfo,
        source: "iframe-highlight-injector"
      }, "*");
    } catch (error) {
      console.warn("无法发送消息到父窗口:", error);
    }
  }

  // 鼠标离开事件处理
  function handleMouseOut(e) {
    if (!highlightEnabled) return;
    var relatedTarget = e.relatedTarget;

    // 如果鼠标移动到高亮相关元素上，不隐藏高亮
    if (relatedTarget && (relatedTarget === highlightBox || relatedTarget === tagLabel || relatedTarget === overlay || relatedTarget === selectedBox || relatedTarget === selectedLabel)) {
      return;
    }
    highlightBox.style.display = "none";
    tagLabel.style.display = "none";
    try {
      window.parent.postMessage({
        type: "iframe-element-hover",
        data: null,
        source: "iframe-highlight-injector"
      }, "*");
    } catch (error) {
      console.warn("无法发送消息到父窗口:", error);
    }
  }

  // 点击事件处理
  function handleClick(e) {
    var target = e.target;
    if (!target || target === overlay || target === highlightBox || target === tagLabel || target === selectedBox || target === selectedLabel) {
      return;
    }

    // 避免处理 html 和 body 元素
    if (target === document.documentElement || target === document.body) {
      return;
    }

    // 检查是否是交互元素，这些元素需要保留默认行为
    var isInteractiveElement = ['input', 'textarea', 'select', 'button', 'a'].includes(target.tagName.toLowerCase());

    // 如果高亮功能启用，对于非交互元素阻止默认行为和事件传播
    if (highlightEnabled) {
      e.preventDefault();
      e.stopPropagation();
    }
    var rect = target.getBoundingClientRect();
    var selector = getElementSelector(target);
    var text = getElementText(target);
    var attributes = getElementAttributes(target);
    console.log("Element clicked:", {
      tagName: target.tagName,
      selector: selector,
      rect: rect
    });

    // 立即更新选中高亮
    updateSelectedHighlight(target);

    // 隐藏悬停高亮，因为现在是选中状态
    highlightBox.style.display = "none";
    tagLabel.style.display = "none";
    var elementInfo = {
      tagName: target.tagName.toLowerCase(),
      rect: {
        left: rect.left,
        top: rect.top,
        right: rect.right,
        bottom: rect.bottom,
        width: rect.width,
        height: rect.height,
        x: rect.x,
        y: rect.y
      },
      selector: selector,
      text: text,
      attributes: attributes,
      url: window.location.href,
      path: window.location.pathname,
      timestamp: Date.now()
    };
    try {
      window.parent.postMessage({
        type: "iframe-element-click",
        data: elementInfo,
        source: "iframe-highlight-injector"
      }, "*");
    } catch (error) {
      console.warn("无法发送消息到父窗口:", error);
    }
  }

  // 监听来自父窗口的消息
  function handleParentMessage(event) {
    console.log("Received message from parent:", event.data);
    if (event.data.type === "iframe-highlight-toggle") {
      var enabled = event.data.enabled;
      console.log("Highlight toggle:", enabled);
      if (enabled) {
        enableHighlight();
      } else {
        disableHighlight();
      }
    } else if (event.data.type === "enable-iframe-highlight") {
      console.log("Enable iframe highlight");
      enableHighlight();
    } else if (event.data.type === "disable-iframe-highlight") {
      console.log("Disable iframe highlight");
      disableHighlight();
    } else if (event.data.type === "toggle-iframe-highlight") {
      var _enabled = event.data.enabled !== undefined ? event.data.enabled : !highlightEnabled;
      console.log("Toggle iframe highlight to:", _enabled);
      if (_enabled) {
        enableHighlight();
      } else {
        disableHighlight();
      }
    } else if (event.data.type === "update-selected-element") {
      var selector = event.data.selector;
      console.log("Update selected element with selector:", selector);
      if (selector) {
        try {
          var element = document.querySelector(selector);
          console.log("Found element by selector:", element);
          updateSelectedHighlight(element);
        } catch (error) {
          console.warn("Failed to select element:", error);
          updateSelectedHighlight(null);
        }
      } else {
        updateSelectedHighlight(null);
      }
    } else if (event.data.type === "clear-selected-element") {
      console.log("Clear selected element");
      updateSelectedHighlight(null);
    }
  }

  // 启用高亮功能
  function enableHighlight() {
    console.log("Enabling highlight");
    document.addEventListener("mouseover", handleMouseOver, true);
    document.addEventListener("mouseout", handleMouseOut, true);
    document.addEventListener("click", handleClick, true);
    highlightEnabled = true;
    overlay.style.display = "block";
  }

  // 禁用高亮功能
  function disableHighlight() {
    console.log("Disabling highlight");
    highlightEnabled = false;
    // 保持事件监听器，但通过 highlightEnabled 变量控制行为
    // 这样可以保留选中状态的显示
    highlightBox.style.display = "none";
    tagLabel.style.display = "none";
    // 不隐藏 selectedBox 和 selectedLabel，保留选中状态
  }

  // 完全禁用高亮功能（移除所有监听器）
  function fullyDisableHighlight() {
    console.log("Fully disabling highlight");
    highlightEnabled = false;
    document.removeEventListener("mouseover", handleMouseOver, true);
    document.removeEventListener("mouseout", handleMouseOut, true);
    document.removeEventListener("click", handleClick, true);
    overlay.style.display = "none";
    highlightBox.style.display = "none";
    tagLabel.style.display = "none";
    selectedBox.style.display = "none";
    selectedLabel.style.display = "none";
  }

  // 添加事件监听
  enableHighlight();
  window.addEventListener("message", handleParentMessage);

  // 暴露全局函数供外部调用
  window.__iframeHighlightControl = {
    enable: enableHighlight,
    disable: disableHighlight,
    fullyDisable: fullyDisableHighlight,
    isEnabled: function isEnabled() {
      return highlightEnabled;
    },
    getSelectedElement: function getSelectedElement() {
      return selectedElement;
    },
    updateSelected: updateSelectedHighlight,
    // 通过消息发送开关控制
    sendToggleMessage: function sendToggleMessage(enabled) {
      window.parent.postMessage({
        type: 'iframe-highlight-status',
        enabled: enabled || highlightEnabled,
        source: 'iframe-highlight-injector'
      }, '*');
    }
  };

  // 通知父窗口脚本已加载
  try {
    window.parent.postMessage({
      type: "iframe-highlight-ready",
      data: {
        url: window.location.href,
        userAgent: navigator.userAgent,
        timestamp: Date.now()
      },
      source: "iframe-highlight-injector"
    }, "*");
  } catch (error) {
    console.warn("无法发送就绪消息到父窗口:", error);
  }

  // 清理函数
  window.__iframeHighlightCleanup = function () {
    fullyDisableHighlight();
    window.removeEventListener("message", handleParentMessage);
    if (overlay.parentElement) {
      overlay.parentElement.removeChild(overlay);
    }
    delete window.__iframeHighlightInitialized;
    delete window.__iframeHighlightCleanup;
  };
})();
</script>
</body>
</html>
