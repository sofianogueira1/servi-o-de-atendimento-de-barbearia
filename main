/*
Turma: 10B - 8h - Professor Gustavo
Membros: Miguel Tolentino Rabelo, Sofia Gischewski Costa Nogueira, Gustavo da Frota Roiffé
*/


#include <iostream>
#include <string>
#include <iomanip>
#include <fstream>
#include <limits>

using namespace std;

struct Atendimento {
    string tipo_de_servico;
    int data;
    int hora;
    string cliente;
    string unidade;
    string profissional;
    double valor;
};

void aumentar_memoria(Atendimento *&atendimentos, int &capacidade) {

    Atendimento *novo = new Atendimento[capacidade * 2];

    for (int i = 0; i < capacidade; i++) {
        novo[i] = atendimentos[i];
    }

    delete[] atendimentos;

    atendimentos = novo;
    capacidade = capacidade * 2;
}

void ordenar(Atendimento *atendimentos, int quantidade) {

    for (int i = 0; i < quantidade - 1; i++) {
        for (int j = 0; j < quantidade - 1 - i; j++) {

            if (atendimentos[j].tipo_de_servico > atendimentos[j + 1].tipo_de_servico) {

                Atendimento auxiliar = atendimentos[j];
                atendimentos[j] = atendimentos[j + 1];
                atendimentos[j + 1] = auxiliar;
            }
        }
    }
}

int busca_binaria(Atendimento *atendimentos, int inicio, int fim, string servico) {

    if (inicio > fim) {
        return -1;
    }

    int meio = (inicio + fim) / 2;

    if (atendimentos[meio].tipo_de_servico == servico) {
        return meio;
    }

    if (servico < atendimentos[meio].tipo_de_servico) {
        return busca_binaria(atendimentos, inicio, meio - 1, servico);
    }

    return busca_binaria(atendimentos, meio + 1, fim, servico);
}

bool data_valida(int data) {

    int dia = data / 1000000;
    int mes = (data / 10000) % 100;
    int ano = data % 10000;

    if (ano < 1 or mes < 1 or mes > 12 or dia < 1) {
        return false;
    }

    int dias_mes = 31;

    if (mes == 4 or mes == 6 or mes == 9 or mes == 11) {
        dias_mes = 30;
    }

    if (mes == 2) {
        dias_mes = 28;

        if (ano % 400 == 0 or (ano % 4 == 0 and ano % 100 != 0)) {
            dias_mes = 29;
        }
    }

    if (dia > dias_mes) {
        return false;
    }

    return true;
}

void cadastrar_atendimento(Atendimento &atendimento) {

    cout << "Digite o tipo de servico: ";
    getline(cin, atendimento.tipo_de_servico);

    cout << "Digite a data (DDMMAAAA): ";
    cin >> atendimento.data;

    while (!data_valida(atendimento.data)) {

        cout << "Data invalida!" << endl;
        cout << "Digite a data (DDMMAAAA): ";
        cin >> atendimento.data;
    }

    cout << "Digite a hora (HHMM): ";
    cin >> atendimento.hora;

    while (atendimento.hora < 0 or
           atendimento.hora > 2359 or
           atendimento.hora / 100 > 23 or
           atendimento.hora % 100 > 59) {

        cout << "Hora invalida!" << endl;
        cout << "Digite a hora (HHMM): ";
        cin >> atendimento.hora;
    }

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "Digite o cliente: ";
    getline(cin, atendimento.cliente);

    cout << "Digite a unidade: ";
    getline(cin, atendimento.unidade);

    cout << "Digite o profissional: ";
    getline(cin, atendimento.profissional);

    cout << "Digite o valor: R$ ";
    cin >> atendimento.valor;

    while (atendimento.valor <= 0) {

        cout << "Valor invalido!" << endl;
        cout << "Digite o valor: R$ ";
        cin >> atendimento.valor;
    }

    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    cout << "Atendimento cadastrado com sucesso!" << endl;
}

void mostrar_atendimentos(Atendimento *atendimentos, int quantidade, int i) {

    if (i >= quantidade) {
        return;
    }

    cout << "------------------------" << endl;
    cout << "       ATENDIMENTO      " << endl;
    cout << "------------------------" << endl;

    cout << "Tipo de servico: "
         << atendimentos[i].tipo_de_servico << endl;

    cout << "Data: "
         << setw(2) << setfill('0')
         << atendimentos[i].data / 1000000 << "/"
         << setw(2) << setfill('0')
         << (atendimentos[i].data / 10000) % 100 << "/"
         << setw(4) << setfill('0')
         << atendimentos[i].data % 10000 << endl;

    cout << "Hora: "
         << setw(2) << setfill('0')
         << atendimentos[i].hora / 100 << ":"
         << setw(2) << setfill('0')
         << atendimentos[i].hora % 100 << endl;

    cout << setfill(' ');

    cout << "Cliente: " << atendimentos[i].cliente << endl;
    cout << "Unidade: " << atendimentos[i].unidade << endl;
    cout << "Profissional: " << atendimentos[i].profissional << endl;

    cout << "Valor: R$ " << fixed << setprecision(2)
         << atendimentos[i].valor << endl;

    mostrar_atendimentos(atendimentos, quantidade, i + 1);
}

