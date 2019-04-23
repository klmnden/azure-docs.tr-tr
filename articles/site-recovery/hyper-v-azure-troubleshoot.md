---
title: Azure Site Recovery ile Azure'a olağanüstü durum kurtarma için Hyper-V sorunlarını giderme | Microsoft Docs
description: Hyper-V ile Azure Site Recovery kullanarak Azure'a çoğaltma, olağanüstü durum kurtarma sorunlarını giderme açıklar
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/14/2019
ms.author: rajanaki
ms.openlocfilehash: 8bb790571e1499bd45fb8bee27f4f1896046cbc2
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149104"
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Azure'a çoğaltma ve yük devretme için Hyper-V sorunlarını giderme

Bu makalede çoğaltılan Hyper-V Vm'lerini azure'a, şirket genelinde gelebilir ortak sorunları kullanarak [Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Koruma sorunları etkinleştir

Hyper-V VM'ler için korumayı etkinleştirdiğinizde, ilgili sorun yaşıyorsanız, aşağıdaki önerileri denetleyin:

1. Hyper-V konakları ve sanal makineleri tüm karşıladığını denetlemek [gereksinimler ve Önkoşullar](hyper-v-azure-support-matrix.md).
2. Hyper-V sunucuları System Center Virtual Machine Manager (VMM) bulutlarında yer alıyorsa, hazırladığınıza doğrulayın [VMM sunucusu](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Hyper-V konaklarında Hyper-V sanal makine Yönetimi hizmetinin çalışıp çalışmadığını denetleyin.
4. Sanal makineye Hyper-V-VMMS\Admin oturum açma görünen sorunlarını denetleyin. Bu günlük bulunan **uygulama ve hizmet günlükleri** > **Microsoft** > **Windows**.
5. Konuk sanal Makinede, WMI etkin ve erişilebilir olduğunu doğrulayın.
   - [Hakkında bilgi edinin](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) temel WMI sınama.
   - [Sorun giderme](https://aka.ms/WMiTshooting) WMI.
   - [Sorun giderme](https://technet.microsoft.com/library/ff406382.aspx#H22) WMI komut dosyaları ve Hizmetleri ile ilgili sorunlar.
6. Konuk sanal Makinede, Integration Services'ın en son sürümünü çalıştırdığından emin olun.
    - [Denetleme](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) en son sürüme sahip.
    - [Tutun](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) tümleştirme hizmetleri güncel.
    
## <a name="replication-issues"></a>Çoğaltma sorunları

Aşağıdaki gibi başlangıçtaki ve devam eden çoğaltma ile ilgili sorunları giderme:

1. Çalıştırdığınızdan emin olun [en son sürümü](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) Site Recovery Hizmetleri.
2. Çoğaltma duraklatıldı olup olmadığını doğrulayın:
   - Hyper-V Yöneticisi konsolunda sanal makine durumunu denetleyin.
   - Kritik ise, VM'ye sağ tıklayın > **çoğaltma** > **çoğaltma durumunu görüntüle**.
   - Çoğaltma duraklatıldı tıklatmak **çoğaltmayı devam ettir**.
3. Gerekli hizmetlerin çalıştığından emin olun. Değilseniz, yeniden başlatın.
    - VMM olmadan Hyper-V çoğaltma yapıyorsanız bu hizmetlerin Hyper-V konağında çalıştığını kontrol edin:
        - Sanal Makine Yönetimi Hizmeti
        - Microsoft Azure kurtarma Hizmetleri Aracısı hizmeti
        - Microsoft Azure Site Recovery hizmeti
        - WMI sağlayıcısı ana bilgisayar hizmeti
    - VMM ile ortamda çoğaltma yapıyorsanız bu hizmetlerin çalıştığından emin olun:
        - Hyper-V konağında sanal makine Yönetimi hizmeti, Microsoft Azure kurtarma Hizmetleri aracısı ve WMI sağlayıcısı konak Hizmeti'nin çalıştığından emin olun.
        - VMM sunucusunda, System Center Virtual Machine Manager hizmetinin çalıştığından emin olun.
4. Hyper-V server ve Azure arasındaki bağlantıyı denetleyin. Bağlantıyı denetlemek için Hyper-V konağında Görev Yöneticisi'ni açın. Üzerinde **performans** sekmesinde **açık Kaynak İzleyicisi**. Üzerinde **ağ** sekmesi > **ağ etkinliği ile işlem**, cbengine.exe büyük veri birimleri (MB) etkin bir şekilde gönderip göndermediğini denetleyin.
5. Hyper-V konakları için Azure depolama blobu URL'si bağlanıp bağlanamadığınızı denetleyin. Ana bağlanmak, seçin denetleyin ve varsa denetlenecek **cbengine.exe**. Görünüm **TCP bağlantıları** Azure depolama blobu konaktan bağlantısını doğrulamak için.
6. Performans sorunlarını, aşağıda açıklandığı gibi kontrol edin.
    
### <a name="performance-issues"></a>Performans sorunları

Ağ bant genişliği sınırlamaları çoğaltma etkileyebilir. Şu şekilde sorunlarını giderme:

1. [Denetleme](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) bant genişliği veya kısıtlamaları ortamınızdaki azaltma varsa.
2. Çalıştırma [dağıtım planlayıcısı Profil Oluşturucu](hyper-v-deployment-planner-run.md).
3. Profil Oluşturucu çalıştırdıktan sonra takip [bant genişliği](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) öneriler.
4. Denetleme [veri değişim sıklığı sınırlamaları](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Yüksek veri değişim sıklığı bir VM'de görürseniz, aşağıdakileri yapın:
   - Sanal makinenize bir yeniden eşitleme için işaretlenir denetleyin.
   - İzleyin [adımları](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) değişim kaynağını araştırmak için.
   - Değişim sıklığı, kullanılabilir disk alanı yüzdesi 50 HRL günlük dosyaları aştığında oluşabilir. Bu sorun ise, sorunun oluştuğu tüm VM'ler için daha fazla depolama alanı sağlayın.
   - Çoğaltma duraklatılmış olmadığından denetleyin. İse, artan boyutuna katkıda bulunabilir hrl dosya değişiklikleri yazma devam eder.
 

## <a name="critical-replication-state-issues"></a>Kritik çoğaltma durumu sorunları

1. Çoğaltma durumunu denetlemek için şirket içi Hyper-V Manager konsoluna bağlanmak, sanal Makineyi seçin ve sistem durumu doğrulayın.

    ![Çoğaltma durumu](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Tıklayın **çoğaltma durumunu görüntüle** ayrıntıları görmek için:

    - Çoğaltma duraklatıldı, VM'ye sağ tıklayın > **çoğaltma** > **çoğaltmayı devam ettir**.
    - Site Recovery, yapılandırılmış bir Hyper-V konağındaki VM aynı kümedeki farklı bir Hyper-V konağı veya tek başına makine geçerse, VM için çoğaltma etkilenmiş değil. Yalnızca yeni Hyper-V konağı tüm önkoşulları karşıladığından ve Site Recovery'de yapılandırıldığından emin olun.

## <a name="app-consistent-snapshot-issues"></a>Uygulamayla tutarlı anlık görüntüsü sorunları

Uygulamayla tutarlı bir anlık görüntü, VM'nin içindeki uygulama verilerinin zaman içinde nokta anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS) anlık görüntü alınırken VM'deki uygulamalar tutarlı bir durumda olmasını sağlar.  Bu bölümde, karşılaşabileceğiniz bazı yaygın sorunlar açıklanmaktadır.

### <a name="vss-failing-inside-the-vm"></a>Sanal makine içinde başarısız olan VSS

1. Integration services'ın en son sürümü yüklü ve çalışıyor olduğunu kontrol edin.  Hyper-V ana bilgisayarda yükseltilmiş bir PowerShell isteminden aşağıdaki komutu çalıştırarak bir güncelleştirme kullanılabilir olup olmadığını denetleyin: **get-vm | belirleyin adı, durum, IntegrationServicesState**.
2. VSS Hizmetleri çalışır ve iyi durumda olduğundan emin olun:
   - Hizmetlerini denetlemek için konuk VM oturum açın. Ardından, bir yönetici komut istemi açın ve tüm VSS yazıcılarının iyi durumda olup olmadığını denetlemek için aşağıdaki komutları çalıştırın.
       - **Vssadmin listesi yazıcılar**
       - **Vssadmin listesi gölge**
       - **Vssadmin listesi sağlayıcıları**
   - Çıktıyı denetleyin. Yazıcılarının hatalı durumda olduğundan, aşağıdakileri yapın:
       - VSS işlemi hataları için VM üzerindeki uygulama olay günlüğünü denetleyin.
   - Başarısız yazıcıyla birlikte ilgili hizmetlerin yeniden başlatmayı deneyin:
     - Birim Gölge Kopyası
       - Azure Site Recovery VSS sağlayıcısı
   - Bunu yaptıktan sonra birkaç uygulamayla tutarlı anlık görüntüleri başarıyla oluşturuldu, görmek için saat bekleyin.
   - Son çare olarak, sanal Makineyi yeniden başlatmayı deneyin. Bu, yanıt vermeyen durumda olan hizmetleri çözebilir.
3. VM disklerine sahip olmadığınız kontrol edin. Bu, uygulamayla tutarlı anlık görüntüler için desteklenmez. Disk Yönetimi (diskmgmt.msc) denetleyebilirsiniz.

    ![Dinamik disk](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. VM'ye bir iSCSI diski almadığınızı denetleyin. Bu özellik desteklenmez.
5. Yedekleme hizmeti etkinleştirildiğinden emin olun. İçinde etkin olduğunu doğrulayın **Hyper-V ayarları** > **tümleştirme hizmetleri**.
6. VSS anlık görüntülerini alma uygulamaları ile çakışma olmadığından emin olun. Birden fazla uygulama VSS anlık görüntüsünü aynı zaman çakışmalarını en çalışıyorsanız oluşabilir. Örneğin, Site Recovery çoğaltma ilkenizi bir anlık görüntüsünü almak için zamanlandı VSS anlık görüntüleri yedekleme uygulaması sürüyorsa.   
7. VM bir yüksek bir karmaşıklık oranı yaşıyor denetleyin:
    - Hyper-V konağında performans sayaçlarını kullanarak Konuk sanal makineleri için günlük veri değişikliği hızınıza ölçebilirsiniz. Veri değişiklik oranı ölçmek için sayaç aşağıdaki etkinleştirin. Bu değer bir örneği arasında VM diskleri VM karmaşası alınacağı 5-15 dakika için toplama.
        - Kategori: "Hyper-V sanal depolama cihazı"
        - Sayaç: "Yazma Bayt / sn"</br>
        - Bu veri değişim hızı artırmak veya VM veya uygulamalarına ne kadar meşgul olduğunu bağlı olarak yüksek bir düzeyde kalır.
        - Ortalama kaynak disk veri değişim sıklığı, 2 MB/sn'lik Site Recovery için standart depolama için ' dir. [Daha fazla bilgi](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - Buna ek olarak şunları yapabilirsiniz [depolama ölçeklenebilirlik hedefleri doğrulayın](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).
8. Çalıştırma [dağıtım Planlayıcısı](hyper-v-deployment-planner-run.md).
9. Önerileri gözden [ağ](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) ve [depolama](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>Hyper-V konak içinde başarısız olan VSS

1. VSS hatalarının ve öneriler için olay günlüklerini kontrol edin:
    - Hyper-V konak sunucusunda Hyper-V yönetici olay günlüğünde açın **Olay Görüntüleyicisi'ni** > **uygulama ve hizmet günlükleri** > **Microsoft**  >  **Windows** > **Hyper-V** > **yönetici**.
    - Uygulamayla tutarlı anlık görüntü hatalarını belirten tüm olaylar olup olmadığını doğrulayın.
    - Tipik bir hatadır: "Hyper-V başarısız oldu 'XYZ' sanal makinesi için VSS anlık görüntü kümesi oluşturmak: Yazıcı, geçici olmayan bir hatayla karşılaştı. Hizmet yanıt vermiyorsa VSS hizmetini yeniden sorunları çözebilir."

2. VM için VSS anlık görüntülerini oluşturmak için Hyper-V tümleştirme hizmetleri sanal makinede yüklü olduğundan ve yedekleme (VSS) tümleştirme hizmeti etkin olduğunu denetleyin.
    - Tümleştirme hizmetleri VSS hizmeti/Daemon Konuk çalıştıran olduğundan emin olun ve bir **Tamam** durumu.
    - Bu komutu Hyper-V konağındaki yükseltilmiş bir PowerShell oturumundan denetleyebilirsiniz **et-Vmıntegrationservice - VMName<VMName>-adı VSS** Konuk VM açılarak bu bilgi alabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services).
    - VM yedekleme/VSS tümleştirme Hizmetleri'nin çalışıyor ve iyi durumda olduğundan emin olun. Aksi halde bu hizmetler ve Hyper-V konak sunucusunda Hyper-V Birim Gölge kopyası istek sahibi hizmeti yeniden başlatın.

### <a name="common-errors"></a>Sık karşılaşılan hatalar

**Hata kodu** | **İleti** | **Ayrıntılar**
--- | --- | ---
**0x800700EA** | "Başarısız Hyper-V sanal makinesi için VSS anlık görüntü kümesi oluşturmak: Daha fazla veri kullanılabilir. (0x800700EA). Yedekleme işlemi devam ediyor, VSS anlık görüntü ayarlanmış üretimi başarısız olabilir.<br/><br/> Sanal makine için çoğaltma işlemi başarısız oldu: Daha fazla veri mevcut değil." | Sanal makinenizin dinamik disk etkin olup olmadığını denetleyin. Bu özellik desteklenmez.
**0x80070032** | "Hyper-V Birim Gölge kopyası istek sahibi, sanal makineye bağlanamadı <. / VMname > sürüm Hyper-V tarafından beklenen sürüm eşleşmediğinden | En son Windows güncelleştirmelerini'nın yüklü olup olmadığını denetleyin.<br/><br/> [Yükseltme](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) Integration Services'ın en son sürüme.



## <a name="collect-replication-logs"></a>Toplama çoğaltma günlükleri

Tüm Hyper-V çoğaltma olay bulunan Hyper-V-VMMS\Admin günlüğüne kaydedilir **uygulama ve hizmet günlükleri** > **Microsoft** > **Windows**. Ayrıca, bir analitik günlüğü için Hyper-V sanal makine Yönetimi hizmeti, şu şekilde etkinleştirebilirsiniz:

1. Analitik ve hata ayıklama günlükleri Olay Görüntüleyicisi'nde görüntülenebilir olun. Günlükleri Olay Görüntüleyicisi'nde kullanılabilir yapmak için tıklatın **görünümü** > **Analitik ve hata ayıklama günlüklerini göster.**. Analitik günlüğü altında görünür **Hyper-V-VMMS**.
2. İçinde **eylemleri** bölmesinde tıklayın **günlüğü etkinleştir**. 

    ![Günlüğünü etkinleştir](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Etkinleştirildikten sonra görünür **Performans İzleyicisi**, olarak bir **olay izleme oturumu** altında **veri toplayıcı kümeleri**. 
4. Toplanan bilgileri görmek için günlüğü devre dışı bırakarak izleme oturumu durdurun. Ardından günlüğü kaydetmek ve Olay Görüntüleyicisi'nde yeniden açın veya gerektiği şekilde dönüştürmek için diğer araçları kullanın.


### <a name="event-log-locations"></a>Olay günlüğü konumları

**Olay günlüğü** | **Ayrıntılar** |
--- | ---
**Uygulama ve hizmet günlükleri/Microsoft/VirtualMachineManager/sunucu/Admin** (VMM sunucusu) | VMM sorunlarını gidermek için günlükleri.
**Uygulama ve hizmet günlükleri/MicrosoftAzureRecoveryServices/çoğaltma** (Hyper-V ana bilgisayarı) | Microsoft Azure kurtarma Hizmetleri Aracısı sorunlarını gidermek için günlükleri. 
**Uygulama ve hizmet günlükleri/Microsoft/Azure Site Recovery/sağlayıcısı/Operational** (Hyper-V ana bilgisayarı)| Microsoft Azure Site Recovery hizmeti sorunlarını gidermek için günlükleri.
**Uygulama ve hizmet günlükleri/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V ana bilgisayarı) | Hyper-V VM yönetimi sorunlarını gidermek için günlükleri.

### <a name="log-collection-for-advanced-troubleshooting"></a>Gelişmiş sorun giderme için günlük toplama

Bu araçlar, Gelişmiş sorun giderme konusunda yardımcı olabilir:

-   Site Recovery günlük koleksiyonu kullanarak VMM için gerçekleştirmek [Desteği Tanılama Platformu (SDP) aracı](https://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   VMM olmadan Hyper-V için [bu aracı karşıdan](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab), ve günlükleri toplamak için Hyper-V konağı üzerinde çalıştırın.

