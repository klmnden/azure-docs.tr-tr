---
title: Azure API Management'ta dış bir önbellek kullanma | Microsoft Docs
description: Yapılandırma ve Azure API Management'ta dış bir önbellek kullanma hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: apimpm
ms.openlocfilehash: e2362d06fa0ef795122a2d47a7a621b66fdd9470
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780350"
---
# <a name="use-an-external-azure-cache-for-redis-in-azure-api-management"></a>Azure API Management'ta Redis için bir dış Azure önbellek kullanma

Yerleşik önbelleği kullanan ek olarak, Azure API Management ayrıca yanıtlarını dış bir Azure önbelleği için Redis önbelleğe almak için sağlar.

Dış bir önbellek kullanmak, bazı sınırlamalar yerleşik bir önbellek üstesinden gelmek için sağlar. İsteyip istemediğini özellikle yararlıdır:

* API Management güncelleştirmeleri sırasında düzenli aralıklarla temizlenmiş önbelleğinizi olmamasına özen gösterin
* Önbelleği yapılandırma hakkında daha fazla denetime sahip
* API Management katmanınız için izin verdiğinden daha fazla veri önbelleği
* API Management tüketim katmanı ile önbelleğe almayı kullanma

Önbelleğe alma hakkında daha ayrıntılı bilgi için bkz. [API Management önbelleğe alma ilkeleri](api-management-caching-policies.md) ve [Azure API Management'te özel önbelleğe alma](api-management-sample-cache-by-key.md).

![APIM için kendi önbelleğinizi Getir](media/api-management-howto-cache-external/overview.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * API Yönetimi'nde bir dış önbellek Ekle

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ Anlamak [Azure API Yönetimi'nde önbelleğe alma](api-management-howto-cache.md)

## <a name="create-cache"> </a> Azure önbelleği için Redis oluşturma

Bu bölümde, Azure Redis için bir Azure önbelleği oluşturma açıklanmaktadır. Azure Cache, Redis içinde zaten veya Azure dışında <a href="#add-external-cache">atla</a> sonraki bölüme.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Bir dış önbellek Ekle

Dış bir Azure önbelleği için Redis Azure API Management'ta eklemek için aşağıdaki adımları izleyin.

![APIM için kendi önbelleğinizi Getir](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> **Kullanmak** ayar bölgesel dağıtım iletişim hangi API Management API yönetimi, çok bölgeli yapılandırmasını durumunda yapılandırılmış önbellek ile belirtir. Belirtilen önbellek **varsayılan** önbelleklerle bölgesel bir değer tarafından geçersiz kılınır.
>
> Örneğin, API Management, Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde barındırılır ve varsa iki önbellekler yapılandırılmış, biri **varsayılan** , diğeri **Güneydoğu Asya**, API Yönetimi'nde  **Güneydoğu Asya** diğer iki bölgeleri kullanırken, kendi önbellek kullanacağı **varsayılan** önbellek girişi.

### <a name="add-an-azure-cache-for-redis-from-the-same-subscription"></a>Bir Azure önbelleği için Redis aynı abonelikten ekleyin.

1. API Management örneğinizin Azure portalındaki göz atın.
2. Seçin **dış önbellek** sol taraftaki menüden sekmesi.
3. Tıklayın **+ Ekle** düğmesi.
4. Önbelleğinize seçin **önbellek örneği** alan açılır.
5. Seçin **varsayılan** veya istenen bölgede belirtin **kullanmak** alan açılır.
6. **Kaydet**’e tıklayın.

### <a name="add-an-azure-cache-for-redis-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Bir Azure önbelleği için Redis genel olarak geçerli bir Azure aboneliği veya Azure dışında barındırılan Ekle

1. API Management örneğinizin Azure portalındaki göz atın.
2. Seçin **dış önbellek** sol taraftaki menüden sekmesi.
3. Tıklayın **+ Ekle** düğmesi.
4. Seçin **özel** içinde **önbellek örneği** alan açılır.
5. Seçin **varsayılan** veya istenen bölgede belirtin **kullanmak** alan açılır.
6. Azure Cache, Redis bağlantı dizesi olarak sağlamak **bağlantı dizesi** alan.
7. **Kaydet**’e tıklayın.

## <a name="use-the-external-cache"></a>Dış önbellek kullanma

Dış önbellek, Azure API Yönetimi'nde yapılandırıldıktan sonra önbelleğe alma ilkeleri aracılığıyla kullanılabilir. Bkz: [Azure API Management performansını artırmak için önbelleğe alma ekleme](api-management-howto-cache.md) ayrıntılı adımlar için.

## <a name="next-steps"> </a>Sonraki adımlar

* Önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz. [API Management ilke başvurusunda][API Management policy reference] [Önbelleğe alma ilkeleri][Caching policies].
* Anahtar kullanım ilkesi ifadeleri hakkında daha fazla bilgi için bkz. [Azure API Management’te özel önbelleğe alma](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
