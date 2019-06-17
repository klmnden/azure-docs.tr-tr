---
title: Azure Güvenlik Merkezi nedir?| Microsoft Docs
description: Azure Güvenlik Merkezi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/06/2019
ms.author: v-mohabe
ms.openlocfilehash: 28e85f2e9caacc0cc30dcc1a073414c34bc2ab0e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064323"
---
# <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?

Azure Güvenlik Merkezi, Azure'da değil - ister veri merkezlerinizdeki güvenlik duruşunu güçlendirir ve hibrit iş yüklerinizi bulutta - genelinde Gelişmiş tehdit koruması sağlayan bir birleşik Altyapı güvenliği yönetim sistemi olan yanı Şirket içi.

Kaynaklarınızın güvenliğini sağlama bir çabasıdır bulut sağlayıcısı, Azure ve sizin, müşteri var. İş yüklerinizi buluta ve aynı zamanda Iaas (hizmet olarak altyapı) için PaaS (hizmet olarak platform) ve SaaS (hizmet olarak yazılım) oluştu çok daha fazla müşteri sorumluluğu yoktur taşıdığınızda, taşıma olarak güvenli olduğundan emin olmanız gerekir. Azure Güvenlik Merkezi ağınıza sağlamlaştırmak hizmetlerinizin güvenliğini ve güvenlik duruşunuzu üzerine olduğunuzdan emin olmak için gereken araçları sağlar.

Azure Güvenlik Merkezi, üç en Acil güvenlik sorunlarını ele alır:

-   **İş yüklerini hızla değişen** – hem bir gücü, hem de bulut için kimlik doğrulaması. Bir yandan, son kullanıcılara daha fazlasını yetkisine sahiptir. Diğer, nasıl, sürekli değişen hizmetlerini kullandığını ve oluşturma, güvenlik standartlarınıza kadar olduğundan emin olun ve en iyi güvenlik uygulamalarını izleyin?

-   **Saldırıları'giderek daha karmaşık** - iş yüklerinizi çalıştırmak, saldırıları daha fazla almaya devam her yerde karmaşık. Geçerli olan genel bulut iş yüklerinizi korumak sahip olduğunuz, güvenlik izlerseniz yoksa daha savunmasız en iyi bırakabilirsiniz bir İnternet'e yönelik iş yükü yöntemler.

-   **Güvenlik yetenekleri olan kısa tedarik** -güvenlik uyarıları ve uyarı sistemleri sayısı kadar ortamlarınızda korunmasını emin olmak için yönetici deneyimi ve gerekli arka plan sayısı kadar outnumbers. En son saldırıları ile güncel kalma güvenlik dünyasına sürekli değişen bir ön olsa da, yerinde kalmasını olanaksız hale sabit, zordur.

Bu zorluklar karşı korunmanıza yardımcı olmak için Güvenlik Merkezi, araçlar sağlar:

-   **Güvenlik duruşunu güçlendirin**: Güvenlik Merkezi ortamınızın değerlendirir ve kaynaklarınızın durumunu anlama sayesinde, güvenli olsalar da?

-   **Tehditlere karşı koruma**: Güvenlik Merkezi, iş yüklerinizi değerlendirir ve tehdit önleme öneriler ve tehdit algılama uyarıları başlatır.

-   **Güvenli hızlı**: Güvenlik Merkezi'nde, bulut hızı her şeyi gerçekleştirilir. Yerel olarak tümleşik olduğundan, Güvenlik Merkezi dağıtımının autoprovisioning ve Azure Hizmetleri ile koruma sağlama, kolay bir işlemdir.

## <a name="architecture"></a>Mimari

Güvenlik Merkezi, yerel olarak Azure, Azure - Service Fabric gibi PaaS hizmetlerine bir parçası olduğundan SQL veritabanları ve depolama hesapları - izlenen ve Güvenlik Merkezi tarafından herhangi bir dağıtıma araya olmadan korumalı.

Ayrıca, Güvenlik Merkezi Azure dışı sunucular ve sanal makineleri bulutta veya şirket içinde hem Windows hem de Linux sunucuları için Microsoft Monitoring Agent yükleyerek korur. Azure sanal makineleri otomatik olarak sağlanan Güvenlik Merkezi'nde.

