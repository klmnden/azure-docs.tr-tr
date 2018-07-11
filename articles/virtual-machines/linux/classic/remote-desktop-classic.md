---
title: Bir Linux VM'ye Uzak Masaüstü | Microsoft Docs
description: Yükleme ve klasik dağıtım modeli için bir Microsoft Azure Linux VM bağlanmak için Uzak Masaüstü yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 5e68774c3edb7d82fef388c593a6b96c52857be6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927749"
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Uzak Masaüstü kullanarak bir Microsoft Azure Linux VM’ye bağlanma
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager sürümünü Bu makale için bkz. [burada](../use-remote-desktop.md).

## <a name="overview"></a>Genel Bakış
RDP (Uzak Masaüstü Protokolü), Windows için kullanılan özel bir protokoldür. Bir Linux VM (sanal makine) uzaktan bağlanmak için RDP nasıl kullanabileceğimizi?

Bu kılavuz size yanıt verir! Yükleme ve yapılandırma xrdp için Uzak Masaüstü ile bir Windows makineden bağlanmanızı sağlayan Microsoft Azure Linux VM, şirket yardımcı olur. Bu kılavuzdaki örnek olarak, Ubuntu veya OpenSUSE çalışan Linux VM kullanacağız.

Bir Windows makineden Uzak Masaüstü ile Linux sunucunuza bağlanmasına olanak sağlayan bir açık kaynak RDP sunucusuna xrdp aracıdır. RDP VNC (sanal ağ hesaplama) daha iyi performans sahiptir. VNC JPEG kalitede grafiklere kullanarak oluşturur ve RDP hızlı ve kristal Temizle iken yavaş olabilir.

> [!NOTE]
> Zaten bir Microsoft Azure Linux çalıştıran VM olması gerekir. Oluşturma ve bir Linux VM ayarlama hakkında bilgi için bkz: [Azure Linux VM öğretici](createportal-classic.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Uzak Masaüstü için bir uç nokta oluşturma
Bu belgedeki varsayılan uç nokta 3389 Uzak Masaüstü için kullanacağız. 3389 uç noktası olarak ayarlama `Remote Desktop` aşağıdaki gibi kullanarak Linux vm'nize:

![image](./media/remote-desktop/endpoint-for-linux-server.png)

Sanal Makineniz için uç nokta ayarlamayı ayarlama işlemini bilmiyorsanız, bkz. [bu kılavuz](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Gnome Masaüstü yükleme
Linux VM'nize bağlanmak `putty`, yükleyip `Gnome Desktop`.

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
> Aşağıdaki komutta kullanmakta olduğunuz sürümle OpenSUSE sürümüne güncelleştirin. Aşağıdaki örnekte içindir `OpenSUSE 13.2`.
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Xrdp başlatın ve önyüklemede xdrp hizmetini ayarlama
OpenSUSE için kullanın:

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

Ubuntu için xrdp başlatılabilir ve önyüklemede yüklemeden sonra otomatik olarak etkinleştirilir.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Ubuntu 12.04LTS daha sonra bir Ubuntu sürümü kullanıyorsanız xfce kullanma
Geçerli sürümü xrdp Gnome Masaüstü Ubuntu sürümleri için Ubuntu 12.04LTS daha desteklemediğinden, kullanacağız `xfce` Masaüstü yerine.

Yüklenecek `xfce`, bu komutu kullanın:

```bash
sudo apt-get install xubuntu-desktop
```

Ardından etkinleştirmeniz `xfce` bu komutu kullanarak:

```bash
echo xfce4-session >~/.xsession
```

Config dosyasını düzenleme `/etc/xrdp/startwm.sh`:

```bash
sudo vi /etc/xrdp/startwm.sh   
```

Satır Ekle `xfce4-session` satırından önce `/etc/X11/Xsession`.

Xrdp hizmetini yeniden başlatmak için bunu kullanın:

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Bir Windows makineden Linux VM'nize bağlanın
Bir Windows makinede uzak masaüstü istemcisini başlatın ve Linux VM DNS adınızı girin. Veya Azure portalında sanal makinenizin panoya gidin ve `Connect` Linux VM'nize bağlanmak için. Bu durumda, oturum açma penceresi görürsünüz:

![image](./media/remote-desktop/no2.png)

Kullanıcı adı ve parolayla Linux vm'nizin oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
Xrdp kullanma hakkında daha fazla bilgi için bkz. [ http://www.xrdp.org/ ](http://www.xrdp.org/).
