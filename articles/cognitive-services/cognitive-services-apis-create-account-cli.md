---
title: Azure CLI kullanarak bir Bilişsel Hizmetler hesabı oluşturma
titlesuffix: Azure Cognitive Services
description: Azure CLI kullanarak Azure Bilişsel hizmetler API hesabı oluşturma
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: acafc2c42c2946632496b646d001c58d6b48c2a6
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657722"
---
# <a name="create-a-cognitive-services-account-using-the-azure-command-line-interfacecli"></a>Azure komut satırı arabirimi kullanarak Bilişsel Hizmetler hesabı oluşturma

Bu hızlı başlangıçta, Azure Bilişsel hizmetler için kaydolun ve tek hizmet veya birden çok hizmet aboneliği olan bir hesap oluşturma hakkında bilgi edineceksiniz kullanma [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Bu hizmetler, Azure tarafından temsil edilen [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), bir veya daha fazla Azure Bilişsel hizmetler API'leri bağlanmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği. [Hesap oluşturma](https://azure.microsoft.com/free/) ücretsiz.
* [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="install-the-azure-cli-and-sign-in"></a>Azure CLI'yı yükleme ve uygulamada oturum açma 

[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)’yi yükleyin. CLI yerel yüklemesinde açmanız çalıştırma [az login](https://docs.microsoft.com/cli/azure/reference-index#az-login) komutu:

```console
az login
```

Yeşil kullanabilirsiniz **deneyin** düğmesini tarayıcınızda şu komutları çalıştırın.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Yeni bir Bilişsel hizmetler Azure kaynak grubu oluşturun

Bilişsel hizmetler için aboneliklerinizi Azure kaynakları tarafından temsil edilir. Tüm Bilişsel Hizmetler hesabı (ve ilişkili Azure kaynağı) bir Azure kaynak grubuna ait olmalıdır.

### <a name="choose-your-resource-group-location"></a>Kaynak grubu konumunuzu seçin

Bir kaynak oluşturmak için kullanılabilen Azure konumlardan birine, aboneliğiniz için gerekir. İle kullanılabilir konumların bir listesini alabilirsiniz [az hesabı konumları-Listele](/cli/azure/account#az-account-list-locations) komutu. Çoğu Bilişsel hizmetler, çeşitli konumlardan erişilebilir. Size en yakın seçin veya hizmet için hangi konumların kullanılabilir bakın.

> [!IMPORTANT]
> * Azure Bilişsel hizmetler çağırırken gerekir Azure konumunuz unutmayın.
> * Bazı Bilişsel hizmetlerin kullanılabilirliğini bölgeye göre farklılık gösterebilir. Daha fazla bilgi için [bölgelere göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Azure CLI kullanarak azure konumunuz sonra yeni bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group#az-group-create) komutu.

Aşağıdaki örnekte, azure konumu değiştirin `westus2` biriyle Azure konumları, aboneliğiniz için kullanılabilir.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Bir bilişsel hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturulurken, bunların ile kullanmak istediğiniz hizmet "tür" bilmeniz gerekir [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya sku) istediğiniz. Kaynak oluşturulurken, parametre olarak bu ve diğer bilgileri kullanır.

> [!NOTE]
> Birçok Bilişsel hizmetler, hizmeti denemek için kullanabileceğiniz ücretsiz bir katman vardır. Ücretsiz katmanı kullanmak için `F0` kaynağınızın sku olarak.

### <a name="vision"></a>Görsel

| Hizmet                    | tür                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel görüntü işleme - tahmin | `CustomVision.Prediction` |
| Custom Vision - eğitim   | `CustomVision.Training`   |
| Yüz tanıma API'si                   | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |
| Mürekkep Tanıma             | `InkRecognizer`           |

### <a name="search"></a>Ara

| Hizmet            | tür                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Konuşma

| Hizmet            | tür                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

### <a name="language"></a>Dil

| Hizmet            | tür                |
|--------------------|---------------------|
| Formu anlama | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

### <a name="decision"></a>Karar verme

| Hizmet           | tür               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |

Kullanılabilir bir Bilişsel hizmet listesi ile "tür" bulabilirsiniz [az cognitiveservices account liste türleri](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) komutu:

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Yeni bir kaynak, kaynak grubunuza ekleyin

Oluşturun ve yeni bir Bilişsel hizmetler kaynağa abone olmak için kullanın [az cognitiveservices hesabının oluşturma](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) komutu. Bu komut, daha önce oluşturduğunuz kaynak grubunu yeni bir Faturalandırılabilir kaynak ekler. Yeni kaynak oluşturulurken, fiyatlandırma Katmanı (veya sku ile birlikte), kullanmak istediğiniz hizmet "tür" bilmeniz gerekir ve bir Azure konumu:

F0 (ücretsiz) kaynak Anomali adlı algılayıcısı için oluşturabileceğiniz `anomaly-detector-resource` ile aşağıdaki komutu.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-subscription"></a>Aboneliğiniz için anahtarları alma

Komut satırı arabirimi yerel yüklemesinde açmak için kullandığınız [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutu.

```console
az login
```

Kullanım [az cognitiveservices account anahtarları listesi](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) Bilişsel hizmet kaynağınızın anahtarlarını almak için komutu.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Temizleme ve Bilişsel hizmetler abonelik kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, kaynak grubuyla ilişkili diğer tüm kaynakları siler.

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için az group delete komutunu kullanın.

```azurecli-interactive
az group delete --name storage-resource-group
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel hizmetler isteklerine kimlik doğrulaması](authentication.md)
* [Azure Bilişsel hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)
