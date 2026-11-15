# Canal de atualização do Vitalis OS

Este repositório **não tem código-fonte**. Ele é o canal por onde as máquinas
com Vitalis OS instalado recebem correção: um manifesto assinado e os pacotes
que ele descreve.

É público porque é a máquina instalada que o lê, sem credencial nenhuma. Um
canal privado exigiria um token dentro de cada máquina — que é exatamente o
tipo de segredo que não se distribui.

## O que a máquina faz

1. Busca `manifesto.json`.
2. **Confere a assinatura** contra a chave pública embutida no próprio
   sistema. Manifesto sem assinatura, com assinatura de outra chave ou com
   um único byte alterado é recusado e nada é baixado.
3. Compara as versões com o que já tem instalado e baixa só o que falta,
   segurança primeiro.
4. Confere o SHA-256 e o tamanho de cada pacote contra o manifesto. O que não
   bate é apagado, não guardado.
5. Instala, guardando o binário anterior como `.velho`.

Nada disso depende de servidor nosso no ar: o canal é este repositório.

## Por que a assinatura, se já há SHA-256

O SHA-256 prova que o arquivo chegou como o manifesto descreveu. Quem troca o
manifesto troca os hashes junto — sozinho, ele não prova nada sobre **quem**
publicou. A assinatura é a única prova de origem, e é por isso que a máquina
recusa o manifesto antes de olhar qualquer pacote.

A chave privada não está aqui e nunca estará. Ela vive com quem publica.

## Estrutura

```
manifesto.json      o que existe, em que versão, com que hash, assinado
pacotes/            os binários publicados, um arquivo por versão
```

Pacote antigo não é apagado: uma máquina que perdeu a conexão no meio do
download ainda tem o manifesto anterior em mãos e precisa poder terminar.

## Publicação

Não se comita à mão. Quem publica é a Ação `Canal` do repositório do
Vitalis OS, a partir de uma etiqueta `v1.2.3`: ela compila, roda os testes,
assina com a chave do canal e faz o commit aqui.
