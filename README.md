##############################################################################################################################################################################

Nesse video ensinarei como se fazer o uso conjunto do nvidia-xrun + PRIME Render Offload obtendo assim economia de energia e boa performance em jogos quando for necessário.

##############################################################################################################################################################################

TENHA SEMPRE UM BACKUP

Antes de começar faça um beckup com o Timeshift. Pode baixar o aplicativo com o comando

sudo apt-get install timeshift 

e instale o bleachbit para limpar modificações feitas do sistema durante o processo

sudo apt-get install bleachbit

##############################################################################################################################################################################

BAIXE O NVIDIA-XRUN

Deve baixar o nvidia-xrun do site abaixo cliacando no botão verde "clone or download" e depois doownload no link do site: 

https://github.com/Witko/nvidia-xrun

##############################################################################################################################################################################

BAIXE OS DRIVERS DO SITE OFICIAL DA NVIDIA

Baixe os drivers ddo site oficial da nVidia para sua placa: 

https://www.nvidia.com/Download/index.aspx?lang=en-us

Caso queira drivers beta:

https://developer.nvidia.com/vulkan-driver

Baixe as dependencias para estes drivers

dpkg --add-architecture i386
sudo apt-get autoclean
sudo apt-get clean
sudo apt-get update 
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get -f install
sudo dpkg --configure -a
sudo apt install firmware-linux build-essential gcc-multilib linux-headers-$(uname -r) initramfs-tools
sudo apt build-dep linux

Se vc tiver um kernel prório, pode ser que sua instalação do driver não tenha sucesso. Nesse caso,
recomendo que compile também os headers do seu kernel que o problema deve ser resolvido. =)

##############################################################################################################################################################################

DEPENDENCIAS DO PRIME RENDER OFFLOAD DESENCONTRADAS NO DEBIAN STABLE 10

Temos que o PRIME Render Offloaod tem como dependencias encontradas nos repositórios Sid:

Para fazer o PRIME Render offload funcionar vc precisa das dependencias encontradas somente no Sid ou Bullseye:

ii  xorg-server-source                     2:1.20.7-3                               all          Xorg X server - source files
ii  xorg-sgml-doctools                     1:1.11-1                                 all          Common tools for building X.Org SGML documentation
ii  xserver-xorg                           1:7.7+20                                 amd64        X.Org X server
ii  xserver-xorg-core                      2:1.20.7-3                               amd64        Xorg X server - core server
ii  xserver-xorg-dev                       2:1.20.7-3                               amd64        Xorg X server - development files
ii  xserver-xorg-input-all                 1:7.7+20                                 amd64        X.Org X server -- input driver metapackage
ii  xserver-xorg-input-libinput            0.29.0-1                                 amd64        X.Org X server -- libinput input driver
ii  xserver-xorg-input-wacom               0.34.99.1-1+b1                           amd64        X.Org X server -- Wacom input driver
ii  xserver-xorg-legacy                    2:1.20.7-3                               amd64        setuid root Xorg server wrapper
ii  xserver-xorg-video-intel               2:2.99.917+git20190815-1                 amd64        X.Org X server -- Intel i8xx, i9xx display driver

##############################################################################################################################################################################

PROCESSO DE INSTAÇÂO SERÁ VIA TTY

Agora para garantir que iremos sempre trabalhar sem nenhuma interface grafica faça e podermos instalar os drivers da nVidia e os pacotes xserver-xorg, faça:

systemctl set-default multi-user.target && systemctl reboot

##############################################################################################################################################################################

BAIXANDO DEPENDÊNCIAS DO PRIME RENDER OFFLOAD

Fazendo login vamos instalar os drivers da nvidia e os pacotes necessários para ativarmmos o PRIME Render offload. Faça:

sudo cat 'deb http://deb.debian.org/debian/ sid main' >> /etc/apt/sources.list && sudo apt-get update

Faça  também

sudo apt install -y --no-install-recommends --no-install-suggests xserver-common xserver-xephyr xserver-xorg xserver-xorg-core xserver-xorg-dev xserver-xorg-input-all xserver-xorg-legacy > xorg-install


