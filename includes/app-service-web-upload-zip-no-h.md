---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.openlocfilehash: e4bc749047bbf0d55fc60a699ac956421775d5b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710024"
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
