---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 1ea0b01997d3d5cecff73f951c51de5898c2f07a
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503472"
---
Aşağıdaki Azure CLI komutları bir Anomali algılayıcısı kaynak ücretsiz fiyatlandırma katmanında sağlanır. Tıklayarak **deneyin** düğmesi, kodu yapıştırın ve enter tuşuna basın Azure cloud Shell'i çalıştırılacak. Bu, uygulamanızın kimliğini doğrulamak için anahtarlarınızı yazdırır. Tamamlandıktan sonra [bir ortam değişkenini oluşturmak](../articles/cognitive-services/cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) , anahtarlardan birini için adlı `ANOMALY_DETECTOR_KEY`.

> [!NOTE]
> * İsteğe bağlı olarak şunları yapabilirsiniz:
>    * kullanarak bir kaynak oluşturun [Azure portalında](../articles/cognitive-services/cognitive-services-apis-create-account.md), veya [Azure CLI](../articles/cognitive-services/cognitive-services-apis-create-account.md) yerel makinenizde.
>    * Alma bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) geçerli 7 gün boyunca ücretsiz. Bunun imzalama üzerinde kullanıma sunulacak sonra [Azure Web sitesi](https://azure.microsoft.com/try/cognitive-services/my-apis/).
>    * Bu kaynak görünümünde [Azure portalında](https://portal.azure.com/). 

Bilişsel hizmetler, sağladığınız Azure kaynakları tarafından temsil edilir. Tüm Bilişsel Hizmetler hesabı (ve ilişkili Azure kaynağı) bir Azure kaynak grubuna ait olmalıdır.

1. Bir Azure kaynak grubu oluşturun

    ```azurecli-interactive
    az group create \
        --name example-anomaly-detector-resource-group \
        --location westus2
    ```

2. Ücretsiz katmanında bir Anomali algılayıcısı kaynağı oluşturun

    ```azurecli-interactive
    az cognitiveservices account create \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group \
        --kind AnomalyDetector \
        --sku F0 \
        --location westus2 \
        --yes
    ```

3. Kaynağınızın anahtarları listeleme

    ```azurecli-interactive
    az cognitiveservices account keys list \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group
    ```