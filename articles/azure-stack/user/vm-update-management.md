---
title: VM güncelleştirme ve Azure Stack ile Yönetimi | Microsoft Docs
description: Windows ve Linux Azure Stack'te dağıtılan Vm'leri yönetmek için Vm'leri, güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri Azure automation'da Azure İzleyicisi'ni kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: jeffgilb
ms.reviewer: rtiberiu
ms.lastreviewed: 03/20/2019
ms.openlocfilehash: cb8258c0f837d0e70ba87a26246f055b0efe5c00
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316160"
---
# <a name="azure-stack-vm-update-and-management"></a>Azure Stack VM güncelleştirme ve yönetim
Aşağıdaki Azure Otomasyonu çözüm özellikleri, Windows ve Linux Vm'leri, Azure Stack kullanarak dağıtılmış yönetmek için kullanabilirsiniz:

- **[Güncelleştirme yönetimi](https://docs.microsoft.com/azure/automation/automation-update-management)**. Güncelleştirme yönetimi çözümü ile hızlı bir şekilde tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu değerlendirebilir ve bu Windows ve Linux Vm'leri için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

- **[Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking)**. Yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosya ve izlenen sunucularda Linux Daemon'ları için değişiklikler, işleme için bulutta Azure İzleyici'hizmetine gönderilir. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder. Değişiklik izleme Panoda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

- **[Stok](https://docs.microsoft.com/azure/automation/automation-vm-inventory)**. Bir Azure Stack sanal makine için izleme envanteri ayarlamak ve stok toplama yapılandırmak için bir tarayıcı tabanlı kullanıcı arabirimi sağlar.

- **[VM'ler için Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**. Azure İzleyici Vm'leri için Azure'da ve Azure Stack'te sanal makinelerinizi (VM) izler ve uygun ölçekte sanal makine ölçek kümeleri. Windows ve Linux Vm'leri sistem durumu ve performansı analiz eder ve kendi işlemlerini ve diğer kaynakları ve işlemleri dış bağımlılıkları izler. 

> [!IMPORTANT]
> Bu çözümler Azure Vm'leri yönetmek için kullanılan olanlarla aynıdır. Hem Azure hem de Azure Stack Vm'leri aynı şekilde, aynı arabiriminden aynı araçları kullanılarak yönetilir. Azure Stack Vm'leri de aynı Azure Vm'leri olarak Azure Stack ile güncelleştirme yönetimi, değişiklik izleme, stok ve Azure sanal makineleri izleme çözümleri kullanırken ücretlendirilir.

## <a name="prerequisites"></a>Önkoşullar
Bu özellikleri güncelleştirin ve Azure Stack Vm'leri yönetmek için kullanmadan önce birkaç önkoşulların karşılanması gerekir. Bu, Azure Stack Yönetim Portalı yanı sıra Azure portalı alınması gereken adımlar içerir.

### <a name="in-the-azure-portal"></a>Azure portalında
Azure Stack VM'ler için Azure İzleyici Vm'leri, Envanter, değişiklik izleme ve güncelleştirme yönetimi Azure Otomasyon özellikleri kullanmak için önce bu çözümleri azure'da etkinleştirmeniz gerekir.

> [!TIP]
> Bu özellikleri Azure Vm'leri için etkin zaten varsa, önceden mevcut olan LogAnalytics çalışma kimlik bilgilerinizi kullanabilirsiniz. LogAnalytics Workspaceıd ve kullanmak istediğiniz birincil anahtar zaten varsa, atlayın [sonraki bölümde](./vm-update-management.md#in-the-azure-stack-administration-portal). Aksi takdirde, yeni LogAnalytics çalışma alanını ve Otomasyon hesabı oluşturmak için bu bölümdeki devam edin.

Bu çözümler etkinleştirmenin ilk adımı [LogAnalytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) Azure aboneliğinizdeki. Bir Log Analytics çalışma alanı, kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Azure İzleyici günlükleri ortamıdır. Bir çalışma alanı oluşturduktan sonra çalışma alanı kimliği ve anahtarı not edin. Bu bilgileri görüntülemek için çalışma alanı dikey penceresine gidin, tıklayarak **Gelişmiş ayarlar**ve gözden geçirme **çalışma alanı kimliği** ve **birincil anahtar** değerleri. 

Ardından, şunları yapmalısınız [Otomasyon hesabı oluşturma](https://docs.microsoft.com/azure/automation/automation-create-standalone-account). Bir Otomasyon hesabı, Azure Automation kaynaklarınız için bir kapsayıcıdır. Ortamlarınızı ayırmak veya daha fazla Otomasyon iş akışları ve kaynakları düzenlemek için bir yol sunar. Otomasyon hesabı oluşturduktan sonra envanteri etkinleştirmek için değişiklik izleme ve yönetim özelliklerini güncelleştirin. Bunu yapmak için her özelliği etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında, kullanmak istediğiniz Otomasyon hesabınıza gidin.

2. Çözümü etkinleştirmek için seçin (ya da **Envanter**, **değişiklik izleme**, veya **güncelleştirme yönetimi**).

3. Kullanım **çalışma alanı seçin...**  açılır listede kullanmak için Log Analytics çalışma alanı seçin.

4. Kalan tüm bilgilerin doğru olduğunu doğrulayın ve ardından **etkinleştirme** çözümü etkinleştirmek için.

5. Tüm üç çözümlerle 2-4 arası adımları yineleyin. 

   [![](media/vm-update-management/1-sm.PNG "Otomasyon hesabı özellikleri etkinleştir")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="enable-azure-monitor-for-vms"></a>Sanal makineler için Azure İzleyicisi'ni etkinleştirme

Azure sanal makinelerinizi (VM) sanal makineler için Azure İzleyici izler ve uygun ölçekte sanal makine ölçek kümeleri. Windows ve Linux Vm'leri sistem durumu ve performansı analiz eder ve kendi işlemlerini ve diğer kaynakları ve işlemleri dış bağımlılıkları izler.

VM'ler için Azure İzleyici, bir çözüm olarak, başka bir bulut sağlayıcısında veya şirket içi VM'ler için performans ve uygulama bağımlılıklarını izlemek için destek içerir. Üç anahtar özellik kapsamlı Öngörüler sunun:

1. Windows ve Linux çalıştıran Azure VM'lerin mantıksal bileşenler: Önceden yapılandırılmış bir sistem durumu ölçütlerine göre ölçülür ve değerlendirilen koşul karşılandığında, sizi uyarır. 

2. Önceden tanımlanmış popüler performans grafiklerini: Konuk VM işletim sisteminden temel performans ölçümlerini görüntüleyin.

3. Bağımlılık Haritası: Çeşitli kaynak gruplarında ve Aboneliklerde VM'den birbirine bağlı bileşenlerle görüntüler.

Log Analytics çalışma alanı oluşturulduktan sonra çalışma alanı koleksiyonu Linux ve Windows Vm'leri için performans sayaçları etkinleştirme yanı sıra yüklemek ve çalışma alanınızdaki ServiceMap ve InfrastructureInsights çözümü etkinleştirmek gerekir. İşlem açıklanan [VM'ler için Azure İzleyici'ı Dağıtma](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#deploy-azure-monitor-for-vms) Kılavuzu.

### <a name="in-the-azure-stack-administration-portal"></a>Azure Stack Yönetim Portalı'nda
Azure portalında Azure Otomasyonu çözümleri etkinleştirdikten sonra sonraki bir bulutun Yöneticisi olarak Azure Stack yönetim portalında oturum açın ve indirmek için ihtiyaç duyduğunuz **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** ve **Azure İzleyici, güncelleştirme ve Linux için yapılandırma yönetimi** uzantısı Azure Stack Market öğesi. 

   ![Azure İzleyici, güncelleştirme ve yapılandırmasını yönetim uzantısı Market öğesi](media/vm-update-management/2.PNG) 

Vm'leri eşlemesi çözümü için Azure İzleyici etkinleştirmek ve ağ bağımlılıklarını Öngörüler elde etmek için de karşıdan gerekecektir **Azure İzleyici bağımlılık aracısını**:

   ![Azure İzleyici bağımlılık Aracısı](media/vm-update-management/2-dependency.PNG) 

## <a name="enable-update-management-for-azure-stack-virtual-machines"></a>Azure Stack sanal makineler için güncelleştirme yönetimini etkinleştirme
Azure Stack sanal makineler için güncelleştirme yönetimini etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure Stack kullanıcı portalda oturum açın.

2. Azure Stack Kullanıcı-Portalı'nda, bu çözümleri etkinleştirmek istediğiniz sanal makinelerin uzantıları dikey penceresine gidin **+ Ekle**seçin **Azure güncelleştirme ve yapılandırma yönetimi** Uzantı seçeneğine tıklayıp **Oluştur**:

   [![](media/vm-update-management/3-sm.PNG "VM uzantısı dikey penceresi")](media/vm-update-management/3-lg.PNG#lightbox)

3. Daha önce oluşturulan çalışma alanı kimliği ve birincil anahtar tıklatıp LogAnalytics çalışma alanında aracıyla bağlantı sağlayan **Tamam** uzantısını dağıtmak için.

   [![](media/vm-update-management/4-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/4-lg.PNG#lightbox) 

4. Bölümünde anlatıldığı gibi [Otomasyon güncelleştirme yönetimi belgeleri](https://docs.microsoft.com/azure/automation/automation-update-management), yönetmek istediğiniz her VM için güncelleştirme yönetimi çözümü etkinleştirmeniz gerekir. Çalışma alanınıza raporlayan tüm sanal makineler için çözümü etkinleştirmek için işaretleyin **güncelleştirme yönetimi**, tıklayın **yönetme makineler**ve ardından **tüm mevcut ve gelecekteki makinelerdeEtkinleştir** seçeneği.

   [![](media/vm-update-management/5-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Azure Stack Vm'leri çalışma alanına bu rapor için her çözümü etkinleştirmek için bu adımı yineleyin. 
  
Azure güncelleştirme ve yapılandırma yönetimi uzantısı etkinleştirildikten sonra bir tarama yönetilen her VM için günde iki kez gerçekleştirilir. API 15 dakikada bir sorguya son güncelleştirme zamanı durumu değişip değişmediğini belirlemek için çağrılır. Durum değiştiyse, bir Uyumluluk taraması başlatılır.

Vm'leri tarandıktan sonra güncelleştirme yönetimi çözümünü Azure Automation hesabında görünür: 

   [![](media/vm-update-management/6-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> 30 dakika ve Panoda yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin görüntülenmesi için 6 saat arasında sürebilir.

Azure Stack VM'ler artık Azure sanal makineleri ile birlikte zamanlanmış güncelleştirme dağıtımlarının eklenebilir.

## <a name="enable-azure-monitor-for-vms-running-on-azure-stack"></a>Azure Stack üzerinde çalışan sanal makineler için Azure İzleyicisi'ni etkinleştirme
VM olduğunda **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** ve **Azure İzleyici bağımlılık aracısını** yüklü uzantılar, onu başlayacak verileri raporlama [Azure İzleyici VM'ler için](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) çözüm. 

> [!TIP]
> **Azure İzleyici bağımlılık aracısını** uzantısı, herhangi bir parametre gerektirmez. Azure İzleyici Vm'leri harita bağımlılık aracısı için hiçbir veri aktarır değil ve güvenlik duvarları veya bağlantı noktaları için herhangi bir değişiklik yapılması gerekmez. Harita verileri her zaman doğrudan Azure İzleyici'hizmetine veya üzerinden Log Analytics aracısını tarafından aktarılan [OMS ağ geçidi](https://docs.microsoft.com/azure/azure-monitor/platform/gateway) BT güvenlik ilkeleriniz bilgisayarların internet'e bağlanmak için ağ üzerinde izin verme durumunda.

VM'ler için Azure İzleyici, birkaç ana performans göstergelerini (KPI'lar) bir sanal makineye ne kadar iyi belirlemenize yardımcı olmak için şu gerçekleştiriyor hedefleyen bir dizi performans grafiklerini içerir. Performans sorunlarını, anormallikleri belirlemek ya da seçili ölçüme göre kaynak kullanımını görüntülemek için her bir makine listeleyen bir perspektif geçiş grafikleri bir süre boyunca kaynak kullanımını gösterir. Performans ile işlem yapılırken dikkate alınması gereken çok sayıda öğe olsa da, Azure İzleyici Vm'leri izleyiciler anahtar işletim sistemi performans göstergeleri için işlemci, bellek, ağ bağdaştırıcısı ve disk kullanımı için ilgili. Performans sistem durumu izleme özelliğini tamamlar ve bir olası sistem bileşeni hatası, destek ayarlama ve iyileştirme verimlilik elde etmek için gösteren sorunları ortaya veya kapasite planlamasını desteklemek yardımcı olur.

   ![Azure İzleyici performans sekmesi](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-performance/vminsights-performance-aggview-01.png)

Windows ve Linux sanal makinelerinde Azure Stack'te çalışan bulunan uygulama bileşenlerini görüntüleme Azure İzleyici ile iki şekilde VM'ler için doğrudan bir sanal makineden veya Azure İzleyici'den VM grupları arasında gösterilebilir.
[VM'ler (Önizleme) Map uygulama bileşenleri anlamak için Azure İzleyici'ı kullanarak](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-maps) makale deneyimi iki perspektiften arasındaki eşleme özelliğini nasıl kullanacağınızı anlamanıza yardımcı olur.

   ![Azure İzleyici performans sekmesi](https://docs.microsoft.com/azure/azure-monitor/insights/media/vminsights-maps/map-multivm-azure-monitor-01.png)


## <a name="enable-update-management-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak güncelleştirme yönetimini etkinleştirme
Çok sayıda Azure Stack VM'ler varsa, kullanabileceğiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) daha kolay vm'lerde çözümü dağıtmak için. Şablon, Microsoft Monitoring Agent uzantısını mevcut bir Azure Stack VM'ye dağıtır ve mevcut bir Azure LogAnalytics çalışma alanına ekler.
 
## <a name="next-steps"></a>Sonraki adımlar
[SQL Server VM performansını iyileştirme](azure-stack-sql-server-vm-considerations.md)




