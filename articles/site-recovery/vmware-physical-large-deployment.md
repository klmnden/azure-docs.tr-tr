---
title: Azure Site Recovery ile VMware Vm'lerini veya fiziksel sunucuları çok sayıda için Azure'da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Çok sayıda şirket içi VMware Vm'lerini veya fiziksel sunucuları için azure'a Azure Site Recovery ile olağanüstü durum kurtarma ayarlamayı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: raynew
ms.openlocfilehash: e96aafe61c0d8547ffca9e97bfd9e90c9529155f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237275"
---
# <a name="set-up-disaster-recovery-at-scale-for-vmware-vmsphysical-servers"></a>VMware Vm'lerini/fiziksel sunucuları için uygun ölçekte olağanüstü durum kurtarmayı ayarlama

Bu makalede şirket içi VMware Vm'leri veya fiziksel sunucular, üretim ortamında çok sayıda (> 1000) için azure'da olağanüstü durum kurtarma ayarlamayı açıklar kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.


## <a name="define-your-bcdr-strategy"></a>BCDR stratejinizi tanımlayın

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisi kapsamında, kurtarma noktası hedeflerine (RPO'lar) ve iş kolu uygulamalarını ve iş yükleri için kurtarma zamanı hedeflerine (Rto'lar) tanımlayın. RTO ölçer süresi zaman ve hizmet düzeyi, içinde bir iş uygulaması veya işlem geri yüklenen ve sürekliliği sorunları önlemek için kullanılabilir olmalıdır.
- Site Recovery, VMware Vm'leri ve fiziksel sunucuları için sürekli çoğaltma sağlar ve bir [SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) RTO için.
- VMware Vm'leri ve ihtiyacınız olan Azure kaynakları anlamak için büyük ölçekli bir olağanüstü durum kurtarma planlaması gibi kapasite hesaplamaları için kullanılacak bir RTO değeri belirtebilirsiniz.


## <a name="best-practices"></a>En iyi uygulamalar

Büyük ölçekli bir olağanüstü durum kurtarma için genel bazı en iyi yöntemler. En iyi bu belgenin sonraki bölümlerinde daha ayrıntılı ele alınmıştır.

- **Hedef gereksinimlerini tanımlamaya**: Olağanüstü durum kurtarma işlemini ayarladığınız önce kullanıma azure'da kapasite ve kaynak gereksinimlerini tahmin edin.
- **Plan için Site Recovery bileşenlerini**: Hangi Site Recovery bileşenlerini (yapılandırma sunucusu, işlem sunucusu), tahmini kapasitenizi karşılamak gereken her şekil.
- **Bir veya daha fazla genişleme işlem sunucusu ayarlayabilir**: Varsayılan olarak yapılandırma sunucusunda çalıştırılan işlem sunucusu kullanmayın. 
- **En son güncelleştirmeleri çalıştırma**: Site Recovery ekibi, yeni sürümlerini düzenli olarak Site Recovery bileşenlerini serbest bırakır ve en son sürümleri çalıştırıyorsanız emin olmanız gerekir. İle yardımcı olmak için izleme [yenilikler](site-recovery-whats-new.md) güncelleştirmeleri ve [etkinleştirme ve güncelleştirmeleri yükleme](service-updates-how-to.md) bunlar release olarak.
- **Proaktif izleme**: Olağanüstü durum kurtarma çalışır hale geldikçe çoğaltılan makinelerin altyapı kaynaklarını ve sistem proaktif olarak izlemeniz gerekir.
- **Olağanüstü durum kurtarma tatbikatlarını**: Olağanüstü durum kurtarma tatbikatı düzenli olarak çalıştırmanız gerekir. Bunlar, üretim ortamınıza etkisi yoktur, ancak sağlamaya yardımcı olmak azure'a Bu yük devretme gerektiğinde beklendiği gibi çalışır.



## <a name="gather-capacity-planning-information"></a>Kapasite planlama bilgileri toplayın

Değerlendirmek ve hedef (Azure) kapasitenizi ihtiyaçlarınızı tahmin etmenize yardımcı olmak için şirket içi ortamınız hakkında bilgi toplayın.
- VMware için bunu yapmak, VMware Vm'leri için dağıtım Planlayıcısını çalıştırın.
- Fiziksel sunucuları için el ile bilgi toplayın.

### <a name="run-the-deployment-planner-for-vmware-vms"></a>VMware Vm'leri için dağıtım Planlayıcısını çalıştırın

Dağıtım Planlayıcısını VMware şirket içi ortamınız hakkında bilgi toplamak için yardımcı olur.

