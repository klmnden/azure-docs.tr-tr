---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma - Azure Batch | Microsoft Docs
description: Bölgesel bir kesinti için toplu işlem uygulamanızı tasarlamayı öğrenin
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2019
ms.author: lahugh
ms.openlocfilehash: b863785575263fedd144b3d599962a8e1559e0a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549761"
---
# <a name="design-your-application-for-high-availability"></a>Uygulamanızı yüksek kullanılabilirlik için tasarlama

Azure Batch bölgesel bir hizmettir. Batch, tüm Azure bölgelerinde kullanıma sunulmuştur, ancak bir Batch hesabı oluşturulurken bir bölgeyle ilişkili olmalıdır. Batch hesabı için tüm işlemleri daha sonra bu bölgeye uygulayın. Örneğin, havuzları ve ilişkili sanal makinelerin (VM'ler) Batch hesabıyla aynı bölgede oluşturulur.

Batch kullanan bir uygulama tasarlarken, bir bölgede kullanılabilir olmaması Batch olasılığını dikkate almanız gerekir. Nadir bir durumla karşılaşmanız mümkündür toplu işin tamamını bir bütün olarak bölge ile ilgili bir sorun olduğunda, hizmet bölge veya belirli Batch hesabınızda bir sorun.

Her zaman uygulama veya çözümü kullanarak toplu kullanılabilir olması gerekir ve ardından başka bir bölgeye yük devretme tasarlanmalıdır veya her zaman en az iki bölgeleri arasında bölmek iş yüküne sahiptir. Her iki yaklaşım, farklı bir bölgede yer alan her hesapla en az iki Batch hesabı gerektirir.

## <a name="multiple-batch-accounts-in-multiple-regions"></a>Birden çok bölgede birden fazla Batch hesapları

Çeşitli bölgelerdeki birden fazla Batch hesabı kullanarak, uygulamanızın bir Batch hesabında başka bir bölge kullanılamaz duruma gelirse çalışmaya devam etmesini sağlar. Birden çok hesap kullanarak, uygulamanızın yüksek oranda kullanılabilir olması gerekiyorsa, özellikle önemlidir.

Bazı durumlarda, uygulama her zaman en az iki bölgeleri kullanmak için tasarlanmış olabilir. Önemli miktarda kapasiteye ihtiyaç duyduğunuzda, örneğin, birden çok bölgede kullanarak büyük ölçekli bir uygulamanın veya gelecekteki büyümeye gereksinimini karşılamak için gerekli olabilir.

## <a name="design-considerations-for-providing-failover"></a>Yük devretme sağlamak için tasarım konuları

Bir çözümdeki tüm bileşenlerin düşünülmesi gereken başka bir bölgeye yük devretme olanağı sağlarken dikkate alınması gereken önemli bir nokta olan; yalnızca bir ikinci Batch hesabınız için yeterli değil. Örneğin, çoğu Batch uygulamalarda, bir Azure depolama hesabı, kabul edilebilir performans için aynı bölgede olmasını gerektiren bir Batch hesabı ve depolama hesabı gereklidir.

Yük devretme için bir çözüm tasarlarken aşağıdaki noktaları göz önünde bulundurun:

- Batch hesabı ve depolama hesabı gibi her bölgede tüm gerekli hesapları önceden oluşturun. Genellikle değil için oluşturulan hesapları sahip herhangi bir ücret yoktur, yalnızca depolanan veriler ya da hesap kullanılır.
- Çekirdekler Batch hesabı kullanarak gerekli sayıda ayırabilir için kotalar hesaplarında önceden ayarlandığından emin olun.
- Uygulama bir bölgede dağıtımını otomatik hale getirmek için şablonları ve/veya betikler kullanın.
- Uygulama ikili dosyalarını ve başvuru verilerini tüm bölgelerde güncel kalmasını sağlayın. Güncel kalma bölge çevrimiçi hızlı bir şekilde karşıya yükleme ve dağıtım dosyalarının için beklemek zorunda kalmadan sağlanabildiğinden emin olursunuz. Örneğin, havuz düğümlerine yüklemek için özel bir uygulama ise depolanır ve Batch uygulama paketleri kullanılarak başvurulan sonra uygulamanın yeni bir sürümü üretildiğinde her Batch hesabına yüklediniz ve havuz yapılandırması tarafından başvurulan (veya Yeni sürüm varsayılan sürüm olun).
- Uygulamayı farklı bir bölgeye çağrılırken, toplu işlem, depolama ve diğer hizmetleri, kolayca geçiş istemciler veya yük.
- Bir yük devretme başarılı olmak için en iyi yöntem genellikle normal bir işlemin parçası olarak başka bir bölgeye geçiş sağlamaktır. Örneğin, iki dağıtımı ile farklı bölgelerdeki her ay alternatif bölgeye geçiş.

## <a name="next-steps"></a>Sonraki adımlar

- Batch hesapları ile oluşturma hakkında daha fazla bilgi [Azure portalında](batch-account-create-portal.md), [Azure CLI](cli-samples.md), [Powershell](batch-powershell-cmdlets-get-started.md), veya [toplu yönetim API'si](batch-management-dotnet.md).
- Varsayılan kotaları, Batch hesabıyla ilişkilidir; [bu makalede](batch-quota-limit.md) ayrıntıları varsayılan kota değerlerini ve kotalar artırılabilir nasıl açıklar.
