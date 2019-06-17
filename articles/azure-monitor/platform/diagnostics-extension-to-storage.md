---
title: Azure Depolama’da Tanılama Verilerini Depolama ve Görüntüleme
description: Azure Tanılama verileri Azure Depolama'ya alma ve görüntüleme
services: azure-monitor
author: jpconnock
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/01/2016
ms.author: jeconnoc
ms.subservice: diagnostic-extension
ms.openlocfilehash: 23379e9d9bb29efb7fb026260e8245e8eb8a2d71
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60395076"
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Azure Depolama’daki tanılama verilerini depolama ve görüntüleme
Microsoft Azure storage öykünücüsü veya Azure depolama için Aktarım sürece tanılama verilerini kalıcı olarak depolanmaz. Bir kez depolama alanında, çeşitli araçlar biriyle görüntülenebilir.

## <a name="specify-a-storage-account"></a>Bir depolama hesabı belirtin
ServiceConfiguration.cscfg dosyasında kullanmak istediğiniz depolama hesabını belirtin. Hesap bilgileri, bir bağlantı dizesi bir yapılandırma ayarı olarak tanımlanır. Aşağıdaki örnek, Visual Studio'da yeni bir bulut hizmeti projesi için oluşturulan varsayılan bağlantı dizesi gösterir:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Bir Azure depolama hesabı için hesap bilgileri sağlamak için bu bağlantı dizesi değiştirebilirsiniz.

Toplanmakta olan tanılama veri türüne bağlı olarak, Azure Tanılama veya Blob hizmeti, hem de tablo hizmeti kullanır. Kalıcı veri kaynakları ve bunların biçimleri aşağıdaki tabloda gösterilmektedir.

| Veri kaynağı | Depolama biçimi |
| --- | --- |
| Azure günlükleri |Tablo |
| IIS 7.0 günlükleri |Blob |
| Azure Tanılama altyapısı günlükleri |Tablo |
| Başarısız istek izleme günlükleri |Blob |
| Windows Olay günlükleri |Tablo |
| Performans sayaçları |Tablo |
| Kilitlenme bilgi dökümleri |Blob |
| Özel hata günlükleri |Blob |

## <a name="transfer-diagnostic-data"></a>Tanılama veri aktarımı
SDK 2.5 ve daha sonra tanılama veri aktarım isteğinin yapılandırma dosyası aracılığıyla ortaya çıkabilir. Zamanlanan aralıklarda yapılandırmasında belirtilen tanılama veri aktarabilir.

SDK 2.4 ve önceki Tanılama verileri de yapılandırma dosyası programlı olarak aktarmak isteyebilir. Programlı bir yaklaşım, isteğe bağlı aktarımları da sağlar.

> [!IMPORTANT]
> Azure depolama hesabınız için tanılama veri aktarırken, tanılama verilerinizi kullanan depolama kaynakları için ücret yansıtılmaz.
> 
> 

## <a name="store-diagnostic-data"></a>Tanılama veri Store
Günlük verilerini aşağıdaki adlara sahip Blob veya tablo depolama alanında depolanır:

**Tabloları**

* **WadLogsTable** - İzleme dinleyicisi kullanarak kod içinde yazılan günlükleri.
* **WADDiagnosticInfrastructureLogsTable** -Tanılama izleme ve yapılandırma değişiklikleri.
* **WADDirectoriesTable** – tanı İzleyicisi İzleme dizinleri.  IIS başarısız istek günlükleri ve özel dizinleri, bu IIS günlükler içerir.  Blob günlük dosyasının konumunu kapsayıcı alanında belirtilir ve blob adını RelativePath alandır.  Azure sanal makinesinde yeterdir AbsolutePath alan dosyasının adını ve konumunu belirtir.
* **WADPerformanceCountersTable** – performans sayaçları.
* **WADWindowsEventLogsTable** – Windows olayı günlüğe kaydeder.

**Bloblar**

* **wad denetim kapsayıcısı** – (yalnızca SDK 2.4 ve önceki) Azure tanılama denetimleri XML yapılandırma dosyalarını içerir.
* **wad IIS failedreqlogfiles** – IIS başarısız istek günlükleri bilgilerini içerir.
* **wad IIS logfiles** – IIS günlükler hakkında bilgi içerir.
* **"özel"** – özel bir kapsayıcı tabanlı tanı İzleyicisi tarafından izlenen dizinleri yapılandırma.  Bu blob kapsayıcının adını WADDirectoriesTable içinde belirtilir.

## <a name="tools-to-view-diagnostic-data"></a>Tanılama verilerini görüntülemek için Araçlar
Çeşitli araçları, depolamaya aktarıldıktan sonra verileri görüntülemek kullanılabilir. Örneğin:

* Microsoft Visual Studio için Azure Araçları'nı yüklediyseniz, sunucu Gezgini'nde Visual Studio - Azure Depolama düğümü sunucu Gezgini'nde salt okunur blob ve tablo verilerini Azure depolama hesaplarınızı görüntülemek için kullanabilirsiniz. Yerel depolama öykünücüsü hesabınızdan verileri görüntüleyebilir ve ayrıca depolama hesapları için Azure oluşturduysanız. Daha fazla bilgi için [gözatma ve Sunucu Gezgini ile depolama kaynaklarını yönetme](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage).
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, OSX ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına uygulamadır.
* [Azure Management Studio](https://www.cerebrata.com/products/azure-management-studio/introduction) görüntüleyin, indirin ve Azure'da çalışan uygulamalar tarafından toplanan Tanılama verileri yönetme olanak tanıyan Azure tanılama Yöneticisi içerir.

## <a name="next-steps"></a>Sonraki Adımlar
[Azure Tanılama ile bulut Hizmetleri uygulamasında akış izleme](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)


