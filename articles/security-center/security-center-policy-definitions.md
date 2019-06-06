---
title: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları | Microsoft Docs
description: İzlenen Azure Güvenlik Merkezi'nde azure İlkesi tanımları.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c89cb1aa-74e8-4ed1-980a-02a7a25c1a2f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/19/2019
ms.author: monhaber
ms.openlocfilehash: 25ed9cb624474d5da56d385f4e9c155918ec8eab
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428341"
---
# <a name="azure-security-policies-monitored-by-security-center"></a>Güvenlik Merkezi tarafından izlenen azure güvenlik ilkeleri
Bu makalede, Azure Güvenlik Merkezi'nde izleyebilirsiniz Azure ilke tanımlarını listesini sağlar. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [güvenlik ilkeleriyle çalışma](tutorial-security-policy.md).

## <a name="available-security-policies"></a>Kullanılabilir güvenlik ilkeleri

Güvenlik Merkezi tarafından izlenen yerleşik ilkeleri hakkında bilgi edinmek için aşağıdaki tabloya bakın:

| İlke | İlkenin yaptığı |
| --- | --- |
|Sanal makine ölçek kümeleri için tanılama günlüklerine etkinleştirilmelidir.|Böylece bir etkinlik kaydı araştırma bir olay ya da güvenliğinin aşılması için kullanılabilir günlükleri etkinleştirmenizi öneririz.|
|RootManageSharedAccessKey olay hub'ı ad alanından kaldırılması dışında tüm yetkilendirme kuralları|Azure Event Hubs istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalıklı güvenlik modeli ile hizalamak için kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|Olay hub'ı varlık yetkilendirme kurallarında tanımlanması gerekir|Event Hubs varlıklar, en az ayrıcalıklı erişim vermek için yetkilendirme kurallarının var olup olmadığını denetleyin.|
|Güvenlik Duvarı ve sanal ağ yapılandırmaları ile depolama hesaplarının kısıtlanması gerekir|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. İzin verilen ağları uygulamalardan yalnızca depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli bir internet ya da şirket içi istemcileri bağlantılara izin vermek için belirli bir Azure sanal ağları veya genel internet IP adresi trafiğe aralıkları erişim verin.|
|Özel bir RBAC kurallarının kullanımını denetleme|Hataya açık alanlardır özel rol tabanlı erişim denetimi (RBAC) rolleri yerine "sahibi, katkıda bulunan, okuyucu" gibi yerleşik roller denetim. Özel roller kullanılmasını, özel bir durum olarak kabul edilir ve sıkı gözden geçirme ve tehdit modellemesi gerektirir.|
|Azure Stream analytics'te tanılama günlükleri etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Güvenli aktarım depolama hesapları için etkinleştirilmiş olmalıdır|Güvenli aktarım depolama hesabınızda gereksinimlerini denetleyin. Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar. Ayrıca, ADAM-de-gizlice ve oturum ele geçirme adam gibi ağ katmanı saldırılarına karşı Aktarımdaki verileri korur.|
|SQL server için Azure AD Yöneticisi sağlanmalıdır|Azure AD kimlik doğrulamasını etkinleştirmek SQL Server için Azure Active Directory (Azure AD) yönetici sağlama denetim. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcı ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimini destekler.|
|Service Bus ad alanındaki tüm yetkilendirme kurallarını RootManageSharedAccessKey dışında kaldırılmalıdır|Azure Service Bus istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalıklı güvenlik modeli ile hizalamak için erişim ilkelerini kuyruklar ve konular erişebilmesi için yalnızca belirli bir varlık için varlık düzeyinde oluşturun.|
|Service Bus tanılama günlüklerinde etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunları ayarlamak için bir yıl tutun. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Service fabric'te EncryptAndSign ClusterProtectionLevel özelliği ayarlanmalıdır|Service Fabric, üç birincil küme sertifikası kullanan düğümden düğüme iletişimi için koruma düzeyleri sağlar: None, oturum ve EncryptAndSign. Tüm düğümler için iletileri şifrelenir ve dijital olarak imzalanmış emin olmak için koruma düzeyini ayarlayın.|
|Azure Active Directory istemci kimlik doğrulaması kullanmanız gerekir|Service Fabric Azure AD'de yalnızca üzerinden istemci kimlik doğrulaması kullanımını denetleyin.|
|Tanılama günlüklerini arama hizmetleri etkinleştirilmelidir|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştirilmelidir.|Yalnızca Redis için Azure Cache için SSL aracılığıyla bağlantıları etkinleştirme denetim. Kimlik doğrulama sunucusu ile hizmet arasında güvenli bağlantı kullanımı sağlar. Ayrıca, ADAM-de-gizlice ve oturum ele geçirme adam gibi ağ katmanı saldırılarına karşı Aktarımdaki verileri korur.|
|Tanılama günlükleri Logic apps'teki etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Tanılama günlükleri, anahtar Kasası'nda etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Tanılama günlükleri Olay Hub'ındaki etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Azure Data Lake Store tanılama günlüklerine etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunları bir yıla kadar tutun. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Data Lake Analytics için tanılama günlüklerine etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Depolama hesapları için yeni AzureRM kaynaklar geçirilmelidir|Azure Resource Manager depolama hesaplarınız için güvenlik geliştirmeleri sağlamak için kullanın. Bunlar: <br>-Daha güçlü erişim denetimi (RBAC)<br>-Daha iyi denetim<br>-Azure Resource Manager tabanlı bir dağıtım ve idare<br>-Yönetilen bir kimlik erişimi<br>-Azure Key Vault için erişim gizli dizileri<br>-Azure AD tabanlı kimlik doğrulaması<br>-Etiketleri ve daha kolay güvenlik yönetimi için kaynak grupları desteği|
|Sanal makineler yeni AzureRM kaynaklarına geçirilmelidir|Azure Resource Manager sanal makineleriniz için güvenlik geliştirmeleri sağlamak için kullanın.  Bunlar: <br>-Daha güçlü erişim denetimi (RBAC)<br>-Daha iyi denetim<br>-Azure Resource Manager tabanlı bir dağıtım ve idare<br>-Yönetilen bir kimlik erişimi<br>-Azure Key Vault için erişim gizli dizileri<br>-Azure AD tabanlı kimlik doğrulaması<br>-Etiketleri ve daha kolay güvenlik yönetimi için kaynak grupları desteği|
|Batch hesapları ölçüm uyarı kuralları yapılandırılmalıdır|Gerekli ölçüm etkinleştirmek için Azure Batch hesapları ölçüm uyarı kuralları yapılandırmasını denetleyin.|
|Batch hesapları, tanılama günlükleri etkinleştirilmelidir.|Denetim günlüklerini etkinleştirme ve bunlar için bir yıla kadar tutabilirsiniz. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|Otomasyon hesabı değişkenlerde şifreleme etkinleştirilmelidir.|Hassas verileri depoladığınızda, Azure Otomasyonu hesabı değişken varlıkları şifrelenmesini etkinleştirmek önemlidir.|
|App Service içindeki tanılama günlükleri etkinleştirilmelidir.|Uygulama tanılama günlüklerini etkinleştirme denetim. Bu etkinlik kayıtlarını araştırma için bir güvenlik olayı ortaya veya ağınızın tehlikeye oluşturur.|
|SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir.|SQL veritabanları için saydam veri şifreleme durumunu denetle.|
|SQL server denetimi etkinleştirilmelidir|SQL sunucu düzeyinde denetimin var olup olmadığını denetleyin.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle|Azure Güvenlik Merkezi'nde şifrelenmemiş SQL sunucuları veya önerilen olarak veritabanları izler.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme|Azure Güvenlik Merkezi, SQL sunucularını ve veritabanlarını denetimi önerildiği gibi açık olmayan izler.|
|\[Önizleme]: Sistem güncelleştirmeleri makinelerinizde yüklü olması gerekir|Azure Güvenlik Merkezi, önerilen şekilde sunucularınızdaki eksik güvenlik sistem güncelleştirmeleri izler.|
|\[Önizleme]: Depolama hesapları için eksik blob şifrelemesini denetler|Depolama hesapları, blob şifrelemesini kullanmayın denetim. Bu yalnızca Microsoft Depolama kaynak türleri, diğer sağlayıcılar depolamadan geçerlidir. Azure Güvenlik Merkezi, olası ağ tam zamanında erişim önerildiği şekilde izler.|
|\[Önizleme]: Sanal makinelerde Just-In-Time ağ erişim denetimi uygulanmalıdır|Azure Güvenlik Merkezi, olası ağ tam zamanında erişim önerildiği şekilde izler.|
|\[Önizleme]: Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir.|Azure Güvenlik Merkezi, olası uygulama beyaz listesi yapılandırması izler.|
|\[Önizleme]: Ağ güvenlik grupları, sanal makineler için eksik yapılandırılmalıdır|Azure Güvenlik Merkezi izleyiciler önerildiği gibi çok esnek kurallara sahip güvenlik grupları ağ.|
|\[Önizleme]: Güvenlik Yapılandırması makinelerinizde güvenlik açıkları düzeltilen|Azure Güvenlik Merkezi önerildiği şekilde yapılandırılmış temeli karşılamayan sunucular izler.| 
|\[Önizleme]: Uç nokta korumasını sanal makinelerde yüklü olması gerekir|Azure Güvenlik Merkezi yüklü olan bir Microsoft System Center Endpoint Protection Aracısı önerildiği şekilde olmayan sunucularını izler.|
|\[Önizleme]: Sanal makinelerde disk şifrelemesini uygulanmalıdır|Azure Güvenlik Merkezi'nin önerdiği disk şifrelemesini sahip olmayan sanal makineleri izler.|
|\[Önizleme]: Güvenlik açıklarını bir güvenlik açığı değerlendirme çözümü tarafından düzeltilen|Önerildiği gibi Azure Güvenlik Merkezi'nde Güvenlik Açığı değerlendirme çözümü olmayan Vm'leri ve güvenlik açığı değerlendirme çözümü tarafından algılanan güvenlik açıklarını izleyin.|
|\[Önizleme]: Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme|Azure Güvenlik Merkezi izleyiciler web önerildiği şekilde web uygulaması güvenlik duvarı koruması olmayan uygulamaları.|
|\[Önizleme]: Uç nokta koruma çözümüne sanal makinelerde yüklü olması gerekir|Azure Güvenlik Merkezi izleyiciler önerildiği gibi ileri nesil güvenlik duvarı koruması olmayan uç ağ.|
|\[Önizleme]: Güvenlik açıklarını SQL veritabanlarınızda düzeltilen|İzleyici güvenlik açığı değerlendirmesi tarama sonuçları ve veritabanı güvenlik açıklarını düzeltme önerilir.|
|\[Önizleme]: En fazla 3 sahipleri, aboneliğiniz için belirlenen|Güvenliği aşılmış bir sahip tarafından ihlal olasılığını azaltmak için en fazla üç abonelik sahipleri belirlediğiniz öneririz.|
|\[Önizleme]: Aboneliğinize atanmış birden fazla sahibi olmalıdır.|Yönetici erişim sağlamak için birden fazla abonelik sahibi belirlemeniz önerilir yedeklilik.|
|\[Önizleme]: Aboneliğinizde sahip izinleri ile hesapları MFA etkinleştirilmelidir |Çok faktörlü kimlik doğrulaması (MFA) sahip izinleriniz hesapları veya kaynak ihlalini önlemek için tüm abonelik hesapları için etkinleştirilmiş olmalıdır.|
|\[Önizleme]: MFA abonelik hesaplarınızı yazma izinlerine sahip etkinleştirilmiş olmalıdır|Hesapları veya kaynak ihlalini önlemek için yazma izinlerine sahip tüm abonelik hesapları için çok faktörlü kimlik doğrulaması etkinleştirilmelidir.|
|\[Önizleme]: Mfa'yı Okuma izinleri olan abonelik hesaplarınızı etkinleştirilmiş olmalıdır|Çok faktörlü kimlik doğrulaması okuma hesapları veya kaynak ihlalini önlemek için iznine sahip tüm abonelik hesapları için etkinleştirilmiş olmalıdır.|
|\[Önizleme]: Sahip izinleri ile kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor|Sahip izinleriniz kullanım dışı bırakılmış hesapların aboneliğinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları oturum engellendi.|
|\[Önizleme]: Kullanım dışı bırakılan hesapların aboneliğinizden kaldırılması gerekiyor|Kullanım dışı bırakılmış hesapların aboneliklerinizden kaldırılması gerekiyor. Kullanım dışı bırakılmış hesapları oturum engellendi.|
|\[Önizleme]: Sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekiyor|Sahip izinleri olan dış hesapları aboneliğinizden izinleri erişimi engellemek için kaldırılmalıdır.|
|\[Önizleme]: Yazma sahip dış hesapların aboneliğinizden kaldırılması gerekiyor izinleri|Sahip dış hesapların aboneliğinizden izlenmeyen erişimi engellemek için izinleri kaldırılmalıdır yazın.|
|\[Önizleme]: Okuma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekiyor|Okuma izinlerine sahip dış hesapların aboneliğinizden izlenmeyen erişimi engellemek için kaldırılmalıdır.|




## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

* [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md): Planlama ve tasarım konuları Azure Güvenlik Merkezi'nde anlama hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

Azure İlkesi hakkında daha fazla bilgi için bkz: [Azure İlkesi nedir?](../governance/policy/overview.md).
