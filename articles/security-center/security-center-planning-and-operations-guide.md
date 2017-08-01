---
title: "Güvenlik Merkezi Planlama ve İşlemler Kılavuzu | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi&quot;ni benimsemeden önce planlama ve günlük işlemlere ilişkin dikkat edilmesi gerekenler konusunda yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: c502e4363dbaa37455d1aad90d1e9fa855fd09b0
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-planning-and-operations-guide"></a>Azure Güvenlik Merkezi planlama ve işlemler kılavuzu
Bu kılavuz, kurumları Azure Güvenlik Merkezi'ni kullanmayı planlayan bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenlik çözümleyicileri ve bulut yöneticilerine yöneliktir.

>[!NOTE] 
>Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanacak. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Platform Geçişi](security-center-platform-migration.md) makalesine bakın. Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>

## <a name="planning-guide"></a>Planlama Kılavuzu
Bu kılavuzda, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline dayanarak Güvenlik Merkezi kullanımınızı iyileştirmek için izleyeceğiniz adımlar ve görevlerin kümesi sağlanmıştır. Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir. Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

* Güvenlik Rolleri ve Erişim Denetimleri
* Güvenlik İlkeleri ve Öneriler
* Veri Koleksiyonu ve Storage
* Devam Eden Güvenlik İzleme
* Olay Yanıtı

Sonraki bölümde, gereksinimlerinize bağlı olarak bu alanların her birini nasıl planlayacağınızı ve bu önerileri nasıl uygulayacağınızı öğreneceksiniz.

> [!NOTE]
> Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).
> 

## <a name="security-roles-and-access-controls"></a>Güvenlik rolleri ve erişim denetimleri
Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıdaki diyagramda, hayali kişiler ile bunların rollerinin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örneğin:

**Ömer (Bulut İş Yükü Sahibi)**

* Bir bulut iş yükünü ve onunla ilgili kaynakları yönetme
* Korumaları şirket güvenlik ilkesiyle uyumlu bir şekilde uygulamak ve sürdürmekle sorumlu

**Emel (CISO/CIO)**

* Şirket güvenliğinin tüm boyutlarından sorumlu
* Bulut iş yüklerinin tamamında şirketin güvenlik duruşunu anlamak istiyor
* Önemli saldırı ve risklerden haberdar olması gerekiyor

**Ali (BT Güvenliği)**

* Uygun korumaların uygulanmakta olduğundan emin olmak için şirketin güvenlik ilkelerini ayarlar
* İlkelerle uyumluluğu izler
* Yöneticiler veya denetçiler için raporlar oluşturur

**Zehra (Güvenlik İşlemleri)**

* Güvenlik uyarılarını 7/24 izler ve bunlara yanıt verir
* Bulut İş Yükü Sahibi veya BT Güvenlik Analistine İletir

**Salih (Güvenlik Analisti)**

* Atakları araştırır
* Bulut İş Yükü Sahibi ile birlikte çalışarak düzeltme uygulama 

Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../active-directory/role-based-access-built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../active-directory/role-based-access-control-configure.md) kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişimi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Bu role ait kullanıcı, Güvenlik Merkezi’nde öneriler, uyarılar, ilke ve sistem durumunu içeren haklarını görüntüleyebilir, ancak değişiklik yapamaz.
- **Güvenlik yöneticisi**: Güvenlik okuyucusu ile aynıdır, ancak aynı zamanda güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.

Yukarıda açıklanan Güvenlik Merkezi rolleri, Azure’un Depolama, Web ve Mobil veya Nesnelerin İnterneti gibi diğer hizmet alanlarına erişemez.  

> [!NOTE]
> Bir kullanıcının Azure’da Güvenlik Merkezi’ni görebilmesi için en azından bir abonelik, kaynak grubu sahibi veya katkıda bulunan olması gerekir. 
> 
> 

Önceki diyagramda açıklanan kişiler kullanıldığında aşağıdaki RBAC gerekli olur:

**Ömer (Bulut İş Yükü Sahibi)**

* Kaynak Grubu Sahibi/Ortak Çalışanı

**Ali (BT Güvenliği)**

* Abonelik Sahibi/Ortak Çalışan veya Güvenlik Yöneticisi

**Zehra (Güvenlik İşlemleri)**

* Uyarıları görüntülemek için Abonelik Okuyucusu veya Güvenlik Okuyucusu
* Uyarıları kapatmak için Abonelik Sahibi/Ortak Çalışanı veya Güvenlik Okuyucusu gerekir

**Salih (Güvenlik Analisti)**

* Uyarıları görüntülemek için Abonelik Okuyucusu
* Uyarıları kapatmak Abonelik Sahibi/Ortak Çalışanı gerekir
* Çalışma alanına erişim gerekli olabilir

