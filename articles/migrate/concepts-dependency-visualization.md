---
title: "Azure geçirmek, bağımlılık Görselleştirme | Microsoft Docs"
description: "Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 10bc249aa70852dce71c96e46319925cececdd8b
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="dependency-visualization"></a>Bağımlılık görselleştirme

[Azure geçirmek](migrate-overview.md) Hizmetleri geçiş Azure için şirket içi makine grupları değerlendirir. Makineleri birlikte gruplamak için bağımlılık görselleştirme kullanabilirsiniz. Bu makalede, bu özellik hakkında bilgi sağlar.


## <a name="overview"></a>Genel Bakış

Azure geçirmek bağımlılık görsel öğeyi geçiş değerlendirmek için artan güvenle gruplar oluşturmanıza olanak sağlar. Bağımlılık görselleştirme kullanarak belirli makinelerin veya makine grubu arasında ağ bağımlılıkları görüntüleyebilirsiniz. Bu işlevselliği emin olmak kullanışlı veya kayıp (veya unutulursa makineler) uygulamaları ve iş yüklerini birden fazla makine arasında çalıştırdığınızda geçiş işleminin.  

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure geçirme kullanır [hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md) çözümde [günlük analizi](../log-analytics/log-analytics-overview.md) bağımlılık görselleştirme için.
- Bir Azure geçiş projesi oluşturduğunuzda, aboneliğinizde bir OMS günlük analizi çalışma alanı oluşturulur.
- Çalışma alanı adı önekine sahip geçiş proje için belirttiğiniz addır **geçirmek-**ve isteğe bağlı olarak sonekine olan bir sayı. 
- Günlük analizi çalışma alanından gidin **Essentials** proje bölümünü **genel bakış** sayfası.
- Oluşturulan çalışma anahtarla etiketli **MigrateProject**ve değeri **proje adı**. Azure portalında aramak için bunları kullanabilirsiniz.  

    ![Log Analytics çalışma alanı](./media/concepts-dependency-visualization/oms-workspace.png)

Bağımlılık görselleştirme kullanmak için aracıları analiz etmek istediğiniz her şirket içi makinede yükleyip gerekir.  

## <a name="do-i-need-to-pay-for-it"></a>Bunun için ödeme gerekiyor mu?

Evet. Günlük analizi çalışma alanı varsayılan olarak oluşturulur, ancak Azure geçirmek bağımlılık görselleştirme kullanmadığınız sürece kullanılmaz. Bağımlılık görselleştirme kullanın (veya çalışma alanı Azure geçirme dışında kullanıyorsanız), çalışma alanı kullanımı için sizden ücret kesilir.  [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/insight-analytics/) hizmet Haritası çözüm fiyatlandırma hakkında. 

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanı nasıl yönetebilirim?

Azure geçirme dışında günlük analizi çalışma alanını kullanabilirsiniz. İçinde oluşturulduğu geçiş proje silerseniz silinmez. Çalışma alanı artık ihtiyacınız varsa [silmeden](../log-analytics/log-analytics-manage-access.md) el ile.

Geçiş proje silmediğiniz sürece Azure geçirmek tarafından oluşturulan bir çalışma alanı silmeyin. Bunu yaparsanız, bağımlılıkları beklendiği gibi çalışmıyor.

## <a name="next-steps"></a>Sonraki adımlar

[Grup makineler makine bağımlılıklarını kullanma](how-to-create-group-machine-dependencies.md)