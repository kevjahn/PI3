# PI3

Este projeto visa a construção de um medidor de PM 2.5, material particulado, utilizando sensor de medição de poeira, display LCD, microcontrolador e com opção de sistema de alimentação via energia solar.

O material particulado PM 2.5 são partículas poluentes de até 2,5 micrômetro encontradas no ar. Estas são nocivas ao ser humano, portanto seus índices devem ser analisados e controlados.  Alguns países possuem medição em tempo real como os Estados Unidos e países da União Europeia. 

Estas partículas podem causar doenças respiratórias, cardiovasculares e afeta o desenvolvimento do cérebro humano, causando impactos cognitivos diversos.  Alguns estudiosos acreditam ser um dos principais poluentes do ar. A composição química deste elemento é importante tanto para o ser humano quanto para o clima.

No Brasil não há legislação sobre o PM 2.5 e é pouco monitorado. Os equipamentos encontrados para medição do mesmo chegam a custar R$ 2.600,00, porém, é possível encontrar versões mais baratas  com informações em Mandarim. Sendo assim, este projeto visa desenvolver um aparelho de medição portátil da qualidade do ar em relação ao PM 2.5, utilizando alimentação via energia solar.

O sensor pretendido a ser utilizado era o DSM501A, pois é o sensor mais facilmente encontrado a venda no Brasil. Este sensor faz a medição da poeira no ar, retornando em seus pinos de saída pulsos de PWM. Através do tempo que os pulsos ficam em nível lógico baixo e alto, é possível dimensionar a quantidade da partícula no ar. 

Porem, após pesquisas foi observado que este sensor DSM501A é uma copia do sensor Shinyei PPD 42NS. Foi feito uma pesquisa com diversos sensores de medição da qualidade do ar em Beijing, e o resultado foi que o DSM501A tinha um péssimo desempenho e portanto descartado. A pesquisa visava comparar sensores de baixo custo vs alto custo, e o resultado obtido foi que os sensores Shinyei possuem excelente desempenho.

As informações serão mostradas num display LCD e os dados tratados utilizando um microcontrolador (Não escolhido ainda). A opção de alimentação via painel solar será feita utilizando placa solar e conversor DC-DC (casamento de impedâncias). Será utilizado uma chave para opção de alimentação via Energia Solar ou por fonte comum. 





