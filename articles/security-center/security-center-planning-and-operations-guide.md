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
   ms.date="08/11/2016"
   ms.author="yurid"/>


# Azure Güvenlik Merkezi planlama ve işlemler kılavuzu
Bu kılavuz, kurumları Azure Güvenlik Merkezi'ni kullanmayı planlayan bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenlik çözümleyicileri ve bulut yöneticilerine yöneliktir.

## Planlama Kılavuzu
Bu kılavuzda, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline dayanarak Güvenlik Merkezi kullanımınızı iyileştirmek için izleyeceğiniz adımlar ve görevlerin kümesi sağlanmıştır. Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir. Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

- Güvenlik Rolleri ve Erişim Denetimleri
- Güvenlik İlkeleri ve Öneriler
- Veri Koleksiyonu ve Storage
- Devam Eden Güvenlik İzleme
- Olay Yanıtı

Sonraki bölümde, gereksinimlerinize bağlı olarak bu alanların her birini nasıl planlayacağınızı ve bu önerileri nasıl uygulayacağınızı öğreneceksiniz.

> [AZURE.NOTE] Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).


## Güvenlik rolleri ve erişim denetimleri
Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıdaki diyagramda, hayali kişiler ile bunların rollerinin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-ga.png)

Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örneğin:

**Jeff (Bulut İş Yükü Sahibi)**

- Bir bulut iş yükünü ve onunla ilgili kaynakları yönetme
- Korumaları şirket güvenlik ilkesiyle uyumlu bir şekilde uygulamak ve sürdürmekle sorumlu

**Emel (CISO/CIO)**

- Şirket güvenliğinin tüm boyutlarından sorumlu
- Bulut iş yüklerinin tamamında şirketin güvenlik duruşunu anlamak istiyor
- Önemli saldırı ve risklerden haberdar olması gerekiyor

**David (BT Güvenliği)**

- Uygun korumaların uygulanmakta olduğundan emin olmak için şirketin güvenlik ilkelerini ayarlar
- İlkelerle uyumluluğu izler
- Yöneticiler veya denetçiler için raporlar oluşturur

**Zehra (Güvenlik İşlemleri)**

- Güvenlik uyarılarını 7/24 izler ve bunlara yanıt verir
- Bulut İş Yükü Sahibi veya BT Güvenlik Analistine İletir

**Salih (Güvenlik Analisti)**

- Atakları araştırır
- Uyarıları düzeltir veya Bulut İş Yükü Sahibi ile birlikte çalışarak düzeltme uygular 


Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../active-directory/role-based-access-built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../active-directory/role-based-access-control-configure.md) kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişmi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Önceki diyagramda açıklanan kişiler kullanıldığında aşağıdaki RBAC gerekli olur:

**Jeff (Bulut İş Yükü Sahibi)**

- Kaynak Grubu Sahibi/Ortak Çalışanı

**David (BT Güvenliği)**

- Abonelik Sahibi/Ortak Çalışanı

**Zehra (Güvenlik İşlemleri)**

- Uyarıları Görüntülemek için Abonelik Okuyucusu
- Uyarıları Kapatmak İçin Gerekli Olan Abonelik Sahibi/Ortak Çalışanı

**Salih (Güvenlik Analisti)**

- Uyarıları Görüntülemek için Abonelik Okuyucusu
- Uyarıları Kapatmak veya Düzeltmek İçin Gerekli Olan Abonelik Sahibi/Ortak Çalışanı
- Storage Hizmetine Erişim Gerekli Olabilir

Dikkate alınması gereken bazı diğer önemli bilgiler:

- Yalnızca abonelik Sahipleri ve Katkıda Bulunanları güvenlik ilkesini düzenleyebilir
- Yalnızca abonelik ve kaynak grubu Sahipleri ve Katkıda bulunanları bir kaynak için güvenlik önerilerini uygulayabilir.

