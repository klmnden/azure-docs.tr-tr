---
title: "Özel bağlayıcı SSS - Azure Logic Apps | Microsoft Docs"
description: "Gereksinimleri, vb. oluşturma hakkında daha fazla özel bağlayıcılar tetikleyiciler, hakkında SSS"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 225e56de3985acae871ddec447b763e7de61cb80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="faq-custom-connectors"></a>Sık sorulan sorular: Özel bağlayıcılar

## <a name="requirements"></a>Gereksinimler

**S:** REST API'leri olmadan bir bağlayıcı yapı? </br>
**Y:** Hayır, bir bağlayıcı oluşturmak için kararlı HTTP REST API'leri hizmetiniz için desteklemesi gerekir. 

**S:** hangi araçları bir bağlayıcı oluşturmak için kullanabilir miyim? </br>
**Y:** yetenekleri ve herhangi bir hizmet barındırma, API Management ve daha fazla bilgi için Azure uygulama hizmeti gibi bir API olarak sunmaya yönelik kullanan hizmetler Azure sahiptir.

**S:** hangi kimlik doğrulama türleri desteklenir? </br>
**Y:** bu desteklenen kimlik doğrulama standartları kullanabilirsiniz:

* [OAuth 2.0](https://oauth.net/2/)dahil [Azure Active Directory](https://azure.microsoft.com/develop/identity/) veya Dropbox, GitHub ve SalesForce gibi belirli hizmetleri
* Genel OAuth 2.0
* [Temel kimlik doğrulaması](https://swagger.io/docs/specification/authentication/basic-authentication/)
* [API anahtarı](https://swagger.io/docs/specification/authentication/api-keys/)

## <a name="triggers"></a>Tetikleyiciler

**S:** Tetikleyiciler Web kancalarını olmadan yapı? </br>
**Y:** Hayır, Azure mantıksal uygulamaları ve Microsoft Flow desteği yalnızca Web kancası tabanlı tetikleyiciler için özel bağlayıcılar. Uygulama için diğer desenleri talep etmek istiyorsanız, ilgili kişi [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com) API'nizi hakkında daha fazla ayrıntı.

## <a name="certification"></a>Sertifika

**Q**: bir Microsoft iş ortağı veya bağımsız yazılım satıcısı (ISV) değilim. Bağlayıcılar oluşturabilir miyim? </br>
**A**: Evet, kuruluşunuzda iç kullanım için bu bağlayıcıların kaydedebilirsiniz, ancak onaylamak ve genel olarak bir bağlayıcı yayın istiyorsanız, ya da temel alınan hizmet veya API kullanmak için mevcut açık haklarına sahip olmanız gerekir.

## <a name="other"></a>Diğer

**S:** My API'lerini kullanan bir dinamik ana bilgisayar. Nasıl t ile OpenAPI uygulamadan? </br>
**Y:** özel bağlayıcılar, dinamik ana bilgisayar desteklemez. Bunun yerine, statik bir ana bilgisayar geliştirme ve sınama amacıyla kullanın. Bağlayıcınızı onaylamak istiyorsanız, dinamik uygulama hakkında Microsoft temsilcinizden öğrenebilirsiniz.

**S:** Postman koleksiyonu V2 desteklemez? </br>
**Y:** Hayır, yalnızca Postman koleksiyonu V1 şu anda desteklenmiyor.

**S:** OpenAPI 3.0 desteği musunuz? </br>
**Y:** Hayır, yalnızca OpenAPI 2.0 şu anda desteklenmiyor.