## Environment Variables

Dexios can read your key from an environment variable! Just set `DEXIOS_KEY` and it will automatically be detected and used. Due to using different salts and nonces for every encryption, there is no inherent risk in reusing keys - although it's not a good security practice.