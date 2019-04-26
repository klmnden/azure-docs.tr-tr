---
title: Azure Geçişi'ndeki bağımlılık görselleştirmesi | Microsoft Docs
description: Azure geçişi hizmetinde değerlendirme hesaplamaları genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: 8df587db7655e2aafd876d80581f3296c8c99fbf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201576"
---
# <a name="dependency-visualization"></a>Bağımlılık görselleştirme

[Azure geçişi](migrate-overview.md) Hizmetleri grupları şirket içi makinelerin Azure'a geçiş için değerlendirir. Grupları oluşturmak için Azure geçişi bağımlılık görselleştirme işlevini kullanabilirsiniz. Bu makalede, bu özellik hakkında bilgi sağlar.

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

## <a name="overview"></a>Genel Bakış

Azure Geçişi'ndeki bağımlılık görselleştirmesi geçiş değerlendirmeler için yüksek güvenilirliğe sahip gruplar oluşturmanıza olanak sağlar. Ağ makinelerin bağımlılıklarını görüntülemek ve birlikte Azure'a geçirilmesi için gereken ilgili makinelerin tanımlanması bağımlılık görselleştirmesi kullanarak. Bu işlev, uygulamanızı oluşturan ve Azure'a birlikte geçirilmesi gereken makine tamamen uyumlu olmadığı senaryolarda yararlıdır.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure geçişi kullanan [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) çözümde [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md) bağımlılık görselleştirmesi için.
- Bağımlılık görselleştirmesi yararlanmak için yeni veya var olan, Log Analytics çalışma alanı yeniden eşlemeniz gerekir ile bir Azure geçişi projesi.
- Yalnızca oluşturma veya geçiş projesi oluşturulduğu aynı Abonelikteki bir çalışma alanı ekleyin.
- Bir Log Analytics çalışma alanı için bir proje eklemek için Git **Essentials** projenin bölümünü **genel bakış** sayfasında ve tıklayın **yapılandırma gerektirir**

    ![Log Analytics çalışma alanını ilişkilendir](./media/concepts-dependency-visualization/associate-workspace.png)

- Bir çalışma alanı ilişkilendirilirken yeni bir çalışma alanı oluşturun veya mevcut bir paylaşımın seçeneği alırsınız:
  - Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı için bir ad belirtmeniz gerekir. Çalışma alanı aynı bölgede oluşturulduktan sonra [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) geçiş projesi olarak.
  - Mevcut bir çalışma alanı eklediğinizde, geçiş projesi ile aynı abonelikte kullanılabilir olan tüm çalışma arasından seçim yapabilirsiniz. Yalnızca bu çalışma alanlarının bir bölgede oluşturulan listelendiğine dikkat edin burada [hizmet eşlemesi desteklenir](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Bir çalışma alanı eklemek için çalışma alanına 'Reader' erişiminiz olduğundan emin olun.

  > [!NOTE]
  > Bir çalışma alanı bir projeye ekledikten sonra daha sonra değiştiremezsiniz.

- İlişkili çalışma alanı anahtarı ile etiketlenmiş **geçiş projesi**ve değer **proje adı**, hangi Azure portalında aramak için kullanabilirsiniz.
- Projeyle ilişkili çalışma alanına gidin, gidebilirsiniz **Essentials** projenin bölümünü **genel bakış** sayfasında ve çalışma alanına erişim

    ![Log Analytics çalışma alanına gidin](./media/concepts-dependency-visualization/oms-workspace.png)

Bağımlılık görselleştirmesi kullanmak için indirip analiz etmek istediğiniz her bir şirket içi makinede aracıları yüklemeniz gerekir.  

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) her makinede yüklü olması gerekir.
- [Bağımlılık aracısını](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) her makinede yüklü olması gerekir.
- Ayrıca, internet bağlantısı olmayan makineleriniz varsa, indirmek ve Log Analytics gateway yükler gerekir.

Bu aracılar, bağımlılık görselleştirmesi kullanmıyorsanız değerlendirmek istediğiniz makinelerde gerekmez.

## <a name="do-i-need-to-pay-for-it"></a>Bunun için ödeme yapmam gerekiyor?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Azure Geçişi'ndeki bağımlılık görselleştirmesi özelliğini kullanımını hizmet eşlemesi gerektirir ve yeni veya var olan, Log Analytics çalışma alanı ilişkilendirmenizi gerektirir Azure geçişi projesi ile. Azure Geçişi'ndeki bağımlılık görselleştirme işlevini Azure Geçişi'ndeki ilk 180 gün boyunca ücretsizdir.

1. Bu Log Analytics çalışma alanında hizmet eşlemesi dışında herhangi bir çözüm kullanımını sık erişimliye doğurur [standart Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ücretleri.
2. Hiçbir ek ücret ödemeden geçiş senaryolarını desteklemek üzere hizmet eşlemesi çözümünü tüm Log Analytics çalışma alanını Azure geçişi projesi ile ilişkili bir günden itibaren ilk 180 gün için ücret uygulanmaz. 180 gün dolduktan sonra standart Log Analytics ücretleri geçerli olacaktır.

Çalışma alanına aracıları kaydettiğinizde, kimlik ve proje tarafından verilen Yükleme Aracısı adımları sayfasında anahtarın kullanın.

Çalışma alanı, Azure geçişi projesi silindiğinde, onunla birlikte silinmez. Proje silme gönderin, hizmet eşlemesi kullanım ücretsiz olmayacak ve her düğüm, Log Analytics çalışma alanını Ücretli katmanına göre ücretlendirilir.

> [!NOTE]
> Bağımlılık görselleştirmesi özelliğini bir Log Analytics çalışma alanı hizmet eşlemesini kullanır. 28 Şubat 2018'den itibaren Azure geçişi, genel kullanılabilirlik duyurusuyla birlikte bu özellik artık başka bir ücret ödemeden kullanılabilir. Yapmak üzere yeni bir proje oluşturmak ihtiyacınız olacak ücretsiz kullanım çalışma alanını kullanın. Genel kullanılabilirlik öncesi mevcut çalışma alanları hala ücrete, bu nedenle yeni bir projeye taşımak için önerilir.

Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanı nasıl yönetebilirim?

Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz. Geçiş projesi içinde oluşturulduğu silerseniz silinmez. Çalışma alanı artık ihtiyacınız kalmadığında [silmeden](../azure-monitor/platform/manage-access.md) el ile.

Geçiş projesi silmediğiniz sürece, Azure geçişi tarafından oluşturulan çalışma alanını silme. Bunu yaparsanız, bağımlılık görselleştirme işlevini beklendiği gibi çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar
- [Makine bağımlılıkları kullanan Grup makineleri](how-to-create-group-machine-dependencies.md)
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) bağımlılık görselleştirme hakkında sık sorulan sorular hakkında.