Dikkate alınması gereken bazı diğer önemli bilgiler:

* Güvenlik ilkesini yalnızca abonelik Sahipleri/Katkıda Bulunanları ve Güvenlik Yöneticileri düzenleyebilir
* Yalnızca abonelik ve kaynak grubu Sahipleri ve Katkıda bulunanları bir kaynak için güvenlik önerilerini uygulayabilir.

Güvenlik Merkezi için RBAC kullanarak erişim denetimini planlarken Güvenlik Merkezi'ni kuruluşunuzdaki hangi kişilerin kullanacağını anladığınızdan emin olun. Ayrıca, bunların hangi türde görevler gerçekleştireceğini anlayın ve daha sonra RBAC’yi buna göre yapılandırın.

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca kaynakların güvenlik durumu hakkındaki bilgileri görüntülemesi gereken ancak eyleme geçmeyecek olan kullanıcıların (örneğin, önerileri uygulamak veya ilkeleri düzeltmek), Okuyucu rolüne atanmaları gerekir.
> 
> 

## <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler
Güvenlik ilkesi, belirtilen abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız.

Abonelik düzeyinde etkinleştirilen ilkeler, aşağıdaki diyagramda gösterildiği gibi abonelikteki tüm kaynak gruplarına otomatik olarak yayılır:

![Güvenlik İlkeleri](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Hangi ilkelerin değiştiğini gözden geçirmeniz gerekiyorsa [Azure Denetim Günlükleri](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/)'ni kullanabilirsiniz. İlke değişiklikleri her zaman Azure Denetim Günlükleri'ne kaydedilir.
> 
> 

### <a name="security-recommendations"></a>Güvenlik önerileri
Güvenlik ilkelerini yapılandırmadan önce her bir [güvenlik önerisini](security-center-recommendations.md) gözden geçirip bu ilkelerin sahip olduğunuz çeşitli abonelikler ve kaynak grupları için uygun olup olmadığını belirleyin. [Güvenlik Önerilerini](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) ele almak için hangi eylemlerde bulunulacağını ve kuruluşunuzda yeni önerileri izlemekten ve gerekli adımların atılmasından kimin sorumlu olacağını anlamak da önemlidir.

Güvenlik Merkezi, Azure aboneliğiniz için güvenlik kişi ayrıntılarını sağlamanızı önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Bu öneriyi etkinleştirme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](security-center-provide-security-contact-details.md) konusunu okuyun.

## <a name="data-collection-and-storage"></a>Veri koleksiyonu ve depolama
Azure Güvenlik Merkezi, sanal makinelerinizden güvenlik verilerini toplamak için Microsoft Monitoring Agent’ı (Operations Management Suite ve Log Analytics hizmeti tarafından kullanılan aracının aynısı) kullanır. Bu aracıdan toplanan veriler, Log Analytics çalışma alanlarınızda depolanır.

### <a name="agent"></a>Aracı

Güvenlik ilkenizde veri toplama etkinleştirildikten sonra, desteklenen tüm Azure VM’lere ve oluşturulan tüm yeni VM’lere Microsoft Monitoring Agent ([Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) veya [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) için) yüklenir.  VM’de zaten Microsoft Monitoring Agent yüklüyse, Azure Güvenlik Merkezi yüklü olan aracıyı kullanır. Aracının işlemi müdahaleci olmayacak şekilde tasarlanmıştır ve VM performansı üzerinde çok az etki yapar.

Windows için Microsoft Monitoring Agent, TCP bağlantı noktası 443’ün kullanılmasını gerektirir. Daha fazla bilgi için [Sorun giderme makalesine](security-center-troubleshooting-guide.md) bakın.

Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Ancak, Microsoft Monitoring Agent diğer Azure yönetim ve izleme hizmetleri tarafından kullanılıyor olabileceğinden, Güvenlik Merkezi’nde veri toplamayı kapattığınızda aracı otomatik olarak kaldırılmayabilir. Gerekirse aracıyı el ile kaldırabilirsiniz.

> [!NOTE]
> Desteklenen VM'lerin listesini bulmak için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).
> 

### <a name="workspace"></a>Çalışma alanı

Microsoft Monitoring Agent’tan Azure Güvenlik Merkezi adına toplanan veriler, sanal makinenin Coğrafi konumu göz önünde bulundurularak Azure aboneliğinizle ilişkili Log Analytics çalışma alanlarında veya yeni çalışma alanlarında depolanır. 

Azure portalında, Azure Güvenlik Merkezi tarafından oluşturulanlar dahil olmak üzere Log Analytics çalışma alanlarınızın listesine göz atabilirsiniz. Yeni çalışma alanları için bir ilgili kaynak grubu oluşturulur. İkisi de şu adlandırma kuralını izler: 