ATENÇÃO: NÂO FAÇAM O COMANDO SUDO APT-GET UPGRADE OU SUDO APT-GET DIST-UPGRADE ATÉ O FIM DO PROCESSO DE INSTALAÇÂO E REMOÇÃO DA LINHA "deb http://deb.debian.org/debian/ sid main" DO SEU
SOURCES.LIST E A UTILIZAÇÂO DO BLEACHBIT. ISSO É ARRISCADO, SE VC NÂO SABE MANTER SEU COMPUTADOR. SOMENTE USUARIOS EXPERIENTES USAM O REPOSÍTÓRIO SID NO DIA A DIA, SE COMETEU ESSE ENGANO,
USE O TIMESHIFT. =) 

Avisos a parte. Leia o xorg-install e veja se tem algum pacote MUITO importante que não precisa ser removido. 
Usualmente, quando MUITOS (a perder de vista) pacotes ficam desnecessários é pq uma determinada funcionalidade do sistema se perdeu. Isso é ruim e não é esperado aqui, mas se atente e 
pesquise caso fique nervoso. =) Uma quantidade pequena de bibliotecas perdidas geralmente não significa nada além de um comportamento normal do update. Alias, isso acontece até mesmo com o comando
sudo apt-get upgrade e esse comando, por si só, não representa riscos e somente beneficios. <3 

##############################################################################################################################################################################

BAIXANDO OS DRIVERS CORRETOS PARA SUA PLACA INTEGRADA PARA UTILIZAÇÂO DIARIA

Agora vc precisa instalar os drivers da sua placa de video integrada para poder usar o nvidia-xrun em completude. Instale o neoetch:

sudo apt-get install neofetch. Então: 

renan@debian:~$ neofetch

       _,met$$$$$gg.          renan@debian 
    ,g$$$$$$$$$$$$$$$P.       ------------ 
  ,g$$P"     """Y$$.".        OS: Debian GNU/Linux 10 (buster) x86_64 
 ,$$P'              `$$$.     Host: Nitro AN515-52 V1.28 
