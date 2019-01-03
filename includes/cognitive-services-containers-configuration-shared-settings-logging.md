---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: c8235fa65a3be91956ffad731d47488a852e42a2
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977385"
---
`Logging` Ayarlarını yönetme kapsayıcınız için ASP.NET Core oturum açma desteği. Bir ASP.NET Core uygulaması için kullandığınız kapsayıcınız için aynı yapılandırma ayarları ve değerleri kullanabilirsiniz. 

Aşağıdaki günlük kaydı sağlayıcıları kapsayıcı tarafından desteklenir:

|Sağlayıcı|Amaç|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Hata ayıklama](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` oturum açma sağlayıcısı. Tüm ASP.NET Core yapılandırma ayarlarını ve bu oturum açma sağlayıcısı için varsayılan değerleri desteklenir.|
|[Disk](#disk-logging)|JSON oturum açma sağlayıcısı. Bu oturum açma sağlayıcısı için çıktı bağlama günlük verilerini yazar.|

### <a name="disk-logging"></a>Disk günlüğe kaydetme

`Disk` Oturum açma sağlayıcısı, aşağıdaki yapılandırma ayarları destekler:  

| Ad | Veri türü | Açıklama |
|------|-----------|-------------|
| `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Not:** Bu değer ayarlanmalıdır `json` günlük sağlayıcısını etkinleştirmek için. Bu değer aynı zamanda bir kapsayıcı örneği oluşturulurken bir çıkış bağlama belirtmeden belirtilirse, bir hata oluşur. |
| `MaxFileSize` | Tamsayı | Megabayt (MB) günlük dosyasının en büyük boyutu. Yeni bir günlük dosyası, geçerli günlük dosyası boyutunu karşıladığından veya bu değeri aşarsa, oturum açma sağlayıcısı tarafından başlatılır. -1 belirtilmezse, günlük dosyasının boyutu, çıkış bağlama yalnızca en büyük dosya boyutuyla sınırlıdır. Varsayılan değer 1'dir. |

ASP.NET Core günlük desteği yapılandırma hakkında daha fazla bilgi için bkz. [ayarları dosya Yapılandırması](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).
