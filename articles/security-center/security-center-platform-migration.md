---
title: "Azure Güvenlik Merkezi Platform Geçişi | Microsoft Docs"
description: "Bu belgede, Azure Güvenlik Merkezi verilerinin toplanma biçiminde yapılan bazı değişiklikler açıklanmaktadır."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d49f7986e09a90c5c4c49c0d3963d0cd8514713a
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017


---
<a id="azure-security-center-platform-migration" class="xliff"></a>

# Azure Güvenlik Merkezi platform geçişi

Haziran 2017’nin başlarından itibaren Azure Güvenlik Merkezi tarafından verilerin toplanma ve depolanma biçiminde önemli değişiklikler yapılmaktadır.  Bu değişiklikler, güvenlik verilerini kolayca arama ve diğer Azure yönetim ve izleme hizmetleriyle daha uyumlu olma gibi yeni özellikleri kullanıma açıyor.

> [!NOTE]
> Platform geçişinin üretim kaynaklarınızı etkilememesi bekleniyor ve sizin herhangi bir işlem yapmanız gerekmiyor.


<a id="whats-happening-during-this-platform-migration" class="xliff"></a>

## Bu platform geçişi sırasında ne oluyor?

Güvenlik Merkezi, VM’lerinizden güvenlik verilerini toplamak için eskiden Azure İzleme Aracısı’nı kullanıyordu. Buna güvenlik açıklarını tanımlamak için kullanılan güvenlik yapılandırmalarıyla ilgili bilgiler ve tehditleri algılamak için kullanılan güvenlik olayları dahildir. Bu veriler, Azure’daki Depolama hesabınızda veya hesaplarınızda saklanıyordu.

Bundan sonra, Güvenlik Merkezi Microsoft Monitoring Agent’ı (Operations Management Suite ve Log Analytics hizmeti tarafından kullanılan aracının aynısı) kullanacak. Bu aracıdan toplanan veriler, sanal makinenin coğrafi konumu göz önünde bulundurularak Azure aboneliğinizle ilişkili bir *Log Analytics* [çalışma alanında](../log-analytics/log-analytics-manage-access.md) veya yeni çalışma alanlarında depolanır.

<a id="agent" class="xliff"></a>

## Aracı

Geçiş kapsamında, şu anda veri toplanmakta olan tüm Azure VM’lerine Microsoft Monitoring Agent ([Windows](../log-analytics/log-analytics-windows-agents.md) veya [Linux](../log-analytics/log-analytics-linux-agents.md) için) yüklenir.  VM’de zaten Microsoft Monitoring Agent yüklüyse, Güvenlik Merkezi yüklü olan aracıyı kullanır.

Veri kaybı olmaksızın sorunsuz bir geçiş sağlamak amacıyla, her iki aracı bir süreliğine (genellikle birkaç gün) yan yana çalışır. Bu sayede, Microsoft geçerli işlem hattını kullanımdan kaldırmadan önce yeni veri işlem hattının çalışır durumda olduğunu doğrulayabilir. Doğrulama işleminin ardından, Azure İzleme Aracısı VM’lerinizden kaldırılır. Sizin herhangi bir iş yapmanız gerekmez. Tüm müşterilerin geçişi tamamlandıktan sonra bir e-posta bildirimi alırsınız.
 
Geçiş sırasında Azure İzleme Aracısı’nı el ile kaldırmanız güvenlik verilerinde boşluklara yol açabileceğinden, bunu yapmanız önerilmez. Daha fazla yardıma ihtiyacınız varsa lütfen [Microsoft Müşteri Hizmetleri ve Destek](https://support.microsoft.com/contactus/) birimine başvurun. 

Windows için Microsoft Monitoring Agent, 443 numaralı TCP bağlantı noktasının kullanımını gerektirir. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](security-center-troubleshooting-guide.md)’nu okuyun.


> [!NOTE] 
> Microsoft Monitoring Agent diğer Azure yönetim ve izleme hizmetleri tarafından kullanılıyor olabileceğinden, Güvenlik Merkezi’nde veri toplamayı kapattığınızda aracı otomatik olarak kaldırılmayabilir. Bununla birlikte, gerekirse aracıyı el ile kaldırabilirsiniz.

<a id="workspace" class="xliff"></a>

## Çalışma alanı

Daha önce açıklandığı gibi, Microsoft Monitoring Agent’tan Güvenlik Merkezi adına toplanan veriler, sanal makinenin coğrafi konumu göz önünde bulundurularak Azure aboneliğinizle ilişkili Log Analytics çalışma alanlarında veya yeni çalışma alanlarında depolanır.

Azure portalında, Güvenlik Merkezi tarafından oluşturulanlar dahil olmak üzere Log Analytics çalışma alanlarınızın listesine göz atabilirsiniz. Yeni çalışma alanları için bir ilgili kaynak grubu oluşturulur. İkisi de şu adlandırma kuralını izler:

- Çalışma Alanı: *DefaultWorkspace-[abonelik-kimliği]-[bölge]*
- Kaynak Grubu: *DefaultResouceGroup-[bölge]* 
 
Güvenlik Merkezi tarafından oluşturulan çalışma alanları için veriler 30 gün boyunca tutulur. Mevcut çalışma alanları için elde tutma süresi, çalışma alanının fiyatlandırma katmanını temel alır.

> [!NOTE]
> Güvenlik Merkezi tarafından daha önce toplanan veriler Depolama hesaplarınızda kalır. Geçiş tamamlandıktan sonra bu Depolama hesaplarını silebilirsiniz.

<a id="oms-security-solution" class="xliff"></a>

### OMS Güvenlik Çözümü 

OMS Güvenlik Çözümü, çözümü yüklememiş olan mevcut müşterilerin çalışma alanına Microsoft tarafından yalnızca Azure VM’lerini hedefleyecek şekilde yükleniyor. OMS yönetim konsolundan bu çözümün kaldırılması otomatik olarak düzeltilemeyen bir işlem olduğundan, bunun yapılması önerilmez.


<a id="other-updates" class="xliff"></a>

## Diğer güncelleştirmeler

Platform geçişinin yanı sıra bazı ek, küçük güncelleştirmeleri de kullanıma sunuyoruz:

- Ek işletim sistemi sürümleri desteklenecek. [Buradan](security-center-faq.md#virtual-machines) listeye bakabilirsiniz.
- İşletim Sistemi güvenlik açıklarının listesi genişletilecek. [Buradan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) listeye bakabilirsiniz.
- [Fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/) saatlere eşit olarak bölünecek (önceden günlere bölünüyordu) ve bu sayede bazı müşteriler tasarruf edebilecek.
- Standart fiyatlandırma katmanındaki müşteriler için Veri Toplama gerekli olacak ve otomatik olarak etkinleştirilecek.
- Azure Güvenlik Merkezi, Azure uzantıları aracılığıyla dağıtılmamış kötü amaçlı yazılım çözümlerini bulmaya başlayacak. İlk olarak Symantec Endpoint Protection ve Windows 2016 için Defender’ı bulma özelliği kullanılabilecek.


