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
ms.openlocfilehash: 4f26f3a31dc6303e991c39fc7f85d9bf254d57bc
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884869"
---
# <a name="point-in-time-snapshot"></a>Zaman içinde nokta anlık görüntü

Azure uygulama yapılandırması, ne zaman yeni bir anahtar-değer çifti oluşturulur ve daha sonra değiştirilen kesin zamanlar kayıtlarını tutar. Bu kayıtlar, anahtar-değer değişiklikleri eksiksiz bir zaman çizelgesi oluşturur. Bir uygulama yapılandırma deposu, anahtar-değer geçmişini yeniden yapılandırma ve son değerini geçerli kadar belirli bir anda yeniden yürüt. Bu özellik, "saat-geriye doğru seyahat" ve eski bir anahtar-değer almak verir. Örneğin, bir önceki yapılandırmayı uygulama geri almak için gereken bir durumda kurtarabilmeniz dün, hemen önce en son dağıtım için yapılandırma ayarlarını alabilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Anahtar değerlerini almak için bir REST API çağrısı HTTP üst bilgisinde bir süre anlık görüntü anahtar-değer belirtmeniz gerekir. Örneğin:

        GET /revisions HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Şu anda uygulama yapılandırması, değişiklik geçmişi 7 gün saklar.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: ASP.NET web uygulaması oluşturma](quickstart-aspnet-core-app.md)  
