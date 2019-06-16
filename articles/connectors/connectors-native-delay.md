---
title: İş akışlarında - Azure Logic Apps sonraki eylem gecikmesi
description: Azure Logic Apps'te Gecikmeli veya Gecikmeli kadar eylemleri kullanarak mantıksal uygulama iş akışlarınızla sonraki eylem çalıştırılacak bekleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
tags: connectors
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 27475fb3f086dbc5166a473e9d657d2dab723938
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66297669"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Azure Logic Apps'te sonraki eylem çalıştırma gecikmesi

Mantıksal uygulamanızı sonraki eylem çalıştırmadan önce bir süre beklemek için yerleşik ekleyebilirsiniz **gecikme - zamanlama** mantıksal uygulamanızın iş akışında önce bir eylemi. Yerleşik ekleyebilirsiniz **Geciktir: zamanlama** eylemi belirli bir tarih ve saat sonraki eylem çalıştırmadan önce kadar bekleyin. Yerleşik zamanlama eylemleri ve tetikleyicileri hakkında daha fazla bilgi için bkz. [zamanlama ve otomatik olarak yinelenen bir çalıştırma, görevleri ve Azure Logic Apps ile iş akışlarını](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **gecikme**: Belirtilen sayıda saniye, dakika, saat, gün, hafta veya sonraki eylemi çalıştırılmadan önce bir ay gibi zamanı birimleri bekleyin.

* **Geciktir:** : Belirtilen tarih ve saat sonraki eylemi çalıştırılmadan önce kadar bekleyin.

Bu eylemler için bazı örnek yöntemler şunlardır:

* Durum güncelleştirmesi e-posta göndermek için bir hafta kadar bekleyin.

* Veri alma ve sürdürme önce HTTP çağrısı tamamlanana kadar iş akışınızı gecikme.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkında temel bilgilere [logic apps](../logic-apps/logic-apps-overview.md). Bir eylem kullanabilmeniz için mantıksal uygulamanızı ilk bir tetikleyici ile başlamalıdır. Gecikme eylemi eklemeden önce diğer eylemler ekleyin ve istediğiniz herhangi bir tetikleyici kullanabilirsiniz. Bu konuda, bir Office 365 Outlook tetikleyicisini kullanır. Logic apps kullanmaya yeni başladıysanız öğrenin [ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Gecikme eylemi ekleme

1. Mantıksal Uygulama Tasarımcısı gecikme eylemi, eklemek istediğiniz adımı altında seçin **yeni adım**.

   Adımları arasındaki gecikme eylemi eklemek için adımları bağlanan okun üzerine işaretçiyi taşıyın. Görünen artı işaretini (+) tıklayın ve ardından seçin **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "gecikme" girin. Eylem listesinden şu eylemi seçin: **gecikme**

   !["Gecikme" Eylem Ekle](./media/connectors-native-delay/add-delay-action.png)

1. Sonraki eylem çalışmadan önce beklenecek süreyi belirtin.

   ![Gecikme süresini ayarlama](./media/connectors-native-delay/delay-time-intervals.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Count | count | Evet | Integer | Gecikme zaman birimlerinin sayısı |
   | Birim | Birim | Evet | String | Örneğin, zaman birimi: `Second`, `Minute`, `Hour`, `Day`, `Week`, veya `Month` |
   ||||||

1. İş akışınızı çalıştırmak istediğiniz diğer tüm eylemler ekleyin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Gecikme ekleme-eylem kadar

1. Mantıksal Uygulama Tasarımcısı gecikme eylemi, eklemek istediğiniz adımı altında seçin **yeni adım**.

   Adımları arasındaki gecikme eylemi eklemek için adımları bağlanan okun üzerine işaretçiyi taşıyın. Görünen artı işaretini (+) tıklayın ve ardından seçin **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "gecikme" girin. Eylem listesinden şu eylemi seçin: **Geciktir:**

   !["Geciktir" Eylem Ekle](./media/connectors-native-delay/add-delay-until-action.png)

1. İş akışını sürdürmek istediğiniz zamanı bitiş tarihi ve saati belirtin.

   ![Gecikme süresini sona erdirmek ne zaman damgası belirtin](./media/connectors-native-delay/delay-until-timestamp.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Zaman damgası | timestamp | Evet | String | Bitiş tarihi ve saati şu biçimi kullanarak iş akışını sürdürme için: <p>YYYY-AA-ssZ <p>Örneğin, 18 Eylül 2017 2: 00'da isterseniz belirtin "2017-09-18T14:00:00Z". <p>**Not:** Bu saat biçimi izlemelidir [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi en sonda boşluk olmadan "Z" harfi eklemeniz gerekir. Bu "Z" eş değeri başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). |
   ||||||

1. İş akışınızı çalıştırmak istediğiniz diğer tüm eylemler ekleyin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturmanıza, zamanlamanıza ve yineleme tetikleyicisi ile yinelenen görevleri ve iş akışları çalıştırma](../connectors/connectors-native-recurrence.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)