const https = require('https');
const fs = require('fs');

// Read the SSL certificate and private key files
const options = {
  key: fs.readFileSync('server.key'),
  cert: fs.readFileSync('server.cert')
};

// Create a simple request listener
const requestListener = function (req, res) {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello from the secure HTTPS server!\n');
};

// Create the HTTPS server
const server = https.createServer(options, requestListener);

// Listen on port 8443 (common for HTTPS development, 443 is standard for production)
const PORT = 8443;
server.listen(PORT, () => {
  console.log(`HTTPS Server running on https://localhost:${PORT}/`);
  console.log('NOTE: Since this is a self-signed certificate, your browser will likely show a warning. You\'ll need to accept the risk to proceed.');
});

// Basic error handling
server.on('error', (err) => {
  if (err.code === 'EADDRINUSE') {
    console.error(`Port ${PORT} is already in use. Please choose another port or stop the conflicting process.`);
  } else {
    console.error(`Server error: ${err.message}`);
  }
});