* Çalışma Alanı: *DefaultWorkspace-[abonelik-kimliği]-[bölge]*
* Kaynak Grubu: *DefaultResouceGroup-[bölge]*

Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanları için veriler 30 gün boyunca tutulur. Mevcut çalışma alanları için elde tutma süresi, çalışma alanının fiyatlandırma katmanını temel alır.

> [!NOTE]
> Microsoft, bu verilerin gizlilik ve güvenliğini korumak için önemli taahhütlerde bulunur. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Veri işleme ve gizlilik hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Veri Güvenliği](security-center-data-security.md) makalesini okuyun.
> 

## <a name="ongoing-security-monitoring"></a>Devam eden güvenlik izleme
Güvenlik Merkezi önerilerinin ilk yapılandırması ve uygulamasından sonraki adım, Güvenlik Merkezi işlem süreçlerini dikkate almaktır.

Azure portaldan Güvenlik Merkezi'ne erişmek için **Gözat**'a tıklayıp **Filtre** alanına **Güvenlik Merkezi** yazabilirsiniz. Kullanıcının aldığı görünümler, uygulanan bu filtrelere göre belirlenir; aşağıdaki örnekte, ele alınması gereken çok sayıda soruna sahip bir ortam gösterilmektedir:

![pano](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Güvenlik Merkezi normal işlem yordamlarınıza müdahale etmez, dağıtımlarınızı pasif olarak izler ve etkinleştirdiğiniz güvenlik ilkelerine bağlı olarak öneriler sağlar.

Geçerli Azure ortamınız için Güvenlik Merkezi’ni kullanmayı ilk kez seçtiğinizde, **Öneriler** kutucuğunda yer alan veya kaynak başına (**İşlem**, **Ağ**, **Depolama ve Veriler**, **Uygulama**) sunulan tüm önerileri gözden geçirdiğinizden emin olun.

Tüm önerilere değindikten sonra değinilen tüm kaynaklar için **Önleme** bölümünün yeşil olması gerekir. Yalnızca kaynak güvenlik durumu ve öneriler kutucuklarındaki değişikliklere göre eyleme geçeceğiniz için devam eden izleme bu noktada daha kolay olur.

**Algılama** bölümü daha reaktiftir; bunlar, ya şu anda var olan veya geçmişte oluşan ya da Güvenlik Merkezi denetimleri ile 3. taraf sistemleri tarafından algılanan sorunlarla ilgili uyarılardır. Güvenlik Uyarıları kutucuğu, her gün bulunan tehlike algılama uyarıları sayısını ve bunların farklı önem derecesi kategorilerine (düşük, orta, yüksek) dağılımlarını temsil eden çubuk grafiklerini gösterir. Güvenlik Uyarıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve uyarılara yanıt verme](security-center-managing-and-responding-alerts.md)

> [!NOTE]
> Güvenlik Merkezi verilerinizi görselleştirmek için Microsoft Power BI'dan da yararlanabilirsiniz. Bkz. [Power BI ile Azure Güvenlik Merkezi verilerinden öngörü alma](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Yeni veya değiştirilmiş kaynakları izleme
Çoğu Azure ortamı, düzenli olarak çalışmaya başlatılan ve yavaşlatılan yeni kaynaklarla dinamiktir (örneğin, yapılandırmalar veya değişiklikler vb.) Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Buna PaaS web rolleri ve çalışan rolleri de dahildir. Veri Koleksiyonu [Güvenlik İlkesi](security-center-policies.md)'nde etkinleştirilirse sanal makineleriniz için ek izleme işlevleri otomatik olarak etkinleştirilir.

