# daftarhadirsiswa


Berikut adalah **satu file HTML lengkap** yang sudah mencakup semua fitur yang Anda minta sebelumnya:

1.  **Login Guru & Siswa** (Dengan Password Otomatis).
2.  **Manajemen Kelas** (Tambah, Edit, Hapus, Reset Password).
3.  **Input Absensi Manual** (Per Tanggal & Kelas).
4.  **Menu Kelas X.1 s/d X.5** (Sesuai permintaan).
5.  **Tombol Hapus Berfungsi** (Sudah diperbaiki).
6.  **Fitur Download CSV** (Untuk dikirim via WA/Email di menu Riwayat).

Silakan simpan kode di bawah ini dengan nama file `absensi.html` lalu buka di browser (Chrome/Edge).

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Absensi - SMAN 1 KAPONTORI</title>
    <style>
        :root {
            --primary: #2563eb;
            --secondary: #1e40af;
            --accent: #f59e0b;
            --danger: #ef4444;
            --success: #10b981;
            --text-light: #f3f4f6;
            --text-dark: #1f2937;
            --glass-bg: rgba(255, 255, 255, 0.95);
            --glass-border: rgba(255, 255, 255, 0.2);
            --shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; transition: all 0.3s ease; }
        body { width: 100%; min-height: 100vh; color: var(--text-dark); position: relative; overflow-x: hidden; }

        /* Background Changer */
        #bg-container { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; background-size: cover; background-position: center; transition: background-image 1.5s ease-in-out; }
        .overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.5); }

        .container { max-width: 1200px; margin: 0 auto; padding: 20px; position: relative; z-index: 1; }
        .hidden { display: none !important; }

        /* Header & Nav */
        header { display: flex; justify-content: space-between; align-items: center; background: var(--glass-bg); backdrop-filter: blur(8px); padding: 15px 25px; border-radius: 15px; box-shadow: var(--shadow); margin-bottom: 20px; flex-wrap: wrap; gap: 15px; }
        .brand h1 { font-size: 1.5rem; color: var(--primary); font-weight: 800; }
        .brand p { font-size: 0.9rem; color: #666; }
        .header-controls { display: flex; gap: 15px; align-items: center; }
        .lang-switch, .logout-btn { padding: 8px 15px; border: none; border-radius: 8px; cursor: pointer; font-weight: 600; font-size: 0.9rem; }
        .lang-switch { background: var(--accent); color: white; }
        .logout-btn { background: var(--danger); color: white; }

        /* Info Bar */
        .info-bar { display: flex; justify-content: space-between; background: rgba(255,255, 255, 0.95); padding: 10px 20px; border-radius: 10px; margin-bottom: 20px; font-size: 0.95rem; box-shadow: 0 2px 10px rgba(0,0,0,0.1); flex-wrap: wrap; }
        .online-users { display: flex; align-items: center; gap: 10px; }
        .user-avatar { width: 30px; height: 30px; background: #ddd; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 0.7rem; font-weight: bold; color: #555; border: 2px solid var(--primary); }

        /* Login Card */
        .login-card { background: var(--glass-bg); backdrop-filter: blur(10px); padding: 40px; border-radius: 20px; box-shadow: var(--shadow); max-width: 400px; margin: 80px auto; text-align: center; }
        .login-card h2 { margin-bottom: 20px; color: var(--secondary); }
        .form-group { margin-bottom: 20px; text-align: left; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: 600; }
        .form-group input, .form-group select { width: 100%; padding: 12px; border: 2px solid #ddd; border-radius: 8px; font-size: 1rem; }
        .form-group input:focus, .form-group select:focus { outline: none; border-color: var(--primary); }
        .btn-primary { width: 100%; padding: 12px; background: var(--primary); color: white; border: none; border-radius: 8px; font-size: 1rem; font-weight: bold; cursor: pointer; }
        .btn-primary:hover { background: var(--secondary); }
        .btn-secondary { width: 100%; padding: 10px; background: #94a3b8; color: white; border: none; border-radius: 8px; margin-top: 10px; cursor: pointer; }

        /* Dashboard Layout */
        .dashboard-grid { display: grid; grid-template-columns: 1fr 3fr; gap: 20px; }
        @media (max-width: 768px) { .dashboard-grid { grid-template-columns: 1fr; } }

        .card { background: var(--glass-bg); padding: 20px; border-radius: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .card h3 { border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; color: var(--primary); }

        /* Student View Specifics */
        .attendance-action { text-align: center; padding: 30px; }
        .big-btn { width: 100%; padding: 25px; font-size: 1.5rem; background: linear-gradient(135deg, var(--success), #059669); color: white; border: none; border-radius: 15px; cursor: pointer; box-shadow: 0 4px 15px rgba(16, 185, 129, 0.4); margin-bottom: 15px; }
        .big-btn:active { transform: scale(0.98); }

        .status-badge { display: inline-block; padding: 5px 12px; border-radius: 20px; font-size: 0.85rem; font-weight: bold; }
        .status-present { background: #d1fae5; color: #065f46; }

        /* Teacher View Specifics */
        .tabs { display: flex; gap: 10px; margin-bottom: 20px; overflow-x: auto; }
        .tab-btn { padding: 10px 20px; background: white; border: 1px solid #ddd; border-radius: 8px; cursor: pointer; white-space: nowrap; }
        .tab-btn.active { background: var(--primary); color: white; border-color: var(--primary); }
        .tab-btn.management { background: var(--accent); color: white; border-color: var(--accent); }
        .tab-btn.manual { background: var(--success); color: white; border-color: var(--success); }

        table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
        th, td { padding: 12px; text-align: left; border-bottom: 1px solid #ddd; }
        th { background: #f8f9fa; color: #444; font-weight: 700; }
        tr:hover { background: #f1f5f9; }

        /* Password Style */
        .pass-cell { font-family: 'Courier New', Courier, monospace; background: #f1f5f9; padding: 4px 8px; border-radius: 4px; color: #333; font-weight: bold; letter-spacing: 1px; }

        /* Management & Class Cards */
        .class-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 15px; margin-bottom: 25px; }
        .class-card { background: white; border: 2px solid #e5e7eb; border-radius: 12px; padding: 20px; text-align: center; cursor: pointer; font-weight: bold; font-size: 1.2rem; color: var(--text-dark); transition: transform 0.2s, border-color 0.2s; }
        .class-card:hover, .class-card.active { border-color: var(--primary); background: #eff6ff; transform: translateY(-5px); }
        .class-card.active { color: var(--primary); }

        .btn-small { padding: 5px 10px; border-radius: 5px; border: none; cursor: pointer; font-size: 0.8rem; color: white; margin-right: 5px; }
        .btn-danger { background: var(--danger); }
        .btn-edit { background: var(--accent); }
        .btn-pass { background: #64748b; font-size: 0.7rem; }
        .btn-download { background: #0ea5e9; color: white; border: none; border-radius: 5px; padding: 5px 10px; cursor: pointer; font-size: 0.8rem; display: inline-flex; align-items: center; gap: 5px; }

        /* Manual Attendance Specifics */
        .manual-controls { display: flex; gap: 15px; align-items: flex-end; margin-bottom: 20px; background: #f8fafc; padding: 15px; border-radius: 10px; flex-wrap: wrap; }
        .manual-controls .form-group { margin-bottom: 0; flex: 1; min-width: 150px; }
        
        .status-radio-group { display: flex; gap: 10px; }
        .radio-label { display: flex; align-items: center; cursor: pointer; font-size: 0.9rem; }
        .radio-label input { margin-right: 5px; accent-color: var(--primary); }

        .recap-summary { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .summary-box { background: #f0f9ff; padding: 15px; border-radius: 10px; text-align: center; border: 1px solid #bae6fd; }
        .summary-box h4 { font-size: 2rem; color: var(--primary); }

        /* Toast */
        #toast { visibility: hidden; min-width: 250px; background-color: #333; color: #fff; text-align: center; border-radius: 8px; padding: 16px; position: fixed; z-index: 100; right: 30px; bottom: 30px; font-size: 17px; box-shadow: 0 4px 12px rgba(0,0,0,0.3); }
        #toast.show { visibility: visible; animation: fadein 0.5s, fadeout 0.5s 2.5s; }
        @keyframes fadein { from {bottom: 0; opacity: 0;} to {bottom: 30px; opacity: 1;} }
        @keyframes fadeout { from {bottom: 30px; opacity: 1;} to {bottom: 0; opacity: 0;} }
    </style>
</head>
<body>

    <!-- Dynamic Background -->
    <div id="bg-container"><div class="overlay"></div></div>

    <div class="container">
        <!-- Header -->
        <header>
            <div class="brand">
                <h1 id="school-name">SMAN 1 KAPONTORI</h1>
                <p id="subject-info">Mata Pelajaran: Informatika</p>
                <small id="teacher-info">Guru: Unggul Menawan, S.Pd.</small>
            </div>
            <div class="header-controls">
                <div id="clock-display" style="font-weight: bold; font-size: 1.1rem; margin-right: 10px;">--:--:--</div>
                <button class="lang-switch" onclick="toggleLanguage()" id="btn-lang">EN</button>
                <button class="logout-btn hidden" id="btn-logout" onclick="logout()">Keluar</button>
            </div>
        </header>

        <!-- Info Bar -->
        <div class="info-bar">
            <div id="date-display">Loading Date...</div>
            <div class="online-users">
                <span id="online-label">Online: </span>
                <div id="online-list" style="display: flex; gap: 5px;"></div>
            </div>
        </div>

        <!-- Login View -->
        <main id="view-login">
            <div class="login-card">
                <h2 id="login-title">Selamat Datang</h2>
                <p id="current-year-display" style="margin-bottom: 20px; color: #666;"></p>
                
                <div class="form-group">
                    <label for="username" id="lbl-username">Username (Nama Siswa)</label>
                    <input type="text" id="username" placeholder="Masukkan nama atau username">
                </div>
                <div class="form-group">
                    <label for="password" id="lbl-password">Password</label>
                    <input type="password" id="password" placeholder="Masukkan password">
                </div>
                <button class="btn-primary" onclick="handleLogin()" id="btn-login">Masuk</button>
                <p style="margin-top: 15px; font-size: 0.75rem; color: #666;">
                    Guru: guru / guru123 <br> Siswa: Nama Lengkap / Password (Dari Guru)
                </p>
            </div>
        </main>

        <!-- Student View -->
        <main id="view-student" class="hidden dashboard-grid">
            <aside>
                <div class="card">
                    <h3 id="profile-title">Profil Siswa</h3>
                    <p><strong>Nama:</strong> <span id="student-name-display">Loading...</span></p>
                    <p><strong>Kelas:</strong> <span id="student-class-display">Loading...</span></p>
                    <p><strong>Mapel:</strong> Informatika</p>
                    <p><strong>Guru:</strong> Unggul Menawan, S.Pd.</p>
                </div>
                <div class="card">
                    <h3 id="my-history-title">Riwayat Saya</h3>
                    <button class="btn-download" style="margin-bottom:10px; width:100%;" onclick="downloadStudentCSV()">📥 Download Riwayat (CSV)</button>
                    <ul id="student-history-list" style="list-style: none; padding-left: 0; font-size: 0.9rem;"></ul>
                </div>
            </aside>
            
            <section>
                <div class="card attendance-action">
                    <h2 id="welcome-student">Halo!</h2>
                    <p style="margin-bottom: 20px;" id="today-date">Tanggal Hari Ini</p>
                    <button class="big-btn" onclick="studentCheckIn()" id="btn-checkin">HADIR SEKARANG</button>
                    <p id="last-attendance-msg" style="margin-top: 10px; font-weight: bold; color: #666;"></p>
                </div>
            </section>
        </main>

        <!-- Teacher View -->
        <main id="view-teacher" class="hidden">
            <div class="card">
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
                    <h3 id="dashboard-title">Dashboard Guru</h3>
                    <div><span class="status-badge status-present">Unggul Menawan, S.Pd.</span></div>
                </div>

                <div class="tabs">
                    <button class="tab-btn active" onclick="switchTeacherTab('realtime')" id="tab-realtime">Real-time</button>
                    <button class="tab-btn manual" onclick="switchTeacherTab('manual-attendance')" id="tab-manual-attendance">Absensi Siswa</button>
                    <button class="tab-btn" onclick="switchTeacherTab('history')" id="tab-history">Riwayat Lengkap</button>
                    <button class="tab-btn" onclick="switchTeacherTab('monthly')" id="tab-monthly">Rekapan Bulanan</button>
                    <button class="tab-btn" onclick="switchTeacherTab('yearly')" id="tab-yearly">Rekapan Tahunan</button>
                    <button class="tab-btn management" onclick="switchTeacherTab('management')" id="tab-management">Manajemen Kelas</button>
                </div>

                <!-- Tab Content: Realtime -->
                <div id="content-realtime">
                    <h4 id="lbl-today-attendance">Absensi Hari Ini (Auto)</h4>
                    <div style="overflow-x: auto; margin-top: 10px;">
                        <table id="table-realtime">
                            <thead><tr><th id="th-time">Jam</th><th id="th-name">Nama Siswa</th><th id="th-status">Status</th></tr></thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>

                <!-- Tab Content: MANUAL ATTENDANCE -->
                <div id="content-manual-attendance" class="hidden">
                    <div class="manual-controls">
                        <div class="form-group">
                            <label for="manual-date" id="lbl-date-select">Tanggal</label>
                            <input type="date" id="manual-date">
                        </div>
                        <div class="form-group">
                            <label for="manual-class" id="lbl-class-select">Kelas</label>
                            <select id="manual-class">
                                <option value="">-- Pilih Kelas --</option>
                            </select>
                        </div>
                        <button class="btn-primary" style="width: auto; margin-bottom: 20px;" onclick="loadManualAttendanceTable()" id="btn-load-table">Tampilkan</button>
                    </div>

                    <div style="overflow-x: auto; margin-bottom: 20px;">
                        <table id="table-manual">
                            <thead>
                                <tr>
                                    <th style="width: 50px;">No</th>
                                    <th id="th-name-manual">Nama Siswa</th>
                                    <th id="th-status-manual">Status Kehadiran</th>
                                </tr>
                            </thead>
                            <tbody id="tbody-manual">
                                <tr><td colspan="3" style="text-align:center; color: #666;">Silakan pilih Tanggal dan Kelas untuk memuat data.</td></tr>
                            </tbody>
                        </table>
                    </div>

                    <div style="text-align: right;">
                        <button class="btn-primary" onclick="saveManualAttendance()" id="btn-save-manual">Simpan Absensi</button>
                    </div>
                </div>

                <!-- Tab Content: History -->
                <div id="content-history" class="hidden">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
                        <h4 id="lbl-history">Riwayat Absensi (Detail Detik)</h4>
                        <button class="btn-download" onclick="downloadTeacherCSV()">📥 Download Data (CSV)</button>
                    </div>
                    <div style="overflow-x: auto; margin-top: 10px; max-height: 400px;">
                        <table id="table-history">
                            <thead><tr><th id="th-date">Tanggal</th><th id="th-time-detail">Waktu (Detik)</th><th id="th-name-detail">Nama</th><th id="th-status-detail">Status</th></tr></thead>
                            <tbody></tbody>
                        </table>
                    </div>
                </div>

                <!-- Tab Content: Monthly -->
                <div id="content-monthly" class="hidden">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
                        <h4 id="lbl-monthly">Rekapan Bulanan</h4>
                        <button class="btn-download" onclick="downloadTeacherCSV()">📥 Download (CSV)</button>
                    </div>
                    <div style="margin-bottom: 15px;">
                        <select id="month-select" onchange="renderMonthlyRecap()" style="padding: 5px; border-radius: 5px;"></select>
                        <select id="year-select-month" onchange="renderMonthlyRecap()" style="padding: 5px; border-radius: 5px;"></select>
                    </div>
                    <div class="recap-summary" id="monthly-summary"></div>
                    <div style="overflow-x: auto;"><table id="table-monthly"><thead><tr><th>Nama Siswa</th><th>Hadir</th><th>Sakit/Izin</th><th>Alfa</th><th>Total%</th></tr></thead><tbody></tbody></table></div>
                </div>

                <!-- Tab Content: Yearly -->
                <div id="content-yearly" class="hidden">
                    <h4 id="lbl-yearly">Rekapan Tahunan</h4>
                    <div style="margin-bottom: 15px;"><select id="year-select-year" onchange="renderYearlyRecap()" style="padding: 5px; border-radius: 5px;"></select></div>
                    <div class="recap-summary" id="yearly-summary"></div>
                    <div style="overflow-x: auto;"><table id="table-yearly"><thead><tr><th>Nama Siswa</th><th>Total Hadir</th><th>Presentase</th></tr></thead><tbody></tbody></table></div>
                </div>

                <!-- Tab Content: MANAGEMENT -->
                <div id="content-management" class="hidden">
                    <div class="class-grid" id="class-card-container"></div>
                    <div style="display: grid; grid-template-columns: 1fr 2fr; gap: 20px;">
                        <div style="background: #f8fafc; padding: 20px; border-radius: 10px; border: 1px dashed #cbd5e1;">
                            <h4 style="margin-bottom: 15px; color: var(--primary);" id="form-title">Tambah Siswa Baru</h4>
                            <div class="form-group"><label>Nama Lengkap</label><input type="text" id="new-student-name" placeholder="Nama Siswa..."></div>
                            <div class="form-group"><label>Pilih Kelas</label><select id="new-student-class"></select></div>
                            <button class="btn-primary" onclick="addOrUpdateStudent()" id="btn-save-student">+ Tambah Siswa</button>
                            <button class="btn-secondary hidden" onclick="cancelEdit()" id="btn-cancel-edit">Batal Edit</button>
                            <p style="margin-top:10px; font-size:0.8rem; color:#666;">*Data disimpan otomatis di browser.</p>
                            
                            <hr style="margin: 20px 0; border: 0; border-top: 1px solid #ddd;">
                            <button class="btn-danger" onclick="resetSystemData()" style="width:100%; background-color: #991b1b; font-size:0.9rem;">Reset Semua Data (Pabrik)</button>
                        </div>
                        <div>
                            <h4 style="margin-bottom: 10px;">Data Siswa <span id="current-class-label" style="font-weight:normal; color: #666;"></span></h4>
                            <div style="overflow-x: auto; max-height: 400px;">
                                <table id="table-students">
                                    <thead><tr><th>ID</th><th>Nama</th><th>Kelas</th><th>Password</th><th>Aksi</th></tr></thead>
                                    <tbody></tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </main>
        
        <footer style="text-align: center; margin-top: 30px; color: white; padding: 10px; font-size: 0.8rem;">
            &copy; <span id="footer-year"></span> SMAN 1 KAPONTORI - Informatika
        </footer>
    </div>

    <!-- Toast -->
    <div id="toast">Notifikasi</div>

    <script>
        // --- DATA & STATE ---
        const STORAGE_KEYS = { STUDENTS: 'sman1_students', ATTENDANCE: 'sman1_attendance' };

        // Utility to generate random password
        function generateRandomPassword() {
            const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
            let password = "";
            for (let i = 0; i < 6; i++) {
                password += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return password;
        }

        const state = {
            lang: 'id',
            currentUser: null,
            backgroundIndex: 0,
            selectedClassFilter: null,
            editingStudentId: null,
            bgUrls: [
                'https://picsum.photos/seed/school1/1920/1080',
                'https://picsum.photos/seed/tech2/1920/1080',
                'https://picsum.photos/seed/library3/1920/1080',
                'https://picsum.photos/seed/class4/1920/1080'
            ],
            // ONLY CLASSES X.1 TO X.5
            classes: [
                { id: 'X.1', name: 'Kelas X.1' },
                { id: 'X.2', name: 'Kelas X.2' },
                { id: 'X.3', name: 'Kelas X.3' },
                { id: 'X.4', name: 'Kelas X.4' },
                { id: 'X.5', name: 'Kelas X.5' }
            ],
            // DEMO STUDENTS (Specific names removed, IDs start from 5)
            students: [
                { id: 5, name: "Eko Prasetyo", class: "X.3", password: generateRandomPassword() },
                { id: 6, name: "Fajar Nugraha", class: "X.3", password: generateRandomPassword() },
                { id: 8, name: "Hadi Kusuma", class: "X.5", password: generateRandomPassword() }
            ],
            attendance: [] 
        };

        function saveData() {
            localStorage.setItem(STORAGE_KEYS.STUDENTS, JSON.stringify(state.students));
            localStorage.setItem(STORAGE_KEYS.ATTENDANCE, JSON.stringify(state.attendance));
        }

        function initMockData() {
            const now = new Date();
            state.students.forEach(student => {
                for(let i=1; i<now.getDate(); i++) {
                    if(Math.random() > 0.2) {
                        const day = new Date(now.getFullYear(), now.getMonth(), i, 7 + Math.floor(Math.random()*2), Math.floor(Math.random()*59), Math.floor(Math.random()*59));
                        state.attendance.push({
                            studentId: student.id,
                            name: student.name,
                            timestamp: day.getTime(),
                            status: 'Hadir'
                        });
                    }
                }
            });
        }

        const i18n = {
            id: {
                loginTitle: "Selamat Datang", lblUser: "Username (Nama)", lblPass: "Password", btnLogin: "Masuk", btnLogout: "Keluar",
                subject: "Mata Pelajaran: Informatika", online: "Online: ", dashboard: "Dashboard Guru",
                realtime: "Real-time", manualAtt: "Absensi Siswa", history: "Riwayat Lengkap", monthly: "Rekapan Bulanan", yearly: "Rekapan Tahunan", management: "Manajemen Kelas",
                thTime: "Jam", thName: "Nama", thStatus: "Status", thPass: "Password",
                thDate: "Tanggal", thTimeDetail: "Waktu (Detik)",
                lblToday: "Absensi Hari Ini (Auto)", lblManualAtt: "Input Absensi Manual",
                lblHistory: "Riwayat Absensi (Detail Detik)", lblMonthly: "Rekapan Bulanan", lblYearly: "Rekapan Tahunan",
                lblDateSel: "Tanggal", lblClassSel: "Kelas", thNameMan: "Nama Siswa", thStatusMan: "Status Kehadiran",
                btnLoad: "Tampilkan", btnSave: "Simpan Absensi", btnResetPass: "Reset",
                checkIn: "HADIR SEKARANG", welcomeStud: "Halo,",
                msgSuccess: "Absensi Berhasil Dicatat!", msgError: "Username atau Password Salah!", msgSaved: "Data Absensi Disimpan!",
                msgAddSuccess: "Siswa Berhasil Ditambahkan & Disimpan!", msgUpdateSuccess: "Data Siswa Diperbarui!", msgDelSuccess: "Siswa Berhasil Dihapus!",
                msgPassReset: "Password siswa berhasil di-reset.", msgDownload: "Mengunduh Data...",
                msgResetConfirm: "PERINGATAN: Semua data siswa dan absensi akan dihapus permanen dan dikembalikan ke kondisi awal. Lanjutkan?",
                welcome: "Selamat Datang di SMAN 1 KAPONTORI", teacherName: "Guru: Unggul Menawan, S.Pd.",
                stHadir: "Hadir", stSakit: "Sakit", stIzin: "Izin", stAlfa: "Alfa",
                formAdd: "Tambah Siswa Baru", formEdit: "Edit Data Siswa"
            },
            en: {
                loginTitle: "Welcome", lblUser: "Username (Name)", lblPass: "Password", btnLogin: "Login", btnLogout: "Logout",
                subject: "Subject: Informatics", online: "Online: ", dashboard: "Teacher Dashboard",
                realtime: "Real-time", manualAtt: "Student Attendance", history: "Full History", monthly: "Monthly Recap", yearly: "Yearly Recap", management: "Class Management",
                thTime: "Time", thName: "Name", thStatus: "Status", thPass: "Password",
                thDate: "Date", thTimeDetail: "Time (Seconds)",
                lblToday: "Today's Attendance (Auto)", lblManualAtt: "Manual Attendance Input",
                lblHistory: "Attendance History (Seconds Detail)", lblMonthly: "Monthly Recap", lblYearly: "Yearly Recap",
                lblDateSel: "Date", lblClassSel: "Class", thNameMan: "Student Name", thStatusMan: "Attendance Status",
                btnLoad: "Load", btnSave: "Save Attendance", btnResetPass: "Reset",
                checkIn: "CHECK IN NOW", welcomeStud: "Hello,",
                msgSuccess: "Attendance Recorded Successfully!", msgError: "Wrong Username or Password!", msgSaved: "Attendance Data Saved!",
                msgAddSuccess: "Student Added & Saved Successfully!", msgUpdateSuccess: "Student Data Updated!", msgDelSuccess: "Student Deleted Successfully!",
                msgPassReset: "Student password has been reset.", msgDownload: "Downloading Data...",
                msgResetConfirm: "WARNING: All student and attendance data will be permanently deleted and reset to default. Continue?",
                welcome: "Welcome to SMAN 1 KAPONTORI", teacherName: "Teacher: Unggul Menawan, S.Pd.",
                stHadir: "Present", stSakit: "Sick", stIzin: "Permission", stAlfa: "Absent",
                formAdd: "Add New Student", formEdit: "Edit Student Data"
            }
        };

        // --- CSV EXPORT FUNCTIONS (NEW) ---
        function downloadTeacherCSV() {
            let dataToExport = state.attendance;
            dataToExport.sort((a,b) => b.timestamp - a.timestamp);

            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "ID Siswa,Nama Siswa,Tanggal,Jam,Kelas,Status\n"; 

            dataToExport.forEach(row => {
                const d = new Date(row.timestamp);
                const dateStr = d.toLocaleDateString('id-ID'); 
                const timeStr = d.toLocaleTimeString('id-ID', {hour12: false}); 
                
                const student = state.students.find(s => s.id === row.studentId);
                const studentClass = student ? student.class : "Unknown";
                const safeName = row.name.replace(/,/g, " ");

                csvContent += `${row.studentId},${safeName},${dateStr},${timeStr},${studentClass},${row.status}\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Data_Absensi_Full_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showToast(i18n[state.lang].msgDownload);
        }

        function downloadStudentCSV() {
            if(!state.currentUser) return;

            const myLogs = state.attendance.filter(a => a.studentId === state.currentUser.id).sort((a,b) => b.timestamp - a.timestamp);
            
            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "ID,Nama Lengkap,Tanggal,Jam,Status\n";

            myLogs.forEach(row => {
                const d = new Date(row.timestamp);
                const dateStr = d.toLocaleDateString('id-ID');
                const timeStr = d.toLocaleTimeString('id-ID', {hour12: false});
                const safeName = row.name.replace(/,/g, " ");
                
                csvContent += `${row.studentId},${safeName},${dateStr},${timeStr},${row.status}\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Riwayat_Absensi_${state.currentUser.name.replace(/\s+/g, '_')}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);

            showToast(i18n[state.lang].msgDownload);
        }

        // --- CORE FUNCTIONS ---
        function init() {
            const savedStudents = localStorage.getItem(STORAGE_KEYS.STUDENTS);
            if (savedStudents) {
                state.students = JSON.parse(savedStudents);
            }

            const savedAttendance = localStorage.getItem(STORAGE_KEYS.ATTENDANCE);
            if (savedAttendance) {
                state.attendance = JSON.parse(savedAttendance);
            } else {
                initMockData();
            }

            const year = new Date().getFullYear();
            document.getElementById('current-year-display').innerText = year;
            document.getElementById('footer-year').innerText = year;
            
            const todayStr = new Date().toISOString().split('T')[0];
            document.getElementById('manual-date').value = todayStr;

            setInterval(updateClock, 1000);
            updateClock();

            setInterval(changeBackground, 20000);
            changeBackground();

            setInterval(simulateOnlineUsers, 5000);
            simulateOnlineUsers();

            populateDropdowns();
        }

        function updateClock() {
            const now = new Date();
            const timeStr = now.toLocaleTimeString('id-ID', { hour12: false });
            document.getElementById('clock-display').innerText = timeStr;
            const dateOptions = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const dateStr = now.toLocaleDateString('id-ID', dateOptions);
            document.getElementById('date-display').innerText = dateStr;
            if (state.currentUser && state.currentUser.name) document.getElementById('today-date').innerText = dateStr;
        }

        function changeBackground() {
            const container = document.getElementById('bg-container');
            container.style.backgroundImage = `url('${state.bgUrls[state.backgroundIndex]}')`;
            state.backgroundIndex = (state.backgroundIndex + 1) % state.bgUrls.length;
        }

        function simulateOnlineUsers() {
            const list = document.getElementById('online-list');
            list.innerHTML = '';
            const shuffled = [...state.students].sort(() => 0.5 - Math.random());
            const selected = shuffled.slice(0, Math.floor(Math.random() * 3) + 3);
            selected.forEach(s => {
                const avatar = document.createElement('div');
                avatar.className = 'user-avatar';
                avatar.title = s.name;
                avatar.innerText = s.name.substring(0,2).toUpperCase();
                list.appendChild(avatar);
            });
            const teacher = document.createElement('div');
            teacher.className = 'user-avatar'; teacher.style.background = '#bfdbfe'; teacher.title = "Unggul Menawan, S.Pd."; teacher.innerText = "UM";
            list.appendChild(teacher);
        }

        function toggleLanguage() {
            state.lang = state.lang === 'id' ? 'en' : 'id';
            document.getElementById('btn-lang').innerText = state.lang === 'id' ? 'EN' : 'ID';
            applyLanguage();
        }

        function applyLanguage() {
            const t = i18n[state.lang];
            document.getElementById('login-title').innerText = t.loginTitle;
            document.getElementById('lbl-username').innerText = t.lblUser;
            document.getElementById('lbl-password').innerText = t.lblPass;
            document.getElementById('btn-login').innerText = t.btnLogin;
            document.getElementById('btn-logout').innerText = t.btnLogout;
            document.getElementById('subject-info').innerText = t.subject;
            document.getElementById('online-label').innerText = t.online;
            document.getElementById('dashboard-title').innerText = t.dashboard;
            document.getElementById('tab-realtime').innerText = t.realtime;
            document.getElementById('tab-manual-attendance').innerText = t.manualAtt;
            document.getElementById('tab-history').innerText = t.history;
            document.getElementById('tab-monthly').innerText = t.monthly;
            document.getElementById('tab-yearly').innerText = t.yearly;
            document.getElementById('tab-management').innerText = t.management;
            
            document.getElementById('th-time').innerText = t.thTime; document.getElementById('th-name').innerText = t.thName; document.getElementById('th-status').innerText = t.thStatus;
            document.getElementById('lbl-today-attendance').innerText = t.lblToday;
            document.getElementById('lbl-history').innerText = t.lblHistory;
            document.getElementById('lbl-monthly').innerText = t.lblMonthly;
            document.getElementById('lbl-yearly').innerText = t.lblYearly;
            document.getElementById('lbl-date-select').innerText = t.lblDateSel;
            document.getElementById('lbl-class-select').innerText = t.lblClassSel;
            document.getElementById('btn-load-table').innerText = t.btnLoad;
            document.getElementById('btn-save-manual').innerText = t.btnSave;
            document.getElementById('th-name-manual').innerText = t.thNameMan;
            document.getElementById('th-status-manual').innerText = t.thStatusMan;
            document.getElementById('checkIn').innerText = t.checkIn;
            document.getElementById('welcome-student').innerText = t.welcomeStud;
            document.getElementById('teacher-info').innerText = t.teacherName;
            
            if(state.currentUser && typeof state.currentUser === 'object') {
                document.getElementById('welcome-student').innerText = t.welcomeStud + " " + state.currentUser.name + "!";
            }

            if(state.editingStudentId) {
                document.getElementById('form-title').innerText = t.formEdit;
            } else {
                document.getElementById('form-title').innerText = t.formAdd;
            }
        }

        function showToast(message) {
            const x = document.getElementById("toast");
            x.innerText = message; x.className = "show";
            setTimeout(function(){ x.className = x.className.replace("show", ""); }, 3000);
        }

        // --- AUTHENTICATION ---
        function handleLogin() {
            const u = document.getElementById('username').value.toLowerCase();
            const p = document.getElementById('password').value;

            if (u === 'guru' && p === 'guru123') {
                state.currentUser = 'guru';
                enterTeacherView();
            } 
            else {
                const studentUser = state.students.find(s => s.name.toLowerCase() === u);
                if (studentUser && studentUser.password === p) {
                    state.currentUser = studentUser;
                    enterStudentView();
                } else {
                    showToast(i18n[state.lang].msgError);
                }
            }
        }

        function logout() {
            state.currentUser = null;
            document.getElementById('username').value = ''; document.getElementById('password').value = '';
            document.getElementById('view-login').classList.remove('hidden');
            document.getElementById('view-student').classList.add('hidden');
            document.getElementById('view-teacher').classList.add('hidden');
            document.getElementById('btn-logout').classList.add('hidden');
        }

        // --- STUDENT LOGIC ---
        function enterStudentView() {
            document.getElementById('view-login').classList.add('hidden');
            document.getElementById('view-student').classList.remove('hidden');
            document.getElementById('btn-logout').classList.remove('hidden');
            
            document.getElementById('student-name-display').innerText = state.currentUser.name;
            document.getElementById('student-class-display').innerText = state.currentUser.class;
            
            updateClock(); 
            applyLanguage();
            renderStudentHistory();
        }

        function studentCheckIn() {
            if(!state.currentUser || typeof state.currentUser !== 'object') return;

            const today = new Date(); today.setHours(0,0,0,0);
            const lastLog = state.attendance.find(a => a.studentId === state.currentUser.id && a.timestamp >= today.getTime()); 
            if (lastLog) { showToast("Anda sudah absen hari ini!"); return; }
            const now = new Date();
            state.attendance.push({ 
                studentId: state.currentUser.id, 
                name: state.currentUser.name, 
                timestamp: now.getTime(), 
                status: 'Hadir' 
            });
            saveData();
            showToast(i18n[state.lang].msgSuccess); renderStudentHistory();
        }

        function renderStudentHistory() {
            if(!state.currentUser) return;
            const list = document.getElementById('student-history-list');
            list.innerHTML = '';
            const myLogs = state.attendance.filter(a => a.studentId === state.currentUser.id).sort((a,b) => b.timestamp - a.timestamp).slice(0, 5);
            if(myLogs.length === 0) { list.innerHTML = '<li>Belum ada riwayat.</li>'; return; }
            myLogs.forEach(log => {
                const d = new Date(log.timestamp);
                const dateStr = d.toLocaleDateString('id-ID', {day: 'numeric', month: 'short'});
                const timeStr = d.toLocaleTimeString('id-ID', {hour: '2-digit', minute:'2-digit'});
                const li = document.createElement('li');
                li.style.marginBottom = '8px'; li.style.padding = '5px'; li.style.background = '#f9fafb'; li.style.borderRadius = '5px';
                li.innerHTML = `<b>${dateStr}</b> - ${timeStr} <span class="status-badge status-present">${log.status}</span>`;
                list.appendChild(li);
            });
        }

        // --- TEACHER LOGIC ---
        function enterTeacherView() {
            document.getElementById('view-login').classList.add('hidden');
            document.getElementById('view-teacher').classList.remove('hidden');
            document.getElementById('btn-logout').classList.remove('hidden');
            renderTeacherRealtime();
        }

        function switchTeacherTab(tabName) {
            ['realtime', 'manual-attendance', 'history', 'monthly', 'yearly', 'management'].forEach(t => {
                document.getElementById(`content-${t}`).classList.add('hidden');
            });
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(`content-${tabName}`).classList.remove('hidden');
            
            const tabs = ['realtime', 'manual-attendance', 'history', 'monthly', 'yearly', 'management'];
            const idx = tabs.indexOf(tabName);
            document.querySelectorAll('.tab-btn')[idx].classList.add('active');

            if(tabName === 'realtime') renderTeacherRealtime();
            if(tabName === 'manual-attendance') {};
            if(tabName === 'history') renderHistory();
            if(tabName === 'monthly') renderMonthlyRecap();
            if(tabName === 'yearly') renderYearlyRecap();
            if(tabName === 'management') renderManagement();
        }

        // --- MANAGEMENT LOGIC (FIXED) ---
        function resetSystemData() {
            if(confirm(i18n[state.lang].msgResetConfirm)) {
                localStorage.removeItem(STORAGE_KEYS.STUDENTS);
                localStorage.removeItem(STORAGE_KEYS.ATTENDANCE);
                location.reload();
            }
        }

        function resetPassword(id) {
            const targetId = Number(id); // Fix: Convert ID from String to Number
            const index = state.students.findIndex(s => s.id === targetId);
            if(index !== -1) {
                state.students[index].password = generateRandomPassword();
                saveData();
                showToast(i18n[state.lang].msgPassReset);
                renderManagement();
            }
        }

        function cancelEdit() {
            state.editingStudentId = null;
            document.getElementById('new-student-name').value = '';
            document.getElementById('new-student-class').value = '';
            
            const btn = document.getElementById('btn-save-student');
            const btnCancel = document.getElementById('btn-cancel-edit');
            
            btn.innerText = "+ Tambah Siswa";
            btn.style.background = "var(--primary)";
            btnCancel.classList.add('hidden');
            
            applyLanguage();
        }

        function editStudent(id) {
            const targetId = Number(id); // Fix: Convert ID from String to Number
            const student = state.students.find(s => s.id === targetId);
            if(!student) return;

            state.editingStudentId = targetId;
            document.getElementById('new-student-name').value = student.name;
            document.getElementById('new-student-class').value = student.class;

            const btn = document.getElementById('btn-save-student');
            const btnCancel = document.getElementById('btn-cancel-edit');

            btn.innerText = "Update Siswa";
            btn.style.background = "var(--accent)";
            btnCancel.classList.remove('hidden');
            
            applyLanguage();
            document.querySelector('#content-management').scrollIntoView({behavior: 'smooth'});
        }

        function addOrUpdateStudent() {
            const nameInput = document.getElementById('new-student-name');
            const classInput = document.getElementById('new-student-class');
            const name = nameInput.value.trim();
            const cls = classInput.value;
            if(!name) { showToast("Nama siswa tidak boleh kosong!"); return; }

            if (state.editingStudentId) {
                // Update Logic
                const index = state.students.findIndex(s => s.id === state.editingStudentId);
                if (index !== -1) {
                    state.students[index].name = name;
                    state.students[index].class = cls;
                    showToast(i18n[state.lang].msgUpdateSuccess);
                }
                cancelEdit();
            } else {
                // Add Logic
                const newId = state.students.length > 0 ? Math.max(...state.students.map(s=>s.id)) + 1 : 1;
                state.students.push({ 
                    id: newId, 
                    name: name, 
                    class: cls, 
                    password: generateRandomPassword()
                });
                showToast(i18n[state.lang].msgAddSuccess);
                nameInput.value = '';
            }

            saveData(); 
            renderManagement();
        }

        function deleteStudent(id) {
            const targetId = Number(id); // Fix: Convert ID from String to Number
            if(confirm("Yakin ingin menghapus siswa ini? Data akan dihapus permanen.")) {
                state.students = state.students.filter(s => s.id !== targetId);
                saveData(); 
                showToast(i18n[state.lang].msgDelSuccess); 
                renderManagement();
            }
        }

        function renderManagement() {
            const container = document.getElementById('class-card-container'); container.innerHTML = '';
            const allCard = document.createElement('div');
            allCard.className = `class-card ${state.selectedClassFilter === null ? 'active' : ''}`;
            allCard.innerText = "Semua Kelas";
            allCard.onclick = () => { state.selectedClassFilter = null; renderManagement(); };
            container.appendChild(allCard);

            state.classes.forEach(cls => {
                const card = document.createElement('div');
                card.className = `class-card ${state.selectedClassFilter === cls.id ? 'active' : ''}`;
                card.innerText = cls.name;
                card.onclick = () => { 
                    state.selectedClassFilter = cls.id; 
                    document.getElementById('new-student-class').value = cls.id;
                    renderManagement(); 
                };
                container.appendChild(card);
            });

            const classSelect = document.getElementById('new-student-class');
            const currentSelection = classSelect.value;
            classSelect.innerHTML = '';
            state.classes.forEach(cls => {
                const opt = document.createElement('option'); opt.value = cls.id; opt.innerText = cls.name; classSelect.appendChild(opt);
            });
            if(currentSelection) classSelect.value = currentSelection;
            else if(state.selectedClassFilter) classSelect.value = state.selectedClassFilter;

            const tbody = document.querySelector('#table-students tbody'); tbody.innerHTML = '';
            const label = document.getElementById('current-class-label');
            label.innerText = state.selectedClassFilter ? `(${state.selectedClassFilter})` : '(Semua)';

            const filteredStudents = state.selectedClassFilter ? state.students.filter(s => s.class === state.selectedClassFilter) : state.students;
            if (filteredStudents.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" style="text-align:center;">Tidak ada data siswa di kelas ini.</td></tr>';
            } else {
                filteredStudents.forEach(s => {
                    const tr = `
                        <tr>
                            <td>${s.id}</td>
                            <td>${s.name}</td>
                            <td><span style="font-weight:bold; color:var(--primary);">${s.class}</span></td>
                            <td><span class="pass-cell">${s.password}</span></td>
                            <td>
                                <button class="btn-small btn-pass" onclick="resetPassword(${s.id})">Reset Pass</button>
                                <button class="btn-small btn-edit" onclick="editStudent(${s.id})">Edit</button>
                                <button class="btn-small btn-danger" onclick="deleteStudent(${s.id})">Hapus</button>
                            </td>
                        </tr>`;
                    tbody.innerHTML += tr;
                });
            }
        }

        // --- MANUAL ATTENDANCE LOGIC ---
        function loadManualAttendanceTable() {
            const dateInput = document.getElementById('manual-date').value;
            const classInput = document.getElementById('manual-class').value;

            if(!dateInput || !classInput) {
                showToast("Pilih Tanggal dan Kelas terlebih dahulu!");
                return;
            }

            const tbody = document.getElementById('tbody-manual');
            tbody.innerHTML = '';

            const selectedDate = new Date(dateInput).setHours(0,0,0,0);
            const classStudents = state.students.filter(s => s.class === classInput);

            if(classStudents.length === 0) {
                tbody.innerHTML = `<tr><td colspan="3" style="text-align:center;">Tidak ada siswa di kelas ini.</td></tr>`;
                return;
            }

            classStudents.forEach((student, index) => {
                const existingRecord = state.attendance.find(a => {
                    const recDate = new Date(a.timestamp).setHours(0,0,0,0);
                    return a.studentId === student.id && recDate === selectedDate;
                });

                const status = existingRecord ? existingRecord.status : '';

                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${index + 1}</td>
                    <td><strong>${student.name}</strong></td>
                    <td>
                        <div class="status-radio-group" data-sid="${student.id}">
                            <label class="radio-label"><input type="radio" name="status_${student.id}" value="Hadir" ${status === 'Hadir' ? 'checked' : ''}> ${i18n[state.lang].stHadir}</label>
                            <label class="radio-label"><input type="radio" name="status_${student.id}" value="Sakit" ${status === 'Sakit' ? 'checked' : ''}> ${i18n[state.lang].stSakit}</label>
                            <label class="radio-label"><input type="radio" name="status_${student.id}" value="Izin" ${status === 'Izin' ? 'checked' : ''}> ${i18n[state.lang].stIzin}</label>
                            <label class="radio-label"><input type="radio" name="status_${student.id}" value="Alfa" ${status === 'Alfa' ? 'checked' : ''}> ${i18n[state.lang].stAlfa}</label>
                        </div>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function saveManualAttendance() {
            const dateInput = document.getElementById('manual-date').value;
            const classInput = document.getElementById('manual-class').value;
            const selectedDate = new Date(dateInput).setHours(0,0,0,0);

            if(!dateInput || !classInput) {
                showToast("Data Tanggal atau Kelas belum valid.");
                return;
            }

            const rows = document.querySelectorAll('#tbody-manual tr');
            if(rows.length === 0 || rows[0].cells.length === 1) {
                showToast("Tidak ada data siswa untuk disimpan.");
                return;
            }

            let updateCount = 0;

            rows.forEach(row => {
                const radios = row.querySelectorAll('input[type="radio"]');
                const sid = parseInt(radios[0].name.split('_')[1]);
                
                let selectedStatus = null;
                radios.forEach(r => { if(r.checked) selectedStatus = r.value; });

                if(selectedStatus) {
                    const existingIndex = state.attendance.findIndex(a => {
                        const recDate = new Date(a.timestamp).setHours(0,0,0,0);
                        return a.studentId === sid && recDate === selectedDate;
                    });

                    const now = new Date();
                    const isToday = (new Date().setHours(0,0,0,0) === selectedDate);
                    const timeToUse = isToday ? now.getTime() : (new Date(dateInput + "T08:00:00").getTime());

                    const studentName = state.students.find(s => s.id === sid).name;

                    if(existingIndex > -1) {
                        state.attendance[existingIndex].status = selectedStatus;
                        state.attendance[existingIndex].timestamp = timeToUse;
                    } else {
                        state.attendance.push({
                            studentId: sid,
                            name: studentName,
                            timestamp: timeToUse,
                            status: selectedStatus
                        });
                    }
                    updateCount++;
                }
            });

            saveData();
            showToast(i18n[state.lang].msgSaved + ` (${updateCount} siswa)`);
        }

        function renderTeacherRealtime() {
            const tbody = document.querySelector('#table-realtime tbody'); tbody.innerHTML = '';
            const today = new Date(); today.setHours(0,0,0,0);
            const todayLogs = state.attendance.filter(a => a.timestamp >= today.getTime()).sort((a,b) => a.timestamp - b.timestamp);
            if(todayLogs.length === 0) { tbody.innerHTML = '<tr><td colspan="3" style="text-align:center;">Belum ada data hari ini</td></tr>'; return; }
            todayLogs.forEach(log => {
                const d = new Date(log.timestamp);
                const timeStr = d.toLocaleTimeString('id-ID', {hour: '2-digit', minute:'2-digit', second:'2-digit'});
                const tr = `<tr><td>${timeStr}</td><td>${log.name}</td><td><span class="status-badge status-present">${log.status}</span></td></tr>`;
                tbody.innerHTML += tr;
            });
        }

        function renderHistory() {
            const tbody = document.querySelector('#table-history tbody'); tbody.innerHTML = '';
            const allLogs = [...state.attendance].sort((a,b) => b.timestamp - a.timestamp);
            allLogs.forEach(log => {
                const d = new Date(log.timestamp);
                const dateStr = d.toLocaleDateString('id-ID');
                const timeStr = d.toLocaleTimeString('id-ID', {hour: '2-digit', minute:'2-digit', second:'2-digit'});
                const tr = `<tr><td>${dateStr}</td><td>${timeStr}</td><td>${log.name}</td><td><span class="status-badge status-present">${log.status}</span></td></tr>`;
                tbody.innerHTML += tr;
            });
        }

        function populateDropdowns() {
            const monthSel = document.getElementById('month-select');
            const yearSelMonth = document.getElementById('year-select-month');
            const yearSelYear = document.getElementById('year-select-year');
            const months = ["Januari", "Februari", "Maret", "April", "Mei", "Juni", "Juli", "Agustus", "September", "Oktober", "November", "Desember"];
            months.forEach((m, i) => { const opt = document.createElement('option'); opt.value = i; opt.innerText = m; monthSel.appendChild(opt); });
            monthSel.value = new Date().getMonth();
            const currentYear = new Date().getFullYear();
            for(let y = currentYear - 1; y <= currentYear + 1; y++) {
                const opt = document.createElement('option'); opt.value = y; opt.innerText = y;
                yearSelMonth.appendChild(opt.cloneNode(true));
                yearSelYear.appendChild(opt);
            }
            yearSelMonth.value = currentYear; yearSelYear.value = currentYear;

            const classSelect = document.getElementById('manual-class');
            state.classes.forEach(cls => {
                const opt = document.createElement('option'); opt.value = cls.id; opt.innerText = cls.name; classSelect.appendChild(opt);
            });
        }

        function renderMonthlyRecap() {
            const monthIdx = parseInt(document.getElementById('month-select').value);
            const year = parseInt(document.getElementById('year-select-month').value);
            const tbody = document.querySelector('#table-monthly tbody'); tbody.innerHTML = '';
            const monthLogs = state.attendance.filter(a => { const d = new Date(a.timestamp); return d.getMonth() === monthIdx && d.getFullYear() === year; });
            const recap = {};
            state.students.forEach(s => { recap[s.id] = { name: s.name, hadir: 0, sakit: 0, alfa: 0 }; });
            monthLogs.forEach(log => { if(recap[log.studentId]) recap[log.studentId].hadir++; });
            const totalHadir = monthLogs.length;
            document.getElementById('monthly-summary').innerHTML = `<div class="summary-box"><h4>${totalHadir}</h4><span>Total Kehadiran</span></div><div class="summary-box"><h4>${state.students.length}</h4><span>Total Siswa</span></div>`;
            for(let id in recap) {
                const s = recap[id]; const daysInMonth = new Date(year, monthIdx + 1, 0).getDate();
                const percentage = Math.round((s.hadir / daysInMonth) * 100);
                const tr = `<tr><td>${s.name}</td><td style="color:green; font-weight:bold;">${s.hadir}</td><td style="color:orange;">${s.sakit}</td><td style="color:red;">${s.alfa}</td><td>${percentage}%</td></tr>`;
                tbody.innerHTML += tr;
            }
        }

        function renderYearlyRecap() {
            const year = parseInt(document.getElementById('year-select-year').value);
            const tbody = document.querySelector('#table-yearly tbody'); tbody.innerHTML = '';
            const yearLogs = state.attendance.filter(a => { const d = new Date(a.timestamp); return d.getFullYear() === year; });
            const recap = {};
            state.students.forEach(s => { recap[s.id] = { name: s.name, total: 0 }; });
            yearLogs.forEach(log => { if(recap[log.studentId]) recap[log.studentId].total++; });
            const schoolDays = 200;
            for(let id in recap) {
                const s = recap[id]; const pct = Math.min(100, Math.round((s.total / schoolDays) * 100));
                const tr = `<tr><td>${s.name}</td><td>${s.total} Hari</td><td><div style="background:#ddd; height:10px; border-radius:5px; width:100px;"><div style="background:var(--primary); height:100%; width:${pct}%; border-radius:5px;"></div></div><small>${pct}%</small></td></tr>`;
                tbody.innerHTML += tr;
            }
            const totalAll = yearLogs.length;
            document.getElementById('yearly-summary').innerHTML = `<div class="summary-box"><h4>${totalAll}</h4><span>Total Absensi Tahun Ini</span></div>`;
        }

        window.onload = init;
    </script>
</body>
</html>
```
