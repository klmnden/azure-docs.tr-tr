---
title: Veri bilimi sanal makine tabanlı ekip ortamları - Azure giriş | Microsoft Docs
description: Bir kuruluş ekip ortamında veri bilimi VM dağıtmak için desenleri.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analytics, takım veri bilimi işlemi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 8486b0be1fb5e1385da3c7ad55f6410a1059df93
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309257"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Veri bilimi sanal makine tabanlı ekip analizi ve AI ortamı 
[Veri bilimi sanal makine](overview.md) (DSVM) yapay Intelligence (AI) ve veri analizi için önceden oluşturulmuş yazılımıyla Azure platformu zengin bir ortam sağlar. 

Geleneksel olarak, DSVM tek tek analytics Masaüstü olarak kullanıldı. Tek tek veri bilimcilerine önceden oluşturulmuş analytics ortamlarına paylaşılan bu kavramı verimliliğini kazanır. Yinelenen temalardan birini bir büyük analytics takımlar analytics ortamlarını kendi veri bilimcileri ve AI geliştiricileri için planlama yaparken, geliştirme ve deneme için paylaşılan analytics altyapısıdır. Bu altyapı kurumsal BT uygun olarak yönetilen de veri bilimi/analytics takımlar arasında işbirliğini ve tutarlılık kolaylaştırmak ilkeleri. 

Ayrıca, paylaşılan bir altyapı sağlar analytics ortamı daha iyi kullanmalarını sağlar. Bazı kuruluşlar, bir "analytics sandbox." ekip tabanlı veri bilimi/analytics altyapı çağırın Hızlı bir şekilde verileri anlamak, denemeleri çalıştırmak, varsayımlar doğrulamak ve üretim ortamına etkilemeden Tahmine dayalı modelleri oluşturmak için çeşitli veri varlıklarına erişmek veri bilimcilerine sağlar. 

DSVM Azure altyapı düzeyinde çalıştığından, BT yöneticileri kurumsal BT ilkeleriyle uyumlu çalışmaya DSVM kolayca yapılandırabilirsiniz. DSVM kurumsal veri varlıklarına erişimi olan çeşitli paylaşım mimarileri denetimli bir şekilde uygulama tam esneklik sunar. 

Bu bölümde bazı desenleri ve DSVM ekip tabanlı veri bilimi altyapı olarak dağıtmak için kullanabileceğiniz yönergeler açıklanmaktadır. Herhangi bir Azure VM uygulamak için bu düzenleri için yapı taşları Azure altyapısından (Iaas), hizmet olarak gelir. Bu standart Azure altyapı özellikleri veri bilimi VM uygulama makaleleri bu dizi odak noktasıdır. 

Anahtar kuruluş takım analytics ortamında yapı taşlarını bazıları şunlardır:

* [Autoscaled havuzu veri bilimi sanal makineler](dsvm-pools.md)
* [Ortak kimlik ve havuzdaki DSVMs herhangi bir çalışma alanına erişim](dsvm-common-identity.md)
* [Veri kaynaklarına güvenli erişim](dsvm-secure-access-keys.md)


Bu makale dizisi her önceki öğeler için kılavuz ve işaretçileri sağlar. Tüm değerlendirmeleri ve büyük kuruluş yapılandırmalarında DSVM dağıtmanın gereksinimlerini kapsamaz. DSVM örnekleri, kuruluşunuzda uygulama çalışırken kullanabileceğiniz diğer Azure belgelerine şöyledir: 

* [Ağ güvenliği](https://docs.microsoft.com/azure/security/azure-network-security)
* [İzleme](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) ve [Yönetimi](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Günlük kaydı ve denetim](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [İlke ayarı ve zorlama](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Kötü Amaçlı Yazılımdan Koruma](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Veri bulma ve idare](https://docs.microsoft.com/azure/data-catalog/)

[Azure Mimari Merkezi](https://docs.microsoft.com/en-us/azure/architecture/) düzenleri oluşturma ve bulut tabanlı analytics altyapınızı yönetme ve ayrıntılı bir uçtan uca mimarisi sağlar. 
