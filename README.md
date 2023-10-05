# InstallCodeChat

  
<details>
<summary>Manual de Instalação Chatwoot</summary>

### Atualize sua máquina com os últimos pacotes

```bash
sudo apt update && apt upgrade -y
```

### Baixe o instalador automático do Chatwoot

```bash
wget https://get.chatwoot.app/linux/install.sh
```

### Execute a permisão no arquivo install.sh

```bash
chmod +x install.sh
```

### Inicie a instalação, digite "yes" para SSL, em seguida digite seu dominio e prossiga confimando com yes.
### Esse processo vai levar média ~ 15

  ```bash
./install.sh --install
  ```

Use as opções abaixo

yes

app.dominio.com.br

contato@dominio.com.br

yes para todos

### Alterando Idioma e ativando sua tela de cadastro

```bash
nano /home/chatwoot/chatwoot/.env
```

Altere a linha:

`DEFAULT_LOCALE=pt_BR` para `ENABLE_ACCOUNT_SIGNUP=true`

```bash
systemctl daemon-reload && systemctl restart chatwoot.target
```

Acesse: app.seudominio.com.br

Faça seu cadastro

### Habilitando configurações ocultas do Chatwoot no banco de dados PostgreSQL

```bash
sudo -i -u postgres psql
\c chatwoot_production
```

```bash
update installation_configs set locked = false;
```

```bash
\q
```

</details>

<details>
  
<summary>Instalando CodeChat</summary>

```bash
cd
```

```bash
sudo apt update && apt upgrade -y
```

```bash
git clone https://github.com/code-chat-br/whatsapp-api.git
```

```bash
cd whatsapp-api
```

```bash
cd src

```bash
mv dev-env.yml env.yml
```

```bash
nano env.yml
```

Altere Linha 72

```bash
URL: https://conector.site/webhook/codechat
```

Altere Linha 73

ENABLED: false

para

ENABLED: true

```bash
cd ..

```bash
npm i
```

```bash
npm run build
```

```bash
pm2 start 'npm run start' --name Codechat
```

```bash
sudo nano /etc/nginx/sites-available/codechat
```

```bash
server {

  server_name codechat.dominio.com.br;

  location / {

    proxy_pass http://127.0.0.1:8080;

    proxy_http_version 1.1;

    proxy_set_header Upgrade $http_upgrade;

    proxy_set_header Connection 'upgrade';

    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header X-Forwarded-Proto $scheme;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    proxy_cache_bypass $http_upgrade;

    proxy_buffering off;

    proxy_cache off;

  }

  }
```

```bash
sudo ln -s /etc/nginx/sites-available/codechat /etc/nginx/sites-enabled
```

```bash
sudo certbot --nginx
```

```bash
sudo service nginx restart
```

**EXECUTE COMANDO ABAIXO PARA NÃO CAIR QUANDO REINICIAR A VPS**

```bash
sudo pm2 startup ubuntu -u root && sudo pm2 startup ubuntu -u root --hp /root && sudo pm2 save
```

</details>

<details>

<summary>Instalando Integrador</summary>

```bash
cd
```

```bash
sudo apt update && apt upgrade -y
```

```bash
git clone https://github.com/w3nder/chatwoot-codechat
```

```bash
cd chatwoot-codechat
```

```bash
nano .env
```

```bash
PORT = 1234
CHATWOOT_ACCOUNT_ID = NUMEROCONTACHATWOOT
CHATWOOT_TOKEN = TOKENDOCHATWOOT
CHATWOOT_BASE_URL = https://chatwoot.seusite.com.br
CODECHAT_BASE_URL = https://codechat.seusite.com.br
CODECHAT_API_KEY = t8OOEeISKzpmc3jjcMqBWYSaJsafdefer
TOSIGN=true
IMPORT_MESSAGES_SENT=true
 ```

```bash
npm install pm2 -g
```

```bash
npm install
```

```bash
npm run build
```

```bash
pm2 start dist/app.js --name conector
```

```bash
sudo nano /etc/nginx/sites-available/conector
```

```bash
server {

  server_name conector.dominio.com.br;

  location / {

    proxy_pass http://127.0.0.1:1234;

    proxy_http_version 1.1;

    proxy_set_header Upgrade $http_upgrade;

    proxy_set_header Connection 'upgrade';

    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header X-Forwarded-Proto $scheme;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    proxy_cache_bypass $http_upgrade;

    proxy_buffering off;

    proxy_cache off;

  }

  }
  
 ```

```bash
sudo ln -s /etc/nginx/sites-available/conector /etc/nginx/sites-enabled

```bash
sudo certbot --nginx
```

```bash
sudo service nginx restart
```

EXECUTE COMANDO ABAIXO PARA NÃO CAIR QUANDO REINICIAR A VPS

```bash
sudo pm2 startup ubuntu -u root && sudo pm2 startup ubuntu -u root --hp /root && sudo pm2 save
```




</details>

**Conectando Caixa de Entrada**


WEBHOOK CHATWOOT:

Adicione essa url no seu Chatwoot

https://conector.site/webhook/chatwoot

Crie um contato chamado BOT

Adicione numero telefone ao mesmo

+123456

Chame contato BOT escreva 

/iniciar
