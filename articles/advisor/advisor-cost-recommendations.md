---
title: "Azure maliyeti Advisor önerileri | Microsoft Docs"
description: "Azure dağıtımlarınızı maliyetini en iyi duruma getirmek için Azure Danışmanı'nı kullanın."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 51320d93689da3e37c0946d8877b27a11793d9c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-cost-recommendations"></a>Advisor maliyet önerileri

En iyi duruma getirme ve genel Azure azaltmak Danışmanı yardımcı olur, boşta ve az kaynaklarını tanımlayarak ayırın. Önerileri maliyet **maliyet** Danışmanı Pano sekmesinde.

![Advisor maliyet sekmesi](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>En iyi duruma getirme sanal makine harcamanız az örneklerini yeniden boyutlandırma 
Belirli uygulama senaryoları düşük kullanımı tasarım gereği yol açabilecek olsa da, sanal makine sayısını ve boyutunu yöneterek para genellikle kaydedebilirsiniz. Advisor 14 gün boyunca, sanal makine kullanımını izler ve düşük kullanımı sanal makineleri tanımlar. Sanal makineler, CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün düşük kullanımı sanal makineler olarak kabul edilir.

Advisor kapatmak veya yeniden boyutlandırmak seçebilmeleri sanal makineniz çalıştırmaya devam tahmini maliyeti gösterir.  

![Sanal makineler yeniden boyutlandırma için öneriler maliyet Danışmanı](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Birden çok SQL veritabanlarının performans hedeflerini yönetmek için uygun maliyetli bir çözüm kullanın
Advisor esnek veritabanı havuzu oluşturma yararlanabilir SQL server örnekleri tanımlar. Esnek veritabanı havuzları değişen kullanım desenlerini sahip birden çok veritabanı performans amaçlarını yönetmek için basit ve ekonomik bir çözüm sağlar. Azure esnek havuzları hakkında daha fazla bilgi için bkz: [bir Azure esnek havuz nedir?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Esnek veritabanı havuzlarına ilişkin öneriler maliyet Danışmanı](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Maliyet önerileri Azure Danışmanı erişme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede **daha fazla hizmet**.

3. Hizmet menü bölmesinde altında **izleme ve Yönetim**, tıklatın **Azure Danışmanı**.  
 Advisor Panosu görüntülenir.

4. Advisor Panoda tıklatın **maliyet** sekmesi.

5. Önerileri almak ve ardından istediğiniz aboneliği seçin **alma önerileri**.

> [!NOTE]
> Advisor önerileri erişmek için öncelikle *aboneliğinizi kaydetmek* Danışmanı ile. Bir abonelik kayıtlı olduğunda bir *abonelik sahibi* Danışmanı Pano başlatır ve tıkladığında **alma önerileri** düğmesi. Bu bir *tek seferlik işlem*. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik, bir kaynak grubu ya da belirli bir kaynak için.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:
* [Advisor giriş](advisor-overview.md)
* [Kullanmaya Başlama](advisor-get-started.md)
* [Advisor performans önerileri](advisor-cost-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-cost-recommendations.md)
* [Advisor güvenlik önerileri](advisor-cost-recommendations.md)
