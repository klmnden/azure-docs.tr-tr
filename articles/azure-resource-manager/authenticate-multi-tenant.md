---
title: Kiracılar genelinde - Azure Resource Manager kimlik doğrulaması
description: Azure Resource Manager kimlik doğrulama isteklerini kiracılar genelinde nasıl işlediğini açıklar.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: tomfitz
ms.openlocfilehash: 6554c05f40f580a6d7ae086e1d09834298f86621
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60550776"
---
# <a name="authenticate-requests-across-tenants"></a>Kiracılar genelinde isteklerin kimliklerini doğrular

Çok kiracılı bir uygulama oluştururken farklı kiracıda olan kaynaklar için kimlik doğrulama isteklerini işlemek gerekebilir. Sık karşılaşılan bir senaryodur, tek bir kiracı bir sanal makinede bir sanal ağ başka bir kiracıda birleştirmelisiniz andır. Azure Resource Manager, farklı kiracıların isteklerinin kimliğini doğrulamak için yardımcı belirteçleri depolamak için bir üstbilgi değerini sağlar.

## <a name="header-values-for-authentication"></a>Kimlik doğrulaması için üstbilgi değerleri

İstek aşağıdaki kimlik doğrulaması üstbilgi değerleri vardır:

| Üst bilgi adı | Açıklama | Örnek değer |
| ----------- | ----------- | ------------ |
| Yetkilendirme | Birincil belirteç | Taşıyıcı &lt;birincil-token&gt; |
| x-ms-yetkilendirme-yardımcı | Yardımcı belirteç | Taşıyıcı &lt;yardımcı token1&gt;; EncryptedBearer &lt;yardımcı token2&gt;; Taşıyıcı &lt;yardımcı token3&gt; |

İkincil üstbilgi en fazla üç yardımcı belirteçleri barındırabilir. 

Çok kiracılı uygulamanızın kodunda diğer kiracılar için kimlik doğrulama belirteci alma ve yardımcı üstbilgilerinde depolayabilirsiniz. Tüm belirteçler aynı kullanıcı veya uygulama olmalıdır. Kullanıcı veya uygulamanın diğer kiracılara konuk olarak davet edildiniz gerekir.

## <a name="processing-the-request"></a>İstek işleniyor

Uygulamanızın kaynak yöneticisi için bir istek gönderdiğinde, istek kimliği birincil belirteçten çalıştırılır. Birincil belirteç geçerli ve süresi dolmamış olmalıdır. Bu belirteç abonelik yönetebileceği bir kiracısı olmalıdır.

İstek farklı kiracısından bir kaynağa başvurduğunda, Resource Manager istek işlenebileceğini belirlemeye yardımcı belirteçleri denetler. Tüm yardımcı belirteçleri üst bilgisindeki geçerli ve süresi dolmamış olmalıdır. Herhangi bir belirteci süresi Resource Manager 401 yanıt kodunu döndürür. Yanıt, istemci kimliği ve Kiracı kimliği geçersiz belirteç'içeriyor. İkincil üstbilgi Kiracı için geçerli bir belirteç içeriyorsa, çapraz Kiracı istek işlenir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager API'leri ile kimlik doğrulama istekleri gönderme hakkında bilgi edinmek için [aboneliklere erişmek için Kaynak Yöneticisi'ni kullanın kimlik doğrulama API'si](resource-manager-api-authentication.md).
* Belirteçleri hakkında daha fazla bilgi için bkz: [Azure Active Directory erişim belirteçleri](/azure/active-directory/develop/access-tokens).
