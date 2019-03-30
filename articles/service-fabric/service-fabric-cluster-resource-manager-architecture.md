---
title: Resource Manager mimarisi | Microsoft Docs
description: Bir mimari genel bakış, Service Fabric Küme Kaynak Yöneticisi.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bfbdb05e8d2764d2b878e22d236cae30519da176
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666809"
---
# <a name="cluster-resource-manager-architecture-overview"></a>Küme Kaynak Yöneticisi mimarisine genel bakış
Service Fabric Küme Kaynak Yöneticisi kümede çalışan merkezi bir hizmettir. Bu, özellikle kaynak tüketimine ve herhangi bir yerleştirme kuralları göre kümedeki hizmetlerin istenen durum yönetir. 

Küme kaynaklarını yönetmek için Service Fabric Küme Kaynak Yöneticisi birkaç bilgilere sahip olmanız gerekir:

- Şu anda hangi hizmetlerin var
- Her hizmet geçerli (veya varsayılan) kaynak tüketimi 
- Kalan küme kapasitesi 
- Kümedeki düğümler kapasitesi 
- Her bir düğümde tüketilen kaynak miktarını

Belirli bir hizmete kaynak tüketimini zaman içinde değişebilir ve Hizmetleri genellikle birden fazla kaynak türü hakkında dikkatli olun. Farklı hizmetleri arasında ölçülen fiziksel gerçek ve fiziksel kaynaklar olabilir. Hizmetleri, bellek ve disk tüketimi gibi fiziksel ölçümleri izleyebilirsiniz. Daha yaygın olarak, hizmetleri, mantıksal ölçümleri - "WorkQueueDepth" veya "TotalRequests" gibi şeyleri önem verdiğiniz. Mantıksal ve fiziksel ölçümleri aynı kümede kullanılabilir. Ölçümleri birçok hizmette paylaşılabilir veya belirli bir hizmete özgüdür.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
En azından aynı kişiler takan farklı hats olan veya sahipleri ve operatörleri kümenin hizmet ve uygulama yazarların farklı olabilir. Uygulamanızı geliştirirken, bunu hakkında birkaç şeyi biliyor. Bunu kullanacaktır kaynakları tahmin sahip ve farklı Hizmetleri dağıtılmalıdır. Örneğin, web katmanı ve veritabanı hizmetleri barındırmamalıdır sırada Internet'e, açık düğümleri üzerinde çalışmak gerekir. Başka bir örnek olarak, web Hizmetleri büyük olasılıkla CPU ve ağ, bellek ve disk tüketimi hakkında daha fazla veri katmanı Hizmetleri bakım sırasında tarafından kısıtlanmıştır. Ancak, üretim veya hizmet yükseltme yöneten hizmet için bir canlı site olayı işleme kişiye yapmak için başka bir işe sahip ve farklı araçlar gerektirir. 

Kümeyi ve Hizmetleri dinamik şunlardır:

- Kümedeki düğüm sayısını büyütme ve küçültme
- Düğümler farklı boyutlar ve türlerinin gelir ve Git
- Hizmetleri kaldırıldıysa, oluşturulabilir ve onların istenen kaynak ayırma ve yerleştirme kuralları değiştirme
- Yükseltme ya da diğer yönetim işlemlerini uygulama kümesi aracılığıyla altyapı düzeylerinde dönebilirsiniz
- Hataları, herhangi bir zamanda gerçekleşebilir.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Küme Kaynak Yöneticisi bileşenlerini ve veri akışı
Küme Kaynak Yöneticisi, bu hizmetlerin içindeki her bir hizmet nesnesi tarafından her hizmet gereksinimlerini ve kaynakların tüketimini izlemek vardır. Küme Kaynak Yöneticisi iki kavramsal bölümden oluşur: her düğüm ve hataya dayanıklı bir hizmet çalışan aracıları. Her düğüm izleme yük aracılarda hizmetlerinden toplama bunları raporlar ve bunları düzenli aralıklarla rapor. Küme Kaynak Yöneticisi hizmeti, yerel ve aracıları, geçerli yapılandırmanız temelinde tepki verdiğini gelen tüm bilgileri toplar.

Aşağıdaki diyagramda göz atalım:

<center>

![Kaynak dengeleyici mimarisi][Image1]
</center>

Çalışma zamanı sırasında gerçekleşebilir birçok değişiklik yoktur. Örneğin, şimdi deyin bazı servislerini kaynakların miktarını değiştirir, bazı hizmetler başarısız olur ve bazı düğümler ekleme ve küme. Bir düğümdeki tüm değişiklikleri toplanır ve düzenli aralıklarla nerede bunlar yeniden toplanır, analiz ve depolanan Küme Kaynak Yöneticisi hizmetine (1,2) gönderilir. Hizmet değişikliklerini arar ve herhangi bir eylem gerekli olup olmadığını belirler. her birkaç saniyede (3). Örneğin, bazı boş düğüm kümesine eklediğiniz fark. Sonuç olarak, bazı hizmetler için bu düğümleri geçmeye karar. Küme Kaynak Yöneticisi ayrıca belirli bir düğüm aşırı yüklendi veya belirli hizmetleri başarısız veya silinmiş, fark başka bir yerde kaynakları serbest bırakma.

Şimdi Aşağıdaki diyagramda arayın ve sonra ne olacağına bakalım. Küme Kaynak Yöneticisi değişikliklerin gerekli olduğunu belirler varsayalım. Bunu diğer sistem hizmetlerini (içinde belirli Yük Devretme Yöneticisi) ile gerekli değişiklikleri yapmak için düzenler. Ardından gerekli komutları uygun düğümlere (4) gönderilir. Örneğin, Resource Manager Düğüm5 fazlaydı ve bu nedenle hizmet B Düğüm5 ' Düğüm4 için taşıma kararı fark varsayalım. ' % S'yapılandırması (5) sonunda küme şöyle görünür:

<center>

![Kaynak dengeleyici mimarisi][Image2]
</center>

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi kümesi tanımlamak için birçok seçenek vardır. Bunlar hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](./service-fabric-cluster-resource-manager-cluster-description.md)
- Küme Kaynak Yöneticisi'nin birincil görevlerini kümeye yeniden Dengeleme ve yerleştirme kuralları zorlama. Bu davranışların yapılandırma hakkında daha fazla bilgi için bkz. [Service Fabric kümenizi Dengeleme](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