void carregar_arquivo(Atendimento *&atendimentos, int &quantidade, int &capacidade) {

    ifstream leitura("atendimentos.txt");

    if (!leitura.is_open()) {
        return;
    }

    while (true) {

        if (quantidade == capacidade) {
            aumentar_memoria(atendimentos, capacidade);
        }

        if (!getline(leitura, atendimentos[quantidade].tipo_de_servico, ';')) {
            break;
        }

        leitura >> atendimentos[quantidade].data;
        leitura.ignore(1);

        leitura >> atendimentos[quantidade].hora;
        leitura.ignore(1);

        getline(leitura, atendimentos[quantidade].cliente, ';');
        getline(leitura, atendimentos[quantidade].unidade, ';');
        getline(leitura, atendimentos[quantidade].profissional, ';');

        leitura >> atendimentos[quantidade].valor;
        leitura.ignore(numeric_limits<streamsize>::max(), '\n');

        if (leitura.fail()) {
            break;
        }

        quantidade++;
    }

    leitura.close();
}

void salvar_arquivo(Atendimento *atendimentos, int quantidade) {

    ofstream arquivo("atendimentos.txt");

    if (!arquivo.is_open()) {
        cout << "Erro ao salvar o arquivo!" << endl;
        return;
    }

    for (int i = 0; i < quantidade; i++) {

        arquivo << atendimentos[i].tipo_de_servico << ";"
                << atendimentos[i].data << ";"
                << atendimentos[i].hora << ";"
                << atendimentos[i].cliente << ";"
                << atendimentos[i].unidade << ";"
                << atendimentos[i].profissional << ";"
                << atendimentos[i].valor << endl;
    }

    arquivo.close();

    cout << "Dados salvos no arquivo!" << endl;
}

int main() {

    int capacidade = 10;
    int quantidade = 0;
    int opcao;

    Atendimento *atendimentos = new Atendimento[capacidade];

    carregar_arquivo(atendimentos, quantidade, capacidade);

    do {

        cout << endl;
        cout << "========= MENU =========" << endl;
        cout << "1 - Cadastrar atendimento" << endl;
        cout << "2 - Listar atendimentos" << endl;
        cout << "3 - Buscar atendimento" << endl;
        cout << "4 - Salvar e sair" << endl;
        cout << "Escolha uma opcao: ";

        if (!(cin >> opcao)) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Opcao invalida!" << endl;
            continue;
        }

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        switch (opcao) {

            case 1:

                if (quantidade == capacidade) {
                    aumentar_memoria(atendimentos, capacidade);
                }

                cadastrar_atendimento(atendimentos[quantidade]);
                quantidade++;

                break;

            case 2:

                if (quantidade == 0) {
                    cout << "Nenhum atendimento cadastrado!" << endl;
                }
                else {
                    ordenar(atendimentos, quantidade);

                    cout << endl << "TODOS OS ATENDIMENTOS" << endl;

                    mostrar_atendimentos(atendimentos, quantidade, 0);
                }

                break;

            case 3: {

                if (quantidade == 0) {
                    cout << "Nenhum atendimento cadastrado!" << endl;
                    break;
                }

                ordenar(atendimentos, quantidade);

                string servico;

                cout << "Digite o tipo de servico que deseja buscar: ";
                getline(cin, servico);

                int posicao = busca_binaria( atendimentos, 0, quantidade - 1, servico  );

                if (posicao == -1) {
                    cout << "Servico nao encontrado!" << endl;
                }
                else {
                    cout << "Servico encontrado!" << endl;
                    cout << "Cliente: "
                         << atendimentos[posicao].cliente << endl;
                    cout << "Profissional: "
                         << atendimentos[posicao].profissional << endl;
                    cout << "Valor: R$ " << fixed << setprecision(2)
                         << atendimentos[posicao].valor << endl;
                }

                break;
            }

            case 4:

                ordenar(atendimentos, quantidade);
                salvar_arquivo(atendimentos, quantidade);

                cout << "Encerrando o programa..." << endl;

                break;

            default:

                cout << "Opcao invalida! Escolha de 1 a 4." << endl;
        }

    } while (opcao != 4);

    delete[] atendimentos;

    return 0;
}
