#include <stdio.h>
#include <string.h>

#define MAX_RECEITAS 100

// Enumeração para categorias de receitas
typedef enum {
    ENTRADA,
    PRATO_PRINCIPAL,
    SOBREMESA,
    BEBIDA,
    LANCHE,
    OUTRO
} CategoriaReceita;

// Função para converter enum para string para exibir
const char* categoriaToString(CategoriaReceita c) {
    switch (c) {
        case ENTRADA: return "Entrada";
        case PRATO_PRINCIPAL: return "Prato Principal";
        case SOBREMESA: return "Sobremesa";
        case BEBIDA: return "Bebida";
        case LANCHE: return "Lanche";
        case OUTRO: return "Outro";
        default: return "Desconhecido";
    }
}

// Estrutura para armazenar receita
typedef struct {
    char nome[100];
    char ingredientes[500];
    CategoriaReceita categoria;
} Receita;

// Vetor para armazenar receitas
Receita receitas[MAX_RECEITAS];
int quantidadeReceitas = 0;

// Função para ler uma string com espaços do teclado
void lerString(char* buffer, int tamanho) {
    fgets(buffer, tamanho, stdin);
    // Remove o '\n' do final se existir
    size_t len = strlen(buffer);
    if (len > 0 && buffer[len - 1] == '\n')
        buffer[len - 1] = '\0';
}

// Função para adicionar receita
void adicionarReceita() {
    if (quantidadeReceitas >= MAX_RECEITAS) {
        printf("Limite de receitas atingido!\n");
        return;
    }

    Receita r;

    printf("Digite o nome da receita: ");
    lerString(r.nome, 100);

    printf("Digite os ingredientes (sempre com virgula): ");
    lerString(r.ingredientes, 500);

    printf("Escolha a categoria:\n");
    printf("0 - Entrada\n1 - Prato Principal\n2 - Sobremesa\n3 - Bebida\n4 - Lanche\n5 - Outro\n");
    int cat;
    scanf("%d", &cat);
    getchar(); // limpar o '\n' do buffer
    if (cat < 0 || cat > 5) {
        printf("Categoria inválida, definindo como Outro.\n");
        cat = OUTRO;
    }
    r.categoria = (CategoriaReceita)cat;

    receitas[quantidadeReceitas++] = r;
    printf("Receita adicionada com sucesso!\n");
}

// Função para listar todas as receitas
void listarReceitas() {
    if (quantidadeReceitas == 0) {
        printf("Nenhuma receita cadastrada.\n");
        return;
    }
    for (int i = 0; i < quantidadeReceitas; i++) {
        printf("\nReceita %d:\n", i + 1);
        printf("Nome: %s\n", receitas[i].nome);
        printf("Ingredientes: %s\n", receitas[i].ingredientes);
        printf("Categoria: %s\n", categoriaToString(receitas[i].categoria));
    }
}

// Função para listar receitas por categoria
void listarPorCategoria() {
    printf("Escolha a categoria para filtrar:\n");
    printf("0 - Entrada\n1 - Prato Principal\n2 - Sobremesa\n3 - Bebida\n4 - Lanche\n5 - Outro\n");
    int cat;
    scanf("%d", &cat);
    getchar(); // limpar o '\n' do buffer
    if (cat < 0 || cat > 5) {
        printf("Categoria inválida.\n");
        return;
    }
    CategoriaReceita c = (CategoriaReceita)cat;
    int encontrou = 0;
    for (int i = 0; i < quantidadeReceitas; i++) {
        if (receitas[i].categoria == c) {
            printf("\nReceita %d:\n", i + 1);
            printf("Nome: %s\n", receitas[i].nome);
            printf("Ingredientes: %s\n", receitas[i].ingredientes);
            encontrou = 1;
        }
    }
    if (!encontrou)
        printf("Nenhuma receita encontrada nesta categoria.\n");
}

int main() {
    int opcao;

    do {
        printf("\n--- Livro de Receitas Digital ---\n");
        printf("1 - Adicionar Receita\n");
        printf("2 - Listar Todas as Receitas\n");
        printf("3 - Listar Receitas por Categoria\n");
        printf("0 - Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        getchar(); // limpar o '\n' do buffer

        switch (opcao) {
            case 1:
                adicionarReceita();
                break;
            case 2:
                listarReceitas();
                break;
            case 3:
                listarPorCategoria();
                break;
            case 0:
                printf("Saindo do programa...\n");
                break;
            default:
                printf("opçao invalida, tente novamente.\n");
        }
    } while (opcao != 0);

    return 0;
}
