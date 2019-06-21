---
title: Azure SignalR hizmeti nedir?
description: Azure SignalR hizmeti genel bakış.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 06/20/2019
ms.author: zhshang
ms.openlocfilehash: dc7ba3585ec49921c0a0e66185fc5550d3d4a006
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303608"
---
# <a name="what-is-azure-signalr-service"></a>Azure SignalR hizmeti nedir?

Azure SignalR Hizmeti, HTTP üzerinden uygulamalara gerçek zamanlı web işlevselliği ekleme işlemini basitleştirir. Bu gerçek zamanlı işlevsellik, hizmetin bağlı istemcilere tek sayfalık bir web veya mobil uygulaması gibi içerik güncelleştirmeleri göndermesine olanak tanır. Sonuç olarak, istemciler sunucuyu yoklama veya yeni HTTP isteklerini güncelleştirmek üzere gönderme gereksinimi olmadan güncelleştirilir.


Bu makalede Azure SignalR Hizmetine genel bir bakış sunulmaktadır.

## <a name="what-is-azure-signalr-service-used-for"></a>Azure SignalR Hizmeti ne için kullanılır?

Azure SignalR hizmeti gerçek zamanlı olarak istemciye sunucusundan veri gönderme gerektiren herhangi bir senaryoya kullanabilirsiniz.

Genellikle gerektiren geleneksel gerçek zamanlı özelliklerle sunucusundan yoklama Azure SignalR hizmeti de kullanabilirsiniz.

Azure SignalR hizmeti çok çeşitli sektörlerde, gerçek zamanlı içerik güncelleştirmeleri gerektiren herhangi bir uygulama türü için kullanıldı. Biz, Azure SignalR hizmeti kullanmanız yararlı olan bazı örnekler listelenmektedir:

* **Yüksek sıklık düzeyi veri güncelleştirmelerini:** oyun, oylama, yoklama, açık eksiltme.
* **Panolar ve izleme:** şirket Pano, finansal verilerini, anında satış güncelleştirme, çok oyunculu oyun puan tablosu ve IOT izleme.
* **Sohbet:** canlı sohbet odası, sohbet Robotu, çevrimiçi müşteri desteği, gerçek zamanlı alışveriş Yardımcısı, messenger, oyun içi sohbet ve benzeri.
* **Gerçek zamanlı bir konumu haritada:** Lojistik izleme, teslim durumu izleme, nakliye durum güncelleştirmeleri, GPS uygulamalar.
* **Gerçek zamanlı hedeflenen reklamlar:** kişiselleştirilmiş okuma süresi anında iletme reklam ve teklifler, etkileşimli reklamlar.
* **İşbirliğine dayalı uygulamalar:** birlikte yazmayı, Beyaz Tahta uygulamaları ve yazılım toplantı takım.
* **Anında iletme bildirimleri:** sosyal ağ, e-posta, oyun, seyahat uyarısı.
* **Gerçek zamanlı yayın:** ses/video yayın canlı, açıklamalı alt yazı, çevirme, olaylar/haber yayınlama Canlı.
* **IOT ve bağlı cihazlar:** gerçek zamanlı IOT ölçümleri, uzaktan denetim, gerçek zamanlı durum ve izleme konumu.
* **Otomasyon:** Yukarı Akış olaylardan gerçek zamanlı tetikleyici.

## <a name="what-are-the-benefits-using-azure-signalr-service"></a>Avantajları nelerdir Azure SignalR hizmeti kullanarak?

**Standart tabanlı:**

