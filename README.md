#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <locale.h>
#include <time.h>
#include <windows.h>
#include <conio.h>

typedef struct{

char login[30];
char senha[30];

}Login;

typedef struct{
    char nome[30];
    int ano;
    int ativo;
    char exclu[10];
    char diretor[20];
    char sinopse[200];
    char categoria[30];
}filme;

typedef struct {
    char login[30];
    char senha[30];
    char nome[50];
    char cargo[50];
    char cpf[20];
    int salario;
    int status;
    int cadastrado;
}Dados;

typedef struct{
	int num_casa;
	char bairro[50];
	char rua[50];
	char cidade[20];
}Endereco;

typedef struct{
	int status;
	int pos;
	char nome[200];
	char cpf[20];
	int dia_nasc, mes_nasc, ano_nasc;
	char tipo_cliente[10];
	Endereco ender;
}Cliente;

void loading(){

    srand(time(NULL));
    int val, i, j;

    val = rand()%4 + 1;

    switch(val){
        case 1:
            printf("\n\n\tO primeiro filme inteiramente rodado a cores foi \"Becky Sharp\",\n\tde Rouben Mamoulian, produzido pela RKO em 1935.");
            break;

        case 2:
           printf("\n\n\tA maior indústria cinematográfica do mundo pertence à Índia.\n\tO País produz uma média de 700 filmes todos os anos.");
           break;

        case 3:
            printf("\n\n\tA expressão \"sétima arte\", foi criada em 1912\n\tpelo italiano Ricciotto Canuto, se referindo ao cinema.");
            break;

        case 4:
            printf("\n\n\t\"Scarface\" (Brian De Palma, 1983), remake do filme de 1932\n\trealizado por Howard Hughes, é o filme com mais palavrões,\n\tum total de 203, uma média de um palavrão a cada 29 segundos.");
            break;

    }


    printf("\n\n\n\n");

    printf ("Carregando: \n\n");

   for (i = 0; i <= 70; i++)
   {
      //printf ("\r  %d%%", i);
      printf ("  %d%%\r", i);
      fflush (stdout);

      for (j = 0; j < i; j++)
      {
         if (!j) // Da espaco na barra pra nao enconstar na borda da tela
           printf ("  ");

         printf ("%c", 219);
         Sleep(3);
      }
   }

   printf ("\n\nFinalizando...");
   Sleep (2000);
   printf ("\r \t\t\t\t  CARREGADO\n\n\n");

}

void pesquisarf(FILE *filmes){

    system("cls");
    filme movie[200];
    char pesquisa[50];
    int i, k;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    printf("Digite o filme que deseja procurar: ");
    gets(pesquisa);
    k = 0;

    for(i=0; i<200; i++){
        if((strcmp(pesquisa, movie[i].nome) == 0) && (movie[i].ativo == 1)){
            printf("\nNome: %s\nExclusividade: %s\nAno: %i\nDiretor: %s\nCategoria: %s\nSinopse: %s", movie[i].nome, movie[i].exclu, movie[i].ano, movie[i].diretor, movie[i].categoria, movie[i].sinopse);
            k = 1;
            break;
        }
    }

    if(k == 0){
        printf("Filme não encontrado");
    }
    getchar();

}

void listarf(FILE *filmes, char pesquisa[50]){

    system("cls");
    filme movie[200];
    int i;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);

    for(i=0;i<200;i++){
        if((movie[i].ano != 0) && (movie[i].ativo == 1) && (strcmp(pesquisa, movie[i].categoria) == 0)){
            printf("%s(%s)\t", movie[i].nome, movie[i].exclu);
        }
    }
    getchar();

}

