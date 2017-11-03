---
title: "Günlük analizi veri güvenliği | Microsoft Docs"
description: "Nasıl günlük analizi gizliliğinizi korur ve verilerinizin güvenliğini sağlama hakkında bilgi edinin."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 91af774560860b35913e57b49fb7a1dd59f5640f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-data-security"></a>Günlük analizi veri güvenliği
Microsoft gizliliğinizi korumayı taahhüt eder ve verilerinizin güvenliğini sağlama, yazılım ve hizmetlerinin, sunarken, kuruluşunuzun BT altyapısı yönetmenize yardımcı olur. Verilerinizi başkalarına güvenilen bu güven sıkı güvenlik gerektiren farkındayız. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

Güvenli hale getirme ve verileri koruma Microsoft'taki önceliğe sahiptir. Herhangi bir sorunuz, öneriler veya bizim güvenlik ilkeleri de dahil olmak üzere aşağıdaki bilgileri ile ilgili sorunlar ile başvuracaklarını [Azure destek seçenekleri](http://azure.microsoft.com/support/options/).

Bu makalede nasıl veri toplanan, işlenen ve günlük analizi Operations Management Suite (OMS) tarafından güvenli hale getirilmiş açıklanmaktadır. Aracılar, web hizmetine bağlanmak, işlemsel veri toplamak için System Center Operations Manager kullanın ya da günlük analizi tarafından kullanılmak üzere Azure Tanılama verileri almak için kullanabilirsiniz. Toplanan veriler, Microsoft Azure üzerinde barındırılan günlük analizi hizmeti için sertifika tabanlı kimlik doğrulaması ve SSL 3 kullanarak Internet üzerinden gönderilir. Gönderilmeden önce veri aracı tarafından sıkıştırılır.

Günlük analizi hizmeti aşağıdaki yöntemleri kullanarak bulut tabanlı verilerinizi güvenli bir şekilde yönetir:

* veriler arasında ayrım yapma
* veri saklama
* Fiziksel güvenlik
* Olay yönetimi
* Uyumluluk
* güvenlik standartları sertifikaları

## <a name="data-segregation"></a>veriler arasında ayrım yapma
Müşteri verileri OMS hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. Her bir müşteri uzun vadeli veri kaynakları barındıran ayrılmış bir Azure blob var.

