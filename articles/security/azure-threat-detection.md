---
title: "Azure Gelişmiş tehdit algılama | Microsoft Docs"
description: "Kimlik koruması ve yeteneklerini öğrenin."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: d2fab26d8ff9f006cfed82685a738b791d0b0624
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="azure-advanced-threat-detection"></a>Azure Gelişmiş tehdit algılama
## <a name="introduction"></a>Giriş

### <a name="overview"></a>Genel Bakış

Microsoft, teknik incelemeler, güvenlik genel bakışlar, en iyi yöntemler ve denetim listeleri çeşitli güvenlikle ilgili özellikleri hakkında bulunan Azure müşterileri ve Azure platformu çevreleyen yardımcı olmak üzere bir dizi geliştirmiştir. Konular avantajlarına ve derinliği bakımından aralığı ve düzenli aralıklarla güncelleştirilir. Bu belge aşağıdaki soyut bölümünde özetlenen serisi bir parçası değil.

### <a name="azure-platform"></a>Azure platformu

Azure işletim sistemlerinin programlama dilleri, çerçeveleri, Araçlar, veritabanları ve cihazları, en geniş seçim destekleyen bir genel bulut hizmeti platformudur.
Aşağıdaki programlama dillerini destekler:
-   Linux kapsayıcıları Docker Tümleştirmesi ile çalıştırın.
-   JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma
-   Yapı geri-iOS, Android ve Windows cihazları sona erer.

Azure genel bulut Hizmetleri aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Ne zaman kuruluş verilerinizin korunmasına ve güvenlik ve sistem geçici idare sağlamak için sorumlu olduğu bir kuruluş olan ortak bir buluta geçirdiğiniz.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Azure çok çeşitli yapılandırmak ve güvenlik, uygulama dağıtımları gereksinimlerini karşılamak için özelleştirmek için seçenekler sağlar. Bu belge, bu gereksinimleri karşılayan yardımcı olur.

### <a name="abstract"></a>Soyut

Microsoft Azure teklifleri Hizmetleri aracılığıyla Gelişmiş tehdit algılama işlevselliği yerleşik Azure Active Directory, Azure Operations Management Suite (OMS) ve Azure Güvenlik Merkezi gibi. Bu koleksiyon güvenlik hizmetlerini ve özellikleri Azure dağıtımlarınızı içinde neler olduğunu anlamak için basit ve hızlı bir yol sağlar.

Bu teknik incelemesindeki "Microsoft Azure yaklaşıyor" tehdit güvenlik açığı tanılama yönlendirecek ve riski analiz sunucuları ve diğer Azure kaynaklarına karşı hedeflenen kötü amaçlı etkinliği ile ilişkili. Bu, kimlik ve güvenlik açığı yönetimi en iyi duruma getirilmiş çözümleriyle yöntemleri Azure platformu ve müşteri dönük güvenlik hizmetleri ve teknolojileri tarafından belirlemenize yardımcı olabilir.

Bu teknik incelemede Azure platformu ve müşteri dönük denetimlerin teknolojisine odaklanır ve SLA'ları, fiyatlandırma modelleri ve DevOps yöntem değerlendirmeleri adresine denemez.

## <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

![Azure Active Directory Kimlik Koruması](./media/azure-threat-detection/azure-threat-detection-fig1.png)


[Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) özelliğidir [Azure AD Premium P2](https://docs.microsoft.com/azure/active-directory/active-directory-editions) edition risk olaylarına ve olası güvenlik açıklarını kuruluşunuzdaki kimlikleri etkileyen genel bir bakış sağlar. İçin bulut tabanlı kimlikleri on Microsoft güvenliğini sağlama ve Azure AD kimlik koruması ile Microsoft bu aynı koruma sistemleri kullanılabilir Kurumsal müşteriler için yapılmasıdır. Kimlik koruması kullanan mevcut Azure AD anomali algılama özellikleri aracılığıyla kullanılabilen [Azure AD anormal etkinlik raporları](https://docs.microsoft.com/azure/active-directory/active-directory-view-access-usage-reports#anomalous-activity-reports)ve gerçek zamanlı anormallikleri algılayabilir yeni risk olayı türleri sunar.

Kimlik koruması anormallikleri algılayıp Kimlikteki tehlikede olduğunu gösterebilecek olayları risk Uyarlamalı machine learning algoritmaları ve buluşsal yöntemler kullanır. Bu verileri kullanarak, kimlik koruması, raporlar ve bu risk olayları araştırmanıza ve uygun düzeltme veya azaltma eylemi alın etkinleştirmek uyarılar oluşturur.

Ancak Azure Active Directory kimlik koruması birden çok bir izleme ve raporlama aracıdır. Risk olaylara göre risk tabanlı ilkeleri, kuruluşunuzun kimlikleri otomatik olarak korunacak yapılandırmanızı sağlayacak şekilde her bir kullanıcı için bir kullanıcı risk düzeyi kimlik koruması hesaplar.

Diğer yanı sıra bu risk tabanlı ilkeleri [koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) Azure Active Directory tarafından sağlanan ve [EMS](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access), otomatik olarak engelleyebilir veya dahil Uyarlamalı düzeltme eylemleri sunar parola sıfırlama ve çok faktörlü kimlik doğrulaması zorlama.

### <a name="identity-protections-capabilities"></a>Identity Protection'ın özellikleri

Azure Active Directory kimlik koruması izleme ve Raporlama Aracı büyük. Kuruluşunuzdaki kimlikleri korumak için belirtilen risk düzeyi erişildiğinde otomatik olarak algılanan sorunları yanıt risk tabanlı ilkeleri yapılandırabilirsiniz. Azure Active Directory ve EMS tarafından sağlanan diğer koşullu erişim denetimlerini yanı sıra bu ilkeleri otomatik olarak engellemek ya da dahil olmak üzere parola sıfırlamaları Uyarlamalı düzeltme eylemleri ve çok faktörlü kimlik doğrulaması zorlama başlatın.

Hesaplarınızı Azure kimlik koruması yardımcı olabilecek yollardan bazılarını örnekleri güvenli ve kimlikler şunları içerir:

[Algılama risk olaylarına ve riskli hesapları:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#detection)
-   Machine learning ve buluşsal kurallarını kullanarak altı risk olayı türleri algılama
-   Kullanıcı risk düzeyleri hesaplama
-   Genel güvenlik duruşunu güvenlik açıkları vurgulayarak güvenliğini geliştirmek için özel öneriler sağlama

[Risk olaylarını araştırma:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#investigation)
-   Risk olayları için bildirimleri gönderme
-   Risk olaylarını ilgili ve bağlamsal bilgileri kullanarak araştırma
-   Araştırmalar izlemek için temel iş akışı sağlama
-   Parola sıfırlama gibi düzeltme eylemleri kolay erişim sağlama

[Risk bağlı olarak koşullu erişim ilkeleri:](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection#risky-sign-ins)
-   Oturum açma işlemleri engelleme veya çok faktörlü kimlik doğrulaması zorluklarını gerektirerek riskli oturum açma işlemlerini azaltmak için ilke.
-   Blok veya güvenli riskli kullanıcı hesapları için ilke
-   Çok faktörlü kimlik doğrulaması için kullanıcıların İlkesi

### <a name="azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM)

İle [Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure),

![Azure AD Privileged Identity Management](./media/azure-threat-detection/azure-threat-detection-fig2.png)

yönetin, denetleyin ve erişimi kuruluşunuz içinde izleyin. Azure AD'deki ve Office 365 ya da Microsoft Intune gibi diğer çevrimiçi Microsoft hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

Azure AD Privileged Identity Management yardımcı olur:

-   Bir uyarı alırsınız ve Azure AD Yöneticiler ve Office 365 ve Intune gibi Microsoft Online Services "tam zamanında" yönetimsel erişimi hakkında rapor oluşturun.

-   Yönetici erişim geçmişine ve değişiklikler hakkında raporlar yönetici atamaları alma

-   Ayrıcalıklı bir rol için erişim hakkında uyarı alın

## <a name="microsoft-operations-management-suite-oms"></a>Microsoft Operations Management Suite (OMS)

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür. OMS, bulut tabanlı bir hizmet olarak uygulandığından altyapı hizmetlerine en az yatırımla onu kısa süre içinde çalışır duruma getirebilirsiniz. Yeni güvenlik özellikleri, devam eden bakım kaydetme otomatik olarak teslim edilir ve maliyetleri yükseltin.

Değerli hizmetleri, kendi sağlamanın yanı sıra, OMS System Center bileşenleriyle gibi tümleştirebilirsiniz [System Center Operations Manager](https://blogs.technet.microsoft.com/cbernier/2013/10/23/monitoring-windows-azure-with-system-center-operations-manager-2012-get-me-started/) mevcut güvenlik yönetimi Yatırımlar buluta genişletmek için. Tamamen karma bir yönetim deneyiminin elde edilebilmesi için System Center ve OMS birlikte çalışabilir.

### <a name="holistic-security-and-compliance-posture"></a>Bütünsel güvenlik ve uyumluluk duruş

[OMS güvenlik ve Denetim Panosu](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) kuruluşunuzun kapsamlı bir görünüme sağlar BT güvenlik tutumunu dikkat etmeniz gereken önemli sorunlar için yerleşik arama sorguları. Güvenlik ve Denetim Panosu giriş ekranı OMS güvenlikle ilgili her şeyi ' dir. Bu pano, size bilgisayarlarınızın güvenlik durumuyla ilgili yüksek düzeyde öngörü sağlar. Ayrıca son 24 saat, 7 gün veya herhangi bir özel zaman dilimine ait tüm olayları görüntüleme becerisine sahiptir.

OMS panolar hızla yardımcı olmak ve BT işlemleri dahil olmak üzere, tüm bağlamında herhangi bir ortamın genel güvenlik duruşunu kolayca anlamak: yazılım güncelleştirme değerlendirme, kötü amaçlı yazılımdan koruma değerlendirmesi ve yapılandırma temellerini. Ayrıca, güvenlik günlüğü verilerini güvenlik ve uyumluluk denetim işlemleri kolaylaştırmak için kolayca erişilebilir.

OMS Güvenlik ve Denetim panosu dört ana kategoride düzenlenmiştir:

![OMS Güvenlik ve Denetim panosu](./media/azure-threat-detection/azure-threat-detection-fig3.jpg)

-   **Güvenlik etki alanları:** Bu alanda, güvenlik kayıtlarını zaman içinde daha fazlasını keşfetmek, kötü amaçlı yazılım değerlendirmesi erişebilir, değerlendirme, ağ güvenliği, kimlik ve erişim bilgileri, bilgisayarları güvenlik olayları ile güncelleştirin ve hızlı bir şekilde sahip olacaktır Azure Güvenlik Merkezi panosunu erişin.

-   **Önem düzeyindeki sorunlar:** bu seçeneği etkin sorunlar sayısı ve bu sorunların önem hızla belirlemenize olanak tanır.

-   **Algılama (Önizleme):** kaynaklarınızı karşı yer alırken güvenlik uyarıları görselleştirme saldırı desenleri tanımlamanızı sağlar.

-   **Tehdit bilgileri:** giden kötü amaçlı IP trafik, kötü amaçlı tehdit türünü ve burada bu IP'leri geliyor gelen gösteren bir harita sunucularıyla toplam sayısı görselleştirme tarafından saldırı desenleri tanımlamanızı sağlar.

-   **Genel güvenlik sorgular:** bu seçenek, ortamınızı izlemek için kullanabileceğiniz en yaygın güvenlik sorguları listesini sağlar. Bu sorguları birine tıkladığınızda, bu sorgu için sonuçlarla arama dikey pencere açılır.

### <a name="insight-and-analytics"></a>Insight and Analytics
Ortasındaki [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) Azure bulutta barındırılan OMS deposu.

![Insight and Analytics](./media/azure-threat-detection/azure-threat-detection-fig4.png)

Veriler, veri kaynakları yapılandırılarak ve aboneliğinize çözümler eklenerek bağlı kaynaklardan depoya toplanır.

![aboneliği](./media/azure-threat-detection/azure-threat-detection-fig5.png)

Veri kaynakları ve çözümler, kendi özellikleri olan, ancak depoya yapılan sorgularda yine de birlikte analiz edilebilen farklı kayıt türleri oluşturacaktır. Böylece farklı kaynaklar tarafından toplanan farklı veri türleriyle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.


Günlük analizi ile etkileşim çoğunu olan herhangi bir tarayıcısında çalışır ve, yapılandırma ayarlarını erişimi olan ve birden çok araç çözümlemek ve hareket için toplanan verileri sağlayan OMS Portalı aracılığıyla. Portalından kullandığınız [oturum aramaları](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-searches) toplanan verileri çözümlemek için sorgular oluşturun burada [panolar](https://docs.microsoft.com/azure/log-analytics/log-analytics-dashboards), en değerli arar ve Grafikgörünümlerleözelleştirebileceğiniz[çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions), ek işlevsellik ve çözümleme araçları sağlar.

![Analiz Araçları](./media/azure-threat-detection/azure-threat-detection-fig6.png)

Çözümler, Log Analytics’e işlevler ekler. Genellikle bulutta çalışır ve OMS deposunda toplanan verilere ilişkin analiz sağlarlar. Günlük Aramaları veya OMS panosundaki çözüm tarafından sağlanan ek kullanıcı arabirimi kullanılarak çözümlenebilecek yeni kayıt türlerinin toplanmasını da belirtebilirler.
Denetim ve güvenlik çözümleri bu tür bir örnek verilmiştir.



### <a name="automation--control-alert-on-security-configuration-drifts"></a>Otomasyon ve Denetim: güvenlik yapılandırması uyarıdaki drifts

Azure Automation PowerShell üzerine dayalı olan ve Azure bulutta çalışan runbook'ları ile yönetimsel işlemlerini otomatik hale getirir. Runbook'lar yerel kaynakların yönetilmesi için yerel veri merkezinizdeki bir sunucuda da yürütülebilir. Azure Otomasyonu PowerShell DSC (İstenen durum yapılandırması) ile yapılandırma yönetimini sağlar.

![Azure Otomasyonu](./media/azure-threat-detection/azure-threat-detection-fig7.png)

Oluşturup Azure'da barındırılan DSC kaynakları yönetmek ve bunlara Bulut ve şirket içi sistemleri tanımlamak ve otomatik olarak yapılandırmalarını zorunlu veya güvenlik yapılandırmalarını ilke içinde kalmasını güvence altına yardımcı olmak için kayması raporlarda almak için geçerlidir.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızı korumanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi, Azure sağlar. Tanımlayabilir hizmet içinde yalnızca, Azure aboneliklerinize karşı aynı zamanda karşı ilkeleri [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), daha ayrıntılı olabilir.

![Azure Güvenlik Merkezi](./media/azure-threat-detection/azure-threat-detection-fig8.png)

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Bunlar Microsoft’un bulut ve şirket içindeki genel varlığından edinilen kapsamlı bir telemetri kümesine erişebilmektedir. Geniş kapsamlı ve çeşitlilik barındıran bu veri kümeleri Microsoft’un yeni saldırı modellerini ve şirket içi müşteri ve kuruluş ürünlerinin yanı sıra çevrimiçi hizmetleri üzerindeki eğilimleri keşfetmesini sağlamaktadır.

Bu nedenle, saldırganlar yeni ve giderek karmaşık ortaya çıkardıkça Güvenlik Merkezi algılama algoritmalarını hızlı bir şekilde güncelleştirebilirsiniz. Bu yaklaşım ayak ile hızlı taşıma tehdit ortamında tutmanıza yardımcı olur.

![Güvenlik Merkezi](./media/azure-threat-detection/azure-threat-detection-fig9.jpg)

Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini otomatik olarak toplayarak çalışır.  Tehditleri tanımlamak için birden fazla kaynaktan bilgileri ilişkilendirerek, bu bilgileri çözümler.
Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri sıçramalar ve [makine öğrenme](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) – elle yaklaşımlar kullanılarak ve evrimi tahmin etmeye tanımlamak imkansız olan tehditler tüm bulut yapısındaki olayları değerlendirmek için kullanılan teknolojiler saldırıları. Bu güvenlik analizleri şunları içerir.

### <a name="threat-intelligence"></a>Tehdit Bilgisi

Microsoft yoğun miktarda genel tehdit bilgisine sahiptir.
Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft dijital Suçlar birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden fazla kaynaktan, telemetri akar.

![Tehdit Bilgisi](./media/azure-threat-detection/azure-threat-detection-fig10.jpg)

Araştırmacılar ayrıca büyük bulut hizmeti sağlayıcıları ve üçüncü tarafların bilgi akışlarına abone olan kişiler arasında paylaşılan tehdit bilgilerini alır. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir. Bazı örnekler:

-   **Güç, Machine Learning - gücünden** Azure Güvenlik Merkezi veri çok büyük miktarda Azure dağıtımlarınızı hedefleme tehditleri algılamak için kullanılan bulut ağ etkinliği hakkında erişimi vardır. Örneğin:

-   **Deneme yanılma zorla algılamaların -** Machine learning SSH, RDP ve SQL bağlantı noktalarına deneme yanılma saldırıları algılamak üzere verir geçmiş bir uzaktan erişim girişimlerini desenini oluşturmak için kullanılır.

-   **Giden DDoS ve Botnet algılama** -ortak amacı bulut kaynakları hedefleme saldırıları, bu kaynakları işlem gücünü diğer saldırılara yürütmek için kullanmaktır.

-   **Yeni davranış analizi sunucularını ve Vm'leri -** bir sunucu veya sanal makine tehlikeye sonra çok çeşitli teknikleri algılama önleme, Kalıcılık olduktan ve maliyet sırasında bu sistemde kötü amaçlı kod yürütmek için saldırganlar tespit Güvenlik denetimleri.

-   **Azure SQL veritabanı tehdit algılama -** erişmek veya veritabanlarını yararlanmak tehdit algılama olağan dışı ve zararlı gösteren anormal veritabanı etkinliklerini tanımlayan Azure SQL veritabanı için çalışır.

### <a name="behavioral-analytics"></a>Davranış analizi

Davranış analizi, verileri analiz eden ve bilinen modeller koleksiyonuyla karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir.

![Davranış analizi](./media/azure-threat-detection/azure-threat-detection-fig11.jpg)


Bunlar, kötü amaçlı davranışların uzman analistler tarafından dikkatlice çözümlenmesiyle de belirlenir. Azure Güvenlik Merkezi, davranış analizi, sanal makine günlükleri, sanal ağ cihaz günlükleri, yapı günlükleri, kilitlenme bilgi dökümleri ve diğer kaynakları çözümleme göre tehlike giren kaynaklarını belirlemek için kullanabilirsiniz.

Ayrıca, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle bir bağlantı vardır. Bu bağıntı yerleşik tehlike göstergeleriyle tutarlı olayları tanımlamaya yardımcı olur.

Bazı örnekler:
-   **Şüpheli işlem yürütme:** saldırganlar tespit edilmeden kötü amaçlı yazılım yürütmek için çeşitli teknikler. Örneğin, bir saldırganın kötü amaçlı yazılım yasal sistem dosyalarıyla aynı adı vermek, ancak bu dosyaları alternatif bir konuma yerleştirin, çok zararsız dosyası gibi bir ad kullanın veya dosyanın gerçek uzantısını maskeleyebilir. Güvenlik Merkezi modelleri, davranışları işler ve işlem yürütmeleri izleyerek bunlar gibi aykırı değerleri algılar.

-   **Gizli kötü amaçlı yazılım ve istismarı denemeleri:** karmaşık kötü amaçlı yazılımlar diske hiçbir zaman yazma veya diske depolanmış yazılım bileşenlerini şifreleyerek geleneksel kötü amaçlı yazılımdan koruma ürünleri alınmadan kurtulması. Kötü amaçlı yazılım izlemeleri işleve bellekte bırakmalıdır gibi ancak, bu tür kötü amaçlı yazılım bellek analizi kullanılarak algılanabilir. Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Kilitlenme dökümündeki belleği analiz eden Azure Güvenlik Merkezi yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve gizlice performansını etkilemeden tehlikeye giren bir makineye içinde kalır için kullanılan teknikleri algılayabilir, Makine.

-   **Yanal hareket ve iç keşif:** güvenliği aşılmış bir ağ ve bulmak/toplamak değerli veri kalıcı hale getirmek için saldırganlar genellikle yanal riskli makineden başkalarına aynı ağdaki hareket etme girişimi. Güvenlik Merkezi bir saldırganın ağda uzaktan komut yürütme, ağ araştırma ve hesap numaralandırma gibi genişletme denemelerini bulmak için işlemi ve oturum açma etkinliklerini izler.

-   **Kötü amaçlı PowerShell komut dosyaları:** PowerShell çeşitli amaçlarla hedef sanal makinelerde kötü amaçlı kod yürütmek üzere saldırganlar tarafından kullanılabilir. Güvenlik Merkezi şüpheli etkinliklerin kanıtı için PowerShell etkinliğini inceler.

-   **Giden saldırılar:** saldırganlar genellikle bulut kaynaklarını ek saldırılar yerleştirmek üzere bu kaynakları kullanan amacı hedefleyin. Diğer sanal makinelere karşı deneme yanılma saldırıları başlatmak, istenmeyen posta göndermek veya açık bağlantı noktalarını ve Internet üzerindeki diğer cihazları taramak için riskli sanal makineler, örneğin, kullanılıyor olabilir. Ağ trafiğine machine learning uygulayan Güvenlik Merkezi giden ağ iletişimlerinin normu aştığını algılayabilir. Ne zaman istenmeyen posta, Güvenlik Merkezi ayrıca karşılık gelen olağandışı e-posta trafiğini posta büyük olasılıkla olup olmadığını belirlemek için Office 365'ten ile alınan veya bir meşru e-posta kampanyasının sonucu.

### <a name="anomaly-detection"></a>Anomali algılama

Azure Güvenlik Merkezi, tehditleri tanımlamak için anormallik algılamayı da kullanır. Davranış analizinden (büyük veri kümelerinden türetilmiş bilinen modellere bağlıdır) farklı olarak anormallik algılama daha fazla “kişiselleştirilmiştir” ve dağıtımlarınıza özel taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliğini belirlemek için machine learning uygulanır ve sonra bir güvenlik olayını gösterebilecek aykırı değer koşullarını tanımlamak üzere kurallar oluşturulur. Bir örneği aşağıda verilmiştir:

-   **RDP/SSH deneme yanılma saldırıları gelen:** dağıtımlarınız her gün ve diğer sanal birkaç veya tüm oturumları makinelerde birçok oturumları yoğun sanal makineler olabilir. Azure Güvenlik Merkezi bu sanal makineler için taban çizgisi oturum açma etkinliğini belirler ve machine learning normal oturum açma etkinlikleri tanımlamak kullanın. Taban çizgisi için tanımlanan herhangi bir farklılık olması halinde bir uyarı oluşturulabilir daha sonra oturum açma özellikleri, ilgili. Yine machine learning neyin önemli olduğunu belirler.

### <a name="continuous-threat-intelligence-monitoring"></a>Sürekli tehdit bilgileri izleme

Azure Güvenlik Merkezi tehdit kapsamındaki değişiklikleri sürekli olarak izleyen güvenlik araştırması ve veri bilimi ekipleri dünyanın her ile çalışır. Buna aşağıdaki girişimler dahildir:

-   **Tehdit bilgisi izleme:** tehdit Intelligence mekanizmaları, göstergeleri, uygulamaları ve var olan veya yeni ortaya çıkan tehditlere uygulanabilir öneriler içerir. Bu bilgileri güvenlik topluluğu içinde paylaşılır ve Microsoft, iç ve dış kaynaklardan gelen tehdit bilgisi akışlarını sürekli olarak izler.

-   **Sinyal paylaşımı:** Bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası cihazları arasında Microsoft'un geniş Portföyünde güvenlik ekiplerinden alınan bilgiler paylaşılır ve analiz edilir.

-   **Microsoft Güvenlik uzmanları:** hukuk gibi Uzman güvenlik alanlarında çalışan ve web saldırıcı algılama takımlar arasında Microsoft ile devam eden katılım.

-   **Algılama ayarı:** gerçek müşteri veri kümelerine göre algoritmalar çalıştırılır ve güvenlik Araştırmacıları sonuçları doğrulamak üzere müşterilerle. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleşik çabalar yeni ve geliştirilmiş algılamalarla sonuçlanır ve sizin hiçbir şey yapmanıza gerek kalmaz.

## <a name="advanced-threat-detection-features---other-azure-services"></a>Gelişmiş tehdit algılama özellikleri - diğer Azure Hizmetleri

### <a name="virtual-machine-microsoft-antimalware"></a>Sanal makine: Microsoft kötü amaçlı yazılımdan koruma

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure uygulamaları ve Kiracı ortamları için bir tek Aracısı çözümü için İnsan aracılığı olmadan arka planda çalışması için tasarlanmış. Her iki temel güvenli stl'deki varsayılan ile uygulama iş yüklerinin gereksinimlerini temel veya Gelişmiş kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere özel yapılandırma koruması dağıtabilirsiniz. Azure kötü amaçlı yazılımdan koruma güvenlik için Azure sanal makineleri bir seçenektir ve tüm Azure PaaS sanal makinelere otomatik olarak yüklenir.

**Dağıtma ve uygulamalarınız için Microsoft Antimalware etkinleştirmek için Azure özellikleri**

#### <a name="microsoft-antimalware-core-features"></a>Microsoft kötü amaçlı yazılımdan koruma çekirdek Özellikler

-   **Gerçek zamanlı koruma -** etkinlik bulut Hizmetleri ve Algıla ve kötü amaçlı yazılım yürütme engellemek için sanal makinelerde izler.

-   **Zamanlanmış tarama -** etkin olarak çalışan programlar dahil olmak üzere kötü amaçlı yazılım, algılamak için hedeflenen tarama düzenli olarak gerçekleştirir.

-   **Kötü amaçlı yazılım düzeltme -** otomatik olarak algılanan kötü amaçlı yazılım silme veya kötü amaçlı dosyaları karantinaya alma ve kötü amaçlı kayıt defteri girdilerini temizleme gibi eylemi alır.

-   **İmza güncelleştirmeleri -** koruma önceden belirlenen bir frekansında güncel olduğundan emin olmak için en son koruma imzaları (virüs tanımları) otomatik olarak yükler.

-   **Kötü amaçlı yazılımdan koruma Altyapısı güncelleştirmeleri -** Microsoft Antimalware altyapısı otomatik olarak güncelleştirir.

-   **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri –** Microsoft Antimalware platform otomatik olarak güncelleştirir.

-   **Etkin bir koruma -** telemetri meta verileri gelişen tehditlere ve gerçek zamanlı zaman uyumlu imza teslim üzerinden etkinleştirme hızlı yanıt emin olmak için Microsoft Azure algılanan tehditlere ve şüpheli kaynaklar hakkında raporlar Microsoft etkin koruma sistemi (MAPS).

-   **Örnekleri raporlama -** ve hizmet iyileştirmek ve sorun giderme etkinleştirmek yardımcı olmak için Microsoft Antimalware hizmetinde örnekleri raporlar sağlar.

-   **Dışlamalar –** koruma ve performans ve/veya diğer nedenlerle tarama dışında tutmak için sürücü ve uygulama ve hizmet yöneticileri işlemleri, belirli dosyaları yapılandırma sağlar.

-   **Kötü amaçlı yazılımdan koruma olay toplama -** kötü amaçlı yazılımdan koruma hizmeti durumu, kuşkulu etkinlikleri ve işletim sistemi olay günlüğünde gerçekleştirilecek düzeltme eylemleri kaydeder ve Müşteri'nin Azure Storage hesabınıza toplar.

### <a name="azure-sql-database-threat-detection"></a>Azure SQL veritabanı tehdit algılama

[Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/) Azure SQL Database hizmetinde oluşturulmuş yeni bir güvenlik Intelligence özelliğidir. Öğrenmek için profil ve anormal veritabanı etkinliklerini algılar aralıksız çalışma, Azure SQL veritabanı tehdit algılama veritabanına olası tehditler tanımlar.

Bunlar ortaya çıktığında güvenlik görevlileri veya diğer atanmış yöneticileri şüpheli veritabanı etkinlikleri hakkında anında bildirim edinebilirsiniz. Her bir bildirim şüpheli Etkinlik ayrıntıları sağlar ve daha fazla araştırmak ve tehdidi azaltmak nasıl önerir.

Şu anda, Azure SQL veritabanı tehdit algılama, olası güvenlik açıkları ve SQL ekleme saldırıları ve anormal veritabanı erişimi desenleri algılar.

Tehdit algılama e-posta bildirimi aldıktan sonra kullanıcılar gidin ve bir denetim Görüntüleyicisi'ni açar Mail'de ayrıntılı bağlantısı kullanarak ilgili denetim kayıtları görüntülemek ve/veya önceden yapılandırılmış geçici ilgili denetim kayıtlarının gösterir Excel şablonu denetleme Aşağıdaki göre kuşkulu olay süresi:
-   Anormal veritabanı etkinliklerini ile veritabanı/sunucu için Denetim depolama

-   Olay sırasındaki denetim günlüğü yazmak için kullanılan ilgili denetim depolama tablosu

-   Olay oluştuktan sonra aşağıdaki saat kayıtlarını denetim.

-   Denetim kayıtlarının (bazı algılayıcılar için isteğe bağlı) Olay sırasındaki benzer olay kimliği

SQL veritabanı tehdit algılayıcılar aşağıdaki algılama yöntemlerini birini kullanın:

-   **Belirlenimci algılama –** şüpheli desenler (dayalı kurallar) bilinen saldırıları eşleşen SQL istemci sorgularda algılar. Bu yöntemler yüksek algılama ve düşük yanlış pozitif sahiptir ancak "atomik algılama" kategoride kaldığından kapsamı sınırlı.

-   **Behavioural algılama –** son 30 gün içinde görülen olmayan veritabanı için olağan dışı davranış anormal bir etkinliğin hataları.  SQL istemci anormal etkinliği için bir örnek depo başarısız oturum açma/sorguları, ayıklanırken veri hacmi yüksek, olağan dışı kurallı sorgular ve veritabanına erişmek için kullanılan tanınmayan IP adresleri olabilir

### <a name="application-gateway-web-application-firewall"></a>Uygulama ağ geçidi Web uygulaması güvenlik duvarı

[Web uygulaması güvenlik duvarı](../app-service/environment/app-service-app-service-environment-web-application-firewall.md) özelliğidir [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-webapplicationfirewall-overview) uygulama ağ geçidi için standart kullanan web uygulamaları için koruma sağlar [uygulama teslim denetimi](https://kemptechnologies.com/in/application-delivery-controllers)işlevleri. Web uygulaması güvenlik duvarı olmadığından bu çoğunu karşı koruyarak [OWASP ilk 10 ortak web güvenlik açıkları](https://www.owasp.org/index.php/Top_10_2010-Main)

![Uygulama ağ geçidi Web uygulaması Firewally](./media/azure-threat-detection/azure-threat-detection-fig13.png)

-   SQL ekleme koruması

-   Siteler arası komut dosyası koruması

-   Komut ekleme, HTTP isteği kaçakçılığı, HTTP yanıtı bölme ve uzak dosya ekleme saldırıcı gibi Yaygın Web Saldırıları Koruması

-   HTTP protokolü ihlallerine karşı koruma

-   Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma

-   Robotlar, gezginler ve tarayıcıları önleme

-   Ortak uygulama yapılandırma hataları (diğer bir deyişle, Apache, IIS, vb.) algılanması

Uygulama ağ geçidi WAF yapılandırma aşağıdaki avantajı sağlar:

-   Arka uç kodunda herhangi bir değişiklik yapmadan web uygulamanızı web güvenlik açıklarından ve saldırılarından koruma.

-   Bir uygulama ağ geçidinin arkasında birden fazla web uygulamasını aynı anda koruma. Application Gateway, tek bir ağ geçidinin arkasında tümü web saldırılarına karşı korunabilecek 20’ye kadar web sitesini barındırmayı destekler.

-   Uygulama ağ geçidi WAF günlükleri tarafından oluşturulan gerçek zamanlı raporları kullanarak web uygulamanızı saldırılara karşı izleyin.

-   Bazı uyumluluk denetimleri, İnternet’e yönelik tüm uç noktaların bir WAF çözümü tarafından korunmasını gerektirir. WAF etkinken uygulama ağ geçidini kullanarak, bu uyumluluk gereksinimlerini karşılayabilirsiniz.

### <a name="anomaly-detection--an-api-built-with-azure-machine-learning"></a>Anomali algılama – Azure Machine Learning ile yerleşik bir API

Anomali algılama anormal desenleri farklı türde zaman serisi verilerinizi algılamak için yararlı olan Azure Machine Learning ile yerleşik bir API'dir. API her veri noktası uyarıları oluşturmak için kullanılabilir, zaman serisinde bir anomali puan atar izleme panoları veya gişe sistemleri ile bağlama.

[Anomali algılama API](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api) zaman serisi veri anormallikleri aşağıdaki türlerini saptayabilir:

-   **Ani ve Dips:** Örneğin, bir hizmet için oturum açma hatalarının sayısı veya bir e-ticaret sitesi kullanıma sayısında izlerken, olağan dışı ani dıps güvenlik saldırılarına belirtmek veya hizmet kesintilerini.

-   **Pozitif ve negatif eğilimleri:** bilgi işlem bellek kullanımı izlerken, örneğin, boş bellek boyutu küçültme olası bir bellek sızıntısı hatırlanması; hizmet sırası uzunluğu izlerken, kalıcı bir yukarı eğilim bir arka plandaki gösterebilir yazılım sorunu.

-   **Düzey değişiklikleri ve dinamik aralık değerleri değişiklikleri:** hizmet yükseltin ya da yükseltme izlemek ilginç sonra özel durumlar düzeylerde alt sonra düzeyindeki bir hizmet gecikmeleri Örneğin, değişiklikleri.

Machine learning göre API sağlar:

-   **Esnek ve sağlam algılama:** kullanıcıların duyarlılık ayarlarını yapılandırmak ve anormallikleri Mevsimlik ve Mevsimlik olmayan veri kümesi arasında algılamak anomali algılama modelleri sağlar. Kullanıcılar, API algılama ihtiyaçlarına göre daha az veya daha fazla duyarlı yapmak için anomali algılama modelini ayarlayabilir. Bu, veri ile ve Mevsimlik desenleri olmadan daha az veya daha fazla görünür anormallikleri algılama anlamına gelir.

-   **Ölçeklenebilir ve zamanında algılama:** pahalı ve veri kümelerini dinamik olarak değiştirme milyonlarca için ölçeklenebilir uzmanlar etki alanı bilgi sahibi ayarlamak mevcut eşiklerle izleme geleneksel bir yoludur. Bu API anomali algılama modellerinde öğrenilen ve modelleri geçmiş ve gerçek zamanlı verileri otomatik olarak ayarlanmıştır.

-   **Öngörülü ve tıklatılabilir algılama:** yavaş eğilim ve düzey değişikliği algılama erken anomali algılama için uygulanır. Algılanan erken anormal sinyalleri araştırmanıza ve sorun alanlarına hareket insanlar yönlendirmek için kullanılabilir.  Ayrıca, kök neden analizi modelleri ve uyarı araçları bu anomali algılama API hizmeti üstünde geliştirilebilir.

Anomali algılama API kadar çeşitli senaryoları hizmet durumu ve izleme, IOT, performans izlemeyi ve ağ trafiğini izleme KPI gibi için etkili ve verimli bir çözümdür. Burada, bu API yararlı olabilir bazı yaygın senaryolar şunlardır:
- BT departmanları zamanında izleme olayları, hata kodu, kullanım günlük ve performans (CPU, bellek ve benzeri) araçları gerekir.

-   Çevrimiçi Ticaret siteleri müşteri etkinlikleri, sayfa görünümleri, tıklama ve benzeri izlemek istiyorsunuz.

-   Su, gaz, elektrik ve diğer kaynakların tüketimini izlemek yardımcı programı şirketler istiyor.

-   Yönetim Hizmetleri tesis/oluşturma sıcaklık, nemi, trafiği ve benzeri izlemek istersiniz.

-   IOT/üreticileri algılayıcı verilerini zaman serisi izleme iş akışı, kalite ve benzeri için kullanmak istediğiniz.

-   Çağrı merkezleri gibi hizmet sağlayıcıları gereken hizmet isteğe bağlı eğilimi, olay birim izleme, kuyruk uzunluğu vb. bekleyin.

-   İş analizi grupları istediğiniz iş KPI'leri (örneğin, Satış Birim, fiyatlandırma müşteri düşüncelerin) izlemek gerçek zamanlı anormal taşıma.

### <a name="cloud-app-security"></a>Cloud App Security

[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) Microsoft Cloud Security yığınının kritik bir bileşendir. Bulut uygulamalarını promise tam olarak yararlanmasını ancak etkinlik geliştirilmiş görünürlük aracılığıyla denetiminde tutmak hareket ederken, kuruluşunuzun yardımcı olabilecek kapsamlı bir çözümdür. Ayrıca, bulut uygulamaları genelinde kritik veri korumasını artırmaya yardımcı olur.

Gölge BT ortaya çıkarmaya, riskleri değerlendirmeye, ilkeleri zorunlu tutmanıza, etkinlikleri araştırmaya ve tehditleri durdurmaya yardımcı olan araçlar ile kuruluşunuz daha güvenli bir şekilde buluta kritik verilerin denetimini korurken taşıyabilirsiniz.

<table style="width:100%">
 <tr>
   <td>Keşif</td>
   <td>BT Cloud App Security ile gölge BT'yi ortaya çıkarın. Uygulamaları, etkinlikleri, kullanıcılar, veri ve bulut ortamınızdaki dosyaları bularak görünürlük elde edilir. Bulutunuzla bağlantılı üçüncü taraf uygulamalar bulur.</td>
 </tr>
 <tr>
   <td>Araştır</td>
   <td>Riskli uygulamalar, belirli kullanıcılar ve ağınızdaki dosya derin Dalış için bulut adli araçları kullanarak, bulut uygulamalarınızı araştırın. Toplanan verilerdeki düzenleri bulun. Bulutunuzu izlemek için raporlar oluşturur.</td>

 </tr>
 <tr>
   <td>Denetim</td>
   <td>İlkeleri ve ağ bulut trafiği üzerinde en yüksek düzeyde denetim elde etmek için uyarılar ayarlayarak riski azaltın. Cloud App Security kullanıcılarınız için güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için kullanın.</td>

 </tr>
 <tr>
   <td>Koruma</td>
   <td>Cloud App Security tasdik veya uygulamaları engelle, veri kaybını önleme, Denetim izinleri ve paylaşımı zorunlu kılmak için kullanın ve özel raporlar ve uyarılar oluşturur.</td>

 </tr>
 <tr>
   <td>Denetim</td>
   <td>İlkeleri ve ağ bulut trafiği üzerinde en yüksek düzeyde denetim elde etmek için uyarılar ayarlayarak riski azaltın. Cloud App Security kullanıcılarınız için güvenli, tasdikli bulut uygulaması alternatiflerine geçirmek için kullanın.</td>

 </tr>
</table>


![Cloud App Security](./media/azure-threat-detection/azure-threat-detection-fig14.png)

Cloud App Security görünürlük tarafından bulut ile tümleşir.

-   Harita ve bulut ortamınız ve bulut uygulamalarını tanımlamak için cloud Discovery kullanılarak kuruluşunuz kullanıyor.


-   Tasdit etme ve bulut uygulamalarında yasaklamaktadır.



-   Görünürlüğü ve İdaresi, bağlandığınız uygulamalar için API ' larını sağlayıcısı yararlanmak dağıtımı kolay uygulama bağlayıcıları kullanma.

-   Sürekli denetimi ayarı ve sürekli olarak ince ayar, ilkeleri tarafından size yardımcı olur.

Bu kaynaklardan veri toplama üzerinde Cloud App Security Gelişmiş bir analiz verileri üzerinde çalışır. Hemen anormal etkinlikler için uyarı ve bulut ortamınıza derin bir görünürlük sağlar. Cloud App Security bir ilke yapılandırın ve bulut ortamınızdaki her şeyi korumak için kullanın.

## <a name="third-party-atd-capabilities-through-azure-marketplace"></a>Azure Market üzerinden üçüncü taraf ATD özellikleri

### <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web uygulaması güvenlik duvarı gelen web trafiği ve blokları SQL eklemelerini, siteler arası komut dosyası, kötü amaçlı yazılım yüklemeleri ve uygulama DDoS ve web uygulamalarınızı hedefleyen diğer saldırılara olup olmadığını denetler. Ayrıca arka uç web sunucularından yanıtları veri kaybını önleme (DLP) inceler. Tümleşik erişim denetimi altyapısı yöneticilerinin kimlik doğrulama, yetkilendirme ve hesap oluşturma (kuruluşlar güçlü kimlik doğrulama ve kullanıcı denetimi sağlayan AAA), için ayrıntılı erişim denetimi ilkeleri oluşturmasını sağlar.

**Vurgular:**
-   Algılar ve SQL eklemelerini, siteler arası komut dosyası, kötü amaçlı yazılım yüklemeleri, uygulama DDoS veya herhangi bir uygulamanız saldırıları engeller.

-   Kimlik doğrulaması ve erişim denetimi.

-   Hassas verileri algılamak ve maskeleyebilir ya da sızmasını bilgilerinin engellemek için giden trafiği tarar.

-   Web uygulama içeriğini önbelleğe alma, sıkıştırma ve diğer trafik iyileştirmeleri gibi özelliklerini kullanarak, teslimini hızlandırır.

Web uygulaması güvenlik duvarı Azure Market yerinde kullanılabilir örneği aşağıda verilmiştir:

[Barracuda Web uygulaması güvenlik duvarı, Brocade sanal Web uygulaması Güvenlik Duvarı (Brocade vWAF), Imperva SecureSphere ve ThreatSTOP IP Güvenlik Duvarı.](https://azure.microsoft.com/marketplace/partners/brocade_communications/brocade-virtual-web-application-firewall-templatevtmcluster/)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Güvenlik Merkezi algılama özellikleri](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities)

Azure Güvenlik Merkezi'nin Gelişmiş algılama özelliklerini Microsoft Azure kaynaklarınızı hedefleyen etkin tehditleri belirlemenize yardımcı olur ve hızlı yanıt vermek için gereken bilgileri sağlar.

- [Azure SQL veritabanı tehdit algılama](https://azure.microsoft.com/blog/azure-sql-database-threat-detection-your-built-in-security-expert/)

Azure SQL veritabanı tehdit algılama, kendi endişelere kendi veritabanı için olası tehditler hakkında Yardım.
