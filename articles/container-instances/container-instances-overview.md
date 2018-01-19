---
title: "Azure Container Instances’a genel bakış"
description: "Azure Container Instances’ı Anlama"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 01e539856adbdcf02dc4e49087a3ab71b328db5a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="azure-container-instances"></a>Azure Container Instances

Kapsayıcılar, bulut uygulamalarını hızlı bir şekilde paketlemek, dağıtmak ve yönetmek için giderek daha fazla tercih edilmektedir. Azure Container Instances, herhangi bir sanal makine sağlamak ve daha yüksek düzeyde bir hizmeti benimsemek zorunda kalmadan Azure içinde kapsayıcı çalıştırmanın en hızlı ve en kolay yolunu sunar.

Yalıtılmış kapsayıcılarda çalışabileceğiniz senaryolar (basit uygulamalar, görev otomasyonu ve sürüm işleri gibi) için Azure Container Instances harika bir çözümdür. Eksiksiz bir kapsayıcı düzenlemesi gerektiren senaryolar (birden çok kapsayıcıda hizmet bulma, otomatik ölçeklendirme ve eşgüdümlü uygulama yükseltmeleri gibi) için [Azure Container Service (AKS)](../aks/index.yml) hizmetini öneririz.

## <a name="fast-startup-times"></a>Hızlı başlangıç süreleri

Kapsayıcılar, sanal makinelerde önemli başlangıç süresi avantajları sunar. Azure Container Instances sayesinde Azure’da, sanal makineleri sağlamaya ve yönetmeye gerek kalmadan saniyeler içinde kapsayıcı başlatabilirsiniz.

## <a name="hypervisor-level-security"></a>Hiper yönetici düzeyinde güvenlik

Geçmişte kapsayıcılar, uygulama bağımlılığı yalıtımı ve kaynak idaresi olanakları sağlıyor ancak birden çok kiracılı zorlu kullanımlar için yeterli kabul edilmiyordu. Azure Container Instances ile uygulamanız,sanal makine yerine kapsayıcıda yalıtılır.

## <a name="custom-sizes"></a>Özel boyutlar

Kapsayıcılar genellikle tek bir uygulamayı çalıştırmak üzere iyileştirilmiştir, ancak söz konusu uygulamaların tam gereksinimleri önemli ölçüde farklı olabilir. Azure Container Instances kullandığınızda, CPU çekirdekleri ve bellek açısından tam olarak gerek duyduğunuz kadar kaynak isteyebilirsiniz. Yalnızca istedikleriniz için, saniye başına faturalandırılır ve harcamalarınızı ihtiyaçlarınıza uyacak şekilde ayarlayabilirsiniz.

## <a name="public-ip-connectivity"></a>Genel IP bağlantısı

Azure Container Instances ile kapsayıcılarınızı İnternet üzerinde genel IP adresi ile doğrudan kullanıma sunabilirsiniz. Gelecekte ağ olanaklarımızı, sanal ağlar ile tümleştirmeyi, yük dengeleyicileri ve Azure ağ altyapısının diğer çekirdek bölümlerini içerecek şekilde genişleteceğiz.

## <a name="persistent-storage"></a>Kalıcı depolama

Azure Container Instances ile durum alma ve durumu kalıcı hale getirme işlemleri için, [Azure Dosyaları paylaşımlarını doğrudan bağlama](container-instances-mounting-azure-files-volume.md) olanağı sunuyoruz.

## <a name="linux-and-windows-containers"></a>Linux ve Windows kapsayıcıları

Azure Container Instances sayesinde, aynı API ile hem Windows hem de Linux kapsayıcıları zamanlayabilirsiniz. [Kapsayıcı gruplarınızı](container-instances-container-groups.md) oluştururken işletim sistemi türünü belirmeniz yeterlidir.

Bazı özellikler şu anda Linux kapsayıcılarıyla kısıtlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="co-scheduled-groups"></a>Birlikte zamanlanmış gruplar

Azure Container Instances, aynı ana makineyi, yerel ağı, depolama alanını ve yaşam döngüsünü paylaşan [birden çok kapsayıcı grubunun](container-instances-container-groups.md) zamanlanmasını destekler. Böylece ana uygulamanızı, günlüğe kaydetme gibi diğer destekleyici uygulamalarla birleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç kılavuzumuzu](container-instances-quickstart.md) kullanarak bir kapsayıcıyı tek bir komutla Azure’a dağıtmayı deneyin.