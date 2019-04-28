---
title: Bir Azure Resource Manager şablonu kullanarak Cihaz Sağlama’yı ayarlama | Microsoft Docs
description: Azure Hızlı Başlangıç - Şablon kullanarak Azure IoT Hub Cihazı Sağlama Hizmeti’ni ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 06/18/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 3360bfa7eed15f72fb78f698e837d887e9c8aa85
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126486"
---
# <a name="set-up-the-iot-hub-device-provisioning-service-with-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu ile IoT Hub Cihazı Sağlama Hizmeti’ni ayarlama

Cihazlarınızın sağlanması için gerekli Azure bulut kaynaklarını programlı olarak ayarlamak için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)’ı kullanabilirsiniz. Bu adımlarda, IoT hub ve yeni bir IoT Hub Cihazı Sağlama Hizmeti oluşturmanın yanı sıra Azure Resource Manager şablonunu kullanarak iki hizmeti birbirine bağlama işlemleri gösterilmektedir. Bu Hızlı Başlangıç, kaynak grubu oluşturmak ve şablonu dağıtmak için gerekli programlı adımları gerçekleştirmek için [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) kullanır. Ancak bu adımları gerçekleştirmek ve şablonunuzu dağıtmak için [Azure portalını](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)’i, .NET’i, ruby’yi veya başka bir programlama dilini kolayca kullanabilirsiniz. 


## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Bu Hızlı Başlangıç, Azure CLI’yı yerel olarak çalıştırmanızı gerektirir. Azure CLI 2.0 veya sonraki bir sürümünü yüklemiş olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).


## <a name="sign-in-to-azure-and-create-a-resource-group"></a>Azure’da oturum açma ve kaynak grubu oluşturma

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde [oturum açma komutunu][lnk-login-command] çalıştırın:
    
    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

2. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Kullanabileceğiniz [Azure hesaplarını listelemek için aşağıdaki komutu][lnk-az-account-command] kullanın:
    
    ```azurecli
    az account list 
    ```

    IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

3. IoT hub’ları ve sağlama hizmetleri gibi Azure bulut kaynaklarını bir kaynak grubunda oluşturursunuz. Mevcut bir kaynak grubunu kullanın veya [kaynak grubu oluşturmak için aşağıdaki komutu][lnk-az-resource-command] çalıştırabilirsiniz:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Önceki örnekte kaynak grubu Batı ABD konumunda oluşturulur. `az account list-locations -o table` komutunu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz.
    >
    >

## <a name="create-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu oluşturma

Kaynak grubunuzda sağlama hizmeti ve bağlı bir IoT hub’ı oluşturmak için bir JSON şablonunu kullanın. Mevcut bir sağlama hizmetinde veya IoT hub’ında değişiklik yapmak için bir Azure Resource Manager şablonu da kullanabilirsiniz.

