---
title: 'İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri | Microsoft Docs'
description: Uygulamaları sırasında veri merkezi arızalarına karşı dayanıklı olmasını sağlamak için Azure bölgesel eşleme hakkında bilgi edinin.
author: rayne-wiselman
manager: carmon
ms.service: multiple
ms.topic: article
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: e23b5ff9917eda7272e378aa70d6e2dd79f4b9f1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918962"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri

## <a name="what-are-paired-regions"></a>Eşleştirilmiş bölgeleri nelerdir?

Azure dünyanın dört bir yanındaki birden çok coğrafi çalışmaktadır. Her Azure coğrafyası dünyanın en az bir Azure bölgesi içeren tanımlanmış bir alandır. Bir Azure bölgesine bir veya daha fazla veri içeren bir coğrafyadaki alanıdır.

Her Azure bölgesi aynı coğrafyadaki birlikte bölgesel çift yaparak başka bir bölgeyle eşleştirilir. Brezilya Güney, kendi Coğrafya dışında bir bölgeyle eşleştirilir istisnadır. Bölge çiftlerinin arasında Azure platform güncelleştirmelerinin (Planlı bakım) serileştiren, böylece yalnızca bir eşleştirilmiş bölge aynı anda güncelleştirilir. Kurtarma için birden çok bölgede etkileyen bir kesinti olması durumunda en az bir bölge çiftindeki her öncelik verilir.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Şekil 1 – Azure bölgesel çiftler

| Coğrafya | Eşleştirilmiş bölgeler |  |
|:--- |:--- |:--- |
| Asya |Doğu Asya |Güneydoğu Asya |
| Avustralya |Avustralya Doğu |Avustralya Güneydoğu |
| Avustralya |Avustralya Orta |Avustralya Orta 2 |
| Brezilya |Güney Brezilya |Orta Güney ABD |
| Kanada |Kanada Orta |Doğu Kanada |
| Çin |Çin Kuzey |Çin Doğu|
| Çin |Çin Kuzey 2 |Çin Doğu 2|
| Avrupa |Kuzey Avrupa |Batı Avrupa |
| Fransa |Fransa Orta|Fransa Güney|
| Almanya |Almanya Orta |Almanya Kuzeydoğu |
| Hindistan |Orta Hindistan |Güney Hindistan |
| Hindistan |Batı Hindistan |Güney Hindistan |
| Japonya |Japonya Doğu |Japonya Batı |
| Güney Kore |Kore Orta |Kore Güney |
| Kuzey Amerika |Doğu ABD |Batı ABD |
| Kuzey Amerika |Doğu ABD 2 |Orta ABD |
| Kuzey Amerika |Orta Kuzey ABD |Orta Güney ABD |
| Kuzey Amerika |Batı ABD 2 |Batı Orta ABD 
| Güney Afrika | Güney Afrika Kuzey | Güney Afrika Batı
| UK |Birleşik Krallık Batı |Birleşik Krallık Güney |
| Birleşik Arap Emirlikleri | BAE Kuzey | BAE Orta
| ABD Savunma Bakanlığı |US DoD Doğu |US DoD Orta |
| ABD Devleti |ABD Devleti Arizona |ABD Devleti Texas |
| ABD Devleti |US Gov Iowa |ABD Devleti Virginia |
| ABD Devleti |ABD Devleti Virginia |ABD Devleti Texas |

Tablo 1 - Azure bölgesel çiftler eşleme

- Batı Hindistan, tek yönlü eşleştirilir. Güney Hindistan Batı Hindistan'ın ikincil bölgeye olduğu halde Orta Hindistan Güney Hindistan'ın ikincil bölgeye olduğu.
- Brezilya Güney benzersiz olduğundan, kendi Coğrafya dışında bir bölgeyle eşleştirilir. Brezilya Güney'nın ikincil bölgeye Orta Güney ABD ' dir. Güney Brezilya Güney Orta ABD'ın ikincil bölgeye değil.
- ABD Devleti Iowa'nın ikincil bölgeye ABD Devleti Virginia ' dir.
- ABD Devleti Virginia'nın ikincil bölgeye US Gov Teksas ' dir.
- ABD Devleti Texas ikincil bölgeye ABD Devleti Arizona ' dir.


İş sürekliliği, olağanüstü durum kurtarma (BCDR) yapılandırma Azure'un yalıtım ve kullanılabilirlik ilkelerinden yararlanmak için bölgesel çiftler arasında öneririz. Birden çok etkin bölgeler destekleyen uygulamalar için mümkün olduğu durumlarda bir bölge çiftindeki her iki bölgeleri kullanmanızı öneririz. Bu, en iyi uygulamalar ve olağanüstü bir simge durumuna küçültülmüş kurtarma zamanı kullanılabilirliği garanti eder. 

