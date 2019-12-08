# Projeto Integrador 3 - 2019/2



Instituto Federal de Santa Catarina - *Campus* Florianópolis Departamento Acadêmico de Eletrônica Curso de Engenharia Eletrônica 

Aluno:

- Kevin Jahn Ferreira - engkevinn@gmail.com

Professores:

- Daniel Lohmann
- Reginaldo Steinbach

# Introdução



De acordo com o Departamento de Saúde dos Estados Unidos, o material particulado fino 2.5 (PM 2.5) é um poluente do ar que afeta a saúde das pessoas quando os níveis estão altos. PM2.5 são pequenas partículas no ar que reduzem a visibilidade e fazem com que o ar pareça nebuloso quando os níveis são elevados. Segundo o Departamento de Saúde de Nova York, os níveis externos de PM 2.5 são elevados em dias com pouca ou nenhuma mistura de vento ou ar. Em algumas cidades pelo mundo um aviso é emitido à população quando os níveis da partícula estão elevados para uso de máscara ou evitar longas saídas às ruas.

O termo de partícula fina ou material particulado 2.5 se dá devido ao fato de elas possuírem tamanho igual ou inferior a 2,5 micrometros de tamanho. O tamanho de uma partícula PM 2.5 é 30 vezes menor que um fio de cabelo humano. Desta maneira, devido ao seu pequeno tamanho elas são capazes de adentrar profundamente no nosso corpo, alcançando o pulmão. Além das diversas doenças respiratórias, a exposição prolongada a essa partícula pode causar doenças no coração. Alguns outros estudos estão sendo feitos relacionando PM 2.5 a demência e obesidade.

Este material particulado fino é produzido a partir de diversas fontes, como carros, caminhões, indústrias, cozinhas, cigarros, entre outros. Nos Estados Unidos, a Agência de Proteção Ambiental estabeleceu padrões para qualidade do ar em 1997, sendo atualizado em 2012. Foi estabelecido que até 35 microgramas por metro cúbico de PM 2.5 num dia seria considerado saudável, e apenas 12 ug/m^3 na média anual.

No Brasil, em algumas cidades já está sendo monitorado o nível de PM 2.5, porem o valor é convertido para AQL, diferenciando-se de outros medidores ao redor do mundo. A medição pode ser acompanhada nesse site: http://aqicn.org/map/brazil/ . Pode-se observar na imagem abaixo que a maior concentração está em grandes cidades. Nesse site, é possível visualizar os medidores pelo mundo: http://berkeleyearth.org/air-quality-real-time-map/ .

![1575762132530](C:\Users\User\Desktop\Engenharia\semestre 2019 2\PI 3\PI3\1575762132530.png)

**Figura 1**

Neste projeto foi utilizado a tabela abaixo para categorizar a qualidade do ar em bom ou danoso.

![1571920268445](C:\Users\User\Desktop\Engenharia\semestre 2019 2\PI 3\PI3\1571920268445.png)

​									**Figura 2**

# Equipamentos

