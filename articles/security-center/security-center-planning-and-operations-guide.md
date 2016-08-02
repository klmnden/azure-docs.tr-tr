<properties
   pageTitle="Güvenlik Merkezi Planlama ve İşlemler Kılavuzu | Microsoft Azure"
   description="Bu belge, Azure Güvenlik Merkezi'ni benimsemeden önce planlama ve günlük işlemlere ilişkin dikkat edilmesi gerekenler konusunda yardımcı olur."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/25/2016"
   ms.author="yurid"/>
 
# Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu
Bu kılavuz, kuruluşlarının Azure Güvenlik Merkezi'ni kullanmayı planladığı bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenlik çözümleyicileri ve bulut yöneticileri içindir.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir.

## Azure Güvenlik Merkezi'ne genel bakış
Azure Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır. 

Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).

## Planlama Kılavuzu
Bu kılavuzda, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline dayanarak Azure Güvenlik Merkezi kullanımınızı iyileştirmek için izleyeceğiniz adımlar ve görevlerin kümesi sağlanmıştır. Azure Güvenlik Merkezi'nden tam olarak yararlanabilmek için kuruluşunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullanacaklarını anlamak oldukça önemlidir. Azure Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

- Güvenlik Rolleri ve Erişim Denetimleri
- Güvenlik İlkeleri ve Öneriler
- Veri Koleksiyonu ve Storage
- Devam Eden Güvenlik İzleme 
- Olay Yanıtı

Sonraki bölümde, bu alanların her birini planlamayı ve bu önerileri gereksinimlerinize göre uygulamayı öğreneceksiniz. 

## Güvenlik rolleri ve erişim denetimleri 

Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıda, hayali kişilerin, bu kişilerle ilgili rollerin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01.png)

Azure Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örnek:

**Jeff (Bulut İş Yükü Sahibi)**

