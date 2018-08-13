---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/26/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 6502ea1733e37e06172833c944c58101b3c049f2
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "40044000"
---
Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Azure kaynaklarını ve kaynak gruplarını silme işlemi geri alınamaz. Bu öğeler silindikten sonra kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT hub'ını tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubu içinde oluşturduysanız, kaynak grubunu silmek yerine IoT hub kaynağını silin.
>

Yalnızca IoT hub’ı silmek için aşağıdaki komutu çalıştırın. \<YourIoTHub> değerini kendi IoT hub’ınızın adıyla ve \<TestResources> değerini kaynak grubunuzun adıyla değiştirin:

```azurecli-interactive
az iot hub delete --name <YourIoTHub> --resource-group <TestResources>
```


Kaynak grubunun tamamını adıyla silmek için:

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.

2. **Ada göre filtrele** metin kutusuna IoT hub'ınızı içeren kaynak grubunun adını girin. 

3. Sonuç listesindeki kaynak grubunuzun sağ tarafında yer alan üç noktayı (**...**) seçin ve sonra da **Kaynak grubunu sil**’i seçin.

    ![Kaynak grubunu silme](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yeniden girin ve ardından **Sil**’i seçin. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.






