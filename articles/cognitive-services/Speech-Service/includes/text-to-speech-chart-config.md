---
title: Konuşma kapsayıcılara yükleme
titleSuffix: Azure Cognitive Services
description: Metin okuma helm grafiği yapılandırma seçeneklerinin ayrıntılı olarak açıklanmaktadır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: e6c7dcd3015b0b8ab5b3c719ebd2397bc814b81a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711499"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Metin okuma (alt grafik: grafikler/textToSpeech)

"Genel" grafik geçersiz kılmak için ön ek ekleme `textToSpeech.` üzerinde herhangi bir parametre daha belirgin hale getirin. Örneğin, karşılık gelen parametre örneğin geçersiz kılacak olan `textToSpeech.numberOfConcurrentRequest` geçersiz kılmalar `numberOfConcurrentRequest`.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | Olmadığını **metin okuma** etkinleştirildi. | `false` |
| `numberOfConcurrentRequest` | İçin eş zamanlı istek sayısını **metin okuma** hizmeti. Bu grafik, bu değere göre CPU ve bellek kaynakları otomatik olarak hesaplar. | `2` |
| `optimizeForTurboMode`| Olup hizmetinin metin girişi metin dosyaları için en iyi duruma getirmek gerekir. Varsa `true`, bu grafik hizmetine daha fazla CPU kaynağı ayırır. | `false` |
| `image.registry`| **Metin okuma** docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Metin okuma** docker görüntü deposu. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | **Metin okuma** docker resim etiketi. | `latest` |
| `image.pullSecrets` | Gizli dizileri görüntü çekmek için **metin okuma** docker görüntüsü. | |
| `image.pullByHash`| İster docker görüntüsünü karma olarak alınır. Varsa `true`, `image.hash` gereklidir. | `false` |
| `image.hash`| **Metin okuma** docker görüntü karması. Yalnızca ne zaman kullanılır `image.pullByHash: true`.  | |
| `image.args.eula` (gerekli) | Lisans kabul ettiğiniz gösterir. Yalnızca geçerli değer `accept` | |
| `image.args.billing` (gerekli) | Fatura uç noktası URI değerini Azure portalının konuşma genel bakış sayfasında kullanılabilir. | |
| `image.args.apikey` (gerekli) | Fatura bilgileri izlemek için kullanılır. ||
| `service.type` | Kubernetes hizmet türünü **metin okuma** hizmeti. Bkz: [Kubernetes hizmet türleri yönergeleri](https://kubernetes.io/docs/concepts/services-networking/service/) daha fazla ayrıntı için ve bulut sağlayıcı desteği doğrulayın. | `LoadBalancer` |
| `service.port`|  Bağlantı noktası **metin okuma** hizmeti. | `80` |
| `service.autoScaler.enabled` | Olmadığını [yatay Pod otomatik Ölçeklendiricinin](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) etkinleştirilir. Varsa `true`, `text-to-speech-autoscaler` içindeki bir Kubernetes kümesi dağıtılır. | `true` |
| `service.podDisruption.enabled` | Olmadığını [Pod kesintisi bütçe](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkinleştirilir. Varsa `true`, `text-to-speech-poddisruptionbudget` içindeki bir Kubernetes kümesi dağıtılır. | `true` |