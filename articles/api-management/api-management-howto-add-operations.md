---
title: "Azure API Management'te bir API'ye işlem ekleme | Microsoft Docs"
description: "Azure API Management'te bir API işlemleri eklemeyi öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 8b047c0826590d1cb6a79a2f14ca07764dc2b409
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Azure API Management'te bir API'ye işlem ekleme
Bir API API Management kullanmadan önce operations eklenmesi gerekir. Bu kılavuz, ekleme ve farklı bir API işlemlerinin API Management'te yapılandırma gösterilmektedir.

## <a name="add-operation"></a>Bir işlem ekleme
Operations eklenir ve bir API yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

İstenen API yayımcı Portalı'nda seçin ve ardından **Operations** sekmesi. 

![İşlemler][api-management-operations]

Tıklatın **ekleme işlemi** yeni bir işlem eklemek için. **Yeni işlem** görüntülenir ve **imza** sekmesi varsayılan olarak seçilir.

![Ekleme işlemi][api-management-add-operation]

Belirtin **HTTP fiili** aşağı açılan listeden seçerek.

![HTTP yöntemi][api-management-http-method]

<a name="url-template"></a>

URL şablonu, bir veya daha fazla URL yolu kesimleri ve sıfır veya daha fazla sorgu dizesi parametreleri oluşan bir URL parçadaki yazarak tanımlayın. API temel URL'sini eklenmiş URL şablon, tek bir HTTP işlemi tanımlar. Daha fazla küme ayraçları tanımlanan değişken bölümleri adlı veya birini içerebilir. Bu değişken bölümleri şablon parametreleri olarak adlandırılır ve istek API Yönetimi platformunuz tarafından işlendiğinde isteğin URL'den ayıklanan değerler dinamik olarak atanır.

> URL şablon joker karakter düzenleri içerebilir. Örneğin, belirten `/*` geri, HTTP yöntemine yönelik tüm istekleri hizmet İleri sona erer.

![URL şablonu][api-management-url-template]

<a name="rewrite-url-template"></a>

İsterseniz, belirtin **URL yeniden yazma şablon**. Bu, dönüştürülmüş bir URL yeniden yazma şablon göre aracılığıyla arka uç çağrılırken ön uç üzerinde gelen istekleri işlemek için standart URL şablonu kullanmanıza olanak sağlar. Şablon parametreleri URL şablondan yeniden yazma şablonunda kullanılmalıdır. Aşağıdaki örnek, önceki örnekten web hizmeti yol kesimi, URL şablonları kullanarak API Management platform yayımlanan API bir sorgu parametresi olarak sağlanabilir olarak kodlanmış nasıl içerik türünü gösterir.

![URL şablonu yeniden yazma][api-management-url-template-rewrite]

İşlemi arayanlara biçimini kullanacak `/customers?customerid=ALFKI` ve bu öğesine eşlenecek `/Customers('ALFKI')` arka uç hizmetine zaman çağrılır.

**Görüntü** adı ve **açıklama** işlemi bir açıklama belirtin ve Geliştirici Portalı'nda bu API kullanan geliştiriciler için belgeleri sağlamak için kullanılır.

![Açıklama][api-management-description]

Düz metin veya HTML olarak işlemi açıklama belirtilen **açıklama** metin kutusu.

## <a name="operation-caching"></a>İşlemi önbelleğe alma
Yanıt önbelleğe alma API Tüketiciler, düşürmeye bant genişliği kullanımını ve HTTP web üzerindeki yükü hizmet API uygulama düşüşleri tarafından algılanan gecikmesini azaltır. 

Hızla ve kolayca işlem için önbelleğe almayı etkinleştirmek için seçin **önbelleğe alma** sekmesinde ve denetleme **etkinleştirmek** onay kutusu.

![Önbelleğe alma][api-management-caching-tab]

**Süre** işlemi yanıt sırasında önbellekte kalan süreyi belirtir. Varsayılan değer 3600 saniye veya 1 saat olur.

