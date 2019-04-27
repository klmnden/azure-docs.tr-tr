---
title: İstek özellikleri Azure Time Series Insights müşteri verilerini | Microsoft Docs
description: Azure Time Series Insights müşteri verilerini talep özelliklerin özeti.
author: ashannon7
ms.author: anshan
manager: cshankar
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.custom: seodec18
ms.openlocfilehash: 30f6b1fd953f89170a18d56bf0353c643853074e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60880717"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti

Azure zaman serisi görüşleri depolama, analiz ve görselleştirme bileşenleriyle almak, depolamak, keşfedin ve milyarlarca olayı aynı anda keşfedip analiz daha kolay hale yönetilen bir bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Görüntüleme, dışarı aktarma ve bir veri sahibi isteği tabi olabilir kişisel verilerini silme için Azure Time Series Insights Kiracı yönetici, Azure portalı veya REST API'lerini kullanabilirsiniz. Hizmet gdpr veri sahibi istekleri için Azure portalını kullanarak, çoğu kullanıcının tercih bu işlemleri gerçekleştirmek için daha az karmaşık bir yöntem sağlar.

## <a name="identifying-customer-data"></a>Müşteri verileri tanımlama

Azure Time Series Insights, Yöneticiler ve kullanıcılar, zaman serisi görüşleri ile ilişkili veri olarak kişisel verileri dikkate alır. Zaman serisi görüşleri ortamına erişimi olan kullanıcılar Azure Active Directory nesne Kimliğini depolar. Kullanıcı e-posta adresleri, Azure portalında görüntüler, ancak bu e-posta adresleri Time Series Insights içinde depolanmaz, bunlar Azure Active Directory'de Azure Active Directory nesne kimliği kullanılarak dinamik olarak arandığı.

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kiracı Yöneticisi, Azure portalını kullanarak müşteri verileri silebilirsiniz.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Portal aracılığıyla müşteri verileri silmeden önce Azure portalındaki zaman serisi görüşleri ortamından kullanıcının erişim ilkelerini kaldırmalısınız. Daha fazla bilgi için [Azure portalını kullanarak zaman serisi görüşleri ortamına veri erişimi verme](time-series-insights-data-access.md).

Ayrıca, REST API kullanarak erişim ilkeleri üzerinde silme işlemleri gerçekleştirebilirsiniz. Daha fazla bilgi için [erişim ilkelerini - Sil](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/delete).

Time Series Insights, ilke dikey penceresinde Azure portalı ile tümleşiktir. Time Series Insights hem de ilke dikey penceresinde görüntülemek için dışarı aktarma ve hizmet içinde depolanan kullanıcı verilerini silmek sağlar. İlke dikey penceresinde Time Series Insights içinde kullanıcı verileri silme işlemi, Azure portal sonuçlarının içinde gerçekleştirilen eylem silin. Örneğin, kullanıcının kişisel kaydedilmiş sorgu varsa bu sorguyu Time Series Insights Gezgini'nden kalıcı olarak silinir. Kullanıcının paylaşılan kaydedilmiş bir sorgu varsa, sorgu devam ediyorsa, ancak kullanıcı bilgilerini kalıcı olarak silinir. Aşağıdaki Not, şu görevleri gerçekleştirmek yönergeler içerir.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma

Benzer şekilde veri silmek için bir kiracı Yöneticisi görüntüleyebilir ve Azure portalında ilke dikey penceresinden Time Series Insights içinde depolanan verileri dışarı aktarma.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Kiracı yöneticisiyseniz, içinde zaman serisi görüşleri ortamına veri erişimi ilkeleri Azure Portalı'nda görüntüleyebilirsiniz. Daha fazla bilgi için [Azure portalını kullanarak zaman serisi görüşleri ortamına veri erişimi verme](time-series-insights-data-access.md).

Erişim ilkeleri "listesi ortama göre" işlemi belirtilen REST API kullanarak dışarı aktarma işlemleri mümkündür. Daha fazla bilgi için [erişim ilkeleri - listesi tarafından ortamı](https://docs.microsoft.com/rest/api/time-series-insights-management/accesspolicies/listbyenvironment).

## <a name="to-delete-data-stored-within-time-series-insights"></a>Time Series Insights içinde depolanan verileri silmek için

Kişisel verileri, zaman serisi görüşleri depolama, kullanıcı ve yönetici verileri farklı bir senaryodan içine yolu sağlayabilirsiniz. Time Series Insights kişisel verileri olarak depolanan veriler göz önünde bulundurun, dışarı aktarma ve aşağıdaki adımları kullanarak bu verileri sil:

**Verileri dışarı aktarma ve görüntüleme**

Time Series Insights içinde depolanan verileri dışarı aktarma ve görüntülemek için bu veriler için arama yapmanız gerekmiyor. Time Series Insights Gezgini veya zaman serisi görüşleri sorgu API'leri, görüntülemek ve veri dışarı aktarmak için kullanabilirsiniz. Time Series Insights Gezginini kullanarak verileri dışarı aktarmak ve görüntülemek için söz konusu kullanıcı verileri bulmak için ilk arayın. Arama yaptıktan sonra sağ tıklatın ve grafik **olayları keşfet**. Olay ızgarası, görünür ve CSV ve JSON verileri dışarı aktarma seçenekleri sunar.

Daha fazla bilgi için [Azure Time Series Insights gezgininin](time-series-insights-explorer.md).

**Verileri Sil**

Şu anda, zaman serisi görüşleri veri ayrıntılı olarak silinmesini desteklemez. Ancak, zaman serisi görüşleri bekletme ilkeleri yapılandırarak Time Series Insights içinde depolanan müşteri verileri kaldırma özelliği sağlar. Herhangi bir sayıda gün silme gereksinimlerinizi desteklemek için tüm Time Series Insights ortamınızı saklama süresini ayarlayabilirsiniz.

Daha fazla bilgi için [bekletme zaman serisi Öngörülerinde yapılandırma](time-series-insights-how-to-configure-retention.md).