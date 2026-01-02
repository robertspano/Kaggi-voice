const express = require('express');
const axios = require('axios');
const app = express();
app.use(express.json());

const GOOGLE_API_KEY = process.env.GOOGLE_API_KEY;

app.post('/speak', async (req, res) => {
    try {
        const text = req.body.text || "Hæ, ég er Kaggi.";
        const response = await axios.post(
            `https://texttospeech.googleapis.com/v1/text:synthesize?key=${GOOGLE_API_KEY}`,
            {
                input: { text: text },
                voice: { languageCode: "is-IS", name: "is-IS-Standard-A" },
                audioConfig: { audioEncoding: "MP3" }
            }
        );
        res.json({ audioContent: response.data.audioContent });
    } catch (error) {
        res.status(500).send(error.message);
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server keyrir á porti ${PORT}`));
