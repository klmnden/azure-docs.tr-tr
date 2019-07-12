---
title: Gözcü Azure Önizleme için veri kaynağına bağlanın? | Microsoft Docs
description: Azure Gözcü için veri kaynaklarına bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a3b63cfa-b5fe-4aff-b105-b22b424c418a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 9f64497cdf27729cebc243deca1def9ff1e5c680
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673849"
---
# <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Yerleşik Azure Gözcü, ilk veri kaynaklarınıza bağlanması gerekir. Azure Sentinel Microsoft çözümleri, kutunun ve Microsoft tehdit koruması çözümlerinin yanı sıra, Office 365, Azure AD, Azure ATP dahil olmak üzere, Microsoft 365 kaynakları dahil olmak üzere, gerçek zamanlı tümleştirme sağlayan dışında kullanılabilir bağlayıcılar sayısı ile birlikte sunulur ve Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft olmayan çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca ortak olay biçimi, veri kaynaklarınızı Azure Gözcü ile de bağlanmak için Syslog veya REST API de kullanabilirsiniz.  

1. Menüsünde **veri bağlayıcıları**. Bu sayfa, Azure Gözcü sağlayan bağlayıcılar ve durumlarını tam listesini görmenizi sağlar. Bağlamayı seçin ve istediğiniz bağlayıcıyı seçin **açık Bağlayıcısı sayfasının**. 

   ![Veri toplayıcılar](./media/collect-data/collect-data-page.png)

1. Özel bağlayıcı sayfasında, tüm önkoşulların yerine getirilmesini ve Azure Gözcü için verileri bağlanma yönergeleri emin olun. Bu, Azure Gözcü ile eşitlemeye başlamak günlükleri için biraz zaman alabilir. Bağlandıktan sonra verileri bir özetini görmek **alınan veriler** graph ve bağlantı durumunu veri türleri.

   ![Toplayıcıları bağlanma](./media/collect-data/opened-connector-page.png)
  
1. Tıklayın **sonraki adımlar** Azure Gözcü sağlar özel veri türü için kullanıma hazır içerik listesini almak için sekmesinde.

   ![Veri toplayıcılar](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Veri bağlantı yöntemi

Aşağıdaki veri bağlantı yöntemlerine Azure Gözcü tarafından desteklenir:

- **Microsoft Hizmetleri**:<br> Microsoft Hizmetleri yerel olarak, out-hazır tümleştirme için Azure temel yararlanarak bağlı, aşağıdaki çözümlerin birkaç tıklamayla bağlanabilir:
    - [Office 365](connect-office-365.md)
    - [Azure AD denetim günlüklerini ve oturum açma işlemleri](connect-azure-active-directory.md)
    - [Azure etkinlik](connect-azure-activity.md)
    - [Azure AD Kimlik Koruması](connect-azure-ad-Identity-protection.md)
    - [Azure Güvenlik Merkezi](connect-azure-security-center.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Gelişmiş tehdit koruması](connect-azure-atp.md)
    - [Cloud App Security'yi](connect-cloud-app-security.md)
    - [Windows Güvenlik olayları](connect-windows-security-events.md) 
    - [Windows Güvenlik Duvarı](connect-windows-firewall.md)

- **Dış çözümleri API aracılığıyla**: Bazı veri kaynakları, bağlı veri kaynağı tarafından sağlanan API'leri kullanarak bağlanır. Genellikle, çoğu güvenlik teknolojileri bir olay günlükleri alınabilir API kümesi sağlar. API'leri Azure Gözcü için bağlanın ve belirli veri türlerini toplayın ve bunları Azure Log Analytics'e gönderme. API aracılığıyla bağlı cihazları şunlardır:
    - [Barracuda](connect-barracuda.md)
    - [Symantec](connect-symantec.md)
- **Dış çözümleri aracı üzerinden**: Azure Sentinel bir aracı yoluyla Syslog protokolünü kullanarak gerçek zamanlı günlük akışını yapabilmek için diğer tüm veri kaynaklarına bağlanabilir. <br>Çoğu Gereçleri, kendisi ve günlüğü hakkında daha fazla veri günlüğe içeren olay iletileri göndermek için Syslog protokolünü kullanır. Günlüklerinin biçimi değişir, ancak çoğu Gereçleri Common Event Format (CEF) standart destekler. <br>Microsoft Monitoring Agent'ı temel alır, Azure Gözcü aracı biçimlendirilmiş CEF günlükleri Log Analytics tarafından alınan bir biçime dönüştürür. Gereç türüne bağlı olarak aracıyı doğrudan gereç veya ayrılmış bir Linux sunucusu üzerinde yüklü. Linux için aracıyı olayların Syslog daemon'dan UDP üzerinden alır, ancak yüksek hacimli Syslog olayları toplamak için bir Linux makine nerede beklendiği durumlarda, TCP üzerinden aracı Syslog cinini ve Log analytics'e buradan gönderilirler.
    - Güvenlik duvarları, Ara sunucuları ve uç noktaları:
        - [F5](connect-f5.md)
        - [Denetim noktası](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [Fortinet](connect-fortinet.md)
        - [Palo Alto](connect-paloalto.md)
        - [Diğer CEF cihazları](connect-common-event-format.md)
        - [Diğer Syslog cihazları](connect-syslog.md)
    - DLP çözümleri
    - [Tehdit zekası sağlayıcıları](connect-threat-intelligence.md)
    - [DNS makineler](connect-dns.md) -doğrudan DNS makinede yüklü aracı
    - Linux sunucuları
    - Diğer Bulutlar
    
## Aracı bağlantı seçenekleri<a name="agent-options"></a>

Azure Gözcü için dış cihazınıza bağlanmak için aracı adanmış bir makinede dağıtılması gerekir (VM veya şirket içi) Gereci ve Azure Gözcü arasındaki iletişimi desteklemek için. Aracı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım, yalnızca ayrılmış makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir. 


![Azure'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracı vm'sinde başka bir bulut, mevcut bir Azure sanal makinesinde el ile veya bir şirket içi makinede dağıtabilirsiniz.

![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)


## <a name="next-steps"></a>Sonraki adımlar

- Gözcü Azure ile çalışmaya başlamak için Microsoft Azure aboneliği gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Bilgi nasıl [ekleme verilerinizi Azure Gözcü](quickstart-onboard.md), ve [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
