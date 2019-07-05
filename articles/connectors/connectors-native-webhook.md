---
title: Azure Logic Apps'te olay-tabanlı görevler ve iş akışları oluşturun
description: Tetiklemek için Duraklat ve otomatik görevler, süreçleri ve iş akışları Azure Logic Apps kullanarak bir uç noktada gerçekleşen olayları temel Sürdür
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: c2658df185d4836210c496d2c46a00a3541257a2
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541405"
---
# <a name="automate-event-based-tasks-and-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Azure Logic Apps'te HTTP Web kancalarını kullanarak olay-tabanlı görevler ve iş akışlarını otomatikleştirin

İle [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP Web kancası Bağlayıcısı bekleyin ve mantıksal uygulamalar oluşturarak bir HTTP veya HTTPS uç noktada gerçekleşen belirli olayları temelinde çalıştırılacak iş akışlarını otomatik hale getirebilirsiniz. Örneğin, bir hizmet uç noktası için belirli bir olay iş akışını tetikleyen ve belirtilen eylemleri çalıştırmak yerine önce düzenli olarak denetleme bekleyerek izleyen bir mantıksal uygulama oluşturabilirsiniz veya *yoklama* Bu uç nokta.

Bazı örnek olay-tabanlı iş akışları şunlardır:

* Gelen ulaşması için bir öğe için bekleyin bir [Azure olay hub'ı](https://github.com/logicappsio/EventHubAPI) önce bir mantıksal uygulama çalıştırması tetikleniyor.
* Bir iş akışı devam etmeden önce onay bekler.

## <a name="how-do-webhooks-work"></a>Web kancaları nasıl çalışır?

Bir HTTP Web kancası tetikleyici olay tabanlı, denetimi veya düzenli olarak yeni öğeler için yoklama bağımlı değildir. Mantıksal uygulama bir Web kancası tetikleyici ile başlar veya mantıksal uygulama devre dışı iken etkin olarak, Web kancası tetikleyicisine değiştirdiğinizde kaydettiğinizde *abone* belirli bir hizmet veya kaydederek uç noktası için bir *geriçağırmaURL'si* hizmet veya uç nokta. Tetikleyici, sonra bu hizmeti veya mantıksal uygulama çalışmaya başlar URL'yi çağırmak için uç nokta için bekler. Benzer şekilde [istek tetikleyicisi](connectors-native-reqres.md), belirtilen olay durumda hemen mantıksal uygulama başlatılır. Tetikleyici *abonelikten çıkma* hizmet ya da uç nokta tetikleyici kaldırın ve mantıksal uygulamanızı kaydedin veya mantıksal uygulamanızdan değiştirdiğinizde etkin devre dışı.

Olay tabanlı bir HTTP Web kancası eylem ayrıca ve *abone* belirli bir hizmet veya kaydederek uç noktası için bir *geri çağırma URL'si* hizmet veya uç nokta. Web kancası eylemi mantıksal uygulamanın iş akışı duraklatır ve aramaları çalıştıran logic app sürdürür önce URL'yi hizmet veya uç nokta kadar bekler. Eylem mantıksal uygulama *abonelikten çıkma* hizmet veya bu gibi durumlarda uç noktası:

* Web kancası eylemi başarıyla tamamlandığında
* Bir yanıtı beklenirken mantıksal uygulama çalıştırması iptal edilirse
* Uygulama mantığı önce zaman aşımına uğruyor

Örneğin, Office 365 Outlook bağlayıcının [ **onay e-posta Gönder** ](connectors-create-api-office365-outlook.md) eylem Bu desen aşağıdaki Web kancası eylem örneği verilmiştir. Bu düzen, Web kancası eylemi kullanarak herhangi bir hizmeti genişletebilirsiniz.

Daha fazla bilgi için şu konulara bakın:

* [HTTP Web kancası tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Web kancaları ve abonelikler](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Bir Web kancası desteği, özel API'ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Zaten dağıtılmış bir uç nokta URL'sini ya da Web kancası destekleyen API abone olma ve aboneliği desenini [logic apps'teki Web kancası Tetikleyicileri](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) veya [Web kancası eylemleri logic apps'teki](../logic-apps/logic-apps-create-api-app.md#webhook-actions) uygun şekilde

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

* Hedef uç noktasında belirli olaylar için beklenecek istediğiniz mantıksal uygulaması. HTTP Web kancası tetikleyici ile başlayın için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP Web kancası eylemi kullanmak için mantıksal uygulamanızın istediğiniz herhangi bir tetikleyici ile başlayın. Bu örnek HTTP tetikleyicisi, ilk adım olarak kullanır.

## <a name="add-an-http-webhook-trigger"></a>Bir HTTP Web kancası tetikleyici ekleme

Bu yerleşik bir tetikleyici, bir geri çağırma URL'si belirtilen hizmetine kaydeder ve bu hizmet için bu URL'yi bir HTTP POST isteği göndermek bekler. Bu olay meydana geldiğinde tetiklenir ve mantıksal uygulamayı hemen çalıştırılır.

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

1. Tasarımcıda arama kutusuna filtreniz olarak "http Web kancası" yazın. Gelen **Tetikleyicileri** listesinden **HTTP Web kancası** tetikleyici.

   ![HTTP Web kancası tetikleyicisini seçin](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   Bu örnek daha açıklayıcı bir ad adım sahip olacak şekilde "HTTP Web kancası tetikleyici" tetikleyiciye yeniden adlandırır. Ayrıca, örnek daha sonra bir HTTP Web kancası eylem ekler ve her iki adları benzersiz olmalıdır.

1. İçin değerler sağlayın [HTTP Web kancası tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) kullanmak için abonelik ve aramalar, örneğin aboneliği istiyorsanız:

   ![HTTP Web kancası tetikleyici parametreleri girin](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

   HTTP Web kancası kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Mantıksal uygulamanızın Tetikleyici etkinleştirildiğinde çalıştırılan eylemleri iş akışı oluşturmaya devam edin.

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmek Bitti, unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

   Mantıksal uygulamanızı kaydetme abone ol uç noktasına çağrı ve bu mantıksal uygulama tetiklemek için geri çağırma URL'si kaydeder.

1. Artık, her hedef hizmet gönderen bir `HTTP POST` istek geri çağırma URL'si için mantıksal uygulama etkinleşir ve isteği aracılığıyla iletilen tüm verileri içerir.

## <a name="add-an-http-webhook-action"></a>HTTP Web kancası Eylem Ekle

Bu yerleşik eylem, bir geri çağırma URL'si belirtilen hizmetine kaydeder, mantıksal uygulama iş akışı duraklatır ve bu hizmet için bu URL'yi bir HTTP POST isteği göndermek bekler. Bu olay meydana geldiğinde, eylem, mantıksal uygulama çalıştırma sürdürür.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

   Bu örnekte, ilk adım olarak HTTP Web kancası tetikleyici kullanır.

1. HTTP Web kancası eylem eklemek istediğiniz adımı altında seçin **yeni adım**.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Tasarımcıda arama kutusuna filtreniz olarak "http Web kancası" yazın. Gelen **eylemleri** listesinden **HTTP Web kancası** eylem.

   ![HTTP Web kancası eylemi seçin](./media/connectors-native-webhook/select-http-webhook-action.png)

   Bu örnek daha açıklayıcı bir ad adım sahip olacak şekilde "HTTP Web kancası eylemi" eylemini yeniden adlandırır.

1. Değerleri için HTTP Web kancası benzer olan eylem parametrelerini girin [HTTP Web kancası tetikleyici parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger) kullanmak için abonelik ve aramalar, örneğin aboneliği istiyorsanız:

   ![HTTP Web kancası eylem parametrelerini girin](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   Abone uç noktası bu eylem çalıştırıldığında, mantıksal uygulama çalışma zamanı sırasında çağırır. Mantıksal uygulamanız sonra iş akışı duraklatır ve göndermek hedef hizmet için bekleyen bir `HTTP POST` istek geri çağırma URL'si. Eyleme başarıyla tamamlandığında, eylemi uç noktasından abonelikten çıkma ve mantıksal uygulamanızı sürdürür iş akışını çalıştıran.

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

   HTTP Web kancası kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Birbirine benzer, tetikleyici ve eylem parametreleri hakkında daha fazla bilgi için bkz. [HTTP Web kancası parametreleri](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger).

### <a name="output-details"></a>Çıkış Ayrıntıları

Bir HTTP Web kancası tetikleyicisine veya bu bilgileri döndüren eylem çıkışları hakkında daha fazla bilgi aşağıda verilmiştir:

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
