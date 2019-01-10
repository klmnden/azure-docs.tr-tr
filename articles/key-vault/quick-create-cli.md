---
title: "Azure Hızlı Başlangıç: Azure CLI kullanarak Key Vault'tan gizli dizi ayarlama ve alma | Microsoft Docs"
description: Azure CLI kullanarak Azure Key Vault'tan gizli dizi ayarlamayı ve almayı gösteren hızlı başlangıç
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 4acc894f-fee0-4c2f-988e-bc0eceea5eda
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/08/2019
ms.author: barclayn
ms.openlocfilehash: 9b57dfcd6ecd00f9f1bb8ec752e0996ee52305db
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159052"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-azure-cli"></a>Hızlı Başlangıç: Ayarlayın ve Azure CLI kullanarak Azure Key Vault gizli dizi alma

Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [Genel Bakış](key-vault-overview.md) bölümünü inceleyebilirsiniz. Azure CLI, komut veya betikler kullanarak Azure kaynakları oluşturup yönetmek için kullanılır. Bu hızlı başlangıçta bir anahtar kasası oluşturacaksınız. Bu işlemi tamamladıktan sonra bir gizli dizli depolayacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

CLI kullanarak Azure'da oturum açma için şunu yazabilirsiniz:

```azurecli
az login
```

CLI aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için göz atın [oturum Azure CLI ile oturum açın](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *ContosoResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Key Vault oluşturacaksınız. Bazı bilgileri sağlamanız gerekir:

- Bu hızlı başlangıç için **Contoso-vault2** kullanılır. Testinizde benzersiz bir ad sağlamanız gerekir.
- **ContosoResourceGroup** kaynak grubu adı.
- **Doğu ABD** konumu.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

Bu cmdlet’in çıktısı, yeni oluşturulan Key Vault’un özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa adı**: Bu örnekte, olan **Contoso-Vault2**. Bu adı diğer Key Vault komutları için kullanacaksınız.
- **Kasa URI'si**: Bu örnekte, olan https://contoso-vault2.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç ek adım uygulamanız gerekir. Bu parola bir uygulama tarafından kullanılabilir. Parola **ExamplePassword** şeklindedir ve içinde **Pa$$w0rd** değerini depolar.

Key Vault’ta **Pa$$w0rd** değerini depolayacak **ExamplePassword** adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault secret set --vault-name "Contoso-Vault2" --name "ExamplePassword" --value "Pa$$w0rd"
```

Artık Azure Key Vault'a eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/ExamplePassword** komutunu kullanın. 

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "ExamplePassword" --vault-name "Contoso-Vault2"
```

Artık bir Key Vault oluşturdunuz, bir gizli dizli depoladınız ve bunu aldınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerekli değilse, [az group delete](/cli/azure/group#delete) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. Kaynakları aşağıda gösterildiği gibi silebilirsiniz:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturdunuz ve içinde bir gizli dizi depoladınız. Key Vault ve bunu uygulamalarınızla nasıl kullanabileceğiniz hakkında daha fazla bilgi almak için, Key Vault ile birlikte çalışan web uygulamalarına yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> Azure kaynakları için yönetilen kimlikler kullanan bir web uygulamasındaki Key Vault’tan bir gizli diziyi okuma hakkında bilgi almak için aşağıdaki [Bir Azure web uygulamasını Key Vault’tan gizli dizi okuyacak şekilde yapılandırma](quick-create-net.md) öğreticisine geçin.
