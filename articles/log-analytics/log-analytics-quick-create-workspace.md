---
title: Azure günlük analizi çalışma alanı oluşturma | Microsoft Docs
description: Bulut ve şirket içi ortamlarından yönetim çözümleri ve veri toplamayı etkinleştirmek için günlük analizi çalışma alanı oluşturmayı öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: 8ac3d2d90909d740d28eb05396b915280f58c8ba
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
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
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. **Oluştur**’a tıklayın, ardından şu öğeler için seçim yapın:

  * Yeni **OMS Çalışma Alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * İçin **kaynak grubu**, mevcut bir kaynağı kullanmak için seçtiğiniz zaten Kurulum grubu ya da yeni bir tane oluşturun.  
  * Bir seçin **konumu**.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
  * 2 Nisan 2018 sonra oluşturulan yeni bir abonelik içindeki bir çalışma alanı oluşturuyorsanız, otomatik olarak kullanacak *GB başına* fiyatlandırma planı ve bir fiyatlandırma katmanı seçme seçeneği kullanılamaz.  Nisan 2 önce veya varolan bir EA kayıt bağlı aboneliği için oluşturulmuş var olan bir abonelik için bir çalışma alanı oluşturuyorsanız, üç fiyatlandırma katmanları arasında seçim yapma olanağı vardır.  Ücretsiz katmanı seçmek için bu hızlı başlangıç adımıdır.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)<br>  

3. **OMS Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Kullanılabilir bir çalışma alanı sahip olduğunuza göre telemetri izleme koleksiyonu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştırın ve ek veri ve analitik Öngörüler sağlamak için bir yönetim çözümünü ekleyin. 

* Azure Tanılama veya Azure storage ile Azure kaynaklardan veri toplamayı etkinleştirmek için bkz: [toplamak Azure Hizmeti günlüklerini ve günlük analizi kullanmak için ölçümleri](log-analytics-azure-storage.md).  
* [System Center Operations Manager veri kaynağı olarak ekleme](log-analytics-om-agents.md) Operations Manager yönetim grubunuzu raporlama aracılardan veri toplamak ve günlük analizi çalışma alanı deponuz depolamak için. 
* Bağlantı [Configuration Manager](log-analytics-sccm.md) koleksiyonlar hiyerarşideki üye olan bilgisayarlara içeri aktarmak için.  
* Gözden geçirme [yönetim çözümleri](/log-analytics-add-solutions.md) kullanılabilir ve ekleme veya bir çözüm alanınızdan kaldırın.