Güvenlik Merkezi için RBAC kullanarak erişim denetimini planlarken Güvenlik Merkezi'ni kuruluşunuzdaki hangi kişilerin kullanacağını anladığınızdan emin olun. Ayrıca, bunların hangi türde görevler gerçekleştireceğini anlayın ve daha sonra RBAC’yi buna göre yapılandırın.

> [AZURE.NOTE] Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca kaynakların güvenlik durumu hakkındaki bilgileri görüntülemesi gereken ancak eyleme geçmeyecek olan kullanıcıların (örneğin, önerileri uygulamak veya ilkeleri düzeltmek), Okuyucu rolüne atanmaları gerekir.

## Güvenlik ilkeleri ve öneriler
Güvenlik ilkeleri, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız.

Abonelik düzeyinde etkinleştirilen ilkeler, aşağıdaki diyagramda gösterildiği gibi abonelikteki tüm kaynak gruplarına otomatik olarak yayılır:

![Güvenlik İlkeleri](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-ga.png)

Önceki şekilde gösterildiği gibi, kaynak gruplarına yönelik güvenlik ilkeleri abonelik düzeyinden devralınabilir.

Farklı ilke kümesi gerektiren kaynak grubunda kaynaklarınızın olduğu bazı senaryolarda, devralmayı devre dışı bırakabilir ve belirli bir Kaynak Grubuna özel ilkeler uygulayabilirsiniz.

Belirli kaynak gruplarında özel ilkelere gereksinim duyuyorsanız kaynak grubundaki devralmayı devre dışı bırakmanız ve güvenlik ilkelerini değiştirmeniz gerekir. Örneğin, SQL Saydam Veri Şifrelemesi ilkesini gerektirmeyen bazı iş yükleriniz varsa ilkeyi abonelik düzeyinde devre dışı bırakın ve yalnızca SQL TDE'nin gerekli olduğu kaynak gruplarında etkinleştirin.

Farklı kaynak grupları için özel ilkeler oluşturmaya başladığınızda ilke çakışması olması durumunda (aboneliğe karşı kaynak grup) kaynak grup ilkesinin korunacağını bilerek ilke dağıtımınızı planlamanız gerekir.

> [AZURE.NOTE] Hangi ilkelerin değiştiğini gözden geçirmeniz gerekiyorsa [Azure Denetim Günlükleri](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/)'ni kullanabilirsiniz. İlke değişiklikleri her zaman Azure Denetim Günlükleri'ne kaydedilir.

### Güvenlik önerileri

Güvenlik ilkelerini yapılandırmadan önce her bir [güvenlik önerisini](security-center-recommendations.md) gözden geçirip bu ilkelerin sahip olduğunuz çeşitli abonelikler ve kaynak grupları için uygun olup olmadığını belirleyin. Güvenlik Önerilerinin uygulanması için hangi eylemin gerçekleştirildiğini anlamanız da önemlidir.

**Uç Nokta Koruması**: Bir sanal makinenin etkinleştirilmiş bir uç nokta koruma çözümü yoksa Güvenlik Merkezi bir çözüm yüklemenizi önerir. Şirket içinde zaten benimsediğiniz bir uç nokta koruma çözümü varsa, aynı kötü amaçlı yazılımdan koruma yazılımını Azure VM'leriniz için kullanıp kullanmayacağınıza karar vermeniz gerekir. Güvenlik Merkezi, size çeşitli uç nokta koruma seçenekleri sunar.  Ücretsiz Microsoft Kötü Amaçlı Yazılımdan Koruma Yazılımı'nı kullanabilir veya tümleşik iş ortaklarının uç nokta koruma çözümleri listesinden seçebilirsiniz. Güvenlik Merkezi'ni kullanarak kötü amaçlı yazılımdan koruma yazılımı dağıtma hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde Uç Nokta Koruması Yükleme](security-center-install-endpoint-protection.md).

