# 🛡️ Client-Side Receipt & Barcode Validator | Validador Local de Comprovantes

<p align="center">
  <a href="#-english">🇺🇸 English</a> •
  <a href="#-português">🇧🇷 Português</a>
</p>

---

<div id="-english"></div>

## 🇺🇸 English

A zero-backend, privacy-first web application built with vanilla **HTML5, CSS3, and JavaScript** that validates payment receipts (images and PDFs) directly in the browser.

Powered by **Tesseract.js (WebAssembly)** and **PDF.js**, it extracts transaction codes locally, applies heuristic OCR autocorrection, and cross-checks them against expected system values using a 3-tier similarity engine—ensuring sensitive financial data never leaves the user's device.

### 🚀 Key Features

- **100% Offline & Privacy-First:** Zero server uploads. All image rendering and OCR processing run locally in the browser via WebAssembly.
- **Multi-Format Support:** Handles `.jpg`, `.jpeg`, `.png`, and `.pdf` files (automatically rendering the first page of PDFs).
- **Quick Paste & Drag-and-Drop:** Supports direct clipboard image pasting (`Ctrl+V`) as well as drag-and-drop file uploads.
- **Smart OCR Autocorrection:** Automatically fixes common OCR character misreads (e.g., `O` → `0`, `I/l` → `1`, `S` → `5`, `Z` → `2`) and isolates the best matching 44-digit barcode block.
- **3-Tier Validation Engine:**
  - **Exact Match (Green | 98% – 100%):** Receipt barcode matches the system record.
  - **Warning Alert (Amber | 83% – 97%):** High similarity detected, but middle digits differ (e.g., same payer/issuer, different billing month or amount). Requires manual review.
  - **Mismatch (Red | < 83%):** Complete divergence; invalid or unrelated receipt.
- **Dynamic Light / Dark Theme:** Built-in theme switcher with persistent styling variables.
- **Instant Reset:** One-click clear button resets inputs, canvas previews, and validation states.

### 🛠️ Tech Stack

- **HTML5 / CSS3 / Vanilla JavaScript (ES6+):** Single-file, dependency-free architecture using CSS custom properties.
- **Tesseract.js (v5):** Client-side OCR engine running via WebAssembly.
- **PDF.js (Mozilla):** Client-side PDF rendering and canvas conversion.
- **Font Awesome (v6):** Vector UI icons.

### 📦 How to Use

No web server, build step, or backend environment (Node.js, Apache, Nginx) is required:

1. Clone this repository or download `index.html` (`prototipo.html`).
2. Open the file directly in any modern browser (Chrome, Edge, Firefox, Brave).
3. Enter the expected barcode or transaction number in the system field.
4. Upload, drag-and-drop, or paste (`Ctrl+V`) the payment receipt.
5. Click **"Ler & Comparar Comprovante"** (*Read & Compare Receipt*).

---

<div id="-português"></div>

## 🇧🇷 Português

Aplicação web *zero-backend* focada em privacidade e agilidade operacional, desenvolvida em **HTML5, CSS3 e JavaScript puro** para validar comprovantes de pagamento (imagens e PDFs) diretamente no navegador.

Utilizando **Tesseract.js (WebAssembly)** e **PDF.js**, o sistema extrai códigos de barras e linhas digitáveis localmente, aplica autocorreção heurística de caracteres e realiza o cruzamento de dados em 3 níveis de similaridade — garantindo que nenhum dado financeiro sensível seja enviado para servidores externos.

### 🚀 Principais Funcionalidades

- **100% Offline e Privado:** Nenhum dado ou arquivo sai da máquina do usuário. Todo o processamento ocorre localmente no navegador via WebAssembly.
- **Múltiplos Formatos:** Suporte a arquivos de imagem `.jpg`, `.jpeg`, `.png` e documentos `.pdf` (renderizando a primeira página automaticamente).
- **Colagem Rápida (Ctrl+V) e Drag-and-Drop:** Permite colar prints da área de transferência diretamente na tela ou arrastar arquivos para a zona de upload.
- **Autocorreção OCR Inteligente:** Corrige caracteres comumente confundidos na leitura óptica (como `O` por `0`, `I/l` por `1`, `S` por `5`, `Z` por `2`) e isola o bloco de dígitos mais compatível com o código esperado.
- **Validação em 3 Níveis:**
  - **Sucesso Exato (Verde | 98% a 100%):** O código do comprovante confere perfeitamente com o sistema.
  - **Alerta de Atenção (Amarelo | 83% a 97%):** Alta semelhança detectada, mas com divergência nos dígitos centrais (indica possível boleto de outro mês ou valor do mesmo cliente). Exige conferência manual.
  - **Falha Total (Vermelho | Abaixo de 83%):** Divergência completa (documento inválido ou de outro pagador).
- **Tema Claro / Escuro:** Alternância dinâmica de tema diretamente na interface.
- **Limpeza Instantânea:** Botão dedicado para resetar campos, pré-visualizações em canvas e status para a próxima consulta.

### 🛠️ Tecnologias Utilizadas

- **HTML5 / CSS3 / JavaScript Puro (ES6+):** Arquitetura autossuficiente em arquivo único com variáveis CSS e layout responsivo.
- **Tesseract.js (v5):** Engine de OCR compilada para rodar localmente no navegador via WebAssembly.
- **PDF.js (Mozilla):** Biblioteca para renderização e conversão de páginas PDF em imagem no cliente.
- **Font Awesome (v6):** Ícones vetoriais da interface.

### 📦 Como Usar

Como o projeto é puramente *frontend*, não exige servidores web (Apache, Nginx ou Node.js) nem etapas de compilação:

1. Clone este repositório ou baixe o arquivo `index.html` (`prototipo.html`).
2. Abra o arquivo diretamente em qualquer navegador moderno (Chrome, Edge, Firefox, Brave).
3. Informe o código de barras ou número esperado no sistema.
4. Selecione, arraste ou cole (`Ctrl+V`) o comprovante de pagamento.
5. Clique em **"Ler & Comparar Comprovante"**.

---

## 📄 License | Licença

This project is open-source and free to use, modify, and adapt for internal operations or personal projects.  
Este projeto é aberto e de uso livre para adaptações e melhorias em operações internas ou projetos pessoais.
