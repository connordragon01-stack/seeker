[aletheia.html](https://github.com/user-attachments/files/22992579/aletheia.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aletheia Prototype Web</title>
<style>
    body { font-family: Arial, sans-serif; background-color: #f9f9f9; padding: 20px; }
    h1 { color: #333; }
    table { border-collapse: collapse; width: 100%; margin-top: 20px; }
    th, td { border: 1px solid #999; padding: 8px; text-align: center; }
    th { background-color: #333; color: #fff; }
    input, button { padding: 8px; margin: 5px 0; }
    .pillars, .actions { margin-top: 20px; }
</style>
</head>
<body>

<h1>Aletheia Prototype Web</h1>

<div class="pillars">
    <h2>Pillars</h2>
    <input type="text" id="newPillar" placeholder="Add new pillar">
    <button onclick="addPillar()">Add Pillar</button>
    <div id="pillarList"></div>
</div>

<div class="actions">
    <h2>Actions</h2>
    <input type="text" id="actionName" placeholder="Action name">
    <input type="text" id="actionScores" placeholder="Scores (comma separated)">
    <button onclick="addAction()">Add Action</button>
</div>

<div class="scores">
    <h2>Action Scores</h2>
    <button onclick="calculateScores()">Calculate Best Action</button>
    <div id="scoreTable"></div>
    <h3 id="bestAction"></h3>
</div>

<script>
let pillars = ["PRESERVE_LIFE", "ETHICS", "EQUITY", "OBEDIENCE"];
let actions = [];

function updatePillarList() {
    document.getElementById('pillarList').innerText = pillars.join(", ");
}

function addPillar() {
    const p = document.getElementById('newPillar').value.trim();
    if(p && !pillars.includes(p)) {
        pillars.push(p);
        updatePillarList();
        document.getElementById('newPillar').value = '';
    }
}

function addAction() {
    const name = document.getElementById('actionName').value.trim();
    const scores = document.getElementById('actionScores').value.trim().split(',').map(Number);
    if(name && scores.length === pillars.length) {
        let action = { "name": name };
        for(let i=0; i<pillars.length; i++) {
            action[pillars[i]] = scores[i];
        }
        actions.push(action);
        document.getElementById('actionName').value = '';
        document.getElementById('actionScores').value = '';
        alert('Action added!');
    } else {
        alert('Scores must match number of pillars.');
    }
}

function calculateScores() {
    let tableHTML = "<table><tr><th>Action</th>";
    for(let p of pillars) tableHTML += `<th>${p}</th>`;
    tableHTML += "<th>Total Score</th></tr>";

    let bestScore = -1;
    let bestActionName = "";
    for(let action of actions) {
        let total = 0;
        tableHTML += `<tr><td>${action.name}</td>`;
        for(let p of pillars) {
            let val = action[p] || 0;
            total += val;
            tableHTML += `<td>${val}</td>`;
        }
        tableHTML += `<td>${total}</td></tr>`;
        if(total > bestScore) {
            bestScore = total;
            bestActionName = action.name;
        }
    }
    tableHTML += "</table>";
    document.getElementById('scoreTable').innerHTML = tableHTML;
    document.getElementById('bestAction').innerText = "Best action according to pillars: " + bestActionName;
}

// Initialize pillar display
updatePillarList();
</script>

</body>
</html>
