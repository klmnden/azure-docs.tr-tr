---
title: Yük devretme Azure hataları için sorun giderme | Microsoft Docs
description: Bu makalede, azure'a yük devretme, sık karşılaşılan sorunları giderme yolları açıklanır.
author: ponatara
manager: abhemraj
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/04/2019
ms.author: mayg
ms.openlocfilehash: 2156ee6cf27ecfa32b19ad5bbef7549e99c3f7ef
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61280662"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>VMware VM veya fiziksel makinenin azure'a yük devri sırasında karşılaşılan sorunları giderme

Azure'da bir sanal makine yük devretmesi yaparken aşağıdaki hatalardan birini alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.

## <a name="failover-failed-with-error-id-28031"></a>Hata kimliği 28031 yük devretme başarısız oldu

Site kurtarma, başarısız bir yük devredilen Azure sanal makine oluşturmak ulaşamadı. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makine oluşturmak kullanılabilir yeterli kotası yoktur: Kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](https://aka.ms/getazuresupport) Kotayı artırmak için.

* Yük devretme sanal makineler aynı kullanılabilirlik kümesindeki farklı boyutta ailelerinin deniyorsunuz. Aynı kullanılabilirlik kümesindeki tüm sanal makineler için aynı boyut ailesi seçtiğinizden emin olun. Sanal makinenin işlem ve ağ ayarlarına giderek boyutunu değiştirin ve ardından yük devretmeyi yeniden deneyin.

* Bir sanal makine oluşturulmasını önleyen bir abonelikte bir ilke yoktur. Sanal makine oluşturmaya izin ver ve ardından yük devretmeyi yeniden deneyin ilkesini değiştirin.

## <a name="failover-failed-with-error-id-28092"></a>Hata kimliği 28092 yük devretme başarısız oldu

Site kurtarma, başarısız bir ağ arabirimi oluşturmak mümkün değildi yük devredilen sanal makine. Aboneliğindeki ağ arabirimlerini oluşturmak kullanılabilir yeterli kotası olduğundan emin olun. Kullanılabilir kota aboneliğine giderek denetleyebilirsiniz -> kullanım ve kotalar. Açabileceğiniz bir [yeni destek isteği](https://aka.ms/getazuresupport) Kotayı artırmak için. Yeterli kotanız sonra bu bir aralıklı olabilir gönderme, işlemi yeniden deneyin. Ardından denemelere sorun devam ederse, bu belgenin sonunda bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Hata kimliği 70038 yük devretme başarısız oldu

Site Recovery Azure'da Klasik sanal makine üzerinde başarısız oluşturmak mümkün değildi. Nedeniyle oluşabilir:

* Oluşturulacak sanal makine için gerekli olan bir sanal ağ gibi kaynaklardan biri mevcut değil. Sanal makinenin işlem ve ağ ayarlarında sağlanan sanal ağ oluşturma veya bir sanal ağ zaten var ve ardından yük devretmeyi yeniden deneyin ayarını değiştirin.

## <a name="failover-failed-with-error-id-170010"></a>Hata kimliği 170010 ile yük devretme başarısız oldu

Site kurtarma, başarısız bir yük devredilen Azure sanal makine oluşturmak ulaşamadı. Şirket içi sanal makine için bir iç hidrasyonu etkinliğin başarısız olduğundan meydana gelmiş olabilir.

Azure'da herhangi bir makineye getirmek için bazı sürücüler önyükleme olmasını başlangıç durumu ve DHCP autostart durumunda olması gibi hizmetler Azure ortamına gerektirir. Bu nedenle, yük devretme sırasındaki hidrasyonu etkinlik başlangıç türünü dönüştürür **atapi, intelide, storflt, vmbus ve storvsc sürücüleri** önyükleme başlatmak için. Ayrıca DHCP gibi birkaç hizmet başlangıç türünü otomatik başlatma için dönüştürür. Bu etkinlik belirli ortam sorunları nedeniyle başarısız olabilir. 

El ile sürücüleri için başlangıç türünü değiştirmek için **Windows konuk işletim sistemi**, izleyerek aşağıdaki adımları:

1. [İndirme](https://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) çalıştırma ve Hayır hidrasyonu betik olarak izler. Bu betik, VM hidrasyonu gerektirip gerektirmediğini denetler.

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

Bu, normalde endişeye neden değildir ve planlanmamış yük devretmeler için genellikle yok sayılabilir. Yük devretme planlanmış makine düzgün bir şekilde yük devretme öncesinde kapatıldığında emin olun ve çoğaltma verileri Azure'a gönderilecek şirket bekleyen yeterli zaman sağlamalısınız. Ardından **son** seçeneğini [yük devretme ekran](site-recovery-failover.md#run-a-failover) böylece sonra VM yük devretmesi için kullanılan bir kurtarma noktasına azure'da bekleyen tüm veriler işlenir.

## <a name="unable-to-select-the-datastore"></a>Veri deposu seçmek oluşturulamıyor

Bu sorun, veri deposu Azure portalında bir yük devretmeyle sanal makineyi yeniden korumak çalışırken görmek kaldıramadığınızda gösterilir. Bunun nedeni, asıl hedef, Azure Site Recovery hizmetine eklenen vCenters altında bir sanal makine olarak tanınmıyor.

Bir sanal makine yeniden korunuyor hakkında daha fazla bilgi için bkz. [yeniden koruma ve bir şirket içi siteye geri makineleri, azure'a yük devredildikten sonra başarısız](vmware-azure-reprotect.md).

Bu sorunu çözmek için:

Ana el ile oluşturmanız, kaynak makinenin yöneten vcenter hedef. Veri deposu sonraki vCenter bulma ve yenileme fabric işlemlerinden sonra kullanılabilir.

> [!Note]
> 
> Bulma ve yenileme fabric işlemlerinin tamamlanması 30 dakika sürebilir. 

## <a name="linux-master-target-registration-with-cs-fails-with-an-ssl-error-35"></a>Bir SSL hatası 35 CS ile Linux ana hedef kaydı başarısız 

Yapılandırma sunucusu ile Azure Site Recovery ana hedef kaydını ana hedefte etkinleştiriliyor kimliği doğrulanmış Proxy nedeniyle başarısız olur. 
 
Bu hata, yükleme günlüğünde aşağıdaki dizeleri ile belirtilir: 

```
RegisterHostStaticInfo encountered exception config/talwrapper.cpp(107)[post] CurlWrapper Post failed : server : 10.38.229.221, port : 443, phpUrl : request_handler.php, secure : true, ignoreCurlPartialError : false with error: [at curlwrapperlib/curlwrapper.cpp:processCurlResponse:231]   failed to post request: (35) - SSL connect error. 
```

Bu sorunu çözmek için:
 
1. Yapılandırma sunucusundaki sanal makine bir komut istemi açın ve aşağıdaki komutları kullanarak proxy ayarlarını doğrulayın:

    cat /etc/environment Yankı $http_proxy $erişmek echo 

2. Önceki komutların çıktısı http_proxy veya erişmek ayarları tanımlanır gösteriyorsa, yapılandırma sunucusu ile ana hedef iletişim engelini kaldırmak için aşağıdaki yöntemlerden birini kullanın:
   
   - İndirme [PsExec aracı](https://aka.ms/PsExec).
   - Sistem kullanıcı bağlamı erişmek ve proxy adresi yapılandırılıp yapılandırılmadığını belirlemek için Aracı'nı kullanın. 
   - Proxy yapılandırılmışsa, IE açın PsExec Aracı'nı kullanarak bir sistem kullanıcı bağlamında.
  
     **psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"**

   - Ana hedef sunucusunun yapılandırma sunucusu ile iletişim kurabildiğinden emin olun için:
  
     - Internet Explorer'ın ana hedef sunucu IP adresi proxy üzerinden atlamak için proxy ayarlarını değiştirin.   
     Or
     - Ana hedef sunucusundaki proxy devre dışı bırakın. 


## <a name="next-steps"></a>Sonraki adımlar
- Sorun giderme [Windows VM ile RDP bağlantısı](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Sorun giderme [Linux VM'ye SSH bağlantısı](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Daha fazla yardıma ihtiyacınız olursa, ardından sorgunuza gönderin [Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veya bu belgenin sonunda bir yorum yazın. Gereken yardımcı olması etkin bir topluluk sahibiz.
