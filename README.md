# Projeto-de-Conclus-o-Semestral

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PACIENTES 100

typedef struct {
    int id;
    char nome[50];
    int idade;
    char diagnostico[100];
    float valorDiaria;
    char status[20];
} Paciente;

Paciente pacientulus[MAX_PACIENTES];
int qntP = 0;

void carregar(){
    FILE *fp = fopen("pacientes.txt", "r");

    qntP = 0;

    if(fp == NULL){
        return;
    }

    while(qntP < MAX_PACIENTES && fscanf(fp, "%d/%49[^/]/%d/%99[^/]/%f/%19[^\n]\n",
        &pacientulus[qntP].id,
        pacientulus[qntP].nome,
        &pacientulus[qntP].idade,
        pacientulus[qntP].diagnostico,
        &pacientulus[qntP].valorDiaria,
        pacientulus[qntP].status) == 6){
        	qntP++;
    }

    fclose(fp);
}

void salvar(){
    FILE *fp = fopen("pacientes.txt", "w");

    if(fp == NULL){
        printf("Erro ao abrir o arquivo.\n");
        return;
    }

    for(int i = 0; i < qntP; i++){
        fprintf(fp, "%d/%s/%d/%s/%.2f/%s\n",
            pacientulus[i].id,
            pacientulus[i].nome,
            pacientulus[i].idade,
            pacientulus[i].diagnostico,
            pacientulus[i].valorDiaria,
            pacientulus[i].status);
    }

    fclose(fp);
}

void mostrarPaciente(Paciente p){
    printf("\n========================\n");
    printf("ID: %d\n", p.id);
    printf("Nome: %s\n", p.nome);
    printf("Idade: %d\n", p.idade);
    printf("Diagnostico: %s\n", p.diagnostico);
    printf("Valor da diaria: %.2f\n", p.valorDiaria);
    printf("Status: %s\n", p.status);
}

void cadastrar(){
    carregar();

    if(qntP >= MAX_PACIENTES){
        printf("\nLimite de pacientes atingido!\n");
        return;
    }

    printf("\n=== CADASTRO DE PACIENTE ===\n");

    printf("Digite o ID do paciente:\n");
    scanf("%d", &pacientulus[qntP].id);
    getchar();

    printf("Digite o nome do paciente:\n");
    fgets(pacientulus[qntP].nome, 50, stdin);
    pacientulus[qntP].nome[strcspn(pacientulus[qntP].nome, "\n")] = '\0';

    printf("Digite a idade do paciente:\n");
    scanf("%d", &pacientulus[qntP].idade);
    getchar();

    printf("Digite o diagnostico do paciente:\n");
    fgets(pacientulus[qntP].diagnostico, 100, stdin);
    pacientulus[qntP].diagnostico[strcspn(pacientulus[qntP].diagnostico, "\n")] = '\0';

    printf("Digite o valor da diaria do paciente:\n");
    scanf("%f", &pacientulus[qntP].valorDiaria);
    getchar();

    printf("Digite o status do paciente (Internado/Alta):\n");
    fgets(pacientulus[qntP].status, 20, stdin);
    pacientulus[qntP].status[strcspn(pacientulus[qntP].status, "\n")] = '\0';

    qntP++;

    salvar();

    printf("\nPaciente cadastrado com sucesso!\n");
}

void listarTodos(){
    carregar();

    if(qntP == 0){
        printf("\nNenhum paciente cadastrado.\n");
        return;
    }

    printf("\n=== LISTA DE PACIENTES ===\n");

    for(int i = 0; i < qntP; i++){
        mostrarPaciente(pacientulus[i]);
    }
}

void listarInternados(){
    carregar();

    int internados = 0;

    for(int i = 0; i < qntP; i++){
        if(strcmp(pacientulus[i].status, "Internado") == 0){
            mostrarPaciente(pacientulus[i]);
            internados++;
        }
    }

    if(internados == 0){
        printf("\nNenhum paciente internado no momento.\n");
    }
}

