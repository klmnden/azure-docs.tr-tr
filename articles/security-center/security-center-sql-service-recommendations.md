---
title: Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma | Microsoft Docs
description: Bu belge adresleri yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure SQL Hizmeti ve veri koruma ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: terrylan
ms.openlocfilehash: 177deb779ca3e3e9575a41ab9a37bb51d5e79df8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008088"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur.  Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler), ağ, SQL, veri ve uygulama.

Bu makalede Azure SQL hizmet ve verilere uygulanır önerileri ele alır. Azure SQL sunucuları ve veritabanları, SQL veritabanları için şifreleme ve Azure depolama hesabınızın şifrelemesini etkinleştirmek için denetimi etkinleştirme etrafında önerileri Merkezi.  Aşağıdaki tabloda, hizmet ve veri kullanılabilir SQL önerileri ve uygulamanız durumunda her birinin yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.
### <a name="monitor-data-security"></a>Veri güvenliğini izleme

**Önleme** bölümünde **Veri güvenliği**’ne tıkladığınızda, **Veri Kaynakları** bölümünü SQL ve Depolama’ya yönelik önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için [öneriler](security-center-sql-service-recommendations.md) içerir. Depolama şifrelemesi hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) bölümünü okuyun.

![Veri Kaynakları](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

**SQL Önerileri** altında herhangi bir öneriye tıklayabilir ve sorunu çözmek için yapılacak diğer eylemlerle ilgili daha fazla ayrıntı görebilirsiniz. Aşağıdaki örnekte, **SQL veritabanlarında Veritabanı Denetimi ve Tehdit algılama** önerisinin genişletilmiş hali gösterilmiştir.

![SQL önerisi hakkındaki ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**SQL veritabanlarında Denetimi ve Tehdit algılamayı etkinleştirme** bölümünde aşağıdaki bilgiler bulunur:

* SQL veritabanlarının bir listesi
* Bulundukları sunucu
* Bu ayarın sunucudan devralınmış olduğuna veya bu veritabanında benzersiz olduğuna dair bilgiler
* Geçerli durum
* Sorunun önem derecesi

Bu öneriyle ilgilenmek için veritabanına tıkladığınızda, **Denetim ve Tehdit algılama** bölümü aşağıdaki ekran görüntüsünde gösterildiği gibi açılır.

![Denetim ve Tehdit algılama](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Denetimi etkinleştirmek için **Denetim** seçeneğinin altında **AÇIK**'ı seçin.

## <a name="data-and-storage-recommendations"></a>Veri ve depolama önerileri

|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|Depolama hesabı|20|Depolama hesabı için güvenli aktarım gerektir|Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|Redis|20|Yalnızca güvenli bağlantılar Redis Cache'inizi etkinleştir|Yalnızca Redis cache'e SSL aracılığıyla bağlantıları etkinleştirin. Güvenli bağlantı kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|SQL|15|SQL veritabanlarında saydam veri şifrelemesini etkinleştirme|Bekleyen verileri korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifrelemesini etkinleştirin.|
|SQL|15|SQL sunucularında denetimi etkinleştirme|Azure SQL sunucuları için denetimi etkinleştirin. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.)|
|SQL|15|SQL veritabanlarında denetimi etkinleştirme|Azure SQL veritabanları için denetimi etkinleştirin. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.)|
|Data lake analytics|15|Data Lake Analytics, bekleyen şifreleme etkinleştir|Saydam veri şifrelemesi, Data Lake Analytics bölgesinde, bekleyen verilerin güvenliğini sağlamak etkinleştirin. Bekleme sırasında şifreleme saydamdır, yani Data Lake Analytics kaydetmeden önce verileri otomatik olarak şifreler ve almadan öncesinde verilerin şifresini çözer. İçinde uygulamalar gereken değişiklik yok ve şifreleme nedeniyle Data Lake Analytics ile etkileşim kuran hizmetler. Bekleme sırasında şifreleme fiziksel hırsızlığa veri kaybı riski en aza indirir ve ayrıca yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur.|
|Data lake store|15|Data Lake Store için bekleyen şifrelemenin etkinleştir|Saydam veri şifrelemesi, Data Lake Store bekleyen veriler güvenli hale getirmek etkinleştirin. Bekleme sırasında şifreleme saydamdır, yani Data Lake Store kaydetmeden önce verileri otomatik olarak şifreler ve almadan öncesinde verilerin şifresini çözer. Uygulamalar ve hizmetler şifreleme uyum sağlamak için Data Lake Store ile etkileşimde bulunan herhangi bir değişiklik yapmanız gerekmez. Bekleme sırasında şifreleme fiziksel hırsızlığa veri kaybı riski en aza indirir ve ayrıca yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur.|
|Depolama hesabı|15|Azure Depolama Hesabı için şifrelemeyi etkinleştirin|Azure depolama hizmeti şifrelemesi, bekleyen veriler için etkinleştirin. Azure depolama alanına yazılır ve alma önce şifrelerini çözer, verileri şifreleyerek depolama hizmeti şifrelemesi (SSE) çalışır. SSE, şu anda yalnızca Azure Blob hizmeti için kullanılabilir ve blok blobları, sayfa blobları için kullanılabilir ve ekleme blobları.|
|Data lake analytics|5|Data Lake analytics'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|Data lake store|5|Azure Data Lake Store tanılama günlüklerine etkinleştir|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|SQL|30|SQL veritabanlarınızda güvenlik açıklarını düzeltin|SQL güvenlik açığı değerlendirmesi, veritabanınızı güvenlik açıkları için tarar ve yanlış yapılandırmalar, aşırı izinleri ve korumasız hassas veriler gibi en iyi sapmaları kullanıma sunar. Güvenlik açıkları bulundu çözme, veritabanı güvenliği stature büyük ölçüde artırabilir.|
|SQL|20|SQL server için Azure AD Yöneticisi sağlama|Azure AD kimlik doğrulamasını etkinleştirmek için SQL server için Azure AD Yöneticisi sağlayın. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimi veritabanı kullanıcıları ve diğer Microsoft hizmetleri sağlar.|
|Depolama hesabı|15|Depolama hesabı için sınırsız ağ erişimi devre dışı bırak|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. Bunun yerine, yalnızca izin verilen ağları uygulamalardan depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli Internet'ten bağlantılara izin vermek ya da şirket içi istemciler için erişim belirli bir Azure sanal ağları gelen trafiği veya genel Internet IP adresi aralıkları için verilebilir.|
|Depolama hesabı|1||Depolama hesapları için yeni AzureRM kaynakları geçirme|Depolama hesaplarınız gibi güvenlik geliştirmeleri sağlamak için yeni Azure Resource Manager v2 kullanın: yönetilen kimlikleri için anahtar kasasına erişim daha güçlü erişim denetimi (RBAC), daha iyi denetim, Resource Manager tabanlı bir dağıtım ve yönetim erişim Gizli dizileri, Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları.|



## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
