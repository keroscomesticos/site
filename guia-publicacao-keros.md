# Guia de Publicação — Site KEROS Indústria de Cosméticos

Este documento explica, passo a passo, como colocar o site da KEROS no ar. Foram preparados **dois pacotes de publicação** que atendem a dois cenários diferentes. Escolha o que melhor se encaixa na sua estrutura:

| Pacote | Arquivo | Quando usar | Custo aproximado |
|---|---|---|---|
| **GitHub Pages (estático)** | `keros-site-github-pages.zip` | Publicação rápida e gratuita, conteúdo institucional (marcas, produtos, páginas, galeria). Não exige servidor nem banco de dados. | **Grátis** |
| **Universal Node.js** | `keros-site-universal-node.zip` | Publicação completa com painel administrativo (CMS), formulários com registro de leads, banco de talentos, upload de arquivos e gerenciamento total do conteúdo pela web. Exige hospedagem Node.js e banco MySQL. | Varia (há opções baratas, a partir de ~US$ 5/mês) |

O site funciona nas duas versões com as mesmas 18 páginas institucionais, as 12 fotos reais da fábrica, as logos oficiais da KEROS e das marcas (Vizzage Professional, Sallon Linda e Pró-Thess) e o mesmo design azul/dourado/branco. Todas as páginas exibem um **botão flutuante de WhatsApp** configurado para o número **(31) 99795-4926** (5531997954926), e os canais de contato — SAC, Fale Conosco, Comercial, Representantes, Fornecedores e Banco de Talentos — apontam para os e-mails **keros@keros.com.br** e **keros.comestivos@gmail.com** (os links de e-mail já abrem o cliente de e-mail com os dois destinatários preenchidos). O catálogo de produtos traz **423 produtos das três linhas** importados da planilha unificada — sem valores unitários e totais, conforme solicitado, mantendo nome, descrição, instruções de uso, especificações, categoria e gramatura/ml. As páginas de marcas contam com os textos institucionais de cada linha. A diferença está apenas em **como o conteúdo é atualizado**: na versão estática, editando arquivos; na versão com servidor, pelo painel administrativo.

---

## Parte 1 — Publicar no GitHub Pages (gratuito, recomendado para começar)

### O que é esta versão

É a versão 100% estática do site. Todo o conteúdo público — marcas com textos institucionais, linhas, **423 produtos das três marcas** (sem preços), configurações, FAQ, galeria e SEO — está embutido no arquivo `content-gh.json` e as imagens (logos e fotos da fábrica) na pasta `images/`. Não há servidor, banco de dados ou login — apenas arquivos que o GitHub Pages entrega gratuitamente.

### Passo 1 — Criar uma conta e um repositório no GitHub

