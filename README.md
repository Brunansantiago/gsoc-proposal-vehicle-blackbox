# gsoc-proposal-vehicle-blackbox

Resumo
       Este repositório documenta o desenvolvimento de uma prova de conceito de um gateway de segurança independente, projetado para atuar como uma "Caixa Preta" veicular. A proposta central deste sistema é o envio automatizado de mensagens para uma central de resgates contendo as coordenadas geográficas precisas do acidente, garantindo que as vítimas sejam localizadas e socorridas com a maior agilidade possível, mesmo em situações de falha elétrica total do veículo.

Justificativa

       A fundamentação deste trabalho parte da análise de dados críticos de segurança viária no Brasil. Segundo registros da Polícia Rodoviária Federal (PRF) e dados do DATASUS, eventos de alto impacto cinético, como capotamentos e saídas de pista, apresentam taxas de letalidade desproporcionais nas rodovias federais. Estudos do IPEA corroboram que essa mortalidade é agravada em zonas rurais ou de difícil acesso, onde a imobilização do veículo retarda drasticamente a janela de tempo necessária para o resgate. Paralelamente, no escopo de veículos autônomos e drones, a ejeção da bateria durante o impacto é a principal causa de perda definitiva de equipamentos de alto custo.
      Diante dessa problemática, identifica-se uma vulnerabilidade em sistemas embarcados convencionais: a dependência direta do barramento de energia principal. Se a bateria é destruída no impacto, a comunicação cessa instantaneamente, tornando o veículo e seus ocupantes invisíveis para o sistema de emergência justamente no momento mais crítico.
      O objetivo deste projeto é aplicar fundamentos de circuitos elétricos e programação para propor uma solução prática para esse problema. O sistema funciona como uma rede de segurança de último recurso (safety net of last resort), capaz de identificar o acidente de forma autônoma e utilizar uma reserva capacitiva isolada para transmitir o pedido de socorro, otimizando a logística de salvamento e a recuperação de ativos.
      
Tecnologias usadas:

ESP32; Sim7000G; Neo-6M; Mpu-6050; MQ-2; optoacoplador PC817; conversor DC-DC Buck, Diodo Schottky (1N5822), banco de supercapacitores (10F-15F) e módulo BMS de balanceamento.

O código foi feito em C++ e sua simulação foi realizada pelo Wokwi.

Metodologia de Funcionamento

A lógica do firmware e o design de hardware foram estruturados nas seguintes frentes:

    Detecção de Impacto por Duplo Gatilho: Algoritmo que monitora continuamente a Força G (> 4.0G) e o ângulo de inclinação (Roll > 70°). A máquina de estados cruza essas variáveis para garantir que o pedido de resgate seja real, mitigando falsos positivos.
    Interrupção de Hardware (GPIO Sense): Monitoramento da queda de tensão no barramento principal, sinalizando a perda de energia milissegundos antes da falha total do processador.
    Transmissão "Death Gasp": Rotina que utiliza a energia armazenada nos supercapacitores para despachar um payload JSON para a nuvem, contendo o diagnóstico da falha e as coordenadas geográficas para o resgate.
    Modo Beacon (Sinalizador): Após o alerta inicial, o sistema entra em Deep Sleep e desperta periodicamente a cada 10 minutos para emitir sinais de localização, auxiliando as equipes de busca em campo.
    Isolamento Elétrico: Uso de Diodo Schottky para impedir que um curto-circuito na bateria principal do veículo drene a energia emergencial do banco de supercapacitores.

Etapas do projeto:

    Modelagem da máquina de estados e algoritmos de filtragem no simulador Wokwi.
    Integração com a nuvem e validação de tráfego de pacotes via MQTT.
    Prototipagem do circuito de isolamento e análise da curva de descarga do supercapacitor.
    Design e roteamento da Placa de Circuito Impresso (PCB).
