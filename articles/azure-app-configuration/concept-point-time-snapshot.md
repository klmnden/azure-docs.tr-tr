---
title: Azure uygulama yapılandırması zaman içinde nokta anlık görüntü | Microsoft Docs
description: Azure uygulama yapılandırmasında nasıl-belirli bir noktaya anlık görüntü genel bir bakış çalışır
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 83770e8c5f415670855b5cf2502d02c4d6919440
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60741147"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure uygulama yapılandırması kesin zamanlar ne zaman yeni bir anahtar-değer çifti oluşturulur ve ardından değişiklik kayıtları tutar. Bu kayıtlar, anahtar-değer değişiklikleri eksiksiz bir zaman çizelgesi oluşturur. Bir uygulama yapılandırma deposu, herhangi bir anahtar değer geçmişini yeniden yapılandırma ve son değerini geçerli kadar belirli bir anda yeniden yürütün. Bu özellik sayesinde, "saat-geriye doğru seyahat" ve eski bir anahtar değerini almak. Örneğin, bir önceki yapılandırmayı kurtarmak ve uygulama geri alma için dünün yapılandırma ayarları, hemen önce en son dağıtım elde edebilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Anahtar değerlerini almak için bir REST API çağrısı HTTP üst bilgisinde bir süre anlık görüntü anahtar değerlerini belirtin. Örneğin:

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Şu anda, uygulama yapılandırmasını yedi gün boyunca değişiklik geçmişini tutar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