void atualizarPacienteCompleto(){
    carregar();

    int id;
    int achou = 0;

    if(qntP == 0){
        printf("\nNenhum paciente cadastrado para atualizar.\n");
        return;
    }

    printf("Digite o ID do paciente que deseja atualizar:\n");
    scanf("%d", &id);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(pacientulus[i].id == id){
            achou = 1;

            printf("Digite o novo nome do paciente:\n");
            fgets(pacientulus[i].nome, 50, stdin);
            pacientulus[i].nome[strcspn(pacientulus[i].nome, "\n")] = '\0';

            printf("Digite a nova idade do paciente:\n");
            scanf("%d", &pacientulus[i].idade);
            getchar();

            printf("Digite o novo diagnostico do paciente:\n");
            fgets(pacientulus[i].diagnostico, 100, stdin);
            pacientulus[i].diagnostico[strcspn(pacientulus[i].diagnostico, "\n")] = '\0';

            printf("Digite o novo valor da diaria do paciente:\n");
            scanf("%f", &pacientulus[i].valorDiaria);
            getchar();

            printf("Digite o novo status do paciente (Internado/Alta):\n");
            fgets(pacientulus[i].status, 20, stdin);
            pacientulus[i].status[strcspn(pacientulus[i].status, "\n")] = '\0';

            salvar();

            printf("\nPaciente atualizado com sucesso!\n");
            break;
        }
    }

    if(achou == 0){
        printf("\nPaciente com ID %d nao encontrado.\n", id);
    }
}

void atualizarProntuario(){
    carregar();

    int idProcurado;
    int achou = 0;

    printf("Informe o ID do paciente:\n");
    scanf("%d", &idProcurado);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(pacientulus[i].id == idProcurado){
            achou = 1;

            printf("Digite o novo diagnostico:\n");
            fgets(pacientulus[i].diagnostico, 100, stdin);
            pacientulus[i].diagnostico[strcspn(pacientulus[i].diagnostico, "\n")] = '\0';

            salvar();

            printf("\nProntuario atualizado com sucesso!\n");
            break;
        }
    }

    if(achou == 0){
        printf("\nPaciente nao encontrado.\n");
    }
}

void deletar(){
    carregar();

    int id;
    int achou = 0;

    if(qntP == 0){
        printf("\nNenhum paciente cadastrado para deletar.\n");
        return;
    }

    printf("Digite o ID do paciente que deseja deletar:\n");
    scanf("%d", &id);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(pacientulus[i].id == id){
            achou = 1;

            for(int j = i; j < qntP - 1; j++){
                pacientulus[j] = pacientulus[j + 1];
            }

            qntP--;

            salvar();

            printf("\nPaciente com ID %d deletado com sucesso.\n", id);
            break;
        }
    }

    if(achou == 0){
        printf("\nPaciente com ID %d nao encontrado.\n", id);
    }
}

void buscarPorId(){
    carregar();

    int idBusca;
    int encontrou = 0;

    printf("\nDigite o ID do paciente: ");
    scanf("%d", &idBusca);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(pacientulus[i].id == idBusca){
            printf("\nPACIENTE ENCONTRADO\n");
            mostrarPaciente(pacientulus[i]);
            encontrou = 1;
        }
    }

    if(encontrou == 0){
        printf("\nPaciente nao encontrado.\n");
    }
}

void buscarPorNome(){
    carregar();

    char nomeBusca[50];
    int encontrou = 0;

    printf("\nDigite o nome do paciente: ");
    scanf(" %[^\n]", nomeBusca);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(strcmp(pacientulus[i].nome, nomeBusca) == 0){
            printf("\nPACIENTE ENCONTRADO\n");
            mostrarPaciente(pacientulus[i]);
            encontrou = 1;
        }
    }

    if(encontrou == 0){
        printf("\nPaciente nao encontrado.\n");
    }
}

void filtrarPorIdade(){
    carregar();
    
    int idadeMin;
    int idadeMax;
    int encontrou = 0;

    printf("\nDigite a idade minima: ");
    scanf("%d", &idadeMin);

    printf("Digite a idade maxima: ");
    scanf("%d", &idadeMax);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(pacientulus[i].idade >= idadeMin && pacientulus[i].idade <= idadeMax){
            mostrarPaciente(pacientulus[i]);
            encontrou = 1;
        }
    }

    if(encontrou == 0){
        printf("\nNenhum paciente encontrado.\n");
    }
}

void filtrarPorDiagnostico(){
    carregar();

    char diagnosticoBusca[100];
    int encontrou = 0;

    printf("\nDigite o diagnostico: ");
    scanf(" %[^\n]", diagnosticoBusca);
    getchar();

    for(int i = 0; i < qntP; i++){
        if(strcmp(pacientulus[i].diagnostico, diagnosticoBusca) == 0){
            mostrarPaciente(pacientulus[i]);
            encontrou = 1;
        }
    }

    if(encontrou == 0){
        printf("\nNenhum paciente encontrado.\n");
    }
}

void filtrarInternados(){
    carregar();

    int encontrou = 0;

    for(int i = 0; i < qntP; i++){
        if(strcmp(pacientulus[i].status, "Internado") == 0){
            mostrarPaciente(pacientulus[i]);
            encontrou = 1;
        }
    }

    if(encontrou == 0){
        printf("\nNenhum paciente internado.\n");
    }
}