## <a name="data-retention"></a>Veri saklama
Dizinli günlük arama veriler depolanır ve fiyatlandırma planınıza göre tutulur. Daha fazla bilgi için bkz: [günlük analizi fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft, 30 gün için OMS çalışma kapatıldıktan sonra müşteri verilerini siler. Microsoft Azure depolama hesabı verilerin bulunduğu da siler. Müşteri verileri kaldırıldığında, fiziksel sürücü yok yok.

Aşağıdaki tabloda bazı OMS ve bunlar toplamak örnekler veri türleri kullanılabilir çözümler listelenmektedir.

| **Çözüm** | **Veri türleri** |
| --- | --- |
| Yapılandırma Değerlendirmesi |Yapılandırma verileri, meta verileri ve durum verileri |
| Kapasite Planlaması |Performans verilerini ve meta veriler |
| Kötü Amaçlı Yazılımdan Koruma |Yapılandırma verileri, meta veriler |
| Sistem Güncelleştirme Değerlendirmesi |Meta verileri ve durum verileri |
| Günlük Yönetimi |Kullanıcı tanımlı olay günlükleri, Windows olay günlüklerini ve/veya IIS günlükleri |
| Değişiklik İzleme |Yazılım envanteri ve Windows hizmeti meta verileri |
| SQL ve Active Directory değerlendirme |WMI verilerini, kayıt defteri verilerini, performans verileri ve SQL Server dinamik yönetim sonuçları görüntüleyin |

Aşağıdaki tabloda veri türlerine örnek olarak gösterilmiştir:

| **Veri türü** | **Alanları** |
| --- | --- |
| Uyarı |Ad, uyarı açıklaması, Basemanagedentityıd, sorun kimliği, IsMonitorAlert, RuleId, çözünürlük durumu, önceliği, önem derecesi, kategori, sahibi, çözüm bulan, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount Uyarısı TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Yapılandırma |CustomerID, Agentıd, Entityıd, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Olay |Olay Kimliği, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, sayı, kategori, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Not:** Windows olay günlüğüne özel alanlara sahip olayları yazdığınızda, OMS bunları toplar. |
| Meta Veriler |Basemanagedentityıd, ObjectStatus, kuruluş birimi, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP adresi, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adres, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Performans |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, görüntülendiğinden, TimeSampled, TimeAdded |
| Durum |StateChangeEventId, stateId, NewHealthState, OldHealthState, bağlam, TimeGenerated, TimeAdded, StateId2, Basemanagedentityıd, Monitorıd, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fiziksel güvenlik
Günlük analizi OMS hizmetindeki Microsoft personeli tarafından manned ve tüm etkinlikler günlüğe kaydedilir ve denetlenebilir. Hizmet tamamen Azure üzerinde çalışır ve Azure ortak mühendislik ölçütler ile uyumludur. Sayfasında 18 Azure varlıklar fiziksel güvenliğinin ilgili ayrıntıları görüntüleyebilirsiniz [Microsoft Azure güvenlik genel bakış](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Alanları güvenliğini sağlamak için fiziksel erişim hakları artık aktarımı ve sonlandırma dahil olmak üzere OMS hizmetine sorumluluğu olan herkes için bir iş günü içinde değişir. Kullanırız adresindeki genel fiziksel altyapı hakkında bilgi edinebilirsiniz [Microsoft Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Olay yönetimi
OMS tüm Microsoft hizmetlerini uygun bir olay Yönetimi işlemini sahiptir. Özetlemek gerekirse, biz:

* Burada güvenlik sorumluluk bir kısmı Microsoft'a ait ve bir bölümü müşteriye ait bir paylaşılan sorumluluk modeli kullanır
* Azure güvenlik olayları yönetme
  * Bir olay algılandığında bir araştırma Başlat
  * Etki ve bir olay yanıtlama çağrısı ekip üyesi tarafından bir olayın önem değerlendirin. Üzerinde bulgu göre değerlendirme olabilir veya daha fazla güvenlik yanıtı takım yükselmesine neden olabilir değil.
  * Teknik veya adli araştırma yürütün, kapsama, azaltma ve çözüm stratejileri tanımlamak için güvenlik yanıtı uzmanları tarafından bir olay tanılayın. Güvenlik ekibi müşteri verilerini yasadışı veya yetkisiz bir kişi için açık hale inanırsa, müşteri olay bildirim işleminin Paralel yürütme paralel olarak başlar.  
  * Sabitle ve olaydan kurtarın. Olay yanıtlama ekibi sorunu azaltmak için bir kurtarma planı oluşturur. Etkilenen sistemleri karantinaya alma gibi kriz kapsama adımları hemen ve Tanılama ile paralel ortaya çıkabilir. Uzun vadeli Azaltıcı Etkenler hemen risk geçtikten sonra oluşan planlanan.  
  * Olayı kapatın ve bir Proje Sonrası-Değerlendirme yürütün. Olay yanıtlama takım bir proje sonrası-ilkeleri, yordamları ve olay yinelenmesini önlemek için işlemleri gözden geçirmek için amacınız ile olay ayrıntılarını özetleyen değerlendirme oluşturur.
* Güvenlik olayları müşterileri bildir
  * Etkilenen müşteriler ve mümkün olduğunca bildiren bir duyuru ayrıntılı olarak etkilenen herkes sağlamak için kapsam belirleme
  * Böylece kendi ucunda bir araştırma yapabileceğiniz ve değil unduly geciktirme bildirim işlemi sırasında son kullanıcılar yaptığınız taahhüt karşılayan müşterilerle ayrıntılı yeterli bilgi sağlamak için bir duyuru oluşturun.
  * Onayla ve gerektiği gibi olay bildirin.
  * Bir olay bildirimi aşırı gecikme olmadan ve yasal veya sözleşme taahhüt göre müşterilerle bildirin. Güvenlik olayları bildirimleri Microsoft seçer, e-posta aracılığıyla dahil olmak üzere herhangi bir yöntemle bir veya daha fazla müşteri'nin Yöneticiler teslim edilir.
* Takım hazırlık ve eğitim gerçekleştir
  * Microsoft personeli, güvenlik ve bunları belirlemek ve şüphe duyulan güvenlik sorunları bildirmek için yardımcı tanıma eğitim tamamlamak için gereklidir.  
  * Microsoft Azure hizmeti çalışma operatörlerinin erişimlerinin müşteri verilerini barındırma/küçük harfe duyarlı sistemleri çevreleyen toplama eğitim sorumlulukları vardır.
  * Microsoft Güvenlik Yanıt personeli kendi rolleri için özelleştirilmiş eğitim alma

Tüm Müşteri verilerinin kaybına ortaya çıkarsa, size bir gün içinde her bir müşteri bildirin. Ancak, müşteri veri kaybı ile OMS hiçbir zaman oluştu. Ayrıca, biz oluşturulduğu veri kopyalarını bulundurmak ve coğrafi olarak dağıtılmış.

Microsoft güvenlik olaylarına nasıl yanıt vereceğini hakkında daha fazla bilgi için bkz: [bulutta Microsoft Azure Security Response](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Uyumluluk
OMS yazılım geliştirme ve hizmet ekibin bilgi güvenliği ve idare program yasalarına ve düzenlemelerine konusunda açıklandığı gibi aynılarını kendi iş gereksinimlerini destekleyen ve [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [ Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Nasıl OMS güvenlik gereksinimlerini oluşturur, güvenlik denetimleri tanımlar, yönetir ve riskleri izleyen de açıklanmaktadır vardır. Yıllık, biz gözden geçirme ilkeler, standartlar, yordamlar ve yönergeleri.

Her OMS geliştirme ekibi üyesi resmi uygulama güvenlik eğitimi alır. Dahili olarak, sürüm denetimi sistemi yazılım geliştirme için kullanırız. Her yazılım projesi sürüm denetimi sistemi tarafından korunur.

Microsoft, denetlediği ve tüm Microsoft hizmetlerinde değerlendirir güvenlik ve uyumluluk bir ekip sahiptir. Bilgi güvenliği görevlileri ekip olun ve OMS geliştirmek mühendislik Departmanlar ile ilişkili değildir. Güvenlik görevlileri kendi yönetim zinciri varsa ve güvenlik ve uyumluluk sağlamak üzere ürünleri ve Hizmetleri, bağımsız değerlendirmeleri yürütün.

Microsoft'un Panosu Yönetim kurulu tüm bilgi güvenliği programlarını Microsoft'taki hakkında yıllık bir rapor olarak bildirilir.

OMS yazılım geliştirme ve hizmet ekibinin etkin olarak Microsoft Legal ve uyumluluk ekipleri ve çeşitli sertifikaları almak için diğer endüstri iş ortakları ile çalışmaktadır.

## <a name="certifications-and-attestations"></a>Sertifikaları ve attestations
OMS günlük analizi aşağıdaki gereksinimleri karşıladığından:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Ödeme Kartı endüstrisi (PCI uyumlu) veri güvenliği standardı (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) PCI güvenlik standartları Council tarafından.
* [Hizmet kuruluş denetimleri (SOC) 1 Type 1 ve SOC 2 tür 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) uyumlu
* [HIPAA ve HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) HIPAA iş ilişkilendirmek anlaşması olan şirketler için
* Windows ortak mühendislik ölçütler
* Microsoft Güvenilir Bilgi İşlem
* Bir Azure hizmeti OMS kullandığı bileşenlerinin Azure uyumluluk gereksinimlerine uyacak. Hakkında daha fazla bilgi edinebilirsiniz [Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> Bazı sertifikalar/attestations içinde günlük analizi eski adı altında listelenir *operasyonel Öngörüler*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Güvenlik veri akışı bulut
Aşağıdaki diyagramda bir bulut güvenlik mimarisini bilgi akışını şirketinizden gösterir ve sonuç olarak sizin tarafınızdan OMS Portalı'nda görülen günlük analizi hizmeti, olduğu gibi güvenliği nasıl sağlanır taşır. Her adım hakkında daha fazla bilgi diyagram izler.

![OMS veri toplama ve güvenlik görüntüsü](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Günlük analizi ve veri toplama için kaydolun
Kuruluşunuz için günlük analizi veri göndermek, Windows aracıları, Azure sanal makineleri veya OMS aracıları için Linux çalıştıran aracılar yapılandırın. Operations Manager aracıları kullanırsanız, daha sonra Yapılandırma Sihirbazı'nı işlemler konsolunda bunları yapılandırmak için kullanın. (Bu da, diğer tek tek kullanıcılara veya bir grup kişi olabilir) kullanıcılar, bir veya daha fazla OMS hesapları (OMS çalışma alanları) oluşturabilir ve aracıları şu hesaplardan birini kullanarak kaydedin:

* [Kuruluş Kimliği](../active-directory/sign-up-organization.md)
* [Microsoft hesabı - Outlook Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Burada veri, bir araya getirilir, analiz, sunulan ve toplanan bir OMS çalışma alanıdır. Bir çalışma alanı öncelikle bölüm veri için bir yol olarak kullanılır ve her çalışma benzersizdir. Örneğin, bir OMS çalışma alanıyla yönetilen üretim verilerinizi ve başka bir çalışma alanıyla yönetilen test verileri isteyebilirsiniz. Çalışma alanları, bir yönetici kullanıcı erişimini denetleme verileri de yardımcı. Her çalışma kendisiyle ilişkili birden çok kullanıcı hesabı olabilir ve her kullanıcı hesabı birden çok OMS çalışma erişebilir. Veri Merkezi bölgeye göre çalışma alanları oluşturun. Her çalışma alanı, diğer veri merkezlerine OMS hizmet kullanılabilirliği için öncelikle bölgede çoğaltılır.

Yapılandırma Sihirbazı tamamlandığında, Operations Manager için her bir Operations Manager yönetim grubunda günlük analizi hizmeti ile bağlantı kurar. Yönetim grubu hangi bilgisayarların hizmete veri göndermeye izin seçmek için bilgisayarları Ekleme Sihirbazı'nı kullanın. Diğer Aracısı türleri için her güvenli bir şekilde OMS hizmetine bağlanır.

Günlük analizi hizmeti ile bağlı sistemler arasındaki tüm iletişimi şifrelenir.  TLS (HTTPS) protokolü, şifreleme için kullanılır.  Günlük analizi şifreleme protokolleri en son gelişmeleri güncel olduğundan emin olmak için Microsoft SDL işlem izler.

Aracısı her tür için günlük analizi veri toplar. Toplanan veri türünde kullanılan çözümleri türlerine bağlıdır. Veri toplama sırasında özetini görebilirsiniz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Ayrıca, daha ayrıntılı bilgi çoğu çözümleri için kullanılabilir. Bir çözüm önceden tanımlanmış görünümleri, günlük arama sorguları, veri toplama kuralları ve işleme mantığı paketidir. Yalnızca Yöneticiler bir çözüm almak için günlük analizi kullanabilirsiniz. Çözüm aktarıldıktan sonra (kullanılıyorsa) Operations Manager yönetim sunucuları ve ardından seçmiş olduğunuz tüm aracıları taşınır. Daha sonra aracılar verilerini topla.

## <a name="2-send-data-from-agents"></a>2. Aracılardan verileri gönder
Tüm aracı türleri bir kayıt anahtarı ile kaydeder ve aracı ve sertifika tabanlı kimlik doğrulaması ve SSL bağlantı noktası 443 ile kullanarak günlük analizi hizmeti arasında güvenli bir bağlantı oluşturulur. OMS bir gizli deposu oluşturmak ve anahtarları korumak için kullanır. Özel anahtarları her 90 günde döndürülür ve Azure'da depolanır ve katı yasal düzenleme ve uyumluluk uygulamaları izleyen Azure işlemleri tarafından yönetilir.

Operations Manager ile günlük analizi hizmeti ile bir çalışma alanı kaydetmek ve Operations Manager yönetim sunucusu arasında güvenli bir HTTPS bağlantısı kurulur.

Azure sanal makinelerde çalışan Windows aracılar için salt okunur depolama anahtarı Azure tablolardaki Tanılama Olayları okumak için kullanılır.

Tüm aracı herhangi bir nedenle hizmetine iletişim kuramıyor ise, toplanan verileri yerel olarak geçici bir önbellekte depolanır ve yönetim sunucusu sekiz dakikada iki saat verileri yeniden dener. Aracının önbelleğe alınmış verileri işletim sisteminin kimlik bilgisi deposunu tarafından korunur. Hizmet veri iki saat sonra işleyemezse aracıları veri sırası. Sıra dolduğunda, veri türleri, performans verileri ile başlayan bırakarak OMS başlatır. Böylece, değiştirebilirsiniz Aracısı kuyruk sınırı bir kayıt defteri anahtarı gerekli ise. Toplanan veriler sıkıştırılır ve onu tüm yük onlara eklemez şekilde şirket içi veritabanlarını atlayarak hizmetine gönderilir. Toplanan veriler gönderildikten sonra önbellekten kaldırılır.

Yukarıda açıklandığı gibi aracılarınızı verileri Microsoft Azure veri merkezleri için SSL üzerinden gönderilir. İsteğe bağlı olarak, ek güvenlik için veri sağlamak için ExpressRoute kullanabilirsiniz. Çok protokollü etiket anahtarlama (MPLS) VPN, bir ağ hizmeti sağlayıcısı tarafından sağlanan gibi ExpressRoute doğrudan mevcut WAN ağınız, Azure'a bağlanmak için bir yoldur. Daha fazla bilgi için bkz: [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Günlük analizi hizmeti alır ve verileri işler
Günlük analizi hizmeti gelen verilerin güvenilir bir kaynaktan sertifikalar ve Azure kimlik doğrulaması ile veri bütünlüğünü doğrulayarak olmasını sağlar. İşlenmemiş ham verileri bir BLOB depolanır [Microsoft Azure depolama](../storage/common/storage-introduction.md) ve şifreli değil. Bununla birlikte, her bir Azure depolama blob yalnızca bu kullanıcı tarafından erişilebilir olan benzersiz birtakım anahtarlarını kümesi vardır. Depolanan verilerin türünü alınacak ve veri toplamak için kullanılan çözümlerinin türlerine bağlıdır. Ardından, günlük analizi hizmeti Azure storage blobu için ham verileri işler.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Veri erişimi için günlük analizi kullanın
Günlük analizi için OMS portalında kuruluş hesabı veya önceden ayarladığınız Microsoft hesabı kullanarak oturum açabilirsiniz. OMS portalı OMS günlük analizi arasındaki tüm trafiği güvenli bir HTTPS kanal üzerinden gönderilir. OMS Portalı'nı kullanarak, bir oturum kimliği Kullanıcı istemcide (web tarayıcısı) oluşturulur ve oturum sona kadar veri yerel önbellekte depolanır. Sona erdi, önbellek silinir. Kişisel bilgi içermez, istemci-tarafı tanımlama bilgilerini otomatik olarak kaldırılmaz. Oturum tanımlama bilgileri HTTPOnly işaretlenir ve güvenli hale getirilir. Önceden belirlenen boşta kalma süresi OMS portalı oturum sonlandırıldı.

OMS Portalı'nı kullanarak, verileri bir CSV dosyasına dışarı aktarabilir ve arama API'lerini kullanarak verilere erişebilirsiniz. CSV verme verme başına 50.000 satır sınırlıdır ve API veri arama başına 5.000 satır sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md) günlük analizi hakkında daha fazla bilgi ve dakika cinsinden başlamak ve çalıştırmak için.
