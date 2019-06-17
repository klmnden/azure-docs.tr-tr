---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 7c081b3bc5f9e6273f680b24897f9aced4999afa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129020"
---
## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Dış bir şablonu dağıtmak için tam olarak, tüm dış dağıtımda olduğu gibi şablon URI'si belirtin. Yerel bir şablonu dağıtmak için Cloud Shell için depolama hesabına şablonunuzu yüklemelisiniz. Bu bölümde, cloud shell hesabınıza şablonu yükle ve yerel dosya olarak dağıtmak açıklar. Cloud Shell kullanmadıysanız bkz [Azure Cloud shell'e genel bakış](../articles/cloud-shell/overview.md) kurulumu hakkında bilgi için.

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
