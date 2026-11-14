# Documentação Técnica: Validador Local de Comprovantes (Client-Side OCR)

## 1. Visão Geral e Arquitetura

O **Validador Local de Comprovantes** é uma aplicação web *Single-Page* (arquivo único [`index.html`](../index.html)) projetada para operar **100% no lado do cliente (*Client-Side*)**. A ferramenta utiliza exclusivamente o hardware local (CPU e memória RAM) da máquina do operador para processar imagens e documentos PDF, extrair textos via OCR (Reconhecimento Óptico de Caracteres) e validar códigos de barras ou linhas digitáveis contra os dados esperados no sistema financeiro — garantindo total privacidade e zero dependência de servidores externos.

### Garantias de Privacidade e Independência

* **Zero Tráfego de Dados Sensíveis:** Nenhum arquivo de comprovante, dado pessoal, valor ou código de barras é enviado para servidores externos ou APIs de terceiros.
* **Execução Offline:** As bibliotecas externas (`Tesseract.js`, `PDF.js` e `Font Awesome`) são carregadas via CDN apenas na abertura inicial da página. Após o carregamento em cache, toda a operação matemática e de leitura ocorre localmente via *WebAssembly*.
* **Autossuficiência Visual:** Ícones e *favicons* são embutidos diretamente no código via vetores e *Data URIs* SVG, evitando falhas de carregamento em ambientes restritos.

---

## 2. Histórico de Evolução e Melhorias Implementadas

Durante o ciclo de desenvolvimento e testes em ambiente operacional, a ferramenta passou por diversas iterações para resolver desafios práticos de usabilidade e precisão de leitura:

### Fase 1: Prova de Conceito e Motor OCR

* Implementação base utilizando `Tesseract.js` (idioma português) para leitura óptica de imagens (`.jpg`, `.png`).
* Integração com `PDF.js` para renderizar a primeira página de arquivos `.pdf` em um elemento `<canvas>` oculto em alta resolução (`scale: 2.0`), permitindo que o OCR leia documentos PDF sem necessidade de conversão manual prévia.

### Fase 2: Identidade Visual e Experiência do Usuário (UX)

* **Interface Limpa e Focada:** Design minimalista com paleta neutra e destaque em verde esmeralda (`#10b981`), centralizando o foco na área de conferência.
* **Alternância de Tema (Dark / Light Mode):** Implementação de variáveis CSS (`:root` e `[data-theme="dark"]`) controladas por um botão no cabeçalho.
* **Produtividade na Entrada de Dados:**
  * Suporte a **Colagem Direta (`Ctrl + V`)** de capturas de tela (*PrintScreen*) direto da área de transferência.
  * Suporte a **Arrastar e Soltar (*Drag & Drop*)**.
  * Adição do botão **Limpar** para resetar formulários, diagnósticos e memória de arquivo para a próxima consulta.

### Fase 3: Tratamento de Dados e Inteligência de Validação

* **Sanitização de Input:** Remoção automática de pontos, espaços, traços e barras do código inserido pelo operador antes da comparação.
* **Autocorreção de Caracteres do OCR (`corrigirTextoOCR`):** Resolução de falhas clássicas de leitura óptica através da conversão preventiva de letras visualmente semelhantes a números (`O`/`o` $\rightarrow$ `0`, `I`/`l`/`|` $\rightarrow$ `1`, `S` $\rightarrow$ `5`, `Z` $\rightarrow$ `2`).
* **Extração Baseada em Âncoras (`extrairTrechoInteligente`):** Como os comprovantes bancários possuem dezenas de outros números (datas, horários, autenticações, CNPJs), o algoritmo utiliza os **4 primeiros dígitos** (banco/moeda) e os **4 últimos dígitos** do código esperado como âncoras para isolar a janela exata de $\approx 44$ dígitos correspondente ao boleto.
* **Sistema de Classificação em 3 Níveis (Prevenção de Falsos Positivos):**
  Boletos do mesmo banco e da mesma empresa emissora compartilham prefixos e sufixos idênticos, mudando apenas os dígitos centrais (fator de vencimento e valor). Para evitar que um boleto de um mês anterior seja aprovado por engano, a validação foi dividida em três faixas:

| Nível | Faixa de Compatibilidade | Visual | Significado Operacional |
| :--- | :--- | :--- | :--- |
| **1. Sucesso** | $\ge 98\%$ ou Exato | Verde | O código do boleto confere perfeitamente com o sistema. |
| **2. Alerta** | $83\%$ a $97,9\%$ | Amarelo / Laranja | Alta semelhança estrutural, mas divergência nos dígitos centrais. **Indica provável boleto de outro mês ou valor diferente do mesmo cliente.** Exige conferência manual. |
| **3. Falha** | $< 83\%$ | Vermelho | Divergência total. Documento ilegível, inválido ou pertencente a outro cliente/banco. |

---

## 3. Estrutura Técnica do Código

O arquivo único [`index.html`](../index.html) está dividido em três camadas principais:

1. **Camada de Estrutura e Metadados (`<head>` e `<body>`):**
   * Configuração de *favicon* embutido em SVG e título da aba.
   * Estrutura semântica contendo cabeçalho institucional genérico, área de input do sistema, *drop-zone* interativa, caixa de diagnóstico do OCR, botões de ação e barra de status.
2. **Camada de Estilização (`<style>`):**
   * Baseada em *CSS Custom Properties* para troca instantânea de temas.
   * Feedback visual dinâmico para os estados `.match` (verde), `.warning` (amarelo) e `.mismatch` (vermelho).
3. **Camada Lógica (`<script>`):**
   * `definirArquivo(file)` & *Listeners* (`paste`, `drop`, `change`): Gerenciam a captura do arquivo em memória.
   * `converterPdfParaImagem(file)`: Utiliza `pdfjsLib` para transformar a página 1 do PDF em uma string `DataURL` PNG.
   * `corrigirTextoOCR(texto)`: Limpa ruídos alfabéticos comuns gerados pelo Tesseract.
   * `extrairTrechoInteligente(textoLido, esperado)`: Algoritmo de janela deslizante (*sliding window*) com pontuação ponderada por prefixo e sufixo.
   * `calcularSimilaridade(str1, str2)`: Compara caractere por caractere para definir a porcentagem de precisão.
   * `limparTudo()`: Reseta o estado da aplicação e limpa o `<canvas>`.

---

## 4. Guia Rápido de Uso para Operadores

1. Abra a aplicação online ou execute o arquivo [`index.html`](../index.html) localmente em qualquer navegador moderno (Google Chrome, Microsoft Edge, Firefox).
2. Copie a linha digitável ou código de barras do sistema financeiro (pode conter pontos e espaços) e cole no campo **"Código de Barras / Número Esperado no Sistema"**.
3. Envie o comprovante clicando na caixa pontilhada, arrastando o arquivo PDF/Imagem ou simplesmente pressionando **`Ctrl + V`** caso tenha tirado um print da tela.
4. Clique em **"Ler & Comparar Comprovante"** e aguarde alguns segundos enquanto o processador local realiza a leitura.
5. Verifique o resultado colorido na tela. Ao finalizar, clique em **"Limpar"** para iniciar a próxima conferência.[cite: 10]
