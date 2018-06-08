---
title: Giriş için veri bilimi sanal makine tabanlı ekip ortamları - Azure | Microsoft Docs
description: Veri bilimi VM Kurumsal takımlar ortamı olarak dağıtmak için desenleri.
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
ms.openlocfilehash: d2aa3c8582227363e9365f213cdf351b9f4a81af
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830546"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Takım analizi ve AI ortamı veri bilimi sanal makine tabanlı 
[Veri bilimi sanal makine](overview.md) (DSVM) zengin bir ortam AI ve veri analizi için önceden derlenmiş yazılım olan Azure buluta sağlar. Geleneksel olarak DSVM tek tek analytics Masaüstü olarak kullanılan ve tek tek veri bilimcilerine önceden derlenmiş analytics ortamlarına paylaşılan bu kavramı verimliliğini sağlamak. Kurumsal BT uygun olarak yönetilen bir paylaşılan analytics geliştirme ve deneme altyapısı için yinelenen temalardan birini olan büyük analytics takımlar analytics ortamlarını kendi veri bilimcileri ve AI geliştiricileri için planlama yaparken, ilkeleri Bu da işbirliği ve tutarlılık tüm veri bilimi kolaylaştırır / analytics ekipleri. Ayrıca, paylaşılan bir altyapı sağlar analytics ortamı daha iyi kullanmalarını sağlar. Ekip tabanlı veri bilimi / analytics altyapı bazı kuruluşlar tarafından da bir "Analytics Sandbox" adlandırılır hızlı bir şekilde verileri anlamak, denemeleri çalıştırmak, varsayımınızın doğrulamak, Tahmine dayalı modelleri güvenli bir şekilde oluşturmak veri bilimcilerine sağlar çeşitli veri varlıklarına erişimi yaparken üretim ortamına etkileyen olmadan. 

Azure altyapı düzeyinde DSVM çalışır olduğundan, BT yöneticileri erişimi olan çeşitli paylaşım mimarileri uygulama enterprise ve teklifleri tam esneklik BT ilkelerine uyumlu çalışmaya DSVM taşımalarına yapılandırabilirsiniz Kurumsal veri varlıklarını denetimli bir şekilde. 

Bu bölüm, bazı desenleri ve DSVM ekip tabanlı veri bilimi altyapı olarak dağıtmak için kullanılan kuralları açıklar.  Bu düzenleri için yapı taşları doğrudan Azure Iaas (hizmet olarak altyapı) ve bu nedenle yararlanılabilir herhangi bir Azure VM için geçerlidir. Bu standart Azure altyapı özellikleri veri bilimi VM uygulama makaleleri bu dizi belirli odak noktasıdır. 

Anahtar kuruluş takım analytics ortamında yapı taşlarını bazıları şunlardır:

* [Otomatik ölçeklendirilmiş havuzu veri bilimi sanal makineler](dsvm-pools.md)
* [Ortak kimlik ve havuzdaki DSVMs birinden çalışma alanına erişim](dsvm-common-identity.md)
* [Veri kaynaklarına güvenli erişim](dsvm-secure-access-keys.md)


Bu makaleler dizide yönerge ve işaretçileri her birinde yönleri üzerinde sağlanmaktadır. Belli ki, birkaç ek hususlar ve vardır gereksinimlerini DSVM henüz doğrudan bu makaleler dizide kapsamında olmaması büyük kuruluş yapılandırmalarında dağıtırken. Diğer önemli noktalar ve kuruluşunuzdaki DSVM örneklerinde uygulama çalışırken taşımalarına kullanılabilir genel Azure belgelerine işaretçiler bazıları aşağıda verilmiştir. 

* [Ağ güvenliği](https://docs.microsoft.com/azure/security/azure-network-security)
* [İzleme](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) ve [Yönetimi](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Günlüğe kaydetme ve denetleme](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [İlke ayarı ve zorlama](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)
* [Kötü amaçlı yazılım](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Veri bulma ve idare](https://docs.microsoft.com/azure/data-catalog/)

[Azure Mimari Merkezi](https://docs.microsoft.com/en-us/azure/architecture/) ayrıca ayrıntılı uçtan uca mimarisi ve desenler oluşturma ve bulut tabanlı analytics altyapınızı yönetme sağlar harika bir kaynak değildir. 
