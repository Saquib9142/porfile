const express = require('express');
const multer = require('multer');
const fs = require('fs');
const axios = require('axios');
const path = require('path');
require('dotenv').config();

const app = express();
const upload = multer({ dest: 'uploads/' });

const {
  GITHUB_TOKEN,
  GITHUB_USERNAME,
  GITHUB_REPO
} = process.env;

app.post('/upload', upload.single('image'), async (req, res) => {
  const filePath = req.file.path;
  const fileName = req.file.originalname;
  const fileContent = fs.readFileSync(filePath, { encoding: 'base64' });

  const githubApiUrl = `https://api.github.com/repos/${GITHUB_USERNAME}/${GITHUB_REPO}/contents/images/${fileName}`;

  try {
    const response = await axios.put(githubApiUrl, {
      message: `Uploaded ${fileName}`,
      content: fileContent
    }, {
      headers: {
        Authorization: `token ${GITHUB_TOKEN}`,
        Accept: 'application/vnd.github.v3+json'
      }
    });

    // Delete the file from local temp
    fs.unlinkSync(filePath);

    return res.json({
      success: true,
      url: response.data.content.download_url
    });

  } catch (error) {
    console.error(error.response?.data || error.message);
    return res.status(500).json({ success: false, error: 'Upload failed' });
  }
});

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
