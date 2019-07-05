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
ms.date: 07/01/2019
ms.author: apimpm
ms.openlocfilehash: 59b44dcc9ec3a1f7c274f426a19aa8ed2258db3e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509301"
---
# <a name="configure-a-custom-domain-name"></a>Özel bir etki alanı adı yapılandırma

Azure API Management hizmet örneği oluşturduğunuzda, Azure, azure-api.net alt etki alanı atar (örneğin, `apim-service-name.azure-api.net`). Ancak, kendi özel etki alanı adı gibi kullanarak, API yönetim uç noktalarını getirebilir **contoso.com**. Bu öğreticide bir API Management örneği tarafından kullanıma sunulan uç noktalarına var olan özel bir DNS adı eşlemeyle ilgili bilgi gösterir.

> [!WARNING]
> Uygulamalarının güvenliğini artırmak için sertifika sabitleme kullanmak isteyen müşteriler, özel etki alanı adı kullanmalıdır > ve yönettikleri sertifika varsayılan sertifika değil. Varsayılan Sertifika sabitleme müşterilerin bunun yerine olacaktır > önerilen uygulama değil, yoksa denetlemek, sertifikanın özelliklerini üzerinde sabit bir bağımlılık alma.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

-   Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

-   API Management örneği. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
-   Size ait bir özel etki alanı adı. Kullanmak istediğiniz özel etki alanı adı ayrı olarak temin ve bir DNS sunucusunda barındırılan gerekir. Bu konuda bir özel etki alanı adı barındırmak nasıl yönergeler sağlamaz.
-   Genel ve özel bir anahtara sahip geçerli bir sertifikası olması gerekir (. PFX). Konu veya konu alternatif adı (SAN) (Bu URL'leri SSL üzerinden güvenli bir şekilde kullanıma sunmak API Management örneği sağlar) etki alanı adı ile eşleşmesi gerekir.

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Özel etki alanı adı ayarlamak için Azure portalını kullanma

1. API Management Örneğinize gidin [Azure portalında](https://portal.azure.com/).
1. Seçin **özel etki alanları ve SSL**.

    Bir özel etki alanı adı atamak için uç noktalar vardır. Şu anda aşağıdaki uç noktaların bulunmaktadır:

    - **Proxy** (varsayılan: `<apim-service-name>.azure-api.net`),
    - **Portal** (varsayılan: `<apim-service-name>.portal.azure-api.net`),
    - **Yönetim** (varsayılan: `<apim-service-name>.management.azure-api.net`),
    - **SCM** (varsayılan: `<apim-service-name>.scm.azure-api.net`).

    > [!NOTE]
    > Tüm uç noktalar veya bunlardan bazıları güncelleştirebilirsiniz. Yaygın olarak, müşteriler güncelleştirme **Proxy** (Bu URL'yi API Management aracılığıyla kullanıma sunulan API çağırmak için kullanılır) ve **portalı** (Geliştirici Portalı URL'si). **Yönetim** ve **SCM** uç noktaları, API Management örneği sahipleri tarafından yalnızca dahili olarak kullanılır ve bu nedenle daha az sıklıkta özel etki alanı atanır. Çoğu durumda, belirtilen bir uç nokta için yalnızca tek bir özel etki alanı ayarlanabilir. Ancak, **Premium** katmanı destekleyen birden çok ana bilgisayar adları için ayarlama **Proxy** uç noktası.

1. Güncelleştirmek istediğiniz uç nokta seçin.
1. Sağdaki pencerede **özel**.

    - İçinde **özel etki alanı adı**, kullanmak istediğiniz adı belirtin. Örneğin, `api.contoso.com`. Joker karakter etki alanı adlarını (örneğin, \*. etkialanı.com) da desteklenir.
    - İçinde **sertifika**, Key Vault'tan bir sertifika seçin. Geçerli bir yükleyebilirsiniz. PFX dosyasını açıp sağlayan kendi **parola**, sertifika bir parolayla korunuyorsa.

    > [!TIP]
    > Azure Key Vault için sertifikaları yönetme ve bunların ayarlanması için autorotate kullanmanızı öneririz.
    > Özel etki alanı SSL sertifikası yönetmek için Azure anahtar kasası kullanıyorsanız, sertifika Key Vault'a eklediğiniz emin olun [olarak bir _sertifika_](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate)değil bir _gizli_.
    >
    > Bir SSL sertifikası getirmek için API Management listenin bir get gizli dizileri Azure Key Vault'a sertifikayı içeren izinleriniz olmalıdır. Azure portalını kullanarak, tüm gerekli yapılandırma adımları otomatik olarak tamamlanır. Yönetim API'si veya komut satırı araçlarını kullanarak, bu izinler el ile verilmesi gerekir. Bu iki adımda gerçekleştirilir. İlk olarak, yönetilen kimliği etkin olduğundan emin olun ve bu sayfada gösterilen sorumlu kimliğini not edin için API Management örneğinizin yönetilen kimlikleri sayfayı kullanın. İkinci olarak, izin listesinde verin ve sertifikayı içeren Azure Key Vault'a gizli izinleri bu asıl kimliği alın.
    >
    > Sertifika için autorotate ayarlarsanız (API Management katmanınızı SLA'sı - Geliştirici katmanı dışındaki tüm katmanlarda ı. e. varsa) API Management hizmet için kapalı kalma süresi olmadan otomatik olarak en son sürümünü ayarlama seçer.

1. Uygula düğmesini tıklatın.

    > [!NOTE]
    > Sertifika atama işleminin 15 dakika veya daha fazla dağıtım boyutuna bağlı olarak alabilir. Geliştirici SKU kapalı kalma süresi varsa, temel ve daha yüksek SKU kapalı kalma süresi yoktur.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Yükseltme ve hizmetinizi ölçeklendirin](upgrade-and-scale.md)
