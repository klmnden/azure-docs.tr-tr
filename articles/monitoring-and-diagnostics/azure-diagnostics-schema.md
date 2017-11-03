---
title: "Azure tanılama uzantı yapılandırmasını şema sürümleri ve geçmiş | Microsoft Docs"
description: "Azure sanal makineler, VM ölçek kümesi, Service Fabric ve Cloud Services performans sayacı toplama ilgili."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 119e8a237f24cdc80a1ab8e376f2b308c9eada05
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a>Azure tanılama uzantı yapılandırmasını şema sürümleri ve geçmişi
Bu sayfa dizinlerinin Azure tanılama uzantı şema sürümlerinde Microsoft Azure SDK'sı bir parçası olarak gönderilir.  

> [!NOTE]
> Azure tanılama uzantısını performans sayaçları ve diğer istatistikleri toplamak için kullanılan bileşendir:
> - Azure Sanal Makineler 
> - Sanal Makine Ölçek Kümeleri
> - Service Fabric 
> - Cloud Services 
> - Ağ Güvenlik Grupları
> 
> Bu sayfa, yalnızca bu hizmetlerden biri kullanıyorsanız geçerlidir.

Azure tanılama uzantısını Azure monitör, Application Insights ve günlük analizi gibi diğer Microsoft tanılama ürünlerle kullanılır. Daha fazla bilgi için bkz: [Microsoft izleme araçlarına genel bakış](monitoring-overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Grafik sevkiyat azure SDK ve tanılama sürümleri  

|Azure SDK sürümü | Tanılama genişletme sürümü | modeli|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |eklentisi|  
|2.0 - 2.4         |1.0                            |eklentisi|  
|2.5               |1.2                            |Uzantısı|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|



 İlk Azure SDK'sını yüklendiğinde Azure tanılama sürümü var anlamına eklenti modelinde--sevk azure tanılama sürüm 1.0 ile birlikte gelir.  

 Azure tanılama SDK 2.5 (tanılama sürüm 1.2) ile başlayan bir uzantı modeline geçti. Yeni özellikleri kullanmak için Araçlar yalnızca yeni Azure SDK'ları içinde kullanılabilir, ancak Azure Tanılama'yı kullanarak herhangi bir hizmeti son sevkiyat sürüm doğrudan Azure'dan çekme. Örneğin, hala SDK 2.5 kullanan herkes yeni özellikleri kullanıyorsanız önceki tabloda bakılmaksızın gösterilen en son sürümünü yüklemeye.  

## <a name="schemas-index"></a>Şemalar dizini  
Azure tanılama farklı sürümlerini farklı yapılandırma şemaları kullanabilir. 

[Tanılama 1.0 yapılandırma şeması](azure-diagnostics-schema-1dot0.md)  

[Tanılama 1.2 yapılandırma şeması](azure-diagnostics-schema-1dot2.md)  

[Tanılama 1.3 ve daha sonra yapılandırma şeması](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a>Sürüm geçmişi


### <a name="diagnostics-extension-19"></a>Tanılama uzantısını 1.9 
Docker desteği eklendi.


### <a name="diagnostics-extension-181"></a>Tanılama uzantısını 1.8.1 
Bir SAS belirteci bir depolama hesabı anahtarı yerine özel yapılandırma dosyasında belirtebilirsiniz. Bir SAS belirteci sağlanırsa, depolama hesabı anahtarı göz ardı edilir.


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


### <a name="diagnostics-extension-18"></a>Tanılama uzantısını 1,8 
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
EventHub için rota özelliği eklenmiştir.

### <a name="diagnostics-extension-15"></a>Tanılama uzantısını 1.5
İç havuzlar öğesi ve Tanılama verileri gönderme olanağı eklenen [Application Insights](../application-insights/app-insights-cloudservices.md) sistem ve altyapı düzeyinde yanı sıra, uygulamanızın üzerinde sorunları tanılamak kolaylaştırır.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 ve tanılama uzantısı 1.3 
Visual Studio bulut hizmeti projeleri için aşağıdaki değişiklikler yapıldı. (Bu değişikliklerin Azure SDK'ın sonraki sürümleri için de geçerlidir.)

* Yerel öykünücüsü artık tanılama destekler. Bu tanılama verilerini toplamak ve geliştirme ve Visual Studio'da test ederken, uygulamanızın doğru izlemeleri oluşturuyor olun anlamına gelir. Bağlantı dizesi `UseDevelopmentStorage=true` Azure storage öykünücüsü kullanarak bulut hizmeti projenizi Visual Studio'da çalıştırıyorsanız tanılama veri toplamayı etkinleştirir. Tüm tanılama verilerini (geliştirme depolama) depolama hesabında toplanır.
* Tanılama depolama hesabı bağlantı dizesi (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) bir kez daha hizmet yapılandırma (.cscfg) dosyasında depolanır. Azure SDK 2.5 diagnostics.wadcfgx dosyasında tanılama depolama hesabı belirtilmedi.

Azure SDK 2.4 ve önceki bağlantı dizesini nasıl çalıştığı ve nasıl Azure SDK 2.6 ve daha sonra çalıştığı arasında önemli bazı farklar vardır.

* Azure SDK 2.4 ve önceki sürümlerinde, bağlantı dizesi tanılama günlükleri aktarmak için depolama hesabı bilgilerini almak için bir çalışma zamanı tanılama eklenti tarafından kullanıldı.
* Azure SDK 2.6 ve daha sonra tanılama bağlantı dizesi yayımlama sırasında uygun depolama hesap bilgileriyle tanılama uzantısını yapılandırmak için Visual Studio tarafından kullanılır. Bağlantı dizesi, Visual Studio yayımlama için kullanacağı farklı hizmet yapılandırması için farklı depolama hesapları tanımlamanıza olanak sağlar. Ancak, tanılama eklentisi artık (sonra Azure SDK 2.5) kullanılabilir olmadığından, .cscfg dosyası tek başına tanılama uzantısını etkinleştiremezsiniz. Uzantısı ayrı olarak Visual Studio veya PowerShell gibi araçlar aracılığıyla etkinleştirmeniz gerekir.
* Tanılama uzantısını PowerShell ile yapılandırma işlemini basitleştirmek için Visual Studio Paketi çıktısını de tanılama uzantısını her rol için ortak yapılandırma XML içeriyor. Visual Studio tanılama bağlantı dizesi ortak yapılandırmada depolama hesabı bilgilerini doldurmak için kullanır. Genel yapılandırma dosyaları Uzantıları klasöründe oluşturulur ve desen PaaSDiagnostics izleyin. <RoleName>. PubConfig.xml. Tüm PowerShell tabanlı dağıtımlar, her yapılandırma bir Role eşleştirmek için bu deseni kullanabilirsiniz.
* .Cscfg dosyasında bağlantı dizesini de Azure portal tarafından içinde görüntülenebilir tanılama verilerini erişmek için kullanılan **izleme** sekmesi. Bağlantı dizesi, ayrıntılı izleme verileri portalda göstermek için bu hizmeti yapılandırmak için gereklidir.

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Azure SDK 2.6 ve daha sonra geçirme projeleri
Ardından .wadcfgx dosyasında belirtilen tanılama depolama hesabı varsa Azure SDK 2.5-Azure SDK 2.6 veya sonraki sürümleri geçirilirken var. kalır. Farklı depolama hesapları farklı depolama yapılandırmaları için kullanma esnekliğini yararlanmak için bağlantı dizesi projenize el ile eklemeniz gerekir. Azure SDK 2.4 veya daha önceki Azure SDK 2.6 proje geçiş, tanılama bağlantı dizeleri korunur. Ancak, nasıl bağlantı dizeleri Azure SDK 2.6 belirtildiği şekilde önceki bölümde davranılır değişiklikler lütfen unutmayın.

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Visual Studio tanılama depolama hesabı nasıl belirler
* Bir tanılama bağlantı dizesi .cscfg dosyasında belirtilmediği takdirde, Visual Studio yayımlarken ve genel yapılandırma xml dosyalarını paketlemesi sırasında oluştururken tanılama uzantısını yapılandırmak için kullanır.
* Bir tanılama bağlantı dizesi .cscfg dosyasında belirtilirse, ardından Visual Studio .wadcfgx dosyasında belirtilen depolama hesabı yayımlama ve genel yapılandırma xml dosyalarını oluşturmak tanılama uzantısını yapılandırmak üzere kullanmaya geri döner paketleme olduğunda.
* .Cscfg dosyası tanılama bağlantı dizesinde .wadcfgx dosya depolama hesabında daha önceliklidir. Bir tanılama bağlantı dizesi .cscfg dosyasında belirtilmediği takdirde, Visual Studio kullanan ve .wadcfgx depolama hesabında yok sayar.

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>"Güncelleştirme geliştirme storage bağlantı dizelerini..." yaptığı onay kutusu musunuz?
Onay kutusunu **güncelleştirme geliştirme storage bağlantı dizelerini tanılama ve önbelleğe alma için Microsoft Azure depolama hesabı kimlik bilgileriyle Microsoft Azure yayımlama sırasında** tüm geliştirme güncelleştirmek için kullanışlı bir yöntem sunar Yayımlama sırasında belirtilen Azure depolama hesabı ile depolama hesabı bağlantı dizeleri.

Örneğin, bu onay kutusunu seçin ve tanılama bağlantı dizesini belirtir varsayalım `UseDevelopmentStorage=true`. Projeyi Azure'da yayımlarken, Visual Studio Yayımlama Sihirbazı'nda belirtilen depolama hesabıyla tanılama bağlantı dizesi otomatik olarak güncelleştirir. Gerçek depolama hesabı tanılama bağlantı dizesi olarak belirtilmişse, ancak, ardından o hesabı yerine kullanılır.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Tanılama işlevleri farklılıkları Azure SDK 2.4 ve önceki ve Azure SDK 2,5 ve üzeri
Projenizi Azure SDK 2.4 Azure SDK 2.5 veya sonraki bir sürümüne yükseltiyorsanız, aşağıdaki tanılama işlevleri farklılıkları göz önünde bulundurmanız gerekir.

* **Yapılandırma API'leri dışıdır** – tanılama program yapılandırması Azure SDK 2.4 veya önceki sürümlerde kullanılabilir, ancak Azure SDK 2.5 ve daha sonra kullanım dışı bırakılmıştır. Tanılama yapılandırmanızı kodda şu anda tanımlanmış olması durumunda, bu ayarları çalışmaya devam Diagnostics sırayla geçirilen proje en baştan yeniden yapılandırmanız gerekir. Tanılama yapılandırması için Azure SDK 2.4 diagnostics.wadcfg ve diagnostics.wadcfgx ve sonraki sürümler için Azure SDK 2.5 dosyasıdır.
* **Bulut hizmeti uygulamaları için tanılama yalnızca rol düzeyinde örnek düzeyinde yapılandırılabilir.**
* **Uygulamanızı dağıtma her zaman, tanılama yapılandırması güncelleştirilir** – tanılama yapılandırmanızı Server Explorer'dan değiştirirseniz ve uygulamanızı yeniden dağıtın bu eşlik sorunlara neden olabilir.
* **Azure SDK 2.5 ve daha sonra kilitlenme bilgi dökümleri tanılama yapılandırma dosyasındaki kodu değil, yapılandırılan** – kodda yapılandırılmış kilitlenme bilgi dökümleri varsa, el ile yapılandırma koddan yapılandırma dosyasına aktarmak çünkü gerekir kilitlenme bilgi dökümleri geçiş sırasında Azure SDK 2.6 aktarılmaz.

