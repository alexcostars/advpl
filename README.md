# ADVPL

## Database

### Select e Update via ADVPL

Estrutura padrão para ler e gravar valores no banco de dados via ADVPL

``` ADVPL
private cCODCLI := '000001'
//define a tabela que será iterada, juntamente com a variável que será utilizada para referenciar a tabela
dbselectarea("SA1")
//define o índice do banco de dados que será utilizado para filtrar as informações
SA1->(dbsetorder(1))
//Filtrando as informações a serem percorridas
if SA1->(dbSeek(cCODCLI))

	while SA1->(!Eof())
		//define a condição de parada
		//independente do filtro feito com o dbSeek, a tabela continuará a ser percorrida, mesmo que os registros não correspondam ao filtro. Desta forma, é necessário incluir uma condição de parada para evitar que os registros que não devem ser percorridos sejam iterados.
		if SA1->A1_COD <> cCODCLI
			Exit
		endif
		
		//lendo (select)
		variavel := SA1->A1_CLINOME
		
		//alterando (update)
		Reclock("SA1",.F.)
			SA1->A1_CLINOME := "Alex Costa"
		MsUnlock("SA1")
		
		SA1->(dbSkip())
	enddo
endif
```

### Insert via ADVPL

Estrutura padrão para inserir valores no banco de dados via ADVPL

``` ADVPL
//define a tabela que será iterada, juntamente com a variável que será utilizada para referenciar a tabela
dbselectarea("SA1")
Reclock("SA1",.T.)
	SA1->SA1_CODCLI := 1
	SA1->SA1_NOMECLI := "Alex Costa"
MsUnlock("SA1")
```

## ParamBox

Estrutura padrão de um ParamBox

``` ADVPL
Static Function SFParamBox()
	Local aPergs := {}
	Local aRet := {}
	Local lRet := .T.
	
	//filds do ParamBox

	If !ParamBox(aPergs,"Parametros",aRet,,,,,,,,.F.)
		lRet := .F.
	EndIf
Return ({lRet,aRet})
```

### Filds do ParamBox

Caixa de texto:
``` ADVPL
aadd(aPergs,{ 1, "{{descrição}}", Space({{tamanho}}), "{{mascara}}", '.T.', "{{alias}}", '.T.', {{tamanho}}, .F.})
```
| Atributo        | Descrição                                                                       | Exemplo           |
| ----------------|:-------------------------------------------------------------------------------:| :----------------:|
| {{descrição}}   | descrição que identifica a informação a ser preenchida no campo                 | Nome do cliente   |
| {{tamanho}}     | Largura da caixa de texto (medida em caracteres)                                | 40                |
| {{mascara}}     | Mascara do campo. Texto: **@!**, data: **@E 99/99/9999**,                       | @!                |
| {{alias}}       | Caso seja necessário adicionar um botão de Pesquisar ao lado do campo, deve ser inserido o alias da tabela a ser utilizada para a pesquisa  | SA1 |

Caixa de seleção:
``` ADVPL
aadd(aPergs,{ 2, "{{descrição}}", "{{opção_padrão}}", { {{opções}} }, {{tamanho}}, '.T.', .T.})
```
| Atributo            | Descrição                                                             | Exemplo                   |
| ------------------- |:---------------------------------------------------------------------:| :------------------------:|
| {{descrição}}       | descrição que identifica a informação a ser selecionada               | Prefixo                   |
| {{opção_padrão}}    | Define o valor que deve estar marcado por default                     | 1 - NFe                   |
| {{opções}}          | Define os valores possíveis de serem selecionados pelo usuário        | "1 - NFe","2 - NFSe"      |
| {{tamanho}}         | Largura da caixa de texto (medida em caracteres)                      | 40                        |
