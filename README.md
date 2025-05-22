const express = require('express');
const multer = require('multer');
const cors = require('cors');
const fs = require('fs');
const { Configuration, OpenAIApi } = require('openai');

const app = express();
const port = 5000;

const upload = multer({ dest: 'uploads/' });
app.use(cors());
app.use
const express = require('express');
const multer = require('multer');
const cors = require('cors');
const fs = require('fs');
const { Configuration, OpenAIApi } = require('openai');
require('dotenv').config();

const app = express();
const port = 5000;
const upload = multer({ dest: 'uploads/' });

app.use(cors());
app.use(express.json());

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY, // Use .env for safety
});

const openai = new OpenAIApi(configuration);

app.post('/edit', upload.single('image'), async (req, res) => {
  const prompt = req.body.prompt;

  try {
    const response = await openai.createImage({
      prompt: prompt,
      n: 1,
      size: '1024x1024',
      response_format: 'b64_json'
    });

    const b64 = response.data.data[0].b64_json;
    const imgBuffer = Buffer.from(b64, 'base64');

    res.setHeader('Content-Type', 'image/png');
    res.send(imgBuffer);
  } catch (error) {
    console.error(error.response?.data || error.message);
    res.status(500).send('Image generation failed.');
  }
});

app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
OPENAI_API_KEY=your_openai_api_key_here
npm install express multer cors openai dotenv
node server.js
