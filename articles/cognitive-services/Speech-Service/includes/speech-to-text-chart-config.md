---
title: Konuşma kapsayıcılara yükleme
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne helm grafiği yapılandırma seçeneklerinin ayrıntılı olarak açıklanmaktadır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1b46c58d3f3c804052e637f7bde2e1a456764dba
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717241"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Konuşmayı metne (alt grafik: grafikler/speechToText)

"Genel" grafik geçersiz kılmak için ön ek ekleme `speechToText.` üzerinde herhangi bir parametre daha belirgin hale getirin. Örneğin, karşılık gelen parametre örneğin geçersiz kılacak olan `speechToText.numberOfConcurrentRequest` geçersiz kılmalar `numberOfConcurrentRequest`.

|Parametre|Açıklama|Varsayılan|
| -- | -- | -- |
| `enabled` | Olmadığını **konuşma metin** etkinleştirildi. | `false` |
| `numberOfConcurrentRequest` | İçin eş zamanlı istek sayısını **konuşma metin** hizmeti. Bu grafik, bu değere göre CPU ve bellek kaynakları otomatik olarak hesaplar. | `2` |
| `optimizeForAudioFile`| Olup hizmetinin ses dosyaları aracılığıyla ses girişi için en iyi duruma getirmek gerekir. Varsa `true`, bu grafik hizmetine daha fazla CPU kaynağı ayırır. | `false` |
| `image.registry`| **Konuşma metin** docker görüntü kayıt defteri. | `containerpreview.azurecr.io` |
| `image.repository` | **Konuşma metin** docker görüntü deposu. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | **Konuşma metin** docker resim etiketi. | `latest` |
| `image.pullSecrets` | Gizli dizileri görüntü çekmek için **konuşma metin** docker görüntüsü. | |
| `image.pullByHash`| İster docker görüntüsünü karma olarak alınır. Varsa `true`, `image.hash` gereklidir. | `false` |
| `image.hash`| **Konuşma metin** docker görüntü karması. Yalnızca ne zaman kullanılır `image.pullByHash: true`.  | |
| `image.args.eula` (gerekli) | Lisans kabul ettiğiniz gösterir. Yalnızca geçerli değer `accept` | |
| `image.args.billing` (gerekli) | Fatura uç noktası URI değerini Azure portalının konuşma genel bakış sayfasında kullanılabilir. | |
| `image.args.apikey` (gerekli) | Fatura bilgileri izlemek için kullanılır. ||
| `service.type` | Kubernetes hizmet türünü **konuşma metin** hizmeti. Bkz: [Kubernetes hizmet türleri yönergeleri](https://kubernetes.io/docs/concepts/services-networking/service/) daha fazla ayrıntı için ve bulut sağlayıcı desteği doğrulayın. | `LoadBalancer` |
| `service.port`|  Bağlantı noktası **konuşma metin** hizmeti. | `80` |
| `service.autoScaler.enabled` | Olmadığını [yatay Pod otomatik Ölçeklendiricinin](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) etkinleştirilir. Varsa `true`, `speech-to-text-autoscaler` içindeki bir Kubernetes kümesi dağıtılır. | `true` |
| `service.podDisruption.enabled` | Olmadığını [Pod kesintisi bütçe](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) etkinleştirilir. Varsa `true`, `speech-to-text-poddisruptionbudget` içindeki bir Kubernetes kümesi dağıtılır. | `true` |