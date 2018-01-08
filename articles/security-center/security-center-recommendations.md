---
title: "Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme | Microsoft Docs"
description: "Bu belge, Azure Güvenlik Merkezi'nde öneriler, Azure kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalın nasıl yardımcı aracılığıyla açıklanmaktadır."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 2cb4a1c944d6893ca7913eef4e93620059f2a839
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme
Bu belge, Azure kaynaklarınızı korumanıza yardımcı olmak için Azure Güvenlik Merkezi'nde öneriler kullanmak nasıl aracılığıyla açıklanmaktadır.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belge hakkında adım adım kılavuz değildir.
>
>

## <a name="what-are-security-recommendations"></a>Güvenlik önerileri nelerdir?
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

## <a name="implementing-security-recommendations"></a>Güvenlik önerilerini uygulama
### <a name="set-recommendations"></a>Ayarlama önerileri
İçinde [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md), getirmeyi öğrenin:

* Güvenlik ilkeleri yapılandırın.
* Veri koleksiyonunu Aç.
* Güvenlik ilkenizin bir parçası olarak görmek için hangi önerilerin seçin.

Geçerli ilke önerileri Merkezi sistem güncelleştirmeleri, temel kurallar, kötü amaçlı yazılımdan koruma programları etrafında [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) alt ağlar ve ağ arabirimleri, SQL veritabanı denetimi, SQL veritabanında saydam veri şifrelemesi web uygulaması güvenlik duvarı ve.  [Güvenlik ilkelerini ayarlama](security-center-policies.md) her öneri seçeneği açıklamasını sağlar.

### <a name="monitor-recommendations"></a>İzleyici önerileri
Bir güvenlik ilkesi tanımladıktan sonra, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak için kaynaklarınızın güvenlik durumunu analiz eder. **Önerileri** altında döşeme **genel bakış** Güvenlik Merkezi tarafından tanımlanan önerileri toplam sayısı bildiğiniz sağlar.

![Öneriler döşeme][1]

Her öneri ayrıntılarını görmek için seçin **önerileri döşeme** altında **genel bakış**. **Öneriler** açar.

![Filtre önerileri][2]

Öneriler her satırın belirli bir öneriyi temsil ettiği bir tablo biçiminde gösterilir. Bu tablonun sütunlarının şunlardır:

* **Açıklama**: öneri ve bunu çözmek için yapılması gerekenler açıklanmaktadır.
* **Kaynak**: Bu öneri uygulandığı kaynakları listeler.
* **Durum**: önerinin geçerli durumu açıklar:
  * **Açık**: öneri henüz ele kurmadı.
  * **Devam eden**: öneri şu anda kaynaklara uygulanıyor ve herhangi bir işlem yapmanız gerekmez.
  * **Çözümlenen**: öneri zaten tamamlandı (Bu durumda, satır gri renkte görüntülenir).
* **ÖNEM DERECESİ**: Belirli bir önerinin önem derecesini açıklar:
  * **Yüksek**: bir güvenlik açığı (örneğin, bir uygulama, bir VM veya bir ağ güvenlik grubu) anlamlı bir kaynakta var ve dikkat gerektiriyor.
  * **Orta**: bir güvenlik açığı var ve kritik olmayan veya ek adımları da ortadan kaldırmak için veya bir işlemin tamamlanması için gerekli.
  * **Düşük**: bir güvenlik açığı bulunduğunu ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulan değil, ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

Aşağıdaki tabloda kullanılabilir öneriler ve onu uygularsanız, her biri yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.