**Sistem Güncelleştirmeleri**: Güvenlik Merkezi, IaaS ve Cloud Services (PaaS) için güvenlik veya kritik işletim sistemi güncelleştirmeleri eksik olan sanal makineleri tanımlar. Gerektiğinde güncelleştirmeleri uygulamaktan kimin sorumlu olduğunu ve bunların nasıl uygulanacağını dikkate alın. Birçok kuruluş WSUS, Windows Update veya başka bir araç kullanır.

**Temel Yapılandırmalar**: Sanal makine işletim sistemi yapılandırmaları önerilen temel yapılandırmalarla eşleşmiyorsa bir öneri kullanıma sunulur. [Buradan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) temel yapılandırmalar kümesini gözden geçirin ve işletim sistemi yapılandırmalarının nasıl uygulanacağını göz önünde bulundurun.

**Disk Şifrelemesi**: Şifrelenmemiş sanal makine diskleriniz varsa, Güvenlik Merkezi tarafından Azure Disk Şifrelemesi uygulamanız önerilir. Bu özellik, işletim sistemi ve veri diskleri için toplu şifreleme sağlamak amacıyla Windows için BitLocker ve Linux için DM-Crypt’ten yararlanır. Bu öneri tarafından bu şifrelemeyi nasıl gerçekleştireceğiniz hakkında yönergeler içeren [adım adım kılavuz](security-center-disk-encryption.md) bölümüne yönlendirilirsiniz.

Değinmeniz gereken birçok şifreleme senaryosu olduğunu unutmayın. Bu senaryoların her biri için benzersiz gereksinimleri planlamanız gerekir:

- Kendi şifreleme anahtarlarınızı kullanarak şifrelediğiniz VHD'lerden yeni Azure Virtual Machines'i şifreleme
- Azure Galerisi'nden oluşturulan yeni Azure Virtual Machines'i şifreleme
- Zaten Azure'da çalışan Azure Virtual Machines'i şifreleme

Gereksinimleri planlama, bu senaryoların her biri için farklı olacaktır. Bu senaryoların her birine ilişkin ayrıntılar için bkz. [Azure Disk Şifrelemesi teknik incelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Web Uygulaması Güvenlik Duvarı**: Güvenlik Merkezi, web uygulamaları çalıştıran sanal makineleri tanımlar ve bir Web Uygulaması Güvenlik Duvarı (WAF) yüklemenizi önerir. Kuruluşunuz için en uygun olan kullanılabilir iş ortağı çözümlerini değerlendirin ve çözümün nasıl lisanslanacağını belirleyin (iş ortakları Kendi Lisansını Getir ve/veya Kullandıkça Öde modellerini destekleyebilir). Azure Güvenlik Merkezi'ni kullanarak Azure VM'lerinizde web uygulaması güvenlik duvarı dağıtma hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi'nde bir web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md).

**Yeni Nesil Güvenlik Duvarı**: Check Point ve kısa süre sonra Cisco ile Fortinet gibi önde gelen satıcıların sanal makinelerini hazırlamanızı sağlar. Bu da ağ korumalarını Azure’da yerleşik olan Ağ Güvenlik Grupları'nın ötesine genişletir. Güvenlik Merkezi, Yeni Nesil Güvenlik Duvarı'nın önerildiği dağıtımları bulur ve sanal gereç sağlamanızı imkan tanır.

**Sanal Ağ**: Güvenlik Merkezi, [Ağ Güvenlik Grupları](../virtual-network/virtual-networks-nsg.md)'nın uygulanıp uygulanmadığını ve gelen trafik kurallarıyla düzgün şekilde yapılandırılıp yapılandırılmadığını denetlemek için [Azure Sanal Ağ](https://azure.microsoft.com/documentation/services/virtual-network/) altyapınızı ve yapılandırmanızı değerlendirir. Hangi trafik kurallarının tanımlanacağını dikkate almanız ve ilgili güvenlik önerilerini uygulayacak kişilerle bu konu hakkında iletişim kurmanız gerekir.

Güvenlik Merkezi, Azure aboneliğiniz için güvenlik kişi ayrıntılarını sağlamanızı önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Bu öneriyi etkinleştirme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](security-center-provide-security-contact-details.md) konusunu okuyun.

