# Hardware_Galileo-gen1_C
## Envio de dados via Galileo usando MQTTLens
### Informações do Galileo
- Processador Intel Quark SoC X1000 32-bit 400MHz
- Alimentação 5V
- Memória 256MB
- Sistema Operacional Linux (Yocto Project based Linux)
- Slot para cartão Micro SD

* [1º Passo: Instalar o Kit IoT Developer - Yocto](#passo1)
* [2º Passo: Leitura de dados do sensor DHT11](#passo2)
* [3º Passo: Conectando o Galileo a uma rede Ethernet](#passo3)


<a name="passo1"></a>
## 1º Passo: Instalar o Kit IoT Developer - Yocto:

Para Instalar o Sistema Operacional Linux na placa é necessario um Micro SD vazio, e de no mínimo  4GB e no máximo 32GB. Faça o download da Imagem do Sistema Operacional na pagina de downloads da Intel® [Kit IoT Developer](https://software.intel.com/en-us/iot/hardware/galileo/downloads) ou vá direto para o download [aqui](https://software.intel.com/galileo-image/latest). Após baixar o arquivo faça o download do [7-Zip](http://www.7-zip.org/) e extraia a imagem em uma pasta do computador.

Atenção: Mude a data para o dia atual renomeando o arquivo da imagem Ex:``iot-devkit-prof-dev-image-galileo-20171704.direct`` no formato ``iot-devkit-prof-dev-image-galileo-AAAADDMM.direct``. Baixe e instale o gravador de cartão SD [Win32_Disk_Imager](http://sourceforge.net/projects/win32diskimager). Selecione onde está o seu micro SD e click em "Write".

![atulaizar a data](https://cloud.githubusercontent.com/assets/17688443/25824618/718cdf44-3416-11e7-9963-569d5faf189c.png)

Após a instalação da imagem do Linux para o cartão Micro SD, é possível acessar o sistema operacional Linux via telnet, sendo uma maneira de se comunicar com o Galileo por endereço IP. Usando o DHCP ou atribuição de endereço IP estático, cria-se uma comunicação via telnet.

Coloque o SD  no slot no Galileo.
 
![sd](https://cloud.githubusercontent.com/assets/17688443/25824783/e9e28dfe-3416-11e7-809e-418264fc7331.png)

<a name="passo2"></a>
## 2º Passo: Leitura de dados do sensor DHT11:

Para extração dos dados de umidade e temperatura, será necessário a montagem de um circuito contendo um sensor DHT11, um potenciômetro ou um resistor com faixa de valores entre 4,7k e 10k ohms (No caso do potenciômetro, deve-se variar a resistência do mesmo até que seja possível o envio das informações, pois em alguns casos com uma resistência de 4,7k ohms não se consegue efetuar a leitura dos dados do sensor DHT11. O mesmo raciocínio deve ser utilizado para a escolha do valor correto do resitor caso prefira usar este) e um diodo 1N4007. Um exemplo de uma possível montagem pode ser visto abaixo.

![possível montagem](https://posinatel-my.sharepoint.com/personal/andrep_get_inatel_br/_layouts/15/guestaccess.aspx?docid=13aafbe77bdb748ee91ba9fb9d2924b30&authkey=AY6ohm1wdMnJByutQrv7i44)

A utilização do sensor DHT no Galileo contém certas complicações. A primeira é que as bibliotecas do Arduino forem executadas no contexto do userspace do Linux, recebendo, então, uma prioridade diferente. Os relógios fixos e a contagem de laços não são aplicáveis ​​ao Galileo. A segunda consiste em que com apenas um fio, DHT e muitos outros dispositivos de um fio não funcionarão com o Galileo porque o mesmo demora muito tempo ao mudar um pino de uma direção para outra. Isso ocorre pois o Intel Galileo usa expansores IO para o gpio o qual controla a direção do pino e esses expansores IO estão conectados ao Galileo via I2C.

Devido aos obstáculos citados acima, foi necessário fazer algumas alterações nas bibliotecas do sensor DHT11. Tais bibliotecas modificadas podem ser encontradas [aqui](https://github.com/AndreNadalini/Hardware_Galileo-gen1_C/tree/master/Libraries%20for%20DHT).
(Observação: A versão da IDE Arduino utilizada foi a 1.6.0 a qual pode ser baixada em [arduino.cc](https://www.arduino.cc/en/main/OldSoftwareReleases). Vale lembrar que foi necessário habilitar a placa Intel Galileo nesta edição.)

<a name="passo3"></a>
## 3º Passo: Conectando o Galileo a uma rede Ethernet:

Para que a placa consiga enviar dados para a nuvem, é necessário que esta esteja na mesma rede ethernet que o seu computador. Conecte o cabo de rede na Intel Galileo através da porta, pois para acessar o Linux da placa precisamos antes descobrir o IP da mesma. Abra a IDE do Arduino e execute o seguinte código:

```bash
void setup() {
  Serial.begin(9600);
  //digite 'a' para mostrar o IP
  while(Serial.read()!='a'){
    Serial.println("hello");
    delay(1000);
  }
  //ifconfig para serial monitor
  system("ifconfig > /dev/ttyGS0");
}
 
void loop() {
 
}
```
Abra o monitor serial no canto superior direito e digite 'a' para mostrar o IP da placa.

![ip](https://cloud.githubusercontent.com/assets/17688443/25825135/29f54660-3418-11e7-8056-863d2a8f0807.png)

O IP da placa esta dentro do retângulo vermelho como mostra a figura acima. Este IP será utilizado no código para o envio das informações do sensor, veremos isto logo a seguir.
