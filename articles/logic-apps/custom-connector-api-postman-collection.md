---
title: "Özel API'leri ve bağlayıcıları Postman - Azure Logic Apps açıklamak | Microsoft Docs"
description: "Tanımlamak, Grup ve özel API'leri ve bağlayıcıları düzenlemek için Postman koleksiyon oluşturma"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 072d544e5d29c4abb3d69651e53cfb33d5e0c9e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="describe-custom-apis-and-custom-connectors-with-postman"></a>Özel API'leri ve özel bağlayıcıları ile Postman açıklayın

Geliştirmeyi kolaylaştırmak için [özel API'leri](../logic-apps/logic-apps-create-api-app.md) ve [özel Bağlayıcılar](../logic-apps/custom-connector-overview.md) daha hızlı ve daha kolay oluşturabileceğiniz [Postman](https://www.getpostman.com/) açıklamak için OpenAPI belgeler yerine, koleksiyonları, API ve bağlayıcıları. Düzenlemek ve Grup postman koleksiyonları Yardım ile ilgili API istekleri. Örneğin, bağlayıcılar Azure mantıksal uygulamaları, Microsoft Flow veya Microsoft PowerApps oluştururken Postman kullanabilirsiniz. 

Bu öğretici nasıl oluşturulacağını gösterir bir [Postman koleksiyonu](https://www.getpostman.com/docs/postman/collections/creating_collections) kullanarak [algılamak dil API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) içinde [Azure Bilişsel hizmetler metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) örnek olarak. Bu API dil, düşünceleri ve API için geçirdiğiniz metin anahtar tümcecikleri tanımlar.

## <a name="prerequisites"></a>Ön koşullar

* Postman için yeni başladıysanız [Postman uygulama yükleme](https://www.getpostman.com/apps).

* Azure aboneliği. Bir aboneliğiniz yoksa ile başlayabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Aksi takdirde kaydolun bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/).

* Bir Azure aboneliğiniz varsa, metin Analytics API'ler tamamlayarak kaydolun [buraya görev 1](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-signup.md). 

## <a name="create-a-postman-collection"></a>Postman koleksiyonu oluşturun

Bir koleksiyon oluşturmadan önce API uç noktası için bir HTTP isteği oluşturun. 

### <a name="create-an-http-request-for-your-api"></a>API için bir HTTP isteği oluşturma

1. Böylece oluşturabileceğiniz Postman uygulamasını açın bir [HTTP isteği](https://www.getpostman.com/docs/postman/sending_api_requests/requests) API uç noktanız için. Postman ait daha fazla bilgi için bkz: [istekleri belgelerine](https://www.getpostman.com/docs/postman/sending_api_requests/requests).

   1. Üzerinde **Oluşturucu** sekmesinde, HTTP yöntemini seçin, istek URL'si, API uç noktası için girin ve varsa bir yetkilendirme protokolünü seçin. 
   Hazır olduğunuzda, seçin **Params**.

      Bu öğretici için bu örnekte, bu ayarları kullanabilirsiniz:

      ![İsteği oluşturun: "HTTP yöntemi", "İstek URL'si", "Yetkilendirme"](./media/custom-connector-api-postman-collection/01-create-api-http-request.png)

      | Parametre | Önerilen değer | 
      | --------- | --------------- | 
      | **HTTP yöntemi** | YAYINLA | 
      | **İstek URL'si** | `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` | | 
      | **Yetkilendirme** | "Kimlik doğrulama yok" | | 
      ||| 

   2. Şimdi, sorgu veya yol parametreleri istek URL'sindeki olarak kullanmak için anahtar-değer çiftleri girebilirsiniz. Postman bu öğeler birlikte bir sorgu dizesinde birleştirir.
   İşiniz bittiğinde seçin **üstbilgileri**.

      ![İstek devam: "Parametre"](./media/custom-connector-api-postman-collection/02-create-api-http-request-params.png)

      | Parametre | Önerilen değer | 
      | --------- | --------------- | 
      | **Parametreleri** | **Anahtar**: "numberOfLanguagesToDetect" </br>**Değer**: "1" | 
      ||| 

   3. Anahtar-değer çiftleri için isteği başlığı girin. 
   Üstbilgi adı olarak istediğiniz herhangi bir dize girin. Ortak HTTP üst bilgileri için açılır listeden seçebilirsiniz. İşiniz bittiğinde seçin **gövde**. 
   
      ![İstek devam: "Üstbilgileri"](./media/custom-connector-api-postman-collection/03-create-api-http-request-header.png)

      | Parametre | Değer | 
      | --------- | ----- | 
      | **Üstbilgileri** | **Anahtar**: "Ocp-Apim-Subscription-Key" </br>**Değer**: *-API-abonelik-anahtarınız*, Bilişsel hizmetler hesabınızda bulabilirsiniz <p>**Anahtar**: "Content-Type" </br> **Değer**: "application/json" | 
      ||| 

   4. İstek gövdesinde göndermek istediğiniz içerik girin. 
   İstek yanıt geri alarak çalışıp çalışmadığını denetlemek için seçin **Gönder**. 
   
      ![İstek devam: "Body"](./media/custom-connector-api-postman-collection/04-create-api-http-request-body.png)

      | Parametre | Önerilen değer | 
      | --------- | --------------- |    
      | **Gövde** | ```{"documents": [{ "id": "1", "text": "Hello World"}]}``` | 
      ||| 

      Yanıt alan sonucu dahil olmak üzere API, tam yanıtı içerir veya herhangi bir hata oluştu.

      ![İstek yanıt Al](./media/custom-connector-api-postman-collection/05-create-api-http-request-response.png)

2. Sonra istek works kaydetmeniz isteğiniz Postman koleksiyonuna denetlediyseniz. 

   1. **Kaydet**'i seçin. 
      
      !["Kaydet" i seçin](./media/custom-connector-api-postman-collection/06a-save-request.png)
 
   2. Altında **Kaydet isteği**, sağlayan bir **istek adı** ve isteğe bağlı olarak bir **Açıklama İsteği**. 

      > [!NOTE]
      > Özel Bağlayıcınızı daha sonra bu değerleri kullanır. Bu nedenle, daha sonra özel API'nin Özet işlemi ve açıklama için kullandığınız aynı değerleri sağladığınızdan emin olun.

   3. Seçin **+ Oluştur koleksiyonu** ve koleksiyon adı sağlayın. 
   Bir koleksiyon klasörü oluşturur, onay işaretini seçin seçin **kaydetmeye *-Postman-koleksiyon-adınız***.

      ![İsteği Kaydet](./media/custom-connector-api-postman-collection/06b-save-request.png)

### <a name="save-the-request-response"></a>İstek Yanıtı Kaydet

İsteğiniz kaydettikten sonra yanıt kaydedebilirsiniz. İstek daha sonra yüklediğinizde bu şekilde yanıt örnek olarak görüntülenir.

1. Yanıt pencerenin seçin **Kaydet yanıt**. 

   ![Yanıtı Kaydet](./media/custom-connector-api-postman-collection/07-create-api-http-request-save-response.png)

   İstek başına yalnızca bir yanıt özel bağlayıcıları destekler. 
   İstek başına birden çok yanıtı kaydederseniz, yalnızca ilki kullanılır.

2. Örnek için bir ad ve seçin **Kaydet örnek**.

3. Postman koleksiyonunuzu ek istekleri ve yanıtları oluşturmaya devam edin.

### <a name="export-your-postman-collection"></a>Postman koleksiyonunuzu dışa aktarın

1. İşiniz bittiğinde, koleksiyonunuzu bir JSON dosyasına aktarın.

   ![Koleksiyonu dışarı aktarma](./media/custom-connector-api-postman-collection/08-export-http-request.png)

2. Seçin **koleksiyonu v1** verme biçimi ve JSON dosyasını kaydetmek istediğiniz konuma gözatın.

   > [!NOTE]
   > Şu anda yalnızca V1 özel bağlayıcıları için çalışır.

   ![Verme biçimini seçin: "Koleksiyon v1"](./media/custom-connector-api-postman-collection/09-export-format.png)
   
Şimdi, özel API'leri ve bağlayıcıları oluşturmak için bu Postman koleksiyonunu kullanabilirsiniz. Koleksiyon verdikten sonra JSON dosyası Logic Apps, akış veya PowerApps aktarabilirsiniz.

> [!IMPORTANT]
> Postman koleksiyondan özel bir bağlayıcı oluşturuyorsanız, kaldırdığınızdan emin olun `Content-type` eylemleri ve Tetikleyicileri başlığından. Hedef hizmet, örneğin, akış, otomatik olarak bu üstbilgisi ekler. Ayrıca, kimlik doğrulama üstbilgileri kaldırın `securityDefintions` bölümünden, eylemleri ve tetikler.

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps: özel bağlayıcılar kaydetme](../logic-apps/logic-apps-custom-connector-register.md)
* [Akış: Bağlayıcınızı kaydetme](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)
* [PowerApps: Bağlayıcınızı kaydetme](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector)
* [Özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)