## Veri koleksiyonu ve depolama

Güvenlik izlemenin tüm VM'leriniz için kullanılabilir olmasını sağlamak üzere her bir aboneliğinize için veri koleksiyonunu açmanızı kesinlikle öneririz. Veri koleksiyonu, Azure Monitoring Agent (ASMAgentLauncher.exe) ve Azure Security Monitoring uzantısı (ASMMonitoringAgent.exe) aracılığıyla etkinleştirilir.

Azure Security Monitoring uzantısı, güvenlikle ilgili çeşitli yapılandırmaları tarar ve güvenlik günlüklerini sanal makineden toplar. Bu veriler, belirlediğiniz Storage hesabına gönderilir. Ayrıca, tarama yöneticisi (ASMSoftwareScanner.exe) sanal makineye de yüklenir ve yama tarayıcısı olarak kullanılır.

Veri koleksiyonu güvenlik ilkesinde etkinleştirildikten sonra izleme aracısı ve uzantılar, Azure'da sağlanan tüm var olan ve yeni desteklenen sanal makinelere otomatik olarak yüklenir.  Aracının işlemi müdahaleci değildir ve VM'nin performansını etkilemez.

> [AZURE.NOTE] Azure Güvenlik İzleme Aracısı ile ilgili sorunları gidermek için [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](security-center-troubleshooting-guide.md)’nu okuyun.

Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Daha önce dağıtılan izleme aracılarını silmek için Aracıları Sil menü seçeneğini belirleyin.

> [AZURE.NOTE] Desteklenen VM'lerin listesini bulmak için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).

Sanal makinelerinizin çalıştığı her bir bölge için bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçin. Her bölge için bir depolama hesabı seçmezseniz sizin için bir tane oluşturulur. Her bölge için bir depolama konumu seçebilir veya tüm bilgileri merkezi bir konumda saklayabilirsiniz. Güvenlik ilkeleri, Azure abonelik düzeyinde ve kaynak grubu düzeyinde ayarlanabilirken depolama hesabına yönelik bölge yalnızca abonelik düzeyinde seçilebilir.

Farklı Azure kaynakları arasında paylaşılan bir depolama hesabı kullanıyorsanız boyut sınırlamaları ve kısıtlamalar hakkında daha fazla bilgi için [Azure Storage Hizmeti Ölçeklenebilirliği ve Performans Hedefleri](../storage/storage-scalability-targets.md) makalesini okuyun. Ayrıca sizin aboneliğinizin de depolama hesabı sınırlamaları vardır, bu sınırlamaları daha iyi anlamak için bkz.[Azure aboneliğinin ve hizmetlerinin sınırlamaları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).

