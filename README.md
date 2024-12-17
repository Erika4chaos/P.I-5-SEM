# P.I-5-SEM - Explicação
Documentação de todo desenvolvimento realizado no Projeto Integrador do 5° semestre de engenharia da computação, no Centro Universitário Senac. O intuito desse projeto foi criar um sistema de energia renovável, e realizar coleta e tratamento de dados a partir de um microprocessador.

Para a coleta e análise dos dados gerados pelas duas placas solares, foi utilizada uma
placa ESP32 devido à sua capacidade de processamento, conectividade Wi-Fi, e
versatilidade na integração de sensores.

Através da coleta e análise de dados, bem como da interpretação dos resultados obtidos,
busca-se descobrir e analisar dados acerca da eficácia, viabilidade e potencial de ambos os
sistemas para contribuir com a transição global para uma matriz energética mais sustentável
e renovável.


Erika Oliveira foi responsável pela concepção e desenvolvimento da maquete física,
criando uma representação visual do sistema em funcionamento, e contribuindo para uma
compreensão mais clara por parte dos usuários. Além disso, Erika assumiu a programação
do banco de dados, garantindo que os dados coletados pelo ESP32 fossem armazenados
de forma adequada para análise posterior. Adicionalmente, foi também responsável pela
criação e formatação da apresentação de slides e banner utilizados no dia da entrega.

A programação da ESP32 envolveu scripts específicos para processar os dados recebidos
dos sensores e garantir a transmissão eficiente ao banco de dados, utilizando protocolos de
comunicação adequados disponiveis através da platarforma Arduino IDE.


O Arduino Integrated Development Environment (IDE) é uma aplicação de software de
código aberto utilizada para o desenvolvimento de projetos de hardware baseados em
microcontroladores e microprocessadores. Desenvolvido principalmente para a plataforma
Arduino, o IDE oferece uma interface gráfica intuitiva e simplificada para escrever, compilar
e carregar código em dispositivos Arduino e em uma variedade de outras placas compatíveis.
A linguagem de programação utilizada no Arduino IDE é baseada em C/C++, com
extensões e simplificações para facilitar o desenvolvimento de projetos de hardware por
usuários com diferentes níveis de experiência. O IDE é equipado com uma vasta biblioteca
padrão que contém funções pré-definidas para acessar e controlar os periféricos e
dispositivos conectados às placas Arduino.


O gerenciamento e processamento de dados neste projeto são conduzidos por meio de
um banco de dados MySQL, acessível e administrado através do PHPMYADMIN. Para isso,
é essencial a ativação do servidor utilizando o XAMPP, configurando tanto o Apache quanto
o MySQL, e garantindo a presença de um arquivo index.php atualizado dentro do diretório
do XAMPP, que serve como servidor para o projeto em questão.

Uma parte crucial do processo é a utilização de uma API para lidar com os dados enviados
pelo dispositivo de coleta, no caso o ESP32 (DOIT DEVKIT). Esta API é responsável por
receber os dados enviados pelo dispositivo e inseri-los no banco de dados MySQL, utilizando
as credenciais fornecidas pelo professor para acesso ao banco de dados.

Outro aspecto importante é a visualização dos resultados obtidos a partir da coleta de
dados dos sensores, como temperatura, umidade, tensão e corrente. Para isso, é
implementado um código que se comunica com o banco de dados e o código PHP, 
retornando os gráficos correspondentes em uma página web com um tempo de resposta
breve. As linguagens de programação utilizadas para esta finalidade são HTML, CSS e
JavaScript. O HTML define a estrutura da página, o CSS o estilo e o JavaScript é responsável
por carregar os dados do servidor utilizando a URL do PHP, processá-los e criar os gráficos
utilizando a biblioteca Chart.js.

As interações entre HTML, PHP, MySQL e JavaScript ocorrem de forma integrada: o
ESP32 lê os dados dos sensores e estabelece a conexão com o servidor. Uma vez
estabelecida a conexão, os dados são enviados ao servidor via HTTP GET, onde são
processados e armazenados no banco de dados. Posteriormente, ao ser atualizada, a página
web solicita ao servidor as informações mais recentes e as exibe em forma de gráficos.

Além disso, alguns pontos cruciais das operações incluem a definição dos cabeçalhos da
resposta JSON para permitir o acesso de qualquer origem (CORS), as credenciais de acesso
ao banco de dados que incluem informações como o nome do banco de dados, nome de
usuário, senha e host, e a verificação da conexão com o banco de dados antes de realizar
qualquer operação. Após receber e validar os parâmetros enviados pelo dispositivo, os
dados são inseridos na tabela IOT_project por meio de uma consulta SQL preparada. Por
fim, a resposta é retornada, indicando sucesso ou erro na inserção dos dados, seguida pelo
fechamento da conexão com o banco de dados.

O objetivo principal dessa maquete é proporcionar um exemplo prático e lúdico do
funcionamento de painéis solares em escala reduzida, visando transmitir o funcionamento
do projeto com autenticidade e elegância.
Para a confecção da maquete, foram utilizados materiais como MDF de 6mm de
espessura, cola de madeira e de contato, folha de acetato acrílico, tinta acrílica, madeira,
plantas artificiais, cascalho de aquário, esponjas reutilizadas, massa corrida, lixa de 8mm e
pincéis. A combinação desses materiais proporcionou a criação de uma maquete detalhada
e realista, capaz de transmitir de forma eficaz os conceitos e benefícios do uso de energia
solar em residências.
