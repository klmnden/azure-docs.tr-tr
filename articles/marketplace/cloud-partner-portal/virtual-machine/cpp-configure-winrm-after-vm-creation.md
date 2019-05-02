---
title: Azure sanal makine oluşturulduktan sonra WinRM yapılandırma | Azure Market
description: Azure'da barındırılan bir sanal makine oluşturulduktan sonra Windows Uzaktan Yönetim (WinRM) yapılandırma açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: pabutler
ms.openlocfilehash: 4a4248efcfda76dfd8907069e167fdfa144d0365
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938510"
---
# <a name="configure-winrm-after-virtual-machine-creation"></a>Sanal makine oluşturulduktan sonra WinRM'yi yapılandırın

Bu makalede, var olan Azure'da barındırılan sanal makine'de (VM) HTTPS üzerinden WinRM etkinleştirmek için yapılandırma açıklanmaktadır.  Bu yapılandırma, yalnızca Windows tabanlı Vm'lere uygulanır ve şu iki aşamalı işlemi gerektirir:

1. Bağlantı noktası trafik için WinRM HTTPS protokolü üzerinden etkinleştirin.  Bu ayar, sanal makinenizde Azure portalında yapılandıracaksınız.
2. Sağlanan PowerShell betiklerini çalıştırarak WinRM'yi etkinleştirin. sanal Makinenizi yapılandırın.


## <a name="enabling-port-traffic"></a>Bağlantı noktası trafik etkinleştirme

WinRM HTTPS protokolü üzerinden, Azure Market'te sunulan önceden yapılandırılmış Windows Vm'leri varsayılan olarak etkin değildir 5896, bağlantı noktası kullanır. Bu protokolü etkinleştirmek için yeni bir kural ile ağ güvenlik grubu (NSG) eklemek için aşağıdaki adımları kullanın [Azure portalında](https://portal.azure.com).  Nsg'ler hakkında daha fazla bilgi için bkz. [güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview).

1.  Dikey penceresine gidin **sanal makineler >**  <*vm-adı*>  **> ayarları/ağ**.
2.  NSG adına tıklayın (Bu örnekte, **testvm11002**) özelliklerini görüntülemek için:

    ![Ağ güvenlik grubu özellikleri](./media/nsg-properties.png)
 
3. Altında **ayarları**seçin **gelen güvenlik kuralları** bu dikey penceresini görüntüleyin.
4. Tıklayın **+ Ekle** adlı yeni bir kural oluşturmak için `WinRM_HTTPS` için TCP bağlantı noktası 5986.

    ![Gelen ağ güvenlik Kuralı Ekle](./media/nsg-new-rule.png)

5. Tıklayın **Tamam** değerleri sağlama işlemini tamamladıktan sonra.  Gelen güvenlik kuralları listesinde, aşağıdaki yeni girişleri içermelidir.

    ![Gelen ağ güvenlik kuralları listesi](./media/nsg-new-inbound-listing.png)


## <a name="configure-vm-to-enable-winrm"></a>WinRM etkinleştirmek için VM yapılandırma 

Etkinleştirin ve Windows sanal makinenizde Windows Uzaktan yönetim özelliğini yapılandırmak için aşağıdaki adımları kullanın.   

1. Azure'da barındırılan sanal makinenize Uzak Masaüstü bağlantısı kurun.  Daha fazla bilgi için [nasıl bağlanmak ve bir Azure Windows çalıştıran sanal makine için oturum açma](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).  Kalan adımları, sanal makinenizde çalıştırılacak.
2. Aşağıdaki dosyaları indirmek ve bunları sanal makinenizde bir klasöre kaydedin:
    - [ConfigureWinRM.ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1)
    - [makecert.exe](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe)
    - [winrmconf.cmd](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd)
3. Açık **PowerShell konsolunu** yükseltilmiş ayrıcalıklarla (**yönetici olarak çalıştır**). 
4. Gerekli parametresi sağlayarak aşağıdaki komutu çalıştırın: sanal Makineniz için tam etki alanı adı (FQDN): <br/>
   `ConfigureWinRM.ps1 <vm-domain-name>`

    ![PowerShell yapılandırma betiğini 1](./media/powershell-file1.png)

    Bu betik, aynı klasörde olan diğer iki dosya bağlıdır.


## <a name="next-steps"></a>Sonraki adımlar

WinRM yapılandırdıktan sonra hazır olduğunuz [bağlı kendi vhd'lerden sanal makinenizin dağıtma](./cpp-deploy-vm-vhd.md).
