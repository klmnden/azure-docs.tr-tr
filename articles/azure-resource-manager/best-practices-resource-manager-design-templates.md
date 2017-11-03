---
title: "Azure şablonları karmaşık çözümleri tasarlama | Microsoft Docs"
description: "Karmaşık senaryolar için Azure Resource Manager şablonları tasarlamak için en iyi uygulamaları gösterir"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: dcc31f7a8c85a8f7fbd554371a66fb1e348bca17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Karmaşık çözümleri dağıtımı sırasında Azure Resource Manager şablonları için Tasarım desenleri
Azure Resource Manager şablonları temel alarak esnek bir yaklaşım kullanarak karmaşık topolojiler hızlı ve tutarlı bir şekilde dağıtabilirsiniz. Çekirdek teklifleri geliştikçe bu dağıtımlar kolayca uyarlayabilirsiniz veya türevleri aykırı değer senaryoları ya da müşteriler için uygun hale getirmek için.

Bu konuda daha büyük bir Teknik İnceleme bir parçasıdır. Tam kağıt okumak için karşıdan [World sınıfı Azure Resource Manager şablonları konuları ve kanıtlanmış yöntemler](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Şablonları, uyumluluk ve Okunabilirlik JavaScript nesne gösterimi (JSON) temel Azure Kaynak Yöneticisi'nin avantajları birleştirebilirsiniz. Şablonları kullanarak şunları yapabilirsiniz:

* Topolojileri ve yüklerini tutarlı bir şekilde dağıtın.
* Tüm kaynaklarınızı birlikte kaynak gruplarını kullanarak bir uygulama yönetin.
* Rol tabanlı erişim denetimi (RBAC) kullanıcılar, gruplar ve hizmetler için uygun erişim vermek için geçerlidir.
* Etiketleme ilişkilendirmeleri toplamaları faturalama gibi görevleri kolaylaştırmak için kullanın.

Bu makalede tüketim senaryoları, mimari ve bizim tasarım oturumlar ve gerçek şablonu uygulamaları Azure Müşteri danışma ekibi (AzureCAT) müşterilerle sırasında tanımlanan uygulama desenleri ayrıntıları sağlar. Uygulamaları geliştirme üst Linux tabanlı OSS teknolojilerini 12 şablonları tarafından haberdar kanıtlanmış akademik gölgeden uzak Bu yaklaşımlar: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, DataStax kuruluş tarafından Apache güç kaynağı Cassandra, Elasticsearch, Jenkins, MongoDB, PostgreSQL, Redis ve Nagios. 

Bu makalede bu world sınıfı Azure Resource Manager şablonları düzenlenmesine yardımcı olacak yöntemler kanıtlanmış paylaşır.  

Müşteriler bizim iş birkaç Resource Manager şablonu kullanımı deneyimleri kuruluşlar, sistem tümleştiricileri (SI) s ve CSV arasında belirledik. Aşağıdaki bölümler, farklı müşteri türleri için ortak senaryolar ve desenler üst düzey bir genel bakış sağlar.

## <a name="enterprises-and-system-integrators"></a>Kuruluşlar ve sistem tümleştiricileri
Büyük kuruluşlar içinde biz sık Resource Manager şablonları iki tüketicileri bkz: İç yazılım geliştirme ekipleri ve kurumsal BT. İlgili noktaların aynısı uygulamak için senaryoları senaryoları SIS eşlenecek kuruluşlar için bulduk.

### <a name="internal-software-development-teams"></a>İç yazılım geliştirme ekiplerinin
İşinizi desteklemek üzere yazılım ekibinizin geliştirir, şablonları teknolojileri iş özgü çözümleri kullanmak için hızlı bir şekilde dağıtmak için kolay bir yol sağlar. Şablonları, hızlı bir şekilde gerekli becerilere kazanmak ekip üyelerinin etkinleştirmek eğitim ortamlar oluşturmak için de kullanabilirsiniz.

Şablon olarak kullanabilirsiniz-genişletmek veya bunları gereksinimlerinize uyacak şekilde oluşturun. İçinde şablonları etiketleme kullanarak, takım, proje, tek tek ve eğitim gibi çeşitli görünümlerle fatura özeti sağlayabilirsiniz.

İşletmeler genellikle bir çözümün tutarlı bir dağıtım için bir şablon oluşturmak için yazılım geliştirme ekipleri istiyor. Bu ortam içinde belirli öğeleri sabit kalır ve geçersiz kılınamaz şablon kısıtlamaları kolaylaştırır. Örneğin, bir banka Programcı Kişisel depolama hesabı için veri göndermek için bir bankacılık çözümü gözden edilemez şekilde RBAC dahil etmek için bir şablon gerektirebilir.

### <a name="corporate-it"></a>Kurumsal BT
Kurumsal BT kuruluşları bulut kapasite ve bulutta barındırılan özellikleri teslim etmek için şablonlar normalde kullanın.

#### <a name="cloud-capacity"></a>Bulut kapasitesi
Genel bir bulut kapasitesi ekiplerin sağlamak kurumsal BT grupları için "standart boyutları küçük, Orta gibi sunumu ve büyük olan ısı boyutlarına" yoludur. Boyutta ısı teklifleri farklı kaynak türleri karıştırabilirsiniz ve mümkün kılan Standartlaştırma düzeyini sağlarken miktarları şablonları kullanın. Şablonlar, şirket ilkelerini uygulamaya zorlar ve etiketleme kullanan kuruluşlar için geri ödeme sağlamak için kullandığı tutarlı bir şekilde kapasitesi sunar.

Örneğin, geliştirme, test ve üretim ortamlarını içinde yazılım geliştirme ekipleri çözümleri dağıtabilirsiniz sağlamanız gerekebilir. Ortam önceden tanımlanmış ağ topolojisi ve ortak Internet ve paket incelemesi erişimi yöneten kurallar gibi yazılım geliştirme ekipleri değiştiremezsiniz öğeleri vardır. Ayrıca bu ortamlar için kuruluşa özgü rolleri ortamı için birbirinden ayrı erişim haklarına sahip olabilir.

#### <a name="cloud-hosted-capabilities"></a>Bulutta barındırılan özellikleri
Bağımsız yazılım paketleri veya iş iç satırlarına sunulan bileşik teklifleri dahil olmak üzere, bulutta barındırılan özellikleri desteklemek için şablonları kullanabilirsiniz. Hizmet olarak analytics bileşik sunumun bir örnek olabilir — analizi, Görselleştirme ve diğer teknolojiler — önceden tanımlanmış ağ topolojisini en iyi duruma getirilmiş ve bağlı bir yapılandırmasına sunulan.

Bulutta barındırılan özellikleri bunlar derlendiği sunumu bulut kapasitesi tarafından kurulan güvenlik ve rol konuları etkilenir. Bu özellikler, olduğu gibi veya yönetilen bir hizmet olarak sunulur. İkincisi, erişim kısıtlı rolleri için yönetim amacıyla ortamına erişimi etkinleştirmek için gereklidir.

## <a name="cloud-service-vendors"></a>Bulut hizmeti satıcılar
Birçok csv görüştükten sonra müşteriler ve ilişkili gereksinimleri için Hizmetleri dağıtmak için kullanabileceğiniz birden çok yaklaşımlar belirledik.

### <a name="csv-hosted-offering"></a>CSV barındırılan teklifi
Kendi Azure aboneliğinizde teklifinizle barındırıyorsanız, iki barındırma yaklaşım ortaktır: her müşteri için ayrı bir dağıtım dağıtma veya tüm müşteriler için kullanılan bir paylaşılan altyapı destekleyen ölçek birimleri dağıtma.

* **Ayrı dağıtımları her müşteri için.** Ayrı dağıtımları her müşteri Sabit topolojiler farklı bilinen yapılandırmaları gerektirir. Bu dağıtımlar, başka bir sanal makine (VM) boyutları, değişen sayıda düğüm ve ilişkili depolama farklı miktarda olabilir. Etiketleme dağıtımları her müşteri dökümü faturalama için kullanılır. RBAC müşterilere bulut ortamlarına yönlerini erişmesine izin vermek için etkinleştirilebilir.
* **Ölçek birimleri paylaşılan çok kiracılı ortamlarda.** Bir şablon çok kiracılı ortamları için bir ölçek birimi temsil edebilir. Bu durumda, aynı alt tüm müşterileri desteklemek için kullanılır. Dağıtımları kullanıcı sayısı ve işlem sayısı gibi barındırılan teklifi için kapasite düzeyini teslim kaynakların bir grubu temsil eder. Bu ölçek birimleri artırılabilir veya isteğe bağlı gerektirdiği şekilde azaltılabilir.

### <a name="csv-offering-injected-into-customer-subscription"></a>Müşteri aboneliğini eklenen CSV teklifi
Son müşteriler tarafından ait abonelikleri içine yazılım dağıtmak isteyebilirsiniz. Şablonları, bir müşterinin Azure hesaba ayrı dağıtımları dağıtmak için kullanabilirsiniz.

Güncelleştirme ve dağıtım müşterinin hesabına içinde yönetmek için bu dağıtımlar RBAC kullanın.

### <a name="azure-marketplace"></a>Azure Market
Tanıtım ve Teklifleriniz Azure Marketi gibi bir Market üzerinden satmak için ayrı bir müşteri'nin Azure hesabında çalıştırmak dağıtım türlerini sunmak için şablonlar geliştirebilirsiniz. Bu ayrı dağıtımlar genellikle ısı boyutu (küçük, Orta, büyük), ürün/izleyici türü (topluluk, geliştirici, Kurumsal) veya özellik türünü (Basit, yüksek kullanılabilirlik) tanımlanabilir.  Bazı durumlarda, bu tür VM türüne veya disk sayısı gibi dağıtımın belirli öznitelikler belirtmenizi sağlar.

## <a name="oss-projects"></a>OSS projeleri
Açık kaynak projeleri içinde hızlı bir şekilde kanıtlanmış yöntemleri kullanarak bir çözümü dağıtmak bir topluluk Resource Manager şablonları etkinleştirin. Topluluk zamanla düzeltebilirsiniz şekilde şablonlar GitHub deposunda depolayabilir. Kullanıcıların bu şablonları kendi Azure abonelikleri dağıtın.

Aşağıdaki bölümlerde çözümünüzü tasarlamadan önce göz önünde bulundurmanız gereken şeyleri tanımlayın.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Bir VM içinde ve dışında ne olduğunu tanımlama
Şablonunuzu tasarlarken, sanal makineleri (VM'ler) içinde ve dışında nedir bakımından gereksinimleri bakmak yararlıdır:

* Etiketleme, ağ topolojisini sertifikaları/parolalar ve rol tabanlı erişim denetimi için başvuran gibi dış VM'ler ve diğer kaynakları, dağıtımınızın anlamına gelir. Tüm bu kaynaklar, şablonunuza bir parçasıdır.
* İç anlamına gelir yüklü yazılımlar ve genel istenen durum yapılandırması. VM uzantıları veya komut dosyaları gibi diğer mekanizmaları tamamen veya kısmen kullanılır. Bu mekanizmaların tanımlanır ve şablon tarafından yürütülen ancak içinde değil.

"Kutu içinde" yaptığınız etkinlik ortak örnekler-  

* Yükleme veya sunucu rolleri ve özellikleri kaldırma
* Yükleme ve yazılım düğümü veya küme düzeyinde yapılandırma
* Bir web sunucusu üzerindeki Web siteleri dağıtma
* Veritabanı şemalarını dağıtma
* Kayıt defteri ya da başka tür yapılandırma ayarlarını yönetme
* Dosyaları ve dizinleri yönetin
* Başlatma, durdurma ve işlem ve hizmetleri yönetmek
* Yerel grupları ve kullanıcı hesaplarını yönetme
* Yükleme ve paketleri (.msi, .exe, yum, vb.) yönetme
* Ortam değişkenleri yönetme
* Yerel (Windows PowerShell, bash, vs.) komut dosyalarını çalıştır

### <a name="desired-state-configuration-dsc"></a>İstenen durum yapılandırması (DSC)
Vm'leriniz dağıtım ötesinde iç durumuyla ilgili düşünürsek, bu dağıtımın "tanımlanan ve kullanıma yapılandırmasından kaynak denetimine kayma değil" emin olmak istersiniz. Bu yaklaşım, geliştiricilerin sağlar veya işlem personeli geçici olmayan vetted, test veya kaynak denetiminde kaydedilen bir ortamda değişiklik yok. Bu denetim, el ile yapılan değişiklikler kaynak denetiminde olmadığından önemlidir. Bunlar ayrıca standart dağıtımının bir parçası değildir ve gelecekte otomatik dağıtımları yazılım etkiler.

İç çalışanlarınızın istenen durum yapılandırması da güvenlik açısından önemlidir. Bilgisayar korsanlarının düzenli olarak tehlikeye ve yazılım sistemleri yararlanma çalışıyorsunuz. Başarılı olduğunda, dosyaları yüklemek ve aksi durumda güvenliği aşılmış bir sistem durumunu değiştirmek için yaygın bir sorundur. İstenen durum Yapılandırması'nı kullanarak, istenen ve gerçek durumu arasındaki farkları belirleyin ve bilinen yapılandırmasını geri yükleyin.

En popüler mekanizmalar DSC - PowerShell DSC, Chef ve Puppet için kaynak uzantıları vardır. Bu uzantılar her VM ilk durumunda dağıtabilirsiniz ve istenen durumu korunur emin olmak için de kullanılabilir.

## <a name="common-template-scopes"></a>Ortak şablon kapsamları
Ortaya çıkan üç anahtar çözüm şablonları kapsamları deneyimi bizim gördük. Bu üç kapsamları – kapasite, yetenek ve uçtan uca çözüm – aşağıdaki bölümlerde açıklanmıştır.

### <a name="capacity-scope"></a>Kapasite kapsamı
Kapasite kapsam düzenlemeleri ve ilkeleri ile uyumlu olacak şekilde önceden yapılandırılmış olan standart topolojisinde kaynak kümesi sunar. En yaygın örnek bir kurumsal BT veya SI senaryo standart geliştirme ortamında dağıtıyor.

### <a name="capability-scope"></a>Özellik kapsamı
Bir özellik kapsamı dağıtma ve belirli bir teknolojinin topolojisini yapılandırma odaklanmıştır. SQL Server'ı Cassandra, Hadoop gibi teknolojileri de dahil olmak üzere genel senaryoları.

### <a name="end-to-end-solution-scope"></a>Uçtan uca çözüm kapsamı
Uçtan uca çözüm kapsamını tek bir özellik hedeflenen ve bunun yerine birden çok yeteneklerini oluşan bir uçtan uca çözüm sunduğunuzdan üzerinde odaklanmıştır.  

Bir çözüm kapsamlı şablon kapsamı bir veya daha fazla özellik kapsamlı şablonlarıyla çözüme özel kaynakları, mantığı ve istenen durumu kümesi olarak ortaya çıkmaktadır. Çözüm kapsamlı bir şablon bir uçtan uca veri ardışık düzen çözüm şablonu örnektir. Şablon Kafka, Storm ve Hadoop gibi birden çok özellik kapsamlı çözüm şablonlarıyla çözüme özel topoloji ve durum karışımı.

## <a name="choosing-free-form-vs-known-configurations"></a>Serbest biçimli bilinen yapılandırmaları ve seçme
Bir şablon tüketicileri utmost esneklik, ancak birçok konuları serbest biçimli yapılandırmaları bilinen yapılandırmaları karşılaştırması kullanıp kullanmayacağınızı seçimi etkileyen başlangıçta düşünebilirsiniz. Bu bölümde anahtar müşteri gereksinimlerine ve bu belgede paylaşılan yaklaşım şeklinde teknik konuları tanımlar.

### <a name="free-form-configurations"></a>Serbest biçimli yapılandırmaları
Yüzey üzerinde ideal serbest biçimli yapılandırmaları ses. Bir VM türü seçin ve rasgele bir düğümü sayısını sağlayın olanak sağlar ve bu düğümler için diskleri ekli — ve bunu bir şablon için parametre olarak yapın. Ancak, bu yaklaşım bazı senaryolar için uygun değil.

İçinde [sanal makineler için Boyutlar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)farklı VM türler ve kullanılabilir boyutları tanımlanır ve her dayanıklı sayısı (2, 4, 8, 16 veya 32), diskleri eklenebilir. Her bağlı disk 500 IOPS sağlar ve bu disklerin'ün katları IOPS bu sayının bir çarpanı havuza alınmış. Örneğin, 16 disk 8.000 IOPS sağlamak için havuza. Havuzu içinde Linux Microsoft Windows depolama alanları veya pahalı olmayan (RAID) diskler yedek dizisi kullanarak işletim sistemini, yapılandırmada gerçekleştirilir.

Serbest biçimli yapılandırma seçimi çeşitli VM türleri ve bu örneklerde, çeşitli diskler VM türü için ve VM içeriği yapılandırmak için bir veya daha fazla komut dosyası için boyutları birkaç VM örnekleri sağlar.

Bu esneklik genellikle her düğüm türü için sağlanan şekilde bir dağıtım yöneticisi gibi düğümler ve veri düğümlerini birden çok tür olabilir yaygın bir durumdur.

Tüm anlamlı kümeler dağıtmak başlangıç olarak, bu karmaşık senaryolar ile çalışmaya başlayın. Hadoop kümesi dağıtma, örneğin, 8 ana düğüm ve 200 veri düğümleri ve ana her düğümde havuza alınmış 4 ekli disk ve havuza alınmış 16 ekli disk başına, veri düğüm ile 208 VM'ler ve 3,232 diskleri yönetmek için gerekir.

Bir depolama hesabı depolama hesabı bölümleme adresindeki arayın ve bu topoloji uyum sağlamak için depolama hesapları uygun sayısını belirlemek için hesaplamaları kullanmanız gerekir böylece tanımlanan 20.000 işlemleri/saniye, sınırlamak istek yukarıdaki kısıtlama. Serbest biçimli yaklaşım tarafından desteklenen birleşimleri sayıda verildiğinde, dinamik hesaplamalar uygun bölümleme belirlemek için gereklidir. Azure Resource Manager şablonu dili şu anda bu kodda, uygun bilgileri benzersiz, sabit kodlanmış bir şablonla oluşturma hesaplamalar gerekir böylece matematik işlevleri sağlamaz.

Kurumsal BT ve sı senaryoları, birisi şablonları korumak ve bir veya daha fazla kuruluşlar için dağıtılan topolojileri destekler. Bu ek yükü — farklı yapılandırmaları ve her bir müşteri için şablonlar — tercih gölgeden uzak olan.

Müşterinizin Azure aboneliği ortamlarda dağıtmak için bu şablonları kullanabilirsiniz, ancak kurumsal BT ekipleri ve CSV genellikle bunları kendi abonelikleri müşterilerine fatura için bir geri ödeme işlevini kullanarak dağıtabilirsiniz. Bu senaryolarda, bir abonelik havuzu birden çok müşteri için kapasite dağıtmak ve densely abonelik karmaşıklığını en aza indirmek için abonelikleri doldurulmuş dağıtımları tutmak için hedeftir — yönetmek için başka bir deyişle, daha fazla abonelik. Gerçekten dinamik dağıtım boyutlarıyla yoğunluğu bu tür elde dikkatli planlama ve ek geliştirme kuruluş adına askılama özelliğinin çalışması için gerektirir.

Ayrıca, bir API çağrısı aracılığıyla abonelikleri oluşturamazsınız ancak Portalı aracılığıyla el ile yapmanız gerekir. Abonelik sayısı arttıkça, sonuçta ortaya çıkan tüm abonelik karmaşıklığını insan etkileşimi gerektirir — otomatik olamaz. Dağıtımları boyutlarda çok değişkenlik ile el ile abonelikleri kullanılabilir olduğundan emin olmak için abonelik sayısı önceden sağlamak zorunda kalırsınız.

Bu etkenler göz önünde bulundurularak bir gerçekten serbest biçimli en önce blush daha az çekici bir yapılandırmadır.

### <a name="known-configurations--the-t-shirt-sizing-approach"></a>Yapılandırmaları bilinen — ısı boyutlandırma yaklaşımı
Bunun yerine toplam esneklik ve sayısız Çeşitlemeler sağlayan bir şablon teklif daha deneyimi bizim genel bir desen bilinen yapılandırmaları seçme özelliği sağlamaktır — yürürlükte, küçük, Orta ve büyük sanal gibi standart ısı boyutları. Diğer ısı boyutları community edition veya enterprise edition gibi ürün teklifleri gösterilebilir.  Diğer durumlarda, harita azaltmak gibi iş yüküne özgü yapılandırmaları bir teknolojisi – olabilir veya hiç sql.

Birçok kuruluş BT kuruluşları, OSS satıcılar ve SIS tekliflerini kullanılabilir duruma bugün bu şekilde şirket içi, sanallaştırılmış ortamlarda (kuruluşların) veya hizmet olarak yazılım (SaaS) tekliflerini (csv ve OSVs).

Bu yaklaşım, müşteri için yapılandırılmış farklı boyutlarda iyi, bilinen yapılandırmaları sağlar. Bilinen yapılandırmaları son müşterilere gerekir, kendi boyutlandırma küme belirlemek, platform kaynak kısıtlamaları faktörü ve sonuçta elde edilen bölümleme depolama hesapları ve diğer kaynakları (nedeniyle, küme boyutu ve kaynak tanımlamak için matematik yapın kısıtlamaları). Yapılandırmaları etkinleştirmek kolayca sağ ısı boyutu seçmek müşteriler bilinen — diğer bir deyişle, belirli bir dağıtım. Müşteri için daha iyi bir deneyim sağlamaya ek olarak, bilinen yapılandırmaları az sayıda kolay desteklemek ve yoğunluğu daha yüksek düzeyde sunmanıza yardımcı olabilir.

Isı boyutlarına odaklanmış bir bilinen yapılandırma yaklaşım değişen bir boyutu dahilindeki düğümlerin sayısını da sahip olabilirsiniz. Örneğin, bir küçük ısı boyutu 3 ve 10 düğümler arasında olabilir.  Isı boyutu en fazla 10 düğümleri uyum ve tüketici serbest biçimli seçimleri tanımlanan en büyük boyutu kadar yapma yeteneği sağlamak için tasarlanmış.  

İş yükü türüne göre bir ısı boyutu yapısı dağıtılabilir ancak düğüm üzerinde iş yükü ayrı düğüm boyutu ve yazılım yapılandırmasına sahip düğüm sayısı bakımından daha fazla serbest biçimli olabilir.

Topluluk veya kuruluş olabilir ayrı kaynak türleri ve dağıtılabilir, düğüm sayısı üst sınırı gibi ürün teklifleri üzerinde temel ısı boyutları arasında farklı teklifleri lisans değerlendirmeleri veya Özellik kullanılabilirliği genellikle bağlıdır.

JSON tabanlı şablonlar kullanarak benzersiz çeşitleri olan müşteriler de barındırabilir. Aykırı değerlerini ile ilgilenirken, uygun planlama ve geliştirme, destek ve maliyeti konuları dahil edebilirsiniz.

Müşteri şablon tüketim senaryoları ve bu belge listesinin başında tanımlanan gereksinimler temelinde, şablon ayrıştırma için bir örüntü belirledik.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Kapasite ve yetenek kapsamlı çözüm şablonları
Ayrıştırma şablon geliştirme modüler bir yaklaşım destekler yeniden olduğunu, test ve tooling genişletilebilirlik sağlar. Bu bölüm, nasıl bir ayrıştırma yaklaşım kapasite veya özelliği kapsam şablonlarıyla uygulanabilir hakkında ayrıntı sağlar.

Bu yaklaşım, ana şablon şablon tüketiciden parametre değerlerini alır ve ardından aşağıda gösterildiği gibi çeşitli şablonlar ve komut dosyaları için aşağı bağlar. Parametreler, statik değişkenler ve oluşturulan değişkenleri bağlı şablonları ve değerlerini sağlamak için kullanılır.

![Şablon parametreleri](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Bir ana şablona bağlı şablonları sonra çağrısına geçilen parametreler**

Aşağıdaki bölümlerde, şablonları ve tek bir şablon içine ayrılmış betikleri türlerini odaklanır. Bölümler durum bilgilerini şablonları arasında geçirme yaklaşımları sunar. Her bir şablon ve görüntünün betik türlerinde örnekler birlikte açıklanmaktadır. Bağlamsal bir örnek için bkz: "araya getirilmesi: örnek uygulama" belgesinde.

### <a name="template-metadata"></a>Şablon meta verileri
Şablon meta verilerine (metadata.json dosyası) insanlar ve yazılım sistemleri tarafından okunabilir JSON şablonunda tanımlayan anahtar/değer çiftleri içerir.

![Şablon meta verileri](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Şablon meta verilerine metadata.json dosyasında açıklanan**

Yazılım aracılarının metadata.json dosyasını alın ve bir web sayfası veya dizin şablonunda bilgileri ve bağlantı yayımlayabilirsiniz. Öğeleri dahil *ItemDisplayName*, *açıklama*, *Özet*, *githubUsername*, ve *dateUpdated*.

Bir örnek dosyası tamamının aşağıda gösterilmiştir.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. The database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Ana şablon
Ana Şablon parametreleri bir kullanıcıdan alır, karmaşık nesne değişkenleri doldurmak için bu bilgileri kullanır ve bağlı şablonları yürütür.

![Ana şablon](./media/best-practices-resource-manager-design-templates/main-template.png)

**Ana Şablon parametreleri bir kullanıcıdan alır.**

Sağlanan bir bilinen yapılandırma türü olarak da bilinen ısı boyutu parametresi nedeniyle standartlaştırılmış değerlerini gibi küçük, Orta veya büyük parametresidir. Uygulamada, bu parametre birden çok yolla kullanabilirsiniz. Ayrıntılar için bu belgenin sonraki bölümlerinde "bilinen yapılandırma kaynakları şablonu" konusuna bakın.

Bazı kaynaklar kullanıcı parametresi tarafından belirtilen yeni yapılandırma bağımsız olarak dağıtılır. Bu kaynakları tek paylaşılan kaynak şablonu kullanılarak sağlanır ve paylaşılan kaynak şablonu ilk çalıştırma için diğer şablonları tarafından paylaşılır.

Bazı kaynaklar isteğe bağlı olarak belirtilen bilinen yapılandırma bağımsız olarak dağıtılır.

### <a name="shared-resources-template"></a>Paylaşılan kaynaklar şablonu
Bu şablon tüm bilinen yapılandırmalar arasında ortak olan kaynaklar sunar. Sanal ağı, kullanılabilirlik kümeleri ve dağıtılan bilinen yapılandırma şablonu bağımsız olarak gerekli olan diğer kaynakları içerir.

![Şablon kaynakları](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Paylaşılan kaynaklar şablonu**

Sanal ağ adı gibi kaynak adları ana şablona dayalı olarak. Bunları, kuruluşunuzun gerektirdiği gibi kullanıcı bir parametre olarak almak veya bunları bu şablonu içindeki bir değişken olarak belirtin.

### <a name="optional-resources-template"></a>İsteğe bağlı kaynakları şablonu
İsteğe bağlı kaynakları şablonu bir parametre veya değişken değeri temel alınarak program aracılığıyla dağıtılan kaynaklar içeriyor.

![İsteğe bağlı kaynaklar](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**İsteğe bağlı kaynakları şablonu**

Örneğin, dağıtılmış bir ortama genel Internet'ten dolaylı erişim sağlayan bir jumpbox yapılandırmak için bir isteğe bağlı kaynakları şablonu kullanabilirsiniz. Jumpbox etkinleştirilmesi gerekip gerekmediğini belirlemek için bir parametre veya değişken kullanırsınız ve *concat* şablonu için bir hedef adı gibi yapı işlevi *jumpbox_enabled.json*. Şablon Bağlama elde edilen değişkeni jumpbox yüklemek için kullanırsınız.

Birden fazla yerde isteğe bağlı kaynakları şablondan bağlayabilirsiniz:

* Uygulanabilir olduğunda her dağıtım için bir parametre tabanlı bağlantı paylaşılan kaynakları şablonu oluşturun.
* Bilinen yapılandırmaları seçmek için uygulanabilir olduğunda — Örneğin, yalnızca büyük dağıtımlarında yükleyin — bilinen yapılandırma şablonu parametre tabanlı veya değişken güdümlü bir bağlantı oluşturmak.

Belirli bir kaynak isteğe bağlı olup şablon tüketici tarafından ancak bunun yerine şablon sağlayıcı tarafından yönlendirilen değil. Örneğin, ürün eklenti (ortak csv) veya belirli bir ürün gereksinimi karşılamak için veya ilkelerini zorlamak için gerekebilir (SIS ve kurumsal BT için yaygın grupları). Bu durumlarda, kaynak dağıtılmalıdır olup olmadığını belirlemek için bir değişkeni kullanabilirsiniz.

### <a name="known-configuration-resources-template"></a>Bilinen yapılandırma kaynakları şablonu
Ana şablonunda dağıtmak için istenen bilinen yapılandırmayı belirtmek şablon tüketici izin vermek için bir parametre gösterilebilir. Genellikle, bilinen bu yapılandırmayı sabit yapılandırma boyutları gibi korumalı alan, küçük, Orta ve büyük bir dizi ısı boyutu yaklaşımı kullanır.

![Yeni yapılandırma kaynakları](./media/best-practices-resource-manager-design-templates/known-config.png)

**Bilinen yapılandırma kaynakları şablonu**

Isı boyutu yaklaşım yaygın olarak kullanılır, ancak herhangi bir bilinen yapılandırmaları kümesi parametreleri temsil edebilir. Örneğin, ortam geliştirme, Test ve ürün gibi kurumsal uygulama için bir dizi belirtebilirsiniz. Veya, bir bulut hizmeti için farklı ölçek birimleri, ürün sürümleri veya topluluk, geliştirici veya Enterprise gibi ürün yapılandırmaları göstermek için kullanabilirsiniz.

Paylaşılan kaynak şablonuyla olduğu gibi değişkenleri için bilinen yapılandırmaları şablon herhangi birinden geçirilir:

* Bir son kullanıcı — diğer bir deyişle, ana şablon gönderilen parametreleri.
* Bir kuruluş — iç gereksinimleri ve ilkeleri temsil eden başka bir deyişle, ana şablondaki değişkenleri.

### <a name="member-resources-template"></a>Üye kaynakları şablonu
İçinde bilinen bir yapılandırma, bir veya daha fazla üye düğüm türleri genellikle dahil edilir. Örneğin, Hadoop ile ana düğüm ve veri düğümlerini vardır. MongoDB yüklüyorsanız, veri düğümlerini ve bir arbiter sahip. DataStax dağıtıyorsanız, veri düğümlerini ve yüklü OpsCenter'yle bir VM'yi sahip.

![Üye kaynakları](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Üye kaynakları şablonu**

Her düğüm türünü VM'ler, bağlı diskler, yükleme ve kurma düğümler, sanal makineleri, örneklerinin sayısını ve diğer ayrıntılar için bağlantı noktası yapılandırmaları için komut dosyaları sayıda farklı boyutlarda. Her düğüm türü kendi üye alır şekilde dağıtma ve bir altyapının yanı sıra VM dahilinde yazılım için yapılandırmak ve dağıtmak için komut dosyaları yürütme için ayrıntıları içeren kaynak şablonu.

VM'ler için genellikle iki komut dosyaları kullanılan, yaygın olarak yeniden kullanılabilir ve özel komut dosyaları türleridir.

### <a name="widely-reusable-scripts"></a>Yaygın olarak yeniden kullanılabilir komut dosyaları
Yaygın olarak yeniden kullanılabilir komut dosyaları birden çok şablon türlerini kullanılabilir. Bu yaygın bir şekilde yeniden kullanılabilir komut dosyaları daha iyi örneklerinden birini diskler havuz ve IOPS daha fazla sayıda kazanmak için Linux üzerinde RAID ayarlar. VM ile yüklenen yazılım bağımsız olarak, bu komut dosyası için genel senaryolar kanıtlanmış yöntemleri kullanılmasını sağlar.

![Yeniden kullanılabilir komut dosyaları](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Üye kaynakları şablonları yaygın olarak yeniden kullanılabilir komut dosyaları çağırabilirsiniz**

### <a name="custom-scripts"></a>Özel komut dosyaları
Şablonları sık vm'lerde yazılımı yükleme ve yapılandırma bir veya daha fazla komut dosyalarını arayın. Genel bir desen bir veya daha fazla üye türleri birden çok örneğini dağıtıldığı büyük topolojileri ile görülür. Paralel olarak çalışan her VM için bir yükleme komut dosyası başlatılan tüm sanal makineleri (veya belirtilen üye türündeki tüm VM'ler) dağıtıldıktan sonra çağrılan bir kurulum komut dosyası tarafından izlenen.

![Özel komut dosyaları](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Üye kaynakları şablonları VM yapılandırması gibi belirli bir amaç için komut dosyaları çağırın**

## <a name="capability-scoped-solution-template-example---redis"></a>Yetenek kapsamlı bir çözüm şablonu örnek - Redis
Bir uygulama nasıl çalışabilir göstermek için dağıtım ve standart ısı boyutlarda Redis yapılandırmasını kolaylaştıran bir şablonu oluşturmanın bir pratik örneğe bakalım.  

Dağıtım için bir dizi paylaşılan kaynakları (sanal ağ, depolama hesabı, kullanılabilirlik kümeleri) ve isteğe bağlı bir kaynak (jumpbox) vardır. Isı boyutları (küçük, Orta, büyük) temsil birden çok bilinen yapılandırmaları vardır, ancak her tek bir düğüm ile yazın. İki amaca özel komut dosyaları (yükleme, yapılandırma) vardır.

### <a name="creating-the-template-files"></a>Şablon dosyaları oluşturma
Azuredeploy.json adlı bir ana şablon oluşturursunuz.

Paylaşılan kaynaklar paylaşılan resources.json adlı şablonu oluşturma

Jumpbox_enabled.json adlı bir jumpbox dağıtımını etkinleştirmek için isteğe bağlı bir kaynak şablonu oluşturma

Üye kaynak düğüm resources.json adlı tek bir şablon oluşturmak için redis yalnızca bir tek düğümlü türü kullanır.

Redis ile tek tek her düğüme yükleyin ve ardından kümesi istiyor.  Yükleme uydurmak ve ayarlamak için komut dosyaları, redis küme install.sh ve redis küme setup.sh var.

### <a name="linking-the-templates"></a>Şablonları bağlama
Bağlama, sanal ağ oluşturur paylaşılan kaynakları şablona ana şablon bağlantıları şablonu kullanıyor.

Şablonun bir jumpbox dağıtmış olup olmadığını belirlemenizi sağlamak için ana şablonda mantığı eklenir. Bir *etkin* değerini *EnableJumpbox* parametresi gösterir müşteri bir jumpbox dağıtmak istiyor. Bu değer sağlandığında şablonunu art arda ekler *_enabled* jumpbox özelliği için bir temel şablonu adı için bir son eki olarak.

Ana şablonu uygulayan *büyük* parametre değeri ısı boyutları ve ardından kullanımlar için temel şablonu adı için bir son eki olarak şablon bağlantı değeri *technology_on_os_large.json*.

Bu çizim topoloji benzeyecektir.

![Şablon redis](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Bir Redis şablonu için şablon yapısı**

### <a name="configuring-state"></a>Yapılandırma durumu
Kümedeki düğümler, durumunu yapılandırmak için iki adımı vardır, her ikisi de amacı belirli komut dosyaları tarafından temsil edilen.  "redis-küme-install.sh" Redis yükler ve kümeyi "redis-küme-setup.sh" ayarlar.

### <a name="supporting-different-size-deployments"></a>Farklı boyutu dağıtımları destekleme
Değişkenleri ısı boyutu şablon için belirtilen boyut dağıtmak için her tür düğüm sayısını belirtir (*büyük*). Ardından bu kaynak döngüleri sayısal sıra numarasına sahip bir düğüm adı eklenerek kaynaklara benzersiz adlar sağlayarak, kullanarak VM örneklerinin sayısını dağıtır *copyındex ()*. Isı adı şablonunda tanımlandığı şekilde sıcak ve sıcak bölgesi VM'ler için şu adımları mu

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Ayrıştırma ve uçtan uca çözüm şablonları kapsamı
Uçtan uca çözüm kapsamlı bir çözüm şablonu bir uçtan uca çözüm sunduğunuzdan üzerinde odaklanmıştır.  Bu yaklaşım genellikle birden çok özellik kapsamlı Şablonları ek kaynaklar, mantığı ve durum ile oluşan bir bileşimdir.

Vurgulanmış olarak aşağıdaki görüntü, kapsamlı yetenek şablonları için kullanılan aynı modelin bir uçtan uca çözüm kapsamlı şablonları için genişletilir.

Bir paylaşılan kaynakları şablon ve isteğe bağlı kaynakları şablonları kapasite olduğu gibi aynı işlevi sunar ve yetenek şablon yaklaşımlar kapsamlı ancak uçtan uca çözüm için kapsamlı.

Uçtan uca çözüm kapsamlı olarak şablonları de genellikle ısı boyutları olabilir, belirli bir bilinen yapılandırma çözümü için gerekli olan yapılandırma kaynakları bilinen şablonu yansıtır.

Bir veya daha fazla yetenek bilinen yapılandırma kaynakları şablonu bağlantılar için uçtan uca çözüm ilgili çözüm şablonları yanı sıra uçtan uca çözüm için gerekli üye kaynak şablonları kapsamlı.

Çözümün ısı boyutunu tek tek özellik kapsamlı şablonu farklı olarak bilinen yapılandırma kaynakları şablonu değişkenler kapsamlı aşağı akış özelliği çözüm şablonları için uygun değerleri sağlamak için kullanılır uygun ısı boyutu dağıtın.

![Uçtan uca](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**Kapsamlı bir çözüm şablonları uçtan uca çözüm şablonu kapsamları için kolayca Genişletilebilir Kapasite veya özelliği için kullanılan modeli**

## <a name="preparing-templates-for-the-marketplace"></a>Market şablonları hazırlama
Önceki yaklaşım taşımalarına kuruluşlar, SIS ve CSV şablonlarını dağıtma veya müşterileri, kendi dağıtmak etkinleştirmek istediğiniz yere senaryoları düzenler.

Başka bir şablon Market üzerinden senaryoyu dağıtmaya istenen.  Bu ayrıştırma yaklaşım bazı küçük değişikliklerle Market de çalışır.

Daha önce belirtildiği gibi şablonları farklı dağıtım türleri Market satışı sunmak için kullanılabilir. Farklı dağıtım türleri ısı boyutları (küçük, Orta, büyük), ürün/izleyici türü (topluluk, geliştirici, Kurumsal) ya da özellik türünü (Basit, yüksek kullanılabilirlik) olabilir.

Aşağıda gösterildiği gibi var olan uçtan uca çözüm ya da kapsamlı yetenek şablonları kolayca Market'te farklı bilinen yapılandırmaları listelemek için kullanılabilir.

Ana Şablon parametreleri tshirtSize adlı gelen parametre kaldırmak için ilk olarak değiştirilmiştir.

Bilinen yapılandırma kaynakları şablona ayrı dağıtım türlerini eşleme olsa da, bunlar da ortak kaynakları ve paylaşılan kaynakları şablon ve potansiyel olarak isteğe bağlı kaynak şablonları de bulunan yapılandırma gerekir.

Şablonunuzu marketinde yayımlama istiyorsanız, şablonu içinde katıştırılmış bir değişkene tshirtSize önceden kullanılabilir gelen parametresinin yerini alır, ana şablonunuzu farklı kopyalarını oluşturun.

![Market](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Şablon Market için kapsamlı bir çözüm uyarlama**

## <a name="next-steps"></a>Sonraki adımlar
* İçine ve dışına şablonları durumu paylaşma hakkında bilgi edinmek için [Azure Resource Manager şablonları durumda paylaşımı](best-practices-resource-manager-state.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

