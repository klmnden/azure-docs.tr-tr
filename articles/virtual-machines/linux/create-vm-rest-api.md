---
title: Azure REST API ile bir Linux sanal makinesi oluşturma | Microsoft Docs
description: Azure REST API ile yönetilen diskler ve SSH kimlik doğrulaması kullanan azure'da bir Linux sanal makinesi oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
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
ms.openlocfilehash: 11d9f5efb452d46e5ca30169861582f6f2bbbd1b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969402"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>REST API ile SSH kimlik doğrulaması kullanan bir Linux sanal makinesi oluşturma

Azure sanal makineler'de (VM), konum, donanım boyutu, işletim sistemi görüntüsü ve oturum açma kimlik bilgileri gibi çeşitli parametreleri tarafından tanımlanır. Bu makalede, SSH kimlik doğrulaması kullanan bir Linux sanal makine oluşturmak için REST API kullanma işlemini göstermektedir.

Oluşturun veya bir sanal makineyi güncelleştirmek için aşağıdakileri kullanın *PUT* işlemi:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>İstek oluştur

Oluşturulacak *PUT* isteği `{subscription-id}` parametresi gereklidir. Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). Tanımladığınız bir `{resourceGroupName}` ve `{vmName}` kaynaklarınız için birlikte `api-version` parametresi. Bu makalede `api-version=2017-12-01`.

Aşağıdaki üst bilgiler gereklidir:

| İstek üstbilgisi   | Açıklama |
|------------------|-----------------|
| *İçerik türü:*  | Gereklidir. Kümesine `application/json`. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

İstek oluşturma hakkında daha fazla bilgi için bkz. [bir REST API istek/yanıt bileşenleri](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Aşağıdaki ortak tanımları, istek gövdesi oluşturmak için kullanılır:

| Ad                       | Gerekli | Tür                                                                                | Açıklama  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | dize                                                                              | Kaynak konumu. |
| ad                       |          | dize                                                                              | Sanal makinenin adı. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Sanal makine için donanım ayarlarını belirtir. |
| properties.storageProfile  |          | [Datadisks](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Sanal makine diskleri için depolama ayarlarını belirtir. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Sanal makine için işletim sistemi ayarlarını belirtir. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Sanal makine ağ arabirimleri belirtir. |

İstek gövdesinde kullanılabilir tanımlar tam bir listesi için bkz. [sanal makineleri oluşturma veya güncelleştirme istek gövdesi tanımlarını](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek istek gövdesi, Premium yönetilen diskler kullanan bir Ubuntu LTS 18.04 görüntüsünü tanımlar. SSH ortak anahtarı kimlik doğrulaması kullanılır ve sahip olduğunuz bir var olan sanal ağ arabirim kartı (NIC) VM kullanan [daha önce oluşturduğunuz](../../virtual-network/virtual-network-network-interface.md). SSH ortak anahtarınızı sağlayın *osProfile.linuxConfiguration.ssh.publicKeys.keyData* alan. Gerekirse, [SSH anahtar çifti oluşturma](mac-create-ssh-keys.md).

```json
{
  "location": "eastus",
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
      "computerName": "myVM",
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
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  },
  "name": "myVM"
}
```

## <a name="responses"></a>Yanıtlar

Oluşturulacak veya güncelleştirilecek bir sanal makine işlemi için iki başarılı yanıtlar vardır:

| Ad        | Tür                                                                              | Açıklama |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 TAMAM      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Tamam          |
| 201 oluşturuldu | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Oluşturulan     |

REST API yanıtları hakkında daha fazla bilgi için bkz: [yanıt iletisini işlemek](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Örnek yanıt

Sıkıştırılmış bir *201 oluşturuldu* bir VM oluşturur, önceki örnek istek gövdesi yanıttan gösteren bir *Vmıd* atanmış olan ve *provisioningState* olan*Oluşturma*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure REST API'lerini veya Azure CLI veya Azure PowerShell gibi diğer yönetim araçları hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure işlem sağlayıcısı REST API'si](/rest/api/compute/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)
- [Azure CLI](/cli/azure/)
- [Azure PowerShell Modülü](/powershell/azure/overview)