SignalR, gerçek zamanlı web uygulamaları derlemek için kullanılan birkaç tekniğin özetini sunar. [WebSockets](https://wikipedia.org/wiki/WebSocket) ideal aktarım yöntemidir ancak başka seçenek olmadığında [Sunucu ile Gönderilen Olaylar (SSE)](https://wikipedia.org/wiki/Server-sent_events) ve Uzun Yoklama gibi diğer teknikler de kullanılır. SignalR, sunucu ve istemci üzerinde desteklenen özelliklere göre uygun taşıma tekniğini otomatik olarak algılar ve başlatır.

**Yerel ASP.NET Core desteği:**

SignalR hizmeti, ASP.NET Core ve ASP.NET ile yerel bir programlama deneyimi sağlar. SignalR Service yeni SignalR uygulama geliştirme veya mevcut SignalR öğesinden geçiş için SignalR hizmet tabanlı uygulama en az çaba gerektirir.
SignalR hizmeti, ASP.NET Core'nın yeni özelliği, sunucu tarafı Blazor de destekler.

**Geniş istemci desteği:**

İstemciler, web ve mobil tarayıcılar, Masaüstü uygulamaları, mobil uygulamalar, sunucu işlemi, IOT cihazları ve oyun konsolları gibi çok geniş bir yelpazede ile SignalR hizmeti çalışır. SignalR hizmet SDK'ları farklı dillerde sunar. Yerel ASP.NET Core veya ASP.NET yanı sıra C# SDK'ları, SignalR Service Ayrıca JavaScript istemci web istemcileri ve birçok yeni JavaScript çerçevesi etkinleştirmek için SDK'sı sağlar. Java istemci SDK'sı, Android yerel uygulamaları dahil olmak üzere Java uygulamaları için de desteklenir. SignalR hizmeti REST API'sini destekler ve sunucusuz Azure işlevleri ve Event Grid ile tümleştirmeler aracılığıyla.

**Büyük ölçekli istemci bağlantılarını işleyeceğini:**

SignalR hizmetini gerçek zamanlı büyük ölçekli uygulamalar için tasarlanmıştır. SignalR hizmeti sağlayan Ölçekle birlikte çalışmak üzere birden çok örneği istemci bağlantıları milyonlarca. Hizmet birden fazla küresel bölgeye parçalama, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla için de destekler.

**Şirket içinde SignalR barındırma iş yükünü kaldırın:**

Şirket içinde barındırılan SignalR uygulamaları karşılaştırıldığında, SignalR hizmete geçme istemci bağlantıları ve ölçekler işleyen arka planlarını yönetme ihtiyacını ortadan kaldırır. Tam olarak yönetilen hizmet ayrıca web uygulamaları basitleştirir ve maliyet barındırma kaydeder. SignalR hizmet bağlantıları, milyonlarca için tüm Azure standart, güvenlik ve uyumluluk sağlamasının yanı sıra SLA'sı garanti ölçeklenen, global erişim ve birinci sınıf veri merkezi ve ağ'ı sunar.

![Yönetilen SignalR hizmeti](./media/signalr-overview/managed-signalr-service.png)

**Farklı Mesajlaşma desenleri için zengin bir API sunar:**

SignalR hizmeti belirli bir bağlantı, tüm bağlantılar ya da belirli bir kullanıcıya ait ya da bir rastgele grubuna yerleştirildiği bağlantıların bir alt ileti göndermeyi olanak sağlar.

## <a name="how-to-use-azure-signalr-service"></a>Azure SignalR Service Nasıl Kullanılır

Azure SignalR hizmeti ile bir program olarak burada listelenen örneklerden bazıları için birçok farklı yolu vardır:

- **[Bir ASP.NET Core SignalR Uygulamasını Ölçeklendirme](signalr-concept-scale-aspnet-core.md)** - Yüzbinlerce bağlantıya ölçeklendirmek için Azure SignalR Hizmetini bir ASP.NET Core SignalR uygulaması ile tümleştirin.
- **[Sunucusuz, gerçek zamanlı uygulamalar oluşturma](signalr-concept-azure-functions.md)** JavaScript, C# ve Java gibi dillerde sunucusuz, gerçek zamanlı uygulamalar oluşturmak için Azure İşlevleri tümleştirmesini Azure SignalR Hizmeti ile kullanın.
- **[REST API aracılığıyla istemcilere sunucudan iletileri gönderme](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)** - Azure SignalR Hizmeti, REST kullanılabilen tüm dillerle uygulamaların SignalR Hizmeti ile bağlı istemcilere iletiler gönderebilmesi için REST API’sini sunar.