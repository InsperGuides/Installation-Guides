---
Insper: Laboratório Ágil
date: Outubro 2019
---

Guia de Instalação de Drivers Nvidia no Linux (Ubuntu 18.04LTS)
===

Instalar os drivers das GPUs da Nvidia no linux nem sempre é uma tarefa trivial. Nesse guia espero esclarecer alguns pontos para que seja mais fácil para você entender o que está acontecendo durante a instalação.

---

### Passo 1: Verificar qual as informações da sua GPU:

Usaremos o seguinte comando no bash para descobrir qual é a versão da placa de vídeo instalada no sistema:

```
hwinfo --gfxcard --short
```

![hwinfo](images/hardware_check_hwinfo.png)

ou

``` 
sudo lshw -C display
``` 
![lshw](images/hardware_check_lshw.png.png)

---

### Passo 2: Instalar os Drivers da GPU usando o GUI do Linux

Abra as aplicações do Linux, ou pressione a tecla e pesquise por: 
```
update manager
```
![software_e update_icon](images/software_e_update_icon_print.png)

Após isso na parte de software & updates, selecione a aba **Additional Drivers** para abrir 
a lista de drivers disponíveis.
![software_e_update_selection](images/software_e_update_selection2.png)

Aqui será listado diversos Drivers da Nvidia, porém nem todos serão compatíveis com a sua placa, para descobrir qual é a melhor opção consulte o site da [Nvidia.](https://www.nvidia.com.br/Download/index.aspx?lang=br)

Coloque o seu S.O. e a versão da placa de vídeo que consultamos no **Passo 1**.

### Passo 2.1: Instalar os Drivers da GPU usando o terminal

Abra o terminal e insira:
```
apt search nvidia-driver
```

Para pesquisar os pacotes necessários: 
```
apt-cache search nvidia-driver
```

Após verificar qual são os drivers disponíveis instale o driver pertinente ao seu computador através do comando: 
```
sudo apt install nvidia-driver<numero_do_driver_aqui>
```

Reinicie o linux:
```
sudo reboot
```

---

### Passo 3: Verificar se o driver foi instalado com sucesso

Após a instalação é a forma mais simples de conferir se o driver foi instalado com sucesso é através do comando:

</n>

```
nvidia-smi
```


Ele irá te mostrar um detalhamento da sua placa:

![imagem do driver instalado com sucesso](images/nvidia-smi.png)

---

## Erros conhecidos

Ao instalar esse drivers, podem acontecer alguns erros, durante ou no final do processo. Alguns vão apenas te impossibilitar de usar algumas funções da placa, outros irão te impossibilitar instalar o drive.

### Erro 1:

A seguinte mensagem pode aparecer no seu console no momento que tiver instalando o driver via terminal:

```
WARNING: Unable to find suitable destination to install 32-bit compatibility libraries
```

Esse erro pode ser ignorado **dependendo** do que você precisa. Se você for usar o Linux para construir arquiteturas 32 bit mais parrudas, ou for rodar algum jogo, ela será necessária. Para resolver, basta instalar as depêndencias de arquitetura:

```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libc6:i386
```
### Erro 2:

```
E:  Unmet dependencies.  Try  'apt --fix-broken install'  with  no packages (or specify a solution).
```

Essa mensagem irá aparecer caso você tenha instalado uma versão anterior do driver. Exemplo: você está instalando a nvdia-418 e ainda tem instalado a nvidia-350.

Pare resolver é simples, você pode forçar a instalação do seguinte pacote:

```
sudo dpkg -i --force-overwrite /var/cache/apt/archives/nvidia-396_396.44-0ubuntu1_amd64.deb
```

### Erro 3:

A instalação completa sem nenhum erro, seja durante o processo de download ou de compilação, porém ao executar o **Passo 3** ela retorna um erro ou não mostra nenhum driver da Nvidia instalado, isso pode ser ocasionado por conta do Mok Manager* não consegui acessar os pacotes.

Ao reiniciar o computador **Passo 2.1** antes inicializar o Linux, aparecerá uma tela azul.

![MOK_1](images/MOK_1.png)

selecione: `enroll MOK`

Dependendo da configuração do seu S.O ele pode ou não pedir seu super usuário e senha.

![MOK_1](images/MOK_2.png)

selecione: `enroll key from hash`

Você verá uma outra tela mostrando as instalações que represaram no reinicialização do sistema caso tenha alguma, ele perguntará se deseja reiniciar o sistema, selecione `yes`. 

Após a reinicialização, repita o **passo 3** para verificar se tudo está correto.
