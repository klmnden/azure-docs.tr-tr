---
title: Terraform arka uç olarak Azure Depolama'yı kullanma
description: Terrafom durumu Azure depolamada depolamak için bir giriş.
services: terraform
author: neilpeterson
ms.service: terraform
ms.topic: article
ms.date: 09/13/2018
ms.author: nepeters
ms.openlocfilehash: c27c6bc5f2071203c9a9dd5a94e73c0cb4626598
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45608308"
---
# <a name="store-terraform-state-in-azure-storage"></a>Azure Depolama'daki Store Terraform durumu

Terraform durumu, Terraform yapılandırmaları ile dağıtılan kaynakları karşılaştırmak için kullanılır. Durum kullanan, Terraform eklemek, güncelleştirmek veya silmek için hangi Azure kaynakları bilir. Varsayılan olarak, Terraform durumu yerel olarak çalıştırılırken depolanır *Terraform uygulamak*. Bu yapılandırma birkaç nedenlerden dolayı ideal değildir:

- Yerel durumu iyi bir ekip veya ofisinizin çalışmıyor
- Terraform durumu hassas bilgiler içerebilir.
- Yanlışlıkla silme işlemi olasılığını artırır durumunu yerel olarak depolama

Terraform, Terraform durumu için Uzaktan Depolama olduğu bir durum arka ucu kavramını içerir. Bir durum arka ucu kullanırken, durum dosyası Azure depolama gibi bir veri deposunda depolanır. Bu belge, yapılandırma ve Terraform durumu arka uç olarak Azure depolama kullanma işlemi açıklanmaktadır.

## <a name="configure-storage-account"></a>Depolama hesabı yapılandırın

Azure depolama arka uç olarak kullanmadan önce bir depolama hesabı oluşturulmalıdır. Depolama hesabı, Azure portalı, PowerShell, Azure CLI veya kendisini Terraform ile oluşturulabilir. Azure CLI ile depolama hesabı yapılandırmak için aşağıdaki örneği kullanın.

```azurecli-interactive
#!/bin/bash

RESOURCE_GROUP_NAME=tfstatestorage
STORAGE_ACCOUNT_NAME=tfstatestorage$RANDOM
CONTAINER_NAME=tfstatestorage

# Ceeate resoruce group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY

echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "ARM_ACCESS_KEY: $ACCOUNT_KEY"
```

Depolama hesabı adı, kapsayıcı adı ve depolama erişim anahtarını not edin. Bu değerler, uzak durumu yapılandırırken gereklidir.

## <a name="configure-state-backend"></a>Yapılandırma durumu arka uç

Terraform durumu arka uç çalıştırırken yapılandırılmış *Terraform init*. Durum arka uç yapılandırmak için aşağıdaki veriler gereklidir.

- storage_account_name - Azure depolama hesabı adı.
- container_name - blob kapsayıcısının adı.
- anahtarı - dosyasının oluşturulmasını durumun adını depolar.
- access_key - depolama erişim tuşu'nu tıklatın.

İçin bir ortam değişkenini kullanmak için önerilir ancak bu değerlerin her birini Terraform yapılandırma dosyası veya komut satırında belirtilebilir `access_key`. Bir ortam değişkeni engel anahtar yazılmakta olan disk için.

Adlı bir ortam değişkenini oluşturmak `ARM_ACCESS_KEY` Azure depolama erişim anahtarı değerine sahip.

```console
export ARM_ACCESS_KEY=<storage access key>
```

Azure depolama hesabı erişim anahtarı daha iyi korumak için Azure anahtar Kasası'nda depolayın. Ortam değişkenini sonra aşağıdakine benzer bir komutu kullanılarak ayarlanabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault belgelerindeki][azure-key-vault].

```console
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Terraform arka uç kullanılacak şekilde yapılandırmak için dahil bir *arka uç* türünde yapılandırma *azurerm* Terraform yapılandırması içinde. Ekleme *storage_account_name*, *container_name*, ve *anahtar* yapılandırma bloğuna değerleri.

Aşağıdaki örnek bir Terraform arka ucu yapılandırır ve oluşturur ve Azure kaynak grubu.

```json
terraform {
  backend "azurerm" {
    storage_account_name  = "tstate"
    container_name        = "tfstate"
    key                   = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "state-demo-secure" {
  name     = "state-demoe"
  location = "eastus"
}
```

Şimdi, yapılandırmayla başlatma *Terraform init* ve yapılandırmasıyla çalıştırın *Terraform uygulamak*. Tamamlandığında, durum dosyası Azure depolama Blobunda bulabilirsiniz.

## <a name="state-locking"></a>Durum kilitleme

Azure depolama blobu durumu depolama için kullanılırken, blob durumunu yazan herhangi bir işlemden önce otomatik olarak kilitlenir. Bu yapılandırma bozulmaya neden olabilir birden çok eş zamanlı durumu işlemleri engeller. Daha fazla bilgi için [durumu kilitleme] [ terraform-state-lock] Terraform belgeleri.

Kilit bkz: blob Azure portalı veya diğer Azure Yönetim Araçları ancak incelerken olabilir.

![Kilit ile Azure blob](media/terraform-backend/lock.png)

## <a name="next-steps"></a>Sonraki adımlar

Yedeklenen Terraform yapılandırmasını hakkında daha fazla bilgi [Terraform arka uç belgelerine][terraform-backend].

<!-- LINKS - external -->
[azure-key-vault]: ../key-vault/quick-create-cli.md
[terraform-azurerm]: https://www.terraform.io/docs/backends/types/azurerm.html
[terraform-backend]: https://www.terraform.io/docs/backends/
[terraform-state-lock]: https://www.terraform.io/docs/state/locking.html
