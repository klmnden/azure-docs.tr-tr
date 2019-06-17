---
title: Yüksek kullanılabilirlik - Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM yüksek kullanılabilirlik örneği ve temel konuları
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: barclayn
ms.openlocfilehash: 784a6821502a0e02a74d619047604ffe47df7e97
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60914184"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure ayrılmış HSM yüksek kullanılabilirlik

Azure ayrılmış HSM, Microsoft'un yüksek oranda kullanılabilir veri merkezleri tarafından underpinned. Bununla birlikte, yüksek oranda kullanılabilir herhangi bir veri merkezine yerelleştirilmiş hataları ve olağanüstü durumlarda, bölgesel düzeyindeki hatalar savunmasız durumdadır. Microsoft, HSM cihazlarına farklı veri merkezlerindeki birden çok cihaz sağlama tek bir rafa paylaşımı aygıtları götürmez emin olmak için bir bölge içinde dağıtır. Bu HSM'ler Gemalto yüksek kullanılabilirlik grubu özelliğini kullanarak bir bölgedeki veri merkezleri arasında eşleştirme yaparak daha yüksek bir kullanılabilirlik düzeyi sağlanabilir. Ayrıca bir olağanüstü durum kurtarma durumda bölgesel yük devretme adres bölgeler arasında çifti cihazlara mümkündür. Bu çok katmanlı bir yüksek kullanılabilirlik yapılandırmasıyla, herhangi bir cihaz hata otomatik olarak çalışan uygulamaları korumak için getirilecektir. Başarısız olan herhangi bir CİHAZDAN vakitli şekilde değiştirilebilmesi tüm veri merkezlerinde de yedek aygıtlarının ve bileşenlerinin tesise var.

## <a name="high-availability-example"></a>Yüksek kullanılabilirlik örneği

Yüksek kullanılabilirlik için HSM cihazlarına yazılım düzeyinde yapılandırma konusunda bilgi 'Gemalto Luna ağ HSM Yönetim Kılavuzu'nda ' dir. Bu belgede kullanılabilir [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/).

Aşağıdaki diyagramda yüksek oranda kullanılabilir bir mimari gösterilmektedir. Birden çok bölgede hem de ayrı bir bölgede eşleştirilmiş birden çok cihazlarda kullanır. Bu mimari, en az dört HSM cihazlarına ve sanal ağ bileşenleri kullanır.

![Yüksek kullanılabilirlik diyagramı](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmete tüm temel kavramlarını cihaz sağlama ve uygulama tasarım veya dağıtımınızın önce iyi anlaşıldığından önerilir.
Daha fazla kavramı düzey konular:

* [Dağıtım mimarisi](deployment-architecture.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)

HSM cihazlarına yüksek kullanılabilirlik için yapılandırma belirli Ayrıntılar için lütfen Yönetici kılavuzları için Gemalto müşteri desteği Portalı'na bakın ve 6 bölümüne bakın.
