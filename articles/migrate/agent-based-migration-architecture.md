---
title: Azure geçişi Server Geçiş Aracı tabanlı geçiş mimarisi
description: Azure geçişi sunucusu geçişi ile VMware VM geçiş aracı tabanlı genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 21c779587842c976ba93d7fa592a91ee714bc55c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811160"
---
# <a name="agent-based-migration-architecture"></a>Aracı tabanlı geçiş mimarisi

Bu makalede, mimari ve işlemlerdeki Azure geçişi Server Geçiş Aracı ile aracı tabanlı çoğaltma için kullanılan genel bir bakış sağlar.

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve AWS/GCP VM örneklerini azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

## <a name="agent-based-replication"></a>Aracı tabanlı çoğaltma

Aracı tabanlı çoğaltma aracı geçirmek için kullanılan Azure geçişi sunucusu çoğaltma VMware Vm'lerini ve fiziksel sunucuları azure'a şirket içi. Ayrıca, AWS örneklerini ve GCP Vm'leri de dahil olmak üzere sanal makineleri, özel ve genel bulut yanı sıra diğer sanallaştırılmış yerinde sunucularınızı geçirmek için de kullanılabilir.

Azure geçişi Server Geçiş Aracı, VMware geçiş için birkaç seçenek sunar:

- Bu makalede açıklandığı gibi aracı tabanlı çoğaltmayı kullanarak geçiş.
- Herhangi bir şey yükler gerek kalmadan sanal makineleri geçirmek için aracısız çoğaltma.

Daha fazla bilgi edinin [VMware için bir geçiş yöntemini seçme](server-migrate-overview.md).

## <a name="server-migration-and-azure-site-recovery"></a>Sunucu geçiş ve Azure Site Recovery

Azure geçişi sunucusu geçişi geçirme şirket içinde ve genel bulut iş yüklerini azure'a yönelik bir araçtır. Bu, geçiş için optimize edilmiştir. Site kurtarma, olağanüstü durum kurtarma aracıdır. Azure geçiş sunucu ve Site Recovery veri çoğaltma için kullanılan bazı yaygın teknolojisi bileşenleri paylaşma, ancak farklı amaçlara hizmet eder.

## <a name="architectural-components"></a>Mimari bileşenler

![Mimari](./media/agent-based-replication-architecture/architecture.png)

Aracı tabanlı geçiş için kullanılan bileşenleri tabloda özetlenmiştir.

**Bileşen** | **Ayrıntılar** | **Yükleme**
--- | --- | ---
**Çoğaltma gereç** | Çoğaltma (yapılandırma sunucusu) şirket içi ortam ve Azure geçişi Server Geçiş Aracı arasında bir köprü görevi gören bir şirket içi makine gereçtir. Böylece Azure sunucusu geçişi, çoğaltma ve geçiş düzenleyebilir gerecin şirket içi VM Envanter bulur. Gereç iki bileşenden oluşur:<br/><br/> **Yapılandırma sunucusu**: Azure geçişi sunucusu geçişi için bağlanır ve çoğaltma düzenler.<br/> **İşlem sunucusu**: Veri çoğaltma işlemini gerçekleştirir. VM verileri alır, sıkıştırır ve şifreler ve Azure aboneliğinize gönderir. Burada, Server Geçiş yönetilen disklere veri yazar. | Varsayılan olarak, işlem sunucusu, çoğaltma gerecini yapılandırma sunucusu ile birlikte yüklenir.
**Ulaşım hizmeti** | Mobility hizmeti, çoğaltmak ve geçirmek istediğiniz her makinede yüklü bir aracısıdır. Makinede çoğaltma verilerini işlem sunucusuna gönderir. Bir dizi farklı Mobility hizmeti aracıları kullanılabilir. | Mobility hizmeti yükleme dosyalarını, çoğaltma gerecinde yer alır. İndirin ve işletim sistemi ve sürümü, çoğaltmak istediğiniz makinenin uygun olarak gereksinim aracısını yükleyin.

### <a name="mobility-service-installation"></a>Mobility hizmeti yüklemesi

Aşağıdaki yöntemleri kullanarak Mobility hizmetini dağıtabilirsiniz:

- **Göndererek**: Mobility hizmeti, bir makine için korumayı etkinleştirdiğinizde işlem sunucusu tarafından yüklenir. 
- **El ile yükleme**: Kullanıcı Arabirimi veya komut istemi aracılığıyla her makinede Mobility hizmetini el ile yükleyebilirsiniz.

Mobility hizmeti, çoğaltma Gereci ile iletişim kurar ve makineler çoğaltılır. Çoğaltma gereç, işlem sunucusu ya da çoğaltılan makineler üzerinde çalışan virüsten koruma yazılımınız varsa, aşağıdaki klasörleri taramanın dışında bırakılmalıdır:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site kurtarma
- C:\Program dosyaları (x86) \Microsoft Azure Site kurtarma
- C:\ProgramData\ASR\agent (üzerindeki Mobility hizmetinin yüklü olduğu makinelerle Windows)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Azure için ilk çoğaltma başlar.
2. İlk çoğaltma sırasında Mobility hizmeti makine disklerden verileri okur ve işlem sunucusuna gönderir.
3. Bu veriler, Azure aboneliğinizdeki diskinin bir kopyasını oluşturmak için kullanılır. 
4. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Çoğaltma, blok düzeyinde ve yakın.
4. Mobility hizmetini durdurur, işletim sistemi depolama alt sistemi ile tümleştirerek VM disk belleği için yazar. Bu yöntem, artımlı çoğaltma için çoğaltma makinesi üzerinde disk g/ç işlemleri önler. 
5. Bir makine için izlenen değişiklikler, işlem sunucusuna HTTPS 9443 numaralı bağlantı noktasında gelen olarak gönderilir. Bu bağlantı noktası değiştirilebilir. İşlem sunucusu sıkıştırır ve şifreler ve Azure'a gönderir. 

