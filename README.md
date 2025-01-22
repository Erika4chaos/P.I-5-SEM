# Projeto Integrador - 5° Semestre de Engenharia da Computação

Este repositório contém a documentação completa do desenvolvimento realizado no Projeto Integrador do 5° semestre de Engenharia da Computação no Centro Universitário Senac.

## Descrição do Projeto

O objetivo deste projeto foi criar um sistema de energia renovável e realizar a coleta e tratamento de dados utilizando um microprocessador. O projeto incluiu o monitoramento de dados gerados por placas solares, processamento de informações e a apresentação de resultados em uma interface visual.

---

## Tecnologias Utilizadas

- **Microcontrolador:** ESP32 (DOIT DEVKIT)
- **Banco de Dados:** MySQL
- **Linguagens de Programação:**
  - Backend: PHP
  - Frontend: HTML, CSS, JavaScript
- **Bibliotecas:**
  - Chart.js (para visualização de dados)

---

## Implementação

### Coleta e Processamento de Dados

Para a coleta e análise de dados gerados pelas placas solares, utilizamos uma placa **ESP32**, devido às seguintes características:
- Capacidade de processamento eficiente.
- Conectividade Wi-Fi.
- Versatilidade na integração com sensores.

A seguir está o código utilizado para coletar dados do banco e retorná-los em formato JSON:

```php
<?php

// Configurações do banco de dados
$servername = "localhost";
$username = "(*/ω＼*)";
$password = "(^///^)";
$dbname = "engenharia_08";

// Conexão com o banco de dados
$conn = new mysqli($servername, $username, $password, $dbname);

// Verifica a conexão
if ($conn->connect_error) {
    die("Conexão falhou: " . $conn->connect_error);
}

// Consulta para selecionar os dados
$sql = "SELECT hora, temperatura FROM dados";
$result = $conn->query($sql);

// Array para armazenar os dados
$data = array();

if ($result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        $data[] = $row;
    }
}

// Retorna os dados no formato JSON
header('Content-Type: application/json');
echo json_encode($data);

$conn->close();
?>



## Envio de Dados via ESP32
A API criada recebe os dados do ESP32 e os insere no banco de dados. O seguinte código Arduino foi utilizado para envio dos dados:


#include <Ethernet.h>
#include <SPI.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress server(192, 168, 1, 100); // Endereço IP do servidor
EthernetClient client;

void setup() {
  Ethernet.begin(mac);
  Serial.begin(9600);
  delay(1000);
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
    client.println("Host: 192.168.1.100");
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

## Visualização de Dados
Para exibir os dados coletados de forma visual, implementamos uma interface web. O HTML, CSS e JavaScript foram usados para estruturar, estilizar e carregar os dados.
A biblioteca Chart.js foi utilizada para criar os gráficos. Abaixo está o código da página:

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
    // Função para carregar os dados do banco e exibir no gráfico
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

    // Chama a função ao carregar a página
    window.onload = carregarDados;
  </script>
</body>
</html>