1. Aşağıdaki çatı içeriğiyle **template.json** adlı bir Azure Resource Manager şablonu oluşturmak için bir metin düzenleyici kullanın. 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": []
   }
   ```

2. **parameters** bölümünü aşağıdaki içerikle değiştirin. parameters bölümü, başka bir dosyadan geçirilebilen parametreleri belirtir. Bu bölüm, oluşturulacak IoT hub’ının ve sağlama hizmetinin adını belirtir. Bu aynı zamanda hem IoT hub’ına hem de sağlama hizmetine ilişkin konumu belirtir. Değerler, IoT hub’larını ve sağlama hizmetlerini destekleyen Azure bölgeleriyle sınırlıdır. Cihaz Sağlama Hizmeti için desteklenen konumların listesi için aşağıdaki `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırabilir veya [Azure Durum](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" araması yapabilirsiniz.

   ```json
    "parameters": {
        "iotHubName": {
            "type": "string"
        },
        "provisioningServiceName": {
            "type": "string"
        },
        "hubLocation": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "westus",
                "westeurope",
                "northeurope",
                "southeastasia",
                "eastasia"
            ]
        }
    },

   ```

3. **variables** bölümünü aşağıdaki içerikle değiştirin. Bu bölüm daha sonra sağlama hizmetinin ve IoT hub’ının bağlanması için gereken IoT hub bağlantı dizesinin oluşturulması için kullanılan değerleri belirtir. 
 
   ```json
    "variables": {        
        "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
        "iotHubKeyName": "iothubowner",
        "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
    },

   ```

4. Bir IoT hub’ı oluşturmak için **kaynaklar** koleksiyonuna aşağıdaki satırları ekleyin. JSON, bir IoT Hub’ının oluşturulması için gerekli özellik alt sınırını belirtir. **name** ve **location** özellikleri parametre olarak geçirilir. Bir şablonda IoT Hub’ı için belirtebileceğiniz özellikler hakkında daha fazla bilgi edinmek için bkz. [Microsoft.Devices/IotHubs şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.devices/iothubs).

   ```json
        {
            "apiVersion": "2017-07-01",
            "type": "Microsoft.Devices/IotHubs",
            "name": "[parameters('iotHubName')]",
            "location": "[parameters('hubLocation')]",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "tags": {
            },
            "properties": {
            }            
        },

   ``` 

5. Sağlama hizmetini oluşturmak için **kaynaklar** koleksiyonundaki IoT hub belirtiminden sonra aşağıdaki satırları ekleyin. Parametrelerde sağlama hizmetinin **name** ve **location** özellikleri geçirilir. **iotHubs** koleksiyonunda sağlama hizmetine bağlanacak IoT hub’larını belirtin. Her bağlı IoT hub’ı için en azından **connectionString** ve **location** özelliklerini belirtmeniz gerekir. Ayrıca her IoT hub’ında **allocationWeight** ve **applyAllocationPolicy** gibi özelliklerin yanı sıra sağlama hizmetinin kendisi üzerinde **allocationPolicy** ve **authorizationPolicies** gibi özellikler de ayarlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Microsoft.Devices/provisioningServices şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.devices/provisioningservices).

   **dependsOn** özelliği, Kaynak Yöneticisi’nin sağlama hizmetini oluşturmadan önce IoT hub’ını oluşturmasını sağlamak için kullanılır. Şablon, önce hub’ın ve hub’a ait anahtarların oluşturulması için IoT hub’ının bağlantı dizesinin sağlama hizmetiyle bağlantısını belirtmesini gerektirir. Şablon, bağlantı dizesini oluşturmak için **concat** ve **listKeys** işlevlerini kullanır. Daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablon işlevleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions).

   ```json
        {
            "type": "Microsoft.Devices/provisioningServices",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "name": "[parameters('provisioningServiceName')]",
            "apiVersion": "2017-11-15",
            "location": "[parameters('hubLocation')]",
            "tags": {},
            "properties": {
                "iotHubs": [
                    {
                        "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                        "location": "[parameters('hubLocation')]",
                        "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                    }
                ]
            },
            "dependsOn": ["[parameters('iotHubName')]"]
        }

   ```

6. Şablon dosyasını kaydedin. Tamamlanan şablon aşağıdaki gibi görünür:

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "iotHubName": {
               "type": "string"
           },
           "provisioningServiceName": {
               "type": "string"
           },
           "hubLocation": {
               "type": "string",
               "allowedValues": [
                   "eastus",
                   "westus",
                   "westeurope",
                   "northeurope",
                   "southeastasia",
                   "eastasia"
               ]
           }
       },
       "variables": {        
           "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
           "iotHubKeyName": "iothubowner",
           "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
       },
       "resources": [
           {
               "apiVersion": "2017-07-01",
               "type": "Microsoft.Devices/IotHubs",
               "name": "[parameters('iotHubName')]",
               "location": "[parameters('hubLocation')]",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "tags": {
               },
               "properties": {
               }            
           },
           {
               "type": "Microsoft.Devices/provisioningServices",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "name": "[parameters('provisioningServiceName')]",
               "apiVersion": "2017-11-15",
               "location": "[parameters('hubLocation')]",
               "tags": {},
               "properties": {
                   "iotHubs": [
                       {
                           "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                           "location": "[parameters('hubLocation')]",
                           "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                       }
                   ]
               },
               "dependsOn": ["[parameters('iotHubName')]"]
           }
       ]
   }
   ```

## <a name="create-a-resource-manager-parameter-file"></a>Bir Kaynak Yöneticisi parametre dosyası oluşturma

Son adımda tanımladığınız şablon, IoT Hub’ının adını, sağlama hizmetinin adını ve bunların oluşturulacağı konumu (Azure bölgesi) belirtmek için parametreleri kullanır. Bu parametreleri ayrı bir dosyada geçirirsiniz. Bunu yapmanız birden fazla dağıtım için aynı şablonu yeniden kullanabilmenizi sağlar. Parametre dosyasını oluşturmak için şu adımları uygulayın:

1. Aşağıdaki çatı içeriğiyle **parameters.json** adlı bir Azure Resource Manager parametre dosyası oluşturmak için bir metin düzenleyici kullanın: 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {}
       }
   }
   ```

