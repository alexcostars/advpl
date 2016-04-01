# ADVPL

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
