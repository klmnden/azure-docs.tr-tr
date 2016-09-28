<properties
    pageTitle="Azure Site Recovery ile hangi iş yüklerini koruyabilirsiniz?" 
    description="Azure Site Recovery, şirket içi sanal makinelerin ve fiziksel sunucuların Azure'a veya ikincil bir şirket içi siteye yönelik çoğaltma, yük devretme ve kurtarma işlemlerini koordine ederek iş yüklerinizi ve uygulamalarınızı korur." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/06/2016" 
    ms.author="raynew"/>


# Azure Site Recovery ile hangi iş yüklerini koruyabilirsiniz?


Bu makalede, Azure Site Recovery ile koruyabileceğiniz iş yükleri ve uygulamalar açıklanmaktadır.

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## Genel Bakış

Kuruluşlar; uygulamaların, iş yüklerinin ve verilerin planlanmış ve planlanmamış kesinti süreleri içinde nasıl kullanılabilir durumda kaldığını ve mümkün olan en kısa zamanda normal çalışma koşullarına nasıl döndüğünü belirleyen bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyar.

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure'a) veya ikincil bir siteye çoğaltılmasını düzenleyen ve böylece uygulama tabanlı dayanıklılık çözümleri sunarak BCDR stratejinize katkıda bulunan bir Azure hizmetidir. Uygulamalarınızın Windows tabanlı veya Linux tabanlı olması; fiziksel sunucularda, VMware'de ya da Hyper-V VM'lerinde çalışıyor olması fark etmeksizin, çoğaltmayı düzenleme, olağanüstü durum kurtarma testi yapma, yük devretme ve yeniden çalışma gibi işlemler için Site Recovery'yi kullanabilirsiniz.


Site Recovery; SharePoint, Exchange, Dynamics, SQL Server ve Active Directory dahil olmak üzere Microsoft uygulamalarıyla tümleşik bir şekilde çalışır. Ayrıca Microsoft, aralarında Oracle, SAP, IBM ve Red Hat'in de bulunduğu önde gelen sağlayıcılarla birlikte çalışarak uygulamalarının ve hizmetlerinin (Azure dahil olmak üzere) Microsoft platformlarında sıkıntısız bir şekilde çalışmasını sağlar. Bir çözümü her uygulama için özelleştirebilirsiniz.

## Uygulama koruması için neden Site Recovery'yi kullanmam gerekir?

Site Recovery, uygulama düzeyinde koruma ve kurtarmaya şu yollarla katkıda bulunur: 

- En önemli iş uygulamalarının ihtiyaçlarını karşılamak üzere 30 saniye kadar kısa RPO değerleriyle neredeyse tamamen zaman uyumlu çoğaltma.
- Tek veya çok katmanlı uygulamalar için uygulamayla tutarlı anlık görüntüler.
- SQL Server AlwaysOn tümleştirmesinin yanı sıra, AD çoğaltması, SQL AlwaysOn, Exchange Veritabanı Kullanılabilirlik Grupları (DAG'ler) ve Oracle Data Guard'ın içinde bulunduğu diğer uygulama düzeyi çoğaltma teknolojileriyle ortaklık.
- Uygulama yığınınızın tümünü tek tıklamayla kurtarmanıza ve harici betiklerin yanı sıra kendi eylemlerinizi de dahil etmenize olanak sağlayan esnek kurtarma planları. 
- Site Recovery'de ve Azure'da bulunan, IP adreslerini ayırabilme, yük dengelemeyi yapılandırabilme ve düşük RTO ağ geçişleri için Azure Traffic Manager ile tümleşik çalışabilme özelliklerini içeren, uygulama ağ gereksinimlerini basitleştirmeye yönelik gelişmiş ağ yönetimi.
-  İndirilebilen ve kurtarma planlarıyla tümleştirilebilen üretime hazır ve uygulamaya özgü betikler sağlayan zengin bir otomasyon kitaplığı.



##İş yükü özeti

Site Recovery, desteklenen bir sanal makinede çalışan herhangi bir uygulamayı çoğaltabilir. Ayrıca, uygulamaya özgü ek testler yapmak için ürün ekipleriyle de ortaklıklar kurduk.

**İş yükü** | **Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma**
---|---|---|---|---
Active Directory, DNS | E | E | E | E 
Web uygulamaları (IIS, SQL) | E | E | E | E
SCOM | E | E | E | E
SharePoint | E | E | E | E
SAP<br/><br/>Küme olmayan seçenekler için SAP sitesini Azure'a çoğaltma | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi)
Exchange (DAG olmayan) | E | Çok yakında | E | E
Uzak Masaüstü/VDI | E | E | E | Yok 
Linux (işletim sistemi ve uygulamalar) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) 
Dynamics AX | E | E | E | E
Dynamics CRM | E | Çok yakında | E | Çok yakında
Oracle | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi) | E (Microsoft tarafından test edildi)
Windows Dosya Sunucusu | E | E | E | E


