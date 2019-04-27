---
title: Oturum Analytics veri güvenliği | Microsoft Docs
description: Nasıl Log Analytics, gizliliğinizi korur ve verilerinizin güvenliğini sağlar hakkında bilgi edinin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: magoedte
ms.openlocfilehash: dd4efcd2f1d4cbf497ad1fde6936088513cb5fd0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60759947"
---
# <a name="log-analytics-data-security"></a>Oturum Analytics veri güvenliği
Bu belge özelliği hakkında bilgiler tamamlamak için Azure İzleyici, Log Analytics, özel bilgiler sağlamak için tasarlanmıştır [Azure Güven Merkezi](../../security/security-microsoft-trust-center.md).  

Bu makalede nasıl veri toplanan, işlenen ve Log Analytics tarafından güvenliği sağlanan açıklanmaktadır. Web hizmetine bağlanmak, işletimsel veri toplamak için System Center Operations Manager'ı kullanın veya Log Analytics tarafından kullanılmak üzere Azure Tanılama verileri almak için aracıları'nı kullanabilirsiniz. 

Log Analytics hizmetine aşağıdaki yöntemleri kullanarak buluttaki verilerinizi güvenli bir şekilde yönetir:

* veriler arasında ayrım yapma
* Veri saklama
* Fiziksel güvenlik
* Olay yönetimi
* Uyumluluk
* güvenlik standartları sertifikaları

