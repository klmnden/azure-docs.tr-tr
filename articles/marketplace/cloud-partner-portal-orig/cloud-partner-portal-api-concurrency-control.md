---
title: Eşzamanlılık denetimi | Microsoft Docs
description: API'leri yayımlamaya bulut iş ortağı portalı için eşzamanlılık denetimi stratejiler.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ecf0bb6ac7fc77e804c9fc8d62aba52810de5640
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60625015"
---
<a name="concurrency-control"></a>Eşzamanlılık Denetimi
===================

API'leri yayımlamaya bulut iş ortağı portalı yapılan her çağrı kullanmak için hangi eşzamanlılık denetimi stratejisi açıkça belirtmeniz gerekir. Sunulamamasından **IF-Match** üst bilgisi bir HTTP 400 hata yanıtında neden olur. Eşzamanlılık denetimi için iki stratejileri sunuyoruz.

-   **İyimser** -update işleminin gerçekleştirilmesi istemci en son verileri okumak bu yana veriler değişip değişmediğini doğrular.
-   **Bir WINS son** -istemci doğrudan son okuma süresi beri olup başka bir uygulama değiştirdiği bağımsız olarak verileri güncelleştirir.

<a name="optimistic-concurrency-workflow"></a>İyimser eşzamanlılık iş akışı
-------------------------------

Beklenmeyen düzenleme yok, kaynaklarınıza yapıldığını güvence altına almak için aşağıdaki iş akışı ile iyimser eşzamanlılık stratejisini kullanmanızı öneririz.

1.  API'leri kullanarak bir varlığı alır. Yanıt, varlığın depolanan geçerli sürümü (zamanında yanıt) tanımlayan bir ETag değeri içeriyor.
2.  Güncelleştirme sırasında aynı bu ETag değeri, zorunlu olarak dahil **IF-Match** isteği üstbilgisi.
3.  API varlığı atomik bir işlem içinde geçerli ETag değeri istekte alınan ETag değeri karşılaştırır.
    *   ETag değerler farklıysa, API döndürür bir `412 Precondition Failed` HTTP yanıtı. Bu hata, istemcinin en son alınan olduğundan ya da başka bir işlem varlık güncelleştirilmiş veya istekte belirtilen ETag değeri yanlış olduğunu gösterir.
    *  ETag değerleri aynıysa veya **IF-Match** üstbilgi yıldız işareti joker karakter içeriyor (`*`), API istenen işlemi gerçekleştirir. API işlemi, ayrıca depolanan ETag değeri varlığın güncelleştirir.


> [!NOTE]
> Joker karakter (*) belirterek **IF-Match** üstbilgi sonuçları son bir WINS eşzamanlılık stratejisini kullanarak API. Bu durumda, ETag karşılaştırma yapılmaz ve tüm denetimleri kaynak güncelleştirilir. 
