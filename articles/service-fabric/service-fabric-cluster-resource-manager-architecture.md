---
title: Resource Manager mimarisi | Microsoft Docs
description: "Service Fabric Küme Kaynağı Yöneticisi'nin mimari bir özeti."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f0d2202c17bf4d378a625a61e941edf7f3f24636
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>Küme kaynak Yönetici Mimarisine Genel Bakış
Service Fabric kümesi Kaynak Yöneticisi kümede çalışan merkezi bir hizmettir. Özellikle kaynak tüketimini ve herhangi bir yerleştirme kuralı göre kümedeki hizmetlerin istenen durumunu yönetir. 

Kümenizdeki kaynakları yönetmek için Service Fabric kümesi Resource Manager çeşitli bilgi parçalarını sahip olmanız gerekir:

- Hangi Hizmetleri şu anda var
- Her hizmet geçerli (veya varsayılan) kaynak tüketimi 
- Kalan küme kapasite 
- Kümedeki düğümler kapasitesi 
- Her bir düğümde tüketilen kaynaklarının miktarını

Belirli bir hizmeti kaynak tüketimini zamanla değişebilir ve Hizmetleri hakkında daha fazla kaynak türü genellikle dikkat edin. Farklı hizmetler arasında ölçülen gerçek fiziksel ve fiziksel kaynakları olabilir. Hizmetleri, bellek ve disk kullanımı gibi fiziksel ölçümleri izleyebilirsiniz. Daha yaygın olarak, hizmetler, mantıksal ölçümleri - "WorkQueueDepth" veya "TotalRequests" gibi şeyleri önem verdiğiniz. Mantıksal ve fiziksel ölçümleri aynı kümede kullanılabilir. Ölçümleri birçok hizmetleri arasında paylaşılabilir veya belirli bir hizmete özgü olmalıdır.

## <a name="other-considerations"></a>Diğer konular
Aynı kişiler takan farklı şapkalar en az olan veya sahipleri ve işleçler kümesinin hizmet ve uygulama yazarların farklı olabilir. Uygulamanızı geliştirirken hangi gerektirdiğini hakkında birkaç şey bildirin. Tahmini tüketen kaynakların sahip ve nasıl farklı Hizmetleri dağıtılmalıdır. Örneğin, web katmanı veritabanı hizmetleri vermemelisiniz sırada Internet'e açık düğümlerinde çalışması gerekir. Başka bir örnek olarak, web Hizmetleri büyük olasılıkla CPU ve bellek ve disk kullanımı hakkında daha fazla veri katmanı Hizmetleri bakım sırasında ağ tarafından kısıtlanmıştır. Ancak, bu hizmet üretim veya kimin hizmet yükseltme yönetme için Canlı site olay işleme kişinin yapmak için farklı bir iş vardır ve farklı araçlar gerektirir. 

Küme ve Hizmetleri dinamik şunlardır:

- Kümedeki düğüm sayısını büyür ve küçültme
- Farklı boyutlarda ve türleri düğümlerinin dönün ve Git
- Hizmetleri kaldırılmış oluşturulabilir ve onların istenen kaynak ayırma ve yerleştirme kuralları değiştirme
- Yükseltme veya diğer yönetim işlemleri uygulama küme aracılığıyla altyapı düzeylerini geri alabilirsiniz
- Herhangi bir zamanda hataları oluşabilir.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Küme Kaynak Yöneticisi bileşenleri ve veri akışı
Bu hizmetlerin içindeki her hizmet nesnesi tarafından her hizmet gereksinimlerine ve kaynaklarının kullanımını izlemek Küme Kaynak Yöneticisi'ni vardır. Küme Kaynak Yöneticisi'ni kavramsal iki bölümden oluşur: her düğüm ve hataya dayanıklı bir hizmeti çalıştıran aracılar. Her düğüm izleme yük aracısında Hizmetleri'nden toplama bunları bildirir ve düzenli aralıklarla rapor. Küme Kaynak Yöneticisi hizmeti yerel aracıları ve kendi geçerli yapılandırmasını temel alarak tepki verdiğini tüm bilgileri toplar.

Aşağıdaki diyagramda bakalım:

<center>
![Kaynak dengeleyici mimarisi][Image1]
</center>

Çalışma zamanı sırasında meydana gelebilir birçok değişiklik yoktur. Örneğin, şimdi deyin bazı hizmetlerini kullanma kaynakları miktarını, bazı hizmetleri başarısız ve bazı düğümler birleştirme ve değişiklikleri küme bırakın. Bir düğüm üzerindeki tüm değişiklikleri toplanır ve düzenli aralıklarla burada bunlar yeniden birleştirilir, analiz ve depolanan Küme Kaynak Yöneticisi Hizmeti'ne (1,2) gönderilir. Hizmet değişikliklerini arar ve herhangi bir eylem gerekli olup olmadığını belirleyen her birkaç saniyede (3). Örneğin, bazı boş düğümler kümeye eklendiğini fark. Sonuç olarak, bazı hizmetler için düğümleri taşıma karar verir. Küme Kaynak Yöneticisi'ni de belirli bir düğüme aşırı yüklendiğinde veya belirli hizmetleri başarısız veya silinmiş, fark başka bir yerde kaynakları serbest bırakma.

Şimdi Aşağıdaki diyagramda bakın ve ne olacağını bakın. Küme Kaynak Yöneticisi'ni değişikliklerin gerekli olduğunu belirler varsayalım. Diğer sistem hizmetleriyle (belirli Yük Devretme Yöneticisi) gerekli değişiklikleri düzenler. Ardından gerekli komutları uygun düğümlerine (4) gönderilir. Örneğin, Kaynak Yöneticisi'ni Düğüm5 fazlaydı ve bu nedenle hizmet B Düğüm4 için Düğüm5 taşımak karar fark varsayalım. Yeniden yapılandırma (5) sonunda, küme şöyle görünür:

<center>
![Kaynak dengeleyici mimarisi][Image2]
</center>

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi'ni küme açıklamak için birçok seçeneğiniz vardır. Bunları hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](./service-fabric-cluster-resource-manager-cluster-description.md)
- Küme Kaynak Yöneticisi'nin birincil görevlerini küme dengelenmesi ve yerleştirme kuralları uygulanması. Bu davranışların yapılandırma hakkında daha fazla bilgi için bkz: [Dengeleme Service Fabric kümesi](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
