---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: e319356d555f26354ea29dc7be068bf6168abb17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455160"
---
## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Dosya local.settings.json uygulama ayarları, bağlantı dizeleri ve yerel geliştirme araçları tarafından kullanılan ayarları depolar. Local.settings.json dosyasında ayarları, yalnızca yerel olarak çalıştırırken kullanılır. Yerel ayarlar dosyasını aşağıdaki yapıya sahiptir:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Aşağıdaki ayarlar, yerel olarak çalıştırılırken desteklenir:

| Ayar      | Açıklama                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Ayarlandığında `true`, tüm değerlerin bir yerel makine anahtarı kullanılarak şifrelenir. İle kullanılan `func settings` komutları. Varsayılan değer `false`. |
| **`Values`** | Uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri dizisi. Bu anahtar-değer (dize-dize) çiftleri işlev uygulamanızda Azure, uygulama ayarlarında gibi karşılık [ `AzureWebJobsStorage` ]. Birçok tetikleyiciler ve bağlamalar gibi bir bağlantı dizesi uygulama ayarına başvuran bir özelliği olan `Connection` için [Blob Depolama tetikleyicisi](../articles/azure-functions/functions-bindings-storage-blob.md#trigger---configuration). Tür özellikleri için tanımlanan bir uygulama ayarı ihtiyacınız `Values` dizisi. <br/>[`AzureWebJobsStorage`] gerekli bir uygulama dışındaki HTTP tetikleyici ayarlanıyor. <br/>Sürüm 2.x çalışma zamanı işlevleri gerektiriyor [`FUNCTIONS_WORKER_RUNTIME`] ayarını projeniz için temel araçları tarafından oluşturulur. <br/> Olduğunda [Azure storage öykünücüsü](../articles/storage/common/storage-use-emulator.md) ayarlayabileceğiniz yerel olarak yüklü [ `AzureWebJobsStorage` ] için `UseDevelopmentStorage=true` ve temel araçları öykünücüsü kullanır. Geliştirme sırasında kullanışlıdır ancak gerçek depolama bağlantısı dağıtımdan önce test etmeniz gerekir.<br/> Değerleri, dizeleri ve JSON nesneleri veya dizi olması gerekir. Ayar adları nokta içeremez (`:`) veya çift alt çizgi (`__`); çalışma zamanı tarafından ayrılmış olan.  |
| **`Host`** | Bu bölümdeki ayarlarını yerel olarak çalıştırılırken işlevleri ana bilgisayar işlemi özelleştirin. Bu, Azure'da çalıştırırken de geçerli host.json ayarlarından ayrıdır. |
| **`LocalHttpPort`** | Yerel işlevler ana çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar (`func host start` ve `func run`). `--port` Komut satırı seçeneği bu değerin üzerine göre önceliklidir. |
| **`CORS`** | İzin verilen çıkış noktaları tanımlar [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Kaynakları, boşluk virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir, her türlü kaynağa gelen isteklere izin verir. |
| **`CORSCredentials`** |  İzin vermek için true olarak ayarlanmış `withCredentials` istekleri. |
| **`ConnectionStrings`** | Bu koleksiyon, işlev bağlamaları tarafından kullanılan bağlantı dizeleri için kullanmayın. Bu koleksiyon yalnızca genellikle bağlantı dizeleri alma çerçeveleri tarafından kullanılan `ConnectionStrings` gibi bir yapılandırma bölümünü dosya [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Bağlantı dizelerini bu nesne, sağlayıcı türü ortamı eklenir [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Bu koleksiyondaki öğelerin diğer uygulama ayarları ile Azure'a yayımlanmaz. Bu değerleri açıkça eklemelidir `Connection strings` , işlev uygulaması ayarları koleksiyonu. Oluşturuyorsanız bir [ `SqlConnection` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) işlev kodunuzu bağlantı dizesi değerindeki saklamalısınız **uygulama ayarları** , diğer bağlantılarla portalında. |

[`AzureWebJobsStorage`]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
