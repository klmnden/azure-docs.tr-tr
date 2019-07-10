---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Bir Azure container örneği kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 38addf4651373ba0f4df411325218a255c835508
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711411"
---
## <a name="create-an-azure-container-instance-resource"></a>Bir Azure Container Instance kaynağı oluşturun

1. Git [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) Container Instances için sayfa.

2. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Subscription|Aboneliğinizi seçin.|
    |Resource group|Kullanılabilir kaynak grubu seçin veya yeni bir tane oluşturmanız `cognitive-services`.|
    |Kapsayıcı adı|Gibi bir ad girin `cognitive-container-instance`. Ad, daha düşük bir büyük harf olarak olmalıdır.|
    |Location|Dağıtım için bir bölge seçin.|
    |Görüntü türü|`Public`|
    |Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Konum kullanılan aynı olabilir `docker pull` komutu, başvurmak [kapsayıcı depoları ve görüntüleri](../../cognitive-services-container-support.md#container-repositories-and-images) için kullanılabilir görüntü adlarını ve bunların karşılık gelen bir depo.|
    |İşletim sistemi türü|`Linux`|
    |Size|Belirli bir Bilişsel hizmet kapsayıcınız için önerilen önerileri boyutunu değiştir:<br>2 CPU çekirdekleri<br>4 GB

3. Üzerinde **ağ** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı Noktaları|TCP bağlantı noktası olarak `5000`. Kapsayıcı 5000 numaralı bağlantı noktasında kullanıma sunar.|

4. Üzerinde **Gelişmiş** sekmesinde, gerekli girin **ortam değişkenlerini** kapsayıcının [ayarları faturalama](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) ACI kaynak:

    | Anahtar | Değer |
    |--|--|
    |`apikey`|Öğesinden kopyalanan **anahtarları** metin analizi kaynak sayfası. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Öğesinden kopyalanan **genel bakış** metin analizi kaynak sayfası. Örnek: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Tıklayın **gözden geçir ve Oluştur**
1. Doğrulama denetimini geçtikten tıklayın **Oluştur** oluşturma işlemini tamamlamak için
1. Kaynak başarıyla dağıtıldıktan sonra hazır