void cadastrarf(FILE *filmes){

    filme movie[200];
    int i, vip;
    char nome[50];
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    printf("DIGITE O NOME DO FILME:(sem acentos)\n");
    gets(nome);

    for(i=0; i<200; i++){
        if((movie[i].ano == 0) && (strcmp(movie[i].nome, nome) != 0)){
            strcpy(movie[i].nome, nome);
            printf("DIGITE O ANO DO FILME:(sem acentos)\n");
            scanf("%i", &movie[i].ano);
            fflush(stdin);
            printf("DIGITE O NOME DO DIRETOR DO FILME:(sem acentos)\n");
            gets(movie[i].diretor);
            printf("DIGITE A SINOPSE DO FILME:(sem acentos)\n ");
            gets(movie[i].sinopse);
            printf("DIGITE A CATEGORIA DO FILME:(sem acentos)\n");
            gets(movie[i].categoria);
            printf("ESCOLHA A EXCLUSIVIDADE DO FILME:    1 - VIP     2 - NORMAL\n");
            scanf("%i", &vip);
            fflush(stdin);
            switch(vip){
                case 1:
                    strcpy(movie[i].exclu, "VIP");
                    break;
                case 2:
                    strcpy(movie[i].exclu, "NORMAL");
                    break;
            }
            movie[i].ativo = 1;
            break;
            }
        if(strcmp(movie[i].nome, nome) == 0){
            printf("\nO FILME JÁ SE ENCONTRA NO CATÁLOGO OU ESTÁ DESATIVADO\n");
            getchar();
            break;
        }
    }
    filmes = fopen("Arquivos\\filmes.txt", "wb");
    fwrite(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
}

void excluirf(FILE *filmes, char pesquisa[50]){

    filme movie[200];
    int i, remo;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    for(i=0; i<200; i++){
        if(strcmp(pesquisa, movie[i].nome) == 0){
            printf("\nTem certeza que deseja remover esse filme?\n");
            printf("           1 - Sim        2 - Não\n\n");
            scanf("%i", &remo);
            fflush(stdin);
            if(remo == 1){
                printf("\n\nFilme removido\n");
                movie[i].ativo = 0;
            }
            if(remo != 1){
                printf("\n\nFilme continua no catálogo\n");
            }
            break;
        }
    }
    filmes = fopen("Arquivos\\filmes.txt", "wb");
    fwrite(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
}

void reativarf(FILE *filmes, char pesquisa[50]){

    filme movie[200];
    int i, remo;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    for(i=0; i<200; i++){
        if(strcmp(pesquisa, movie[i].nome) == 0){
            printf("\nTem certeza que deseja reativar esse filme?\n");
            printf("           1 - Sim        2 - Não\n\n");
            scanf("%i", &remo);
            fflush(stdin);
            if(remo == 1){
                printf("\n\nFilme reativado\n");
                movie[i].ativo = 1;
            }
            if(remo != 1){
                printf("\n\nFilme continua fora do catálogo\n");
            }
            break;
        }
    }
    filmes = fopen("Arquivos\\filmes.txt", "wb");
    fwrite(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
}

void Logar(FILE *arq, FILE *f, FILE *filmes, FILE *log, FILE *logi){

    Login lgn;
    Dados lon[200];
    char id[30];
    char sn[30];
    int i, k;
    char cp[20];
    log = fopen("Arquivos\\login.txt", "rb");
    fread(&lgn, sizeof(lgn), 1, log);
    fclose(log);
    logi = fopen("Arquivos\\funcionario.txt", "rb");
    fread(&lon, sizeof(lon), 1, logi);
    fclose(logi);
    do{
    system("cls");
    tela1();
    printf("\n\t\t\tLogin: ");
    gets(id);
    printf("\n\t\t\tSenha: ");
    gets(sn);
    k=0;
    if((strcmp(lgn.senha, sn) == 0) && (strcmp(lgn.login, id) == 0)){
        system("cls");
        printf("\n\n\t\t\t************************\n");
        printf("\t\t\t*  LOGADO COM SUCESSO  *\n");
        printf("\t\t\t************************\n\n");
        k = 1;
        strcpy(cp, "Admin");
        break;
    }

    for(i=0;i<200;i++){
        if((strcmp(lon[i].senha, sn) == 0) && (strcmp(lon[i].login, id) == 0)){
            system("cls");
            printf("\n\n\t\t\t************************\n");
            printf("\t\t\t*  LOGADO COM SUCESSO  *\n");
            printf("\t\t\t************************\n\n");
            strcpy(cp, lon[i].nome);
            k = 1;
            break;
        }
    }
    for(i=0;i<200;i++){
        if(((strcmp(lon[i].senha, sn) != 0) || (strcmp(lon[i].login, id) != 0)) && (k != 1)){
            system("cls");
            printf("\n\n\t\t\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n");
            printf("\t\t\t@   LOGIN OU SENHA INCORRETO   @\n");
            printf("\t\t\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n\n");
            getchar();
            break;
        }
    }
    }while(k != 1);

    getchar();
    system("cls");
    loading();
    getchar();
    system("cls");
    tela2(arq, f, filmes, cp);


}

void tela1(){
    printf("\n\n\t@@@@@@  @@@@  @@@@@@   @@@@@@@@   @@@      @@@    @@@@    @@@@\n");
    printf("\t   @@  @@  @@ @@   @@  @@@        @@@      @@@     @@@@  @@@@\n");
    printf("\t  @@   @@  @@ @@@@@@   @@@@@@@@   @@@      @@@      @@@@@@@@\n");
    printf("\t @@    @@@@@@ @@       @@@        @@@      @@@     @@@@  @@@@\n");
    printf("\t@@@@@@ @@  @@ @@       @@@        @@@@@@@  @@@    @@@@    @@@@\n");


}

void tela2(FILE *arq, FILE *f, FILE *filmes, char cp[20]){

    setlocale(LC_ALL, "Portuguese");
    srand(time(NULL));
    int acao, sair;
    sair = 0;
    do{
    system("color BD");
    filme movie[200];
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    system("cls");
    printf("Logado como: %s\n", cp);
    printf("\t\t\t\t\t\t************************\n");
    printf("\t\t\t\t\t\t* BEM VINDO AO ZAPFLIX *\n");
    printf("\t\t\t\t\t\t************************\n");
    printf("\t\tMENU\n");
    printf("1 - Configurações dos Clientes\t\t2 - Configurações dos Filmes\n");
    printf("3 - Configurações dos Funcionários\t4 - Busca\n");
    printf("5 - Catálogo de filmes\t\t\t6 - Sair\n\n");
    scanf("%i", &acao);
    fflush(stdin);

    switch(acao){
        case 1:
            configc(arq);
            break;
        case 2:
            configf(filmes);
            break;
        case 3:
            configfun(f);
            break;
        case 4:
            pesquisar(arq, f, filmes);
            break;
        case 5:
            catalogo(filmes);
            break;
        case 6:
            printf("\n\n\nDeseja realmente sair do programa?\n");
            printf("      1 - Sim        2 - Não\n\n");
            scanf("%i", &sair);
            break;
    }
    }while(sair != 1);

}

void catalogo(FILE *filmes){

    int acao, sair;
    char pesquisa[50];
    system("cls");
    printf("Escolha a categoria do filme.\n");
    printf("1 - Ação\t\t\t2 - Brasileiro\n");
    printf("3 - Comédia\t\t\t4 - Desenho\n");
    printf("5 - Drama\t\t\t6 - Ficção Científica\n");
    printf("7 - Infantil\t\t\t8 - Romance\n");
    printf("9 - Série\t\t\t10 - Terror\n");
    printf("\t\t11 - VOLTAR\n\n");
    scanf("%i", &acao);
    fflush(stdin);
    switch(acao){
        case 1:
            strcpy(pesquisa, "Ação");
            listarf(filmes, pesquisa);
            break;
        case 2:
            strcpy(pesquisa, "Brasileiro");
            listarf(filmes, pesquisa);
            break;
        case 3:
            strcpy(pesquisa, "Comédia");
            listarf(filmes, pesquisa);
            break;
        case 4:
            strcpy(pesquisa, "Desenho");
            listarf(filmes, pesquisa);
            break;
        case 5:
            strcpy(pesquisa, "Drama");
            listarf(filmes, pesquisa);
            break;
        case 6:
            strcpy(pesquisa, "Ficção Científica");
            listarf(filmes, pesquisa);
            break;
        case 7:
            strcpy(pesquisa, "Infantil");
            listarf(filmes, pesquisa);
            break;
        case 8:
            strcpy(pesquisa, "Romance");
            listarf(filmes, pesquisa);
            break;
        case 9:
            strcpy(pesquisa, "Série");
            listarf(filmes, pesquisa);
            break;
        case 10:
            strcpy(pesquisa, "Terror");
            listarf(filmes, pesquisa);
            break;
        case 11:
            printf("\n\n\nDeseja realmente voltar?\n");
            printf("       1 - Sim       2 - Não\n\n");
            scanf("%i", &sair);
            fflush(stdin);
            break;
    }

}

void editarf(FILE *filmes){

    filme movie[200];
    char pesquisa[50];
    int i, remo, k;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    printf("Digite o filme que deseja editar: ");
    gets(pesquisa);
    k = 0;
    for(i=0; i<200; i++){
        if(strcmp(pesquisa, movie[i].nome) == 0){
            printf("\nNome: %s\nExclusividade: %s\nAno: %i\nDiretor: %s\nCategoria: %s\nSinopse: %s\n\n", movie[i].nome, movie[i].exclu, movie[i].ano, movie[i].diretor, movie[i].categoria, movie[i].sinopse);
            printf("O que deseja editar do filme?\n\n");
            printf("1 - Nome\n");
            printf("2 - Ano\n");
            printf("3 - Diretor\n");
            printf("4 - Categoria\n");
            printf("5 - Sinopse\n");
            printf("6 - Remover filme\n");
            printf("7 - Reativar filme\n\n");
            scanf("%i", &remo);
            fflush(stdin);
            k = 1;

            switch(remo){
                case 1:
                    printf("DIGITE O NOME DO FILME: ");
                    gets(movie[i].nome);
                    filmes = fopen("Arquivos\\filmes.txt", "wb");
                    fwrite(&movie, sizeof(movie), 1, filmes);
                    fclose(filmes);
                    break;
                case 2:
                    printf("DIGITE O ANO DO FILME: ");
                    scanf("%i", &movie[i].ano);
                    fflush(stdin);
                    filmes = fopen("Arquivos\\filmes.txt", "wb");
                    fwrite(&movie, sizeof(movie), 1, filmes);
                    fclose(filmes);
                    break;
                case 3:
                    printf("DIGITE O NOME DO DIRETOR DO FILME: ");
                    gets(movie[i].diretor);
                    filmes = fopen("Arquivos\\filmes.txt", "wb");
                    fwrite(&movie, sizeof(movie), 1, filmes);
                    fclose(filmes);
                    break;
                case 4:
                    printf("DIGITE A CATEGORIA DO FILME: ");
                    gets(movie[i].categoria);
                    filmes = fopen("Arquivos\\filmes.txt", "wb");
                    fwrite(&movie, sizeof(movie), 1, filmes);
                    fclose(filmes);
                    break;
                case 5:
                    printf("DIGITE A SINOPSE DO FILME: ");
                    gets(movie[i].sinopse);
                    filmes = fopen("Arquivos\\filmes.txt", "wb");
                    fwrite(&movie, sizeof(movie), 1, filmes);
                    fclose(filmes);
                    break;
                case 6:
                    excluirf(filmes, pesquisa);
                    break;
                case 7:
                    reativarf(filmes, pesquisa);
                    break;
            }
            break;
        }
    }
    if(k == 0){
        printf("\n\nSem resultados...\n");
    }


}

void configf(FILE *filmes){

    int acao, sair;
    do{
    system("cls");
    printf("\t\t\t******************************\n");
    printf("\t\t\t* MENU DE EDIÇÃO DOS FILMES  *\n");
    printf("\t\t\t******************************\n\n\n");
    printf("1 - Cadastrar\n");
    printf("2 - Editar\n");
    printf("3 - Voltar\n\n");
    printf("Digite a função desejada: ");
    scanf("%i", &acao);
    fflush(stdin);

    switch(acao){
        case 1:
            system("cls");
            cadastrarf(filmes);
            break;
        case 2:
            system("cls");
            editarf(filmes);
            getchar();
            break;
        case 3:
            printf("\n\n\nDeseja realmente voltar?\n");
            printf("       1 - Sim       2 - Não\n\n");
            scanf("%i", &sair);
            fflush(stdin);
            break;
    }
    }while(sair != 1);

}

void listarfun(FILE *f){
    system("cls");
    Dados funcionario[200];
    f = fopen("Arquivos\\funcionario.txt", "rb");
    int i;
    printf("\t\tLISTA DE FUNCIONÁRIOS\n\n");
    fread(&funcionario, sizeof(funcionario),1, f);
    fclose(f);

        for(i=0;i<200;i++){
            if((funcionario[i].status == 1) && (funcionario[i].cadastrado == 1)){
                printf("--------------\n");
                printf("Nome : %s\n", funcionario[i].nome);
                printf("Cpf : %s\n", funcionario[i].cpf);
                printf("Salário : %.i\n", funcionario[i].salario);
                printf("Cargo : %s\n", funcionario[i].cargo);
                printf("---------------\n");
        }
    }
}

void cadastrarfun(FILE *f){

    system("cls");
    setlocale(LC_ALL, "Portuguese");
    int i, j;
    Dados funcionario[200];
    f = fopen("Arquivos\\funcionario.txt", "rb");
    fread(&funcionario,sizeof(funcionario),1,f);
    fclose(f);

    for(i=0;i<200;i++){
        if(funcionario[i].cadastrado == 0){
            printf("Digite o nome do funcionário:(sem acentos)\n");
            gets(funcionario[i].nome);
            printf("Digite o cargo do funcionário:(sem acentos)\n");
            gets(funcionario[i].cargo);
            printf("Digite o CPF do funcionário:(no formato 123.456.789-10)\n");
            gets(funcionario[i].cpf);
            printf("Digite o salário do funcionário:\n");
            scanf("%i", &funcionario[i].salario);
            fflush(stdin);
            printf("Digite um nome de usuário:(sem acentos)\n");
            gets(funcionario[i].login);
            printf("Digite uma senha:(sem acentos)\n");
            gets(funcionario[i].senha);
            funcionario[i].status = 1;
            funcionario[i].cadastrado = 1;
            f = fopen("Arquivos\\funcionario.txt", "wb");
            fwrite(&funcionario, sizeof(funcionario),1, f);
            printf("\nFuncionário cadastrado!\n");
            fclose(f);
            break;
        }
    }

    system("pause");
}

void pesquisarfun(FILE *f){
    system("cls");
    int i, k = 0;
    Dados funcionario[200];
    char pesquisa[50];
    f = fopen("Arquivos\\funcionario.txt", "rb");
    fread(&funcionario, sizeof(funcionario),1, f);
    fclose(f);
    printf("Digite o cpf do funcionário(no formato 123.456.789-10)::\n");
    gets(pesquisa);
    for(i=0;i<200;i++){

        if(strcmp(pesquisa, funcionario[i].cpf) == 0)
        {
            if(funcionario[i].status==1)
                {
                    printf("--------------\n");
                    printf("Nome : %s\n", funcionario[i].nome);
                    printf("Cpf : %s\n", funcionario[i].cpf);
                    printf("Salário : %.i\n", funcionario[i].salario);
                    printf("Cargo : %s\n", funcionario[i].cargo);
                    printf("Login : %s\n", funcionario[i].login);
                    printf("---------------\n");
                    k = 1;
                    system("pause");
                    break;
                    }
                }

            }
        if(k==0){
                    printf("\nSem resultados...\n");
                    printf("\nPressione ENTER para retornar...\n");
        }
}

void excluirfun(FILE *f){
    system("cls");
    int i;
    int exc;
    Dados funcionario[200];
    char pesquisa[50];
    f = fopen("Arquivos\\funcionario.txt", "rb");
    fread(&funcionario, sizeof(funcionario),1, f);
    fclose(f);
	printf("Digite o cpf do funcionário:\n");
    gets(pesquisa);


	for(i=0;i<200;i++){
        if(strcmp(pesquisa, funcionario[i].cpf) == 0)
            {
                printf("--------------\n");
                printf("Nome : %s\n", funcionario[i].nome);
                printf("Cpf : %s\n", funcionario[i].cpf);
                printf("Salário : %.i\n", funcionario[i].salario);
                printf("Cargo : %s\n", funcionario[i].cargo);
                printf("---------------\n");

		printf("\nDeseja realmente excluir este funcionário?\n(1)Sim;(2)Não;\n");
		scanf("%i", &exc);

			if(exc == 1){
					f = fopen("Arquivos\\funcionario.txt", "wb");
					funcionario[i].status = 0;
					fwrite(&funcionario,sizeof(funcionario),1,f);
					printf("\nFuncionário excluído!\n");
					fclose(f);
					system("pause");
					break;
				}
			}
		}
}

void pesquisar(FILE *arq, FILE *f, FILE *filmes){

    int c, i;
    do{
    system("cls");
    printf("\t\t*************************\n");
    printf("\t\t*    TELA  DE  BUSCA    *\n");
    printf("\t\t*************************\n\n");
    printf("O que deseja buscar?\n");
    printf("1 - Cliente\n");
    printf("2 - Filme\n");
    printf("3 - Funcionário\n");
    printf("4 - Voltar\n\n");
    scanf("%i", &c);
    fflush(stdin);

    switch(c){
        case 1:
            c = 1;
            pesquisarc(arq);
            getchar();
            break;
        case 2:
            c = 2;
            pesquisarf(filmes);
            getchar();
            break;
        case 3:
            c = 3;
            pesquisarfun(f);
            getchar();
            break;
        case 4:
            printf("\n\n\nDeseja realmente voltar?\n");
            printf("       1 - Sim       2 - Não\n\n");
            scanf("%i", &i);
            fflush(stdin);
            break;
        default:
            printf("Opção inválida\n");
            getchar();
            break;

    }
    }while(i != 1);

}

void editarfun(FILE *f){





    system("cls");
    Dados funcionario[200];
    char pesquisa[50];
    int i, k, naveg;
    k = 0;
    f = fopen("Arquivos\\funcionario.txt", "rb");
    fread(&funcionario, sizeof(funcionario),1,f);
    fclose(f);
    printf("Digite o CPF do funcionário que deseja alterar:(no formato 123.456.789-10) \n");
    gets(pesquisa);

    for(i=0;i<200;i++){
        if(strcmp(pesquisa, funcionario[i].cpf) == 0)
            {
                        system("cls");
                        k = 1;
                        printf("\n--------------\n");
                        printf("Nome : %s\n", funcionario[i].nome);
                        printf("Cargo : %s\n", funcionario[i].cargo);
                        printf("CPF : %s\n", funcionario[i].cpf);
                        printf("Salário : %i\n", funcionario[i].salario);
                        printf("Login : %s\n", funcionario[i].login);
                        printf("--------------\n");
                        printf("\nO que deseja alterar ?\n");
                        printf("(1)-Nome;\n");
                        printf("(2)-Cargo;\n");
                        printf("(3)-CPF;\n");
                        printf("(4)-Salário;\n");
                        printf("(5)-Login;\n");
                        printf("(6)-Senha;\n");
                        printf("(7)-Excluir funcionário;\n");
                        scanf("%i", &naveg);
                        fflush(stdin);

                switch(naveg){

                case 1 : system("cls");
                         printf("Digite um novo nome: \n");
                         gets(funcionario[i].nome);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         printf("\nAlterado!\n");
                         break;

                case 2 : system("cls");
                         printf("Digite um novo cargo: \n");
                         gets(funcionario[i].cargo);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         printf("\nAlterado!\n");
                         break;

                case 3 : system("cls");
                         printf("Digite um novo CPF: \n");
                         gets(funcionario[i].cpf);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         printf("\nAlterado!\n");
                         break;

                case 4 : system("cls");
                         printf("Digite um novo salário: \n");
                         scanf("%i", &funcionario[i].salario);
                         fflush(stdin);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         break;

                case 5 : system("cls");
                         printf("Digite um novo login: \n");
                         gets(funcionario[i].login);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         printf("\nAlterado!\n");
                         break;

                case 6 : system("cls");
                         printf("Digite uma nova senha: \n");
                         gets(funcionario[i].senha);
                         f = fopen("Arquivos\\funcionario.txt", "wb");
                         fwrite(&funcionario, sizeof(funcionario),1,f);
                         fclose(f);
                         printf("\nAlterado!\n");
                         break;

                case 7 : excluirfun(f); break;

                }break;
            }
        }
        if(k == 0){
                printf("\n\nSem resultados...\n");
                system("pause");
            }
}

void configfun(FILE *f){


    int op, sair;

    do{
        system("cls");
        printf("\t\t**********************************\n");
        printf("\t\t* MENU DE EDIÇÃO DE FUNCIONÁRIOS *\n");
        printf("\t\t**********************************\n\n\n");

        printf("(1)-Cadastrar;\n");
        printf("(2)-Editar;\n");
        printf("(3)-Voltar;\n\n");
        scanf("%i", &op);
        fflush(stdin);

        switch(op){

            case 1 : system("cls");
                     cadastrarfun(f);
                     getchar();
                     break;

            case 2 : system("cls");
                     editarfun(f);
                     getchar();
                     break;

            case 3 : printf("Deseja realmente voltar ? (1)Sim / (2)Não\n");
                     scanf("%i", &sair);
                     fflush(stdin);
                     break;

        }



    }while(sair != 1);

}

void configc(FILE *arq){


    int op, sair;
    sair = 0;
    do{
        system("cls");
        printf("\t\t**********************************\n");
        printf("\t\t*   MENU DE EDIÇÃO DE CLIENTES   *\n");
        printf("\t\t**********************************\n\n\n");

        printf("(1)-Cadastrar;\n");
        printf("(2)-Editar;\n");
        printf("(3)-Voltar;\n\n");
        scanf("%i", &op);
        fflush(stdin);

        switch(op){

            case 1 : system("cls");
                     cadastrarc(arq);
                     getchar();
                     break;

            case 2 : system("cls");
                     editarc(arq);
                     getchar();
                     break;

            case 3 : printf("Deseja realmente voltar ? (1)Sim / (2)Não\n");
                     scanf("%i", &sair);
                     fflush(stdin);
                     break;

        }



    }while(sair != 1);

}

void editarc(FILE* arq){
	setlocale(LC_ALL, "Portuguese");
	system("cls");
	Cliente a[200];
	arq = fopen("Clientes.txt", "rb");
	if(arq == NULL)
    {
    	printf("NÃO EXISTE CLIENTES CADASTRADOS!");
    	fclose(arq);
    }
    else
    {
        fread(&a, sizeof(a), 1, arq);
        fclose(arq);
        int op, x, i;
        do
        {
            printf("\n\t\t\t(1)-ALTERAR DADOS;\n\t\t\t(2)-REMOVER CLIENTE;\n\t\t\t(3)-REATIVAR CLIENTE;\n\n");
            scanf("%i", &op);
            if(op == 1)
            {
                getchar();
                char cpf[20];
                printf("DIGITE O CPF DO CLIENTE PARA CONTINUAR:\n");
                gets(cpf);
                setbuf(stdin, NULL);
                system("cls");
                int op1;
                printf("\t\t\tO QUE DESEJA ALTERAR?\n\t\t\t(1)-NOME;\n\t\t\t(2)-CPF;\n\t\t\t(3)-DATA DE NASCIMENTO;\n\t\t\t(4)-ENDEREÇO;\n\t\t\t(5)-TIPO DE CONTA\n\n");
                scanf("%i", &op1);
                getchar();
                system("cls");
                switch(op1)
                {
                    setbuf(stdin, NULL);
                    case 1:
                    {
                        char alt[20];
                        printf("DIGITE UM NOVO NOME:\n ");
                        gets(alt);
                        for(i = 0; i < 200; i++)
                        {
                            if(strcmp(a[i].cpf, cpf)==0)
                                {
                                    strcpy(a[i].nome, alt);
                                }
                        }
                        arq = fopen("Clientes.txt", "wb");
                        fwrite(&a, sizeof(a), 1, arq);
                        fclose(arq);
                        system("cls");
                        break;
                    }
                    case 2:
                    {
                        char alt[20];
                        printf("DIGITE O NOVO CPF:\n");
                        gets(alt);
                        for(i = 0; i < 200; i++)
                        {
                            if(strcmp(a[i].cpf, cpf)==0)
                                {
                                    strcpy(a[i].cpf, alt);
                                }
                        }
                        arq = fopen("Clientes.txt", "wb");
                        fwrite(&a, sizeof(a), 1, arq);
                        fclose(arq);
                        system("cls");
                        break;
                    }
                    case 3:
                    {
                        int dia, mes, ano;
                        printf("DIGITE A NOVA DATA DE NASCIMENTO: (OBS: DIGITE POR ESPAÇOS, dd mm aa)");
                        scanf("%i %i %i", &dia, &mes, &ano);
                        for(i = 0; i < 200; i++)
                        {
                            if(strcmp(a[i].cpf, cpf)==0)
                                {
                                    a[i].dia_nasc = dia;
                                    a[i].mes_nasc = mes;
                                    a[i].ano_nasc = ano;
                                }
                        }
                        arq = fopen("Clientes.txt", "wb");
                        fwrite(&a, sizeof(a), 1, arq);
                        fclose(arq);
                        system("cls");
                        break;
                    }
                    case 4:
                    {
                        int num;
                        char bairro[50];
                        char rua[50];
                        char cidade[50];
                        printf("DIGITE O NOVO ENDEREÇO:\nCIDADE: ");
                        gets(cidade);
                        setbuf(stdin, NULL);
                        printf("\nBAIRRO: ");
                        setbuf(stdin, NULL);
                        gets(bairro);
                        setbuf(stdin, NULL);
                        printf("\nRUA: ");
                        gets(rua);
                        setbuf(stdin, NULL);
                        printf("\nNº DA CASA: ");
                        scanf("%i", &num);
                        setbuf(stdin, NULL);
                        for(i = 0; i < 200; i++)
                        {
                            if(strcmp(a[i].cpf, cpf)==0)
                            {
                                strcpy(a[i].ender.cidade, cidade);
                                strcpy(a[i].ender.bairro, bairro);
                                strcpy(a[i].ender.rua, rua);
                                a[i].ender.num_casa = num;
                            }
                        }
                        arq = fopen("Clientes.txt", "wb");
                        fwrite(&a, sizeof(a), 1, arq);
                        fclose(arq);
                        system("cls");
                        break;
                    }
                    case 5:
                    {
                        char tipo[10];
                        printf("DIGITE O TIPO DE CONTA: (OBS: DIGITE COM LETRAS MAIÚSCULAS)\n");
                        gets(tipo);
                        for(i = 0; i < 200; i++)
                        {
                            if(strcmp(a[i].cpf, cpf)==0)
                            {
                                strcpy(a[i].tipo_cliente, tipo);
                            }
                        }
                        arq = fopen("Clientes.txt", "wb");
                        fwrite(&a, sizeof(a), 1, arq);
                        fclose(arq);
                        system("cls");
                        break;
                    }
                }
            }
            if(op == 2)
            {
                setbuf(stdin, NULL);
                excluirc(arq);
                break;
            }
            if(op == 3)
            {
                int i;
                setbuf(stdin, NULL);
                char cpf[20];
                printf("DIGITE O CPF DO CLIENTE:\n");
                gets(cpf);
                setbuf(stdin, NULL);
                system("cls");
                for(i = 0; i < 200; i++)
                {
                    if(strcmp(a[i].cpf, cpf)==0)
                    {
                        a[i].status = 1;
                        break;
                    }
                }
                arq = fopen("Clientes.txt", "wb");
                fwrite(&a, sizeof(a), 1, arq);
                fclose(arq);
                system("cls");
            }
            printf("\t\t\tOPERAÇÃO REALIZADA COM SUCESSO!\n");
            printf("\n\t\t\tDESEJA REALIZAR OUTRA OPERAÇÃO?\n(1)-SIM\t(2)-NÃO\n");
            scanf("%i", &x);
            system("cls");
        }while(x == 1);
    }

}

void excluirc(FILE* arq){
	setlocale(LC_ALL, "Portuguese");
	Cliente a[200];
	arq = fopen("Clientes.txt", "rb");
	fread(&a, sizeof(a), 1, arq);
	fclose(arq);
	int i;
	char pesquisa[20];
	setbuf(stdin, NULL);
	if(arq == NULL)
    {
    	printf("NÃO EXISTE CLIENTES CADASTRADOS!");
	}
	else
	{
		printf("DIGITE O CPF DO CLIENTE QUE DESEJA EXCLUIR: \n");
		gets(pesquisa);
		setbuf(stdin, NULL);
		system("cls");
		int op;
		printf("TEM CERTEZA QUE DESEJA EXCLUIR ESSE CLIENTE? \n(1)-SIM\t(2)-NÃO\n");
		scanf("%i", &op);
		getchar();
		if(op == 1)
		{
			for(i = 0 ;i < 200; i++)
			{
				if(a[i].status == 1 && (strcmp(pesquisa, a[i].cpf) == 0))
				{
					a[i].status = 0;
					break;
				}
			}
			arq = fopen("Clientes.txt", "wb");
			fwrite(&a, sizeof(a), 1, arq);
			fclose(arq);
			system("cls");
			printf("CLIENTE EXCLUIDO COM SUCESSO!\n\n");
		}
	}
}

void pesquisarc(FILE* arq){
	setlocale(LC_ALL, "Portuguese");
	Cliente a[200];
    system("cls");
    char pesquisa[50];
    arq = fopen("Clientes.txt", "rb");
    fread(&a, sizeof(a), 1, arq);
    fclose(arq);
    if(arq == NULL)
    {
    	printf("NÃO EXISTE CLIENTES CADASTRADOS!");
	}
	else
	{
		setbuf(stdin, NULL);
	    printf("PESQUISAR:\nDIGITE O CPF DO CLIENTE:\n");
	    gets(pesquisa);
	    system("cls");
    	int n;
    	for(n = 0; n < 200; n++)
    	{
    		if(a[n].dia_nasc != 0)
    		{
		        if(strcmp(pesquisa, a[n].cpf) == 0 && a[n].status == 1)
		        {
		            printf("NOME:%s\nCPF:%s \nDATA DE NASCIMENTO:%i/%i/%i ", a[n].nome, a[n].cpf, a[n].dia_nasc, a[n].mes_nasc, a[n].ano_nasc);
					printf("\nENDEREÇO:%s, %s, %s Nº%i\nTIPO DE CLIENTE:%s\n\n", a[n].ender.cidade, a[n].ender.bairro, a[n].ender.rua, a[n].ender.num_casa, a[n].tipo_cliente);
		            break;
		        }
				else if (n == 199)
				{
					printf("NÃO EXISTE CLIENTES ATIVOS!");
				}
			}
		}
	}
}

void cadastrarc(FILE* arq){
	setlocale(LC_ALL, "Portuguese");
	int i = 0;
	Cliente a[200];
	arq = fopen("Clientes.txt", "rb");
	fread(&a, sizeof(a), 1, arq);
	fclose(arq);

	system("cls");
	for(i = 0 ; i<200; i ++)
	{
		if(a[i].dia_nasc == 0)
		{

		printf("INSIRA OS DADOS DO CLIENTE: \n");
		a[i].status = 1;
		printf("1-NOME: \n");
		gets(a[i].nome);
		setbuf(stdin, NULL);
		printf("2-CPF: \n");
		gets(a[i].cpf);
		setbuf(stdin, NULL);
		printf("3-DATA DE NASCIMENTO: (OBS: DIGITE POR ESPAÇOS, dd mm aa)\n");
		scanf("%i", &a[i].dia_nasc);
		scanf("%i", &a[i].mes_nasc);
		scanf("%i", &a[i].ano_nasc);
		setbuf(stdin, NULL);
		printf("4-ENDEREÇO\n");
		printf("CIDADE: \n");
		gets(a[i].ender.cidade);
		setbuf(stdin, NULL);
		printf("BAIRRO: \n");
		gets(a[i].ender.bairro);
		setbuf(stdin, NULL);
		printf("RUA: \n");
		gets(a[i].ender.rua);
		setbuf(stdin, NULL);
		printf("Nº DA CASA: \n");
		setbuf(stdin, NULL);
		scanf("%i", &a[i].ender.num_casa);
		setbuf(stdin, NULL);
		printf("TIPO DE CLIENTE:\n");
		gets(a[i].tipo_cliente);
		setbuf(stdin, NULL);
    	fclose(arq);
    	break;
		}
	}
	arq = fopen("Clientes.txt", "wb");
	fwrite(&a, sizeof(a), 1, arq);
	fclose(arq);
}

int main(){
    setlocale(LC_ALL, "Portuguese");
    FILE *filmes;
    FILE *f;
    int acao, sair;
    Login lgn;
    FILE *log;
    FILE *logi;
    FILE *arq;
    strcpy(lgn.login, "admin");
    strcpy(lgn.senha, "admin");
    mkdir("Arquivos");
    log = fopen("Arquivos\\login.txt", "wb");
    fwrite(&lgn, sizeof(lgn), 1, log);
    fclose(log);
    Logar(arq, f, filmes, log, logi);

    return 0;
}
