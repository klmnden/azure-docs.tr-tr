---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 7b0059dbaafe42200ebfa8dc65ea585a49c5b83f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712478"
---
`Logging` Ayarlarını yönetme kapsayıcınız için ASP.NET Core oturum açma desteği. Bir ASP.NET Core uygulaması için kullandığınız kapsayıcınız için aynı yapılandırma ayarları ve değerleri kullanabilirsiniz. 

Aşağıdaki günlük kaydı sağlayıcıları kapsayıcı tarafından desteklenir:

|Sağlayıcı|Amaç|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Hata ayıklama](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Disk](#disk-logging)|JSON oturum açma sağlayıcısı. Bu oturum açma sağlayıcısı için çıktı bağlama günlük verilerini yazar.|

Bu kapsayıcı komut çıktı bağlama JSON biçimine günlük bilgileri depolar:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Bu kapsayıcı komut, hata ayıklama bilgilerini gösterir. ön ekine sahip `dbug`, kapsayıcı çalışırken:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Disk günlüğe kaydetme

`Disk` Oturum açma sağlayıcısı, aşağıdaki yapılandırma ayarları destekler:  

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Not:** Bu değer ayarlanmalıdır `json` günlük sağlayıcısını etkinleştirmek için. Bu değer aynı zamanda bir kapsayıcı örneği oluşturulurken bir çıkış bağlama belirtmeden belirtilirse, bir hata oluşur. |
| `MaxFileSize` | Tamsayı | Megabayt (MB) günlük dosyasının en büyük boyutu. Yeni bir günlük dosyası, geçerli günlük dosyası boyutunu karşıladığından veya bu değeri aşarsa, oturum açma sağlayıcısı tarafından başlatılır. -1 belirtilmezse, günlük dosyasının boyutu, çıkış bağlama yalnızca en büyük dosya boyutuyla sınırlıdır. Varsayılan değer 1'dir. |

ASP.NET Core günlük desteği yapılandırma hakkında daha fazla bilgi için bkz. [ayarları dosya Yapılandırması](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

