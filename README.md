# formulario_pesquisadeconsumo_analgesicosparamenstruacao
Formulário de pesquisa desenvolvido em Python como forma de entrevistar e armazenar dados do público feminino acerca de compra e consumo de analgésicos. Com tal sistema é possível mapear e estudar novas estratégias de marketing de vendas para o público feminino pois será coletados dados já filtrados.

No próprio código já há filtragem para garantir que a entrevistada preencha esses requisistos:
#1)Maior de 10 anos pois é a idade padrão de menstruação
#2)Alguém que já vivenciou a menstruação pelo menos uma vez
#3)Alguém que necessite de analgésicos e vivencie os sinais e sintomas tratados pelos medicamentos citados
----------------------------------------------------------------------------------------------------------------

# imports
import customtkinter as ctk

# criar janela e mainloop
janela = ctk.CTk()

janela.geometry('600x1000')
janela.title('PESQUISA PARA MULHERES')


menu_label = ctk.CTkLabel(janela,text="BEM VINDA(O),O QUE VOCÊ DESEJA FAZER HOJE?\n" \
"INSIRA OS DADOS PARA RESPONDER À PESQUISA\n"
"SE DESEJA CONSULTAR O HISTÓRICO,DIGITE A SENHA DOS FUNCIONÁRIOS")
menu_label.pack(pady=5)


# sobre a coleta do nome
label_nome = ctk.CTkLabel(janela,text="Digite o seu nome")
label_nome.pack()

campo_nome = ctk.CTkEntry(janela)
campo_nome.pack(pady = 5)

# sobre a idade
label_idade = ctk.CTkLabel(janela,text="Digite a sua idade")
label_idade.pack()

campo_idade = ctk.CTkEntry(janela)
campo_idade.pack(pady=5)


# Já menstrua?
menstrua_label = ctk.CTkLabel(janela,text="Você já menstrua?")
menstrua_label.pack()

opcao_menstruacao = ctk.CTkOptionMenu(janela,values = ["SIM","NÃO"])
opcao_menstruacao.pack()

# Menarca
label_menarca = ctk.CTkLabel(janela, text="Com quantos anos ocorreu a menarca?")
label_menarca.pack()

campo_menarca = ctk.CTkEntry(janela)
campo_menarca.pack(pady=5)

# Estado
label_estado = ctk.CTkLabel(janela,text="Sigla do estado:")
label_estado.pack()

campo_estado = ctk.CTkEntry(janela)
campo_estado.pack(pady=5)



# colicas e analgesicos
label_colica = ctk.CTkLabel(janela,text="Você sente cólicas menstruais?")
label_colica.pack()

opcao_colica = ctk.CTkOptionMenu(janela,values = ["SIM","NÃO"])
opcao_colica.pack(pady=5)

label_analgesico = ctk.CTkLabel(janela,text="Você utiliza medicamentos para alívio das cólicas menstruais?")
label_analgesico.pack()

campo_analgesico = ctk.CTkOptionMenu(janela,values=["SIM","NÃO"])
campo_analgesico.pack(pady=5)

label_digitanal = ctk.CTkLabel(janela,text="Digite o nome do medicamento que você utiliza:")
label_digitanal.pack()

campo_digitanal = ctk.CTkEntry(janela)
campo_digitanal.pack(pady=5)

label_recomendacao = ctk.CTkLabel(janela,text="Qual destes analgésicos você recomenda?\n"
                                  "ADVIL\n"
                                  "ALIVUM\n" 
                                  "ATROFEM\n"
                                  "DIGITE O NOME:")
label_recomendacao.pack()

campo_recomendacao = ctk.CTkEntry(janela)
campo_recomendacao.pack(pady=5)


# senha
senha_label = ctk.CTkLabel(janela,text="INSIRA A SENHA DOS FUNCIONÁRIOS")
senha_label.pack()

campo_senha = ctk.CTkEntry(janela)
campo_senha.pack(pady=5)

# resultados
label_resultado = ctk.CTkLabel(janela, text="")
label_resultado.pack(pady=10)


# variaveis globais
respostas =[]

# funcoes

def dados_pessoais():
   
    nome = campo_nome.get()
    idade_texto = campo_idade.get().strip()
    if not idade_texto.isdigit():
        label_resultado.configure(text="digite uma idade válida")
        return False
    idade = int((idade_texto))
    if idade <= 10:
        label_resultado.configure(text="Desculpe,mas você não faz parte do perfil que estamos procurando")
        return False
    prim_ment = opcao_menstruacao.get().upper()
    if prim_ment in ["NÃO","NAO"]:
        label_resultado.configure(text="Precisamos da resposta de pessoas que vivenciaram a menstruação,conversaremos novamente quando você vivenciar isso,certo?")
        return False
       
    

    menarca_texto = campo_menarca.get()
    if not menarca_texto.isdigit():
        print("digite uma idade válida")
        return False
    menarca = int(menarca_texto)
    regiao = campo_estado.get().strip().upper()
    
    participantes = { "nome" : nome,
                     "idade":idade,
                     "menarca":menarca,
                     "regiao":regiao
    }


    respostas.append(participantes)
    analgesico()
    return participantes

    
def analgesico():


    colica = opcao_colica.get()
    if colica == "SIM":
         medicamento = campo_analgesico.get()
         if medicamento in ["SIM","CLARO","É","AHAM"]:
            nome_medic = campo_digitanal.get().upper()
            if nome_medic in ["ADVIL","ALIVIUM","ATROFEM"]:
                recomendacao = campo_recomendacao.get().upper()
            else:
                print("Obrigada por nos dizer o seu medicamento usual,encerramos por aqui")
                return False
         else : 
            label_resultado.configure(text="Ok,Registramos que a senhora não utiliza analgésicos\n" \
            "por n]ao fazer parte do nosso perfil,encerramos a pesquisa por aqui")
            return False
    else:    
        label_resultado.configure(text="Compreendemos que não há experiência com medicamentos,\n" \
        "Encerramos por aqui")
        return False



def historico(respostas):

    senha = campo_senha.get()
    if senha == "145":
        if len(respostas)== 0:
            label_resultado.configure(text="SEM REGISTROS AINDA")
            return
        for participante in respostas:
            label_resultado.configure(f"{participante}")
            return
    else:
        label_resultado.configure(text="SENHA INCORRETA")
        return False

# Botões
botao_salvar = ctk.CTkButton(
    janela,
    text="SALVAR PESQUISA",
    command=dados_pessoais
)
botao_salvar.pack(pady=10)

botao_historico = ctk.CTkButton(
    janela,
    text="CONSULTAR HISTÓRICO",
    command=lambda: historico(respostas)
)
botao_historico.pack(pady=10)


        

janela.mainloop()

