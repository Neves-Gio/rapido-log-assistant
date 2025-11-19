# rapido-log-assistant
Assistente inteligente da RápidoLog — fluxo completo de atendimento automatizado usando IA, embeddings, n8n e integrações avançadas.
Autor: Giovanni Gomes Neves
Descrição: Desenvolvedor Full Stack especializado em automações com IA e n8n.
Este projeto implementa um sistema completo de atendimento automatizado para a RápidoLog,
composto por FAQ inteligente, rastreamento de encomendas e abertura de chamados, tudo
orquestrado dentro do n8n, utilizando RAG, Supabase com pgvector, OpenAI e integrações via API.
A assistente virtual Bia conduz as conversas de forma carismática, objetiva e humanizada.
 Objetivo do Desafio
Desenvolver um fluxo completo de atendimento automatizado para a RápidoLog utilizando IA,
cobrindo: - FAQ com busca semântica - Rastreamento com explicação humanizada - Abertura guiada de
chamados
 1. FAQ Inteligente (RAG com Embeddings)
O módulo de FAQ utiliza busca vetorial para fornecer respostas precisas e contextualizadas.
 Como funciona:
Os documentos são divididos em chunks e armazenados em public.chunks .
Cada chunk recebe um embedding gerado com text-embedding-3-small.
O usuário envia uma pergunta → o sistema gera embedding.
O Supabase retorna o chunk mais semelhante.
O GPT responde somente com base no conteúdo encontrado.
 Estrutura da tabela chunks
id
doc
section
text
embedding (vector(1536))
 Regras aplicadas no FAQ
Responder em até 3 frases.
Não inventar informações.
Se não encontrar resposta, usar fallback:
Fluxo Geral
1.
2.
3.
4.
5.
•
•
•
•
•
•
•
•
1
"Você pode consultar o status pelo rastreamento. Se o prazo estiver fora do
normal, posso abrir um chamado."
 2. Rastreamento Inteligente
O módulo de rastreamento consulta a API oficial e traduz a resposta técnica para um texto humanizado.
 Como funciona:
Cliente envia código (ex: RL2024001234BR).
API retorna status, origem, destino e previsão.
Node JavaScript normaliza a resposta.
Bia apresenta o status de forma clara e amigável.
 Comportamento da assistente
Sempre se apresenta:
"Oie, meu nome é Bia, atendente virtual da RápidoLog "
Explica o status em linguagem simples.
NUNCA inventa informações.
Se o código for inválido, responde com empatia.
 3. Abertura Guiada de Chamados
A abertura de tickets funciona de forma conversacional e 100% guiada, avançando etapa por etapa.
 Etapas do fluxo
perguntar_nome
perguntar_telefone
perguntar_codigo
perguntar_tipo
perguntar_descricao
confirmar_dados
finalizado
 Lógica interna
O modelo recebe sempre:
{
"ticket": {
"nome": "",
"telefone": "",
1.
2.
3.
4.
•
•
•
•
•
•
•
•
•
•
•
2
"codigo_rastreio": "",
"tipo_problema": "",
"descricao": ""
},
"proxima_etapa": "string",
"modelo_respondeu": "string"
}
Com base nisso, a Bia faz APENAS a pergunta necessária da etapa atual.
 Regras importantes
Uma pergunta por vez.
Nunca pular etapas.
Nunca repetir perguntas.
Nunca inventar dados.
Sempre responder com carisma e clareza.
 Arquitetura Geral dos Fluxos (n8n)
Fluxo principal:
Webhook → Identificar intenção → FAQ / Rastreamento / Chamado → Resposta da
Bia
Fluxo de embeddings:
Criado para gerar embeddings automaticamente: 1. Buscar chunks sem embedding 2. Gerar
embedding via OpenAI 3. Atualizar Supabase
 Tecnologias Utilizadas
Tecnologia Função
n8n Orquestração de automações
OpenAI GPT-4.1-MINI Modelo leve, rápido e barato
OpenAI Embeddings Similaridade para RAG
Supabase + pgvector Armazenamento de embeddings
JavaScript Nodes Normalização de dados
API de Rastreamento Status da encomenda
•
•
•
•
•
3
 Estrutura do Repositório
root/
 ├─ workflows/
 │ ├─ faq.json
 │ ├─ rastreamento.json
 │ └─ chamados.json
 ├─ images/
 │ └─ flow.png
 ├─ README.md
 Instruções de Instalação e Execução
1. Clonar o repositório
git clone https://github.com/seu-usuario/rapido-log-assistant.git
cd rapido-log-assistant
2. Instalar o n8n (local)
npm install -g n8n
3. Criar arquivo .env (se necessário)
OPENAI_API_KEY=xxxxx
SUPABASE_URL=xxxxx
SUPABASE_KEY=xxxxx
4. Iniciar o n8n
n8n start
5. Importar os workflows
No painel do n8n: - Settings → Import workflows → selecionar arquivos dentro de workflows/
 Licença
Este projeto segue a licença MIT, permitindo uso livre, modificação e distribuição.
4
 Autor
Giovanni Gomes Neves
Desenvolvedor Full Stack especializado em automações com IA e n8n
