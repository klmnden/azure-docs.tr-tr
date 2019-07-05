---
title: Azure Otomasyonu - güncelleştirme yönetimi SCCM koleksiyonlarını kullanarak güncelleştirmeleri
description: Bu makale, SCCM ile yönetilen bilgisayarların güncelleştirmelerini yönetmek üzere System Center Configuration Manager’ı bu çözümle yapılandırmanıza yardımcı olmaya yöneliktir.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 92a93982cdd042a92b006cab7052ad4a6fee6fff
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478196"
---
# <a name="integrate-system-center-configuration-manager-with-update-management"></a>System Center Configuration Manager güncelleştirme yönetimi ile tümleştirme

PC, sunucu ve mobil cihazları yönetmek için System Center Configuration Manager’a yatırım yapmış müşteriler aynı zamanda yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve olgunluğundan yararlanmaktadır.

Rapor ve oluşturma ve önceden yazılım güncelleştirme dağıtımlarını Configuration Manager'ı hazırlama tarafından yönetilen Windows sunucularını güncelleştirme ve kullanarak tamamlanmış güncelleştirme dağıtımlarının ayrıntılı durumunu alın [güncelleştirme yönetimi çözümü](automation-update-management.md). Güncelleştirme uyumluluğu raporlamasının ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için Configuration Manager kullanıyorsanız, güvenlik güncelleştirmeleri ile güncelleştirme yönetimi çözümü yönetilse de, Configuration Manager için raporlama devam edebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Olmalıdır [güncelleştirme yönetimi çözümünü](automation-update-management.md) Otomasyon hesabınıza eklendi.
* Şu anda System Center Configuration Manager ortamınız tarafından yönetilen Windows sunucularının da Güncelleştirme Yönetimi çözümü etkin olan Log Analytics çalışma alanına rapor göndermesi gerekir.
* Bu özellik System Center Configuration Manager geçerli dal sürümü 1606 ve üstünü etkinleştirilir. Configuration Manager Merkezi Yönetim sitenizi veya tek başına bir birincil sitede Azure İzleyici günlüklerine ile tümleştirmek ve koleksiyonları içeri aktarmak için gözden [Azure İzleyici için Configuration Manager'ı bağlama günlüklerini](../azure-monitor/platform/collect-sccm.md).  
* Windows aracıları Configuration Manager’dan güvenlik güncelleştirmeleri almazsa Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.   

Azure IaaS içinde barındırılan istemcileri mevcut Configuration Manager ortamınızla nasıl yönettiğiniz birincil olarak Azure veri merkezleri ile altyapınız arasında mevcut olan bağlantıya bağlıdır. Bu bağlantı, Configuration Manager altyapısında yapmanız gereken her türlü tasarım değişikliğini ve bu değişiklikleri desteklemeyle ilgili maliyetleri etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](/sccm/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="configuration"></a>Yapılandırma

### <a name="manage-software-updates-from-configuration-manager"></a>Configuration Manager’dan yazılım güncelleştirmelerini yönetme 

Güncelleştirme dağıtımlarını Configuration Manager’dan yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Configuration Manager'için bağlanır. Güncelleştirme içeriği, dağıtım Configuration Manager’dan yönetiliyormuş gibi istemci bilgisayar önbelleğinden alınabilir.

1. Configuration Manager hiyerarşinizde en üst düzeydeki siteden bir yazılım güncelleştirme dağıtımı oluşturmak için [yazılım güncelleştirme işlemini dağıtma](/sccm/sum/deploy-use/deploy-software-updates) bölümünde açıklanan işlemi kullanın. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturarak güncelleştirme yönetimi çözümü tarafından yönetilir.

1. Azure Otomasyonu'nda seçin **güncelleştirme yönetimi**. İçinde açıklanan adımları izleyerek yeni bir dağıtımını oluşturun [güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun seçmek için açılır Configuration Manager koleksiyonu. Aşağıdaki önemli noktalara dikkat edin: bir. Seçili Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri yerine bunu dikkate **süresi** zamanlanmış dağıtımda tanımlanan ayarı.
    b. Hedef koleksiyonun üyeleri (doğrudan Log Analytics ağ geçidi veya Ara sunucu aracılığıyla) Internet bağlantısı olması gerekir.

Azure Otomasyonu ile güncelleştirme dağıtımını tamamladıktan sonra seçili bilgisayar grubunun üyesi olan hedef bilgisayarlar zamanlanan saatte güncelleştirmeleri kendi yerel istemci önbelleğine yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

### <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu yazılım güncelleştirmelerini yönetme

Configuration Manager istemcisi olan Windows Server VM’lerinin güncelleştirmelerini yönetmek için, istemci ilkesini bu çözüm tarafından yönetilen tüm istemcilere ait Yazılım Güncelleştirme Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [System Center Configuration Manager'da istemci ayarlarını yapılandırma](/sccm/core/clients/deploy/configure-client-settings) makalesini inceleyin.

Bu yapılandırma değişikliğini uyguladıktan sonra açıklanan adımları izleyerek yeni bir dağıtım oluşturma [güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun Configuration Manager koleksiyonunu seçmek için açılır.

## <a name="next-steps"></a>Sonraki adımlar

