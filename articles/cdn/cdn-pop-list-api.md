---
title: Geçerli Verizon POP listesini almak için Azure CDN | Microsoft Docs
description: REST API kullanarak geçerli Verizon POP listesini almak öğrenin.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: f796d18a03e14fdf652af72366762e1365523f09
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754723"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Azure CDN için geçerli Verizon POP listesini alma

REST API IP'leri kümesi Verizon'ın noktası varlığı (POP) sunucuları için almak için kullanabilirsiniz. Bu POP sunucular Verizon profilinde Azure içerik teslim ağı (CDN) uç ile ilişkili kaynak sunuculara isteklerde (**verizon'dan Azure CDN standart** veya **verizon'danAzureCDNPremium**). Bu IP'ler kümesini istemci istekleri için POP yaparken göreceğiniz IP'leri farklı olduğunu unutmayın. 

POP listesini almak için REST API işlemi sözdizimi için bkz: [kenar düğümleri - liste](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Tipik kullanım örneği

Güvenlik nedeniyle, yalnızca geçerli Verizon POP istekleri kaynak sunucuya yapılır zorlamak için bu IP listesi kullanabilirsiniz. Örneğin, birisi ana bilgisayar adı veya IP adresi için bir CDN uç noktanın kaynak sunucu belirlediyseniz, bir doğrudan Azure CDN tarafından sağlanan güvenlik özellikleri ve bu nedenle ölçekleme atlayarak kaynak sunucu için istekleri yapabilirsiniz. IP'leri bir kaynak sunucuda izin verilen tek gibi IP'leri döndürülen listede ayarlayarak, bu senaryo engellenebilir. Bunlar emin olmak için günde bir en az bir kez almak en son POP listesi vardır. 

## <a name="next-steps"></a>Sonraki adımlar

REST API'si hakkında daha fazla bilgi için bkz: [Azure CDN REST API](https://docs.microsoft.com/en-us/rest/api/cdn/).