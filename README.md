<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ระบบติดตามอารมณ์และพฤติกรรม</title>
  <link rel="stylesheet" href="styles.css" />
  <!-- Chart.js CDN for simple mood statistics -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <div class="container">
    <header>
      <h1>ระบบติดตามอารมณ์และพฤติกรรม</h1>
      <div style="display:flex;gap:8px;align-items:center">
        <div id="sessionInfo"></div>
        <button id="btnLogout" class="hidden">Log out</button>
      </div>
    </header>

    <main>
      <!-- Login / Role selection -->
      <section id="loginSection" class="card">
        <h2>เข้าสู่ระบบ (ตัวอย่าง)</h2>
        <div class="row">
          <label>เลือกบทบาท:</label>
          <select id="roleSelect">
            <option value="student">นักเรียน</option>
            <option value="teacher">ครู</option>
          </select>
        </div>
        <div class="row" id="userSelectRow">
          <label id="userLabel">เลือกนักเรียน:</label>
          <select id="userSelect"></select>
        </div>
        <div class="row">
          <button id="btnLogin">เข้าสู่ระบบ</button>
          <button id="btnReset" class="danger">Reset Demo</button>
        </div>
      </section>

      <!-- Student Dashboard -->
      <section id="studentDashboard" class="card hidden">
        <h2>แดชบอร์ดนักเรียน - <span id="studentName"></span></h2>

        <div class="grid">
          <div class="card small">
            <h3>1) บันทึกอารมณ์ประจำวัน (My diary)</h3>
            <div id="moodForm">
              <div class="moodRow" id="moodOptions"></div>
              <textarea id="moodNote" placeholder="บันทึกสั้น ๆ วันนี้เป็นอย่างไร..."></textarea>
              <div class="row">
                <button id="btnSaveMood">บันทึกอารมณ์</button>
              </div>
            </div>
            <h4>บันทึกล่าสุด</h4>
            <ul id="diaryList" class="list"></ul>
          </div>

          <div class="card small">
            <h3>2) ดาวเด็กดี & แลกของรางวัล</h3>
            <div class="row">
              <div>ดาวของฉัน: <strong id="starCount">0</strong> ⭐</div>
            </div>
            <div class="rewards">
              <div class="reward">
                <div>เวลาพัก 5 นาที</div>
                <div>10 ⭐ <button data-reward="break" class="btnRedeem">แลก</button></div>
              </div>
              <div class="reward">
                <div>คูปองเครื่องเขียน</div>
                <div>12 ⭐ <button data-reward="stationery" class="btnRedeem">แลก</button></div>
              </div>
              <div class="reward">
                <div>คูปองเครื่องดื่ม/อาหาร</div>
                <div>15 ⭐ <button data-reward="snack" class="btnRedeem">แลก</button></div>
              </div>
            </div>
            <h4>ประวัติการแลก</h4>
            <ul id="redeemHistory" class="list"></ul>
          </div>

          <div class="card small">
            <h3>3) นัดหมายขอเข้าปรึกษาครู</h3>
            <input id="apptDate" type="date" />
            <input id="apptTime" type="time" />
            <textarea id="apptNote" placeholder="เหตุผล/รายละเอียดสั้น ๆ"></textarea>
            <div class="row">
              <button id="btnRequestAppt">ส่งคำขอนัดหมาย</button>
            </div>
            <h4>ประวัติคำขอ</h4>
            <ul id="apptHistory" class="list"></ul>
          </div>

          <div class="card small">
            <h3>4) แบบทดสอบทางจิตวิทยาเบื้องต้น</h3>
            <div id="quizArea">
              <div id="quizQuestions"></div>
              <div class="row">
                <button id="btnSubmitQuiz">ส่งแบบทดสอบ</button>
              </div>
            </div>
            <h4>ผลแบบทดสอบ</h4>
            <ul id="quizHistory" class="list"></ul>
          </div>
        </div>
      </section>

      <!-- Teacher Dashboard -->
      <section id="teacherDashboard" class="card hidden">
        <h2>แดชบอร์ดครู - <span id="teacherName"></span></h2>

        <div class="grid">
          <div class="card small">
            <h3>1) บันทึกอารมณ์ของครู (My diary)</h3>
            <div id="teacherMoodForm">
              <div class="moodRow" id="teacherMoodOptions"></div>
              <textarea id="teacherMoodNote" placeholder="บันทึกสั้น ๆ"></textarea>
              <div class="row">
                <button id="btnTeacherSaveMood">บันทึก</button>
              </div>
            </div>
            <h4>บันทึกล่าสุด (ครู)</h4>
            <ul id="teacherDiaryList" class="list"></ul>
          </div>

          <div class="card small">
            <h3>2) จัดการดาวให้รายบุคคล</h3>
            <div class="row">
              <label>เลือกนักเรียน:</label>
              <select id="teacherStudentSelect"></select>
            </div>
            <div class="row">
              <label>จำนวนดาว (ใส่ + หรือ -):</label>
              <input id="starDelta" type="number" value="1" />
            </div>
            <div class="row">
              <button id="btnAdjustStars">บันทึกการให้/ลดดาว</button>
            </div>
            <h4>ประวัติดาวของนักเรียน</h4>
            <ul id="teacherStarHistory" class="list"></ul>
          </div>

          <div class="card small">
            <h3>3) บันทึกรายงานพฤติกรรมส่งครูที่ปรึกษา</h3>
            <div class="row">
              <label>เลือกนักเรียน:</label>
              <select id="reportStudentSelect"></select>
            </div>
            <textarea id="reportNote" placeholder="รายละเอียดรายงานพฤติกรรม"></textarea>
            <div class="row">
              <button id="btnSendReport">ส่งรายงาน</button>
            </div>
            <h4>ประวัติรายงาน</h4>
            <ul id="reportHistory" class="list"></ul>
          </div>

          <div class="card small">
            <h3>4) กล่องข้อความขอนัดหมายจากนักเรียน</h3>
            <div id="apptInbox"></div>
          </div>

          <div class="card small">
            <h3>5) สถิติภาพรวมอารมณ์ของนักเรียน</h3>
            <canvas id="moodChart" height="200"></canvas>
          </div>
        </div>
      </section>

    </main>

    <footer>
      <small>Demo: เก็บข้อมูลใน localStorage — ปรับต่อได้เพื่อเชื่อม backend</small>
    </footer>
  </div>

  <script src="app.js"></script>
</body>
</html>
