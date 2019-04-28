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
ms.openlocfilehash: 9d9369afd36f64c27cd2222cab0de5912aa913de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60909205"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Güvenlik Merkezi tarafından izlenen azure güvenlik ilkeleri
Bu makalede, Azure Güvenlik Merkezi'nde izleyebilirsiniz Azure ilke tanımlarını listesini sağlar. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [güvenlik ilkeleriyle çalışma](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Kullanılabilir güvenlik ilkeleri

Güvenlik Merkezi tarafından izlenen yerleşik ilkeleri hakkında bilgi edinmek için aşağıdaki tabloya bakın:

| İlke | İlkenin yaptığı |
| --- | --- |
|Azure Service Fabric ve sanal makine ölçek kümeleri günlükleri, Denetim tanılamasını etkinleştirme|Böylece bir etkinlik kaydı araştırma bir olay ya da güvenliğinin aşılması için kullanılabilir günlükleri etkinleştirmenizi öneririz.|
|Event Hubs ad alanlarını denetim yetkilendirme kuralları|Azure Event Hubs istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalıklı güvenlik modeli ile hizalamak için kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|Event Hubs varlıklardaki yetkilendirme kuralları varlığını denetle|Event Hubs varlıklar, en az ayrıcalıklı erişim vermek için yetkilendirme kurallarının var olup olmadığını denetleyin.|
|Depolama hesaplarına kısıtlanmamış ağ erişimini denetleyin|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. İzin verilen ağları uygulamalardan yalnızca depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli bir internet ya da şirket içi istemcileri bağlantılara izin vermek için belirli bir Azure sanal ağları veya genel internet IP adresi trafiğe aralıkları erişim verin.|
|Özel RBAC kurallarının kullanımını denetleyin|Hataya açık alanlardır özel rol tabanlı erişim denetimi (RBAC) rolleri yerine "sahibi, katkıda bulunan, okuyucu" gibi yerleşik roller denetim. Özel roller kullanılmasını, özel bir durum olarak kabul edilir ve sıkı gözden geçirme ve tehdit modellemesi gerektirir.|
|Azure Stream Analytics'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Depolama hesaplarına güvenli aktarımı denetleyin|Güvenli aktarım depolama hesabınızda gereksinimlerini denetleyin. Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar. Ayrıca, ADAM-de-gizlice ve oturum ele geçirme adam gibi ağ katmanı saldırılarına karşı Aktarımdaki verileri korur.|
|SQL Server için Azure Active Directory Yöneticisi sağlama denetleme|Azure AD kimlik doğrulamasını etkinleştirmek SQL Server için Azure Active Directory (Azure AD) yönetici sağlama denetim. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcı ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimini destekler.|
|Service Bus ad alanlarında yetkilendirme kurallarını denetleyin|Azure Service Bus istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalıklı güvenlik modeli ile hizalamak için erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde oluşturun.|
|Service Bus'ta tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunları ayarlamak için bir yıl tutun. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Service Fabric'te ClusterProtectionLevel özelliğinin EncryptAndSign olarak ayarlanmasını denetleyin|Service Fabric, üç birincil küme sertifikası kullanan düğümden düğüme iletişimi için koruma düzeyleri sağlar: None, oturum ve EncryptAndSign. Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın.|
|Service Fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleyin|Service Fabric Azure AD'de yalnızca üzerinden istemci kimlik doğrulaması kullanımını denetleyin.|
|Arama hizmeti için tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Yalnızca güvenli bağlantı Azure önbelleği için Redis etkinleştirme denetleme|Yalnızca Redis için Azure Cache için SSL aracılığıyla bağlantıları etkinleştirme denetim. Kimlik doğrulama sunucusu ile hizmet arasında güvenli bağlantı kullanımı sağlar. Ayrıca, ADAM-de-gizlice ve oturum ele geçirme adam gibi ağ katmanı saldırılarına karşı Aktarımdaki verileri korur.|
|Logic Apps'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Key Vault'ta tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Event Hubs, tanılama günlüklerini etkinleştirme denetleme|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Azure Data Lake Store'da tanılama günlüklerinin etkinleştirilmesini denetle|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar tutun. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Data Lake Analytics'te tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Klasik depolama hesaplarının kullanımını denetleyin|Azure Resource Manager depolama hesaplarınız için güvenlik geliştirmeleri sağlamak için kullanın. Bunlar: <br>-Daha güçlü erişim denetimi (RBAC)<br>-Daha iyi denetim<br>-Azure Resource Manager tabanlı bir dağıtım ve idare<br>-Yönetilen bir kimlik erişimi<br>-Azure Key Vault için erişim gizli dizileri<br>-Azure AD tabanlı kimlik doğrulaması<br>-Etiketleri ve daha kolay güvenlik yönetimi için kaynak grupları desteği|
|Klasik sanal makinelerin kullanımını denetleyin|Azure Resource Manager sanal makineleriniz için güvenlik geliştirmeleri sağlamak için kullanın.  Bunlar: <br>-Daha güçlü erişim denetimi (RBAC)<br>-Daha iyi denetim<br>-Azure Resource Manager tabanlı bir dağıtım ve idare<br>-Yönetilen bir kimlik erişimi<br>-Azure Key Vault için erişim gizli dizileri<br>-Azure AD tabanlı kimlik doğrulaması<br>-Etiketleri ve daha kolay güvenlik yönetimi için kaynak grupları desteği|
|Batch hesaplarında ölçüm uyarısı kurallarının yapılandırılmasını denetleyin|Gerekli ölçüm etkinleştirmek için Azure Batch hesapları ölçüm uyarı kuralları yapılandırmasını denetleyin.|
|Batch hesaplarında tanılama günlüklerinin etkinleştirilmesini denetleyin|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Otomasyon hesabı değişkenleri şifrelenmesini etkinleştirme denetleme|Hassas verileri depoladığınızda, Azure Otomasyonu hesabı değişken varlıkları şifrelenmesini etkinleştirmek önemlidir.|
|App Service'te tanılama günlüklerini etkinleştirme denetleme|Uygulama tanılama günlüklerini etkinleştirme denetim. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Saydam veri şifreleme durumunu denetle|SQL veritabanları için saydam veri şifreleme durumunu denetle.|
|SQL sunucu düzeyi denetim ayarları denetle|SQL sunucu düzeyinde denetimin var olup olmadığını denetleyin.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle|Azure Güvenlik Merkezi'nde şifrelenmemiş SQL sunucuları veya önerilen olarak veritabanları izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme|Azure Güvenlik Merkezi, SQL sunucularını ve veritabanlarını denetimi önerildiği gibi açık olmayan izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme|Azure Güvenlik Merkezi, önerilen şekilde sunucularınızdaki eksik güvenlik sistem güncelleştirmeleri izler.|
|\[Önizleme]: Depolama hesapları için eksik blob şifrelemesini denetler|Depolama hesapları, blob şifrelemesini kullanmayın denetim. Bu yalnızca Microsoft Depolama kaynak türleri, diğer sağlayıcılar depolamadan geçerlidir. Azure Güvenlik Merkezi, olası ağ tam zamanında erişim önerildiği şekilde izler.|
|\[Önizleme]: Azure Güvenlik Merkezi tam zamanında erişim olası ağ izleme|Azure Güvenlik Merkezi, olası ağ tam zamanında erişim önerildiği şekilde izler.|
|\[Önizleme]: Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme|Azure Güvenlik Merkezi, olası uygulama beyaz listesi yapılandırması izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde esnek ağın genelini erişimi izleme|Azure Güvenlik Merkezi izleyiciler önerildiği gibi çok esnek kurallara sahip güvenlik grupları ağ.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izleyin|Azure Güvenlik Merkezi önerildiği şekilde yapılandırılmış temeli karşılamayan sunucular izler.| 
|\[Önizleme]: Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme|Azure Güvenlik Merkezi yüklü olan bir Microsoft System Center Endpoint Protection Aracısı önerildiği şekilde olmayan sunucularını izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş VM disklerini izleyin|Azure Güvenlik Merkezi'nin önerdiği disk şifrelemesini sahip olmayan sanal makineleri izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde VM güvenlik açıklarını izleyin|Önerildiği gibi Azure Güvenlik Merkezi'nde Güvenlik Açığı değerlendirme çözümü olmayan Vm'leri ve güvenlik açığı değerlendirme çözümü tarafından algılanan güvenlik açıklarını izleyin.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme|Azure Güvenlik Merkezi izleyiciler web önerildiği şekilde web uygulaması güvenlik duvarı koruması olmayan uygulamaları.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izleme|Azure Güvenlik Merkezi izleyiciler önerildiği gibi ileri nesil güvenlik duvarı koruması olmayan uç ağ.|
|\[Önizleme]: SQL güvenlik açığı değerlendirme sonuçları Azure Güvenlik Merkezi'nde izleme|İzleyici güvenlik açığı değerlendirmesi tarama sonuçları ve veritabanı güvenlik açıklarını düzeltme önerilir.|
|\[Önizleme]: Denetim sahip bir abonelik için en yüksek sayısı|Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için en fazla üç abonelik sahipleri belirlediğiniz öneririz.|
|\[Önizleme]: Denetim sahip abonelik için en düşük sayısı|Yönetici erişim sağlamak için birden fazla abonelik sahibi belirlemeniz önerilir yedeklilik.|
|\[Önizleme]: Mfa'yı bir abonelikte etkin olmayan sahip izinleri ile hesapları denetleme|Çok faktörlü kimlik doğrulaması (MFA) sahip izinleriniz hesapları veya kaynak ihlalini önlemek için tüm abonelik hesapları için etkinleştirilmiş olmalıdır.|
|\[Önizleme]: Mfa'yı bir abonelikte etkin olmayan yazma izinleri ile hesapları denetleme|Hesapları veya kaynak ihlalini önlemek için yazma izinlerine sahip tüm abonelik hesapları için çok faktörlü kimlik doğrulaması etkinleştirilmelidir.|
|\[Önizleme]: Mfa'yı bir abonelikte etkin olmayan okuma izinleri olan hesaplar denetleme|Çok faktörlü kimlik doğrulaması okuma hesapları veya kaynak ihlalini önlemek için iznine sahip tüm abonelik hesapları için etkinleştirilmiş olmalıdır.|
|\[Önizleme]: Bir abonelikte sahip izinleri ile kullanım dışı bırakılmış hesapları denetleme|Sahip izinleriniz kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları oturum engellendi.|
|\[Önizleme]: Bir Abonelikteki kullanım dışı bırakılmış hesapları denetleme|Kullanım dışı bırakılmış hesapların aboneliklerinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları oturum engellendi.|
|\[Önizleme]: Bir abonelikte sahip izinleri olan dış hesapları denetleme|Sahip izinleri olan dış hesapları aboneliğinizden izinleri erişimi engellemek için kaldırılmalıdır.|
|\[Önizleme]: Bir abonelikte yazma izinleri olan dış hesapları denetleme|Sahip dış hesapların aboneliğinizden izlenmeyen erişimi engellemek için izinleri kaldırılmalıdır yazın.|
|\[Önizleme]: Bir Abonelikteki Okuma izinleri olan dış hesapları denetleme|Okuma izinlerine sahip dış hesapların aboneliğinizden izlenmeyen erişimi engellemek için kaldırılmalıdır.|




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

* [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md): Planlama ve tasarım konuları Azure Güvenlik Merkezi'nde anlama hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

Azure İlkesi hakkında daha fazla bilgi için bkz: [Azure İlkesi nedir?](../governance/policy/overview.md).
