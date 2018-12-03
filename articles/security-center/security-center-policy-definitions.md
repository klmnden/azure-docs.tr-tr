---
title: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları | Microsoft Docs
description: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: rkarlin
ms.openlocfilehash: a4f9fc31f411d36e63775a3665b6dfe27eec7710
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319382"
---
# <a name="azure-security-policies-monitored-by-azure-security-center"></a>Azure Güvenlik Merkezi tarafından izlenen azure güvenlik ilkeleri
Bu makalede Güvenlik Merkezi'ndeki izlenebilir Azure ilke tanımlarının bir listesi sağlanmaktadır.

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
|Redis Cache önbelleğinizde yalnızca güvenli bağlantıların etkinleştirilmesini denetleyin|Yalnızca Redis cache'e SSL aracılığıyla bağlantıları etkinleştirme denetim. Güvenli bağlantı kullanımı hizmeti sunucusu arasındaki kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.| 
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
|[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını izle|Şifrelenmemiş SQL sunucuları veya veritabanları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izle|SQL denetimi açık olmayan SQL sunucuları ve veritabanları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izle|Eksik güvenlik sistem güncelleştirmeleri sunucularınızda Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Depolama hesapları için eksik blob şifrelemesini denetle|Bu ilke blob şifrelemesi olmayan depolama hesaplarını denetler. Yalnızca Microsoft.Storage kaynak türlerine uygulanır, diğer depolama sağlayıcılarına uygulanmaz de geçerlidir. Olası ağ tam zamanında erişim, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde olası ağ Tam Zamanında (JIT) erişimini izleyin|Olası ağ erişimini yalnızca zamanında (JIT), Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde olası uygulama Güvenilenler Listesine Alma işlemini izle|Olası uygulama beyaz listesi yapılandırması Azure Güvenlik Merkezi tarafından izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde güvenliği esnek ağ erişimini izle|Çok esnek kurallara sahip ağ güvenlik grupları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izle|Yapılandırılmış temeli karşılamayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izle|Yüklü bir Endpoint Protection Aracısı olmayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş VM Disklerini izle|Etkinleştirilmiş bir disk şifrelemesi olmayan VM'ler Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde VM Güvenlik Açıklarını İzle|Güvenlik Açığı Değerlendirme çözümü tarafından algılanan güvenlik açıklarını ve bir Güvenlik Açığı Değerlendirme çözümü olmayan VM'leri, Azure Güvenlik Merkezi'nde öneriler olarak izler.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde korumasız web uygulamasını izle|Bir Web uygulaması güvenlik duvarı koruması olmayan Web uygulamaları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izle|Bir sonraki nesil güvenlik duvarı koruması olmayan ağ uç noktaları, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir.| 
|[Önizleme]: Azure Güvenlik Merkezi'nde SQL güvenlik açığı değerlendirmesi sonuçlarını izleyin|Güvenlik Açığı Değerlendirmesi tarama sonuçlarını ve veritabanı güvenlik açıklarını nasıl düzelteceğiniz hakkındaki önerileri izleyin.| 
|[Önizleme]: Bir aboneliğin en yüksek sahip sayısını denetle|Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için en fazla 3 abonelik sahibi belirlemeniz önerilir.| 
|[Önizleme]: Bir aboneliğin en düşük sahip sayısını denetle|Yönetici erişimi fazlalığı sağlamak için birden fazla abonelik sahibi atamanız önerilir.| 
|[Önizleme]: Bir abonelikteki sahip izinleri olan MFA için etkinleştirilmemiş hesapları denetle|Hesap veya kaynak ihlalini önlemek amacıyla, sahip izinleri olan tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir.| 
|[Önizleme]: Bir abonelikteki yazma izinleri olan MFA için etkinleştirilmemiş hesapları denetle|Hesap veya kaynak ihlalini önlemek amacıyla, yazma ayrıcalıkları olan tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir.| 
|[Önizleme]: Bir abonelikteki okuma izinleri olan MFA için etkinleştirilmemiş hesapları denetle|Hesap veya kaynak ihlalini önlemek için, Multi-Factor Authentication (MFA) okuma ayrıcalıkları olan tüm abonelik hesapları için etkinleştirilmelidir.| 
|[Önizleme]: Bir abonelikteki sahip izinleri olan kullanım dışı hesapları denetle|Sahip izinleri ile kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları açmasını engelleyen hesaplarıdır.| 
|[Önizleme]: Bir abonelikteki kullanım dışı hesapları denetle|Kullanım dışı bırakılmış hesapların aboneliklerinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları açmasını engelleyen hesaplarıdır.| 
|[Önizleme]: Bir abonelikteki sahip izinleri olan dış hesapları denetle|Sahip izinleri olan dış hesaplar, izlenmeyen erişimi engellemek için aboneliğinizden kaldırılmalıdır.| 
|[Önizleme]: Bir abonelikteki yazma izinleri olan dış hesapları denetle|İzlenmeyen erişimi engellemek için yazma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekiyor.| 
|[Önizleme]: Bir abonelikteki okuma izinleri olan dış hesapları denetle|İzlenmeyen erişimi engellemek için okuma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekiyor.| 




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
