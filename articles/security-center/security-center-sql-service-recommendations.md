---
title: Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma | Microsoft Docs
description: Bu belge adresleri yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure SQL Hizmeti ve veri koruma ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: rkarlin
ms.openlocfilehash: 479e6d222c9fc7d007252bab12e807ec337ea9e6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332474"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur.  Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler), ağ, SQL, veri ve uygulama.


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
|Depolama hesabı|20|Depolama hesabına güvenli aktarım gerektirme|Güvenli aktarım depolama hesabınıza yalnızca güvenli bağlantı (HTTPS) gelen istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|Redis|20|Yalnızca güvenli bağlantılar, Azure önbelleği için Redis etkinleştir|Azure Cache için SSL aracılığıyla yalnızca bağlantıları Redis için etkinleştirin. Güvenli bağlantı kullanımı, sunucu ve hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarına karşı ortadaki-de-gizlice ve oturum ele geçirme adam gibi Aktarımdaki verileri korur.|
|SQL|15|SQL veritabanlarında saydam veri şifrelemesini etkinleştirme|Bekleyen verileri korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifrelemesini etkinleştirin.|
|SQL|15|SQL sunucularında denetimi etkinleştirme|Azure SQL sunucuları için denetimi etkinleştirin. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.)|
|SQL|15|SQL veritabanlarında denetimi etkinleştirme|Azure SQL veritabanları için denetimi etkinleştirin. (Yalnızca azure SQL Hizmeti. Sanal makineler üzerinde çalışan SQL dahil değildir.)|
|Data lake analytics|15|Data Lake Analytics, bekleyen şifreleme etkinleştir|Saydam veri şifrelemesi, Data Lake Analytics bölgesinde, bekleyen verilerin güvenliğini sağlamak etkinleştirin. Bekleme sırasında şifreleme saydamdır, yani Data Lake Analytics kaydetmeden önce verileri otomatik olarak şifreler ve almadan öncesinde verilerin şifresini çözer. İçinde uygulamalar gereken değişiklik yok ve şifreleme nedeniyle Data Lake Analytics ile etkileşim kuran hizmetler. Bekleme sırasında şifreleme fiziksel hırsızlığa veri kaybı riski en aza indirir ve ayrıca yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur.|
|Data lake store|15|Data Lake Store için bekleyen şifrelemenin etkinleştir|Saydam veri şifrelemesi, Data Lake Store bekleyen veriler güvenli hale getirmek etkinleştirin. Bekleme sırasında şifreleme saydamdır, yani Data Lake Store kaydetmeden önce verileri otomatik olarak şifreler ve almadan öncesinde verilerin şifresini çözer. Uygulamalar ve hizmetler şifreleme uyum sağlamak için Data Lake Store ile etkileşimde bulunan herhangi bir değişiklik yapmanız gerekmez. Bekleme sırasında şifreleme fiziksel hırsızlığa veri kaybı riski en aza indirir ve ayrıca yasal uyumluluk gereksinimlerini karşılamaya yardımcı olur.|
|Data lake analytics|5|Data Lake Analytics'te tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|Data lake store|5|Azure Data Lake Store'da tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. |
|SQL|30|SQL veritabanlarınızdaki güvenlik açıklarını düzeltme|SQL güvenlik açığı değerlendirmesi, veritabanınızı güvenlik açıkları için tarar ve yanlış yapılandırmalar, aşırı izinleri ve korumasız hassas veriler gibi en iyi sapmaları kullanıma sunar. Güvenlik açıkları bulundu çözme, veritabanı güvenliği stature büyük ölçüde artırabilir.|
|SQL|20|SQL sunucusuna Azure AD yöneticisi sağlama|Azure AD kimlik doğrulamasını etkinleştirmek için SQL server için Azure AD Yöneticisi sağlayın. Azure AD kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimi veritabanı kullanıcıları ve diğer Microsoft hizmetleri sağlar.|
|Depolama hesabı|15|Depolama hesabına sınırsız ağ erişimini devre dışı bırakma|Depolama hesabı güvenlik duvarı ayarlarınızda sınırsız ağ erişimi denetleyin. Bunun yerine, yalnızca izin verilen ağları uygulamalardan depolama hesabına erişebilmesi için ağ kurallarını yapılandırın. Belirli bir Internet ya da şirket içi istemcileri bağlantılara izin vermek için belirli bir Azure sanal ağları gelen trafiği veya genel Internet IP adresi aralıkları için erişim verebilirsiniz.|
|Depolama hesabı|1|Azure Resource Manager modeli kaynakları için depolama hesapları geçirme|Depolama hesaplarınız için yeni Azure Resource Manager v2 gibi güvenlik geliştirmeleri sağlamak için kullanın: yönetilen kimlikleri için anahtar kasasına erişim daha güçlü erişim denetimi (RBAC), daha iyi denetim, Resource Manager tabanlı bir dağıtım ve yönetim erişim gizli ve Azure AD tabanlı kimlik doğrulaması ve etiketleri için destek ve daha kolay güvenlik yönetimi için kaynak grupları.|

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
