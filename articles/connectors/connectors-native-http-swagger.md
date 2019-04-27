---
title: Azure Logic Apps'ten REST uç noktalarını çağırma | Microsoft Docs
description: Görevler ve REST uç noktaları ile iletişim kurmak HTTP + Swagger'ı kullanarak iş akışlarını otomatikleştirmek Azure Logic Apps Bağlayıcısı
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
tags: connectors
ms.topic: article
ms.date: 07/18/2016
ms.openlocfilehash: 9408b66f74391b080ef46c758b07850b2ae8de57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60448657"
---
# <a name="call-rest-endpoints-with-http--swagger-connector-in-azure-logic-apps"></a>HTTP + Swagger REST uç noktalarına çağrı Azure Logic Apps Bağlayıcısı

Herhangi bir REST uç noktası için birinci sınıf bir bağlayıcı oluşturabilirsiniz bir [Swagger belgesinin](https://swagger.io) da HTTP + Swagger kullandığınızda, mantıksal uygulama iş akışı eylemi. Ayrıca, birinci sınıf bir mantıksal Uygulama Tasarımcısı deneyimi ile herhangi bir REST uç noktasını çağırmak için mantıksal uygulamaları genişletebilirsiniz.

Bağlayıcılarla mantıksal uygulamalar oluşturma konusunda bilgi almak için bkz: [yeni bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Kullanım HTTP + Swagger bir tetikleyici veya eylemi

HTTP + Swagger tetikleyin ve aynı iş eylemi [HTTP eylemi](connectors-native-http.md) ancak API yapısı ve çıkışları göstererek Logic App Tasarımcısı'nda daha iyi bir deneyim sunmak [Swagger meta verileri](https://swagger.io). HTTP da kullanabileceğinizi + bir tetikleyici olarak Swagger Bağlayıcısı. Yoklama tetikleyici uygulamak istiyorsanız, açıklanan yoklama desenler izleyen [logic apps'ten diğer API'leri, hizmetleri ve sistemleri çağırmak için özel API'ler oluşturma](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Daha fazla bilgi edinin [mantıksal uygulama Tetikleyicileri ve eylemleri](../connectors/apis-list.md).

Kullanım HTTP + Swagger işlem bir eylem olarak bir mantıksal uygulama bir iş akışında ilişkin bir örnek aşağıda verilmiştir.

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna **swagger** listesi HTTP + Swagger eylem.
   
    ![HTTP + Swagger'ı seçin eylemi](./media/connectors-native-http-swagger/using-action-1.png)
4. Swagger belgesinin URL'sini yazın:
   
   * Mantıksal Uygulama Tasarımcısı'ndan çalışmak için URL bir HTTPS uç noktası olması gerekir ve CORS etkinleştirmesi gerekir.
   * Swagger belgesinin bu gereksinimi karşılamıyorsa, belge depolamak için CORS'yi etkinleştirerek Azure depolamayı kullanabilirsiniz.
5. Tıklayın **sonraki** okuyun ve Swagger belgesi işlemek için.
6. HTTP çağrı için gerekli olan herhangi bir parametre ekleyin.
   
    ![HTTP eylemi tamamlandı](./media/connectors-native-http-swagger/using-action-2.png)
7. Kaydet ve mantıksal uygulamanızı yayımlamak için tıklatın **Kaydet** tasarımcı araç çubuğunda.

### <a name="host-swagger-from-azure-storage"></a>Azure depolama biriminden konak Swagger
Değil barındırılan veya, güvenlik ve tasarımcı için çıkış noktaları arası gereksinimleri karşılamıyor bir Swagger belgesi başvurmak isteyebilirsiniz. Bu sorunu çözmek için Swagger belgesinin Azure Storage'a depoladığınız ve belge başvurmak CORS'yi etkinleştirme.  

Oluşturma, yapılandırma ve Azure Depolama'da Swagger belgeleri depolamak için adımlar şunlardır:

1. [Azure Blob Depolama ile bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Bu adımı gerçekleştirmek için izinleri ayarla **genel erişim**.

2. CORS bloba etkinleştirin. 

   Bu ayar otomatik olarak yapılandırmak için kullanabileceğiniz [bu PowerShell Betiği](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Swagger dosyası için blob karşıya yükleyin. 

   Bu adımda gerçekleştirdiğiniz [Azure portalında](https://portal.azure.com) veya gibi bir araçla [Azure Depolama Gezgini](https://storageexplorer.com/).

4. Azure Blob depolama alanındaki bir belge için bir HTTPS bağlantısı başvuru. 

   Bağlantı şu biçimi kullanır:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Teknik ayrıntılar
Tetikleyiciler ve Eylemler ile ilgili ayrıntıları aşağıda verilmiştir, bu da HTTP + Swagger Bağlayıcısı destekler.

## <a name="http--swagger-triggers"></a>HTTP + Swagger Tetikleyicileri
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. HTTP + Swagger Bağlayıcısı, bir tetikleyici vardır. [Tetikleyiciler hakkında daha fazla bilgi](../connectors/apis-list.md).

| Tetikleyici | Açıklama |
| --- | --- |
| HTTP + Swagger |HTTP çağrısı ve yanıt içeriği döndürür |

## <a name="http--swagger-actions"></a>HTTP + Swagger eylemleri
Bir eylem mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. HTTP + Swagger Bağlayıcısı olası bir eylem vardır. [Eylemler hakkında daha fazla bilgi](../connectors/apis-list.md).

| Eylem | Açıklama |
| --- | --- |
| HTTP + Swagger |HTTP çağrısı ve yanıt içeriği döndürür |

### <a name="action-details"></a>Eylem ayrıntıları
HTTP + Swagger Bağlayıcısı olası tek bir eylem ile birlikte gelir. Aşağıda, her eylemleri, bunların gerekli ve isteğe bağlı bir giriş alanlarını ve bunların kullanımıyla ilişkili çıkış ayrıntıları hakkında bilgi verilmektedir.

#### <a name="http--swagger"></a>HTTP + Swagger
Giden HTTP isteği, Swagger meta verileri ile ilgili Yardım olun.
Bir yıldız işareti (*) gerekli bir alan anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Yöntem * |method |Kullanılacak HTTP fiili. |
| URI * |uri |HTTP isteği için URI. |
| Üst bilgiler |Üst bilgileri |HTTP üstbilgileri dahil etmek için bir JSON nesnesi. |
| Gövde |body |HTTP istek gövdesi. |
| Kimlik Doğrulaması |kimlik doğrulaması |İstek için kullanılacak kimlik doğrulaması. Daha fazla bilgi için [HTTP Bağlayıcısı](connectors-native-http.md#authentication). |

**Çıkış Ayrıntıları**

HTTP yanıtı

| Özellik Adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |object |Yanıt üst bilgileri |
| Gövde |object |Yanıt nesnesi |
| Durum Kodu |int |HTTP durum kodu |

### <a name="http-responses"></a>HTTP yanıtları
Çeşitli eylemler çağrı yaparken, belirli yanıtlarını alabilirsiniz. Aşağıdaki karşılık gelen yanıtları ve açıklamaları özetleyen bir tablodur.

| Ad | Açıklama |
| --- | --- |
| 200 |Tamam |
| 202 |Kabul Edildi |
| 400 |Hatalı istek |
| 401 |Yetkilendirilmemiş |
| 403 |Yasak |
| 404 |Bulunamadı |
| 500 |İç sunucu hatası. Bilinmeyen bir hata oluştu. |

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Diğer bağlayıcıları bulma](apis-list.md)
