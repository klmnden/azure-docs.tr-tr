---
title: Bir Azure bulut hizmeti izleme | Microsoft Docs
description: Hangi Azure bulut hizmeti izleme içerir ve hangi seçeneklerinizi bazı açıklar şunlardır.
services: cloud-services
documentationcenter: ''
author: thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: adegeo
ms.openlocfilehash: f3a3a1beb8540ee8ab0502379396c06ea505fb44
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
ms.locfileid: "29149915"
---
# <a name="introduction-to-cloud-service-monitoring"></a>Bulut hizmeti izleme giriş

Herhangi bir bulut hizmeti için temel performans ölçümlerini izleyebilir. Her bulut hizmet rolü en düşük düzeyde veri toplar: CPU kullanımı, ağ kullanımını ve disk kullanımı. Bulut hizmeti varsa `Microsoft.Azure.Diagnostics` bir rolü, bu rolü uygulanan uzantısı ek veri noktalarını toplayabilir. Bu makalede bulut Hizmetleri için Azure tanılama tanıtılmaktadır.

Temel izleme ile performans sayacı verilerini rol örneklerinden örneklenen ve 3 dakika aralıklarla toplanır. Bu temel izleme verilerini depolama hesabınızdaki depolanmaz ve ek ücret ödemeden kendisiyle ilişkili.

Gelişmiş izleme ile ek ölçümler örneklenen ve 5 dakika, 1 saat ve 12 saat aralıklarla toplanır. Toplanan veri tabloları, bir depolama hesabında depolanır ve 10 gün sonra temizlenir. Kullanılan depolama hesabı role göre yapılandırılmıştır; farklı depolama hesapları için farklı roller kullanabilirsiniz. Bu bir bağlantı dizesi ile yapılandırılmış [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyaları.


## <a name="basic-monitoring"></a>Temel izleme

Girişte belirtildiği gibi bir bulut hizmeti temel izleme verilerini otomatik olarak ana bilgisayar sanal makineden toplar. Bu veriler, CPU yüzdesi, ağ giriş/çıkış ve disk okuma/yazma içerir. Toplanan izleme verilerini otomatik olarak Azure portalındaki bulut hizmetinin genel bakış ve ölçümleri sayfalarında görüntülenir. 

Temel izleme bir depolama hesabı gerektirmez. 

![temel bulut hizmeti döşeme izleme](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Gelişmiş izleme içerir kullanarak **Azure tanılama** uzantısı (ve isteğe bağlı olarak Application Insights SDK'sı) izlemek istediğiniz rolü. Tanılama uzantısını adlı bir yapılandırma dosyası (her bir rolü) kullanan **diagnostics.wadcfgx** izlenen tanılama ölçümleri yapılandırmak için. Azure tanılama uzantısını toplar ve bir Azure depolama hesabında verileri depolar. Bu ayarları yapılandırılan **.wadcfgx**, [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef), ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyaları. Bu fazladan olduğu anlamına gelir ilişkili maliyet Gelişmiş izleme.

Her rol oluşturulduğundan, Visual Studio için Azure tanılama uzantısını ekler. Bu tanılama uzantısını aşağıdaki bilgi türlerini toplayabilirsiniz:

* Özel performans sayaçları
* Uygulama günlükleri
* Windows olay günlükleri
* .NET olay kaynağı
* IIS günlükleri
* Temel ETW bildirimi
* Kilitlenme bilgi dökümleri
* Müşteri hata günlükleri

> [!IMPORTANT]
> Bu veriler storage hesabına toplanır olsa da, portal mu **değil** grafik verileri için yerel bir yol sağlar. Uygulamanıza Application Insights gibi başka bir hizmet tümleştirmek önerilir.

## <a name="setup-diagnostics-extension"></a>Tanılama uzantısını Kurulumu

Ürününe sahip değilseniz, ilk olarak, bir **Klasik** depolama hesabı [oluşturmak](../storage/common/storage-create-storage-account.md#create-a-storage-account). Emin olun depolama hesabı ile oluşturulur **Klasik dağıtım modeli** belirtilen.

Ardından, gitmek **depolama hesabı (Klasik)** kaynak. Seçin **ayarları** > **erişim anahtarları** ve kopyalama **birincil bağlantı dizesi** değeri. Bulut hizmeti için bu değer gerekir. 

Etkinleştirilmesi Gelişmiş tanılama değiştirmelisiniz iki yapılandırma dosyası yok **ServiceDefinition.csdef** ve **ServiceConfiguration.cscfg**.

### <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef

İçinde **ServiceDefinition.csdef** dosya, adlı yeni bir ayar Ekle `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` her rol için Gelişmiş tanılama kullanır. Yeni bir proje oluşturduğunuzda, visual Studio bu değer dosyasına ekler. Eksik olması durumunda, artık ekleyebilirsiniz. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Bu her eklenmelidir yeni bir ayar tanımlar **ServiceConfiguration.cscfg** dosya. 

İki büyük olasılıkla sahip **.cscfg** dosyaları, bir adlı **ServiceConfiguration.cloud.cscfg** Azure ve adlı bir dağıtmak için **ServiceConfiguration.local.cscfg** Bu adres benzetilmiş ortamında yerel dağıtımları için kullanılır. Açın ve her değişiklik **.cscfg** dosya. Adlı bir ayar Ekle `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. Değer kümesine **birincil bağlantı dizesi** Klasik depolama hesabının. Geliştirme makinenizde yerel depolama kullanmak istiyorsanız, kullanmak `UseDevelopmentStorage=true`.

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />
      
      <!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
      -->
```

## <a name="use-application-insights"></a>Application Insights kullanın

Visual Studio bulut hizmetinden yayımladığınızda, tanılama verilerini Application Insights'a gönderme seçeneği verilir. O anda uygulama Öngörüler Azure kaynak oluşturabilir veya mevcut bir Azure kaynağı için verileri gönderin. Bulut hizmetiniz, kullanılabilirlik, performans, hataları ve kullanım için Application Insights tarafından izlenebilir. Özel grafikler verileri görebilmesi için Application Insights eklenebilir, önemli en. Rol örneği verileri bulut hizmeti projenizi Application Insights SDK'sı kullanarak toplanabilir. Application Insights tümleştirme hakkında daha fazla bilgi için bkz: [Application Insights bulut hizmetleriyle](../application-insights/app-insights-cloudservices.md).

Performans sayaçlarını (ve diğer ayarları) görüntülemek için Application Insights kullanabilmenize karşın, Windows Azure Diagnostics uzantısı aracılığıyla, yalnızca sizin belirttiğiniz not içine Application Insights SDK'sı ile tümleştirerek zengin bir deneyim almak için Worker ve web rolleri.


## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights ile bulut hizmetleri hakkında bilgi edinin](../application-insights/app-insights-cloudservices.md)
- [Performans sayaçlarını kurma](diagnostics-performance-counters.md)

