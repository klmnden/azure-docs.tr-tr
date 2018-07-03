---
title: LUIS anahtarlarınızı yönetme | Microsoft Docs
description: Language Understanding (LUIS), programlı bir API uç noktası ve dış anahtarları yönetmek için kullanın.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 80064584468c109d0bad49f1a22c485fa9cce846
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344527"
---
# <a name="manage-your-luis-keys"></a>LUIS anahtarlarınızı yönetme
Bir anahtar, yazar ve LUIS uygulamanızı yayımlayın veya uç noktanızı sorgu olanak tanır. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Önemli kavramlar
Bkz: [LUIS anahtarlarında](luis-concept-keys.md) LUIS yazma ve uç nokta temel kavramları anlamak için.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Uç noktası anahtarı atama
Üzerinde **uygulamayı Yayımla** sayfasında, bir anahtar zaten var. **kaynakları ve anahtarları** tablo. Bu yazma (Başlangıç) anahtarıdır. 

1. Bir LUIS anahtar oluşturmak [Azure portalında](https://portal.azure.com). Daha fazla bilgi için bkz. [Azure'ı kullanarak bir uç noktası anahtarı oluşturma](luis-how-to-azure-subscription.md).
 
2. Önceki adımda oluşturulan LUIS anahtarı eklemek için tıklatın **anahtarı Ekle** açmak için düğmeyi **uygulamanız için bir anahtar atama** iletişim. 

    ![Uygulamanız için bir anahtar atama](./media/luis-manage-keys/assign-key.png)
3. İletişim kutusunda bir kiracı seçin. 
 
    > [!Note]
    > Azure'da, bir kiracı, istemci veya kuruluş hizmeti ile ilişkili Azure Active Directory Kimliğini temsil eder. Daha önce bir Azure aboneliği için tek tek Microsoft Account kaydolduysanız kiracınız zaten mevcuttur! Azure portalında oturum açtığınızda, otomatik olarak oturum açtığınız [varsayılan kiracınızda](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). Bu kiracıyı kullanın ücretsizdir, ancak bir kuruluş yöneticisi hesabı oluşturmak isteyebilirsiniz.

4. Eklemek istediğiniz Azure LUIS anahtar ile ilişkili Azure aboneliği seçin.

5. Azure LUIS hesabı seçin. Hesap bölgesi parantez içinde görüntülenir. 

    ![Tuşuna basın](./media/luis-manage-keys/assign-key-filled-out.png)

6. Bu uç noktası anahtarı atadıktan sonra tüm uç nokta sorguları kullanın. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Yayımlama bölgeleri
Yayımlama hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md) yayımlama çalıştırmasına dahil olmak üzere [Avrupa](luis-reference-regions.md#publishing-to-europe), ve [Avustralya](luis-reference-regions.md#publishing-to-australia). Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesi istediğiniz yayımlama bölgesiyle ilgili uygulama oluşturduğunuzdan emin olun.

## <a name="unassign-key"></a>Anahtar atamasını Kaldır

* İçinde **kaynakları ve anahtarlar listesi**, atamasını kaldırmak istediğiniz varlığı yanındaki çöp kutusu simgesine tıklayın. ' A tıklayarak **Tamam** silme işlemini onaylamak için onay iletisi.
 
    ![Varlık atamasını Kaldır](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> LUIS anahtar atamayı kaldırma Azure aboneliğinizden silmez.

## <a name="next-steps"></a>Sonraki adımlar

Anahtarınızı uygulamanızda yayımlayacağınızı **uygulamayı Yayımla** sayfası. Yayımlama hakkında yönergeler için bkz: [uygulamayı Yayımla](luis-how-to-publish-app.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website