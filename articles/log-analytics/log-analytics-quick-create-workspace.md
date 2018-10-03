---
title: Azure portalında Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure portalında, Bulut ve şirket içi ortamlarından yönetim çözümleri ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptal
ms.date: 08/23/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: edebeec493b025a81a99c0458344aafe59e769e9
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040883"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portalında Log Analytics çalışma alanı oluşturma
Kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Log Analytics ortamı olan Azure portalında, Log Analytics çalışma alanı ayarlayabilirsiniz.  Bu makalede açıklanan adımları aşağıdaki kaynaklardan veri toplama işlemini istiyorsanız gereklidir:

* Aboneliğinizdeki Azure kaynakları
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen
* System Center Configuration Manager cihaz koleksiyonları 
* Azure depolama biriminden Tanılama veya günlük verileri

Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

*  [Azure sanal makinelerden veri toplama](log-analytics-quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından verileri toplama](log-analytics-quick-collect-linux-computer.md)
*  [Karma Windows bilgisayardan veri topla](log-analytics-quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.

    ![Azure portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)
  
2. **Oluştur**’a tıklayın, ardından şu öğeler için seçim yapın:

  * Yeni bir ad verin **Log Analytics çalışma alanı**, gibi *DefaultLAWorkspace*. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * İçin **kaynak grubu**, mevcut bir kaynağı kullanmayı da tercih Kurulumu zaten grup veya yeni bir tane oluşturun.  
  * Kullanılabilir seçin **konumu**.  Daha fazla bilgi için bkz [Log Analytics'in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
  * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  Mevcut bir Kurumsal Anlaşma (EA) kaydına bağlı aboneliğe veya 2 Nisan'dan önce oluşturulmuş mevcut bir abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)  

3. Gerekli bilgileri girdikten sonra **Log Analytics çalışma alanı** bölmesinde tıklayın **Tamam**.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin. 

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](log-analytics-azure-storage.md).  
* [System Center Operations Manager veri kaynağı olarak ekleme](log-analytics-om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için. 
* Connect [Configuration Manager](log-analytics-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [yönetim çözümleri](https://docs.microsoft.com/azure/monitoring/monitoring-solutions-inventory?toc=%2fazure%2flog-analytics%2ftoc.json) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
