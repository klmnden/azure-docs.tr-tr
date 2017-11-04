---
title: "İzleme ve sanal makineleri ve fiziksel sunucuları için koruması sorunlarını giderme | Microsoft Docs"
description: "Azure Site Recovery, çoğaltma, yük devretme ve kurtarma Azure veya ikincil veri merkezine şirket içi sunucularda bulunan sanal makinelerin düzenler. İzleme ve Virtual Machine Manager veya Hyper-V sitesi koruması sorunlarını gidermek için bu makaleyi kullanın."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: 2d033e5af13660c99aba813c58b743bf94a6b95a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>İzleme ve sanal makineleri ve fiziksel sunucuları için koruması sorunlarını giderme
Bu izleme ve sorun giderme kılavuzu, çoğaltma durumunu izlemek ve Azure Site Recovery için sorun giderme teknikleri hakkında bilgi edinin yardımcı olur.

## <a name="understand-the-components"></a>Bileşenlerini anlama
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware sanal makine ya da şirket içi ve Azure arasında çoğaltma için fiziksel sunucu sitesi dağıtımı
Bir şirket içi VMware sanal makinesi veya fiziksel sunucu ve Azure arasında veritabanı kurtarma ayarlamak için yapılandırma sunucusu, ana hedef sunucusu ve işlem sunucusu bileşenleri sanal makinede veya sunucu ayarlamanız gerekir. Kaynak sunucu için korumayı etkinleştirdiğinizde, Azure Site Recovery Microsoft Azure App Service Mobile Apps özelliğini yükler. Şirket içi kesinti ve sonra kaynak sunucu üzerinden Azure, Azure işlem sunucusu ve bir ana hedef sunucusu şirket içinde şirket içi kaynak sunucusunu yeniden oluşturmak için ayarlamak için müşterinin ihtiyaç duyduğu başarısız olur.