1. Acesse [github.com](https://github.com) e crie uma conta (se ainda não tiver).
2. No canto superior direito, clique no botão **+** → **New repository** (Novo repositório).
3. No campo **Repository name**, digite um nome — o nome escolhido define o endereço do site. Exemplo: se digitar `keros-site`, o endereço será `https://SEU-USUARIO.github.io/keros-site/`.
4. Marque **Public** (repositórios públicos são requisito do GitHub Pages gratuito) e clique em **Create repository**.

### Passo 2 — Enviar os arquivos do site

1. Faça o download do arquivo `keros-site-github-pages.zip`.
2. Descompacte o ZIP. Dentro dele há uma pasta `dist-github/`.
3. **Importante:** o conteúdo que deve ir para o GitHub é o que está **dentro** de `dist-github/` — os arquivos `index.html`, `404.html`, a pasta `assets/` e a pasta `images/` devem ficar **diretamente na raiz do repositório** (não a pasta `dist-github` inteira).
4. De volta ao repositório no GitHub, clique em **uploading an existing file** (acima da lista de arquivos, ou em **Add file** → **Upload files**).
5. Arraste **todo o conteúdo de `dist-github/`** para a área de upload e clique em **Commit changes**.

### Passo 3 — Ativar o GitHub Pages

1. No repositório, vá em **Settings** (Configurações) → **Pages** (menu lateral esquerdo).
2. Em **Source**, escolha o branch **main** (ou `master`) e a pasta **/(root)**. Clique em **Save**.
3. Aguarde de 1 a 3 minutos. O GitHub publica o site automaticamente e mostra o endereço no topo da página.
4. Abra o endereço exibido. **Este pacote detecta automaticamente o sub-pasta do repositório** — se o repositório se chama `keros-site`, o site funcionará corretamente em `https://SEU-USUARIO.github.io/keros-site/` sem nenhuma configuração adicional. O arquivo `404.html` incluído cuida dos endereços diretos das páginas internas.

### Passo 4 — (Opcional) Conectar um domínio próprio

1. Ainda em **Settings → Pages**, na seção **Custom domain**, digite o domínio desejado (ex.: `www.keros.com.br`) e clique em **Save**.
2. No registro do domínio (Registro.br, GoDaddy etc.), configure os apontamentos solicitados pelo GitHub (geralmente registros `CNAME`/`A`). As instruções exatas aparecem na mesma página.

### Como atualizar o conteúdo nesta versão

| O que atualizar | Como fazer |
|---|---|
| Textos, marcas, produtos, telefones, e-mail, WhatsApp, FAQ, SEO | Abra `content-gh.json` em qualquer editor de texto, edite os valores e reenvie ao GitHub (commit + push). Estrutura comentada e simples. |
| Imagens | Substitua ou adicione arquivos em `images/` com o mesmo padrão de nome (`XX_nome.jpg`) e referencie o mesmo caminho no `content-gh.json`. |
| Aplicar a mudança | Após o commit, o site atualiza sozinho em cerca de 1 a 3 minutos. |

**Limitações da versão estática:** o painel administrativo (`/admin`) não está disponível e os formulários (contato, SAC, representantes, fornecedores, banco de talentos) não gravam dados — por isso, em sites estáticos, recomenda-se direcionar o visitante ao WhatsApp/e-mail para contato. Os dados dos formulários e o gerenciamento de conteúdo pela web só estão disponíveis na versão com servidor (Parte 2).

---

## Parte 2 — Publicar em hospedagem Node.js (versão completa com CMS)

### O que é esta versão

É o código-fonte completo do site, com servidor Node.js e banco de dados MySQL. Inclui o painel administrativo em `/admin`, onde é possível gerenciar marcas, produtos, linhas, vagas, galeria, configurações do site (telefone, WhatsApp, endereços), SEO por página e acompanhar os leads recebidos pelos formulários. Funciona em **qualquer hospedagem com Node.js 20+** — Railway, Render, Hostinger (plano com Node.js), HostGator/DreamHost/Hostinger com acesso SSH, VPS com Docker etc.

### Passo 1 — Preparar a hospedagem e o banco de dados

1. Contrate uma hospedagem que suporte **Node.js 20 ou superior** (ou um VPS onde você possa instalá-lo).
2. Crie um banco de dados **MySQL 8+** na hospedagem e anote a string de conexão, que tem o formato:
   ```
   mysql://usuario:senha@servidor:3306/nome_do_banco
   ```

### Passo 2 — Enviar os arquivos do site

1. Faça o download do arquivo `keros-site-universal-node.zip`.
2. Descompacte na sua hospedagem (via painel de arquivos, `git clone`, SCP ou FTP, conforme o suporte da hospedagem).
3. A estrutura contém as pastas `client/`, `server/`, `drizzle/`, `scripts/` e o arquivo `package.json` na raiz.

### Passo 3 — Instalar as dependências e criar o banco

No diretório raiz do projeto (onde está o `package.json`), execute na hospedagem:

```
pnpm install        # ou: npm install
```

Em seguida, crie as tabelas do banco de dados a partir do script SQL incluído no pacote:

```
mysql -u usuario -p nome_do_banco < scripts/local_schema.sql
```

(Se a hospedagem não oferecer acesso ao `mysql` no terminal, importe o arquivo `scripts/local_schema.sql` pelo phpMyAdmin ou painel equivalente.)

### Passo 4 — Configurar as variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto com o seguinte conteúdo (ajuste os valores):

```
NODE_ENV=production
PORT=3000
DATABASE_URL=mysql://usuario:senha@servidor:3306/nome_do_banco
JWT_SECRET=<gere_uma_chave_aleatoria_de_32_ou_mais_caracteres>
```

Para gerar a chave do `JWT_SECRET`, use no terminal:

```
openssl rand -hex 32
```

Observações importantes sobre o banco:

- O site precisa que `DATABASE_URL` aponte para um MySQL acessível. Sem ele, o painel administrativo e os formulários com registro não funcionarão.
- As variáveis `OAUTH_SERVER_URL` e `OWNER_OPEN_ID` são usadas apenas pelo login administrativo da plataforma Manus. Na hospedagem externa, o primeiro acesso administrativo é feito com a conta e senha do usuário do banco na tabela `users` (criada pelo script SQL); se preferir, defina `OAUTH_SERVER_URL` apontando para o portal de autenticação existente, ou mantenha o padrão de login local do template.

### Passo 5 — Compilar e iniciar o site

```
pnpm run build
pnpm start
```

O site ficará disponível em `http://localhost:3000` (a variável `PORT` pode ser ajustada conforme a hospedagem). Configure na hospedagem um processo permanente (Railway/Render fazem isso automaticamente; em VPS, use PM2: `pnpm add -g pm2 && pm2 start dist/index.js`).

### Passo 6 — Acessar o painel administrativo

Abra `https://SEU-DOMINIO/admin`. O primeiro acesso define o administrador (conta criada pelo script SQL). A partir do painel é possível gerenciar todo o conteúdo do site sem tocar em arquivos.

### Passo 7 — Conectar o domínio

Aponte o domínio desejado (ex.: `www.keros.com.br`) para o IP/serviço da hospedagem conforme a documentação dela (Registro.br, para domínios .br, usa apontamento no painel do registrador).

---

## Parte 3 — Recomendação prática

| Situação | Recomendação |
|---|---|
| Quer o site no ar hoje, sem custo e sem infraestrutura | GitHub Pages (Parte 1) |
| Precisa receber e organizar leads, vagas e conteúdo via painel web | Hospedagem Node.js com banco MySQL (Parte 2) |
| Quer as duas coisas | Publique o institucional no GitHub Pages e use o WhatsApp/e-mail como canal principal; migre para a versão completa quando o CMS for necessário |

Em qualquer cenário, as imagens da fábrica já estão embutidas nos pacotes e os textos podem ser revisados antes da publicação. As logos oficiais (KEROS, Vizzage Professional, Sallon Linda e Pró-Thess) já estão integradas ao site — no cabeçalho, rodapé, favicon e nas páginas de marcas — e fazem parte dos dois pacotes. No catálogo, como as fotos individuais dos produtos ainda não foram fornecidas, o site exibe uma imagem genérica da linha nos cards; quando as fotos estiverem prontas, basta adicionar os arquivos à pasta `images/` e referenciá-los no `content-gh.json` (campo `imageUrl` de cada produto) ou pelo painel administrativo na versão com CMS.

---

*Documento preparado para KEROS Indústria de Cosméticos Ltda. — Ipatinga/MG. Versão 1.0 — agosto de 2026.*
