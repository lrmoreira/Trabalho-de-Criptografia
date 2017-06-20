# Trabalho-de-Criptografia
Software que faz criptografia de uma mensagem
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


 
char TEXTO[100000];
int TAM_TEXTO= 100000;
int TEXTCRIPT [100000];
//=====================
//int CHAVECRIPT [100000];
char CHAVE [6] ;
int TAM_CHAVE = 6; //chave = senha
//=====================


FILE * AbrirArquivo(char modo);

void FecharArquivo(FILE *arquivo);

bool Criptografar();

bool descriptografar();

void ReceberTexto(char mensagem[]);

void descript(void);

void menu();

bool descriptchave();
    
int main(){

	system("color a");
    menu();
    return 0;
}



FILE * AbrirArquivo(char modo){
    FILE *arquivo;

        switch (modo){
        	case 'g':
            	arquivo = fopen("text_cript.txt", "wt");
            	break;
       		case 'l':
            	arquivo = fopen("text_cript.txt", "rt");
            	break;
        	case 'a':
            	arquivo = fopen("text_cript.txt","a");
            	break;
        			}
        	if(arquivo == NULL){
            	printf("\nOcorreu um erro ao abrir o arquivo");
            	system("pause");
        	}
        return arquivo;
}

void FecharArquivo(FILE *arquivo){
    fclose(arquivo);
}


bool Criptografar(){
	
	int ChaveCript;
	char chave [TAM_TEXTO];
	int i;
	FILE *arquivo;
	
	printf("\n\nPara que o processo de Criptografia seja mais seguro, ensira um senha!");
	printf("\nDigite uma SENHA  de Exatas 6 Caracteres ");
	gets(chave);
	strcpy(CHAVE,chave);
	
	if (chave > CHAVE ){
		printf("\nVoce Ultrapassou o limite de caracteres\n");
		}						
			
		for (i = 0; i <= strlen(TEXTO);i++){
			TEXTCRIPT[i] = TEXTO[i];
		//	printf(" %d ",TEXTCRIPT[i]); Serve apenas para verificar se está fazendo o calculo certo 
		}	
					
		for(int j=0; j < strlen(TEXTO); ){
        	for(int i=0;i<TAM_CHAVE; i++){
            	ChaveCript = CHAVE[i];
            	TEXTCRIPT[j] *= ChaveCript;
           		j++;
        	}
    	}
		arquivo = AbrirArquivo('g');//modo de gravação
		for (i = 0; i < strlen(TEXTO);i++){
			fprintf(arquivo, "%d ", TEXTCRIPT[i]);
		}
		FecharArquivo(arquivo);
		
		arquivo = fopen("chave.txt","wt");
			for (i=0; i<strlen(CHAVE);i++){
				fprintf(arquivo,"%x ",chave[i]);
			}
		FecharArquivo(arquivo);
		
		
	
	
	    return true;
}


bool descriptchave(){
	FILE *arquivo;
	int i;
	int verificar[TAM_CHAVE];
	char verificar2[TAM_CHAVE];
	arquivo = fopen("chave.txt", "rt");
	
	if (arquivo == NULL){
		printf("\nOcorreu um erro ao abrir o arquivo que contem a chave, O Arquivo não possui a chave");
	}
	
	printf("\n Nesse ambiente voce descobira a chave para ter acesso a mensagem enviada...\n\n");
	printf(" A Chave para essa mensagem é : ");
	for (i=0; i<=5;i++){
		fscanf(arquivo,"%x ", &verificar[i]);
	//	printf("%d ", verificar[i]);
		verificar2[i] =   (char) verificar[i];
		printf("%c",verificar2[i]);
}

	printf("\n\nNao se esqueca de sua chave ...\n ");
		system("pause");

}

