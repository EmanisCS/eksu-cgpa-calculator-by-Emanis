<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>EKSU GPA & CGPA Calculator Made By Emanis</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background-color: #f9f9f9;
    }

    h1, h2 {
      color: #2c3e50;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 30px;
      background-color: #fff;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: center;
    }

    th {
      background-color: #f0f0f0;
    }

    input[type="text"], input[type="number"] {
      width: 90%;
      padding: 6px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }

    button {
      padding: 12px 25px;
      background-color: #27ae60;
      color: white;
      border: none;
      font-size: 16px;
      cursor: pointer;
      border-radius: 4px;
    }

    button:hover {
      background-color: #219150;
    }

    #result {
      font-size: 18px;
      font-weight: bold;
      margin-top: 20px;
      background: #eafaf1;
      padding: 15px;
      border-left: 6px solid #2ecc71;
    }

    .semester-label {
      text-align: left;
      padding: 10px 0;
      font-size: 18px;
      color: #34495e;
    }

    .add-course-btn {
      margin-top: 10px;
      background-color: #2980b9;
      padding: 10px 20px;
    }

    .add-course-btn:hover {
      background-color: #3498db;
    }
  </style>
</head>
<body>
  <h1>EKSU GPA & CGPA Calculator Made by Emanis</h1>

  <form id="cgpaForm">
    <div class="semester-label">First Semester</div>
    <table id="firstSemester">
      <tr>
        <th>Course Title</th>
        <th>Grade (A–F)</th>
        <th>Credit Unit</th>
      </tr>
      <!-- 10 rows for First Semester -->
      <tr>
        <td><input type="text" id="fs_course0" placeholder="e.g. MTH101"></td>
        <td><input type="text" id="fs_grade0" maxlength="1" placeholder="A–F"></td>
        <td><input type="number" id="fs_credit0" min="1" placeholder="e.g. 3"></td>
      </tr>
    </table>
    <button type="button" class="add-course-btn" id="addFirstCourse">Add Course</button>

    <div class="semester-label">Second Semester</div>
    <table id="secondSemester">
      <tr>
        <th>Course Title</th>
        <th>Grade (A–F)</th>
        <th>Credit Unit</th>
      </tr>
      <!-- 10 rows for Second Semester -->
      <tr>
        <td><input type="text" id="ss_course0" placeholder="e.g. CHM102"></td>
        <td><input type="text" id="ss_grade0" maxlength="1" placeholder="A–F"></td>
        <td><input type="number" id="ss_credit0" min="1" placeholder="e.g. 2"></td>
      </tr>
    </table>
    <button type="button" class="add-course-btn" id="addSecondCourse">Add Course</button>

    <button type="submit">Calculate GPA & CGPA</button>
  </form>

  <div id="result"></div>

  <script>
    const gradeMap = {
      A: 5.0,
      B: 4.0,
      C: 3.0,
      D: 2.0,
      E: 1.0, // If your school uses it
      F: 0.0
    };

    function calculateGPA(prefix) {
      let totalPoints = 0;
      let totalCredits = 0;

      for (let i = 0; i < 10; i++) {
        const grade = document.getElementById(`${prefix}_grade${i}`)?.value.trim().toUpperCase();
        const credit = parseFloat(document.getElementById(`${prefix}_credit${i}`)?.value);

        if (!gradeMap.hasOwnProperty(grade) || isNaN(credit) || credit <= 0) continue;

        totalCredits += credit;
        totalPoints += gradeMap[grade] * credit;
      }

      return {
        gpa: totalCredits > 0 ? totalPoints / totalCredits : 0,
        totalCredits,
        totalPoints
      };
    }

    document.getElementById('addFirstCourse').addEventListener('click', function() {
      const table = document.getElementById('firstSemester');
      const rowCount = table.rows.length;
      if (rowCount < 12) {  // 10 rows max + header
        const row = table.insertRow(rowCount);
        row.innerHTML = `
          <td><input type="text" id="fs_course${rowCount-1}" placeholder="e.g. MTH101"></td>
          <td><input type="text" id="fs_grade${rowCount-1}" maxlength="1" placeholder="A–F"></td>
          <td><input type="number" id="fs_credit${rowCount-1}" min="1" placeholder="e.g. 3"></td>
        `;
      }
    });

    document.getElementById('addSecondCourse').addEventListener('click', function() {
      const table = document.getElementById('secondSemester');
      const rowCount = table.rows.length;
      if (rowCount < 12) {  // 10 rows max + header
        const row = table.insertRow(rowCount);
        row.innerHTML = `
          <td><input type="text" id="ss_course${rowCount-1}" placeholder="e.g. CHM102"></td>
          <td><input type="text" id="ss_grade${rowCount-1}" maxlength="1" placeholder="A–F"></td>
          <td><input type="number" id="ss_credit${rowCount-1}" min="1" placeholder="e.g. 2"></td>
        `;
      }
    });

    document.getElementById('cgpaForm').addEventListener('submit', function(e) {
      e.preventDefault();

      const first = calculateGPA('fs');
      const second = calculateGPA('ss');

      const totalCredits = first.totalCredits + second.totalCredits;
      const totalPoints = first.totalPoints + second.totalPoints;

      const cgpa = totalCredits > 0 ? totalPoints / totalCredits : 0;

      document.getElementById('result').innerHTML = `
        First Semester GPA: ${first.gpa.toFixed(2)}<br>
        Second Semester GPA: ${second.gpa.toFixed(2)}<br>
        <strong>Cumulative CGPA: ${cgpa.toFixed(2)}</strong>
      `;
    });
  </script>
</body>
</html>
