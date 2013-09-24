---
layout: post
title: "[Off-Topic] Transforme seu roteador wireless em um NAS, MediaServer UPnP/DLNA e BitTorrent client com OpenWRT"
slug: "off-topic-transforme-seu-roteador-wireless-em-um-nas-mediaserver-upnp-dlna-bittorrent-client-com-openwrt"
date: 2013-09-24 10:00
comments: true
categories: off-topic, linux
author: rodrigopanachi
---

**TL;DR** [OpenWRT](http://openwrt.org) é uma distro Linux embarcável em roteadores que permite customizar e instalar serviços sem a necessidade de compilar um novo firmware. Em outras palavras, é um "firmware on steroids" para roteadores.

## Mas por quê?

* Economia. Apenas substituindo o firmware do roteador é possível adicionar funcionalidade e recursos, economizando uma grana preciosa com dispositivos semelhantes, como AppleTV, NAS dedicado, etc.
* Funcionalidade. Provavelmente o roteador fica ligado 100% do tempo na sua casa, sendo utilizado apenas para compartilhar a internet. Praticamente um servidor disponível 24 horas por dia sem utilizaçao! Não mais...
* Diversão. Substituir o firmware do seu roteador por uma distro baseada em Linux e configurar todos os serviços manualmente... é diversão pura!
* Por que eu quero (e posso)!

## Instalação

**Atenção: existe a possibilidade (embora pequena) de algo sair muito errado e você ganhar um peso de papel. Prossiga por sua conta e risco... ou se tiver coragem!**

O primeiro passo é descobrir o modelo do seu roteador e verificar a compatibilidade nesta página: http://wiki.openwrt.org/toh/start. Se não encontrar o modelo ou houver um aviso de não-compatibilidade, sinto muito mas não vai rolar para você.

Se seu roteador for compatível, haverá o **target** da imagem e um link para uma wiki com um how-to específico do modelo, instruções de instalação, resolução de problemas, etc. Procure pelo nome da release que você deverá baixar daqui http://downloads.openwrt.org. A imagem tem o formato _openwrt-TARGET-generic-MODELO-VERSAO-squashfs-factory.bin_

Por exemplo, meu roteador é um **TP-Link WR1043ND**. Procurando na página http://wiki.openwrt.org/toh/start, o target compatível com meu roteador é _ar71xx_:

![](/images/posts/openwrt-1.png)

Acessei a página de downloads, naveguei pelos diretórios da release, depois target e encontrei o arquivo _openwrt-ar71xx-generic-tl-wr1043nd-v1-squashfs-factory.bin_ para download.

![](/images/posts/openwrt-2.png)

Quando baixar a imagem, é só fazer a atualização de firmware normalmente no seu roteador pelo admin:

![](/images/posts/openwrt-3.png)

Confirme e reze muito! Aguarde o roteador atualizar e reiniciar.

Deste ponto em diante, será necessário conectar no roteador com cabo, para setar uma senha de root e configurar o wifi. Este processo é relativamente trivial, basta utilizar a interface web do admin, sem segredo...

![](/images/posts/openwrt-4.png)

Uma vez definida a senha e o wifi configurado, é possível acessar seu roteador via ssh:

{% codeblock %}

$ ssh root@192.168.1.1
root@192.168.1.1's password:

BusyBox v1.19.4 (2013-03-14 11:28:31 UTC) built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 ATTITUDE ADJUSTMENT (12.09, r36088)
 -----------------------------------------------------
  * 1/4 oz Vodka      Pour all ingredients into mixing
  * 1/4 oz Gin        tin with ice, strain into glass.
  * 1/4 oz Amaretto
  * 1/4 oz Triple sec
  * 1/4 oz Peach schnapps
  * 1/4 oz Sour mix
  * 1 splash Cranberry juice
 -----------------------------------------------------
root@OpenWrt:~#

{% endcodeblock %}

Se você chegou até aqui, parabéns! Você teve coragem. E felizmente a parte difícil já passou...

Nota: dependendo do seu roteador, a versão do OpenWRT pode variar. Leia a [wiki](http://wiki.openwrt.org/) do modelo do seu roteador para instruções específicas e resolução de problemas.

**Importante**: caso o pior aconteça (como acabar a energia no meio do processo de flash da firmware) e você não queira utilizar seu roteador como peso de papel, tente seguir os procedimento de "debriking" em http://wiki.openwrt.org/doc/howto/generic.debrick

## Configurando o NAS

Para o NAS, vamos montar as partições do HD externo e configurar uma partição de swap pois o roteador provavelmente não tem memória interna suficiente para dar conta do recado.

Neste exemplo, vou usar meu HD de 1TB com uma partição formatada em ext4 e outra partição de 1GB formatada como linux+swap. Não vou usar FAT32 nem NTFS e nem recomendo! A idéia aqui é deixar o HD plugado eternamente no roteador e acessá-lo pela rede.

### Instalando os pacotes necessários

Para nossa sorte, o OpenWRT conta com um gerenciador de pacotes que facilita (e muito) a instalação das libs e módulos necessários para montar o HD externo e compartilhá-lo na rede via Samba ou NFS.

Logue no roteador via ssh e instale os seguintes pacotes:

{% codeblock %}

root@OpenWrt:~# opkg update
root@OpenWrt:~# opkg install kmod-usb-storage block-mount kmod-fs-ext4

{% endcodeblock %}

O ultimo pacote vai depender do sistema de arquivos do seu HD. Por exemplo, kmod-fs-ext2, etc. Veja os módulos disponíveis em http://wiki.openwrt.org/doc/howto/storage

Verifique se as partições já foram detectadas rodando _blkid_:

{% codeblock %}

root@OpenWrt:~# blkid
/dev/mtdblock2: TYPE="squashfs"
/dev/sda1: LABEL="MEDIA" UUID="1ecad5f1-0000-0000-000f-e8b7f6ac651d" TYPE="ext4"

{% endcodeblock %}

### Montando as partições do HD

Agora vamos configurar o fstab para montar as partições automaticamente quando o roteador for ligado. Edite o arquivo _/etc/config/fstab_ com o seguinte (use o vi):

{% codeblock %}

config global automount
        option from_fstab 1
        option anon_mount 0

config global autoswap
        option from_fstab 1
        option anon_swap 0

config mount
        option target   /mnt/media
        option device   /dev/sda1
        option fstype   ext4
        option options  rw,sync
        option enabled  1
        option enabled_fsck 0

config swap
        option device   /dev/sda2
        option enabled  1

{% endcodeblock %}

No meu caso, vou montar a partição _/dev/sda1_ em _/mnt/media_, então é necessário criar o diretório de destino, ativar e iniciar o serviço _fstab_:

{% codeblock %}

root@OpenWrt:~# mkdir /mnt/media
root@OpenWrt:~# /etc/init.d/fstab enable
root@OpenWrt:~# /etc/init.d/fstab start

{% endcodeblock %}

Pronto, seu HD externo pode ser acessado em _/mnt/media_ e o swap foi montado na segunda partição.

### Compartilhando na rede

Vou utilizar [NFS](http://wiki.openwrt.org/doc/howto/nfs.server) para compartilhar a partição na rede, mas você também pode utilizar Samba seguindo http://wiki.openwrt.org/doc/howto/cifs.server

{% codeblock %}

root@OpenWrt:~# opkg update
root@OpenWrt:~# opkg install nfs-kernel-server

{% endcodeblock %}

Edite (ou crie) o arquivo _/etc/exports_ com o conteúdo:

{% codeblock %}

/mnt/media 192.168.1.0/255.255.255.0(rw,sync,no_subtree_check)

{% endcodeblock %}

Ative e inicie os serviços necessários:

{% codeblock %}

root@OpenWrt:~# /etc/init.d/portmap start
root@OpenWrt:~# /etc/init.d/portmap enable
root@OpenWrt:~# /etc/init.d/nfsd start
root@OpenWrt:~# /etc/init.d/nfsd enable

{% endcodeblock %}

Pronto! O server (roteador) está configurado. Para montar esta partição no client, por exemplo Ubuntu, siga:

{% codeblock %}

$ sudo apt-get install nfs-common
$ sudo mkdir /media/nas
$ sudo mount -t nfs 192.168.1.1:/mnt/media /media/nas

{% endcodeblock %}

Groovy! Seu NAS foi montado no seu desktop em _/media/nas_. Pode compartilhar seus arquivos a vontade!

## Configurando o MediaServer (minidlna)

Logue no roteador e instale os pacotes necessários:

{% codeblock %}

root@OpenWrt:~# opkg update
root@OpenWrt:~# opkg install minidlna

{% endcodeblock %}

Para configurar, basta editar o arquivo _/etc/config/minidlna_ como segue:

{% codeblock %}
config minidlna config
	option 'enabled' '1'
	option port '8200'
	option interface 'br-lan'
	option friendly_name 'OpenWrt DLNA Server'
	option db_dir '/mnt/media/minidlna/db'
	option log_dir '/mnt/media/minidlna/log'
	option inotify '1'
	option enable_tivo '0'
	option strict_dlna '0'
	option presentation_url ''
	option notify_interval '900'
	option serial '12345678'
	option model_number '1'
	option root_container '.'
	list media_dir '/mnt/media'
	option album_art_names 'Cover.jpg/cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thumb.jpg'

{% endcodeblock %}

Ative e inicie o serviço:

{% codeblock %}

root@OpenWrt:~# /etc/init.d/minidlna enable
root@OpenWrt:~# /etc/init.d/minidlna start

{% endcodeblock %}

Pronto! O MediaServer está configurado e compartilhando seus arquivos na rede. Aqui na minha SmartTV aparece assim:

![MiniDLNA na Samsung SmartTV](/images/posts/minidlna.jpg)

## Configurando o BitTorrent client (transmission)

Vamos instalar os pacotes:

{% codeblock %}

root@OpenWrt:~# opkg update
root@OpenWrt:~# opkg install transmission-daemon

{% endcodeblock %}

E configurar o serviço editando _/etc/config/transmission_ e alterando as opções a seguir (mantenha todas as outras configs):

{% codeblock %}

config transmission
	option enabled 1
	option config_dir '/mnt/media/transmission/config'
	option download_dir '/mnt/media/transmission/complete'
	option incomplete_dir '/mnt/media/transmission/incomplete'
	option incomplete_dir_enabled true
	option ratio_limit 2.0000
	option ratio_limit_enabled true
	option rpc_authentication_required false
	option rpc_password ''
	option rpc_username ''
	option speed_limit_up 5
	option speed_limit_up_enabled true

{% endcodeblock %}

Ative e inicie o serviço:

{% codeblock %}

root@OpenWrt:~# /etc/init.d/transmission enable
root@OpenWrt:~# /etc/init.d/transmission start

{% endcodeblock %}

O Transmission roda como um daemon, sendo controlado remotamente. Para isso, será necessário adicionar a seguintes regra de farewall, adicionando no final do arquivi _/etc/config/firewall_:

{% codeblock %}

config rule
        option src *
        option proto tcp
        option dest_port 9091
        option target ACCEPT

{% endcodeblock %}

Reinicie o firewall executando _/etc/init.d/firewall restart_

Para gerenciar seus torrents, instale a extensão [.torrent to Transmission](https://chrome.google.com/webstore/detail/torrent-to-transmission/kfnbjgeekkbjcddceemhjajjiclnoekb) no Chrome, baixe o [Transmission Remote GUI](https://code.google.com/p/transmisson-remote-gui/) para Windows, Mac e Linux ou ainda o [Remote Transmission](https://play.google.com/store/apps/details?id=com.neogb.rtac) para Android.

![Transmission GUI](/images/posts/transmission-gui.jpg)

Caso seu roteador tenha espaço disponível, você pode instalar o pacote _transmission-web_ para gerenciar seus torrents diretamente do navegador.

Ao adicionar um .torrent pelo GUI, o mesmo será baixado diretamente no seu roteador!

## Conclusão

Embora relativamente complicado, todo procedimento para instalação e configuração não é nada diferente do que estamos acostumados a fazer diariamente no Linux, como devops, seja configurando uma VPS ou montando um NAS na rede.

A liberdade que o OpenWRT oferece é imensa. O repositório é recheado com pacotes bem úteis e muito fáceis de configurar. Sem falar na economia comparando com um aparelho como AppleTV (que não ofecererá tantos recursos) ou outros media servers do mercado.

E aí, tem coragem??? Poste sua experiência nos comentários! :)

## Referências

* http://wiki.openwrt.org/doc/howto/usb.storage
* http://wiki.openwrt.org/doc/howto/nfs.server
* http://wiki.openwrt.org/doc/uci/minidlna
* http://wiki.openwrt.org/doc/uci/transmission
* http://wiki.openwrt.org/toh/tp-link/tl-wr1043nd
* http://www.rodrigorodrigues.info/index.php/2012/01/openwrt-fazendo-magica-com-linux-no-roteador-parte-1/
* http://knowhow.bart.prokop.name/install/openwrt/wr1043nd
