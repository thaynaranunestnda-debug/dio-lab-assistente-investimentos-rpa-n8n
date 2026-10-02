# 🤖 Assistente de Investimentos com RPA e IA Generativa

Projeto desenvolvido no desafio da **DIO** que combina **RPA (Robotic Process Automation)** com **workflows de IA no N8N** para automatizar a comunicação com clientes do setor financeiro.

O sistema coleta os dados dos clientes, identifica o perfil de investidor de cada um, cruza esse perfil com uma base de produtos de investimento e gera **e-mails personalizados com IA generativa**.

## 🎯 Objetivo

Construir um pipeline de automação que:

- Coleta dados de clientes de uma página web usando **Python**
- Processa as informações em um workflow no **N8N**
- Cruza o perfil de investidor (Conservador, Moderado ou Arrojado) com as opções de investimento disponíveis
- Gera mensagens personalizadas para cada cliente usando um **modelo de linguagem (LLM)**

## 🏗️ Arquitetura

| Etapa | Ferramenta | Função |
|---|---|---|
| Hospedagem | GitHub Pages | Servir a página de clientes e o CSV de investimentos |
| Extração (RPA) | Python + BeautifulSoup | Coletar dados dos clientes via web scraping |
| Orquestração | N8N | Receber, processar e cruzar os dados |
| Geração com IA | OpenAI (GPT-4o) no N8N | Criar e-mails personalizados |

## 🔄 Fluxo do Workflow

1. **Webhook:** recebe os dados dos clientes enviados pelo script Python.
2. **HTTP Request:** busca o arquivo `data.csv` com as opções de investimento.
3. **Code (leitura do CSV):** transforma o CSV em uma lista de produtos (perfil, produto, mínimo e rentabilidade).
4. **Merge:** combina os dados dos clientes com a base de investimentos.
5. **Code (cruzamento):** para cada cliente, filtra os produtos do mesmo perfil que cabem no saldo, escolhe a melhor recomendação, registra o motivo e cria variações de mensagem base.
6. **Code (separação):** transforma o resultado em **um item por cliente**, com o e-mail de destino já definido.
7. **Message a model (IA):** recebe os dados do cliente e a recomendação e gera um e-mail em JSON (`subject`, `text_body`, `html_body`).
8. **Code (tratamento do JSON):** limpa a resposta da IA, converte para campos separados e garante um texto reserva caso a IA devolva um formato inválido.
9. **Envio / Respond to Webhook:** entrega o e-mail padronizado e responde à requisição inicial.

## 🧠 Decisões Técnicas

- **RPA com Python:** simula o que uma pessoa faria manualmente (abrir a página, ler a tabela e enviar os dados), sem depender de uma API.
- **Filtro por saldo:** só são recomendados produtos cujo investimento mínimo é compatível com o saldo do cliente.
- **Normalização de dados:** o perfil é padronizado (sem acentos e em minúsculas) e o saldo convertido de `R$ 12.500,00` para número, evitando erros de comparação.
- **Prompt com regras:** idioma pt-BR, tom amigável e profissional, texto curto, sem promessas de ganho, garantias ou linguagem agressiva.
- **Tratamento de erros:** se a IA não devolver um JSON válido, o fluxo não quebra e usa a mensagem base como reserva.
- **Mensagem base com variações:** o código gera três versões de texto e sorteia uma, que serve de referência para a IA.

## 📁 Estrutura do Repositório

```
├── README.md
├── src/
│   └── extrair_clientes.ipynb   # Script de RPA (Python)
├── n8n/
│   └── workflow.json            # Workflow exportado
└── docs/
    ├── index.html               # Página de clientes
    └── data.csv                 # Opções de investimento
```

## ▶️ Como Executar

1. Faça um fork deste repositório.
2. Importe `n8n/workflow.json` no seu N8N (Cloud ou local).
3. Configure sua credencial da OpenAI no nó de IA.
4. Copie a URL do Webhook e coloque no script `src/extrair_clientes.ipynb`.
5. Execute o notebook no Google Colab e acompanhe o fluxo no N8N.

## 🎥 Demonstração

<img width="1246" height="574" alt="image" src="https://github.com/user-attachments/assets/880e9a4f-a0a1-4855-bdb1-5463030bf257" />


## 👩‍💻 Autora

Thaynara Nunes - Github (thaynaranunestnda)

*Projeto desenvolvido para fins educacionais no desafio "Criando um Assistente de Investimentos com RPA e IA Generativa", da DIO.*
---

**Bons estudos e mãos à obra** 🚀

Se tiver dúvidas, lembre-se: a melhor forma de aprender é experimentando. Erre, corrija e celebre cada pequena vitória no caminho.
