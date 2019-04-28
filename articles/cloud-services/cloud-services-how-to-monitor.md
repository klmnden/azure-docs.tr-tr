---
title: Azure bulut hizmeti izleme | Microsoft Docs
description: Hangi Azure bulut hizmeti izleme içerir ve hangi seçeneklerinizi bazılarını açıklar olan.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: ''
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: jeconnoc
ms.openlocfilehash: 844fef9a87c1db06c6415c59d4be26caf928382b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61432911"
---
# <a name="introduction-to-cloud-service-monitoring"></a>Bulut hizmeti izleme giriş

Herhangi bir bulut hizmeti için temel performans ölçümlerini izleyebilir. Her bulut hizmeti rolü en düşük düzeyde veri toplar: CPU kullanımı, ağ kullanımı ve disk kullanımı. Bulut hizmetine sahipse `Microsoft.Azure.Diagnostics` uzantısı bir rolü, bu rolü uygulanan ek noktaları veri toplayabilir. Bu makalede bulut Hizmetleri için Azure tanılama tanıtır.

Temel izleme ile performans sayacı verileri rol örneklerindeki örneklenir ve 3 dakikalık aralıklarla toplanır. Bu temel izleme verilerini depolama hesabınızda depolanmaz ve hiçbir ek ücret ile ilişkili.

Gelişmiş izleme ile ek ölçümler örneklenir ve 1 saat ile 12 saat 5 dakikalık aralıklarla toplanır. Toplanan veriler, tablolar, bir depolama hesabında depolanır ve 10 gün sonra temizlenir. Kullanılan depolama hesabı, rolü tarafından yapılandırılır; farklı depolama hesapları için farklı roller kullanabilirsiniz. Bu bağlantı dizesinde yapılandırılmış [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyaları.


## <a name="basic-monitoring"></a>Temel izleme

Giriş belirtildiği gibi bir bulut hizmeti temel izleme verilerini otomatik olarak konak sanal makineden toplar. Bu veriler, CPU yüzdesi, ağ daraltma/genişletme ve disk okuma/yazma içerir. Toplanan izleme verilerinin otomatik olarak Azure portalındaki bulut hizmetinin genel bakış ve ölçümleri sayfalarında görüntülenir. 

Temel izleme, bir depolama hesabı gerektirmez. 

![kutucukları izleme temel bir bulut hizmeti](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Gelişmiş izlemeyi içerir kullanarak **Azure tanılama** uzantısı (ve isteğe bağlı olarak Application Insights SDK'sı) izlemek istediğiniz rol. Tanılama uzantısını adlı bir yapılandırma dosyası (her bir rolü) kullanan **diagnostics.wadcfgx** izlenen tanılama ölçümünü yapılandırmak için. Azure tanılama uzantısı, toplar ve bir Azure depolama hesabında verileri depolar. Bu ayarları içinde yapılandırılmış **.wadcfgx**, [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef), ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyaları. Bu ek olduğu anlamına gelir ile ilişkili maliyetini Gelişmiş izleme.

Her rolün oluşturulduğundan, Visual Studio için Azure tanılama uzantısını ekler. Bu tanılama uzantısı, aşağıdaki bilgi türlerini toplayabilirsiniz:

* Özel performans sayaçları
* Uygulama günlükleri
* Windows olay günlükleri
* .NET olay kaynağı
* IIS günlükleri
* Bildirim tabanlı ETW
* Kilitlenme bilgi dökümleri
* Müşteri hata günlükleri

> [!IMPORTANT]
> Tüm bu veriler depolama hesabında toplanır, ancak portalda mu **değil** verilerin grafiğini oluşturmak için yerel bir yol sağlar. Uygulamanıza Application Insights gibi başka bir hizmete tümleştirme önemle tavsiye edilir.

## <a name="setup-diagnostics-extension"></a>Kurulum tanılama uzantısı

Öğeniz yoksa ilk olarak, bir **Klasik** depolama hesabı [oluşturmak](../storage/common/storage-quickstart-create-account.md). Emin depolama hesabı ile oluşturulduğundan **Klasik dağıtım modelini** belirtilen.

Ardından, gitmek **depolama hesabı (Klasik)** kaynak. Seçin **ayarları** > **erişim anahtarları** ve kopyalama **birincil bağlantı dizesi** değeri. Bulut hizmeti için bu değere ihtiyacınız. 

İki yapılandırma dosyası etkinleştirilmesi Gelişmiş tanılama değiştirmelisiniz **ServiceDefinition.csdef** ve **ServiceConfiguration.cscfg**.

### <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef

İçinde **ServiceDefinition.csdef** adlı yeni bir ayar ekleyin `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` her rol için Gelişmiş tanılama kullanır. Yeni bir proje oluşturduğunuzda, visual Studio dosyayı bu değeri ekler. Eksik olması durumunda, artık ekleyebilirsiniz. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Bu, her eklenmelidir yeni bir ayarı tanımlar **ServiceConfiguration.cscfg** dosya. 

Büyük olasılıkla iki tane **.cscfg** dosyaları, bir adlı **ServiceConfiguration.cloud.cscfg** Azure ve adlı bir dağıtmak için **ServiceConfiguration.local.cscfg** benzetilmiş ortamında yerel dağıtımlar için kullanılmıştır. Açın ve her **.cscfg** dosya. Adlı bir ayar ekleyin `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`. Değerine **birincil bağlantı dizesi** Klasik depolama hesabı. Geliştirme makinenizde yerel depolama kullanmak istiyorsanız, kullanın `UseDevelopmentStorage=true`.

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

Visual Studio bulut hizmetinden yayımladığınızda, Application Insights'a tanılama verilerini gönderme seçeneği sunulur. O anda Application Insights Azure kaynak oluşturabilir veya mevcut bir Azure kaynak verileri gönderin. Kullanılabilirlik, performans, hatalar ve kullanım için bulut hizmetinizi Application Insights tarafından izlenebilir. Özel grafikler verileri görebilmesi için Application Insights eklenebilir en, önemlidir. Rol örneği veri, bulut hizmeti projenizi Application Insights SDK'sını kullanarak toplanabilir. Application Insights'ı tümleştirme hakkında daha fazla bilgi için bkz. [Cloud Services ile Application Insights](../azure-monitor/app/cloudservices.md).

Performans sayaçlarını (ve diğer ayarları) görüntülemek için Application Insights kullanabilmenize karşın Windows Azure tanılama uzantısı, yalnızca belirttiğiniz Not uygulamasına Application Insights SDK'sı ile tümleştirerek daha zengin bir deneyim elde, Worker ve web rolleri.


## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights ile bulut hizmetleri hakkında bilgi edinin](../azure-monitor/app/cloudservices.md)
- [Performans sayaçlarını kurma](diagnostics-performance-counters.md)

