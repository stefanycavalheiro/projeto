import datetime

# Cadastro de Culturas
class Cultura:
    def __init__(self, nome, tipo, dias_para_colheita):
        self.nome = nome
        self.tipo = tipo
        self.dias_para_colheita = dias_para_colheita

    def __str__(self):
        return f"Cultura: {self.nome}, Tipo: {self.tipo}, Dias para Colheita: {self.dias_para_colheita}"

# Função para adicionar culturas
def cadastrar_cultura(nome, tipo, dias_para_colheita):
    cultura = Cultura(nome, tipo, dias_para_colheita)
    return cultura

# Função para calcular a previsão de colheita com base na data de plantio
def calcular_previsao_colheita(data_planto, dias_para_colheita):
    previsao_colheita = data_planto + datetime.timedelta(days=dias_para_colheita)
    return previsao_colheita

# Banco de tipos de solos do Paraná
solos_parana = {
    "argissolos": "Os argissolos são caracterizados pelo acúmulo de argila no Horizonte B. "
                  "São comuns em áreas planas ou inclinadas, podem ser férteis ou pobres e muito suscetíveis à erosão.",
    
    "cambissolos": "Os cambissolos ainda não completaram seu estágio de formação. São rasos e com um horizonte B pouco desenvolvido.",
    
    "chernossolos": "Os chernossolos são solos muito férteis, com alta concentração de matéria orgânica e nutrientes.",
    
    "gleissolos": "Os gleissolos são comuns em regiões costeiras e áreas de planícies fluviais, com coloração acinzentada.",
    
    "latossolos": "Os latossolos são solos profundos, porosos e permeáveis, com grande potencial produtivo, sendo comuns em todo o Brasil.",
    
    "espodossolos": "Espodossolos são solos arenosos, ácidos e com baixa concentração de nutrientes, encontrados em algumas regiões do Brasil.",
    
    "nitossolos": "Os nitossolos são solos profundos e bem drenados, muito férteis e comuns especialmente no Paraná.",
    
    "organossolos": "Os organossolos são formados por materiais orgânicos com alta concentração de carbono, ocorrendo em pequenas áreas.",
    
    "planossolos": "Os planossolos são solos pouco profundos, encontrados em áreas planas e depressões, como no Pantanal.",
    
    "plintossolos": "Os plintossolos possuem plintita, com cores cinzentas, vermelhas e amareladas, encontrados no Norte do Brasil.",
    
    "vertissolos": "Os vertissolos têm alto teor de argila, com rachaduras quando secos, encontrados em áreas de relevo plano e ondulado."
}

# Função para verificar a necessidade de irrigação
def verificar_necessidade_irrigacao(previsao_clima, tipo_solo):
    if previsao_clima == "chuva":
        return "Não é necessário irrigar, previsão de chuva."
    elif tipo_solo == "areia":
        return "Solo arenoso, irrigação necessária."
    elif tipo_solo == "argila":
        return "Solo argiloso, irrigação leve necessária."
    elif tipo_solo == "solo fértil":
        return "Solo fértil, irrigação moderada necessária."
    else:
        return "Solo normal, irrigação moderada necessária."

# Função principal para interagir com o usuário
def main():
    print("Bem-vindo ao Planejamento de Colheitas!")
    print("Estado selecionado: Paraná (PR)")

    # Exibir os tipos de solos para o Paraná
    print("\nTipos de solos para o Paraná:")
    for i, (tipo_solo, descricao) in enumerate(solos_parana.items(), start=1):
        print(f"{i}. {tipo_solo.capitalize()} - {descricao[:80]}...")  # Exibe apenas o início da descrição

    # Perguntar ao usuário qual tipo de solo ele possui
    escolha_solo = int(input("\nEscolha o tipo de solo de sua região (1-10): "))

    # Validar a escolha
    if escolha_solo < 1 or escolha_solo > 10:
        print("Escolha inválida! Considerando o primeiro tipo de solo.")
        tipo_solo_escolhido = list(solos_parana.keys())[0]
    else:
        tipo_solo_escolhido = list(solos_parana.keys())[escolha_solo - 1]

    descricao_solo = solos_parana[tipo_solo_escolhido]
    print(f"\nVocê escolheu o solo: {tipo_solo_escolhido.capitalize()}")
    print(f"Descrição do solo: {descricao_solo}")

    # Perguntar sobre a cultura
    cultura_nome = input("\nEscolha a cultura (Milho, Soja, Trigo): ").capitalize()

    if cultura_nome == "Milho":
        # Perguntar se o milho é para Silagem ou para Grão
        tipo_milho = input("Escolha o tipo de milho (Silagem ou Grão): ").capitalize()
        if tipo_milho == "Silagem":
            dias_para_colheita = 90  # Milho para Silagem
        elif tipo_milho == "Grão":
            dias_para_colheita = 120  # Milho para Grão
        else:
            print("Tipo de milho inválido. Usando milho para grão por padrão.")
            tipo_milho = "Grão"
            dias_para_colheita = 120
    else:
        tipo_milho = None
        if cultura_nome == "Soja":
            dias_para_colheita = 150
        else:
            dias_para_colheita = 90  # Para o Trigo

    # Perguntar sobre a data do plantio (formato dd/mm/aaaa)
    data_planto_input = input("Informe a data do plantio (dd/mm/aaaa): ")
    data_planto = datetime.datetime.strptime(data_planto_input, "%d/%m/%Y").date()

    # Perguntar sobre a previsão climática
    previsao_clima = input("Escolha a previsão climática (chuva, ensolarado, nublado): ").lower()

    # Cadastrar a cultura
    if tipo_milho:
        cultura = cadastrar_cultura(f"Milho ({tipo_milho})", "Cereal", dias_para_colheita)
    else:
        cultura = cadastrar_cultura(cultura_nome, "Cereal", dias_para_colheita)

    print(f"\nCultura cadastrada: {cultura}")

    # Calcular a previsão de colheita
    previsao_colheita = calcular_previsao_colheita(data_planto, dias_para_colheita)
    print(f"\nData de plantio: {data_planto}")
    print(f"Previsão de colheita: {previsao_colheita}")

    # Verificar a necessidade de irrigação
    irrigacao = verificar_necessidade_irrigacao(previsao_clima, tipo_solo_escolhido)
    print(f"\nNecessidade de Irrigação: {irrigacao}")

# Chama a função principal
if __name__ == "__main__":
    main()
