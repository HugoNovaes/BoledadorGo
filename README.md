# Manual do BoletadorGo

O BoletadorGo utiliza os arquivos _settings.json_, _template.txt_ e _codintegracao.json_.
O arquivo _settings.json_ contém os parâmetros para inicialização do BoletadorGo, os quais são os seguintes:

## Arquivo settings.json:
```
{
    "numThreads": 4,
    "freqImpressao": 250,
    "limiteFila": 8192,
    "timeout": 30,
    "endpoint": "https://localhost:8081/execute",
    "token": "Bearer eyJraWQiOm51bGwsImFsZyI6IlJTMjU2In0.eyJpc3MiOiJTZWN1cml0eS1XZWIiLCJhdWQiOiJCYW5rcHJvIiwic3ViIjoiYWRtaW4iLCJuYmYiOjE2ODEzOTc4OTQsImp0aSI6IjFXNzJ5WVJ6Z1laNHpVMEY3QWl0clEiLCJpYXQiOjE2ODEzOTc5NTR9.GfdjbETS91LYALpCsRab3fWl2kcUScuXnjZgyyvU25MV9hMVhNNcA5G-P6SD1z1sPidFFx8TpBe2sm1N1QHN1mAOxGccexOMp8Psk51r_EB-bcliGd3JLAMr82qpHQb_HM__ikCagKxAAEOWMEc_lE_i8EzGejs4zad3kwGjp9IUrfSwlMuObClX-pTdcpZ6TtCHEaNOE7biFJ7CoEJT-p4uQE6MpnzH02BlosmtkMH_vDCkaoHs7xfUwJN4UkBTkjnBAf0YsX5CAViR-RAXIX4mebt_UZNVaLOO5Dym6nuhNJdljGxqmCYlkjSp0302N-yqi7-EG1MhNJusDfxkKg"
}
```

### numThreads
    A quantidade de threads que o BoletadorGo utilizará. Recomenda-se utilizar a mesma
    quantidade de threads da CPU para uma melhor performance. Por exemplo, se a CPU
    possui 8 núcleos e 16 threads, utilize o valor 16.

### freqImpressao
    A velocidade em que o BoletadorGo imprimirá os resultados na tela, em milisegundos.
    Quanto menor esse valor, maior a frequência da impressão. Entretanto, isso pode
    impactar na performance do boletador.

### limiteFila
    Quantidade de operações que o BoletadorGo irá enfileirar caso o _endpoint_ demore
    a responder.

### timeout
    Tempo limite de resposta para cada envio de operação ao _endpoint_.

### endpoint
    Endereço do método a ser acionado na API de destino das rajadas.

### token
    Token JWT a ser utilizado caso a API exiga uma conexão autenticada. Incluir o tipo
    de autenticação no conteúdo do token. Exemplo "Bearer xxxxxxxxx".

# Arquivo template.txt
    Arquivo no formato json com marcações precedidas por '#' que serão substituídas
    pelo BoletadorGo por valores aleatórios ou sequenciais, dependendo do tipo de variável.

## Variáveis do template

### #DATA
    Substitui a variável pela data atual do sistema.
    Exemplo: "DataMovimento": "#DATA"

### #CODINT
    Código sequencial gerado a partir do sequencial existente no arquivo _codintegracao.json_.
    Exemplo: "IdIntegracao": #CODINT

### #CODCLIENTE
    Código do cliente existente no arquivo na pasta Data\Clientes.csv
    Exemplo: "CodCliente": #CODCLIENTE

### #NOME
    Nome do cliente associado ao código sorteado previamente no #CODCLIENTE.
    Exemplo: "NomeCliente": "#NOME"

### #VALOR
    Valor aleatório entre 1.00 e 10000.00 sorteado pelo BoletadorGo.
    Exemplo: "PrecoUnitario": #VALOR

