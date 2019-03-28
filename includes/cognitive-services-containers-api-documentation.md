---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: 94e95864d8bac2d6dc0ff690a2a8f53bd2db5a40
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522525"
---
## <a name="validate-container-is-running"></a>Doğrulama kapsayıcı çalışıyor 

Kapsayıcı doğrulamak için birkaç yol çalıştığı vardır: 

|İstek|Amaç|
|--|--|
|`http://localhost:5000/`|Kapsayıcı, bir giriş sağlar.|
|`http://localhost:5000/status`|GET ile istendi, kapsayıcı doğrulamak için bir uç nokta sorgu neden olmadan çalışıyor. Kubernetes için bu kullanılabilir [canlılık ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|Kapsayıcı uç noktaları için belgeleri tam bir dizi sağlar hem de bir `Try it now` özelliği. Bu özellik, web tabanlı bir HTML formuna ayarlarınızı girdikten ve herhangi bir kod yazmak zorunda kalmadan sorgu yapmanıza olanak tanır. Sorgunun döndürdüğü CURL komutu bir örnek sağlanır sonra HTTP üst bilgilerini gösterme ve gerekli biçim gövde. |

![Kapsayıcının giriş sayfası](./media/cognitive-services-containers-api-documentation/container-webpage.png)
