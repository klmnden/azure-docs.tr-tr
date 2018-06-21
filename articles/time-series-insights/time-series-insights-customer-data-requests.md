---
title: Müşteri verileri Azure zaman serisi Öngörüler özelliklerinde isteği
description: Müşteri verileri isteği özelliklerinin özeti.
author: ashannon7
ms.author: dobett
manager: timlt
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.openlocfilehash: 5714782d4fe44ce4521dd604055e925937a7328d
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295287"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteği özelliklerinin özeti

Azure zaman serisi Öngörüler alma, depolama, keşfetmek ve olayları milyarlarca aynı anda analiz kolaylaştırmak depolama, analiz ve görselleştirme bileşenleri ile yönetilen bir bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Görüntülemek, verme ve veri konu isteği tabi olabilir kişisel verilerini silmek için Azure zaman serisi Öngörüler Kiracı yönetici Azure portalı veya REST API'ler kullanabilirsiniz. Veri konu isteklere hizmet Azure Portalı'nı kullanarak, kullanıcıların çoğunun tercih bu işlemleri gerçekleştirmek için en az karmaşık bir yöntem sağlar.

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Azure zaman serisi Öngörüler, kişisel verilerin Yöneticiler ve zaman serisi Öngörüler kullanıcılarının ilişkili veri dikkate alır. Zaman serisi Öngörüler ortamına erişimi olan kullanıcılar Azure Active Directory nesne kimliği depolar. Kullanıcı e-posta adresleri, Azure portal görüntüler, ancak bu e-posta adresleri zaman serisi Öngörüler içinde depolanmaz, bunlar Azure Active Directory'de Azure Active Directory nesne kimliği kullanılarak dinamik olarak arandığı.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kiracı Yöneticisi, Azure portalını kullanarak müşteri verilerini silebilirsiniz.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Ancak, portal üzerinden müşteri verileri silmeden önce Azure portalındaki zaman serisi Öngörüler ortamından kullanıcının erişim ilkeleri kaldırmanız gerekir. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir zaman serisi Öngörüler ortamı için veri erişim](time-series-insights-data-access.md).

REST API kullanarak erişim ilkeleri silme işlemleri de gerçekleştirebilirsiniz. Daha fazla bilgi için bkz: [erişim ilkelerini - silin](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/delete).

Zaman serisi Öngörüler Azure portalındaki ilke dikey tümleşiktir. Zaman serisi Öngörüler ve ilke dikey penceresinde, görüntülemek, verme ve hizmet içinde depolanan kullanıcı verilerini silmek etkinleştirin. Azure portal sonuçları zaman serisi Öngörüler içindeki kullanıcı verileri silme ilkesi dikey gerçekleştirilen eylem silin. Örneğin, bir kullanıcının kaydedilmiş kişisel sorgu varsa, bu sorgu zaman serisi Öngörüler Gezgini'nden kalıcı olarak silinir. Kullanıcının kaydedilmiş paylaşılan sorgu varsa, sorgu devam ederse, ancak kullanıcı bilgilerini kalıcı olarak silinir. Aşağıdaki nota bu görevleri gerçekleştirmek üzere yönergeler içerir.

## <a name="exporting-customer-data"></a>Müşteri verileri dışarı aktarma

Benzer şekilde verileri silmek için bir kiracı Yöneticisi görüntüleyebilir ve Azure portalında İlkesi dikey penceresinden zaman serisi öngörü depolanan verileri dışarı aktarma.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Bir kiracı Yöneticisi iseniz, Azure portalında veri erişimi ilkelerini zaman serisi Öngörüler ortamda görüntüleyebilirsiniz. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir zaman serisi Öngörüler ortamı için veri erişim](time-series-insights-data-access.md).

"Ortam listeyi" işlemi içinde sağlanan REST API kullanarak erişim ilkeleri verme işlemleri gerçekleştirmek mümkündür. Daha fazla bilgi için bkz: [erişim ilkeleri - listesi tarafından ortamı](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/listbyenvironment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>Zaman serisi Öngörüler içinde depolanan verileri silmek için

Kişisel veriler, zaman serisi Öngörüler depolamaya farklı bir senaryo kullanıcı ve yönetici veri yolu yapabilir. Zaman serisi Öngörüler kişisel veriler olarak depolanan verilere göz önünde bulundurun, verme ve aşağıdaki adımları kullanarak bu verileri sil:

**Görüntülemek ve verileri dışarı aktarma**

Görüntülemek ve zaman serisi Öngörüler içinde depolanan verileri dışarı aktarmak için bu verileri aramanız gerekir. Görüntülemek ve verileri dışarı aktarma için zaman serisi Öngörüler explorer veya zaman serisi Öngörüler sorgu API'leri kullanabilirsiniz. Görüntülemek ve zaman serisi Öngörüler explorer kullanarak verileri dışarı aktarmak için söz konusu kullanıcı verileri bulmak için ilk arayın. Arama yaptıktan sonra sağ tıklatın ve grafik **olayları keşfedin**. Olayları kılavuz görünür ve veri CSV ve JSON verme seçeneklerini sunar.

Daha fazla bilgi için bkz: [Azure zaman serisi Öngörüler explorer](time-series-insights-explorer.md).

**Verileri silme**

Şu anda, zaman serisinin Öngörüler veri ayrıntılı silme işlemini desteklemiyor. Ancak, zaman serisinin Öngörüler bekletme ilkeleri yapılandırarak zaman serisi Öngörüler içinde depolanan müşteri verileri kaldırma özelliği sağlar. Herhangi bir sayıda gün silme gereksinimlerinizi desteklemek için tüm zaman serisi Öngörüler ortama saklama süresi ayarlayabilirsiniz.

Daha fazla bilgi için bkz: [zaman serisi öngörü saklamayı yapılandırmaya](time-series-insights-how-to-configure-retention.md).