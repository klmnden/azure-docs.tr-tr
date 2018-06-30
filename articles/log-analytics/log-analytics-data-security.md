---
title: Günlük analizi veri güvenliği | Microsoft Docs
description: Nasıl günlük analizi gizliliğinizi korur ve verilerinizin güvenliğini sağlama hakkında bilgi edinin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 7596428b4ed067bf53f3b295a1682ed372f8d472
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131453"
---
# <a name="log-analytics-data-security"></a>Günlük analizi veri güvenliği
Bu belge hakkında bilgi tamamlamak için Azure günlük analizi belirli bilgiler sağlamak için yöneliktir [Azure Güven Merkezi](../security/security-microsoft-trust-center.md).  

Bu makalede nasıl veri toplanan, işlenen ve günlük analizi tarafından güvenli hale getirilmiş açıklanmaktadır. Aracılar, web hizmetine bağlanmak, işlemsel veri toplamak için System Center Operations Manager kullanın ya da günlük analizi tarafından kullanılmak üzere Azure Tanılama verileri almak için kullanabilirsiniz. 

Günlük analizi hizmeti aşağıdaki yöntemleri kullanarak bulut tabanlı verilerinizi güvenli bir şekilde yönetir:

* Veriler arasında ayrım yapma
* Veri saklama
* Fiziksel güvenlik
* Olay yönetimi
* Uyumluluk
* Güvenlik standartları sertifikaları