### #TIPOOPERACAO
    Tipo da operação de acordo com a lista de operações a seguir: COMPRA, DESBLOQUEIO, RESGATE_NO_VENCTO, VENDA, COMPRA_CLIENTE_2, COMPRA_A_TERMO, COMPRA_COM_REVENDA, VENDA_CLIENTE_2, VENDA_A_TERMO, VENDA_COM_RECOMPRA, COMPRA_COM_REVENDA_CLIENTE_2, COMPRA_A_TERMO_CLIENTE_2, VINCULACAO, VENDA_COM_RECOMPRA_CLIENTE_2, VENDA_A_TERMO_CLIENTE_2, DESVINCULACAO, REDESCONTO, RECOMPRA, LIQUIDACAO_DE_REDESCONTO, REVENDA, RECOMPRA_CLIENTE_2, COMPRA_INTERMEDIACAO, DEPOSITO_EM_CUSTODIA, REVENDA_CLIENTE_2, VENDA_INTERMEDIACAO, RETIRADA_DE_CUSTODIA, EMISSAO_DE_TITULOS, COMPRA_TERMO_INTERMEDIACAO, BLOQUEIO, RESGATE_DE_TITULOS, VENDA_TERMO_INTERMEDIACAO

    Obs. Devido a limitação da aplicação a interpretar um resgate, recomenda-se não utilizar esse tipo de operação.

    Exemplo: "TipoOperacao": "#TIPOOPERACAO"

### #CLEARING
    O BoletadorGo sorteia o Nome da Clearing a utilizar: CETIP ou SELIC.
    Exemplo: "Clearing": "#CLEARING"

### #TIPOCLIENTE
    O boletador sorteia o Tipo de Cliente entre CLIENTE_INTERNO e MERCADO.
    Exemplo: "TipoCliente": "#TIPOCLIENTE"

### #FORMALIQ
    Fixo em "1".

### #IDPROD
    Fixo em "14"

## Exemplo de um template do Emissão:

```
{
  "siglaSistema": "GEN",
  "codigoIdInstituicao": "CNPJ=20855875000182",
  "data": "#DATA",
  "codigoIntegracao": #CODINT,
  "codigoIdCliente": "#CODCLIENTE",
  "tipoOperacao": "APLICACAO",
  "valor": #VALOR,
  "quantidade": null,
  "idCustodia": null,
  "tipoResgate": null,
  "formaLiquidacao": #FORMALIQ,
  "idProduto": "#IDPROD"
}
```

# Arquivo _codintegracao.json_
    Este arquivo possui apenas 2 parâmetros: "data" e "codIntegracao". Se a data do
    sistema não for a mesma do parâmetro "data", o valor de "codIntegracao" é resetado
    para 1.

    O valor do campo "codIntegracao" é atualizado automaticamente pelo BoletadorGo.
    Caso ocorram error de violação de chave, deve-se consultar qual o valor do último
    código de integração na API de destino e utilizar esse mais 1 no campo "codIntegracao"
    para que o BoletadorGo dê continuidade na geração dos próximos códigos de integração.

```
{
	"data": "2023-05-02",
	"codIntegracao": 1
}
```

# Parâmetros em linha de comando

### --help
    Mostra este help.
    Exemplo: BoletadorGo.exe --help

### --max:n
    Envia "n" transações contidas no arquivo template.txt.
    Exemplo: BoletadorGo.exe --max:10000

### --result:nome
    Grava os retornos no arquivo informado no parâmetro "nome".
    Exemplo: BoletadorGo.exe --result:retorno.txt 

### --envio:nome
    Grava no arquivo "nome" as transações enviadas.
    Exemplo: BoletadorGo.exe --envio:envios.txt

### --template:nome
    Utiliza um arquivo de template em vez do template.txt.
    Exemplo: BoletadorGo.exe --template:template2.txt

### --cnpj
    Gerar somente CNPJ
    Exemplo: BoletadorGo.exe --cnpj

### --cpf
    Gerar somente CPF
    Exemplo: BoletadorGo.exe --cpf

## Exemplos:
####  BoletadorGo
    Sem parâmetros, executa utilizando as configurações default existentes no
    arquivo settings.json
####  BoletadorGo --max:1000
    Executa uma rajada de 1000 operações e termina o processo.
####  BoletadorGo --result:retornos.txt --envio:envios.txt --max:5000
    Executa uma rajada de 5000 operações.
    Grava o resultado retornado pela API no arquivo retornos.txt.
    Grava os envios no arquivo envios.txt.

Obs.: BoletadorGo sem parâmetros: executa envios sem parar.
      Para interromper, tecle Ctrl-C.
