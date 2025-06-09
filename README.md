# ApnaAge
A simple and accurate Bengali to English date converter app
<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <title>ApnaAge - নির্ভুল বাংলা ↔ ইংরেজি জন্মতারিখ ক্যালকুলেটর</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 20px;
      text-align: center;
      max-width: 500px;
      margin: auto;
    }
    input, select, button {
      font-size: 16px;
      padding: 8px;
      margin: 5px 0;
      width: 100%;
      box-sizing: border-box;
    }
    button {
      cursor: pointer;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 4px;
    }
    button:hover {
      background-color: #0056b3;
    }
    hr {
      margin: 40px 0;
    }
    p.result {
      margin-top: 15px;
      font-weight: bold;
      color: #333;
    }
  </style>
</head>
<body>

  <h2>ApnaAge - নির্ভুল বাংলা ↔ ইংরেজি জন্মতারিখ ক্যালকুলেটর</h2>

  <!-- ১. বাংলা → ইংরেজি রূপান্তর -->
  <section>
    <h3>১. বাংলা → ইংরেজি রূপান্তর</h3>
    <label>নাম:</label>
    <input type="text" id="name1" placeholder="তোমার নাম লিখো" />

    <label>সাল টাইপ নির্বাচন করো:</label>
    <select id="yearType1">
      <option value="bangla">বাংলা সাল</option>
      <option value="gregorian">ইংরেজি সাল</option>
    </select>

    <label>জন্ম সাল:</label>
    <input type="number" id="byear" placeholder="যেমন: ১৪২৯ অথবা ২০২২" />

    <label>বাংলা মাস:</label>
    <select id="bmonth">
      <option value="0">বৈশাখ</option>
      <option value="1">জ্যৈষ্ঠ</option>
      <option value="2">আষাঢ়</option>
      <option value="3">শ্রাবণ</option>
      <option value="4">ভাদ্র</option>
      <option value="5">আশ্বিন</option>
      <option value="6">কার্তিক</option>
      <option value="7">অগ্রহায়ণ</option>
      <option value="8">পৌষ</option>
      <option value="9">মাঘ</option>
      <option value="10">ফাল্গুন</option>
      <option value="11">চৈত্র</option>
    </select>

    <label>বাংলা তারিখ (১-৩১):</label>
    <input type="number" id="bday" min="1" max="31" placeholder="১-৩১" />

    <button onclick="convertToEnglish()">বাংলা → ইংরেজি রূপান্তর</button>
    <p id="result1" class="result"></p>
  </section>

  <hr />

  <!-- ২. ইংরেজি → বাংলা রূপান্তর -->
  <section>
    <h3>২. ইংরেজি → বাংলা রূপান্তর</h3>
    <label>নাম:</label>
    <input type="text" id="name2" placeholder="তোমার নাম লিখো" />

    <label>ইংরেজি তারিখ:</label>
    <input type="date" id="edate" />

    <button onclick="convertToBangla()">ইংরেজি → বাংলা রূপান্তর</button>
    <p id="result2" class="result"></p>
  </section>

  <script>
    const banglaMonths = ["বৈশাখ", "জ্যৈষ্ঠ", "আষাঢ়", "শ্রাবণ", "ভাদ্র", "আশ্বিন", "কার্তিক", "অগ্রহায়ণ", "পৌষ", "মাঘ", "ফাল্গুন", "চৈত্র"];
    const weekDays = ["রবিবার", "সোমবার", "মঙ্গলবার", "বুধবার", "বৃহস্পতিবার", "শুক্রবার", "শনিবার"];

    // বাংলা সাল এবং ইংরেজি সালের পার্থক্য
    const YEAR_DIFF = 593;

    function toBanglaNumber(num) {
      const banglaNums = ['০','১','২','৩','৪','৫','৬','৭','৮','৯'];
      return num.toString().split('').map(d => banglaNums[d] || d).join('');
    }

    function convertToEnglish() {
      const name = document.getElementById("name1").value.trim();
      const yearType = document.getElementById("yearType1").value;
      let year = parseInt(document.getElementById("byear").value);
      const month = parseInt(document.getElementById("bmonth").value);
      const day = parseInt(document.getElementById("bday").value);

      if (!name) {
        alert("অনুগ্রহ করে নাম লিখুন।");
        return;
      }
      if (!year || year < 1000) {
        alert("সঠিক সাল লিখুন।");
        return;
      }
      if (!day || day < 1 || day > 31) {
        alert("বাংলা তারিখ ১ থেকে ৩১ এর মধ্যে হতে হবে।");
        return;
      }

      if (yearType === "bangla") {
        year = year + YEAR_DIFF;
      }

      const engMonthStart = [3,4,5,6,7,8,9,10,11,0,1,2];

      let engMonth = engMonthStart[month];
      let engDay = day + 13;

      let engYear = year;

      const daysInEngMonth = [31,28,31,30,31,30,31,31,30,31,30,31];

      if ((engYear % 400 === 0) || (engYear % 4 === 0 && engYear % 100 !== 0)) {
        daysInEngMonth[1] = 29;
      }

      if (engDay > daysInEngMonth[engMonth]) {
        engDay = engDay - daysInEngMonth[engMonth];
        engMonth += 1;
        if (engMonth > 11) {
          engMonth = 0;
          engYear += 1;
        }
      }

      const engDate = new Date(engYear, engMonth, engDay);
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      const engDateStr = engDate.toLocaleDateString('en-GB', options);

      document.getElementById("result1").innerText = `${name}, তোমার ইংরেজি জন্মতারিখ: ${engDateStr}`;
    }

    function convertToBangla() {
      const name = document.getElementById("name2").value.trim();
      const inputDateStr = document.getElementById("edate").value;

      if (!name) {
        alert("অনুগ্রহ করে নাম লিখুন।");
        return;
      }
      if (!inputDateStr) {
        alert("তারিখ নির্বাচন করুন।");
        return;
      }

      const inputDate = new Date(inputDateStr);
      let day = inputDate.getDate();
      let month = inputDate.getMonth();
      let year = inputDate.getFullYear();

      const banglaYear = year - YEAR_DIFF;

      const banglaMonthStart = [8,9,10,11,0,1,2,3,4,5,6,7];

      let banglaMonth = banglaMonthStart.indexOf(month);
      if (banglaMonth === -1) {
        banglaMonth = 0;
      }

      let banglaDay = day - 13;
      if (banglaDay <= 0) {
        banglaMonth -= 1;
        if (banglaMonth < 0) {
          banglaMonth = 11;
        }
        banglaDay += 30;
      }

      const weekday = weekDays[inputDate.getDay()];

      document.getElementById("result2").innerText = `${name}, তোমার বাংলা জন্মতারিখ: ${toBanglaNumber(banglaDay)} ${banglaMonths[banglaMonth]} ${toBanglaNumber(banglaYear)} (${weekday})`;
    }
  </script>

</body>
</html>