bool descriptografar(){
	
	FILE *arquivo;
	int i, j;
	char aux[TAM_CHAVE];
	int texto;
	int cont_chave;
	int cont_texto;
	int atenticacao[TAM_CHAVE];
	char atenticacao2[TAM_CHAVE];
	
	printf("\nDigite a Senha de seguranca\n");
	gets(aux);
			
	arquivo = fopen("chave.txt","rt");
	j=0;

	for (i=0; i <= 5; i++){
		fscanf(arquivo, "%x", &atenticacao[i]);
		atenticacao2[j] = (char) atenticacao[i];
	//	printf("%c ",atenticacao2[i]);
		j++;
	}
	FecharArquivo(arquivo);	

	i = strcmp(atenticacao2, aux);
	if (i>0 || i<0){
		printf("Voce digitou a senha errada\n");
	}
	else{
		
	 arquivo = AbrirArquivo('l');
	 cont_chave = 0;
	 cont_texto = 0;
	 
		while (!feof(arquivo)){
			fscanf(arquivo,"%d", &texto);

			texto /= aux[cont_chave];
			TEXTO[cont_texto] = (char) texto;
		
			cont_texto++;		
		
			if(cont_chave == TAM_CHAVE-1){
            	cont_chave =0;
        	}else{
            	cont_chave++;
}
}
}
		return true;
}


void ReceberTexto(char mensagem[]){
    char aux[TAM_TEXTO];
    FILE *arquivo;
    char pause;
    

    if (strcmp(mensagem,"msg")==0){
        printf("\n\nDigite a MENSAGEM a ser criptografada: ");
        gets(aux);
        strcpy(TEXTO , aux);

        if (strlen(aux) > TAM_TEXTO){
        	printf("\n\nMensagem maior do que o permitido.");
                
        }
    		
    	if(Criptografar()){
        	printf("\n\nPressione enter para abrir a pasta do arquivo gerado!\n");
        	scanf("%c",&pause);
        	setbuf(stdin,NULL);
        	system("explorer C:\\Users\\Lucas\\Desktop\\APS - Criptografia\\"); // funciona se voce tiver essa pasta, tente colocar na pasta que o .txt foi criado
    }
    }
    }


void menu(){
    int opcao;
    FILE *arquivo;
    
    while(1){
    system("cls");
    printf("\n\t\tBem Vindo ao nosso programa de criptografia\n\n");
    printf("O que voce deseja fazer? \n");
    printf("Digite [1] Para Criptografar uma mensagem\n");
    printf("Digite [2] Para Descriptografar uma Mensagem\n");
    printf("Digite [3] Para Descriptografar a Chave recebida\n");
    printf("Digite [4] Para Sair do programa\n\n");
    printf("Escolha uma Opcao: \n");
    scanf ("%d",&opcao);
    fflush(stdin);

    switch(opcao){
        case 1:
            system("cls");
            ReceberTexto("msg");
            break;
        case 2:
            system("cls");
            descriptografar();
           	descript();
            break;
        case 3:
        	system("cls");
        	descriptchave();
        	break;            
        case 4:
            system("cls");
            printf("Programa esta sendo Encerrando . . . .\n");
            system("Pause");
            FecharArquivo(arquivo);
            system("cls");
            exit(0);
        default:
            printf("Voce escolheu uma opcao Invalida! Tente Novamente\n");
            system("Pause");
        }
    }
}


void descript(void){
	
	char pause;
	FILE *arquivo;
	int i =0;
	
	 if (descriptografar()){
			printf("%s", TEXTO);
			printf("\n");
		//	//FecharArquivo(arquivo);
			//AbrirArquivo('g');
		//	for (i = 0; strlen(TEXTO);i++){
			arquivo = fopen("descript.txt","wt");
			fputs(TEXTO, arquivo);
			printf("\n O Arquivo foi gerado mostrando a a Descriptografada");
			printf("\n\nPressione enter para abrir a pasta do arquivo gerado!\n");
        	scanf("%c",&pause);
        	setbuf(stdin,NULL);      	
        	system("pause");
        	system("explorer C:\\Users\\Lucas\\Desktop\\APS - Criptografia\\"); //necessário colocar o diretório correspondente
			
			}
			FecharArquivo(arquivo);
	
}


