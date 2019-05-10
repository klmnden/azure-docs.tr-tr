---
title: Azure Önizleme Gözcü Syslog verilere | Microsoft Docs
description: Azure Gözcü için Syslog veri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 673b1df6094703bebcbfd9d82c1268c01d46e814
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233589"
---
# <a name="connect-your-external-solution-using-syslog"></a>Syslog kullanarak dış çözümünüzü bağlayın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Syslog ile Azure Gözcü destekleyen herhangi bir şirket içi gereç bağlanabilirsiniz. Bu, bir Linux makineye Gereci ve Azure Gözcü arasında dayalı bir aracı kullanarak gerçekleştirilir. Azure'da Linux makinenizde ise gereç veya uygulamayı Azure'da oluşturun ve bağlayın ayrılmış bir çalışma günlüklerinden akışını yapabilirsiniz. Azure'da Linux makinenizde değilse, günlükler, gereçten ayrılmış bir akışını şirket içi VM veya makine Linux için aracıyı yüklediğiniz. 

> [!NOTE]
> Gerecinize Syslog CEF destekliyorsa, daha kapsamlı bir bağlantıdır ve bu seçeneği belirleyin ve yönergeleri izleyin, [CEF verilere bağlanma](connect-common-event-format.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Syslog bağlantı, Linux için aracıyı kullanılarak elde edilir. Varsayılan olarak, Linux için aracıyı olayların Syslog daemon'dan UDP üzerinden ancak gibi Linux Aracısı olaylar diğer cihazlardan alındığında, yapılandırma şekilde değiştirilir burada bir Linux makineye yüksek hacimli Syslog olayları toplamak için beklenen durumlarda alır Syslog daemon'ı ve aracıyı arasında TCP aktarımı kullanın.

## <a name="connect-your-syslog-appliance"></a>Syslog gerecinize bağlanma

1. Gözcü Azure portalında **veri bağlayıcıları** ve **Syslog** Döşe.
2. Linux makinenizi Azure içinde değilse, indirip Azure Gözcü **Linux için aracıyı** appliance'ınız üzerinde. 
1. Azure'da çalışıyorsanız seçin veya bir VM, Syslog iletileri almaya ayrılmış Azure Gözcü çalışma alanı içinde oluşturun. Azure Gözcü çalışma alanlarında VM'yi seçin ve tıklayın **Connect** sol bölmenin üstünde.
3. Tıklayın **bağlanması için günlüklerini yapılandırma** Syslog bağlayıcı Kurulumu edilene. 
4. Tıklayın **yapılandırma dikey penceresini açmak için buraya basın**.
1. Seçin **veri** ardından **Syslog**.
   - Tabloda, gönderdiğiniz her tesis Syslog tarafından olduğundan emin olun. Her özellik için oluşturacağınız izlemek için bir önem derecesini ayarlayın. **Uygula**'ya tıklayın.
1. Syslog makineniz bu tesislerde gönderiyorsanız emin olun. 

3. İlgili şema Log Analytics'te Syslog günlükleri için kullanmak için arama **Syslog**.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Syslog şirket içi cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
