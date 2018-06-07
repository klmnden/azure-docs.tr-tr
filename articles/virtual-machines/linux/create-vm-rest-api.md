---
title: Azure REST API ile Linux sanal makine oluşturma | Microsoft Docs
description: Azure REST API'si ile yönetilen diskleri ve SSH kimlik doğrulaması kullanan azure'da bir Linux sanal makine oluşturmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: e3f41bea26e9a5ff45b31ae9d9a2e5955317ad7a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825381"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>REST API ile SSH kimlik doğrulaması kullanan bir Linux sanal makine oluşturun

Azure'da sanal makine (VM), konum, donanım boyutu, işletim sistemi görüntüsü ve oturum açma kimlik bilgileri gibi çeşitli parametreleri tarafından tanımlanır. Bu makalede REST API SSH kimlik doğrulaması kullanan bir Linux sanal makine oluşturmak için nasıl kullanılacağı gösterilmektedir.

Bir sanal makine oluşturulamıyor veya güncelleştirilemiyor için aşağıdakileri kullanın *PUT* işlemi:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Bir isteği oluşturun

Oluşturmak için *PUT* isteği `{subscription-id}` parametresi gereklidir. Birden çok aboneliğiniz varsa, bkz: [birden çok abonelik ile çalışma](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). Tanımladığınız bir `{resourceGroupName}` ve `{vmName}` , kaynaklar için ile birlikte `api-version` parametresi. Bu makalede kullanan `api-version=2017-12-01`.

Aşağıdaki üst bilgiler gereklidir:

| İstek üstbilgisi   | Açıklama |
|------------------|-----------------|
| *Content-Type:*  | Gereklidir. Kümesine `application/json`. |
| *Yetkilendirme:* | Gereklidir. Geçerli bir ayarla `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

İstek oluşturma hakkında daha fazla bilgi için bkz: [bir REST API istek/yanıt bileşenlerinin](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Aşağıdaki ortak tanımlara bir istek gövdesi oluşturmak için kullanılır:

| Ad                       | Gerekli | Tür                                                                                | Açıklama  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | dize                                                                              | Kaynak konumu. |
| ad                       |          | dize                                                                              | Sanal makine için ad. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Sanal makine için donanım ayarlarını belirtir. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Sanal makine disklerinin depolama ayarlarını belirtir. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Sanal makine için işletim sistemi ayarlarını belirtir. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Sanal makine ağ arabirimleri belirtir. |

İstek gövdesindeki kullanılabilir tanımları tam bir listesi için bkz: [sanal makineler oluşturma veya istek gövdesi defintions güncelleştirme](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek istek gövdesi yönetilen Premium diskleri kullanan bir Ubuntu 18.04 LTS görüntüsünü tanımlar. SSH ortak anahtar kimlik doğrulaması kullanılıyorsa ve sahip olduğunuz bir varolan sanal ağ arabirim kartı (NIC) VM kullanır [daha önce oluşturduğunuz](../../virtual-network/virtual-network-network-interface.md). SSH ortak anahtarınızı sağlamanız *osProfile.linuxConfiguration.ssh.publicKeys.keyData* alan. Gerekirse, [SSH anahtar çifti oluşturmalı](mac-create-ssh-keys.md).

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

Bir sanal makine oluşturulamıyor veya güncelleştirilemiyor işlemin iki başarılı yanıt vardır:

| Ad        | Tür                                                                              | Açıklama |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 TAMAM      | [Sanal makinesi](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Tamam          |
| 201 oluşturuldu | [Sanal makinesi](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Oluşturulan     |

REST API yanıtları hakkında daha fazla bilgi için bkz: [yanıt iletisi işlem](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Örnek yanıt

Bir sıkıştırılmış *201 oluşturuldu* VM oluşturur önceki örnek istek gövdesi yanıttan gösterir bir *Vmıd* atandı ve *provisioningState* olan*Oluşturma*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure REST API'leri veya Azure CLI 2.0 veya Azure PowerShell gibi diğer yönetim araçları hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Azure işlem sağlayıcısı REST API'si](/rest/api/compute/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)
- [Azure CLI 2.0](/cli/azure/)
- [Azure PowerShell Modülü](/powershell/azure/overview)
