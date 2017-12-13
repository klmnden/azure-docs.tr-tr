---
title: "Genel LDAP Bağlayıcısı | Microsoft Docs"
description: "Bu makalede, Microsoft'un genel LDAP bağlayıcısının nasıl yapılandırılacağı açıklanmaktadır."
services: active-directory
documentationcenter: 
author: AndKjell
manager: mtillman
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fe8db8f2a2412a3dfdf31201678c51e4fa0cee30
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Genel LDAP Bağlayıcısı Teknik Başvurusu
Bu makalede genel LDAP Bağlayıcısı'nı açıklar. Makale aşağıdaki ürünler için geçerlidir:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Düzeltme 4.1.3671.0 kullanmanız gerekir ya da daha sonra [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ve FIM2010R2 için bağlayıcı yükleme yoluyla kullanılabilir [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

IETF RFC söz konusu olduğunda, bu belgenin biçimini kullanarak (RFC [RFC numarası] / [bölüm RFC belgede]), örneğin (RFC 4512/4.3).
(Doğru RFC numarasıyla 4500 değiştirmeniz gerekiyor) http://tools.ietf.org/html/rfc4500 daha fazla bilgi bulabilirsiniz.

## <a name="overview-of-the-generic-ldap-connector"></a>Genel LDAP Bağlayıcısı genel bakış
Genel LDAP Bağlayıcısı'nı, eşitleme hizmeti bir LDAP v3 sunucusuyla tümleştirmenize olanak sağlar.

Belirli işlemleri ve şema öğeleri, delta alma işlemini gerçekleştirmek için gerekenler gibi IETF RFC belirtilmedi. Bu işlemler için açıkça belirtilen yalnızca LDAP dizinleri desteklenir.

Üst düzey açısından bakıldığında, aşağıdaki özellikler bağlayıcı bir geçerli sürümü tarafından desteklenir:

| Özellik | Destek |
| --- | --- |
| Bağlı veri kaynağı |Bağlayıcı tüm LDAP v3 sunucuları (RFC 4510 uyumlu) desteklenir. Bunu aşağıdaki ile test edilmiştir: <li>Microsoft Active Directory Basit Dizin Hizmetleri (AD LDS)</li><li>Microsoft Active Directory genel katalog (GC AD)</li><li>389 dizin sunucusu</li><li>Apache dizin sunucusu</li><li>IBM Tivoli DS</li><li>Isode dizini</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Açık DJ</li><li>Açık DS</li><li>Açık LDAP (openldap.org)</li><li>Oracle (önceden Sun) dizin Server Enterprise Edition</li><li>RadiantOne sanal dizin sunucusu (VDS)</li><li>Sun bir dizin sunucusu</li>**Önemli dizinler desteklenmiyor:** <li>Microsoft Active Directory etki alanı [yerleşik Active Directory Bağlayıcısı kullanın] Hizmetleri (AD DS)</li><li>Oracle Internet dizini (OID)</li> |
| Senaryolar |<li>Nesne yaşam döngüsü yönetimi</li><li>Grup Yönetimi</li><li>Parola Yönetimi</li> |
| İşlemler |Aşağıdaki işlemleri tüm LDAP dizinleri desteklenir: <li>Tam içeri aktarma</li><li>Dışarı Aktarma</li>Aşağıdaki işlemleri yalnızca belirtilen dizinleri desteklenir:<li>Delta içeri aktarma</li><li>Parola, parola değiştirme</li> |
| Şema |<li>Şema LDAP şemadan (RFC3673 ve RFC4512/4.2) algılandı</li><li>Yapısal sınıflar, aux sınıfları ve extensibleObject nesne sınıfı (RFC4512/4.3) destekler</li> |

### <a name="delta-import-and-password-management-support"></a>Delta içeri aktarma ve parola yönetimi desteği
Delta içeri aktarma ve parola yönetimi için desteklenen dizinler:

* Microsoft Active Directory Basit Dizin Hizmetleri (AD LDS)
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ayarlama destekler
* Microsoft Active Directory genel katalog (GC AD)
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ayarlama destekler
* 389 dizin sunucusu
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Apache dizin sunucusu
  * Bu dizin kalıcı değişiklik günlüğü olmadığından delta içeri aktarma desteklemiyor
  * Parola ayarlama destekler
