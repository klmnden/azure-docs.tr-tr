---
title: Hyper-V, Azure Site Recovery ile Azure çoğaltma sorunlarını giderme | Microsoft Docs
description: Açıklar nasıl Hyper-V ile Azure Site Kurtarma'yı kullanarak Azure çoğaltma sorunlarını gidermek için
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 04/09/2018
ms.author: rayne
ms.openlocfilehash: 95a33c80b1aeef7fbf8bea0ab760bbd66babdac8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31427238"
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Hyper-V Azure çoğaltma ve yük devretme için sorun giderme

Bu makalede çoğaltma Hyper-V Vm'lerini azure'a, şirket içi karşılaşabileceğiniz ortak sorunları kullanarak [Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Koruma sorunları etkinleştir

Hyper-V VM'ler için korumayı etkinleştirdiğinizde sorunlarıyla karşılaşırsanız aşağıdakileri denetleyin:

1. Hyper-V konakları ve sanal makineleri tüm ile uyumlu onay [gereksinimleri ve Önkoşullar](hyper-v-azure-support-matrix.md).
2. Hyper-V sunucuları System Center Virtual Machine Manager (VMM) bulutlarında yer alıyorsa, hazırladığınız doğrulayın [VMM sunucusu](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Hyper-V konaklarında Hyper-V sanal makine Yönetimi hizmetinin çalışıp çalışmadığını denetleyin.
4. VM üzerinde Hyper-V-VMMS\Admin günlüğünde görünmesini sorunları olup olmadığını denetleyin. Bu günlük bulunan **uygulama ve hizmet günlükleri** > **Microsoft** > **Windows**.
5. Konuk sanal makinede WMI etkin ve erişilebilir olduğunu doğrulayın.
  - [Hakkında bilgi edinin](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) temel WMI sınama.
  - [Sorun giderme](https://aka.ms/WMiTshooting) WMI.
  - [Sorun giderme ](https://technet.microsoft.com/library/ff406382.aspx#H22) WMI komut dosyaları ve Hizmetleri ile ilgili sorunları.
5. Konuk sanal makinede Integration Services'ın en son sürümünü çalıştırdığından emin olun.
    - [Denetleme](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) en son sürüme sahip.
    - [Tutmak](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) güncel Integration Services.
    
## <a name="replication-issues"></a>Çoğaltma sorunları

Aşağıdaki gibi ilk ve devam eden çoğaltma sorunlarını giderme:

1. Çalıştırdığınız emin olun [en son sürümünü](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) Site kurtarma Hizmetleri.
2. Çoğaltma duraklatıldı olup olmadığını doğrulayın:
  - Hyper-V Yöneticisi konsolunda VM sistem durumunu denetleyin.
  - Kritik öneme sahipse VM'ye sağ tıklayın > **çoğaltma** > **çoğaltma durumunu görüntüle**.
  - Çoğaltma duraklatıldı tıklatmak **çoğaltmayı devam ettir**.
3. Gerekli hizmetlerin çalıştığından emin olun. Yoksa, bunları yeniden başlatın.
    - Hyper-V VMM olmadan çoğaltıyorsanız bu hizmetleri Hyper-V ana bilgisayarda çalıştığından emin olun:
        - Sanal Makine Yönetimi Hizmeti
        - Microsoft Azure kurtarma Hizmetleri Aracısı hizmeti
        - Microsoft Azure Site Recovery hizmeti
        - WMI sağlayıcısı ana bilgisayar hizmeti
    - VMM ile ortamda çoğaltıyorsanız hizmetlerin çalıştığından emin olun:
        - Hyper-V konağı üzerinde sanal makine Yönetimi hizmeti, Microsoft Azure kurtarma Hizmetleri aracısı ve WMI sağlayıcısı ana bilgisayar hizmetinin çalıştığını denetleyin.
        - VMM sunucusunda System Center Virtual Machine Manager hizmetinin çalıştığından emin olun.
4. Hyper-V sunucusu ve Azure arasındaki bağlantıyı denetleyin. Bunu yapmak için Hyper V konakta Görev Yöneticisi'ni açın. Üzerinde **performans** sekmesini tıklatın, **açık Kaynak İzleyicisi**. Üzerinde **ağ** sekmesini > **Processess ağ etkinliği ile**, cbengine.exe büyük veri birimleri (MB) etkin bir şekilde gönderme olup olmadığını denetleyin.
5. Hyper-V konakları Azure storage blobu URL'sine bağlanabildiğinizi denetleyin. Bunu yapmak için seçin ve denetleme **cbengine.exe**. Görünüm **TCP bağlantılarını** Azure depolama blobunu konağa bağlantısını doğrulayın.
6. Performans sorunları, aşağıda açıklandığı gibi denetleyin.
    
### <a name="performance-issues"></a>performans sorunları

Ağ bant genişliği sınırlamaları çoğaltma etkileyebilir. Aşağıdaki gibi sorunlarını giderme:

1. [Denetleme](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) bant genişliği veya ortamınızda kısıtlamaları azaltma varsa.
2. Çalıştırma [dağıtım planlayıcısı Profil Oluşturucu](hyper-v-deployment-planner-run.md).
3. Profil Oluşturucu çalıştırdıktan sonra izleyin [bant genişliği](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) öneriler.
4. Denetleme [veri karmaşıklığı sınırlamaları](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Bir VM üzerinde karmaşıklığı yüksek veri görürseniz, aşağıdakileri yapın:
  - VM için yeniden eşitleme işaretlenmişse denetleyin.
  - İzleyin [adımları](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) karmaşıklığı kaynak araştırmak için.
  - Kullanılabilir disk alanı % 50 HRL günlük dosyalarını aşan karmaşası ortaya çıkabilir. Bu sorun ise, sorunun oluştuğu tüm VM'ler için daha fazla depolama alanı sağlayın.
  - Bu çoğaltma duraklatıldı değil denetleyin. İse, artan boyutuna katkıda bulunabilirsiniz hrl dosya değişiklikleri yazma devam eder.
 

## <a name="critical-replication-state-issues"></a>Kritik çoğaltma durumu sorunları

1. Çoğaltma durumunu denetlemek için şirket içi Hyper-V Yöneticisi'ni konsoluna bağlanmak, VM seçin ve sistem durumu doğrulayın.

    ![Çoğaltma durumu](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Tıklatın **çoğaltma durumunu görüntüle** ayrıntıları görmek için:

    - Çoğaltma duraklatıldı durumunda VM'ye sağ tıklayın > **çoğaltma** > **çoğaltmayı devam ettir**.
    - Aynı kümedeki farklı bir Hyper-V konak ya da tek başına makine Site kurtarma için yapılandırılmış bir Hyper-V ana bilgisayar üzerindeki VM'nin diğerine geçerse, sanal makine için çoğaltmayı etkilenmiş değil. Yalnızca yeni Hyper-V ana bilgisayar tüm önkoşulları karşıladığından ve Site kurtarma için yapılandırıldığını denetleyin.

## <a name="app-consistent-snapshot-issues"></a>Uygulamayla tutarlı anlık görüntü sorunları

Uygulama tutarlı bir anlık görüntü VM içinde uygulama verilerinin zaman içinde nokta anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS) anlık görüntü alınırken uygulamaların VM'de tutarlı bir durumda olmasını sağlar.  Bu bölümde karşılaşabileceğiniz bazı yaygın sorunlar ayrıntılarını verir.

### <a name="vss-failing-inside-the-vm"></a>VM içinde başarısız olan VSS

1. Integration services'ın en son sürümü yüklü ve çalışıyor olduğunu denetleyin.  Hyper-V ana bilgisayarda yükseltilmiş bir PowerShell isteminde aşağıdaki komutu çalıştırarak bir güncelleştirme kullanılabilir olup olmadığını denetleyin: **get-vm | seçin adı, durumu, IntegrationServicesState**.
2. VSS hizmetlerinin çalıştığından ve sağlıklı olduğundan emin olun:
    - Bunu yapmak için konuk sanal oturum açın. Ardından bir yönetici komut istemi açın ve tüm VSS yazıcılarının iyi durumda olup olmadığını denetlemek için aşağıdaki komutları çalıştırın.
        - **Vssadmin listesi yazıcılarının**
        - **Vssadmin listesi gölgeleri**
        - **Vssadmin listesi sağlayıcıları**
    - Çıktı denetleyin. Yazarları başarısız bir durumda, aşağıdakileri yapın:
        - VM VSS işlemi hatalar için uygulama olay günlüğünü denetleyin.
    - Başarısız yazıcı ile ilişkili bu hizmetleri yeniden başlatmayı deneyin:
        - Birim Gölge Kopyası
         - Azure Site Recovery VSS sağlayıcısı
    - Bunu yaptıktan sonra birkaç uygulamayla tutarlı anlık görüntüleri başarıyla oluşturuldu, görmek için saat için bekleyin.
    - Son çare olarak VM yeniden başlatmayı deneyin. Bu yanıt veremez duruma hizmetlerini çözebilir.
3. Dinamik diskler VM'yi yok denetleyin. Bu uygulamayla tutarlı anlık görüntüler için desteklenmiyor. Disk Yönetimi'nde (diskmgmt.msc) kontrol edebilirsiniz.

    ![Dinamik disk](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. VM'ye ekli bir iSCSI diski yok denetleyin. Bu özellik desteklenmez.
5. Yedekleme hizmetinin etkin olup olmadığını denetleyin. Bu doğrulama **Hyper-V ayarları** > **Integration Services**.
6. Çakışması VSS anlık görüntüleri alma uygulamaları ile olmadığından emin olun. Birden çok uygulama aynı zaman çakışmaları VSS anlık görüntüsünü çalışıyorsanız oluşabilir. Örneğin, Site Recovery, çoğaltma ilkesi tarafından bir anlık görüntüyü almaya zamanlandığı saatte bir yedekleme uygulaması VSS anlık görüntüleri sürüyorsa.   
7. VM bir yüksek bir karmaşıklık oranı yaşıyor denetleyin:
    - Hyper-V ana bilgisayarda performans sayaçlarını kullanarak Konuk VM'ler için günlük veri değişikliği hızını ölçer. Bunu yapmak için aşağıdaki sayaç etkinleştirin. VM karmaşıklığı almak için bu değeri 5-15 dakika boyunca VM disklere örneği Aggregrate.
        - Kategori: "Hyper-V sanal depolama aygıtı"
        - Sayaç: "Yazma Bayt / sn"</br>
        - Bu veri karmaşıklık oranı artırın veya VM veya kendi uygulamalarını ne kadar meşgul olduğunu bağlı olarak yüksek bir düzeyde kalır.
        - Site Recovery için standart depolama için 2 MB/sn ortalama kaynak disk veri dalgalanmasına olur. [Daha fazla bilgi](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Ayrıca şunları yapabilirsiniz [depolama ölçeklenebilirlik hedefleri doğrulamak](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets.md#scalability-targets-for-a-storage-account).
8. Çalıştırma [dağıtım Planlayıcısı](hyper-v-deployment-planner-run.md).
9. Önerileri gözden [ağ](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>Hyper-V konak içinde başarısız olan VSS

1. VSS hatalarının ve öneriler için olay günlüklerini kontrol edin:
    - Hyper-V ana bilgisayar sunucusunda Hyper-V Yöneticisi olay günlüğü'nde açın **Olay Görüntüleyicisi'ni** > **uygulama ve hizmet günlükleri** > **Microsoft**  >  **Windows** > **Hyper-V** > **yönetici**.
    - Uygulamayla tutarlı anlık görüntü hataları olduğunu gösteren olaylar olup olmadığını doğrulayın.
    - Tipik bir hata: "Hyper-V sanal makinesi 'XYZ' için VSS anlık görüntü kümesi oluşturamadı: yazan geçici olmayan bir hatayla karşılaştı. Hizmet yanıt vermiyorsa VSS hizmetini yeniden başlatma sorunlarını çözebilir."

2. VM için VSS anlık görüntüleri oluşturmak için Hyper-V tümleştirme hizmetleri VM üzerinde yüklü olduğunu ve yedekleme (VSS) tümleştirme hizmeti etkin olduğunu denetleyin.
    - Tümleştirme hizmetleri VSS hizmeti/Daemon Konuk çalıştırıyorsanız ve içinde olduğundan emin olun bir **Tamam** durumu.
    - Bu komutla Hyper-V ana bilgisayarda yükseltilmiş bir PowerShell oturumundan denetleyebilirsiniz **et-Vmıntegrationservice - VMName<VMName>-Name VSS** Konuk sanal açarak bu bilgi edinebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - VM üzerinde yedekleme/VSS tümleştirme hizmetlerinin çalıştığından ve iyi durumda olduğundan emin olun. Aksi takdirde, bu hizmetleri yeniden başlatmak ve ve Hyper-V konak sunucusunda Hyper-V Birim Gölge kopyası istek sahibi hizmeti.

### <a name="common-errors"></a>Sık karşılaşılan hataları

**Hata kodu** | **İleti** | **Ayrıntılar**
--- | --- | ---
**0x800700EA** | "Hyper-V sanal makinesi için VSS anlık görüntü kümesi oluşturamadı: daha fazla veri. (0x800700EA). Yedekleme işlemi devam ediyor VSS anlık görüntü ayarlanmış oluşturma başarısız.<br/><br/> Çoğaltma işlemi için sanal makine başarısız oldu: daha fazla veri kullanılabilir. " | VM dinamik disk etkin olup olmadığını denetleyin. Bu özellik desteklenmez.
**0x80070032** | "Hyper-V Birim Gölge kopyası istek sahibi, sanal makineye bağlanamadı <. / VMname > sürüm Hyper-V tarafından beklenen sürüm eşleşmediğinden | En son Windows güncelleştirmeleri yüklü olup olmadığını denetleyin.<br/><br/> [Yükseltme](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date) Integration Services'ın en son sürüme.



## <a name="collect-replication-logs"></a>Toplama çoğaltma günlükleri

Tüm Hyper-V çoğaltma olay bulunan Hyper-V-VMMS\Admin günlüğüne kaydedilir **uygulama ve hizmet günlükleri** > **Microsoft** > **Windows**. Ayrıca, bir analitik günlük Hyper-V sanal makine Yönetimi hizmeti için şu şekilde etkinleştirebilirsiniz:

1. Analitik ve hata ayıklama günlüklerini Olay Görüntüleyicisi'nde görüntülenebilir olun. Bu, Olay Görüntüleyicisi'nde yapmak için **Görünüm** > **Analitik ve hata ayıklama günlüklerini göster.**. Analitik günlük altında görünür **Hyper-V-VMMS**.
2. İçinde **Eylemler** bölmesinde tıklatın **günlüğü etkinleştir**. 

    ![Günlüğü etkinleştirme](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Bu özellik etkinleştirildikten sonra görünür **Performans İzleyicisi'ni**, olarak bir **olay izleme oturumu** altında **veri toplayıcı kümeleri**. 
4. Toplanan bilgileri görüntülemek için günlüğü devre dışı bırakarak izleme oturumunu durdur. Ardından günlüğünü kaydedin ve yeniden Olay Görüntüleyicisi'nde açın veya gerektiği şekilde dönüştürmek için diğer araçları kullanın.


### <a name="event-log-locations"></a>Olay günlüğü konumları

**Olay günlüğü** | **Ayrıntılar** |
--- | ---
**Uygulamalar ve hizmet günlükleri/Microsoft/VirtualMachineManager/Server/Admin** (VMM sunucusu) | VMM sorunları gidermek için günlüğe kaydeder.
**Uygulamalar ve hizmet günlükleri/MicrosoftAzureRecoveryServices/çoğaltma** (Hyper-V ana bilgisayarı) | Microsoft Azure kurtarma Hizmetleri Aracısı sorunlarını gidermek için günlüğe kaydeder. 
**Uygulamalar ve hizmet günlükleri/Microsoft/Azure Site kurtarma/sağlayıcı/Operational** (Hyper-V ana bilgisayarı)| Microsoft Azure Site Recovery hizmeti sorunlarını gidermek için günlüğe kaydeder.
**Uygulamalar ve hizmet günlükleri/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V ana bilgisayarı) | Hyper-V VM yönetimi sorunlarını gidermek için günlüğe kaydeder.

### <a name="log-collection-for-advanced-troubleshooting"></a>Gelişmiş sorun giderme için günlük toplama

Bu araçlar, Gelişmiş sorun giderme konusunda yardımcı olabilir:

-   VMM için Site Recovery günlük toplama gerçekleştirin ve [Destek Tanılama Platformu (SDP) aracı](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   VMM, olmadan Hyper-V için [bu aracı yükleme](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab), ve günlükleri toplamak için Hyper-V ana bilgisayarda çalıştırın.