Tüm soruları, öneri veya bizim güvenlik ilkeleri de dahil olmak üzere aşağıdaki bilgileri ile ilgili sorunlar ile bizimle [Azure destek seçenekleri](https://azure.microsoft.com/support/options/).

## <a name="sending-data-securely-using-tls-12"></a>TLS 1.2 kullanarak güvenli bir şekilde veri gönderme 

Log analytics'e Aktarımdaki verilerin güvenliğini sağlamak üzere en az kullanmak üzere yapılandırmak için önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**, ve sektör hızla destek bırakmasını taşıma Bu eski protokolleri için. 

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) olarak ayarlanmış bir [son 30 Haziran 2018'ın](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) TLS/SSL ve yükseltme daha da protokolleri güvenli hale getirmek için eski sürümlerini devre dışı bırakmak için. Sonra aracılar üzerinde en az iletişim kuramıyorsa eski destek, Azure bırakır TLS 1.2 değil okunup verileri Log Analytics'e gönderebilirsiniz. 

Otomatik olarak algılamak ve olduklarında kullanılabilir, bu yeni daha güvenli protokolleri yararlanmasına olanak tanıyan platform düzeyi güvenlik özellikleri bozabilir olarak aracınızı sürece yalnızca TLS 1.2 kullanmak için açıkça kesinlikle gerekli ayarlanması önerilmez TLS 1.3. 

### <a name="platform-specific-guidance"></a>Platforma özgü yönergeleri

|Platform/dili | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
|Linux | Linux dağıtımları eğilimli etmenin [OpenSSL](https://www.openssl.org) TLS 1.2 desteği.  | Denetleme [OpenSSL Changelog](https://www.openssl.org/news/changelog.html) OpenSSL sürümünüz desteklenir onaylamak için.|
| Windows 8.0 10 | Desteklenen ve varsayılan olarak etkindir. | Yine de kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings).  |
| Windows Server 2012-2016 | Desteklenen ve varsayılan olarak etkindir. | Yine de kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 ve Windows Server 2008 R2 SP1 | , Varsayılan olarak etkin değildir ancak desteklenir. | Bkz: [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) nasıl etkinleştirileceği hakkında daha fazla ayrıntı için.  |

## <a name="data-segregation"></a>veriler arasında ayrım yapma
Log Analytics hizmeti tarafından alınan ve verilerinizi sonra veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler çalışma alanı etiketlendiğini. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. Verilerinizi, seçtiğiniz bölgede depolama kümesi adanmış bir veritabanında depolanır.

## <a name="data-retention"></a>Veri saklama
Dizinli günlük arama verilerin depolandığı ve fiyatlandırma planınıza göre saklanır. Daha fazla bilgi için [Log Analytics fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/).

Bir parçası olarak, [Abonelik Sözleşmesi](https://azure.microsoft.com/support/legal/subscription-agreement/), Microsoft, koşulları kabul ettiğiniz sözleşmenin verilerinizi korur.  Müşteri verilerini kaldırıldığında, fiziksel sürücü yok edilir.  

Aşağıdaki tabloda kullanılabilir çözümler bazılarını listeler ve bunların toplamak veri türü örnekler sağlar.

| **Çözüm** | **Veri türleri** |
| --- | --- |
| Kapasite ve performans |Performans verilerini ve meta verileri |
| Güncelleştirme Yönetimi |Meta verileri ve durum verileri |
| Günlük Yönetimi |Kullanıcı tanımlı olay günlükleri, Windows olay günlükleri ve/veya IIS günlükleri |
| Değişiklik İzleme |Yazılım envanteri, Windows hizmeti ve Linux daemon meta verileri ve Windows/Linux dosya meta verileri |
| SQL ve Active Directory değerlendirmesi |WMI verilerini, kayıt defteri verilerini, performans verilerini ve SQL Server dinamik yönetim sonuçlarını görüntüleme |

Aşağıdaki tabloda veri türleri gösterilmektedir:

| **Veri türü** | **Alanları** |
| --- | --- |
| Uyarı |Ad, uyarı açıklaması, Basemanagedentityıd, sorun kimliği, IsMonitorAlert, RuleId, ResolutionState, öncelik, önem derecesi, kategori, sahibi, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount Uyarısı TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Yapılandırma |CustomerID, Agentıd, Entityıd, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Olay |EventID, EventOriginalID, BaseManagedEntityInternalId, RuleId, Publisherıd, PublisherName, FullNumber, sayı, kategori, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Not:** Log Analytics, Windows olay günlüğüne olayları özel alanlara yazdığınızda, bunları toplar. |
| Meta Veriler |Basemanagedentityıd, ObjectStatus, kuruluş birimi, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPADDRESS, ForestDNSName, NetbiosComputerName, Sourceserver, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adres, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Performans |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, görüntülendiğinden, TimeSampled, TimeAdded |
| Durum |StateChangeEventId, stateId, NewHealthState, OldHealthState, bağlam, TimeGenerated, TimeAdded, StateId2, Basemanagedentityıd, Monitorıd, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fiziksel güvenlik
Log Analytics hizmeti, Microsoft personeli tarafından yönetilir ve tüm etkinlikleri günlüğe kaydedilir ve denetlenebilir. Log Analytics, bir Azure hizmeti olarak çalıştırılır ve tüm Azure uyumluluk ve güvenlik gereksinimlerini karşılıyor. Fiziksel varlıklarının güvenliği, Azure hakkındaki ayrıntıları 18 sayfasında görüntüleyebilirsiniz [Microsoft Azure güvenliğine genel bakış](https://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Alanları güvenliğini sağlamak için fiziksel erişim hakları, aktarımı ve sonlandırma dahil olmak üzere Log Analytics hizmeti için sorumluluk artık sahip olan herkes için bir iş günü içinde değiştirilir. Kullandığımız en genel fiziksel altyapı okuyabilirsiniz [Microsoft Datacenters](https://azure.microsoft.com/global-infrastructure/).

## <a name="incident-management"></a>Olay yönetimi
Log Analytics için tüm Microsoft hizmetlerini kullanan bir olay Yönetimi sürecinizi sahiptir. Özetlemek gerekirse, biz:

* Burada, güvenlik sorumluluk bir kısmı Microsoft'a ait ve bir bölümü müşteriye ait bir paylaşılan sorumluluk modeli kullanır
* Azure güvenlik olaylarını yönetme:
  * Bir olay algılanması üzerine bir araştırma Başlat
  * Bir nöbet olay yanıtı ekibi üyesi tarafından bir olayın önem derecesine ve etkisini değerlendirin. Kanıta göre değerlendirme olabilir veya daha fazla güvenlik yanıtı ekibi yükselmesine neden olabilir değil.
  * Teknik veya adli bir inceleme gerçekleştirme, kapsamı tanımlama, azaltma ve geçici çözüm stratejileri güvenlik yanıtı uzmanları tarafından yönetilen bir olay tanılayın. Güvenlik ekibi müşteri verileri için yasadışı veya yetkisiz bireysel kilometrelerce uzağa inanırsa, Paralel yürütme müşteri olay bildirim işlemi paralel olarak başlar.  
  * Sabitle ve olaydan kurtarın. Olay yanıtı ekibi, sorunu gidermek için bir kurtarma planı oluşturur. Etkilenen sistemleri karantinaya gibi kriz kapsama adımları hemen ve Tanılama ile paralel ortaya çıkabilir. Uzun vadeli azaltmaları hemen risk geçtikten sonra oluşan planlı.  
  * Olayı kapatmak ve bir son İnceleme gerçekleştirin. Olay yanıtı ekibi bir amaç, ilkeleri, yordamları ve bir yineleme olayın oluşmasını önlemek için işlemleri gözden geçirmek için olay ayrıntılarını özetleyen son İnceleme oluşturur.
* Güvenlik olaylarını müşterileri bilgilendirin:
  * Etkilenen müşteriler ve bir bildirim mümkün olduğunca ayrıntılı olarak etkilenen herkes sağlamak için kapsam belirleme
  * Böylece kendi ucunda bir araştırma yapabileceğiniz ve değil unduly geciktirme bildirim işlemi sırasında kendi son kullanıcıları için yaptığınız herhangi bir yükümlülüklerinizin yerine müşterilerle ayrıntılı yeterli bilgi sağlamak için bir bildirim oluşturun.
  * Doğrulayın ve gerektiği gibi bir olay bildirin.
  * Bir olay bildirimi mantıksız gecikme olmadan ve herhangi bir yasal veya sözleşmeye dayalı taahhüt uygun olarak müşterilerle bildirin. Güvenlik olaylarını bildirimleri bir veya daha fazla müşteri yöneticileri Microsoft seçer, e-posta aracılığıyla da dahil olmak üzere herhangi bir yöntemle gönderilir.
* Takım hazırlık ve alıştırma yapın:
  * Microsoft personeli, güvenlik ve bunları belirlemek ve güvenlik sorunları bildirmek için yardımcı olan tanıma eğitim tamamlamak için gerekli değildir.  
  * Microsoft Azure service üzerinde çalışan işleçlerin erişimleri müşteri verilerini barındıran hassas sistemlerine çevreleyen ayrıca eğitim yükümlülükler vardır.
  * Microsoft Güvenlik Yanıt personeli kendi rolleri için özelleştirilmiş eğitim alın

Herhangi bir müşteri veri kaybı meydana gelirse, biz bir gün içinde her müşteri bildirin. Ancak, müşteri veri kaybı hiçbir zaman hizmetiyle oluştu. 

Microsoft güvenlik olaylarına nasıl yanıt vereceğini hakkında daha fazla bilgi için bkz. [bulutta Microsoft Azure güvenlik yanıtı](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/4/Microsoft%20Azure%20Security%20Response%20in%20the%20cloud.pdf).

## <a name="compliance"></a>Uyumluluk
Log Analytics yazılım geliştirme ve hizmet takımın bilgileri güvenlik ve idare programı, iş gereksinimlerini destekler ve yasa ve düzenlemeler çerçevesinde için açıklandığı uyar [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [ Microsoft Güven Merkezi uyumluluğu](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx). Nasıl Log Analytics'e güvenlik gereksinimlerini oluşturur, güvenlik denetimleri tanımlar, yönetir ve riskleri izleyen de açıklanmaktadır vardır. Yıllık, biz gözden geçirme ilkeler, standartlar, yordamlar ve yönergeler.

Her geliştirme ekibi üyesi resmi uygulama güvenlik eğitim alır. Dahili olarak, bir sürüm denetim sistemi yazılım geliştirme için kullanırız. Her bir yazılım projesi, sürüm denetimi sistemi tarafından korunur.

Microsoft, Microsoft tüm hizmetleri değerlendirir ve denetleyen güvenlik ve uyumluluk bir ekibe sahiptir. Güvenlik müdürleri takım yapın ve Log Analytics geliştiren mühendislik ekipleri ile ilişkili değildir. Güvenlik sorumlularını, kendi yönetim zincir ve güvenlik ve uyumluluk sağlamak için ürün ve Hizmetleri, bağımsız değerlendirmelerini gerçekleştirmek.

Microsoft'un Kurulunun kurucu üyelerinden biriyim, yıllık bir rapor Microsoft'taki tüm bilgi güvenliği programlarını hakkında bildirimde bulunulur.

Log Analytics yazılım geliştirme ve hizmet takımının etkin bir şekilde Microsoft Legal ve uyumluluk ekipleri ve çeşitli sertifikaları almak için diğer endüstri ortakları ile çalışmaktadır.

## <a name="certifications-and-attestations"></a>Sertifikaları ve onayları
Azure Log Analytics, aşağıdaki gereksinimleri karşılar:

* [ISO/IEC 27001](https://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](https://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/blog/iso22301/)
* [Ödeme Kartı Industry (PCI uyumlu) veri güvenliği standardı (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) PCI güvenlik standartları Konseyi tarafından.
* [Hizmet kuruluşu denetimleri (SOC) 1 türü 1 ve SOC 2 tip 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) uyumlu
* [HIPAA ve HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa) bir HIPAA iş ilişkilendirme sözleşmesine sahip şirketler için
* Windows ortak mühendislik ölçütleri
* Microsoft Güvenilir Bilgi İşlem
* Bir Azure hizmeti Azure uyumluluk gereksinimleri için Log Analytics kullandığı bileşenlerinin uyar. Daha fazla bilgi edinebilirsiniz [Microsoft Güven Merkezi uyumluluğu](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx).

> [!NOTE]
> Log Analytics altında önceki adı bazı sertifikalar/karşıladığımızı içinde listelenen *operasyonel İçgörüler*.
>
>

## <a name="cloud-computing-security-data-flow"></a>Bulut bilgi işlem güvenlik veri akışı
Aşağıdaki diyagramda bir bulut güvenlik mimarisi, şirket tarafından bilgi akışını gösterir ve sonuç olarak sizin tarafınızdan Azure Portalı'nda görülen Log Analytics hizmeti olarak nasıl güvenli olduğundan taşır. Diyagramda her bir adım hakkında daha fazla bilgi aşağıdadır.

![Log Analytics veri toplama ve güvenlik görüntüsü](./media/data-security/log-analytics-data-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Log Analytics ve veri toplamak için kaydolun
Kuruluşunuzun verilerini Log Analytics'e göndermek, ortamınızda veya diğer bulut sağlayıcılarından sanal veya fiziksel bilgisayarlar veya Azure sanal makinelerinde çalışan bir Windows veya Linux Aracısı yapılandırın.  Operations Manager'ı kullanırsanız, yönetim grubundan Operations Manager aracısını yapılandırın. (Bu, diğer bireysel kullanıcılar veya bir grup insan olabilir) kullanıcılar, bir veya daha fazla Log Analytics çalışma alanı oluşturun ve aracıları şu hesaplardan birini kullanarak kaydedin:

* [Kuruluş Kimliği](../../active-directory/fundamentals/sign-up-organization.md)
* [Microsoft hesabı - Outlook, Office Live, MSN](https://account.microsoft.com/account)

Burada veri, toplu, analiz, sunulan ve toplanan bir Log Analytics çalışma alanıdır. Bir çalışma alanı öncelikle verileri bölümlemek için bir yol olarak kullanılır ve her bir çalışma alanı benzersizdir. Örneğin, bir çalışma alanıyla yönetilen üretim verilerinizi ve yönetilen başka bir çalışma alanı ile test verilerini isteyebilirsiniz. Çalışma alanları, verilere bir yönetici kullanıcı erişimini denetlemek de yardımcı olur. Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok kullanıcı hesabı içerebilir ve her kullanıcı hesabı, birden fazla Log Analytics çalışma alanına erişebilir. Veri Merkezi bölgeyi temel alan çalışma alanları oluşturun.

Operations Manager için Operations Manager yönetim grubu Log Analytics hizmetiyle bir bağlantı kurar. Ardından, toplamak ve hizmete veri göndermek için hangi yönetim grubunda aracıyla yönetilen sistemi izin yapılandırın. Etkinleştirdiğiniz çözümüne bağlı olarak, bu çözümleri verilerden olan Log Analytics hizmetine veya aracıyla yönetilen sistemi tarafından toplanan verilerin hacmi nedeniyle doğrudan bir Operations Manager yönetim sunucusundan onlara gönderilen ya da doğrudan gönderilir Hizmet Aracısı. Operations Manager tarafından izlenecek olmayan sistemler için her güvenli bir şekilde Log Analytics hizmetine doğrudan bağlanır.

Log Analytics hizmetine bağlı sistemler arasındaki tüm iletişimler şifrelenir. TLS (HTTPS) protokolü, şifreleme için kullanılır.  Log Analytics ile şifreleme protokollerine en son gelişmelerden en güncel olduğundan emin olmak için Microsoft SDL işlemine izler.

Aracısı'nın her tür için Log Analytics verilerini toplar. Toplanan veri türünde kullanılan çözümleri türlerine bağlıdır. Veri toplamayı durdurmak özetini görebilirsiniz [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](../../azure-monitor/insights/solutions.md). Ayrıca, daha ayrıntılı bilgilerin toplanması için çözümlerinin çoğunda kullanılabilir. Bir çözüm önceden tanımlanmış görünümleri, günlük arama sorguları, veri toplama kuralları ve işleme mantığı paketidir. Yalnızca Yöneticiler, Log Analytics çözümü içeri aktarma için kullanabilirsiniz. Çözüm içeri aktardıktan sonra (kullanılıyorsa) Operations Manager yönetim sunucularına ve seçmiş olduğunuz tüm aracılara taşınır. Daha sonra aracıları, verilerini toplayın.

## <a name="2-send-data-from-agents"></a>2. Aracılarından veri gönderme
Bir kayıt anahtarı ile tüm aracı türleri Kaydet ve aracı ve sertifika tabanlı kimlik doğrulaması ve SSL bağlantı noktası 443 ile kullanarak Log Analytics hizmeti arasında güvenli bir bağlantı kurulur. Log Analytics, oluşturmak ve anahtarlarını korumak için bir gizli dizi deposu kullanır. Özel anahtarlar Azure'da depolanır her 90 günde döndürülür ve katı yasal ve uyumluluğa yöntemler izleyen Azure işlemleri tarafından yönetilen.

Operations Manager ile Log Analytics çalışma alanıyla kayıtlı yönetim grubunun Operations Manager yönetim sunucusu ile güvenli bir HTTPS bağlantısı kurar.

Azure sanal makineler üzerinde çalışan Windows veya Linux aracıları için Azure tabloları Tanılama Olayları okumak için bir salt okunur depolama anahtarı kullanılır.  

Yönetim sunucusu ile iletişim kuramıyor, Log Analytics ile tümleşik bir Operations Manager yönetim grubuna raporlama sahip tüm aracılara hizmeti herhangi bir nedenle toplanan verileri yerel olarak yönetim geçici bir önbellekte depolanır Sunucu.   İki saat boyunca sekiz dakikada verileri yeniden göndermeyi deneyin.  Yönetim sunucusu atlar ve doğrudan Log Analytics'e gönderilen veri davranışı Windows Aracısı ile tutarlıdır.  

Yönetim sunucusu Aracısı önbelleğe alınmış verileri ve Windows işletim sisteminin kimlik bilgisi deposu tarafından korunur. Hizmet, iki saat sonra veriyi işleyemiyor, aracıların verileri kuyruğa ekler. Sıranın tam hale gelirse, aracıyı performans verileriyle başlangıç veri türleri, bırakarak başlatır. Değiştirebilmesi aracı kuyruğu bir kayıt defteri anahtarı gerekirse sınırdır. Toplanan verileri sıkıştırılır ve Operations Manager yönetim grubu veritabanları için her türlü yük için eklemez atlayarak hizmetine gönderilir. Toplanan verileri gönderildikten sonra önbellekten kaldırılır.

Yukarıda açıklandığı gibi yönetim sunucusu veya aracılar doğrudan bağlı verileri Microsoft Azure veri merkezleri için SSL üzerinden gönderilir. İsteğe bağlı olarak, veriler için ek güvenlik sağlamak için Expressroute'u kullanabilir. Çok protokollü etiket anahtarlaması (MPLS) VPN gibi ağ hizmeti sağlayıcısı tarafından sağlanan gibi ExpressRoute doğrudan mevcut WAN ağınız, Azure'a bağlanmak için bir yoldur. Daha fazla bilgi için [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Log Analytics hizmetine alır ve verileri işler
Log Analytics hizmeti, sertifikalar ve Azure kimlik doğrulaması ile veri bütünlüğünü doğrulayarak gelen verileri güvenilir bir kaynaktan olmasını sağlar. İşlenmemiş ham veriler, ardından veriler bekleme durumundayken sonunda depolanacak bölgede bir Azure olay Hub'ındaki depolanır. Depolanan verilerin türünü içe ve veri toplamak için kullanılan çözümleri türlerine bağlıdır. Ardından, Log Analytics işlemlerini ham veriler hizmet ve veritabanına alır.

Seçilen fiyatlandırma planı hakkında toplanan verileri veritabanında depolanan saklama süresi bağlıdır. İçin *ücretsiz* katmanı, toplanan veriler kullanılabilir yedi gündür. İçin *Ücretli* katmanı, toplanan verileri varsayılan olarak 31 gün için kullanılabilir, ancak ila 730 gün genişletilebilir. Verileri veri gizliliği emin olmak için Azure depolama, bekleme sırasında şifrelenmiş olarak depolanır ve veriler yerel olarak yedekli depolama (LRS) kullanarak yerel bölge içinde çoğaltılır. Son iki haftalık veri da SSD tabanlı önbellekte depolanır ve bu önbellek şifrelenir.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Verilere erişmek için log Analytics'i kullanma
Log Analytics çalışma alanınızın erişmek için bir kuruluş hesabı ya da daha önce ayarlamış bir Microsoft hesabı kullanarak Azure portalında oturum açın. Log Analytics hizmeti ve portalı arasındaki tüm trafiğe güvenli bir HTTPS kanalı üzerinden gönderilir. Portal kullanırken bir oturum kimliği kullanıcı istemci (tarayıcı) oluşturulur ve veriler, oturum sonlandırılana kadar yerel önbellekte depolanır. Sona erdi, önbellek silinir. Kişisel bilgi içermeyen, istemci tarafı tanımlama bilgilerini otomatik olarak kaldırılmaz. Oturum tanımlama bilgileri HTTPOnly işaretlenir ve güvenli hale getirilir. Önceden belirlenmiş bir boşta kalma süresinden sonra Azure portalı oturum sonlandırıldı.

## <a name="next-steps"></a>Sonraki adımlar
* Uygulamanızın Azure sanal makinelerini aşağıdaki için Log Analytics verilerini nasıl toplayacağınızı öğrenin [Azure VM Hızlı Başlangıç](../../azure-monitor/learn/quick-collect-azurevm.md).  

*  Fiziksel veya sanal Windows veya Linux bilgisayarlardaki ortamınızdaki verileri toplamak için arıyorsanız bkz [Linux bilgisayarlar için Hızlı Başlangıç](../../azure-monitor/learn/quick-collect-linux-computer.md) veya [Hızlı Başlangıç için Windows bilgisayarları](../../azure-monitor/learn/quick-collect-windows-computer.md)

