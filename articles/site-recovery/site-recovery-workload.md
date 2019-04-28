---
title: Azure Site Recovery ile hangi iş yüklerini koruyabilirsiniz? | Microsoft Docs
description: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma kullanılarak korunabilen iş yüklerini açıklar.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 12/31/2018
ms.author: raynew
ms.openlocfilehash: 3ad3438f02ead9063a683a39d4ac5823274d55f7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276146"
---
# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Azure Site Recovery ile hangi iş yüklerini koruyabilirsiniz?

Bu makalede iş yükleri ve uygulamalar için olağanüstü durum kurtarma ile Koruyabileceğiniz [Azure Site Recovery](site-recovery-overview.md) hizmeti.



## <a name="overview"></a>Genel Bakış

Kuruluşlar; uygulamaları, iş yüklerini ve verileri planlanmış ve planlanmamış kesinti süreleri içinde güvenli ve kullanılabilir durumda tutan ve mümkün olan en kısa zamanda normal çalışma koşullarına dönmelerini sağlayan bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Site Recovery, BCDR stratejinize katkıda bulunan bir Azure hizmetidir. Site Recovery’yi kullanarak, buluta veya ikincil bir siteye uygulamayla tutarlı çoğaltma dağıtabilir. Uygulamalarınızın Windows tabanlı veya Linux tabanlı olmasına ya da fiziksel sunucularda, VMware'de ya da Hyper-V’de çalışıyor olmasına bakılmaksızın, çoğaltmayı düzenleme, olağanüstü durum kurtarma testi yapma, yük devretme ve yeniden çalışma gibi işlemler için Site Recovery'yi kullanabilirsiniz.

Site Recovery; SharePoint, Exchange, Dynamics, SQL Server ve Active Directory dahil olmak üzere Microsoft uygulamalarıyla tümleşik bir şekilde çalışır. Microsoft; Oracle, SAP ve Red Hat gibi önde gelen satıcılarla da yakın bir şekilde çalışır. Çoğaltma çözümlerini her uygulama için ayrı olarak özelleştirebilirsiniz.

## <a name="why-use-site-recovery-for-application-replication"></a>Uygulama çoğaltma için neden Site Recovery'yi kullanmam gerekir?

Site Recovery, uygulama düzeyinde koruma ve kurtarmaya şu yollarla katkıda bulunur:

* Desteklenen makinede çalışan iş yükleri için uygulaması belirsiz çoğaltma.
* En önemli iş uygulamalarının ihtiyaçlarını karşılamak üzere 30 saniye kadar kısa RPO değerleriyle neredeyse tamamen zaman uyumlu çoğaltma.
* Tek veya çok katmanlı uygulamalar için uygulamayla tutarlı anlık görüntüler.
* SQL Server AlwaysOn tümleştirmesinin yanı sıra, AD çoğaltması, SQL AlwaysOn, Exchange Veritabanı Kullanılabilirlik Grupları (DAG'ler) ve Oracle Data Guard'ın içinde bulunduğu diğer uygulama düzeyi çoğaltma teknolojileriyle ortaklık.
* Uygulama yığınınızın tümünü tek tıklamayla kurtarmanıza ve harici betiklerin yanı sıra kendi eylemlerinizi de dahil etmenize olanak sağlayan esnek kurtarma planları.
* Site Recovery'de ve Azure'da bulunan, IP adreslerini ayırabilme, yük dengelemeyi yapılandırabilme ve düşük RTO ağ geçişleri için Azure Traffic Manager ile tümleştirme özelliklerini içeren, uygulama ağ gereksinimlerini basitleştirmeye yönelik gelişmiş ağ yönetimi.
* İndirilebilen ve kurtarma planlarıyla tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlayan zengin bir otomasyon kitaplığı.

## <a name="workload-summary"></a>İş yükü özeti
Site Recovery, desteklenen bir makinede çalışan herhangi bir uygulamayı çoğaltabilir. Ayrıca, uygulamaya özgü ek testler yapmak için ürün ekipleriyle de ortaklıklar kurduk.

| **İş yükü** |**Azure VM’lerini Azure’a çoğaltma** |**Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma** |
| --- | --- | --- | --- | --- |---|
| Active Directory, DNS |E |E |E |E |E|
| Web uygulamaları (IIS, SQL) |E |E |E |E |E|
| System Center Operations Manager |E |E |E |E |E|
| SharePoint |E |E |E |E |E|
| SAP<br/><br/>Küme olmayan seçenekler için SAP sitesini Azure'a çoğaltma |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi)|
| Exchange (DAG olmayan) |E |E |E |E |E|
| Uzak Masaüstü/VDI |E |E |E |E |E|
| Linux (işletim sistemi ve uygulamalar) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi) |E (Microsoft tarafından test edildi)|
| Dynamics AX |E |E |E |E |E|
| Windows Dosya Sunucusu |E |E |E |E |E|
| Citrix XenApp ve XenDesktop |E|Yok |E |Yok |E |

