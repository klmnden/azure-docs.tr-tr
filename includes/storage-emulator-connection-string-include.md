---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114668"
---
Depolama öykünücüsü, tek bir sabit hesap ve iyi bilinen bir kimlik doğrulama anahtarı için paylaşılan anahtar kimlik doğrulamasını destekler. Bu hesap ve anahtar depolama öykünücüsü ile kullanmak için izin verilen, yalnızca paylaşılan anahtar kimlik bilgileridir. Bunlar:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Depolama öykünücüsü tarafından desteklenen kimlik doğrulama anahtarı, yalnızca bir istemci kimlik doğrulama kodunuzu işlevselliğini test etmek için tasarlanmıştır. Herhangi bir güvenlik amaca hizmet yok. Depolama öykünücüsü ile üretim depolama hesabınız ve anahtarı kullanamazsınız. Üretim verileri ile geliştirme hesabı kullanmamalıdır.
> 
> Depolama öykünücüsü bağlantı HTTP üzerinden destekler. Ancak, üretimde Azure depolama hesabı kaynaklarına erişim için önerilen protokol https'dir.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Bir kısayol kullanarak öykünücü hesabına bağlanma
Kısayol başvuran, uygulamanızın yapılandırma dosyasında bir bağlantı dizesini yapılandırmak için depolama öykünücüsü için uygulamanızdan bağlanmak için en kolay yolu olan `UseDevelopmentStorage=true`. Depolama öykünücüsü'nde bir bağlantı dizesi örneği İşte bir *app.config* dosyası: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>İyi bilinen hesap adını ve anahtarını kullanarak öykünücü hesabına bağlanma
Öykünücü hesap adını ve anahtarını başvuran bir bağlantı dizesi oluşturmak için bağlantı dizesinde öykücüsünden kullanmak istediğiniz hizmetlerinin her biri için uç noktalar belirtmeniz gerekir. Bağlantı dizesi öykünücü uç noktaları, bir üretim depolama hesabı için farklı başvurur böylece, bu gereklidir. Örneğin, bağlantı dizesi değerini şu şekilde görünür:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Bu değer, yukarıda gösterilen kısayoluna aynıdır `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Bir HTTP Ara sunucusunu belirtin
Depolama öykünücüsü karşı hizmetinizi test ederken kullanmak için bir HTTP Proxy'si de belirtebilirsiniz. Bu, HTTP isteklerini ve yanıtlarını depolama hizmetleri üzerinde işlemler hata ayıklarken gözlemlemek için yararlı olabilir. Bir ara sunucu belirtmek için `DevelopmentStorageProxyUri` seçeneği ile bağlantı dizesi ve proxy URI'si değerini ayarlayın. Örneğin, depolama öykünücüsü için işaret eder ve bir HTTP proxy'sinin yapılandıran bir bağlantı dizesi şu şekildedir:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

