---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 00cc63f53388ab7bea05a0b55784247f63477684
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704260"
---
## <a name="validate-that-a-container-is-running"></a>Bir kapsayıcının çalıştırıldığını doğrula 

Kapsayıcının çalışır durumda olduğunu doğrulamak için birkaç yolu vardır. 

|İstek|Amaç|
|--|--|
|`http://localhost:5000/`|Kapsayıcı, bir giriş sayfası sağlar.|
|`http://localhost:5000/status`|Bir uç nokta sorgu neden olmadan kapsayıcının çalışır durumda olduğunu doğrulamak için GET ile istedi. Bu istek için Kubernetes kullanılabilir [canlılık ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|Kapsayıcı uç noktaları için tam kümesi sağlar ve bir `Try it now` özelliği. Bu özellik, web tabanlı bir HTML formuna ayarlarınızı girdikten ve herhangi bir kod yazmak zorunda kalmadan sorgunun sağlayın. Sorgunun döndürdüğü sonra HTTP üst bilgileri ve gövdesini biçiminde göstermek için gerekli olan bir örnek CURL komutu sağlanır. |

![Kapsayıcının giriş sayfası](./media/cognitive-services-containers-api-documentation/container-webpage.png)
