---
title: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları | Microsoft Docs
description: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: cbc9fa5586b848de79c52142f75b22b744fdae38
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105959"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Azure Güvenlik Merkezi tarafından izlenen azure güvenlik ilkeleri
Bu makalede Güvenlik Merkezi'ndeki izlenebilir Azure ilke tanımlarının bir listesi sağlanmaktadır. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [güvenlik ilkeleriyle çalışma](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Kullanılabilir güvenlik ilkeleri

Güvenlik Merkezi tarafından izlenen yerleşik ilkeleri anlamak için aşağıdaki tabloya bakın:

| İlke | İlkenin yaptığı |
| --- | --- |
|Service Fabric ve Sanal Makine Ölçek Kümelerinde tanılama günlüklerinin etkinleştirilmesini denetle|Günlükleri etkinleştirmeniz önerilir, böylece bir olay veya gizlilik bozulması durumunda araştırma gerekirse etkinlik kaydı yeniden oluşturulabilir.|
|Olay Hub'ı ad alanlarında yetkilendirme kurallarını denetleyin|Olay hub'ı istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık ile hizalamak için güvenlik modeli oluşturmanız gerektiğini erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|Olay Hub'ı varlıklarında yetkilendirme kurallarının varlığını denetleyin|Olay hub'ı varlıklar, en az ayrıcalıklı erişim vermek için yetkilendirme kurallarının var olup olmadığını denetleyin.|
|Depolama hesaplarına kısıtlanmamış ağ erişimini denetleyin|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. Bunun yerine, yalnızca izin verilen ağları uygulamalardan depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli Internet'ten bağlantılara izin vermek ya da şirket içi istemciler için erişim belirli bir Azure sanal ağları gelen trafiği veya genel Internet IP adresi aralıkları için verilebilir.|
|Özel RBAC kurallarının kullanımını denetleyin|Hataya açık alanlardır özel RBAC rolleri yerine 'sahibi, katkıda bulunan, okuyucu' gibi yerleşik roller denetim. Özel rolleri kullanarak özel bir durum olarak kabul edilir ve sıkı gözden geçirme ve tehdit modellemesi gerektirir.|
|Azure Stream Analytics'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.|
|Depolama hesaplarına güvenli aktarımı denetleyin|Denetim gereksinimi güvenli aktarım depolama hesabınızda. Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|SQL sunucusuna Azure Active Directory Yöneticisi sağlanmasını denetleyin.|Azure AD kimlik doğrulamasını etkinleştirmek için bir Azure Active Directory Yöneticisi, SQL server için sağlama denetim. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimi veritabanı kullanıcıları ve diğer Microsoft hizmetleri sağlar.|
|Service Bus ad alanlarında yetkilendirme kurallarını denetleyin|Hizmet veri yolu istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık ile hizalamak için güvenlik modeli oluşturmanız gerektiğini erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde.|
|Service Bus'ta tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.|
|Service Fabric'te ClusterProtectionLevel özelliğinin EncryptAndSign olarak ayarlanmasını denetleyin|Service Fabric, düğümden düğüme iletişim için bir birincil küme sertifikası kullanarak koruma (None, oturum ve EncryptAndSign) üç düzeyleri sağlar. Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın.| 
|Service Fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleyin|Service Fabric'te yalnızca Azure Active Directory yoluyla istemci kimlik doğrulaması kullanımını denetleyin| 
|Arama hizmeti için tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Yalnızca güvenli bağlantılar, Azure önbelleği için Redis etkinleştirme denetleme|Azure Cache için SSL aracılığıyla yalnızca bağlantıları için Redis etkinleştirme denetim. Güvenli bağlantı kullanımı hizmeti sunucusu arasındaki kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.| 
|Logic Apps'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Key Vault'ta tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.|
|Olay Hub'ında tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu bir güvenlik olayı ortaya veya ağınızın tehlikeye araştırma amacıyla etkinlik kayıtlarını mpgo.exe'nin sağlar.| 
|Azure Data Lake Store'da tanılama günlüklerinin etkinleştirilmesini denetle|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Data Lake Analytics'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Klasik depolama hesaplarının kullanımını denetleyin|Azure Resource Manager depolama hesaplarınız için gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlik, anahtar kasasına erişim gizli dizileri için daha güçlü erişim denetimi (RBAC), daha iyi denetim, Azure Resource Manager tabanlı dağıtım ve yönetim erişim , Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları.| 
|Klasik sanal makinelerin kullanımını denetleyin|Azure Resource Manager sanal makineleriniz için gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlik, anahtar kasasına erişim gizli dizileri için daha güçlü erişim denetimi (RBAC), daha iyi denetim, Azure Resource Manager tabanlı dağıtım ve yönetim erişim , Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları.| 
|Batch hesaplarında ölçüm uyarısı kurallarının yapılandırılmasını denetleyin|Ölçüm uyarı kuralları gerekli ölçüm etkinleştirmek için Batch hesabındaki yapılandırmasını denetleyin.| 
|Batch hesaplarında ölçüm uyarısı kurallarının yapılandırılmasını denetleyin|Ölçüm uyarı kuralları gerekli ölçüm etkinleştirmek için Batch hesabındaki yapılandırmasını denetleyin.| 
|Batch hesaplarında tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Otomasyon hesabı değişkenlerinin şifrelemesinin etkinleştirilmesini denetleyin|Gizli veriler depolanırken Otomasyon hesabı değişken varlıklarının şifrelemesini etkinleştirmek önemlidir.| 
|Uygulama Hizmetleri'nde tanılama günlüklerinin etkinleştirilmesini denetleyin|Uygulama tanılama günlüklerini etkinleştirme denetim. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar.| 
|Saydam veri şifreleme durumunu denetle|SQL veritabanları için saydam veri şifreleme durumunu denetle.| 
|SQL sunucu düzeyi Denetim ayarlarını denetle|SQL denetimi sunucu düzeyinde varlığını denetler.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle|Şifrelenmemiş SQL sunucuları veya veritabanları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme|SQL denetimi açık olmayan SQL sunucuları ve veritabanları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme|Eksik güvenlik sistem güncelleştirmeleri sunucularınızda Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Depolama hesapları için eksik blob şifrelemesini denetler|Bu ilke blob şifrelemesi olmayan depolama hesaplarını denetler. Yalnızca Microsoft.Storage kaynak türlerine uygulanır, diğer depolama sağlayıcılarına uygulanmaz de geçerlidir. Olası ağ tam zamanında erişim, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme|Olası ağ erişimini yalnızca zamanında (JIT), Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme|Olası uygulama beyaz listesi yapılandırması Azure Güvenlik Merkezi tarafından izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde esnek ağın genelini erişimi izleme|Çok esnek kurallara sahip ağ güvenlik grupları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izleyin|Yapılandırılmış temeli karşılamayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme|Yüklü bir Endpoint Protection Aracısı olmayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş VM disklerini izleyin|Etkinleştirilmiş bir disk şifrelemesi olmayan VM'ler Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde VM Güvenlik Açıklarını İzleme|Güvenlik Açığı Değerlendirme çözümü tarafından algılanan güvenlik açıklarını ve bir Güvenlik Açığı Değerlendirme çözümü olmayan VM'leri, Azure Güvenlik Merkezi'nde öneriler olarak izler.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme|Bir Web uygulaması güvenlik duvarı koruması olmayan Web uygulamaları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izleme|Bir sonraki nesil güvenlik duvarı koruması olmayan ağ uç noktaları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: SQL güvenlik açığı değerlendirme sonuçları Azure Güvenlik Merkezi'nde izleme|Güvenlik Açığı Değerlendirmesi tarama sonuçlarını ve veritabanı güvenlik açıklarını nasıl düzelteceğiniz hakkındaki önerileri izleyin.| 
|[Önizleme]: Denetim sahip bir abonelik için en yüksek sayısı|Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için en fazla 3 abonelik sahibi belirlemeniz önerilir.| 
|[Önizleme]: Denetim sahip abonelik için en düşük sayısı|Yönetici erişimi fazlalığı sağlamak için birden fazla abonelik sahibi atamanız önerilir.| 
|[Önizleme]: Mfa'yı bir abonelikte etkin olmayan sahip izinleri ile hesapları denetleme|Hesap veya kaynak ihlalini önlemek amacıyla, sahip izinleri olan tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir.| 
|[Önizleme]: Mfa'yı bir abonelikte etkin olmayan yazma izinleri ile hesapları denetleme|Hesap veya kaynak ihlalini önlemek amacıyla, yazma ayrıcalıkları olan tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir.| 
|[Önizleme]: Mfa'yı bir abonelikte etkin olmayan okuma izinleri olan hesaplar denetleme|Hesap veya kaynak ihlalini önlemek için, Multi-Factor Authentication (MFA) okuma ayrıcalıkları olan tüm abonelik hesapları için etkinleştirilmelidir.| 
|[Önizleme]: Bir abonelikte sahip izinleri ile kullanım dışı bırakılmış hesapları denetleme|Sahip izinleri ile kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları açmasını engelleyen hesaplarıdır.| 
|[Önizleme]: Bir Abonelikteki kullanım dışı bırakılmış hesapları denetleme|Kullanım dışı bırakılmış hesapların aboneliklerinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları açmasını engelleyen hesaplarıdır.| 
|[Önizleme]: Bir abonelikte sahip izinleri olan dış hesapları denetleme|Sahip izinleri olan dış hesaplar, izlenmeyen erişimi engellemek için aboneliğinizden kaldırılmalıdır.| 
|[Önizleme]: Bir abonelikte yazma izinleri olan dış hesapları denetleme|İzlenmeyen erişimi engellemek için yazma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekiyor.| 
|[Önizleme]: Bir Abonelikteki Okuma izinleri olan dış hesapları denetleme|İzlenmeyen erişimi engellemek için okuma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekiyor.| 




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md): Planlama ve tasarım konuları Azure Güvenlik Merkezi hakkında anlama hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
