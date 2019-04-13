---
title: Azure tanılama uzantısı yapılandırma şeması sürüm geçmişi
description: Azure sanal makineler, VM ölçek kümeleri, Service Fabric ve Cloud Services performans sayaçları toplamak için uygun.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/20/2018
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: 1230a9bcea01ef394a6299c50b8d5537850cfee5
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526357"
---
# <a name="azure-diagnostics-extension-configuration-schema-versions-and-history"></a>Azure tanılama uzantısı yapılandırma şeması sürümleri ve geçmişi
Bu sayfa dizinlerinin Azure tanılama Uzantı Şeması sürümleri, Microsoft Azure SDK'sı bir parçası olarak gönderildiğini.  

> [!NOTE]
> Azure tanılama uzantısını performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir:
> - Azure Sanal Makineler
> - Sanal Makine Ölçek Kümeleri
> - Service Fabric
> - Cloud Services
> - Ağ Güvenlik Grupları
>
> Bu sayfada, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.

Azure tanılama uzantısı, Application Insights ve Log Analytics içeren Azure İzleyici gibi diğer Microsoft tanılama ürünleriyle kullanılır. Daha fazla bilgi için [Microsoft izleme araçlarına genel bakış](../../azure-monitor/overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Grafik sevkiyat azure SDK ve tanılama sürümleri  

|Azure SDK sürümü | Tanılama uzantısı sürüm | Model|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |Eklenti|  
|2.0 - 2.4         |1.0                            |Eklenti|  
|2,5               |1.2                            |Uzantı|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|
|2.96              |1.11                           |"|


 Azure SDK'sı yüklendiğinde, Azure tanılama sürüm aldığınız anlamına gelir. eklentisini modelinde--ilk sevk edilen azure tanılama sürüm 1.0 ile birlikte gelir.  

 Azure tanılama (tanılama sürüm 1.2) SDK 2.5 ile başlayan bir uzantı modele geçti. Yeni özelliklerinden yararlanmak için Araçlar yalnızca yeni Azure SDK'ları içinde kullanılabilir, ancak Azure Tanılama'yı kullanarak herhangi bir hizmeti doğrudan Azure'dan en son gönderim sürümü seçiyordu. Örneğin, hala SDK 2.5 kullanan herkesin yeni özellikleri kullanıyorsa önceki tabloda bakılmaksızın gösterilen en son sürümü yükleniyor.  

## <a name="schemas-index"></a>Şemaları dizini  
Azure Tanılama'nün farklı sürümlerini farklı yapılandırma şemaları kullanın.

[Tanılama 1.0 yapılandırma şeması](diagnostics-extension-schema-1dot0.md)  

[Tanılama 1.2 yapılandırma şeması](diagnostics-extension-schema-1dot2.md)  

[Tanılama 1.3 ve üzeri yapılandırma şeması](diagnostics-extension-schema-1dot3.md)  

## <a name="version-history"></a>Sürüm geçmişi

### <a name="diagnostics-extension-111"></a>Tanılama uzantısı 1.11
Azure İzleyici havuz desteği eklendi. Bu havuz yalnızca performans sayaçları için geçerlidir. VM, VMSS veya Bulut hizmeti üzerinde özel ölçümleriniz Azure İzleyici için toplanan performans sayaçlarının gönderilmesini sağlar. Azure Monitor havuzu destekler:
* Azure İzleyici aracılığıyla gönderilen tüm performans sayaçlarını alınırken [Azure İzleyici ölçümleri API'leri.](https://docs.microsoft.com/rest/api/monitor/metrics/list)
* Tüm performans sayaçlarını uyarı gönderilen Azure İzleyici ile [uyarı deneyimi birleşik](../../azure-monitor/platform/alerts-overview.md) Azure İzleyici'de
* Joker karakter işleci, performans sayaçları "Örnek" boyutu, ölçüm olarak ele alınıyor. Örneğin, toplanan "LogicalDisk (\*) / DiskWrites/sn" filtre olması ve için her Mantıksal Disk (C:, D:, vb.) bölme "Örnek" boyutta çizim ya da uyarı Disk Yazma/sn sayacı

Azure İzleyici tanılama uzantısı yapılandırmanız yeni bir havuz olarak tanımlayın
```json
"SinksConfig": {
    "Sink": [
        {
            "name": "AzureMonitorSink",
            "AzureMonitor": {}
        },
    ]
}
```

```XML
<SinksConfig>  
  <Sink name="AzureMonitorSink">
      <AzureMonitor/>
  </Sink>
</SinksConfig>
```
> [!NOTE]
> Azure Monitor havuzu, Klasik Vm'leri ve klasik bulut hizmeti için yapılandırma tanılama uzantı özel yapılandırması tanımlanması daha fazla parametre gerektiriyor.
>
> Lütfen şuna başvurun daha fazla ayrıntı için [ayrıntılı tanılama Uzantı Şeması belgeleri.](diagnostics-extension-schema-1dot3.md)

Ardından, Azure İzleyici havuz yönlendirilmesini performans sayaçlarınız yapılandırabilirsiniz.
```json
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "AzureMonitorSink",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT1M",
            "unit": "percent"
        }
    ]
},
```
```XML
<PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
</PerformanceCounters>
```

### <a name="diagnostics-extension-19"></a>Tanılama uzantısını 1.9
Docker desteği eklendi.


### <a name="diagnostics-extension-181"></a>Tanılama uzantısını 1.8.1
Depolama hesabı anahtarı yerine SAS belirteci özel yapılandırmada belirtebilirsiniz. Bir SAS belirteci sağlanmamışsa, depolama hesabı anahtarını göz ardı edilir.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Tanılama uzantısını 1.8
PublicConfig için eklenen depolama türü. StorageType olabilir *tablo*, *Blob*, *TableAndBlob*. *Tablo* varsayılandır.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Tanılama uzantısını 1.7
Event Hubs'a yönlendirmek için özelliği eklendi.

### <a name="diagnostics-extension-15"></a>Tanılama uzantısı 1.5
Havuzlarını öğesi ve Tanılama verileri gönderme olanağı eklendi [Application Insights](../../azure-monitor/app/cloudservices.md) sistem ve altyapı düzeyinde yanı sıra, uygulama genelinde sorunları tanılamak kolaylaştırır.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 ve tanılama uzantısı 1.3
Visual Studio'da bulut hizmeti projeleri için aşağıdaki değişiklikler yapılmıştır. (Bu değişiklikleri da Azure SDK'sının daha yeni sürümler için geçerlidir.)

* Yerel öykünücü artık tanılama destekler. Bu değişiklik, Tanılama verileri toplamak ve Uygulamanızı geliştirirken ve Visual Studio'da test ederken sağ izlemeleri oluşturuyor olun anlamına gelir. Bağlantı dizesi `UseDevelopmentStorage=true` Azure storage öykünücüsü kullanarak bulut hizmeti projenizi Visual Studio'da çalıştırıyorsanız tanılama veri toplamayı etkinleştirir. Tüm Tanılama verileri (geliştirme depolama) depolama hesabında toplanır.
* Tanılama depolama hesabı bağlantı dizesi (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) bir kez daha hizmet yapılandırma (.cscfg) dosyasında depolanır. Azure SDK 2.5 diagnostics.wadcfgx dosyasında tanılama depolama hesabı belirtilmedi.

Azure SDK 2.4 ve önceki bağlantı dizesini nasıl çalıştığı ve Azure SDK 2.6 sürümündeki yenilik ve daha sonra nasıl çalıştığını arasında bazı önemli farklar vardır.

* Azure SDK 2.4 ve önceki sürümlerinde, bağlantı dizesini aktarma tanılama günlükleri için depolama hesabı bilgileri almak için çalışma zamanında tanılama eklenti tarafından kullanıldı.
* Visual Studio Azure SDK 2.6 sürümündeki yenilik ve daha sonra tanılama bağlantı dizesinde yayımlama sırasında uygun depolama hesap bilgileriyle tanılama uzantısını yapılandırmak için kullanır. Bağlantı dizesini, Visual Studio yayımlama sırasında kullanacağınız farklı hizmet yapılandırması için farklı depolama hesaplarında tanımlamanıza olanak sağlar. Tanılama eklentisi artık (Azure SDK 2.5 sonra) kullanılabilir, ancak .cscfg dosyası kendisi tarafından tanılama uzantısını etkinleştirilemiyor. Visual Studio veya PowerShell gibi Araçlar üzerinden ayrı olarak uzantıyı etkinleştirmek zorunda.
* PowerShell ile tanılama uzantısını yapılandırma işlemini basitleştirmek için her rol için tanılama uzantısı için XML ortak yapılandırma Visual Studio Paket çıkışı de içerir. Visual Studio tanılama bağlantı dizesinde genel yapılandırmada mevcut depolama hesabı bilgileri doldurmak için kullanır. Genel yapılandırma dosyalarına Uzantıları klasöründe oluşturulur ve desenler izleyen `PaaSDiagnostics.<RoleName>.PubConfig.xml`. Tüm PowerShell tabanlı dağıtımların bu desen, her yapılandırma bir Role eşlemek için kullanabilirsiniz.
* .Cscfg dosyasındaki bağlantı dizesi, tanılama veri içinde görüntülenebilir erişmek için Azure portal tarafından da kullanılır **izleme** sekmesi. Bağlantı dizesi, ayrıntılı izleme verileri portalda göstermek için bir hizmeti yapılandırmak için gereklidir.

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Azure SDK 2.6 ve daha sonra geçişi projeleri
Ardından Azure SDK 2.6 veya sonraki sürümlerde, Azure SDK 2.5-.wadcfgx dosyasında belirtilen tanılama depolama hesabı olsaydı geçirirken, orada kalır. Farklı depolama yapılandırmaları için farklı depolama hesaplarını kullanma esnekliğini yararlanmak için bağlantı dizesi projenize el ile eklemek zorunda kalırsınız. Ardından bir proje, bir Azure SDK 2.4 veya daha önceki Azure SDK 2.6 geçiriyorsanız, tanılama bağlantı dizelerini korunur. Ancak, nasıl bağlantı dizeleri Azure SDK 2.6 belirtildiği şekilde önceki bölümde ele alındığı değişiklikler unutmayın.

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Visual Studio tanılama depolama hesabı nasıl belirler
* .Cscfg dosyasında belirtilen tanılama bağlantı dizesini, Visual Studio yayımlarken ve genel yapılandırma xml dosyalarını paketleme sırasında oluştururken tanılama uzantısını yapılandırmak için kullanır.
* .Cscfg dosyasında belirtilen tanılama bağlantı dizesi, ardından Visual Studio yeniden yayımlama ve genel yapılandırma xml dosyaları oluşturma tanılama uzantısını yapılandırmak üzere .wadcfgx dosyasında belirtilen depolama hesabı kullanarak döner paketlenirken.
* .Cscfg dosyasını tanılama bağlantı dizesinde .wadcfgx dosyasında depolama hesabı daha önceliklidir. .Cscfg dosyasında belirtilen tanılama bağlantı dizesini, Visual Studio kullanan ve .wadcfgx depolama hesabında yok sayar.

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>"Geliştirme depolama bağlantı dizelerini güncelleştir..." onay kutusunu ne yapar?
Onay kutusunu **güncelleştirme geliştirme depolama bağlantı dizelerini tanılama ve önbelleğe alma için Microsoft Azure depolama hesabı kimlik bilgileriyle Microsoft Azure'a yayımlarken** , tüm geliştirme güncelleştirmek için kullanışlı bir yol sunar Yayımlama sırasında belirtilen Azure depolama hesabı ile depolama hesabı bağlantı dizeleri.

Örneğin, bu onay kutusunu seçin ve tanılama bağlantı dizesini belirtir varsayalım `UseDevelopmentStorage=true`. Projeyi Azure'da yayımlarken, Visual Studio tanılama bağlantı dizesinde Yayımla Sihirbazı'nda belirtilen depolama hesabı ile otomatik olarak güncelleştirecektir. Gerçek depolama hesabı tanılama bağlantı dizesi olarak belirtilmişse, ancak ardından bu hesabı yerine kullanılır.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Tanılama işlevleri farklılıkları Azure SDK 2.4 ve önceki ve Azure SDK 2.5 ve üzeri
Azure SDK 2.4 için Azure SDK 2.5 veya üstü projenizi yükseltiyorsanız aşağıdaki tanılama işlev farklılıkları göz önünde bulundurmanız gerekir.

* **Yapılandırma API'leri kullanım dışı bırakılmıştır** – tanılama program yapılandırması için Azure SDK 2.4 veya önceki sürümlerde kullanılabilir, ancak bu Azure SDK 2.5 ve daha sonra kullanım dışı bırakılmıştır. Tanılama yapılandırmanızı kodda şu anda tanımlanmış olması durumunda, tanılama için sırayla geçirilen proje sıfırdan çalışmaya devam etmek için bu ayarları yeniden yapılandırmanız gerekecektir. Tanılama yapılandırması için Azure SDK 2.4 diagnostics.wadcfg ve Azure SDK 2.5 ve daha sonra diagnostics.wadcfgx dosyasıdır.
* **Bulut hizmet uygulamaları için tanılama yalnızca rol düzeyinde örnek düzeyinde yapılandırılabilir.**
* **Uygulamanızı dağıtma her seferinde, tanılama yapılandırması güncelleştirilir** – Sunucu Gezgini'nden, Tanılama yapılandırmasını değiştirin ve ardından uygulamanızı yeniden dağıtın, bu eşlik sorunlara neden olabilir.
* **Azure SDK 2.5 ve sonraki sürümlerinde, kilitlenme bilgi dökümleri tanılama yapılandırma dosyası, kod içinde değil yapılandırılan** – kodda yapılandırılmış kilitlenme bilgi dökümleri varsa, el ile yapılandırma kod yapılandırma dosyasına aktarmak çünkü gerekir kilitlenme bilgi dökümleri geçiş sırasında için Azure SDK 2.6 aktarılmaz.

