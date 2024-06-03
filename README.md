# atividade-12
#ifndef ALUNO_H
#define ALUNO_H

#define MAX_ALUNOS 5
#define TAMANHO_NOME 50
#define NUM_NOTAS 4

void calcular_notas(int idx);
void adicionar_aluno();
void listar_alunos();
void editar_aluno();

#endif // ALUNO_H
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


int matriculas[MAX_ALUNOS];
char nomes[MAX_ALUNOS][TAMANHO_NOME];
int idades[MAX_ALUNOS];
float notas[MAX_ALUNOS][NUM_NOTAS];
float nota_totals[MAX_ALUNOS];
float medias[MAX_ALUNOS];
char resultados[MAX_ALUNOS][15];
int quantidade_alunos = 0;

void calcular_notas(int idx) {
    nota_totals[idx] = 0;
    for (int i = 0; i < NUM_NOTAS; i++) {
        nota_totals[idx] += notas[idx][i];
    }
    medias[idx] = nota_totals[idx] / NUM_NOTAS;

    if (medias[idx] < 4) {
        strcpy(resultados[idx], "Reprovado");
    } else if (medias[idx] >= 4 && medias[idx] <= 6) {
        strcpy(resultados[idx], "Recuperacao");
    } else {
        strcpy(resultados[idx], "Aprovado");
    }
}

void adicionar_aluno() {
    if (quantidade_alunos >= MAX_ALUNOS) {
        printf("Limite de alunos atingido!\n");
        return;
    }

    int idx = quantidade_alunos;
    printf("Digite a matrícula do aluno: ");
    if (scanf("%d", &matriculas[idx]) != 1) {
        printf("Erro ao ler a matrícula!\n");
        return;
    }
    printf("Digite o nome do aluno: ");
    scanf(" %[^\n]", nomes[idx]); // lê o nome com espaços
    printf("Digite a idade do aluno: ");
    if (scanf("%d", &idades[idx]) != 1) {
        printf("Erro ao ler a idade!\n");
        return;
    }

    for (int i = 0; i < NUM_NOTAS; i++) {
        do {
            printf("Digite a nota %d do aluno (0 a 10): ", i + 1);
            if (scanf("%f", &notas[idx][i]) != 1) {
                printf("Erro ao ler a nota!\n");
                return;
            }
            if (notas[idx][i] < 0 || notas[idx][i] > 10) {
                printf("Nota inválida! Insira um valor entre 0 e 10.\n");
            }
        } while (notas[idx][i] < 0 || notas[idx][i] > 10);
    }

    calcular_notas(idx);
    quantidade_alunos++;
    printf("Aluno adicionado com sucesso!\n");
}

void listar_alunos() {
    if (quantidade_alunos == 0) {
        printf("Nenhum aluno cadastrado!\n");
        return;
    }
    printf("Lista de alunos cadastrados:\n");
    for (int i = 0; i < quantidade_alunos; i++) {
        printf("Matrícula: %d\n", matriculas[i]);
        printf("Nome: %s\n", nomes[i]);
        printf("Idade: %d\n", idades[i]);
        for (int j = 0; j < NUM_NOTAS; j++) {
            printf("Nota %d: %.2f\n", j + 1, notas[i][j]);
        }
        printf("Nota Total: %.2f\n", nota_totals[i]);
        printf("Média: %.2f\n", medias[i]);
        printf("Resultado: %s\n", resultados[i]);
        printf("------------------------\n");
    }
}

void editar_aluno() {
    if (quantidade_alunos == 0) {
        printf("Nenhum aluno cadastrado!\n");
        return;
    }
    int matricula;
    printf("Digite a matrícula do aluno a ser editado: ");
    if (scanf("%d", &matricula) != 1) {
        printf("Erro ao ler a matrícula!\n");
        return;
    }
    int encontrado = -1;
    for (int i = 0; i < quantidade_alunos; i++) {
        if (matriculas[i] == matricula) {
            encontrado = i;
            break;
        }
    }
    if (encontrado == -1) {
        printf("Aluno não encontrado!\n");
        return;
    }

    int idx = encontrado;
    printf("Editando aluno: %s\n", nomes[idx]);
    for (int j = 0; j < NUM_NOTAS; j++) {
        do {
            printf("Digite a nova nota %d do aluno (0 a 10): ", j + 1);
            if (scanf("%f", &notas[idx][j]) != 1) {
                printf("Erro ao ler a nota!\n");
                return;
            }
            if (notas[idx][j] < 0 || notas[idx][j] > 10) {
                printf("Nota inválida! Insira um valor entre 0 e 10.\n");
            }
        } while (notas[idx][j] < 0 || notas[idx][j] > 10);
    }
    calcular_notas(idx);
    printf("Aluno editado com sucesso!\n");
}
#include <stdio.h>


int main() {
    int opcao;
    do {
        printf("\nSistema de Cadastro de Alunos\n");
        printf("1. Adicionar Aluno\n");
        printf("2. Listar Alunos\n");
        printf("3. Editar Aluno\n");
        printf("4. Sair\n");
        printf("Escolha uma opcao: ");
        if (scanf("%d", &opcao) != 1) {
            printf("Erro ao ler a opção!\n");
            while (getchar() != '\n'); // limpa o buffer
            continue;
        }

        switch (opcao) {
            case 1:
                adicionar_aluno();
                break;
            case 2:
                listar_alunos();
                break;
            case 3:
                editar_aluno();
                break;
            case 4:
                printf("Saindo...\n");
                break;
            default:
                printf("Opcao invalida!\n");
                break;
        }
    } while (opcao != 4);

    return 0;
}

