// Rankeid Corp - Full Stack App (Ready to Deploy)

/* This project includes:

1. Backend (Node.js + Express + PostgreSQL + OpenAI API)


2. Frontend (React Native Mobile App)


3. Database Scripts


4. Deployment Ready Configuration */



// ------------------------- // Backend Folder: backend/ // -------------------------

/* Install dependencies: npm init -y npm install express cors body-parser pg bcrypt jsonwebtoken dotenv openai */

// File: backend/server.js require('dotenv').config(); const express = require('express'); const cors = require('cors'); const bodyParser = require('body-parser'); const { Pool } = require('pg'); const bcrypt = require('bcrypt'); const jwt = require('jsonwebtoken'); const { Configuration, OpenAIApi } = require('openai');

const app = express(); app.use(cors()); app.use(bodyParser.json());

// Database connection const pool = new Pool({ user: process.env.DB_USER, host: process.env.DB_HOST, database: process.env.DB_NAME, password: process.env.DB_PASS, port: process.env.DB_PORT, });

// OpenAI setup const configuration = new Configuration({ apiKey: process.env.OPENAI_API_KEY }); const openai = new OpenAIApi(configuration);

// JWT Auth middleware const authenticate = (req, res, next) => { const token = req.headers['authorization']; if (!token) return res.status(401).send('Access denied'); try { const verified = jwt.verify(token, process.env.JWT_SECRET); req.user = verified; next(); } catch { res.status(400).send('Invalid token'); } };

// Routes app.post('/api/register', async (req, res) => { const { username, email, password } = req.body; const hashed = await bcrypt.hash(password, 10); const user = await pool.query('INSERT INTO users(username,email,password) VALUES($1,$2,$3) RETURNING *', [username,email,hashed]); res.json(user.rows[0]); });

app.post('/api/login', async (req,res)=>{# Rankeid-corp-
(server.js, routes/, models/, ai/, package.json, .env.example)