> [AZURE.NOTE] Bu depolama ile ilişkili maliyetler Güvenlik Merkezi hizmeti fiyatına dahil değildir ve normal [Azure depolama fiyatları](https://azure.microsoft.com/pricing/details/storage/) üzerinden ayrı olarak ücretlendirilir.

Ayrıca, performans ve ölçeklenebilirlik ile ilgili dikkat edilmesi noktaların da Azure ortamı boyutunuza ve depolama hesabınızı kullanan kaynaklara göre planlanması gerekir. Daha fazla bilgi için [Microsoft Azure Storage Performansı ve Ölçeklenebilirlik Yapılacaklar Listesi](../storage/storage-performance-checklist.md) bölümünü gözden geçirin.

## Devam eden güvenlik izleme

Güvenlik Merkezi önerilerinin ilk yapılandırması ve uygulamasından sonraki adım, Güvenlik Merkezi işlem süreçlerini dikkate almaktır.

Azure portaldan Güvenlik Merkezi'ne erişmek için **Gözat**'a tıklayıp **Filtre** alanına **Güvenlik Merkezi** yazabilirsiniz. Kullanıcıların aldıkları görünümler, bu uygulanan filtrelere göre yapılır.

Güvenlik Merkezi normal işlem yordamlarınıza müdahale etmez, dağıtımlarınızı pasif olarak izler ve etkinleştirdiğiniz güvenlik ilkelerine bağlı olarak öneriler sağlar.

Güvenlik Merkezi panosu iki ana iki bölüme ayrılır:

- Önleme
- Algılama

Geçerli Azure ortamınıza yönelik Güvenlik Merkezi'nde veri toplamayı ilk defa etkinleştirdiğinizde, **Öneriler** dikey penceresinde yer alan veya her bir kaynak (**Sanal Makine**, **Ağ**, **SQL** ve **Uygulama**) için sunulan tüm önerileri gözden geçirdiğinizden emin olun.

Tüm önerilere değindikten sonra değinilen tüm kaynaklar için **Önleme** bölümünün yeşil olması gerekir. Yalnızca kaynak güvenlik durumu ve öneriler kutucuklarındaki değişikliklere göre eyleme geçeceğiniz için devam eden izleme bu noktada daha kolay olur.

**Algılama** bölümü daha reaktiftir; bunlar, ya şu anda var olan veya geçmişte oluşan ya da Güvenlik Merkezi denetimleri ile 3. taraf sistemleri tarafından algılanan sorunlarla ilgili uyarılardır. Güvenlik Uyarıları kutucuğu, her gün bulunan tehlike algılama uyarıları sayısını ve bunların farklı önem derecesi kategorilerine (düşük, orta, yüksek) dağılımlarını temsil eden çubuk grafiklerini gösterir. Güvenlik Uyarıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve uyarılara yanıt verme](security-center-managing-and-responding-alerts.md)

> [AZURE.NOTE] Güvenlik Merkezi verilerinizi görselleştirmek için Microsoft Power BI'dan da yararlanabilirsiniz. Bkz. [Power BI ile Azure Güvenlik Merkezi verilerinden öngörü alma](security-center-powerbi.md).

### Yeni veya değiştirilmiş kaynakları izleme

Çoğu Azure ortamı, düzenli olarak çalışmaya başlatılan ve yavaşlatılan yeni kaynaklarla dinamiktir (örneğin, yapılandırmalar veya değişiklikler vb.) Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Buna PaaS web rolleri ve çalışan rolleri de dahildir. Veri Koleksiyonu [Güvenlik İlkesi](security-center-policies.md)'nde etkinleştirilirse sanal makineleriniz için ek izleme işlevleri otomatik olarak etkinleştirilir.

![Temel alanlar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-ga.png)

1.  Sanal makineler için **Kaynak güvenlik durumu** kutucuğuna erişip **Virtual Machines** seçeneğine tıklayın. Veri koleksiyonunu etkinleştirme sırasında yaşanan sorunlar veya ilgili öneriler **İzleme Önerileri** bölümünde kullanıma sunulur.
2.  Yeni kaynak için (eğer varsa) hangi güvenlik risklerinin tanımlandığını görmek için **Öneriler**'i görüntüleyin.
3.  Ortamınıza yeni VM'ler eklendiğinde, başlangıçta yalnızca işletim sisteminin yüklenmesi durumu çok yaygındır. Bu VM'ler tarafından kullanılacak diğer uygulamaları dağıtmak için kaynak sahibine bir miktar süre gerekebilir.  İdeal olarak, bu iş yükünün son amacını bilmeniz gerekir. Bu iş yükü bir Uygulama Sunucusu mu olacak? Bu yeni iş yükünün ne olacağına bağlı olarak, bu iş akışındaki üçüncü adım olan uygun **Güvenlik İlkesi**'ni etkinleştirebilirsiniz.
4.  Yeni kaynaklar Azure ortamınıza eklendikçe **Güvenlik Uyarıları** kutucuğunda yeni uyarıların görünmesi mümkündür. Bu kutucukta yeni uyarılar olup olmadığını her zaman doğrulayın ve Güvenlik Merkezi önerilerine göre eyleme geçin.

