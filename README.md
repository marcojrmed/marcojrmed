<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MedTrack Dashboard</title>
    <link rel="icon" href="favicon.png" type="image/png">
    <style>
        /* Estilos Avançados */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f2f5;
            color: #333;
        }
        header {
            background-color: #1e90ff;
            color: white;
            padding: 1rem;
            text-align: center;
            font-size: 1.8rem;
            font-weight: bold;
        }
        nav {
            display: flex;
            justify-content: space-around;
            background-color: #333;
            padding: 0.5rem 0;
        }
        nav a {
            color: white;
            text-decoration: none;
            font-weight: bold;
            padding: 0.5rem 1rem;
            transition: background 0.3s;
        }
        nav a:hover {
            background-color: #1e90ff;
            border-radius: 5px;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            margin: 2rem auto;
            max-width: 1200px;
        }
        .sidebar {
            flex: 1;
            max-width: 200px;
            background-color: #333;
            color: white;
            padding: 1rem;
            border-radius: 5px;
        }
        .content {
            flex: 3;
            background-color: white;
            margin-left: 1rem;
            padding: 1.5rem;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .card {
            margin: 1rem 0;
            padding: 1rem;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        button {
            background-color: #1e90ff;
            color: white;
            border: none;
            padding: 0.7rem 1rem;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s;
            border-radius: 5px;
        }
        button:hover {
            background-color: #0077cc;
        }
        footer {
            text-align: center;
            padding: 1rem;
            background-color: #1e90ff;
            color: white;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
</head>
<body>
    <!-- Cabeçalho -->
    <header>
        MedTrack - Dashboard Inteligente
    </header>
    
    <!-- Barra de Navegação -->
    <nav>
        <a href="#cronograma">Cronograma</a>
        <a href="#simulados">Simulados</a>
        <a href="#resumos">Resumos</a>
        <a href="#relatorios">Relatórios</a>
        <a href="#comunidade">Comunidade</a>
    </nav>

    <!-- Estrutura Principal -->
    <div class="container">
        <!-- Menu Lateral -->
        <div class="sidebar">
            <h3>Menu</h3>
            <ul>
                <li>Cronograma Inteligente</li>
                <li>Simulados Personalizados</li>
                <li>Resumos Automáticos</li>
                <li>Relatórios de Desempenho</li>
                <li>Comunidade</li>
            </ul>
        </div>

        <!-- Conteúdo -->
        <div class="content">
            <!-- Cronograma -->
            <section id="cronograma" class="card">
                <h2>📅 Cronograma Inteligente</h2>
                <p>Insira seus dados para criar um cronograma otimizado.</p>
                <button onclick="gerarCronograma()">Gerar Cronograma</button>
                <p id="cronograma-output"></p>
            </section>

            <!-- Simulados -->
            <section id="simulados" class="card">
                <h2>📝 Simulados Personalizados</h2>
                <button onclick="iniciarSimulado()">Iniciar Simulado</button>
                <p id="simulado-output"></p>
            </section>

            <!-- Resumos -->
            <section id="resumos" class="card">
                <h2>📚 Resumos Automáticos</h2>
                <button onclick="gerarResumo()">Gerar Resumo</button>
                <p id="resumo-output"></p>
            </section>
        </div>
    </div>

    <!-- Rodapé -->
    <footer>
        © 2024 MedTrack. Todos os direitos reservados.
    </footer>

    <!-- JavaScript -->
    <script>
        function gerarCronograma() {
            document.getElementById('cronograma-output').innerText = "Cronograma gerado com sucesso! Confira sua agenda.";
        }
        function iniciarSimulado() {
            document.getElementById('simulado-output').innerText = "Simulado iniciado! Boa sorte!";
        }
        function gerarResumo() {
            document.getElementById('resumo-output').innerText = "Resumo automático gerado! Consulte os detalhes.";
        }
    </script>
</body>
</html>
node -v
npm -v
mkdir medtrack-backend
cd medtrack-backend
npm init -y
npm install express body-parser fs
medtrack-backend/
│
├── server.js
└── db.json
const express = require('express');
const bodyParser = require('body-parser');
const fs = require('fs');

const app = express();
const PORT = 3000;

// Middleware para processar JSON
app.use(bodyParser.json());

// Simula um "banco de dados" usando db.json
const DB_FILE = './db.json';

// Função para ler dados do "banco de dados"
const readData = () => {
    const data = fs.readFileSync(DB_FILE, 'utf-8');
    return JSON.parse(data);
};

// Função para salvar dados no "banco de dados"
const saveData = (data) => {
    fs.writeFileSync(DB_FILE, JSON.stringify(data, null, 2), 'utf-8');
};

// Rota de teste
app.get('/', (req, res) => {
    res.send('Bem-vindo ao backend do MedTrack!');
});

// Rota para gerar cronograma
app.post('/cronograma', (req, res) => {
    const { nome, materias, horasDia } = req.body;

    if (!nome || !materias || !horasDia) {
        return res.status(400).send('Dados incompletos.');
    }

    // Simula a geração do cronograma
    const cronograma = materias.map((materia, index) => ({
        dia: `Dia ${index + 1}`,
        materia,
        horas: horasDia / materias.length
    }));

    // Lê e atualiza os dados
    const db = readData();
    db.cronogramas.push({ usuario: nome, cronograma });
    saveData(db);

    res.status(201).json({ message: 'Cronograma gerado com sucesso!', cronograma });
});

// Rota para listar todos os cronogramas
app.get('/cronograma', (req, res) => {
    const db = readData();
    res.json(db.cronogramas);
});

// Inicia o servidor
app.listen(PORT, () => {
    console.log(`Servidor rodando em http://localhost:${PORT}`);
});
{
  "cronogramas": []
}
node server.js
{
  "nome": "João Silva",
  "materias": ["Anatomia", "Fisiologia", "Bioquímica"],
  "horasDia": 6
}
