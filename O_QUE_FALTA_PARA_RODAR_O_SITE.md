# CRP Luz — O Que Falta Para Rodar e Publicar o Site

Este documento descreve com clareza executiva e operacional o **status atual do projeto**, **como rodar imediatamente**, e o **checklist exato do que falta** para que a landing page institucional da **CRP Luz (Companhia Rio Preto Luz)** / **Smart Rio Preto** esteja no ar publicamente para os cidadãos de São José do Rio Preto.

---

## 🚦 STATUS ATUAL: O QUE JÁ ESTÁ 100% PRONTO

Na camada de **código, desenvolvimento e assets**, o projeto está **100% finalizado e aprovado**:

- [x] **Código-fonte limpo e sem dependências:** HTML5 semântico, CSS3 responsivo com layout editorial e JavaScript Vanilla inlined (sem React, Node ou frameworks pesados).
- [x] **Otimização de mídias:** Pasta `assets/` enxugada de 15,65 MB para **3,55 MB** (redução de 77%), com vídeo do hero oficial (`hero_crpluz.mp4`), poster instantâneo (`hero-poster.jpg`) e fotos operacionais.
- [x] **Segurança e Cache de Borda:** Arquivo `_headers` configurado com HSTS (1 ano), CSP restritivo, anti-clickjacking (`X-Frame-Options: DENY`) e cache escalável para picos.
- [x] **SEO e Indexação:** Arquivo `robots.txt`, tags canônicas, Open Graph e Twitter Cards configurados.
- [x] **Repositório Git sincronizado:** Todo o código está comitado e enviado para a branch `main` em:
  `https://github.com/TheyDreez/crp-luz`.
- [x] **Testes Automatizados de Navegador (Playwright):** 0 erros de console, 0 falhas de carregamento de assets, vídeo rodando e layout validado em Desktop (1920x1080) e Mobile (390x844).

---

## 💻 1. COMO RODAR LOCALMENTE AGORA MESMO (0 DEPENDÊNCIAS)

Para rodar e visualizar o site imediatamente no seu computador, escolha qualquer uma das opções:

### Opção A: Servidor Local Python (Recomendado)
Abra o terminal na pasta do projeto e execute:
```bash
python -m http.server 8080
```
Acesse no navegador: **`http://localhost:8080`**

### Opção B: Direto no Navegador (Dois cliques)
Dê um duplo clique no arquivo **`index.html`** dentro da pasta do projeto. Ele abrirá no Chrome, Edge ou Firefox com funcionamento pleno.

---

## 🌐 2. O QUE FALTA PARA COLOCAR O SITE PÚBLICO NO AR (PRODUÇÃO)

O que resta agora **não é código**, mas sim as **etapas operacionais, administrativas e de infraestrutura de nuvem** que exigem acesso às contas e ao domínio da empresa.

Abaixo está o checklist sequencial do que precisa ser executado:

### 📋 Passo 1: Definir a Hospedagem e Conectar ao GitHub (Tempo estimado: 5 minutos)
- [ ] **Ação:** Criar ou acessar a conta da organização no provedor de borda (recomendado: **Cloudflare Pages**, por ser gratuito, ter banda ilimitada e Anycast CDN global).
- [ ] **Ação:** No painel do Cloudflare (ou provedor de preferência), selecionar:
  - `Workers & Pages` > `Create application` > `Pages` > `Connect to Git`.
  - Conectar com a conta GitHub que possui o repositório `TheyDreez/crp-luz`.
  - Configuração de Build:
    - **Framework preset:** `None`
    - **Build command:** *(vazio)*
    - **Build output directory:** `/` (ou `.`)
    - **Production branch:** `main`
  - Clicar em **Save and Deploy**.
- **Resultado imediato:** O site já estará no ar em uma URL provisória gratuita (ex.: `crp-luz.pages.dev`).

---

### 📋 Passo 2: Apontamento do Domínio Oficial e DNS (Tempo estimado: 10 a 30 minutos)
- [ ] **Ação:** Acessar o órgão de registro do domínio (ex.: **Registro.br** onde está registrado `crpluz.com.br` ou `smartriopreto.com.br`).
- [ ] **Ação:** Apontar os servidores DNS (Nameservers) para a Cloudflare ou configurar os registros:
  - Registro `CNAME` para `www` apontando para o endereço de publicação.
  - Redirecionamento da raiz (`crpluz.com.br`) para `www.crpluz.com.br` (ou vice-versa).
