---
title: "Azure günlük analizi çalışma alanı oluşturma | Microsoft Docs"
description: "Bulut ve şirket içi ortamlarından yönetim çözümleri ve veri toplamayı etkinleştirmek için günlük analizi çalışma alanı oluşturmayı öğrenin."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 8259a97d28effa7bfa9cfb9d7cd9cd2a14c9d906
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portalında günlük analizi çalışma alanı oluşturma
Azure günlük analizi çalışma alanı ayarlayabilirsiniz Portalı'nda, bir benzersiz günlük analizi kendi veri deposu, veri kaynakları ve çözümlerle ortamıdır.  Aşağıdaki kaynaklardan veri toplama üzerinde düşünüyorsanız, bu makalede açıklanan adımları gereklidir:

* Aboneliğinizde Azure kaynakları
* Şirket içi System Center Operations Manager tarafından izlenen bilgisayarlar
* System Center Configuration Manager aygıt koleksiyonları 
* Azure depolama biriminden tanılama veya günlük verileri

Azure Vm'leri ve, ortamınızdaki Windows veya Linux bilgisayarlar gibi diğer kaynakları için aşağıdaki konulara bakın:

*  [Veri toplama hakkında Azure sanal makineler](log-analytics-quick-collect-azurevm.md) 
*  [Linux bilgisayarlar hakkındaki verileri toplama](log-analytics-quick-collect-linux-computer.md)
*  [Windows bilgisayarlar hakkında veri toplama](log-analytics-quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.<br><br> ![Azure portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Tıklatın **oluşturma**ve ardından aşağıdaki öğeler için seçenekleri seçin:

  * İçin yeni bir ad **OMS çalışma**, gibi *DefaultLAWorkspace*. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * İçin **kaynak grubu**, mevcut bir kaynağı kullanmak için seçtiğiniz zaten Kurulum grubu ya da yeni bir tane oluşturun.  
  * Bir seçin **konumu**.  Ek bilgi için bkz: [günlük analizi bulunan bölgelere](https://azure.microsoft.com/regions/services/).
  * Üç farklı seçebilirsiniz **fiyatlandırma katmanlarına** günlük analizi, ancak bulacağınızı seçmek için bu hızlı başlangıç için **ücretsiz** katmanı.  Belirli katmanları hakkında ek bilgi için bkz: [günlük analizi fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Üzerinde gerekli bilgileri girdikten sonra **OMS çalışma** bölmesinde tıklatın **Tamam**.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Kullanılabilir bir çalışma alanı sahip olduğunuza göre telemetri izleme koleksiyonu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştırın ve ek veri ve analitik Öngörüler sağlamak için bir yönetim çözümünü ekleyin. 

* Azure Tanılama veya Azure storage ile Azure kaynaklardan veri toplamayı etkinleştirmek için bkz: [toplamak Azure Hizmeti günlüklerini ve günlük analizi kullanmak için ölçümleri](log-analytics-azure-storage.md).  
* [System Center Operations Manager veri kaynağı olarak ekleme](log-analytics-om-agents.md) Operations Manager yönetim grubunuzu raporlama aracılardan veri toplamak ve günlük analizi çalışma alanı deponuz depolamak için. 
* Bağlantı [Configuration Manager](log-analytics-sccm.md) koleksiyonlar hiyerarşideki üye olan bilgisayarlara içeri aktarmak için.  
* Gözden geçirme [yönetim çözümleri](/log-analytics-add-solutions.md) kullanılabilir ve ekleme veya bir çözüm alanınızdan kaldırın.

