# Aquapulse — Landing Page e Login

O Aquapulse é uma aplicação voltada ao monitoramento da qualidade da água de
represas.

Nesta primeira versão do repositório estão disponíveis:

- **Landing page** (`index.php`) — página pública institucional;
- **Login** (`login.php`) — tela de acesso com autenticação PHP funcional.

O restante do sistema (painel interno, monitoramento, gráficos, mapas,
relatórios e banco de dados) não faz parte deste repositório.

## Stack

PHP puro (sem framework), HTML semântico, CSS e JavaScript sem bibliotecas.
Não há `package.json`, bundler, etapa de build nem banco de dados.

Requisito: **PHP 8.0 ou superior**.

## Como executar

Com o PHP disponível no PATH, a partir da raiz do projeto:

```bash
php -S localhost:8000
```

Depois acesse <http://localhost:8000>.

Se o PHP não estiver no PATH (instalação via XAMPP no Windows, por exemplo):

```powershell
& C:\xampp\php\php.exe -S localhost:8000 -t .
```

Também funciona ao colocar a pasta em `htdocs/` do Apache/XAMPP.

## Acesso de demonstração

O login é validado no servidor contra um usuário simulado em
`backend/storage/mock/users.php` (somente o hash da senha fica no arquivo).

```
e-mail: demo@aquapulse.local
senha:  Aquapulse@123
```

Credencial exclusivamente local, para desenvolvimento. Não usar em produção.

Depois do login, a própria tela exibe o cartão "Acesso validado" com os dados
da sessão e o botão para encerrá-la.

## Rotas

| Endereço | Arquivo | O que é |
| --- | --- | --- |
| `/` | `index.php` | landing page |
| `/login.php` | `login.php` | tela de login |
| `/api/v1/auth/login.php` | POST | cria a sessão |
| `/api/v1/auth/logout.php` | POST | encerra a sessão |
| `/api/v1/auth/me.php` | GET | devolve o usuário da sessão |

## Estrutura

```
index.php                       landing page
login.php                       tela de login

includes/
  config.php                    textos, navegação e helpers de escape
  icons.php                     ícones SVG usados pelas duas telas
  header.php                    cabeçalho + navegação (com menu mobile)
  footer.php                    rodapé
  sections/
    hero.php                    seção 1 — hero
    informacoes.php             seção 2 — por que monitorar represas
    sistema.php                 seção 3 — como o Aquapulse apoia a operação
    vantagens.php               seção 4 — vantagens + chamada final

api/v1/auth/
  login.php  logout.php  me.php endpoints de autenticação (JSON)

backend/
  bootstrap.php                 autoload, tratamento de erros e sessão
  config/session.php            parâmetros do cookie e da expiração
  src/Auth/AuthService.php      regras de autenticação
  src/Http/                     leitura da requisição e resposta JSON
  src/Repositories/             contrato e implementação simulada de usuários
  storage/mock/users.php        usuário de demonstração (fora da pasta pública)

assets/
  css/                          style.css (landing + login) e estilos por seção
  js/                           scripts da landing e do login
  images/                       imagens e logos usados pelas duas telas
```

## Segurança

- A senha é validada no servidor; o navegador recebe apenas um cookie de sessão
  **HttpOnly**, que o JavaScript não consegue ler.
- A API responde **sempre em JSON**, inclusive nos erros (401, 422, 500).
- A pasta `backend/` tem um `.htaccess` que bloqueia acesso direto pela web.
- Não há credenciais reais, tokens ou chaves neste repositório.
