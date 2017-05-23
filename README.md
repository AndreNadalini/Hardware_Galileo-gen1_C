# Hardware_Galileo-gen1_C
## Envio de dados via Galileo usando MQTTLens
### Informações do Galileo
- Processador Intel Quark SoC X1000 32-bit 400MHz
- Alimentação 5V
- Memória 256MB
- Sistema Operacional Linux (Yocto Project based Linux)
- Slot para cartão Micro SD

* [1º Passo: Instalar o Kit IoT Developer - Yocto](#passo1)

<a name="passo1"></a>
## 1º Passo: Instalar o Kit IoT Developer - Yocto:

Para Instalar o Sistema Operacional Linux na placa é necessario um Micro SD vazio, e de no mínimo  4GB e no máximo 32GB. Faça o download da Imagem do Sistema Operacional na pagina de downloads da Intel® [Kit IoT Developer](https://software.intel.com/en-us/iot/hardware/galileo/downloads) ou vá direto para o download [aqui](https://software.intel.com/galileo-image/latest). Após baixar o arquivo faça o download do [7-Zip](http://www.7-zip.org/) e extraia a imagem em uma pasta do computador.

Atenção: Mude a data para o dia atual renomeando o arquivo da imagem Ex:``iot-devkit-prof-dev-image-galileo-20171704.direct`` no formato ``iot-devkit-prof-dev-image-galileo-AAAADDMM.direct``. Baixe e instale o gravador de cartão SD [Win32_Disk_Imager](http://sourceforge.net/projects/win32diskimager). Selecione onde está o seu micro SD e click em "Write".

![atulaizar a data](https://cloud.githubusercontent.com/assets/17688443/25824618/718cdf44-3416-11e7-9963-569d5faf189c.png)

Após a instalação da imagem do Linux para o cartão Micro SD, somos capazes de acessar o sistema operacional Linux via telnet. É uma maneira de se comunicar com o Galileo por endereço IP. Usando o DHCP ou atribuição de endereço IP estático, nós criamos comunicação via telnet.