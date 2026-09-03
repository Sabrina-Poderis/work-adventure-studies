# 🗺️ Kit Inicial do Mapa WorkAdventure

<a href="https://discord.gg/G6Xh9ZM9aR" target="blank"><img src="https://img.shields.io/discord/821338762134290432.svg?style=flat&label=Join%20Community&color=7289DA" alt="Join Community Badge"/></a>
<a href="https://x.com/workadventure_" target="blank"><img src="https://img.shields.io/twitter/follow/workadventure_.svg?style=social" /></a>
![visitors](https://vbr.nathanchung.dev/badge?page_id=workadventure.map-starter-kit&color=00cf00)

![office map thumbnail](./office.png)

🗺️ Este é um kit inicial para ajudar você a criar seu próprio mapa para o [WorkAdventure](https://workadventu.re).

📚 Para entender como usar este kit inicial, siga o[ tutorial](https://docs.workadventu.re/map-building/tiled-editor/).

👨🏻‍🔧 Se você tiver alguma dúvida, sinta-se à vontade para perguntar no [escritório do WorkAdventure](https://play.staging.workadventu.re/@/tcm/workadventure/wa-village).

## 🚀 Faça o Upload do seu mapa

No arquivo `.env`, você pode definir sua estratégia de upload como `MAP_STORAGE` (padrão) ou `GH_PAGES`. Basta comentar a opção que você não deseja usar.

Fazer o upload de um mapa usando o [armazenamento de mapas do WA](https://docs.workadventu.re/map-building/tiled-editor/publish/wa-hosted) hospeda seu projeto nos servidores do WA.

Fazer o upload de um mapa usando as [GitHub Pages](https://docs.github.com/pages) hospeda seu projeto nos servidores do GitHub. A configuração é um pouco mais complexa e só pode ser usada com repositórios públicos (ou com privados se você tiver uma assinatura ativa do GitHub).

## 🗂️ Estrutura do projeto

```
map-starter-kit/
├── 📁 app/                    # Ponto de entrada do servidor (carrega @workadventure/map-starter-kit-core)
│   └── app.ts                 # Reexporta a aplicação Express do core package
│
├── 📁 src/                    # Scripts do mapa (Navegador/WorkAdventure) ⚠️ OBRIGATÓRIO
│   └── main.ts                # Seus scripts do mapa ficam aqui
│
│
├── 📁 tilesets/               # Imagens de tilesets do mapa (PNG)
│
├── 📄 *.tmj                   # Arquivos do mapa (office.tmj, conference.tmj, etc.)
├── 📄 vite.config.ts          # Configuração do Vite
└── 📄 package.json            # Dependências e scripts
```

O **servidor** (aplicação Express, controllers, páginas HTML de publicação, assets estáticos) é fornecido pelo pacote npm **`@workadventure/map-starter-kit-core`**. Atualizar essa dependência lhe dá uma nova interface de publicação e recursos do servidor sem alterar seus mapas ou configuração.

### Referência rápida

- *`src/`*: **Scripts do mapa** (DEVEM estar aqui para compilação) ⚠️
- *`tilesets/`*: Todos os tilesets em PNG
- *`app/`*: **Ponto de entrada do servidor** – carrega o core package; não adicione lógica do servidor aqui

> [!TIP]
> - Se você quiser usar mais de um arquivo de mapa, basta adicionar o novo arquivo na raiz do projeto (recomendamos criar uma cópia de *office.tmj* e editar para evitar erros).
> - Recomendamos usar imagens de **512x512** para as miniaturas dos mapas.
> - Se você for criar sites personalizados para incorporar no mapa, por favor, referencie os arquivos HTML na opção `input` em *buildmap.vite.config.js*.

### 📁 Ponto de entrada do servidor (`app/`)

O diretório `app/` contém apenas o **entry point** que carrega o servidor a partir de **`@workadventure/map-starter-kit-core`**.

- *`app.ts`*: Importa e reexporta a aplicação Express do core package (para o plugin do servidor Vite).

O servidor real (Express, rotas, páginas HTML, upload, armazenamento de mapas) fica na dependência. Para receber atualizações da interface de publicação e do comportamento do servidor, execute `npm update @workadventure/map-starter-kit-core`.

> [!IMPORTANT]
> Não adicione lógica de servidor nem novos controllers em `app/`. O servidor é totalmente fornecido pelo core package.

### 📁 Desenvolvimento de scripts do mapa (`src/`) ⚠️

O diretório `src/` é onde você **DEVE** colocar **todos os scripts relacionados ao mapa** que serão executados no navegador. Consulte [src/README.md](./src/README.md) para documentação detalhada e exemplos.

- *`main.ts`*: Script principal do mapa (referenciado nos arquivos `.tmj`)

> [!IMPORTANT]
> **Todos os scripts do mapa DEVEM ficar no diretório `src/`** para serem compilados e agrupados corretamente pelo Vite. Os scripts neste diretório são:
> - Transformados automaticamente de TypeScript para JavaScript
> - Empacotados com suas dependências npm (como `@workadventure/scripting-api-extra`)
> - Servidos com os tipos MIME corretos
> - Referenciados em seus arquivos de mapa `.tmj` usando caminhos como `src/main.ts`

> [!WARNING]
> Não coloque scripts do mapa fora do diretório `src/`. Eles não serão compilados corretamente e causarão erros no navegador.

## 📜 Requisitos

- Node.js versão >= 18

## 🛠️ Instalação e testes

### Pré-requisitos

- **Node.js** versão >= 18 ([Baixar Node.js](https://nodejs.org/en/))
- **npm** (vem com o Node.js)

### 📦 Instalação

1. Clone ou baixe este repositório
2. Navegue até a raiz do projeto
3. Instale as dependências:

```bash
npm install
```

Isso instalará todas as dependências necessárias, incluindo Vite, TypeScript, pacotes do WorkAdventure e **`@workadventure/map-starter-kit-core`** (servidor e interface de publicação).

### 🚀 Desenvolvimento

#### Iniciar servidor de desenvolvimento

Inicie o servidor de desenvolvimento do Vite com atualização em tempo real:

```bash
npm run dev
```

Isso irá:
- Iniciar o servidor Vite (normalmente em `http://localhost:5173`)
- Habilitar recarga instantânea para atualizações rápidas
- Transformar automaticamente arquivos TypeScript em `src/`
- Servir seus mapas e ativos

> [!TIP]
> O servidor de desenvolvimento abrirá seu navegador automaticamente. Se não abrir, navegue até a URL mostrada no terminal.

#### Testar build de produção

Para testar como seu mapa se comportará em produção:

```bash
# Compila a versão otimizada do mapa para produção
npm run buildmap

# Serve o diretório dist/ com cabeçalhos CORS
npm run preview

```

Isso irá:
- Compilar TypeScript para JavaScript
- Otimizar e empacotar todos os ativos
- Criar uma pasta `dist/` pronta para produção
- Iniciar um servidor de preview para testar o build otimizado

### 📤 Upload do mapa

#### Upload para o WA Map Storage

Para enviar seu mapa para o armazenamento de mapas do WorkAdventure:

```bash
npm run upload
```

Este comando irá:
1. Compilar seu mapa (`npm run buildmap`)
2. Enviar para o WA Map Storage configurado

> [!IMPORTANT]
> Antes do upload, você precisa configurar suas opções de envio. O recurso de upload requer três variáveis de ambiente:

1. **`MAP_STORAGE_URL`** - URL do seu WorkAdventure Map Storage
   - *Desenvolvimento local*: Criada em `.env` pelo comando de upload
   - *CI/CD*: Adicione como segredo do GitHub (opcional)

2. **`MAP_STORAGE_API_KEY`** - Sua chave de API para autenticação
   - *Desenvolvimento local*: Criada em `.env.secret` pelo comando de upload
   - *CI/CD*: Adicione como segredo do GitHub (obrigatório)

3. **`UPLOAD_DIRECTORY`** - Caminho do diretório no servidor de armazenamento
   - *Desenvolvimento local*: Criado em `.env` pelo comando de upload
   - *CI/CD*: Adicione como segredo do GitHub (opcional)

#### Configurar opções de upload

Você pode configurar essas opções através da interface web:
1. Inicie o servidor de desenvolvimento (`npm run dev`)
2. Navegue até a página de configuração de upload
3. Preencha suas credenciais do Map Storage
4. Salve e envie seu mapa

Para mais detalhes, leia a [documentação de upload do WorkAdventure](https://docs.workadventu.re/map-building/tiled-editor/publish/wa-hosted).

### 📋 Scripts disponíveis

| Comando | Descrição |
|---------|-------------|
| `npm run dev` | Inicia o servidor Vite com recarga automática |
| `npm run buildmap` | Compila apenas os arquivos do mapa (sem frontend) |
| `npm run preview` | Serve o diretório `dist/` gerado com cabeçalhos CORS após `npm run buildmap` |
| `npm run upload` | Compila e envia o mapa para o WA Map Storage |
| `npm run upload-only` | Envia o mapa sem recompilar (requer build existente) |

## 📜 Licenças

Este projeto contém várias licenças, conforme abaixo:

* [Licença do código](./LICENSE.code) *(todos os arquivos, exceto os de outras licenças)*
* [Licença do mapa](./LICENSE.map) *(`office.tmj` e a aparência visual do mapa também)*
* [Licença dos ativos](./LICENSE.assets) *(os arquivos dentro da pasta `tilesets/`)*

> [!IMPORTANT]
> Se você adicionar ativos de terceiros ao seu mapa, não se esqueça de:
> 1. Creditar o autor e a licença de um tileset com a propriedade "tilesetCopyright" editando o tileset no Tiled.
> 2. Adicionar o texto da licença do tileset em *LICENSE.assets*.
> 3. Creditar o autor e a licença de um mapa com a propriedade "mapCopyright" nas propriedades personalizadas do mapa.
> 4. Adicionar o texto da licença do mapa em *LICENSE.map*.

## ❓ Precisa de ajuda?

Se você tiver alguma dúvida ou precisar de mais assistência, não hesite em perguntar por [email](mailto:hello@workadventu.re) ou no [Discord](https://discord.gg/G6Xh9ZM9aR)!
