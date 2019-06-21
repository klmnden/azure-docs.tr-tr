---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/02/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: 50a215175d7305834a64b7e0cfbc153431b10b7c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188350"
---
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Portalda bakım için zamanlanmış Vm'leri görüntüleme

Bir planlı bakım dalgası zamanlanmış sonra gelecek bakım dalgasından etkilenen sanal makinelerin listesini görebilirsiniz. 

Azure portalını kullanın ve bakım için zamanlanmış Vm'leri arayın.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol gezinti bölmesinde tıklayın **sanal makineler**.

3. Sanal makineler bölmesinden **sütunları** düğmesi kullanılabilir sütunlar listesini açın.

4. Seçin ve aşağıdaki sütunları ekleyin:

   **Bakım**: VM için bakım durumu gösterilir. Olası değerler şunlardır:
      
      | Değer | Açıklama |
      |-------|-------------|
      | Şimdi başlayın | Bakım kendiniz başlatmak sağlayan Self Servis bakım penceresinde vm'dir. Sanal makinenizde bakım başlatma konusunda aşağıya bakın. | 
      | Zamanlanmış | VM, size başlatma seçeneği sunulmayan bir bakım için zamanlanır. Bakım - bu görünümde veya VM'ye tıklayarak zamanlanmış penceresini seçerek bakım penceresi öğrenebilirsiniz. | 
      | Zaten güncelleştirildi | Sanal makinenizin zaten güncelleştirilmiş ve şu anda başka bir eylem gerekli değildir. | 
      | Daha sonra yeniden deneyin | Bakım başarısız oldu başlattınız. Daha sonraki bir zamanda Self Servis Bakım seçeneğini kullanmanız mümkün olacaktır. | 
      | Şimdi yeniden deneyin | Daha önce başarısız bir kendi kendine başlatılan bakım yeniden deneyebilir. | 
      | - | Sanal makinenize bir planlı bakım dalgası parçası değil. |
      

   **Bakım - Self Servis pencere**: Vm'lerinizde kendi kendine Bakımı başlayabilirsiniz zaman penceresi gösterilir.
   
   **Bakım - zamanlanmış pencere**: Azure, sanal Makinenizin bakım tamamlamak için bakımını yaparken zaman penceresini gösterir. 



## <a name="notification-and-alerts-in-the-portal"></a>Bildirim ve Portalı'nda uyarılar

Azure aboneliğine sahip ve ikincil sahipler gruba e-posta göndererek planlı bakım için zamanlama iletişim kurar. Azure etkinlik günlüğü uyarıları oluşturarak, bu iletişim için ek alıcılar ve kanallar ekleyebilirsiniz. Daha fazla bilgi için [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](../articles/azure-monitor/platform/alerts-activity-log-service-notifications.md).

Ayarladığınızdan emin olun **olay türü** olarak **planlı Bakım** ve **Hizmetleri** olarak **sanal makine ölçek kümeleri** ve/veya **Sanal makineler**
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Portaldan vm'nizde Bakımı Başlat

Sanal makine ayrıntıları bakarken bakımı ile ilgili daha fazla ayrıntı mümkün olacaktır.  
Sanal makinenize bir planlı bakım dalgası içinde yer alıyorsa, VM Ayrıntılar görünümü üst kısmında yeni bir bildirim Şerit eklenir. Ayrıca, mümkünse bakım başlatmak için yeni bir seçenek eklendi. 


Bakım bildirimi planlı bakım hakkında daha fazla ayrıntı içeren koruma sayfasını görmek için tıklayın. Buradan, şunları yapabilecek **Bakımı Başlat** vm'nizde.

Bakım başlattıktan sonra sanal makine korunur ve Bakım durumu birkaç dakika içinde sonucu yansıtacak şekilde güncelleştirilir.

Self Servis penceresi Kaçırıldı, VM'niz Azure tarafından korunur, pencerenin görmeye devam edersiniz. 