Önbellek anahtarlar, böylece her farklı önbellek anahtarına karşılık gelen yanıt kendi ayrı önbelleğe alınan değeri alır yanıtlar arasında ayırt etmek için kullanılır. İsteğe bağlı olarak, özel bir sorgu dizesi parametreleri ve/veya önbellek anahtar değerlerini bilgisayar kullanılacak HTTP üstbilgileri girin **sorgu dizesi parametrelerine göre değişiklik gösterebilir** ve **üstbilgileri göre değişiklik gösterebilir** metin kutuları sırasıyla. Ne zaman belirtilen, tam istek URL'si yok ve önbellek anahtar oluşturma aşağıdaki HTTP üst bilgisi değerler kullanılır: **kabul** ve **Accept-Charset**.

> Önbelleğe alma ve önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz: [sonuçları işlemi önbelleğe almak nasıl Azure API Management'te][How to cache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Parametreleri
İşlem parametrelerini parametreleri sekmesinde yönetilir. Belirtilen parametreleri **URL şablonu** üzerinde **imza** sekmesinde otomatik olarak eklenir ve yalnızca URL şablonu düzenleyerek değiştirilebilir. Ek parametreler el ile girilebilir.

Yeni bir sorgu parametresi eklemek için tıklatın **sorgu parametresi Ekle** ve aşağıdaki bilgileri girin:

* **Ad** -parametre adı.
* **Açıklama** -(isteğe bağlı) parametre kısa bir açıklaması.
* **Tür** -aşağı açılan seçilen parametre türü.
* **Değerleri** -bu parametresine atanan değer. Değerlerden biri varsayılan olarak (isteğe bağlı) işaretlenebilir.
* **Gerekli** -onay kutusunu işaretleyerek parametresi zorunlu kılabilir. 

![İstek parametreleri][api-management-request-parameters]

## <a name="request-body"></a>İstek gövdesinde
İşlem izin veriyorsa (örneğin, PUT, POST) ve tüm desteklenen gösterimi biçimlerinin (örneğin json, XML) içinde örneği sağlayabilir gövde gerektirir. 

> İstek gövdesi doğrulanmazsa ve yalnızca belgeleri amacıyla kullanılır.
> 
> 

Bir istek gövdesi girmek için geçiş **gövde** sekmesi.

Tıklatın **eklemek gösterimi**, istenen içerik türü adı (örn. uygulama/json) yazmaya başlayın, açılan seçin ve istenen istek gövdesi örnek seçili biçiminde metin kutusuna yapıştırın. 

![İstek gövdesi][api-management-request-body]

Ek gösterimler için ayrıca bir isteğe bağlı metin açıklaması belirtebilirsiniz **açıklama** metin kutusu.

## <a name="responses"></a>Yanıtları
İşlemi üretebilir için tüm durum kodları örnekleri yanıtların sağlamak için iyi bir uygulamadır. Her durum kodu birden fazla yanıt gövdesi örnek, desteklenen içerik türlerinin her biri için bir tane olabilir. 

Bir yanıt eklemek için tıklatın **Ekle** ve istenen durum kodu yazmaya başlayın. Bu örnekte durum kodudur **200 Tamam**. Kod açılan görüntülendikten sonra seçmek ve yanıt kodu oluşturulur ve işleminiz için eklendi.

![Yanıt kodu][api-management-response-code]

Tıklatın **eklemek gösterimi**, istenen içerik türü adı (örn. uygulama/json) yazmaya başlayın ve ardından aşağı açılan seçin.

![Gövde içerik türü][api-management-response-body-content-type]

Yanıt gövdesi örnek seçili biçiminde metin kutusuna yapıştırın. 

![Yanıt gövdesi][api-management-response-body]

İsterseniz, isteğe bağlı bir açıklama ekleyin **açıklama** metin kutusu.

İşlemi yapılandırıldıktan sonra tıklatın **kaydetmek**.

## <a name="next-steps"> </a>Sonraki adımlar
Operations API'ye eklendikten sonra sonraki API bir ürünle ilişkilendirin ve böylece geliştiriciler işlemlerini çağırabilirsiniz yayımlamak için adımdır.

* [Oluşturma ve bir ürün yayımlama][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
