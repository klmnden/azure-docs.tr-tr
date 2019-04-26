---
title: Azure Önizleme Gözcü DNS verilere | Microsoft Docs
description: Azure Gözcü DNS verileri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: e34db2bdc78eb846cf4885b1ef083fd3b21e21b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445575"
---
# <a name="connect-your-domain-name-server"></a>Etki alanı adı server'ınıza bağlanın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tüm etki alanı adı sunucusu (Azure Gözcü için Windows üzerinde çalışan DNS) bağlanabilirsiniz. Bu, DNS makinede bir aracı yükleyerek gerçekleştirilir. DNS kullanarak günlükleri, güvenlik, performans ve işlem ile ilgili Öngörüler kuruluşunuzun DNS altyapısına toplayarak, analiz, bilgi edinebilir ve DNS sunucularından analiz ilişkilendirme ve Denetim günlükleri ve diğer ilgili verileri.

DNS günlüğü bağlantısını etkinleştirdiğinizde, şunları yapabilirsiniz:
- Kötü amaçlı etki alanı adlarını çözümlemeye çalışan istemcileri
- Eski kaynak kayıtları tanımlayın
- Sık sık sorgulanan etki alanı adlarını ve DNS sık iletişim kuran istemcileri tanımlama
- DNS sunucularındaki istek yükünü görüntüle
- Dinamik DNS kayıt hataları görüntüle

## <a name="how-it-works"></a>Nasıl çalışır?

DNS bağlantı DNS makinede bir aracı yükleyerek gerçekleştirilir. Aracı olayları DNS'den çeker ve bunları Log Analytics'e geçirir.

## <a name="connect-your-dns-appliance"></a>DNS gerecinize bağlanma

1. Gözcü Azure portalında **veri bağlayıcıları** ve **DNS** Döşe.
1. DNS makinelerinizi Azure'da varsa:
    1. Tıklayın **indirin ve Windows sanal makineleri için aracıyı yükleme**.
    1. İçinde **sanal makineler** listesinde, istediğiniz Azure Gözcü akışını sağlamak için DNS makine seçin. Bu bir Windows VM olduğundan emin olun.
    1. Bu VM için açılır pencerede **Connect**.  
    1. Tıklayın **etkinleştirme** içinde **DNS bağlayıcı** penceresi. 

2. DNS makinenizi Azure VM'deki değilse:
    1. Tıklayın **indirin ve Windows Azure olmayan makineler için aracıyı yükleme**.
    1. İçinde **doğrudan aracı** penceresinde seçin **indirme Windows aracısını (64 bit)** veya **indirme Windows aracısını (32 bit)**.
    1. Aracıyı DNS makinenize yükleyin. Kopyalama **çalışma alanı kimliği**, **birincil anahtar**, ve **ikincil anahtar** ve yükleme sırasında istendiğinde kullanabilirsiniz.

3. İlgili şema için DNS günlükleri Log Analytics'te kullanmak için arama **DnsEvents**.

## <a name="validate"></a>Doğrulama 

İçin şemayı log Analytics'te Ara **DnsEvents** ve olayları olmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için DNS şirket içi cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
