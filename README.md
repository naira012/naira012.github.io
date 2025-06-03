<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Small Repair Loan Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      max-width: 800px;
      margin: auto;
    }
    input, select {
      width: 100%;
      padding: 8px;
      margin: 8px 0;
    }
    button {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    .result {
      margin-top: 20px;
      padding: 10px;
      border: 1px solid #ccc;
    }
    table {
      margin-top: 20px;
      font-size: 14px;
      width: 100%;
      border-collapse: collapse;
    }
    table th, table td {
      border: 1px solid #ccc;
      padding: 5px;
      text-align: right;
    }
    table th {
      background-color: #f2f2f2;
    }
  </style>
</head>
<body>
  <h2>Small Repair Loan Calculator</h2>

  <label>Home Value ($):</label>
  <input type="number" id="homeValue">

  <label>Existing Mortgage Balance ($):</label>
  <input type="number" id="mortgageBalance">

  <label>Monthly Income ($):</label>
  <input type="number" id="monthlyIncome">

  <label>Monthly Debt Payments ($):</label>
  <input type="number" id="monthlyDebts">

  <label>Interest Rate (% annually):</label>
  <input type="number" step="0.01" id="interestRate">

  <label>Loan Term (years):</label>
  <select id="loanTerm">
    <option value="1">1</option>
    <option value="3">3</option>
    <option value="5">5</option>
    <option value="7">7</option>
    <option value="10">10</option>
  </select>

  <label>Desired Loan Amount ($):</label>
  <input type="number" id="loanAmount">

  <button onclick="calculateLoan()">Calculate</button>

  <div class="result" id="results"></div>

  <script>
    function calculateLoan() {
      const HV = parseFloat(document.getElementById("homeValue").value);
      const EMB = parseFloat(document.getElementById("mortgageBalance").value);
      const MI = parseFloat(document.getElementById("monthlyIncome").value);
      const MDP = parseFloat(document.getElementById("monthlyDebts").value);
      const IR = parseFloat(document.getElementById("interestRate").value) / 100;
      const LT = parseInt(document.getElementById("loanTerm").value);
      const DLA = parseFloat(document.getElementById("loanAmount").value);

      const LTV = ((EMB + DLA) / HV) * 100;

      const monthlyRate = IR / 12;
      const n = LT * 12;
   const MP = (DLA * monthlyRate * Math.pow(1 + monthlyRate, n)) / (Math.pow(1 + monthlyRate, n) - 1);

      const DTI = ((MDP + MP) / MI) * 100;
      const totalInterest = MP * n - DLA;

      document.getElementById("results").innerHTML = `
        <strong>Results:</strong><br>
        Loan-to-Value (LTV): ${LTV.toFixed(2)}%<br>
        Monthly Payment: $${MP.toFixed(2)}<br>
        Debt-to-Income (DTI): ${DTI.toFixed(2)}%<br>
        Total Interest Paid: $${totalInterest.toFixed(2)}
      `;

      // Amortization Table
      let amortTable = `
        <h3>Amortization Schedule</h3>
        <table>
          <thead>
            <tr>
              <th>Month</th>
              <th>Payment</th>
              <th>Principal</th>
              <th>Interest</th>
              <th>Balance</th>
            </tr>
          </thead>
          <tbody>
      `;

      let balance = DLA;
      for (let i = 1; i <= n; i++) {
        let interest = balance * monthlyRate;
        let principal = MP - interest;
        balance -= principal;
        if (balance < 0) balance = 0;
        amortTable += `
          <tr>
            <td>${i}</td>
            <td>$${MP.toFixed(2)}</td>
            <td>$${principal.toFixed(2)}</td>
            <td>$${interest.toFixed(2)}</td>
            <td>$${balance.toFixed(2)}</td>
          </tr>
        `;
      }

      amortTable += `</tbody></table>`;
      document.getElementById("results").innerHTML += amortTable;
    }
  </script>
</body>
</html>