## <a name="replicate-active-directory-and-dns"></a>Active Directory'yi ve DNS'yi çoğaltma
Çoğu kurumsal uygulama için Active Directory ve DNS gereklidir. Olağanüstü durum kurtarma sırasında, iş yüklerinizi ve uygulamalarınızı kurtarmadan önce bu altyapı bileşenlerini korumanız ve kurtarmanız gerekir.

Site Recovery'yi kullanarak Active Directory ve DNS için tamamen otomatik bir olağanüstü durum kurtarma planı oluşturabilirsiniz. Örneğin, birincil bir siteden ikincil bir siteye SharePoint ve SAP yük devretmesi gerçekleştirmek istiyorsanız ilk önce Active Directory'ye yük devretmesini gerçekleştiren bir kurtarma planı, daha sonra da Active Directory'ye bağlı olan diğer uygulamaların yük devretmesini gerçekleştirmek için uygulamaya özgü bir ek kurtarma planı ayarlayabilirsiniz.

Active Directory'yi ve DNS'yi koruma hakkında [daha fazla bilgi edinin](site-recovery-active-directory.md).

## <a name="protect-sql-server"></a>SQL Server'ı koruma
SQL Server, şirket içi bir veri merkezinde birçok iş uygulamasına yönelik veri hizmetleri için bir veri hizmetleri altyapısı sağlar.  Site Recovery, SQL Server kullanan çok katmanlı kurumsal uygulamaları korumak için SQL Server HA/DR teknolojileriyle birlikte kullanılabilir. Site Recovery şunları sağlar:

* SQL Server için basit ve ekonomik bir olağanüstü durum kurtarma çözümü. SQL Server tek başına sunucularının ve kümelerinin birden çok sürümünü Azure'a veya ikincil bir siteye çoğaltma olanağı.  
* Azure Site Recovery kurtarma planları ile yük devretme ve yeniden çalışma gibi işlemleri yönetmek için SQL AlwaysOn Kullanılabilirlik Grupları ile tümleştirme olanağı.
* SQL Server veritabanları dahil olmak üzere bir uygulamadaki tüm katmanlara yönelik uçtan uca kurtarma planları.
* SQL Server'ın, azami yükleri Site Recovery ile Azure içindeki daha büyük IaaS sanal makinesi boyutlarına "yığılarak" ölçeklendirilmesi.
* Kolay SQL Server olağanüstü durum kurtarma testi Üretim ortamınızı etkilemeden uyumluluk denetimlerini çalıştırmak ve verileri analiz etmek için test amaçlı yük devretme işlemini çalıştırabilirsiniz.

SQL Server'ı koruma hakkında [daha fazla bilgi edinin](site-recovery-sql.md).

## <a name="protect-sharepoint"></a>SharePoint'i koruma
Azure Site Recovery, SharePoint dağıtımlarının korunmasına şu şekilde yardımcı olur:

