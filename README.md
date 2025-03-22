<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>셀러집합소 - 예약 시스템</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FABD02;
            --secondary-color: #3498db;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --warning-color: #ffc107;
        }
        
        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-color: var(--primary-color);
            color: #333;
            line-height: 1.6;
        }
        
        .header {
            text-align: center;
            padding: 20px 0;
            background-color: var(--primary-color);
        }
        
        .header-title {
            color: white;
            font-size: 1.8rem;
            font-weight: 700;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.2);
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto 30px;
            padding: 25px;
            background-color: white;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 10px;
        }
        
        h2, h3 { 
            text-align: center; 
            margin-bottom: 20px; 
            color: #343a40; 
        }
        
        .tab-container {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            margin-bottom: 25px;
            gap: 10px;
        }
        
        .tab {
            padding: 10px 20px;
            background-color: #f0f0f0;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 500;
            min-width: 90px;
        }
        
        .tab:hover, .tab.active {
            background-color: var(--secondary-color);
            color: white;
            transform: translateY(-2px);
        }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        
        /* 관리자 탭 내용에 대한 명확한 스타일 */
        .admin-tab-content { display: none; }
        .admin-tab-content.active { display: block; }
        
        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            max-width: 500px;
            margin: 0 auto;
        }
        
        input, select, textarea {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-size: 16px;
        }
        
        button {
            padding: 12px 20px;
            background-color: var(--secondary-color);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 500;
        }
        
        button:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
        }
        
        button:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
            transform: none;
        }
        
        .profile-section {
            background-color: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        
        .profile-row {
            display: flex;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        
        .profile-label {
            width: 150px;
            font-weight: 500;
            margin-right: 10px;
        }
        
        .profile-value { flex: 1; min-width: 200px; }
        
        .radio-group { display: flex; gap: 15px; flex-wrap: wrap; }
        .radio-item { display: flex; align-items: center; }
        .radio-item input { margin-right: 5px; }
        
        .admin-section { 
            background-color: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            margin-bottom: 20px;
        }
        
        .admin-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 0 10px rgba(0,0,0,0.05);
        }
        
        .admin-table th, .admin-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        .admin-table th {
            background-color: #f2f2f2;
            font-weight: 500;
        }
        
        .admin-table tr:hover {
            background-color: #f5f5f5;
        }
        
        .admin-filter {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 15px;
            align-items: center;
        }
        
        .status-chip {
            display: inline-block;
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 12px;
            background-color: #eee;
            color: #333;
        }
        
        .status-pending {
            background-color: var(--warning-color);
            color: #856404;
        }
        
        .status-approved {
            background-color: var(--success-color);
            color: white;
        }
        
        .status-rejected {
            background-color: var(--danger-color);
            color: white;
        }
        
        /* 달력 스타일 */
        .calendar {
            width: 100%;
            margin: 20px 0;
            border-radius: 10px;
            overflow: hidden;
            background-color: white;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        
        .calendar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background-color: #f8f9fa;
        }
        
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            padding: 10px;
        }
        
        .calendar-day-name {
            text-align: center;
            font-weight: bold;
            padding: 10px 0;
            background-color: #f0f0f0;
            border-radius: 5px;
        }
        
        .calendar-day {
            position: relative;
            min-height: 70px;
            padding: 5px;
            border: 1px solid #eee;
            border-radius: 5px;
            text-align: center;
            background-color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .calendar-day:hover {
            background-color: #f0f8ff;
            border-color: var(--secondary-color);
            transform: translateY(-2px);
        }
        
        .calendar-day.other-month {
            background-color: #f9f9f9;
            color: #aaa;
            cursor: default;
        }
        
        .calendar-day.selected {
            background-color: #e3f2fd;
            border-color: var(--secondary-color);
        }
        
        .calendar-day.today {
            font-weight: bold;
            border-color: var(--secondary-color);
            border-width: 2px;
        }
        
        .calendar-day-number {
            font-size: 1.2em;
            padding: 5px 0;
        }
        
        .time-panel {
            margin: 20px 0;
            padding: 20px;
            border: 1px solid #eee;
            border-radius: 10px;
            background-color: #f9f9f9;
            display: none;
        }
        
        .time-panel.active {
            display: block;
        }
        
        .time-slots {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-top: 15px;
        }
        
        .time-slot {
            padding: 14px 8px;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            background-color: #d4edda;
            color: #155724;
            transition: all 0.3s;
            font-weight: 500;
        }
        
        .time-slot:hover {
            background-color: var(--secondary-color);
            color: white;
            transform: translateY(-2px);
        }
        
        .reservation-item {
            background-color: #f9f9f9;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 10px;
            border-left: 5px solid var(--primary-color);
        }
        
        /* 모달 스타일 */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
        }
        
        .modal-content {
            background-color: #fefefe;
            margin: 10% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-width: 600px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            position: relative;
        }
        
        .close {
            color: #aaa;
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        
        .close:hover,
        .close:focus {
            color: black;
            text-decoration: none;
        }
        
        /* 로그아웃 버튼 */
        .logout-btn {
            background-color: #6c757d;
            color: white;
            padding: 6px 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            margin-left: 10px;
        }
        
        .logout-btn:hover {
            background-color: #5a6268;
        }
        
        .approval-waiting {
            background-color: #fff3cd;
            border: 1px solid #ffeeba;
            color: #856404;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .approval-rejected {
            background-color: #f8d7da;
            border: 1px solid #f5c6cb;
            color: #721c24;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        /* 예약 상태에 따른 색상 */
        .reservation-pending {
            border-left-color: var(--warning-color);
        }
        
        .reservation-approved {
            border-left-color: var(--success-color);
        }
        
        .reservation-rejected {
            border-left-color: var(--danger-color);
        }
        
        .action-buttons {
            display: flex;
            gap: 5px;
        }
        
        .badge {
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
            color: white;
        }
        
        .badge-warning {
            background-color: var(--warning-color);
            color: #856404;
        }
        
        .badge-success {
            background-color: var(--success-color);
        }
        
        .badge-danger {
            background-color: var(--danger-color);
        }
        
        /* 관리자 달력 스타일 */
        .admin-calendar {
            width: 100%;
            margin: 20px 0;
            border-radius: 10px;
            overflow: hidden;
            background-color: white;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        
        .admin-time-slot {
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            background-color: var(--success-color);
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .admin-time-slot.reserved {
            background-color: var(--secondary-color);
        }
        
        .admin-time-slot.closed {
            background-color: var(--danger-color);
        }
        
        /* 관리자용 예약 정보 패널 */
        .admin-day-details {
            display: none;
            margin-top: 20px;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 10px;
            border: 1px solid #ddd;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        .fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1 class="header-title">셀러집합소 예약 시스템</h1>
    </div>

    <div class="container">
        <div class="tab-container" id="main-tab-container">
            <button class="tab active login-tab" onclick="openTab('login')">로그인</button>
            <button class="tab register-tab" onclick="openTab('register')">회원가입</button>
            <button class="tab" onclick="openTab('reservation')">예약</button>
            <button class="tab admin-only" onclick="openTab('admin')" style="display: none;">관리자</button>
            <button class="tab user-only" onclick="openTab('profile')" style="display: none;">내 정보</button>
            <button class="tab logout-tab" onclick="logout()" style="display: none;">로그아웃</button>
        </div>
        
        <!-- 로그인 탭 -->
        <div id="login" class="tab-content active">
            <h2>로그인</h2>
            <form id="login-form">
                <input type="text" id="login-username" placeholder="아이디" required>
                <input type="password" id="login-password" placeholder="비밀번호" required>
                <button type="submit">로그인</button>
            </form>
        </div>
        
        <!-- 회원가입 탭 -->
        <div id="register" class="tab-content">
            <h2>회원가입</h2>
            <form id="register-form">
                <div style="display: flex; gap: 10px;">
                    <input type="text" id="register-username" placeholder="아이디" style="flex: 1;" required>
                    <button type="button" id="check-username" style="background-color: #6c757d;">중복확인</button>
                </div>
                <div id="username-check-result" style="font-size: 14px;"></div>
                
                <input type="password" id="register-password" placeholder="비밀번호" required>
                <input type="password" id="register-confirm-password" placeholder="비밀번호 확인" required>
                <input type="text" id="register-name" placeholder="이름" required>
                <input type="tel" id="register-phone" placeholder="전화번호" required>
                <input type="email" id="register-email" placeholder="이메일" required>
                
                <h3>사업자 정보</h3>
                <input type="text" id="register-business-name" placeholder="상호명" required>
                <input type="text" id="register-business-number" placeholder="사업자등록번호" required>
                
                <div style="margin: 10px 0;">
                    <label style="display: flex; align-items: center;">
                        <input type="checkbox" id="agree-terms" required style="margin-right: 10px;"> 
                        이용약관에 동의합니다.
                    </label>
                </div>
                
                <div style="margin: 10px 0;">
                    <label style="display: flex; align-items: center;">
                        <input type="checkbox" id="agree-privacy" required style="margin-right: 10px;"> 
                        개인정보 수집 및 이용에 동의합니다.
                    </label>
                </div>
                
                <button type="submit" id="register-button" disabled>회원가입</button>
            </form>
        </div>
        
        <!-- 예약 탭 -->
        <div id="reservation" class="tab-content">
            <h2>예약하기</h2>
            <div id="login-message" style="text-align: center; background-color: #fff3cd; padding: 15px; border-radius: 10px; margin-bottom: 20px;">
                로그인 후 이용 가능합니다.
            </div>
            <div id="approval-pending-message" class="approval-waiting" style="display: none;">
                가입 승인 대기 중입니다. 관리자의 승인 후 이용 가능합니다.
            </div>
            <div id="approval-rejected-message" class="approval-rejected" style="display: none;">
                가입이 거절되었습니다. 관리자에게 문의해주세요.
            </div>
            <div id="reservation-container" style="display: none;">
                <!-- 달력 -->
                <div class="calendar">
                    <div class="calendar-header">
                        <button id="prev-month">&lt;</button>
                        <div id="current-month">2025년 3월</div>
                        <button id="next-month">&gt;</button>
                    </div>
                    <div class="calendar-grid" id="calendar-days">
                        <!-- 달력이 여기에 생성됩니다 -->
                    </div>
                </div>
                
                <!-- 시간대 선택 패널 -->
                <div id="time-panel" class="time-panel">
                    <div id="time-panel-header" style="font-weight: bold; margin-bottom: 15px; text-align: center;">2025년 3월 22일 시간 선택</div>
                    <div id="time-slots" class="time-slots">
                        <!-- 시간 슬롯이 여기에 생성됩니다 -->
                    </div>
                </div>
                
                <div id="my-reservations" style="margin-top: 30px;">
                    <h3>내 예약 현황</h3>
                    <div id="my-reservations-list">
                        <p style="text-align: center;">예약 내역이 없습니다.</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- 내 정보 탭 -->
        <div id="profile" class="tab-content">
            <h2>내 정보</h2>
            <form id="profile-form">
                <div class="profile-section">
                    <h3>기본 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">아이디</div>
                        <div class="profile-value" id="profile-username"></div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">이름</div>
                        <div class="profile-value" id="profile-name"></div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">이메일</div>
                        <div class="profile-value">
                            <input type="email" id="profile-email" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">전화번호</div>
                        <div class="profile-value">
                            <input type="tel" id="profile-phone" required>
                        </div>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h3>사업자 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">상호명</div>
                        <div class="profile-value">
                            <input type="text" id="profile-business-name" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">사업자등록번호</div>
                        <div class="profile-value">
                            <input type="text" id="profile-business-number" required>
                        </div>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h3>셀러 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">판로유형</div>
                        <div class="profile-value">
                            <input type="text" id="profile-sales-type" placeholder="예: 온라인, 오프라인, 혼합" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">셀러명</div>
                        <div class="profile-value">
                            <input type="text" id="profile-seller-name" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">플랫폼</div>
                        <div class="profile-value">
                            <input type="text" id="profile-platform" placeholder="예: 네이버, 카카오, 쿠팡 등" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">주카테고리</div>
                        <div class="profile-value">
                            <input type="text" id="profile-main-category" placeholder="예: 패션, 뷰티, 가전제품 등" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">구독자수</div>
                        <div class="profile-value">
                            <input type="number" id="profile-subscribers" placeholder="숫자만 입력" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">월 방송횟수</div>
                        <div class="profile-value">
                            <input type="number" id="profile-broadcasts-per-month" placeholder="숫자만 입력" required>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">매니저 보유 여부</div>
                        <div class="profile-value">
                            <div class="radio-group">
                                <label class="radio-item">
                                    <input type="radio" name="profile-has-manager" value="yes" required> 있음
                                </label>
                                <label class="radio-item">
                                    <input type="radio" name="profile-has-manager" value="no"> 없음
                                </label>
                            </div>
                        </div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">물류시스템</div>
                        <div class="profile-value">
                            <div class="radio-group">
                                <label class="radio-item">
                                    <input type="radio" name="profile-logistics" value="self" required> 자체
                                </label>
                                <label class="radio-item">
                                    <input type="radio" name="profile-logistics" value="3pl"> 3PL
                                </label>
                                <label class="radio-item">
                                    <input type="radio" name="profile-logistics" value="none"> 없음
                                </label>
                            </div>
                        </div>
                    </div>
                </div>
                
                <button type="submit" style="background-color: var(--success-color);">정보 저장</button>
            </form>
        </div>
        
        <!-- 관리자 탭 -->
        <div id="admin" class="tab-content">
            <h2>관리자 페이지</h2>
            <div id="admin-login-message" style="text-align: center; background-color: #fff3cd; padding: 15px; border-radius: 10px; margin-bottom: 20px;">
                관리자 로그인이 필요합니다.
            </div>
            <div id="admin-panel" style="display: none;">
                <div class="tab-container">
                    <button class="tab active" onclick="showAdminTab('admin-pending')">승인 대기</button>
                    <button class="tab" onclick="showAdminTab('admin-calendar')">예약 관리</button>
                    <button class="tab" onclick="showAdminTab('admin-list')">예약자 리스트</button>
                    <button class="tab" onclick="showAdminTab('admin-users')">가입자 정보</button>
                    <button class="tab" onclick="showAdminTab('admin-approvals')">가입 승인</button>
                </div>
                
                <!-- 관리자 탭 내용 -->
                <div id="admin-pending" class="admin-tab-content" style="display: block;">
                    <div class="admin-section">
                        <h3>승인 대기 예약</h3>
                        <div id="pending-reservations-container">
                            <p>현재 승인 대기 중인 예약이 없습니다.</p>
                        </div>
                    </div>
                </div>
                
                <div id="admin-calendar" class="admin-tab-content" style="display: none;">
                    <div class="admin-section">
                        <h3>예약 관리</h3>
                        <div class="calendar">
                            <div class="calendar-header">
                                <button id="admin-prev-month">&lt;</button>
                                <div id="admin-current-month">2025년 3월</div>
                                <button id="admin-next-month">&gt;</button>
                            </div>
                            <div class="calendar-grid" id="admin-calendar-days">
                                <!-- 관리자 달력이 여기에 생성됩니다 -->
                            </div>
                        </div>
                        
                        <!-- 선택된 날짜의 상세 정보 -->
                        <div id="admin-day-details" class="admin-day-details">
                            <h3 id="admin-selected-date">2025년 3월 22일 예약 현황</h3>
                            <div id="admin-time-slots-container">
                                <!-- 시간대 목록이 여기에 표시됩니다 -->
                            </div>
                        </div>
                        
                        <div style="margin-top: 20px; text-align: center;">
                            <button id="close-all-slots-button" style="background-color: var(--danger-color);">모든 시간대 예약마감</button>
                            <button id="open-all-slots-button" style="background-color: var(--success-color);">모든 시간대 예약가능</button>
                        </div>
                    </div>
                </div>
                
                <div id="admin-list" class="admin-tab-content" style="display: none;">
                    <div class="admin-section">
                        <h3>예약자 리스트</h3>
                        <div class="admin-filter">
                            <div>
                                <label>기간:</label>
                                <select id="date-range-filter">
                                    <option value="all">전체</option>
                                    <option value="today" selected>오늘</option>
                                    <option value="tomorrow">내일</option>
                                    <option value="week">이번주</option>
                                    <option value="month">이번달</option>
                                </select>
                            </div>
                            <div>
                                <label>상태:</label>
                                <select id="status-filter">
                                    <option value="all">모든 상태</option>
                                    <option value="pending">승인 대기</option>
                                    <option value="approved" selected>승인됨</option>
                                    <option value="rejected">거절됨</option>
                                </select>
                            </div>
                            <button id="refresh-list">새로고침</button>
                        </div>
                        
                        <table class="admin-table">
                            <thead>
                                <tr>
                                    <th>날짜</th>
                                    <th>시간</th>
                                    <th>예약자</th>
                                    <th>셀러명</th>
                                    <th>상태</th>
                                    <th>관리</th>
                                </tr>
                            </thead>
                            <tbody id="reservations-table-body">
                                <tr>
                                    <td colspan="6" style="text-align: center;">예약 내역이 없습니다.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <div id="admin-users" class="admin-tab-content" style="display: none;">
                    <div class="admin-section">
                        <h3>가입자 정보</h3>
                        <div class="admin-filter">
                            <div>
                                <label>검색:</label>
                                <input type="text" id="user-search" placeholder="이름 또는 아이디로 검색">
                            </div>
                            <button id="refresh-users">새로고침</button>
                        </div>
                        
                        <table class="admin-table">
                            <thead>
                                <tr>
                                    <th>아이디</th>
                                    <th>이름</th>
                                    <th>셀러명</th>
                                    <th>판로유형</th>
                                    <th>플랫폼</th>
                                    <th>카테고리</th>
                                    <th>관리</th>
                                </tr>
                            </thead>
                            <tbody id="users-table-body">
                                <!-- 사용자 목록이 여기에 표시됩니다 -->
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <div id="admin-approvals" class="admin-tab-content" style="display: none;">
                    <div class="admin-section">
                        <h3>가입 승인</h3>
                        <div class="admin-filter">
                            <div>
                                <label>상태:</label>
                                <select id="approval-status-filter">
                                    <option value="all">모든 상태</option>
                                    <option value="pending" selected>승인 대기</option>
                                    <option value="approved">승인됨</option>
                                    <option value="rejected">거절됨</option>
                                </select>
                            </div>
                            <button id="refresh-approvals">새로고침</button>
                        </div>
                        
                        <table class="admin-table">
                            <thead>
                                <tr>
                                    <th>아이디</th>
                                    <th>이름</th>
                                    <th>이메일</th>
                                    <th>사업자번호</th>
                                    <th>가입일</th>
                                    <th>상태</th>
                                    <th>관리</th>
                                </tr>
                            </thead>
                            <tbody id="approvals-table-body">
                                <!-- 승인 대기자 목록이 여기에 표시됩니다 -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <!-- 사용자 상세 정보 모달 -->
    <div id="userDetailModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeUserDetailModal()">&times;</span>
            <h2>사용자 상세 정보</h2>
            <div id="userDetailContent"></div>
        </div>
    </div>

    <script>
        // 데이터 모델
        let users = [
            { 
                username: 'admin', 
                password: 'wlqgkqth159', 
                name: '관리자', 
                phone: '010-0000-0000', 
                email: 'admin@example.com', 
                isAdmin: true,
                registrationDate: '2025-01-01',
                businessName: '관리자회사',
                businessNumber: '123-45-67890',
                approved: true,
                // 셀러 정보
                salesType: '',
                sellerName: '',
                platform: '',
                mainCategory: '',
                subscribers: 0,
                broadcastsPerMonth: 0,
                hasManager: 'no',
                logistics: 'none'
            },
            {
                username: 'user',
                password: 'user123',
                name: '테스트 사용자',
                phone: '010-1234-5678',
                email: 'user@example.com',
                isAdmin: false,
                registrationDate: '2025-03-01',
                businessName: '테스트 비즈니스',
                businessNumber: '123-45-67890',
                approved: true,
                salesType: '온라인',
                sellerName: '테스트셀러',
                platform: '네이버',
                mainCategory: '패션',
                subscribers: 1000,
                broadcastsPerMonth: 5,
                hasManager: 'yes',
                logistics: 'self'
            }
        ];
        
        // 예약 시간대 가능 여부 (관리자가 설정)
        let timeSlotAvailability = {};
        
        let currentUser = null;
        let usernameChecked = false;
        let currentDate = new Date();
        let selectedDate = '';
        let adminSelectedDate = '';
        let reservations = [];
        
        // 샘플 예약 추가 (테스트용)
        function addSampleReservations() {
            // 오늘 날짜
            const today = new Date();
            const todayString = today.toISOString().split('T')[0];
            
            // 내일 날짜
            const tomorrow = new Date();
            tomorrow.setDate(tomorrow.getDate() + 1);
            const tomorrowString = tomorrow.toISOString().split('T')[0];
            
            // 샘플 예약 추가
            if (reservations.length === 0) {
                reservations.push({
                    id: '1',
                    username: 'user',
                    date: todayString,
                    time: '14:00',
                    status: '예약 완료',
                    timestamp: new Date().toISOString()
                });
                
                reservations.push({
                    id: '2',
                    username: 'user',
                    date: tomorrowString, 
                    time: '10:00',
                    status: '예약 대기',
                    timestamp: new Date().toISOString()
                });
                
                reservations.push({
                    id: '3',
                    username: 'user',
                    date: tomorrowString,
                    time: '16:00',
                    status: '예약 거절',
                    timestamp: new Date().toISOString()
                });
            }
        }
        
        // 로그아웃 함수
        function logout() {
            currentUser = null;
            updateUIForUser();
            openTab('login');
            alert('로그아웃 되었습니다.');
        }
        
        // 관리자 탭 전환 함수
        function showAdminTab(tabId) {
            // 모든 관리자 탭 내용 숨기기
            document.querySelectorAll('.admin-tab-content').forEach(tab => {
                tab.style.display = 'none';
            });
            
            // 모든 관리자 탭 버튼 비활성화
            document.querySelectorAll('#admin-panel .tab-container .tab').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // 선택한 탭 내용 표시
            document.getElementById(tabId).style.display = 'block';
            
            // 선택한 탭 버튼 활성화
            document.querySelector(`#admin-panel .tab-container .tab[onclick="showAdminTab('${tabId}')"]`).classList.add('active');
            
            // 가입자 정보 탭 선택 시 사용자 목록 갱신
            if (tabId === 'admin-users') {
                updateUsersList();
            }
            
            // 가입 승인 탭 선택 시 승인 목록 갱신
            if (tabId === 'admin-approvals') {
                updateApprovalsList();
            }
            
            // 승인 대기 예약 탭 선택 시 목록 갱신
            if (tabId === 'admin-pending') {
                updatePendingReservations();
            }
            
            // 예약자 리스트 탭 선택 시 목록 갱신
            if (tabId === 'admin-list') {
                updateReservationsList();
            }
            
            // 예약 관리 탭 선택 시 달력 렌더링
            if (tabId === 'admin-calendar') {
                renderAdminCalendar();
            }
        }
        
        // 관리자 달력 생성 함수
        function renderAdminCalendar() {
            // 관리자 달력 렌더링 (사용자 달력과 유사하지만 다른 컨테이너 사용)
            const container = document.getElementById('admin-calendar-days');
            container.innerHTML = '';
            
            // 날짜 표시 업데이트
            const monthDisplay = document.getElementById('admin-current-month');
            monthDisplay.textContent = `${currentDate.getFullYear()}년 ${currentDate.getMonth() + 1}월`;
            
            // 요일 이름 추가
            const dayNames = ['일', '월', '화', '수', '목', '금', '토'];
            dayNames.forEach(day => {
                const dayNameElement = document.createElement('div');
                dayNameElement.className = 'calendar-day-name';
                dayNameElement.textContent = day;
                container.appendChild(dayNameElement);
            });
            
            // 해당 월의 첫 번째 날
            const firstDay = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1);
            // 해당 월의 마지막 날
            const lastDay = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0);
            
            // 첫 번째 날의 요일에 따라 앞쪽 빈칸 추가
            const firstDayOfWeek = firstDay.getDay();
            for (let i = 0; i < firstDayOfWeek; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day other-month';
                container.appendChild(emptyDay);
            }
            
            // 날짜 추가
            const today = new Date();
            today.setHours(0, 0, 0, 0); // 오늘 날짜 비교를 위해 시간 초기화
            
            for (let i = 1; i <= lastDay.getDate(); i++) {
                const day = document.createElement('div');
                day.className = 'calendar-day';
                
                const currentDayDate = new Date(currentDate.getFullYear(), currentDate.getMonth(), i);
                currentDayDate.setHours(0, 0, 0, 0); // 시간 초기화
                const dateString = currentDayDate.toISOString().split('T')[0];
                
                // 오늘 날짜 표시
                if (currentDayDate.getTime() === today.getTime()) {
                    day.classList.add('today');
                }
                
                // 선택된 날짜 표시
                if (adminSelectedDate === dateString) {
                    day.classList.add('selected');
                }
                
                // 날짜 번호 추가
                const dayNumber = document.createElement('div');
                dayNumber.className = 'calendar-day-number';
                dayNumber.textContent = i;
                day.appendChild(dayNumber);
                
                // 예약 수 표시
                const dateReservations = reservations.filter(r => r.date === dateString);
                if (dateReservations.length > 0) {
                    const reservationCount = document.createElement('div');
                    reservationCount.style.fontSize = '12px';
                    reservationCount.style.color = 'white';
                    reservationCount.style.backgroundColor = '#3498db';
                    reservationCount.style.borderRadius = '10px';
                    reservationCount.style.padding = '2px 5px';
                    reservationCount.style.margin = '2px auto';
                    reservationCount.style.width = 'fit-content';
                    reservationCount.textContent = `${dateReservations.length}건`;
                    day.appendChild(reservationCount);
                }
                
                // 날짜 클릭 이벤트
                day.addEventListener('click', () => {
                    adminSelectedDate = dateString;
                    
                    // 모든 날짜에서 선택 클래스 제거
                    document.querySelectorAll('#admin-calendar-days .calendar-day').forEach(d => {
                        d.classList.remove('selected');
                    });
                    
                    // 선택된 날짜에 클래스 추가
                    day.classList.add('selected');
                    
                    // 날짜 상세 정보 표시
                    showAdminDayDetails(dateString);
                });
                
                container.appendChild(day);
            }
            
            // 마지막 날의 요일에 따라 뒤쪽 빈칸 추가
            const lastDayOfWeek = lastDay.getDay();
            for (let i = lastDayOfWeek; i < 6; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day other-month';
                container.appendChild(emptyDay);
            }
        }
        
        // 관리자 달력 날짜 선택 시 상세 정보 표시
        function showAdminDayDetails(dateString) {
            const detailsContainer = document.getElementById('admin-day-details');
            const dateHeader = document.getElementById('admin-selected-date');
            const timeSlotsContainer = document.getElementById('admin-time-slots-container');
            
            // 날짜 포맷팅
            const date = new Date(dateString);
            const formattedDate = `${date.getFullYear()}년 ${date.getMonth() + 1}월 ${date.getDate()}일`;
            
            // 헤더 업데이트
            dateHeader.textContent = `${formattedDate} 예약 현황`;
            
            // 시간대 컨테이너 초기화
            timeSlotsContainer.innerHTML = '';
            
            // 선택된 날짜의 예약 가져오기
            const dateReservations = reservations.filter(r => r.date === dateString);
            
            // 10시부터 18시까지 시간대 생성
            for (let hour = 10; hour <= 18; hour++) {
                const timeSlot = document.createElement('div');
                const timeString = `${hour}:00`;
                
                // 해당 시간에 예약이 있는지 확인
                const timeReservation = dateReservations.find(r => r.time === timeString);
                
                // 시간대 가용 여부 확인 (기본값은 available)
                const timeSlotKey = `${dateString}_${timeString}`;
                const isAvailable = (timeSlotAvailability[timeSlotKey] !== false);
                
                // 클래스 및 상태 설정
                let statusText = '';
                let className = 'admin-time-slot';
                
                if (timeReservation) {
                    className += ' reserved';
                    
                    // 예약 상태에 따라 표시
                    if (timeReservation.status === '예약 대기') {
                        statusText = '<span class="badge badge-warning">대기중</span>';
                    } else if (timeReservation.status === '예약 완료') {
                        statusText = '<span class="badge badge-success">승인됨</span>';
                    } else if (timeReservation.status === '예약 거절') {
                        statusText = '<span class="badge badge-danger">거절됨</span>';
                    }
                } else if (!isAvailable) {
                    className += ' closed';
                    statusText = '<span>마감</span>';
                } else {
                    statusText = '<span>가능</span>';
                }
                
                timeSlot.className = className;
                
                // 사용자 정보 표시
                let userInfo = '';
                if (timeReservation) {
                    const user = users.find(u => u.username === timeReservation.username);
                    userInfo = user ? `${user.name} (${user.sellerName || '셀러명 미설정'})` : '알 수 없음';
                }
                
                timeSlot.innerHTML = `
                    <div>${timeString}</div>
                    <div>${userInfo}</div>
                    <div>${statusText}</div>
                `;
                
                // 시간대 클릭 이벤트 (예약이 없는 경우에만 가용성 토글)
                if (!timeReservation) {
                    timeSlot.addEventListener('click', () => {
                        // 시간대 가용성 토글
                        timeSlotAvailability[timeSlotKey] = !isAvailable;
                        
                        // UI 업데이트
                        showAdminDayDetails(dateString);
                    });
                }
                
                timeSlotsContainer.appendChild(timeSlot);
            }
            
            // 상세 정보 패널 표시
            detailsContainer.style.display = 'block';
        }
        
        // 대기 중인 예약 목록 업데이트
        function updatePendingReservations() {
            const container = document.getElementById('pending-reservations-container');
            
            // 승인 대기 중인 예약 필터링
            const pendingReservations = reservations.filter(r => r.status === '예약 대기');
            
            if (pendingReservations.length === 0) {
                container.innerHTML = '<p>현재 승인 대기 중인 예약이 없습니다.</p>';
                return;
            }
            
            // 내용 초기화
            container.innerHTML = '';
            
            // 예약 목록 표시 (날짜 기준 정렬)
            pendingReservations.sort((a, b) => new Date(a.date) - new Date(b.date));
            
            pendingReservations.forEach(reservation => {
                const date = new Date(reservation.date);
                const formattedDate = `${date.getFullYear()}년 ${date.getMonth() + 1}월 ${date.getDate()}일`;
                const user = users.find(u => u.username === reservation.username);
                
                const item = document.createElement('div');
                item.className = 'reservation-item reservation-pending';
                item.innerHTML = `
                    <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                        <strong>${formattedDate} ${reservation.time}</strong>
                        <span class="badge badge-warning">대기중</span>
                    </div>
                    <p><strong>예약자:</strong> ${user ? user.name : '알 수 없음'} (${reservation.username})</p>
                    <p><strong>셀러명:</strong> ${user ? user.sellerName || '-' : '-'}</p>
                    <p><strong>연락처:</strong> ${user ? user.phone : '-'}</p>
                    <div style="display: flex; justify-content: flex-end; margin-top: 10px;">
                        <button class="approve-reservation" data-id="${reservation.id}" style="background-color: var(--success-color); margin-right: 5px;">승인</button>
                        <button class="reject-reservation" data-id="${reservation.id}" style="background-color: var(--danger-color);">거절</button>
                    </div>
                `;
                
                container.appendChild(item);
            });
            
            // 승인 버튼 이벤트 추가
            document.querySelectorAll('.approve-reservation').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = this.getAttribute('data-id');
                    updateReservationStatus(id, '예약 완료');
                });
            });
            
            // 거절 버튼 이벤트 추가
            document.querySelectorAll('.reject-reservation').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = this.getAttribute('data-id');
                    updateReservationStatus(id, '예약 거절');
                });
            });
        }
        
        // 예약 상태 업데이트
        function updateReservationStatus(id, status) {
            const reservation = reservations.find(r => r.id === id);
            if (!reservation) return;
            
            reservation.status = status;
            
            updatePendingReservations();
            updateReservationsList();
            updateMyReservations();
            
            // 관리자 달력 업데이트
            if (adminSelectedDate) {
                showAdminDayDetails(adminSelectedDate);
            }
            
            const statusText = status === '예약 완료' ? '승인' : '거절';
            alert(`예약이 ${statusText}되었습니다.`);
        }
        
        // 모든 예약 목록 업데이트 (관리자 페이지)
        function updateReservationsList() {
            const statusFilter = document.getElementById('status-filter').value;
            const dateRangeFilter = document.getElementById('date-range-filter').value;
            const tbody = document.getElementById('reservations-table-body');
            tbody.innerHTML = '';
            
            // 현재 날짜
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            
            // 날짜 필터링 함수
            const dateFilter = (reservation) => {
                const reservationDate = new Date(reservation.date);
                
                if (dateRangeFilter === 'all') return true;
                if (dateRangeFilter === 'today') {
                    return reservationDate.toDateString() === today.toDateString();
                }
                
                if (dateRangeFilter === 'tomorrow') {
                    const tomorrow = new Date(today);
                    tomorrow.setDate(tomorrow.getDate() + 1);
                    return reservationDate.toDateString() === tomorrow.toDateString();
                }
                
                if (dateRangeFilter === 'week') {
                    const weekLater = new Date(today);
                    weekLater.setDate(weekLater.getDate() + 7);
                    return reservationDate >= today && reservationDate <= weekLater;
                }
                
                if (dateRangeFilter === 'month') {
                    const monthLater = new Date(today);
                    monthLater.setMonth(monthLater.getMonth() + 1);
                    return reservationDate >= today && reservationDate <= monthLater;
                }
                
                return true;
            };
            
            // 상태 필터링
            let filteredReservations = reservations;
            
            if (statusFilter !== 'all') {
                if (statusFilter === 'pending') {
                    filteredReservations = filteredReservations.filter(r => r.status === '예약 대기');
                } else if (statusFilter === 'approved') {
                    filteredReservations = filteredReservations.filter(r => r.status === '예약 완료');
                } else if (statusFilter === 'rejected') {
                    filteredReservations = filteredReservations.filter(r => r.status === '예약 거절');
                }
            }
            
            // 날짜 필터링
            filteredReservations = filteredReservations.filter(dateFilter);
            
            if (filteredReservations.length === 0) {
                const tr = document.createElement('tr');
                tr.innerHTML = '<td colspan="6" style="text-align: center;">예약 내역이 없습니다.</td>';
                tbody.appendChild(tr);
                return;
            }
            
            // 예약 목록 표시 (날짜 기준 정렬)
            filteredReservations.sort((a, b) => new Date(a.date) - new Date(b.date));
            
            filteredReservations.forEach(reservation => {
                const date = new Date(reservation.date);
                const formattedDate = `${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}-${date.getDate().toString().padStart(2, '0')}`;
                const user = users.find(u => u.username === reservation.username);
                
                let statusHtml = '';
                let actionButtons = '';
                
                if (reservation.status === '예약 대기') {
                    statusHtml = '<span class="badge badge-warning">대기중</span>';
                    actionButtons = `
                        <button class="approve-reservation" data-id="${reservation.id}" style="background-color: var(--success-color); margin-right: 5px;">승인</button>
                        <button class="reject-reservation" data-id="${reservation.id}" style="background-color: var(--danger-color);">거절</button>
                    `;
                } else if (reservation.status === '예약 완료') {
                    statusHtml = '<span class="badge badge-success">승인됨</span>';
                    actionButtons = `<button class="reject-reservation" data-id="${reservation.id}" style="background-color: var(--danger-color);">취소</button>`;
                } else if (reservation.status === '예약 거절') {
                    statusHtml = '<span class="badge badge-danger">거절됨</span>';
                    actionButtons = `<button class="approve-reservation" data-id="${reservation.id}" style="background-color: var(--success-color);">승인</button>`;
                }
                
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${formattedDate}</td>
                    <td>${reservation.time}</td>
                    <td>${user ? user.name : '알 수 없음'}</td>
                    <td>${user ? user.sellerName || '-' : '-'}</td>
                    <td>${statusHtml}</td>
                    <td class="action-buttons">${actionButtons}</td>
                `;
                tbody.appendChild(tr);
            });
            
            // 승인 버튼 이벤트 추가
            document.querySelectorAll('.approve-reservation').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = this.getAttribute('data-id');
                    updateReservationStatus(id, '예약 완료');
                });
            });
            
            // 거절 버튼 이벤트 추가
            document.querySelectorAll('.reject-reservation').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = this.getAttribute('data-id');
                    updateReservationStatus(id, '예약 거절');
                });
            });
        }
        
        // 사용자 목록 업데이트
        function updateUsersList() {
            const searchTerm = document.getElementById('user-search').value.toLowerCase();
            const tbody = document.getElementById('users-table-body');
            tbody.innerHTML = '';
            
            // 사용자 필터링
            let filteredUsers = users.filter(user => user.approved === true); // 승인된 사용자만
            if (searchTerm) {
                filteredUsers = filteredUsers.filter(user => 
                    user.username.toLowerCase().includes(searchTerm) ||
                    user.name.toLowerCase().includes(searchTerm) ||
                    (user.sellerName && user.sellerName.toLowerCase().includes(searchTerm)) ||
                    (user.platform && user.platform.toLowerCase().includes(searchTerm))
                );
            }
            
            if (filteredUsers.length === 0) {
                const tr = document.createElement('tr');
                tr.innerHTML = '<td colspan="7" style="text-align: center;">검색 결과가 없습니다.</td>';
                tbody.appendChild(tr);
                return;
            }
            
            // 사용자 목록 표시
            filteredUsers.forEach(user => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${user.username}</td>
                    <td>${user.name}</td>
                    <td>${user.sellerName || '-'}</td>
                    <td>${user.salesType || '-'}</td>
                    <td>${user.platform || '-'}</td>
                    <td>${user.mainCategory || '-'}</td>
                    <td>
                        <button class="view-user" data-username="${user.username}">상세보기</button>
                        ${user.username !== 'admin' ? `<button class="delete-user" data-username="${user.username}">삭제</button>` : ''}
                    </td>
                `;
                tbody.appendChild(tr);
            });
            
            // 상세보기 버튼 이벤트 추가
            document.querySelectorAll('.view-user').forEach(btn => {
                btn.addEventListener('click', function() {
                    const username = this.getAttribute('data-username');
                    showUserDetail(username);
                });
            });
            
            // 삭제 버튼 이벤트 추가
            document.querySelectorAll('.delete-user').forEach(btn => {
                btn.addEventListener('click', function() {
                    const username = this.getAttribute('data-username');
                    if (confirm(`사용자 ${username}을 삭제하시겠습니까?`)) {
                        deleteUser(username);
                    }
                });
            });
        }
        
        // 가입 승인 목록 업데이트
        function updateApprovalsList() {
            const statusFilter = document.getElementById('approval-status-filter').value;
            const tbody = document.getElementById('approvals-table-body');
            tbody.innerHTML = '';
            
            // 사용자 필터링 (관리자 제외)
            let filteredUsers = users.filter(user => !user.isAdmin);
            
            // 상태별 필터링
            if (statusFilter !== 'all') {
                if (statusFilter === 'pending') {
                    filteredUsers = filteredUsers.filter(user => user.approved === undefined);
                } else if (statusFilter === 'approved') {
                    filteredUsers = filteredUsers.filter(user => user.approved === true);
                } else if (statusFilter === 'rejected') {
                    filteredUsers = filteredUsers.filter(user => user.approved === false);
                }
            }
            
            if (filteredUsers.length === 0) {
                const tr = document.createElement('tr');
                tr.innerHTML = '<td colspan="7" style="text-align: center;">해당하는 회원이 없습니다.</td>';
                tbody.appendChild(tr);
                return;
            }
            
            // 사용자 목록 표시 (가입일 기준 내림차순)
            filteredUsers.sort((a, b) => new Date(b.registrationDate) - new Date(a.registrationDate));
            
            // 사용자 목록 표시
            filteredUsers.forEach(user => {
                const tr = document.createElement('tr');
                
                let statusText = '';
                let actionButtons = '';
                
                if (user.approved === true) {
                    statusText = '<span class="status-chip status-approved">승인됨</span>';
                    actionButtons = `<button class="reject-approval" data-username="${user.username}">승인 취소</button>`;
                } else if (user.approved === false) {
                    statusText = '<span class="status-chip status-rejected">거절됨</span>';
                    actionButtons = `<button class="approve-user" data-username="${user.username}">승인하기</button>`;
                } else {
                    statusText = '<span class="status-chip status-pending">대기중</span>';
                    actionButtons = `
                        <button class="approve-user" data-username="${user.username}">승인</button>
                        <button class="reject-approval" data-username="${user.username}">거절</button>
                    `;
                }
                
                tr.innerHTML = `
                    <td>${user.username}</td>
                    <td>${user.name}</td>
                    <td>${user.email}</td>
                    <td>${user.businessNumber}</td>
                    <td>${user.registrationDate}</td>
                    <td>${statusText}</td>
                    <td>${actionButtons}</td>
                `;
                tbody.appendChild(tr);
            });
            
            // 승인 버튼 이벤트 추가
            document.querySelectorAll('.approve-user').forEach(btn => {
                btn.addEventListener('click', function() {
                    const username = this.getAttribute('data-username');
                    approveUser(username, true);
                });
            });
            
            // 거절 버튼 이벤트 추가
            document.querySelectorAll('.reject-approval').forEach(btn => {
                btn.addEventListener('click', function() {
                    const username = this.getAttribute('data-username');
                    approveUser(username, false);
                });
            });
        }
        
        // 사용자 승인/거절
        function approveUser(username, isApproved) {
            const user = users.find(u => u.username === username);
            if (!user) return;
            
            user.approved = isApproved;
            
            updateApprovalsList();
            updateUsersList();
            
            if (isApproved) {
                alert(`사용자 ${username}의 가입이 승인되었습니다.`);
            } else {
                alert(`사용자 ${username}의 가입이 거절되었습니다.`);
            }
        }
        
        // 사용자 삭제 함수
        function deleteUser(username) {
            const index = users.findIndex(user => user.username === username);
            if (index !== -1) {
                users.splice(index, 1);
                updateUsersList();
                updateApprovalsList();
                alert(`사용자 ${username}이 삭제되었습니다.`);
            }
        }
        
        // 사용자 상세 정보 표시
        function showUserDetail(username) {
            const user = users.find(user => user.username === username);
            if (!user) return;
            
            const modal = document.getElementById('userDetailModal');
            const content = document.getElementById('userDetailContent');
            
            // 매니저 및 물류시스템 텍스트 변환
            const hasManagerText = user.hasManager === 'yes' ? '있음' : '없음';
            let logisticsText = '없음';
            if (user.logistics === 'self') logisticsText = '자체';
            else if (user.logistics === '3pl') logisticsText = '3PL';
            
            content.innerHTML = `
                <div class="profile-section">
                    <h3>기본 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">아이디</div>
                        <div class="profile-value">${user.username}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">이름</div>
                        <div class="profile-value">${user.name}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">이메일</div>
                        <div class="profile-value">${user.email}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">전화번호</div>
                        <div class="profile-value">${user.phone}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">가입일</div>
                        <div class="profile-value">${user.registrationDate}</div>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h3>사업자 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">상호명</div>
                        <div class="profile-value">${user.businessName || '-'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">사업자등록번호</div>
                        <div class="profile-value">${user.businessNumber || '-'}</div>
                    </div>
                </div>
                
                <div class="profile-section">
                    <h3>셀러 정보</h3>
                    <div class="profile-row">
                        <div class="profile-label">판로유형</div>
                        <div class="profile-value">${user.salesType || '-'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">셀러명</div>
                        <div class="profile-value">${user.sellerName || '-'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">플랫폼</div>
                        <div class="profile-value">${user.platform || '-'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">주카테고리</div>
                        <div class="profile-value">${user.mainCategory || '-'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">구독자수</div>
                        <div class="profile-value">${user.subscribers || '0'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">월 방송횟수</div>
                        <div class="profile-value">${user.broadcastsPerMonth || '0'}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">매니저 보유 여부</div>
                        <div class="profile-value">${hasManagerText}</div>
                    </div>
                    <div class="profile-row">
                        <div class="profile-label">물류시스템</div>
                        <div class="profile-value">${logisticsText}</div>
                    </div>
                </div>
            `;
            
            modal.style.display = 'block';
        }
        
        // 모달 닫기
        function closeUserDetailModal() {
            document.getElementById('userDetailModal').style.display = 'none';
        }
        
        // 아이디 중복 확인 함수
        function checkUsername(username) {
            if (!username) {
                return {
                    available: false,
                    message: '아이디를 입력해주세요.'
                };
            }
            
            if (username.length < 4) {
                return {
                    available: false,
                    message: '아이디는 4자 이상이어야 합니다.'
                };
            }
            
            const exists = users.some(u => u.username === username);
            
            if (exists) {
                return {
                    available: false,
                    message: '이미 사용 중인 아이디입니다.'
                };
            }
            
            return {
                available: true,
                message: '사용 가능한 아이디입니다.'
            };
        }
        
        // 달력 생성 함수
        function renderCalendar() {
            const container = document.getElementById('calendar-days');
            container.innerHTML = '';
            
            // 날짜 표시 업데이트
            const monthDisplay = document.getElementById('current-month');
            monthDisplay.textContent = `${currentDate.getFullYear()}년 ${currentDate.getMonth() + 1}월`;
            
            // 요일 이름 추가
            const dayNames = ['일', '월', '화', '수', '목', '금', '토'];
            dayNames.forEach(day => {
                const dayNameElement = document.createElement('div');
                dayNameElement.className = 'calendar-day-name';
                dayNameElement.textContent = day;
                container.appendChild(dayNameElement);
            });
            
            // 해당 월의 첫 번째 날
            const firstDay = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1);
            // 해당 월의 마지막 날
            const lastDay = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0);
            
            // 첫 번째 날의 요일에 따라 앞쪽 빈칸 추가
            const firstDayOfWeek = firstDay.getDay();
            for (let i = 0; i < firstDayOfWeek; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day other-month';
                container.appendChild(emptyDay);
            }
            
            // 날짜 추가
            const today = new Date();
            today.setHours(0, 0, 0, 0); // 오늘 날짜 비교를 위해 시간 초기화
            
            for (let i = 1; i <= lastDay.getDate(); i++) {
                const day = document.createElement('div');
                day.className = 'calendar-day';
                
                const currentDayDate = new Date(currentDate.getFullYear(), currentDate.getMonth(), i);
                currentDayDate.setHours(0, 0, 0, 0); // 시간 초기화
                const dateString = currentDayDate.toISOString().split('T')[0];
                
                // 오늘 날짜 표시
                if (currentDayDate.getTime() === today.getTime()) {
                    day.classList.add('today');
                }
                
                // 선택된 날짜 표시
                if (selectedDate === dateString) {
                    day.classList.add('selected');
                }
                
                // 날짜 번호 추가
                const dayNumber = document.createElement('div');
                dayNumber.className = 'calendar-day-number';
                dayNumber.textContent = i;
                day.appendChild(dayNumber);
                
                // 날짜 클릭 이벤트
                if (currentDayDate >= today) {
                    day.addEventListener('click', () => {
                        selectDate(dateString);
                    });
                } else {
                    day.style.opacity = "0.5";
                    day.style.cursor = "not-allowed";
                }
                
                container.appendChild(day);
            }
            
            // 마지막 날의 요일에 따라 뒤쪽 빈칸 추가
            const lastDayOfWeek = lastDay.getDay();
            for (let i = lastDayOfWeek; i < 6; i++) {
                const emptyDay = document.createElement('div');
                emptyDay.className = 'calendar-day other-month';
                container.appendChild(emptyDay);
            }
        }
        
        // 날짜 선택 함수
        function selectDate(dateString) {
            selectedDate = dateString;
            
            // 달력에서 선택된 날짜 표시 업데이트
            document.querySelectorAll('#calendar-days .calendar-day').forEach(day => {
                day.classList.remove('selected');
            });
            
            // 선택된 날짜에 selected 클래스 추가
            const selectedDayEl = Array.from(document.querySelectorAll('#calendar-days .calendar-day:not(.other-month)')).find(day => {
                const dayNum = day.querySelector('.calendar-day-number').textContent;
                const dayDate = new Date(currentDate.getFullYear(), currentDate.getMonth(), parseInt(dayNum));
                const dayString = dayDate.toISOString().split('T')[0];
                return dayString === dateString;
            });
            
            if (selectedDayEl) {
                selectedDayEl.classList.add('selected');
            }
            
            // 시간대 표시
            displayTimeSlots(dateString);
        }
        
        // 시간대 표시 함수
        function displayTimeSlots(dateString) {
            const timePanel = document.getElementById('time-panel');
            const timePanelHeader = document.getElementById('time-panel-header');
            const timeSlotsContainer = document.getElementById('time-slots');
            
            timePanel.classList.add('active');
            
            // 헤더 설정
            const date = new Date(dateString);
            const formattedDate = `${date.getFullYear()}년 ${date.getMonth() + 1}월 ${date.getDate()}일`;
            timePanelHeader.textContent = `${formattedDate} 시간 선택`;
            
            // 시간대 슬롯 생성
            timeSlotsContainer.innerHTML = '';
            
            // 10시부터 18시까지 예약 가능 시간대 생성
            for (let hour = 10; hour <= 18; hour++) {
                const timeString = `${hour}:00`;
                const timeSlotKey = `${dateString}_${timeString}`;
                
                // 이미 해당 시간에 예약이 있는지 확인
                const existingReservation = reservations.find(r => 
                    r.date === dateString && 
                    r.time === timeString && 
                    (r.status === '예약 대기' || r.status === '예약 완료')
                );
                
                // 관리자가 시간대를 마감했는지 확인
                const isAvailable = timeSlotAvailability[timeSlotKey] !== false;
                
                // 예약 불가능한 경우 건너뛰기
                if (existingReservation || !isAvailable) {
                    const blockedSlot = document.createElement('div');
                    blockedSlot.className = 'time-slot';
                    blockedSlot.style.backgroundColor = '#f8d7da';
                    blockedSlot.style.color = '#721c24';
                    blockedSlot.style.cursor = 'not-allowed';
                    blockedSlot.textContent = `${timeString} - ${existingReservation ? '이미 예약됨' : '예약 불가'}`;
                    timeSlotsContainer.appendChild(blockedSlot);
                    continue;
                }
                
                const timeSlot = document.createElement('div');
                timeSlot.className = 'time-slot';
                timeSlot.textContent = `${timeString}`;
                
                // 예약 시간 클릭 이벤트
                timeSlot.addEventListener('click', function() {
                    // 실제 예약 처리
                    alert(`${formattedDate} ${timeString}에 예약이 신청되었습니다. 관리자 승인 후 확정됩니다.`);
                    
                    // 예약 추가
                    addReservation(dateString, timeString);
                    
                    // 내 예약 목록 업데이트
                    updateMyReservations();
                    
                    // 관리자 페이지 승인 대기 목록 업데이트
                    updatePendingReservations();
                    
                    // 시간대 다시 표시
                    displayTimeSlots(dateString);
                });
                
                timeSlotsContainer.appendChild(timeSlot);
            }
        }
        
        // 예약 추가 함수
        function addReservation(date, time) {
            if (!currentUser) return;
            
            // 새 예약 생성
            const newReservation = {
                id: Date.now().toString(),
                username: currentUser.username,
                date: date,
                time: time,
                status: '예약 대기', // 처음에는 대기 상태로 설정
                timestamp: new Date().toISOString()
            };
            
            // 예약 추가
            reservations.push(newReservation);
        }
        
        // 내 예약 목록 업데이트
        function updateMyReservations() {
            if (!currentUser) return;
            
            const container = document.getElementById('my-reservations-list');
            
            // 현재 사용자의 예약만 필터링
            const myReservations = reservations.filter(r => r.username === currentUser.username);
            
            if (myReservations.length === 0) {
                container.innerHTML = '<p style="text-align: center;">예약 내역이 없습니다.</p>';
                return;
            }
            
            // 내용 초기화
            container.innerHTML = '';
            
            // 예약 목록 표시 (최신순 정렬)
            myReservations.sort((a, b) => new Date(b.date) - new Date(a.date));
            
            myReservations.forEach(reservation => {
                const date = new Date(reservation.date);
                const formattedDate = `${date.getFullYear()}년 ${date.getMonth() + 1}월 ${date.getDate()}일`;
                
                let statusClass = 'reservation-item';
                let statusText = '';
                
                if (reservation.status === '예약 대기') {
                    statusClass += ' reservation-pending';
                    statusText = '<span class="badge badge-warning">승인 대기중</span>';
                } else if (reservation.status === '예약 완료') {
                    statusClass += ' reservation-approved';
                    statusText = '<span class="badge badge-success">승인됨</span>';
                } else if (reservation.status === '예약 거절') {
                    statusClass += ' reservation-rejected';
                    statusText = '<span class="badge badge-danger">거절됨</span>';
                }
                
                const item = document.createElement('div');
                item.className = statusClass;
                item.innerHTML = `
                    <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
                        <strong>${formattedDate} ${reservation.time}</strong>
                        ${statusText}
                    </div>
                    <button class="cancel-btn" data-id="${reservation.id}" ${reservation.status === '예약 거절' ? 'style="display:none;"' : ''}>예약 취소</button>
                `;
                
                container.appendChild(item);
            });
            
            // 취소 버튼 이벤트 처리
            document.querySelectorAll('.cancel-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = this.getAttribute('data-id');
                    if (confirm('이 예약을 취소하시겠습니까?')) {
                        // 예약 취소
                        reservations = reservations.filter(r => r.id !== id);
                        // 목록 업데이트
                        updateMyReservations();
                        updatePendingReservations();
                        updateReservationsList();
                    }
                });
            });
        }
        
        // 메인 탭 전환 함수
        function openTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            
            document.querySelectorAll('#main-tab-container .tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.getElementById(tabName).classList.add('active');
            document.querySelector(`#main-tab-container .tab[onclick="openTab('${tabName}')"]`).classList.add('active');
            
            if (tabName === 'profile') updateProfileInfo();
            if (tabName === 'reservation') updateReservationUI();
            if (tabName === 'admin' && currentUser?.isAdmin) {
                document.getElementById('admin-login-message').style.display = 'none';
                document.getElementById('admin-panel').style.display = 'block';
                // 첫 번째 관리자 탭을 활성화
                showAdminTab('admin-pending');
            }
        }
        
        // 예약 UI 업데이트
        function updateReservationUI() {
            const loginMessage = document.getElementById('login-message');
            const approvalPendingMessage = document.getElementById('approval-pending-message');
            const approvalRejectedMessage = document.getElementById('approval-rejected-message');
            const reservationContainer = document.getElementById('reservation-container');
            
            if (currentUser) {
                loginMessage.style.display = 'none';
                
                // 가입 승인 상태에 따라 UI 조정
                if (currentUser.approved === true) {
                    // 승인된 사용자
                    approvalPendingMessage.style.display = 'none';
                    approvalRejectedMessage.style.display = 'none';
                    reservationContainer.style.display = 'block';
                    renderCalendar();
                    updateMyReservations();
                } else if (currentUser.approved === false) {
                    // 거절된 사용자
                    approvalPendingMessage.style.display = 'none';
                    approvalRejectedMessage.style.display = 'block';
                    reservationContainer.style.display = 'none';
                } else {
                    // 승인 대기 중인 사용자
                    approvalPendingMessage.style.display = 'block';
                    approvalRejectedMessage.style.display = 'none';
                    reservationContainer.style.display = 'none';
                }
            } else {
                loginMessage.style.display = 'block';
                approvalPendingMessage.style.display = 'none';
                approvalRejectedMessage.style.display = 'none';
                reservationContainer.style.display = 'none';
            }
        }
        
        // 내 정보 업데이트
        function updateProfileInfo() {
            if (!currentUser) return;
            
            document.getElementById('profile-username').textContent = currentUser.username;
            document.getElementById('profile-name').textContent = currentUser.name;
            document.getElementById('profile-email').value = currentUser.email || '';
            document.getElementById('profile-phone').value = currentUser.phone || '';
            document.getElementById('profile-business-name').value = currentUser.businessName || '';
            document.getElementById('profile-business-number').value = currentUser.businessNumber || '';
            document.getElementById('profile-sales-type').value = currentUser.salesType || '';
            document.getElementById('profile-seller-name').value = currentUser.sellerName || '';
            document.getElementById('profile-platform').value = currentUser.platform || '';
            document.getElementById('profile-main-category').value = currentUser.mainCategory || '';
            document.getElementById('profile-subscribers').value = currentUser.subscribers || '';
            document.getElementById('profile-broadcasts-per-month').value = currentUser.broadcastsPerMonth || '';
            
            const hasManagerRadios = document.getElementsByName('profile-has-manager');
            for (let radio of hasManagerRadios) {
                if (radio.value === (currentUser.hasManager || 'no')) {
                    radio.checked = true;
                }
            }
            
            const logisticsRadios = document.getElementsByName('profile-logistics');
            for (let radio of logisticsRadios) {
                if (radio.value === (currentUser.logistics || 'none')) {
                    radio.checked = true;
                }
            }
        }
        
        // 내 정보 저장
        function saveProfileInfo() {
            if (!currentUser) return false;
            
            currentUser.email = document.getElementById('profile-email').value;
            currentUser.phone = document.getElementById('profile-phone').value;
            currentUser.businessName = document.getElementById('profile-business-name').value;
            currentUser.businessNumber = document.getElementById('profile-business-number').value;
            currentUser.salesType = document.getElementById('profile-sales-type').value;
            currentUser.sellerName = document.getElementById('profile-seller-name').value;
            currentUser.platform = document.getElementById('profile-platform').value;
            currentUser.mainCategory = document.getElementById('profile-main-category').value;
            currentUser.subscribers = document.getElementById('profile-subscribers').value;
            currentUser.broadcastsPerMonth = document.getElementById('profile-broadcasts-per-month').value;
            
            const hasManagerRadio = document.querySelector('input[name="profile-has-manager"]:checked');
            if (hasManagerRadio) {
                currentUser.hasManager = hasManagerRadio.value;
            }
            
            const logisticsRadio = document.querySelector('input[name="profile-logistics"]:checked');
            if (logisticsRadio) {
                currentUser.logistics = logisticsRadio.value;
            }
            
            return true;
        }
        
        // 사용자 권한에 따른 UI 업데이트
        function updateUIForUser() {
            // 로그인/가입 탭 표시 여부
            const loginTab = document.querySelector('.login-tab');
            const registerTab = document.querySelector('.register-tab');
            const logoutTab = document.querySelector('.logout-tab');
            
            if (currentUser) {
                // 로그인된 상태
                loginTab.style.display = 'none';
                registerTab.style.display = 'none';
                logoutTab.style.display = 'block';
            } else {
                // 로그아웃 상태
                loginTab.style.display = 'block';
                registerTab.style.display = 'block';
                logoutTab.style.display = 'none';
            }
            
            // 관리자/일반회원 탭 표시 여부
            const adminTab = document.querySelector('.admin-only');
            const profileTab = document.querySelector('.user-only');
            
            adminTab.style.display = currentUser && currentUser.isAdmin ? 'block' : 'none';
            profileTab.style.display = currentUser && !currentUser.isAdmin ? 'block' : 'none';
            
            // 로그인 페이지로 전환
            if (!currentUser) {
                openTab('login');
            }
        }
        
        // 이벤트 리스너 설정
        document.addEventListener('DOMContentLoaded', function() {
            // 샘플 예약 데이터 추가 (테스트용)
            addSampleReservations();
            
            // ESC 키로 모달 닫기
            window.addEventListener('keydown', function(e) {
                if (e.key === 'Escape') {
                    closeUserDetailModal();
                }
            });
            
            // 모달 외부 클릭 시 닫기
            window.addEventListener('click', function(e) {
                if (e.target === document.getElementById('userDetailModal')) {
                    closeUserDetailModal();
                }
            });
            
            // 달력 월 이동 버튼
            document.getElementById('prev-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() - 1);
                renderCalendar();
            });
            
            document.getElementById('next-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() + 1);
                renderCalendar();
            });
            
            // 관리자 달력 월 이동 버튼
            document.getElementById('admin-prev-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() - 1);
                renderAdminCalendar();
            });
            
            document.getElementById('admin-next-month').addEventListener('click', function() {
                currentDate.setMonth(currentDate.getMonth() + 1);
                renderAdminCalendar();
            });
            
            // 모든 시간대 마감/오픈 버튼
            document.getElementById('close-all-slots-button').addEventListener('click', function() {
                if (!adminSelectedDate) {
                    alert('날짜를 먼저 선택해주세요.');
                    return;
                }
                
                if (confirm('선택한 날짜의 모든 시간대를 예약 마감하시겠습니까?')) {
                    // 해당 날짜의 모든 시간대 마감
                    for (let hour = 10; hour <= 18; hour++) {
                        const timeString = `${hour}:00`;
                        const timeSlotKey = `${adminSelectedDate}_${timeString}`;
                        timeSlotAvailability[timeSlotKey] = false;
                    }
                    
                    // UI 업데이트
                    showAdminDayDetails(adminSelectedDate);
                }
            });
            
            document.getElementById('open-all-slots-button').addEventListener('click', function() {
                if (!adminSelectedDate) {
                    alert('날짜를 먼저 선택해주세요.');
                    return;
                }
                
                if (confirm('선택한 날짜의 모든 시간대를 예약 가능하게 하시겠습니까?')) {
                    // 해당 날짜의 모든 시간대 오픈
                    for (let hour = 10; hour <= 18; hour++) {
                        const timeString = `${hour}:00`;
                        const timeSlotKey = `${adminSelectedDate}_${timeString}`;
                        timeSlotAvailability[timeSlotKey] = true;
                    }
                    
                    // UI 업데이트
                    showAdminDayDetails(adminSelectedDate);
                }
            });
            
            // 아이디 중복 확인 버튼
            document.getElementById('check-username').addEventListener('click', function() {
                const username = document.getElementById('register-username').value;
                const result = checkUsername(username);
                
                const resultElement = document.getElementById('username-check-result');
                resultElement.textContent = result.message;
                
                if (result.available) {
                    resultElement.style.color = 'green';
                    usernameChecked = true;
                    document.getElementById('register-button').disabled = false;
                } else {
                    resultElement.style.color = 'red';
                    usernameChecked = false;
                    document.getElementById('register-button').disabled = true;
                }
            });
            
            // 아이디 입력 필드 변경 시 중복 확인 초기화
            document.getElementById('register-username').addEventListener('input', function() {
                usernameChecked = false;
                document.getElementById('username-check-result').textContent = '';
                document.getElementById('register-button').disabled = true;
            });
            
            // 회원가입 폼 제출
            document.getElementById('register-form').addEventListener('submit', function(e) {
                e.preventDefault();
                
                if (!usernameChecked) {
                    alert('아이디 중복 확인이 필요합니다.');
                    return;
                }
                
                const username = document.getElementById('register-username').value;
                const password = document.getElementById('register-password').value;
                const confirmPassword = document.getElementById('register-confirm-password').value;
                
                if (password !== confirmPassword) {
                    alert('비밀번호가 일치하지 않습니다.');
                    return;
                }
                
                if (!document.getElementById('agree-terms').checked || !document.getElementById('agree-privacy').checked) {
                    alert('이용약관과 개인정보 수집 및 이용에 동의해주세요.');
                    return;
                }
                
                const name = document.getElementById('register-name').value;
                const phone = document.getElementById('register-phone').value;
                const email = document.getElementById('register-email').value;
                const businessName = document.getElementById('register-business-name').value;
                const businessNumber = document.getElementById('register-business-number').value;
                
                const newUser = { 
                    username,
                    password, 
                    name, 
                    phone, 
                    email, 
                    isAdmin: false,
                    registrationDate: new Date().toISOString().split('T')[0],
                    businessName,
                    businessNumber,
                    approved: undefined, // 승인 대기 상태
                    salesType: '',
                    sellerName: '',
                    platform: '',
                    mainCategory: '',
                    subscribers: 0,
                    broadcastsPerMonth: 0,
                    hasManager: 'no',
                    logistics: 'none'
                };
                
                users.push(newUser);
                
                alert('회원가입이 완료되었습니다. 관리자의 승인 후 서비스 이용이 가능합니다.');
                this.reset();
                
                usernameChecked = false;
                document.getElementById('username-check-result').textContent = '';
                document.getElementById('register-button').disabled = true;
                
                // 가입 승인 목록 갱신
                updateApprovalsList();
                
                openTab('login');
            });
            
            // 로그인 폼 제출
            document.getElementById('login-form').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const username = document.getElementById('login-username').value;
                const password = document.getElementById('login-password').value;
                
                const user = users.find(u => u.username === username && u.password === password);
                
                if (user) {
                    currentUser = user;
                    alert('로그인 성공!');
                    
                    updateUIForUser();
                    
                    if (user.isAdmin) {
                        openTab('admin');
                    } else {
                        openTab('reservation');
                    }
                } else {
                    alert('아이디 또는 비밀번호가 잘못되었습니다.');
                }
            });
            
            // 내 정보 폼 제출
            document.getElementById('profile-form').addEventListener('submit', function(e) {
                e.preventDefault();
                
                if (saveProfileInfo()) {
                    alert('정보가 성공적으로 저장되었습니다.');
                    // 가입자 정보 목록 갱신
                    updateUsersList();
                }
            });
            
            // 검색 기능
            document.getElementById('user-search').addEventListener('input', function() {
                updateUsersList();
            });
            
            // 사용자 목록 새로고침 버튼
            document.getElementById('refresh-users').addEventListener('click', function() {
                document.getElementById('user-search').value = '';
                updateUsersList();
            });
            
            // 승인 목록 새로고침 버튼
            document.getElementById('refresh-approvals').addEventListener('click', function() {
                updateApprovalsList();
            });
            
            // 승인 상태 필터 변경
            document.getElementById('approval-status-filter').addEventListener('change', function() {
                updateApprovalsList();
            });
            
            // 예약 목록 새로고침 버튼
            document.getElementById('refresh-list').addEventListener('click', function() {
                updateReservationsList();
            });
            
            // 예약 상태 필터 변경
            document.getElementById('status-filter').addEventListener('change', function() {
                updateReservationsList();
            });
            
            // 날짜 필터 변경
            document.getElementById('date-range-filter').addEventListener('change', function() {
                updateReservationsList();
            });
            
            // 초기 목록 표시
            updateUsersList();
            updateApprovalsList();
            updatePendingReservations();
            updateReservationsList();
        });
    </script>
</body>
</html>