2. Parametre bölümüne **iotHubName** değerini ekleyin. Adı değiştirirseniz bu adın bir IoT hub’ına ilişkin uygun adlandırma kurallarına uygun olduğundan emin olun. Bu ad 3-50 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harf, alfasayısal karakter ya da kısa çizgi (-) içerebilir. 

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
    }
   
   ```

3. Parametre bölümüne **provisioningServiceName** değerini ekleyin. Adı değiştirirseniz bu adın, bir IoT Hub Cihazı Sağlama Hizmeti’ne ilişkin adlandırma kurallarına uygun olduğundan emin olun. Bu ad 3-64 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harf, alfasayısal karakter ya da kısa çizgi (-) içerebilir.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
    }

   ```

4. Parametre bölümüne **hubLocation** değerini ekleyin. Bu değer, hem IoT hub’ının hem de sağlama hizmetinin konumunu belirtir. Değerin, şablon dosyasındaki parametre tanımında yer alan **allowedValues** koleksiyonunda belirtilmiş konumlardan birine karşılık gelmesi gerekir. Bu koleksiyon, değerleri hem IoT hub’larını hem de sağlama hizmetlerini destekleyen Azure konumları ile kısıtlar. Cihaz Sağlama Hizmeti için desteklenen konumların listesine, aşağıdaki `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırararak veya [Azure Durum](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" araması yaparak ulaşabilirsiniz.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
        "hubLocation": {
            "value": "westus"
        }
    }

   ```

5. Dosyayı kaydedin. 


> [!IMPORTANT]
> Hem IoT hub’ı hem de sağlama hizmeti, DNS uç noktaları şeklinde genel olarak bulunabilir hale gelir. Bu nedenle, bunları adlandırırken hassas bilgiler belirtmekten kaçının.
>

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonlarınızı dağıtmak ve dağıtımı doğrulamak için aşağıdaki Azure CLI komutlarını kullanın.

1. Şablonunuzu dağıtmak için [bir dağıtım başlatmak üzere aşağıdaki komutu](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create) çalıştırın:
    
    ```azurecli
     az group deployment create -g {your resource group name} --template-file template.json --parameters @parameters.json
    ```

   Çıkışta **provisioningState** özelliğinin "Succeeded" olarak ayarlanmış olup olmadığını kontrol edin. 

   ![Sağlama çıkışı](./media/quick-setup-auto-provision-rm/output.png) 


2. Dağıtımınızı doğrulamak üzere [kaynakları listelemek](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-list) ve çıkışta yeni sağlama hizmetinin yanı sıra IoT hub’ını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurecli
     az resource list -g {your resource group name}
    ```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Sonraki Hızlı Başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız IoT hub’ı veya sağlama hizmeti gibi [ayrı bir kaynağı silmek][lnk-az-resource-command] ya da bir kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için Azure CLI’yı kullanabilirsiniz.

Sağlama hizmetini silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your provisioning service name} --resource-group {your resource group name}
```
Bir IoT hub’ını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Bir kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name {your resource group name}
```

Ayrıca Azure portalını, PowerShell’i, REST API’lerini veya Azure Resource Manager ya da IoT Hub Cihazı Sağlama Hizmeti için yayımlanmış desteklenen platform SDK’larını kullanarak kaynak gruplarını ve ayrı kaynakları da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT hub'ını ve bir Cihaz Sağlama Hizmeti örneği dağıttınız ve iki kaynağı birbirine bağladınız. Bu yöntemi bir sanal cihaz sağlamak üzere kullanmayı öğrenmek için sanal cihaz oluşturma Hızlı Başlangıcına geçin.

> [!div class="nextstepaction"]
> [Sanal cihaz oluşturmak için hızlı başlangıç](./quick-create-simulated-device.md)


<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
