# Entrega4_LabRedes

**Como disponibilizar página HTML num contêiner Docker no Laboratório ELAN**

*_É necessário ter a plataforma Docker instalada em uma nuvem privada do Elan, além do aplicativo PUTTY._

---
**Acesso ao Docker** 

1. No aplicativo PUTTY, definir o endereço de IP 200.17.141.82 e a porta 3333;
2. Acessar a plataforma fornecendo login e senha;
3. Executar um contêiner Ubuntu: 

    ` $ docker run -i -t ubuntu /bin/bash `
    
    O docker cria um novo contêiner, aloca um sistema de arquivos de leitura e gravação para o contêiner e cria uma interface para conectá-lo a rede, iniciando e executando _/bin/bash_.
    
    Ao encerrar o _/bin/bash_ o contêiner para, mas não é removido, sendo possível reiniciá-lo.
    
    Não é possível acessar as portas configuradas dentro do contêiner a menos que sejam exportadas ou mapeadas como parâmetros.

4. Encerrar _/bin/bash_ com o comando ` exit `;
5. Utilizar os comandos abaixo para copiar a aplicação para o sistema de arquivos locar, depois copiar para o contêiner e executar como servidor (execução realizada em _background):

    ` $ git clone http://github.com/othonb/pythonUDPTCP.git ` 
    
    ` $ cd pythonUDPTCP/TCP ` 
    
5. Alterar o código para a sintaxe do python 2:

    ` $ docker run -d -p 28670:28670 -it --rm  --name  servidortcp -v "$PWD":/var/www/servidortcp -w /var/www/servidortcp python:2 python ./TCPServer.py ` 
    
    - -d : executa como servidor
    - -p : porta externa:porta interna : porta interna é acessada pela aplicação dentro do contêiner, porta externa é acessada a partir da rede do sistema hospedeiro
    
      *Se a porta utilizada estiver ocupada, deve-se trocar a porta
      
    - --rm : apaga a imagem do contêiner após execução
    
      *Recomendado para evitar o acúmulo de imagens
      
    - --name : identificação do contêiner para gerênciar
    - -v : identifica o volume onde os dados vão persistir
    - $PWD":/var/www/servidortcp : a pasta "_/www/servidortcp_" será criada na pasta "_/var_" do conteiner e toco o conteúdo da pasta local será copiado para o contêiner
    
6. Quando o contêiner for parado, será apagado.
