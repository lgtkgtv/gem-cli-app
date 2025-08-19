# Gemini cli 

```
## Node.js  LTS install 

curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt-get install -y nodejs

## ONE TIME:  
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'

npm install -g npm@11.5.2
nodejs -v
npm -v

#   Change the default directory where npm installs global packages. 
#   This moves the installation location from a system-owned directory to a user-owned one, eliminating the need for sudo
export PATH=~/.npm-global/bin:$PATH

## Instructions to build gemini-cli from source code
cd $HOME
git clone https://github.com/google-gemini/gemini-cli.git
cd  gemini-cli

npm install
npm run build
npm run start

node ~/gemini-cli/bundle/gemini.js    # Launches gemini cli 
    OR
alias gem="node ~/gemini-cli/bundle/gemini.js"
# gemini auth login

```

---

# GEMINI_API_KEY 

    https://aistudio.google.com/apikey
    https://ai.google.dev/gemini-api/docs/quickstart?lang=python

```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent" \
  -H 'Content-Type: application/json' \
  -H "X-goog-api-key: $GEMINI_API_KEY" \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain how AI works in a few words"
          }
        ]
      }
    ]
  }'
```