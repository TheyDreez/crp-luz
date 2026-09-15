# CRP Luz — Landing Page Institucional

Landing page oficial e institucional da **CRP Luz (Companhia Rio Preto Luz)**, concessionária responsável pela modernização, expansão e manutenção do parque de iluminação pública de São José do Rio Preto — SP.

O projeto foi construído e otimizado para alta performance, baixo custo operacional, segurança de borda e total resiliência a picos de tráfego.

---

## 🏛️ Arquitetura de Produção

A aplicação é **100% estática (JAMstack puro)**, eliminando servidores de aplicação, bancos de dados e superfícies de ataque dinâmicas.

```text
[ Usuário Final ]
       │  (HTTPS / HTTP/3)
       ▼
[ Cloudflare Edge / CDN Global ]
   ├── DNS Gerenciado (Proxy Ativo: Nuvem Laranja)
   ├── Proteção DDoS & Bot Management (Bot Fight Mode)
   ├── Web Application Firewall (WAF)
   ├── SSL/TLS Full (Strict) + HSTS
   └── Edge Cache (Regras em _headers)
       │  (Zero Roundtrip para Cache Hit)
       ▼
[ Cloudflare Pages Storage ]
   └── Arquivos Estáticos Pré-renderizados (HTML, CSS, JS, Imagens, Vídeo)
```

### Principais Benefícios:
1. **Pico de Acesso Ilimitado:** As requisições são servidas a partir dos mais de 300 data centers da Cloudflare no mundo, sem sobrecarregar servidores de origem.
2. **Custo Operacional Zero / Quase Zero:** Cloudflare Pages oferece banda e requests ilimitados no plano gratuito/básico.
3. **Resiliência Máxima:** Sem processos Node.js, PHP ou bancos de dados para travar ou cair sob carga.
4. **Segurança por Padrão:** Headers de segurança restritivos e superfícies de ataque nulas.

---

## 📁 Estrutura do Projeto

```text
crp-luz/
├── _headers                   # Regras de segurança (CSP, HSTS, X-Frame) e Cache-Control para Cloudflare Pages
├── robots.txt                 # Instruções de rastreamento para buscadores
├── index.html                 # Landing page única e semântica com CSS/JS vanilla inlined
├── README.md                  # Documentação operacional de engenharia
└── assets/
    ├── hero_crpluz.mp4        # Vídeo oficial em alta definição no Hero (1.2 MB otimizado)
    ├── hero-poster.jpg        # Poster instantâneo de carregamento (39 KB)
    ├── logo-light.png         # Logotipo oficial em versão de alto contraste (66 KB)
    ├── slide-1.jpg            # Fotografia operacional: Avenida iluminada
    ├── slide-2.jpg            # Fotografia operacional: Frota e caminhão
    ├── slide-3.jpg            # Fotografia operacional: Corredor viário arborizado
    └── slide-4.jpg            # Fotografia operacional: Equipe técnica noturna
```

---

## 🛠️ Como Executar Localmente

O projeto não requer `npm`, `node_modules` ou compilação.