- Vm'leriniz için tipik bir değişim sıklığı temsil eden bir dönem için dağıtım Planlayıcısını çalıştırın. Bu, daha doğru tahminler ve öneriler oluşturur.
- Planlayıcıyı üzerinde çalıştığı sunucudan aktarım hızını hesaplar olduğundan dağıtım Planlayıcısını configuration server makinesinde çalıştırmanızı öneririz. [Daha fazla bilgi edinin](site-recovery-vmware-deployment-planner-run.md#get-throughput) aktarım hızı ölçme hakkında.
- Ayarlanan bir yapılandırma sunucusu henüz yoksa:
    - [Genel bakışın](vmware-physical-azure-config-process-server-overview.md) Site Recovery bileşenlerinin.
    - [Yapılandırma sunucusu ayarlarsınız](vmware-azure-deploy-configuration-server.md), dağıtım Planlayıcısını üzerinde çalıştırmak için.

Ardından Planlayıcısı aşağıdaki gibi çalıştırın:

1. [Hakkında bilgi edinin](site-recovery-deployment-planner.md) dağıtım Planlayıcısını. Portaldan en son sürümü yükleyebilirsiniz veya [doğrudan indirin](https://aka.ms/asr-deployment-planner).
2. Gözden geçirme [önkoşulları](site-recovery-deployment-planner.md#prerequisites) ve [en son güncelleştirmeleri](site-recovery-deployment-planner-history.md) için dağıtım Planlayıcısı ve [indirin ve ayıklayın](site-recovery-deployment-planner.md#download-and-extract-the-deployment-planner-tool) aracı.
3. [Dağıtım Planlayıcısını çalıştırın](site-recovery-vmware-deployment-planner-run.md) yapılandırma sunucusunda.
4. [Rapor oluşturma](site-recovery-vmware-deployment-planner-run.md#generate-report) tahminler ve öneri özetlemek için.
5. Analiz [rapor önerileri](site-recovery-vmware-deployment-planner-analyze-report.md) ve [maliyet tahminleri](site-recovery-vmware-deployment-planner-cost-estimation.md).

>[!NOTE]
> Varsayılan olarak, araç profil için yapılandırılır ve 1000'e kadar sanal makineler için bir rapor oluşturur. ASRDeploymentPlanner.exe.config dosyasındaki MaxVMsSupported anahtar değerini artırarak bu sınırı değiştirebilirsiniz.

## <a name="plan-target-azure-requirements-and-capacity"></a>Hedef (Azure) gereksinimleri ve kapasite planlama

Toplanan tahminler ve öneri kullanarak hedef kaynaklarını ve kapasiteyi için planlayabilirsiniz. VMware Vm'leri için dağıtım Planlayıcısı çalışmışsa faturaya bir dizi kullanabileceğiniz [rapor önerileri](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations) yardımcı olacak.

- **Uyumlu VM'ler**: Bu sayı, Azure'da olağanüstü durum kurtarma için hazır olan VM'lerin sayısını belirlemek için kullanın. Ağ bant genişliği ve Azure çekirdek hakkında öneriler bu sayısına dayanır.
- **Gerekli ağ bant genişliği**: Uyumlu sanal makinelerin değişiklik çoğaltması için gereken bant genişliğini unutmayın. 
    - Planlayıcıyı çalıştırdığınızda dakika cinsinden istenen RPO belirtin. Öneriler bu RPO, % 100 ve süre %90 karşılamak için gereken bant genişliğini gösterir. 
    - Ağ bant genişliği önerilerini yapılandırma sunucusu ve Planner'da, önerilen işlem sunucularının toplam sayısı için gereken bant genişliğini göz önünde bulundurur.
- **Gerekli Azure çekirdek**: Uyumlu sanal makinelerin sayısına göre temel Not Azure bölgesini hedef gereken çekirdek sayısı. Yeterli çekirdek yoksa, yük devretme işleminde Site Recovery gerekli Azure Vm'leri oluşturmak mümkün olmayacaktır.
- **Önerilen VM toplu iş boyutu**: Önerilen toplu iş boyutu toplu işlemi için 72 saat içinde ilk çoğaltmayı tamamlamak için özelliği varsayılan olarak, bir % 100 RPO karşılamanın yanı sıra dayanır. Saat değeri değiştirilebilir.

Azure kaynakları, ağ bant genişliğini ve toplu işleme VM planlamak için bu önerileri kullanabilirsiniz.

## <a name="plan-azure-subscriptions-and-quotas"></a>Azure abonelikleri ve kotalar planlama

Hedef abonelikte kullanılabilir kotalar yük devretme gerçekleştirmek için yeterli olduğundan emin olmak istiyoruz.

**Görev** | **Ayrıntılar** | **Eylem**
--- | --- | ---
**Çekirdek denetleme** | Kullanılabilir kota çekirdek eşit veya yük devretme sırasında toplam hedef sayısını aşan yoksa, yük devretme işlemleri başarısız olur. | VMware Vm'leri için dağıtım Planlayıcısı çekirdek öneri karşılamak için hedef abonelikte yeterli çekirdek sahip denetleyin.<br/><br/> Fiziksel sunucular için Azure çekirdek, el ile tahminleri karşıladığını denetleyin.<br/><br/> Azure portalında kotalar denetlenecek > **abonelik**, tıklayın **kullanım ve kotalar**.<br/><br/> [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) kota artırma hakkında.
**Yük devretme limitlerini denetleme** | Yük devretme işlemleri sayısı, Site Recovery yük devretme sınırları aşacak gerekmez. |  Yük devretmeleri sınırları aşmanız halinde abonelikleri ekleyebilir ve birden fazla aboneliğe yük devretme veya bir abonelik için kotayı artırmak. 


### <a name="failover-limits"></a>Yük devretme sınırları

Sınırları, makine başına üç disk olduğu varsayılırsa bir saat içinde desteklenen Site Recovery tarafından yük devretmeleri sayısını gösterir.

Ne anlama gelir uyumlu mu? Bir Azure VM başlatmak için Azure bazı sürücüleri önyükleme başlangıç durumda olmasını gerektirir ve otomatik olarak başlayacak şekilde ayarlamak için DHCP gibi hizmetler.
- Uyumlu makineler zaten yerinde bu ayarlarına sahip olur.
- Windows çalıştıran makineler için proaktif olarak uyumluluk denetleyin ve gerekirse uyumlu hale. [Daha fazla bilgi edinin](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010).
- Linux makineler yalnızca yük devretme zaman uyumlu duruma getirilir.

**Makine, Azure ile uyumludur?** | **Azure VM sınırları (yönetilen disk yük devretme)**
--- | --- 
Evet | 2000
Hayır | 1000

- Sınırları, abonelik için hedef bölgede devam eden diğer işleri, diğer en az varsayılır.
- Bazı Azure bölgeleri, küçük ve biraz daha düşük sınırlara sahip olabilir.

## <a name="plan-infrastructure-and-vm-connectivity"></a>Altyapı ve VM bağlantısı planlama

Azure'a yük devredildikten sonra iş yüklerinizi şirket içi olduğu gibi çalışır ve Azure Vm'lerinde çalışan iş yükleri erişmelerini sağlamak için gerekir.

- [Daha fazla bilgi edinin](site-recovery-active-directory.md#test-failover-considerations) hakkında başarısız, Active Directory ve DNS şirket içi altyapınızı Azure üzerinde.
- [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) yük devretmeden sonra Azure Vm'lerine bağlanmak hazırlama hakkında daha fazla.



## <a name="plan-for-source-capacity-and-requirements"></a>Kaynak kapasitesi ve gereksinimleri için planlama

Yeterli yapılandırma sunucusu ve genişleme işlem sunucusu kapasite gereksinimlerini karşılamak için olması önemlidir. Büyük ölçekli dağıtıma başlamadan gibi bir yapılandırma sunucusu ve tek bir genişleme işlem sunucusu ile başlayın. Önceden belirlenmiş sınırları ulaşması gibi ek sunucular ekleyin.

>[!NOTE]
> VMware Vm'leri için dağıtım Planlayıcısı ihtiyacınız yapılandırma hem de işlem sunucuları hakkında bazı öneriler sağlar. Dağıtım Planlayıcısı öneri aşağıdaki yerine aşağıdaki yordamlar, bulunan tabloları kullanmanızı öneririz. 


## <a name="set-up-a-configuration-server"></a>Yapılandırma sunucusunu ayarlama
 
Yapılandırma sunucusu kapasitesini çoğaltılan makineler sayısından etkilenir ve değil tarafından veri değişim sıklığı oranı. Ek yapılandırma sunucularına ihtiyacınız olup olmadığını anlamak için bu tanımlı VM sınırları kullanın.

**CPU** | **Bellek** | **Önbellek diski** | **Makine sınırı çoğaltılan**
 --- | --- | --- | ---
8 Vcpu<br> Yuva 2 * @ 2.5 Ghz 4 çekirdek | 16 GB | 600 TB | 550 makinelere<br> Her makine 100 GB'lık üç disk olduğu varsayılır.

- Bu sınırlar bir OVF şablonunu kullanarak yapılandırma sunucusunu temel alır.
- Sınırları, varsayılan olarak yapılandırma sunucusunda çalıştırılan işlem sunucusu kullanmadığınızı varsayar.

Yeni bir yapılandırma sunucusu eklemeniz gerekiyorsa aşağıdaki yönergeleri izleyin:

- [Yapılandırma sunucusu ayarlarsınız](vmware-azure-deploy-configuration-server.md) VMware VM'LERİNDE olağanüstü durum kurtarma için bir OVF şablonunu kullanarak.
- [Yapılandırma sunucusu ayarlarsınız](physical-azure-set-up-source.md) fiziksel sunucuları için el ile veya VMware dağıtımları, bir OVF şablonunu kullanamazsınız.

Bir yapılandırma sunucusunu ayarlama gibi dikkat edin:

- Bir yapılandırma sunucusunu ayarlayın, abonelik ve bunlar Kurulum sonrasında değiştirilmesi olmamalıdır beri içinde bulunduğu kasa göz önünde bulundurmanız önemlidir. Kasa değiştirmeniz gerekirse, kasa yapılandırma sunucusunun ilişkisini ve bunu yeniden kaydetmeniz gerekir. Bu kasada çoğaltma sanal makinelerinin durdurur.
- Birden fazla ağ bağdaştırıcısı yapılandırma sunucusunu ayarlamak istiyorsanız, bu kümesi sırasında yapmalısınız. Yapılandırma sunucusunu kasaya kaydedildikten sonra bunu yapamaz.

## <a name="set-up-a-process-server"></a>Bir işlem sunucusu ayarlama

İşlem sunucusu kapasite, çoğaltma için etkinleştirilmiş makine sayısına göre değil ve veri değişim hızı etkilenir.

- Büyük dağıtımlar için her zaman en az bir genişleme işlem sunucusu olmalıdır.
- Ek sunuculara gerek olup olmadığını anlamak için aşağıdaki tabloyu kullanın.
- En yüksek teknik özelliklerine sahip bir sunucu eklemenizi öneririz. 


**CPU** | **Bellek** | **Önbellek diski** | **Değişim oranı**
 --- | --- | --- | --- 
12 Vcpu<br> Yuva 2 * @ 2.5 Ghz 6 çekirdek | 24 GB | 1 GB | Günde en fazla 2 TB

İşlem sunucusu ayarlamadıysanız aşağıdaki gibi ayarlayın:

1. Gözden geçirme [önkoşulları](vmware-azure-set-up-process-server-scale.md#prerequisites).
2. Sunucu yükleme [portalı](vmware-azure-set-up-process-server-scale.md#install-from-the-ui), veya [komut satırı](vmware-azure-set-up-process-server-scale.md#install-from-the-command-line).
3. Çoğaltılan makineler yeni sunucu kullanacak şekilde yapılandırın. Çoğaltılan makineler zaten varsa:
    - Yapabilecekleriniz [taşıma](vmware-azure-manage-process-server.md#switch-an-entire-workload-to-another-process-server) yeni işlem sunucusu için bir işlem sunucu iş yükü.
    - Alternatif olarak, [taşıma](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load) belirli sanal makineler için yeni işlem sunucusu.



## <a name="enable-large-scale-replication"></a>Büyük ölçekli çoğaltmayı etkinleştirme

Kapasite planlaması ve gerekli bileşenleri ve altyapı dağıtımı sonra çok sayıda sanal makineleri için çoğaltmayı etkinleştirin.

1. Sıralama makineleri gruplayın. Bir yığın içindeki sanal makineler için çoğaltmayı etkinleştirin ve ardından sonraki toplu geçin.

    - VMware Vm'leri için kullanabileceğiniz [önerilen VM toplu iş boyutu](site-recovery-vmware-deployment-planner-analyze-report.md#recommended-vm-batch-size-for-initial-replication) dağıtım Planlayıcısı raporu içinde.
    - Fiziksel makineler için benzer bir boyut ve veri miktarı olan makinelere ve kullanılabilir ağ aktarım hızı göre toplu tanımlamak öneririz. Bir yandan etrafında aynı süre içinde kendi ilk çoğaltmayı tamamlamak büyük olasılıkla toplu makineler sağlamaktır.
    
2. Disk değişim sıklığı bir makine için yüksek ya da dağıtım thePlanner sınırları aşıyor (günlük dökümleri veya geçici dosyaları gibi) makine çoğaltma yapmanız gerekmez, kritik olmayan dosyaları taşıyabilirsiniz. VMware Vm'leri için ayrı bir disk için bu dosyaları taşıyabilirsiniz ve ardından [o diski hariç](vmware-azure-exclude-disk.md) çoğaltmanın dışında.
3. Çoğaltmayı etkinleştirmeden önce karşılamak makineleri denetleyin [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md#replicated-machines).
4. Yapılandırma için bir çoğaltma ilkesi [VMware Vm'lerini](vmware-azure-set-up-replication.md#create-a-policy) veya [fiziksel sunucuları](physical-azure-disaster-recovery.md#create-a-replication-policy).
5. İçin çoğaltmayı etkinleştirme [VMware Vm'lerini](vmware-azure-enable-replication.md) veya [fiziksel sunucuları](physical-azure-disaster-recovery.md#enable-replication). Seçili makineler için ilk çoğaltmayı devre dışı başlatıyor.

## <a name="monitor-your-deployment"></a>Dağıtımınızı izleme

Çoğaltma sanal makinelerinin ilk batch için kazandırın sonra dağıtımınızı şu şekilde izleme başlatın:  

1. Çoğaltılan makinelerin sistem durumunu izlemek için bir olağanüstü durum kurtarma Yöneticisi atayın.
2. [İzleme olayları](site-recovery-monitor-and-troubleshoot.md) çoğaltılan öğeler ve altyapı için.
3. [Sistem izleme](vmware-physical-azure-monitor-process-server.md) genişleme işlem sunucularınızın.
4. Almak için kaydolun [e-posta bildirimleri](https://docs.microsoft.com/azure/site-recovery/site-recovery-monitor-and-troubleshoot#subscribe-to-email-notifications) izlemeyi kolaylaştırmak için olaylar için.
5. Kuralları normal [olağanüstü durum kurtarma tatbikatlarını](site-recovery-test-failover-to-azure.md), her şeyin beklendiği gibi çalıştığından emin olmak için.


## <a name="plan-for-large-scale-failovers"></a>Büyük ölçekli yük devretmeleri planlama

Olağanüstü bir durumda, çok sayıda makineleri/iş yüklerini azure'a yük devretme yapmanız gerekebilir. Bu olay türü için aşağıdaki gibi hazırlayın.

Yük devretme için şu şekilde önceden hazırlayabilirsiniz:

- [Altyapınız ve Vm'leri hazırlama](#plan-infrastructure-and-vm-connectivity) böylece iş yüklerinizi yük devretmeden sonra kullanıma sunulacak ve böylece kullanıcıların Azure Vm'lerini erişebilir.
- Not [yük devretme sınırları](#failover-limits) bu belgenin daha öncesinde. Yük devretme işlemleri bu sınırlar içinde kalacak emin olun.
- Normal çalıştırma [olağanüstü durum kurtarma tatbikatlarını](site-recovery-test-failover-to-azure.md). Tatbikatları yardımcı olmak için:
    - Yük devretmeden önce dağıtımınızdaki boşlukları bulun.
    - Uygulamalarınız için uçtan uca RTO tahmin edin.
    - İş yükleriniz için uçtan uca RPO'yu tahmin etmek.
    - IP adresi aralığı çakışmaları belirleyin.
    - Tatbikatları çalıştırırken, kullanmayın üretim ağları tatbikatları için üretim ve test ağlarda aynı alt ağ adları kullanmaktan kaçının ve yük devretme testi her ayrıntıya sonra temiz olmasını öneririz.

Büyük ölçekli bir yük devretme çalıştırmak için şunları öneririz:

1. İş yükü yük devretme için kurtarma planları oluşturun.
    - Her bir kurtarma planı, en fazla 50 makine yük devretme tetikleyebilirsiniz.
    - [Daha fazla bilgi edinin](recovery-plan-overview.md) kurtarma planları hakkında.
2. Azure'da el ile gerçekleştirilen tüm görevleri otomatikleştirmek için kurtarma planları, Azure Otomasyonu runbook betikleri ekleyin. DNS vb. güncelleştiriliyor, yük dengeleyicileri yapılandırma tipik görevler içerir. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md)
2. Bunlar Azure ortamı ile uyumlu olacak şekilde devretmeden önce Windows makineleri hazırlayın. [Yük devretme sınırları](#plan-azure-subscriptions-and-quotas) uyumlu makinelerde daha yüksektir. [Daha fazla bilgi edinin](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010) runbook'lar hakkında.
4.  Yük devretme tetiklemek [başlangıç AzRecoveryServicesAsrPlannedFailoverJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/start-azrecoveryservicesasrplannedfailoverjob?view=azps-2.0.0&viewFallbackFrom=azps-1.1.0) PowerShell cmdlet'i olan bir kurtarma planı.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Site kurtarma İzleyicisi](site-recovery-monitor-and-troubleshoot.md)