* Olağanüstü durum kurtarma için yedek bir gruba olan ihtiyacı ve bununla ilgili altyapı maliyetini ortadan kaldırır. Bir grubun tümünü (Web, uygulama ve veritabanı katmanları) Azure'a veya ikincil bir siteye çoğaltmak için Site Recovery'yi kullanın.
* Uygulama dağıtımını ve yönetimini basitleştirir. Birincil siteye dağıtılan güncelleştirmeler otomatik olarak çoğaltılır ve bir grubun ikincil bir siteye yönelik yük devretme ve kurtarma işlemlerinin ardından hemen kullanılabilir. Ayrıca, yedek bir grubun güncel tutulmasına ilişkin maliyetleri ve yönetim karmaşıklığını azaltır.
* Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya çoğaltma ortamı oluşturarak SharePoint uygulaması için geliştirme sürecini basitleştirir.
* SharePoint dağıtımlarını Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirir.

SharePoint'i koruma hakkında [daha fazla bilgi edinin](site-recovery-sharepoint.md).

## <a name="protect-dynamics-ax"></a>Dynamics AX'i koruma
Azure Site Recovery, Dynamics AX ERP çözümünüzün korunmasına şu şekillerde yardımcı olur:

* Tüm Dynamics AX ortamınızı (Web ve AOS katmanları, veritabanı katmanları, SharePoint) Azure'a veya ikincil bir siteye çoğaltma işleminizi düzenler.
* Dynamics AX dağıtımlarının buluta (Azure'a) geçişini basitleştirir.
* Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya oluşturarak Dynamics AX uygulamasının geliştirme ve test sürecini basitleştirir.

Dynamic AX'i koruma hakkında [daha fazla bilgi edinin](site-recovery-dynamicsax.md) 

## <a name="protect-rds"></a>RDS'yi koruma
Uzak Masaüstü Hizmetleri (RDS), sanal masaüstü altyapısını (VDI), oturum tabanlı masaüstlerini ve uygulamaları etkinleştirerek kullanıcıların herhangi bir yerden çalışabilmesine olanak sağlar. Azure Site Recovery ile şunları yapabilirsiniz:

* Yönetilen veya yönetilmeyen, havuza alınmış sanal masaüstlerini ikincil bir siteye; uzak uygulamaları ve oturumları ise ikincil bir siteye ya da Azure'a çoğaltın

* Şunları çoğaltabilirsiniz:

| **RDS** |**Azure VM’lerini Azure’a çoğaltma** | **Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma** | **Fiziksel sunucuları ikincil bir siteye çoğaltma** | **Fiziksel sunucuları Azure'a çoğaltma** |
|---| --- | --- | --- | --- | --- | --- | --- |
| **Havuza Alınmış Sanal Masaüstü (yönetilmeyen)** |Hayır|Evet |Hayır |Evet |Hayır |Evet |Hayır |
| **Havuza Alınmış Sanal Masaüstü (yönetilen ve UPD'siz)** |Hayır|Evet |Hayır |Evet |Hayır |Evet |Hayır |
| **Uzak uygulamalar ve Masaüstü oturumları (UPD'siz)** |Evet|Evet |Evet |Evet |Evet |Evet |Evet |

[Azure Site Recovery kullanarak RDS için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-disaster-recovery-with-azure).

RDS'yi koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb).

## <a name="protect-exchange"></a>Exchange'i koruma
Site Recovery, Exchange'in korunmasına şu şekilde yardımcı olur:

* Site Recovery, tek veya tek başına sunucu gibi küçük Exchange dağıtımları için Azure'a veya ikincil bir siteye çoğaltma ve yük devretme yapabilir.
* Site Recovery, daha büyük dağıtımlar için Exchange DAG'leri ile tümleşir.
* Exchange DAG'leri, kuruluşlarda Exchange olağanüstü durum kurtarma işlemleri için önerilen çözümdür.  Site Recovery kurtarma planlarında siteler arası DAG yük devretmelerini düzenlemek üzere DAG'ler bulunabilir.

Exchange'i koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6).

## <a name="protect-sap"></a>SAP'yi koruma
Site Recovery'yi kullanarak SAP dağıtımınızı şu şekilde koruyun:

* Bileşenleri Azure'a çoğaltarak, şirket içinde çalışan SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
* Bileşenleri başka bir Azure veri merkezine çoğaltarak,Azure çalıştıran SAP NetWeaver ve NetWeaver dışı üretim uygulamalarının korumasını etkinleştirin.
* SAP dağıtımınızı Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirin.
* SAP uygulamaları test etmek için talep üzerine üretim kopyası oluşturarak SAP proje yükseltmelerini, testlerini ve prototip oluşturma işlemlerini basitleştirin.

SAP'yi koruma hakkında [daha fazla bilgi edinin](site-recovery-sap.md).

## <a name="protect-iis"></a>IIS Koruma
Site Recovery'yi kullanarak IIS dağıtımınızı şu şekilde koruyun:

Azure Site Recovery, ortamınızdaki kritik bileşenleri soğuk bir uzak konuma veya Microsoft Azure gibi bir genel buluta çoğaltarak olağanüstü durum kurtarma sağlar. Web sunucusu ve veritabanı ile sanal makineler, kurtarma konumuna çoğaltıldığı için yapılandırma dosyalarının veya sertifikaların ayrıca yedeklenmesi gerekmez. Yük devretme sonrasında değişen ortam değişkenlerine bağımlı uygulama eşlemeleri ve bağlamaları, olağanüstü durum kurtarma planlarına tümleştirilmiş betikler aracılığıyla güncelleştirilebilir. Sanal makineler yalnızca yük devretme durumunda kurtarma konumuna getirilir. Azure Site Recovery ayrıca aşağıdaki özellikleri sağlayarak uçtan uca yük devretmeyi düzenlemenize yardımcı olur:

-   Çeşitli katmanlardaki sanal makinelerin kapatma ve başlatma sıralaması.
-   Uygulama bağımlılıkları ve başlatıldıktan sonra sanal makineler üzerindeki bağlamaların güncelleştirilmesine olanak tanıyan betikler ekleme. Betikler ayrıca DNS sunucusunu kurtarma konumunu işaret edecek şekilde güncelleştirmek için kullanılabilir.
-   Birincil ve kurtarma ağlarını eşleyerek yük devretme öncesinde sanal makinelere IP adresleri ayırın ve böylece yük devretme sonrasında güncelleştirilmesi gerekmeyen betikler kullanın.
-   Web sunucularında birden fazla web uygulaması için tek tıklamayla yük devretme yapabilme olanağı, böylece olağanüstü bir durumda karışıklık kapsamını ortadan kaldırma.
-   DR ayrıntıları için yalıtılmış bir ortamda kurtarma planlarını test edebilme olanağı.

IIS web grubunu koruma hakkında [daha fazla bilgi edinin](https://aka.ms/asr-iis).

## <a name="protect-citrix-xenapp-and-xendesktop"></a>Citrix XenApp ve XenDesktop’u koruma
Citrix XenApp ve XenDesktop dağıtımlarınızı korumak için aşağıdaki gibi Site Recovery kullanın:

* Citrix XenApp ve XenDesktop dağıtımının korumasını etkinleştirmek için, (AD DNS sunucusu, SQL veritabanı sunucusu, Citrix Delivery Controller, StoreFront sunucusu, XenApp Master (VDA), Citrix XenApp License Server) gibi farklı dağıtım katmanlarını Azure’a çoğaltın.
* Citrix XenApp ve XenDesktop dağıtımınızı Azure’a geçirmek için Site Recovery’yi kullanarak buluta geçişi basitleştirin.
* Test ve hata ayıklama uygulamaları için isteğe bağlı üretim benzeri bir kopya oluşturarak Citrix XenApp/XenDesktop testlerini basitleştirin.
* Bu çözüm yalnızca Windows Server işletim sistemi sanal masaüstleri için geçerli olup, istemci sanal masaüstlerinin lisanslaması Azure’da henüz desteklenmediğinden istemci sanal masaüstleri için geçerli değildir.
Azure’da istemci/sunucu masaüstlerini lisanslama hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/licensing-faq/).

Citrix XenApp ve XenDesktop dağıtımlarını koruma hakkında [daha fazla bilgi edinin](site-recovery-citrix-xenapp-and-xendesktop.md). Alternatif olarak, aynı konuyla ilgili ayrıntılı bilgilerin sunulduğu [Citrix teknik incelemesine](https://aka.ms/citrix-xenapp-xendesktop-with-asr) başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM çoğaltma ile [çalışmaya başlayın](azure-to-azure-quickstart.md).
