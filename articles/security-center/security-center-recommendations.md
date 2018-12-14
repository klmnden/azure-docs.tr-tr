---
title: Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi'nde öneriler, Azure kaynaklarınızı korumanıza ve güvenlik ilkelerine uygun kalın nasıl yardımcı aracılığıyla size yol gösterir.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rkarlin
ms.openlocfilehash: d0c61f6e905ca109f3f178996a08f353c36e7880
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337215"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme
Bu belge, Azure kaynaklarınızı korumanıza yardımcı olması için Azure Güvenlik Merkezi'nde öneriler kullanma hakkında bilgi vermektedir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belgede, adım adım bir kılavuz değildir.
>
>

## <a name="what-are-security-recommendations"></a>Güvenlik önerilerini nelerdir?
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

## <a name="implementing-security-recommendations"></a>Güvenlik önerilerini uygulama
### <a name="set-recommendations"></a>Ayarlama önerileri
İçinde [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md), şunların nasıl yapıldığını öğrenirsiniz:

* Güvenlik ilkeleri yapılandırın.
* Veri toplamayı etkinleştir.
* Güvenlik ilkenizin bir parçası olarak görmek için hangi önerilerin seçin.

Geçerli ilke önerileri Merkezi sistem güncelleştirmeleri, temel kurallar, kötü amaçlı yazılımdan koruma programları etrafında [ağ güvenlik grupları](../virtual-network/security-overview.md) alt ağları ve ağ arabirimleri, SQL veritabanı denetimi, SQL veritabanı saydam veri şifrelemesi, ve web uygulaması güvenlik duvarları.  [Güvenlik ilkelerini ayarlama](tutorial-security-policy.md) her öneri seçeneği açıklamasını sağlar.

### <a name="monitor-recommendations"></a>İzleme önerileri
Bir güvenlik ilkesi tanımladıktan sonra, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak için kaynaklarınızın güvenlik durumunu analiz eder. **Önerileri** altında kutucuğuna **genel bakış** Güvenlik Merkezi tarafından tanımlanan öneriler toplam sayısı bildiğiniz sağlar.

![Öneriler kutucuğu][1]

Her önerinin ayrıntıları görmek için seçin **önerileri kutucuğuna** altında **genel bakış**. **Öneriler** açılır.

![Filtre önerileri][2]

Öneriler filtreleyebilirsiniz. Öneriler filtre uygulamak için seçin **filtre** üzerinde **önerileri** dikey penceresi. **Filtre** dikey penceresi açılır ve görmek istediğiniz önem ve durum değerleri seçin.

Öneriler her satırın belirli bir öneriyi temsil ettiği bir tablo biçiminde gösterilir. Bu tablonun sütunlarının şunlardır:

* **AÇIKLAMA**: Öneri ve çözmek için yapılması gerekenler açıklanmaktadır.
* **KAYNAK**: Bu önerinin geçerli olduğu kaynakları listeler.
* **DURUM**: Önerinin geçerli durumu açıklar:
  * **Açık**: Öneri henüz ele edilmemiş.
  * **Devam eden**: Kaynakları şu anda öneri uygulanıyor ve herhangi bir işlem yapmanıza gerek yoktur.
  * **Çözümlenen**: Öneri zaten tamamlandı (Bu durumda, çizgi gri renkte görüntülenir).
* **ÖNEM DERECESİ**: Belirli bir önerinin önem açıklanmaktadır:
  * **Yüksek**: Bir güvenlik açığı anlamlı bir kaynakta (örneğin, bir uygulama, bir VM veya ağ güvenlik grubu) ile var ve ilgilenilmesi gerekiyor.
  * **Orta**: Bir güvenlik açığı var ve bunu ortadan kaldırmak için veya bir işlemi tamamlamak için kritik olmayan veya ek adımlar gereklidir.
  * **Düşük**: Mevcut bir güvenlik açığı olan ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler almazsınız ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

Aşağıdaki tabloda kullanılabilir öneriler ve uygulamanız durumunda her birinin yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.

