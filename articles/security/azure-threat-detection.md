---
title: Azure Gelişmiş tehdit algılama | Microsoft Docs
description: Azure AD kimlik koruması ve özellikleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 2a6a0e6219a45821e2a4416a4e563aa6edb86eba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127181"
---
# <a name="azure-advanced-threat-detection"></a>Azure Gelişmiş tehdit algılama

Azure Active Directory (Azure AD), Azure İzleyici günlüklerine ve Azure Güvenlik Merkezi gibi hizmetler aracılığıyla Gelişmiş tehdit algılama işlevselliği yerleşik azure sunar. Güvenlik Hizmetleri ve özellikler bu koleksiyonu, Azure dağıtımlarınızı içinde neler olduğunu anlamak için basit ve hızlı bir yol sağlar.

Azure, bir çeşit uygulama dağıtımlarınıza ilişkin gereksinimleri karşılamak için güvenlik özelliklerini ve yapılandırma seçenekleri sağlar. Bu makalede, bu gereksinimleri karşılayan anlatılmaktadır.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

[Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) olduğu bir [Azure Active Directory Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edition özelliği, kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını ve risk olayları hakkında genel bir bakış sağlar. Kimlik koruması aracılığıyla kullanılabilen mevcut Azure AD anomali algılama özellikleri kullanan [Azure AD anormal Etkinlik raporlarını](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports)ve gerçek zamanlı anormal durumları algılayabilirsiniz yeni risk olayı türlerini tanıtır.

![Azure AD kimlik koruması diyagramı](./media/azure-threat-detection/azure-threat-detection-fig1.png)

Kimlik koruması, anormallikleri ve risk bir kimliğin gizliliğinin bozulduğunu gösteriyor olabilir olaylarını için Uyarlamalı makine öğrenme algoritmaları ve buluşsal yöntemler kullanır. Siz de bu risk olaylarını incelemenize ve uygun düzeltme veya risk azaltma eylemi bu verileri kullanarak, kimlik koruması raporlar ve uyarılar oluşturur.

Azure Active Directory kimlik koruması bir izleme ve raporlama aracıyla daha büyük. Risk olaylara göre kuruluşunuzun kimliklerini otomatik olarak korumak üzere risk tabanlı ilkeler yapılandırabilirsiniz. böylece kimlik koruması, her kullanıcı için bir kullanıcı risk düzeyi hesaplar.

Diğer ek olarak bu risk tabanlı ilkeler [koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) Azure Active Directory tarafından sağlanan ve [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), otomatik olarak engelleyebilir veya uyarlamalı düzeltme eylemleri sunma, parola sıfırlama ve çok faktörlü kimlik doğrulaması zorlama içerir.

### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

Azure Active Directory kimlik koruması bir izleme ve raporlama aracıyla daha büyük. Kuruluşunuzun kimliklerini korumak için belirtilen risk düzeyi ulaşıldığında otomatik olarak algılanan sorunlara yanıt risk tabanlı ilkeler yapılandırabilirsiniz. Azure Active Directory ve EMS tarafından sağlanan diğer koşullu erişim denetimleri ek olarak bu ilkeleri otomatik olarak engellemek veya uyarlamalı düzeltme eylemleri de dahil olmak üzere parolasını sıfırlar ve çok faktörlü kimlik doğrulaması zorlama başlatın.

Azure kimlik koruması yardımcı olabilecek yollardan bazılarını örnekleri hesaplarınızın güvenli ve kimliklerini şunları içerir:

[Risk olaylarını algılamak ve riskli hesapları](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Makine öğrenme ve buluşsal kurallarını kullanarak altı risk olayı türleri algılayın.
-   Kullanıcı risk düzeyleri hesaplayın.
-   Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu geliştirmek için özel öneriler sağlar.

[Risk olayları araştırma](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Risk olayları için bildirimleri gönderin.
-   Risk olayları ve ilgili bağlamsal bilgileri kullanarak araştırın.
-   Araştırmalar izlemek için temel iş akışları sağlar.
-   Parola sıfırlama gibi düzeltme eylemleri kolay erişim sağlar.

[Risk tabanlı, koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
-   Riskli oturum açma, oturum açma engelleme veya çok faktörlü kimlik doğrulaması zorluklarını gerektirerek azaltın.
-   Engelleme veya riskli kullanıcı hesaplarını güvenli hale getirme.
-   Çok faktörlü kimlik doğrulamasına kaydolacak şekilde kullanıcıların gerektirir.

### <a name="azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management

İle [Azure Active Directory Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)yönetebileceğiniz, erişim denetimi ve izleme, kuruluşunuzun içinde. Bu özellik, Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi içerir.

![Azure AD Privileged Identity Management diyagramı](./media/azure-threat-detection/azure-threat-detection-fig2.png)

PIM size yardımcı olur:

-   Uyarılar ve raporlar hakkında Azure AD yöneticileri ve Office 365 ve Intune gibi çevrimiçi Microsoft hizmetlerine yönetim erişimi just-in-time (JIT) alın.

-   Yönetici atamalarını yönetici erişim geçmişine ve değişiklikleri hakkında rapor alın.

-   Ayrıcalıklı bir role erişimi hakkında uyarı alın.

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan bir Microsoft bulut tabanlı BT yönetimi çözümüdür. Bulut tabanlı bir hizmet olarak Azure İzleyici günlüklerine gerçekleştirilir olduğundan, çalışmaya hızlıca altyapı hizmetleri için çok az yatırım ile sağlayabilirsiniz. Yeni güvenlik özellikleri otomatik olarak devam eden bakım tasarruf sağlarsınız ve yükseltme maliyetlerinden.

Değerli hizmetler kendi, Azure İzleyici hakkında sağlamaya ek olarak günlükleri System Center bileşenleriyle gibi tümleştirebilirsiniz [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/), mevcut güvenlik yönetimi yatırım genişletmek için bulut. System Center ve Azure İzleyici günlüklerine tam karma yönetim deneyimi sağlamak için birlikte çalışır.

### <a name="holistic-security-and-compliance-posture"></a>Bütünsel bir güvenlik ve uyumluluk duruşunu

[Log Analytics güvenlik ve Denetim Panosu](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) kuruluşunuzun kapsamlı bir görünüm sağlar, ilgilenmenizi gerektiren önemli sorunlar için şirket içi arama sorgularıyla de IT güvenlik duruşuna. Güvenlik ve denetim günlüklerini Azure İzleyici'deki güvenlik ilgili her şeyi için giriş ekranı panodur. Bu pano, size bilgisayarlarınızın güvenlik durumuyla ilgili yüksek düzeyde öngörü sağlar. Ayrıca, tüm son 24 saat, 7 gün, olaylarından veya herhangi bir özel süre görüntüleyebilirsiniz.

Azure İzleyici, her türlü ortamda, tüm yazılım güncelleştirme değerlendirmesi, kötü amaçlı yazılımdan koruma değerlendirmesi ve yapılandırma temelleri dahil olmak üzere, BT işlemleri bağlamında, genel güvenlik duruşunu hızla ve kolayca anlamanıza yardım günlüğe kaydeder. Güvenlik günlüğü verileri, güvenlik ve uyumluluk denetim işlemlerini kolaylaştırmak için kolayca erişilebilir.

![Log Analytics güvenlik ve Denetim Panosu](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

Log Analytics güvenlik ve Denetim Panosu dört ana kategoride düzenlenmiştir:

-   **Güvenlik etki alanları**: Daha fazla zaman içindeki güvenlik kayıtları keşfetmenize olanak sağlar; erişim kötü amaçlı yazılım değerlendirmesi; Güncelleştirme değerlendirmelerini; Ağ güvenliği, kimlik ve erişim bilgileri görüntüle Güvenlik olayları içeren bilgisayarları görüntüle ve Azure Güvenlik Merkezi panosuna hızlıca erişin.

-   **Önemli sorunlar**: Etkin sorunların sayısını ve sorunların önem derecesini hızlıca tanımlamanıza olanak sağlar.

-   **Algılamalar (Önizleme)** : Güvenlik Uyarıları karşı kaynaklarınızı oluşunca görüntüleyerek saldırı düzenlerini tanımlayabilmenizi sağlar.

-   **Tehdit bilgileri**: Giden kötü amaçlı IP trafiğine sahip sunucuların toplam sayısı, kötü amaçlı tehdit türü ve IP'ler konumları haritasını görüntüleyerek saldırı düzenlerini tanımlayabilmenizi sağlar.

-   **Ortak Güvenlik sorguları**: Ortamınızı izlemek için kullanabileceğiniz en sık kullanılan güvenlik sorguları listeler. Herhangi bir sorguyu seçtiğinizde, arama bölmesini açar ve bu sorgu için sonuçları görüntüler.

### <a name="insight-and-analytics"></a>İçgörü ve analiz
Merkezine [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) Azure tarafından barındırılan deposu bulunur.

![İçgörü ve analiz diyagramı](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Veri deposuna bağlı kaynaklardan veri kaynakları yapılandırılarak ve aboneliğinize çözümler ekleyerek toplayın.

![Azure izleme günlükleri Panosu](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Veri kaynakları ve çözümleri kendi özellik kümesine sahip ayrı bir kayıt türleri oluşturma, ancak yine de bunları birlikte depoya sorgularda analiz gerçekleştirebilirsiniz. Çeşitli çeşitli kaynaklar tarafından toplanan veriler ile çalışmak için aynı araçları ve yöntemleri kullanın.


Azure İzleyici günlüklerine etkileşiminizi çoğunu olan herhangi bir tarayıcıda çalışan ve yapılandırma ayarlarını erişmenizi ve çok sayıda araç çözümlemenizi ve kullanmanızı sağlayacak toplanan verilerin sağlayan Azure portalı üzerinden. Portaldan şunları kullanabilirsiniz:
* [Günlük aramaları](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) oluşturmak burada toplanan verileri analiz etmek için sorgular.
* [Panolar](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards), en değerli Aramalarınızın grafik görünümleriyle özelleştirebileceğiniz.
* [Çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), ek işlevlerle analiz araçları sağlar.

![Analiz Araçları](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Çözümleri, Azure İzleyici günlüklerine işlevsellik ekler. Bunlar birincil bulutta çalıştırın ve log analytics deposunda toplanan verilerin analizi sağlar. Günlük aramaları veya log analytics panosunda çözüm sağlayan bir ek kullanıcı arabirimi kullanılarak çözümlenebilecek toplanacak yeni kayıt türlerinin çözümleri de tanımlayabilirsiniz.

Güvenlik ve Denetim Panosu, bu tür çözümler örneğidir.

### <a name="automation-and-control-alert-on-security-configuration-drifts"></a>Otomasyon ve Denetim: Güvenlik Yapılandırma drifts hakkında uyar

Azure Otomasyonu, PowerShell tabanlı ve bulutta çalışan runbook'ları ile yönetim işlemlerini otomatikleştirir. Runbook'lar yerel kaynakların yönetilmesi için yerel veri merkezinizdeki bir sunucuda da yürütülebilir. Azure Otomasyonu ile PowerShell Desired State Configuration (DSC) yapılandırma yönetimi sağlar.

![Azure Otomasyon diyagramı](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Oluşturun ve Bulut ve şirket içi sistemlere uygulayabilirsiniz ve Azure'da barındırılan DSC kaynaklarını yönetin. Tanımlayabilir ve otomatik olarak yapılandırmalarını zorunlu ya da bu güvenliğini sağlamaya yardımcı olmak için kayması raporları almak için yaparak yapılandırmaları ilke içinde kalır.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızı korumanıza yardımcı olur. Bu, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Hizmetinde tanımladığınız her iki Azure aboneliklerinize karşı ilkeleri ve [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) daha fazla ayrıntı için.

![Azure Güvenlik Merkezi diyagramı](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Bunlar Microsoft’un bulut ve şirket içindeki genel varlığından edinilen kapsamlı bir telemetri kümesine erişebilmektedir. Geniş kapsamlı ve çeşitlilik barındıran bu veri kümeleri Microsoft’un yeni saldırı modellerini ve şirket içi müşteri ve kuruluş ürünlerinin yanı sıra çevrimiçi hizmetleri üzerindeki eğilimleri keşfetmesini sağlamaktadır.

Bu nedenle, saldırganlar yeni ve giderek karmaşık hale gelen sömürülerini ortaya çıkardıkça Güvenlik Merkezi algılama algoritmalarını hızlı bir şekilde güncelleştirebilirsiniz. Bu yaklaşım hızlı ilerleyen bir tehdit ortama ayak uydurmanıza yardımcı olur.

![Güvenlik Merkezi tehdit algılama](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini otomatik olarak toplayarak çalışır. Bu, tehditleri tanımlamak için birden fazla kaynaktan bilgileri ilişkilendirerek, bu bilgileri analiz eder.

Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri teknolojilerindeki ve [makine öğrenimi](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojileri, tüm bulut yapısındaki olayları değerlendirmek için kullanılır. Gelişmiş analiz elle yaklaşımlar ve saldırıların gelişimi tahmin edilerek belirlenmesi imkansız olan tehditleri algılayabilir. Bu güvenlik analizi türleri, sonraki bölümde ele alınmıştır.

### <a name="threat-intelligence"></a>Tehdit bilgileri

Microsoft küresel tehdit zekasını büyük miktarda erişebilir.

Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft dijital Suçlar birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden çok kaynaktan, telemetri akar.

![Tehdit zekası bulguları](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Araştırmacılar ayrıca büyük bulut hizmeti sağlayıcıları arasında paylaşılan tehdit bilgilerini alır ve bunlar için tehdit bilgileri akışlarından Üçüncü taraflardan abone olun. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir. Bazı örnekler:

-   **Machine learning gücünden yararlanma**: Azure Güvenlik Merkezi, Azure dağıtımlarınızı hedefleyen tehditleri algılamak için kullanılan bulut ağ etkinliğiyle ilgili yüksek miktarda veri erişimi vardır.

-   **Deneme yanılmayı algılama**: Makine öğrenimi, deneme yanılma saldırıları güvenli Kabuk (SSH), Uzak Masaüstü Protokolü (RDP) ve SQL bağlantı noktalarına izin veren geçmiş bir uzaktan erişim girişimlerini düzeni oluşturmak için kullanılır.

-   **Giden DDoS ve botnet algılama**: Ortak bir amacı bulut kaynaklarını hedefleyen saldırılara, bu kaynakları işlem gücünü diğer saldırılar kullanmaktır.

-   **Yeni davranış analizi sunucuları ve Vm'leri**: Bir sunucu veya sanal makine tehlikeye sonra çok çeşitli teknikleri algılama önleme, sağlayarak Kalıcılık ve güvenlik denetimleri catch'te çalışırken bu sistemde kötü amaçlı kod yürütmek için saldırganlar tespit edilmeden.

-   **Azure SQL veritabanı tehdit algılama**: Erişim veritabanı açıklıklarından yararlanmaya yönelik sıra dışı ve zararlı gösteren anormal veritabanı etkinliklerini belirler Azure SQL veritabanı tehdit algılama çalışır.

### <a name="behavioral-analytics"></a>Davranış analizi

Davranış analizi, verileri analiz eden ve bilinen modeller koleksiyonuyla karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir.

![Davranış analizi bulguları](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)

Desenler, ayrıca kötü amaçlı davranışların çözümlenmesiyle Uzman analistler tarafından belirlenir. Azure Güvenlik Merkezi, davranışsal analiz, sanal makine günlükleri, sanal ağ cihaz günlükleri, yapı günlükleri, kilitlenme bilgi dökümleri ve diğer kaynakları Analizine göre tehlike giren kaynakları belirlemek için kullanabilirsiniz.

Ayrıca, desenleri, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle ilişkilendirilir. Bu bağıntı yerleşik tehlike göstergeleriyle tutarlı olayları tanımlamaya yardımcı olur.

Bazı örnekler:
-   **Şüpheli işlem yürütme**: Saldırganlar tespit edilmeden kötü amaçlı yazılım yürütmeye yönelik çeşitli teknikler kullanmaktadır. Örneğin, bir saldırganın kötü amaçlı yazılıma yasal sistem dosyalarıyla aynı adı verip ancak bu dosyaları alternatif konumlara yerleştirebilir, iyi amaçlı bir dosyaya benzer bir ad kullanın veya dosyanın gerçek uzantısını maskeleyebilir. Güvenlik Merkezi modelleri, davranışları ve bunlar gibi aykırı değerleri algılamak için izleme işlem yürütme işlemi.

-   **Gizli kötü amaçlı yazılım ve istismarı denemeleri**: Karmaşık kötü amaçlı yazılımlar diske hiçbir zaman yazmayarak veya diske depolanmış yazılım bileşenlerini şifreleyerek geleneksel kötü amaçlı yazılımdan koruma ürünlerini atlatabilir. Kötü amaçlı yazılım, izlemeleri bellekte işleve bırakmak zorunda olduğundan ancak bu tür bellek analizi kullanılarak algılanabilir. Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Kilitlenme bellek analiz ederek Azure Güvenlik Merkezi yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve performansını etkilemeden bir makineye gizlice kalıcı için kullanılan teknikleri algılayabilir, Makine.

-   **Yana hareket ve iç keşif**: Tehlikeye giren bir ağda kalıcı hale getirmek ve bulun ve değerli verileri Hasat için saldırganlar genellikle riskli makineden başkalarının aynı ağdaki hareket etmeye çalışır. Güvenlik Merkezi, bir saldırganın ağda kapladığı uzaktan komut yürütme, ağ araştırma ve hesap numaralandırma gibi ağ yeri genişletme denemelerini bulmak için işlemi ve oturum açma etkinliklerini izler.

-   **Kötü amaçlı PowerShell betikleri**: PowerShell çeşitli amaçlarla hedef sanal makinelerde kötü amaçlı kod yürütülecek saldırganlar tarafından kullanılabilir. Güvenlik Merkezi şüpheli etkinliklerin kanıtı için PowerShell etkinliğini inceler.

-   **Giden saldırılar**: Saldırganlar genellikle bulut kaynaklarını ek saldırılar yerleştirmek üzere kullanma amacıyla bulut kaynaklarını hedefler. Diğer sanal makinelere karşı deneme yanılma saldırıları başlatmak, istenmeyen posta göndermek veya açık bağlantı noktaları ve internet üzerindeki diğer cihazları taramak için riskli sanal makineler gibi kullanılıyor olabilir. Ağ trafiğine machine learning uygulayan Güvenlik Merkezi giden ağ iletişimlerinin normu aştığını algılayabilir. İstenmeyen posta algılandığında, Güvenlik Merkezi ayrıca olağandışı e-posta trafiği e-posta olup olmadığını belirlemek için Office 365'ten ile ilişkilendirir alınan veya bir meşru e-posta kampanyasının sonucu.

### <a name="anomaly-detection"></a>Anormallik algılama

Azure Güvenlik Merkezi, tehditleri tanımlamak için anormallik algılamayı da kullanır. Davranış analizinden (büyük veri kümelerinden türetilmiş bilinen modellere bağlıdır) farklı olarak anormallik algılama daha fazla “kişiselleştirilmiştir” ve dağıtımlarınıza özel taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliğini belirlemek için Machine learning uygulanır ve ardından kuralları bir güvenlik olayını gösterebilecek aykırı değer koşullarını tanımlamak için oluşturulur. Örnek aşağıda verilmiştir:

-   **Gelen RDP/SSH deneme yanılma saldırıları**: Dağıtımlarınız her gün çok sayıda oturum açılmayan sanal makineler ve birkaç varsa oturum açma bilgilerine sahip diğer sanal makineler olabilir. Azure Güvenlik Merkezi, bu sanal makineler için taban çizgisi oturum açma etkinliğini belirler ve machine learning normal oturum açma etkinlikleri tanımlamak kullanın. İçin tanımlanan temeli ile herhangi bir tutarsızlık varsa oturum açma ilgili özellikleri, bir uyarı oluşturulabilir. Yine machine learning neyin önemli olduğunu belirler.

### <a name="continuous-threat-intelligence-monitoring"></a>Sürekli tehdit bilgisi izleme

Azure Güvenlik Merkezi, tehdit kapsamındaki değişiklikleri sürekli olarak izleyen güvenlik araştırması ve veri bilimi ekipleri tüm dünyada ile çalışır. Buna aşağıdaki girişimler dahildir:

-   **Tehdit bilgisi izleme**: Tehdit zekası mekanizmaları, Göstergeler, uygulamaları ve var olan veya yeni ortaya çıkan tehditlere eyleme dönüştürülebilir öneriler içerir. Bu bilgileri güvenlik topluluğu içinde paylaşılır ve Microsoft iç ve dış kaynaklardan gelen tehdit bilgisi akışlarını sürekli olarak izler.

-   **Sinyal paylaşımı**: Güvenlik Öngörüler bulut geniş çaplı Microsoft Portföyü genelinde ekiplerinin ve şirket içi hizmetler, sunucular ve istemci uç noktası cihazları paylaşılan ve analiz edilir.

-   **Microsoft Güvenlik uzmanları**: Devamlı etkileşim hukuk ve web saldırıcı algılama gibi Uzman güvenlik alanlarında çalışan ekiplerle Microsoft arasında.

-   **Algılama ayarı**: Gerçek müşteri veri kümelerine göre algoritmalar çalıştırılır ve güvenlik Araştırmacıları sonuçları doğrulamak üzere müşterilerle. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleşik çabalar, anında yararlanabilir, yeni ve geliştirilmiş algılamalarla sonuçlanır sizin. Hiçbir işlem yapmanıza gerek yoktur.

## <a name="advanced-threat-detection-features-other-azure-services"></a>Gelişmiş tehdit algılama özellikleri: Diğer Azure Hizmetleri

### <a name="virtual-machines-microsoft-antimalware"></a>Sanal makineler: Microsoft kötü amaçlı yazılımdan koruma

[Microsoft kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure uygulamaları ve Kiracı ortamları için bir tek aracı çözümü için insan müdahalesi olmadan arka planda çalıştırmak için tasarlanmıştır. İle ya da temel güvenli varsayılan olarak, uygulama iş yüklerinin gereksinimlerini temel veya Gelişmiş kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere, özel bir yapılandırma koruması dağıtabilirsiniz. Azure kötü amaçlı yazılımdan koruma otomatik olarak tüm Azure PaaS sanal makinelerde yüklü olan bir güvenlik Azure sanal makineler için bir seçenektir.

#### <a name="microsoft-antimalware-core-features"></a>Microsoft kötü amaçlı yazılımdan koruma temel özellikleri

Dağıtma ve uygulamalarınız için Microsoft kötü amaçlı yazılımdan koruma etkinleştiren Azure özelliklerinin şunlardır:

-   **Gerçek zamanlı koruma**: Bulut Hizmetleri ve sanal makinelere algılanmasına ve kötü amaçlı yazılım çalıştırma etkinliği izler.

-   **Zamanlanmış tarama**: Düzenli olarak etkin olarak çalışan programlar da dahil olmak üzere kötü amaçlı yazılımları algılamak için hedeflenen tarama yapar.

-   **Kötü amaçlı yazılım düzeltme**: Otomatik olarak algılanan kötü amaçlı yazılım siliniyor veya kötü amaçlı dosyaları karantinaya ve kötü amaçlı kayıt defteri girdilerini temizleme gibi üzerinde çalışır.

-   **İmza güncelleştirme**: Otomatik olarak koruma üzerinde önceden belirlenmiş bir sıklık güncel olduğundan emin olmak için en son (virüs tanımları) koruması imzaları yükler.

-   **Kötü amaçlı yazılımdan koruma Altyapısı güncelleştirmeleri**: Microsoft Antimalware Engine otomatik olarak güncelleştirir.

-   **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: Microsoft kötü amaçlı yazılımdan koruma platformu otomatik olarak güncelleştirir.

-   **Etkin koruma**: Algılanan Tehditler ve şüpheli kaynaklar Microsoft Azure için Microsoft active koruma sistemi üzerinden gerçek zamanlı zaman uyumlu imzası teslim etkinleştirme geliştirilmekte olan tehdit ortamını daha hızlı yanıt emin olmak için ilgili raporlar telemetri meta verileri.

-   **Örnekleri raporlama**: Sağlar ve hizmeti geliştirmek ve sorun giderme etkinleştirmek yardımcı olmak için Microsoft kötü amaçlı yazılımdan koruma hizmeti örnekleri raporlar.

-   **Dışlamalar**: Uygulama ve hizmet yöneticileri belirli dosyaları, süreçleri ve sürücüleri korumadan dışarıda bırakma ve performans ve diğer nedenlerle için tarama yapılandırmak sağlar.

-   **Kötü amaçlı yazılımdan koruma olay toplama**: Kötü amaçlı yazılımdan koruma hizmet durumu, şüpheli etkinlikleri ve işletim sistemi olay günlüğünde gerçekleştirilen düzeltme eylemleri kaydeder ve onları müşterinin Azure depolama hesabına toplar.

### <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

[Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) Azure SQL veritabanı hizmetine yerleşik yeni bir güvenlik zekası özelliğidir. Bilgi edinin, profil ve anormal veritabanı etkinliklerini öğrenir sistemlerimizdeki çalışma, Azure SQL veritabanı tehdit algılama olası tehditlerle tanımlar.

Oluşunca güvenlik sorumluları veya diğer atanmış yöneticileri şüpheli veritabanı etkinlikleriyle ilgili anında bildirim alabilirsiniz. Her bir bildirim, şüpheli etkinlik ayrıntılarını sunan ve daha fazla araştırmak ve tehdidi azaltmak nasıl önerir.

Şu anda Azure SQL veritabanı tehdit algılama olası güvenlik açıklarına ve SQL ekleme saldırıları ve anormal veritabanı erişim modellerinin algılar.

Tehdit algılama e-posta bildirimi aldıktan sonra kullanıcılar gidin ve derin bir bağlantıyla ilgili denetim kayıtlarını postada görüntüleyebilir. Bağlantı bir denetim Görüntüleyici veya ilgili denetim kayıtlarını ve şüpheli olayın zamanına yakın gösteren önceden yapılandırılmış bir denetim Excel şablonu aşağıdakilere göre açar:

-   Anormal veritabanı etkinliklerini veritabanı/sunucusu için depolama denetim.

-   Olayın zaman Denetim günlüğüne yazmak için kullanılan depolama tablosu ilgili denetim.

-   Denetim kayıtları olay oluşum takip saat.

-   (Bazı algılayıcıları için isteğe bağlı) olayın zaman denetim kayıtları benzer bir olay kimliği.

SQL veritabanı tehdit algılayıcıları aşağıdaki algılama yöntemleri birini kullanın:

-   **Belirlenimci algılama**: Şüpheli kalıpları (temel kurallar) bilinen saldırıları eşleşen SQL istemci sorgularda algılar. "Atomik algılamalar." kategoride kaldığından bu metodolojide yüksek olan algılama ve düşük yanlış pozitif, ancak sınırlı kapsamı vardır.

-   **Davranış algılama**: Anormal davranış en son 30 gün boyunca görülmemiş veritabanında anormal etkinlik algılar. SQL istemci anormal etkinlik örnekleri, bir depo başarısız oturum açma bilgileri veya sorguları, yüksek hacimli verileri ayıklanırken, olağan dışı kurallı sorguları veya veritabanına erişmek için kullanılan tanınmayan IP adresleri olabilir.

### <a name="application-gateway-web-application-firewall"></a>Application Gateway Web uygulaması güvenlik duvarı

[Web uygulaması Güvenlik Duvarı (WAF)](../app-service/environment/app-service-app-service-environment-web-application-firewall.md) özelliğidir [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) için standart bir uygulama ağ geçidi kullanan web uygulamalarına koruma sağlayan [uygulama teslim denetimi](https://kemptechnologies.com/in/application-delivery-controllers) işlevleri. Web uygulaması güvenlik duvarı bunu yapar çoğunu karşı koruyarak [açık Web uygulaması güvenlik Project (OWASP) en yaygın 10 web güvenlik açığının](https://www.owasp.org/index.php/Top_10_2010-Main).

![Application Gateway Web uygulaması güvenlik duvarı diyagramı](./media/azure-threat-detection/azure-threat-detection-fig13.png)

Korumalar içerir:

-   SQL ekleme koruması.

-   Site komut dosyası koruması arası.

-   Yaygın Web saldırıları koruması, komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi.

-   HTTP protokolü ihlallerine karşı koruma.

-   Eksik gibi HTTP protokolü anormalliklerine karşı koruma, konak kullanıcısı-aracısı ve kabul üst bilgileri.

-   Robotlar, gezginler ve tarayıcıları önleme.

-   Yaygın yanlış uygulama yapılandırmalarını (diğer bir deyişle, Apache, IIS vb.) algılama.

WAF application gateway'iniz yapılandırma aşağıdaki avantajları sağlar:

-   Web uygulamanızı web güvenlik açıklarına ve arka uç kod değişikliği olmadan saldırıları korur.

-   Bir application Gateway'in arkasında aynı anda birden çok web uygulamaları korur. Bir uygulama ağ geçidi 20 adede kadar Web sitesini barındırmayı destekler.

-   Uygulama ağ geçidi WAF günlükleri tarafından oluşturulan gerçek zamanlı raporları kullanarak web uygulamaları saldırılarına karşı izleyiciler.

-   Yardımcı uyumluluk gereksinimlerini karşılayın. Bazı uyumluluk denetimleri, tüm internet'e yönelik uç noktalar bir WAF çözümü tarafından korunmasını gerektirir.

### <a name="anomaly-detection-api-built-with-azure-machine-learning"></a>Anomali algılama API'si: Azure Machine Learning ile oluşturulmuş

Anomali algılama API'si, zaman serisi verilerinizdeki çeşitli anormal desenleri algılamak için yararlı bir API'dir. API uyarılar oluşturmak için kullanılabilecek zaman serisindeki her bir veri noktasına bir anomali puanı atar panolar aracılığıyla izlemek ya da bilet oluşturma Sistemlerinizle bağlanma.

[Anomali algılama API'sini](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) şu zaman serisi verilerinde anomali türlerini algılayabilir:

-   **Ani artışlar ve düşüşler**: Ne zaman bir hizmet için oturum açma hatalarının sayısı veya almaların beklenmedik ani bir e-ticaret sitesinde izleme yapıyorsanız veya güvenlik saldırıları veya hizmet kesintileri yaşandığını gösterebilir.

-   **Pozitif ve negatif eğilimler**: Bellek kullanımı izlerken boş bellek boyutunun azalması olası bir bellek sızıntısı olduğunu gösterir. İzleme hizmet kuyruğunun uzunluğu için temel bir yazılım sorunu kalıcı bir yukarı yönlü eğilim gösteriyor olabilir.

-   **Düzey değişiklikleri ve değerlerin dinamik aralığındaki değişiklikler**: Gecikme sonra hizmet yükseltme veya yükseltme izlenmesi ilginç olabilir sonra özel durum düzeylerinin daha düşük bir hizmet düzeyi değişir.

Makine öğrenme tabanlı API sağlar:

-   **Esnek ve güçlü bir algılama**: Anomali algılama modelleri duyarlılık ayarlarını yapılandırmak ve dönemsel ve mevsimsel olmayan veri kümesi arasında anormallikleri açmasına imkan tanıyın. Kullanıcılar, anomali algılama modelini algılama API'si ihtiyaçlarına göre daha fazla veya daha az duyarlı olmasına ayarlayabilirsiniz. Veri olmadan dönemsel desenleri ve daha az veya daha fazla görünür anormallikleri saptamak anlamına gelir.

-   **Ölçeklenebilir ve hızlı algılama**: Mevcut eşik değeriyle izleme geleneksel uzmanlar etki alanı bilgisini belirlediği pahalı ve veri kümeleri dinamik olarak değiştirme milyonlarca ölçeklenebilir. Bu API anomali algılama modellerinde öğrenilen ve modelleri geçmişe dönük ve gerçek zamanlı verilerden otomatik olarak ayarlanmıştır.

-   **Proaktif ve eyleme dönüştürülebilir algılama**: Yavaş eğilim ve düzeyi değişiklik algılama erken anomali algılama için uygulanabilir. Algılanan erken anormal sinyaller araştırmak ve sorunlu alanları üzerinde işlem yapma insanlar yönlendirmek için kullanılabilir. Ayrıca, kök neden analiz modelleri ve uyarı araçları bu anomali algılama API'si hizmeti üzerinde geliştirilebilir.

Anomali algılama API'si için çok çeşitli hizmet durumu ve KPI izleme, IOT, performans izleme ve ağ trafiğini izleme gibi senaryoları etkili ve verimli bir çözümdür. Burada, bu API yararlı olabilir bazı yaygın senaryolar aşağıda verilmiştir:

- BT departmanları zamanında izleme olayları, hata kodu, günlük kullanım ve performans (CPU, bellek vb.) araçları gerekir.

-   Çevrimiçi Ticaret sitelerini müşteri etkinlikleri, sayfa görüntülemeleri, tıklama ve benzeri izlemek istersiniz.

-   Yardımcı program şirketler, su, doğalgaz, elektrik ve diğer kaynakların tüketimini izlemek istiyorsunuz.

-   Tesis veya yapı Yönetimi Hizmetleri sıcaklık, nem, trafik ve benzeri izlemek istersiniz.

-   IOT/üreticiler, sensör verilerini zaman serisi izleme iş akışı, kalite ve benzeri için kullanmak istediğiniz.

-   Hizmet sağlayıcıları, çağrı merkezleri gibi hizmet talebi eğilimi, olay hacmi izlemek için bekleme kuyruğu uzunluğu vb.

-   İş analizi grupları iş KPI'leri (örneğin, satış hacmini, müşteri yaklaşımları veya fiyatlandırma) izlemek istediğiniz gerçek zamanlı anormal taşıma.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) Microsoft Cloud Security yığınının kritik bir bileşenidir. Bu promise bulut uygulamalarının tüm avantajlarından yararlanabilmek hareket ederken, kuruluşunuzun yardımcı olabilecek kapsamlı bir çözümdür. Bu, Gelişmiş etkinlik görünürlüğü sağlayarak denetimi tutar. Ayrıca, bulut uygulamaları genelinde kritik veri korumasını artırmaya da yardımcı olur.

Kuruluşunuz, gölge BT’yi ortaya çıkarmaya, riskleri değerlendirmeye, ilkeleri zorunlu tutmaya, etkinlikleri araştırmaya ve tehditleri durdurmaya yardımcı olan araçlarla kritik verilerin denetimini elde tutarak buluta daha güvenli bir şekilde geçebilir.

| | |
|---|---|
| Keşif | Cloud App Security ile gölge BT'yi ortaya çıkarın. Uygulamaları, etkinlikleri, kullanıcıları, veri ve bulut ortamınızda dosyaları bularak görünürlük kazanın. Bulutunuzla bağlantılı üçüncü taraf uygulamalarını keşfedin.|
|Araştırma | Riskli uygulamaları, belirli kullanıcıları ve ağınızdaki dosyaları derinlemesine incelemek üzere bulut adli bilişim araçlarını kullanarak bulut uygulamalarınızı araştırın. Bulutunuzdan toplanan verilerdeki modelleri bulun. Bulutunuzu izlemek için raporlar oluşturur. |
| Denetim | İlkeleri ve ağ bulut trafiği üzerinde maksimum denetim elde etmek için uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için cloud App Security'yi kullanın. |
| koruma | Cloud App Security'yi kullanarak tasdik veya uygulamaları engelle, veri kaybını önleme, izinleri ve paylaşımı denetleyin zorlamak ve özel raporlar ve uyarılar oluşturun. |
| Denetim | İlkeleri ve ağ bulut trafiği üzerinde maksimum denetim elde etmek için uyarılar ayarlayarak riski azaltın. Kullanıcılarınızı güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için cloud App Security'yi kullanın. |
| | |


![Cloud App Security diyagramı](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security, bulutunuz ile görünürlüğü tümleştirilebilir:

-   Harita ve bulut ortamınızın ve kullanımdaki bulut uygulamalarınızın tanımlamak için cloud Discovery kullanarak kuruluşunuzun kullanıyor.

-   Tasdik etme ve bulut uygulamalarında yasaklanması.

-   görünürlüğü ve İdaresi için bağlanan uygulamalar için sağlayıcı API'lerinden yararlanan, dağıtımı kolay uygulama bağlayıcıları kullanır.

-   ayar ve sürekli olarak ince ayar yapmak, ilkeleri tanıyarak sürekli denetime sahip yardımcı olur.

Bu kaynaklardan veri toplama işlemini, Cloud App Security Gelişmiş bir analiz üzerinde çalışır. Hemen anormal etkinlikler için uyarı ve bulut ortamınızda derin görünürlük sunar. Cloud App Security'de bir ilke yapılandırın ve bulut ortamınızdaki her şeyi koruyabilirsiniz kullanın.

## <a name="third-party-advanced-threat-detection-capabilities-through-the-azure-marketplace"></a>Azure Marketi aracılığıyla üçüncü taraf Gelişmiş tehdit algılama özellikleri

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web uygulaması güvenlik duvarı gelen web trafiğini inceler ve SQL eklemelerini, siteler arası betik, kötü amaçlı yazılım yüklemelerini, uygulama DDoS saldırılarına ve web uygulamalarınızı hedefleyen diğer saldırıları engeller. Ayrıca, veri kaybı önleme (DLP) için arka uç web sunucularından gelen yanıtları denetler. Tümleşik erişim denetimi altyapısı ayrıntılı erişim denetimi ilkeleri kimlik doğrulama, yetkilendirme, oluşturmak yöneticileri etkinleştirir ve (AAA) hesap, kuruluşların güçlü kimlik doğrulama ve kullanıcı denetimi sağlar.

Web uygulaması güvenlik duvarı aşağıdaki avantajları sağlar:

-   Algılar ve SQL eklemelerini, siteler arası betik, kötü amaçlı yazılım yüklemelerini, uygulama DDoS veya uygulamanızın diğer saldırıları engeller.

-   Kimlik doğrulaması ve erişim denetimi.

-   Hassas verileri algılamak ve maske veya bilgilerinin sızmasını engellemek için giden trafiği tarar.

-   Önbelleğe alma, sıkıştırma ve diğer trafik iyileştirmeleri gibi özellikleri kullanarak web uygulamasını içerik teslimini hızlandırır.

Azure Market'te kullanıma sunulan web uygulaması güvenlik duvarları örnekleri için bkz: [Barracuda WAF, Brocade sanal web uygulaması Güvenlik Duvarı (vWAF), Imperva SecureSphere ve ThreatSTOP IP Güvenlik Duvarı](https://azuremarketplace.microsoft.com/marketplace/apps/barracudanetworks.waf).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi algılama özellikleri](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities): Hızla yanıt vermeniz Öngörüler sağlar ve Azure kaynaklarınızı hedefleyen etkin tehditleri tanımlamanıza yardımcı olacak.

- [Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/): Yardımcı olur, veritabanlarınızı potansiyel tehditlerle ilgili endişelere.
