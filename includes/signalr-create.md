---
title: include dosyası
description: include dosyası
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 28d003e123069c47d87d81570b4a5b69b3b9d64b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66128231"
---
1. Azure SignalR hizmeti kaynak oluşturmak için ilk kez oturum için [Azure portalında](https://portal.azure.com). Sayfanın sol tarafındaki seçin **+ kaynak Oluştur**. İçinde **markette Ara** metin kutusuna **SignalR hizmeti**.

2. Seçin **SignalR hizmeti** seçin ve sonuçları **Oluştur**.

3. Yeni **SignalR** Ayarları sayfasında, yeni SignalR kaynağınız için aşağıdaki ayarları ekleyin:

    | Ad | Önerilen değer | Açıklama |
    | ---- | ----------------- | ----------- |
    | Kaynak adı | *testsignalr* | SignalR kaynağı için kullanılacak benzersiz kaynak adını girin. Adı 1 ila 63 karakter dizesi olması ve yalnızca sayı, harf ve kısa çizgi içermelidir (`-`) karakter. Adı başlayamaz veya kısa çizgi karakteri ile bitemez ve art arda tire karakterler geçerli değildir.|
    | Abonelik | Aboneliğinizi seçin |  SignalR testi için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir aboneliğiniz varsa, otomatik olarak seçilir ve **abonelik** açılan görüntülenmiyorsa.|
    | Kaynak grubu | Adlı bir kaynak grubu oluşturma *SignalRTestResources*| SignalR kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebilirsiniz birden fazla kaynak düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-overview.md). |
    | Location | *Doğu ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |
    | Fiyatlandırma katmanı | *Ücretsiz* | Şu anda **ücretsiz** ve **standart** seçenekleri kullanılabilir. |
    | Panoya sabitle | ✔ | Daha kolay bulmak için bu nedenle, panoya sabitlenmiş kaynak için bu kutuyu seçin. |

4. **Oluştur**’u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra seçin **anahtarları** altında **ayarları**. Birincil anahtar bağlantı dizesini kopyalayın. Azure SignalR hizmeti kaynağı kullanacak şekilde yapılandırmak için bu dize daha sonra kullanacaksınız.

    Bağlantı dizesi aşağıdaki şekildedir:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
