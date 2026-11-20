# calculadora-de-matrizes-
Este programa permite definir a quantidade de matrizes (2 ou mais), valida se a quantidade foi informada antes de preencher os valores, verifica as regras matemáticas de dimensão para soma, subtração e multiplicação, e exibe a matriz solução ao final.(Nota: Como o enunciado repete "subtração" nas opções 2 e 4


README.md

#include <stdio.h>
#include <stdlib.h>

#define MAX 10

void exibirMatriz(int mat[MAX][MAX], int lin, int col) {
    printf("\n--- MATRIZ SOLUCAO ---\n");
    for (int i = 0; i < lin; i++) {
        for (int j = 0; j < col; j++) {
            printf("%d\t", mat[i][j]);
        }
        printf("\n");
    }
    printf("----------------------\n");
}

int main() {
    int opcaoMenu, opcaoOperacao;
    int qtdMatrizes = 0;
    int qtdInformada = 0, matrizesPreenchidas = 0;

    int matrizes[MAX][MAX][MAX];
    int linhas[MAX], colunas[MAX];

    int resultado[MAX][MAX];
    int linRes = 0, colRes = 0;

    do {
        printf("\n=== CALCULADORA DE MATRIZES ===\n");
        printf("1 -> Escolher a quantidade de matrizes\n");
        printf("2 -> Informar matrizes\n");
        printf("3 -> Escolher operacao e calcular\n");
        printf("0 -> Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcaoMenu);

        switch (opcaoMenu) {
            case 1:
                printf("\nQuantas matrizes deseja calcular (minimo 2, maximo %d)? ", MAX);
                scanf("%d", &qtdMatrizes);
                if (qtdMatrizes >= 2 && qtdMatrizes <= MAX) {
                    qtdInformada = 1;
                    matrizesPreenchidas = 0;
                    printf("Quantidade definida para %d matrizes.\n", qtdMatrizes);
                } else {
                    printf("[ERRO] Escolha entre 2 e %d matrizes.\n", MAX);
                    qtdInformada = 0;
                }
                break;

            case 2:
                if (!qtdInformada) {
                    printf("\n[ERRO] Escolha a quantidade de matrizes primeiro (Opcao 1)!\n");
                } else {
                    for (int m = 0; m < qtdMatrizes; m++) {
                        printf("\n--- Matriz %d ---\n", m + 1);
                        printf("Numero de linhas (max %d): ", MAX);
                        scanf("%d", &linhas[m]);
                        printf("Numero de colunas (max %d): ", MAX);
                        scanf("%d", &colunas[m]);

                        for (int i = 0; i < linhas[m]; i++) {
                            for (int j = 0; j < colunas[m]; j++) {
                                printf("Elemento [%d][%d]: ", i + 1, j + 1);
                                scanf("%d", &matrizes[m][i][j]);
                            }
                        }
                    }
                    matrizesPreenchidas = 1;
                    printf("\nMatrizes registradas com sucesso!\n");
                }
                break;

            case 3:
                if (!matrizesPreenchidas) {
                    printf("\n[ERRO] Informe as matrizes primeiro (Opcao 2)!\n");
                    break;
                }

                printf("\nEscolha a operacao desejada:\n");
                printf("1 -> Soma de matrizes\n");
                printf("2 -> Subtracao de matrizes\n");
                printf("3 -> Multiplicacao de matrizes\n");
                printf("4 -> Subtracao de matrizes\n");
                printf("Opcao: ");
                scanf("%d", &opcaoOperacao);

                // Copia a primeira matriz (indice 0) para o resultado inicial
                linRes = linhas[0];
                colRes = colunas[0];
                for (int i = 0; i < linRes; i++) {
                    for (int j = 0; j < colRes; j++) {
                        resultado[i][j] = matrizes[0][i][j];
                    }
                }

                int erroCalculo = 0;

                for (int m = 1; m < qtdMatrizes; m++) {
                    if (opcaoOperacao == 1) { // SOMA
                        if (linRes != linhas[m] || colRes != colunas[m]) {
                            printf("\n[ERRO] Dimensoes incompativeis para soma na Matriz %d!\n", m + 1);
                            erroCalculo = 1;
                            break;
                        }
                        for (int i = 0; i < linRes; i++) {
                            for (int j = 0; j < colRes; j++) {
                                resultado[i][j] += matrizes[m][i][j];
                            }
                        }
                    } else if (opcaoOperacao == 2 || opcaoOperacao == 4) { // SUBTRACAO
                        if (linRes != linhas[m] || colRes != colunas[m]) {
                            printf("\n[ERRO] Dimensoes incompativeis para subtracao na Matriz %d!\n", m + 1);
                            erroCalculo = 1;
                            break;
                        }
                        for (int i = 0; i < linRes; i++) {
                            for (int j = 0; j < colRes; j++) {
                                resultado[i][j] -= matrizes[m][i][j];
                            }
                        }
                    } else if (opcaoOperacao == 3) { // MULTIPLICACAO
                        if (colRes != linhas[m]) {
                            printf("\n[ERRO] Colunas da anterior (%d) != Linhas da Matriz %d (%d)!\n", colRes, m + 1, linhas[m]);
                            erroCalculo = 1;
                            break;
                        }
                        int temp[MAX][MAX] = {0};
                        for (int i = 0; i < linRes; i++) {
                            for (int j = 0; j < colunas[m]; j++) {
                                for (int k = 0; k < colRes; k++) {
                                    temp[i][j] += resultado[i][k] * matrizes[m][k][j];
                                }
                            }
                        }
                        colRes = colunas[m];
                        for (int i = 0; i < linRes; i++) {
                            for (int j = 0; j < colRes; j++) {
                                resultado[i][j] = temp[i][j];
                            }
                        }
                    } else {
                        printf("\nOpcao de operacao invalida!\n");
                        erroCalculo = 1;
                        break;
                    }
                }

                if (!erroCalculo) {
                    exibirMatriz(resultado, linRes, colRes);
                }
                break;

            case 0:
                printf("\nEncerrando calculadora...\n");
                break;

            default:
                printf("\nOpcao invalida!\n");
        }
    } while (opcaoMenu != 0);

    return 0;
}


