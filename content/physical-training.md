
---
title: "Physical Training"
date: 2026-11-18
lastmod: 2026-11-18
layout: "posts"
---

<style>
* { box-sizing: border-box; }

body {
    margin: 0;
    padding: 22px;
    font-family: Arial, sans-serif;
    background: #111827;
    color: white;
}

h1 { text-align: center; margin-bottom: 5px; }

.intro {
    text-align: center;
    color: #cbd5e1;
    margin-bottom: 22px;
}

#cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
    padding: 20px;
    background: #263449;
    border-radius: 14px;
    min-height: 110px;
    margin-bottom: 25px;
}

.card {
    background: #f8fafc;
    color: #172033;
    padding: 12px 16px;
    border-radius: 8px;
    cursor: grab;
    font-weight: bold;
    user-select: none;
    border: 2px solid transparent;
}

.card:active { cursor: grabbing; }

.card.selected {
    border-color: #fbbf24;
    background: #fef3c7;
}

.zones {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
}

.zone {
    min-height: 190px;
    border-radius: 13px;
    padding: 15px;
    border: 2px dashed #64748b;
    background: #1e293b;
}

.zone h3 {
    margin: 0 0 8px;
    text-align: center;
}

.zone p {
    text-align: center;
    font-size: 13px;
    color: #cbd5e1;
}

.zone.over {
    border-color: #fbbf24;
    background: #334155;
}

.zone .card {
    display: inline-block;
    margin: 5px;
    font-size: 13px;
    cursor: default;
}

.movement h3 { color: #60a5fa; }
.activity h3 { color: #34d399; }
.exercise h3 { color: #c084fc; }
.training h3 { color: #fb923c; }

#feedback {
    text-align: center;
    margin: 22px 0;
    font-size: 17px;
    min-height: 25px;
}

.controls {
    text-align: center;
}

button {
    padding: 12px 22px;
    margin: 6px;
    border: none;
    border-radius: 8px;
    background: #fbbf24;
    color: #172033;
    font-size: 15px;
    font-weight: bold;
    cursor: pointer;
}

button:hover { background: #fde68a; }

#score {
    text-align: center;
    color: #fbbf24;
    font-weight: bold;
    margin-top: 15px;
}

@media(max-width:600px) {
    .zones { grid-template-columns: 1fr; }
}
</style>


<h1>Move, Act, Exercise or Train?</h1>

<p class="intro">
Drag each word card into the category you think it belongs to.
</p>

<div id="cards"></div>

<div class="zones">

<div class="zone movement" data-category="movement">
<h3>01. Physical Movements</h3>
<p>Individual bodily actions or changes in body position</p>
</div>

<div class="zone activity" data-category="activity">
<h3>02. Physical Activities</h3>
<p>Bodily movements involving energy expenditure</p>
</div>

<div class="zone exercise" data-category="exercise">
<h3>03. Physical Exercises</h3>
<p>Planned and structured movements for fitness</p>
</div>

<div class="zone training" data-category="training">
<h3>04. Physical Training</h3>
<p>Systematic practice to develop physical abilities</p>
</div>

</div>

<div id="feedback">
Choose a card and classify it.
</div>

<div class="controls">
<button onclick="checkAnswers()">Check Answers</button>
<button onclick="resetGame()">Start Again</button>
</div>

<div id="score"></div>

<script>


const questions = [
    // Physical Movements (6)
    ["Bending the knees", "movement"],
    ["Rotating the shoulders", "movement"],
    ["Raising an arm", "movement"],
    ["Twisting the torso", "movement"],
    ["Kneeling down", "movement"],
    ["Turning the head", "movement"],

    // Physical Activities (6)
    ["Gardening", "activity"],
    ["Dancing", "activity"],
    ["Playing football", "activity"],
    ["Hiking", "activity"],
    ["Cycling to school", "activity"],
    ["Washing the car", "activity"],

    // Physical Exercises (5)
    ["Push-ups", "exercise"],
    ["Squats", "exercise"],
    ["Sit-ups", "exercise"],
    ["Plank", "exercise"],
    ["Skipping for fitness", "exercise"],

    // Physical Training (5)
    ["Endurance training", "training"],
    ["Strength training", "training"],
    ["Agility training", "training"],
    ["Speed training", "training"],
    ["Circuit training", "training"]
];


const cards = document.getElementById("cards");
const zones = document.querySelectorAll(".zone");

let selected = null;

function shuffle(array) {
    return array.sort(() => Math.random() - 0.5);
}

function createCards() {
    cards.innerHTML = "";

    shuffle([...questions]).forEach((item, index) => {
        const card = document.createElement("div");

        card.className = "card";
        card.textContent = item[0];
        card.draggable = true;
        card.dataset.answer = item[1];
        card.id = "card" + index;

        card.addEventListener("dragstart", e => {
            selected = card;
            e.dataTransfer.setData("text/plain", card.id);
        });

        card.addEventListener("click", () => {
            document.querySelectorAll(".card").forEach(c =>
                c.classList.remove("selected")
            );

            selected = card;
            card.classList.add("selected");
            document.getElementById("feedback").textContent =
                "Selected: " + card.textContent +
                ". Now click a category.";
        });

        cards.appendChild(card);
    });
}

zones.forEach(zone => {

    zone.addEventListener("dragover", e => {
        e.preventDefault();
        zone.classList.add("over");
    });

    zone.addEventListener("dragleave", () => {
        zone.classList.remove("over");
    });

    zone.addEventListener("drop", e => {
        e.preventDefault();
        zone.classList.remove("over");

        const id = e.dataTransfer.getData("text/plain");
        const card = document.getElementById(id);

        if (card) {
            zone.appendChild(card);
            card.classList.remove("selected");
            selected = null;
        }
    });

    zone.addEventListener("click", () => {
        if (selected) {
            zone.appendChild(selected);
            selected.classList.remove("selected");
            selected = null;
        }
    });
});

function checkAnswers() {
    let correct = 0;
    let total = 0;

    document.querySelectorAll(".zone .card").forEach(card => {
        total++;

        if (card.dataset.answer ===
            card.parentElement.dataset.category) {
            correct++;
            card.style.borderColor = "#22c55e";
        } else {
            card.style.borderColor = "#ef4444";
        }
    });

    document.getElementById("score").textContent =
        "Correct: " + correct + " / 16 | Classified: " +
        total + " / 16";

    document.getElementById("feedback").textContent =
        correct === 16
        ? "Excellent! You have classified all the cards correctly."
        : "Review the red-bordered cards and try again.";
}

function resetGame() {
    createCards();
    document.getElementById("score").textContent = "";
    document.getElementById("feedback").textContent =
        "Choose a card and classify it.";
    selected = null;
}

createCards();

</script>


