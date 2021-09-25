#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    
	char nome [30];
    
	int nascimento;
    
	
	float gastos;	

}PESSOA;

typedef struct {
	
	PESSOA *pPessoas;
	
	int qtdPessoas;	

}LISTAPESSOAS;

void CriaListaVazia(LISTAPESSOAS *listaP) {
    
    listaP->pPessoas = (PESSOA *) calloc(10, sizeof(PESSOA));
    
}

void CriarListaVaziaMaior(LISTAPESSOAS *listaP, int pos) {
	
	listaP->pPessoas = (PESSOA*) calloc(10 *(1+ (pos/10)),sizeof(PESSOA));
}

void InserirRegistro(char nome [], int nascimento, float gastos, LISTAPESSOAS *listaP, int pos) {
	
	printf("\n POS: %d", pos);
	if ( pos >= 10 && pos % 10 == 0)
	{
		listaP->pPessoas = ( PESSOA * ) realloc (listaP->pPessoas, 10* ((pos/10)+1) * 	sizeof(PESSOA));
		
	}
	
	PESSOA p;
	strncpy(p.nome, nome, 30);
	p.nascimento = nascimento;
	p.gastos = gastos;
	
	memcpy((listaP->pPessoas+pos), &p, sizeof(PESSOA));
}

LISTAPESSOAS BuscarRegistroRm(char nome[], LISTAPESSOAS lista) {
	
	LISTAPESSOAS listaTmp;
	CriarListaVaziaMaior(&listaTmp, lista.qtdPessoas);
	listaTmp.qtdPessoas = lista.qtdPessoas;
	
	int iTmp = 0;
	for (int i = 0; i < lista.qtdPessoas; i++) 
	{
		if (strcmp(nome, (lista.pPessoas+i)->nome) != 0)
		{
			int x = i - iTmp;
			memcpy(listaTmp.pPessoas+x, lista.pPessoas+i, sizeof(PESSOA));
			
		} else {
			iTmp = 1;
		}
		
	}
    listaTmp.qtdPessoas -= 1;
    return listaTmp;
    
}

LISTAPESSOAS BuscarRegistroAt ( char nome [], LISTAPESSOAS lista) {
	
	float novoGasto;
	for (int i = 0; i < lista.qtdPessoas; i++)
	{
    	if (strcmp(nome, (lista.pPessoas+i)-> nome) == 0)
		{
			
			printf("\n Digite o valor gasto pelo cliente: ");
			scanf("%f", &novoGasto);
			(lista.pPessoas+i)->gastos = novoGasto;
			return lista;
			
			
		}
	}
	return lista;
}

void ZerarRegistrosGastos(LISTAPESSOAS lista) {
	
	for (int i = 0; i < lista.qtdPessoas; i++) {
		
		(lista.pPessoas+i)->gastos = 0.0;
	}
	printf("\n Gasto do mês zerado com sucesso!");
}

void ListarMelhorComprador(LISTAPESSOAS lista) {
	
	float gastosTmp = 0.0;
	PESSOA *p;
	for (int i = 0; i < lista.qtdPessoas; i++) 
	{
		printf("\n Gastos do cliente %s: %f", (lista.pPessoas+i)->nome, (lista.pPessoas+i)->gastos);
		if ((lista.pPessoas+i)->gastos > gastosTmp)
		{
			gastosTmp = (lista.pPessoas+i)->gastos;
			p = lista.pPessoas+i;
		}
	}
	
	printf("\n O Melhor Comprador Foi: ");
	printf(" Nome: %s, Nascimento: %d, Gastos: %f", p->nome, p->nascimento, p->gastos);
	
}

void ListaMontanteCliente(char buscaNome[], LISTAPESSOAS lista) 
{
	for (int i = 0; i < lista.qtdPessoas; i++)
	{
		if (strcmp(buscaNome,(lista.pPessoas+i)->nome) == 0)
		{
			printf("\n Cliente: %d", i);
			printf(" Nome: %s, Nascimento: %d, Gastos: %f", (lista.pPessoas+i)->nome, (lista.pPessoas+i)->nascimento, (lista.pPessoas+i)->gastos);
			
		}
	}

}

void ListarPessoas(LISTAPESSOAS lista) {
	
	for (int i = 0; i < lista.qtdPessoas; i++) {
		printf("\n Cliente: %d", 1);
		printf(" Nome: %s, Nascimento: %d, Gastos: %f", (lista.pPessoas+i)->nome, (lista.pPessoas+i)->nascimento, (lista.pPessoas+i)->gastos);
		
	}
}

void MenuPrincipal (){
	printf("\n 11 - Listar Todos.");
	printf("\n 1 - Incluir um novo cliente.");
	printf("\n 2 - Remover cliente.");
	printf("\n 3 - Atualizar gastos de cliente.");
	printf("\n 4 - Zerar gastos por virada do mês.");
	printf("\n 5 - Listar melhor comprador.");
	printf("\n 6 - Listar montante gasto de cliente.");
	printf("\n 0 - Sair.");
	printf("\n Escolha qual das opções deseja seguir: ");
}
int main()
{
    LISTAPESSOAS lista;
	
	CriaListaVazia(&lista);
	lista.qtdPessoas = 0;
	
	char nome[30];
	int nascimento;
	float gastos;
	
	char buscaNome[30];
	
	int escolha = 8;
    
    while (escolha != 0 )
    {
    	switch (escolha) 
		{	
    			printf("\n Digite o nome do cliente: ");
    			scanf("%s", nome);
    			
    			printf("\n Digite o ano de nascimento: ");
    			scanf("%d", &nascimento);
    			
    			printf("\n Digite os gastos deste cliente: ");
    			scanf("%f", &gastos);
    			
    			InserirRegistro(nome, nascimento, gastos, &lista, lista.qtdPessoas);
    			lista.qtdPessoas += 1;
    			printf("\n Cadastro realizado com sucesso!");
    			
    			escolha = 8;
    			break;
    		
    		case 2:
    			printf("\n Digite o nome do cliente que deseja remover: ");
    			scanf("%s", buscaNome);
    			lista = BuscarRegistroRm (buscaNome, lista);
    			printf("\n Remoção feita com sucesso!");
				escolha = 8;
    			break;
    			
    		case 3:
			    printf("\n Digite o nome do cliente que deseja atualizar: ");
				scanf("3s", buscaNome);
				lista = BuscarRegistroAt(buscaNome, lista);
				printf("\n Atualização feita com sucesso!");
				escolha = 8;
				break;
				
			case 4:
				ZerarRegistrosGastos(lista);
				escolha = 8;
				break;
			
			case 5:
			    ListarMelhorComprador(lista);
			    escolha = 8;
			    break;
    		
    		case 6:
    			printf("\n Digite o nome do cliente que deseja: ");
    			scanf("\s ", buscaNome);
    			ListaMontanteCliente(buscaNome, lista);
				escolha = 8;
    			break;
    		
    		case 11:
				ListarPessoas(lista);
				escolha - 8;
				break;
    		
    		default:
    				MenuPrincipal();
    				scanf(" %d", &escolha);
    				break;
    		
		}
		   		

    				
	}
    
	
}
