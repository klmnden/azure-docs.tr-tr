---
title: Azure Önizleme Gözcü DNS verilere | Microsoft Docs
description: Azure Gözcü DNS verileri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 4e6ed18a49a77f8061c975bdf3ecb085ebf71317
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190761"
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

## <a name="connected-sources"></a>Bağlı kaynaklar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| **Bağlı kaynak** | **Destek** | **Açıklama** |
| --- | --- | --- |
| [Windows aracıları](../azure-monitor/platform/agent-windows.md) | Evet | Çözüm, Windows aracılarından DNS bilgilerini toplar. |
| [Linux aracıları](../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır | Çözüm, doğrudan Linux aracılarından DNS bilgi toplamaz. |
| [System Center Operations Manager yönetim grubu](../azure-monitor/platform/om-agents.md) | Evet | Çözüm, bağlı Operations Manager yönetim grubundaki aracılardan DNS bilgilerini toplar. Azure İzleyici Operations Manager Aracısı'ndan doğrudan bir bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |
| [Azure depolama hesabı](../azure-monitor/platform/collect-azure-metrics-logs.md) | Hayır | Azure depolama çözümü tarafından kullanılmaz. |

### <a name="data-collection-details"></a>Veri koleksiyonu ayrıntıları

Çözüm, Log Analytics aracısını yüklendiği DNS sunucularından DNS envanteri ve DNS ilgili olay verilerini toplar. DNS sunucuları, bölge ve kaynak kayıtları sayısı gibi envanterle ilişkili veri DNS PowerShell cmdlet'lerini çalıştırarak toplanır. Verileri iki günde bir kez güncelleştirilir. İlgili olay verileri neredeyse gerçek zamanlı olarak toplanan [analiz ve Denetim günlükleri](https://technet.microsoft.com/library/dn800669.aspx#enhanc) Gelişmiş DNS günlüğe kaydetme ve tanılama Windows Server 2012 R2 tarafından sağlanan.


## <a name="connect-your-dns-appliance"></a>DNS gerecinize bağlanma

1. Gözcü Azure portalında **veri bağlayıcıları** ve **DNS** Döşe.
1. DNS makinelerinizi Azure'da varsa:
    1. Tıklayın **Azure Windows sanal makine üzerinde aracı yükleme**.
    1. İçinde **sanal makineler** listesinde, istediğiniz Azure Gözcü akışını sağlamak için DNS makine seçin. Bu bir Windows VM olduğundan emin olun.
    1. Bu VM için açılır pencerede **Connect**.  
    1. Tıklayın **etkinleştirme** içinde **DNS bağlayıcı** penceresi. 

2. DNS makinenizi Azure VM'deki değilse:
    1. Tıklayın **Azure olmayan makineler aracı yükleme**.
    1. İçinde **doğrudan aracı** penceresinde seçin **indirme Windows aracısını (64 bit)** veya **indirme Windows aracısını (32 bit)** .
    1. Aracıyı DNS makinenize yükleyin. Kopyalama **çalışma alanı kimliği**, **birincil anahtar**, ve **ikincil anahtar** ve yükleme sırasında istendiğinde kullanabilirsiniz.

3. İlgili şema için DNS günlükleri Log Analytics'te kullanmak için arama **DnsEvents**.

## <a name="validate"></a>Doğrulama 

İçin şemayı log Analytics'te Ara **DnsEvents** ve olayları olmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için DNS şirket içi cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