## <a name="ports"></a>Bağlantı Noktaları

**cihaz** | **bağlantı**
--- | --- 
VM'ler | Vm'lerde çalışan mobilite hizmeti ile şirket içi çoğaltma gereç HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim kurar.<br/><br/> Sanal makineleri çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktası (varsayılan olarak çalışan çoğaltma gereçte) işlem sunucusu için gelen gönderin. Bu bağlantı noktası değiştirilebilir.
Çoğaltma gereç | Çoğaltma gereç HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma işlemlerini yönetir.
İşlem sunucusu | İşlem sunucusu çoğaltma verilerini alıp, en iyi duruma getirir ve şifreler ve Azure depolamaya bağlantı noktası 443 üzerinden giden gönderir.


## <a name="performance-and-scaling"></a>Performans ve ölçeklenebilirlik

Varsayılan olarak, hem yapılandırma sunucusu ve işlem sunucusu çalıştıran bir tek çoğaltma uygulaması dağıtın. Yalnızca birkaç makineler çoğaltma yapıyorsanız bu dağıtım, yeterli olur. Ancak, çoğaltmak ve yüzlerce makine geçiş, bir tek işlem sunucusu çoğaltma trafiğini işlemek mümkün olmayabilir. Bu durumda, ek olarak, ölçeği genişletilmiş işlem sunucularını dağıtabilirsiniz.

### <a name="site-recovery-deployment-planner-for-vmware"></a>VMware için Site Recovery dağıtım Planlayıcısı

VMware sanal makinelerini çoğaltıyorsanız, kullanabileceğiniz [Site Recovery dağıtım Planlayıcısı](../site-recovery/site-recovery-deployment-planner.md) performans gereksinimlerini belirlemeye yardımcı olması VMware için günlük verileri dahil olmak üzere oranı ve ihtiyacınız olan işlem sunucularının değiştirme.

### <a name="replication-appliance-capacity"></a>Çoğaltma Gereci kapasite

Bir ek işlem sunucusu dağıtımınızda gerekmediğini anlamak için bu tablodaki değerleri kullanılabilir.

- Günlük değişikliği hızınıza (karmaşıklık oranı) üzerinde 2 TB ise, bir ek işlem sunucusu dağıtın.
- 200'den fazla makineler çoğaltma yapıyorsanız ek çoğaltma Gereci dağıtın.

**CPU** | **Bellek** | **Verileri önbelleğe alma için boş alan** | **Değişim oranı** | **Çoğaltma sınırlamaları**
--- | --- | --- | --- | ---
8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz) | 16 GB | 300 GB | 500 GB veya daha az | < 100 makineler 
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) | 18 GB | 600 GB | 501 GB ila 1 TB | 100-150 makineler.
16 Vcpu (2 yuva * 8 çekirdek \@ 2.5 GHz) | 32 G1 |  1 TB | 1 TB ile 2 TB | 151 200 makine.

### <a name="scale-out-process-server-sizing"></a>Genişleme işlem sunucusu boyutlandırma

Bir genişleme işlem sunucusu dağıtmanız gerekiyorsa, bu tablo sunucusu boyutlandırma kullanıma anlamaya yardımcı olabilir.

**İşlem sunucusu** | **Verileri önbelleğe alma için boş alan** | **Değişim oranı** | **Çoğaltma sınırlamaları**
--- | --- | --- | --- 
4 Vcpu (2 yuva * 2 Çekirdek \@ 2.5 GHz), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 makinelere 
8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz), 12 GB bellek | 600 GB | 251 GB ila 1 TB    | 150 86 makineler.
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz), 24 GB bellek | 1 TB | 1-2 TB | 151 225 makineler.

## <a name="control-upload-throughput"></a>Denetim karşıya yükleme verimi

Her Hyper-V konağında veri yüklemek için kullanılan bant genişliği miktarını sınırlayabilirsiniz. Dikkat et. Çoğaltma ve gecikme geçiş olumsuz etkiler çok düşük değerleri ayarlarsanız.


1. Hyper-V konak veya küme düğümünde oturum açın.
2. Çalıştırma **C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.msc**, Windows Azure Backup MMC ek bileşenini açın.
3. Ek bileşeninde, seçin **özelliklerini değiştirme**.
4. İçinde **azaltma**seçin **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir**. Ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar 512 Kbps ila 1,023 MB/sn olan.
I

### <a name="influence-upload-efficiency"></a>Karşıya yükleme verimliliği etkisi

Çoğaltma için yedek bant genişliğine sahip ve karşıya artırmak istiyorsanız, karşıya yükleme görevi gibi ayrılan iş parçacığı sayısını artırabilirsiniz:

1. Kayıt defteri Regedit ile açın.
2. HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM anahtarına gidin
3. Her bir çoğaltma VM için karşıya veri yükleme için kullanılan iş parçacıklarının sayısı değerini artırın. Varsayılan değer 4'tür ve maksimum değer 32'dir. 

## <a name="next-steps"></a>Sonraki adımlar

Aracı tabanlı deneme [VMware VM geçişi](tutorial-migrate-vmware-agent.md) Azure geçişi sunucusu geçişi kullanan.
