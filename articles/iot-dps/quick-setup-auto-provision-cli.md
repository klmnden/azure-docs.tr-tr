---
title: Azure CLI kullanarak Cihaz Sağlama Hizmeti'ni ayarlama | Microsoft Docs
description: Azure Hızlı Başlangıç - Azure CLI kullanarak Azure IoT Hub Cihazı Sağlama Hizmeti’ni ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 02/26/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 3062fb640985498ba35e23f6310828a2bd59bfed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60363725"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-azure-cli"></a>Azure CLI ile IoT Hub Cihazı Sağlama Hizmeti'ni ayarlama

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu Hızlı Başlangıç, Azure CLI kullanarak IoT hub ile IoT Hub Cihazı Sağlama Hizmeti oluşturma ve iki hizmeti birbirine bağlama işlemlerinin ayrıntılarını içerir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> Bu Hızlı Başlangıçta oluşturduğunuz hem IoT hub hem de sağlama hizmeti, DNS uç noktaları olarak herkes tarafından bulunabilir olacaktır. Bu kaynaklar için kullanılan adları değiştirmeye karar verirseniz, hassas bilgiler kullanmaktan kaçının.
>



## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *westus* konumunda *my-sample-resource-group* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name my-sample-resource-group --location westus
```

> [!TIP]
> Örnekte kaynak grubu Batı ABD konumunda oluşturulur. `az account list-locations -o table` komutunu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz.
>
>

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[az iot hub create](/cli/azure/iot/hub#az-iot-hub-create) komutuyla bir IoT hub oluşturun.

Aşağıdaki örnekte, *westus* konumunda *my-sample-hub* adlı bir IoT hub oluşturulur.  

```azurecli-interactive 
az iot hub create --name my-sample-hub --resource-group my-sample-resource-group --location westus
```

## <a name="create-a-provisioning-service"></a>Sağlama hizmeti oluşturma

[az iot dps create](/cli/azure/iot/dps#az-iot-dps-create) komutuyla bir sağlama hizmeti oluşturun. 

Aşağıdaki örnekte, *westus* konumunda *my-sample-dps* adlı bir sağlama hizmeti oluşturulur.  

```azurecli-interactive 
az iot dps create --name my-sample-dps --resource-group my-sample-resource-group --location westus
```

> [!TIP]
> Örnekte sağlama hizmeti Batı ABD konumunda oluşturulur. `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırarak veya [Azure Durumu](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" için arama yaparak kullanılabilir konumların listesini görüntüleyebilirsiniz. Komutlarda, konumlar tek sözcük veya birden çok sözcük biçiminde belirtilebilir; örneğin: westus, West US, WEST US, Batı ABD, BATI, ABD, vb. Değer büyük/küçük harfe duyarlı değildir. Konumu belirtirken birden çok sözcük biçimini kullanırsanız, değeri çift tırnak içine alın; örneğin, `-- location "West US"`.
>


## <a name="get-the-connection-string-for-the-iot-hub"></a>IoT hub için bağlantı dizesini alma

Cihaz Sağlama Hizmeti ile arasında bağlantı kurmak için IoT hub'ınızın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini almak için [az iot hub show-connection-string](/cli/azure/iot/hub#az-iot-hub-show-connection-string) komutunu kullanın ve komutunu çıkışını kullanarak iki kaynağı bağlarken kullanacağınız değişkeni ayarlayın. 

Aşağıdaki örnekte *hubConnectionString* değişkeni, hub'ın *iothubowner* ilkesinin birincil anahtarına ilişkin bağlantı dizesinin değerine ayarlanır. `--policy-name` parametresiyle farklı bir ilke de belirtebilirsiniz. Komut, komut çıkışından bağlantı dizesini ayıklamak için Azure CLI [query](/cli/azure/query-azure-cli) ve [output](/cli/azure/format-output-azure-cli#tsv-output-format) seçeneklerini kullanır.

```azurecli-interactive 
hubConnectionString=$(az iot hub show-connection-string --name my-sample-hub --key primary --query connectionString -o tsv)
```

Bağlantı dizesini görmek için `echo` komutunu kullanabilirsiniz.

```azurecli-interactive 
echo $hubConnectionString
```

> [!NOTE]
> Bu iki komut, Bash altında çalışan konaklar için geçerlidir. Yerel bir Windows/CMD kabuğu veya PowerShell konağı kullanıyorsanız, söz konusu ortama yönelik doğru söz dizimini kullanacak şekilde komutları değiştirmeniz gerekir.
>

## <a name="link-the-iot-hub-and-the-provisioning-service"></a>IoT hub ile cihaz sağlama hizmetini bağlama

IoT hub ile sağlama hizmetinizi bağlamak için [az iot dps linked-hub create](/cli/azure/iot/dps/linked-hub#az-iot-dps-linked-hub-create) komutunu kullanın. 

Aşağıdaki örnekte *wesus* konumundaki *my-sample-hub* adlı bir IoT hub ile *my-sample-dps* adlı Cihaz Sağlama hizmeti bağlanır. Önceki adımdan *hubConnectionString* değişkeninde depolanan *my-sample-hub* bağlantı dizesi kullanılır.

```azurecli-interactive 
az iot dps linked-hub create --dps-name my-sample-dps --resource-group my-sample-resource-group --connection-string $hubConnectionString --location westus
```

## <a name="verify-the-provisioning-service"></a>Sağlama hizmetini doğrulama

[az iot dps show](/cli/azure/iot/dps#az-iot-dps-show) komutuyla sağlama hizmetinizin ayrıntılarını alın.

Aşağıdaki örnekte *my-sample-dps* adlı sağlama hizmetinin ayrıntıları alınır. Bağlı IoT hub, *properties.iotHubs* koleksiyonunda gösterilir.

```azurecli-interactive
az iot dps show --name my-sample-dps
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Sonraki Hızlı Başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutları kullanarak sağlama hizmetini, IoT hub'ı veya kaynak grubuyla bu grubun tüm kaynaklarını silebilirsiniz.

Sağlama hizmetini silmek için [az iot dps delete](/cli/azure/iot/dps#az-iot-dps-delete) komutunu çalıştırın:

```azurecli-interactive
az iot dps delete --name my-sample-dps --resource-group my-sample-resource-group
```
IoT hub'ı silmek için [az iot hub delete](/cli/azure/iot/hub#az-iot-hub-delete) komutunu çalıştırın:

```azurecli-interactive
az iot hub delete --name my-sample-hub --resource-group my-sample-resource-group
```

Kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için [az group delete](/cli/azure/group#az-group-delete) komutunu çalıştırın:

```azurecli-interactive
az group delete --name my-sample-resource-group
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT hub'ını ve bir Cihaz Sağlama Hizmeti örneği dağıttınız ve iki kaynağı birbirine bağladınız. Bu yöntemi bir sanal cihaz sağlamak üzere kullanmayı öğrenmek için sanal cihaz oluşturma Hızlı Başlangıcına geçin.

> [!div class="nextstepaction"]
> [Sanal cihaz oluşturmak için hızlı başlangıç](./quick-create-simulated-device.md)

