---
title: Azure API Management örneğinizin için bir özel etki alanı adı yapılandırma | Microsoft Docs
description: Bu konuda bir özel etki alanı adını Azure API Management örneğinizin açıklar.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 05/29/2019
ms.author: apimpm
ms.openlocfilehash: a8bfa7c5baa316b4019480bfc146b6cc61eff979
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66141294"
---
# <a name="configure-a-custom-domain-name"></a>Özel bir etki alanı adı yapılandırma 

Azure API Management (APIM) örneğini oluşturduğunuz zaman, azure-api.net alt etki alanı için atar (örneğin, `apim-service-name.azure-api.net`). Ancak, kendi etki alanı adı gibi kullanarak APIM uç noktalarınızı getirebilir **contoso.com**. Bu öğreticide, Azure API Management örneği tarafından kullanıma sunulan uç noktaları için var olan özel bir DNS adı eşlemeyle ilgili bilgi gösterir.

> [!WARNING]
> Uygulamalarının güvenliğini artırmak için sertifika sabitleme kullanmak isteyen müşteriler, özel etki alanı adı kullanmalıdır > ve yönettikleri sertifika varsayılan sertifika değil. Varsayılan Sertifika sabitleme müşterilerin bunun yerine olacaktır > önerilen uygulama değil, yoksa denetlemek, sertifikanın özelliklerini üzerinde sabit bir bağımlılık alma.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Size ait bir özel etki alanı adı. Kullanmak istediğiniz özel etki alanı adı ayrı olarak temin ve bir DNS sunucusunda barındırılan gerekir. Bu konuda bir özel etki alanı adı barındırmak nasıl yönergeler sağlamaz.
+ Genel ve özel bir anahtara sahip geçerli bir sertifikası olması gerekir (. PFX). Konu veya konu alternatif adı (SAN) (Bu URL'leri SSL üzerinden güvenli bir şekilde kullanıma sunmak APIM sağlar) etki alanı adı ile eşleşmesi gerekir.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Özel etki alanı adı ayarlamak için Azure portalını kullanma

1. APIM Örneğinize gidin [Azure portalında](https://portal.azure.com/).
1. Seçin **özel etki alanları ve SSL**.
    
    Bir özel etki alanı adı atamak için uç noktalar vardır. Şu anda aşağıdaki uç noktaların bulunmaktadır: 
   + **Proxy** (varsayılan: `<apim-service-name>.azure-api.net`), 
   + **Portal** (varsayılan: `<apim-service-name>.portal.azure-api.net`),     
   + **Yönetim** (varsayılan: `<apim-service-name>.management.azure-api.net`), 
   + **SCM** (varsayılan: `<apim-service-name>.scm.azure-api.net`).

     >[!NOTE]
     > Tüm uç noktalar veya bunlardan bazıları güncelleştirebilirsiniz. Yaygın olarak, müşteriler güncelleştirme **Proxy** (Bu URL'yi API Management aracılığıyla kullanıma sunulan API çağırmak için kullanılır) ve **portalı** (Geliştirici Portalı URL'si). **Yönetim** ve **SCM** uç noktaları APIM müşteriler tarafından dahili olarak kullanılır ve bu nedenle daha az sıklıkta özel etki alanı atanır.

1. Güncelleştirmek istediğiniz uç nokta seçin. 
1. Sağdaki pencerede **özel**.

   + İçinde **özel etki alanı adı**, kullanmak istediğiniz adı belirtin. Örneğin, `api.contoso.com`. Joker karakter etki alanı adlarını (örneğin, *. etkialanı.com) da desteklenir.
   + İçinde **sertifika**, Key Vault'tan bir sertifika seçin. Geçerli bir yükleyebilirsiniz. PFX dosyasını açıp sağlayan kendi **parola**, sertifika bir parolayla korunuyorsa.

     > [!TIP]
     > Özel etki alanı SSL sertifikası yönetmek için Azure anahtar kasası kullanıyorsanız, sertifika Key Vault'a eklediğiniz emin olun [olarak bir *sertifika*](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate)değil bir *gizli*. Sertifika için autorotate ayarlarsanız, API Management en son sürümü otomatik olarak seçer.

1. Uygula düğmesini tıklatın.

    >[!NOTE]
    >Sertifika atama işleminin 15 dakika veya daha fazla dağıtım boyutuna bağlı olarak alabilir. Geliştirici SKU kapalı kalma süresi varsa, temel ve daha yüksek SKU kapalı kalma süresi yoktur.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Yükseltme ve hizmetinizi ölçeklendirin](upgrade-and-scale.md)
