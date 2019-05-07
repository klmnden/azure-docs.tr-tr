---
title: Azure Depolama'daki güvenli aktarım gerektir | Microsoft Docs
description: Azure depolama ve nasıl etkinleştirileceği konusunda "güvenli aktarım gereklidir" özelliği hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/20/2017
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 7239e7fbe1221acc3c302260045d6fc510db2cbe
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148566"
---
# <a name="require-secure-transfer-in-azure-storage"></a>Azure Depolama'daki güvenli aktarım gerektir

"Güvenli aktarım gereklidir" seçeneği, yalnızca istekleri hesabınıza güvenli bağlantılar elde vererek depolama hesabınızın güvenliğini artırır. Örneğin, depolama hesabınıza erişmek için REST API çağırdığınızda, HTTPS kullanarak bağlanmanız gerekir. "Güvenli aktarım gereklidir", HTTP kullanan istekleri reddeder.

Azure dosyaları hizmeti kullandığınızda, "güvenli aktarım gereklidir" etkinleştirildiğinde şifreleme olmadan herhangi bir bağlantı başarısız olur. Bu, SMB 2.1, şifrelemesiz SMB 3.0 ve Linux SMB istemcisinin bazı sürümleri kullanan senaryolar içerir. 

SDK'sı ile bir depolama hesabı oluşturduğunuzda varsayılan olarak "güvenli aktarım gereklidir" seçeneği devre dışıdır. Ve Azure Portalı'nda bir depolama hesabı oluşturduğunuzda varsayılan olarak etkindir.

> [!NOTE]
> Azure depolama özel etki alanı adları için HTTPS'yi desteklemediğinden özel etki alanı kullanırken bu seçenek uygulanmaz. Ve klasik depolama hesapları desteklenmez.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>Azure portalında "güvenli aktarım gereklidir"'i etkinleştir

"Ayar, bir depolama hesabı oluştururken güvenli aktarım gerekli" açabilirsiniz [Azure portalında](https://portal.azure.com). Ayrıca var olan depolama hesapları için etkinleştirebilirsiniz.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Yeni bir depolama hesabı için güvenli aktarım gerektir

1. Açık **depolama hesabı oluşturma** bölmesinde Azure portalında.
1. Altında **güvenli aktarım gerekli**seçin **etkin**.

   ![Depolama hesabı dikey penceresi oluşturma](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Mevcut bir depolama hesabı için güvenli aktarım gerektir

1. Azure portalında mevcut bir depolama hesabını seçin.
1. Menü bölmesinde, depolama hesabı **ayarları**seçin **yapılandırma**.
1. Altında **güvenli aktarım gerekli**seçin **etkin**.

   ![Depolama hesabı menü bölmesi](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>"Güvenli aktarım gereklidir" etkinleştirme program aracılığıyla

Program aracılığıyla güvenli aktarım gerektir için bu ayarı kullanın _supportsHttpsTrafficOnly_ REST API, araçları ve kitaplıkları ile depolama hesabı özellikleri:

* [REST API](https://docs.microsoft.com/rest/api/storagerp/storageaccounts) (sürüm: 2016-12-01)
* [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) (sürüm: 0.7)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (sürüm: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (sürüm: 1.1.0)
* [.NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (sürüm: 6.3.0)
* [Python SDK'sı](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (sürüm: 1.1.0)
* [Ruby SDK'sı](https://rubygems.org/gems/azure_mgmt_storage) (sürüm: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-powershell"></a>"PowerShell ile ayarı güvenli aktarım gerekli" etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu örnek Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

 Ayarı denetlemek için şu komut satırını kullanın:

```powershell
> Get-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

Bu ayarı etkinleştirmek için aşağıdaki komut satırını kullanın:

```powershell
> Set-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

### <a name="enable-secure-transfer-required-setting-with-cli"></a>"İle CLI'yı ayarlama güvenli aktarım gerekli" etkinleştirme

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 Ayarı denetlemek için şu komut satırını kullanın:

```azurecli-interactive
> az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

Bu ayarı etkinleştirmek için aşağıdaki komut satırını kullanın:

```azurecli-interactive
> az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only true
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": true,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

## <a name="next-steps"></a>Sonraki adımlar
Azure depolama, kapsamlı birlikte güvenli uygulamalar oluşturmalarını sağlayan güvenlik özellikleri sağlar. Daha fazla ayrıntı için [depolama Güvenlik Kılavuzu](storage-security-guide.md).
