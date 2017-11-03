---
title: "PowerShell Bağlayıcısı | Microsoft Docs"
description: "Bu makalede, Microsoft'un Windows PowerShell bağlayıcısının nasıl yapılandırılacağı açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 0e5ccf5a38072e31d85bbc63eb0c608b0c34cfc2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Windows PowerShell Bağlayıcısı Teknik Başvurusu
Bu makalede Windows PowerShell Bağlayıcısı açıklanmaktadır. Makale aşağıdaki ürünler için geçerlidir:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Düzeltme 4.1.3671.0 kullanmanız gerekir ya da daha sonra [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ve FIM2010R2 için bağlayıcı yükleme yoluyla kullanılabilir [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>PowerShell Bağlayıcısı genel bakış
PowerShell Bağlayıcısı'nı, API'ları Windows PowerShell tabanlı teklif dış sistemlerle eşitleme hizmeti tümleştirmenize olanak sağlar. Bağlayıcısı, 2 (ECMA2) framework ve Windows PowerShell arama tabanlı Genişletilebilir bağlantı yönetim Aracısı özellikleri arasında bir köprü sağlar. ECMA framework hakkında daha fazla bilgi için bkz: [Genişletilebilir bağlantı 2.2 Yönetim Aracı başvurusu](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Ön koşullar
Bağlayıcısı'nı kullanmadan önce aşağıdaki eşitleme sunucusunda sahip emin olun:

* 4.5.2 Microsoft .NET Framework veya daha yenisi
* Windows PowerShell 2.0, 3.0 veya 4.0

Eşitleme hizmeti sunucusundaki yürütme ilkesinin Windows PowerShell betikleri çalıştırmak bağlayıcı izin verecek şekilde yapılandırılması gerekir. Bağlayıcı çalıştırır dijital olarak imzalanmış, komut dosyaları yapılandırmadığınız sürece yürütme İlkesi şu komutu çalıştırarak:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Yeni bir bağlayıcı oluşturun
Eşitleme hizmeti Windows PowerShell bağlayıcısı oluşturmak için eşitleme hizmeti tarafından istenen adımlarını yerine getirmesi Windows PowerShell komut dosyalarını bir dizi sağlamanız gerekir. Bağlandığınız veri kaynağı ve ihtiyaç duyduğunuz işlevselliği bağlı olarak, uygulamanız gereken komut dosyalarını değişir. Bu bölümde her, uygulanabilir ve gerekli olduğunda komut dosyalarının özetlenmektedir.

Windows PowerShell Bağlayıcısı'nı her eşitleme hizmeti veritabanı içinde komut dosyalarını depolamak için tasarlanmıştır. Dosya sisteminde depolanan komut dosyaları çalıştırmak mümkün olsa da, her komut dosyası doğrudan içinde bağlayıcı'nın yapılandırması gövdesi eklenecek daha kolaydır.

Bir PowerShell bağlayıcısı oluşturmak için **eşitleme hizmeti** seçin **yönetim Aracısı** ve **oluşturma**. Seçin **PowerShell (Microsoft)** bağlayıcı.

![Bağlayıcı oluşturma](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Bağlantı
Uzak bir sisteme bağlanmak için yapılandırma parametreleri sağlayın. Bu değerler güvenli bir şekilde eşitleme hizmeti tarafından depolanan ve Bağlayıcısı'nı çalıştırdığınızda, Windows PowerShell komut dosyaları için kullanılabilir.

![Bağlantı](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Aşağıdaki bağlantı parametrelerini yapılandırabilirsiniz:

**Bağlantı**

| Parametre | Varsayılan değer | Amaç |
| --- | --- | --- |
| Sunucu |<Blank> |Bağlayıcının bağlanması gereken sunucu adı. |
| Etki alanı |<Blank> |Bağlayıcısı'nı çalıştırdığınızda kullanmak üzere saklamak için kimlik bilgilerini etki alanı. |
| Kullanıcı |<Blank> |Bağlayıcısı'nı çalıştırdığınızda kullanılmak üzere depolamak için kullanıcı adı kimlik bilgisi. |
| Parola |<Blank> |Bağlayıcısı'nı çalıştırdığınızda kullanmak üzere saklamak için kimlik bilgilerini parolası. |
| Bağlayıcı hesabının kimliğine bürün |False |Doğru olduğunda, eşitleme hizmeti bağlamında sağlanan kimlik bilgileri, Windows PowerShell komut dosyalarını çalıştırır. Mümkün olduğunda, önerilir **$Credentials** parametresi geçirilir kadar her komut dosyası, kimliğe bürünme yerine kullanılır. Bu seçeneği kullanmak için gereken ek izinler hakkında daha fazla bilgi için bkz: [kimliğe bürünme için ek yapılandırma](#additional-configuration-for-impersonation). |
| Belirlerken kullanıcı profilini yükle |False |Kimliğe bürünme sırasında bağlayıcı'nın kimlik bilgileri kullanıcı profilini yüklemek için Windows bildirir. Kimliğine bürünülen kullanıcı gezici profili varsa, bağlayıcı gezici profil yüklemez. Bu parametre kullanmak için gereken ek izinler hakkında daha fazla bilgi için bkz: [kimliğe bürünme için ek yapılandırma](#additional-configuration-for-impersonation). |
| Belirlerken oturum açma türü |None |Kimliğe bürünme sırasında oturum açma türü. Daha fazla bilgi için bkz: [dwLogonType] [ dw] belgeleri. |
| Yalnızca imzalı komut dosyaları |False |TRUE ise, Windows PowerShell Bağlayıcısı'nı her komut dosyası geçerli bir dijital imzaya sahip olduğunu doğrular. False ise eşitleme hizmeti sunucusunun Windows PowerShell yürütme İlkesi RemoteSigned veya Kısıtlanmamış olduğundan emin olun. |

**Ortak Modülü**  
Bağlayıcı yapılandırmasında paylaşılan bir Windows PowerShell modülü depolamanıza olanak sağlar. Bağlayıcısı'nı bir komut dosyası çalıştığında, böylece her komut dosyası tarafından içe aktarılabilir Windows PowerShell modülünü dosya sistemine ayıklanır.

İçeri aktarma, dışarı aktarma ve parola eşitleme komut dosyaları için ortak modülü bağlayıcı'nın MAData klasörüne ayıklanır. Şema, doğrulama, hiyerarşi ve bölüm bulma komut dosyaları için ortak modül % TEMP % klasörüne ayıklanır. Her iki durumda da ayıklanan ortak modülü betik ortak modülü betik adı ayarına göre adlandırılır.

MAData klasöründen FIMPowerShellConnectorModule.psm1 adlı bir modül yüklemek için şu deyimi kullanın:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

% TEMP % klasöründen FIMPowerShellConnectorModule.psm1 adlı bir modül yüklemek için şu deyimi kullanın:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parametre doğrulama**  
Doğrulama betiği yönetici tarafından sağlanan bağlayıcı yapılandırma parametreleri geçerli olduğundan emin olmak için kullanılan isteğe bağlı bir Windows PowerShell Betiği ' dir. Doğrulanıyor, bağlantı kimlik bilgileri ve bağlantı parametreleri doğrulama betiği, yaygın kullanımları şunlardır. Doğrulama betiği sonra aşağıdaki sekmeleri denir ve iletişim kutuları değiştirilebilir:

* Bağlantı
* Genel Parametreler
* Bölüm yapılandırma

Doğrulama betiği bağlayıcısından aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Yapılandırma sekmesini veya doğrulama isteği tetiklenen iletişim. |
| ConfigParameters |[KeyedCollection] [ keyk] [dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |

Doğrulama betiği tek bir ParameterValidationResult nesneyi ardışık düzene döndürmelidir.

**Şema bulma**  
Şema bulma komut dosyasını zorunludur. Bu komut dosyası nesne türleri, öznitelikleri ve öznitelik akışı kurallarını yapılandırırken eşitleme hizmetini kullanan öznitelik kısıtlamalarını döndürür. Şema bulma komut dosyasını Bağlayıcısı oluşturma sırasında çalıştırılır ve bağlayıcı'nın şema doldurur. Ayrıca, Eşitleme Hizmeti Yöneticisi'nde yenileme şema eylemi tarafından kullanılır.

Şema bulma komut dosyasını bir bağlayıcı aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection] [ keyk] [dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |

Tek bir komut dosyası döndürmelidir [şema] [ schema] ardışık düzenine nesnesi. Şema nesnesi oluşan [SchemaType] [ schemaT] nesne türleri temsil eden nesneler (örneğin: kullanıcılar ve gruplar). SchemaType nesnesi koleksiyonu tutar [SchemaAttribute] [ schemaA] özniteliklerini temsil eden nesneler (örneğin: verilen ad, Soyadı ve posta adresi) türü.

**Ek parametreler**  
Standart yapılandırma ayarlarına ek olarak, bağlayıcı örneğine özgü ek özel yapılandırma ayarlarını tanımlayabilirsiniz. Bu parametreleri bölüm, bağlayıcı belirtilebilir veya çalışma adımı düzeyleri ve ilgili Windows PowerShell komut dosyasını erişilebilir. Özel yapılandırma ayarlarına düz metin biçiminde eşitleme hizmeti veritabanında depolanabilir veya şifrelenmiş. Eşitleme hizmeti otomatik olarak şifreler ve şifrelerinin çözülmesi güvenli gerektiğinde yapılandırma ayarları.

Özel yapılandırma ayarlarını belirtmek için her parametre adı virgülle (,) ayırın.

Bir komut dosyasından özel yapılandırma ayarlarına erişmek için bir alt çizgi ile ad soneki ( \_ ) ve (Genel, bölüm veya RunStep) parametre kapsamı. Örneğin, genel FileName parametresi erişmek için bu kod parçacığını kullanın:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Özellikler
Yönetim Aracısı Tasarımcısı özellikleri sekmesinde bağlayıcı işlevselliğini ve davranışını tanımlar. Bağlayıcısı oluşturduğunuzda, bu sekmede yaptığınız seçimlere değiştirilemez. Bu tablo yetenek ayarları listeler.

![Özellikler](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Özellik | Açıklama |
| --- | --- |
| [Ayırt edici ad stili][dnstyle] |Ayırt edici ad Bağlayıcısı'nı destekler ve bu nedenle, ne stil gösterir. |
| [Dışarı aktarma türü][exportT] |Dışa aktarma betiği sunulan nesnelerin türünü belirler. <li>AttributeReplace – öznitelik değiştiğinde kümesini birden çok değerli bir öznitelik değerlerini içerir.</li><li>AttributeUpdate – öznitelik değiştiğinde yalnızca birden çok değerli bir özniteliğe farkları içerir.</li><li>MultivaluedReferenceAttributeUpdate - başvuru olmayan birden çok değerli öznitelikler için değerler ve birden çok değerli başvuru özniteliği için yalnızca farkları tam kümesi içerir.</li><li>ObjectReplace – herhangi bir öznitelik değişiklikleri olduğunda bir nesne için tüm özniteliklerini içerir</li> |
| [Veri normalleştirme][DataNorm] |Komut dosyaları için sağlanan önce bağlantı öznitelikleri normalleştirmek için eşitleme hizmeti bildirir. |
| [Nesne onayı][oconf] |Bekleyen alma davranışını eşitleme hizmetini yapılandırır. <li>Normal – içeri aktarma onaylanması için dışarı aktarılan tüm değişiklikleri bekliyor varsayılan davranışı</li><li>NoDeleteConfirmation – bir nesnesi silindiğinde var. oluşturulan hiçbir bekleyen alma</li><li>NoAddAndDeleteConfirmation – bir nesne oluşturulduğunda veya silindiğinde var. oluşturulan hiçbir bekleyen alma</li> |
| DN bağlantı kullanın |Ayırt edici adı stili LDAP olarak ayarlanırsa, bağlantı için bağlayıcı alanı da ayırt edici adı özniteliğidir. |
| Eşzamanlı operasyonlar birkaç bağlayıcı |İşaretlendiğinde, birden çok Windows PowerShell bağlayıcı aynı anda çalışabilir. |
| Bölümler |İşaretlendiğinde, birden fazla bölüm ve bölüm bulma bağlayıcıyı destekler. |
| Hiyerarşisi |İşaretlendiğinde, bir LDAP stili hiyerarşik yapısı bir bağlayıcıyı destekler. |
| İçeri aktarma etkinleştir |İşaretlendiğinde, Bağlayıcısı verileri içe aktarma komut dosyaları aracılığıyla alır. |
| Delta içeri aktarma etkinleştir |İşaretlendiğinde, bağlayıcı içe aktarma komut dosyalarından farkları isteyebilir. |
| Vermeyi etkinleştir |İşaretlendiğinde, bağlayıcı dışarı aktarma komut dosyaları aracılığıyla veri aktarır. |
| Tam vermeyi etkinleştir |İşaretlendiğinde, tüm bağlayıcı alanı dışarı aktarma verme komut dosyalarını destekler. Bu seçeneği kullanmak için etkinleştirme verme ayrıca denetlenmesi gerekir. |
| İlk verme geçişi başvuru değer yok |Başvuru özniteliği, bu onay kutusu işaretlendiğinde, ikinci bir dışarı aktarma geçişinde dışarı aktarılır. |
| Nesne yeniden adlandırma etkinleştir |İşaretlendiğinde, ayırt edici ad değiştirilebilir. |
| Değiştirirken DELETE Ekle |İşaretlendiğinde,-işlemlerini tek bir değiştirme dışarı aktarılır delete ekleyin. |
| Parola işlemleri etkinleştir |İşaretlendiğinde, parola eşitleme komut dosyaları desteklenir. |
| Dışarı aktarma parolasını ilk seferde etkinleştir |Nesne oluşturulduğunda, bu onay kutusu işaretlendiğinde, sağlama işlemi sırasında ayarlamak parolalar dışa. |

### <a name="global-parameters"></a>Genel Parametreler
Yönetim Aracısı Tasarımcısı'nda genel parametreleri sekmesi bağlayıcı tarafından çalıştırılan Windows PowerShell komut dosyalarını yapılandırmanıza olanak sağlar. Bağlantı sekmesinde tanımlanan özel yapılandırma ayarları için genel değerleri de yapılandırabilirsiniz.

**Bölüm bulma**  
Bir bölüm bir paylaşılan şema içinde ayrı bir ad alanıdır. Örneğin, Active Directory'de bir bölüm bir ormandaki her etki alanıdır. Mantıksal gruplandırma içeri aktarma için bir bölümdür ve verme işlemleri. Bir bağlam ve tüm işlemler olduğu sürece bu bağlamda içeri ve dışarı aktarma bölümü olmalıdır. Bölümler bir hiyerarşide LDAP temsil beklenir. Bir bölüm ayırt edici adını alma işleminde tüm döndürülen nesnelerin bir bölüm kapsamında olduğunu doğrulamak için kullanılır. Bölüm ayırt edici adı da meta veri deposu için bağlayıcı alanı sağlama işlemi sırasında bir nesne dışa aktarma sırasında ile ilişkilendirilmesi gereken bölüm belirlemek için kullanılır.

Bölüm bulma komut dosyasını bir bağlayıcı aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |

Komut dosyasını bir ya da tek bir döndürmelidir [bölüm] [ part] nesne veya bölüm nesnelerini ardışık düzene [T] listesi.

**Hiyerarşi bulma**  
Hiyerarşi bulma komut dosyası, yalnızca ayırt edici adı stil özelliği LDAP olduğunda kullanılır. Komut dosyasını bulun ve veya içeri aktarma için kapsam dışına olarak kabul edilen bir dizi kapsayıcıları seçin olanak sağlar ve verme işlemleri için kullanılır. Komut dosyası, yalnızca doğrudan komut dosyasına sağlanan kök düğümünün alt düğümleri listesini sağlaması gerekir.

Hiyerarşi bulma komut dosyasını bir bağlayıcı aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| ParentNode |[HierarchyNode][hn] |Kök düğüm doğrudan alt betik döndürme zorunluluğu hiyerarşisinin. |

Komut dosyasını bir ya da ardışık düzene tek alt HierarchyNode nesne veya alt HierarchyNode nesnelerin bir listesini [T] döndürmelidir.

#### <a name="import"></a>İçeri Aktarma
İçeri aktarma işlemlerini destekleyen bağlayıcılar üç betikleri uygulamalıdır.

**Başlangıç alma**  
Begin içe aktarma komut dosyasının bir almayı Çalıştır adımı başında çalıştırılır. Bu adım sırasında kaynak sistem bağlantı kurmak ve hazırlık adımlarını bağlı sistemden veri içe aktarmadan önce yapın.

Begin içe aktarma komut dosyasının bir bağlayıcı aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Komut dosyasını çalıştır alma (Değişim veya tam) bölümü, hiyerarşi, filigran ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| Türler |[Şema][schema] |Alınan bağlayıcı alanı için şema. |

Tek bir komut dosyası döndürmelidir [OpenImportConnectionResults] [ oicres] ardışık düzene örneğin nesnesi:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Veri alma**  
Komut dosyasını içeri aktarmak için başka veri yok olduğunu gösterir kadar alma veri betiği bağlayıcı tarafından çağrılır. Windows PowerShell Bağlayıcısı 9.999 nesnelerin bir sayfa boyutu vardır. Kodunuzu almak için birden fazla 9.999 nesneleri döndürürse, disk belleği desteklemelidir. Her alma veri betik böylece, bir depolama alanına Filigran kullanabileceğiniz bir özel veri özelliği adlı bağlayıcı açığa çıkarır, kaldığı yerden nesne aktarırken kodunuzu sürdürür.

İçeri aktarma veri betiği bağlayıcısından aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |İçeri aktarmalar sırasında kullanılan Filigran (CustomData) disk belleğine alınan ve delta alır tutar. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Komut dosyasını çalıştır alma (Değişim veya tam) bölümü, hiyerarşi, filigran ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| Türler |[Şema][schema] |Alınan bağlayıcı alanı için şema. |

İçeri aktarma veri betiği listesini yazmalısınız [[CSEntryChange][csec]] ardışık düzenine nesnesi. Bu koleksiyon, içeri aktarılan her nesneyi temsil eden CSEntryChange özniteliklerini oluşur. Tam içeri aktarma çalıştırması sırasında bu koleksiyonu tüm öznitelikleri her nesne için sahip CSEntryChange nesneleri tam bir dizi olmalıdır. Bir Delta içeri aktarma sırasında CSEntryChange nesne ya da almak için her bir nesne ya da (değiştirme modu) değiştirilen nesneleri tam bir gösterimi için öznitelik düzeyi farkları içermesi gerekir.

**Son alma**  
Almayı Çalıştır sonuç son alma betiği çalıştırılır. Bu komut tüm temizleme gerekli görevleri (örneğin, Kapat bağlantıları sistemler için) ve hataları yanıt gerçekleştirmeniz gerekir.

Son içe aktarma komut dosyasının bir bağlayıcı aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Komut dosyasını çalıştır alma (Değişim veya tam) bölümü, hiyerarşi, filigran ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Komut dosyası alma işlemi sona erdi nedeni hakkında bilgilendirir. |

Tek bir komut dosyası döndürmelidir [CloseImportConnectionResults] [ cicres] ardışık düzene örneğin nesnesi:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Dışarı Aktarma
Bağlayıcı alma mimarisiyle aynı, dışarı aktarma desteği bağlayıcılar üç betikleri uygulamalıdır.

**Begin dışarı aktarma**  
Bir verme çalıştırma adım başında başlangıç dışa aktarma betiği çalıştırılır. Bu adım sırasında kaynak sistem bağlantı kurmak ve bağlı sisteme verileri dışarı aktarmadan önce herhangi bir hazırlık adımı gerçekleştirin.

Begin dışa aktarma betiği aşağıdaki parametreleri bağlayıcıyı alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Komut dosyasını çalıştır verme (Değişim veya tam) bölümü, hiyerarşi ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| Türler |[Şema][schema] |Dışa aktarılan bağlayıcı alanı için şema. |

Betik herhangi bir çıktı ardışık düzene vermemelidir.

**Verileri dışarı aktarma**  
Eşitleme hizmeti sayıda tüm bekleyen dışarı aktarmalar işlemek gerekli olan verileri dışa aktar komut dosyasını çağırır. Bağlayıcı alanı bağlayıcı'nın sayfa boyutundan daha fazla bekleyen dışarı aktarmalar varsa, verme veri betiği birden çok kez ve büyük olasılıkla birden çok kez aynı nesne için çağrılabilir.

Dışarı aktarma veri betiği bağlayıcısından aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| CSEntries |IList[CSEntryChange][csec] |Bağlayıcı alanı bekleyen tüm nesneler ile bu geçişi sırasında işlenmek üzere dışarı listesi. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Komut dosyasını çalıştır verme (Değişim veya tam) bölümü, hiyerarşi ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| Türler |[Şema][schema] |Dışa aktarılan bağlayıcı alanı için şema. |

Dışarı aktarma veri betiği döndürmelidir bir [PutExportEntriesResults] [ peeres] ardışık düzenine nesnesi. Bu nesne, bir hata veya bağlantı özniteliği bir değiştirme oluşmadığı sürece dışarı aktarılan her bağlayıcı için sonuç bilgileri içerecek şekilde gerekmez. Örneğin, bir PutExportEntriesResults nesnesi ardışık düzene dönmek için şunu yazın:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Son verme**  
Sonuç dışarı aktarma çalıştırma, son dışarı aktarma komut dosyasının çalışmasını. Bu komut tüm temizleme gerekli görevleri (örneğin, Kapat bağlantıları sistemler için) ve hataları yanıt gerçekleştirmeniz gerekir.

Son dışa aktarma betiği aşağıdaki parametreleri bağlayıcıyı alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Komut dosyasını çalıştır verme (Değişim veya tam) bölümü, hiyerarşi ve beklenen sayfa boyutu türü hakkında bilgilendirir. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Komut dosyası dışarı aktarma sonlandırıldı nedeni hakkında bilgilendirir. |

Betik herhangi bir çıktı ardışık düzene vermemelidir.

#### <a name="password-synchronization"></a>Parola Eşitleme
Windows PowerShell bağlayıcılar parola değişiklikleri/sıfırlama için hedef olarak kullanılabilir.

Parola betik bağlayıcısından aşağıdaki parametreleri alır:

| Ad | Veri türü | Açıklama |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][dize [ConfigParameter][cp]] |Tablo Bağlayıcısı için yapılandırma parametreleri. |
| Kimlik Bilgisi |[PSCredential][pscred] |Yönetici tarafından bağlantı sekmesinde girilmiş olan kimlik bilgileri içerir. |
| Bölüm |[Bölüm][part] |CSEntry bulunduğu dizini bölümü. |
| CSEntry |[CSEntry][cse] |Bağlayıcı alanı girdisi nesne için bir parola değiştirme veya sıfırlama aldı. |
| OperationType |Dize |İşlemi sıfırlama olup olmadığını gösterir (**SetPassword**) veya bir değiştirme (**parola değiştirme**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Hedeflenen parolayı belirtin bayrakları davranışı sıfırlayın. Bu parametre yalnızca OperationType ise kullanılabilir **SetPassword**. |
| EskiParola |Dize |Parola değişiklikleri için nesnenin eski parola ile doldurulur. Bu parametre yalnızca OperationType ise kullanılabilir **parola değiştirme**. |
| #Newpassword |Dize |Komut dosyası ayarlaması gereken nesnenin yeni parola ile doldurulur. |

Parola betik, herhangi bir sonuç Windows PowerShell ardışık düzene dönmek için beklenmiyor. Parola komut dosyasında bir hata meydana gelirse, komut dosyası eşitleme hizmeti sorun hakkında bilgilendirmek için aşağıdaki özel durumlardan birini oluşturması gerekir:

* [PasswordPolicyViolationException] [ pwdex1] – parola bağlı sistemde parola ilkesi karşılamıyorsa oluşturulur.
* [PasswordIllFormedException] [ pwdex2] – parola bağlı sistemi için kabul edilebilir değilse oluşturulur.
* [PasswordExtension] [ pwdex3] – parola betik diğer tüm hatalar için oluşturulur.

## <a name="sample-connectors"></a>Örnek bağlayıcılar
Kullanılabilir örnek bağlayıcılar eksiksiz bir genel bakış için bkz: [Windows PowerShell Bağlayıcısı örnek bağlayıcı koleksiyonu][samp].

## <a name="other-notes"></a>Diğer Notlar
### <a name="additional-configuration-for-impersonation"></a>Kimliğe bürünme için ek yapılandırma
Kullanıcı Kimliğine bürünülen eşitleme hizmeti sunucusunda aşağıdaki izinleri verin:

Aşağıdaki kayıt defteri anahtarları için okuma erişimi:

* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Environment

Eşitleme hizmeti hizmet hesabının güvenlik tanımlayıcısı (SID) belirlemek için aşağıdaki PowerShell komutlarını çalıştırın:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Aşağıdaki dosya sistemi klasörleri okuma erişimi:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Windows PowerShell bağlayıcı adı için {ConnectorName} yer tutucu değiştirin.

## <a name="troubleshooting"></a>Sorun giderme
* Bağlayıcı gidermek günlüğü etkinleştirme hakkında daha fazla bilgi için bkz: [bağlayıcıların ETW İzleme etkinleştirmek için nasıl](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
