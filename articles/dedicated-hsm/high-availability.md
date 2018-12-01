---
title: Azure ayrılmış HSM yüksek kullanılabilirlik | Microsoft Docs
description: Azure ayrılmış HSM anahtar depolama kapasitesini FIPS karşılayan Azure 140-2 Düzey 3 sertifika sağlar.
services: dedicated-hsm
author: barclayn
manager: mbaldwin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: barclayn
ms.openlocfilehash: b02e3da086a3f33a55650fa72661f65028bfb78b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680030"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure ayrılmış HSM yüksek kullanılabilirlik

Azure ayrılmış HSM, Microsoft'un yüksek oranda kullanılabilir veri merkezleri tarafından underpinned. Bununla birlikte, yüksek oranda kullanılabilir herhangi bir veri merkezine yerelleştirilmiş hataları ve olağanüstü durumlarda, bölgesel düzeyindeki hatalar savunmasız durumdadır. Microsoft, HSM cihazlarına farklı veri merkezlerindeki birden çok cihaz sağlama tek bir rafa paylaşımı aygıtları götürmez emin olmak için bir bölge içinde dağıtır. Bu HSM'ler bir bölgedeki veri merkezleri arasında eşleştirme yaparak daha yüksek bir kullanılabilirlik düzeyi sağlanabilir. Ayrıca bir olağanüstü durum kurtarma durumda bölgesel yük devretme adres bölgeler arasında çifti cihazlara mümkündür. Bu çok katmanlı bir yüksek kullanılabilirlik yapılandırmasıyla, herhangi bir cihaz hata otomatik olarak çalışan uygulamaları korumak için getirilecektir. Başarısız olan herhangi bir CİHAZDAN vakitli şekilde değiştirilebilmesi tüm veri merkezlerinde de yedek aygıtlarının ve bileşenlerinin tesise var.

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
