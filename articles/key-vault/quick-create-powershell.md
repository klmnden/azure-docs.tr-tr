---
title: "Azure Hızlı Başlangıç: PowerShell kullanarak Key Vault'tan gizli dizi ayarlama ve alma | Microsoft Docs"
description: ''
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 970fcb19ce696b46f9c9cfcb6b609b35d0964580
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461434"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-powershell"></a>Hızlı Başlangıç: Ayarlayın ve PowerShell kullanarak Azure Key Vault gizli dizi alma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [Genel Bakış](key-vault-overview.md) bölümünü inceleyebilirsiniz. Bu hızlı başlangıçta PowerShell kullanarak bir anahtar kasası oluşturacaksınız. Ardından yeni oluşturulan kasada bir gizli dizi depolayacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Tür `$PSVersionTable.PSVersion` sürümü bulmak için. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Ardından bir Key Vault oluşturacaksınız. Bu adımı uygularken bazı bilgiler gereklidir:

Bu hızlı başlangıçta Anahtar Kasamızın adı olarak “Contoso KeyVault2” kullansak da sizin benzersiz bir ad kullanmanız gerekir.

- **Kasa adı** Contoso-Vault2.
- **Kaynak grubu adı** ContosoResourceGroup.
- **Konum** Doğu ABD.

```azurepowershell-interactive
New-AzKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

Bu cmdlet’in çıktısı, yeni oluşturulan anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

* **Kasa adı**: Olan örnekte **Contoso-Vault2**. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **Kasa URI'si**: Bu örnekte, https://contosokeyvault.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Kasa oluşturma sonrasında Azure hesabınız bu yeni kasa üzerinde herhangi bir işlem yapmasına izin verilen tek hesaptır.

![Anahtar Kasası oluşturma komutu tamamlandıktan sonraki çıktı](./media/quick-create-powershell/output-after-creating-keyvault.png)

## <a name="adding-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç adım uygulamanız gerekir. Bu örnekte, bir uygulama tarafından kullanılabilecek bir parola ekleyeceksiniz. Parola olarak adlandırılır **ExamplePassword** ve değerini depolar **hVFkk965BuUv** da.

Değerini dönüştürmeniz **hVFkk965BuUv** yazarak güvenli bir dize için:

```azurepowershell-interactive
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
```

Ardından, adlı Key Vault'ta bir gizli dizi oluşturmak için aşağıdaki PowerShell komutlarını yazın **ExamplePassword** değerle **hVFkk965BuUv** :

```azurepowershell-interactive
$secret = Set-AzKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'ExamplePassword' -SecretValue $secretvalue
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurepowershell-interactive
(Get-AzKeyVaultSecret -vaultName "Contosokeyvault" -name "ExamplePassword").SecretValueText
```

Artık bir Key Vault oluşturdunuz, bir gizli dizli depoladınız ve bunu aldınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Diğer hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz.

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu, Key Vault komutunu ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturdunuz ve içinde bir yazılım anahtarı depoladınız. Key Vault ve bunu uygulamalarınızla nasıl kullanabileceğiniz hakkında daha fazla bilgi almak için, Key Vault ile birlikte çalışan web uygulamalarına yönelik öğreticiye geçin.

Azure kaynakları için yönetilen kimliklerle bir web uygulamasından Key Vault'tan bir gizli dizi okumak öğrenmek için şu öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Key vault'tan bir gizli dizi okumak için bir Azure web uygulaması yapılandırma](quick-create-net.md).
