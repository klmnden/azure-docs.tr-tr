---
title: "Azure Mobile Engagement hizmet API'leri erişmek için REST API kullanarak"
description: "Azure Mobile Engagement hizmet API'leri erişmek için Mobile Engagement REST API'leri kullanmayı açıklar"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 4b21bca6fee7012ce1dba96950ae075eded414f8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Azure Mobile Engagement hizmet API'leri erişmek için REST kullanma
Azure Mobile Engagement sağlar [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) aygıtları yönetebilmeniz için Reach/anında iletme kampanyalarını vb.

> [!NOTE]
> Azure Mobile Engagement hizmeti, Mart 2018’de devre dışı bırakılacaktır. Şu anda yalnızca mevcut müşteriler tarafından kullanılabilmektedir. Daha fazla bilgi için bkz. [Mobile Engagement nedir?](https://azure.microsoft.com/en-us/services/mobile-engagement/).

REST API'lerini doğrudan kullanmak istemiyorsanız, biz de sağlar. bir [Swagger dosyası](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) için tercih ettiğiniz dili SDK'ları oluşturmak için Araçlar ile kullanabilirsiniz. Kullanmanızı öneririz [AutoRest](https://github.com/Azure/AutoRest) aracı, SDK bizim Swagger dosyasından oluşturun. Bu API'leri ile etkileşime olanak tanıyan bir C# sarmalayıcı kullanarak benzer şekilde bir .NET SDK'sı oluşturduk ve kimlik doğrulama belirteci anlaşma yapın ve kendiniz yenilemek yok. Bkz: [hizmeti API .NET SDK'sı örneği](mobile-engagement-dotnet-sdk-service-api.md) .net SDK'sı için API kullanımı konusunda bilgi almak için
