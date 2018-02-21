---
title: "Azure API Management örneği için bir özel etki alanı adı yapılandırma | Microsoft Docs"
description: "Bu konuda, Azure API Management örneği için bir özel etki alanı adı yapılandırma açıklanmaktadır."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 12/14/2017
ms.author: apimpm
ms.openlocfilehash: 96e233a26af95d4373323867046ca01fe1304608
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-custom-domain-name"></a>Özel bir etki alanı adı yapılandırma 

Azure API Management (APIM) örneği oluşturduğunuzda, bir alt etki alanı adını azure api.net için atar (örneğin, `apim-service-name.azure-api.net`). Ancak, kendi etki alanı adı gibi kullanarak APIM noktalarınızı getirebilir **contoso.com**. Bu öğretici bir Azure API Management örneği tarafından kullanıma sunulan uç noktaları var olan bir özel DNS adını eşleştirmek nasıl gösterir.

> [!WARNING]
> Sertifika sabitleme uygulamalarını güvenliğini artırmak için kullanmak istediğiniz müşteriler, özel etki alanı adı kullanmalıdır > ve yönetmek, varsayılan sertifika sertifikası. Varsayılan Sertifika sabitleme müşteriler yerine olacaktır > sıkı bir bağımlılık bunlar yok denetlemek, sertifika özelliklerini, önerilen bir uygulama değil sürüyor.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Size ait bir özel etki alanı adı. Kullanmak istediğiniz özel etki alanı adını ayrı ayrı temin ve bir DNS sunucusunda barındırılan. Bu konu, ana bilgisayar bir özel etki alanı adı nasıl yönergeler sağlamaz.
+ Bir ortak ve özel anahtarı olan geçerli bir sertifikası olması gerekir (. PFX). Konu veya konu alternatif adı (SAN) (Bu URL'leri SSL üzerinden güvenli bir şekilde kullanıma sunmak APIM sağlar) etki alanı adı ile eşleşmesi gerekir.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Özel etki alanı adı ayarlamak için Azure portalını kullanın

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **özel etki alanları ve SSL**.
    
    Özel etki alanı atayabilirsiniz uç noktalarının sayısını yoktur. Şu anda şu uç noktalar bulunmaktadır: 
    + **Proxy** (varsayılan: `<apim-service-name>.azure-api.net`), 
    + **Portal** (varsayılan: `<apim-service-name>.portal.azure-api.net`),     
    + **Yönetim** (varsayılan: `<apim-service-name>.management.azure-api.net`), 
    + **SCM** (varsayılan: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > Uç noktaları tümünün veya bazılarının güncelleştirebilirsiniz. Genellikle, müşteriler güncelleştirme **Proxy** (Bu URL'yi API Yönetimi kullanıma sunulan API'sini çağırmak için kullanılır) ve **Portal** (Geliştirici Portalı URL). **Yönetim** ve **SCM** uç noktaları APIM müşteriler tarafından dahili olarak kullanılır ve bu nedenle bir özel etki alanı adı daha az sıklıkta atanır.
3. Güncelleştirmek istediğiniz uç nokta seçin. 
4. Sağdaki penceresinde **özel**.

    + İçinde **özel etki alanı adı**, kullanmak istediğiniz adı belirtin. Örneğin, `api.contoso.com`. <br/>Joker karakter etki alanı adları (örneğin, *. alanı.com) de desteklenir.
    + İçinde **sertifika**, geçerli bir belirtin. Karşıya yüklemek istediğiniz PFX dosyası. 
    + Sertifika bir parolası varsa, bu alana giriş **parola** alan.
1. Uygula'yı tıklatın.

    >[!NOTE]
    >Sertifika atama işleminin 15 dakika veya dağıtım boyutuna bağlı olarak daha fazla sürebilir. Geliştirici SKU kapalı kalma süresi varsa, temel ve daha yüksek SKU's kapalı kalma süresi gerekmez.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Yükseltme ve hizmetinizi ölçeklendirme](upgrade-and-scale.md)
