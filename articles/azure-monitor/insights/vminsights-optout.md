---
title: Azure İzleyici'de (Önizleme) sanal makineler için izlemeyi devre dışı | Microsoft Docs
description: Bu makalede, sanal makinelerinizi Azure İzleyici'de sanal makineler için izlemeyi durdurmak açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: eb667486a6e3279cb78fefe02723f14d9f7c9b4f
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155696"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms-preview"></a>Azure İzleyici'de sanal makinelerinizin (Önizleme) sanal makineler için izlemeyi devre dışı bırak

Sanal makinelerin (VM'ler) izleme etkinleştirdikten sonra daha sonra Azure İzleyici'de sanal makineler için izlemeyi devre dışı bırakmak seçebilirsiniz. Bu makalede, bir veya daha fazla sanal makineler için izlemeyi devre dışı bırakmak gösterilmektedir.  

Şu anda, VM'ler için Azure İzleyici seçmeli VM izlemeyi devre dışı bırakma desteklemiyor. Log Analytics çalışma alanınızın, VM'ler ve diğer çözümler için Azure İzleyici destekleyebilir. Ayrıca, diğer izleme verilerinin de toplayabilir. Log Analytics çalışma alanınızda bu hizmetleri sağlıyorsa, etkili ve başlamadan önce izleme devre dışı bırakma yöntemlerini anlamanız gerekir.

VM'ler için Azure İzleyici, bir deneyim sunmak için aşağıdaki bileşenleri kullanır:

* VM'ler ve diğer kaynakları izleme verilerini saklayan bir Log Analytics çalışma.
* Performans sayaçları çalışma alanında yapılandırılmış koleksiyonudur. Koleksiyonu çalışma alanınıza bağlı tüm sanal makineler İzleme yapılandırmasını güncelleştirir.
* `InfrastructureInsights` ve `ServiceMap`, çözümler çalışma alanında yapılandırılmış izleme. Bu çözümlerin çalışma alanınıza bağlı tüm sanal makineler İzleme yapılandırmasını güncelleştirin.
* `MicrosoftMonitoringAgent` ve `DependencyAgent`, Azure VM uzantıları olan. Bu uzantıları verilerini toplar ve çalışma alanına gönderir.

Sanal makineleriniz izlemeyi devre dışı bırakmak hazırlama, bu noktalar göz önünde bulundurun:

* Tek bir VM ile değerlendirilir ve önceden seçilmiş varsayılan Log Analytics çalışma alanı kullanılır, bağımlılık Aracısı VM'den kaldırılıyor ve Log Analytics aracısını bu çalışma alanından bağlantısı kesiliyor. izleme devre dışı bırakabilirsiniz. Sanal Makineyi başka bir amaçla kullanın ve daha sonra farklı bir çalışma alanına yeniden bağlanmak karar istiyorsanız, bu yaklaşım uygundur.
* Diğer izleme çözümleri ve diğer kaynaklardan veri toplamayı destekleyen önceden var olan bir Log Analytics çalışma alanı seçtiyseniz, çözüm bileşenlerini çalışma alanından kesilmesi veya çalışma alanınızı etkilemeden kaldırabilirsiniz.  

>[!NOTE]
> Çözüm bileşenleri çalışma alanınızdan kaldırdıktan sonra Azure sanal makinelerinize sistem durumunu görmek devam edebilir; özellikle performansını görmek ve veri eşleyin portalında ya da görünümüne gidin. Veri, görüntülenmesini sonunda durdurur **performans** ve **harita** görünümleri. Ancak **sistem durumu** görünümü Vm'leriniz için sistem durumunu gösteren devam eder. **Şimdi deneyin** , gelecekte izlemeyi yeniden etkinleştirebilir, böylece seçeneği seçili Azure VM'den kullanıma sunulacaktır.  

## <a name="remove-azure-monitor-for-vms-completely"></a>Azure İzleyicisi'ni VM'ler için tamamen kaldırın

Log Analytics çalışma alanı yine de gerekliyse, Azure İzleyici VM'ler için tamamen kaldırmak için aşağıdaki adımları izleyin. Kaldırırsınız `InfrastructureInsights` ve `ServiceMap` çözümleri çalışma alanından.  

>[!NOTE]
>Hizmet eşlemesi VM'ler için Azure İzleyici etkin ve bundan hala yararlanan önce izleme çözümünü kullandıysanız, bu çözüm aşağıdaki yordamın son adımda açıklandığı gibi kaldırmayın.  
>

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure portalda **Tüm hizmetler**’i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girdinize göre öneriler filtreler. **Log Analytics**’i seçin.
3. Log Analytics çalışma alanlarınızın listesinde çalışma alanını seçin, seçtiğiniz VM'ler için Azure İzleyici etkin olduğunda.
4. Sol tarafta, seçin **çözümleri**.  
5. Çözüm listesinde seçin **InfrastructureInsights (çalışma alanı adı)** . Üzerinde **genel bakış** çözümü seçin sayfasında **Sil**. Onaylamak için sorulduğunda **Evet**.  
6. Çözüm listesinde seçin **ServiceMap (çalışma alanı adı)** . Üzerinde **genel bakış** çözümü seçin sayfasında **Sil**. Onaylamak için sorulduğunda **Evet**.  

Aksi takdirde sanal makineler için Azure İzleyici etkinleştirilmeden önce [performans sayaçları toplamak](vminsights-enable-overview.md#performance-counters-enabled) çalışma alanınızda Windows veya Linux tabanlı VM'ler için [bu kuralları devre dışı](../platform/data-sources-performance-counters.md#configuring-performance-counters) Linux ve Windows için.

## <a name="disable-monitoring-and-keep-the-workspace"></a>İzleme devre dışı bırak ve çalışma tutun  

Log Analytics çalışma alanınızın oluşmaya devam VM'ler için Azure İzleyici değerlendirmek için kullanılan VM üzerinde izleme devre dışı bırakmak için aşağıdaki adımları izleyerek diğer kaynaklardan izleme desteklemesi gerekir. Azure Vm'leri için bağımlılık Aracısı VM uzantısını ve Log Analytics aracısını VM uzantısı Windows veya Linux için sanal makineden doğrudan kaldırmanız. 

>[!NOTE]
>Log Analytics aracısını, kaldırın: 
>
> * Azure Otomasyonu süreçlerini düzenleyin veya yapılandırma ya da güncelleştirmeleri yönetmek için VM yönetir. 
> * Azure Güvenlik Merkezi, güvenlik ve tehdit algılama için VM yönetir. 
>
> Log Analytics aracısını kaldırırsanız, sanal makinenizin proaktif bir şekilde yönetmesini bu hizmet ve çözümler engeller. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Azure portalında **sanal makineler**. 
3. Listeden bir VM seçin. 
4. Sol tarafta, seçin **uzantıları**. Üzerinde **uzantıları** sayfasında **DependencyAgent**.
5. Uzantı özellikleri sayfasında **kaldırma**.
6. Üzerinde **uzantıları** sayfasında **MicrosoftMonitoringAgent**. Uzantı özellikleri sayfasında **kaldırma**.  
