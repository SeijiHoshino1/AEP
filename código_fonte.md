# Código fonte em C 

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <string.h>
#include <unistd.h>

#define FILE_NAME "dados.txt"

typedef struct {
    char user[15];
    char senha[15];
} dados;

void criptografarSenha(char senha[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        senha[i] += 11;
    }
}

void descriptografarSenha(char senha[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        senha[i] -= 11;
    }
}

void addUsuario() {
    system("cls");
    dados x;
    char confirmaSenha[15];
    FILE *file = fopen("dados.txt", "a");

    if (file == NULL) {
        printf("ERRO ao abrir o arquivo\n");
        return;
    }
	
	printf("\n-------------REGISTRAR NOVO USUÁRIO-------------\n\n");
    printf("Novo usuário: ");
    scanf("%s", x.user);

    printf("Senha: ");
    scanf("%s", x.senha);
    printf("Repita a senha: ");
    scanf("%s", confirmaSenha);

    while (strcmp(x.senha, confirmaSenha) != 0) {
        printf("ERRO!\nRepita a senha: ");
        scanf("%s", confirmaSenha);
    }

	criptografarSenha(x.senha, strlen(x.senha));
	
    fprintf(file, "%s %s\n", x.user, x.senha);
    fclose(file);
    system("cls");
    printf("Usuário inserido com sucesso.\n");
    sleep(2);
}

void listarUsuarios() {
    system("cls");
    dados x;
    FILE *file = fopen(FILE_NAME, "r");

    if (file == NULL) {
        printf("ERRO ao abrir o arquivo\n");
        return;
    }

    printf("\nLista de Usuários:\n");
    printf("Nome           Senha\n");

    char linha[50];
    while (fgets(linha, sizeof(linha), file) != NULL) {
        if (sscanf(linha, "%s %s", x.user, x.senha) == 2) {
        	descriptografarSenha(x.senha, strlen(x.senha));
            printf("%-15s %-15s\n", x.user, x.senha);
        }
    }
    fclose(file);
}

void alterarUsuario() {
    system("cls");
    char name[15], senha[15];
    int encontrado = 0;
    dados x;
    FILE *file = fopen("dados.txt", "r");
    FILE *tempFile = fopen("temp.txt", "w");

    if (file == NULL || tempFile == NULL) {
        printf("ERRO ao abrir o arquivo\n");
        if (file) fclose(file);
        if (tempFile) fclose(tempFile);
        return;
    }

    printf("Nome do usuário a alterar: ");
    scanf("%s", name);
    printf("Senha atual do usuário: ");
    scanf("%s", senha);

    criptografarSenha(senha, strlen(senha));

    while (fscanf(file, "%s %s", x.user, x.senha) == 2) {
        if (strcmp(x.user, name) == 0 && strcmp(x.senha, senha) == 0) {
            encontrado = 1;
            printf("Novo Nome: ");
            scanf("%s", x.user);
            printf("Nova Senha: ");
            scanf("%s", x.senha);

            criptografarSenha(x.senha, strlen(x.senha));
        }
        fprintf(tempFile, "%s %s\n", x.user, x.senha);
    }
    fclose(file);
    fclose(tempFile);

    if (encontrado) {
        remove(FILE_NAME);
        rename("temp.txt", FILE_NAME);
        printf("Usuário alterado com sucesso.\n");
    } else {
        remove("temp.txt");
        printf("Usuário não encontrado.\n");
    
	}
}

void excluirUsuario() {
    system("cls");
    char name[15], senha[15];
    int encontrado = 0;
    dados x;
    FILE *file = fopen(FILE_NAME, "r");
    FILE *tempFile = fopen("temp.txt", "w");

    if (file == NULL || tempFile == NULL) {
        printf("ERRO ao abrir o arquivo\n");
        if (file) fclose(file);
        if (tempFile) fclose(tempFile);
        return;
    }

    printf("Usuário a ser excluído: ");
    scanf("%s", name);
    printf("Senha do usuário: ");
    scanf("%s", senha);

    criptografarSenha(senha, strlen(senha));

    while (fscanf(file, "%s %s", x.user, x.senha) == 2) {
        if (strcmp(x.user, name) == 0 && strcmp(x.senha, senha) == 0) {
            encontrado = 1;
            continue;
        }
        fprintf(tempFile, "%s %s\n", x.user, x.senha);
    }
    fclose(file);
    fclose(tempFile);

    if (encontrado) {
        remove(FILE_NAME);
        rename("temp.txt", FILE_NAME);
        printf("Usuário excluído com sucesso.\n");
    } else {
        remove("temp.txt");
        printf("Usuário não encontrado.\n");
    }
}

void menu() {
    int opcao;
    do {
        system("cls");
        printf("\nMENU:\n");
        printf("1. Inserir Usuário\n");
        printf("2. Alterar Usuário\n");
        printf("3. Excluir Usuário\n");
        printf("4. Listar Usuários\n");
        printf("5. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                addUsuario();
                break;
            case 2:
                alterarUsuario();
                break;
            case 3:
                excluirUsuario();
                break;
            case 4:
                listarUsuarios();
                printf("Pressione [Enter] para voltar ao menu\n");
				getchar(); // espera Enter
                getchar(); // espera Enter
                system("cls");
                printf("Voltando ao menu...\n");
                sleep(2);
                break;
            case 5:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida.\n");
        }
    } while (opcao != 5);
}

int main() {
    setlocale(LC_ALL, "Portuguese");
    menu();
    
}
```
