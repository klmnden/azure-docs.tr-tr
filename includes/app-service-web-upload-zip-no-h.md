---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.openlocfilehash: e4bc749047bbf0d55fc60a699ac956421775d5b0
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423550"
---
İçinde [Azure portalında](https://portal.azure.com), tıklayın **kaynak grupları** > **cloud-shell-depolama -\<your_region >**  >   **\<depolama_hesabı_adı >**.

![Cloud Shell depolama hesabı bulunamadı](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

İçinde **genel bakış** sayfa seçin depolama hesabının **dosyaları**.

Otomatik olarak oluşturulan dosya paylaşımını seçip **karşıya**. Bu dosya paylaşımı Cloud shell'de takılı `clouddrive`.

![Karşıya yükleme düğmesini bulabilirsiniz](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Dosya Seçici ve ZIP dosyanızı seçin, ardından tıklatın **karşıya**. 

Cloud Shell'de kullanmak `ls` varsayılan olarak yüklenen ZIP dosyasını gördüğünüzü doğrulamak için `clouddrive` paylaşın.

```azurecli-interactive
ls clouddrive
```