> [!NOTE]
> Anlamak istediğiniz [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
>
>

| Öneri | Açıklama |
| --- | --- |
| [Abonelikler için veri toplamayı etkinleştirin](security-center-enable-data-collection.md) |Her aboneliklerinizi ve tüm Azure sanal makineleri (VM'ler) ve Azure olmayan bilgisayarlar için veri koleksiyonu güvenlik ilkesinde Aç önerir. |
| [Güvenlik yapılandırmalarını Düzelt](security-center-remediate-os-vulnerabilities.md) |Önerilen güvenlik yapılandırma kuralları ile işletim sistemi yapılandırmalarını örneğin hizalamak, parolaların kaydedilmesine izin verme önerir. |
| [Sistem güncelleştirmelerini uygulayın](security-center-apply-system-updates.md) |Windows ve Linux VM'ler ve bilgisayarlar için eksik sistem güvenlik ve kritik güncelleştirmeler dağıtmanızı önerir. |
| [Bir sadece zaman içinde geçerli ağ erişim denetimi](security-center-just-in-time.md) | Yalnızca süresi VM erişimi uygulamanızı önerir. Özellik önizlemededir zaman yalnızca ve Güvenlik Merkezi'nin standart katmanında kullanılabilir. Bkz: [fiyatlandırma](security-center-pricing.md) Güvenlik Merkezi hakkında daha fazla katmanları fiyatlandırma öğrenin. |
| [Sistem güncelleştirmelerinden sonra yeniden başlatın](security-center-apply-system-updates.md#reboot-after-system-updates) |Sistem güncelleştirmelerini uygulama işlemini tamamlamak için VM’yi yeniden başlatmanızı önerir. |
| [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md) |Web uç noktaları için web uygulaması Güvenlik Duvarı (WAF) dağıtmak önerir. WAF öneri açık gelen web bağlantı noktaları (80,443) içeren bir ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP'si için (örnek düzeyinde IP veya yük dengeli IP) gösterilir. </br>Güvenlik Merkezi, uygulama hizmeti ortamı ve sanal makineler üzerinde web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlamasını önerir. Uygulama hizmeti ortamı (ana) olan bir [Premium](https://azure.microsoft.com/pricing/details/app-service/) hizmet planı seçeneği Azure App Service, güvenli bir şekilde Azure App Service uygulamalarını çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar. Ana hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı belgeleri](../app-service/environment/intro.md).</br>Varolan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulamasına Güvenlik Merkezi'nde koruyabilirsiniz. |
| [Uygulama korumasını sonlandırma](security-center-add-web-application-firewall.md#finalize-application-protection) |Bir WAF yapılandırmasını tamamlamak için trafiği WAF Gereci yönlendirilmesi gerekir. Bu öneri aşağıdaki gerekli Kurulum değişiklikleri tamamlar. |
| [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md) |Bir Microsoft iş ortağı güvenlik korumaları artırmak için İleri nesil Güvenlik Duvarı (NGFW) eklemek önerir. |
| [Trafiği yalnızca NGFW aracılığıyla yönlendirme](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Gelen trafik, VM, NGFW üzerinden zorla ağ güvenlik grubu (NSG) kurallarını yapılandırmak önerir. |
| [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md) |VM’lere (yalnızca Windows VM’leri) kötü amaçlı yazılımdan koruma programları sağlamanızı önerir. |
| [Ağ güvenlik grupları alt ağları veya sanal makinelerde etkinleştir](security-center-enable-network-security-groups.md) |Nsg'ler alt ağları veya VM'ler etkinleştirmenizi önerir. |
| [Uç nokta Internet'e aracılığıyla erişimi kısıtlama](security-center-restrict-access-through-internet-facing-endpoints.md) |İçin Nsg'ler gelen trafik kurallarını yapılandırmak önerir. |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |Azure SQL sunucuları için Denetim ve tehdit algılama Aç önerir. (Yalnızca azure SQL Hizmeti. Sanal makinelerde çalışan SQL içermez.) |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |Azure SQL veritabanı için Denetim ve tehdit algılama Aç önerir. (Yalnızca azure SQL Hizmeti. Sanal makinelerde çalışan SQL içermez.) |
| [SQL veritabanlarını saydam veri şifreleme etkinleştir](security-center-enable-transparent-data-encryption.md) |SQL veritabanları için şifrelemeyi etkinleştirmek önerir. (Yalnızca azure SQL Hizmeti.) |
| [VM Aracısını etkinleştirin](security-center-enable-vm-agent.md) |Hangi VM’lerin VM Aracısı gerektirdiğini görmenizi sağlar. VM Aracısı sanal makineler için sağlama düzeltme eki tarama, tarama temel ve kötü amaçlı yazılımdan koruma programları yüklü olması gerekir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir. |
| [Disk şifrelemesi uygulayın](security-center-apply-disk-encryption.md) |Azure Disk Şifrelemesi kullanarak VM’nizi şifrelemenizi önerir (Windows ve Linux VM’leri). Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir. |
| [Güvenlik ilgili kişi bilgilerini belirtin](security-center-provide-security-contact-details.md) |Güvenlik sağlamasını önerir bilgi her aboneliğiniz için başvurun. Bir e-posta adresi ve telefon numarası iletişim bilgileridir. Bilgi güvenliği ekibimiz kaynaklarınızı riske atılması bulursa sizinle iletişim kurmak için kullanılır. |
| [İşletim sistemi sürümünü güncelleştirme](security-center-update-os-version.md) |İşletim sistemi (OS) sürümü kullanılabilir en son sürümüne bulut hizmetiniz için işletim sistemi ailesi için güncelleştirme önerir.  Bulut hizmetleri hakkında daha fazla bilgi için bkz: [bulut hizmetlerine genel bakış](../cloud-services/cloud-services-choose-me.md). |
| [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md) |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| [Güvenlik açıklarını düzeltin](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |VM’nize yüklü güvenlik açığı değerlendirme çözümü tarafından algılanan sistem ve uygulama güvenlik açıklarını görmenizi sağlar. |
| [Azure depolama hesabı için şifrelemeyi etkinleştir](security-center-enable-encryption-for-storage-account.md) | Rest verileri için Azure depolama hizmeti şifrelemesi etkinleştirmenizi önerir. Azure depolama alanına yazılır ve alma önce şifresini çözer verileri şifreleyerek depolama hizmeti şifreleme (SSE) çalışır. SSE yalnızca Azure Blob hizmeti için şu anda kullanılabilir değil ve blok blobları, sayfa bloblarını kullanılabilir ve ekleme blobları. Daha fazla bilgi için bkz: [bekleyen veri için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).</br>SSE yalnızca Resource Manager depolama hesaplarında desteklenir. |

Filtre uygulayabilir ve öneriler yok sayın.

1. Seçin **filtre** üzerinde **önerileri** dikey. **Filtre** dikey penceresi açılır ve görmek istediğiniz önem ve durum değerleri seçin.

2. Bir öneri uygulanamaz olarak belirlerseniz, öneri yok sayın ve görünümünüzü dışında filtre. Bir öneri kapatmanın iki yolu vardır. Tek yönlü bir öğeye sağ tıklayın ve ardından vermektir **atla**. Diğeri, bir öğenin üzerine getirin, sağ tarafta görüntülenen ve ardından üç noktaya tıklayın **atla**. Kapatıldı önerileri tıklatarak görüntüleyebileceğiniz **filtre**ve ardından seçerek **çıkarıldı**.

    ![Öneri yok sayın][3]

### <a name="apply-recommendations"></a>Önerileri geçerlidir
Tüm önerileri gözden geçirdikten sonra bir önce uygulamanız karar verebilirsiniz. Hangi önerileri değerlendirmek için ana parametre önce uygulanması gereken şekilde önem derecesi kullanmanızı öneririz.

Öneriler yukarıdaki tabloda, bir öneri seçin ve bir öneri uygulamak nasıl bir örnek olarak size yol.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik önerilerini giriş yaptınız. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
