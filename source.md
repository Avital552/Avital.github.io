<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מעקב הוצאות אישי</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            font-size: 16px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            padding: 30px;
            text-align: center;
            color: white;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .tabs {
            display: flex;
            background: #f8f9fa;
            border-bottom: 2px solid #e9ecef;
        }

        .tab {
            flex: 1;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            font-weight: bold;
            border: none;
            background: none;
            font-size: 16px;
            color: #495057;
            transition: all 0.3s ease;
        }

        .tab:hover {
            background: #e9ecef;
            color: #007bff;
        }

        .tab.active {
            background: white;
            color: #007bff;
            border-bottom: 3px solid #007bff;
        }

        .content {
            padding: 30px;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .section {
            margin-bottom: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 12px;
            border-right: 5px solid #007bff;
        }

        .section h3 {
            margin-bottom: 20px;
            color: #333;
            font-size: 1.3em;
        }

        .balance-card {
            background: linear-gradient(135deg, #28a745, #20c997);
            color: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 25px;
            box-shadow: 0 8px 25px rgba(40, 167, 69, 0.3);
        }

        .balance-card.negative {
            background: linear-gradient(135deg, #dc3545, #fd7e14);
        }

        .balance-amount {
            font-size: 2.5em;
            font-weight: bold;
            margin: 15px 0;
        }

        .balance-details {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }

        .balance-item h4 {
            font-size: 0.9em;
            margin-bottom: 5px;
            opacity: 0.9;
        }

        .balance-item span {
            font-size: 1.2em;
            font-weight: bold;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-row {
            display: flex;
            gap: 20px;
            align-items: end;
        }

        .form-row .form-group {
            flex: 1;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #495057;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e9ecef;
            border-radius: 8px;
            font-size: 16px;
            font-family: inherit;
            transition: border-color 0.3s ease, box-shadow 0.3s ease;
        }

        input:focus, select:focus {
            border-color: #007bff;
            outline: none;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
        }

        .btn {
            background: linear-gradient(135deg, #007bff, #0056b3);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 123, 255, 0.4);
        }

        .btn-danger {
            background: linear-gradient(135deg, #dc3545, #c82333);
        }

        .btn-danger:hover {
            box-shadow: 0 5px 15px rgba(220, 53, 69, 0.4);
        }

        .btn-small {
            padding: 8px 15px;
            font-size: 14px;
            margin: 0 5px;
        }

        .filter-section {
            background: #e3f2fd;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .filter-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }

        /* [שונה] עטיפה לגלילת טבלאות */
        .table-wrapper {
            overflow-x: auto;
        }

        .expenses-table {
            width: 100%;
            min-width: 600px; /* רוחב מינימלי להפעלת גלילה */
            border-collapse: collapse;
            margin-top: 20px;
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .expenses-table th,
        .expenses-table td {
            padding: 15px;
            text-align: right;
            border-bottom: 1px solid #e9ecef;
            white-space: nowrap; /* מונע שבירת שורות בטבלה */
        }

        .expenses-table th {
            background: linear-gradient(135deg, #007bff, #0056b3);
            color: white;
            font-weight: 600;
        }

        .expenses-table tr:hover {
            background: #f8f9fa;
        }

        .expenses-table tr:last-child td {
            border-bottom: none;
        }

        .category-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .category-item {
            background: white;
            padding: 20px;
            border-radius: 10px;
            border: 2px solid #e9ecef;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: border-color 0.3s ease;
        }

        .category-item:hover {
            border-color: #007bff;
        }

        .message {
            display: none;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
            font-weight: 600;
            text-align: center;
        }

        .message.success {
            background: #d4edda;
            color: #155724;
            border: 2px solid #c3e6cb;
        }

        .message.error {
            background: #f8d7da;
            color: #721c24;
            border: 2px solid #f5c6cb;
        }

        /* ----- סגנונות לתפריט תחתון בנייד  ----- */
        .mobile-nav {
            display: none; 
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #ffffff;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
            justify-content: space-around;
            padding: 8px 0;
            z-index: 1000;
        }

        .mobile-nav-btn {
            background: none;
            border: none;
            color: #8a8a8a;
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 12px;
            font-weight: 600;
            gap: 4px;
            padding: 5px 10px;
            cursor: pointer;
            transition: color 0.2s ease-in-out;
            flex-grow: 1;
        }

        .mobile-nav-btn .icon {
            font-size: 24px;
        }

        .mobile-nav-btn.active {
            color: #007bff;
        }
        
        /* [חדש] סגנון למודאל תפריט "עוד" */
        .modal {
            display: none; 
            position: fixed; 
            z-index: 2000; 
            left: 0;
            top: 0;
            width: 100%; 
            height: 100%; 
            background-color: rgba(0,0,0,0.5); 
        }

        .modal-content {
            background-color: #fefefe;
            margin: auto;
            position: fixed;
            bottom: 80px; /* מעל התפריט הראשי */
            width: 90%;
            max-width: 400px;
            left: 50%;
            transform: translateX(-50%);
            animation-name: slideUp;
            animation-duration: 0.4s;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 5px 25px rgba(0,0,0,0.2);
        }
        
        @keyframes slideUp {
            from {transform: translate(-50%, 50px); opacity: 0}
            to {transform: translate(-50%, 0); opacity: 1}
        }

        .more-menu-list {
            list-style: none;
            padding: 10px 0;
            margin: 0;
        }

        .more-menu-list li {
            padding: 15px 20px;
            font-size: 1.1em;
            border-bottom: 1px solid #f0f0f0;
            cursor: pointer;
        }
        .more-menu-list li:last-child {
            border-bottom: none;
        }
        .more-menu-list li:hover {
            background-color: #f8f9fa;
        }
        
        @media (max-width: 768px) {
            body {
                padding: 10px;
                padding-bottom: 80px; 
            }

            .content {
                padding: 15px;
            }
            
            .tabs {
                display: none;
            }

            .mobile-nav {
                display: flex;
            }
            
            .form-row {
                flex-direction: column;
            }
            
            .filter-grid {
                grid-template-columns: 1fr;
            }
            
            .balance-details {
                flex-direction: column;
                gap: 15px;
            }
            
            .category-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>💰 מעקב הוצאות אישי</h1>
            <p>נהל את הכסף שלך בקלות ובפרטיות מלאה</p>
        </div>

        <div class="tabs">
            <button class="tab active" onclick="showTab('dashboard')">דשבורד</button>
            <button class="tab" onclick="showTab('categories')">קטגוריות</button>
            <button class="tab" onclick="showTab('expenses')">רשימת הוצאות</button>
            <button class="tab" onclick="showTab('fixed-expenses')">הוצאות קבועות</button>
            <button class="tab" onclick="showTab('settings')">הגדרות</button>
        </div>

        <div class="content">
            <div id="dashboard" class="tab-content active">
                <div id="balance-display"></div>

                <div class="section">
                    <h3>⚙️ הגדרות מהירות</h3>
                    <div class="form-row">
                        <div class="form-group">
                            <label>חודש נוכחי</label>
                            <input type="month" id="dashboard-current-month">
                        </div>
                        <div class="form-group">
                            <label>הכנסה חודשית (₪)</label>
                            <input type="number" id="dashboard-monthly-income" min="0" step="0.01">
                        </div>
                    </div>
                </div>

                <div class="section">
                    <h3>➕ הוספת הוצאה מהירה</h3>
                    <div id="dashboard-expense-message" class="message"></div>
                    <form id="dashboard-expense-form">
                        <div class="form-row">
                            <div class="form-group">
                                <label>סכום (₪)</label>
                                <input type="number" id="dashboard-expense-amount" required min="0" step="0.01">
                            </div>
                            <div class="form-group">
                                <label>קטגוריה</label>
                                <select id="dashboard-expense-category" required></select>
                            </div>
                            <div class="form-group">
                                <label>תאריך</label>
                                <input type="date" id="dashboard-expense-date" required>
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group">
                                <label>תיאור (אופציונלי)</label>
                                <input type="text" id="dashboard-expense-description">
                            </div>
                            <div class="form-group" style="display: flex; align-items: end; padding-bottom: 12px; justify-content: center;">
                                <input type="checkbox" id="dashboard-expense-is-fixed" style="width: 18px; height: 18px; margin-left: 10px;">
                                <label for="dashboard-expense-is-fixed" style="margin-bottom: 0; white-space: nowrap;">הוצאה קבועה</label>
                            </div>
                            <div class="form-group">
                                <button type="submit" class="btn">הוסף הוצאה</button>
                            </div>
                        </div>
                    </form>
                </div>

                <div class="section">
                    <h3>📊 הוצאות אחרונות</h3>
                    <div class="table-wrapper">
                        <table class="expenses-table" id="dashboard-recent-expenses">
                            <thead>
                                <tr>
                                    <th>סכום</th>
                                    <th>קטגוריה</th>
                                    <th>תיאור</th>
                                    <th>תאריך</th>
                                    <th>פעולות</th>
                                </tr>
                            </thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="categories" class="tab-content">
                <div class="section">
                    <h3>🏷️ הוספת קטגוריה חדשה</h3>
                    <div id="category-message" class="message"></div>
                    <form id="category-form">
                        <div class="form-row">
                            <div class="form-group">
                                <label>שם הקטגוריה</label>
                                <input type="text" id="category-name" required placeholder="למשל: תחבורה">
                            </div>
                            <div class="form-group">
                                <button type="submit" class="btn">הוסף קטגוריה</button>
                            </div>
                        </div>
                    </form>
                </div>

                <div class="section">
                    <h3>📋 קטגוריות קיימות</h3>
                    <div id="category-list" class="category-grid"></div>
                </div>
            </div>

            <div id="settings" class="tab-content">
                <div class="section">
                    <h3>📅 ניהול חודש נוכחי</h3>
                    <div id="month-message" class="message"></div>
                    <div class="form-group">
                        <label>בחר חודש לעבודה</label>
                        <input type="month" id="settings-current-month" required>
                    </div>
                </div>

                <div class="section">
                    <h3>💵 הגדרת הכנסה חודשית</h3>
                    <div id="income-message" class="message"></div>
                    <div class="form-group">
                        <label>הכנסה חודשית (₪)</label>
                        <input type="number" id="settings-monthly-income" min="0" step="0.01" placeholder="0">
                    </div>
                </div>

                <div class="section">
                    <h3>💾 גיבוי והחזרת נתונים</h3>
                    <p style="margin-bottom: 20px; color: #6c757d;">
                        שמור את הנתונים שלך כקובץ גיבוי או טען קובץ גיבוי קיים
                    </p>
                    <div class="form-row">
                        <div class="form-group">
                            <button class="btn" onclick="exportData()">ייצא נתונים</button>
                        </div>
                        <div class="form-group">
                            <input type="file" id="import-file" accept=".json" style="display: none;">
                            <button class="btn" onclick="document.getElementById('import-file').click()">ייבא נתונים</button>
                        </div>
                    </div>
                </div>
            </div>

            <div id="expenses" class="tab-content">
                <div class="section">
                    <h3>➕ הוספת הוצאה חדשה</h3>
                    <div id="expenses-form-message" class="message"></div>
                    <form id="expense-form">
                        <div class="form-row">
                            <div class="form-group">
                                <label>סכום (₪)</label>
                                <input type="number" id="expense-amount" required min="0" step="0.01">
                            </div>
                            <div class="form-group">
                                <label>קטגוריה</label>
                                <select id="expense-category" required></select>
                            </div>
                            <div class="form-group">
                                <label>תאריך</label>
                                <input type="date" id="expense-date" required>
                            </div>
                        </div>
                        <div class="form-row">
                            <div class="form-group">
                                <label>תיאור (אופציונלי)</label>
                                <input type="text" id="expense-description">
                            </div>
                             <div class="form-group" style="display: flex; align-items: end; padding-bottom: 12px; justify-content: center;">
                                <input type="checkbox" id="expense-is-fixed" style="width: 18px; height: 18px; margin-left: 10px;">
                                <label for="expense-is-fixed" style="margin-bottom: 0; white-space: nowrap;">הוצאה קבועה</label>
                            </div>
                            <div class="form-group">
                                <input type="hidden" id="expense-id">
                                <button type="submit" class="btn" id="expense-submit">הוסף הוצאה</button>
                            </div>
                        </div>
                    </form>
                </div>

                <div class="section">
                    <h3>🔍 סינון הוצאות</h3>
                    <div class="filter-section">
                        <div class="filter-grid">
                            <div class="form-group">
                                <label>חפש בתיאור</label>
                                <input type="text" id="filter-description" placeholder="חפש בתיאור...">
                            </div>
                            <div class="form-group">
                                <label>קטגוריה</label>
                                <select id="filter-category">
                                    <option value="">כל הקטגוריות</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label>סכום מינימום</label>
                                <input type="number" id="filter-min-amount" placeholder="0" min="0" step="0.01">
                            </div>
                            <div class="form-group">
                                <label>סכום מקסימום</label>
                                <input type="number" id="filter-max-amount" placeholder="∞" min="0" step="0.01">
                            </div>
                        </div>
                        <button class="btn" onclick="clearFilters()">נקה כל הסינונים</button>
                    </div>
                </div>

                <div class="section">
                    <h3>📝 רשימת הוצאות מפורטת</h3>
                    <div id="expenses-message" class="message"></div>
                    <div class="table-wrapper">
                        <table class="expenses-table" id="expense-list">
                            <thead>
                                <tr>
                                    <th>סכום</th>
                                    <th>קטגוריה</th>
                                    <th>תיאור</th>
                                    <th>תאריך</th>
                                    <th>פעולות</th>
                                </tr>
                            </thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div id="fixed-expenses" class="tab-content">
                 <div class="section">
                    <h3>🔁 רשימת הוצאות קבועות</h3>
                    <div id="fixed-expenses-message" class="message"></div>
                    <div class="table-wrapper">
                        <table class="expenses-table" id="fixed-expense-list">
                            <thead>
                                <tr>
                                    <th>סכום</th>
                                    <th>קטגוריה</th>
                                    <th>תיאור</th>
                                    <th>תאריך</th>
                                    <th>פעולות</th>
                                </tr>
                            </thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- [שונה] תפריט ניווט תחתון למובייל -->
    <div class="mobile-nav">
        <button class="mobile-nav-btn active" data-tab="dashboard" onclick="showTab('dashboard')">
            <span class="icon">📊</span>
            <span>דשבורד</span>
        </button>
        <button class="mobile-nav-btn" data-tab="categories" onclick="showTab('categories')">
            <span class="icon">🏷️</span>
            <span>קטגוריות</span>
        </button>
        <button class="mobile-nav-btn" data-tab="expenses" onclick="showTab('expenses')">
            <span class="icon">📝</span>
            <span>הוצאות</span>
        </button>
        <button class="mobile-nav-btn" id="more-menu-btn">
            <span class="icon">☰</span>
            <span>עוד</span>
        </button>
    </div>

    <!-- [חדש] מודאל לתפריט "עוד" -->
    <div id="more-menu-modal" class="modal">
        <div class="modal-content">
            <ul class="more-menu-list">
                <li data-tab="fixed-expenses">🔁 הוצאות קבועות</li>
                <li data-tab="settings">⚙️ הגדרות</li>
            </ul>
        </div>
    </div>

    <script>
        const defaultData = {
            categories: ["מחיה", "פרויקט אישי", "בניה ושיפוצים"],
            currentMonth: new Date().toISOString().slice(0, 7),
            monthlyData: {}
        };

        let data = JSON.parse(localStorage.getItem('expenseData')) || defaultData;
        if (!data.categories) data.categories = defaultData.categories;
        if (!data.currentMonth) data.currentMonth = defaultData.currentMonth;
        if (!data.monthlyData) data.monthlyData = {};

        function saveData() {
            localStorage.setItem('expenseData', JSON.stringify(data));
        }

        function showMessage(elementId, message, isSuccess = true) {
            const messageEl = document.getElementById(elementId);
            messageEl.textContent = message;
            messageEl.className = `message ${isSuccess ? 'success' : 'error'}`;
            messageEl.style.display = 'block';
            setTimeout(() => messageEl.style.display = 'none', 3000);
        }

        // [שונה] מעבר בין טאבים עם לוגיקה לתפריט "עוד"
        function showTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            document.querySelectorAll('.tab, .mobile-nav-btn').forEach(btn => btn.classList.remove('active'));
            
            document.getElementById(tabName).classList.add('active');
            
            const desktopTab = document.querySelector(`.tab[onclick="showTab('${tabName}')"]`);
            if (desktopTab) desktopTab.classList.add('active');
            
            const directMobileButton = document.querySelector(`.mobile-nav-btn[data-tab="${tabName}"]`);
            if (directMobileButton) {
                directMobileButton.classList.add('active');
            } else {
                document.getElementById('more-menu-btn').classList.add('active');
            }
            
            const actionMap = {
                'dashboard': updateDashboard,
                'categories': updateCategories,
                'settings': updateSettings,
                'expenses': updateExpenses,
                'fixed-expenses': updateFixedExpenses
            };
            if(actionMap[tabName]) actionMap[tabName]();
        }

        // --- [חדש] לוגיקת תפריט "עוד" ---
        const moreMenuModal = document.getElementById('more-menu-modal');
        const moreMenuBtn = document.getElementById('more-menu-btn');
        
        moreMenuBtn.onclick = () => moreMenuModal.style.display = 'block';
        
        moreMenuModal.querySelectorAll('.more-menu-list li').forEach(item => {
            item.onclick = () => {
                const tabName = item.getAttribute('data-tab');
                showTab(tabName);
                moreMenuModal.style.display = 'none';
            };
        });
        
        window.onclick = function(event) {
            if (event.target == moreMenuModal) moreMenuModal.style.display = 'none';
        }

        // --- שאר פונקציות האפליקציה ---

        function updateCategories() {
            const categorySelects = [
                document.getElementById('dashboard-expense-category'),
                document.getElementById('expense-category'),
                document.getElementById('filter-category')
            ];
            
            categorySelects.forEach(select => {
                const currentValue = select.value;
                select.innerHTML = select.id === 'filter-category' ? '<option value="">כל הקטגוריות</option>' : '<option value="" disabled>בחר קטגוריה</option>';
                data.categories.forEach(category => {
                    const option = document.createElement('option');
                    option.value = category;
                    option.textContent = category;
                    select.appendChild(option);
                });
                select.value = currentValue;
            });

            const categoryList = document.getElementById('category-list');
            categoryList.innerHTML = '';
            data.categories.forEach(category => {
                const categoryItem = document.createElement('div');
                categoryItem.className = 'category-item';
                categoryItem.innerHTML = `<span style="font-weight: 600;">${category}</span><button class="btn btn-danger btn-small" onclick="deleteCategory('${category}')">מחק</button>`;
                categoryList.appendChild(categoryItem);
            });
        }

        function deleteCategory(category) {
            if (confirm(`האם למחוק את הקטגוריה "${category}"?`)) {
                data.categories = data.categories.filter(c => c !== category);
                Object.values(data.monthlyData).forEach(month => {
                    if (month.expenses) {
                        month.expenses = month.expenses.filter(e => e.category !== category);
                    }
                });
                saveData();
                updateAllTabs();
                showMessage('category-message', 'הקטגוריה נמחקה בהצלחה');
            }
        }

        function updateBalance() {
            const month = data.currentMonth;
            const monthData = data.monthlyData[month] || { income: 0, expenses: [] };
            const income = monthData.income || 0;
            const expensesTotal = monthData.expenses?.reduce((sum, e) => sum + parseFloat(e.amount), 0) || 0;
            const balance = income - expensesTotal;
            
            document.getElementById('balance-display').innerHTML = `
                <div class="balance-card ${balance < 0 ? 'negative' : ''}">
                    <h2>מאזן לחודש ${formatMonth(month)}</h2>
                    <div class="balance-amount">${balance.toLocaleString('he-IL')} ₪</div>
                    <div class="balance-details">
                        <div class="balance-item"><h4>הכנסות</h4><span>₪ ${income.toLocaleString('he-IL')}</span></div>
                        <div class="balance-item"><h4>הוצאות</h4><span>₪ ${expensesTotal.toLocaleString('he-IL')}</span></div>
                        <div class="balance-item"><h4>מספר הוצאות</h4><span>${monthData.expenses?.length || 0}</span></div>
                    </div>
                </div>`;
        }

        function updateDashboard() {
            updateBalance();
            document.getElementById('dashboard-current-month').value = data.currentMonth;
            const monthData = data.monthlyData[data.currentMonth] || { income: 0 };
            document.getElementById('dashboard-monthly-income').value = monthData.income || 0;
            document.getElementById('dashboard-expense-date').value = new Date().toISOString().slice(0, 10);
            updateRecentExpenses();
        }

        function updateRecentExpenses() {
            const month = data.currentMonth;
            const monthData = data.monthlyData[month] || { expenses: [] };
            const tbody = document.getElementById('dashboard-recent-expenses').querySelector('tbody');
            tbody.innerHTML = '';
            const expenses = monthData.expenses?.slice(-5).reverse() || [];
            
            if (expenses.length === 0) {
                 tbody.innerHTML = `<tr><td colspan="5" style="text-align:center; padding: 20px;">אין הוצאות להצגה החודש.</td></tr>`;
                 return;
            }
            expenses.forEach(expense => {
                const row = tbody.insertRow();
                row.innerHTML = `
                    <td>${parseFloat(expense.amount).toFixed(2)} ₪</td>
                    <td>${expense.category}</td>
                    <td>${expense.description || '-'}</td>
                    <td>${formatDate(expense.date)}</td>
                    <td><button class="btn btn-small" onclick="editExpense('${expense.id}')">ערוך</button><button class="btn btn-danger btn-small" onclick="deleteExpense('${expense.id}')">מחק</button></td>`;
            });
        }

        function updateSettings() {
            document.getElementById('settings-current-month').value = data.currentMonth;
            const monthData = data.monthlyData[data.currentMonth] || { income: 0 };
            document.getElementById('settings-monthly-income').value = monthData.income || 0;
        }

        function updateExpenses() {
            const month = data.currentMonth;
            const monthData = data.monthlyData[month] || { expenses: [] };
            const tbody = document.getElementById('expense-list').querySelector('tbody');
            
            const descriptionFilter = document.getElementById('filter-description').value.toLowerCase();
            const categoryFilter = document.getElementById('filter-category').value;
            const minAmount = parseFloat(document.getElementById('filter-min-amount').value) || 0;
            const maxAmount = parseFloat(document.getElementById('filter-max-amount').value) || Infinity;

            tbody.innerHTML = '';
            const filteredExpenses = monthData.expenses?.filter(expense => 
                (expense.description || '').toLowerCase().includes(descriptionFilter) &&
                (!categoryFilter || expense.category === categoryFilter) &&
                expense.amount >= minAmount &&
                expense.amount <= maxAmount
            ) || [];

            if (filteredExpenses.length === 0) {
                 tbody.innerHTML = `<tr><td colspan="5" style="text-align:center; padding: 20px;">לא נמצאו הוצאות התואמות לסינון.</td></tr>`;
                 return;
            }
            filteredExpenses.forEach(expense => {
                const row = tbody.insertRow();
                row.innerHTML = `
                    <td>${parseFloat(expense.amount).toFixed(2)} ₪</td>
                    <td>${expense.category}</td>
                    <td>${expense.description || '-'}</td>
                    <td>${formatDate(expense.date)}</td>
                    <td><button class="btn btn-small" onclick="editExpense('${expense.id}')">ערוך</button><button class="btn btn-danger btn-small" onclick="deleteExpense('${expense.id}')">מחק</button></td>`;
            });
            document.getElementById('expense-date').value = new Date().toISOString().slice(0, 10);
        }
        
        function updateFixedExpenses() {
            const month = data.currentMonth;
            const monthData = data.monthlyData[month] || { expenses: [] };
            const tbody = document.getElementById('fixed-expense-list').querySelector('tbody');
            
            document.querySelector('#fixed-expenses h3').innerHTML = `🔁 רשימת הוצאות קבועות לחודש ${formatMonth(month)}`;
            tbody.innerHTML = '';
            const fixedExpenses = monthData.expenses?.filter(expense => expense.isFixed) || [];

            if (fixedExpenses.length === 0) {
                 tbody.innerHTML = `<tr><td colspan="5" style="text-align:center; padding: 20px;">אין הוצאות קבועות לחודש זה.</td></tr>`;
            } else {
                 fixedExpenses.forEach(expense => {
                    const row = tbody.insertRow();
                    row.innerHTML = `
                        <td>${parseFloat(expense.amount).toFixed(2)} ₪</td>
                        <td>${expense.category}</td>
                        <td>${expense.description || '-'}</td>
                        <td>${formatDate(expense.date)}</td>
                        <td><button class="btn btn-small" onclick="editExpense('${expense.id}')">ערוך</button><button class="btn btn-danger btn-small" onclick="deleteExpense('${expense.id}')">מחק</button></td>`;
                });
            }
        }

        function editExpense(id) {
            const month = data.currentMonth;
            const expense = data.monthlyData[month]?.expenses.find(e => e.id === id);
            if (expense) {
                showTab('expenses');
                document.getElementById('expense-amount').value = expense.amount;
                document.getElementById('expense-category').value = expense.category;
                document.getElementById('expense-description').value = expense.description;
                document.getElementById('expense-date').value = expense.date;
                document.getElementById('expense-is-fixed').checked = expense.isFixed || false;
                document.getElementById('expense-id').value = id;
                document.getElementById('expense-submit').textContent = 'שמור שינויים';
                document.querySelector('#expenses .section').scrollIntoView({ behavior: 'smooth' });
            }
        }

        function deleteExpense(id) {
            if (confirm('האם למחוק את ההוצאה הזו?')) {
                const month = data.currentMonth;
                if (data.monthlyData[month]?.expenses) {
                    data.monthlyData[month].expenses = data.monthlyData[month].expenses.filter(e => e.id !== id);
                    saveData();
                    updateAllTabs();
                    showMessage('expenses-message', 'ההוצאה נמחקה בהצלחה');
                }
            }
        }

        function clearFilters() {
            document.getElementById('filter-description').value = '';
            document.getElementById('filter-category').value = '';
            document.getElementById('filter-min-amount').value = '';
            document.getElementById('filter-max-amount').value = '';
            updateExpenses();
        }
        
        function updateAllTabs() {
            const activeTab = document.querySelector('.tab-content.active')?.id || 'dashboard';
            updateCategories();
            showTab(activeTab); 
        }

        function formatDate(dateString) {
            if (!dateString) return '';
            return new Date(dateString).toLocaleDateString('he-IL');
        }

        function formatMonth(monthString) {
            if(!monthString) return '';
            const [year, month] = monthString.split('-');
            return new Date(year, month - 1).toLocaleDateString('he-IL', { year: 'numeric', month: 'long' });
        }

        function exportData() {
            const dataStr = JSON.stringify(data, null, 2);
            const blob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `expense_data_${new Date().toISOString().slice(0, 10)}.json`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            showMessage('income-message', 'הנתונים יוצאו בהצלחה');
        }

        function setupEventListeners() {
            ['dashboard-expense-form', 'expense-form'].forEach(formId => {
                document.getElementById(formId).addEventListener('submit', (e) => {
                    e.preventDefault();
                    const isDashboard = formId.startsWith('dashboard');
                    const prefix = isDashboard ? 'dashboard-' : '';
                    
                    const amount = parseFloat(document.getElementById(`${prefix}expense-amount`).value);
                    const category = document.getElementById(`${prefix}expense-category`).value;
                    const description = document.getElementById(`${prefix}expense-description`).value;
                    const dateValue = document.getElementById(`${prefix}expense-date`).value;
                    const isFixed = document.getElementById(`${prefix}expense-is-fixed`)?.checked || false;
                    const id = isDashboard ? null : document.getElementById('expense-id').value;

                    if (!category) {
                        showMessage(`${prefix}expense-message`, 'יש לבחור קטגוריה', false);
                        return;
                    }
                    if (!data.monthlyData[data.currentMonth]) {
                        data.monthlyData[data.currentMonth] = { income: 0, expenses: [] };
                    }

                    if (id) { 
                        const expense = data.monthlyData[data.currentMonth].expenses.find(ex => ex.id === id);
                        if (expense) Object.assign(expense, {amount, category, description, date: dateValue, isFixed});
                    } else { 
                        data.monthlyData[data.currentMonth].expenses.push({ id: Date.now().toString(), amount, category, description, date: dateValue, isFixed });
                    }

                    saveData();
                    updateAllTabs();
                    e.target.reset();
                    document.getElementById(`${prefix}expense-date`).value = new Date().toISOString().slice(0, 10);
                    if (!isDashboard) {
                         document.getElementById('expense-id').value = '';
                         document.getElementById('expense-submit').textContent = 'הוסף הוצאה';
                    }
                    showMessage(isDashboard ? 'dashboard-expense-message' : 'expenses-form-message', `ההוצאה ${id ? 'עודכנה' : 'נוספה'} בהצלחה`);
                });
            });

            document.getElementById('category-form').addEventListener('submit', (e) => {
                e.preventDefault();
                const name = document.getElementById('category-name').value.trim();
                if (name && !data.categories.includes(name)) {
                    data.categories.push(name);
                    saveData();
                    updateCategories();
                    e.target.reset();
                    showMessage('category-message', 'הקטגוריה נוספה בהצלחה');
                } else {
                    showMessage('category-message', 'הקטגוריה כבר קיימת או שהשם ריק', false);
                }
            });

            ['dashboard-current-month', 'settings-current-month'].forEach(id => {
                document.getElementById(id).addEventListener('change', (e) => {
                    data.currentMonth = e.target.value;
                    saveData();
                    updateAllTabs();
                    document.getElementById('dashboard-current-month').value = data.currentMonth;
                    document.getElementById('settings-current-month').value = data.currentMonth;
                    showMessage(id.startsWith('dashboard') ? 'dashboard-expense-message' : 'month-message', 'החודש עודכן בהצלחה');
                });
            });

            ['dashboard-monthly-income', 'settings-monthly-income'].forEach(id => {
                document.getElementById(id).addEventListener('input', (e) => {
                    if (!data.monthlyData[data.currentMonth]) {
                        data.monthlyData[data.currentMonth] = { income: 0, expenses: [] };
                    }
                    const newIncome = parseFloat(e.target.value) || 0;
                    data.monthlyData[data.currentMonth].income = newIncome;
                    saveData();
                    updateBalance();
                    document.getElementById('settings-monthly-income').value = newIncome;
                    document.getElementById('dashboard-monthly-income').value = newIncome;
                });
            });

            ['filter-description', 'filter-category', 'filter-min-amount', 'filter-max-amount'].forEach(id => {
                document.getElementById(id).addEventListener('input', updateExpenses);
            });

            document.getElementById('import-file').addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (event) => {
                        try {
                            const importedData = JSON.parse(event.target.result);
                            if (importedData.categories && importedData.monthlyData !== undefined) {
                                data = importedData;
                                saveData();
                                updateAllTabs();
                                showMessage('income-message', 'הנתונים יובאו בהצלחה');
                            } else {
                                showMessage('income-message', 'קובץ לא תקין', false);
                            }
                        } catch (error) {
                            showMessage('income-message', 'שגיאה בייבוא הנתונים', false);
                        }
                    };
                    reader.readAsText(file);
                }
            });
        }

        document.addEventListener('DOMContentLoaded', function() {
            setupEventListeners();
            updateAllTabs();
            showTab('dashboard');
        });
    </script>
</body>
</html>
