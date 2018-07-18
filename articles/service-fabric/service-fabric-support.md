---
title: Azure Service Fabric destek seçenekleri hakkında bilgi edinin | Microsoft Docs
description: Azure Service Fabric küme sürümleri desteklenir ve dosya bağlantılarını bilet desteği
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/13/2018
ms.author: pkc
ms.openlocfilehash: 596e71be75453874492aac15d91cb6153c2076f5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112899"
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric destek seçenekleri

Uygulama iş yükleri çalıştıran Service Fabric kümeleriniz için uygun destek sunmak için biz sizin için çeşitli seçenekleri ayarladınız. Gerekli destek düzeyini ve sorunun önem derecesine bağlı olarak, uygun seçenekleri seçin alın. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Üretim sorun bildirin veya Azure için Ücretli destek isteği

Azure'da dağıtılan Service Fabric kümenizdeki sorunları raporlama için bir destek bileti açın [Azure portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) veya [Microsoft destek portalı](http://support.microsoft.com/oas/default.aspx?prid=16146).

Aşağıdakiler hakkında daha fazla bilgi edinin:
 
- [Azure için Microsoft desteğine](https://azure.microsoft.com/support/plans/?b=16.44).
- [Microsoft premier desteği](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Üretim sorun bildirin veya tek başına Service Fabric kümeleri için Ücretli destek isteği

Service Fabric kümenizdeki sorunları dağıtılan şirket içi raporlama veya diğer bulutlarda bir profesyonel destek bileti açmak [Microsoft destek portalı](http://support.microsoft.com/oas/default.aspx?prid=16146).

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Şirket içi Microsoft profesyonel destek](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft premier desteği](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Rapor Azure Service Fabric sorunları 
Service Fabric sorunları raporlama için GitHub deposuna'kurmak ayarladınız.  Biz de etkin bir şekilde aşağıdaki forumları izlemekte olduğunuz.

### <a name="github-repo"></a>GitHub deposu 
Azure Service Fabric sorunların rapor [Service Fabric sorunların git deposu](https://github.com/Azure/service-fabric-issues). Bu deponun sorunları küçük özellik istekleri yapmak için Azure Service Fabric ile izleme ve raporlama için tasarlanmıştır. **Bu raporda Canlı site sorunlarıyla kullanmayın**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow ve MSDN Forumları
[StackOverflow üzerinde Service Fabric etiketi] [ stackoverflow] ve [MSDN Forumu Service Fabric] [ msdn-forum] sorular hakkında nasıl sormak için kullanılan en iyi olan Platform çalışır ve belirli görevlerle nasıl yapabilirsiniz.

### <a name="azure-feedback-forum"></a>Azure geri bildirim Forumu
[Azure Service Fabric için geri bildirim Forumu] [ uservoice-forum] sahip olduğunuz ürün için size en popüler isteklerini gözden geçirirken büyük özellik fikirleri, uzun vadeli planlama bizim Orta parçası olan göndermek için en iyi yerdir. Topluluk içinde önerilerinizi desteği rally etmenizi öneririz.


## <a name="supported-service-fabric-versions"></a>Desteklenen Service Fabric sürümleri.

Kümenizi, desteklenen bir Service Fabric sürümü her zaman çalışır durumda olduğundan emin olun. Gibi ve biz Service Fabric yeni bir sürümünü duyurmaktan zaman önceki sürümü en az 60 gün o tarihten sonra destek sonu için işaretlenir. Yeni yayınlar duyurulan [Service Fabric Ekibi blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/).

Desteklenen bir Service Fabric sürümünü çalıştırıyor kümenizin tutmak hakkında ayrıntılı bilgi aşağıdaki belgelere bakın.

- [Bir Azure kümesinde Service Fabric sürümüne yükseltin ](service-fabric-cluster-upgrade.md)
- [Bir tek başına windows server kümesinde Service Fabric sürümüne yükseltin ](service-fabric-cluster-upgrade-windows-server.md)
 
Desteklenen Service Fabric sürümlerinin listesini ve bunların destek bitiş tarihi aşağıda verilmiştir.

| **Kümedeki Service Fabric çalışma zamanı** | **Doğrudan küme sürümünden yükseltme yapabilirsiniz** |**Uyumlu SDK / NuGet paket sürümleri** | **Destek sonu tarihi** |
| --- | --- |--- | --- |
| 5.3.121 önce tüm küme sürümleri | 5.1.158* |2.3 sürümü küçüktür veya eşittir |20 Ocak 2017 |
| 5.3.* | 5.1.158.* |2.3 sürümü küçüktür veya eşittir |24 Şubat 2017 |
| 5.4.* | 5.1.158.* |Sürüm 2.4 küçüktür veya eşittir |10,2017 olabilir       |
| 5.5.* | 5.4.164.* |Sürüm 2.5 küçüktür veya eşittir |Ağustos 10,2017    |
| 5.6.* | 5.4.164.* |Sürüm 2.6 küçüktür veya eşittir |Ekim 13,2017   |
| 5.7.* | 5.4.164.* |Sürüm 2.7 küçüktür veya eşittir |Aralık 15,2017  |
| 6.0.* | 5.6.205.* |Sürüm 2.8 küçüktür veya eşittir |30,2018 Mart     | 
| 6.1. * | 5.7.221.* |Sürüm 3.0 küçüktür veya eşittir |Temmuz 15,2018      |
| 6.2. * | 6.0.232.* |Sürüm 3.1 küçüktür veya eşittir |Eylül 15,2018 |
| 6.3. * | 6.1.480.* |Sürüm 3.2 küçüktür veya eşittir |Geçerli sürümü ve bu nedenle bitiş tarihi |

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric Önizleme sürümleri - üretim kullanımı için desteklenmiyor.
Zaman zaman, Önizleme kullanıma sunulur, geri bildirim istiyoruz önemli özellikleri olan sürümler sürüm. Bu önizleme sürümleri, yalnızca test amacıyla kullanılmalıdır. Üretim kümenizi her zaman desteklenen ve kararlı bir Service Fabric sürümü çalıştırıyor olmalıdır. Bir önizleme sürümü her zaman bir 255 büyük ve küçük sürüm numarasıyla başlar. Örneğin, bir Service Fabric sürümü 255.255.5703.949 görürseniz, bu sürümü yalnızca test kümeleri kullanılır ve önizleme aşamasındadır. Bu önizleme sürümleri üzerinde de duyurulur [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric) ve ayrıntıları dahil edilen özellikler üzerinde sahip olacaktır.

Bu önizleme sürümleri için Ücretli destek seçeneği yoktur. Altında listelenen seçeneklerden birini kullanın [rapor Azure Service Fabric sorunları](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) soru sormak veya geri bildirim sağlamak için.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure kümesinde Service fabric sürümüne yükseltin ](service-fabric-cluster-upgrade.md)
- [Bir tek başına windows server kümesinde Service Fabric sürümüne yükseltin ](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