Aracılardan gelen ve azure'dan toplanan olayları, iş yüklerinizi güvenli olduğundan emin olmak için izleyin ve tehdit algılama uyarıları (görevleri sağlamlaştırma) öneriler, uyarlanmış sağlamak için güvenlik analiz altyapısı bağıntılı olan. Olabildiğince çabuk kötü amaçlı saldırıları, iş yükleriniz üzerinde gerçekleşen olmayan emin olmak için bu tür uyarılar araştırmanız gerekir.

Güvenlik Merkezi'ni etkinleştirdiğinizde, yerleşik Güvenlik Merkezi güvenlik ilkesi Azure İlkesi'nde Güvenlik Merkezi kategori altında yerleşik bir girişim olarak yansıtılır. Yerleşik girişim, tüm Güvenlik Merkezi kayıtlı abonelikler (ücretsiz veya standart katmanları) otomatik olarak atanır. Yerleşik girişim yalnızca denetim ilkeleri içerir. Azure İlkesi'nde Güvenlik Merkezi ilkeleri hakkında daha fazla bilgi için bkz. [güvenlik ilkeleriyle çalışma](tutorial-security-policy.md).

## <a name="strengthen-security-posture"></a>Güvenlik duruşunu güçlendirme

Azure Güvenlik Merkezi, güvenlik duruşunuzu güçlendirin olanak tanır. Başka bir deyişle, en iyi güvenlik uygulamaları önerilen sağlamlaştırma görevlerini gerçekleştirmek ve bunları makineler, veri hizmetleri ve uygulamaları arasında uygulama yardımcı olur. Bu yönetme ve güvenlik ilkelerini zorunlu içerir ve emin olun, Azure sanal makineler, Azure dışı sunucular ve Azure PaaS hizmetlerine uyumludur. Güvenlik Merkezi, ağ güvenlik Emlak üzerinde odaklı görünürlük ile iş yüklerinizi kuş göz görünümünde sağlamak için ihtiyacınız olan araçları sağlar. 

### <a name="manage-organization-security-policy-and-compliance"></a>Kuruluşunuzun güvenlik ilkesi ve uyumluluğunu yönetmek

Bu, bildiğiniz ve iş yüklerinizi güvenlidir ve güvenlik ilkeleri uyarlanmış ile başlayan emin olmak için temel bir güvenlik olur. Tüm ilkeler Güvenlik Merkezi'nde Azure İlkesi denetimler üzerine yerleşik olduğundan tam aralığıdır ve esnekliğini karşılaşacağınız bir **birinci sınıf ilke çözüm**. Güvenlik Merkezi'nde, abonelikler arasında ve tüm Kiracı için bile yönetim grubu üzerinde çalıştırılacak ilkelerini ayarlayabilirsiniz.

![Güvenlik Merkezi panosu](media/security-center-intro/sc-dashboard.png)

Güvenlik Merkezi, size yardımcı olur **gölge BT abonelikleri belirleyin**. Etiketlenmiş abonelikler bakarak **kapsanmayan** Panonuzda, hemen var. yeni abonelikler oluşturulduğunda bildiğiniz ve bunlar kendi ilkeleri tarafından kapsanan ve Azure Güvenlik Merkezi tarafından korunan emin olun.

![Güvenlik Merkezi ilke Panosu](media/security-center-intro/sc-policy-dashboard.png)

Güvenlik Merkezi'nde Gelişmiş izleme özellikleri da olanak **uyumluluk ve zaman içinde idare yönetmek ve izlemek**.  **Genel uyumluluk** ne kadar aboneliklerinizi yükünüz ilişkili ilkeleriyle uyumlu bir ölçü sağlar. 

![Güvenlik Merkezi ilke zamanla](media/security-center-intro/sc-policy-time.png)

### <a name="continuous-assessments"></a>Devamlı değerlendirmelerle

Güvenlik Merkezi sürekli olarak, bir yüklerinde dağıtılan yeni kaynaklar bulur ve bunların güvenlik en iyi yöntemlere göre yapılandırıldığından yoksa, bayrak ve Önceliklendirilmiş öneriler nelerin listesini almak olup olmadığını değerlendirir. makinelerinizi korumak için düzeltme gerekir.

