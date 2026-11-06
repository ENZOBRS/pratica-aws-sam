# Respostas da Atividade Prática - Serverless

## 1. Cold Start e Init Phase
Na primeira execução do comando `sam local invoke`, o tempo total foi maior porque a infraestrutura precisou de iniciar o ambiente do zero (Cold Start), o que gerou um tempo de `Init Duration`. Na segunda execução, o ambiente já estava quente na memória, logo não houve a fase de Init e a resposta foi muito mais rápida. (Evidência: 1-invocacao-local.png)

## 2. Comportamento Stateless e Warm Containers
Ao chamar a função repetidas vezes com o `curl` normal, a variável `invocacaoNesteAmbiente` voltava sempre para 1. Isso acontece porque a arquitetura Serverless é stateless (sem estado), destruindo o ambiente após cada chamada. Quando reiniciei o servidor com a flag `--warm-containers LAZY`, a máquina foi mantida viva entre as chamadas, permitindo que a variável subisse para 2 e 3. (Evidências: 2-servidor-http.png e 3-warm-containers.png)

## 3. Limite de Tempo (Timeout)
Ao enviar a requisição com `"demorar": true`, o código pausou por 5 segundos. Como o limite padrão (Timeout) no ficheiro `template.yaml` era de apenas 3 segundos, a nuvem cortou a execução para evitar gastos, gerando um "Internal server error". 
Para resolver, alterei o Timeout no `template.yaml` para 10 segundos, recompilei o projeto com `sam build` e a chamada passou a devolver a guia com sucesso. (Evidências: 4-erro-timeout.png e 5-sucesso-timeout.png)

## 4. Demonstração em Vídeo
A execução prática dos testes documentados acima (Invocação Local, Warm Containers e Correção do Timeout) pode ser visualizada no vídeo abaixo:

🎥 [Clique aqui para assistir ao vídeo da demonstração] https://youtu.be/4IfmLFfar48