## Active Directory'yi ve DNS'yi çoğaltma

Çoğu kurumsal uygulama için Active Directory ve DNS gereklidir. Olağanüstü durum kurtarma sırasında iş yüklerinizi ve uygulamalarınızı kurtarmadan önce bu altyapı bileşenlerini korumanız ve kurtarmanız gerekir.

Site Recovery'yi kullanarak Active Directory ve DNS için tamamen otomatik bir olağanüstü durum kurtarma planı oluşturabilirsiniz. Örneğin, birincil bir siteden ikincil bir siteye SharePoint ve SAP yük devretmesi gerçekleştirmek istiyorsanız ilk önce Active Directory'ye yük devretmesini gerçekleştiren bir kurtarma planı; daha sonra da Active Directory'ye bağlı olan diğer uygulamaların yük devretmesini gerçekleştirmek için uygulamaya özgü bir ek kurtarma planı ayarlayabilirsiniz.

Active Directory'yi ve DNS'yi koruma hakkında [daha fazla bilgi edinin](site-recovery-active-directory.md).

## SQL Server'ı koruma

SQL Server, şirket içi bir veri merkezinde birçok iş uygulamasına yönelik veri hizmetleri için bir veri hizmetleri altyapısı sağlar.  Site Recovery, SQL Server kullanan çok katmanlı kurumsal uygulamaları koruma amacıyla SQL Server HA/DR teknolojileriyle birlikte kullanılabilir. Site Recovery şunları sağlar:

- SQL Server için basit ve ekonomik bir olağanüstü durum kurtarma çözümü. SQL Server tek başına sunucularının ve kümelerinin birden çok sürümünü Azure'a veya ikincil bir siteye çoğaltma olanağı.  
- Azure Site Recovery kurtarma planları ile yük devretme ve yeniden çalışma gibi işlemleri yönetmek için SQL AlwaysOn Kullanılabilirlik Grupları ile tümleştirme olanağı.
- SQL Server veritabanları dahil olmak üzere bir uygulamadaki tüm katmanlara yönelik uçtan uca kurtarma planları.
- SQL Server'ın, azami yükleri Site Recovery ile Azure içindeki daha büyük IaaS sanal makinesi boyutlarına "yığılarak" ölçeklendirilmesi 
- Kolay SQL Server olağanüstü durum kurtarma testi Üretim ortamınızı etkilemeden uyumluluk denetimlerini çalıştırmak ve verileri analiz etmek ve için test amaçlı yük devretme işlemini çalıştırabilirsiniz.

SQL Server'ı koruma hakkında [daha fazla bilgi edinin](site-recovery-sql.md).

##SharePoint'i koruma

Azure Site Recovery, SharePoint dağıtımlarının korunmasına şu şekilde yardımcı olur:

- Olağanüstü durum kurtarma için yedek bir gruba olan ihtiyacı ve bununla ilgili altyapı maliyetini ortadan kaldırır. Bir grubun tümünü (Web, uygulama ve veritabanı katmanları) Azure'a veya ikincil bir siteye çoğaltmak için Site Recovery'yi kullanın.
- Uygulama dağıtımını ve yönetimini basitleştirir. Birincil siteye dağıtılan güncelleştirmeler otomatik olarak çoğaltılır ve bir grubun ikincil bir siteye yönelik yük devretme ve kurtarma işlemlerinin ardından hemen kullanılabilir. Ayrıca, yedek bir grubun güncel tutulmasına ilişkin maliyetleri ve yönetim karmaşıklığını azaltır.
- Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya çoğaltma ortamı oluşturarak SharePoint uygulaması için geliştirme sürecini basitleştirir.
- SharePoint dağıtımlarını Azure'a geçirmek için Site Recovery'yi kullanarak buluta geçişi basitleştirir.

