---
title: Bir Azure VM'ye bağlanın, Uzak Masaüstü lisans sunucusu kullanılamıyor | Microsoft Docs
description: Hiçbir Uzak Masaüstü lisans sunucusu kullanılabilir olmadığından RDP başarısız ilgili sorunları giderme hakkında bilgi edinin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 550b971602d1736e0ba3981a5b7ca546862ea034
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318961"
---
# <a name="remote-desktop-license-server-isnt-available-when-you-connect-to-an-azure-vm"></a>Bir Azure VM'ye bağlanın, Uzak Masaüstü lisans sunucusu kullanılamıyor

Bu makale, Azure sanal makinesi (VM) için bir lisans sağlanabilecek Uzak Masaüstü lisans sunucusu yok olduğundan bağlanamadığında sorunu yardımcı olur.

## <a name="symptoms"></a>Belirtiler

Bir sanal makineye (VM) bağlanmaya çalıştığınızda, aşağıdaki senaryolarda karşılaşırsınız:

- VM ekran görüntüsü, işletim sistemini tam olarak yüklenir ve kimlik bilgileri bekleniyor olduğunu gösterir.
- Microsoft Uzak Masaüstü Protokolü (RDP) bağlantısı kurmaya çalışırken aşağıdaki hata iletileri alırsınız:

  - Lisans sağlanabilecek Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi.

  - Hiçbir Uzak Masaüstü lisans sunucusu kullanılabilir. Uzak Masaüstü Hizmetleri, bu bilgisayar, yetkisiz kullanım süresi ve geçerli en az bir Windows Server 2008 Lisans sunucusu için bağlantı kurmadı çalışmayı durdurur. Lisans Tanılama kullanmak için RD Oturumu Ana Bilgisayar Sunucu Yapılandırması'nı açmak için bu iletiyi seçin.

Ancak, yönetici oturumu kullanarak VM normalde bağlanabilirsiniz:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Nedeni

Uzak oturumu başlatmak için lisans sağlamak bir Uzak Masaüstü lisans sunucusu kullanılamıyorsa, bu sorun oluşur. Uzak Masaüstü Oturumu Ana Bilgisayarı rol VM üzerinde ayarlanmış olsa bile birkaç senaryo tarafından kaynaklanabilir:

- Ortamda hiçbir zaman bir Uzak Masaüstü lisans rol vardı ve 180 gün bittikten yetkisiz kullanım süresi.
- Uzak Masaüstü lisans ortamında yüklendi ancak hiçbir zaman etkinleştirilir.
- Bir Uzak Masaüstü Lisansı ortamında, istemci erişim lisansları (CAL) bağlantıyı ayarlamak için eklenmiş yok.
- Bir Uzak Masaüstü Lisansı ortama yüklenmiştir. Kullanılabilir CAL'ler mevcuttur ancak düzgün yapılandırılmış olmayan.
- Uzak Masaüstü lisans CAL'ler mevcuttur ve bu etkinleştirildi. Ancak, diğer bazı sorunları Uzak Masaüstü lisans sunucusu üzerinde bu lisansları ortamında vermesini engeller.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için [işletim sistemi diskini yedekleme](../windows/snapshot-copy-managed-disk.md) ve aşağıdaki adımları izleyin:

1. Yönetici oturumu kullanarak VM'ye bağlanın:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

    Yönetici oturumu kullanarak sanal Makineye bağlanamıyorsanız, kullanabileceğiniz [azure'da sanal makine seri Konsolu](serial-console-windows.md) gibi sanal Makineye erişmek için:

    1. Seri konsol seçerek erişim **destek ve sorun giderme** > **seri konsol (Önizleme)** . VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

    2. CMD örneği için yeni bir kanal oluşturun. Girin **CMD** kanalı başlatmak ve kanal adını almak için.

    3. CMD örneğini çalıştıran kanala geçin. Bu durumda, kanal 1 olmalıdır:

       ```
       ch -si 1
       ```

    4. Seçin **Enter** yeniden ve sanal makine için geçerli bir kullanıcı adı ve parola, yerel veya etki alanı kimliği girin.

2. VM'ye bir Uzak Masaüstü oturumu ana bilgisayarı rolü etkin olup olmadığını denetleyin. Rol etkinleştirilirse, düzgün çalıştığından emin olun. Yükseltilmiş bir komut örneği açın ve aşağıdaki adımları izleyin:

    1. Uzak Masaüstü Oturumu Ana Bilgisayarı rol durumunu denetlemek için aşağıdaki komutu kullanın:

       ```
        reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
        ```

        Bu komut, 0 değerini döndürürse, bu rolü devre dışıdır ve 3. adımına gidebilirsiniz anlamına gelir.

    2. İlkeleri denetlemek ve gerektiği şekilde yeniden yapılandırmak için aşağıdaki komutu kullanın:

       ```
        reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
       ```

        Varsa **LicensingMode** değeri 4, kullanıcı başına dışındaki herhangi bir değere ayarlayın. sonra 4'e ayarlayın:

         ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
        ```

       Varsa **SpecifiedLicenseServers** değeri yok ya da yanlış lisans sunucusu bilgilerini içerir, aşağıdaki gibi değiştirin:

       ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
       ```

    3. Kayıt defterinde değişiklik yaptıktan sonra VM'yi yeniden başlatın.

    4. CAL yoksa, Uzak Masaüstü oturumu ana bilgisayarı rolü kaldırın. Ardından RDP geri normal olarak ayarlanır. Yalnızca VM için iki eş zamanlı RDP bağlantılarına izin verir:

        ```
       dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
        ```

        VM'ye Uzak Masaüstü lisans rol varsa ve kullanılan değil, ayrıca bu rolü kaldırabilirsiniz:

       ```
        dism /ONLINE /Disable-feature /FeatureName:Licensing
       ```

    5. VM'yi Uzak Masaüstü lisans sunucusuna bağlanabildiğinden emin olun. VM lisans sunucusu arasında bağlantı noktası 135 bağlanabilirliği test edebilirsiniz: 

       ```
       telnet <FQDN / IP License Server> 135
       ```

3. Ortamında Uzak Masaüstü lisans sunucusu yok ve bir istiyorsanız yapabilecekleriniz [bir Uzak Masaüstü Lisansı rol hizmeti yükleme](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11)). Ardından [RDS lisansı yapılandırmak](https://blogs.technet.microsoft.com/askperf/2013/09/20/rd-licensing-configuration-on-windows-server-2012/).

4. Uzak Masaüstü lisans sunucusu yapılandırılması ve iyi durumda ise, Uzak Masaüstü lisans sunucusu ile CAL'leri etkinleştirildiğinden emin olun.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) çözümlenen sorununuzun için.
