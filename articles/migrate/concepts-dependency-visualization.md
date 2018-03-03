---
title: "Azure geçirmek, bağımlılık Görselleştirme | Microsoft Docs"
description: "Değerlendirme hesaplamalar Azure geçirmek hizmetindeki genel bir bakış sağlar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 2/21/2018
ms.author: raynew
ms.openlocfilehash: bcbb2ace6686e4052149a5dde1ed837a16c36bad
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
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

Azure Geçişi ek ücret ödenmeden kullanılabilir. Bağımlılık görselleştirme özellikleri Azure geçirmek kullanımı hizmet eşlemesi gerektirir. Bir Azure geçirmek proje oluşturma sırasında Azure geçirmek otomatik olarak yeni bir günlük analizi çalışma alanı sizin adınıza oluşturur.

> [!NOTE]
> Bağımlılık görselleştirme özelliği hizmet Haritası günlük analizi çalışma alanı kullanır. 28 Şubat 2018 itibaren Azure geçirmek genel kullanılabilirlik duyuru ile özelliği ekstra ücret ödemeden kullanıma sunulmuştur. Yapmak üzere yeni bir proje oluşturmak gerekir ücretsiz kullanım çalışma alanını kullanın. Genel availaibility önce varolan çalışma hala chargable, bu nedenle yeni projeye taşımanızı öneririz.

1. Bu günlük analizi çalışma alanı içindeki hizmet Haritası dışındaki tüm çözümleri kullanımını standart günlük analizi ücret uygulanabilir. 
2. Hiçbir ek ücret ödemeden geçiş senaryoları desteklemek için hizmet Haritası çözüm herhangi bir ücret ilk 180 gün sonra standart ücretleri geçerli olacaktır Azure geçirmek projenin oluşturma uygulanmaz.
3. Proje oluşturmanın bir parçası oluşturulan yalnızca bir çalışma alanı olacaktır kullanımı ücretsizdir.

Çalışma alanına aracıları kaydederken kimliği ve Yükleme Aracısı adımları sayfasında Proje tarafından verilen anahtarı kullanın. Varolan bir çalışma alanını kullanın ve Azure geçirmek proje ile ilişkilendirin.

Çalışma alanı, Azure geçirmek proje silindiğinde, onunla birlikte silinmez. Proje silme göndermek, hizmet Haritası kullanım boş değil ve her düğüm günlük analizi çalışma alanı Ücretli katmanı göre ücretlendirilir.

Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz. 

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanı nasıl yönetebilirim?

Azure geçirme dışında günlük analizi çalışma alanını kullanabilirsiniz. İçinde oluşturulduğu geçiş proje silerseniz silinmez. Çalışma alanı artık ihtiyacınız varsa [silmeden](../log-analytics/log-analytics-manage-access.md) el ile.

Geçiş proje silmediğiniz sürece Azure geçirmek tarafından oluşturulan bir çalışma alanı silmeyin. Bunu yaparsanız, bağımlılıkları beklendiği gibi çalışmıyor.

## <a name="next-steps"></a>Sonraki adımlar

[Grup makineler makine bağımlılıklarını kullanma](how-to-create-group-machine-dependencies.md)