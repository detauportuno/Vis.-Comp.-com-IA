 for email in emails:
        # Verificar se o email contém a palavra "booking"
        titulo_email = email.subject
        padrao = r"\bbooking\b"
        correspondencia = re.search(padrao, titulo_email, re.IGNORECASE)    

        # Verificar se o email contém o numero de order
        conteudo_email = email.body
        padrao_2 = r"d{1}-\d{11}"
        correspondencia_2 = re.search(padrao2, conteudo_email)  

        # Verificar se o email contém o a palavra em negrito, que se refere ao nome do cliente
        conteudo_email = email.body
        padrao_3 = r"\*\*(.*?)\*\*"
        correspondencia_3 = re.search(padrao3, conteudo_email, re.IGNORECASE)  

        if correspondencia and correspondencia_2:
            # Separando o nome em negrito (nome do cliente) e colocando ele na posição desejada da planilha
            nome = correspondencia_3.group(1)

            # Separando o numero para colocá-lo na planilha
            numero = correspondencia_2.group(1)

            # Obter a data de envio do email
            data_envio = email.sent_on

            # Converter a data para o formato desejado
            data_formatada = data_envio.strftime("%d-%m-%Y")

            # Escrever o nome e o número nas células correspondentes
            planilha[coluna_nome + str(linha_atual)] = nome.strip()  # Remover espaços em branco            
            planilha[coluna_numero + str(linha_atual)] = numero.strip()  # Remover espaços em branco
            planilha[coluna_gc + str(linha_atual)] = email.sender_name
            planilha[coluna_data + str(linha_atual)] = data_formatada


            # Atualizar a posição das células
            linha_atual += 1
