Template Mestre Z-API – Agente de IA Multicanal (n8n)

Este projeto é um fluxo mestre de Agente de IA desenvolvido no n8n, integrado com Z-API (WhatsApp), OpenAI, Supabase, Redis (Upstash) e Postgres, capaz de:

Receber mensagens de múltiplos formatos

Entender texto, áudio, imagem e PDF

Manter contexto com buffer inteligente

Atender automaticamente ou redirecionar para humano

Registrar leads no Supabase

Responder de forma humanizada via WhatsApp

 Visão Geral da Arquitetura
WhatsApp / Webhook
        ↓
Filtro Inicial
        ↓
Tratamento de Mensagens
(texto | áudio | imagem | PDF)
        ↓
Buffer Redis (contexto)
        ↓
Agente de IA (LangChain + OpenAI)
        ↓
Parser Estruturado
        ↓
Envio via Z-API

 Principais Funcionalidades
 Entrada Multicanal

Webhook recebe mensagens de:

WhatsApp (Z-API)

Telegram

Instagram

Messenger

 Tipos de Mensagens

O fluxo identifica automaticamente:

Tipo	Tratamento
Texto	Enviado direto ao buffer
Áudio	Transcrição com OpenAI
Imagem	Análise com GPT-4o-mini
PDF	Extração de texto
Erro	Mensagem de fallback
 Buffer Inteligente (Redis)

Todas as mensagens do usuário são armazenadas em um buffer temporário:

Chave: {IdConversa}_buffer

Aguarda novas mensagens por X segundos

Junta tudo em um único contexto

Envia para o agente de IA

 Gestão de Leads (Supabase)

Tabela: Leads

Campo	Tipo
nome	text
id_conversa	text

O fluxo:

Verifica se o lead existe

Cria automaticamente se não existir

 Agente de IA

Modelo: gpt-4.1-mini

Prompt configurado para:

Suporte e vendas

Linguagem adaptável

Emojis nas respostas

Uso de contexto de imagem e PDF

Respeito a regras e restrições

🧾 Parser Estruturado

O agente gera respostas no formato:

{
  "mensagens": [
    "Mensagem 1",
    "Mensagem 2"
  ]
}


As mensagens são quebradas em partes curtas e naturais.

📤 Envio de Mensagens

As respostas são enviadas via Z-API:

Em lotes

Com atraso entre mensagens

Humanização da conversa

 Variáveis de Controle

No node Parametros Fluxo:

Variável	Função
EsperaBuffer	Tempo de espera entre mensagens
TempoInatividadeAgente	Tempo para reset
ModeloTemperatura	Criatividade do agente
 Tecnologias Utilizadas

n8n

LangChain

OpenAI (GPT-4.1 / GPT-4o-mini)

Z-API (WhatsApp)

Supabase

Redis (Upstash)

PostgreSQL (memória)

 Como Usar

Importe o JSON no n8n

Configure:

Credenciais do OpenAI

Supabase

Redis

Z-API

Ajuste o Webhook para seu canal

Edite o prompt do agente

Ative o workflow 

 Exemplos

“Quero saber sobre os cursos”

(envia áudio) → transcrição automática

(envia imagem) → descrição inteligente

(envia PDF) → leitura e contexto
