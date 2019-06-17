---
title: Daha fazla veri, öğeleri veya sayfalandırma - Azure Logic Apps ile kayıt Al
description: Azure Logic Apps Bağlayıcısı eylemleri için varsayılan sayfa boyutu sınırını aşmasına sayfalandırma ayarlayın
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 2d1bcf2cf83fab106f79120c3caacc424f839836
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64476549"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Azure Logic Apps'te sayfalandırma kullanarak daha fazla veri, öğe veya kayıt Al

Aldığınızda veri, öğeleri veya kayıtları bir bağlayıcı eylemini kullanarak [Azure Logic Apps](../logic-apps/logic-apps-overview.md), sonuç kümeleri çok büyük eylem aynı anda tüm sonuçlar döndürmüyor alabilirsiniz. Bazı eylemler ile bağlayıcı'nın varsayılan sayfa boyutu sonuç sayısını aşabilir. Bu durumda, eylem sonuçlarını yalnızca ilk sayfasını döndürür. Örneğin, SQL Server bağlayıcı'nın varsayılan sayfa boyutu **satırları Al** eylem 2048'dir, ancak diğer ayarlar göre değişebilir.

Size bazı eylemler çalıştıracak bir *sayfalandırma* mantıksal uygulamanız sayfalandırma sınıra kadar daha fazla sonuç almak ancak eylem tamamlandığında tek bir mesaj olarak bu sonuçları döndürmek için ayarı. Sayfalandırma kullandığınızda belirtmeniz gerekir bir *eşiği* sonuçları döndürmek için eylem istediğiniz hedef sayısı değeri. Eylem, belirtilen eşik ulaşana kadar sonuçlarını alır. Toplam öğe sayısı belirtilen eşik değerinden düşük olduğunda, tüm sonuçları eylemi alır.

Bir bağlayıcı'nın sayfa boyutuna göre sonuçlarını sayfalandırma ayarı alır sayfalarında kapatılıyor. Bu davranışı bazı durumlarda, size, belirtilen eşikten daha fazla sonuç elde anlamına gelir. Örneğin, SQL Server kullanırken **satırları Al** sayfalandırma ayarını destekler eylemini:

* Eylemin varsayılan sayfa boyutu 2048 sayfa başına kayıt ' dir.
* 10\.000 kayıtları ve en düşük 5000 kayıt belirtin varsayalım.
* Sayfalar, bu nedenle en az belirtilen en düşük almak için kayıtlarının sayfalandırma alır, eylem 6144 kayıtları (3 sayfaları x 2048 kayıtlar), 5000 kayıtları döndürür.

Burada belirli eylemler için varsayılan sayfa boyutu aşabilir bağlayıcıları yalnızca bir kısmı bir listesi aşağıda verilmiştir:

* [Azure Blob Depolama](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Oracle Veritabanı](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulama ve sayfalandırma üzerinde etkinleştirmek için istediğiniz eylemi. Mantıksal uygulama yoksa bkz [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Sayfalandırma üzerinde Aç

Bir eylem, mantıksal Uygulama Tasarımcısı'nda sayfalandırma destekleyip desteklemediğini belirlemek için eylem ayarlarını kontrol edin **sayfalandırma** ayarı. Bu örnek, SQL Server'ın sayfalandırma nasıl gösterir **satırları Al** eylem.

1. Eylemin sağ üst köşede bulunan üç noktayı seçin ( **...** ) düğmesini ve **ayarları**.

   ![Eylem ayarları](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Sayfalandırma eylemi destekliyorsa, bir işlem gösterilir **sayfalandırma** ayarı.

1. Değişiklik **sayfalandırma** ayarını **kapalı** için **üzerinde**. İçinde **eşiği** özelliği için döndürülecek eylem istediğiniz sonuçları hedef sayısı bir tamsayı değeri belirtin.

   ![Döndürülecek sonuç sayısı alt sınırını belirtin](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Hazır olduğunuzda seçin **Bitti**.

## <a name="workflow-definition---pagination"></a>İş akışı tanımı - sayfalandırma

Bu özelliği destekleyen bir eylem için sayfalandırma açtığınızda, mantıksal uygulamanızın iş akışı tanımını içeren `"paginationPolicy"` özelliği ile birlikte `"minimumItemCount"` bu eylemin özelliğinde `"runtimeConfiguration"` özelliği, örneğin:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
