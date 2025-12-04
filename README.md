PYCHARM 


import pyodbc

# Conexão com SQL Server
conn = pyodbc.connect(
    'DRIVER={SQL Server};'
    ' ............ ;'  # ajuste conforme sua instância
    'DATABASE=IMC_DB;'
    'Trusted_Connection=yes;'
)

cursor = conn.cursor()

# Função para calcular IMC
def calcular_imc(peso, altura):
    imc = peso / (altura ** 2)
    if imc < 18.5:
        classificacao = "Abaixo do peso"
    elif imc < 25:
        classificacao = "Peso normal"
    elif imc < 30:
        classificacao = "Sobrepeso"
    else:
        classificacao = "Obesidade"
    return imc, classificacao

# Inserir paciente
def inserir_paciente(nome, peso, altura):
    imc, classificacao = calcular_imc(peso, altura)
    cursor.execute("INSERT INTO pacientes (nome, peso, altura, imc, classificacao) VALUES (?, ?, ?, ?, ?)",
                   (nome, peso, altura, imc, classificacao))
    conn.commit()
    print(f"Paciente {nome} cadastrado com IMC {imc:.2f} ({classificacao})")

# Listar pacientes
def listar_pacientes():
    cursor.execute("SELECT * FROM pacientes")
    for paciente in cursor.fetchall():
        print(paciente)

# Menu interativo
if __name__ == "__main__":
    while True:
        print("\n--- Sistema de IMC ---")
        print("1. Inserir paciente")
        print("2. Listar pacientes")
        print("3. Sair")
        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            nome = input("Nome: ")
            peso = float(input("Peso (kg): "))
            altura = float(input("Altura (m): "))
            inserir_paciente(nome, peso, altura)
        elif opcao == "2":
            listar_pacientes()
        elif opcao == "3":
            break
        else:
            print("Opção inválida!")







            SQL SERVER 


            CREATE DATABASE IMC_DB;
GO

USE IMC_DB;
GO

CREATE TABLE pacientes (
    id INT PRIMARY KEY IDENTITY(1,1),
    nome NVARCHAR(100),
    peso FLOAT,
    altura FLOAT,
    imc FLOAT,
    classificacao NVARCHAR(50)
);
