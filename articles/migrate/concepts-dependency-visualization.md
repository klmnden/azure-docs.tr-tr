---
title: Azure Geçişi'ndeki bağımlılık görselleştirmesi | Microsoft Docs
description: Azure geçişi hizmetinde değerlendirme hesaplamaları genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: raynew
ms.openlocfilehash: 5880c98b0f77b75dfb10969311408307b0c4afbd
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603371"
---
# <a name="dependency-visualization"></a>Bağımlılık görselleştirme

[Azure geçişi](migrate-overview.md) Hizmetleri grupları şirket içi makinelerin Azure'a geçiş için değerlendirir. Makineleri birlikte gruplamak için bağımlılık görselleştirmeyi kullanabilirsiniz. Bu makalede, bu özellik hakkında bilgi sağlar.


## <a name="overview"></a>Genel Bakış

Azure Geçişi'ndeki bağımlılık görselleştirmesi, daha yüksek güvenle geçiş değerlendirmek için gruplar oluşturmanıza olanak sağlar. Bağımlılık görselleştirmesi kullanarak belirli bir makine ya da bir makine grubu arasında ağ bağımlılıklarını görüntüleyebilirsiniz. Bu işlevselliği sağlamak yararlı veya kayıp (veya unutulursa makineler) uygulamaları ve iş yüklerini birden çok makine üzerinde çalıştırdığınızda geçiş sürecinin.  

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure geçişi kullanan [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) çözümde [Log Analytics](../log-analytics/log-analytics-overview.md) bağımlılık görselleştirmesi için.
- Bir Azure geçişi projesi oluşturduğunuzda, aboneliğinizde bir Log Analytics çalışma alanı oluşturulur.
- Çalışma alanı adı ön ekine sahip geçiş projesi için belirttiğiniz ad olduğu **geçirme-** ve isteğe bağlı olarak, bir sayı ile sonekli.
- Log Analytics çalışma alanına gidin **Essentials** projenin bölümünü **genel bakış** sayfası.
- Oluşturulan çalışma alanı anahtarı ile etiketlenmiş **geçiş projesi**ve değer **proje adı**. Bunları Azure portalında aramak için kullanabilirsiniz.  

    ![Log Analytics çalışma alanı](./media/concepts-dependency-visualization/oms-workspace.png)

Bağımlılık görselleştirmesi kullanmak için indirip analiz etmek istediğiniz her bir şirket içi makinede aracıları yüklemeniz gerekir.  

## <a name="do-i-need-to-pay-for-it"></a>Bunun için ödeme yapmam gerekiyor?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Azure Geçişi'ndeki bağımlılık görselleştirmesi özelliklerinin kullanımı, hizmet eşlemesi gerektirir. Bir Azure geçişi projesi oluşturma sırasında Azure geçişi otomatik olarak yeni bir Log Analytics çalışma alanı sizin adınıza oluşturur.

> [!NOTE]
> Bağımlılık görselleştirmesi özelliğini bir Log Analytics çalışma alanı hizmet eşlemesini kullanır. 28 Şubat 2018'den itibaren Azure geçişi, genel kullanılabilirlik duyurusuyla birlikte bu özellik artık başka bir ücret ödemeden kullanılabilir. Yapmak üzere yeni bir proje oluşturmak ihtiyacınız olacak ücretsiz kullanım çalışma alanını kullanın. Genel availaibility önce mevcut çalışma alanları hala chargable, bu nedenle yeni bir projeye taşımak için önerilir.

1. Bu Log Analytics çalışma alanında hizmet eşlemesi dışında herhangi bir çözüm kullanımını standart Log Analytics ücret uygulanabilir.
2. Hiçbir ek ücret ödemeden geçiş senaryolarını desteklemek üzere hizmet eşlemesi çözümünü herhangi oluşturmadan sonra standart ücretler uygulanır Azure geçişi projesinin ilk 180 gün için ücret uygulanmaz.
3. Yalnızca proje oluşturmanın bir parçası oluşturulan çalışma alanını olacaktır kullanımı ücretsizdir.

Çalışma alanına aracıları kaydettiğinizde, kimlik ve proje tarafından verilen Yükleme Aracısı adımları sayfasında anahtarın kullanın. Mevcut bir çalışma alanını kullanın ve bunu Azure geçişi projesi ile ilişkilendiren olamaz.

Çalışma alanı, Azure geçişi projesi silindiğinde, onunla birlikte silinmez. Proje silme gönderin, hizmet eşlemesi kullanım boş olur ve her düğüm, Log Analytics çalışma alanını Ücretli katmanına göre ücretlendirilir.

Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanı nasıl yönetebilirim?

Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz. Geçiş projesi içinde oluşturulduğu silerseniz silinmez. Çalışma alanı artık ihtiyacınız kalmadığında [silmeden](../log-analytics/log-analytics-manage-access.md) el ile.

Geçiş projesi silmediğiniz sürece, Azure geçişi tarafından oluşturulan çalışma alanını silme. Bunu yaparsanız, bağımlılıkları, beklendiği gibi çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

[Makine bağımlılıkları kullanan Grup makineleri](how-to-create-group-machine-dependencies.md)
