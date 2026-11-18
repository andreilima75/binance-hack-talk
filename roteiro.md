# Roteiro falado (cerca de 8 minutos)

## 1. Abertura (0:00–0:30)
Em 29 de outubro, a Binance perdeu cerca de 6,4 bilhões de dólares. O FBI atribui o ataque à Coreia do Norte. Nenhuma criptografia foi quebrada. Foram quatro meses e três notebooks de engenheiros: um Homebrew adulterado, uma entrevista falsa, um pyenv trocado e uma ferramenta interna de IA. Tudo vem do relatório preliminar da Binance, e eu vou marcar o que é análise de terceiros.

## 2. Homebrew (0:30–1:15)
Junho. Um engenheiro de Wallet DevOps procura no buscador uma ferramenta de IA para programar. Ele baixa uma cópia adulterada do Homebrew. A segurança percebe, troca o notebook e rotaciona as credenciais. Parece resolvido, mas o atacante já sabe quem ele é e quem trabalha com ele. A máquina foi limpa, mas a informação sobre a equipe já tinha vazado.

## 3. Entrevista (1:15–2:00)
Setembro. Um colega dele recebe uma proposta numa plataforma de vagas legítima. Quem escreve é um "Project Director" de uma empresa que não existe. Primeiro há uma chamada de vídeo ao vivo com um "RH", que mostra uma rede social cheia de fotos pessoais para criar confiança. Depois vem um chat com o "CEO". No fim, ele recebe um teste: baixar um repositório no notebook de trabalho. O código malicioso roda durante a configuração.

## 4. pyenv, as seis operações (2:00–3:15)
O relatório traz as operações de arquivo, e elas acontecem dentro de `/opt/homebrew/Cellar/pyenv/2.8.0/libexec`:

1. Cria um `pyenv_` ao lado do `pyenv` real.
2. Grava nele 3.414 bytes. O original tem 3.344.
3. Apaga o `pyenv` real.
4. Renomeia `pyenv_` para `pyenv`.
5. Apaga o link em `/opt/homebrew/bin`.
6. Cria um link novo e abre um `zsh`.

A partir daí, o pyenv recebe comandos de um servidor do atacante. Repararam que nada disso pediu senha?

## 5. Por que passou (3:15–4:15)
Era um zero-day: um malware que ninguém conhecia, sem assinatura e sem regra de detecção. O antivírus de endpoint não viu nada. O arquivo tinha quase o mesmo tamanho do original e ficava numa pasta que pertence ao próprio usuário. O pyenv continuava funcionando, e o malware apagou os próprios rastros. Cada vez que alguém chamava o pyenv, rodava código do atacante. Segundo analistas, o que pegaria isso são hashes dos binários guardados fora do notebook e rodar o teste de entrevista num ambiente isolado. A Binance não divulgou o nome do malware, o servidor nem por que escolheu o pyenv.

## 6. Ferramenta de IA (4:15–5:00)
20 de outubro. Do notebook do engenheiro B, o atacante usa uma ferramenta interna de IA para controlar as máquinas de A e de um terceiro engenheiro, C. Na máquina de A, ele encontra um token de deploy de contêineres. A Binance não explica como a ferramenta deu esse controle nem diz o nome dela. O relatório não fala em prompt injection nem em MCP. Os analistas fazem a leitura que interessa: se um agente consegue agir na máquina de um colega, ele é uma identidade privilegiada.

## 7. Serviço de assinatura (5:00–5:40)
29 de outubro. Com o token de A, o atacante entra na máquina de C, sobe um contêiner e roda um script contra o serviço interno de assinatura. Nenhuma chave vazou: o próprio serviço assinou os saques. Na Ethereum, ele usou o EIP-7702 para delegar uma carteira quente a um contrato dos atacantes.

## 8. Nonce (5:40–6:30)
Foram 50 autorizações para a mesma carteira. Só vale a que tem o nonce atual, e a carteira continuava fazendo transações. Segundo análises externas, uma delas acertou. A delegação ficou registrada na blockchain, então suspender os saques não adiantou. Como a carteira usa MPC, não havia uma chave única para revogar. A revogação só saiu à 00:10 UTC, quase quatro horas depois. Fica a pergunta: 50 autorizações para a mesma conta, em nonces seguidos, e ninguém barrou?

## 9. Dinheiro (6:30–6:50)
Em resumo: cerca de 44% está congelado ou apreendido, segundo a Elliptic. O resto está em ativos que ninguém congela, com lavagem automatizada. A Binance repôs tudo aos usuários.

## 10. Controles (6:50–7:30)
Uma pergunta para cada etapa:
- Instaladores têm origem verificada?
- Código externo roda isolado?
- Temos detecção por comportamento?
- Qual é o alcance dos nossos agentes de IA?
- Tokens têm escopo mínimo e vida curta?
- Quem pede uma assinatura também a aprova?

## 11. Discussão (7:30–8:00)
Para fechar, três perguntas. Alguém aqui rodaria um teste de recrutador no notebook do trabalho? Que acesso nossas ferramentas de IA têm hoje? Onde uma ação crítica depende de uma única máquina? Escolham uma.
