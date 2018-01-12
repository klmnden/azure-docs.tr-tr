---
title: "Azure Güvenlik Merkezi sık sorulan sorular (SSS) | Microsoft Docs"
description: "Bu SSS, Azure Güvenlik Merkezi ile ilgili sorular yanıtlanmaktadır."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/11/2018
ms.author: terrylan
ms.openlocfilehash: 2bbd0a8be891bd472cdc631a1f8dc79471d66a77
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Güvenlik Merkezi - Sık sorulan sorular (SSS)
Bu SSS, Azure Güvenlik Merkezi, engellemenize, algılamanıza ve Artırılmış görünürlük aracılığıyla tehditleri Microsoft Azure kaynaklarınızın güvenliğini denetlemenize yanıtlamanıza yardımcı olan bir hizmeti ile ilgili sorular yanıtlanmaktadır.

> [!NOTE]
> Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanacak. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi platformu geçiş](security-center-platform-migration.md). Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>
>

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

### <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi nasıl sağlarım?
Azure Güvenlik Merkezi, Microsoft Azure aboneliğiniz ile etkin ve erişilen [Azure portal](https://azure.microsoft.com/features/azure-portal/). ([Portalı oturum açma](https://portal.azure.com)seçin **Gözat**ve kaydırma **Güvenlik Merkezi**).  

## <a name="billing"></a>Faturalandırma
### <a name="how-does-billing-work-for-azure-security-center"></a>Azure Güvenlik Merkezi için fatura iş nasıl yapar?
Güvenlik Merkezi iki katmanı sunulur:

**Ücretsiz katmanı** ortaklarından güvenlik ürün ve Hizmetleri ile Azure kaynaklarını, temel güvenlik ilkesi, güvenlik önerileri ve tümleştirme güvenlik durumunu görünürlük sağlar.

**Standart katmanı** algılama özellikleri dahil olmak üzere, tehdit Intelligence, davranış analizi, anomali algılama, güvenlik olaylarına ve tehdit attribution raporları Gelişmiş tehdit ekler. Standart katman ilk 60 gün boyunca ücretsizdir. 60 gün ötesinde hizmeti kullanmaya devam etmek seçmesi gereken, biz otomatik olarak hizmet için kaydedilecek başlatın.  Yükseltmek için seçin [fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing) güvenlik ilkesinde.

## <a name="permissions"></a>İzinler
Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../active-directory/role-based-access-built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../active-directory/role-based-access-control-configure.md) kullanır.

Güvenlik Merkezi güvenlik sorunları ve güvenlik açıklarını tanımlamak için kaynaklarınızı yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca kaynak sahibi, katkıda bulunan veya okuyucu rolü abonelik veya kaynak ait olduğu kaynak grubu için atandığında ilgili bilgileri görebilirsiniz.

