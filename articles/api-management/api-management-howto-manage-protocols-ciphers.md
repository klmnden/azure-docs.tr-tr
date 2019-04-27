---
title: Protokoller ve şifrelemeleri Azure API Management'te yönetme | Microsoft Docs
description: Protokoller (TLS, SSL) ve Azure API Yönetimi'nde (DES) şifrelemeleri yönetmeyi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: apimpm
ms.openlocfilehash: 91b6cd64a42319b2a5307919c2efe6bc8e5dcd64
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60657817"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Protokoller ve şifrelemeleri Azure API Management'te yönetme

Azure API Management, 3DES şifreleme yanı sıra hem istemci hem de arka uç yüz için birden çok TLS protokolü sürümleri destekler.

Bu kılavuz, iletişim kurallarını yönetme işlemini göstermektedir ve şifreleme için bir Azure API Management örneği yapılandırması.

![Protokoller ve şifrelemeleri, APIM yönetme](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları izlemek için şunlara sahip olmalısınız:

* API Management örneği

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>TLS protokolleri ve 3DES şifreleme yönetme

1. Gidin, **API Management örneği** Azure portalında.
2. Seçin **protokol ayarları** menüsünde.  
3. Etkinleştirmek veya istenen protokollerden veya şifrelerini devre dışı bırakın.
4. **Kaydet**’e tıklayın. Değişiklikler bir saat içinde uygulanır.  

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [TLS (Aktarım Katmanı Güvenliği)](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
* Daha fazla bilgi denetleyin [videoları](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.