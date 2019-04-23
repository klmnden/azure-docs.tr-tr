---
title: Azure Service Fabric destek seçenekleri hakkında bilgi edinin | Microsoft Docs
description: Azure Service Fabric küme sürümleri desteklenir ve dosya bağlantılarını bilet desteği
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: a931de8be07d41cf4daab63aa7691973ee158452
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60005052"
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric destek seçenekleri

Uygulama iş yükleri çalıştıran Service Fabric kümeleriniz için uygun destek sunmak için biz sizin için çeşitli seçenekleri ayarladınız. Gerekli destek düzeyini ve sorunun önem derecesine bağlı olarak, uygun seçenekleri seçin alın. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Üretim sorun bildirin veya Azure için Ücretli destek isteği

Azure'da dağıtılan Service Fabric kümenizdeki sorunları raporlama için bir destek bileti açın [Azure portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) veya [Microsoft destek portalı](https://support.microsoft.com/oas/default.aspx?prid=16146).

Aşağıdakiler hakkında daha fazla bilgi edinin:
 
- [Azure için Microsoft desteğine](https://azure.microsoft.com/support/plans/?b=16.44).
- [Microsoft premier desteği](https://support.microsoft.com/en-us/premier).

> [!Note]
> Bronz güvenilirlik katmanı üzerinde çalıştıran kümeler yalnızca test iş yüklerini çalıştırmanıza olanak tanır. Bronz güvenilirlik üzerinde çalışan bir küme ile ilgili sorun yaşıyorsanız, Microsoft destek ekibi, sorunu giderme de yardımcı olur, ancak kök neden analizi gerçekleştirmez. Lütfen [güvenilirliği kümenin](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) daha fazla ayrıntı için.
>
> Üretime hazır bir küme için gerekli olan hakkında daha fazla bilgi için bkz [üretim hazırlık denetim](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Üretim sorun bildirin veya tek başına Service Fabric kümeleri için Ücretli destek isteği

Service Fabric kümenizdeki sorunları dağıtılan şirket içi raporlama veya diğer bulutlarda bir profesyonel destek bileti açmak [Microsoft destek portalı](https://support.microsoft.com/oas/default.aspx?prid=16146).

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

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric Önizleme sürümleri üretim kullanımı için desteklenmeyen-

Zaman zaman, Önizleme kullanıma sunulur, geri bildirim istiyoruz önemli özellikleri olan sürümler sürüm. Bu önizleme sürümleri, yalnızca test amacıyla kullanılmalıdır. Üretim kümenizi her zaman desteklenen ve kararlı bir Service Fabric sürümü çalıştırıyor olmalıdır. Bir önizleme sürümü her zaman bir 255 büyük ve küçük sürüm numarasıyla başlar. Örneğin, bir Service Fabric sürümü 255.255.5703.949 görürseniz, bu sürümü yalnızca test kümeleri kullanılır ve önizleme aşamasındadır. Bu önizleme sürümleri üzerinde de duyurulur [Service Fabric ekibi blogu](https://blogs.msdn.microsoft.com/azureservicefabric) ve ayrıntıları dahil edilen özellikler üzerinde sahip olacaktır.
Bu önizleme sürümleri için Ücretli destek seçeneği yoktur. Altında listelenen seçeneklerden birini kullanın [rapor Azure Service Fabric sorunları](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) soru sormak veya geri bildirim sağlamak için.

## <a name="next-steps"></a>Sonraki adımlar

[Service Fabric desteklenen sürümler](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples