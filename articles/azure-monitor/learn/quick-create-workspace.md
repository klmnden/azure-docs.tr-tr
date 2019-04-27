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
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: magoedte
ms.openlocfilehash: a68e40b7b1caf184fabb5df62d8b461fa2fa11e2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60539238"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Azure portalında Log Analytics çalışma alanı oluşturma
Kullanım **Log Analytics çalışma alanları** Azure portalını kullanarak bir Log Analytics çalışma alanı oluşturmak için menü. Bir Log Analytics çalışma alanı, Azure İzleyici günlük verileri için benzersiz bir ortamdır. Kendi veri deposu ve yapılandırma her çalışma alanına sahiptir ve veri kaynakları ve çözümleri belirli bir çalışma alanında, verilerini depolamak için yapılandırılır. Aşağıdaki kaynaklardan veri toplama işlemini düşünüyorsanız, Log Analytics çalışma gerektirir:

* Aboneliğinizdeki Azure kaynakları
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen
* System Center Configuration Manager cihaz koleksiyonları 
* Azure depolama biriminden Tanılama veya günlük verileri

Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

*  [Azure sanal makinelerden veri toplama](../learn/quick-collect-azurevm.md) 
*  [Karma Linux bilgisayarından verileri toplama](../learn/quick-collect-linux-computer.md)
*  [Karma Windows bilgisayardan veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Log Analytics çalışma alanları**.

    ![Azure portal](media/quick-create-workspace/azure-portal-01.png)
  
2. Tıklayın **Ekle**ve ardından şu öğeler için seçim:

   * Yeni **Log Analytics çalışma alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. 
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * İçin **kaynak grubu**, mevcut bir kaynağı kullanmayı da tercih Kurulumu zaten grup veya yeni bir tane oluşturun.  
   * Kullanılabilir seçin **konumu**.  Daha fazla bilgi için bkz [Log Analytics'in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/) ve Azure İzleyici'deki Ara **ürünü arama** alan.  
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  Mevcut bir Kurumsal Anlaşma (EA) kaydına bağlı aboneliğe veya 2 Nisan'dan önce oluşturulmuş mevcut bir abonelik için bir çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-create-workspace/create-loganalytics-workspace-02.png)  

3. **Log Analytics Çalışma Alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin. 

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](../platform/collect-azure-metrics-logs.md).  
* [System Center Operations Manager veri kaynağı olarak ekleme](../platform/om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için. 
* Connect [Configuration Manager](../platform/collect-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [izleme çözümleri](../insights/solutions.md) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