Herhangi bir sorunuz, öneriler veya bizim güvenlik ilkeleri de dahil olmak üzere aşağıdaki bilgileri ile ilgili sorunlar ile başvuracaklarını [Azure destek seçenekleri](http://azure.microsoft.com/support/options/).

## <a name="data-segregation"></a>Veriler arasında ayrım yapma
Verilerinizi günlük analizi hizmeti tarafından alınan sonra veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler çalışma etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır. Verilerinizi seçtiğiniz bölgede depolama kümesi adanmış bir veritabanında depolanır.

## <a name="data-retention"></a>Veri saklama
Dizinli günlük arama veriler depolanır ve fiyatlandırma planınıza göre tutulur. Daha fazla bilgi için bkz: [günlük analizi fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/).

Bir parçası olarak, [Abonelik Sözleşmesi](https://azure.microsoft.com/support/legal/subscription-agreement/), Microsoft sözleşmesini başına verilerinizi korur.  Veri silindiğinde, biz de verilerin bulunduğu Azure depolama hesabı silin.  Müşteri verileri kaldırıldığında, fiziksel sürücü yok yok.  

Aşağıdaki tabloda kullanılabilir çözümler bazıları listeler ve bunlar toplamak veri türü örnekleri sağlar.

| **Çözüm** | **Veri türleri** |
| --- | --- |
| Kapasite ve performans |Performans verilerini ve meta veriler |
| Kötü Amaçlı Yazılım Değerlendirmesi |Yapılandırma verileri, meta veriler |
| Güncelleştirme Yönetimi |Meta verileri ve durum verileri |
| Günlük Yönetimi |Kullanıcı tanımlı olay günlükleri, Windows olay günlüklerini ve/veya IIS günlükleri |
| Değişiklik İzleme |Yazılım envanteri, Windows hizmeti ve Linux daemon meta verileri ve Windows/Linux dosya meta verileri |
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
Günlük analizi hizmeti Microsoft personeli tarafından yönetilir ve tüm etkinlikler günlüğe kaydedilir ve denetlenebilir. Günlük analizi bir Azure hizmeti olarak çalıştırılır ve tüm Azure uyumluluk ve güvenlik gereksinimlerini karşılıyor. Sayfasında 18 Azure varlıklar fiziksel güvenliğinin ilgili ayrıntıları görüntüleyebilirsiniz [Microsoft Azure güvenlik genel bakış](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Alanları güvenliğini sağlamak için fiziksel erişim hakları artık aktarımı ve sonlandırma dahil olmak üzere OMS hizmetine sorumluluğu olan herkes için bir iş günü içinde değişir. Kullanırız adresindeki genel fiziksel altyapı hakkında bilgi edinebilirsiniz [Microsoft Datacenters](https://azure.microsoft.com/en-us/global-infrastructure/).

## <a name="incident-management"></a>Olay yönetimi
OMS tüm Microsoft hizmetlerini uygun bir olay Yönetimi işlemini sahiptir. Özetlemek gerekirse, biz:

* Burada güvenlik sorumluluk bir kısmı Microsoft'a ait ve bir bölümü müşteriye ait bir paylaşılan sorumluluk modeli kullanır
* Azure güvenlik olayları Yönet:
  * Bir olay algılandığında bir araştırma Başlat
  * Etki ve bir olay yanıtlama çağrısı ekip üyesi tarafından bir olayın önem değerlendirin. Üzerinde bulgu göre değerlendirme olabilir veya daha fazla güvenlik yanıtı takım yükselmesine neden olabilir değil.
  * Teknik veya adli araştırma yürütün, kapsama, azaltma ve çözüm stratejileri tanımlamak için güvenlik yanıtı uzmanları tarafından bir olay tanılayın. Güvenlik ekibi müşteri verilerini yasadışı veya yetkisiz bir kişi için açık hale inanırsa, müşteri olay bildirim işleminin Paralel yürütme paralel olarak başlar.  
  * Sabitle ve olaydan kurtarın. Olay yanıtlama ekibi sorunu azaltmak için bir kurtarma planı oluşturur. Etkilenen sistemleri karantinaya alma gibi kriz kapsama adımları hemen ve Tanılama ile paralel ortaya çıkabilir. Uzun vadeli Azaltıcı Etkenler hemen risk geçtikten sonra oluşan planlanan.  
  * Olayı kapatın ve bir Proje Sonrası-Değerlendirme yürütün. Olay yanıtlama takım bir proje sonrası-ilkeleri, yordamları ve olay yinelenmesini önlemek için işlemleri gözden geçirmek için amacınız ile olay ayrıntılarını özetleyen değerlendirme oluşturur.
* Güvenlik olayları müşterileri bildir:
  * Etkilenen müşteriler ve mümkün olduğunca bildiren bir duyuru ayrıntılı olarak etkilenen herkes sağlamak için kapsam belirleme
  * Böylece kendi ucunda bir araştırma yapabileceğiniz ve değil unduly geciktirme bildirim işlemi sırasında son kullanıcılar yaptığınız taahhüt karşılayan müşterilerle ayrıntılı yeterli bilgi sağlamak için bir duyuru oluşturun.
  * Onayla ve gerektiği gibi olay bildirin.
  * Bir olay bildirimi aşırı gecikme olmadan ve yasal veya sözleşme taahhüt göre müşterilerle bildirin. Güvenlik olayları bildirimleri Microsoft seçer, e-posta aracılığıyla dahil olmak üzere herhangi bir yöntemle bir veya daha fazla müşteri'nin Yöneticiler teslim edilir.
* Takım hazırlık ve eğitim gerçekleştir:
  * Microsoft personeli, güvenlik ve bunları belirlemek ve şüphe duyulan güvenlik sorunları bildirmek için yardımcı tanıma eğitim tamamlamak için gereklidir.  
  * Microsoft Azure hizmeti çalışma operatörlerinin erişimlerinin müşteri verilerini barındırma/küçük harfe duyarlı sistemleri çevreleyen toplama eğitim sorumlulukları vardır.
  * Microsoft Güvenlik Yanıt personeli kendi rolleri için özelleştirilmiş eğitim alma

Tüm Müşteri verilerinin kaybına ortaya çıkarsa, size bir gün içinde her bir müşteri bildirin. Ancak, müşteri veri kaybı hiçbir zaman hizmetiyle oluştu. 

Microsoft güvenlik olaylarına nasıl yanıt vereceğini hakkında daha fazla bilgi için bkz: [bulutta Microsoft Azure Security Response](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/4/Microsoft%20Azure%20Security%20Response%20in%20the%20cloud.pdf).

## <a name="compliance"></a>Uyumluluk
Günlük analizi yazılım geliştirme ve hizmet ekibin bilgi güvenliği ve idare program yasalarına ve düzenlemelerine konusunda açıklandığı gibi aynılarını kendi iş gereksinimlerini destekleyen ve [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) ve [ Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx). Nasıl günlük analizi güvenlik gereksinimlerini oluşturur, güvenlik denetimleri tanımlar, yönetir ve riskleri izleyen de açıklanmaktadır vardır. Yıllık, biz gözden geçirme ilkeler, standartlar, yordamlar ve yönergeleri.

Her geliştirme ekibi üyesi resmi uygulama güvenlik eğitimi alır. Dahili olarak, sürüm denetimi sistemi yazılım geliştirme için kullanırız. Her yazılım projesi sürüm denetimi sistemi tarafından korunur.

Microsoft, denetlediği ve tüm Microsoft hizmetlerinde değerlendirir güvenlik ve uyumluluk bir ekip sahiptir. Bilgi güvenliği görevlileri ekip olun ve günlük analizi geliştirir mühendislik Departmanlar ile ilişkili değildir. Güvenlik görevlileri kendi yönetim zinciri varsa ve güvenlik ve uyumluluk sağlamak üzere ürünleri ve Hizmetleri, bağımsız değerlendirmeleri yürütün.

Microsoft'un Panosu Yönetim kurulu tüm bilgi güvenliği programlarını Microsoft'taki hakkında yıllık bir rapor olarak bildirilir.

Günlük analizi yazılım geliştirme ve hizmet ekibinin etkin olarak Microsoft Legal ve uyumluluk ekipleri ve çeşitli sertifikaları almak için diğer endüstri iş ortakları ile çalışmaktadır.

## <a name="certifications-and-attestations"></a>Sertifikaları ve attestations
Azure günlük analizi aşağıdaki gereksinimleri karşıladığından:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/blog/iso22301/)
* [Ödeme Kartı endüstrisi (PCI uyumlu) veri güvenliği standardı (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) PCI güvenlik standartları Council tarafından.
* [Hizmet kuruluş denetimleri (SOC) 1 Type 1 ve SOC 2 tür 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) uyumlu
* [HIPAA ve HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa) HIPAA iş ilişkilendirmek anlaşması olan şirketler için
* Windows ortak mühendislik ölçütler
* Microsoft Güvenilir Bilgi İşlem
* Bir Azure hizmeti Azure uyumluluk gereksinimlerine yönelik günlük analizi kullandığı bileşenlerinin uyması. Hakkında daha fazla bilgi edinebilirsiniz [Microsoft Güven Merkezi Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx).

> [!NOTE]
> Bazı sertifikalar/attestations içinde günlük analizi eski adı altında listelenir *operasyonel Öngörüler*.
>
>

## <a name="cloud-computing-security-data-flow"></a>Güvenlik veri akışı bulut
Aşağıdaki diyagramda bir bulut güvenlik mimarisini bilgi akışını şirketinizden gösterir ve sonuç olarak sizin tarafınızdan Azure portal veya OMS Klasik portal görülen günlük analizi hizmeti, olduğu gibi güvenliği nasıl sağlanır taşır. Her adım hakkında daha fazla bilgi diyagram izler.

![Günlük analizi veri toplama ve güvenlik görüntüsü](./media/log-analytics-data-security/log-analytics-data-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Günlük analizi ve veri toplama için kaydolun
Kuruluşunuz için günlük analizi veri göndermek Azure sanal makineler veya sanal veya fiziksel bilgisayarlarda ortamınızda veya diğer bulut sağlayıcısı olarak çalışan bir Windows veya Linux Aracısı yapılandırın.  Operations Manager kullanırsanız yönetim grubundaki Operations Manager Aracısı yapılandırın. (Bu da, diğer tek tek kullanıcılara veya bir grup kişi olabilir) kullanıcılar, bir veya daha fazla günlük analizi çalışma alanı oluşturma ve aracıları şu hesaplardan birini kullanarak kaydedin:

* [Kuruluş Kimliği](../active-directory/fundamentals/sign-up-organization.md)
* [Microsoft hesabı - Outlook Office Live, MSN](https://account.microsoft.com/account)

Günlük analizi çalışma alanı burada veri, bir araya getirilir, analiz, sunulan ve toplanan ' dir. Bir çalışma alanı öncelikle bölüm veri için bir yol olarak kullanılır ve her çalışma benzersizdir. Örneğin, bir çalışma alanıyla yönetilen üretim verilerinizi ve başka bir çalışma alanıyla yönetilen test verileri isteyebilirsiniz. Çalışma alanları, bir yönetici kullanıcı erişimini denetleme verileri de yardımcı. Her çalışma kendisiyle ilişkili birden çok kullanıcı hesabı olabilir ve her kullanıcı hesabı birden çok günlük analizi çalışma erişebilir. Veri Merkezi bölgeye göre çalışma alanları oluşturun. Her çalışma diğer veri merkezlerine bölgede öncelikle günlük analizi hizmeti kullanılabilirlik için çoğaltılır.

Operations Manager için Operations Manager yönetim grubu günlük analizi hizmeti ile bağlantı kurar. Ardından, yönetim grubunda aracıyla yönetilen sistemleri toplamak ve hizmete veri göndermek için kullanılabilir yapılandırın. Etkinleştirdiğiniz çözümüne bağlı olarak, bu çözümlerin verilerden olan günlük analizi hizmetine veya aracıyla yönetilen sistem tarafından toplanan verilerin hacmi nedeniyle doğrudan bir Operations Manager yönetim sunucusundan gönderilen ya da doğrudan gönderilir Hizmet Aracısı. Operations Manager tarafından izlenen olmayan sistemler için her güvenli bir şekilde günlük analizi hizmetine doğrudan bağlanır.

Günlük analizi hizmeti ile bağlı sistemler arasındaki tüm iletişimi şifrelenir.  TLS (HTTPS) protokolü, şifreleme için kullanılır.  Günlük analizi şifreleme protokolleri en son gelişmeleri güncel olduğundan emin olmak için Microsoft SDL işlem izler.

Aracısı her tür için günlük analizi veri toplar. Toplanan veri türünde kullanılan çözümleri türlerine bağlıdır. Veri toplama sırasında özetini görebilirsiniz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Ayrıca, daha ayrıntılı bilgi çoğu çözümleri için kullanılabilir. Bir çözüm önceden tanımlanmış görünümleri, günlük arama sorguları, veri toplama kuralları ve işleme mantığı paketidir. Yalnızca Yöneticiler bir çözüm almak için günlük analizi kullanabilirsiniz. Çözüm aktarıldıktan sonra (kullanılıyorsa) Operations Manager yönetim sunucuları ve ardından seçmiş olduğunuz tüm aracıları taşınır. Daha sonra aracılar verilerini topla.

## <a name="2-send-data-from-agents"></a>2. Aracılardan verileri gönder
Tüm aracı türleri bir kayıt anahtarı ile kaydeder ve aracı ve sertifika tabanlı kimlik doğrulaması ve SSL bağlantı noktası 443 ile kullanarak günlük analizi hizmeti arasında güvenli bir bağlantı oluşturulur. Günlük analizi bir gizli deposu oluşturmak ve anahtarları korumak için kullanır. Özel anahtarları her 90 günde döndürülür ve Azure'da depolanır ve katı yasal düzenleme ve uyumluluk uygulamaları izleyen Azure işlemleri tarafından yönetilir.

Operations Manager ile bir Operations Manager yönetim sunucusu ile güvenli bir HTTPS bağlantısı günlük analizi çalışma alanı ile kayıtlı yönetim grubu oluşturur.

Azure sanal makinelerde çalışan Windows veya Linux aracıları için bir salt okunur depolama anahtarı Azure tablolardaki Tanılama Olayları okumak için kullanılır.  

Yönetim sunucusu ile iletişim kuramıyor ise günlük analizi ile tümleşik bir Operations Manager yönetim grubuna raporlama sahip tüm aracılara hizmet herhangi bir nedenle toplanan veriler için yerel olarak yönetim geçici bir önbellekte depolanır Sunucu.   Sekiz dakikada iki saat verileri yeniden göndermeyi deneyin.  Yönetim sunucusu atlar ve doğrudan günlük analizi için gönderilen veri davranışı Windows aracı ile tutarlıdır.  

Windows veya yönetim sunucusu Aracısı önbelleğe alınmış verileri işletim sisteminin kimlik bilgisi deposunu tarafından korunur. Hizmet veri iki saat sonra işleyemezse aracıları veri sırası. Sıra dolduğunda, veri türleri, performans verileri ile başlayan bırakarak Aracısı'nı başlatır. Böylece, değiştirebilirsiniz Aracısı kuyruk sınırı bir kayıt defteri anahtarı gerekli ise. Toplanan veriler sıkıştırılır ve onu tüm yük onlara eklemez şekilde Operations Manager yönetim grubu veritabanları atlayarak hizmetine gönderilir. Toplanan veriler gönderildikten sonra önbellekten kaldırılır.

Yukarıda açıklandığı gibi yönetim sunucusu veya aracılar doğrudan bağlı verileri Microsoft Azure veri merkezleri için SSL üzerinden gönderilir. İsteğe bağlı olarak, ek güvenlik için veri sağlamak için ExpressRoute kullanabilirsiniz. Çok protokollü etiket anahtarlama (MPLS) VPN, bir ağ hizmeti sağlayıcısı tarafından sağlanan gibi ExpressRoute doğrudan mevcut WAN ağınız, Azure'a bağlanmak için bir yoldur. Daha fazla bilgi için bkz: [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Günlük analizi hizmeti alır ve verileri işler
Günlük analizi hizmeti gelen verilerin güvenilir bir kaynaktan sertifikalar ve Azure kimlik doğrulaması ile veri bütünlüğünü doğrulayarak olmasını sağlar. İşlenmemiş ham verileri sonra verileri sonunda bekleyen depolanacak bölgede bir Azure Event Hub depolanır. Depolanan verilerin türünü alınacak ve veri toplamak için kullanılan çözümlerinin türlerine bağlıdır. Ardından, günlük analizi işlemlerini ham verileri hizmet ve veritabanına alır.

Toplanan verileri veritabanında depolanan saklama süresi seçilen fiyatlandırma plan üzerinde bağlıdır. İçin *serbest* katmanı, toplanan verileri kullanılabilir 7 gündür. İçin *Ödendi* katmanı, toplanan verileri varsayılan olarak 31 gün için kullanılabilir ancak 720 gün için genişletilebilir. Veri deposunda kalan veri gizliliği sağlamak için Azure, şifrelenmiş olarak depolanır. Verilerin son iki hafta da SSD tabanlı önbellekte depolanır ve bu önbelleği şu anda şifrelenmez.  2018 sonraki yarısında gibi şifreleme desteği planlıyoruz.  

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Veri erişimi için günlük analizi kullanın
Günlük analizi çalışma alanınız erişmek için Kurumsal hesap veya önceden ayarladığınız Microsoft hesabı kullanarak Azure portalında oturum açın. Günlük analizi hizmeti ve portalı arasındaki tüm trafiği güvenli bir HTTPS kanal üzerinden gönderilir. Portal kullanırken, bir oturum kimliği Kullanıcı istemcide (web tarayıcısı) oluşturulur ve oturum sona kadar veri yerel önbellekte depolanır. Sona erdi, önbellek silinir. Kişisel bilgi içermez, istemci-tarafı tanımlama bilgilerini otomatik olarak kaldırılmaz. Oturum tanımlama bilgileri HTTPOnly işaretlenir ve güvenli hale getirilir. Önceden belirlenen boşta kalma süresi Azure portalı oturum sonlandırıldı.

## <a name="next-steps"></a>Sonraki adımlar
* Azure sanal makinelerini aşağıdaki günlük analizi ile verileri toplamak öğrenin [Azure VM quickstart](log-analytics-quick-collect-azurevm.md).  

*  Fiziksel veya sanal Windows veya Linux bilgisayarlardan ortamınızdaki verileri toplamak için arıyorsanız, bkz: [Linux bilgisayarlar için Hızlı Başlangıç](log-analytics-quick-collect-linux-computer.md) veya [hızlı başlangıç Windows bilgisayarları için](log-analytics-quick-collect-windows-computer.md)

