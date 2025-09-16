# 2025_FIT4014_ThietkeWeb
Trưởng nhóm chịu trách nhiệm tạo Repo đặt tên 2025_FIT4014_ThietkeWeb  o Tất cả các Bài tập thực hành về sau sẽ thực hiện lưu vào Repo này  o Các dự án tạo ra cần chú ý việc tổ chức thư mục, đặt tên tệp và tên biến một  cách khoa học, theo chuẩn thống nhất  o Khai báo lại link Github trên GoogleSheet 
<!doctype html>
<html lang="vi">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Bài tập 03 - HTML Table</title>
<style>
  body{font-family: Arial, Helvetica, sans-serif; padding:20px;}
  .table-wrap{overflow-x:auto;}
  table.score-table{
    border-collapse: collapse;
    width:100%;
    min-width:900px; /* để giữ bố cục giống mẫu */
    border:2px solid #333;
  }
  table.score-table th, table.score-table td{
    border:1px solid #333;
    padding:8px 10px;
  }
  table.score-table thead th{
    background:#e9f6ff;
    text-align:center;
    font-weight:700;
  }
  table.score-table th.small { font-weight:700; }
  table.score-table td.center { text-align:center; }
  table.score-table td.left { text-align:left; }
  caption{font-weight:700; margin-bottom:8px; text-align:left}
  /* nhấn mạnh các ô số trọng số */
  thead tr.weights th{ background:#fff; font-weight:600; }
</style>
</head>
<body>

<h2>Bài tập 03 — Ví dụ bảng</h2>
<div class="table-wrap">
<table class="score-table" id="scoreTable">
  <caption>Example: Student exam table</caption>
  <thead>
    <!-- Hàng 1: nhóm chính -->
    <tr>
      <th colspan="2">Student</th>
      <th colspan="4">Exam</th>
      <th colspan="4">2nd Exam</th>
      <th colspan="2">Final Grade</th>
    </tr>
    <!-- Hàng 2: nhãn con -->
    <tr>
      <th>Code</th>
      <th>Name</th>

      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Grade</th>

      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Grade</th>

      <th>NR</th>
      <th>R</th>
    </tr>
    <!-- Hàng 3: trọng số (số điểm tối đa / weights) -->
    <tr class="weights">
      <th></th><th></th>
      <th class="center" data-weight="8">8</th>
      <th class="center" data-weight="7">7</th>
      <th class="center" data-weight="5">5</th>
      <th></th>

      <th class="center" data-weight2="6">6</th>
      <th class="center" data-weight2="7">7</th>
      <th class="center" data-weight2="8">8</th>
      <th></th>

      <th></th><th></th>
    </tr>
  </thead>

  <tbody>
    <!-- Dòng 1: John (theo mẫu) -->
    <tr>
      <td class="left">80549061</td>
      <td class="left">John</td>

      <td class="center pct">70%</td>
      <td class="center pct">100%</td>
      <td class="center pct">100%</td>
      <td class="center grade1"></td>

      <td class="center pct2"></td>
      <td class="center pct2"></td>
      <td class="center pct2"></td>
      <td class="center grade2"></td>

      <td class="center finalNR"></td>
      <td class="center finalR"></td>
    </tr>

    <!-- Dòng 2: Mary -->
    <tr>
      <td class="left">80549062</td>
      <td class="left">Mary</td>

      <td class="center pct">10%</td>
      <td class="center pct">50%</td>
      <td class="center pct">50%</td>
      <td class="center grade1"></td>

      <td class="center pct2">100%</td>
      <td class="center pct2">100%</td>
<td class="center pct2">50%</td>
      <td class="center grade2"></td>

      <td class="center finalNR"></td>
      <td class="center finalR"></td>
    </tr>

    <!-- Dòng 3: Claire -->
    <tr>
      <td class="left">80549063</td>
      <td class="left">Claire</td>

      <td class="center pct"></td>
      <td class="center pct"></td>
      <td class="center pct"></td>
      <td class="center grade1"></td>

      <td class="center pct2">50%</td>
      <td class="center pct2">50%</td>
      <td class="center pct2">50%</td>
      <td class="center grade2"></td>

      <td class="center finalNR"></td>
      <td class="center finalR"></td>
    </tr>
  </tbody>
</table>
</div>

<script>
  // đọc trọng số từ header (nếu muốn cấu hình ở 1 chỗ)
  const w1 = [
    Number(document.querySelector('thead tr.weights th[data-weight]')?.getAttribute('data-weight') || 8),
    Number(document.querySelector('thead tr.weights th[data-weight]')?.nextElementSibling?.getAttribute('data-weight') || 7),
    Number(document.querySelector('thead tr.weights th[data-weight]')?.nextElementSibling?.nextElementSibling?.getAttribute('data-weight') || 5)
  ];
  // ở đây chỉ lấy giá trị tĩnh theo mặc định (cách đơn giản)
  const examWeights = [8,7,5];
  const exam2Weights = [6,7,8];

  function parsePercent(text){
    if(!text) return null;
    text = text.toString().trim();
    if(!text) return null;
    if(text.endsWith('%')) text = text.slice(0,-1);
    const n = parseFloat(text);
    return isNaN(n) ? null : n;
  }

  // tính grade theo 3 ô phần trăm và mảng weights
  function computeGrade(percentCells, weights){
    let sum = 0;
    let any = false;
    for(let i=0;i<weights.length;i++){
      const p = parsePercent(percentCells[i]?.textContent || percentCells[i]?.innerText || '');
      if(p !== null){
        sum += (p/100) * weights[i];
        any = true;
      }
    }
    return any ? Math.round(sum*10)/10 : null; // 1 chữ số thập phân
  }

  // chạy tính cho mỗi hàng tbody
  document.querySelectorAll('#scoreTable tbody tr').forEach(tr=>{
    const pct1 = Array.from(tr.querySelectorAll('td.pct')).slice(0,3);
    const pct2 = Array.from(tr.querySelectorAll('td.pct2')).slice(0,3);
    const grade1Cell = tr.querySelector('td.grade1');
    const grade2Cell = tr.querySelector('td.grade2');
    const finalNRCell = tr.querySelector('td.finalNR');
    const finalRCell = tr.querySelector('td.finalR');

    const g1 = computeGrade(pct1, examWeights);
    const g2 = computeGrade(pct2, exam2Weights);

    if(g1 !== null) grade1Cell.textContent = g1.toFixed(1);
    else grade1Cell.textContent = '';

    if(g2 !== null) grade2Cell.textContent = g2.toFixed(1);
    else grade2Cell.textContent = '';

    // logic final: lấy điểm lớn nhất (ví dụ) và làm tròn cho cột R
    const NR = (g1 !== null || g2 !== null) ? Math.max(g1||0, g2||0) : null;
    if(NR !== null){
finalNRCell.textContent = NR.toFixed(1);
      finalRCell.textContent = String(Math.round(NR));
    } else {
      finalNRCell.textContent = '';
      finalRCell.textContent = '';
    }
  });
</script>

</body>
</html>