## <a name="an-example-of-paired-regions"></a>Eşleştirilmiş bölgeler örneği
Şekil 2'in altında olağanüstü durum kurtarma için bölgesel çift kullanan kuramsal bir uygulamanın gösterir. Yeşil sayıları (Azure işlem, depolama ve veritabanı) üç Azure Hizmetleri ve bölgeler arasında çoğaltmak için nasıl yapılandırılacağı bölgeler arası etkinliklerini vurgulayın. Eşleştirilmiş bölgeler arasında dağıtma benzersiz avantajları turuncu sayılarla vurgulanır.

![Eşleştirilmiş bölge avantajları genel bakış](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Şekil 2 – kuramsal Azure bölgesel çift

## <a name="cross-region-activities"></a>Bölgeler arası etkinlikleri
Şekil 2 ' başvurulan gibi.

![Iaas](./media/best-practices-availability-paired-regions/1Green.png) **Azure (Iaas) işlem** – kaynakları kullanılabilir başka bir bölgede bir olağanüstü durum sırasında önceden sağlamak için ek işlem kaynakları hazırlamanız gerekir. Daha fazla bilgi için [Azure dayanıklılık teknik Kılavuzu](resiliency/resiliency-technical-guidance.md).

![Depolama](./media/best-practices-availability-paired-regions/2Green.png) **Azure depolama** -coğrafi olarak yedekli depolama (GRS), Azure depolama hesabınız oluşturulduğunda varsayılan olarak yapılandırılır. GRS ile verileriniz otomatik olarak üç kez birincil bölge içinde ve üç kez eşleştirilmiş bölge içinde çoğaltılır. Daha fazla bilgi için [Azure depolama Yedekliliği seçenekleri](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL veritabanı** – ile Azure SQL veritabanı coğrafi çoğaltma, dünyanın herhangi bir bölgesine işlemlerin zaman uyumsuz çoğaltma yapılandırabilirsiniz; ancak, bu kaynakları dağıtma öneririz bir Çoğu olağanüstü durum kurtarma senaryoları için eşleştirilmiş bölge. Daha fazla bilgi için [coğrafi çoğaltma, Azure SQL veritabanı'nda](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Resource Manager bölgeler arasında kendiliğinden bileşenlerinin mantıksal yalıtım sağlar. Başka bir deyişle, bir bölgede mantıksal hataları başka bir etkisi daha düşüktür.

## <a name="benefits-of-paired-regions"></a>Eşleştirilmiş bölgeler avantajları
Şekil 2 ' başvurulan gibi.  

![Yalıtım](./media/best-practices-availability-paired-regions/5Orange.png)
**fiziksel yalıtım** – bu tüm coğrafyalardaki pratik veya mümkün olmasa da mümkün olan Azure en az 300 mil ayırma bir bölge çiftindeki veri merkezleri arasında tercih ettiği zaman. Fiziksel veri merkezi ayrımı, doğal felaketler, toplumsal karmaşa, güç kesintileri veya her iki bölgeleri aynı anda etkileyen bir fiziksel ağ kesintisi olasılığını azaltır. Yalıtım coğrafyadaki (Coğrafya boyutu, güç/ağ altyapı kullanılabilirliğini, düzenlemelere, vb.) kısıtlamalara tabidir.  

![Çoğaltma](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform tarafından sağlanan çoğaltma** -otomatik çoğaltma eşleştirilmiş bölgeye coğrafi olarak yedekli depolama alanı gibi bazı hizmetler sağlar.

![Kurtarma](./media/best-practices-availability-paired-regions/7Orange.png)
**bölge kurtarma sipariş** – olay geniş kapsamlı bir kesinti her çiftte bir bölgenin kurtarılmasına öncelik verilir. Eşleştirilmiş bölgeler arasında dağıtılan uygulamalar öncelikte kurtarılan bölgelerden birine sahip olacağı garanti edilir. Bir uygulama değil eşleştirilmiş bölgeler arasında dağıtılması durumunda kurtarma – seçilen bölgeleri kurtarılması için son iki olabilecek en kötü durumda gecikebilir.

![Güncelleştirmeleri](./media/best-practices-availability-paired-regions/8Orange.png)
**sıralı güncelleştirme** – planlı Azure sistem güncelleştirmeleri sağlık bölge çiftlerine sırayla (değil aynı zamanda) kapalı kalma süresi, hataları ve nadir bozuk mantıksal hataları etkisini en aza indirmek için güncelleştirin.

![Veri](./media/best-practices-availability-paired-regions/9Orange.png)
**veri yerleşikliği** – vergi ve yasa uygulama dairesi amacıyla veri yerleşikliğinin karşılanması için (hariç, Brezilya Güney) çiftiyle aynı coğrafyada bulunan bir bölgede bulunuyor.
