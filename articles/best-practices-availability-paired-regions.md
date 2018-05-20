---
title: 'İş devamlılığı ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri | Microsoft Docs'
description: Uygulamaların veri merkezi hataları sırasında dayanıklı olmasını sağlamak için Azure bölgesel eşleme hakkında bilgi edinin.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: multiple
ms.topic: article
ms.date: 05/09/2018
ms.author: raynew
ms.openlocfilehash: e2c288af881fa925c1680efdb0f86deec60b7510
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>İş devamlılığı ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri

## <a name="what-are-paired-regions"></a>Hangi bölgeleri eşleştirilmiş?

Azure dünyanın birden çok coğrafi olarak çalışır. Bir Azure Coğrafya, en az bir Azure bölgesi içeren dünya tanımlı bir alandır. Bir Azure bölgesine bir veya daha fazla veri merkezleri içeren bir coğrafi konum içinde bir alandır.

Her Azure bölgesi birlikte bölgesel çifti yapmadan aynı coğrafi konum içinde başka bir bölge ile eşleştirilmiş. Kendi Coğrafya dışında bir bölge ile eşleştirilmiş Brezilya Güney istisnadır.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Şekil 1 – Azure Bölgesel çiftleri

| coğrafi konum | Eşleştirilmiş bölgeler |  |
|:--- |:--- |:--- |
| Asya |Doğu Asya |Güneydoğu Asya |
| Avustralya |Avustralya Doğu |Avustralya Güneydoğu |
| Avustralya |Avustralya Orta |Avustralya Orta (2) |
| Brezilya |Brezilya Güney (2) |Orta Güney ABD |
| Kanada |Kanada Orta |Doğu Kanada |
| Çin |Çin Kuzey |Çin Doğu|
| Avrupa |Kuzey Avrupa |Batı Avrupa |
| Almanya |Almanya Orta |Almanya Kuzeydoğu |
| Hindistan |Orta Hindistan |Güney Hindistan |
| Hindistan |Batı Hindistan (1) |Güney Hindistan |
| Japonya |Japonya Doğu |Japonya Batı |
| Kore |Kore Orta |Kore Güney |
| Kuzey Amerika |Doğu ABD |Batı ABD |
| Kuzey Amerika |Doğu ABD 2 |Orta ABD |
| Kuzey Amerika |Orta Kuzey ABD |Orta Güney ABD |
| Kuzey Amerika |Batı ABD 2 |Batı Orta ABD 
| UK |Birleşik Krallık Batı |Birleşik Krallık Güney |
| ABD Savunma Bakanlığı |US DoD Doğu |US DoD Orta |
| ABD Devleti |ABD Devleti Arizona |ABD Devleti Texas |
| ABD Devleti |ABD kamu Iowa (3) |ABD Devleti Virginia |
| ABD Devleti |ABD kamu Virginia (4) |ABD Devleti Texas |

Tablo 1 - Azure bölgesel çiftlerini eşleme

- (1) Batı Hindistan, yalnızca bir yöndeki başka bir bölge ile eşleştirilmiş olduğundan farklıdır. Güney Hindistan Batı Hindistan'ın ikincil bölge, ancak orta Hindistan Güney Hindistan'ın ikincil bölge gelir.
- (2) Brezilya Güney benzersiz çünkü kendi Coğrafya dışında bir bölge ile eşlenmiş. Brezilya Güney'nın ikincil bölge Orta Güney ABD, ancak orta Güney ABD'ın ikincil bölge Brezilya Güney değil.
- (3) ABD kamu Iowa'nın ikincil bölge BİZE kamu Virginia ancak BİZE kamu Virginia'nın ikincil bölge BİZE kamu Iowa değil.
- (4) ABD kamu Virginia'nın ikincil bölge BİZE kamu Texas ancak BİZE kamu Texas ikincil bölge BİZE kamu Virginia değil.


Azure'nın yalıtım ve kullanılabilirlik ilkelerden yararlanmak için Bölgesel çiftleri arasında iş yükleri çoğaltmak öneririz. Örneğin, planlı Azure sistem güncelleştirmeleri sırayla dağıtılır (değil, aynı anda) eşleştirilmiş bölgeler arasında. Hatta ender olayda hatalı bir güncelleştirme, her iki bölgeleri aynı anda etkilenmez, anlamına gelir. Ayrıca, geniş bir kesinti olasılığı olayda her çifti dışında en az bir bölge kurtarılması öncelik.