- Azure Portal'da Güvenlik Merkezi Önerilerini Görüntüler ve Tamamlar 
- Değişiklikleri İzlemek için Çağrı Oluşturma Sistemini de Kullanabilir (API'yi kullanarak önerileri doldurur)

**REX (CISO/CIO)**

- Power BI veya Excel'den Güvenlik Merkezi Raporlarını Görüntüler

**David (BT Güvenliği)**

- Güvenlik İlkesini Ayarlar ve Azure Portal'da Güvenlik Durumunu Görüntüler
- Power BI'da Verileri Çözümler ve Raporlar Oluşturur 

**Sam (Güvenlik İşlemleri)**

- Azure Portal'da Güvenlik Merkezi Uyarılarını Görüntüler ve Önceliklendirir 
- Var Olan Bir Panoyu Kullanabilir (API'yi kullanarak uyarıları doldurur)

**Sherlock (Güvenlik Çözümleyicisi)**

- Azure Portal'da Güvenlik Merkezi Uyarılarını Görüntüler 
- Var Olan Bir Panoyu Kullanabilir (API'yi kullanarak uyarıları doldurur)
- Power BI'da Uyarı Eğilimlerini Çözümler 
- Storage İçindeki Olay Günlükleri'ni Gözden Geçirir

Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../active-directory/role-based-access-built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../active-directory/role-based-access-control-configure.md) kullanır. Bir kullanıcı Azure Güvenlik Merkezi'ni açtığında yalnızca erişiminin olduğu kaynaklarla ilgili bilgileri görür. Bu da bir kaynak barındıran abonelik veya kaynak grubu için kullanıcının Sahip, Katkı Sağlayan veya Okuyucu rollerine atanması anlamına gelir. Yukarıdaki kişiler kullanıldığında şu RBAC gerekir:

**Jeff (Bulut İş Yükü Sahibi)**

- Kaynak Grubu Sahibi/Ortak Çalışanı

**David (BT Güvenliği)**

- Abonelik Sahibi/Ortak Çalışanı

**Sam (Güvenlik İşlemleri)**

- Uyarıları Görüntülemek için Abonelik Okuyucusu
- Uyarıları Kapatmak İçin Gerekli Olan Abonelik Sahibi/Ortak Çalışanı

**Sherlock (Güvenlik Çözümleyicisi)**

- Uyarıları Görüntülemek için Abonelik Okuyucusu
- Uyarıları Kapatmak veya Düzeltmek İçin Gerekli Olan Abonelik Sahibi/Ortak Çalışanı
- Storage Hizmetine Erişim Gerekli Olabilir

Dikkate alınması gereken bazı diğer önemli bilgiler:

- Yalnızca abonelik Sahipleri ve Katkıda Bulunanları güvenlik ilkesini düzenleyebilir
- Yalnızca abonelik ve kaynak grubu Sahipleri ve Katkıda bulunanları bir kaynak için güvenlik önerilerini uygulayabilir.

Azure Güvenlik Merkezi için RBAC kullanarak erişim denetimini planlarken kuruluşunuzda kimlerin Azure Güvenlik Merkezi'ni kullanacağını ve hangi tür görevleri gerçekleştireceklerini anladığınızdan emin olun. Ardından RBAC'yi uygun şekilde yapılandırın. 

> [AZURE.NOTE] Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca kaynakların güvenlik durumu hakkındaki bilgileri görüntülemesi gereken ancak eyleme geçmeyecek olan kullanıcıların (örneğin, önerileri uygulamak veya ilkeleri düzeltmek), Okuyucu rolüne atanmaları gerekir.

## Güvenlik ilkeleri ve öneriler
Güvenlik ilkesi, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetimler kümesini tanımlar. Azure Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız. 

Abonelik düzeyinde etkinleştirilen ilkeler, aşağıdaki diyagramda gösterildiği gibi abonelik içindeki tüm kaynak gruplarına otomatik olarak yayılır: 

![Güvenlik İlkeleri](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2.png) 

Yukarıdaki şekilde gösterildiği gibi kaynak grupları için güvenlik ilkeleri abonelik düzeyinden devralınabilir. 

Farklı ilke kümesi gerektiren kaynak grubunda kaynaklarınızın olduğu bazı senaryolarda, devralmayı devre dışı bırakabilir ve belirli bir Kaynak Grubuna özel ilkeler uygulayabilirsiniz. 

Belirli kaynak gruplarında özel ilkelere gereksinim duyuyorsanız kaynak grubundaki devralmayı devre dışı bırakmanız ve güvenlik ilkelerini değiştirmeniz gerekir. Örneğin, SQL Saydam Veri Şifrelemesi ilkesini gerektirmeyen bazı iş yükleriniz varsa ilkeyi abonelik düzeyinde devre dışı bırakın ve yalnızca SQL TDE'nin gerekli olduğu kaynak gruplarında etkinleştirin.
 
Farklı kaynak grupları için özel ilkeler oluşturmaya başladığınızda ilke çakışması olması durumunda (aboneliğe karşı kaynak grup) kaynak grup ilkesinin korunacağını bilerek ilke dağıtımınızı planlamanız gerekir. 

> [AZURE.NOTE] Hangi ilkelerin değiştiğini gözden geçirmeniz gerekiyorsa [Azure Denetim Günlükleri](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/)'ni kullanabilirsiniz. İlke değişiklikleri her zaman Azure Denetim Günlükleri'ne kaydedilir.

### Güvenlik önerileri

Güvenlik İlkeleri'ni yapılandırmadan önce her bir [güvenlik önerisini](security-center-recommendations.md) gözden geçirip bunların çeşitli abonelikleriniz ve kaynak gruplarınız için uygun olup olmadığını belirlemeniz gerekir. Ayrıca, Güvenlik Önerilerine değinmek için hangi eylemin gerçekleştirileceğini anlamak önemlidir.

**Uç Nokta Koruması**: Sanal makine, etkin bir uç nokta koruma çözümüne sahip değilse Azure Güvenlik Merkezi bir tane yüklemenizi önerir. Şirket içinde zaten benimsediğiniz bir uç nokta koruma çözümünüz varsa aynı kötü amaçlı yazılımdan koruma yazılımını Azure VM'leriniz için kullanıp kullanmayacağınıza karar vermeniz gerekir. Azure Güvenlik Merkezi, çeşitli uç nokta koruma seçeneklerini size sunar.  Ücretsiz Microsoft Kötü Amaçlı Yazılımdan Koruma Yazılımı'nı kullanabilir veya tümleşik iş ortaklarının uç nokta koruma çözümleri listesinden seçebilirsiniz. Azure Güvenlik Merkezi'ni kullanarak kötü amaçlı yazılımdan koruma yazılımını dağıtma hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde kötü amaçlı yazılımdan koruma yazılımını etkinleştirme](security-center-enable-antimalware.md).

**Sistem Güncelleştirmeleri**: Azure Güvenlik Merkezi, güvenliği veya kritik işletim sistemi güncelleştirmeleri eksik olan sanal makineleri tanımlar ve bulur. Kimin güncelleştirmeleri uygulamak için sorumlu olacağını, ne zaman gereksinim duyacağınızı ve bunların nasıl uygulanacağını dikkate alın. Birçok kuruluş WSUS, Windows Update veya başka bir araç kullanır.

**Temel Yapılandırmalar**: Sanal makine işletim sistemi yapılandırmaları, önerilen temel yapılandırmalarla eşleşmiyorsa bir öneri kullanıma sunulur. Temel yapılandırmalar kümesini [buradan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) gözden geçirmeniz ve işletim sistemi yapılandırmalarının nasıl uygulanacağını göz önünde bulundurmanız gerekir. 

**Disk Şifrelemesi**: Şifrelenmeyen sanal makine diskiniz varsa Azure Güvenlik Merkezi, işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak amacıyla Windows için BitLocker ve Linux için DM-Crypt kullanan Azure Disk Şifrelemesi'ni uygulamanızı önerir. Bu öneri sizi, bu şifrelemeyi nasıl gerçekleştireceğiniz hakkında talimatlar içeren [adım adım kılavuzu](security-center-disk-encryption.md) bölümüne yönlendirir. 

Değinmeniz gereken birçok şifreleme senaryosu olduğunu unutmayın. Bu senaryoların her biri için benzersiz gereksinimleri planlamanız gerekir:

- Kendi şifreleme anahtarlarınızı kullanarak şifrelediğiniz VHD'lerden yeni Azure Virtual Machines'i şifreleme
- Azure Galerisi'nden oluşturulan yeni Azure Virtual Machines'i şifreleme
- Zaten Azure'da çalışan Azure Virtual Machines'i şifreleme

Gereksinimleri planlama, bu senaryoların her biri için farklı olacaktır. Bu senaryoların her birine ilişkin ayrıntılar için bkz. [Azure Disk Şifrelemesi teknik incelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Web Uygulaması Güvenlik Duvarı**: Azure Güvenlik Merkezi, web uygulamaları çalıştıran sanal makineleri tanımlar ve bir Web Uygulaması Güvenlik Duvarı (WAF) yüklemenizi önerir. Kuruluşunuz için en uygun olan kullanılabilir iş ortağı çözümlerini değerlendirin ve çözümün nasıl lisanslanacağını belirleyin (iş ortakları Kendi Lisansını Getir ve/veya Kullandıkça Öde modellerini destekleyebilir). Azure Güvenlik Merkezi'ni kullanarak Azure VM'lerinizde web uygulaması güvenlik duvarının nasıl dağıtılacağı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md).

**Sanal Ağ**: Azure Güvenlik Merkezi, [Ağ Güvenlik Grubu](../virtual-network/virtual-networks-nsg.md)'nun uygulandığını ve gelen trafik kurallarıyla düzgün şekilde yapılandırıldığını denetlemek için [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) altyapınızı ve yapılandırmanızı değerlendirir. Hangi trafik kurallarının tanımlanacağını dikkate almanız ve ilgili güvenlik önerilerini uygulayacak kişilerle bu konu hakkında iletişim kurmanız gerekir.

## Veri Koleksiyonu ve Storage

Güvenlik izlemenin tüm VM'leriniz için kullanılabilir olmasını sağlamak üzere her bir aboneliğinize için veri koleksiyonunu açmanızı kesinlikle öneririz. Veri koleksiyonu, Azure Monitoring Agent (ASMAgentLauncher.exe) ve Azure Security Monitoring uzantısı (ASMMonitoringAgent.exe) aracılığıyla etkinleştirilir. 

Azure Security Monitoring uzantısı, güvenlikle ilgili çeşitli yapılandırmaları tarar ve güvenlik günlüklerini sanal makineden toplar. Bu veriler, belirlediğiniz Storage hesabına gönderilir. Ayrıca, tarama yöneticisi (ASMSoftwareScanner.exe) sanal makineye de yüklenir ve yama tarayıcısı olarak kullanılır.

Veri koleksiyonu güvenlik ilkesinde etkinleştirildikten sonra izleme aracısı ve uzantılar, Azure'da sağlanan tüm var olan ve yeni desteklenen sanal makinelere otomatik olarak yüklenir.  Aracının işlemi müdahaleci değildir ve VM'nin performansını etkilemez.
 
Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Daha önce dağıtılan izleme aracılarını silmek için Aracıları Sil menü seçeneğini belirleyin.

> [AZURE.NOTE] Desteklenen VM'lerin listesini bulmak için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).

Sanal makinelerinizin çalıştığı her bir bölge için bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçin. Her bölge için bir depolama hesabı seçmezseniz sizin için bir tane oluşturulur. Her bölge için bir depolama konumu seçebilir veya tüm bilgileri merkezi bir konumda saklayabilirsiniz. Güvenlik ilkeleri, Azure abonelik düzeyinde ve kaynak grubu düzeyinde ayarlanabilirken depolama hesabına yönelik bölge yalnızca abonelik düzeyinde seçilebilir.

Farklı Azure kaynakları arasında paylaşılan bir depolama hesabı kullanıyorsanız boyut sınırlamaları ve kısıtlamalar hakkında daha fazla bilgi için [Azure Storage Hizmeti Ölçeklenebilirliği ve Performans Hedefleri](../storage/storage-scalability-targets.md) makalesini okuyun. Ayrıca sizin aboneliğinizin de depolama hesabı sınırlamaları vardır, bu sınırlamaları daha iyi anlamak için bkz.[Azure aboneliğinin ve hizmetlerinin sınırlamaları, kotaları ve kısıtlamaları](../azure-subscription-service-limits).

> [AZURE.NOTE] Bu depolama ile ilişkili maliyetler Azure Güvenlik Merkezi hizmeti fiyatına dahil değildir ve ayrı olarak normal [Azure depolama fiyatlarına](https://azure.microsoft.com/pricing/details/storage/) göre ücretlendirilir. 

Ayrıca, performans ve ölçeklenebilirlik ile ilgili dikkat edilmesi noktaların da Azure ortamı boyutunuza ve depolama hesabınızı kullanan kaynaklara göre planlanması gerekir. Daha fazla bilgi için [Microsoft Azure Storage Performansı ve Ölçeklenebilirlik Yapılacaklar Listesi](../storage/storage-performance-checklist.md) bölümünü gözden geçirin.

## Devam eden güvenlik izleme

Azure Güvenlik Merkezi önerilerinin ilk yapılandırması ve uygulamasından sonraki adım, Azure Güvenlik Merkezi işlem süreçlerini dikkate almaktır. 

Azure Güvenlik Merkezi'ne Azure Portal'dan erişmek için **Gözat**'a tıklayıp **Filtre** alanına **Güvenlik Merkezi** yazabilirsiniz. Kullanıcıların aldıkları görünümler, bu uygulanan filtrelere göre yapılır.

Azure Güvenlik Merkezi, normal işlem yordamlarınıza müdahale etmez, pasif olarak dağıtımlarınızı izler ve etkinleştirdiğiniz güvenlik ilkelerine bağlı olarak öneriler sağlar.

Azure Güvenlik Merkezi panosu iki ana iki bölüme ayrılır: 

- Önleme 
- Algılama 

Geçerli Azure ortamınız için Azure Güvenlik Merkezi'nde veri koleksiyonunu ilk defa etkinleştirdiğinizde **Öneriler** dikey penceresinde yer alan veya her bir kaynak için (**Sanal Makine**, **Ağ**, **SQL** ve **Uygulama**) sunulan tüm önerileri gözden geçirdiğinize emin olun. 

Tüm önerilere değindikten sonra değinilen tüm kaynaklar için **Önleme** bölümünün yeşil olması gerekir. Yalnızca kaynak güvenlik durumu ve öneriler kutucuklarındaki değişikliklere göre eyleme geçeceğiniz için devam eden izleme bu noktada daha kolay olur.

**Algılama** bölümü daha reaktiftir; bunlar, ya şu anda var olan veya geçmişte oluşan ya da Azure Güvenlik Merkezi denetimleri ile 3. taraf sistemleri tarafından algılanan sorunlarla ilgili uyarılardır. Güvenlik Uyarıları kutucuğu, her gün bulunan tehlike algılama uyarıları sayısını ve bunların farklı önem derecesi kategorilerine (düşük, orta, yüksek) dağılımlarını temsil eden çubuk grafiklerini gösterir. Güvenlik Uyarıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve uyarılara yanıt verme](security-center-managing-and-responding-alerts.md) 

> [AZURE.NOTE] Ayrıca, Azure Güvenlik Merkezi verilerinizi görselleştirmek için Microsoft Power BI'dan da yararlanabilirsiniz. Bkz. [Power BI ile Azure Güvenlik Merkezi verilerinden öngörü alma](security-center-powerbi.md).

### Yeni veya değiştirilmiş kaynakları izleme

Çoğu Azure ortamı, düzenli olarak çalışmaya başlatılan ve yavaşlatılan yeni kaynaklarla dinamiktir (örneğin, yapılandırmalar veya değişiklikler vb.) Azure Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Azure Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Veri Koleksiyonu Güvenlik İlkesi'nde etkinleştirilirse sanal makineleriniz için ek izleme işlevleri otomatik olarak etkinleştirilir. 

![Temel alanlar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3.png) 

1.  Sanal makineler için **Kaynak güvenlik durumu** kutucuğuna erişip **Virtual Machines** seçeneğine tıklayın. Veri koleksiyonunu etkinleştirme sırasında yaşanan sorunlar veya ilgili öneriler **İzleme Önerileri** bölümünde kullanıma sunulur.
2.  Yeni kaynak için (eğer varsa) hangi güvenlik risklerinin tanımlandığını görmek için **Öneriler**'i görüntüleyin.
3.  Ortamınıza yeni VM'ler eklendiğinde, başlangıçta yalnızca işletim sisteminin yüklenmesi durumu çok yaygındır. Bu VM'ler tarafından kullanılacak diğer uygulamaları dağıtmak için kaynak sahibine bir miktar süre gerekebilir.  İdeal olarak, bu iş yükünün son amacını bilmeniz gerekir. Bu iş yükü bir Uygulama Sunucusu mu olacak? Bu yeni iş yükünün ne olacağına bağlı olarak, bu iş akışındaki üçüncü adım olan uygun **Güvenlik İlkesi**'ni etkinleştirebilirsiniz.
4.  Yeni kaynaklar Azure ortamınıza eklendikçe **Güvenlik Uyarıları** kutucuğunda yeni uyarıların görünmesi mümkündür. Bu kutucukta yeni uyarılar olup olmadığını her zaman doğrulayın ve Azure Güvenlik Merkezi önerilerine göre eyleme geçin.

Ayrıca güvenlik riskleri, önerilen temel yapılandırmalardan sapma durumları ve güvenlik uyarıları oluşturan yapılandırma değişikliklerini tanımlamak için var olan kaynakların durumunu düzenli aralıklarla izlemek isteyeceksiniz. Azure Güvenlik Merkezi Panosu'nda başlatın. Tutarlı bir şekilde gözden geçirmek için burada üç temel alanınız vardır.

![İşlemler](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png) 

1.  **Kaynak güvenlik durumu** paneli, temel kaynaklarınıza hızlı erişim sağlar. Virtual Machines'inizi, Ağları, SQL'i ve Uygulamaları izlemek için bu seçeneği kullanın. 
2.  **Öneriler** paneli Azure Güvenlik Merkezi önerilerini gözden geçirmenizi sağlar. Devam eden izleme işlemi sırasında önerileri günlük olarak almadığınızı fark edebilirsiniz. İlk Azure Güvenlik Merkezi ayarlamasında tüm önerilere değindiğiniz için bu durum normaldir. Bundan dolayı bu bölümde her gün yeni bilgilere sahip olmayabilirsiniz ve bilgilere sadece gerekli olduğunda erişmeniz gerekebilir.
3.  **Algılama** paneli çok sık veya çok seyrek aralıklarla değişebilir. Güvenlik uyarılarınızı her zaman gözden geçirin ve Azure Güvenlik Merkezi önerilerine göre eyleme geçin. 

## Olay Yanıtı

Azure Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir.

> [AZURE.NOTE] Bu makale, kendi Olay Yanıtı planınızı oluşturmanız için yardım etme amacını taşımaz. Kendi planınızı oluşturmanıza yardımcı olması için National Institute of Standards and Technology'nin (NIST) [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) belgesini kullanabilirsiniz.

Her Güvenlik Uyarısı, saldırının yapısını daha iyi anlamanız ve olası risk azaltmalarını önermek için kullanılabilecek bilgiler sağlar. Ayrıca bazı uyarılar, daha fazla bilgi veya Azure'daki diğer bilgi kaynakları için bağlantılar sağlar. Daha fazla araştırma hakkında bilgi edinmek ve risk azaltmaya başlamak için sağlanan bilgileri kullanabilirsiniz.

Aşağıdaki örnek, şüpheli bir RDP etkinliğinin gerçekleşmesini gösterir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5.png)

Gördüğünüz gibi bu dikey pencere, saldırının gerçekleştiği zaman, kaynak ana bilgisayar adı, hedef VM ile ilgili ayrıntıları gösterir ve ayrıca öneri adımları sunar. Bazı durumlarda, saldırının kaynak bilgileri boş olabilir. Bu türden bir davranış hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Uyarıları'nda Eksik Kaynak Bilgileri](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

| **Düzey**        | **Saldırı Algılama**                     | **Önerilen Risk Azaltma**                                                                       |
|------------------|------------------------------------------|-----------------------------------------------------------------------------------------------|
| Ağ          | RDP deneme yanılmasını algılama                | Gereksiz açık uç noktasını kaldırarak saldırı yüzeyini azaltma, ACL'leri yapılandırma, güçlü parola kullanma |
| Sanal makineler | Yanlış dizinden çalışan SVCHOST | VM'yi farklı bir alt ağa (yalıtılmış) taşıma, tarama ve yeniden oluşturma                        |
| Uygulama      | Azure Veritabanı'na SQL ekleme          | Veritabanı yapılandırmalarını sabitleme                                                                |



## Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz



<!---HONumber=Jun16_HO2-->


