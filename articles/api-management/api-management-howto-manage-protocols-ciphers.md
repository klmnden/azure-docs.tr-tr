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
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: a55d16a35b5eec1af2b24d02e158905493615999
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441121"
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
2. Seçin **SSL** menüsünde.  
    ![Protokoller ve şifrelemeleri, APIM - yönetme menüsü](./media/api-management-howto-manage-protocols-ciphers/api-management-menu.png)
3. Etkinleştirmek veya istenen protokollerden veya şifrelerini devre dışı bırakın.
4. **Kaydet**’e tıklayın. Değişiklikler bir saat içinde uygulanır.  
    ![Protokoller ve şifrelemeleri, APIM - kaydetme yönetme](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers-save.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [TLS (Aktarım Katmanı Güvenliği)](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
* Daha fazla bilgi denetleyin [videoları](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management hakkında.