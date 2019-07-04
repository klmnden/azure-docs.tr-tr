---
title: Azure Logic Apps'ten REST Uç noktalara bağlanmak
description: Azure Logic Apps kullanarak otomatik görevler, süreçleri ve iş akışlarını REST uç noktalarını izleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/05/2019
tags: connectors
ms.openlocfilehash: f0410ed7a98e4838e41407868cf26b5254811ae3
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541663"
---
# <a name="call-rest-endpoints-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak REST uç noktalarını çağırma

İle [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP + Swagger Bağlayıcısı, herhangi bir REST uç noktası aracılığıyla düzenli olarak çağıran iş akışlarını otomatik hale getirebilirsiniz bir [Swagger dosyası](https://swagger.io) oluşturan mantıksal uygulamalar tarafından. HTTP + Swagger tetikleyin ve aynı iş eylemi [HTTP tetikleyici ve eylem](connectors-native-http.md) ancak API yapısı ve Swagger dosyası tarafından tanımlanan çıkış göstererek mantıksal Uygulama Tasarımcısı'nda daha iyi bir deneyim sağlar. Yoklama tetikleyici uygulamak için açıklanan yoklama yapıdadır [logic apps'ten diğer API'leri, hizmetleri ve sistemleri çağırmak için özel API'ler oluşturma](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hedef REST uç noktasını tanımlayan Swagger dosyası URL'si

  Genellikle, REST uç noktasını iş Bağlayıcısı bu ölçütü karşılaması gerekir:

  * Swagger dosyası genel olarak erişilebilir olan bir HTTPS URL'si üzerinde barındırılması gerekir.

  * Swagger dosyası olmalıdır [çıkış noktaları arası kaynak paylaşımı (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) etkin.

  Bir Swagger dosyası değil bulunan veya güvenlik ve çıkış noktaları arası gereksinimleri karşılamıyor başvurmak için [Swagger dosyası bir Azure depolama hesabındaki bir blob kapsayıcıya yüklemeniz](#host-swagger)ve bu depolama hesabında, bu nedenle CORS'yi etkinleştirme Dosya, başvurabilirsiniz.

  Bu konuda kullanım örnekleri [Bilişsel hizmetler yüz tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/face/overview), gerektiren bir [Bilişsel Hizmetler hesabı ve erişim anahtarını](../cognitive-services/cognitive-services-apis-create-account.md).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

* Hedef uç noktasını çağırmak istediğiniz mantıksal uygulaması. Başlamak da HTTP + Swagger tetiklemek, [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Kullanım HTTP + Swagger eylem, mantıksal uygulamanızın istediğiniz herhangi bir tetikleyici ile başlayın. Bu örnek kullanımları HTTP + Swagger tetikleyici ilk adım olarak.

## <a name="add-an-http--swagger-trigger"></a>Bir HTTP + Swagger ekleme tetikleyicisi

Bu yerleşik bir tetikleyici bir REST API'sini açıklayan ve bu dosyanın içeriğini içeren bir yanıt döndürür bir Swagger dosyası için bir URL bir HTTP isteği gönderir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

1. Üzerinde tasarımcıda arama kutusuna filtreniz olarak "swagger" girin. Gelen **Tetikleyicileri** listesinden **da HTTP + Swagger** tetikleyici.

   ![Select da HTTP + Swagger tetikleyin](./media/connectors-native-http-swagger/select-http-swagger-trigger.png)

1. İçinde **SWAGGER uç nokta URL'si** kutusuna için Swagger dosyası URL'sini girin ve seçin **sonraki**.

   Bu örnekte Batı ABD bölgesi için bulunan Swagger URL'si [Bilişsel hizmetler yüz tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236):

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Swagger uç noktası için URL girin](./media/connectors-native-http-swagger/http-swagger-trigger-parameters.png)

1. Tasarımcı Swagger dosyası tarafından tanımlanan işlemlerini gösterir, kullanmak istediğiniz işlemi seçin.

   ![Swagger dosyası işlemlerinde](./media/connectors-native-http-swagger/http-swagger-trigger-operations.png)

1. Uç nokta çağrısında eklemek istediğiniz seçili işlem göre değişiklik gösteren tetikleyici parametreler için değerleri girin. Uç noktasını çağırmak için yineleme tetikleyicisi istediğiniz sıklığı için ayarlayın.

   Bu örnekte, tetikleyiciyi yeniden adlandırır "HTTP + Swagger tetikleyin: Yüz tanıma - algıla"adım daha açıklayıcı bir ad sahip olacak şekilde.

   ![İşlem ayrıntıları](./media/connectors-native-http-swagger/http-swagger-trigger-operation-details.png)

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

   HTTP + Swagger kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz: [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Mantıksal uygulamanızın Tetikleyici etkinleştirildiğinde çalıştırılan eylemleri iş akışı oluşturmaya devam edin.

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

## <a name="add-an-http--swagger-action"></a>Bir HTTP + Swagger ekleme eylemi

Bu yerleşik eylem, bu dosyanın içeriğini içeren bir yanıt döndürür ve bir REST API'sini açıklayan Swagger dosyası URL'si için bir HTTP isteği yapar.

1. [Azure Portal](https://portal.azure.com) oturum açın. Mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın.

1. HTTP + Swagger eklemek istediğiniz adımı altında seçme eylemini **yeni adım**.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. Artı işaretini seçin ( **+** ), görünür ve ardından **Eylem Ekle**.

1. Üzerinde tasarımcıda arama kutusuna filtreniz olarak "swagger" girin. Gelen **eylemleri** listesinden **da HTTP + Swagger** eylem.

    ![HTTP + Swagger'ı seçin eylemi](./media/connectors-native-http-swagger/select-http-swagger-action.png)

1. İçinde **SWAGGER uç nokta URL'si** kutusuna için Swagger dosyası URL'sini girin ve seçin **sonraki**.

   Bu örnekte Batı ABD bölgesi için bulunan Swagger URL'si [Bilişsel hizmetler yüz tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236):

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Swagger uç noktası için URL girin](./media/connectors-native-http-swagger/http-swagger-action-parameters.png)

1. Tasarımcı Swagger dosyası tarafından tanımlanan işlemlerini gösterir, kullanmak istediğiniz işlemi seçin.

   ![Swagger dosyası işlemlerinde](./media/connectors-native-http-swagger/http-swagger-action-operations.png)

1. Uç nokta çağrısında eklemek istediğiniz seçili işlem göre farklılık eylem parametreleri için değerleri girin.

   Bu örnek için hiç parametre yok, ancak bu eylem için yeniden adlandırır "HTTP + Swagger eylem: Yüz tanıma - tanımlamak"adım daha açıklayıcı bir ad sahip olacak şekilde.

   ![İşlem ayrıntıları](./media/connectors-native-http-swagger/http-swagger-action-operation-details.png)

1. Kullanılabilir diğer parametre eklemek için açık **yeni parametre Ekle** listesinde ve istediğiniz parametreleri seçin.

   HTTP + Swagger kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz: [HTTP kimlik doğrulaması tetikleyiciler ve Eylemler](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. İşlemi tamamladığınızda, mantıksal uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet**.

<a name="host-swagger"></a>

## <a name="host-swagger-in-azure-storage"></a>Azure depolama alanında konak Swagger

Swagger dosyası değil barındırılan veya bir Azure depolama hesabındaki blob kapsayıcısına bu dosyayı karşıya yükleme ve bu depolama hesabında CORS'yi etkinleştirme güvenlik ve çıkış noktaları arası gereksinimlerini karşılayan değil başvurabilirsiniz. Swagger dosyaları Azure Storage'a depoladığınız oluşturmak ve ayarlamak için bu adımları izleyin:

1. [Bir Azure depolama hesabı oluşturun](../storage/common/storage-create-storage-account.md).

1. Şimdi blob için CORS'yi etkinleştirin. Depolama hesabınızın menüsünde **CORS**. Üzerinde **Blob hizmeti** sekme, bu değerleri belirtin ve ardından **Kaydet**.

   | Özellik | Değer |
   |----------|-------|
   | **İzin verilen çıkış noktaları** | `*` |
   | **İzin verilen yöntemleri** | `GET`, `HEAD`, `PUT` |
   | **İzin verilen üst bilgiler** | `*` |
   | **Sunulan üst bilgiler** | `*` |
   | **Maksimum yaş** (saniye cinsinden) | `200` |
   |||

   Bu örnek kullansa [Azure portalında](https://portal.azure.com), gibi bir araç kullanın [Azure Depolama Gezgini](https://storageexplorer.com/), ya da otomatik olarak bu örneği kullanarak bu ayarı yapılandırın [PowerShellBetiği](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

1. [Bir blob kapsayıcısı oluşturursunuz](../storage/blobs/storage-quickstart-blobs-portal.md). Kapsayıcının üzerinde **genel bakış** bölmesinde **erişim düzeyini Değiştir**. Gelen **genel erişim düzeyi** listesinden **Blob (yalnızca BLOB'lar için anonim okuma erişimi)** seçip **Tamam**.

1. [Swagger dosyası blob kapsayıcısına yükleyin](../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob), ya da aracılığıyla [Azure portalında](https://portal.azure.com) veya [Azure Depolama Gezgini](https://storageexplorer.com/).

1. Blob kapsayıcısında dosyaya başvurmak için büyük/küçük harfe Bu biçim izleyen bir HTTPS bağlantısı kullanın:

   `https://<storage-account-name>.blob.core.windows.net/<blob-container-name>/<swagger-file-name>`

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bir HTTP + Swagger çıkışları hakkında daha fazla bilgi aşağıdadır tetikleyici veya eylemi. HTTP + Swagger çağrısı bu bilgileri döndürür:

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