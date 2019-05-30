---
title: Azure CLI kullanarak Azure blok zinciri hizmeti oluşturma
description: Azure CLI kullanarak blok zinciri üye oluşturmak için Azure blok zinciri hizmetini kullanın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: 4589a9de4c2a8fa43e4e653d4447c7a7715a6e42
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399953"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak bir Azure blok zinciri hizmet blockchain üye oluştur

Azure Blockchain hizmet İş mantığınızın bir akıllı sözleşme yürütmek için kullanabileceğiniz bir blok zinciri platformudur. Bu hızlı başlangıçta Azure CLI kullanarak blok zinciri üye oluşturarak başlayın gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz, bu hızlı başlangıçta Azure CLI Sürüm 2.0.51 gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Blok zinciri üyesi yeni bir konsorsiyum çekirdek muhasebe Protokolü çalışan Azure blok zinciri hizmeti oluşturun. Çeşitli parametreleri ve özellikleri geçirmek için ihtiyacınız vardır. Örnek parametre değerleriniz ile değiştirin.

```azurecli-interactive
az resource create --resource-group myResourceGroup --name myblockchainmember --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties "{ \"location\": \"eastus\", \"properties\": {\"password\": \"strongMemberAccountPassword@1\", \"protocol\": \"Quorum\", \"consortium\": \"myConsortiumName\", \"consortiumManagementAccountPassword\": \"strongConsortiumManagementPassword@1\" }, \"sku\": { \"name\": \"S0\" } }"
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının oluşturulduğu kaynak grubu adı. Önceki bölümde oluşturduğunuz kaynak grubunu kullanın.
| **name** | Azure Blockchain hizmet blok zinciri üyelik tanımlayan benzersiz bir ad. Ad için genel bir uç nokta adresi kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`.
| **konum** | Blok zinciri üye oluşturulduğu azure bölgesi. Örneğin, `eastus`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
| **Parola** | Üyenin varsayılan işlem düğümü için parola. Parola, blok zinciri üyenin varsayılan işlem düğümü genel uç bağlanırken için temel kimlik doğrulaması kullanın.
| **Consortium** | Katılma veya oluşturma consortium adı.
| **consortiumAccountPassword** | Consortium hesap parolası olarak da bilinen üye hesabı paroladır. Üye hesabı parolası, bu üye için oluşturulan Ethereum hesabı için özel anahtarını şifrelemek için kullanılır. Üye hesabı ve üyesi hesap parolası consortium yönetimi için kullanın.
| **skuName** | Katman türü. S0 standart ve B0 için temel için kullanın.

Blockchain üye ve destekleyen kaynaklar oluşturmak için yaklaşık 10 dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz blockchain üye kullanabilirsiniz. Artık gerekli olmadığında silerek kaynakları silebilirsiniz `myResourceGroup` oluşturduğunuz kaynak grubunu Azure Blockchain hizmeti tarafından.

Kaynak grubunu kaldırmak için aşağıdaki komutu çalıştırın ve tüm ilgili kaynakları.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Blok zinciri üyesi oluşturduğunuza göre işlem düğümü hızlı başlangıçlar için Bağlan birini deneyin [Geth](connect-geth.md), [MetaMask](connect-metamask.md), veya [Truffle](connect-truffle.md).

> [!div class="nextstepaction"]
> [Truffle bağlanmak için kullandığınız bir Azure blok zinciri hizmet ağ](connect-truffle.md)
