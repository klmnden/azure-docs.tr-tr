---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: f4925401235aedb341a7e29ca36b079126647f7b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124268"
---
## <a name="validate-that-a-container-is-running"></a>Bir kapsayıcının çalıştırıldığını doğrula 

Kapsayıcının çalışır durumda olduğunu doğrulamak için birkaç yolu vardır. 

|İste|Amaç|
|--|--|
|`http://localhost:5000/`|Kapsayıcı, bir giriş sayfası sağlar.|
|`http://localhost:5000/status`|Bir uç nokta sorgu neden olmadan kapsayıcının çalışır durumda olduğunu doğrulamak için GET ile istedi. Bu istek için Kubernetes kullanılabilir [canlılık ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|Kapsayıcı uç noktaları için tam kümesi sağlar ve bir `Try it now` özelliği. Bu özellik, web tabanlı bir HTML formuna ayarlarınızı girdikten ve herhangi bir kod yazmak zorunda kalmadan sorgunun sağlayın. Sorgunun döndürdüğü sonra HTTP üst bilgileri ve gövdesini biçiminde göstermek için gerekli olan bir örnek CURL komutu sağlanır. |

![Kapsayıcının giriş sayfası](./media/cognitive-services-containers-api-documentation/container-webpage.png)