![Şirket içi ve Azure arasında çoğaltma için VMware/fiziksel sitesi dağıtımı](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Sanal Makine Yöneticisi site dağıtım için şirket içi siteler arasında çoğaltma
İki şirket içi konumlara arasında veritabanı kurtarma ayarlamak için Azure Site Recovery Sağlayıcısı'nı indirin ve Sanal Makine Yöneticisi sunucuya yüklemeniz gerekir. Sağlayıcının Azure portalından tetiklenen tüm işlemler için şirket içi işlemleri çevrilen sağlamak için Internet bağlantısı gerekir.

![Sanal Makine Yöneticisi site dağıtım için şirket içi siteler arasında çoğaltma](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Şirket içi konumları ve Azure arasında çoğaltma için Sanal Makine Yöneticisi sitesi dağıtımı
Şirket içi konumları ve Azure arasında veritabanı kurtarma ayarladığınızda, Azure Site Recovery Sağlayıcısı'nı indirin ve Sanal Makine Yöneticisi sunucuya yüklemeniz gerekir. Ayrıca, her Hyper-V ana bilgisayarda yüklü olması gerekir Azure kurtarma Hizmetleri aracısını yüklemeniz gerekir. [Daha fazla bilgi edinin](site-recovery-hyper-v-azure-architecture.md) daha fazla bilgi için.

![Şirket içi konumları ve Azure arasında çoğaltma için Sanal Makine Yöneticisi sitesi dağıtımı](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Şirket içi konumları ve Azure arasında çoğaltma için Hyper-V sitesi dağıtımı
Bu işlem, sanal makine yöneticisi dağıtıma benzer. Azure Site Recovery sağlayıcısı ve Azure kurtarma Hizmetleri aracısını Hyper-V ana bilgisayarın üzerinde yüklü yalnızca farktır. [Daha fazla bilgi edinin](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Yapılandırma, koruma ve kurtarma işlemlerini izleme
Her işlem Azure Site Recovery, denetlenen ve altında izlenen **İŞLERİ** sekmesi. Yapılandırma, koruma veya kurtarma hatası için Git **İŞLERİ** sekmesinde ve hatalar için bakın.

![İŞLER sekmesinde başarısız filtresi](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Hataları altında bulursanız **İŞLERİ** sekmesi, iş'i tıklatın ve'ı tıklatın **hata ayrıntıları** iş.

![HATA ayrıntılarını düğmesi](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Hata ayrıntılarını bir olası neden ve sorunu için öneri belirlemenize yardımcı olur.

![Belirli bir iş için hata ayrıntılarını gösteren iletişim kutusu](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Önceki örnekte, devam eden başka bir işlem koruma yapılandırması başarısız olmasına neden görünüyor. Öneriye dayalı sorunu çözün ve ardından **yeniden** işlemi yeniden başlatmak için.

![İŞLER sekmesinde Yeniden Başlat düğmesi](media/site-recovery-monitoring-and-troubleshooting/image6.png)

**Yeniden** seçeneği tüm işlemler için kullanılabilir değil. Bir işlem yoksa **yeniden** seçeneği, nesneye geri dönün ve yeniden işlemini yineleyin. Devam eden herhangi bir işi iptal edebilirsiniz kullanarak **iptal** düğmesi.

![İptal düğmesi](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Sanal makineler için çoğaltma durumunu izleme
Azure Site Recovery sağlayıcıları her korunan varlıklar için uzaktan izlemek için Azure portalını kullanabilirsiniz. Tıklatın **KORUNAN ÖĞELER**ve ardından **VMM BULUTLARI** veya **koruma grupları**. **VMM BULUTLARI** sekmesi üzerinde Virtual Machine Manager tabanlı dağıtımlar için kullanılabilir, yalnızca. Diğer senaryolar için korumalı altında varlıklardır **koruma grupları** sekmesi.

![VMM Bulutları ve koruma grupları seçenekleri](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Bir Korunan varlık altında ilgili buluta tıklayın veya tüm kullanılabilir tüm işlemleri görmek için koruma grubu, alt bölmesinde gösterilir.

![Seçilen bir Korunan varlık için kullanılabilir seçenekleri](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Önceki ekran görüntüsünde gösterildiği gibi sanal makine sistem durumu olan **kritik**. Tıklayabilirsiniz **hata ayrıntıları** hatayı görmek için alt düğmesinde. Temel **olası nedenleri** ve **öneri**, sorunu çözün.

![Hata ayrıntılarını düğmesi](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Hatalar ve hata ayrıntıları iletişim kutusunda öneriler](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Herhangi bir etkin işlem sürüyor veya başarısız oldu, Git **İŞLERİ** görüntülemek için belirli bir iş hatası görüntülemek için daha önce belirtildiği gibi.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Şirket içi Hyper-V sorunlarını giderme
Şirket içi Hyper-V manager konsoluna bağlanmak, sanal makineyi seçin ve çoğaltma durumunu görün.

![Hyper-V Yöneticisi konsolunda çoğaltma sistem durumunu görüntülemek için seçeneği](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Bu durumda, **çoğaltma sistem durumunu** olan **kritik**. Sanal makineye sağ tıklayın ve ardından **çoğaltma** > **çoğaltma durumunu görüntüle** ayrıntıları görmek için.

![Belirli bir sanal makine için çoğaltma durumu](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Sanal makine için çoğaltma duraklatıldı durumunda sanal makineye sağ tıklayın ve ardından **çoğaltma** > **çoğaltmayı devam ettir**.

![Hyper-V manager konsolundan devam ettirin seçeneği](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Bir sanal makine küme veya tek başına makine içinde yeni bir Hyper-V konağı geçirir ve Hyper-V konağı Azure Site Recovery ile yapılandırılmış sanal makine için çoğaltma etkilenmiş olmayacaktır. Yeni Hyper-V ana bilgisayar tüm önkoşulları karşıladığını ve Azure Site Recovery kullanarak yapılandırıldığından emin olun.

### <a name="event-log"></a>Olay günlüğü
| Olay kaynakları | Ayrıntılar |
| --- |:--- |
| **Uygulamalar ve hizmet günlükleri/Microsoft/VirtualMachineManager/Server/Admin** (Virtual Machine Manager sunucusu) |Birçok farklı Sanal Makine Yöneticisi sorunlarını gidermek için yararlı günlüğü sağlar. |
| **Uygulamalar ve hizmet günlükleri/MicrosoftAzureRecoveryServices/çoğaltma** (Hyper-V ana bilgisayarı) |Çok sayıda Microsoft Azure kurtarma Hizmetleri Aracısı sorunlarını gidermek için yararlı günlüğü sağlar. <br/> ![Hyper-V ana bilgisayar için çoğaltma olay kaynağı konumu](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Uygulamalar ve hizmet günlükleri/Microsoft/Azure Site kurtarma/sağlayıcı/Operational** (Hyper-V ana bilgisayarı) |Çok sayıda Microsoft Azure Site Recovery hizmeti sorunlarını gidermek için yararlı günlüğü sağlar. <br/> ![Hyper-V ana bilgisayar için işletimsel olay kaynağı konumu](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Uygulamalar ve hizmet günlükleri/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V ana bilgisayarı) |Birçok Hyper-V sanal makine yönetimi sorunlarını gidermek için yararlı günlüğü sağlar. <br/> ![Hyper-V ana bilgisayar için Sanal Makine Yöneticisi olay kaynağı konumu](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Hyper-V çoğaltma günlük seçenekleri
Hyper-V çoğaltma için ilgili tüm olayları Hyper-V-VMMS içinde kaydedilir\\yönetici günlük uygulama ve hizmet günlükleri altında bulunan\\Microsoft\\Windows. Ayrıca, Hyper-V sanal makine Yönetimi hizmeti için bir analitik günlüğü etkinleştirebilirsiniz. Bu günlük etkinleştirmek için öncelikle Analitik ve hata ayıklama günlüklerini Olay Görüntüleyicisi'nde görüntülenebilir olun. Olay Görüntüleyicisi'ni açın ve ardından **Görünüm** > **Analitik ve hata ayıklama günlüklerini göster**.

![Analitik ve hata ayıklama günlüklerini göster seçeneği](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analitik günlüğü'nün altında görülebilir **Hyper-V-VMMS**.

![Analitik oturum Olay Görüntüleyicisi'ni ağacı](media/site-recovery-monitoring-and-troubleshooting/image15.png)

İçinde **Eylemler** bölmesinde tıklatın **günlüğü etkinleştir**. Bu özellik etkinleştirildikten sonra görünür **Performans İzleyicisi'ni** olarak bir **olay izleme oturumu** altında bulunan **veri toplayıcı kümeleri.**

![Performans İzleyicisi ağacında olay izleme oturumları](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Toplanan bilgileri görüntülemek için önce günlüğü devre dışı bırakarak izleme oturumunu durdur. Günlüğünü kaydedin ve yeniden Olay Görüntüleyicisi'nde açın veya istendiği gibi dönüştürmek için diğer araçları kullanın.

## <a name="reach-out-for-microsoft-support"></a>Microsoft Support ulaşın
### <a name="log-collection"></a>Günlük toplama
Sanal Makine Yöneticisi site koruma için bkz [Destek Tanılama Platformu (SDP) aracını kullanarak Azure Site Recovery günlük toplama](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) gerekli günlükleri toplamak için.

Hyper-V site koruma için indirme [aracı](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) ve günlükleri toplamak için Hyper-V ana bilgisayarına yürütün.

VMware/fiziksel sunucusu senaryoları için başvurmak [VMware ve fiziksel site koruma için Azure Site Recovery günlük toplama](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) gerekli günlükleri toplamak için.

Aracı rastgele adlandırılmış bir alt klasör % LocalAppData%\ElevatedDiagnostics altında günlüklerde yerel olarak toplar.

![Hyper-V sitesi korumadan gösterilen örnek adımlar.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Bir destek bileti açın
Azure Site Recovery için bir destek bileti yükseltmek için Azure destek için URL'de kullanarak ulaşmak <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>KB makaleleri
* [Sürücü harfi üzerinden başarısız veya Azure'a geçirilen korumalı sanal makineler için koruma](http://support.microsoft.com/kb/3031135)
* [Şirket içinden Azure'a koruma ağ bant genişliği kullanımını yönetme](https://support.microsoft.com/kb/3056159)
* [Azure Site kurtarma: sanal makine için korumayı etkinleştirmeye çalıştığınızda "Küme kaynağı bulunamadı" hatası](http://support.microsoft.com/kb/3010979)
* [Anlama ve sorun giderme Hyper-V çoğaltma Kılavuzu](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Sık karşılaşılan Azure Site Recovery hataları ve bunların çözümleri
Sık karşılaşılan hataları ve bunların çözümleri aşağıda verilmiştir. Her hata ayrı wiki sayfa içinde belgelenmiştir.

### <a name="general"></a>Genel
* <span style="color:green;">Yeni</span> [işleri "bir işlem devam ediyor." hatasıyla başarısız oluyor Hata 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">Yeni</span> ["Sunucusu Internet'e bağlı değil" hatası ile başarısız olan işler. Hata 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Kurulum
* [Sanal Makine Yöneticisi sunucunun bir iç hata nedeniyle kaydedilemedi. Lütfen hata hakkında daha fazla ayrıntı için Site Recovery portalında jobs görünümüne bakın. Sunucuyu kaydetmek için kurulumu yeniden çalıştırın.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Hyper-V kurtarma Yöneticisi Kasası'na bir bağlantı kurulamıyor. Proxy ayarlarını doğrulayın veya daha sonra yeniden deneyin.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Yapılandırma
* [Koruma grubu oluşturulamadı: sunucularının listesi alınırken bir hata oluştu.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Hyper-V konak kümesi en az bir statik ağ bağdaştırıcısı içeriyor veya hiçbir bağlı bağdaştırıcı DHCP kullanacak şekilde yapılandırılır.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Sanal Makine Yöneticisi eylemi tamamlamak için izinlere sahip değil.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Koruma yapılandırılırken abonelik içindeki depolama hesabını seçemezsiniz.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Koruma
* <span style="color:green;">Yeni</span> ["Koruma sanal makine için koruma yapılandırılamadı" hatası ile başarısız olan korumayı etkinleştir. Hata 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">Yeni</span> ["Sanal makine için koruma etkinleştirilemedi." hatası ile başarısız olan korumayı etkinleştir Hata 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">Yeni</span> [dinamik geçiş hata 23848 - sanal makine, Canlı türü kullanılarak taşınacak olduğuna. Bu sanal makinenin kurtarma koruma durumunu bozar.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Aracı ana makinede yüklü değil bu yana korumayı etkinleştirme başarısız oldu.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Çoğaltma sanal makinesi için uygun bir konak - düşük işlem kaynakları nedeniyle bulunamıyor.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Çoğaltma sanal makinesi için uygun bir konak - bağlı hiçbir mantıksal ağ nedeniyle bulunamıyor.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Bağlanamıyor çoğaltma konak makineye - bağlantı kurulamadı.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Kurtarma
* Sanal Makine Yöneticisi konak işlemini tamamlayamıyor:
  * [Sanal makine için seçilen kurtarma noktası yük: genel erişim reddedildi hatası.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Hyper-V sanal makine için seçilen kurtarma noktası yük başaramadı: işlem iptal edildi.  Daha yeni bir kurtarma noktası deneyin. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Sunucu ile bağlantı kurulan (0x00002EFD) bulunamadı.
    * [Hyper-V sanal makine için çoğaltmayı tersine çevirme etkinleştiremedi.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Hyper-V sanal makinesi sanal makine için çoğaltma etkinleştirilemedi.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Sanal makine için yük devretme kaydedilemedi.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Kurtarma planı planlanan yük devretme için hazır olmayan sanal makineler içeriyor.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [Sanal makine planlanmış yük devretme için hazır değil.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Sanal makine çalışmıyorsa ve kapalı değil.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Bant dışı işlemi, bir sanal makine ve yürütme yük devretme işlemi başarısız oldu.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Yük devretme sınaması
  * [Test yük devretmesi sürüyor olduğundan, yük devretme başlatılamadı.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">Yeni</span> yük devretme zaman aşımına uğruyor 'PreFailoverWorkflow görevle WaitForScriptExecutionTaskTimeout' sanal makine ya da bilgisayarın ait olduğu alt ağ ile ilişkilendirilmiş ağ güvenlik grubu yapılandırma ayarları nedeniyle. Başvurmak ['PreFailoverWorkflow görev WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) Ayrıntılar için.

### <a name="configuration-server-process-server-master-target"></a>Yapılandırma sunucusu, işlem sunucusu, ana hedef
* [PS/CS VM olarak barındırıldığı ESXi ana renkli kilitlenme mor ekran ile başarısız olur.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Uzak Masaüstü'nü yük devretme sonrasında sorun giderme
* Birçok müşteri başarısız bağlanmak için sorunları karşılaştığı azure'da sanal makine üzerinde. [Sanal makinenin RDP için sorun giderme belgeye kullanmak](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Bir kaynak yöneticisi sanal makinede bir genel IP ekleme
Varsa **Bağlan** portalda düğmesine griyse ve Azure'a bir Express Route veya siteden siteye VPN bağlantısı üzerinden bağlanmamış, oluşturmak ve Uzak Masaüstü'nü kullanmadan önce sanal makinenize genel bir IP adresi atamak gerekir / Paylaşılan Kabuk. Ardından sanal makinenin ağ arabirimi genel IP ekleyebilirsiniz.  

![Genel IP ağ arabirimi sanal makine eklenemedi](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
