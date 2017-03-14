#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <locale.h>
#include <time.h>
#include <windows.h>

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
            printf("\n%s\n%i\n%s\n%s", movie[i].nome, movie[i].ano, movie[i].diretor, movie[i].sinopse);
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
    printf("DIGITE O NOME DO FILME: ");
    gets(nome);

    for(i=0; i<200; i++){
        if((movie[i].ano == 0) && (strcmp(movie[i].nome, nome) != 0)){
            strcpy(movie[i].nome, nome);
            printf("DIGITE O ANO DO FILME: ");
            scanf("%i", &movie[i].ano);
            fflush(stdin);
            printf("DIGITE O NOME DO DIRETOR DO FILME: ");
            gets(movie[i].diretor);
            printf("DIGITE A SINOPSE DO FILME: ");
            gets(movie[i].sinopse);
            printf("DIGITE A CATEGORIA DO FILME: ");
            gets(movie[i].categoria);
            printf("ESCOLHA A EXCLUSIVIDADE DO FILME:    1 - VIP     2 - NORMAL\n");
            scanf("%i", &vip);
            fflush(stdin);
            switch(vip){
                case 1:
                    strcpy(movie[i].exclu, "VIP");
                    break;
                case 2:
                    strcpy(movie[i].exclu, "LIVRE");
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

void Logar(FILE *log){

    Login lgn;
    char id[30];
    char sn[30];
    log = fopen("Arquivos\\login.txt", "rb");
    fread(&lgn, sizeof(lgn), 1, log);
    do{
    system("cls");
    tela1();
    printf("\n\t\t\tLogin: ");
    gets(id);
    printf("\n\t\t\tSenha: ");
    gets(sn);
    if((strcmp(lgn.login, id) != 0) || (strcmp(lgn.senha, sn) != 0)){
        system("cls");
        printf("\n\n\t\t\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n");
        printf("\t\t\t@   LOGIN OU SENHA INCORRETO   @\n");
        printf("\t\t\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n\n");
        getchar();
    }
    if((strcmp(lgn.senha, sn) == 0) && (strcmp(lgn.login, id) == 0)){
        system("cls");
        printf("\n\n\t\t\t************************\n");
        printf("\t\t\t*  LOGADO COM SUCESSO  *\n");
        printf("\t\t\t************************\n\n");
        break;
    }
    }while((strcmp(lgn.senha, sn) != 0) || (strcmp(lgn.login, id) != 0));
    fclose(log);

}
void tela1(){
    printf("\n\n\t@@@@@@  @@@@  @@@@@@   @@@@@@@@   @@@      @@@    @@@@    @@@@\n");
    printf("\t   @@  @@  @@ @@   @@  @@@        @@@      @@@     @@@@  @@@@\n");
    printf("\t  @@   @@  @@ @@@@@@   @@@@@@@@   @@@      @@@      @@@@@@@@\n");
    printf("\t @@    @@@@@@ @@       @@@        @@@      @@@     @@@@  @@@@\n");
    printf("\t@@@@@@ @@  @@ @@       @@@        @@@@@@@  @@@    @@@@    @@@@\n");


}

void tela2(FILE *filmes){

    srand(time(NULL));
    int acao, sair;
    sair = 0;
    do{
    system("color 20");
    filme movie[200];
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    system("cls");
    printf("\t\t\t\t\t\tBEM VINDO AO ZAPFLIX\n\n");
    printf("\tMENU\n");
    printf("1 - Configurações dos Clientes\t\t2 - Configurações dos Filmes\n");
    printf("3 - Configurações dos Funcionários\t4 - Busca\n");
    printf("5 - Catálogo de filmes\t\t\t6 - Sair\n\n");
    scanf("%i", &acao);
    fflush(stdin);

    switch(acao){
        case 1:
            break;
        case 2:
            configf(filmes);
            break;
        case 3:
            break;
        case 4:
            pesquisarf(filmes);
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
    int i, remo;
    filmes = fopen("Arquivos\\filmes.txt", "rb");
    fread(&movie, sizeof(movie), 1, filmes);
    fclose(filmes);
    printf("Digite o filme que deseja editar: ");
    gets(pesquisa);
    for(i=0; i<200; i++){
        if(strcmp(pesquisa, movie[i].nome) == 0){
            printf("O que deseja editar do filme?\n\n");
            printf("1 - Nome\n");
            printf("2 - Ano\n");
            printf("3 - Diretor\n");
            printf("4 - Categoria\n");
            printf("5 - Sinopse\n");
            printf("6 - Remover filme\n\n");
            scanf("%i", &remo);


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
            }
            break;
        }
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




int main()
{
    setlocale(LC_ALL, "Portuguese");
    FILE *filmes;
    int acao, sair;
    Login lgn;
    FILE *log;
    strcpy(lgn.login, "admin");
    strcpy(lgn.senha, "admin");
    mkdir("Arquivos");
    log = fopen("Arquivos\\login.txt", "wb");
    fwrite(&lgn, sizeof(lgn), 1, log);
    fclose(log);
    Logar(log);
    getchar();
    system("cls");
    loading();
    getchar();
    system("cls");
    tela2(filmes);

    return 0;
}
