---
title: Azure REST API ile bir Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure REST API ile yönetilen diskler ve SSH kimlik doğrulaması kullanan azure'da bir Linux sanal makinesi oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: a7f624bc85d35048a8f9afa0f527ae592a24fbf1
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667938"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>REST API ile SSH kimlik doğrulaması kullanan bir Linux sanal makinesi oluşturma

Diskler gibi çeşitli kaynaklar azure'da bir Linux sanal makinesi (VM) oluşur ve ağ arabirimleri ve konum, boyutu ve işletim sistemi görüntüsü ve kimlik doğrulama ayarları gibi parametreleri tanımlar.

Azure portal, Azure CLI 2. 0'da, birçok Azure SDK'ları, Azure Resource Manager şablonları ve Ansible veya Terraform gibi çok sayıda üçüncü taraf araçları aracılığıyla Linux VM oluşturabilirsiniz. Bu araçlar, sonuçta Linux VM oluşturmak için REST API kullanın.

Bu makalede yönetilen diskler ve SSH kimlik doğrulaması ile Ubuntu 18.04-LTS çalıştıran bir Linux VM oluşturmak için REST API'sini kullanmayı gösterir.

## <a name="before-you-start"></a>Başlamadan önce

Oluşturma ve isteği göndermek için önce ihtiyacınız olacak:

* `{subscription-id}` Aboneliğiniz için
  * Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)
* A `{resourceGroupName}` önceden oluşturduğunuz
* A [sanal ağ arabirimi](../../virtual-network/virtual-network-network-interface.md) aynı kaynak grubunda
* SSH anahtar çifti (yapabilecekleriniz [yeni bir tane oluşturmanız](mac-create-ssh-keys.md) tane yoksa)

## <a name="request-basics"></a>İstek temelleri

Oluşturun veya bir sanal makineyi güncelleştirmek için aşağıdakileri kullanın *PUT* işlemi:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

Ek olarak `{subscription-id}` ve `{resourceGroupName}` parametreleri belirtmeniz gerekir `{vmName}` (`api-version` isteğe bağlıdır, ancak bu makalede ile test edilmiştir `api-version=2017-12-01`)

Aşağıdaki üst bilgiler gereklidir:

| İstek üstbilgisi   | Açıklama |
|------------------|-----------------|
| *Content-Type:*  | Gerekli. Kümesine `application/json`. |
| *Authorization:* | Gerekli. Geçerli bir kümesi `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

REST API istekleri ile çalışma hakkında genel bilgi için bkz. [bir REST API istek/yanıt bileşenleri](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Aşağıdaki ortak tanımları, istek gövdesi oluşturmak için kullanılır:

| Ad                       | Gerekli | Tür                                                                                | Açıklama  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | Doğru     | dize                                                                              | Kaynak konumu. |
| name                       |          | dize                                                                              | Sanal makinenin adı. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Sanal makine için donanım ayarlarını belirtir. |
| properties.storageProfile  |          | [Datadisks](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Sanal makine diskleri için depolama ayarlarını belirtir. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Sanal makine için işletim sistemi ayarlarını belirtir. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Sanal makine ağ arabirimleri belirtir. |

Bir örnek istek gövdesi, aşağıda verilmiştir. VM adı belirttiğinizden emin olun `{computerName}` ve `{name}` parametreler, altında oluşturduğunuz ağ arabirimi adı `networkInterfaces`, kullanıcı adınıza `adminUsername` ve `path`ve *genel*bölümü, SSH anahtar çifti ('de, örneğin, bulunan `~/.ssh/id_rsa.pub`) içinde `keyData`. Diğer parametreleri değiştirmek isteyebilirsiniz `location` ve `vmSize`.  

```json
{
  "location": "eastus",
  "name": "{vmName}",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "{vmName}",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  }
}
```

İstek gövdesinde kullanılabilir tanımlar tam bir listesi için bkz. [sanal makineleri oluşturma veya güncelleştirme isteği gövdesi tanımları](/rest/api/compute/virtualmachines/createorupdate#definitions).

## <a name="sending-the-request"></a>İstek gönderiliyor

Bu HTTP isteği gönderilirken tercihinizi istemci kullanabilir. Ayrıca bir [tarayıcı içi araç](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate) tıklayarak **deneyin** düğmesi.

### <a name="responses"></a>Responses

Oluşturulacak veya güncelleştirilecek bir sanal makine işlemi için iki başarılı yanıtlar vardır:

| Ad        | Tür                                                                              | Açıklama |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 TAMAM      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Tamam          |
| 201 oluşturuldu | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Oluşturuldu     |

Sıkıştırılmış bir *201 oluşturuldu* bir VM oluşturur, önceki örnek istek gövdesi yanıttan gösteren bir *Vmıd* atanmış olan ve *provisioningState* olan*Oluşturma*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

REST API yanıtları hakkında daha fazla bilgi için bkz: [yanıt iletisini işlemek](/rest/api/azure/#process-the-response-message).

## <a name="next-steps"></a>Sonraki adımlar

Azure REST API'lerini veya Azure CLI veya Azure PowerShell gibi diğer yönetim araçları hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure işlem sağlayıcısı REST API'si](/rest/api/compute/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)
- [Azure CLI](/cli/azure/)
- [Azure PowerShell Modülü](/powershell/azure/overview)
