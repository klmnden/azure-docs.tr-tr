---
title: Azure VM konuk işletim sistemi güvenlik duvarı yanlış yapılandırılmış. | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: fcea5e4e6bb108f1a8d8036e51a5dae8a9e6431b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711025"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>Azure VM konuk işletim sistemi güvenlik duvarı yanlış yapılandırılmış

Bu makalede, yanlış yapılandırılmış konuk işletim sistemi güvenlik duvarı Azure sanal makinesinde nasıl tanıtır.

## <a name="symptoms"></a>Belirtiler

1.  VM tam olarak yüklendiğinden, sanal makine (VM) Karşılama ekranı gösterilir.

2.  Konuk işletim sisteminin nasıl yapılandırıldığına bağlı olarak olabilir VM ulaşmasını bazı veya hiçbir ağ trafiği.

## <a name="cause"></a>Nedeni

Konuk sistemi Güvenlik Duvarı'nın hatalı yapılandırılması bazı veya tüm VM ağ trafiği türlerinin engelleyebilirsiniz.

## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce sistem diski etkilenen sanal makinenin anlık görüntüsünü yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

Bu sorunu gidermek için seri konsolu veya [çevrimdışı VM'yi onarın](troubleshoot-rdp-internal-error.md#repair-the-vm-offline) sanal Makinenin sistem diskini bir kurtarma sanal Makinesine ekleyerek.

## <a name="online-mitigations"></a>Çevrimiçi bir risk azaltma işlemleri

Bağlanma [seri konsolu ve bir PowerShell örneği açın](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Seri konsol VM üzerinde etkin değilse, "Onarım VM çevrimdışı" bölümüne aşağıdaki Azure makalesini gidin:

 [Uzak Masaüstü aracılığıyla Azure VM'ye bağlanmaya çalışırken bir iç hata oluşur.](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)

Aşağıdaki kuralları ya da VM (RDP) üzerinden erişim sağlamak veya sorun giderme daha kolay bir deneyim sağlamak üzere düzenlenebilir:

*   Uzak Masaüstü (TCP-Gelen): Bu birincil erişim sağlayan VM Azure'da RDP vererek standart bir kuraldır.

*   Windows Uzaktan Yönetim (HTTP-gelen): PowerShell, Azure, bu tür bir erişim'ı kullanarak sanal Makineye bağlanmanızı sağlar bu kural uzak komut dosyası ve sorun giderme komut dosyası çalıştırma kullanılmasına izin verir.

*   Dosya ve Yazıcı Paylaşımı (SMB-gelen): Bu kural, bir sorun giderme seçeneği olarak ağ paylaşımı erişimi sağlar.

*   Dosya ve Yazıcı Paylaşımı (yankı isteği - Icmpv4 gelen): Bu kural, VM ping olanak tanır.

Seri konsol erişimi örnekte, güvenlik duvarı kuralı geçerli durumunu sorgulayabilirsiniz.

*   Bir parametre görünen ad'ı kullanarak sorgu:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(DisplayName.*<FIREWALL RULE NAME>)" -context 9,4 | more
    ```

*   Yerel uygulama tarafından kullanılan bağlantı noktasını kullanarak sorgu:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<APPLICATION PORT>)" -context 9,4 | more
    ```

*   Uygulamanın kullandığı yerel IP adresi kullanarak sorgu:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalIP.*<CUSTOM IP>)" -context 9,4 | more
    ```

*   Kuralı devre dışı olduğunu görürseniz, aşağıdaki komutu çalıştırarak etkinleştirebilirsiniz:

    ```cmd
    netsh advfirewall firewall set rule name="<RULE NAME>" new enable=yes
    ```

*   Sorun giderme için güvenlik duvarı profili kapatabilirsiniz:

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Güvenlik Duvarı doğru şekilde ayarlanması bunu yaparsanız, sorun giderme tamamladıktan sonra Güvenlik Duvarı'nı yeniden etkinleştirin.

    > [!Note]
    > Bu değişikliği uygulamak için VM'yi yeniden başlatma gerekmez.

*   VM RDP aracılığıyla bağlanmak bir kez daha deneyin.

### <a name="offline-mitigations"></a>Çevrimdışı bir risk azaltma işlemleri

1.  Etkinleştirmek veya güvenlik duvarı kurallarını devre dışı bırakmak için başvurmak [etkinleştirmek veya devre dışı bir güvenlik duvarı kuralı, bir Azure VM konuk işletim sistemine](enable-disable-firewall-rule-guest-os.md).

2.  İçinde olup olmadığını denetleyin [konuk işletim sistemi güvenlik duvarı engelleme gelen trafiği senaryo](guest-os-firewall-blocking-inbound-traffic.md).

3.  Erişiminizi güvenlik duvarı olup engelliyor hakkında şüpheli hala açıksa, başvurmak [konuk işletim sistemi güvenlik duvarı Azure VM'de devre dışı](disable-guest-os-firewall-windows.md)ve ardından Konuk sistemi güvenlik duvarı doğru kurallarını kullanarak yeniden etkinleştirin.