## <a name="an-example-of-paired-regions"></a>Eşleştirilmiş bölgeler örneği
Şekil 2'in altında olağanüstü durum kurtarma için bölgesel çifti kullanan kuramsal bir uygulamayı gösterir. Yeşil sayıları (Azure, depolama, işlem ve veritabanı) üç Azure Hizmetleri ve bölgeler arasında çoğaltmak için nasıl yapılandırıldığına çapraz bölge etkinliklerini vurgulayın. Eşleştirilmiş bölgeler arasında dağıtma benzersiz avantajları turuncu numaralarına göre vurgulanır.

![Eşleştirilmiş bölge avantajları genel bakış](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Şekil 2 – kuramsal Azure bölgesel çifti

## <a name="cross-region-activities"></a>Çapraz bölge etkinlikleri
Şekil 2 başvurulan gibi.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure işlem (PaaS)** – kaynaklar kullanılabilir başka bir bölgede bir olağanüstü durum sırasında önceden sağlamak için ek işlem kaynakları sağlamanız gerekir. Daha fazla bilgi için bkz: [Azure dayanıklılık teknik kılavuz](resiliency/resiliency-technical-guidance.md).

![Depolama](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -bir Azure depolama hesabı oluşturduğunuzda, coğrafi olarak yedekli depolama (GRS) varsayılan olarak yapılandırılır. GRS ile otomatik olarak üç kez verilerinizi, birincil bölge içinde ve üç kez eşleştirilmiş bölgede çoğaltılır. Daha fazla bilgi için bkz: [Azure depolama artıklığı seçeneği](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL veritabanlarını** – ile Azure SQL standart coğrafi çoğaltma, eşleştirilmiş bir bölge için işlemleri zaman uyumsuz olarak çoğaltılmasını yapılandırabilirsiniz. Premium coğrafi çoğaltma ile dünyanın her bölge için çoğaltma yapılandırabilirsiniz; Ancak, bu kaynakları çoğu olağanüstü durum kurtarma senaryoları için eşlenmiş bir bölgede dağıttığınız öneririz. Daha fazla bilgi için bkz: [Azure SQL Database coğrafi çoğaltma](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Resource Manager kendiliğinden bölgeler arasında Hizmet Yönetim bileşenlerinin mantıksal yalıtım sağlar. Bu mantıksal hataları bir bölgede başka bir etkisi olasılığı daha düşüktür anlamına gelir.

## <a name="benefits-of-paired-regions"></a>Eşleştirilmiş bölgeler yararları
Şekil 2 başvurulan gibi.  

![Yalıtım](./media/best-practices-availability-paired-regions/5Orange.png)
**fiziksel yalıtım** – olası, Azure, bu tüm coğrafi bölgelerde pratik veya mümkün olmasa da, en az 300 mil bölgesel çiftindeki veri merkezleri arasında ayrım tercih eder. Fiziksel veri merkezi ayrımı doğal afetler, hukuki unrest, güç kesintileri veya aynı anda hem bölgeler etkilemeden fiziksel ağ kesintileri olasılığını azaltır. Yalıtım kısıtlamaları Coğrafya (Coğrafya boyutu, güç/ağ altyapısı kullanılabilirlik, düzenlemeler, vb.) içinde tabidir.  

![Çoğaltma](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform tarafından sağlanan çoğaltma** -eşleştirilmiş bölgeye otomatik çoğaltma coğrafi olarak yedekli depolama alanı gibi bazı hizmetler sağlar.

![Kurtarma](./media/best-practices-availability-paired-regions/7Orange.png)
**bölge kurtarma sipariş** – geniş bir kesinti her çifti dışında bir bölge kurtarılması öncelik durumunda. Eşleştirilmiş bölgeler arasında dağıtılan uygulamalar için bir öncelik ile kurtarılan bölgelerinin garanti. Bir uygulama değil eşleştirilmelidir bölgeler arasında dağıtılırsa, Kurtarma – seçilen bölgeler kurtarılacak son iki olabilir kötü durumda gecikebilir.

![Güncelleştirmeleri](./media/best-practices-availability-paired-regions/8Orange.png)
**sıralı güncelleştirme** – planlanan Azure sistem güncelleştirmelerini yapılır eşleştirilmiş bölgelere sırayla (değil, aynı anda) kapalı kalma süresi, hataları ve bozuk ender olayda mantıksal hatalarının etkisini en aza indirmek için güncelleştirin.

![Veri](./media/best-practices-availability-paired-regions/9Orange.png)
**veri residency** – vergi ve yasa zorlama dairesi amaçları için veri residency gereksinimlerini karşılamak için çiftini (hariç Brezilya Güney) olarak aynı Coğrafya içinde bir bölgede bulunuyor.
