---
title: "Hedef Azure Otomasyonu - güncelleştirme yönetimi SCCM koleksiyonları kullanarak güncelleştirmeleri | Microsoft Docs"
description: "Bu makale, SCCM ile yönetilen bilgisayarların güncelleştirmelerini yönetmek üzere System Center Configuration Manager’ı bu çözümle yapılandırmanıza yardımcı olmaya yöneliktir."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: gwallace
ms.openlocfilehash: c1636ce84f6d0d228dcb15e62af4a88b50fd84bd
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="integrate-system-center-configuration-manager-with-update-management"></a>System Center Configuration Manager güncelleştirme yönetimi ile tümleştirme

PC, sunucu ve mobil cihazları yönetmek için System Center Configuration Manager’a yatırım yapmış müşteriler aynı zamanda yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve olgunluğundan yararlanmaktadır.

Rapor ve oluşturarak ve yazılım güncelleştirme dağıtımları Yapılandırma Yöneticisi'nde ' önceden hazırlama yönetilen Windows Server update ve kullanarak tamamlanmış güncelleştirme dağıtımlarının ayrıntılı durumunu Al [güncelleştirme yönetimi çözümü](automation-update-management.md). Güncelleştirme uyumluluğu raporlamasının ancak değil Windows sunucuları ile güncelleştirme dağıtımları yönetmek için Configuration Manager kullanıyorsanız, güvenlik güncelleştirmeleri güncelleştirme yönetimi çözümüyle yönetilen sırasında Configuration Manager için raporlama devam edebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bilmeniz gereken [güncelleştirme yönetimi çözümü](automation-update-management.md) Otomasyon hesabınıza eklendi.
* Şu anda System Center Configuration Manager ortamınız tarafından yönetilen Windows sunucularının da Güncelleştirme Yönetimi çözümü etkin olan Log Analytics çalışma alanına rapor göndermesi gerekir.
* Bu özellik, System Center Configuration Manager geçerli dal sürüm 1606 ve daha yüksek etkindir. Configuration Manager merkezi yönetim sitenizi veya tek başına birincil sitenizi Log Analytics ile tümleştirmek ve koleksiyonları içeri aktarmak için [Configuration Manager’ı Log Analytics’e Bağlama](../log-analytics/log-analytics-sccm.md) makalesini gözden geçirin.  
* Windows aracıları Configuration Manager’dan güvenlik güncelleştirmeleri almazsa Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.   

Azure IaaS içinde barındırılan istemcileri mevcut Configuration Manager ortamınızla nasıl yönettiğiniz birincil olarak Azure veri merkezleri ile altyapınız arasında mevcut olan bağlantıya bağlıdır. Bu bağlantı, Configuration Manager altyapısında yapmanız gereken her türlü tasarım değişikliğini ve bu değişiklikleri desteklemeyle ilgili maliyetleri etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](/sccm/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="configuration"></a>Yapılandırma

### <a name="manage-software-updates-from-configuration-manager"></a>Configuration Manager’dan yazılım güncelleştirmelerini yönetme 

Güncelleştirme dağıtımlarını Configuration Manager’dan yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure otomasyonu için günlük analizi çalışma alanınız bağlı istemci bilgisayarları güncelleştirmeleri uygulamak için Configuration Manager'için bağlanır. Güncelleştirme içeriği, dağıtım Configuration Manager’dan yönetiliyormuş gibi istemci bilgisayar önbelleğinden alınabilir.

1. Configuration Manager hiyerarşinizde en üst düzeydeki siteden bir yazılım güncelleştirme dağıtımı oluşturmak için [yazılım güncelleştirme işlemini dağıtma](/sccm/sum/deploy-use/deploy-software-updates) bölümünde açıklanan işlemi kullanın. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, sonraki adımda bir zamanlanan güncelleştirme dağıtımı oluşturarak güncelleştirme yönetimi çözümü tarafından yönetilir.

1. Azure Otomasyonu'nda seçin **güncelleştirme yönetimi**. Açıklanan adımları izleyerek yeni bir dağıtımını oluşturun [bir güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun seçmek için açılır Configuration Manager koleksiyonu. Aşağıdaki önemli noktaları aklınızda tutun: bir. Bakım penceresinde seçili Configuration Manager aygıt koleksiyonda tanımlanmış olması durumunda, bunun yerine koleksiyonun üyeleri dikkate **süresi** ayarı zamanlanmış dağıtımda tanımlı.
    b. Hedef koleksiyonun üyeleri İnternet bağlantısına sahip olmalıdır (doğrudan veya proxy sunucusu ya da OMS Ağ Geçidi aracılığıyla).

Azure Otomasyonu güncelleştirmesi dağıtımıyla tamamladıktan sonra seçili bilgisayar grubunun üyesi olan hedef bilgisayarlara güncelleştirmeleri zamanlanan saat kendi yerel istemci önbelleğine yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

### <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyon yazılım güncelleştirmelerini yönetme

Configuration Manager istemcisi olan Windows Server VM’lerinin güncelleştirmelerini yönetmek için, istemci ilkesini bu çözüm tarafından yönetilen tüm istemcilere ait Yazılım Güncelleştirme Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [System Center Configuration Manager'da istemci ayarlarını yapılandırma](/sccm/core/clients/deploy/configure-client-settings) makalesini inceleyin.

Bu yapılandırma değişikliği gerçekleştirdikten sonra açıklanan adımları izleyerek yeni bir dağıtımını oluşturun [bir güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun Configuration Manager koleksiyonu seçmek için açılır.

## <a name="next-steps"></a>Sonraki adımlar
