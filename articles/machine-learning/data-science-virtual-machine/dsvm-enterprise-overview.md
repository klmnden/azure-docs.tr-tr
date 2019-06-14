---
title: Veri bilimi sanal makinesi tabanlı takım ortamlarına - Azure giriş | Microsoft Docs
description: Veri bilimi sanal Makinesi'ne kurumsal bir takım ortamı dağıtmak için desenler.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, team data science Process'i
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 2e17ab5cfe51f3772148cc730c982671d602a79a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502156"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Veri bilimi sanal makinesi tabanlı takım analizi ve yapay ZEKA ortamı 
[Veri bilimi sanal makinesi](overview.md) (DSVM), Azure platformunda yapay zeka (AI) ve veri analizi için önceden oluşturulmuş yazılım ile zengin bir ortam sağlar. 

Geleneksel olarak, bir tek analize yönelik Masaüstü DSVM kullanıldı. Tek tek veri bilimcileri, önceden oluşturulmuş analiz ortamını paylaşılan bu kavramı ile üretkenliği elde edin. Yinelenen temalardan birini bir büyük analytics takımlar, veri bilimcileri ve yapay ZEKA geliştiricilerine için analiz ortamlarını planlarken, geliştirme ve deneme için paylaşılan analiz altyapısıdır. Bu altyapı, kurumsal BT ayarlarına uygun olarak yönetilen ayrıca veri bilimi/analiz takımlar arasında işbirliğini ve tutarlılık kolaylaştırmak, ilkeleri. 

Paylaşılan altyapı da sağlayan BT analiz ortamını daha iyi kullanmak için. Bazı kuruluşlar, ekip tabanlı veri bilimi/analiz altyapısı bir "analiz sandbox." çağırın. Ancak, veri uzmanları, hızlı bir şekilde verileri anlamak, denemeleri çalıştırmak, sınamakta doğrulamak ve üretim ortamına etkilemeden Tahmine dayalı modeller oluşturmak için çeşitli veri varlıklarına erişmek etkinleştirir. 

DSVM Azure altyapı düzeyinde çalıştığından, BT yöneticileri kurumsal BT ilkelerle çalışmaya DSVM kolayca yapılandırabilirsiniz. DSVM kurumsal veri varlıklarına erişimi olan çeşitli paylaşım mimarileri denetimli bir şekilde uygulama tam esneklik sunar. 

Bu bölümde, bazı desen ve bir ekip tabanlı veri bilimi altyapısı olarak DSVM dağıtmak için kullanabileceğiniz yönergeleri ele alınmaktadır. Bunlar herhangi bir Azure sanal makinelere uygulamak için bu desenleri için yapı taşları Azure altyapıdan (Iaas), hizmet olarak gelir. Bu makale serisi, odak noktası, standart Azure altyapı yeteneklere veri bilimi sanal makinesi için uygulama üzerinde ' dir. 

Bir kurumsal takım analiz ortamını temel yapı taşları bazıları şunlardır:

* [Veri bilimi sanal makineleri Autoscaled havuzu](dsvm-pools.md)
* [Ortak kimlik ve havuzdaki Dsvm'leri birinden bir çalışma alanına erişim](dsvm-common-identity.md)
* [Veri kaynaklarına güvenli erişim](dsvm-secure-access-keys.md)


Bu makale serisi, rehberlik ve işaretçileri her önceki öğeler için sağlar. Tüm önemli noktalar ve büyük kuruluş yapılandırmalarında DSVM dağıtmanın gereksinimlerini ele alınmamıştır. DSVM örnekleri, kuruluşunuzda uygulama çalışırken kullanabileceğiniz diğer Azure belgeleri aşağıda verilmiştir: 

* [Ağ güvenliği](https://docs.microsoft.com/azure/security/azure-network-security)
* [İzleme](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) ve [Yönetimi](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Günlük kaydı ve denetim](https://docs.microsoft.com/azure/security/azure-log-audit)
* [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [İlke ayarı ve zorlama](../../governance/policy/overview.md)
* [Kötü Amaçlı Yazılımdan Koruma](https://docs.microsoft.com/azure/security/azure-security-antimalware)
* [Şifreleme](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Veri bulma ve idare](https://docs.microsoft.com/azure/data-catalog/)

[Azure Mimari Merkezi](https://docs.microsoft.com/azure/architecture/) desenleri oluşturmak ve bulut tabanlı analiz altyapınızı yönetmek için ve ayrıntılı bir uçtan uca mimarisi sağlar. 
