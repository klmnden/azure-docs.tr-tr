---
title: Yük devretme Azure hataları için sorun giderme | Microsoft Docs
description: Bu makalede, azure'a yük devretme, sık karşılaşılan sorunları giderme yolları açıklanır.
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: da5ae8220c63333b92c671a3f2ab288ef5ff4b48
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679087"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Bir sanal makinenin azure'a yük devri sırasında karşılaşılan sorunları giderme

Azure'da bir sanal makine yük devretmesi yaparken aşağıdaki hatalardan birini alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Hata kimliği 28031 yük devretme başarısız oldu

Site kurtarma, başarısız bir yük devredilen Azure sanal makine oluşturmak ulaşamadı. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makine oluşturmak kullanılabilir yeterli kotası yoktur: kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için.

* Yük devretme sanal makineler aynı kullanılabilirlik kümesindeki farklı boyutta ailelerinin deniyorsunuz. Aynı kullanılabilirlik kümesindeki tüm sanal makineler için aynı boyut ailesi seçtiğinizden emin olun. Sanal makinenin işlem ve ağ ayarlarına giderek boyutunu değiştirin ve ardından yük devretmeyi yeniden deneyin.

* Bir sanal makine oluşturulmasını önleyen bir abonelikte bir ilke yoktur. Sanal makine oluşturmaya izin ver ve ardından yük devretmeyi yeniden deneyin ilkesini değiştirin.

## <a name="failover-failed-with-error-id-28092"></a>Hata kimliği 28092 yük devretme başarısız oldu

Site kurtarma, başarısız bir ağ arabirimi oluşturmak mümkün değildi yük devredilen sanal makine. Aboneliğindeki ağ arabirimlerini oluşturmak kullanılabilir yeterli kotası olduğundan emin olun. Kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için. Yeterli kotanız sonra bu bir aralıklı olabilir gönderme, işlemi yeniden deneyin. Ardından denemelere sorun devam ederse, bu belgenin sonunda bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Hata kimliği 70038 yük devretme başarısız oldu

Site Recovery Azure'da Klasik sanal makine üzerinde başarısız oluşturmak mümkün değildi. Nedeniyle oluşabilir:

* Oluşturulacak sanal makine için gerekli olan bir sanal ağ gibi kaynaklardan biri mevcut değil. Sanal makinenin işlem ve ağ ayarlarında sağlanan sanal ağ oluşturma veya bir sanal ağ zaten var ve ardından yük devretmeyi yeniden deneyin ayarını değiştirin.

## <a name="failover-failed-with-error-id-170010"></a>Hata kimliği 170010 ile yük devretme başarısız oldu

Site kurtarma, başarısız bir yük devredilen Azure sanal makine oluşturmak ulaşamadı. Şirket içi sanal makine için bir iç hidrasyonu etkinliğin başarısız olduğundan meydana gelmiş olabilir.

Azure'da herhangi bir makineye getirmek için bazı sürücüler önyükleme olmasını başlangıç durumu ve DHCP autostart durumunda olması gibi hizmetler Azure ortamına gerektirir. Bu nedenle, yük devretme sırasındaki hidrasyonu etkinlik başlangıç türünü dönüştürür **atapi, intelide, storflt, vmbus ve storvsc sürücüleri** önyükleme başlatmak için. Ayrıca DHCP gibi birkaç hizmet başlangıç türünü otomatik başlatma için dönüştürür. Bu etkinlik belirli ortam sorunları nedeniyle başarısız olabilir. Sürücüleri başlatma türü el ile değiştirmeniz için izleyin aşağıdaki adımları:

