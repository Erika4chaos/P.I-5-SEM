# P.I-5-SEM - Explicação
Documentação de todo desenvolvimento realizado no Projeto Integrador do 5° semestre de engenharia da computação, no Centro Universitário Senac. O intuito desse projeto foi criar um sistema de energia renovável, e realizar coleta e tratamento de dados a partir de um microprocessador.

Para a coleta e análise dos dados gerados por duas placas solares, foi utilizada uma
placa ESP32 devido à sua capacidade de processamento, conectividade Wi-Fi, e
versatilidade na integração de sensores.

Através da coleta e análise de dados, bem como da interpretação dos resultados obtidos,
busca-se descobrir e analisar dados acerca da eficácia, viabilidade e potencial de ambos os
sistemas para contribuir com a transição global para uma matriz energética mais sustentável
e renovável.


" Erika Oliveira foi responsável pela concepção e desenvolvimento da maquete física,
criando uma representação visual do sistema em funcionamento, e contribuindo para uma
compreensão mais clara por parte dos usuários. Além disso, Erika assumiu a programação
do banco de dados, garantindo que os dados coletados pelo ESP32 fossem armazenados
de forma adequada para análise posterior. Adicionalmente, foi também responsável pela
criação e formatação da apresentação de slides e banner utilizados no dia da entrega." 

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

<?php

// executa uma consulta para selecionar os dados de hora e temperatura
// da tabela dados, e retorna os resultados no formato JSON.

// Configurações do banco de dados
$servername = "localhost";
$username = "engenharia_08";
$password = "gaviaoreal";
$dbname = "engenharia_08";

// Conexão com o banco de dados
$conn = new mysqli($servername, $username, $password, $dbname);

// Verifica a conexão
if ($conn->connect_error) {
    die("Conexão falhou: " . $conn->connect_error);
}

// Query para selecionar os dados do banco de dados
$sql = "SELECT hora, temperatura FROM dados";
$result = $conn->query($sql);

// Array para armazenar os dados
$data = array();

// Verifica se há dados e os adiciona ao array
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        $data[] = $row;
    }
}

// Retorna os dados no formato JSON
header('Content-Type: application/json');
echo json_encode($data);

$conn->close();
?>



Uma parte crucial do processo é a utilização de uma API para lidar com os dados enviados
pelo dispositivo de coleta, no caso o ESP32 (DOIT DEVKIT). Esta API é responsável por
receber os dados enviados pelo dispositivo e inseri-los no banco de dados MySQL, utilizando
as credenciais fornecidas pelo professor para acesso ao banco de dados.

#include <Ethernet.h>
#include <SPI.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress server(192, 168, 1, 100); // Endereço IP do servidor onde o script PHP está hospedado
EthernetClient client;

void setup() {
  Ethernet.begin(mac);
  Serial.begin(9600);
  delay(1000); // Espera a conexão Ethernet
}

void loop() {
  float temperatura = lerTemperatura(); // Função fictícia para ler a temperatura
  String hora = obterHora();           // Função fictícia para obter a hora atual

  if (client.connect(server, 80)) {
    Serial.println("Conectado ao servidor");
    client.print("GET /insere_dados.php?");
    client.print("hora=");
    client.print(hora);
    client.print("&temperatura=");
    client.print(temperatura);
    client.println(" HTTP/1.1");
    client.println("Host: 192.168.1.100"); //
    client.println("Connection: close");
    client.println();
  } else {
    Serial.println("Conexão falhou");
  }

  delay(60000); 
}

float lerTemperatura() {
  // Código para ler a temperatura do sensor
}

String obterHora() {
  // Código para obter a hora
}


Outro aspecto importante é a visualização dos resultados obtidos a partir da coleta de
dados dos sensores, como temperatura, umidade, tensão e corrente. Para isso, é
implementado um código que se comunica com o banco de dados e o código PHP, 
retornando os gráficos correspondentes em uma página web com um tempo de resposta
breve. As linguagens de programação utilizadas para esta finalidade são HTML, CSS e
JavaScript. O HTML define a estrutura da página, o CSS o estilo e o JavaScript é responsável
por carregar os dados do servidor utilizando a URL do PHP, processá-los e criar os gráficos
utilizando a biblioteca Chart.js.

<!DOCTYPE html>
    
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Dados de Temperatura</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <canvas id="graficoTemperatura"></canvas>

  <script>

//criamos uma página HTML simples com um elemento <canvas> onde o gráfico será desenhado.
// Em seguida, usamos JavaScript para carregar os dados do arquivo PHP dados.php (que ainda não foi criado),
// e depois usamos a biblioteca Chart.js para
//criar um gráfico de barras com os dados de hora e temperatura.

    // Função para carregar os dados do banco de dados e exibir no gráfico
    function carregarDados() {
      fetch('dados.php')
      .then(response => response.json())
      .then(data => {
        const horas = data.map(d => d.hora);
        const temperaturas = data.map(d => d.temperatura);
        
        // Configuração do gráfico
        const ctx = document.getElementById('graficoTemperatura').getContext('2d');
        new Chart(ctx, {
          type: 'bar',
          data: {
            labels: horas,
            datasets: [{
              label: 'Temperatura',
              backgroundColor: 'rgba(255, 99, 132, 0.2)',
              borderColor: 'rgba(255, 99, 132, 1)',
              borderWidth: 1,
              data: temperaturas
            }]
          },
          options: {
            scales: {
              y: {
                beginAtZero: true
              }
            }
          }
        });
      });
    }

    // Chama a função para carregar os dados quando a página carregar
    window.onload = carregarDados;

  </script>
</body>
</html>

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