Ayrıca güvenlik riskleri, önerilen temel yapılandırmalardan sapma durumları ve güvenlik uyarıları oluşturan yapılandırma değişikliklerini tanımlamak için var olan kaynakların durumunu düzenli aralıklarla izlemek isteyeceksiniz. Güvenlik Merkezi Panosu'nda başlayın. Tutarlı bir şekilde gözden geçirmek için burada üç temel alanınız vardır.

![İşlemler](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png)

1.  **Kaynak güvenlik durumu** paneli, temel kaynaklarınıza hızlı erişim sağlar. Virtual Machines'inizi, Ağları, SQL'i ve Uygulamaları izlemek için bu seçeneği kullanın.
2.  **Öneriler** paneli Güvenlik Merkezi önerilerini gözden geçirmenizi sağlar. Devam eden izleme işlemi sırasında önerileri günlük olarak almadığınızı fark edebilirsiniz; ilk Güvenlik Merkezi ayarlamasında tüm önerilere değindiğiniz için bu durum normaldir. Bundan dolayı bu bölümde her gün yeni bilgilere sahip olmayabilirsiniz ve bilgilere sadece gerekli olduğunda erişmeniz gerekebilir.
3.  **Algılama** paneli çok sık veya çok seyrek aralıklarla değişebilir. Güvenlik uyarılarınızı her zaman gözden geçirin ve Güvenlik Merkezi önerilerine göre eyleme geçin.

## Olay yanıtı

Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir. Güvenlik Merkezi tehdit algılama özelliğinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md) konusunu okuyun.

Bu makale kendi Olay Yanıtı planınızı oluşturmanıza yardımcı olmaya yönelik değildir, ancak olay yanıtı aşamalarının temeli olarak Bulut yaşam döngüsünde Microsoft Azure Güvenlik Yanıtı kullanılacaktır. Aşamalar aşağıdaki diyagramda gösterilmiştir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [AZURE.NOTE] Kendi planınızı oluşturmanıza yardımcı olması için National Institute of Standards and Technology'nin (NIST) [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) belgesini kullanabilirsiniz.

Güvenlik Merkezi Uyarılarını aşağıdaki aşamalar sırasında kullanabilirsiniz:

- **Algılama**: bir veya daha fazla kaynakta şüpheli bir etkinliği tanımlayın. 
- **Değerlendirme**: şüpheli etkinlik hakkında daha fazla bilgi edinmek için ilk değerlendirmeyi gerçekleştirin.
- **Tanılama**: sorunu çözmeye yönelik teknik yordamı yürütmek için düzeltme adımlarını kullanın.

Her Güvenlik Uyarısı, saldırının yapısını daha iyi anlamanız ve olası risk azaltmalarını önermek için kullanılabilecek bilgiler sağlar. Ayrıca bazı uyarılar, daha fazla bilgi veya Azure'daki diğer bilgi kaynakları için bağlantılar sağlar. Daha fazla araştırma ve gidermeyi başlatma hakkında verilen bilgileri kullanabilirsiniz.

Aşağıdaki örnek, gerçekleşmekte olan şüpheli bir RDP etkinliğini gösterir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Gördüğünüz gibi bu dikey pencere, saldırının gerçekleştiği zaman, kaynak ana bilgisayar adı, hedef VM ile ilgili ayrıntıları gösterir ve ayrıca öneri adımları sunar. Bazı durumlarda, saldırının kaynak bilgileri boş olabilir. Bu türden bir davranış hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Uyarıları'nda Eksik Kaynak Bilgileri](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

> [AZURE.NOTE] [Bir Olay Yanıtı için Azure Güvenlik Merkezi ve Microsoft Operations Management Suite’ten Yararlanma](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) videosunda, bu aşamaların her birinde Güvenlik Merkezi’nin nasıl kullanılabileceğini anlamanıza yardımcı olabilecek bazı tanıtımlar görebilirsiniz.

## Ayrıca bkz.
Bu belgede, Güvenlik Merkezi benimsemeyi nasıl planlayacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Sep16_HO3-->