* IBM Tivoli DS
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Isode dizini
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Novell eDirectory ve NetIQ eDirectory
  * Delta içeri aktarma için ekleme, güncelleştirme ve yeniden adlandırma işlemlerini destekler
  * Delta içeri aktarma için silme işlemlerini desteklemiyor
  * Parola ve parola değiştirme destekler ayarlayın
* Açık DJ
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Açık DS
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Açık LDAP (openldap.org)
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ayarlama destekler
  * Parola değiştirme desteklemiyor
* Oracle (önceden Sun) dizin Server Enterprise Edition
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* RadiantOne sanal dizin sunucusu (VDS)
  * Sürüm 7.1.1 kullanıyor olmanız gerekir veya üzeri
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın
* Sun bir dizin sunucusu
  * Delta içeri aktarma için tüm işlemleri destekler
  * Parola ve parola değiştirme destekler ayarlayın

### <a name="prerequisites"></a>Ön koşullar
Bağlayıcısı'nı kullanmadan önce aşağıdaki eşitleme sunucusunda sahip emin olun:

* 4.5.2 Microsoft .NET Framework veya daha yenisi

### <a name="detecting-the-ldap-server"></a>LDAP sunucusu algılama
Bağlayıcı üzerinde çeşitli teknikler algılamak ve LDAP sunucusu tanımlamak için kullanır. Bağlayıcı kök DSE, satıcı adı/sürümü kullanır ve benzersiz nesneleri ve öznitelikleri belirli LDAP sunucuları bilinmektedir bulmak için şema inceler. Bu veriler, bulundu, bağlayıcı yapılandırma seçeneklerinde önceden doldurmak için kullanılır.

### <a name="connected-data-source-permissions"></a>Bağlı veri kaynağı izinleri
Alma işlemini gerçekleştirmek ve bağlı dizindeki nesnelerin işlemler dışarı aktarmak için bağlayıcı hesabı yeterli izniniz olması gerekir. Bağlayıcı izinlerine dışarı aktarmak yazma ve Okuma izinleri içeri aktarabilmek gerekir. İzni yapılandırması hedef dizin yönetimi deneyimleri içinde gerçekleştirilir.

### <a name="ports-and-protocols"></a>Bağlantı noktalarını ve protokolleri
Bağlayıcı, LDAP için 389 ve 636 LDAPS için varsayılan yapılandırmasında belirtilen bağlantı noktası numarası kullanır.

LDAPS için SSL 3.0 veya TLS kullanmanız gerekir. SSL 2.0 desteklenmez ve devre dışı bırakılamaz.

### <a name="required-controls-and-features"></a>Gerekli denetimleri ve özellikleri
Aşağıdaki LDAP denetimleri/özellikleri bağlayıcısının düzgün çalışması için LDAP sunucusunda kullanılabilir olması gerekir:  
`1.3.6.1.4.1.4203.1.5.3`True/False filtreleri

True/False filtre sık LDAP dizinleri tarafından desteklenen olarak bildirilmedi ve üzerinde gösterebilir **genel sayfa** altında **zorunlu özellikleri bulunamadı**. Oluşturmak için kullanılan **veya** birden çok nesne türlerini alırken örneğin LDAP sorguları filtreleri. Birden fazla nesne türü içe aktarırsanız, LDAP sunucunuzun bu özelliğini destekler.

