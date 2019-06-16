---
title: Klasik dağıtım modeli API'lerini Azure İzleyici emeklilik ölçümleri ve otomatik ölçeklendirme
description: Ölçümler ve otomatik ölçeklendirme Klasik API'leri de adlı Azure Hizmet Yönetimi (ASM) veya devre dışı bırakılıyor RDFE dağıtım modeli
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: robb
ms.openlocfilehash: ce54b63aa7831ed40a8592d536c43fc83fdc5567
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60709992"
---
# <a name="azure-monitor-retirement-of-classic-deployment-model-apis-for-metrics-and-autoscale"></a>Klasik dağıtım modeli API'lerini Azure İzleyici emeklilik ölçümleri ve otomatik ölçeklendirme

Azure İzleyici (eski adıyla Azure ilk yayımlandığında öngörüleri) şu anda oluşturup yönetmek için otomatik ölçeklendirme ayarları ve klasik VM'ler ve klasik bulut Hizmetleri ölçümleri kullanma olanağı sunar. Klasik dağıtım modeli tabanlı API'ler olacaktır özgün kümesini **30 Haziran 2019 sonra kullanımdan** tüm bölgelerde tüm Azure genel ve özel bulutlarda.   

Aynı işlemleri aracılığıyla desteklenen bir Azure Resource Manager API'leri için bir yıl boyunca bağlı. Azure portalında, otomatik ölçeklendirme ve ölçümler için yeni REST API'lerini kullanır. Yeni bir SDK'sı, PowerShell ve CLI bu Resource Manager API'lerine göre de kullanılabilir. Yeni izleme için iş ortağı hizmetlerimizi kullanma Resource Manager tabanlı Azure İzleyici REST API'leri.  

## <a name="who-is-not-affected"></a>Kimin etkilenmez

Otomatik ölçeklendirme Azure portalı üzerinden yönetiyorsanız [yeni Azure İzleyici SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/), PowerShell, CLI veya Resource Manager şablonları, herhangi bir eylemi gerekli değildir.  

Azure portalı veya yoluyla çeşitli ölçümleri kullanma, [izleme iş ortağı Hizmetleri](../../azure-monitor/platform/partners.md), herhangi bir eylemi gerekli değildir. Microsoft, izleme için yeni API'ler geçirmek için iş ortakları ile çalışmaktadır.

## <a name="who-is-affected"></a>Kimler etkilenir

Aşağıdaki bileşenleri kullanıyorsanız, bu makale için geçerlidir:

- **Klasik Azure Insights SDK'sı** - kullanıyorsanız [Klasik Azure Insights SDK'sı](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/), geçiş için yeni Azure İzleyici SDK kullanarak [.NET](https://github.com/azure/azure-libraries-for-net#download) veya [Java](https://github.com/azure/azure-libraries-for-java#download). İndirme [Azure İzleyici SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/).

- **Klasik otomatik ölçeklendirme** - aradığınız [Klasik otomatik ölçeklendirme ayarları API'leri](https://msdn.microsoft.com/library/azure/mt348562.aspx) işiniz, Araçlar veya bu adı kullanıyor [Klasik Azure Insights SDK'sı](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/), kullanmayageçişyapmanızgerekir[ Azure İzleyici Kaynak Yöneticisi REST API](https://docs.microsoft.com/rest/api/monitor/autoscalesettings).

- **Klasik ölçüm** - ölçümleri kullanarak tüketen [Klasik REST API'leri](https://msdn.microsoft.com/library/azure/dn510374.aspx) veya [Klasik Azure Insights SDK'sı](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/) ürettikleri araçlarından kullanmaya geçer [ Azure İzleyici Kaynak Yöneticisi REST API](https://docs.microsoft.com/rest/api/monitor/autoscalesettings). 

Kod veya özel araçlar Klasik API'leri olup çağırma emin değilseniz, aşağıdaki konumda arayın:

- Kod veya aracı başvurulan URI gözden geçirin. Klasik API'leri bir URI kullanın https://management.core.windows.net. Yeni URI'si için kaynak tabanlı API'ler ile başlayan Yöneticisi kullanılmalı https://management.azure.com/.

- Derleme adı makinenizde karşılaştırın. Eski Klasik derleme altındadır https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/.

- Ölçümleri veya otomatik ölçeklendirme API'leri erişmek için sertifika kimlik doğrulaması kullanıyorsanız, bir Klasik uç noktası ve kitaplık kullanmaktadır. Daha yeni Resource Manager API'leri aracılığıyla hizmet sorumlusu veya kullanıcı asıl Azure Active Directory kimlik doğrulaması gerektirir.

- Aşağıdaki bağlantılardan herhangi birine adresindeki başvurulan çağrıları kullanıyorsanız, eski Klasik API'leri kullanıyor.

  - [Windows.Azure.Management.Monitoring sınıf kitaplığı](https://docs.microsoft.com/previous-versions/azure/dn510414(v=azure.100))

  - [(Klasik) .NET izleme](https://docs.microsoft.com/previous-versions/azure/reference/mt348562(v%3dazure.100))

  - [IMetricOperations arabirimi](https://docs.microsoft.com/previous-versions/azure/reference/dn802395(v%3dazure.100))

## <a name="why-you-should-switch"></a>Neden geçiş yapmanız gerekir

Otomatik ölçeklendirme ve ölçümler için tüm mevcut özellikler yeni API'ler çalışmaya devam eder.  

Üzerinden geçiş için daha yeni Resource Manager tabanlı özellikleri gibi özellikler tutarlı rol tabanlı erişim denetimi (RBAC) arasında tüm izleme hizmetleri için API'leri gelir. Ölçümler için ek işlevler de sahip olabilirsiniz: 

- boyutları için destek
- Tüm hizmetler arasında tutarlı 1 dakikalık ölçüm ayrıntı 
- daha iyi sorgulama
- daha yüksek veri saklama (ölçümleri vs 93 gün. 30 gün) 

Azure Resource Manager tabanlı Azure İzleyici API'leri daha iyi performans, ölçeklenebilirlik ve güvenilirlik gelen tüm diğer hizmetler gibi genel. 

## <a name="what-happens-if-you-do-not-migrate"></a>Geçişini ne olur

### <a name="before-retirement"></a>Önce devre dışı bırakma

Azure hizmetlerinizi veya iş yüklerini doğrudan herhangi bir etki olmayacak.  

### <a name="after-retirement"></a>Sonra devre dışı bırakma

Klasik API'leri daha önce listelenen tüm çağrıları başarısız olur ve aşağıdaki sorguyu benzer hata iletileri döndürür:

Otomatik ölçeklendirme için: *Bu API kullanım dışıdır. Otomatik ölçeklendirme ayarlarını yönetmek için Azure portalı, Azure İzleyici SDK'sı, PowerShell, CLI veya Resource Manager şablonlarını kullanma*.  

Ölçümler için: *Bu API kullanım dışıdır. Kullanım Azure portalı, Azure İzleyici SDK'sı, PowerShell, CLI sorgulamak için ölçümleri*.

## <a name="email-notifications"></a>E-posta bildirimleri

Kullanımdan kaldırma bildirimi e-posta adresleri şu hesabı rolleri için gönderildi: 

- Hesabı ve hizmet yöneticileri
- Diğer yöneticiler  

Herhangi bir sorunuz varsa bizimle iletişime geçin MonitorClassicAPIhelp@microsoft.com.  

## <a name="references"></a>Başvurular

- [Azure İzleyici için daha yeni REST API'leri](https://docs.microsoft.com/rest/api/monitor/) 
- [Yeni Azure İzleyici SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)
