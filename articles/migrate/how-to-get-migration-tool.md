---
title: "Makineleri geçir değerlendirme sonra Azure geçirme ile | Microsoft Docs"
description: "Geçiş için öneriler almayı açıklar Azure geçiş hizmeti ile bir değerlendirme çalıştırdıktan sonra makineleri."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 809c6e85-0928-45e2-a7c7-6824d860e134
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 730111411a080a3134ad69fee8bfceead4d27fa4
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="migrate-machines-after-assessment"></a>Değerlendirme makineleri geçirin


[Azure geçirme](migrate-overview.md) bunlar Azure geçiş için uygun ve makine Azure'da çalışan için boyutlandırma ve maliyet tahminler sunar olup olmadığını denetlemek için şirket içi makineler değerlendirir. Şu anda Azure geçirmek yalnızca makineleri geçiş için değerlendirir. Geçiş, diğer Azure hizmetlerini kullanarak gerçekleştirilir.

Bu makalede, bir geçiş değerlendirmesi çalıştırdıktan sonra Geçiş Aracı ilgili öneriler alın açıklar.

## <a name="migration-tool-suggestion"></a>Geçiş Aracı önerisi

Geçiş Araçları ile ilgili önerileri almak için şirket içi makinelerde aracıları yüklemeniz gerekir.  

1. Bir Azure geçirmek projesi oluşturun, şirket içi makineleri Bul ve geçiş değerlendirme oluşturun. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. Karşıdan yükle ve Azure geçirmek aracıları önerilen geçiş yöntemi görmek istediğiniz her şirket içi makineye yükleyin. [Bu yordamı izlemeden](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) aracıları yüklemek için.
2. Yükseltme ve shift geçiş için uygun olan şirket içi makinelerinizi tanımlayın. Bunlar üzerinde çalışan uygulamalar herhangi bir değişiklik gerektirmez ve olarak geçirilebilir VM'ler bunlar.
3. Yükseltme ve shift geçiş için Azure Site RECOVERY'yi kullanarak öneririz. [Daha fazla bilgi edinin](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternatif olarak, Azure geçişi destekleyen 3. taraf araçları kullanabilirsiniz.
4. Örneğin, tüm VM yerine belirli bir uygulama geçirmek istiyorsanız yükseltme shift geçiş için uygun olmayan şirket içi makineler varsa, diğer geçiş araçlarını kullanabilir. Örneğin, önerdiğimiz [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/campaigns/database-migration/) geçirmek istiyorsanız, şirket içi veritabanlarını böyle bir SQL Server, MySQL veya Oracle Azure.


## <a name="review-suggested-migration-methods"></a>Önerilen geçiş yöntemleri gözden geçirin

1. Önerilen geçiş yöntemi sağlayabilmek için önce bir Azure geçirmek projesi oluşturun, şirket içi makineler bulmak ve geçiş değerlendirme çalıştırmak gerekir. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. Değerlendirme oluşturulduktan sonra projede görüntülemek > **genel bakış** > **Pano**. Tıklatın **değerlendirme Hazırlık**

    ![Değerlendirme Hazırlık](./media/tutorial-assessment-vmware/assessment-report.png)  

3. İçinde **önerilen aracı**, geçiş için kullanabileceğiniz araçlar önerileri gözden geçirin.

    ![Önerilen aracı](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](concepts-assessment-calculation.md) değerlendirmelerinin nasıl hesaplandığını hakkında.