',$$P       ,ggs.     `$$b:   Kernel: 4.19.0-8-amd64 
`d$$'     ,$P"'   .    $$$    Uptime: 1 hour, 23 mins 
 $$P      d$'     ,    $$P    Packages: 2712 (dpkg) 
 $$:      $$.   -    ,d$$'    Shell: bash 5.0.3 
 $$;      Y$b._   _,d$P'      Resolution: 1920x1080 
 Y$$.    `.`"Y$$$$P"'         WM: Xfwm4 
 `$$b      "-.__              WM Theme: Adapta 
  `Y$$                        Theme: Adwaita [GTK3] 
   `Y$$.                      Icons: Adwaita [GTK3] 
     `$$b.                    Terminal: xfce4-terminal 
       `Y$$b.                 Terminal Font: Monospace 12 
          `"Y$b._             CPU: Intel i7-8750H (12) @ 4.100GHz 
              `"""            GPU: Intel UHD Graphics 630 
                              GPU: NVIDIA GeForce GTX 1050 Ti Mobile 
                              Memory: 1316MiB / 32010MiB 

                                                    
No meu caso informou que tenho uma Intel UHD Graphics 630 que é suportada pelo pacote xserver-xorg-video-intel. Assim, eu faço:

sudo apt-get install xserver-xorg-video-intel.

##############################################################################################################################################################################

POR PRECAUÇÃO NÂO DEIXE O NOUVEAU INICIAR

Antes de instalar os graficos da Nvidia, não deixe seus módulo nouveau carregarer na inicialização (é possível isso acontecer se vc intalou o xserver-xorg-video-all):

sudo echo 'blacklist nouveau' >> /etc/modprobe.d/blacklist.conf. Os módulos (drivers) nouveau não funcionam em conjunto com os drivers da Nvidia. =) 

Antes do processo de instalação da nvidia copie os arquivos xorg.conf em /etc/X11 e também os arquivos com final .conf (*.conf) para para sua pasta do usuario /home/<nome do seu usuario> fazendo:

sudo cp /etc/X11/xorg.conf /home/<nome do seu usuario> $$ cp -a /etc/X11/xorg.conf.d/ home/<nome do seu usuario>

Agora vamos instalar os drivers da Nvidia fazendo:

sudo bash ~/Downloads/NVIDIA-*.run ou use o autocompletar para ser mais seguro. 

No meu caso, 

sudo bash ~/Downloads/NVIDIA-Linux-x86_64-440.58.01.run

Vc deve dizer sim para tudo, exceto para a ultima opção que diz 'generate a new X configuration file' utilizando o nvidia-xconfig, pois essa ultima pode fazer vc perder video numa proxima 
reinicialização do sistema com o servidor grafico ativado.

##############################################################################################################################################################################

CONFERINDO A INSTALAÇÂO

Se vc não deu sim na ultima opção de instalação drivers, parabéns, vc fez certo! 

Se vc deu sim na ultima opção, não se preocupe, veja se vc tem um arquivo com nome .backup ou nvidia-xconfig no meio (comando ls lista arquivos). 

Se tiver, faça 


sudo cp /home/<nome do seu usuario>/xorg.conf /etc/X11/

E tudo está correto e voltou ao normal. =) 

Agora vc deve estar com video nas próximas reinicializações. 



Veja se dentro do xorg.conf.d tem arquivos. Para tanto, faça:

ls -Ah /etc/X11/xorg.conf.d

Se a pasta não existe ou a pasta está vazia, vc está bem. Se ela existe e tem arquivos dentro, junte todos os arquivos dessa pasta e coloque dentro do xorg.conf fazendo o comando:

sudo cat /etc/X11/xorg.conf.d/*.conf >> /etc/X11/xorg.conf.

Nesse ponto, vc pode remover essa pasta (vc fez o backup na sua home com o comando cp -a /etc/X11/xorg.conf.d/ home/<nome do seu usuario> ).

Agora vc terá video em todas as reinicializações quando terminarmos o processo e o nvidia-xrun está pronto para funcionar, bastanto instalar.

##############################################################################################################################################################################

COLOCANDO OS ARQUIVOS DO NVIDIA-RUN NO LUGAR APROPRIADO (INSTALAÇÂO DO NVIDIA-XRUN)

Vá na pasta Download e extraia os arquivos. É possível fazer isso por terminal fazendo:

unzip nvidia-xrun-master.zip

Entre no novo diretório:

cd nvidia-xrun-master e abra o terminal nesse diretório e copie os arquivos da poasta de acordo com essa estrutura (essa parte é chata):

* **nvidia-xrun** - uses following dir structure:
* **/usr/bin/nvidia-xrun** - the executable script
* **/etc/X11/nvidia-xorg.conf** - the main X confing file
* **/etc/X11/xinit/nvidia-xinitrc** - xinitrc config file. Contains the setting of provider output source
* **/etc/X11/xinit/nvidia-xinitrc.d** - custom xinitrc scripts directory
* **/etc/X11/nvidia-xorg.conf.d** - custom X config directory
* **/etc/systemd/system/nvidia-xrun-pm.service** systemd service
* **/etc/default/nvidia-xrun** - nvidia-xrun config file

Faça apos isso (alterar donos e permitir execução):

chown root:root /etc/X11/xinit/nvidia-xinitrc && chmod +x /etc/X11/xinit/nvidia-xinitrc

chown root:root /usr/bin/nvidia-xrun && chmod +x /usr/bin/nvidia-xrun

Se vc baixou os drivers pelo reporítório, vai precisar seguir o tutorial: 

https://wiki.debian.org/NvidiaGraphicsDrivers/NvidiaXrun. 

Caso tenha dificuldades, realmente sugiro ler a Wiki do Debian que é realmente muito didática. <3 

##############################################################################################################################################################################

IMPEÇA QUE DRIVERS DA PLACA DE VIDEO DA NVIDIA CARREGEM NO BOOT PARA EVITAR DESCONFORTOS NO USO DO NVIDIA-XRUN

Essa parte é feita para evitar que a sessão grafica iniciada com o nvidia-xrun não trave quando for deslogar dessa sessão e além disso garantir economia de bateria.

Para evitar problemas com o nvidia-xrun vamos evitar que os drivers da Nvidia carreguem até termos video nas inicializações do sistema. 

Crie um arquivo chamado blacklist-nvidia.conf na pasta /etc/X11/modprobe.d com as entradas 

blacklist nvidia
blacklist nvidia_drm 
blacklist nvidia_modeset
blacklist nvidia_uvm.

Assim,

cat /etc/modprobe.d/blacklist-nvidia.conf

Devera ter como saida :

blacklist nvidia
blacklist nvidia_drm 
blacklist nvidia_modeset
blacklist nvidia_uvm.  

##############################################################################################################################################################################
DESLIGANDO SUA PLACA DE VIDEO ATÉ QUE ATIVE O NVIDIA-XRUN NOVAMENTE:

Agora inicie o serviço para ocultar sua placa de video do Kernel para ativar "ultra" economia de energia:

systemctl enable nvidia-xrun-pm.

##############################################################################################################################################################################
CONFIGURANDO O NVIDIA-XRUN PARA PRIME RENDER OFFLOAD NO LUGAR DO USUAL REVERSE PRIME

Nesse momento temos reverse PRIME configurado. Vale a pena notar que reverse PRIME tem uma performance absurda com OpenGL! No entanto, devido ao Bug da nvidia com aplicações Vulkan 
quando temos o PRIME synchronization ligado para aplicações Vulkan (https://wiki.archlinux.org/index.php/PRIME e https://devtalk.nvidia.com/default/topic/1044496/linux/hangs-freezes-when-vulkan-v-sync-vk_present_mode_fifo_khr-is-enabled/) vale a pena utilizar PRIME Render Offload nessas condições. Então a opção é utilizar o PRIME Render Offload para aplicações Vulkan comentando todas as linhas do arquivo /etc/X11/nvidia-xorg.conf e colocando no final do arquivo /etc/X11/nvidia-xorg.conf as seguintes opções: 

Section "ServerLayout"
  Identifier "layout"
  Option "AllowNVIDIAGPUScreens"
EndSection

ou 

Section "ServerLayout"
  Identifier "layout"
  Screen 0 "iGPU"
  Option "AllowNVIDIAGPUScreens"
EndSection

Section "Device"
  Identifier "iGPU"
  Driver "modesetting"
EndSection

Section "Screen"
  Identifier "iGPU"
  Device "iGPU"
EndSection

Section "Device"
  Identifier "dGPU"
  Driver "nvidia"
EndSection

##############################################################################################################################################################################

VOLTANDO A SESSÂO GRAFICA

Finalmente a instalação terminou e vc pode iniciar a interface grafica com sucesso:

systemctl set-default graphical.target && sudo reboot

##############################################################################################################################################################################

COMO EXECUTAR APLICAÇÔES GRAFICAS VIA NVIDIA_XRUN

A fim de utilizarmos o nvidia-xrun teremos que trocar de TTY fazendo CTRL+ALT+F1 e fazendo 

nvidia-xrun xfce4-session, nvidia-xrun openbox-session, nvidia-xrun startkde ou nvidia-xrun gnome-session (aquele que te agrada e tem instalado no sistema).

Se tudo foi feito correto, deve acontecer que o comando xrandr --listproviders tenha a seguinte saida:

renan@debian:~$ xrandr --listproviders 
Providers: number : 2
Provider 0: id: 0x43 cap: 0xf, Source Output, Sink Output, Source Offload, Sink Offload crtcs: 3 outputs: 1 associated providers: 0 name:modesetting
Provider 1: id: 0x248 cap: 0x0 crtcs: 0 outputs: 0 associated providers: 0 name:NVIDIA-G0


##############################################################################################################################################################################

LIMPANDO CACHE DE ARQUIVOS DO APT-GET e RETIRANDO O REPOSITÓRIO SID DO SOURCES.LIST

Utilize o bleachbit para isso:

sudo bleachbit
 
Faça limpeza inclusive as limpezas irrelevantes desmarcando somente o "Limpar espaço de disco" para caso vc use SSD.

sudo nano /etc/apt/sources.list

e remova ou comente a linha

deb http://deb.debian.org/debian/ sid main

##############################################################################################################################################################################


##############################################################################################################################################################################

PRINCIPAIS PROBLEMAS PROBLEMAS QUE PODERIAM A VIR A ACONTECER:

##############################################################################################################################################################################

######################################## 

TELA PRETA OU PISCANTE APOS BOOT MESMO CHECANDO ARQUIVOS EM XORG.CONF e XORG.CONF.D:

Todo problema assim ter que ser resolvido via Recovey em opções avançadas de Boot do GRUB

Aqui temos dois videos explicando como logar em Wifi nessas situações drasticas: 

https://www.youtube.com/watch?v=c_7xcM26iQY&t=352s (Testei)

https://www.youtube.com/watch?v=dGHo1TeDi88&t=125s (Não testei).

E, talvez, vc precise reinstalar seu display manager (talvez pelos repositórios Sid), então no meu caso eu faço:

sudo apt --no-install-recommends --no-install-suggests reinstall lightdm, pq uso o lightdm, mas no seu caso ver a saida "cat /etc/X11/default-display-manager" 
e baixar seu display manager

Talvez vc tenha que resintalar o xinit também, então:

sudo apt-get install xinit xterm

Pode tentar combinações das proximos problema também. =)

########################################

NVIDIA NVIDIA-XRUN NÃO FUNCIONA E TESTEI AS DUAS SOLUÇÔES DO TUTORIAL PARA /etc/X11/nvidia-xorg.conf

1) Verifique se colocou os arquivos no local correto seguindo a wiki do Debian:

https://wiki.debian.org/NvidiaGraphicsDrivers/NvidiaXrun e siga o tutorial de lá. =) 

2) Verifique se seu BusID da placa de video está correto. No meu caso :

renan@debian:~$ lspci
00:00.0 Host bridge: Intel Corporation 8th Gen Core Processor Host Bridge/DRAM Registers (rev 07)
00:01.0 PCI bridge: Intel Corporation Skylake PCIe Controller (x16) (rev 07)
00:02.0 VGA compatible controller: Intel Corporation UHD Graphics 630 (Mobile)
00:08.0 System peripheral: Intel Corporation Skylake Gaussian Mixture Model
00:12.0 Signal processing controller: Intel Corporation Cannon Lake PCH Thermal Controller (rev 10)
00:14.0 USB controller: Intel Corporation Cannon Lake PCH USB 3.1 xHCI Host Controller (rev 10)
00:14.2 RAM memory: Intel Corporation Cannon Lake PCH Shared SRAM (rev 10)
00:14.3 Network controller: Intel Corporation Wireless-AC 9560 [Jefferson Peak] (rev 10)
00:15.0 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH Serial IO I2C Controller (rev 10)
00:15.1 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH Serial IO I2C Controller (rev 10)
00:16.0 Communication controller: Intel Corporation Cannon Lake PCH HECI Controller (rev 10)
00:17.0 SATA controller: Intel Corporation Cannon Lake Mobile PCH SATA AHCI Controller (rev 10)
00:1d.0 PCI bridge: Intel Corporation Cannon Lake PCH PCI Express Root Port (rev f0)
00:1d.5 PCI bridge: Intel Corporation Cannon Lake PCH PCI Express Root Port (rev f0)
00:1e.0 Communication controller: Intel Corporation Device a328 (rev 10)
00:1f.0 ISA bridge: Intel Corporation Device a30d (rev 10)
00:1f.3 Audio device: Intel Corporation Cannon Lake PCH cAVS (rev 10)
00:1f.4 SMBus: Intel Corporation Cannon Lake PCH SMBus Controller (rev 10)
00:1f.5 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH SPI Controller (rev 10)
01:00.0 VGA compatible controller: NVIDIA Corporation GP107M [GeForce GTX 1050 Ti Mobile] (rev a1)
06:00.0 Non-Volatile memory controller: Sandisk Corp WD Black 2018/PC SN520 NVMe SSD (rev 01)
07:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTL8411B PCI Express Card Reader (rev 01)
07:00.1 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 12)

Informou que minha placa de video é dada por :

01:00.0 VGA compatible controller: NVIDIA Corporation GP107M [GeForce GTX 1050 Ti Mobile] (rev a1)

Assim devo ter meu Device da nvidia configurado assim:

Section "Device"
  Identifier "dGPU"
  Driver "nvidia"
  BusID "PCI:1:0:0"
EndSection

Verifique se seu BusID está em decimal. Eventualmente, algumas maquinas tem BusID da forma 

3c:00.0 VGA compatible controller: NVIDIA Corporation GP107M [GeForce GTX 1050 Ti Mobile] (rev a1). Esse bus ID com letras ou com dois digitos estão escritos em hexadecimal, então
converta para decimal com o comando:

bash -c "echo $(( 16#3c ))" e informou 60. Substitua 3c por 60. =)

3) Verifique se no arquivo /etc/default/nvidia-xrun CONTROLLER_BUS_ID e DEVICE_BUS_ID estão corretos. No meu caso,  

# Bus ID of the PCI express controller
CONTROLLER_BUS_ID=0000:00:01.0, pois meu PCI x16 é dado por 00:01.0 PCI bridge: Intel Corporation Skylake PCIe Controller (x16) (rev 07)

# Bus ID of the graphic card 
DEVICE_BUS_ID=0000:01:00.0, pois minha paca de video dedidaca é dada por  01:00.0 VGA compatible controller: NVIDIA Corporation GP107M [GeForce GTX 1050 Ti Mobile] (rev a1). 

Tente em hexadecimal e decimal.

Caso acima não deu certo, então com o comando lshw eu tenho no meio de uma saida gigantesca aparece algo assim:

*-pci:0
             description: PCI bridge
             product: Skylake PCIe Controller (x16)
             vendor: Intel Corporation
             physical id: 1
             bus info: pci@0000:00:01.0
             version: 07
             width: 32 bits
             clock: 33MHz
             capabilities: pci normal_decode bus_master cap_list
             configuration: driver=pcieport
             resources: irq:16 ioport:4000(size=4096) memory:a3000000-a40fffff ioport:90000000(size=301989888)
           *-display
                description: VGA compatible controller
                product: NVIDIA Corporation
                vendor: NVIDIA Corporation
                physical id: 0
                bus info: pci@0000:01:00.0
                version: a1
                width: 64 bits
                clock: 33MHz
                capabilities: vga_controller bus_master cap_list rom
                configuration: driver=nvidia latency=0
                resources: irq:157 memory:a3000000-a3ffffff memory:90000000-9fffffff memory:a0000000-a1ffffff ioport:4000(size=128) memory:a4000000-a407ffff

Indicando que o PCI express (x16) que controla minha placa de video é de BusID 0000:00:01.0 e minha placa de video é de BusID 0000:01:00.0. Se atentem que sua placa de video está abaixo de *-display.

########################################

COMO VOLTAR A TER A PLACA DE VIDEO SEMPRE APARECENDO NO LSPCI:

Caso queira não ter a economia de energia faça ou queira fazer uma configuração diferente faça:

systemctl disable nvidia-xrun-pm && sudo rm /etc/systemd/system/nvidia-xrun-pm.service


########################################

AS LETRAS NÂO ABREM CORRETAMENTE OU O COMANDO ROOT NÂO FUNCIONA

sudo apt install -y --no-install-recommends --no-install-suggests xfonts-100dpi xfonts-75dpi xfonts-base xfonts-cyrillic xfonts-cyrillic xfonts-encodings xfonts-scalable xfonts-utils sudo > avoiding-lack-of-packages

Para ver as mudanças faça:

nano lack-of-packages 

########################################

BLEACHBIT NÂO ABRE NO LOGADO NO USUARIO ROOT

Esse é um comportamento esperado. O usuário root não tem permissão para executar uma aplicação GUI na sua tela, mas somente o proprio usuário que logou na sessão. Nesse caso, 
acrescente seu usuario em /etc/sudoers. No meu caso o usuario é renan e eu fiz que meu usuario tivesse as mesmas permissões que o root sobre todo meu sistema:

# User privilege specification
root    ALL=(ALL:ALL) ALL
renan   ALL=(ALL:ALL) ALL.

E faça sudo bleachbit. =)

########################################

AUTOR SOBRE WAYLAND

Se ainda não tem video e vc usa Wayland (Gnome ou KDE) e tem um compositor instalado, instale a ultima versão do Wayland do sid:

sudo apt-get -t sid install xwayland. 

Se não funcionar, tente reinstalar seu DE

Se usa KDE: 

sudo apt-get reinstall kde-full 

Se usa gnome

sudo apt-get reinstall gnome

########################################

ABANAR DE MÂOS SOBRE WAYLAND

Aqui vem um abanar de mãos... eu não testei o Wayland. Apesar das ultimas versões dos drivers da Nvidia estarem funcionando para Wayland (KDE E GONE), talvez vc sofra um bug. 
Tente baixar as versões do driver da nvidia pelo repositório estavel que suponho que essa seja a solução (as vezes pessoal do Debian fez um patch para os drivers). Espero que tenha sucesso
e paginas do ArchWiki podem te ajudar. =) O Timeshift também te ajudará a reverter o caso. 


########################################