* STM32F103T8

  ![Resultado de imagem para stm32f103 blue pill](https://miro.medium.com/max/750/1*etp3_d50qXURqgutedaA5A.jpeg)

  ​										**Figura 3**

* Nokia 5110

  ![Resultado de imagem para display nokia 5110](https://www.baudaeletronica.com.br/media/catalog/product/cache/1/image/9df78eab33525d08d6e5fb8d27136e95/d/i/display-nokia-5110-2.jpg)

  ​										**Figura 4**

* PPD42NS

  ![Resultado de imagem para ppd42ns](https://media-cdn.seeedstudio.site/media/catalog/product/cache/9d0ce51a71ce6a79dfa2a98d65a0f0bd/h/t/httpsstatics3.seeedstudio.comseeedimg2016-11uurjzjuilzanpukibjpvbl23.jpg)

  ​											**Figura 5**

# Proposta



Este projeto visa desenvolver uma unidade medidora de material particulado fino (PM2.5). 

1. Concepção
   * Considerar as tecnologias a utilizar;
   * Desenvolver os conceitos de PM 2.5, técnicas para validação, estudo do sensor.

2. Design
   * Case do produto;
   * Lógica do algoritmo.

3. Implementação
   * Realização do produto;
   * Escrita do código;
   * Testes;
   * Validações.

4. Operação

   * Agrupar as partes;

   * Manutenção.

     

# Concepção



Nesse momento, foram levantados os sensores que poderiam ser utilizados considerando o orçamento do projeto, além de serem analisados o microcontrolador e linguagem de programação adequados. Nesta etapa, também foi pensado em formas de validar o sensor.

Inicialmente pretendia-se utilizar o sensor DSM501A, por ser o mais facilmente encontrado à venda no Brasil. Esse sensor faz a medição da poeira no ar, retornando em seus pinos de saída pulsos de PWM. Através do tempo que os pulsos ficam em nível lógico baixo e alto, é possível dimensionar a quantidade da partícula no ar. 

Porém, devido a uma pesquisa realizada em Beijing entre diferentes tipos de sensores de medição de PM 2.5, foi decidido utilizar o PPD42NS da Shinyei.  A pesquisa visava comparar sensores de baixo custo vs alto custo, e o resultado obtido foi que os sensores Shinyei possuem excelente desempenho, muito superiores ao DSM501A.

O microcontrolador escolhido para o projeto foi o STM32F103T8, possui Cortex M3 e 20kb de RAM, suporta 5V em seus pinos GPIOS, necessários para o sensor e possui saída de alimentação de 5V. Para testes iniciais, foi utilizado um ESP8266. O fabricante do sensor disponibiliza um código em Arduino para testes do Sensor. 

O código pode ser visto a baixo:

```c++
int pin = 4;
unsigned long duration;
unsigned long starttime;
unsigned long sampletime_ms = 60000;//sampe 30s&nbsp;;
unsigned long lowpulseoccupancy = 0;
float ratio = 0;
float concentration = 0;
void setup() {
  Serial.begin(9600);
  pinMode(4, INPUT);
  starttime = millis();//get the current time;
}
void loop() {
  duration = pulseIn(pin, LOW);
//  Serial.print(duration);
 // Serial.print(" teste\n"); 
  lowpulseoccupancy = lowpulseoccupancy + duration;
  //Serial.print(lowpulseoccupancy);
  //Serial.print(" occuoancy\n");
  if ((millis() - starttime) >= sampletime_ms) //if the sampel time = = 30s
  {
    ratio = lowpulseoccupancy / (sampletime_ms * 10.0); // Integer percentage 0=&gt;100
    concentration = 1.1 * pow(ratio, 3) - 3.8 * pow(ratio, 2) + 520 * ratio + 0.62; // using spec sheet curve
    Serial.print("concentration = ");
    Serial.print(concentration);
    Serial.println(" pcs/0.01cf");
    Serial.println("\n");
    lowpulseoccupancy = 0;
    starttime = millis();
  }
}
```

Este código retorna uma saída em pcs/0.01cf, porém a unidade mais utilizada para o PM 2.5 é ug/m^3. A conversão pode ser feita multiplicando o valor de saída por 0.0283168.

Em resumo, o sensor funciona emitindo pulsos de nível lógico baixo em um pino de saída. Através desses pulsos, é contabilizado o tempo que a saída permanece em nível lógico baixo. Em seguida, após um intervalo de tempo são realizados alguns cálculos para obtenção do resultado final. 

Após algumas pesquisas e testes, foi observado que quanto maior o tempo da amostra, mais preciso é o resultado. Vale registrar que o sensor leva aproximadamente 3 minutos para aquecer e começar a dar resultados coerentes. 

Algumas ações podem ser feitas para melhorar a precisão do sensor, como vedar uma das entradas de ar do sensor, deixando apenas duas saídas descobertas. Em algumas referências é sugerido adicionar um resistor de 10k, caso a saída esteja muito sensível, no pino 5 (threshold) ao GND. Este resistor é adicionado para reduzir a tensão pela metade no threshold.  O sensor deve ser deixado na vertical.

O local da fita a ser colocada para vedar o buraco e melhorar a precisão pode ser observado abaixo:

![](C:\Users\User\Desktop\Engenharia\semestre 2019 2\PI 3\PI3\1575751026018.png)

​																			**Figura 6**

O sensor possui um resistor de 1 W internamente que aquece as partículas, agitando as mesmas. Há um led infravermelho que emite uma luz que reflete nas partículas, sendo captados em seguida por um photodiodo.

Abaixo há uma imagem do sensor por dentro.

![](C:\Users\User\Desktop\Engenharia\semestre 2019 2\PI 3\PI3\httpsstatics3.seeedstudio.comseeedimg2016-11uurjzjuilzanpukibjpvbl23.jpg)

​									**Figura 7**

O circuito elétrico do sensor pode ser observado na próxima figura.

![](C:\Users\User\Desktop\Engenharia\semestre 2019 2\PI 3\PI3\1575751486623.png)

 

​								**Figura 8**

A linguagem escolhida para o produto final foi C. O código do projeto pode ser visualizado neste GitHub.

As informações da saída devem ser simples e de fácil entendimento, portanto, pensou-se em utilizar um display para mostrar a informação acerca da qualidade do ar, além da quantidade de PM 2.5 no momento.



# Design



Nesta etapa foi pensado em como seria o case do produto. O sensor PPD42NS é desenvolvido para ambientes fechados, portanto, numa maneira de melhorar a precisão e evitar algumas interferências o produto deve ficar dentro de uma caixa com entrada e saída de ar, para circulação do mesmo. O produto idealizado deve ser de fácil transporte, portanto o case precisa ser leve. 

Referente à lógica do algoritmo, foi decidido utilizar o FreeRTOS para facilitar no acréscimo de funções que o projeto pode ter em versões futuras. É necessário um GPIO de entrada, para leitura do sensor e 5 GPIOs para o display LCD. O GPIO de entrada deve suportar 5V de tensão. É necessário uma função para contar o tempo em milissegundos, visto o que foi comentado a cima sobre o funcionamento do sensor. Desta maneira a função HAL_GetTick() foi utilizada. Esta função retorna o tempo em milissegundos desde que o microcontrolador foi ligado, semelhante à função millis() do Arduino. Mais a frente será detalhado melhor como foi estruturado o código, e sua lógica.

# Implementação



Como mencionado na etapa do Design, foi utilizado FreeRTOS e portanto a lógica fundamental do código pode ser resumido em uma única Task, visualizada abaixo. Os demais arquivos de configuração e inicialização podem ser visto neste repositório do GitHub.

```c
void vTask_main(void *pvParameters)
{
	static uint32_t duration = 0, sampletime_ms = 60000, lowpulse =0, timeinit, timeend, concentration, ratio;
		while(1)
		{

		if(HAL_GPIO_ReadPin(B9_GPIO_Port,B9_Pin) == 0)
		 {
		 timeinit =  HAL_GetTick();
		// printf("1\n");
		 while(HAL_GPIO_ReadPin(B9_GPIO_Port,B9_Pin) == 0){
		 timeend = HAL_GetTick();
		// printf("2\n");
		 }
	} else{
		duration = timeend - timeinit;
		//printf("3\n");
	}
		if(duration <=0 || duration >=100){ //sanity test
			duration = 0;
		}

		//printf("teste duration: %d\n", duration);

		lowpulse = lowpulse + duration;
		if(lowpulse >170000){
			lowpulse =0;
		}

		if((HAL_GetTick() - starttime) >= sampletime_ms){
		ratio = (lowpulse*100/(sampletime_ms*10));
		concentration = (((11/10) * (ratio*ratio*ratio))  - ((19/5) * (ratio*ratio)) + (520 * ratio) + (31/50))/100;
		concentration = (concentration * 283)/10000; //this is to convert to ug/m^3
		//printf("duration: %d low pulse: %d ratio:%d concentration: %d \n", duration, lowpulse, ratio/100, concentration); //debug
		limpa_LCD();
		goto_XY(0, 0);
		string_LCD_Nr("",concentration,3);
		goto_XY(20, 0);
		string_LCD("ug/m^3");
		//imprime_LCD();
		//escreve_Nr_Peq(0, 0,concentration, 4);
		lowpulse = 0;
	    starttime = HAL_GetTick();
	    if(concentration<=26)
	    {
	    	goto_XY(4, 4);
	    	string_LCD("Good air!");
	    }
	    else if(concentration>26){
	    	goto_XY(4, 4);
	    	string_LCD("Unhealthy!");
	    }
		}
			//até aqui

			osDelay(1);
		}
	//	vTaskDelete(vTask1_id);
}
```



O código pode ser dividido em 3 etapas. A primeira etapa é reconhecer que o pino está em nível lógico baixo e então, contar em milissegundos o tempo que ele permanece no mesmo estado. Desta maneira, quando o pino for igual a zero será armazenado numa variável o valor de retorno da função HAL_GetTick(). Em seguida, é feito um laço de repetição para coletar o ultimo valor de HAL_GetTick(), logo antes de alterar o nível lógico para alto. Desta maneira, com a diferença do valor inicial e final temos o tempo em milissegundos que o pulso ficou em zero. Semelhante à função pulseIn() do código do fabricante. Este processo é repetido por 60 segundos, tempo escolhido para amostragem. A fim de evitar valores absurdos devido a algum mal funcionamento, um teste de sanidade é feito. Caso a duração seja menor que zero, ou seja, negativo ou maior que 100 ms, a variável duration é zerada.

A segunda etapa é comparar se já se passaram 60 segundos, se sim, é coletado a duração total do pulso em nível lógico baixo dividido pelo tempo da amostra. Para utilização de variável do tipo inteiro de 32 bits foi multiplicado por 100 este valor, para maior precisão. Em seguida, são feitos os cálculos através da equação fornecida pelo fabricante e realizado a conversão para ug/m^3.

A terceira etapa é mostrar as informações no display, zerar a variável lowpulse e obter o instante final para obtermos mais 60 segundos para uma nova amostragem.

Para visualização dos resultados e debug na etapa de produção do código, foi utilizado a comunicação serial através de um printf(). Desta maneira, os resultados podem ser visualizados num terminal qualquer.

Os primeiros testes foram feitos com o código do fabricante. Após validar que o sensor estava funcionando, foi testado com o código final. Para o teste foi deixado o sensor num quarto fechado, simulando um ambiente limpo e controlado, ou seja, sem movimentação do ar ou poluentes consideráveis no ar. Os resultados variavam de 0 a 15 ug/m^3, o que classifica o ar como bom segundo a tabela mostrada na introdução.  Em seguida, foi queimado papel e plástico próximo a entrada de ar do sensor. Os resultados obtidos foram entre 50 e 150 ug/m^3 validando então o funcionamento do código e do produto.

A média anual de PM 2.5 em São Paulo, capital, foi de 17,3 em 2016. A cidade de Florianópolis por ser uma cidade litorânea tende a ser menos poluída. Desta maneira, podemos visualizar que o resultado obtido está dentro dos padrões. (https://www.redesocialdecidades.org.br/br/SP/sao-paulo/concentracoes-de-pm2-5)

# Conclusão



É necessário obter outros sensores para comparação de fato do resultado, pois Florianópolis não possui nenhuma unidade medidora do material particulado fino 2.5. Outros testes mais rigorosos podem ser realizados, como por exemplo inserir o sensor num saco a vácuo para eliminar completamente qualquer poeira do ar. A utilização de um microcontrolador com ponto flutuante de alta precisão se faz necessário para realização dos cálculos e conversões de forma precisa.   

Para versões futuras é necessário fazer a conexão do dispositivo com a internet (IoT) para disponibilizar os dados de forma online, desta maneira, tornando acessível a todos. 

 



# Referência Bibliográfica 

https://indiaairquality.com/2014/12/14/measuring-the-pickle-jr-a-modified-ppd42-with-an-attached-fan/ Acesso em: 07 dez. 2019.

https://funprojects.blog/2017/05/31/dust-monitor/ Acesso em: 07 dez. 2019.

https://www3.epa.gov/region1/airquality/pm-aq-standards.html Acesso em: 07 dez. 2019.

https://www.epa.vic.gov.au/your-environment/air/air-pollution/pm25-particles-in-air Acesso em: 07 dez. 2019.

https://www.redesocialdecidades.org.br/br/SP/sao-paulo/concentracoes-de-pm2-5 Acesso em: 07 dez. 2019.

https://www.health.ny.gov/environmental/indoors/air/pmq_a.htm Acesso em: 07 dez. 2019.

http://berkeleyearth.org/air-quality-real-time-map/ Acesso em: 07 dez. 2019.





