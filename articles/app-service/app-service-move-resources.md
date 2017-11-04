---
title: "Bir Azure Web uygulaması için başka bir kaynak grubu taşıma | Microsoft Docs"
description: "Burada, Web uygulamaları ve uygulama hizmetleri bir kaynak grubundan diğerine taşıyabilirsiniz senaryoları açıklanmıştır."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: zarizvi
ms.openlocfilehash: 4d5d6540dd0b51a4f172649a1fd9a2343efeceee
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>Bir Azure Web uygulaması başka bir kaynak grubuna taşıma
Aynı abonelik başka bir kaynak grubunda veya farklı bir abonelik kaynak grubunda bir Web uygulaması ve/veya ilgili kaynaklarını taşıyabilirsiniz. Bu, standart kaynak yönetimi azure'da bir parçası olarak gerçekleştirilir. Daha fazla bilgi için bkz: [yeni abonelik veya kaynak grubuna taşımak Azure kaynakları](../azure-resource-manager/resource-group-move-resources.md).

## <a name="limitations-when-moving-within-the-same-subscription"></a>Aynı abonelik taşırken sınırlamaları

Bir Web uygulaması taşınırken _aynı abonelik içindeki_, karşıya yüklenen SSL sertifikalarını taşıyamazsınız. Ancak, karşıya yüklenen SSL sertifikasını taşımadan yeni kaynak grubu için bir Web uygulaması taşıyabilirsiniz ve uygulamanızın SSL işlevselliği hala çalışmaktadır. 

Web uygulaması ile SSL sertifikası taşımak istiyorsanız, aşağıdaki adımları izleyin:

1.  Karşıya yüklenen sertifikanın Web uygulamasından silin.
2.  Web uygulaması taşıyın.
3.  Sertifika taşınan Web uygulamasına yükleyin.

## <a name="limitations-when-moving-across-subscriptions"></a>Abonelikler arasında taşıma sınırlamaları

Bir Web uygulaması taşınırken _Aboneliklerdeki_, aşağıdaki sınırlamalar uygulanır:

- Hedef kaynak grubu mevcut tüm uygulama hizmeti kaynaklarına sahip olmamalıdır. Uygulama hizmeti kaynaklar şunlardır:
    - Web Apps
    - App Service planları
    - Karşıya yüklenen veya alınan SSL sertifikaları
    - App Service ortamları
- Kaynak grubundaki tüm uygulama hizmeti kaynaklar birlikte taşınmalıdır.
- Uygulama hizmeti kaynaklar yalnızca bunlar ilk olarak oluşturulduğu kaynak grubundan taşınabilir. Bir uygulama hizmeti kaynak artık kendi özgün kaynak grubunda değilse, bunu geri, özgün kaynak grubuna ilk taşınmalıdır ve ardından abonelikler arasında taşınabilmesi. 
