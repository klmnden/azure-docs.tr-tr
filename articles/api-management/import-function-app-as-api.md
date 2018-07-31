---
title: Bir Azure İşlevleri uygulamasını Azure portalda API olarak içeri aktarma | Microsoft Docs
description: Bu öğreticide, Azure API Management’ı kullanarak bir Azure İşlevleri uygulamasını API olarak içeri aktarmayı öğreneceksiniz.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 670fa58de7155028b0f72f1f819b9f269e07b9eb
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239061"
---
# <a name="import-an-azure-functions-app-as-an-api"></a>Azure İşlevleri uygulamasını API olarak içeri aktarma

Bu makalede bir Azure İşlevleri uygulamasını API olarak içeri aktarma adımları gösterilmektedir. Makalede ayrıca Azure API Management API'sinin nasıl test edileceği de anlatılmaktadır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure İşlevleri uygulamasını API olarak içeri aktarma
> * Azure portalında API’yi test etme
> * API'yi geliştirici portalında test etme

## <a name="prerequisites"></a>Ön koşullar

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md) hızlı başlangıcını tamamlayın.
+ Aboneliğinizde bir Azure İşlevleri uygulaması bulunduğundan emin olun. Daha fazla bilgi için bkz. [Azure İşlevleri uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app).
+ Azure İşlevleri uygulamanızın [OpenAPI tanımını oluşturun](../azure-functions/functions-openapi-definition.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Arka uç API'sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünde **API'ler** öğesini seçin.
2. **Yeni API ekleyin** listesinde **İşlevler Uygulaması**'nı seçin.

    ![İşlevler uygulaması](./media/import-function-app-as-api/function-app-api.png)
3. Aboneliğinizdeki İşlevler uygulamalarının listesini görmek için **Göz at**’ı seçin.
4. Uygulamayı seçin. API Management, seçili uygulamayla ilişkili Swagger’ı bulur, getirir ve içeri aktarır. 
5. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu API Management örneğinde tanımlayan bir addır. Sonek bu API Management örneğinde benzersiz olmalıdır.
6. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda, **Sınırsız** ürünü kullanılır. API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız, API'yi bir ürüne ekleyin. API'yi ürüne ekleme işlemini API'yi oluştururken veya daha sonra gerçekleştirebilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Birden fazla API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Bir geliştirici abone olduğunda ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinir. API Management örneğini siz oluşturduysanız yönetici olursunuz. Yöneticiler varsayılan olarak tüm ürünlere abone olur.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
7. **Oluştur**’u seçin.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Azure API Management’ı Azure İşlevleri anahtarlarıyla doldurma

İçeri aktarılan Azure İşlevleri uygulamaları anahtarlarla korunuyorsa API Management otomatik olarak bu işlevler için *adlandırılmış değerler* oluşturur. API Management ancak girdileri gizli dizilerle doldurmaz. Aşağıdaki adımları her giriş için tekrarlayın:  

1. API Management örneğinde **Adlandırılmış değerler** sekmesine gidin.
2. Girdiyi seçin ve kenar çubuğundan **Değeri göster**’i seçin.

    ![Adlandırılmış değerler](./media/import-function-app-as-api/apim-named-values.png)

3. **Değer** kutusunda görüntülenen metin **\<Azure İşlevleri adı\> kodu** şeklindeyse **İşlevler Uygulamaları**'na ve ardından **İşlevler**'e gidin.
4. **Yönet**'e tıklayın ve ardından uygulamanızın kimlik doğrulama yöntemine göre ilgili anahtarı kopyalayın.

    ![İşlevler uygulaması - Anahtarları kopyalama](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Anahtarı **Değer** kutusuna yapıştırın ve **Kaydet**'i seçin.

    ![İşlevler uygulaması - Anahtar değerleri yapıştırma](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-api-management-api-in-the-azure-portal"></a>Yeni API Management API'sini Azure portalda test etme

İşlemleri doğrudan Azure portaldan çağırabilirsiniz. Azure portalı kullanarak bir API'nin işlemlerini kolayca görüntüleyebilir ve test edebilirsiniz.  

1. Bir önceki bölümde oluşturduğunuz API'yi seçin.
2. **Test** sekmesini seçin.
3. Bir işlem seçin.

    Sayfa, sorgu parametrelerinin ve üst bilgilerin alanlarını görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri **Ocp-Apim-Subscription-Key** üst bilgisidir. API Management örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur. 
4. **Gönder**’i seçin.

    Arka uç **200 OK** şeklinde ve bazı verilerle yanıt verir.

## <a name="call-operation"></a>Geliştirici portalından işlem çağırma

API’leri test etmek için geliştirici portalından da işlem çağırabilirsiniz. 

1. [Arka uç API’sini içeri aktarma ve yayımlama](#create-api) bölümünde oluşturduğunuz API’yi seçin.
2. **Geliştirici portalı**'nı seçin.

    Geliştirici portalı sitesi açılır.
3. Oluşturduğunuz **API**’yi seçin.
4. Test etmek istediğiniz işlemi seçin.
5. **Deneyin**'i seçin.
6. **Gönder**’i seçin.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)