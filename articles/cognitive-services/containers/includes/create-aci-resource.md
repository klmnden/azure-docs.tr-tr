---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Bir azure container örneği (ACI) kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: b24a78873c3220b969fc548d2553e465e80773cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455066"
---
## <a name="create-an-azure-container-instance-aci-resource"></a>Bir Azure Container örneği (ACI) kaynak oluştur

1. Git [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) Container Instances için sayfa.

2. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Aboneliğinizi seçin.|
    |Kaynak grubu|Kullanılabilir kaynak grubu seçin veya yeni bir tane oluşturmanız `cognitive-services`.|
    |Kapsayıcı adı|Gibi bir ad girin `cognitive-container-instance`. Bu ad, daha düşük bir büyük harf olarak olmalıdır.|
    |Location|Dağıtım için bir bölge seçin.|
    |Görüntü türü|`Public`|
    |Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Bu, kullandığınız konumun aynısını olabilir `docker pull` komutu _örneğin_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |İşletim sistemi türü|`Linux`|
    |Size|Belirli bir Bilişsel hizmet kapsayıcınız için önerilen önerileri boyutu değiştirin.:<br>2 Çekirdek<br>4 GB

3. Üzerinde **ağ** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı Noktaları|TCP bağlantı noktası olarak `5000`. Kapsayıcı 5000 numaralı bağlantı noktasında kullanıma sunar.|

4. Üzerinde **Gelişmiş** sekmesinde, üzerinden bir kapsayıcının geçirmek için aşağıdaki ayrıntıları girin [gerekli faturalandırma ayarlar](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) ACI kaynağa:

    |Gelişmiş sayfa anahtarı|Gelişmiş sayfa değeri|
    |--|--|
    |`apikey`|Öğesinden kopyalanan **anahtarları** metin analizi kaynak sayfası. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Öğesinden kopyalanan **genel bakış** metin analizi kaynak sayfası. Örnek: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Tıklayın **gözden geçir ve Oluştur**
1. Doğrulama denetimini geçtikten tıklayın **Oluştur** oluşturma işlemini tamamlamak için
1. Kaynak başarıyla dağıtıldığında, kullanıma hazır