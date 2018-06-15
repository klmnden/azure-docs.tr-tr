---
title: Makineleri geçir değerlendirme sonra Azure geçirme ile | Microsoft Docs
description: Geçiş için öneriler almayı açıklar Azure geçiş hizmeti ile bir değerlendirme çalıştırdıktan sonra makineleri.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: 242a8b95e7eb278a7884eec7d0cc6a607bdf24d4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204068"
---
# <a name="migrate-machines-after-assessment"></a>Değerlendirmenin ardından makineleri geçirme


[Azure geçirme](migrate-overview.md) bunlar Azure geçiş için uygun ve makine Azure'da çalışan için boyutlandırma ve maliyet tahminler sunar olup olmadığını denetlemek için şirket içi makineler değerlendirir. Şu anda Azure geçirmek yalnızca makineleri geçiş için değerlendirir. Geçiş, diğer Azure hizmetlerini kullanarak gerçekleştirilir.

Bu makalede, bir geçiş değerlendirmesi çalıştırdıktan sonra Geçiş Aracı ilgili öneriler alın açıklar.

## <a name="migration-tool-suggestion"></a>Geçiş Aracı önerisi

Geçiş Araçları ile ilgili öneriler almak için şirket içi ortamdaki bir derin bulma yapmanız gerekir. Derin bulma şirket içi makinelerde aracıları yükleyerek gerçekleştirilir.  

1. Bir Azure geçirmek projesi oluşturun, şirket içi makineleri Bul ve geçiş değerlendirme oluşturun. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. Karşıdan yükle ve Azure geçirmek aracıları önerilen geçiş yöntemi görmek istediğiniz her şirket içi makineye yükleyin. [Bu yordamı izlemeden](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) aracıları yüklemek için.
2. Yükseltme ve shift geçiş için uygun olan şirket içi makinelerinizi tanımlayın. Bunlar üzerinde çalışan uygulamalar herhangi bir değişiklik gerektirmez ve olarak geçirilebilir VM'ler bunlar.
3. Yükseltme ve shift geçiş için Azure Site RECOVERY'yi kullanarak öneririz. [Daha fazla bilgi edinin](../site-recovery/tutorial-migrate-on-premises-to-azure.md). Alternatif olarak, Azure geçişi destekleyen üçüncü taraf araçları kullanabilirsiniz.
4. Diğer bir deyişle, yükseltme ve shift geçiş için uygun olmayan şirket içi makineler varsa, tüm bir VM'yi yerine belirli bir uygulama geçirmek istiyorsanız diğer geçiş araçları kullanabilirsiniz. Örneğin, önerdiğimiz [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/campaigns/database-migration/) geçirmek istiyorsanız, şirket içi veritabanlarını böyle bir SQL Server, MySQL veya Oracle Azure.


## <a name="review-suggested-migration-methods"></a>Önerilen geçiş yöntemleri gözden geçirin

1. Önerilen geçiş yöntemi sağlayabilmek için önce bir Azure geçirmek projesi oluşturun, şirket içi makineler bulmak ve geçiş değerlendirme çalıştırmak gerekir. [Daha fazla bilgi edinin](tutorial-assessment-vmware.md).
2. Değerlendirme oluşturulduktan sonra projede görüntülemek > **genel bakış** > **Pano**. Tıklatın **değerlendirme Hazırlık**

    ![Hazır olma durumu değerlendirmesi](./media/tutorial-assessment-vmware/assessment-report.png)  

3. İçinde **önerilen aracı**, geçiş için kullanabileceğiniz araçlar önerileri gözden geçirin.

    ![Önerilen araç](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Sonraki adımlar

Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
