---
title: Azure kimlik bilgilerini içerik denetleyici | Microsoft Docs
description: API'leri ile kullanılacak içerik yönetici kimlik bilgilerini yönetin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 4531fa4c8bbb7bb9c1daeaaac6f9e7078dae250a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351520"
---
# <a name="manage-credentials"></a>Kimlik bilgilerini yönetme

İçerik yönetici kimlik bilgilerinizi aşağıdaki konumlarda oluşturulur:

- [Azure portalı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [İçerik denetleyici gözden geçirme aracı](http://contentmoderator.cognitive.microsoft.com/)

Bu makalede, bunları nerede bulacağını ve birbirleriyle nasıl ilişkili olduğunu açıklar.

## <a name="the-azure-portal"></a>Azure portalı

Azure portal panosunda içerik denetleyici hesabınızı seçin. Altında **kaynak yönetimi**seçin **anahtarları**. Anahtarı kopyalamak için anahtar sağındaki simgeyi seçin.

![Azure portalında yönetici anahtarları içerik](images/credentials-azure-portal-keys.PNG)

### <a name="how-to-use-your-azure-account-with-the-review-tool"></a>Azure hesabınız için İnceleme aracı ile kullanma
Gözden geçirme API'leri Azure anahtarınızı kullanmak için listelenen kaynak kimliği kopyalama **özellikleri** ekran aşağıdaki ekran görüntüsünde ve gözden geçirme aracın kimlik bilgilerini ekranda girin **Güvenilenler listesine kaynak Kimlikleri'ni** alanları aşağıda gösterildiği gibi **kaynak kimliği** bölümü. 

İçinde içerik denetleyici kullanılabilir iş akışları için Azure anahtarınızı kullanmak için bu alana giriş **Apim abonelik anahtar Ocp** alanındaki **iş akışı ayarları** bölümünde aşağıda gösterildiği gibi  **İş akışları** bölümü.

> [!NOTE]
> Gözden geçirme Aracı'nı iki yerde anahtarı ve kaynak kimliği ile Azure aboneliğinizden değiştirdiğiniz sonra **deneme Ocp-Apim-Subscription-Key** ekran artık kullanılır, ancak her zaman kullanılabilir kimlik bilgisi görüntülenir.
> Deneme anahtarı her ay 1 isteği / saniye (RPS) en fazla 5000 işlemleri için sınırlar.

![Azure portalında içerik denetleyici kaynak kimliği](images/credentials-azure-portal-resourceid.PNG)


## <a name="the-review-tool"></a>Gözden geçirme aracı

Pano, gözden geçirme aracındaki **ayarları** sekmesine **kimlik bilgileri**.

![Gözden geçirme aracında içerik yönetici kimlik bilgileri](images/credentials-trial-resource-workflow.PNG)

Aşağıdaki bölümde daha ayrıntılı önceki görüntüde inceler:


### <a name="api"></a>API

İlk bölümü listeleri, **API uç noktası gözden**, **kimliği ekip**ve **Apim Ocp için abonelik anahtar (içerik denetleyici deneme anahtarı)** gözden geçirme ekibinizin bir parçası olarak oluşturulan oluşturma. Tüm içerik denetleyici gözden geçirme API dahil olmak üzere API'leri çağırmak için bunları kullanın.

Ayrıca, bölge tanımlayıcısı API uç noktanız için unutmayın. Örneğin, **westus** bölgede "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Gözden geçirme aracında içerik yönetici anahtarı](images/credentials-trialkey.PNG)


### <a name="resource-id"></a>Kaynak kimliği

Hiçbir kaynak kimliği ile boş olarak ikinci bölümü başlatır çıkışı **Azure abonelik anahtarınızı gözden geçirme API ile kullanmak için daha önce gösterildiği gibi kaynak kimliği ekranına gidin ve gösterilen alana kopyalayın**. İsabet **'+'** kaynak kimliğinizi kaydetmek için

> [!NOTE]
> İçerik denetleyici aboneliğinizin bölge ekibinizin algılar ve takım verilere erişmek için gözden geçirme takımın bölge eşleşmelidir. Örneğin, bu sayfada görüntülerinde **Batı ABD** bölge **(4)** içerik denetleyici Azure aboneliği ve gözden geçirme ekibinizin içerir.

![Gözden geçirme aracında içerik denetleyici kaynak kimliği](images/credentials-resourceids.PNG)


### <a name="workflows"></a>İş akışları

Üçüncü bölümü, iş akışlarını çalıştırmak için kullanılan bilgileri gösterir. Varsayılan olarak otomatik olarak oluşturulan deneme anahtarı gösteren çıkışı başlar. 

**Bir Azure aboneliği edinme Azure anahtarınızla güncelleştirilmesini**. Diğer iki alanın sırasıyla ekran metin ve resim değerlendirmek işlemlerinde terim ve resim listeleri kullanma izin verin.

![İçerik yönetici iş akışı kimlik bilgilerini gözden geçirme aracı](images/credentials-workflow.PNG)


## <a name="next-steps"></a>Sonraki adımlar

* İçerik yönetici kimlik bilgilerini kullanmayı öğrenin, [iş akışları](workflows.md).