Bkz: [izinleri Azure Güvenlik Merkezi'nde](security-center-permissions.md) rolleri ve Güvenlik Merkezi'nde izin verilen eylemleri hakkında daha fazla bilgi için.

## <a name="data-collection"></a>Veri toplama
Güvenlik Merkezi güvenlik durumlarına değerlendirmek, güvenlik önerileri sağlamak ve tehditlere karşı sizi uyarmak için sanal makinelerinizi veri toplar. Güvenlik Merkezi ilk eriştiğinizde, veri toplama, aboneliğinizin tüm sanal makinelerin etkinleştirilir. Güvenlik Merkezi İlkesi'nde veri toplamayı de etkinleştirebilirsiniz.

### <a name="how-do-i-disable-data-collection"></a>Veri toplama nasıl devre dışı bırakabilirim?
Azure Güvenlik Merkezi ücretsiz katmanı kullanıyorsanız, sanal makinelerden veri toplamayı dilediğiniz zaman devre dışı bırakabilirsiniz. Veri toplama standart katman abonelikler için gereklidir. Veri koleksiyonu Güvenlik İlkesi'nde bir abonelik için devre dışı bırakabilirsiniz. ([Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **İlkesi**.)  Bir abonelik seçtiğinizde, yeni bir dikey pencere açılır ve devre dışı bırakma seçeneği sağlar **veri toplama**.

### <a name="how-do-i-enable-data-collection"></a>Veri toplama nasıl etkinleştirebilirim?
Güvenlik İlkesi, Azure aboneliğinizde veri koleksiyonunu etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure portalında oturum açın](https://portal.azure.com)seçin **Gözat**seçin **Güvenlik Merkezi**seçip **İlkesi**. Ayarlama **veri toplama** için **üzerinde**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkin olduğunda ne olur?
Veri toplama etkin olduğunda, tüm var olan Microsoft Monitoring Agent otomatik olarak sağlanır ve desteklenen herhangi bir yeni dağıtılan sanal makineler abonelikte.

İşlem oluşturma olayı 4688 aracının verir ve *CommandLine* olay 4688 içinde alan. VM üzerinde oluşturulan yeni işlemleri tarafından olay günlüğüne kaydedilir ve Güvenlik Merkezi algılama hizmetleri tarafından izlenen. Her yeni işlem için kayıtlı ayrıntıları hakkında bilgi için bkz: [4688 açıklama alanlarında](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Aracı ayrıca VM üzerinde oluşturulan 4688 olayları toplar ve bunları aramada depolar.

Güvenlik Merkezi VM şüpheli etkinlik algılarsa, müşteri e-posta ile varsa bildirilir [güvenlik bilgilerini](security-center-provide-security-contact-details.md) sağlanmış. Bir uyarı da Güvenlik Merkezi'nin güvenlik uyarıları panosunda görünür olur.

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>İzleme Aracısı Sunucularım performansını etkiler mi?
Aracı nominal miktarda sistem kaynağı tüketir ve performans üzerinde çok az etkisi olması gerekir. Performans etkisi ve aracı ve uzantı ile ilgili daha fazla bilgi için bkz [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?
Bu Aracıdan toplanan verileri aboneliğinizle ilişkili olan bir günlük analizi çalışma veya yeni bir çalışma alanı olarak depolanır. Daha fazla bilgi için bkz: [veri güvenliği](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Azure Güvenlik Merkezi'ni kullanma
### <a name="what-is-a-security-policy"></a>Bir güvenlik ilkesi nedir?
Bir güvenlik ilkesi belirtilen abonelik içindeki kaynaklar için önerilen denetimleri kümesini tanımlar. Azure Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre Azure aboneliklerinize ilkeleri tanımlar.

Azure Güvenlik Merkezi sürücü güvenlik önerilerini ve izlemeyi etkin güvenlik ilkeleri. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Bir güvenlik ilkesi değiştirebilecekleri?
Güvenlik ilkesini değiştirmek için bir güvenlik yöneticisi veya sahibi veya katkıda bu aboneliğin olması gerekir.

Bir güvenlik ilkesi yapılandırma konusunda bilgi edinmek için [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Güvenlik açısından nedir?
Azure Güvenlik Merkezi, ayrıca Azure kaynaklarınızın güvenlik durumunu çözümler. Olası güvenlik açıkları tanımlandığında, öneri oluşturulur. Öneriler gerekli denetimini yapılandırma işleminde size kılavuzluk. Örnekler şunlardır:

* Tanımlamak ve kötü amaçlı yazılımları kaldırmanıza yardımcı olmak için kötü amaçlı yazılımdan koruma sağlama
* Yapılandırma [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md) ve kuralları trafiği denetlemek için sanal makineler
* Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir web uygulaması Güvenlik Duvarı'nın sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Güvenlik İlkeleri'nde etkinleştirilmiş öneri burada gösterilir.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure Kaynaklarım geçerli güvenlik durumunu nasıl görebilirim?
**Güvenlik Merkezi genel bakış** dikey işlem, ağ, depolama ve veri ve uygulamaları tarafından ayrıntılarıyla ortamınızın genel güvenlik duruşunu gösterir. Olası güvenlik açıkları belirlediyseniz, her kaynak türünün bir göstergesi gösteren vardır. Her bölme tıklatarak aboneliğinizi kaynaklarında envanterini yanı sıra Güvenlik Merkezi tarafından belirlenen güvenlik sorunlarını listesini görüntüler.

### <a name="what-triggers-a-security-alert"></a>Bir güvenlik uyarısını neyin tetikleyeceğini?
Azure Güvenlik Merkezi otomatik olarak toplar, analiz eder ve Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini birleştirir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows Hata Raporlama kullanılarak algılanan Gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü amaçlı yazılım veya Web uygulaması güvenlik duvarı gibi tümleşik iş ortağı güvenlik çözümlerini güvenlik uyarıları

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Algılanan ve Microsoft Security Response Center karşı Azure Güvenlik Merkezi tarafından uyarı tehditlere arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC) select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve Üçüncü taraflardan tehdit Intelligence ve kötüye şikayetlerinden alır. MSRC müşteri verilerini yasadışı veya yetkisiz bir şirket tarafından erişildiğinde veya Azure Müşteri'nin kullanımını hükümlerini kabul edilebilir kullanım için uyumlu değil, hale, güvenlik olay Yöneticisi müşteri size bildirir. Bildirim, genellikle bir güvenlik kişi belirtilmezse, Azure Güvenlik Merkezi veya Azure abonelik sahibi alanında belirtilen güvenlik kişilere e-posta göndererek oluşur.

Güvenlik Merkezi, sürekli olarak Müşteri'nin Azure ortamı izler ve çok çeşitli kötü amaçlı etkinliği otomatik olarak algılamak için analiz uygular bir Azure hizmetidir. Bu algılamaların güvenlik uyarıları Güvenlik Merkezi Panosu'nda olarak ortaya çıkmış.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Hangi Azure kaynaklarını Azure Güvenlik Merkezi tarafından izlenen?
Azure Güvenlik Merkezi aşağıdaki Azure kaynakları izler:

* Sanal makineler (VM'ler) (dahil olmak üzere [bulut Hizmetleri](../cloud-services/cloud-services-choose-me.md))
* Azure Sanal Ağları
* Azure SQL Hizmeti
* Azure Storage hesabı
* Azure Web uygulamaları (içinde [uygulama hizmeti ortamı](../app-service/environment/intro.md))
* Bir web uygulaması güvenlik duvarı sanal makineleri ve uygulama hizmeti ortamı gibi Azure aboneliğinizle tümleşik iş ortağı çözümleri

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Sanal makineler ne tür destekleniyor mu?
İzleme ve öneriler her ikisini de kullanarak oluşturulan sanal makineleri için (VM'ler) kullanılabilir [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md).

Bkz: [desteklenen platformlar Azure Güvenlik Merkezi'nde](security-center-os-coverage.md) desteklenen platformlar listesi.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Neden Azure Güvenlik Merkezi my Azure VM'de çalışan kötü amaçlı yazılımdan koruma çözümünü tanımıyor?
Azure Güvenlik Merkezi Azure uzantılar kötü amaçlı yazılımdan koruma görünürlüğe sahiptir. Örneğin, Güvenlik Merkezi, sağlanan bir görüntüyü veya kendi işlemlerini (örneğin, yapılandırma yönetimi sistemleri) kullanarak, sanal makinelerde kötü amaçlı yazılımdan koruma yüklediyseniz önceden yüklenmiş kötü amaçlı yazılımdan koruma algılayabilir değil.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Neden ileti "Tarama verileri eksik" Benim VM için sağlarım?
Bir VM için tarama veri olduğunda bu ileti görüntülenir. Süre (bir saatten az) için veri toplamayı Azure Güvenlik Merkezi'nde etkinleştirildikten sonra doldurmak tarama verileri alabilir. Tarama verileri ilk popülasyonunu sonra tarama verisi yok hiç veya son tarama veri olduğundan, bu iletiyi alabilirsiniz. Taramaları durdurulmuş bir durumda, bir VM için doldurmayın. Tarama verileri yakın zamanda (uygun olarak 30 gün varsayılan değeri olan Windows aracısı için bekletme ilkesi) doldurulmuş değil, bu iletiyi de görüntülenebilir.

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi, işletim sistemi güvenlik açıkları, sistem güncelleştirmeleri ve uç nokta koruma sorunları için ne sıklıkta tarama?
Gecikme Güvenlik Merkezi'nde güvenlik açıkları için güncelleştirmeleri tarar ve sorunları alır:

- İşletim sistemi güvenlik yapılandırmalarını – veri 48 saat içinde güncelleştirildi
- Sistem güncelleştirmeleri – veri 24 saat içinde güncelleştirildi
- Endpoint Protection sorunlarını – 8 saat içinde güncelleştirilmiş verileri

Güvenlik Merkezi, genellikle her saat yeni veriler için tarar. Gecikme yukarıdaki son tarama değil veya bir tarama başarısız olduğu en kötü bir durum senaryosu değerlerdir.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>İleti "VM Aracısı eksik mi?" neden alıyorum
Veri toplamayı etkinleştirmek için sanal makinelerin VM Aracısı'nın yüklü olması gerekir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. Diğer Vm'lerde VM Aracısı'nı yükleme hakkında daha fazla bilgi için blog gönderisine bakın [VM aracısı ve Uzantılar](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
