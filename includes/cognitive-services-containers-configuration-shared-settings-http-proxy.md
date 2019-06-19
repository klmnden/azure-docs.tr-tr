---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: fe1b4699a300831294c26b103d322fb83ad87d3b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188641"
---
Giden istek yapmak için bir HTTP proxy yapılandırmanız gerekiyorsa, bu iki bağımsız değişkeni kullanın:

| Ad | Veri türü | Açıklama |
|--|--|--|
|HTTP_PROXY|string|Örneğin, kullanmak için proxy `http://proxy:8888`<br>< proxy URL'si >|
|HTTP_PROXY_CREDS|string|proxy'ye göre örneğin, kullanıcıadı kimlik doğrulaması için gereken tüm kimlik bilgileri.|
|`<proxy-user>`|string|Proxy kullanıcı.|
|`proxy-password`|string|İlişkili parolayı `<proxy-user>` proxy için.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```