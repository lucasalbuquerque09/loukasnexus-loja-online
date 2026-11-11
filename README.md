# LN Loja Online

Loja virtual responsiva feita com HTML, CSS e JavaScript, com Firebase Authentication e Cloud Firestore. Inclui vitrine, checkout, acompanhamento de pedidos, painel administrativo e suporte a instalação como PWA. Não há etapa de build.

## Funcionalidades

- Catálogo com busca, categorias, destaques, variações, cupons e estoque.
- Carrinho e checkout com entrega ou retirada e opções de pagamento configuráveis.
- Criação de pedidos e consulta de andamento pelo código do pedido.
- Painel protegido por login, com gestão de pedidos, produtos, clientes, estoque, PDV, caixa, contas e relatórios.
- Personalização da identidade visual e das informações da loja.
- PWA para instalar a loja e o painel em dispositivos compatíveis.

> O projeto não processa pagamentos online. A forma de pagamento é registrada no pedido para ser combinada com o cliente.

## Tecnologias

- HTML, CSS e JavaScript sem framework.
- Firebase Authentication para o acesso administrativo.
- Cloud Firestore para produtos, configurações, pedidos e dados internos.
- Firebase JavaScript SDK 10.12.2 carregado pela CDN.
- Service worker e Web App Manifest para recursos PWA.

## Estrutura

| Caminho | Descrição |
| --- | --- |
| `index.html` | Vitrine e checkout |
| `acompanhar.html` | Consulta do pedido pelo código |
| `admin.html` | Painel administrativo |
| `firebase-config.js` | Configuração do app Web Firebase |
| `regras.md` | Regras a publicar no Cloud Firestore |
| `js/db.js` | Acesso compartilhado ao Firebase e configurações padrão |
| `js/store.js` | Funcionalidades da loja |
| `js/acompanhar.js` | Consulta de pedidos |
| `js/admin*.js` | Módulos do painel administrativo |
| `css/` | Estilos da loja, do painel e da tela de abertura |
| `manifest*.webmanifest`, `sw.js`, `icons/` | Instalação e cache PWA |

## Configuração do Firebase

Cada instalação precisa de um projeto Firebase próprio ou de um projeto ao qual você tenha acesso.

1. No [Firebase Console](https://console.firebase.google.com/), crie um projeto e um app Web.
2. Crie o banco em **Firestore Database**.
3. Em **Authentication → Sign-in method**, habilite **E-mail/senha**.
4. Copie os valores `firebaseConfig` do app Web para `firebase-config.js`, substituindo os valores existentes.
5. Crie o usuário administrativo em **Authentication → Users**.
6. Abra `regras.md` e altere a lista retornada por `emailsAdmin()` para incluir o e-mail do administrador. Para mais administradores, inclua os e-mails nessa lista.
7. No **Firestore Database → Regras**, publique o conteúdo completo de `regras.md`.

As regras são parte essencial da configuração: não substitua por regras abertas de teste. Os dados da configuração do app Web, incluindo `apiKey`, identificam o projeto e não substituem as regras de segurança do Firestore.

## Executar localmente

É necessário abrir o projeto por HTTP, não diretamente via `file://`.

No Windows, abra o terminal na pasta do projeto e execute:

```powershell
py -m http.server 8000
```

Se `py` não estiver disponível, tente `python -m http.server 8000`. Em seguida, acesse:

- Loja: <http://localhost:8000/>
- Painel: <http://localhost:8000/admin.html>
- Acompanhar pedido: <http://localhost:8000/acompanhar.html>

Não é necessário instalar dependências nem executar um processo de build.

## Publicar

O projeto pode ser hospedado em um serviço de arquivos estáticos. Para usar Firebase Hosting:

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy --only hosting
```

Durante a configuração, selecione o projeto Firebase correto, use a pasta do projeto como diretório público e não sobrescreva os arquivos existentes. Configure o serviço para servir `index.html` como página inicial. Em **Authentication → Settings → Authorized domains**, adicione o domínio usado pela loja; sem isso, o login administrativo pode falhar.

## Uso

- Cadastre categorias, produtos, estoque, fretes e cupons no painel.
- Ajuste nome da loja, contatos, aparência, entrega/retirada e meios de pagamento em **Configurações**.
- Clientes fazem pedidos pela vitrine e consultam o andamento em **Acompanhar pedido** usando o código recebido.
- O acesso administrativo exige uma conta no Firebase Authentication e um e-mail incluído em `emailsAdmin()` nas regras publicadas.

## Segurança e limitações

- As permissões são aplicadas pelas regras do Firestore em `regras.md`; mantenha-as publicadas e atualizadas.
- Leituras públicas são necessárias para exibir catálogo e configurações. A consulta de acompanhamento usa o código do pedido; não compartilhe códigos publicamente.
- Preços e totais são calculados no navegador. Confira os pedidos antes de confirmá-los.
- O projeto não integra gateway de pagamento, emissão fiscal ou backend próprio.
- Em domínios diferentes de `localhost`, use HTTPS para login e recursos PWA.