- [ ] **Ação:** No painel DNS da Cloudflare, garantir que o status do proxy esteja ativado (**Nuvem Laranja / Proxied**). Isso garante que o tráfego passe pelos escudos de proteção contra ataques DDoS e absorva os picos de tráfego.

---

### 📋 Passo 3: Ativar Certificados de Segurança SSL/TLS (Tempo estimado: 2 minutos)
- [ ] **Ação:** No painel da CDN/Cloudflare, ir na aba **SSL/TLS**.
- [ ] **Ação:** Marcar o modo de criptografia como **Full (Strict)**.
- [ ] **Ação:** Ativar a opção **Always Use HTTPS** (força redirecionamento automático de `http://` para `https://`).
- [ ] **Ação:** Ativar **Automatic HTTPS Rewrites** e **HTTP/3 (QUIC)**.

---

### 📋 Passo 4: Ativar Regras de Proteção contra Ataques e Bots (Tempo estimado: 3 minutos)
- [ ] **Ação:** No painel de Segurança / WAF da Cloudflare:
  - Ativar o **Bot Fight Mode** (bloqueia scrapers e robôs maliciosos que possam consumir banda).
  - Deixar o nível de segurança em **Medium** (ou elevar para **High** se houver campanha massiva na televisão/rádio).

---

### 📋 Passo 5: Validação Institucional e Teste do Canal 0800 (Tempo estimado: 15 minutos)
- [ ] **Ação:** **Fazer uma ligação teste real para `0800 921 4477`:**
  - Ligar a partir de um telefone celular (operadoras Claro, Vivo, TIM).
  - Ligar a partir de um telefone fixo.
  - Confirmar se a Unidade de Resposta Audível (URA) ou os atendentes da central de reparos da CRP Luz estão recebendo a chamada normalmente.
- [ ] **Ação:** **Aprovação formal do conteúdo editorial:**
  - Submeter os textos da página (especialmente o trecho sobre o Smart Rio Preto: 3.000 câmeras, OCR, 309 semáforos e reconhecimento facial) para validação jurídica/institucional da diretoria da concessionária antes de anunciar publicamente.

---

## 👥 MATRIZ DE RESPONSABILIDADE (QUEM FAZ O QUÊ)

| Item | Descrição | Responsável | Status |
| :--- | :--- | :--- | :--- |
| **Código & Assets** | HTML, CSS, JS, compressão de vídeo e imagens | **Engenheiro / Desenvolvedor** | ✅ **Concluído** |
| **Headers & Cache** | CSP, HSTS, robots.txt, regras de cache de borda | **Engenheiro / Desenvolvedor** | ✅ **Concluído** |
| **Documentação Técnica** | Especificação para Cloudflare e Consultoria de Cloud | **Engenheiro / Desenvolvedor** | ✅ **Concluído** |
| **Acesso ao GitHub** | Permissão de leitura do repo `TheyDreez/crp-luz` no Cloudflare | **Dono da conta GitHub / DevOps** | ⏳ **Aguardando** |
| **Configuração do Domínio** | Acesso ao Registro.br / DNS para apontar `crpluz.com.br` | **Administrador de TI / Dono do Domínio** | ⏳ **Aguardando** |
| **Painel de Cloud / CDN** | Conectar repo, ligar Proxy e ativar SSL Full Strict | **DevOps / Consultoria de Cloud** | ⏳ **Aguardando** |
| **Validação da Linha 0800** | Testar se o número `0800 921 4477` atende ligações | **Equipe Operacional / Atendimento** | ⏳ **Aguardando** |
| **Aprovação de Conteúdo** | Validação jurídica do texto do Smart Rio Preto | **Diretoria / Comunicação CRP Luz** | ⏳ **Aguardando** |

---

## 🚀 RESUMO EM UMA LINHA

> **Para rodar no seu computador agora:** Basta executar `python -m http.server 8080` ou dar dois cliques no `index.html`.
> **Para colocar no ar na internet:** Basta conectar o repositório GitHub `TheyDreez/crp-luz` no Cloudflare Pages (leva 3 minutos) e apontar o domínio `crpluz.com.br` no Registro.br.
