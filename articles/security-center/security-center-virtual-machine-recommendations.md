---
title: Azure Güvenlik Merkezi'nde sanal makine önerileri | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerini, sanal makineler ve bilgisayarlar ve web uygulamaları ve App Service ortamları korunmasına nasıl yardımcı olacağını açıklar.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2018
ms.author: rkarlin
ms.openlocfilehash: a4aaf440856746895a31914aeee2bddec2ce23f6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60545013"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>Azure Güvenlik Merkezi kaynak önerilerini anlama


## <a name="recommendations"></a>Öneriler
Aşağıdaki tablolar kullanılabilir işlem anlamanıza yardımcı olması için referans olarak kullanın ve uygulama önerileri ve uygulamanız durumunda her birinin ne yaptığını Hizmetleri.

### <a name="computers"></a>Bilgisayarlar
| Öneri | Açıklama |
| --- | --- |
| [Abonelikler için veri toplamayı etkinleştirin](security-center-enable-data-collection.md) |Her bir aboneliğiniz ve aboneliklerinizdeki tüm sanal makinelerine (VM) için güvenlik ilkesinde veri toplamayı etkinleştirmenizi önerir. |
| [Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) | Bekleyen veri için Azure depolama hizmeti şifrelemesi etkinleştirmenizi önerir. Azure depolama alanına yazılır ve alma önce şifrelerini çözer, verileri şifreleyerek depolama hizmeti şifrelemesi (SSE) çalışır. SSE, şu anda yalnızca Azure Blob hizmeti için kullanılabilir ve blok blobları, sayfa blobları için kullanılabilir ve ekleme blobları. Daha fazla bilgi için bkz. [bekleyen veriler için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).</br>SSE, yalnızca Resource Manager depolama hesaplarında desteklenir. Klasik depolama hesapları şu anda desteklenmemektedir. Klasik ve Resource Manager dağıtım modelleri anlamak için bkz [Azure dağıtım modelleri](../azure-classic-rm.md). |
| [Güvenlik yapılandırmalarını düzeltme](security-center-remediate-os-vulnerabilities.md) |İşletim sistemi yapılandırmalarınızı önerilen güvenlik yapılandırması kurallarını hizalama önerir örn izin verme Parolaların kaydedilmesine izin. |
| [Sistem güncelleştirmelerini uygulayın](security-center-apply-system-updates.md) |VM’lerinize eksik sistem güvenliği güncelleştirmelerini ve kritik güncelleştirmeleri dağıtmanızı önerir. |
| [Just-ın-Time geçerli ağ erişim denetimi](security-center-just-in-time.md) | Tam zamanında VM erişimini uygulanmasını önerir. Yalnızca zaman özellik önizlemededir ve Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). |
| [Sistem güncelleştirmelerinden sonra yeniden başlatın](security-center-apply-system-updates.md#reboot-after-system-updates) |Sistem güncelleştirmelerini uygulama işlemini tamamlamak için VM’yi yeniden başlatmanızı önerir. |
| [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md) |VM’lere (yalnızca Windows VM’leri) kötü amaçlı yazılımdan koruma programları sağlamanızı önerir. |
| [VM Aracısını etkinleştirin](security-center-enable-vm-agent.md) |Hangi VM’lerin VM Aracısı gerektirdiğini görmenizi sağlar. Düzeltme eki tarama, temel tarama ve kötü amaçlı yazılımdan koruma programları sağlamak üzere VM’lere VM Aracısı yüklenmelidir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir. |
| [Disk şifrelemesi uygulayın](security-center-apply-disk-encryption.md) |Azure Disk Şifrelemesi kullanarak VM’nizi şifrelemenizi önerir (Windows ve Linux VM’leri). Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir. |
| [İşletim sistemi sürümünü güncelleştirme](security-center-update-os-version.md) |İşletim sistemi (OS) sürümünü bulut hizmetinize kullanılabilir en son sürümü için işletim sistemi ailesi için güncelleştirmeniz önerir.  Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Cloud Services'e genel bakış](../cloud-services/cloud-services-choose-me.md). |
| [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md) |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| [Güvenlik açıklarını düzeltin](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |VM’nize yüklü güvenlik açığı değerlendirme çözümü tarafından algılanan sistem ve uygulama güvenlik açıklarını görmenizi sağlar. |

### Uygulama Hizmetleri <a name="app-services"></a>
| Öneri | Açıklama |
| --- | --- |
| App Service yalnızca HTTPS üzerinden erişilebilir olmalıdır | App Service'in erişim HTTPS üzerinden sınırı, yalnızca, önerir. |
| Web uygulaması için Web yuvalarını devre dışı bırakılmalıdır| Web uygulamaları içinde Web yuvaları kullanımını dikkatle gözden geçirin önerir.  Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır. |
| Web uygulamanız için özel etki alanları kullanın | Bir web uygulaması kimlik avı gibi genel saldırılara ve DNS ile ilgili diğer saldırılara karşı korumak için özel etki alanları kullanmanızı önerir. |
| Web uygulaması için IP kısıtlamalarını yapılandırma | Uygulamanıza erişmek için izin verilen IP adreslerinin bir listesi tanımladığınız önerir.  IP kısıtlamaları kullanımı, bir web uygulaması genel saldırılara karşı korur. |
| Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar | WEBSITE_LOAD_CERTIFICATES parametre ayarladığınız değil önerir ' *'. Parametre ayarı '* ' tüm sertifikalar, web uygulamaları kişisel sertifika deposuna yüklenecektir anlamına gelir.  Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir. |
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


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
