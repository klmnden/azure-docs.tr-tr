---
title: Azure İzleyici (Önizleme) sanal makineler için izlemeyi devre dışı bırakmayı öğrenin | Microsoft Docs
description: Bu makalede, sanal makinelerin, sanal makineler için Azure İzleyici ile izleme nasıl kesmek açıklanır.
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
ms.openlocfilehash: 0f35ea3e35277ee7f1afd8278a31f45ed20c6995
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65522130"
---
# <a name="how-to-disable-monitoring-of-your-virtual-machines-with-azure-monitor-for-vms-preview"></a>Sanal makinelerinizi (Önizleme) VM'ler için Azure İzleyici ile izleme devre dışı bırakma

Sanal makinelerinizi izleme etkinleştirdikten sonra bunları Azure İzleyici ile VM'ler için izlemek istediğiniz karar verirseniz, izleme devre dışı bırakabilirsiniz. Bu makalede, tek veya birden çok sanal makine için bunu gösterilmektedir.  

Şu anda Azure İzleyici VM'ler için seçime bağlı olarak, sanal makinelerin izleme devre dışı bırakma desteklemez. Log Analytics çalışma alanınızı yanı sıra bu çözüm ve diğer çözümleri destekler, diğer izleme verilerini toplamak için yapılandırılmışsa, devam etmeden önce aşağıda açıklanan yöntemlerden ve etkisini anlamak önemlidir.

VM'ler için Azure İzleyici, bir deneyim sunmak için aşağıdaki bileşenleri kullanır:

* İzleme verilerini saklayan bir Log Analytics çalışma alanında, VM'ler ve diğer kaynaklardan toplanan.
* Tüm sanal makineler izleme yapılandırması güncelleştirmeleri çalışma alanında yapılandırılmış performans sayaçlarını toplamayı çalışma alanına bağlı.
* Çalışma alanında - yapılandırılmış iki izleme çözümleri **InfrastructureInsights** ve **ServiceMap**, tüm sanal makineler için izleme yapılandırmasını güncelleştirme çalışma alanına bağlı.
* İki Azure sanal makine uzantıları, **MicrosoftMonitoringAgent** ve **DependencyAgent**, toplamak ve çalışma alanına veri göndermek.

Aşağıdaki sanal makinelerinizi VM'ler için Azure İzleyici ile izleme devre dışı bırakmak hazırlık yaparken göz önünde bulundurun:

* Tek bir VM ile değerlendirdiğiniz ve önceden seçilmiş varsayılan Log Analytics çalışma alanını kabul ettiyseniz, bağımlılık Aracısı VM'den kaldırılıyor ve Log Analytics aracısını bu çalışma alanından bağlantısı kesiliyor. izleme devre dışı bırakabilirsiniz. Diğer amaçlar için VM kullanarak düşünüyorsanız ve farklı bir çalışma alanına yeniden bağlanmak daha sonra karar verirseniz, bu yaklaşım uygundur.
* Diğer izleme çözümleri ve diğer kaynaklardan veri koleksiyonunu desteklemek için Log Analytics çalışma alanı kullanıyorsanız, çalışma alanınıza kesintisi veya etkisi olmadan çalışma alanından Vm'leri çözüm bileşenlerini Azure İzleyici kaldırabilirsiniz.  

>[!NOTE]
> Çözüm bileşenleri çalışma alanınızdan kaldırdıktan sonra Azure sanal makinelerinize sistem durumunu görmek devam edebilirsiniz; portalında ya da görünümüne gidin, özellikle performans ve harita verileri. Veri, performans ve harita görünümü süre sonra görünen sonunda durur; ancak durum görünümün Vm'leriniz için sistem durumu göstermeye devam eder. **Şimdi deneyin** seçeneği seçili Azure gelecekte izleme işlemini yeniden etkinleştirebilirsiniz olanak tanımak için VM'den kullanıma sunulacaktır.  

## <a name="complete-removal-of-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici kaldırılmasını tamamlamak

Aşağıdaki adımlar, Log Analytics çalışma alanı yine de gerekliyse VM'ler için Azure İzleyici tamamen kaldırmak açıklanmaktadır. Kaldırmak için oluşturacağınız **InfrastructureInsights** ve **ServiceMap** çözümleri çalışma alanından.  

>[!NOTE]
>Bu çözüm izleme çözümü VM'ler için Azure İzleyicisi'ni etkinleştirmek önce hizmet eşlemesi kullandığınız ve sunucuyu hala kullanan, aşağıda 6. adımda açıklandığı gibi kaldırmayın.  
>

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Log Analytics kaynak listesinde yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Log Analytics çalışma alanlarınızın listesinde, seçtiğiniz zaman çalışma alanını seçin ekleme VM'ler için Azure İzleyici.
4. Sol bölmeden **çözümleri**.  
5. Çözüm listesinde seçin **InfrastructureInsights (çalışma alanı adı)** ve ardından **genel bakış** sayfasında çözüm için **Sil**.  Onaylamak için sorulduğunda **Evet**.  
6. Çözümleri listesinden seçin **ServiceMap (çalışma alanı adı)** ve ardından **genel bakış** sayfasında çözüm **Sil**.  Onaylamak için sorulduğunda **Evet**.  

VM'ler için Azure İzleyici ekleme önce yokmuş [etkin performans sayaçlarını toplamayı](vminsights-enable-overview.md#performance-counters-enabled) açıklanan adımlarıizleyerekbukurallarıdevredışıbırakmakgereksinimWindowsveyaLinuxtabanlıVM'lerçalışmaalanınızdakiiçin[burada](../platform/data-sources-performance-counters.md#configuring-performance-counters) Windows ve Linux için.

## <a name="disable-monitoring-for-an-azure-vm-and-retain-workspace"></a>Bir Azure sanal makinesi için izlemeyi devre dışı bırak ve çalışma alanı koru  

Aşağıdaki adımlar, VM'ler için Azure İzleyici değerlendirmek için etkin bir sanal makine için izlemeyi devre dışı bırakmak açıklamaktadır, ancak Log Analytics çalışma alanını başka kaynaklardan izlemeyi desteklemek için hala gereklidir. Bir Azure VM varsa, Log Analytics aracısını VM uzantısı ve bağımlılık aracısını VM uzantısı, Windows/Linux için doğrudan sanal makineden kaldırın oluşturacaksınız. 

>[!NOTE]
>Sanal makine tarafından yönetiliyorsa işlemlerini düzenlemek için Azure Otomasyonu yapılandırma, yönetme veya güncelleştirmelerini yönetme veya Azure Güvenlik Merkezi tarafından yönetilen güvenlik yönetimi ve tehdit algılama için Log Analytics aracısını kaldırılmayacak. Aksi takdirde, bu hizmet ve çözümler, sanal Makinenizin proaktif olarak yönetme engeller. 

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 
2. Azure portalında **sanal makineler**. 
3. Listeden bir VM seçin. 
4. Sol bölmeden **uzantıları** ve **uzantıları** sayfasında **DependencyAgent**.
5. Uzantı özellikleri sayfasında tıklayın **kaldırma**.
6. Üzerinde **uzantıları** sayfasında **MicrosoftMonitoringAgent** uzantı özellikleri sayfasında tıklatıp **kaldırma**.  
