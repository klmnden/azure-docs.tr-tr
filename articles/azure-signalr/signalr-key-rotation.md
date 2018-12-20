---
title: Azure SignalR hizmeti için erişim anahtar döndürme
description: Müşteri neden düzenli olarak erişim anahtarlarını döndürme gerekiyor ve portal GUI ve CLI ile nasıl yapılacağı hakkında genel bir bakış.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 2c0f60b0ef3a90372fc4a095c830f39bc148f354
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636160"
---
# <a name="access-key-rotation-for-azure-signalr-service"></a>Azure SignalR hizmeti için erişim anahtar döndürme

Her Azure SignalR hizmeti örneğinin erişim anahtarlarını çifti vardır: Birincil ve ikincil anahtarlar. Bunlar, istekleri hizmete yaparken SignalR istemcilerin kimliğini doğrulamak için kullanılır. Anahtarları örnek uç nokta URL'si ile ilişkili değildir. Anahtarlarınızı güvenli tutmalarını ve düzenli olarak döndür. Bir anahtar yeniden oluştururken diğeri ile bağlantı sağlamak için iki erişim tuşu sağlanır.

## <a name="why-rotate-access-keys"></a>Neden döndürme erişim anahtarları?

Geliştiriciler, düzenli olarak erişim anahtarlarını döndürmek için güvenlik nedeni ve uyumluluk gereksinimini önerilir.

## <a name="how-to-regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluşturmak nasıl?

1. [Azure portala](https://portal.azure.com/) gidin ve kimlik bilgilerinizle oturum açın.

1. Bulma **anahtarları** bölüm anahtarlarını yeniden oluşturmak istediğiniz Azure SignalR hizmeti örneğinden.

1. Tıklayın **anahtarları** Gezinti menüsünde.

1. Tıklayın **birincil anahtarı yeniden** veya **ikincil anahtar yeniden**.

Yeni bir anahtar ve karşılık gelen bağlantı dizesi oluşturulur ve görüntülenir.

 ![Anahtarları Yeniden Oluştur](media/signalr-key-rotation/regenerate-keys.png)

Ayrıca anahtarlar kullanarak yeniden oluşturabilirsiniz [Azure CLI](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew).

## <a name="update-configurations-with-new-connection-strings"></a>Yeni bağlantı dizeleri ile yapılandırmaları güncelleştirme

1. Yeni oluşturulan bağlantı dizesini kopyalayın.

1. Yeni bağlantı dizesini kullanmak için tüm yapılandırmaları güncelleştirin.

1. Uygulama, gerektiği gibi yeniden başlatın.

## <a name="forced-access-key-regeneration"></a>Zorlanmış erişim anahtarını yeniden üretme

Azure SignalR hizmeti belirli bir durum altında bir zorunlu erişim anahtarını yeniden üretme uygulanmasını şart koşabilir. Hizmet müşterileri e-posta ve portal bildirimi aracılığıyla uyarır. Bu iletişim almak veya hizmet hatası nedeniyle erişim anahtarı karşılaşırsanız, bu kılavuzu izleyerek anahtarları döndürün.

## <a name="next-steps"></a>Sonraki adımlar

Erişim anahtarlarını düzenli olarak iyi güvenlik uygulaması olarak döndürme öneririz.

Bu kılavuzda, erişim anahtarlarını yeniden oluştur hakkında öğrendiniz. Azure işlevleri veya OAuth ile kimlik doğrulaması hakkında sonraki öğreticiler için devam edin.

> [!div class="nextstepaction"]
> [ASP.NET Core kimliği ile tümleştirin](./signalr-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Kimlik Doğrulama Özelliklerine Sahip Sunucusuz Gerçek Zamanlı Uygulama Derleme](./signalr-authenticate-azure-functions.md)