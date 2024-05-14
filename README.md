def menu():
    return """
    [d] Depositar
    [s] Sacar
    [e] Extrato
    [q] Sair

    => """

def depositar(saldo, extrato):
    try:
        valor = float(input("Informe o valor do depósito: "))
        if valor > 0:
            saldo += valor
            extrato.append(f"Depósito: R$ {valor:.2f}")
            print(f"Depósito de R$ {valor:.2f} realizado com sucesso.")
        else:
            print("Operação falhou! O valor informado é inválido.")
    except ValueError:
        print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato

def sacar(saldo, extrato, numero_saques, LIMITE_SAQUES, limite):
    try:
        valor = float(input("Informe o valor do saque: "))
        excedeu_saldo = valor > saldo
        excedeu_limite = valor > limite
        excedeu_saques = numero_saques >= LIMITE_SAQUES

        if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")
        elif excedeu_limite:
            print("Operação falhou! O valor do saque excede o limite. O limite é de R$ 500,00 por saque.")
        elif excedeu_saques:
            print("Operação falhou! Número máximo de saques excedido. O limite diário é de 3 saques, totalizando R$ 1500,00.")
        elif valor > 0:
            saldo -= valor
            extrato.append(f"Saque: R$ {valor:.2f}")
            numero_saques += 1
            print(f"Saque de R$ {valor:.2f} realizado com sucesso.")
        else:
            print("Operação falhou! O valor informado é inválido.")
    except ValueError:
        print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato, numero_saques

def exibir_extrato(saldo, extrato):
    print("\n================ EXTRATO ================")
    if not extrato:
        print("Não foram realizadas movimentações.")
    else:
        for operacao in extrato:
            print(operacao)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")

def main():
    saldo = 0.0
    limite = 500.0
    extrato = []
    numero_saques = 0
    LIMITE_SAQUES = 3

    while True:
        opcao = input(menu())

        if opcao == "d":
            saldo, extrato = depositar(saldo, extrato)
        elif opcao == "s":
            saldo, extrato, numero_saques = sacar(saldo, extrato, numero_saques, LIMITE_SAQUES, limite)
        elif opcao == "e":
            exibir_extrato(saldo, extrato)
        elif opcao == "q":
            print("Obrigado por utilizar o sistema bancário. Até logo!")
            break
        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")

if __name__ == "__main__":
    main()
