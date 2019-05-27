---
title: Azure hızlı başlangıç - Azure Resource Manager şablonu kullanarak bir Azure anahtar kasası ve gizli dizi oluşturma | Microsoft Docs
description: Azure anahtar kasaları oluşturma ve Azure Resource Manager şablonu kullanarak parola kasalarına ekleme gösteren hızlı başlangıç.
services: key-vault
author: mumian
manager: dougeby
tags: azure-resource-manager
ms.service: key-vault
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/22/2019
ms.author: jgao
ms.openlocfilehash: 802c0409fe3ac88f73c383958d2337be09ef7992
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66016472"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-resource-manager-template"></a>Hızlı Başlangıç: Ayarlayın ve Resource Manager şablonu kullanarak Azure Key Vault gizli dizi alma

[Azure Key Vault](./key-vault-overview.md) anahtarları, parolaları, sertifikaları gibi gizli öğeleri ve diğer gizli dizileri güvenli bir depoya sağlayan bir bulut hizmetidir. Bu hızlı başlangıçta bir key vault ile bir gizli dizi oluşturmak için Resource Manager şablonu dağıtma işlemini üzerinde odaklanır. Resource Manager şablonları geliştirme hakkında daha fazla bilgi için bkz. [Resource Manager belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.keyvault/allversions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonda izinlerin yapılandırılması için Azure AD kullanıcı nesnesi kimliğiniz gerekir. Aşağıdaki yordam ' % s'nesne kimliği (GUID) alır.

    1. Bunu aşağıdaki Azure PowerShell veya Azure CLI komutunu çalıştırın **deneyin**ve ardından komut kabuğu bölmesine yapıştırın. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**. 

        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --upn-or-object-id $upn --query "objectId" 
        ```

        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        ```

    2. Nesne Kimliği yazma Bu hızlı başlangıçta bir sonraki bölümde ihtiyacınız.

## <a name="create-a-vault-and-a-secret"></a>Bir kasa ve gizli dizi oluşturma

Bu hızlı başlangıçta kullanılan şablon dandır [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-key-vault-create/). Daha fazla Azure anahtar kasası şablonu örnekleri bulunabilir [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault).

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon, bir anahtar kasası ve gizli dizi oluşturur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-key-vault-create%2Fazuredeploy.json"><img src="./media/quick-create-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.  

    ![Resource Manager şablonu Key Vault tümleştirmesi portal dağıtımı](./media/quick-create-template/create-key-vault-using-template-portal.png)

    Belirtilmediyse, anahtar kasası ve gizli dizi oluşturmak için varsayılan değeri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: seçin **Yeni Oluştur**, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**. 
    * **Konum**: Bir konum seçin.  Örneğin, **Orta ABD**.
    * **Key Vault adı**: içinde genel olarak benzersiz olması gereken anahtar kasası için bir ad girin. vault.azure.net ad alanı.  
    * **Kiracı Kimliği**: Şablon işlevi kiracı kimliğinizi otomatik olarak alır.  Varsayılan değeri değiştirmeyin.
    * **Ad kullanıcı kimliği**: aldığınız Azure AD kullanıcı nesne kimliği girin [önkoşulları](#prerequisites).
    * **Gizli dizi adı**: anahtar kasasında depolama gizli dizi için bir ad girin.  Örneğin, **adminpassword**.
    * **Gizli değer**: gizli değer girin.  Bir parola depoluyorsanız Önkoşullarında oluşturduğunuz oluşturulan parola kullanmak için önerilir.
    * **Yukarıdaki hüküm ve koşulları durumu için kabul ediyorum**: Seçin.
3. **Satın al**'ı seçin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Anahtar kasası ve gizli anahtarını denetlemek için Azure portalını kullanabilir veya oluşturulan gizli listelemek için aşağıdaki Azure CLI veya Azure PowerShell betiğini kullanın.

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault secret list --vault-name $keyVaultName
```

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultSecret -vaultName $keyVaultName
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Azure CLI veya Azure Powershell kullanarak kaynak grubunu silmek için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```
```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault Ana Sayfası](https://azure.microsoft.com/services/key-vault/)
* [Azure Key Vault Belgeleri](https://docs.microsoft.com/azure/key-vault/)
* [Node için Azure SDK](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/)
