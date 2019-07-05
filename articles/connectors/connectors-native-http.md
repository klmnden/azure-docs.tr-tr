---
title: Azure Logic Apps'ten HTTP veya HTTPS Uç noktalara bağlanmak
description: Azure Logic Apps kullanarak otomatik görevler, süreçleri ve iş akışları HTTP veya HTTPS uç noktaları izleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: fa5fd3ef8b144826468f56ea2a14be592cef5dc1
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541289"
---
# <a name="call-http-or-https-endpoints-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak HTTP veya HTTPS uç noktalarına çağrı

İle [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP Bağlayıcısı, mantıksal uygulamalar oluşturarak tüm HTTP veya HTTPS uç noktası düzenli olarak çağıran iş akışlarını otomatik hale getirebilirsiniz. Örneğin, belirli bir zamanlamaya göre uç noktanın denetleyerek Web siteniz için hizmet uç noktası izleyebilirsiniz. Aşağı doğru giden, Web sitesi gibi bu uç noktada bir özel olay meydana geldiğinde olay mantıksal uygulamanızın iş akışı tetikler ve belirtilen eylemleri çalıştırır.

Denetlenecek veya *yoklama* bir uç nokta düzenli bir zamanlamaya göre HTTP tetikleyicisi ilk adım olarak, iş akışınızı kullanabilirsiniz. Her denetimi, bir çağrı tetikleyici gönderir veya *isteği* uç noktası. Uç noktanın yanıt, mantıksal uygulamanızın iş akışı çalıştırır olup olmadığını belirler. Tetikleyici, mantıksal uygulama eylemleri yanıta herhangi bir içerik geçirir.

HTTP eylem istediğiniz zaman uç noktasını çağırmak için akışınızda herhangi bir adım olarak kullanabilirsiniz. Uç noktanın yanıt kalan iş akışının eylemlerini çalışma şeklini belirler.

Hedef uç noktanın yeteneği, temel HTTP Bağlayıcısı Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2 sürümleri destekler. Logic Apps, olası en yüksek desteklenen sürümünü kullanarak üzerinden uç noktası ile görüşür. Uç nokta 1.2 destekliyorsa, bu nedenle, örneğin, bağlayıcı 1.2 ilk kullanır. Aksi halde, sonraki en yüksek desteklenen sürüm Bağlayıcısı'nı kullanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Aramak istediğiniz hedef uç nokta URL'si

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

* Hedef uç noktasını çağırmak istediğiniz mantıksal uygulaması. HTTP tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylem kullanmak için mantıksal uygulamanızın istediğiniz herhangi bir tetikleyici ile başlayın. Bu örnek HTTP tetikleyicisi, ilk adım olarak kullanır.

## <a name="add-an-http-trigger"></a>HTTP tetikleyicisi Ekle

Bu yerleşik bir tetikleyici, bir uç nokta için belirtilen URL için HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

1. Tasarımcıda arama kutusuna filtreniz olarak "http" yazın. Gelen **Tetikleyicileri** listesinden **HTTP** tetikleyici.

   ![HTTP tetikleyicisini seçin](./media/connectors-native-http/select-http-trigger.png)

   Bu örnek daha açıklayıcı bir ad adım sahip olacak şekilde "HTTP tetikleyicisi" tetikleyiciye yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP eylemi ekler ve her iki adları benzersiz olmalıdır.

1. İçin değerler sağlayın [HTTP tetikleyicisi parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) hedef uç noktasına çağrıda dahil etmek istediğiniz. Hedef uç nokta denetlemek için yineleme tetikleyicisi istediğiniz sıklığı için ayarlayın.

   ![HTTP tetikleyicisi parametreleri girin](./media/connectors-native-http/http-trigger-parameters.png)

   HTTP için kullanılabilen kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

1. Mantıksal uygulamanızın Tetikleyici etkinleştirildiğinde çalıştırılan eylemleri iş akışı oluşturmaya devam edin.

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmek Bitti, unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

## <a name="add-an-http-action"></a>HTTP Eylem Ekle

Bu yerleşik eylem, bir uç nokta için belirtilen URL için HTTP çağrısı yapar ve bir yanıt döndürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

   Bu örnek HTTP tetikleyicisi, ilk adım olarak kullanır.

1. HTTP eylem eklemek istediğiniz adımı altında seçin **yeni adım**.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Tasarımcıda arama kutusuna filtreniz olarak "http" yazın. Gelen **eylemleri** listesinden **HTTP** eylem.

   ![HTTP eylemi seçin](./media/connectors-native-http/select-http-action.png)

   Bu örnek, bir adım daha açıklayıcı bir ad sahip olacak şekilde "HTTP eylemi" eylemini yeniden adlandırır.

1. İçin değerler sağlayın [HTTP eylem parametrelerini](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) hedef uç noktasına çağrıda dahil etmek istediğiniz.

   ![HTTP eylem parametrelerini girin](./media/connectors-native-http/http-action-parameters.png)

   HTTP için kullanılabilen kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyici ve eylem parametreleri hakkında daha fazla bilgi için aşağıdaki bölümlere bakın:

* [HTTP tetikleyicisi parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger)
* [HTTP eylem parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action)

### <a name="output-details"></a>Çıkış Ayrıntıları

Bir HTTP tetikleyicisi veya bu bilgileri döndüren eylem çıkışları hakkında daha fazla bilgi aşağıda verilmiştir:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| Üst bilgileri | object | İstek üstbilgileri |
| Gövde | object | JSON nesnesi | İstek gövdesi içeriğe sahip nesne |
| Durum kodu | int | İstek durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Kabul edildi |
| 400 | Hatalı istek |
| 401 | Yetkilendirilmemiş |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen bir hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