Güvenlik Merkezi sağlar, sürekli olarak ağınızın güvenlik durumunu izleme için en güçlü araçlardan birini **ağ eşlemesi**. Haritayı, her düğüm düzgün yapılandırılıp yapılandırılmadığını görmek için iş yüklerinizi topolojisini görmek sağlar. Nasıl düğümlerinizi, olası bir saldırganın ağınızda katlamayı kolaylaştırmak istenmeyen bağlantıları engelle yardımcı olan bağlandığını görebilirsiniz.

![Güvenlik Merkezi ağ eşlemesi](media/security-center-intro/sc-net-map.png)

Güvenliğiniz için Azaltıcı Güvenlik Merkezi hale ekleyerek bir adım daha kolay, uyarılar bir **güvenli puanı**. Güvenli puanları sunulmuştur aldığınız her önerinin genel güvenlik duruşunuzu ne kadar önemli olduğunu anlamanıza yardımcı olması için her bir öneri ile ilişkili. Bu, etkinleştirme önemlidir **güvenlik işinizin önceliğini belirlemeye**.

![Güvenlik Merkezi güvenli puanı](media/security-center-intro/sc-secure-score.png)

### <a name="optimize-and-improve-security-by-configuring-recommended-controls"></a>En iyi duruma getirmek ve önerilen denetimleri yapılandırarak güvenliğini artırın

Kendi önerilerini Azure Güvenlik Merkezi'nin değeri kalbi arasındadır. Öneriler, iş yükleriniz üzerinde bulunan belirli güvenlik konuları için uyarlanabilir ve Güvenlik Merkezi Güvenlik Yöneticisi, yalnızca, güvenlik açıklarının, ancak bunları kurtulmak belirli yönergeleri ile sağlayarak çalışır.

![Güvenlik Merkezi önerileri](media/security-center-intro/sc-recommendations.png)

Bu şekilde, Güvenlik Merkezi, yalnızca güvenlik ilkelerini ayarlamak için ancak güvenli yapılandırma standartları kaynaklarınız genelinde uygulamak için sağlar.

Öneriler her kaynaklarınızı saldırı yüzeyini azaltmanıza yardımcı olur. Azure sanal makineleri, Azure dışı sunucuları ve SQL ve depolama hesapları gibi Azure PaaS Hizmetleri ve daha fazla - burada kaynak türlerinin farklı değerlendirilir ve kendi standartlarına sahip içeren.

![Güvenlik Merkezi önerisini örneği](media/security-center-intro/sc-recommendation-example.png)

## <a name="protect-against-threats"></a>Tehditlere karşı koruma sağlayın

Güvenlik Merkezi'nin tehdit koruması, algılamak ve Azure'da olarak hizmet (Iaas) katmanı, Azure dışı sunucuları yanı platformlar hizmet (PaaS) olarak olduğu gibi altyapı tehditlerini önlemek sağlar.

Güvenlik Merkezi'nin tehdit koruması otomatik olarak uyarıları hikayenin başlattığınız yerde, saldırı kampanyasını ve hangi daha iyi anlamanıza yardımcı olması için siber sonlandırma zinciri Analizine, göre ortamınızdaki karşılık gelen fusion sonlandırma zinciri analizi içerir. tür bir etkisi, kaynaklarınız üzerinde vardı.



![Güvenlik Merkezi saldırı önerisi](media/security-center-intro/sc-attack-recommendation.png)

### <a name="advanced-threat-protection"></a>Gelişmiş tehdit koruması

Güvenlik Merkezi ile Windows Defender Gelişmiş tehdit koruması ile yerel tümleştirme hazır olursunuz. Bu, herhangi bir yapılandırma sunucuları ve Windows sanal makineleri tam olarak Güvenlik Merkezi önerileri ve değerlendirme ile tümleştirildiğinden anlamına gelir. Gelişmiş tehdit algılama ayrıca Linux sanal makineleri ve sunucular için hazır olarak sunulur.

Ayrıca, Güvenlik Merkezi sunucu ortamlarında uygulama denetim ilkelerini otomatikleştirmenizi sağlar. Güvenlik Merkezi'ndeki Uyarlamalı uygulama denetimleri Windows sunucularınız genelinde uçtan uca uygulama beyaz listeye ekleme özelliğini etkinleştirin. Kurallar oluşturabilir ve ihlalleri denetleyin gerekmez, bunu tüm otomatik olarak sizin yerinize yapılır.

### <a name="protect-paas"></a>PaaS koruyun

