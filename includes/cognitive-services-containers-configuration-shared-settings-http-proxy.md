---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 9911b1c92bdca6c0cdf064ea484cfb603e659467
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712570"
---
Giden istek yapmak için bir HTTP proxy yapılandırmanız gerekiyorsa, bu iki bağımsız değişkeni kullanın:

| Ad | Veri türü | Açıklama |
|--|--|--|
|HTTP_PROXY|dize|Örneğin, kullanmak için proxy `http://proxy:8888`<br>< proxy URL'si >|
|HTTP_PROXY_CREDS|dize|proxy'ye göre örneğin, kullanıcıadı kimlik doğrulaması için gereken tüm kimlik bilgileri.|
|`<proxy-user>`|dize|Proxy kullanıcı.|
|`proxy-password`|dize|İlişkili parolayı `<proxy-user>` proxy için.|
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