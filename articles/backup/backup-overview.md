---
title: Azure Backup nedir?
description: Azure Backup hizmeti ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize parçası olarak dağıtma hakkında genel bir bakış sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 298c9fabca9d1994e0b952fdf8b48b70370c3ec2
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490679"
---
# <a name="what-is-azure-backup"></a>Azure Backup nedir?

Azure Backup hizmeti, Microsoft Azure bulutuna verileri yedekler. Şirket içi makinelerin ve iş yükleri ve Azure sanal makineleri (VM) yedekleme.


## <a name="why-use-azure-backup"></a>Azure Backup'ı neden kullanmalısınız?

Azure Backup şu önemli avantajlara sahiptir:

- **Şirket içi yedekleme boşaltma**: Azure Backup, şirket içi kaynaklarınızı buluta yedekleme için basit bir çözüm sunar. Alma kısa ve uzun vadeli yedekleme karmaşık dağıtmaya gerek kalmadan şirket yedekleme çözümleri. Bant veya site dışında yedekleme gereksinimini ortadan kaldırır.
- **Azure Iaas Vm'leri yedekleme**: Azure Backup, özgün verilerin yanlışlıkla edilmesine karşı koruma sağlamak için bağımsız ve ayrı yedeklemeler sağlar. Yedeklemeler ile yerleşik kurtarma noktaları yönetilen bir kurtarma Hizmetleri kasasında depolanır. Yapılandırma ve ölçeklenebilirlik özellikleri, basit, yedeklemeleri en iyi duruma getirilir ve gerektiğinde kolayca geri yükleyebilirsiniz.
- **Ölçeği kolayca** - Azure Backup temel alınan gücü ve sınırsız ölçek Azure bulutunun yüksek kullanılabilirlik sunmak için kullanır - herhangi bir bakım ve izleme maliyetleri ile. Olaylar hakkında bilgi almak için uyarılar oluşturabilirsiniz ancak buluttaki verilerinizin yüksek kullanılabilirliği konusunda endişelenmeniz gerekmez.
- **Sınırsız veri aktarımı alma** - Azure Backup, gelen miktarını sınırlamaz veya giden veri aktarımı. Azure Backup ayrıca aktarılan veriler için ücret talep etmez. Ancak, büyük miktarda veriyi içeri aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanırsanız gelen verilerden ücret alınır. Bu maliyet hakkında daha fazla bilgi için bkz. [Azure Backup’ta çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md). Giden veriler, geri yükleme işlemi sırasında bir Kurtarma Hizmetleri kasasından aktarılan verileri tanımlar.
- **Verileri güvenli tutmak**: Veri şifreleme güvenli şekilde iletilmesini ve genel bulutta verilerinizin depolama sağlar. Şifreleme parolası yerel olarak depolanır ve hiçbir zaman Azure'a iletilmez veya orada depolanmaz. Verileri geri yüklemeniz gerekirse, şifreleme parolası veya anahtarı yalnızca sizde olur.
- **Uygulamayla tutarlı yedekler almak**: Bir uygulamayla tutarlı yedekleme, kurtarma noktası yedek kopyayı geri yüklemek için tüm gerekli verileri içeren anlamına gelir. Azure Backup, verileri geri yüklerken ek düzeltmelere gerek kalmaması için uygulamayla tutarlı yedeklemeler yapılmasını sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.
- **Kısa ve uzun süreli saklama alma**: **Uzun süreli saklama** - Kısa süreli ve uzun süreli veri saklama için Kurtarma Hizmetleri kasasını kullanabilirsiniz. Azure, bir Kurtarma Hizmetleri kasasında verileri saklama süresini kısıtlamaz. Verileri bir kasada dilediğiniz süre boyunca bekletebilirsiniz. Azure Backup, korunan her örnek için 9999 kurtarma noktası sınırına sahiptir. Bu sınırın yedekleme gereksinimlerinizi nasıl etkileyebileceği hakkında bir açıklama için bu makaledeki [Yedekleme ve bekletme](backup-introduction-to-azure-backup.md#backup-and-retention) bölümüne bakın.
- **Otomatik depolama yönetimi** - Karma ortamlar genelde heterojen depolamaya (bazıları şirket içi, bazıları ise bulutta olan) ihtiyaç duyar. Azure Backup çözümünde, şirket içi depolama cihazlarının kullanımıyla ilişkili maliyetler yoktur. Azure Backup, yedekleme alanını otomatik olarak ayırıp yönetir ve "kullandıkça öde" modelini kullanır. Kullandıkça öde yöntemiyle yalnızca kullandığınız depolama alanı için ödeme yaparsınız. Daha fazla bilgi için bkz. [Azure fiyatlandırma makalesi](https://azure.microsoft.com/pricing/details/backup).
- **Birden çok depolama seçeneği** - Yüksek kullanılabilirliğin bir özelliği de depolama çoğaltmadır. Azure Backup iki tür çoğaltma sunar: [Yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) ve [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md). İhtiyacınız olan yedek depolama seçeneğini belirleyin:
    - Yerel olarak yedekli depolama (LRS), verilerinizi veri merkezine yer alan bir depolama ölçek birimine üç kez kopyalar (verilerinizin üç kopyasını oluşturur). Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarına karşı korumak için düşük maliyetli bir seçenektir.
    - Coğrafi olarak yedekli depolama (GRS), varsayılan ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS’nin maliyeti LRS’den yüksektir ancak bölgesel kesintiler olsa bile daha yüksek veri dayanıklılık düzeyi sunar.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>Azure Backup ve Azure Site Recovery arasındaki fark nedir?

Azure Backup ve Azure Site Recovery Hizmetleri İşletmenizde bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. BCDR iki geniş amaçları oluşur:

- Kesintiler meydana geldiğinde iş verilerinizi güvenli ve kurtarılabilir tutun.
- Uygulamalarınızı ve iş yüklerini çalıştırmaya planlanmış ve Planlanmamış kapalı kalma süreleri tutun.

Her iki hizmet tamamlayıcı ancak farklı bir işlevsellik sağlar.

- **Azure Site Recovery**: Site Recovery, şirket içi makinelerin ve Azure Vm'leri için olağanüstü durum kurtarma çözümü sunar. Makineleri ikincil bir birincil konumdan çoğaltma. Olağanüstü durumla karşılaştığınızda, makineleri ikincil konuma yük devretme ve makinelere oradan erişebilirsiniz. Her şey çalışır duruma normalde yeniden üstündeyse başarısız birincil site kurtarılır arka makineleri.
- **Azure yedekleme**: Azure Backup hizmeti, şirket içi makinelerin ve Azure Vm'leri verileri yedekler. Veri yedeklenebilir ve dosyaları, klasörleri, makine sistem durumu yedeklemesi ve uygulama durumunu algılayan yedekleme de dahil olmak üzere ayrıntılı bir düzeyde, kurtarılır. Azure yedekleme, Site Recovery değerinden daha ayrıntılı bir düzeyde verileri işler. Örneğin, dizüstü bilgisayardaki sunu bozulduysa, sunuyu geri yüklemek için Azure Backup kullanın. Güvenli ve erişilebilir bir VM yapılandırma ve verileri tutmak istiyorsanız, Site RECOVERY'yi kullanabilirsiniz.  

BCDR gereksinimlerinizi şekil yardımcı olması için tablo noktalarını kullanın. 

**Hedefi** | **Ayrıntılar** | **Karşılaştırma**
--- | --- | --- | --- |
**Veri yedekleme/saklama** | Yedekleme verileri korunur ve gün, ay veya yıl bile uyumluluk açısından bakıldığında gerekirse saklanan. | Azure yedekleme gibi yedekleme çözümleri, ince yedeklemek istediğiniz veri çekmek izin ve ince yedekleme ve bekletme ilkeleri ayarlayın.<br/><br/> Site Recovery, aynı ince ayar yapma izin vermez.
**Kurtarma noktası hedefi (RPO)** | Kurtarma işleminin gerekli olduğu durumlarda kabul edilebilir veri kaybı miktarı. | Daha fazla değişken RPO yedeklemelere sahip.<br/><br/> Veritabanı Yedeklemeleri 15 dakika kadar düşük RPO'lar VM yedeklemeleri genellikle bir gün, bir RPO bulunur.<br/><br/> Site Recovery, düşük RPO'ya sağlar, bu çoğaltma sürekli veya sık, olduğundan kaynak ve çoğaltma kopyalama arasındaki delta küçük olmasını sağlayın.
**Kurtarma süresi hedefi (RTO)** |Bir geri yükleme veya kurtarma işlemini tamamlamak için geçen süre. | Daha büyük RPO nedeniyle, bir yedekleme çözümünün işlemesi gereken veri miktarı genellikle çok daha yüksektir; bu da daha uzun RTO'lara yol açar. Örneğin, bandın şirket dışı bir konumdan taşınması için harcanan süreye bağlı olarak, bantlardan veri geri yükleme işlemi birkaç gün sürebilir. | Site kurtarma gibi olağanüstü durum kurtarma çözümleri düşük RPO'ya sahip hedef daha yüksek bir düzeyde kaynağı ile eşitlenmiş çoğaltma sürekli/sık genellikle anlamına gelir. |

## <a name="what-backup-scenarios-are-supported"></a>Hangi yedekleme senaryoları desteklenir mi?

Azure yedekleme, hem şirket içi makinelerin ve Azure sanal makinelerini yedekleyebilirsiniz.

**Makine** | **Senaryoyu oluşturan yedekleme**
--- | ---
**Şirket içi makineler (fiziksel/sanal)** |  Tek şirket içi makineleri yedekleyebilir.<br/><br/>System Center Data Protection Manager (DPM) tarafından korunan şirket içi makinelerin yedekleyebilirsiniz<br/><br/> Microsoft Azure Backup sunucusu (MABS) tarafından korunan şirket içi makineleri yedekleyebilir...
**Azure Vm'leri** | Tek bir Azure sanal makinelerini yedekleyebilirsiniz.<br/><br/> Azure DPM veya MABS ile korunan sanal makineleri yedekleyebilirsiniz.

### <a name="back-up-servers"></a>Sunucularını yedekleme

Geri şirket içi sunucular ve iş yükleri veya Azure Vm'leri ve kendi iş yüklerini, ilk yedekleme sunucusuna ve kurtarma Hizmetleri kasası isteyebilirsiniz. 

**Backup sunucusu** | **Ayrıntılar**
--- | ---
**System Center Data Protection Manager (DPM)** | DPM tarafından korunan verileri yedeklemek için Azure Backup'ı kullanabilirsiniz:<br/><br/> -DPM şirket içi çalıştırıyor olabilir (fiziksel veya sanal) veya Azure.<br/><br/> -Farklı türde verileri DPM sunucusuna yedeklemenin tarafından şirket içi makinelerin ve Azure Vm'leri üzerinde çalışan verileri koruyabilirsiniz.<br/><br/> Sırayla DPM sunucusu Azure Yedekleme hizmetini kullanarak bir kurtarma Hizmetleri kasasına yedeklenebilir.<br/><br/> DPM sunucusu ve koruduğu makineler aynı ağda olması gerekir. Şirket içi makineler, yalnızca bir şirket içi DPM sunucusu tarafından korunabilir. Benzer şekilde, Azure Vm'leri Azure'da çalışan DPM tarafından yalnızca korunabilir.
**Microsoft Azure Backup sunucusu (MABS)** | MABS tarafından korunan verileri yedeklemek için Azure Backup'ı kullanabilirsiniz:<br/><br/> -MABS şirket içi çalıştırıyor olabilir (fiziksel veya sanal) veya Azure.<br/><br/> -Farklı türde verileri MABS yedeklemenin tarafından şirket içi makinelerin ve Azure Vm'leri üzerinde çalışan verileri koruyun.<br/><br/> -Sırayla MABS Azure Backup hizmetini kullanarak bir kurtarma Hizmetleri kasasına yedeklenebilir.<br/><br/> -MABS MABS kullanarak banda yedekleme yapamaz dışında DPM için benzer bir işlevsellik sağlar. MABS, System Center lisansı gerektirmez.<br/><br/> DPM gibi şirket içi makinelerin yalnızca bir şirket içi MABS tarafından korunabilir. Azure sanal makineler yalnızca azure'da bir MABS tarafından korunabilir.

İlk DPM/MABS ve ardından bir kasa için yedekleme avantajları şunlardır:

- Yedekleme DPM/MAB uygulama durumunu algılayan yedeklemelerini de SQL Server, Exchange ve SharePoint gibi sık kullanılan uygulamaları için iyileştirilmiş sağlayan ek dosya/klasör/birim yedeklemelerinin ve makine durumu yedeklemeleri (çıplak, sistem durumu).
- Azure Backup aracısının yedeklemek istediğiniz her makineye yüklemeniz gerekmez. Her makine DPM/MABS koruma aracısını çalıştırır ve DPM sunucusu/MABS üzerinde yalnızca Azure Yedekleme Microsoft Azure kurtarma Hizmetleri Aracısı'nı çalıştırır.
- Daha fazla esneklik ve yedeklemeleri için ayrıntılı zamanlama seçenekleri var.
- Tek bir konsolda koruma gruplarına toplamak birden çok makineleri için yedeklemeleri yönetebilir. Bu uygulamalar birden çok makinede yük katmanlı halde bulunan ve birlikte yedeklemek istediğiniz özellikle yararlı olur.

## <a name="what-can-be-backed-up"></a>Ne yedeklenebilir?

**Makine** | **Backup sunucusu** | **Yedekleme**
--- | --- | ---
Şirket içi Windows Vm'leri | DPM veya MABS yedeklenmeyen | Dosyaları, klasörleri, sistem durumu yedekleme.
Azure sanal makineleri (Windows ve Linux) | DPM veya MABS yedeklenmeyen | Dosyaları, klasörleri, sistem durumu yedekleme.<br/><br/> Uygulama durumunu algılayan Windows makineleri için ve dosya algılayan Linux makineler için yedeklemeler.
Şirket içi Vm'leri/Azure Vm'leri | DPM tarafından korunan | Geri herhangi bir şey DPM tarafından korunan dosyaları/klasörleri/paylaşımları/birimler ve uygulamaya özgü veriler dahil olmak üzere. [Bilgi](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807) ne DPM yedekleme yapabilirsiniz.
Şirket içi Vm'leri/Azure Vm'leri | MABS tarafından korunan | Geri herhangi bir şey MABS tarafından korunan dosyaları/klasörleri/paylaşımları/birimler ve uygulamaya özgü veriler dahil olmak üzere. [Bilgi](backup-mabs-protection-matrix.md) ne MABS yedekleyebilirsiniz.

## <a name="what-backup-agents-do-i-need"></a>Hangi yedekleme aracıları ihtiyacım var?
**Senaryo** | **Aracı** | **Ayrıntılar**
--- | --- | ---
Şirket içi makineleri (yedekleme sunucusuna yok) | Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı Windows makinede çalıştırır. | İndirin ve MARS Aracısı Azure Backup dağıtım sırasında yüklersiniz.<br/><br/> Yalnızca Windows makineler için destek.
Azure sanal makineler (yedekleme sunucusuna yok) | Açık Aracısı gerekli. Azure VM yedekleme için Azure VM uzantısı çalıştırır. | İlk Azure VM yedeklemesi çalıştırdığınızda uzantısı yüklenir.<br/><br/> Windows ve Linux desteği için destek.
Böylece, şirket içi DPM tarafından korunan makineler/Azure Vm'leri. | MARS Aracısı, DPM sunucusunda çalışır. | MARS Aracısı ayrı makinelerde gerekmez.<br/><br/> DPM koruma aracısını, DPM dağıttığınızda, koruduğunuz her makineye yüklenir. 
Şirket içi makinelerin ve Azure tarafından MABS korumalı Vm'leri yedekleyin | MARS Aracısı MABS üzerinde çalışır. | MARS Aracısı ayrı makinelerde gerekmez.<br/><br/> MABS dağıttığınızda, MABS koruma aracısını koruduğunuz her makineye yüklenir. 


## <a name="which-backup-agent-should-i-use"></a>Hangi yedekleme aracısı kullanmam gerekir?

**Backup** | **Çözüm** | **Sınırlama**
--- | --- | ---
Şirket içi Windows makineleri yedekleme istiyorsunuz. Makineleri DPM veya MABS tarafından korunmayan | MARS Aracısı makinesine yükleyin. | Dosyaları, klasörleri ve sistem durumu yedekleme Azure'a yedekleyebilirsiniz. Uygulama durumunu algılayan yedeklemelerini değildir.
Şirket içi Linux makineleri yedeklemek istiyorsanız. Makineleri DPM veya MABS tarafından korunmayan. | DPM veya MABS Azure'a yedeklemek için dağıtmanız gerekebilir.
Şirket içi Windows makinelerde çalışan uygulamaların yedeğini istiyorum | Uygulama durumunu algılayan yedekleme için DPM veya MABS Windows makineleri korunmalıdır.
Azure Vm'lerini yedekleme istiyorum | Azure Yedekleme'yi kullanarak bir yedekleme çalıştırın. Backup uzantısı Windows veya Linux Azure VM üzerinde otomatik olarak yapılandırılır. | VM diskleri yedeklenir.<br/><br/> Windows VM'ler için uygulamayla tutarlı yedeklemedir. Linux için yedekleme dosyası tutarlıdır. Uygulama durumunu algılayan gerekiyorsa bu özel betiklerle yapılandırmanız gerekir.
Azure Vm'lerini yedekleme ve kurtarma ayarları ayrıntılı esnekliği yedekleme istiyorum | DPM veya MABS yedekleme zamanlaması için daha fazla esneklik ve dosya, klasör, birimler, uygulamalar ve sistem durumu geri yükleme ve koruma için tam esneklik için Azure'da çalışan Azure sanal makinelerini koruyun.


## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme](backup-architecture.md) mimarisini ve bileşenlerini farklı yedekleme senaryoları için.
- [Doğrulama](backup-support-matrix.md) desteklenen özellikler ve ayarlar için yedekleme.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

