---
title: VM güncelleştirme ve Azure Stack ile Yönetimi | Microsoft Docs
description: Azure Stack'te dağıtılan Windows Vm'leri yönetmek için Azure Otomasyonu'nda güncelleştirme yönetimi, değişiklik izleme ve sayım çözümlerini kullanmayı öğrenin.
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
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: rtiberiu
ms.openlocfilehash: be793fa5d346d05e6b7bd9f93f1108b7a3542fa6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959181"
---
# <a name="azure-stack-vm-update-and-management"></a>Azure Stack VM güncelleştirme ve yönetim
Azure Stack kullanılarak dağıtılan Windows Vm'leri yönetmek için aşağıdaki Azure Otomasyonu çözüm özellikleri kullanabilirsiniz:

- **[Güncelleştirme yönetimi](https://docs.microsoft.com/azure/automation/automation-update-management)**. Güncelleştirme yönetimi çözümü ile hızlı bir şekilde tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu değerlendirebilir ve bu Windows Vm'leri için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

- **[Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking)**. Yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve izlenen sunucularda dosya değişiklikleri işleme için buluttaki Log Analytics hizmetine gönderilir. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder. Değişiklik izleme Panoda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

- **[Stok](https://docs.microsoft.com/azure/automation/automation-vm-inventory)**. Bir Azure Stack Windows sanal makinesi için izleme envanteri ayarlamak ve stok toplama yapılandırmak için bir tarayıcı tabanlı kullanıcı arabirimi sağlar. 

> [!IMPORTANT]
> Bu çözümler Azure Vm'leri yönetmek için kullanılan olanlarla aynıdır. Hem Azure hem de Azure Stack Windows Vm'leri aynı şekilde, aynı arabiriminden aynı araçları kullanılarak yönetilir. Azure Stack Vm'leri de aynı Azure Vm'leri olarak Azure Stack ile güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri kullanırken ücretlendirilir.

## <a name="prerequisites"></a>Önkoşullar
Bu özellikleri güncelleştirin ve Azure Stack Windows Vm'leri yönetmek için kullanmadan önce birkaç önkoşulların karşılanması gerekir. Bu, Azure Stack Yönetim Portalı yanı sıra Azure portalı alınması gereken adımlar içerir.

### <a name="in-the-azure-portal"></a>Azure portalında
Azure Stack Windows Vm'leri için Envanter, değişiklik izleme ve güncelleştirme yönetimi Azure Otomasyon özellikleri kullanmak için önce bu çözümleri azure'da etkinleştirmeniz gerekir.

> [!TIP]
> Bu özellikleri Azure Vm'leri için etkin zaten varsa, önceden mevcut olan LogAnalytics çalışma kimlik bilgilerinizi kullanabilirsiniz. LogAnalytics Workspaceıd ve kullanmak istediğiniz birincil anahtar zaten varsa, atlayın [sonraki bölümde](./vm-update-management.md#in-the-azure-stack-administration-portal). Aksi takdirde, yeni LogAnalytics çalışma alanını ve Otomasyon hesabı oluşturmak için bu bölümdeki devam edin.

Bu çözümler etkinleştirmenin ilk adımı [LogAnalytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) Azure aboneliğinizdeki. Log Analytics çalışma alanını kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Log Analytics Ortamı ' dir. Bir çalışma alanı oluşturduktan sonra çalışma alanı kimliği ve anahtarı not edin. Bu bilgileri görüntülemek için çalışma alanı dikey penceresine gidin, tıklayarak **Gelişmiş ayarlar**ve gözden geçirme **çalışma alanı kimliği** ve **birincil anahtar** değerleri. 

Ardından, şunları yapmalısınız [Otomasyon hesabı oluşturma](https://docs.microsoft.com/azure/automation/automation-create-standalone-account). Bir Otomasyon hesabı, Azure Automation kaynaklarınız için bir kapsayıcıdır. Ortamlarınızı ayırmak veya daha fazla Otomasyon iş akışları ve kaynakları düzenlemek için bir yol sunar. Otomasyon hesabı oluşturduktan sonra envanteri etkinleştirmek için değişiklik izleme ve yönetim özelliklerini güncelleştirin. Bunu yapmak için her özelliği etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında, kullanmak istediğiniz Otomasyon hesabınıza gidin.

2. Çözümü etkinleştirmek için seçin (ya da **Envanter**, **değişiklik izleme**, veya **güncelleştirme yönetimi**).

3. Kullanım **çalışma alanı seçin...**  açılır listede kullanmak için Log Analytics çalışma alanı seçin.

4. Kalan tüm bilgilerin doğru olduğunu doğrulayın ve ardından **etkinleştirme** çözümü etkinleştirmek için.

5. Tüm üç çözümlerle 2-4 arası adımları yineleyin. 

   [![](media/vm-update-management/1-sm.PNG "Otomasyon hesabı özellikleri etkinleştir")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="in-the-azure-stack-administration-portal"></a>Azure Stack Yönetim Portalı'nda
Azure portalında Azure Otomasyonu çözümleri etkinleştirdikten sonra sonraki bir bulutun Yöneticisi olarak Azure Stack yönetim portalında oturum açın ve indirmek için ihtiyaç duyduğunuz **Azure güncelleştirme ve yapılandırma yönetimi** Azure uzantısı Yığın Market öğesi. 

   ![Azure güncelleştirme ve yapılandırma yönetimi uzantısı Market öğesi](media/vm-update-management/2.PNG) 

## <a name="enable-update-management-for-azure-stack-virtual-machines"></a>Azure Stack sanal makineler için güncelleştirme yönetimini etkinleştirme
Azure Stack Windows Vm'leri için güncelleştirme yönetimini etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure Stack kullanıcı portalda oturum açın.

2. Azure Stack Kullanıcı-Portalı'nda, bu çözümleri etkinleştirmek istediğiniz sanal makineleri Windows uzantıları dikey penceresine gidin **+ Ekle**seçin **Azure güncelleştirme ve yapılandırma yönetimi**uzantısı seçeneğine tıklayıp **Oluştur**:

   [![](media/vm-update-management/3-sm.PNG "Windows VM uzantısı dikey penceresi")](media/vm-update-management/3-lg.PNG#lightbox)

3. Daha önce oluşturulan çalışma alanı kimliği ve birincil anahtar tıklatıp LogAnalytics çalışma alanında aracıyla bağlantı sağlayan **Tamam** uzantısını dağıtmak için.

   [![](media/vm-update-management/4-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/4-lg.PNG#lightbox) 

4. Bölümünde anlatıldığı gibi [Otomasyon güncelleştirme yönetimi belgeleri](https://docs.microsoft.com/azure/automation/automation-update-management), yönetmek istediğiniz her VM için güncelleştirme yönetimi çözümü etkinleştirmeniz gerekir. Çalışma alanınıza raporlayan tüm sanal makineler için çözümü etkinleştirmek için işaretleyin **güncelleştirme yönetimi**, tıklayın **yönetme makineler**ve ardından **tüm mevcut ve gelecekteki makinelerdeEtkinleştir** seçeneği.

   [![](media/vm-update-management/5-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Azure Stack Windows Vm'leri çalışma alanına bu rapor için her çözümü etkinleştirmek için bu adımı yineleyin. 
  
Azure güncelleştirme ve yapılandırma yönetimi uzantısı etkinleştirildikten sonra bir tarama yönetilen her Windows VM için günde iki kez gerçekleştirilir. Windows API 15 dakikada bir sorguya son güncelleştirme zamanı durumu değişip değişmediğini belirlemek için çağrılır. Durum değiştiyse, bir Uyumluluk taraması başlatılır.

Vm'leri tarandıktan sonra güncelleştirme yönetimi çözümünü Azure Automation hesabında görünür: 

   [![](media/vm-update-management/6-sm.PNG "Çalışma alanı kimliği ve anahtarı sağlayarak")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> 30 dakika ve Panoda yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin görüntülenmesi için 6 saat arasında sürebilir.

Azure Stack Windows Vm'leri artık Azure sanal makineleri ile birlikte zamanlanmış güncelleştirme dağıtımlarının eklenebilir.

## <a name="enable-update-management-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak güncelleştirme yönetimini etkinleştirme
Çok sayıda Azure Stack Windows Vm'leri varsa, kullanabileceğiniz [bu Azure Resource Manager şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) daha kolay Windows vm'lerinde çözümü dağıtmak için. Şablon, Microsoft Monitoring Agent uzantısını mevcut bir Windows VM'ye dağıtır ve mevcut bir Azure LogAnalytics çalışma alanına ekler.
 
## <a name="next-steps"></a>Sonraki adımlar
[SQL Server performansı en iyi duruma getirme](azure-stack-sql-server-vm-considerations.md)
