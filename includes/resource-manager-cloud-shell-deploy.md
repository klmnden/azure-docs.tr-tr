---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/25/2018
ms.author: tomfitz
ms.openlocfilehash: a2ee8705be3f34b6df113c68d88e375411f84bf2
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440502"
---
## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Ancak, ilk şablonunuzu depolama hesabına Cloud Shell yüklemeniz gerekir. Daha önce Cloud Shell kullanmadıysanız, kurulumu hakkında bilgi için bkz. [Azure Cloud Shell’e Genel Bakış](../articles/cloud-shell/overview.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. **Bloblar**'ı seçin.

   ![Bloblar'ı seçin](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. **+ Kapsayıcı**'yı seçin.

   ![Kapsayıcı ekleme](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Kapsayıcı, bir ad ve bir erişim düzeyi verin. Bu makaledeki örnek şablonu hiçbir hassas bilgi içeriyor, böylece anonim okuma erişimi verin. **Tamam**’ı seçin.

   ![Kapsayıcı değerleri sağlayın](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Oluşturduğunuz kapsayıcıyı seçin.

   ![Yeni kapsayıcı Seç](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. **Karşıya Yükle**’yi seçin.

   ![Blobu karşıya yükle](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Karşıya yüklendikten sonra şablonu seçin.

   ![Yeni şablonu seçin](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. URL'yi kopyalayın.

   ![URL'yi Kopyala](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
