---
title: Azure Content Moderator - Content Moderator kimlik bilgilerini yönetme
titlesuffix: Azure Cognitive Services
description: API'leri kullanmanız gerekecektir Content Moderator kimlik bilgilerini yönetin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0da6b6b0fef0f998e20789253b2a65c54121532c
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260018"
---
# <a name="manage-content-moderator-service-credentials"></a>Content Moderator hizmet kimlik bilgilerini yönetme

Content Moderator kimlik bilgilerinizi aşağıdaki konumlarda oluşturulur:

- [Azure portalı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/)

Bu makalede, bunları nerede bulacağını ve birbirleriyle nasıl ilişki kuracağını açıklanmaktadır.

## <a name="the-azure-portal"></a>Azure portal

Azure portal panosunda Content Moderator hesabınızı seçin. Altında **kaynak yönetimi**seçin **anahtarları**. Anahtarı kopyalamak için sağ anahtarının simgesini seçin.

![Azure portalında Content Moderator anahtarları](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Azure hesabı ile İnceleme aracını kullanın ve API gözden geçirin
Azure anahtarınızı İnceleme API'lerini kullanmak için listelenen kaynak kimliği kopyalayın **özellikleri** ekran aşağıdaki ekran görüntüsünde ve gözden geçirme Aracı'nın kimlik bilgilerini ekranda girin **beyaz listeye kaynak kimlikleri** aşağıda gösterildiği gibi alanları **kaynak kimliği** bölümü. 

> [!NOTE]
> Content Moderator aboneliğinizin bölge takımınızın tanıması ve takım verilere erişmek için gözden geçirme takımın bölge eşleşmelidir. Örneğin, bu sayfada, resimlerdeki **Batı ABD** bölge **(4)** Content Moderator Azure aboneliği ve gözden geçirme takımınızın içerir.
>
> Sonra iki yerde gözden geçirme aracı kaynak kimliği ve anahtarı ile Azure aboneliğinizden değiştirin, **deneme Ocp-Apim-Subscription-Key** ekran artık kullanılmamaktadır ancak her zaman kullanılabilir kimlik bilgileri görüntülenir.
> Deneme anahtarı en fazla 5000 işlem / ay (RP'ler) saniyede 1 isteğinde sınırlar.

![Azure portalında Content Moderator kaynak kimliği](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Gözden geçirme aracı iş akışları ile Azure hesabını kullanma

Content Moderator içinde kullanılabilir iş akışları için Azure anahtarınızı kullanmak için bu alana giriş **Ocp-Apim-Subscription-Key** alanındaki **iş akışı ayarları** bölümünde aşağıda gösterildiği gibi  **İş akışları** bölümü. İsabet **'+'** kaynak kimliğinizi kaydetmek için

![Content Moderator iş akışı kimlik bilgilerini gözden geçirme aracı](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>Gözden geçirme aracı

Panoyu gözden geçirme aracındaki **ayarları** sekmesinde **kimlik bilgilerini**.

![Content Moderator İnceleme aracı kimlik bilgileri](images/credentials-trial-resource-workflow.PNG)

Aşağıdaki bölümde, önceki resimde daha ayrıntılı bir şekilde inceler:

### <a name="api"></a>API

İlk bölümü listeler, **API uç noktası gözden**, **kimliği takım**ve **Ocp-Apim-Subscription-Key (Content Moderator deneme anahtarı)** gözden geçirme takımınızın kapsamında oluşturulan oluşturma. Tüm Content Moderator gözden geçirme API dahil olmak üzere API'leri, çağırmak için bunları kullanın.

Ayrıca, bölge tanımlayıcısı API uç noktanız için unutmayın. Örneğin, **westus** bölgede "https:\//westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Content Moderator İnceleme aracı anahtarı](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Kaynak kimliği

Bu alan kümesi önceki bölümde ele alınmıştır [gözden geçirme aracı ve API'si ile Azure hesabı kullanmak](credentials.md#use-the-azure-account-with-the-review-tool-and-review-api). Bu alan için önceki bölümde açıklandığı gibi Azure kaynak kimliği eklemediğiniz sürece bu genellikle boş bir alandır.

### <a name="workflows"></a>İş akışları

Bu alan kümesi önceki bölümde ele alınmıştır [gözden geçirme aracı iş akışları ile Azure hesabını kullanma](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool). Varsayılan olarak, iş akışlarını çalıştırmak için otomatik olarak oluşturulan deneme anahtarıyla gözden geçirme Aracı'nı kullanır, ve hangi başlangıç olarak gösterilir. Terim ve görüntü listeleri ekran metin ve resim değerlendirmek işlemlerinde içinse diğer iki alanı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Content Moderator kimlik bilgilerini kullanmayı öğrenin, [iş akışları](workflows.md).
