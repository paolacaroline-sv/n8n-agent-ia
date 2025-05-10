# n8n Workflow: Agente de IA para Atendimento Inteligente

Este projeto foi desenvolvido como parte dos meus estudos, tendo como principal objetivo utilizar a plataforma low/no-code **n8n** para automatizar um sistema de atendimento inteligente.
O fluxo combina ferramentas, criando um assistente virtual capaz de responder a perguntas com uma base de dados personalizada.

---

## Objetivo do Projeto
Objetivo do projeto é entregar um **assistente virtual** de um time esportivo que:
- Auxilia torcedores com informações atualizadas sobre o time, títulos, jogadores, produtos da loja e muito mais.
- Personaliza a experiência com base no histórico de interações.
- Tem sua base de conhecimento atualizada de forma eficaz e eficiente.

---

## Ferramentas Utilizadas

### **1. Google Drive**
- **Função**: Armazenamento de arquivos que servem como base de conhecimento para o agente, e captação de dados para ações futuras.
- **Integração no Workflow**: Monitoramento de novos arquivos para contextualização da IA.

### **2. OpenAI**
- **Função**: Geração de respostas inteligentes e personalizada individualmente com cada fã.
- **Integração no Workflow**: Criação de embeddings (vetores de contexto), processamento de mensagens recebidas e elaboração de respostas.

### **3. Supabase + PostgreSQL**
- **Função**: Armazenamento de embeddings dos arquivos e do histórico de interações.
- **Integração no Workflow**: Busca e mantém histórico de informações relevantes para respostas.

### **4. Telegram**
- **Função**: Canal principal de interação com os usuários.
- **Integração no Workflow**: Recebimento e envio de mensagens.

### **5. n8n**
- **Função**: Orquestração do fluxo de trabalho.
- **Integração no Workflow**: Automação de todas as etapas, desde o processamento de arquivos até o atendimento no Telegram.

---

## Estrutura do Workflow

Devido a limitações da conta de teste gratuito, as duas estruturas ficaram no mesmo workflow, mas o recomendado é separados para melhor escalabilidade do projeto.

### **1. Atualização Automática da Base de Conhecimento**
Essa parte do fluxo garante que, sempre que um arquivo for criado ou atualizado no **Google Drive**, o sistema processe o conteúdo e atualize a base de conhecimento do agente.

![workflow n8n documentação](https://i.imgur.com/YKY6pwk.png)


#### **Etapas:**
1. **File Updated / File Created**: Gatilhos que monitoram mudanças no Google Drive.
2. **Set File ID**: Identifica campos relevantes do trigger, para facilitar a estruturação do workflow.
3. **Deletar Arquivos Antigos**: Identificação e limpeza de versões antigas no Supabase.
4. **Google Drive** e **Extract From File**: Download e extração do conteúdo dos arquivos.
5. **Default Data Loader**: Transformar os dados da mensagem em um formato estruturado que a IA possa processar.
6. **Recursive Text Splitter**: Divide o conteúdo carregado em pedaços menores e mais compreensíveis para os modelos de IA.
7. **Embeddings OpenAI**: Conversão do texto em embeddings para análise contextual.
8. **Supabase Vector Store**: Armazenamento dos embeddings no banco vetorial do Supabase.

💡 **Resultado:** A base de conhecimento do agente é atualizada automaticamente sempre que um arquivo é alterado, garantindo que ele trabalhe com informações precisas e atualizadas.

---

### **2. Atendimento via Telegram**
Essa parte do fluxo cuida de toda a interação com os usuários no Telegram, utilizando IA para fornecer respostas personalizadas e contextualizadas.

![workflow n8n telegram](https://i.imgur.com/YfFC53Y.png)

#### **Etapas:**
1. **Telegram Trigger**: Recebe mensagens enviadas pelos usuários.
2. **Filtro Usuário Real**: Ignora mensagens de bots, grupos ou inválidas.
3. **Edit Fields**: Identifica campos relevantes do trigger, para facilitar a estruturação do workflow.
4. **Captação de Leads**: Salva dados do usuário para ações futuras.
5. **Primeira Mensagem**: Envia uma mensagem de boas-vindas caso seja o primeiro contato.
6. **AI Agent**:
   - **OpenAI Chat**: Gera as respostas com base no contexto.
   - **Postgres Chat Memory**: Mantém o histórico das conversas.
   - **Supabase Vector Store**: Busca informações relevantes no banco vetorial.
7. **Telegram**: Envia a resposta processada de volta ao usuário.

💡 **Resultado:** Um assistente virtual, capaz de entender o contexto e responder perguntas tanto para novos visitantes, quanto para os mais fãs do time.

---

## Como Executar este Workflow

### **Pré-requisitos**
1. Conta configurada nas plataformas:
   - **n8n** 
   - **Google Drive**
   - **OpenAI**
   - **Supabase**
   - **Telegram**
4. Ambiente para rodar o n8n (local ou em nuvem).

### **Passos para Configuração**
1. Importe o workflow para o n8n.
2. Configure as credenciais para cada ferramenta integrada.
3. Crie um bot oficial do Telegram, através do @BotFather, para ativar o trigger de mensagem.
4. Crie de uma pasta no **Google Drive**, para triggers de arquivos.
5. Configure uma mensagem de boas vindas.
6. Configure um prompt personalizado para o IA Agent.
7. Certifique-se de que os gatilhos estão ativos.

---

## Possíveis Melhorias
- Adicionar canais de interação (ex.: WhatsApp ou Web Chat).
- Captação de dados e área de interesse para conhecer melhor os torcedores do time.
- Integrar com APIs externas para enriquecer as respostas do agente.
- Refinar o modelo de IA para melhorar a personalização.

---

## Aprendizados e Reflexões
- **Criação de Prompts Claros**: Um prompt bem construído é a base para extrair o melhor de sistemas baseados em IA. Define o propósito, limita ambiguidades, e garante que as respostas atendam às expectativas com maior eficiência e qualidade.
- **Automação Simples, Impacto Grande**: Mesmo soluções simples, como este workflow, podem gerar valor significativo, poupar tempo com atividades operacionais repetitivas e escalar rapidamente.
- **IA como Aliada**: Frenquentemente encontro questões como: será que vale a pena começar agora? Será que a IA substituirá os desenvolvedores? Ao ter esse contato com automação, percebi que, antes de sermos concorrentes, somos aliadas.  

---

## Sobre o Projeto
<p>Esse workflow foi desenvolvido como parte do meu processo de aprendizagem, sendo minha primeira experiência prática explorando o potencial da automação integrada com inteligência artificial, através de uma plataforma low-code.</p>
<p>Além disso, esta documentação também faz parte do meu aprendizado. É a primeira vez que documento um projeto, e algumas partes ainda não estão totalmente claras para mim. Por isso, se você chegou até aqui, seu feedback será muito gratificante!</p>

---

Se tiver dúvidas ou sugestões, fico à disposição!
<p>Muito obrigada! 💛</p>
<h6>Conecte-se comigo: <a href="https://linkedin.com/in/paolacaroline-sv/" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="https://www.linkedin.com/in/paolacaroline-sv/" height="15" width="25" /></a></h6>