Benzersiz bir tanımlayıcı bağlantı olduğu bir dizin kullanırsanız aşağıdaki da kullanılabilir olmalıdır (daha fazla bilgi için bkz: [yapılandırma bağlayıcılarını](#configure-anchors) bölümü):  
`1.3.6.1.4.1.4203.1.5.1`Tüm işlem öznitelikleri

Dizine ne dizinine bir çağrısında sığabilecek daha çok nesne varsa, disk belleği kullanmak için önerilir. Sayfalama çalışmak aşağıdaki seçeneklerden birini gerekir:

**Seçenek 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Seçenek 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Her iki seçenek bağlayıcı Yapılandırması etkinleştirilirse, pagedResultsControl kullanılır.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl yalnızca silinen nesneleri görebilmek için USNChanged delta içe aktarma yöntemi ile kullanılır.

Bağlayıcı, sunucuda mevcut seçenekler algılamaya çalışır. Seçenekleri algılanamaz ise bir uyarı Bağlayıcısı özelliklerinde genel sayfasında mevcuttur. Mevcut tüm denetimler/özellikleri tüm LDAP sunucuları destekledikleri ve bu uyarıyı mevcut olsa bile, bağlayıcı sorunsuz çalışabilir.

### <a name="delta-import"></a>Delta içeri aktarma
Delta içeri aktarma yalnızca için destek directory algılandı. Aşağıdaki yöntemlerden şu anda kullanılır:

* LDAP Accesslog. Bkz: [http://www.openldap.org/doc/admin24/overlays.html#Access günlüğe kaydetme](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* LDAP değişim günlüğü. Bkz: [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Zaman damgası. Novell/NetIQ eDirectory için bağlayıcı almak için son tarih kullanan oluşturuldu ve nesneleri güncelleştirildi. Novell/NetIQ eDirectory silinen nesneleri almak eşdeğer bir anlamına gelir sağlamaz. Başka bir delta içeri aktarma yöntemi LDAP sunucusunda etkinse, bu seçenek de kullanılabilir. Bu seçenek silinmiş alma nesnelere mümkün değildir.
* USNChanged. Bkz: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Desteklenmiyor
Aşağıdaki LDAP özellikler desteklenmez:

* Sunucuları (RFC 4511/4.1.10) arasında LDAP başvuruları

## <a name="create-a-new-connector"></a>Yeni bir bağlayıcı oluşturun
Bir genel LDAP bağlayıcısı oluşturmak için **eşitleme hizmeti** seçin **yönetim Aracısı** ve **oluşturma**. Seçin **genel LDAP (Microsoft)** bağlayıcı.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Bağlantı
Bağlantı sayfasında, konak, bağlantı noktası ve bağlama bilgilerini belirtmeniz gerekir. Bağlama olduğu bağlı olarak seçilen, ek aşağıdaki bölümlerdeki bilgileri sağlamış.

![Bağlantı](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* Bağlantı zaman aşımı ayarını yalnızca ilk sunucuya bağlantı için şema tespit edilirken kullanılır.
* Varsa bağlama anonim, ardından hiçbiri kullanıcı adı / parola veya sertifika kullanılır.
* Diğer bağlamaları için bilgi ya da kullanıcı adı girin / parola veya bir sertifika seçin.
* Kerberos kimlik doğrulaması için kullanıyorsanız, aynı zamanda kullanıcının bölge/etki alanı sağlar.

**Öznitelik diğer adları** metin kutusu RFC4522 sözdizimiyle şemasında tanımlanan öznitelikleri için kullanılır. Bu öznitelikler şeması algılama sırasında algılanamıyor ve bağlayıcı özniteliklerle tanımlamak için Yardım gerekiyor. Örneğin aşağıdaki doğru userCertificate özniteliği bir ikili öznitelik olarak tanımlamak için öznitelik diğer adları kutusunda girilmesi gerekir:

`userCertificate;binary`

Bu yapılandırma gibi nasıl görünebilir için bir örnek verilmiştir:

![Bağlantı](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Seçin **işletimsel öznitelikleri şemada yer** de sunucu tarafından oluşturulan öznitelikler eklemek için onay kutusunu. Bu nesnenin ne zaman oluşturulduğu ve en son güncelleştirme zamanı gibi öznitelikleri içerir.

Seçin **Genişletilebilir öznitelikleri şemada yer** genişletilebilen nesneler (RFC4512/4.3) kullanılır ve bu seçeneğin etkinleştirilmesi, tüm nesne üzerinde kullanılacak her bir özniteliği sağlar. Bu özellik bağlı dizin kullanmadığınız sürece seçeneğin tutmak için öneri olacak şekilde bu seçeneğin belirlenmesi şeması çok büyük yapar.

### <a name="global-parameters"></a>Genel Parametreler
Genel Parametreler sayfasında DN ek LDAP özellikleri ve delta değişiklik günlüğü için yapılandırın. LDAP sunucusu tarafından sağlanan bilgilerle önceden doldurulmuş haldedir sayfasıdır.

![Bağlantı](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Üst kısmında, sunucu adı gibi sunucu kendisi tarafından sağlanan bilgileri gösterir. Bağlayıcı ayrıca zorunlu denetimleri kök DSE mevcut olduğunu doğrular. Bu denetimler listelenmiyorsa, bir uyarı görüntülenir. Bazı LDAP dizinleri kök DSE'ındaki tüm özelliklere listelemez ve bir uyarı mevcut olsa bile bağlayıcı sorunsuz çalışır mümkündür.

**Denetimler desteklenen** onay kutularını belirli işlemler için davranışını denetler:

* Seçili ağaç silme ile bir hiyerarşi silinmiş bir LDAP çağrısı ile. Seçili ağaç silme ile bağlayıcı gerekirse bir özyinelemeli silme yapar.
* Seçili disk belleğine alınan sonuçlarla bağlayıcı çalışma adımları belirtilen boyutta bir disk belleğine alınan alma yapar.
* VLVControl ve SortControl alternatif LDAP dizininden verileri okumak için pagedResultsControl olur.
* Her üç seçenek (pagedResultsControl, VLVControl ve SortControl) seçilmemiş ise bağlayıcı büyük bir dizin ise, hangi başarısız olabilir tek bir işlemde tüm nesne alır.
* Delta içeri aktarma yöntemini USNChanged olduğunda ShowDeletedControl yalnızca kullanılır.

Değişiklik günlüğü DN, örneğin delta değişiklik günlüğü tarafından kullanılan adlandırma bağlamdır **cn = değişim günlüğü**. Delta içeri aktarma yapabilmek için bu değeri belirtilmelidir.

Varsayılan değişiklik günlüğü DNs listesi aşağıdadır:

| Dizin | Delta değişiklik günlüğü |
| --- | --- |
| Microsoft AD LDS ve AD GC |Otomatik olarak algılanır. USNChanged. |
| Apache dizin sunucusu |Mevcut değil. |
| Dizin 389 |Değişiklik günlüğü. Varsayılan değer kullanılacak: **cn = değişim günlüğü** |
| IBM Tivoli DS |Değişiklik günlüğü. Varsayılan değer kullanılacak: **cn = değişim günlüğü** |
| Isode dizini |Değişiklik günlüğü. Varsayılan değer kullanılacak: **cn = değişim günlüğü** |
| Novell/NetIQ eDirectory |Mevcut değil. Zaman damgası. Bağlayıcı kullandığı almak için tarih/saat son güncelleştirme eklenir ve kayıtlar güncelleştirildi. |
| Açık DJ/DS |Değişiklik günlüğü.  Varsayılan değer kullanılacak: **cn = değişim günlüğü** |
| Açık LDAP |Erişim günlüğü. Varsayılan değer kullanılacak: **cn accesslog =** |
| Oracle DSEE |Değişiklik günlüğü. Varsayılan değer kullanılacak: **cn = değişim günlüğü** |
| RadiantOne VDS |Sanal dizin. VDS için bağlı dizin bağlıdır. |
| Sun bir dizin sunucusu |Değişiklik günlüğü. Varsayılan değer kullanılacak: **cn = değişim günlüğü** |

Parola özniteliği bağlayıcı parola değişikliği parolayı ayarlamak için kullanması gereken öznitelik adı ve parola ayarlama işlemleri.
Bu değer ayarlanırsa varsayılan olarak olur **userPassword** ancak belirli bir LDAP sistemi için gerektiğinde değiştirilebilir.

Ek bölümlere listesinde otomatik olarak algılanan ek ad alanları Ekle mümkündür. Örneğin, tümü aynı anda içeri aktarılmalıdır mantıksal kümesi birkaç sunucuya yapmak isterseniz bu ayarı kullanılabilir. Yalnızca Active Directory birden çok etki alanı bir ormanda olabilir ancak bir şema tüm etki alanları paylaşımı gibi aynı bu kutuda ek ad alanlarını girerek benzetimi yapılabilir. Her ad alanı farklı sunuculardan alabilir ve daha fazla yapılandırma bölümleri ve hiyerarşileri sayfasında yapılandırılır. Yeni bir satır almak için Ctrl + Enter kullanın.

### <a name="configure-provisioning-hierarchy"></a>Sağlama hiyerarşisini Yapılandır
Bu sayfayı sağlanan, nesne türü örneğin kuruluş birimi DN bileşen, örneğin OU eşlemek için kullanılır.

![Hiyerarşi sağlama](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Sağlama hiyerarşisini yapılandırarak, gerektiğinde bir yapı otomatik olarak oluşturmak için bağlayıcı yapılandırabilirsiniz. Ad alanı dc ise contoso, örneğin, = dc = com ve yeni bir nesne cn = Joe, ou = Seattle, = c = US, dc = contoso, dc = com sağlandığına sonra bunlar zaten dizinde mevcut değilse bağlayıcı bir nesne türü ülke ABD için ve bir kuruluş birimi Seattle için oluşturabilirsiniz.

### <a name="configure-partitions-and-hierarchies"></a>Bölümleri ve hiyerarşileri Yapılandır
Bölümleri ve hiyerarşileri sayfasında, içeri ve dışarı aktarma planladığınız nesneleri ile tüm ad alanlarını seçin.

![Bölümler](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Her ad alanı için de bağlantı ekranda belirtilen değerleri geçersiz kılarsınız bağlantı ayarlarını yapılandırmak mümkündür. Bu değerleri kendi varsayılan boş değer olarak bırakılırsa bilgiler bağlantı ekranından kullanılır.

Hangi kapsayıcılar ve OU'lar bağlayıcı almak ve vermek seçmek mümkündür.

Bir arama yaparken bu bölümdeki tüm kapsayıcıları üzerinden gerçekleştirilir. Durumlarda bu davranış, performansın düşmesine yol açar kapsayıcıları çok sayıda olduğu.

>[!NOTE]
Genel LDAP Mart 2017 güncelleştirme başlangıç bağlayıcı aramaları yalnızca seçilen kapsayıcılara kapsamdaki sınırlı olabilir. Bu, aşağıdaki resimde gösterildiği gibi 'Aramada yalnızca seçili kapsayıcıları' onay kutusunu seçerek yapılabilir.

![Yalnızca seçili kapsayıcısında arama](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Yer işaretlerini Yapılandır
Bu sayfa, her zaman önceden yapılandırılmış bir değere sahip ve değiştirilemez. Sunucu satıcı tanımladıysanız bağlantı değişmez özniteliği, örneğin bir nesne için GUID ile doldurulabilir. Değil algılandı veya sabit bir özniteliği için bilinen bağlayıcı dn (ayırt edici adı) bağlantı kullanır.

![tutturucular](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


LDAP sunucuları ve kullanılan bağlantı listesi aşağıdadır:

| Dizin | Bağlantı özniteliği |
| --- | --- |
| Microsoft AD LDS ve AD GC |objectGUID |
| 389 dizin sunucusu |DN |
| Apache dizini |DN |
| IBM Tivoli DS |DN |
| Isode dizini |DN |
| Novell/NetIQ eDirectory |GUID |
| Açık DJ/DS |DN |
| Açık LDAP |DN |
| Oracle ODSEE |DN |
| RadiantOne VDS |DN |
| Sun bir dizin sunucusu |DN |

## <a name="other-notes"></a>Diğer Notlar
Bu bölümde, bu bağlayıcı belirli veya diğer nedenlerle bilmek önemli yönlerinden bilgi sağlar.

### <a name="delta-import"></a>Delta içeri aktarma
Açık LDAP delta filigranı UTC tarih/saat ' dir. Bu nedenle, FIM eşitleme hizmeti ve açık LDAP arasındaki saatler eşitlenmelidir. Aksi durumda, bazı girişler delta değişiklik günlüğü devre dışı bırakılacak.

Novell eDirectory için delta içeri aktarma hiçbir nesne silme algılama değil. Bu nedenle, düzenli aralıklarla tüm silinmiş nesneleri bulmak için tam içeri aktarma çalıştırmak gereklidir.

Tarih/saat üzerine temel bir delta değişiklik günlüğü ile dizinleri için tam içeri aktarma düzenli zamanlarda çalışmak üzere kullanmamanız önerilir. Bu işlem dissimilarities ve LDAP sunucusu arasında şu anda bağlayıcı alanı nedir ve bulmak için eşitleme altyapısı sağlar.

## <a name="troubleshooting"></a>Sorun giderme
* Bağlayıcı gidermek günlüğü etkinleştirme hakkında daha fazla bilgi için bkz: [bağlayıcıların ETW İzleme etkinleştirmek için nasıl](http://go.microsoft.com/fwlink/?LinkId=335731).