void ordenarPorIdade(){
    carregar();

    Paciente temp;

    if(qntP == 0){
        printf("\nNenhum paciente cadastrado para ordenar.\n");
        return;
    }

    for(int i = 0; i < qntP - 1; i++){
        for(int j = 0; j < qntP - i - 1; j++){
            if(pacientulus[j].idade > pacientulus[j + 1].idade){
                temp = pacientulus[j];
                pacientulus[j] = pacientulus[j + 1];
                pacientulus[j + 1] = temp;
            }
        }
    }

    salvar();

    printf("\nPacientes ordenados por idade com sucesso!\n");
}

void ordenarPorNome(){
    carregar();

    Paciente temp;

    if(qntP == 0){
        printf("\nNenhum paciente cadastrado para ordenar.\n");
        return;
    }

    for(int i = 0; i < qntP - 1; i++){
        for(int j = 0; j < qntP - i - 1; j++){
            if(strcmp(pacientulus[j].nome, pacientulus[j + 1].nome) > 0){
                temp = pacientulus[j];
                pacientulus[j] = pacientulus[j + 1];
                pacientulus[j + 1] = temp;
            }
        }
    }

    salvar();

    printf("\nPacientes ordenados por nome com sucesso!\n");
}

void menuBusca(){
    int opcao;

    do{
        printf("\n===== MENU DE BUSCA =====\n");
        printf("1 - Buscar por ID\n");
        printf("2 - Buscar por nome\n");
        printf("0 - Voltar\n");
        printf("Opcao: ");
        scanf("%d", &opcao);
        getchar();

        switch(opcao){
            case 1:
                buscarPorId();
                break;

            case 2:
                buscarPorNome();
                break;

            case 0:
                printf("\nVoltando ao menu principal...\n");
                break;

            default:
                printf("\nOpcao invalida!\n");
        }

    }while(opcao != 0);
}

void menuFiltro(){
    int opcao;

    do{
        printf("\n===== MENU DE FILTROS =====\n");
        printf("1 - Filtrar por idade\n");
        printf("2 - Filtrar por diagnostico\n");
        printf("3 - Filtrar internados\n");
        printf("0 - Voltar\n");
        printf("Opcao: ");
        scanf("%d", &opcao);
        getchar();

        switch(opcao){
            case 1:
                filtrarPorIdade();
                break;

            case 2:
                filtrarPorDiagnostico();
                break;

            case 3:
                filtrarInternados();
                break;

            case 0:
                printf("\nVoltando ao menu principal...\n");
                break;

            default:
                printf("\nOpcao invalida!\n");
        }

    }while(opcao != 0);
}

void menuOrdenacao(){
    int opcao;

    do{
        printf("\n===== MENU DE ORDENACAO =====\n");
        printf("1 - Ordenar por idade crescente\n");
        printf("2 - Ordenar por nome\n");
        printf("0 - Voltar\n");
        printf("Opcao: ");
        scanf("%d", &opcao);
        getchar();

        switch(opcao){
            case 1:
                ordenarPorIdade();
                break;

            case 2:
                ordenarPorNome();
                break;

            case 0:
                printf("\nVoltando ao menu principal...\n");
                break;

            default:
                printf("\nOpcao invalida!\n");
        }

    }while(opcao != 0);
}

int main(){
    int opcao;

    do{
        printf("\n===== SISTEMA DE ANALISE HOSPITALAR =====\n");
        printf("1 - Cadastrar paciente\n");
        printf("2 - Listar todos os pacientes\n");
        printf("3 - Listar pacientes internados\n");
        printf("4 - Buscar pacientes\n");
        printf("5 - Filtrar pacientes\n");
        printf("6 - Ordenar registros\n");
        printf("7 - Atualizar paciente completo\n");
        printf("8 - Atualizar prontuario\n");
        printf("9 - Dar alta / remover paciente\n");
        printf("0 - Sair\n");
        printf("Opcao: ");
        scanf("%d", &opcao);
        getchar();

        switch(opcao){
            case 1:
                cadastrar();
                break;

            case 2:
                listarTodos();
                break;

            case 3:
                listarInternados();
                break;

            case 4:
                menuBusca();
                break;

            case 5:
                menuFiltro();
                break;

            case 6:
                menuOrdenacao();
                break;

            case 7:
                atualizarPacienteCompleto();
                break;

            case 8:
                atualizarProntuario();
                break;

            case 9:
                deletar();
                break;

            case 0:
                printf("\nEncerrando sistema...\n");
                break;

            default:
                printf("\nOpcao invalida!\n");
        }

    }while(opcao != 0);

    return 0;
}