Güvenlik Merkezi Azure PaaS hizmetlerinde tehditleri algılamanıza yardımcı olur. Azure App Service, Azure SQL, Azure depolama hesabı dahil olmak üzere Azure Hizmetleri ve daha fazla veri hizmetlerini hedefleyen tehditleri algılayabilir. Ayrıca Microsoft Cloud App Security'nin kullanıcı ve varlık davranış analizi (UEBA) anomali algılama, Azure etkinlik günlüklerini gerçekleştirmek için yerel tümleştirme yararlanabilirsiniz.

### <a name="block-brute-force-attacks"></a>Blok deneme yanılma saldırıları zorla

Güvenlik Merkezi, deneme yanılma saldırılarına maruz kalma riskinizi sınırlandırmanıza yardımcı olur. Sanal makine bağlantı noktalarına tam zamanında VM erişimi kullanarak erişim azaltarak gereksiz erişim engelleyerek, ağ sağlamlaştırma. Seçili bağlantı noktaları üzerinde sınırlı bir süre ve kaynak IP adresi aralıkları veya IP adreslerine izin yalnızca yetkili kullanıcılar için güvenli erişim ilkeleri ayarlayabilirsiniz.

![Güvenlik Merkezi deneme yanılma](media/security-center-intro/sc-brute-force.png)

### <a name="protect-data-services"></a>Veri Hizmetleri koruyun

Güvenlik Merkezi yardımcı olan özellikler içeren Azure SQL'de verileriniz otomatik sınıflandırma gerçekleştirin. Ayrıca, Azure SQL ve depolama hizmetleri ve bunların etkisini öğrenmek için öneriler değerlendirmeleri için olası güvenlik açıklarını elde edebilirsiniz.

### <a name="protect-iot-and-hybrid-cloud-workloads-preview"></a>IOT ve hibrit bulut iş yüklerini (Önizleme) koruma

IOT (nesnelerin interneti) için Azure Güvenlik Merkezi hibrit iş yükü koruması edge, şirket içinde çalışan iş yükleri arasında birleşik görünürlük ve denetim, Uyarlamalı tehdit önleme ve akıllı tehdit algılama ve yanıt sunarak basitleştirir Azure ve diğer bulutlarda. Daha fazla bilgi için [IOT (Önizleme) için Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/asc-for-iot/).

## <a name="get-secure-faster"></a>Daha hızlı, güvenli alın

Microsoft Cloud App Security ve Windows Defender Gelişmiş tehdit koruması emin olmaya yardımcı olmak gibi diğer Microsoft Güvenlik çözümleriyle sorunsuz tümleştirme birlikte (Azure İlkesi ve Azure İzleyici günlüklerine dahil) yerel Azure tümleştirmesi, güvenlik çözümü kapsamlı olarak eklemek için basit ve alma.

Ayrıca, şirket içi veri merkezleri ve diğer bulutlarda çalışan iş yükleri için Azure dışında tam çözüm genişletebilirsiniz.

### <a name="automatically-discover-and-onboard-azure-resources"></a>Otomatik olarak bulmak ve yerleşik Azure kaynakları

Güvenlik Merkezi, Azure ve Azure ile sorunsuz ve yerel tümleştirme sağlar kaynakları. Azure İlkesi ile yerleşik Güvenlik Merkezi ilkeleri, tüm Azure kaynaklarınız genelinde arasındaki tüm güvenlik Öykü birlikte çekeceği ve oluştururken tamamı yeni bulunan kaynaklar için otomatik olarak uygulanan emin anlamına gelir Azure'da.

Kapsamlı günlük toplama - Windows ve Linux günlükleri tüm güvenlik analiz altyapısında çevrelerini ve öneriler ve Uyarılar oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik Merkezi ile çalışmaya başlamak için bir Microsoft Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Güvenlik Merkezi’nin Ücretsiz fiyatlandırma katmanı, Azure aboneliğiniz ile etkinleştirilir. Gelişmiş güvenlik yönetimi ve tehdit algılama yeteneklerinden yararlanmak için Standart fiyatlandırma katmanına yükseltmeniz gerekir. Standart katmanında ücretsiz girişiminde bulunulur. Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/) bakın.
- Şimdi, Güvenlik Merkezi standart etkinleştirmek için hazır olduğunuzda [hızlı başlangıç: Yerleşik Azure aboneliğinizi Güvenlik Merkezi standart](security-center-get-started.md) adımlarında size kılavuzluk eder.

