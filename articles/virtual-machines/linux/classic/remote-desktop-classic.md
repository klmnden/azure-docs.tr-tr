---
title: Bir Linux VM, Uzak Masaüstü'nü | Microsoft Docs
description: Yükleme ve klasik dağıtım modeli için bir Microsoft Azure Linux VM bağlanmak için Uzak Masaüstü yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 0e1bfe468e1572ca98be956d39d82df562dce0e6
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30238622"
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Uzak Masaüstü kullanarak bir Microsoft Azure Linux VM’ye bağlanma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Güncelleştirilmiş Kaynak Yöneticisi bu makalenin sürümü için bkz: [burada](../use-remote-desktop.md).

## <a name="overview"></a>Genel Bakış
RDP (Uzak Masaüstü Protokolü), Windows için kullanılan özel bir protokoldür. Nasıl bir Linux VM (sanal makine) uzaktan bağlanmak için ki RDP kullanabilir?

Bu kılavuz size yanıt verir! Yüklemenizi ve, Microsoft Azure Linux için Uzak Masaüstü ile bir Windows makineden erişmenize olanak sağlayan bir VM'nin üzerinde yapılandırma xrdp yardımcı olur. Bu kılavuz örnekte olarak Ubuntu veya OpenSUSE çalışan Linux VM kullanacağız.

Xrdp aracı bir Windows makineden Uzak Masaüstü'nü Linux sunucunuza bağlanmasına izin veren bir açık kaynak RDP sunucusudur. RDP VNC (sanal ağ bilgisayar) daha iyi performans sahiptir. VNC JPEG kaliteli grafikler kullanarak işler ve RDP hızlı ve Billur iken yavaş olabilir.

> [!NOTE]
> Zaten bir Microsoft Azure Linux çalıştıran VM olması gerekir. Bir Linux VM oluşturup için bkz: [Azure Linux VM'de Öğreticisi](createportal-classic.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Uzak Masaüstü için bir uç noktası oluşturma
Bu belge varsayılan uç noktası 3389 Uzak Masaüstü için kullanacağız. 3389 uç noktası olarak ayarlamanız `Remote Desktop` aşağıdaki gibi Linux VM'ye:

![görüntü](./media/remote-desktop/endpoint-for-linux-server.png)

VM için uç nokta ayarlamayı ayarlama bilmiyorsanız, bkz: [bu kılavuz](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Gnome Masaüstü yükleme
Linux VM bağlanmak `putty`, yükleyip `Gnome Desktop`.

Ubuntu için kullanın:

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop
```

OpenSUSE için kullanın:

```bash
sudo zypper install gnome-session
```

## <a name="install-xrdp"></a>Xrdp yükleyin
Ubuntu için kullanın:

```bash
sudo apt-get install xrdp
```

OpenSUSE için kullanın:

> [!NOTE]
> Aşağıdaki komutu kullanarak sürümüyle OpenSUSE sürüme güncelleştirin. Aşağıdaki örnek içindir `OpenSUSE 13.2`.
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Xrdp başlatın ve önyükleme yukarı xdrp hizmeti ayarlayın
OpenSUSE için kullanın:

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

Ubuntu için xrdp başlatıldı ve olması önyükleme-yukarı yüklemeden sonra otomatik olarak etkinleştirilir.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Ubuntu 12.04LTS'den sonraki bir Ubuntu sürümü kullanıyorsanız xfce kullanma
Xrdp geçerli sürümü Gnome Masaüstü Ubuntu sürümleri için daha sonraki Ubuntu 12.04LTS desteklemediğinden, kullanacağız `xfce` Masaüstü yerine.

Yüklemek için `xfce`, bu komutu kullanın:

```bash
sudo apt-get install xubuntu-desktop
```

Daha sonra etkinleştirin `xfce` bu komutu kullanma:

```bash
echo xfce4-session >~/.xsession
```

Yapılandırma dosyası Düzenle `/etc/xrdp/startwm.sh`:

```bash
sudo vi /etc/xrdp/startwm.sh   
```

Satırı ekleyin `xfce4-session` satırından önce `/etc/X11/Xsession`.

Xrdp hizmetini yeniden başlatmak için bunu kullanın:

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Bir Windows makineden Linux VM Bağlan
Bir Windows makinesine uzak masaüstü istemcisini başlatın ve Linux VM DNS adı girin. Veya Azure portalında, VM panoya gidin ve tıklayın `Connect` Linux VM'nize bağlanmak için. Bu durumda, oturum açma penceresi görürsünüz:

![görüntü](./media/remote-desktop/no2.png)

Kullanıcı adı ve parola, Linux VM oturum.

## <a name="next-steps"></a>Sonraki adımlar
Xrdp kullanma hakkında daha fazla bilgi için bkz: [ http://www.xrdp.org/ ](http://www.xrdp.org/).
