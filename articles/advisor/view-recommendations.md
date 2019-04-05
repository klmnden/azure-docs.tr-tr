---
title: Sizin için önemli olan Azure Danışmanı önerileri görüntüle
description: Görebilir ve gürültüsünü azaltmak için Azure Danışmanı önerileri filtre uygulayabilirsiniz.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: 9f599a63fd5f52420f1b79e769d4f7bca9683b32
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59052845"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Sizin için önemli olan Azure Danışmanı önerileri görüntüle

Azure Danışmanı, Azure dağıtımlarınızın iyileştirilmesine yardımcı olacak öneriler sağlanmıştır. Advisor'ın içinde yalnızca sizin için önemli olan için önerilerin daraltmak için yardımcı olan birkaç özellik erişebilirsiniz.

## <a name="configure-subscriptions-and-resource-groups"></a>Abonelikler ve kaynak gruplarını yapılandırma

Advisor abonelikler ve kaynak grupları, siz ve kuruluşunuz için önemli işaretleyin olanağı sağlar. Yalnızca seçtiğiniz kaynak grupları ve abonelikler için önerileri görürsünüz. Varsayılan olarak, tüm seçilir. Bu abonelik veya kaynak grubu erişimi olan herkesin aynı ayarları uygulamak için abonelik veya kaynak grubu için yapılandırma ayarlarını uygulayın. Yapılandırma ayarları, Azure portalından veya programlama yoluyla değiştirilebilir.

Azure portalında değişiklik yapmak için:

1. Açık [Azure Danışmanı](https://aka.ms/azureadvisordashboard) Azure portalında.

1. Seçin **yapılandırma** menüsünde.

   ![Advisor yapılandırma menüsü](./media/view-recommendations/configuration.png)

1. Kutuyu işaretleyin **INCLUDE** sütunu için herhangi bir abonelik veya kaynak grupları, Advisor önerileri almak için. Kutusu devre dışıysa, bir yapılandırma Bu abonelik veya kaynak grubunda değişikliği yapma iznine sahip. Daha fazla bilgi edinin [izinleri Azure Danışmanı'nda](permissions.md).

1. Tıklayın **Uygula** altındaki bir değişiklik yaptıktan sonra.

## <a name="filtering-your-view-in-the-azure-portal"></a>Azure portalında Görünümü Filtresi

Yapılandırma ayarları değişti kadar etkin kalır. Tek bir görüntülemeye yönelik önerilerin sınırlandırmak istiyorsanız, sağlanan Danışman panelinin en üstünde açılan listeler kullanabilirsiniz. Genel bakış, yüksek kullanılabilirlik, güvenlik, performans, maliyet ve tüm önerilerin panellerinden abonelik, kaynak türleri ve görmek istediğiniz öneri durumu seçebilirsiniz.

   ![Advisor filtreleme menüsü](./media/view-recommendations/filtering.png)

## <a name="dismissing-and-postponing-recommendations"></a>Kapatma ve öneri erteleniyor

Azure Danışmanı'nı kapatın veya erteleme tek bir kaynak hakkında öneriler sağlar. Bir öneri kapatmak, el ile etkinleştirmeniz sürece tekrar görmek değil. Ancak, bir öneri erteleniyor bir süre sonra öneri otomatik olarak yeniden etkinleştirildiğinde belirtmenize olanak tanır. Erteleme Azure portalından veya programlama yoluyla gerçekleştirilebilir.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Azure portalında tek bir öneriyi ertele 

1. Açık [Azure Danışmanı](https://aka.ms/azureadvisordashboard) Azure portalında.
1. Önerileri görmek için bir öneri kategorisi seçin
1. Öneri listesinden bir öneri seçin
1. Erteleyebilir veya kapatmak istediğiniz öneri için Ertele'yi veya Kapat'ı seçin

     ![Advisor filtreleme menüsü](./media/view-recommendations/postpone-dismiss.png)

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Erteleyebilir veya Azure portalında bir birden çok önerileri Kapat

1. Açık [Azure Danışmanı](https://aka.ms/azureadvisordashboard) Azure portalında.
1. Önerileri görmek için bir öneri kategorisi seçin.
1. Bir öneri önerileri listesinden seçin.
1. Sol tarafındaki satır erteleyebilir veya öneri kapatmak istediğiniz tüm kaynaklar için onay kutusunu seçin.
1. Seçin **Ertele'yi** veya **atla** en sol üst köşesindeki tablosu.

     ![Advisor filtreleme menüsü](./media/view-recommendations/postpone-dismiss-multiple.png)

> [!NOTE]
> Kapat veya bir öneriyi ertele katkıda bulunan veya sahip iznine ihtiyacınız var. Azure Danışmanı izinler hakkında daha fazla bilgi edinin.

> [!NOTE]
> Seçim kutularının devre dışıysa, öneriler hala yükleniyor. Erteleyebilir veya kapatmak denemeden önce yüklemek tüm öneriler için lütfen bekleyin.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Ertelenen veya kapatılmış bir öneri yeniden etkinleştirme

Ertelenen veya kapatıldı bir öneri etkinleştirebilirsiniz. Bu eylem, Azure portalından veya programlama yoluyla gerçekleştirilebilir. Azure portalında:

1. Açık [Azure Danışmanı](https://aka.ms/azureadvisordashboard) Azure portalında.

1. Genel Bakış panelde filtreyi değiştirin **ertelendi**. Danışman, ardından Ertelenen veya kapatılmış önerileri görüntüler.

    ![Advisor filtreleme menüsü](./media/view-recommendations/activate-postponed.png)

1. Görmek için bir kategori seçin **ertelendi** ve **çıkarıldı** öneriler.

1. Bir öneri önerileri listesinden seçin. Bu önerilerde açar **Ertelenen & kapatıldı** sekmesi kendisi için bu öneriyi ertelendi veya bırakıldığı kapatıldı kaynakları göstermek için zaten seçili.

1. Tıklayarak **etkinleştirme** satırın sonunda. Tıklattıktan sonra bu kaynak için etkin ve bu nedenle bu tablodan kaldırılan kullanılması önerilir. Öneri görünür **etkin** sekmesi.
 
     ![Advisor filtreleme menüsü](./media/view-recommendations/activate-postponed-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Danışmanı'nda sizin için önemli olan önerileri nasıl görüntüleyebileceğiniz açıklanır. Advisor hakkında daha fazla bilgi için bkz: 

- [Azure Danışmanı nedir?](advisor-overview.md)
- [Advisor'ı kullanmaya başlama](advisor-get-started.md)
- [Azure Danışmanı izinleri](permissions.md)