### Opção 1: Python 3
```bash
python -m http.server 8080
```
Acesse: [http://localhost:8080](http://localhost:8080)

### Opção 2: Node.js (se disponível)
```bash
npx serve .
```

### Opção 3: Direto no Navegador
Você pode abrir o arquivo `index.html` diretamente em qualquer navegador moderno com dois cliques.

---

## 🚀 Publicação em Produção (Cloudflare Pages)

### 1. Conexão com o Repositório Git (Recomendado)
1. Acesse o **Cloudflare Dashboard** ([dash.cloudflare.com](https://dash.cloudflare.com/)).
2. No menu lateral, navegue até **Workers & Pages** > **Create application** > **Pages** > **Connect to Git**.
3. Selecione o repositório `crp-luz` (ex: `TheyDreez/crp-luz`).
4. Configure o build:
   - **Framework preset:** `None`
   - **Build command:** *(deixar vazio)*
   - **Build output directory:** `.` (ou `/`)
   - **Production branch:** `main`
5. Clique em **Save and Deploy**. Em menos de 30 segundos o projeto estará publicado com uma URL `.pages.dev`.

### 2. Configuração de Domínio Personalizado
1. No projeto Pages, acesse a aba **Custom domains**.
2. Clique em **Set up a custom domain**.
3. Insira o domínio oficial (ex: `crpluz.com.br` e `www.crpluz.com.br`).
4. O Cloudflare cuidará automaticamente do apontamento de DNS e da emissão dos certificados SSL/TLS universais.

---

## 🛡️ Configurações Recomendadas no Cloudflare Dashboard

Para garantir a máxima segurança, velocidade e absorção de picos:

### 1. DNS
- Certifique-se de que os registros `A` / `CNAME` do domínio estejam com o **Proxy Status: Proxied (Nuvem Laranja ativada)**. Isso garante que todo o tráfego passe pelos escudos da Cloudflare.

### 2. SSL/TLS
- **Encryption Mode:** `Full (Strict)`
- **Always Use HTTPS:** `Enabled` (Ativo)
- **Minimum TLS Version:** `TLS 1.2`
- **Automatic HTTPS Rewrites:** `Enabled` (Ativo)
- **HTTP/3 (with QUIC):** `Enabled` (Ativo)

### 3. WAF & Segurança
- **Security Level:** `Medium` (ou `High` durante grandes campanhas na TV/rádio).
- **Bot Fight Mode:** `Enabled` (Bloqueia requisições abusivas de bots conhecidos e scrapers maliciosos).
- **Rate Limiting:** Como o site é totalmente estático e servido pelo cache de borda, **não é necessário** configurar rate limiting agressivo pago. Se desejar, crie uma regra gratuita no WAF limitando acessos por IP que excedam 300 requisições/minuto.

### 4. Otimização de Performance
- **Early Hints:** `Enabled`
- **Brotli:** `Enabled`
- **Auto Minify:** Desativado (o código já está otimizado e minificado no HTML original).

---

## 🔒 Headers de Segurança e Cache (`_headers`)

O arquivo `_headers` na raiz do projeto é interpretado nativamente pelo Cloudflare Pages:

| Header | Valor | Finalidade |
| :--- | :--- | :--- |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains; preload` | Força conexões HTTPS por 1 ano |
| `X-Frame-Options` | `DENY` | Impede ataques de Clickjacking / embedding em iframes |
| `X-Content-Type-Options` | `nosniff` | Previne MIME-sniffing malicioso |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Protege metadados de navegação |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=(), browsing-topics=()` | Desativa recursos desnecessários do navegador |
| `Content-Security-Policy` | `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; img-src 'self' data:; media-src 'self';` | Bloqueia injeção de scripts e recursos de terceiros não autorizados |
| `Cache-Control` (HTML) | `public, max-age=0, must-revalidate` | Permite atualizações imediatas no browser dos usuários |
| `Cache-Control` (Assets) | `public, max-age=86400, stale-while-revalidate=604800` | Armazena assets estáticos em cache na borda (CDN) e no navegador |

### Como Validar os Headers via Terminal
```bash
curl -I https://www.crpluz.com.br
```
Verifique a presença dos headers `Strict-Transport-Security`, `Content-Security-Policy` e `cf-cache-status: HIT`.

---

## 🔄 Fluxo de Atualização Contínua

Qualquer alteração feita na branch `main` dispara um deploy atômico e instantâneo no Cloudflare Pages:

```bash
# 1. Faça as modificações necessárias
git add .
git commit -m "docs: atualização das diretrizes de iluminação"

# 2. Envie para o GitHub
git push origin main
```
Em cerca de 15 a 30 segundos, a nova versão estará no ar globalmente sem tempo de inatividade (*zero-downtime*).

---

## ⏪ Rollback Imediato (Zero-Downtime)

Se alguma publicação introduzir um problema:
1. No painel do **Cloudflare Pages**, clique no projeto `crp-luz`.
2. Acesse a aba **Deployments**.
3. Localize o deploy estável anterior na lista.
4. Clique nos três pontinhos `...` ao lado do deploy e selecione **Rollback to this deployment**.
5. O tráfego global é redirecionado instantaneamente (em menos de 2 segundos) para a versão estável anterior.

---

## ✅ Checklist Pré e Pós-Deploy

### Pré-Deploy:
- [x] Ausência de bibliotecas pesadas ou dependências dinâmicas no client.
- [x] Otimização de assets: vídeo do hero e imagens pesam menos de 4 MB somados.
- [x] Arquivo `_headers` presente com regras de segurança e cache.
- [x] Arquivo `robots.txt` presente permitindo indexação correta.
- [x] Teste de carregamento e reprodução de vídeo em desktop e mobile.
- [x] Validação de links de atendimento telefônico (`tel:08009214477`).

### Pós-Deploy:
- [ ] Validar status do SSL/TLS no navegador (cadeado verde/seguro).
- [ ] Conferir headers de resposta HTTP via `curl -I` ou DevTools.
- [ ] Verificar reprodução suave do vídeo hero e presença do poster de fallback.
- [ ] Testar navegação em dispositivo móvel real sob conexão 4G.
- [ ] Confirmar ativação do **Bot Fight Mode** e **Full (Strict)** no painel Cloudflare.
