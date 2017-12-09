---
title: "Azure storage'da güvenli aktarımı gerektiren | Microsoft Docs"
description: "\"Güvenli aktarım gerekli\" özelliği Azure Storage ve bunun nasıl etkinleştirileceğini öğrenin."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: 797ac45a41cdf655e7465a01875a0394081c08a7
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="require-secure-transfer-in-azure-storage"></a>Azure storage'da güvenli aktarımı gerektirir

"Güvenli aktarım gerekli" seçeneği, yalnızca istekleri hesabına güvenli bağlantılardan vererek depolama hesabınızın güvenliğini artırır. Örneğin, depolama hesabınıza erişmek için REST API'leri arıyoruz, HTTPS kullanarak bağlanmanız gerekir. "Güvenli gerekli aktarım" HTTP kullanan istekleri reddeder.

Azure dosya hizmeti kullandığınızda, "güvenli aktarım gerekli" etkinleştirilmişse, şifreleme olmadan herhangi bir bağlantı başarısız olur. Bu, SMB 2.1, SMB 3.0 şifreleme olmadan ve bazı Linux SMB istemcisi sürümleri kullanan senaryolar içerir. 

Varsayılan olarak "güvenli aktarım gerekli" seçeneği devre dışıdır.

> [!NOTE]
> Azure depolama özel etki alanı adları HTTPS desteklemediğinden özel etki alanı kullanırken bu seçenek uygulanmıyor. Ve klasik depolama hesapları desteklenmez.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>"Güvenli aktarım gerekli" Azure portalında etkinleştir

"Depolama hesabı oluşturduğunuzda, ayarı gerekli güvenli aktarımı" açabilirsiniz [Azure portal](https://portal.azure.com). Ayrıca var olan depolama hesapları için etkinleştirebilirsiniz.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Güvenli aktarımı için yeni bir depolama hesabı gerektirir

1. Açık **depolama hesabı oluşturma** Azure portalında bölmesi.
1. Altında **güvenli aktarımı gerekli**seçin **etkin**.

  ![Depolama hesabı dikey penceresi oluşturma](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Güvenli aktarımı için varolan bir depolama hesabı gerektirir

1. Azure Portalı'nda mevcut bir depolama hesabını seçin.
1. Depolama alanında menü bölmesi altında hesap **ayarları**seçin **yapılandırma**.
1. Altında **güvenli aktarımı gerekli**seçin **etkin**.

  ![Depolama hesabı menü bölmesi](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>"Güvenli aktarım gerekli" etkinleştirmek program aracılığıyla

Güvenli aktarımı program aracılığıyla gerektirecek şekilde ayarı kullanmak _supportsHttpsTrafficOnly_ REST API, Araçlar ya da kitaplıkları ile depolama hesabı özellikleri:

* [REST API](https://docs.microsoft.com/rest/api/storagerp/storageaccounts) (sürüm: 2016-12-01)
* [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0) (sürüm: 4.1.0'da)
* [CLI](https://pypi.python.org/pypi/azure-cli-storage/2.0.11) (sürüm: 2.0.11)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/) (sürüm: 1.1.0)
* [.NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview) (sürüm: 6.3.0)
* [Python SDK](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0) (sürüm: 1.1.0)
* [Söyleniş SDK](https://rubygems.org/gems/azure_mgmt_storage) (sürüm: 0.11.0)

### <a name="enable-secure-transfer-required-setting-with-powershell"></a>"PowerShell ile ayarlanması gereken güvenli aktarımı" etkinleştirme

Bu örnek, Azure PowerShell modülü 4.1 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Çalıştırma `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

 Ayarını denetlemek için aşağıdaki komut satırını kullanın:

```powershell
> Get-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

Ayarı etkinleştirmek için aşağıdaki komut satırını kullanın:

```powershell
> Set-AzureRmStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

### <a name="enable-secure-transfer-required-setting-with-cli"></a>"CLI ile ayarlanması gereken güvenli aktarımı" etkinleştirme

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 Ayarını denetlemek için aşağıdaki komut satırını kullanın:

```azurecli-interactive
> az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

Ayarı etkinleştirmek için aşağıdaki komut satırını kullanın:

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
Azure depolama birlikte geliştiricilerin güvenli uygulamalar oluşturmasını sağlama güvenlik özellikleri kapsamlı bir kümesini sağlar. Daha fazla ayrıntı için Git [depolama Güvenlik Kılavuzu](storage-security-guide.md).
