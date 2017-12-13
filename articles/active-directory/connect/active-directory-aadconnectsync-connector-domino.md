---
title: "Lotus Domino Bağlayıcısı | Microsoft Docs"
description: "Bu makalede, Microsoft'un Lotus Domino bağlayıcısının nasıl yapılandırılacağı açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: mtillman
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/119/2017
ms.author: barclayn
ms.openlocfilehash: 80151134821c6106382c58bf0ec68ea0f6d4646a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino Bağlayıcısı Teknik Başvurusu
Bu makalede Lotus Domino Bağlayıcısı açıklanmaktadır. Makale aşağıdaki ürünler için geçerlidir:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Düzeltme 4.1.3671.0 kullanmanız gerekir ya da daha sonra [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ve FIM2010R2 için bağlayıcı yükleme yoluyla kullanılabilir [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Lotus Domino Bağlayıcısı genel bakış
Lotus Domino Bağlayıcısı, eşitleme hizmeti IBM'in Lotus Domino sunucusuyla tümleştirmenize olanak sağlar.

Üst düzey açısından bakıldığında, aşağıdaki özellikler bağlayıcı bir geçerli sürümü tarafından desteklenir:

| Özellik | Destek |
| --- | --- |
| Bağlı veri kaynağı |Sunucu: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>İstemci:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Senaryolar |<li>Nesne yaşam döngüsü yönetimi</li><li>Grup Yönetimi</li><li>Parola Yönetimi</li> |
| İşlemler |<li>Tam ve Delta içeri aktarma</li><li>Dışarı Aktarma</li><li>Ayarlayın ve HTTP parola parolasını değiştirme</li> |
| Şema |<li>Kişi (gezici kullanıcı, kişi (kişiler sertifika ile))</li><li>Grup</li><li>Kaynak (kaynak, yer, çevrimiçi toplantı)</li><li>Posta veritabanı</li><li>Desteklenen nesnelerinin öznitelikleri dinamik bulma</li><li>Bir kuruluş & kuruluş birimlerine (OU) ile en fazla 250 özel certifiers desteği</li> |

Lotus Domino Bağlayıcısı'nı Lotus Notes istemci Lotus Domino sunucusu ile iletişim kurmak için kullanır. Bu bağımlılık gruplarındaki sonucu olarak, desteklenen Lotus Notes istemcisi eşitleme sunucusuna yüklenmesi gerekir. İstemci ve sunucu arasındaki iletişimin Lotus Notes .NET birlikte çalışabilirliği (Interop.domino.dll) arabirimi aracılığıyla uygulanır. Bu arabirim Microsoft.NET platform ve Lotus Notes istemci arasındaki iletişimi kolaylaştırır ve Lotus Domino belgeler ve görünümler erişimi destekler. Delta içeri aktarma için da C++ yerel arabirimi (Seçilen delta alma yöntemine bağlı olarak) kullanılır mümkündür.

### <a name="prerequisites"></a>Ön koşullar
Bağlayıcısı'nı kullanmadan önce eşitleme sunucusunda aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* 4.5.2 Microsoft .NET Framework veya daha yenisi
* Lotus Notes istemci eşitleme sunucunuzda yüklenmelidir
* Lotus Domino Bağlayıcısı Domino dizin sunucusunda bulunması için varsayılan Lotus Domino LDAP şema veritabanı (schema.nsf) gerektirir. Mevcut değilse, çalışan veya Domino sunucusunda LDAP hizmetini yeniden yükleyebilirsiniz.

### <a name="connected-data-source-permissions"></a>Bağlı veri kaynağı izinleri
Lotus Domino Bağlayıcısı desteklenen görevleri gerçekleştirmek için aşağıdaki grupların bir üyesi olması gerekir:

* Tam erişim yöneticileri
* Yöneticiler
* Veritabanı Yöneticileri

Aşağıdaki tabloda, her işlem için gerekli izinler listelenmektedir:

| İşlem | Erişim hakları |
| --- | --- |
| İçeri Aktarma |<li>Ortak belgeler okuma</li><li> Tam Erişim Yöneticisi (tam erişim Yöneticiler grubunun üyesi olduğunda otomatik olarak etkin ACL'de erişiminiz.)</li> |
| Dışarı aktarma ve parola ayarlama |Etkili erişim: <li>Belgeleri oluşturma</li><li>Belgeleri silme</li><li>Ortak belgeler okuma</li><li>Ortak belgeler yazma</li><li>Replicate veya Kopyala belgeler</li>Dışarı aktarma işlemleri için aşağıdaki rolleri de gerekir: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Doğrudan işlemler ve AdminP
İşlem ya da doğrudan Domino dizine veya AdminP işlemiyle gidin. Aşağıdaki tablolarda desteklenen liste tüm nesneleri, operations ve uygulanabilirse, ilgili uygulama yöntemi:

**Birincil adres defteri**

| Nesne | Oluştur | Güncelleştirme | Sil |
| --- | --- | --- | --- |
| Kişi |AdminP |Doğrudan |AdminP |
| Grup |AdminP |Doğrudan |AdminP |
| MailInDB |Doğrudan |Doğrudan |Doğrudan |
| Kaynak |AdminP |Doğrudan |AdminP |

**İkincil adres defteri**

| Nesne | Oluştur | Güncelleştirme | Sil |
| --- | --- | --- | --- |
| Kişi |Yok |Doğrudan |Doğrudan |
| Grup |Doğrudan |Doğrudan |Doğrudan |
| MailInDB |Doğrudan |Doğrudan |Doğrudan |
| Kaynak |Yok |Yok |Yok |

Bir kaynak oluşturulduğunda, notlar belge oluşturulur. Benzer şekilde, bir kaynak silindiğinde, notlar belge silinir.

### <a name="ports-and-protocols"></a>Bağlantı noktalarını ve protokolleri
IBM Lotus Notes istemcisi ve Domino sunucuları notları uzak yordam çağrısı (NRPC TCP/IP'yi burada kullanması gereken NRPC) kullanarak iletişim kurar. Varsayılan bağlantı noktası numarası 1352 olmakla birlikte, Domino Yöneticisi tarafından değiştirilebilir.

### <a name="not-supported"></a>Desteklenmiyor
Aşağıdaki işlemleri Lotus Domino Bağlayıcısı'nı bir geçerli sürümü tarafından desteklenmez:

* Posta kutusu sunucuları arasında taşıyın.

## <a name="create-a-new-connector"></a>Yeni bir bağlayıcı oluşturun
### <a name="client-software-installation-and-configuration"></a>İstemci yazılımı yükleme ve yapılandırma
Lotus Notes sunucusunda yüklü olması **önce** Bağlayıcısı yüklenir.

Yükleme sırasında yaptığınızdan emin olun bir **tek kullanıcılı yükleme**. Varsayılan **çok kullanıcı yükleme** çalışmıyor.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Yalnızca gerekli Lotus Notes özellikleri özellikleri sayfasında, yüklemek ve **istemci tek oturum açma**. Çoklu oturum açma Domino sunucuya oturum açabilmesi bağlayıcı için gereklidir.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Not:** başlangıç Lotus Notes bağlayıcı kullandığınız hesabı ile aynı sunucuda bulunan bir kullanıcı ile bir kez hizmet hesabı. Ayrıca sunucu Lotus Notes istemcide kapatmak emin olun. Bağlayıcı Domino sunucuya bağlanmaya aynı anda çalıştırılması olamaz.

### <a name="create-connector"></a>Bağlayıcı oluşturma
Lotus Domino bağlayıcısı oluşturmak için **eşitleme hizmeti** seçin **yönetim Aracısı** ve **oluşturma**. Seçin **Lotus Domino (Microsoft)** bağlayıcı.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Eşitleme hizmeti sürümünüz yapılandırma yeteneğini sağlayıp **mimarisi**, Bağlayıcısı'nı çalıştırmak için varsayılan değer olarak ayarlandığından emin olun **işlem**.

### <a name="connectivity"></a>Bağlantı
Bağlantı sayfasında Lotus Domino sunucu adını belirtmek ve oturum açma kimlik bilgilerini girmeniz gerekir.  
![Bağlantı](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Domino sunucu özelliği sunucu adı için iki biçim destekler:

* serverName
* ServerName/DirectoryName

**ServerName/DirectoryName** biçimi olduğundan bu öznitelik için tercih edilen biçim bağlayıcı Domino sunucusu kurduğunda daha hızlı yanıt sağlar.

Sağlanan UserID dosya eşitleme hizmeti yapılandırma veritabanında depolanır.

İçin **Delta içeri aktarma** Bu seçenekler vardır:

* **None**. Bağlayıcı hiçbir delta içeri aktarmalar yapmaz.
* **Ekleme/güncelleştirme**. Bağlayıcı mu delta içeri aktarma ekleme ve güncelleştirme işlemlerinde. Silme için bir **tam içeri aktarma** işlemi gereklidir. Bu işlem, .net birlikte çalışabilirliği kullanıyor.
* **Ekleme/güncelleştirme/silme**. Bağlayıcı mu delta içeri aktarma ekleme, güncelleştirme ve silme işlemleri. Bu işlem, yerel C++ arabirimleri kullanıyor.

İçinde **şema seçenekleri** aşağıdaki seçeneklere sahip olursunuz:

* **Varsayılan şema**. Bağlayıcı Domino sunucusu şemadan algılar. Bu seçim varsayılan seçenektir.
* **DSML şema**. Domino sunucu şema imkanı sunmuyorsa yalnızca kullanılır. Ardından şemasıyla DSML dosyası oluşturabilir ve bunun yerine alabilirsiniz. DSML hakkında daha fazla bilgi için bkz: [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

İleri'yi tıklattığınızda, kullanıcı kimliği ve parola yapılandırma parametrelerini doğrulanır.

### <a name="global-parameters"></a>Genel Parametreler
Genel parametreleri sayfasında, saat dilimi ve içeri aktarma yapılandırın ve işlemi seçeneği verin.  
![Genel Parametreler](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Domino sunucu saat dilimi** parametre Domino sunucunuzun konumunu tanımlar.

Bu yapılandırma seçeneği desteklemek için gereken **delta içeri aktarma** işlemleri eşitleme hizmeti, sağladığından son iki içeri aktarmalar arasındaki değişiklikleri belirleme.

>[!Note]
Genel parametreleri ekran Mart 2017 güncelleştirmede başlangıç kullanıcının silinirken kullanıcının posta veritabanını silmek için seçenek içerir.

![Kullanıcının posta Sil](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>İçeri aktarma ayarları, yöntemi
**Tam içeri aktarma gerçekleştirmek tarafından aktar** Bu seçenekler vardır:

* Arama
* Görünüm (önerilir)

**Arama** olan Domino ancak bu dizin kullanarak dizinleri gerçek zamanlı olarak güncelleştirilmez ve sunucudan döndürülen veriler her zaman doğru değil. Birden çok değişikliği bir sistem, bu seçenek genellikle iyi çalışmaz ve bazı durumlarda yanlış siler sağlar. Ancak, **arama** hızlıdır **Görünüm**.

**Görünüm** çünkü veri doğru durumunu sağlar önerilen seçenektir. Arama biraz daha yavaş çalışıyor.

#### <a name="creation-of-virtual-contact-objects"></a>Sanal bağlantı nesnelerini oluşturma
**Etkinleştirmek oluşturulmasını \_kişi nesnesi** Bu seçenekler vardır:

* None
* Başvuru olmayan değerleri
* Başvuru ve başvuru olmayan değerleri

Domino içinde başka nesneler başvurmak için birçok farklı biçimlerde başvuru öznitelikleri içerebilir. Farklı Çeşitlemeler, bağlayıcı Implements temsil etmek için \_nesneleri, kişi olarak da bilinen **sanal kişiler** (VC). Bu nesneler varolan MV nesnelere katılmanız için oluşturulan veya yeni nesneler olarak öngörülen. Bu şekilde, öznitelik başvuruları korunabilir.

Bu ayarı etkinleştirerek ve bir başvuru özniteliği içeriğini DN biçiminde değilse, bir \_ilgili kişi nesnesi oluşturulur. Örneğin, bir grubun üyesi özniteliği SMTP adresi içerebilir. Kısaad ve diğer öznitelikleri başvurusu öznitelikleri mevcut olması mümkündür. Bu senaryo için seçin **olmayan başvuru değerlerini**. Bu yapılandırma, Domino uygulamaları için en yaygın ayardır.

Lotus Domino ayrı adres defterleri aynı nesneyi temsil eden farklı ayırt edilen adlara sahip olması için yapılandırıldığında, bu da oluşturmak mümkündür \_nesneleri için bir adres defteri içinde bulunan tüm başvuru değerleri başvurun. Bu senaryo için seçin **başvurusu ve başvuru olmayan değerleri** seçeneği.

Özniteliği birden çok değer varsa **FullName** Domino içinde sonra da başvuruları çözümlenebilir şekilde sanal kişiler oluşturma etkinleştirmek istediğiniz. Örneğin, bu öznitelik bir marriage veya divorce sonra birden çok değer olabilir. Onay kutusunu işaretleyin **etkinleştir... FullName sahip birden çok değer** bu senaryo için.

Doğru özniteliklerinde katılarak \_kişi nesneleri MV nesne katılması.

Bu nesneler VC sahiptir =\_ilgili kişi kendi DN eklediniz.

#### <a name="import-settings-conflict-object"></a>Ayarları içeri aktarmak, nesne çakışıyor
**Çakışma nesne Dışla**

Büyük bir Domino uygulamasında birden fazla nesne çoğaltma sorunları nedeniyle aynı DN olması mümkündür. Bu durumlarda, iki farklı UniversalIDs ancak aynı DN nesneleriyle bağlayıcı görür. Bu çakışma bağlayıcı alanı oluşturulan geçici bir nesne neden olur. Bağlayıcı Domino içinde çoğaltma kurbana seçilen nesneleri yoksayabilirsiniz. Bu onay kutusu seçili tutmanız önerilir.

#### <a name="export-settings"></a>Dışarı aktarma ayarları
Varsa seçeneği **AdminP başvurularını güncelleştirme için kullanmak** üyesi gibi başvuru öznitelikleri verilmesini doğrudan çağrısı ve AdminP işlemi kullanmıyor seçildiyse. Yalnızca AdminP tutarlılığını korumak için yapılandırılmadı olduğunda bu seçeneği kullanın.

#### <a name="routing-information"></a>Yönlendirme bilgileri
Domino içinde bir başvuru özniteliği DN için soneki olarak ekli yönlendirme bilgileri olduğunu mümkündür. Örneğin, bir grup üyesi özniteliğinde içerebilir **CN =example/organization@ABC**. Sonek @ABC yönlendirme bilgileri. Yönlendirme bilgilerini Domino tarafından farklı bir kuruluşta bir sistem olabilir doğru Domino Sistem e-posta göndermek için kullanılır. Yönlendirme bilgileri alanında bağlayıcının kapsamında kuruluş içinde kullanılan yönlendirme sonekleri belirtebilirsiniz. Bir başvuru özniteliği şu değerlerden biri sonek olarak bulunursa, yönlendirme bilgilerini başvurusundan kaldırılır. Bir başvuru değeri yönlendirme ekini belirtildiyse, bu değerlerden birine eşleştirilemiyor varsa bir \_ilgili kişi nesnesi oluşturulur. Bu \_kişi nesneleri ile oluşturulan **RO = @<RoutingSuffix>**  DN eklenir. Bu \_kişi nesneleri gerçek bir nesneye gerekiyorsa birleştirmek izin vermek için de aşağıdaki öznitelikler eklenir: \_routingName, \_contactName, \_displayName ve UniversalID.

#### <a name="additional-address-books"></a>Ek adres defterleri
Sahip değilse **directory Yardım** yüklü, ikincil adres defterleri adını sağlayan ve ardından bu adres defterleri el ile girebilirsiniz.

#### <a name="multivalued-transformation"></a>Birden çok değerli dönüştürme
Lotus Domino birçok öznitelikte birden çok değerli. Karşılık gelen meta veri deposu öznitelikleri genellikle tek değerli. İçeri ve dışarı aktarma işlemi seçeneği yapılandırarak, etkilenen öznitelikleri gerekli çeviri ile yardımcı olmak bağlayıcı etkinleştirin.

**Dışarı aktarma**  
Dışa aktarma işlemi seçeneği iki modlarını destekler:

* Öğe Ekle
* Öğeyi değiştirin

**Maddesini** – bu seçeneği işaretlediğinizde Bağlayıcısı'nı her zaman içinde Domino öznitelik geçerli değerlerini kaldırın ve sağlanan değerlerle değiştirin. Sağlanan değerli tek değerli veya birden çok değerli olabilir.

Örnek: Bir kişi nesnesinin Yardımcısı özniteliği aşağıdaki değerlerden sahiptir:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Adlı yeni bir yardımcı varsa **David Alexander** atanan bu kişinin nesneye oluşur:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Öğe ekleme** – bu seçeneği işaretlediğinizde bağlayıcı Domino ve INSERT yeni değerleri veri listesi üst öznitelikte üzerindeki var olan değerleri korur.

Örnek: Bir kişi nesnesinin Yardımcısı özniteliği aşağıdaki değerlerden sahiptir:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Adlı yeni bir yardımcı varsa **David Alexander** atanan bu kişinin nesneye oluşur:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**İçeri Aktarma**  
Alma işlemi seçeneği iki modlarını destekler:

* Varsayılan
* Tek değer için birden çok değerli

**Varsayılan** –, tüm değerlerin tüm öznitelikleri alma işlemi varsayılan seçeneği seçin.

**Tek değer için birden çok değerli** – bu seçeneği işaretlediğinizde birden çok değerli özniteliği tek değerli bir özniteliğe dönüştürülür. Birden fazla değer varsa, (Bu değer genellikle de en son sayısıdır) üstteki değeri kullanılır.

Örnek: Bir kişi nesnesinin Yardımcısı özniteliği aşağıdaki değerlerden sahiptir:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Bu öznitelik için en son güncelleştirme **David Alexander**. Alma işlemi seçeneği Multivalued tek değere ayarlandığından Bağlayıcısı yalnızca alır **David Alexander** bağlayıcı alanına.

Birden çok değerli öznitelikler tek değerli öznitelikler dönüştürmek için mantığı grup üyesi özniteliği ve kişi fullname özniteliği için geçerli değildir.

Bu içeri aktarma yapılandırmak ve dönüştürme kuralları, öznitelik başına birden çok değerli öznitelikler için bir özel durum Genel kural olarak dışarı aktarmak da mümkündür. Bu seçeneği yapılandırmak için [objecttype] girin. [attributename] içinde **dışlama öznitelik listesi alma** ve **dışlama öznitelik listesi verme** metin kutuları. Örneğin, Person.Assistant girin ve tüm değerleri almak için genel bayrağını ayarlayın, yalnızca ilk değer Yardımcısı için içeri aktarılır.

#### <a name="certifiers"></a>Certifiers
Tüm kuruluşun/kuruluş birimlerini bağlayıcı tarafından listelenir. Birincil adres defterine kişi nesneleri dışarı aktarmak için kendi parola ile certifier gereklidir.

Tüm certifiers aynı parolayı varsa **tüm Certifers parolasını** kullanılabilir. Ardından buraya parola girin ve yalnızca certifier dosyası belirtin.

Ardından yalnızca içe aktarırsanız, tüm certifiers belirtmeniz gerekmez.

### <a name="configure-provisioning-hierarchy"></a>Sağlama hiyerarşisini Yapılandır
Lotus Domino Bağlayıcısı'nı yapılandırırken bu iletişim sayfasını atlayın. Lotus Domino Bağlayıcısı'nı sağlama hiyerarşisini desteklemez.  
![Hiyerarşi sağlama](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Bölümleri ve hiyerarşileri Yapılandır
Bölümleri ve hiyerarşileri Yapılandır NAB=names.nsf adlı birincil adres defteri seçmeniz gerekir. Varsa, birincil adres defteri ek olarak ikincil adres defterleri seçebilirsiniz.  
![Bölümler](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Öznitelikleri Seç
Öznitelikler yapılandırdığınızda ile önek tüm öznitelikleri seçmelisiniz  **\_MMS\_**. Lotus Domino yeni nesnelere sağlarken bu öznitelikler gereklidir

![Öznitelikler](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Nesne yaşam döngüsü yönetimi
Bu bölümde Domino farklı nesneleri genel bir bakış sağlar.

### <a name="person-objects"></a>Kişi nesneleri
Kişi nesnesi, kuruluş ve kuruluş birimleri kullanıcıları temsil eder. Varsayılan öznitelikleri yanı sıra Domino yönetici bir kişinin nesnesine özel öznitelikler ekleyebilirsiniz. En az bir kişi nesnesi tüm zorunlu öznitelikler eklemeniz gerekir. Zorunlu öznitelikler tam bir listesi için bkz: [Lotus Notes özellikleri](#lotus-notes-properties). Bir kişi nesnesi kaydetmek için aşağıdaki gereksinimleri karşılamanız gerekir:

* Adres Defteri (names.nsf) tanımlanmalıdır ve birincil adres defteri olması gerekir.
* Kuruluşunuzda belirli bir kullanıcı kaydı için O/OU certifier kimliği ve parola olmalıdır / kuruluş birimi.
* Lotus Notes özellikleri bir kişi nesnesi için belirli bir dizi ayarlamanız gerekir. Bu özellikler, kişi nesnesi sağlamak için kullanılır. Daha fazla bilgi için adlı bölüme bakın [Lotus Notes özellikleri](#lotus-notes-properties) belgesinde.
* İlk HTTP bir kişi için bir öznitelik ve küme hazırlama sırasında paroladır.
* Kişi nesnesi aşağıdaki üç desteklenen türlerden biri olmalıdır:
  1. Posta dosyası ve bir kullanıcı kimliği dosyası olan normal kullanıcı
  2. Gezici kullanıcı (Normal gezici tüm veritabanı dosyaları içeren kullanıcı)
  3. Kişiler (herhangi bir kimliği dosyası olan kullanıcı)

Kişiler (dışında kişiler) daha fazla gruplandırılabilir BİZE ve uluslararası kullanıcılar değeri tarafından tanımlandığı şekilde \_MMS\_IDRegType özelliği. Bu kişi kullanarak Lotus Domino sunuculara erişmek için Notlar istemci notları kimliği ve bir kişi belge vardır. Notlar posta kullanıyorsanız, sonra da posta dosyası sahiptirler. Kullanıcı etkin hale için kayıtlı olması gerekir. Daha fazla bilgi için bkz.

* [Notlar kullanıcıları ayarlama](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Kullanıcı kaydı](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Kullanıcıları yönetme](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Kullanıcıların yeniden adlandırma](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Bu işlemler Lotus Domino gerçekleştirilen ve eşitleme hizmetinde içeri aktarıldı.

### <a name="resources-and-rooms"></a>Kaynakları ve odaları
Lotus Domino veritabanında başka türde bir kaynaktır. Kaynakları konferans odaları ekipmanının projektörler gibi çeşitli türleriyle olabilir. Kaynak türü özniteliği tarafından tanımlanan Lotus Domino Bağlayıcısı tarafından desteklenen kaynakların alt türleri şunlardır:

| Kaynak türü | Kaynak türü özniteliği |
| --- | --- |
| Yer |1 |
| Kaynak (diğer) |2 |
| Çevrimiçi toplantı |3 |

Kaynak nesne türü çalışması aşağıdakiler gereklidir:

* Kaynak ayırma veritabanını bağlı Domino sunucuda zaten var olmalıdır
* Site kaynak için zaten tanımlandı

Kaynak ayırma veritabanı belgeleri üç tür içerir:

* Site profili
* Kaynak
* Rezervasyon

Kaynak ayırma veritabanı ayarlama ile ilgili daha fazla bilgi için bkz: [kaynak ayırmaları veritabanı kurma](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**, Güncelleştirme, oluşturma ve kaynakları silme**  
Oluşturma, güncelleştirme ve silme işlemleri, kaynak ayırma veritabanında Lotus Domino Bağlayıcısı tarafından gerçekleştirilir. Kaynaklar, belge Names.nsf (diğer bir deyişle, birincil adres defteri) olarak oluşturulur. Düzenleme ve kaynakları silme hakkında daha fazla bilgi için bkz: [düzenleme ve kaynak belgeleri silme](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**İçeri ve dışarı aktarma işlemi kaynaklar için**  
Kaynaklar için içe aktarılan ve gibi herhangi bir nesne türü eşitleme hizmeti dışarı. Nesne türü yapılandırması sırasında kaynak olarak seçin. Başarılı dışarı aktarma işlemine ilişkin ayrıntılar için kaynak türü, konferans veritabanı ve Site adı olmalıdır.

### <a name="mail-in-databases"></a>Posta veritabanları
Bir posta veritabanı postalar almak için tasarlanmış bir veritabanıdır. Tüm özel Lotus Domino kullanıcı hesabıyla ilişkilendirilmemiş bir Lotus Domino posta kutusudur (diğer bir deyişle, kendi kimlik dosyasını ve parolası yok). Bir posta veritabanı ve kendi e-posta adresi ile ilişkili benzersiz bir kimliği ("kısa adı") sahiptir.

Farklı bir posta kutusu farklı kullanıcılar arasında paylaşılabilen kendi e-posta adresi olan bir gereksinimi varsa (örneğin, group@contoso.com), bir posta veritabanı oluşturulur. Bu posta kutusu erişimi, erişim denetim listesi (posta açmasına izin notları kullanıcılarının adlarını içeren ACL aracılığıyla), denetlenir.

Başlıklı bölümde gerekli öznitelikler listesi için bkz: [zorunlu öznitelikler](#mandatory-attributes) bu makalenin ilerisinde yer.

Bir veritabanı posta almak için tasarlanmış bir posta veritabanı belge Lotus Domino oluşturulur. Bu belge, veritabanının bir kopyasını depolar her sunucunun Domino dizininde olması gerekir. Veritabanı posta belgesi oluşturma hakkında daha ayrıntılı bir açıklaması için bkz: [bir posta veritabanı belge oluşturma](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Bir posta veritabanı oluşturmadan önce veritabanı zaten var olmalıdır (Lotus yönetici tarafından oluşturulmuş olmalıdır) Domino sunucusunda.

### <a name="group-management"></a>Grup Yönetimi
Aşağıdaki kaynaklardan Lotus Domino Grup Yönetimi ayrıntılı bir genel bakış elde edebilirsiniz:

* [Grupları kullanma](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Grup oluşturma](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Oluşturma ve grupları değiştirme](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Grupları yönetme](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Bir grubu yeniden adlandırma](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Parola Yönetimi
Kayıtlı bir Lotus Domino kullanıcı için parola iki tür vardır:

1. Kullanıcı parolası (User.ID dosyasında depolanan)
2. Internet / HTTP parola

Lotus Domino Bağlayıcısı, yalnızca HTTP parolayla işlemleri destekler.

Parola yönetimi gerçekleştirmek için yönetim Aracısı Tasarımcısı'nda bağlayıcı için parola yönetimini etkinleştirmeniz gerekir. Parola yönetimini etkinleştirmek için seçin **parola yönetimini etkinleştirin** üzerinde **uzantıları Yapılandır** iletişim sayfası.  
![Uzantıları Yapılandır](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Internet parola işlemleri aşağıdaki Lotus Domino Bağlayıcısı desteği:

* Parola ayarlama: Parola ayarlama Domino kullanıcı yeni bir HTTP/Internet parola koyar. Varsayılan olarak aynı zamanda kilidi hesabıdır. Unlock bayrağı eşitleme altyapısı WMI arabirimde açıktır.
* Parolayı Değiştir: Bu senaryoda, bir kullanıcı parolasını değiştirmek isteyebilirsiniz veya belirtilen bir süre sonra parolayı değiştirmesi istenir. Bu işlem yapılacak yerleştirin, hem (eski ve yeni parolayı) zorunludur. Değiştirilen sonra yeni parolayı Lotus Domino güncelleştirilir.

Daha fazla bilgi için bkz.

* [Internet kilitleme özelliğini kullanma](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Internet parolaları yönetme](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Başvuru bilgileri
Bu bölümde, öznitelik tanımlarını ve Lotus Domino Bağlayıcısı için öznitelik gereksinimleri gibi listelenir.

### <a name="lotus-notes-properties"></a>Lotus Notes özellikleri
Lotus Domino dizininize kişi nesneleri sağladığınızda nesnelerinizi belirli değerler ile doldurulan belirli bir özellikler kümesi olmalıdır. Bu değerleri yalnızca için gereken işlemleri oluşturun.

Aşağıdaki tabloda bu özellikleri listeler ve bunları bir açıklamasını sağlar.

| Özellik | Açıklama |
| --- | --- |
| \_MMS_AltFullName |Kullanıcı alternatif tam adı. |
| \_MMS_AltFullNameLanguage |Kullanıcının diğer tam adını belirtmek için kullanılacak dili. |
| \_MMS_CertDaysToExpire |Geçerli tarihten önce sertifika gün sayısı sona erer. Belirtilmezse, varsayılan iki yıl geçerli tarihten itibaren tarihidir. |
| \_MMS_Certifier |Certifier kuruluş hiyerarşisi adını içeren özellik. Örneğin: OU = OrganizationUnit, O = Org, C = ülke =. |
| \_MMS_IDPath |Özelliği boşsa, herhangi bir kullanıcı kimliği dosyası eşitleme sunucusunda yerel olarak oluşturulur. Özellik bir dosya adı içeriyorsa, bir kullanıcı kimliği dosyası madata klasöründe oluşturulur. Özelliği, tam bir yol da içerebilir. |
| \_MMS_IDRegType |Kişi, kişiler, BİZE kullanıcılar ve uluslararası kullanıcılar sınıflandırılabilir. Olası değerler aşağıdaki tabloda listelenmektedir: <li>0 - başvurun</li><li>1 - ABD kullanıcı</li><li>2 - uluslararası kullanıcı</li> |
| \_MMS_IDStoreType |ABD ve uluslararası kullanıcılar için gerekli özellik. Özelliği kullanıcı kimliği iliştirerek notları adres defteri veya kullanıcının posta dosyasında depolanan olup olmadığını belirten bir tamsayı değeri içerir. Kullanıcı Kimliği dosya eki adres defteri içinde ise, bu isteğe bağlı olarak bir dosya olarak oluşturulabilir \_MMS_IDPath. <li>Boş - depolama kimliği dosya kimliği kasadaki (kişiler için kullanılır) tanımlama dosyası yok.</li><li> 1 - notları adres defteri eki. \_MMS_Password özelliği, ekleri olan kullanıcı kimliği dosyaları için ayarlanmış olması gerekir</li><li>2 - kimliği kişinin posta dosyasında depolar. \_MMS_UseAdminP kişi kaydı sırasında oluşturulan posta dosyasını izin vermek için false olarak ayarlanmalıdır. \_MMS_Password özelliği, kullanıcı kimliği dosyaları için ayarlanmış olması gerekir.</li> |
| \_MMS_MailQuotaSizeLimit |E-posta veritabanı dosyası için izin verilen megabayt sayısı. |
| \_MMS_MailQuotaWarningThreshold |Bir uyarı vermeden önce e-posta veritabanı dosyası için izin verilen megabayt sayısı. |
| \_MMS_MailTemplateName |Kullanıcının e-posta dosyasını oluşturmak için kullanılan e-posta şablon dosyası. Bir şablon belirtilirse, belirtilen şablonu kullanarak posta dosyası oluşturulur. Hiçbir Şablon belirtilmezse, varsayılan şablon dosyası dosyayı oluşturmak için kullanılır. |
| \_MMS_OU |OU adı certifier altında isteğe bağlı özellik. Bu özellik, kişiler için boş olmalıdır. |
| \_MMS_Password |Kullanıcılar için gerekli özellik. Özelliği nesnenin tanımlama dosyası için parolayı içerir. |
| \_MMS_UseAdminP |Özelliği, posta dosyası Domino sunucusundaki AdminP işlemi tarafından (verme işlemi için zaman uyumsuz) oluşturulmalıdır true olarak ayarlanmalıdır. Özelliği false olarak ayarlanırsa, posta dosyası Domino kullanıcıyla (verme işleminde zaman uyumlu) oluşturulur. |

Bir ilişkili tanımlama dosyası ile bir kullanıcı için \_MMS_Password özelliği, bir değer bulunmalıdır. Lotus Notes istemcisi aracılığıyla e-posta erişimi için bir kullanıcının MailServer ve MailFile özelliklerini bir değer içermesi gerekir.

Bir Web tarayıcısı üzerinden e-posta erişmek için aşağıdaki özellikleri değerler içermesi gerekir:

* MailFile - posta dosyasının depolandığı Lotus Domino sunucusunda bir yol içeriyor gerekli özelliği'ni tıklatın.
* MailServer - Lotus Domino sunucusu adını içeren gerekli özelliği'ni tıklatın. Bu değer Domino sunucusundaki Lotus posta dosyasını oluştururken kullanılacak adı olur.
* HTTPPassword - nesnesinin Web erişim parolasını içeren isteğe bağlı özellik.

Posta yetenek olmadan Domino sunucusuna erişmek için HTTPPassword özelliği bir değer içermelidir. MailFile özelliği ve MailServer özelliği boş olamaz.

İle \_MMS_ IDStoreType = 2 (depolama kimliği) posta dosyasındaki NotesRegistrationclass MailSystem özelliği REG_MAILSYSTEM_INOTES (3) ayarlanır.

### <a name="mandatory-attributes"></a>Zorunlu öznitelikler
Lotus Domino Bağlayıcısı'nı çoğunlukla bu tür nesneleri (belge türleri) destekler:

* Grup
* Posta veritabanı
* Kişi
* Kişi (hiçbir certifier kişiyle)
* Kaynak

Bu bölümde bir Domino sunucuya dışarı aktarmak desteklenen her nesne için zorunlu olan öznitelikler listelenir.

| Nesne Türü | Zorunlu öznitelikler |
| --- | --- |
| Grup |<li>ListName</li> |
| Ana veritabanı |<li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Kişi |<li>Soyadı</li><li>MailFile</li><li>Kısaad</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Kişi (hiçbir certifier kişiyle) |<li>\_MMS_IDRegType</li> |
| Kaynak |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Site</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Ortak sorunlar ve sorular
### <a name="schema-detection-does-not-work"></a>Şema algılama çalışmıyor
Şema algılayamayabilir için schema.nsf dosyasının Domino sunucu üzerinde var olduğunu gereklidir. LDAP sunucuda yüklüyse, bu dosya yalnızca görünür. Şema algılanamaz değilse, aşağıdakileri doğrulayın:

* Dosya schema.nsf Domino sunucusunun kök klasörde bulunur
* Kullanıcının schema.nsf dosyasını görmek için izni yok.
* LDAP sunucusu yeniden başlatılmasını zorlar. Açık **Lotus Domino konsol** ve **söyleyin LDAP ReloadSchema** şemayı yeniden yüklemek için komutu.

### <a name="not-all-secondary-address-books-are-visible"></a>Tüm İkincil adres defterleri görülebilir
Özelliğini Domino Bağlayıcısı'nı kullanır **Directory Yardım** ikincil adres defterleri bulmak için. İkincil adres defterleri eksikse doğrulayıp [Directory Yardım](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) etkin ve Domino sunucuda yapılandırılmış.

### <a name="custom-attributes-in-domino"></a>Domino özel öznitelikler
Domino Bağlayıcısı tarafından tüketilebilir özel özniteliği olarak göründüğü şekilde şemayı genişletmek için birkaç yolu vardır.

**Yaklaşım 1: Lotus Domino şemasını genişletme**

1. Domino Directory şablonu {PUBNAMES. bir kopyasını oluşturun NTF} izleyerek [adımları](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (uygulamasını varsayılan IBM Lotus Domino dizini şablonunu özelleştirmeniz gerekir değil):
2. {CONTOSO. kopyalama, Domino dizin şablonunu açın Domino Tasarımcısı'nda oluşturuldu ve bu adımları NTF} şablonu:
   * Paylaşılan öğelerini tıklatın ve alt genişletin
   * ${ObjectName} InheritableSchema alt çift tıklayın (burada {ObjectName} adıdır varsayılan yapısal nesne sınıfı, örneğin: kişi).
   * Bu öznitelik için şema {MyPersonAtrribute} ve karşılık gelen eklemek istediğiniz özniteliğin adı. Bir alan seçin tarafından oluşturma **oluşturma** menüsüne ve ardından **alan** menüsünde.
   * Eklenen alanında türünü, stili, boyutu, yazı tipi ve diğer ilgili parametreleri alan özellikleri penceresinde seçerek özelliklerini ayarlayın.
   * Varsayılan olarak bu öznitelik için belirtilen ad aynı değer özniteliği tutun (öznitelik adı MyPersonAttribute ise, örneğin, aynı ada sahip varsayılan değer tutun).
   * ${ObjectName} InheritableSchema alt güncelleştirilmiş değerlerle kaydedin.
3. Domino Directory şablonu {PUBNAMES. değiştirin NTF} yeni özelleştirilmiş şablonuyla {CONTOSO. NTF} izleyerek [adımları](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Domino yönetici kapatın ve LDAP hizmetini yeniden başlatın ve LDAP şemayı yeniden yüklemek için Domino konsolunu açın:
   * Domino konsolunda Ekle komutu altında **Domino komutu** - LDAP hizmetini yeniden başlatmak için Dosyalanan metin [yeniden görev LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * Şema LDAP yeniden yüklemek için LDAP ReloadSchema söyleyin söyleyin LDAP komutu - kullanın
5. Açık Domino yönetici ve select kişiler ve Gruplar sekmesinde eklenen öznitelik içinde domino yansıtılır görmek için kişi ekleyebilirsiniz.
6. Gelen Schema.nsf açmak **dosyaları** sekmesinde ve eklenen öznitelik dominoPerson LDAP nesne sınıfına yansıtılan bakın.

**Yaklaşım 2: bir auxClass ile özel bir öznitelik oluşturma ve nesne sınıfıyla ilişkilendirme**

1. Domino Directory şablonu {PUBNAMES. bir kopyasını oluşturun NTF} izleyerek [adımları](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (hiçbir zaman varsayılan IBM Lotus Domino dizini şablonu özelleştirme):
2. {CONTOSO. kopyalama, Domino dizin şablonunu açın Domino Tasarımcısı'nda oluşturulan NTF} şablonu.
3. Sol bölmede, paylaşılan kod ve alt formlar seçin.
4. Yeni alt'ı tıklatın
5. Yeni alt özelliklerini belirlemek için aşağıdakileri yapın:
   * Yeni alt açıkken, tasarım - alt Özellikler'i seçin.
   * Name özelliği yanındaki--Örneğin, TestSubform yardımcı nesne sınıfı için bir ad girin.
   * "Insert alt... iletişim seçili dahil" Options özelliği tutun
   * Seçenekler özelliği "işleme geçirme notları HTML'de." seçimini kaldırın
   * Diğer özellikler aynı bırakın ve alt özellikler kutusunu kapatın.
   * Kaydedin ve yeni alt kapatın.
6. Yardımcı nesne sınıfını tanımlamak için bir alan eklemek için aşağıdakileri yapın:
   * Oluşturduğunuz alt açın.
   * Seçin oluşturma - alan.
   * Örneğin alan iletişim kutusunun temel bilgiler sekmesinde adı yanındaki herhangi bir ad belirtin: {MyPersonTestAttribute}.
   * Eklenen alanında tür, stil, boyutu, yazı tipi ve ilgili özellikleri seçerek özelliklerini ayarlayın.
   * Varsayılan olarak bu öznitelik için belirtilen ad aynı değer özniteliği tutun (öznitelik adı MyPersonTestAttribute ise, örneğin, aynı ada sahip varsayılan değer tutun).
   * Güncelleştirilmiş değerlerle alt kaydedin ve aşağıdakileri yapın:
     * Paylaşılan kod ve alt sol bölmesinde seçin
     * Yeni alt seçin ve tasarım - tasarım özellikleri seçin.
     * Soldan üçüncü sekmesini tıklatın ve seçin **bu Yasak tasarım değişikliğini yayılması**.
7. ${ObjectName} ExtensibleSchema alt, (burada {ObjectName} varsayılan yapısal nesne sınıfı, örneğin – kişi adıdır) açın.
8. Kaynak Ekle ve (oluşturduğunuz, örneğin – TestSubform) alt seçin ve ${ObjectName} ExtensibleSchema alt kaydedin.
9. Domino Directory şablonu {PUBNAMES. değiştirin NTF} yeni özelleştirilmiş şablonuyla {CONTOSO. NTF} izleyerek [adımları](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Domino yönetici kapatın ve LDAP hizmetini yeniden başlatın ve LDAP şemayı yeniden yüklemek için Domino konsolunu açın:
    * Domino konsolunda Ekle komutu altında **Domino komutu** - LDAP hizmetini yeniden başlatmak için Dosyalanan metin [yeniden görev LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * Şema LDAP yeniden söyleyin LDAP komutunu kullanın **söyleyin LDAP ReloadSchema**.
11. Domino yönetici açın ve eklenen öznitelik domino Ekle kişi yansıtılan görmek için kişi ve grupları sekmesini seçin (diğerlerinin altında sekmesinde).
12. Gelen Schema.nsf açmak **dosyaları** sekmesinde ve eklenen öznitelik TestSubform LDAP yardımcı nesne sınıfı altında yansıtılır bakın.

**Yaklaşım 3: özel öznitelik ExtensibleObject sınıfına ekleyin.**

1. Kök dizinine yerleştirilen {Schema.nsf} dosyasını aç
2. Altında sol menüden LDAP nesne sınıfları seçin **tüm şema belgeleri** tıklatıp **eklemek nesne sınıfı** düğmesi:
3. (Burada zzz varsayılan yapısal nesne sınıfı, örneğin kişi adıdır) {zzzExtensibleSchema} biçiminde LDAP adını belirtin. Örneğin, nesne sınıfı için kişi şemasını için LDAP adı {PersonExtensibleSchema} sağlayın.
4. Şemayı genişletmek istediğiniz üst nesne sınıfı adı sağlayın. Örneğin, kişi nesne sınıfı için şemayı genişletmek için üst nesne sınıfı adı {dominoPerson} sağlar:
5. Nesne sınıfı için karşılık gelen geçerli bir OID sağlar.
6. Zorunlu veya isteğe bağlı öznitelik türlerini alan gereksinimi göredir altında genişletilmiş/özel öznitelikleri seçin:
7. Gerekli öznitelikler için ExtensibleObjectClass ekledikten sonra tıklatın **Kaydet ve Kapat**.
8. Bir ExtensibleObjectClass genişletilmiş öznitelikleri olan ilgili varsayılan nesne sınıfı için oluşturulur.

## <a name="troubleshooting"></a>Sorun giderme
* Bağlayıcı gidermek günlüğü etkinleştirme hakkında daha fazla bilgi için bkz: [bağlayıcıların ETW İzleme etkinleştirmek için nasıl](http://go.microsoft.com/fwlink/?LinkId=335731).
