---
title: HALUK içinde tuşlarınızı yönetme | Microsoft Docs
description: Dil anlama (HALUK) programlı API, uç noktayı ve dış anahtarları yönetmek için kullanın.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: b1d0d01621769a6bfc0e14e703d7f988403703c6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355745"
---
# <a name="manage-your-luis-keys"></a>HALUK tuşlarınızı yönetme
Bir anahtar, yazar ve HALUK uygulamanızı yayınlama veya uç noktanızı sorgu olanak sağlar. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Önemli kavramlar
Bkz: [HALUK anahtarlarında](luis-concept-keys.md) HALUK yazma ve uç nokta temel kavramları anlamanız için.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Uç noktası anahtarı atama
Üzerinde **Yayımla uygulama** sayfasında, bir anahtar zaten var. **kaynakları ve anahtarları** tablo. Bu yazma (Başlangıç) anahtarıdır. 

1. HALUK anahtar oluşturma [Azure portal](https://portal.azure.com). Daha ayrıntılı yönergeler için bkz: [Azure kullanarak bir abonelik anahtarı oluşturma](luis-how-to-azure-subscription.md).
 
2. Önceki adımda oluşturduğunuz HALUK anahtarı eklemek için tıklatın **anahtar Ekle** açmak için düğmeye **uygulamanız için bir anahtar atama** iletişim. 

    ![Uygulamanız için bir anahtar atama](./media/luis-manage-keys/assign-key.png)
3. İletişim kutusunda bir kiracı seçin. 
 
    > [!Note]
    > Azure'da, bir kiracı istemcisi veya bir hizmetle ilişkili kuruluşun Azure Active Directory kimliği temsil eder. Daha önce Azure aboneliği için tek tek Microsoft Account kaydolduysanız, bir kiracı zaten var! Azure portalında oturum açtığınızda, otomatik olarak oturum açmış [varsayılan Kiracı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). Bu Kiracı kullanabilirsiniz, ancak bir kuruluş yöneticisi hesabı oluşturmak isteyebilirsiniz.

4. Eklemek istediğiniz Azure HALUK anahtar ile ilişkili Azure abonelik seçin.

5. Azure HALUK hesabı seçin. Hesap bölge parantez içinde görüntülenir. 

    ![Anahtarı seçin](./media/luis-manage-keys/assign-key-filled-out.png)

6. Bu uç noktası anahtarı atadıktan sonra tüm uç nokta sorgularda kullanın. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Yayımlama bölgeleri
Yayımlama hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md) Yayımlama özelliği de dahil olmak üzere [Avrupa](luis-reference-regions.md#publishing-to-europe), ve [Avustralya](luis-reference-regions.md#publishing-to-australia). Yayımlama bölgeler bölgeler yazma farklıdır. Uygulama geliştirme bölge istediğiniz yayımlama bölge ilgili oluşturduğunuzdan emin olun.

## <a name="unassign-key"></a>Anahtar atamayı kaldırma

* İçinde **kaynakları ve anahtarlar listesi**, atamasını kaldırmak istediğiniz varlık yanındaki çöp kutusu simgesine tıklayın. Ardından **Tamam** silme işlemini onaylamak için onay iletisi.
 
    ![Varlık atamayı kaldırma](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> HALUK anahtar atamayı kaldırma Azure aboneliğinizden silmez.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı yayımlamak için anahtarınızı kullanma **Yayımla uygulama** sayfası. Yayımlama hakkında yönergeler için bkz: [Yayımla uygulama](PublishApp.md).

[LUIS]: luis-reference-regions.md#luis-website