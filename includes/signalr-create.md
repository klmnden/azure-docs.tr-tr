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
ms.openlocfilehash: c26e2de2c492c51c35527979ef0450ed7c2de499
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100836"
---
1. Yeni bir Azure SignalR Service kaynağı oluşturmak için ilk olarak [Azure portal](https://portal.azure.com) oturumu açın. Sayfanın sol üst kısmında **+ Kaynak oluştur**'a tıklayın. **Market içinde ara** metin kutusuna **SignalR Service** yazın ve **Enter**'a basın.

2. Sonuçlarda **SignalR Service**'e ve **Oluştur**'a tıklayın.

3. Yeni açılan **SignalR** ayarları sayfasında yeni SignalR kaynağınız için aşağıdaki ayarları girin:

    | Adı | Önerilen değer | Açıklama |
    | ---- | ----------------- | ----------- |
    | Kaynak Adı | *testsignalr* | SignalR kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Ad `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmaz.|
    | Abonelik | Aboneliğinizi seçin |  SignalR testi için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa bu otomatik olarak seçilir ve **Abonelik** açılan penceresi görüntülenmez.|
    | Kaynak grubu | *SignalRTestResources* adlı yeni bir kaynak grubu oluşturun| SignalR kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden fazla kaynağı düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-overview.md). |
    | Konum | *Doğu ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |
    | Fiyatlandırma katmanı | *Ücretsiz* | Şu anda **Ücretsiz** ve **Standart** seçenekleri mevcuttur. |
    | Panoya sabitle | ✔ | Kaynağın daha kolay bulunması amacıyla panonuza sabitlenmesi için bu kutuyu işaretleyin. |

4. **Oluştur**’a tıklayın. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra **AYARLAR** bölümünde **Anahtarlar**'a tıklayın. Birincil anahtar bağlantı dizenizi kopyalayın. Bunu daha sonra uygulamanızı Azure SignalR Service kaynağını kullanacak şekilde yapılandırmak için kullanacaksınız.

    Bağlantı dizesi aşağıdaki şekildedir:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