SharePoint'i koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae).


## Dynamics AX'i koruma

Azure Site Recovery, Dynamics AX ERP çözümünüzü korumanıza şu şekillerde yardımcı olur:

- Tüm Dynamics AX ortamınızı (Web ve AOS katmanları, veritabanı katmanları, SharePoint) Azure'a veya ikincil bir siteye çoğaltma işleminizi düzenler.
- Dynamics AX dağıtımlarının buluta (Azure'a) geçişini basitleştirir.
- Test ve hata ayıklama işlemleri için üretim benzeri isteğe bağlı bir kopya oluşturarak Dynamics AX uygulamasının geliştirme ve test sürecini basitleştirir.

Dynamic AX'i koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) 

## RDS'yi koruma 

Uzak Masaüstü Hizmetleri (RDS), sanal masaüstü altyapısını (VDI), oturum tabanlı masaüstlerini ve uygulamaları etkinleştirerek kullanıcıların herhangi bir yerden çalışabilmesine olanak sağlar. Azure Site Recovery ile şunları yapabilirsiniz:

- Yönetilen veya yönetilmeyen, havuza alınmış sanal masaüstlerini ikincil bir siteye; uzak uygulamaları ve oturumları ise ikincil bir siteye ya da Azure'a çoğaltın
- Şunları çoğaltabilirsiniz:

**RDS** | **Hyper-V VM'lerini ikincil bir siteye çoğaltma** | **Hyper-V VM'lerini Azure'a çoğaltma** | **VMware VM'lerini ikincil bir siteye çoğaltma** | **VMware VM'lerini Azure'a çoğaltma** | **Fiziksel sunucuları ikincil bir siteye çoğaltma** | **Fiziksel sunucuları Azure'a çoğaltma**
---|---|---|---|---|---|---
**Havuza Alınmış Sanal Masaüstü (yönetilmeyen)** | Yes | Hayır | Yes | Hayır | Yes | Hayır
**Havuza Alınmış Sanal Masaüstü (yönetilen ve UPD'siz)** | Yes | Hayır | Yes | Hayır | Yes | Hayır
**Uzak uygulamalar ve Masaüstü oturumları (UPD'siz)** | Yes | Evet | Evet | Evet | Evet | Yes


RDS'yi koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb).


## Exchange'i koruma

Site Recovery, Exchange'in korunmasına şu şekilde yardımcı olur:

- Site Recovery, tek veya kümelenmemiş sunucular gibi küçük Exchange dağıtımları için Azure'a veya ikincil bir siteye çoğaltma ve yük devretme yapabilir.
- Site Recovery, daha büyük dağıtımlar için Exchange DAG'leri ile tümleşir.
- Exchange DAG'leri, kuruluşlarda Exchange olağanüstü durum kurtarma işlemleri için önerilen çözümdür.  Site Recovery kurtarma planlarında siteler arası DAG yük devretmelerini düzenlemek üzere DAG'ler bulunabilir.


Exchange'i koruma hakkında [daha fazla bilgi edinin](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6).

## SAP'yi koruma

Site Recovery'yi kullanarak SAP dağıtımınızı şu şekilde koruyun: 

- Azure'a veya ikincil bir siteye farklı dağıtım katmanları çoğaltarak tüm SAP dağıtımının korunmasını sağlayın.
- SAP dağıtımınızı Azure'a geçirmek için Site Recovery'yi kullanarak bulut geçişini basitleştirin.
- Test ve hata ayıklama uygulamaları için isteğe bağlı üretim benzeri bir kopya oluşturarak SAP geliştirmesini ve testini basitleştirin.

SAP'yi koruma hakkında [daha fazla bilgi edinin](http://aka.ms/asr-sap).

## Sonraki adımlar

Site Recovery dağıtımına [hazırlanma](site-recovery-best-practices.md)




<!--HONumber=Sep16_HO3-->