1. [İndirme](http://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) çalıştırma ve Hayır hidrasyonu betik olarak izler. Bu betik, VM hidrasyonu gerektirip gerektirmediğini denetler.

    `.\Script-no-hydration.ps1`

    Hydration gerekiyorsa aşağıdaki sonucu verir:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    VM yok hidrasyonu gereksinimini karşılar durumunda, komut dosyası "Bu sistemi Hayır hidrasyonu gereksinimini karşılar" sonucu verecektir. Bu durumda, tüm sürücüleri ve Hizmetleri Azure gerektirdiği durumunda olan ve hydration VM'de gerekli değildir.

2. VM yok hidrasyonu gereksinimi karşılamıyorsa Hayır hidrasyonu kümesi betiği aşağıdaki gibi çalıştırın.

    `.\Script-no-hydration.ps1 -set`
    
    Bu sürücüleri başlangıç türünü dönüştürülür ve sonucu verecektir aşağıdaki gibi:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Bağlama/RDP veya SSH ile başarısız kurulamıyor sanal makinede bağlantı düğmesi gri nedeniyle sanal makine üzerinde

Varsa **Connect** yük devredilen VM azure'da düğmesi gri ve Azure'a bir Express Route veya siteden siteye VPN bağlantısı aracılığıyla, daha sonra bağlanmamış

1. Git **sanal makine** > **ağ**, gerekli ağ arabiriminin adına tıklayın.  ![Ağ arabirimi](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Gidin **IP yapılandırmaları**, gerekli IP yapılandırması ad alanında bulunan'ye tıklayın. ![IP yapılandırmaları](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Genel IP adresi etkinleştirmek için tıklayın **etkinleştirme**. ![IP etkinleştir](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Tıklayarak **gerekli ayarları Yapılandır** > **Yeni Oluştur**. ![Yeni Oluştur](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Ortak adres adını girin, için varsayılan seçenekleri seçin **SKU** ve **atama**, ardından **Tamam**.
6. Şimdi, yaptığınız değişiklikleri kaydetmek için tıklatın **Kaydet**.
7. Paneller kapatın ve gidin **genel bakış** sanal makineye bağlanma/RDP bölümü.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Bağlama/RDP/SSH - VM Bağlan düğmesi kullanılabilir oluşturulamıyor

Varsa **Connect** devredilen VM'nin azure'da düğmesine (gri değil) kullanılabilir, ardından denetleyin **önyükleme tanılaması** listelenen hataları denetleyin ve sanal makine üzerinde [bu makalede](../virtual-machines/windows/boot-diagnostics.md).

1. Sanal makine başlatılmamışsa daha eski bir kurtarma noktasına yük devretmeyi deneyin.
2. Sanal makinenin içindeki uygulama devretmeyi deneyin bir uygulama ile tutarlı kurtarma noktasına değilse.
3. Sanal Makine etki alanına katılmış ise, etki alanı denetleyicisinin doğru şekilde çalıştığından emin olun. Bu takip ederek yapılabilir adımları aşağıda verilen:

    a. Aynı ağda yeni bir sanal makine oluşturun.

    b.  aynı etki alanına katılacak şekilde tutabileceğinden emin başarısız yük devredilen sanal makinenin görünmesi beklenen.

    c. Etki alanı denetleyicisi ise **değil** doğru bir şekilde çalışmasını daha sonra deneyin başarısız'nda oturum açtıktan sonra bir yerel yönetici hesabı kullanarak sanal makine üzerinde.
4. Özel bir DNS sunucusu kullanıyorsanız erişilebilir olduğundan emin olun. Bu takip ederek yapılabilir adımları aşağıda verilen:

    a. Aynı ağda yeni bir sanal makine oluşturma ve

    b. sanal makineyi özel DNS sunucusunu kullanarak ad çözümlemesi mümkün olup olmadığını denetleyin

>[!Note]
>Önyükleme tanılama dışındaki herhangi bir ayarı etkinleştirerek Azure VM Aracısı, yük devretmeden önce sanal makinede yüklü olmasını gerektirir

## <a name="unexpected-shutdown-message-event-id-6008"></a>Beklenmeyen kapatma iletisi (Olay Kimliği 6008)

Bir Windows VM yük devretme sonrasında, kurtarılan bir VM üzerinde bir beklenmeyen şekilde kapanması iletisi alırsanız, önyükleme yaparken, bir VM kapatma durumuna, yük devretme için kullanılan kurtarma noktasında yakalanamadı gösterir. VM tümüyle kapatılmış değil, bir noktaya kurtarma kullandığınızda ortaya çıkar.

Bu, normalde endişeye neden değildir ve planlanmamış yük devretmeler için genellikle yok sayılabilir. Planlı bir yük devretme durumunda makine düzgün bir şekilde yük devretme öncesinde kapatıldığında emin olun ve çoğaltma verileri Azure'a gönderilecek şirket bekleyen yeterli zaman sağlayın. Ardından **son** seçeneğini [yük devretme ekran](site-recovery-failover.md#run-a-failover) böylece sonra VM yük devretmesi için kullanılan bir kurtarma noktasına azure'da bekleyen tüm veriler işlenir.

## <a name="retaining-drive-letter-after-failover"></a>Yük devretmeden sonra sürücü harfi korunuyor
Yük devretmeden sonra sanal makinelerde sürücü harfini korumak için ayarlayabileceğiniz **SAN ilkesinin** için sanal makine şirket içine **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="next-steps"></a>Sonraki adımlar
- Sorun giderme [Windows VM ile RDP bağlantısı](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Sorun giderme [Linux VM'ye SSH bağlantısı](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Daha fazla yardıma ihtiyacınız olursa, ardından sorgunuza gönderin [Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veya bu belgenin sonunda bir yorum yazın. Gereken yardımcı olması etkin bir topluluk sahibiz.
