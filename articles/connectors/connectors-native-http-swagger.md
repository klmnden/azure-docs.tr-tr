---
title: "REST uç HTTP + Swagger ile çağrı Azure Logic Apps bağlayıcı | Microsoft Docs"
description: "HTTP + Swagger ile Swagger aracılığıyla mantığı uygulamalardan REST Uç noktalara bağlanmak Bağlayıcısı"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: eccfd87c-c5fe-4cf7-b564-9752775fd667
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 0487dbedddee684c75420bd66effe2c963a18624
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-http--swagger-action"></a>HTTP + Swagger ile başlama eylemi

Tüm REST uç noktası aracılığıyla birinci sınıf bir bağlayıcı oluşturabilirsiniz bir [Swagger belgesinin](https://swagger.io) kullandığınızda, HTTP + Swagger mantığı uygulama akışınızın eylem. Birinci sınıf bir mantıksal Uygulama Tasarımcısı deneyim herhangi bir REST uç nokta çağırmak için mantıksal uygulamalar da genişletebilirsiniz.

Logic apps ile bağlayıcılar oluşturmayı öğrenmek için bkz: [yeni bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>HTTP kullan + tetikleyicinin veya bir eylem olarak Swagger

HTTP + tetiklemek Swagger ve aynı iş eylemi [HTTP eylemi](connectors-native-http.md) ancak API yapısı ve çıkışlarından göstererek mantığı Uygulama Tasarımcısı'nda daha iyi bir deneyim sağlamak [Swagger meta verileri](https://swagger.io). HTTP de kullanabilirsiniz + Swagger bağlayıcı tetikleyici olarak. Yoklama Tetik uygulamak istiyorsanız, açıklanan yoklama modeli izleyen [diğer API'leri, hizmetleri ve sistemleri mantığı uygulamalardan çağırmak için özel API oluşturma](../logic-apps/logic-apps-create-api-app.md#polling-triggers).

Daha fazla bilgi edinmek [mantığı uygulama tetikleyiciler ve Eylemler](connectors-overview.md).

Kullanım HTTP + Swagger işlem olarak bir mantıksal uygulama bir iş akışında bir eylemi nasıl örnek aşağıda verilmiştir.

1. Seçin **yeni adım** düğmesi.
2. Seçin **Eylem Ekle**.
3. Eylem arama kutusuna yazın **swagger** listesi HTTP + Swagger eylem.
   
    ![HTTP + Swagger seçin eylemi](./media/connectors-native-http-swagger/using-action-1.png)
4. Swagger belgesinin URL'sini yazın:
   
   * Mantıksal Uygulama Tasarımcısı'ndan çalışmak için URL bir HTTPS uç noktası olması gerekir ve CORS etkinleştirdiniz.
   * Swagger belgesinin bu gereksinimi karşılamıyorsa, kullanabileceğiniz [Azure Storage CORS'yi ile](#hosting-swagger-from-storage) belge depolamak için.
5. Tıklatın **sonraki** okuyun ve Swagger belgeyi işlemek için.
6. HTTP çağrısı için gerekli olan parametreleri ekleyin.
   
    ![Tam HTTP eylemi](./media/connectors-native-http-swagger/using-action-2.png)
7. Kaydetmek ve mantıksal uygulamanızı yayımlamak için **kaydetmek** tasarımcı araç.

### <a name="host-swagger-from-azure-storage"></a>Azure depolama biriminden konak Swagger
Değil barındırılan ya da, güvenlik ve tasarımcı çıkış noktaları arası gereksinimlerini karşılamıyor Swagger belgeye başvurmak isteyebilirsiniz. Bu sorunu çözmek için Swagger belgesinin Azure depolama alanına depolar ve belgeye başvurmak CORS etkinleştirin.  

Oluşturma, yapılandırma ve Azure depolama alanında Swagger belgeleri depolamak için adımlar şunlardır:

1. [Azure Blob storage ile Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md). Bu adımı gerçekleştirmek için izinleri ayarla **genel erişim**.

2. CORS blob üzerindeki etkinleştirin. 

   Bu ayarı otomatik olarak yapılandırmak için kullanabileceğiniz [bu PowerShell Betiği](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1).

3. Swagger dosyası için blob karşıya yükleyin. 

   Bu adımdaki gerçekleştirebilirsiniz [Azure portal](https://portal.azure.com) veya gibi bir araçtan [Azure Storage Gezgini](http://storageexplorer.com/).

4. Bir HTTPS bağlantısı Azure Blob Depolama belgesine başvurun. 

   Bağlantı bu biçimi kullanır:

   `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`

## <a name="technical-details"></a>Teknik Ayrıntılar
Tetikleyiciler ve Eylemler için Ayrıntılar verilmiştir, bu HTTP + Swagger bağlayıcısını destekler.

## <a name="http--swagger-triggers"></a>HTTP + Swagger Tetikleyicileri
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi edinin.](connectors-overview.md) HTTP + Swagger Bağlayıcısı bir tetikleyici vardır.

| Tetikleyici | Açıklama |
| --- | --- |
| HTTP + Swagger |Bir HTTP çağrısı yapmak ve yanıt içeriği döndürür |

## <a name="http--swagger-actions"></a>HTTP + Swagger Eylemler
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi edinin.](connectors-overview.md) HTTP + Swagger bağlayıcının bir olası eylem.

| Eylem | Açıklama |
| --- | --- |
| HTTP + Swagger |Bir HTTP çağrısı yapmak ve yanıt içeriği döndürür |

### <a name="action-details"></a>Eylem ayrıntıları
HTTP + Swagger bağlayıcı olası bir eylem ile birlikte gelir. Aşağıda, her eylemleri, bunların gerekli ve isteğe bağlı giriş alanları ve kullanımı ile ilişkilendirilmiş ilgili çıkış ayrıntıları hakkında bilgi verilmektedir.

#### <a name="http--swagger"></a>HTTP + Swagger
Giden HTTP isteğinden Swagger meta verileri Yardım olun.
Bir yıldız işareti (*) gerekli bir alan anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Yöntemi * |yöntem |HTTP fiili'kullanılacak. |
| URI* |uri |HTTP isteği için URI. |
| Üst bilgiler |headers |Eklenecek HTTP üstbilgilerini JSON nesnesinin. |
| Gövde |body |HTTP istek gövdesi. |
| Kimlik Doğrulaması |kimlik doğrulaması |İstek için kullanılacak kimlik doğrulaması. Daha fazla bilgi için bkz: [HTTP Bağlayıcısı](connectors-native-http.md#authentication). |

**Çıkış Ayrıntıları**

HTTP yanıtı

| Özellik Adı | Veri türü | Açıklama |
| --- | --- | --- |
| Üst bilgiler |nesne |Yanıt üst bilgileri |
| Gövde |nesne |Yanıt nesnesi |
| Durum Kodu |Int |HTTP durum kodu |

### <a name="http-responses"></a>HTTP yanıtları
Çeşitli eylemler için çağrıları yapılırken belirli yanıtları alabilirsiniz. Karşılık gelen yanıtları ve açıklamaları özetleyen tablosu aşağıdadır.

| Ad | Açıklama |
| --- | --- |
| 200 |Tamam |
| 202 |Kabul Edildi |
| 400 |Hatalı istek |
| 401 |Yetkilendirilmemiş |
| 403 |Yasak |
| 404 |Bulunamadı |
| 500 |İç sunucu hatası. Bilinmeyen bir hata oluştu. |

- - -
## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Diğer bağlayıcıları Bul](apis-list.md)