> [!NOTE]
> Öğrenmek istediğiniz [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
>
>

| Öneri | Açıklama |
| --- | --- |
| [Abonelikler için veri toplamayı etkinleştirin](security-center-enable-data-collection.md) |Her abonelik sayınıza ve tüm Azure sanal makineleri (VM'ler) ve Azure olmayan bilgisayarlar için güvenlik ilkesinde veri toplamayı kapatma önerir. |
| [Güvenlik yapılandırmalarını düzeltme](security-center-remediate-os-vulnerabilities.md) |İşletim sistemi yapılandırmalarınızı önerilen güvenlik yapılandırması kurallarını, örneğin hizalama, parolaların kaydedilmesine izin verme önerir. |
| [Sistem güncelleştirmelerini uygulayın](security-center-apply-system-updates.md) |Windows ve Linux sanal makineleri ve bilgisayarlar için eksik sistem güvenliği güncelleştirmelerini ve kritik güncelleştirmeleri dağıtmanızı önerir. |
| [Just-ın-Time geçerli ağ erişim denetimi](security-center-just-in-time.md) | Tam zamanında VM erişimini uygulanmasını önerir. Tam zamanında özelliği Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). |
| [Sistem güncelleştirmelerinden sonra yeniden başlatın](security-center-apply-system-updates.md#reboot-after-system-updates) |Sistem güncelleştirmelerini uygulama işlemini tamamlamak için VM’yi yeniden başlatmanızı önerir. |
| [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md) |Web uç noktaları için web uygulaması Güvenlik Duvarı (WAF) dağıtmanızı önerir. Bir WAF öneri, bir giden açık web bağlantı noktaları (80,443) ile ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP için (örnek düzeyi IP veya yük dengeli IP) gösterilir. </br>Güvenlik Merkezi, sanal makinelerde ve App Service ortamında web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlamanızı önerir. App Service ortamı (ASE) olan bir [Premium](https://azure.microsoft.com/pricing/details/app-service/) hizmet planı seçeneği Azure App Service, Azure App Service uygulamalarını güvenli olarak çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar. ASE hakkında daha fazla bilgi için bkz: [App Service ortamı belgeleri](../app-service/environment/intro.md).</br>Var olan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulaması Güvenlik Merkezi'nde koruyabilirsiniz. |
| [Uygulama korumasını sonlandırma](security-center-add-web-application-firewall.md#finalize-application-protection) |Bir WAF yapılandırmasını tamamlamak için WAF gerecine trafiği gelmesi gerekir. Bu öneriyi şu gerekli Kurulum değişiklikleri tamamlar. |
| [Yeni Nesil Güvenlik Duvarı ekleme](security-center-add-next-generation-firewall.md) |Bir Microsoft iş ortağından, güvenlik korumaları artırmak için bir sonraki Generation Firewall (NGFW) eklemenizi önerir. |
| [Trafiği yalnızca NGFW aracılığıyla yönlendirme](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Önerir, NGFW aracılığıyla sanal makinenize gelen trafiği zorlamak ağ güvenlik grubu (NSG) kurallarını yapılandırın. |
| [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md) |VM’lere (yalnızca Windows VM’leri) kötü amaçlı yazılımdan koruma programları sağlamanızı önerir. |
| [Alt ağlar veya sanal makinelerde ağ güvenlik gruplarını etkinleştirme](security-center-enable-network-security-groups.md) |Nsg'leri alt ağlara veya sanal makineleri üzerinde etkinleştirmenizi önerir. |
| [Internet'e yönelik uç noktalarla erişimi sınırlama](security-center-restrict-access-through-internet-facing-endpoints.md) |Nsg'ler için gelen trafik kuralları yapılandırma önerir. |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |Azure SQL sunucuları için Denetim ve tehdit algılamayı etkinleştirmek önerir. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.) |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |Azure SQL veritabanı için Denetim ve tehdit algılamayı etkinleştirmek önerir. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.) |
| [SQL veritabanlarında saydam veri şifrelemesini etkinleştirme](security-center-enable-transparent-data-encryption.md) |SQL veritabanları için şifreleme etkinleştirmenizi önerir. (Yalnızca azure SQL Hizmeti.) |
| [VM Aracısını etkinleştirin](security-center-enable-vm-agent.md) |Hangi VM’lerin VM Aracısı gerektirdiğini görmenizi sağlar. VM Aracısı sağlama düzeltme eki tarama, temel tarama ve kötü amaçlı yazılımdan koruma programları Vm'lerde yüklü olmalıdır. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir. |
| [Disk şifrelemesi uygulayın](security-center-apply-disk-encryption.md) |Azure Disk Şifrelemesi kullanarak VM’nizi şifrelemenizi önerir (Windows ve Linux VM’leri). Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir. |
| [Güvenlik ilgili kişi bilgilerini belirtin](security-center-provide-security-contact-details.md) |Güvenlik sağlamanızı önerir bilgileri her bir aboneliğinizde başvurun. Bir e-posta adresi ve telefon numarası iletişim bilgileridir. Bilgileri güvenlik ekibimizin kaynaklarınızın tehlikede olduğunu tespit olursa sizinle iletişim kurmak için kullanılır. |
| [İşletim sistemi sürümünü güncelleştirme](security-center-update-os-version.md) |İşletim sistemi (OS) sürümünü bulut hizmetinize kullanılabilir en son sürümü için işletim sistemi ailesi için güncelleştirmeniz önerir.  Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Cloud Services'e genel bakış](../cloud-services/cloud-services-choose-me.md). |
| [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md) |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| [Güvenlik açıklarını düzeltin](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |VM’nize yüklü güvenlik açığı değerlendirme çözümü tarafından algılanan sistem ve uygulama güvenlik açıklarını görmenizi sağlar. |
| [Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) | Bekleyen veri için Azure depolama hizmeti şifrelemesi etkinleştirmenizi önerir. Azure depolama alanına yazılır ve alma önce şifrelerini çözer, verileri şifreleyerek depolama hizmeti şifrelemesi (SSE) çalışır. SSE, şu anda yalnızca Azure Blob hizmeti için kullanılabilir ve blok blobları, sayfa blobları için kullanılabilir ve ekleme blobları. Daha fazla bilgi için bkz. [bekleyen veriler için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).</br>SSE, yalnızca Resource Manager depolama hesaplarında desteklenir. |
| [Uyarlamalı uygulama denetimlerini etkinleştir](security-center-adaptive-application.md) | Uyarlamalı uygulama denetimleri, Windows vm'lerinde uygulanmasını önerir. Bu özellik, Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). |
| App Service yalnızca HTTPS üzerinden erişilebilir olmalıdır | App Service'in erişim HTTPS üzerinden sınırı, yalnızca, önerir. |
| Web uygulaması için Web yuvalarını devre dışı bırakılmalıdır| Web uygulamaları içinde Web yuvaları kullanımını dikkatle gözden geçirin önerir.  Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır. |
| Web uygulamanız için özel etki alanları kullanın | Bir web uygulaması kimlik avı gibi genel saldırılara ve DNS ile ilgili diğer saldırılara karşı korumak için özel etki alanları kullanmanızı önerir. |
| Web uygulaması için IP kısıtlamalarını yapılandırma | Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesi tanımladığınız önerir.  IP kısıtlamaları kullanımı, bir web uygulaması genel saldırılara karşı korur. |
| Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar | WEBSITE_LOAD_CERTIFICATES parametre ayarladığınız değil önerir '*'. Parametre ayarı '*' tüm sertifikalar, web uygulamaları kişisel sertifika deposuna yüklenecektir anlamına gelir.  Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir. |
| CORS her kaynağın uygulamanıza erişmesine izin vermemelidir | Web uygulamanızla etkileşim kurmak yalnızca gerekli etki alanlarına izin önerir. Kaynak kaynak paylaşımı (CORS) tüm etki alanları web uygulamanıza erişmek izin vermemelisiniz. |
| Kullanım en son .NET Framework Web uygulaması için desteklenir. | En son .NET Framework sürümü için en son güvenlik sınıflarını kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| Web uygulaması için desteklenen en son Java sürümünü kullanın | En son güvenlik sınıfları için en son Java sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| Web uygulaması için desteklenen en son PHP sürümünü kullanın | En son güvenlik sınıfları için en son PHP sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md) |Web uç noktaları için web uygulaması Güvenlik Duvarı (WAF) dağıtmanızı önerir. Bir WAF öneri, bir giden açık web bağlantı noktaları (80,443) ile ilişkili ağ güvenlik grubu olan tüm genel kullanıma yönelik IP için (örnek düzeyi IP veya yük dengeli IP) gösterilir.</br></br>Güvenlik Merkezi, sanal makinelerde ve App Service ortamında web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir WAF sağlamanızı önerir. App Service ortamı (ASE) olan bir [Premium](https://azure.microsoft.com/pricing/details/app-service/) hizmet planı seçeneği Azure App Service, Azure App Service uygulamalarını güvenli olarak çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar. ASE hakkında daha fazla bilgi için bkz: [App Service ortamı belgeleri](../app-service/environment/intro.md).</br></br>Var olan WAF dağıtımlarınız için bu uygulamaları ekleyerek, birden çok web uygulaması Güvenlik Merkezi'nde koruyabilirsiniz. |
| [Uygulama korumasını sonlandırma](security-center-add-web-application-firewall.md#finalize-application-protection) |Bir WAF yapılandırmasını tamamlamak için WAF gerecine trafiği gelmesi gerekir. Bu öneriyi şu gerekli Kurulum değişiklikleri tamamlar. |
| Web uygulaması için desteklenen en son Node.js sürümünü kullanın | En son güvenlik sınıfları için en son Node.js sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir | Web uygulamanızla etkileşim kurmak yalnızca gerekli etki alanlarına izin önerir. Kaynak kaynak paylaşımı (CORS) tüm etki alanlarının işlev uygulamanıza erişmek izin vermemelisiniz. |
| İşlev uygulaması için özel etki alanları kullanın | Bir işlev uygulaması, kimlik avı gibi genel saldırılara ve DNS ile ilgili diğer saldırılara karşı korumak için özel etki alanları kullanmanızı önerir. |
| İşlev uygulaması için IP kısıtlamalarını yapılandırma | Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesi tanımladığınız önerir. IP kısıtlamaları kullanımını bir işlev uygulaması genel saldırılara karşı korur. |
| İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır | İşlev uygulamaları, erişim HTTPS üzerinden sınırı, yalnızca, önerir. |
| Uzaktan hata ayıklama için işlev uygulaması kapatılmalıdır | Bunu kullanmak artık ihtiyacınız yoksa işlev uygulaması için hata ayıklamasını kapatma önerir. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir. |
| İşlev uygulaması için Web yuvalarını devre dışı bırakılmalıdır | İşlev uygulamaları içinde Web yuvaları kullanımını dikkatle gözden geçirin önerir. Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır. |
| Aboneliğinizde birden çok sahip belirleyin | Yönetici erişimi fazlalığı sağlamak için birden fazla abonelik sahibi belirlemeniz önerir. |
| Aboneliğinizde en fazla 3 sahipleri belirleyin | Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için 3'ten az abonelik sahipleri belirlemek önerir. |
| Aboneliğinizi sahip izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için yönetici ayrıcalıkları olan tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Aboneliğinizde yazma izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için yazma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Aboneliğinizde Okuma izinleri olan hesaplar için mfa'yı etkinleştirme | Hesapları veya kaynak ihlalini önlemek için okuma ayrıcalıklarına sahip tüm abonelik hesapları için multi-Factor Authentication (MFA) etkinleştirmenizi önerir. |
| Okuma izinleri olan dış hesapları aboneliğinizden kaldırın | İzlenmeyen erişimi engellemek için aboneliğinizden okuma ayrıcalıklarına sahip dış hesapların kaldırmanızı önerir. |
| Yazma izinleri olan dış hesapları aboneliğinizden kaldırın | İzlenmeyen erişimi engellemek için aboneliğinizden yazma ayrıcalıklarına sahip dış hesapların kaldırmanızı önerir. |
| Sahip izinleri olan dış hesapları aboneliğinizden kaldırın | Sahibi izinleri olan dış hesapları aboneliğinizden izlenmeyen erişimi engellemek için kaldırmanızı önerir. |
| Kullanım dışı bırakılmış hesapları abonelikten Kaldır | Önerir kaldırdığınız hesapları aboneliklerinizden kullanım dışı. |
| Sahip izinleri ile kullanım dışı bırakılmış hesapları abonelikten Kaldır | Önerir kaldırdığınız aboneliklerinizden sahip izinleri ile hesapları kullanım dışı. |

### <a name="apply-recommendations"></a>Önerileri uygulama
Tüm önerileri gözden geçirdikten sonra bir önce uygulamanız karar verebilirsiniz. Hangi önerileri değerlendirmek için ana parametresi ilk uygulanması gereken şekilde önem derecesi kullanmanızı öneririz.

Öneriler yukarıdaki tabloda, bir öneri seçin ve bir öneri uygulamak nasıl bir örnek olarak yol.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik önerilerini yaptınız. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
