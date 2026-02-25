const express = require("express");
const app = express();
const port = 3000;

app.use(express.json());

// Array-based storage
let cards = [];

/* ========================
   RESTful API Endpoints
======================== */

// GET all cards
app.get("/api/cards", (req, res) => {
    res.json(cards);
});

// POST new card
app.post("/api/cards", (req, res) => {
    const card = {
        id: Date.now(),
        suit: req.body.suit,
        value: req.body.value
    };
    cards.push(card);
    res.json(card);
});

// PUT update card
app.put("/api/cards/:id", (req, res) => {
    const id = parseInt(req.params.id);
    const card = cards.find(c => c.id === id);

    if (card) {
        card.suit = req.body.suit;
        card.value = req.body.value;
        res.json(card);
    } else {
        res.status(404).json({ message: "Card not found" });
    }
});

// DELETE card
app.delete("/api/cards/:id", (req, res) => {
    const id = parseInt(req.params.id);
    cards = cards.filter(c => c.id !== id);
    res.json({ message: "Card deleted" });
});

/* ========================
   HTML + CSS + JS UI
======================== */

app.get("/", (req, res) => {
    res.send(`
    <!DOCTYPE html>
    <html>
    <head>
        <title>Playing Card Collection</title>
        <style>
            body {
                font-family: Arial;
                background: #f4f6f9;
                text-align: center;
                margin-top: 40px;
            }
            .container {
                width: 600px;
                margin: auto;
                background: white;
                padding: 20px;
                border-radius: 10px;
                box-shadow: 0 0 10px rgba(0,0,0,0.1);
            }
            input {
                padding: 8px;
                margin: 5px;
            }
            button {
                padding: 8px 12px;
                background: #007bff;
                color: white;
                border: none;
                border-radius: 5px;
                cursor: pointer;
            }
            button:hover {
                background: #0056b3;
            }
            table {
                width: 100%;
                margin-top: 20px;
                border-collapse: collapse;
            }
            th, td {
                border: 1px solid #ddd;
                padding: 8px;
            }
            th {
                background: #007bff;
                color: white;
            }
            .delete-btn {
                background: red;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <h2>Playing Card Collection</h2>
            <input type="text" id="suit" placeholder="Suit (Hearts, Spades)">
            <input type="text" id="value" placeholder="Value (A, 2, K)">
            <button onclick="addCard()">Add Card</button>

            <table>
                <thead>
                    <tr>
                        <th>Suit</th>
                        <th>Value</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody id="cardTable"></tbody>
            </table>
        </div>

        <script>
            async function fetchCards() {
                const res = await fetch('/api/cards');
                const data = await res.json();
                const table = document.getElementById("cardTable");
                table.innerHTML = "";

                data.forEach(card => {
                    table.innerHTML += \`
                        <tr>
                            <td>\${card.suit}</td>
                            <td>\${card.value}</td>
                            <td>
                                <button class="delete-btn" onclick="deleteCard(\${card.id})">Delete</button>
                            </td>
                        </tr>
                    \`;
                });
            }

            async function addCard() {
                const suit = document.getElementById("suit").value;
                const value = document.getElementById("value").value;

                await fetch('/api/cards', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ suit, value })
                });

                document.getElementById("suit").value = "";
                document.getElementById("value").value = "";
                fetchCards();
            }

            async function deleteCard(id) {
                await fetch('/api/cards/' + id, {
                    method: 'DELETE'
                });
                fetchCards();
            }

            fetchCards();
        </script>
    </body>
    </html>
    `);
});

app.listen(port, () => {
    console.log("Server running at http://localhost:3000");
});