![Temel alanlar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Sanal makineler için **Önleme** bölümü altındaki **İşlem**’e tıklayın. Verileri etkinleştirme veya ilgili öneriler hakkında tüm sorunlar **Genel Bakış** sekmesinde ve **İzleme Önerileri** bölümünde gösterilir.
2. Yeni kaynak için (eğer varsa) hangi güvenlik risklerinin tanımlandığını görmek için **Öneriler**'i görüntüleyin.
3. Ortamınıza yeni VM'ler eklendiğinde, başlangıçta yalnızca işletim sisteminin yüklenmesi durumu çok yaygındır. Bu VM'ler tarafından kullanılacak diğer uygulamaları dağıtmak için kaynak sahibine bir miktar süre gerekebilir.  İdeal olarak, bu iş yükünün son amacını bilmeniz gerekir. Bu iş yükü bir Uygulama Sunucusu mu olacak? Bu yeni iş yükünün ne olacağına bağlı olarak, bu iş akışındaki üçüncü adım olan uygun **Güvenlik İlkesi**'ni etkinleştirebilirsiniz.
4. Yeni kaynaklar Azure ortamınıza eklendikçe **Güvenlik Uyarıları** kutucuğunda yeni uyarıların görünmesi mümkündür. Bu kutucukta yeni uyarılar olup olmadığını her zaman doğrulayın ve Güvenlik Merkezi önerilerine göre eyleme geçin.

Ayrıca güvenlik riskleri, önerilen temel yapılandırmalardan sapma durumları ve güvenlik uyarıları oluşturan yapılandırma değişikliklerini tanımlamak için var olan kaynakların durumunu düzenli aralıklarla izlemek isteyeceksiniz. Güvenlik Merkezi Panosu'nda başlayın. Tutarlı bir şekilde gözden geçirmek için burada üç temel alanınız vardır.

![İşlemler](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. **Önleme** bölüm paneli, temel kaynaklarınıza hızlı erişim sağlar. İşlem, Ağ, Depolama ve veriler ile Uygulamalar’ı izlemek için bu seçeneği kullanın.
2. **Öneriler** paneli Güvenlik Merkezi önerilerini gözden geçirmenizi sağlar. İzleme devam ederken, günlük olarak önerileri almadığınızı fark edebilirsiniz; ilk Güvenlik Merkezi kurulumunda tüm önerileri ele aldığınız için bu durum normaldir. Bundan dolayı bu bölümde her gün yeni bilgilere sahip olmayabilirsiniz ve bilgilere sadece gerekli olduğunda erişmeniz gerekebilir.
3. **Algılama** bölümü, çok sık veya çok nadir değişebilir. Güvenlik uyarılarınızı her zaman gözden geçirin ve Güvenlik Merkezi önerilerine göre eyleme geçin.

## <a name="incident-response"></a>Olay yanıtı
Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir. Güvenlik Merkezi tehdit algılama özelliğinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md) konusunu okuyun.

Bu makale kendi Olay Yanıtı planınızı oluşturmanıza yardımcı olmaya yönelik değildir, ancak olay yanıtı aşamalarının temeli olarak Bulut yaşam döngüsünde Microsoft Azure Güvenlik Yanıtı kullanılacaktır. Aşamalar aşağıdaki diyagramda gösterilmiştir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Kendi planınızı oluşturmanıza yardımcı olması için National Institute of Standards and Technology'nin (NIST) [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) belgesini kullanabilirsiniz.
> 

Güvenlik Merkezi Uyarılarını aşağıdaki aşamalar sırasında kullanabilirsiniz:

* **Algılama**: bir veya daha fazla kaynakta şüpheli bir etkinliği tanımlayın. 
* **Değerlendirme**: şüpheli etkinlik hakkında daha fazla bilgi edinmek için ilk değerlendirmeyi gerçekleştirin.
* **Tanılama**: sorunu çözmeye yönelik teknik yordamı yürütmek için düzeltme adımlarını kullanın.

Her Güvenlik Uyarısı, saldırının yapısını daha iyi anlamanız ve olası risk azaltmalarını önermek için kullanılabilecek bilgiler sağlar. Ayrıca bazı uyarılar, daha fazla bilgi veya Azure'daki diğer bilgi kaynakları için bağlantılar sağlar. Verilen bilgileri daha fazla araştırma ya da risk azaltma için kullanabilir ve ayrıca çalışma alanınızda depolanmış güvenlikle ilgili verileri arayabilirsiniz.

Aşağıdaki örnek, gerçekleşmekte olan şüpheli bir RDP etkinliğini gösterir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Gördüğünüz gibi bu dikey pencere, saldırının gerçekleştiği zaman, kaynak ana bilgisayar adı, hedef VM ile ilgili ayrıntıları gösterir ve ayrıca öneri adımları sunar. Bazı durumlarda, saldırının kaynak bilgileri boş olabilir. Bu türden bir davranış hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Uyarıları'nda Eksik Kaynak Bilgileri](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

[Bir Olay Yanıtı için Azure Güvenlik Merkezi ve Microsoft Operations Management Suite’ten Yararlanma](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) videosunda, bu aşamaların her birinde Güvenlik Merkezi’nin nasıl kullanılabileceğini anlamanıza yardımcı olabilecek bazı tanıtımlar görebilirsiniz.

> [!NOTE]
> Olay Yanıtı işleminiz sırasında size yardımcı olması için Güvenlik Merkezi özelliklerini kullanma hakkında daha fazla bilgi için [Olay Yanıtı için Azure Güvenlik Merkezi’nden Yararlanma](security-center-incident-response.md) makalesini okuyun. 
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi benimsemeyi nasıl planlayacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.


