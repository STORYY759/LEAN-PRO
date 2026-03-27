# LEAN-PRO <!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Lean Pro</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body {
font-family: Arial;
background: #0f0f0f;
color: white;
padding: 20px;
}
.container { max-width: 500px; margin: auto; }
input, select, button {
width: 100%; margin: 8px 0; padding: 10px;
border-radius: 8px; border: none;
}
button { background: #ff6b3d; color: white; }
.card {
background: #1c1c1c;
padding: 15px;
border-radius: 10px;
margin-top: 15px;
}
</style>
</head>

<body>
<div class="container">

<h2>🔥 Lean Pro</h2>

<!-- PROFIL -->
<div class="card">
<h3>Profil</h3>
<input id="poids" placeholder="Poids (kg)">
<input id="bodyfat" placeholder="Body fat %">
<input id="steps" placeholder="Pas / jour">
</div>

<!-- OBJECTIF -->
<div class="card">
<h3>Objectif</h3>
<select id="goal">
<option value="-500">Perte rapide</option>
<option value="-300">Perte lente</option>
<option value="0">Maintien</option>
<option value="300">Prise</option>
</select>
</div>

<button onclick="calculate()">Calculer</button>

<div class="card" id="result"></div>

<!-- 📸 IA CALORIES -->
<div class="card">
<h3>📸 Calories (texte rapide)</h3>
<input id="meal" placeholder="Ex: riz + poulet + sauce">
<button onclick="analyzeMeal()">Analyser</button>
<div id="mealResult"></div>
</div>

<!-- 📊 POIDS GRAPH -->
<div class="card">
<h3>📊 Suivi poids</h3>
<input id="newWeight" placeholder="Ajouter poids">
<button onclick="addWeight()">Ajouter</button>
<canvas id="chart"></canvas>
</div>

<!-- 🏋️ MUSCU -->
<div class="card">
<h3>🏋️ Tracker muscu</h3>
<input id="exercise" placeholder="Ex: Bench press">
<input id="reps" placeholder="Reps">
<input id="weightLift" placeholder="Poids">
<button onclick="addWorkout()">Ajouter</button>
<div id="workoutList"></div>
</div>

<!-- 🎯 PLAN -->
<div class="card">
<h3>🎯 Plan automatique</h3>
<button onclick="generatePlan()">Générer</button>
<div id="plan"></div>
</div>

</div>

<script>

// ===== CALCUL =====
function calculate() {
let poids = +document.getElementById("poids").value;
let bf = +document.getElementById("bodyfat").value;
let steps = +document.getElementById("steps").value;
let goal = +document.getElementById("goal").value;

let lean = poids * (1 - bf/100);
let bmr = 370 + (21.6 * lean);
let neat = steps * 0.04;
let tef = (bmr + neat) * 0.1;

let tdee = bmr + neat + tef;
let target = tdee + goal;

document.getElementById("result").innerHTML =
`🔥 TDEE : ${tdee.toFixed(0)} kcal<br>🎯 Objectif : ${target.toFixed(0)} kcal`;
}

// ===== IA CALORIES (SIMPLIFIÉE) =====
function analyzeMeal() {
let meal = document.getElementById("meal").value;

// simulation IA (simple)
let calories = meal.length * 20;

document.getElementById("mealResult").innerHTML =
`🍽️ Estimation : ${calories} kcal`;
}

// ===== GRAPH =====
let weights = JSON.parse(localStorage.getItem("weights")) || [];

function addWeight() {
let w = +document.getElementById("newWeight").value;
weights.push(w);
localStorage.setItem("weights", JSON.stringify(weights));
updateChart();
}

function updateChart() {
new Chart(document.getElementById("chart"), {
type: "line",
data: {
labels: weights.map((_, i) => i+1),
datasets: [{ data: weights }]
}
});
}

updateChart();

// ===== MUSCU =====
let workouts = JSON.parse(localStorage.getItem("workouts")) || [];

function addWorkout() {
let ex = document.getElementById("exercise").value;
let reps = document.getElementById("reps").value;
let weight = document.getElementById("weightLift").value;

workouts.push(`${ex} - ${reps} reps - ${weight}kg`);
localStorage.setItem("workouts", JSON.stringify(workouts));

displayWorkouts();
}

function displayWorkouts() {
document.getElementById("workoutList").innerHTML =
workouts.map(w => `<p>${w}</p>`).join("");
}

displayWorkouts();

// ===== PLAN AUTO =====
function generatePlan() {
let plan = `
🏋️ 3x muscu
🏃 2x cardio
🚶 8000+ pas / jour
🍽️ déficit contrôlé
😴 sommeil 7h+
`;
document.getElementById("plan").innerHTML = plan;
}

</script>

</body>
</html>
