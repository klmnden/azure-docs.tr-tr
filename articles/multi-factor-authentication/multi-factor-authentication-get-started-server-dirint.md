---
title: Azure Multi-Factor Authentication ile Active Directory arasında dizin tümleştirme
description: Bu, dizinleri eşitleyebilmeniz için Azure Multi-Factor Authentication Server ile Active Directory’yi nasıl tümleştireceğinizi açıklayan Azure Multi-factor authentication sayfasıdır.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtand

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban

---
# Azure MFA Sunucusu ile Active Directory arasında dizin tümleştirme
Dizin Tümleştirme bölümü, Active Directory veya başka bir LDAP dizini ile tümleştirmek üzere sunucuyu yapılandırmanızı sağlar.  Dizin şeması ile eşleşmesi için öznitelikleri yapılandırmanıza ve kullanıcıların otomatik eşitlemesini ayarlamanıza olanak tanır.

## Ayarlar
Varsayılan olarak, Azure Multi-Factor Authentication Sunucusu kullanıcıları Active Directory'den aktarmak ya da eşitlemek üzere yapılandırılmıştır.  Sekme, varsayılan davranışın üzerine yazmanızı ve farklı bir LDAP dizini, ADAM dizini ya da belirli bir Active Directory etki alanı denetçisine bağlamanızı sağlar.  Ayrıca, LDAP Kimlik Doğrulaması için kullanmak üzere RADIUS hedefi olarak LDAP ya da LDAP Bağlama sunma, IIS Kimlik Doğrulaması için önceden kimlik doğrulaması ya da Kullanıcı Portalı için birincil kimlik doğrulaması sağlar.  Aşağıdaki tabloda tek tek ayarlar açıklanır.

![Ayarlar](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Özellik | Açıklama |
| --- | --- |
| Active Directory Kullan |İçeri aktarma ve eşitleme için Active Directory’yi kullanmak üzere Active Directory Kullan seçeneğini seçin.  Bu varsayılan ayardır. <br>Not: Active Directory tümleştirmenin düzgün çalışması için bilgisayarın bir etki alanına eklenmesi ve, etki alanı hesabıyla oturum açmanız gerekir. |
| Güvenilen etki alanlarını dahil et |Aracının geçerli etki alanı tarafından güvenilen etki alanlarına, ormandaki başka bir etki alanına ya da orman güvenine dahil olan etki alanlarına bağlanmaya çalışmasını sağlamak için Güvenilen etki alanlarını dahil et onay kutusunu işaretleyin.  Kullanıcıları güvenilen etki alanlarından içeri aktarmaz ya da eşitlemezken, performansını iyileştirmek için onay kutusunun işaretini kaldırın.  Varsayılan olarak işaretlidir. |
| Özel LDAP yapılandırması kullan |İçeri aktarma ve eşitleme için belirtilen LDAP ayarlarını kullanmak üzere LDAP Kullan seçeneğini seçin. Not: LDAP Kullan seçildiğinde, kullanıcı arabirimi başvuruları Active Directory'den iken LDAP olarak değiştirir. |
| Düzenle düğmesi |Düzenle düğmesi geçerli LDAP yapılandırması ayarlarının değiştirilmesini sağlar. |
| Öznitelik kapsam sorgularını kullan |Öznitelik kapsam sorgularının kullanılıp kullanılmaması gerektiğini gösterir.  Öznitelik kapsam sorguları, kayıtları başka bir kaydın özniteliğindeki girişlere göre niteleyerek verimli dizin aramaları yapmayı sağlar.  Azure Multi-Factor Authentication Sunucusu bir güvenlik grubunun üyesi olan kullanıcıları verimli şekilde sorgulamak için öznitelik kapsam sorgularını kullanır.   <br>Not: Öznitelik kapsam sorgularının desteklendiği bazı durumlar vardır, ancak kullanılmamalıdır.  Örneğin, Active Directory bir güvenlik grubu birden fazla etki alanı üyesini içerdiğinde, öznitelik kapsam sorgularıyla sorun yaşayabilir.  Bu durumda onay kutusunun işaretlenmemiş olması gerekir. |

LDAP yapılandırması ayarları aşağıdaki tabloda açıklanmaktadır.

| Özellik | Açıklama |
| --- | --- |
| Sunucu |LDAP dizinini çalıştıran sunucunun ana bilgisayar adını veya IP adresini girin.  Noktalı virgülle ayrılarak bir yedek sunucu de belirtilebilir. <br>Not: Bağlama Türü SSL olduğunda, genellikle tam bir ana bilgisayar adı gereklidir. |
| Temel DN |Tüm dizin sorgularının başlatılacağı temel dizin nesnesinin ayırt edici adını girin.  Örneğin, dc=abc,dc=com. |
| Bağlama türü - Sorgular |LDAP dizinini aramak için bağlanırken kullanmak üzere uygun bağlama türünü seçin.  Bu, içeri aktarımlar, eşitleme ve kullanıcı adı çözümleme için kullanılır. <br><br>  Anonim - Adsız bağlantı gerçekleştirilir.  Bağlama DN’si ve Bağlama Parolası kullanılmaz.  Bu yalnızca, LDAP dizini anonim bağlamaya izin verirse ve izinler uygun kayıtların ve özniteliklerin sorgulanmasına izin verirse çalışır.  <br><br> Basit - Bağlama DN’si ve Bağlama Parolası LDAP dizinine bağlanmak için düz metin olarak geçirilir.  Bu yalnızca sunucuya erişilebildiğini ve bağlama hesabının uygun erişime sahip olduğunu doğrulama doğrultusunda test etmek amacıyla kullanılmalıdır.  Bunun yerine, SSL’nin uygun sertifika yüklendikten sonra kullanılması önerilir.  <br><br> SSL - Bağlama DN’si ve Bağlama Parolası LDAP dizinine bağlanmak için SSL kullanarak şifrelenir.  Bu, LDAP dizininin güvendiği bir sertifikanın yerel olarak yüklenmesi gerektirir.  <br><br> Windows - Bağlama Kullanıcı Adı ve Bağlama Parolası bir Active Directory etki alanı denetleyicisine veya ADAM dizinine güvenli bir şekilde bağlanmak için kullanılır.  Bağlama Kullanıcı Adı boş bırakılırsa, oturum açan kullanıcının hesabı bağlamak için kullanılır. |
| Bağlama türü - Kimlik doğrulamaları |LDAP bağlama kimlik doğrulaması gerçekleştirirken kullanmak üzere uygun bağlama türünü seçin.  Bağlama türü altındaki bağlama türü açıklamalarına bakın - Sorgular  Örneğin, bu SSL bağlama LDAP bağlama kimlik doğrulamaları için kullanılırken, sorgular için Anonim bağlama kullanılmasını sağlar. |
| Bağlama DN’si veya Bağlama kullanıcı adı |LDAP dizinine bağlanırken kullanmak üzere hesabın kullanıcı kaydı ayırt edici adını girin.<br><br>Bağlama ayırt edici adı yalnızca Bağlama Türü Basit ya da SSL olduğunda kullanılır.  <br><br>Bağlama Türü Windows olduğunda, LDAP dizinine bağlanırken kullanmak üzere Windows hesabı kullanıcı adını girin.  Boş bırakılırsa, oturum açan kullanıcının hesabı bağlamak için kullanılır. |
| Bağlama Parolası |LDAP dizinine bağlanmak için kullanılan Bind DN’si ya da kullanıcı adı için bağlama parolasını girin.  Multi-Factor Auth Sunucusu AdSync Hizmeti için parolayı yapılandırmak üzere, eşitleme etkinleştirilmeli ve hizmet yerel makinede çalışıyor olmalıdır.  Parola, Multi-Factor Auth Sunucusu AdSync Hizmeti’nin çalıştığı hesabın altında Windows’ta Depolanan Kullanıcı Adları ve Parolalar’a kaydedilir.  Parola ayrıca, Multi-Factor Auth Sunucusu kullanıcı arabiriminin çalıştığı hesap altına ve Multi-Factor Auth Sunucusu Hizmeti’nin çalıştığı hesap altına da kaydedilir.  <br><br> Not: Parola yalnızca yerel sunucunun Windows’ta Depolanan Kullanıcı Adları ve Parolalar bölümünde depolandığından, bu adım parolaya erişmesi gereken her Multi-Factor Auth Sunucusu’nda gerçekleştirilmelidir. |
| Sorgu boyutu sınırı |Bir dizin aramasının döndüreceği maksimum kullanıcı sayısı için boyut sınırını belirtin.  Bu sınır LDAP dizinindeki yapılandırmayla eşleşmelidir.  Sayfalamanın desteklenmediği büyük aramalar için, içeri aktarma ve eşitleme işlemleri kullanıcıları toplu olarak almayı dener.  Burada belirtilen boyut sınırı LDAP dizininde yapılandırılan sınırdan daha büyükse, bazı kullanıcılar eksik olabilir. |
| Test düğmesi |LDAP sunucusuna bağlanmayı test etmek için Test düğmesine tıklayın.  <br><br> Not: LDAP Kullan seçeneğinin bağlama test etmek için seçilmiş olması gerekmez.  Bu, LDAP yapılandırması kullanılmadan önce bağlamanın test edilmesini sağlar. |

## Filtreler
Filtreler, dizin araması yaparken kayıtları nitelemek üzere ölçüt belirlemenizi sağlar.  Filtre ayarlayarak, eşitlemek istediğiniz nesnelerin kapsamını belirleyebilirsiniz.  

![Filtreler](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Multi-Factor Authentication aşağıdaki 3 seçeneğe sahiptir:

* **Kapsayıcı filtresi** - Dizin araması yaparken kapsayıcı kayıtlarını nitelemek için kullanılan filtre ölçütlerini belirtin.  Active Directory ve ADAM için genellikle (|(objectClass=organizationalUnit)(objectClass=container)) kullanılır.  Diğer LDAP dizinleri için, her kapsayıcı nesnesi türünü niteleyen filtre ölçütü dizin şemasına bağlı olarak kullanılmalıdır.  <br>Not: Boş bırakılırsa varsayılan olarak ((objectClass=organizationalUnit)(objectClass=container)) kullanılır.
* **Güvenlik grubu filtresi** - Dizin araması yaparken güvenlik grubu kayıtlarını nitelemek için kullanılan filtre ölçütlerini belirtin.  Active Directory ve ADAM için genellikle (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)) kullanılır.  Diğer LDAP dizinleri için, her güvenlik grubu nesnesi türünü niteleyen filtre ölçütü dizin şemasına bağlı olarak kullanılmalıdır.  <br>Not: Boş bırakılırsa varsayılan olarak (&(objectCategory=group)(groupType:1.2.840.113556.1.4.804:=-2147483648)) kullanılır.
* **Kullanıcı filtresi** - Dizin araması yaparken kullanıcı kayıtlarını nitelemek için kullanılan filtre ölçütlerini belirtin.  Active Directory ve ADAM için genellikle (&(objectClass=user)(objectCategory=person)) kullanılır.  Diğer LDAP dizinleri için, dizin şemasına bağlı olarak (objectClass=inetOrgPerson) ya da benzeri kullanılmalıdır. <br>Not: Boş bırakılırsa varsayılan olarak (&(objectCategory=person)(objectClass=user)) kullanılır.

## Öznitelikler
Öznitelikler, belirli bir dizin için gerektiği şekilde özelleştirilebilir.  Bu, özel öznitelikler eklemenizi ve eşitlemeyi yalnızca size gereken özniteliklere ayarlamanızı sağlar.  Her öznitelik alanı değeri, dizin şemasında tanımlandığı şekilde öznitelik adı olmalıdır.  Daha fazla bilgi için aşağıdaki tabloyu kullanın.

![Öznitelikler](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Özellik | Açıklama |
| --- | --- |
| Benzersiz tanımlayıcı |Kapsayıcı, güvenlik grubu ve kullanıcı kayıtlarının benzersiz tanımlayıcısı olarak hizmet eden özniteliğin öznitelik adını girin.  Active Directory'de, bu genellikle objectGUID’dir.  Diğer LDAP uygulamalarında, entryUUID veya benzer bir şey olabilir.  ObjectGUID varsayılandır. |
| ... (Öznitelik Seç) düğmeleri |Her öznitelik alanının yanında, listeden öznitelik seçilmesini sağlayan Öznitelik Seç iletişim kutusunu görüntüleyen bir “...” düğmesi vardır. <br><br>Öznitelik Seç iletişim kutusu<br><br>Not: Öznitelikler el ile girilebilir ve öznitelik listesindeki bir öznitelikle eşleşmesi gerekli değildir. |
| Benzersiz tanımlayıcı türü |Benzersiz tanımlayıcı özniteliği türünü seçin.  Active Directory'de objectGUID özniteliğinin türü GUID’dir.  Diğer LDAP uygulamalarında, ASCII Bayt Dizisi ya da Dize türünden olabilir.  GUID varsayılandır. <br><br>Not: Eşitleme Öğelerine kendi Benzersiz Tanımlayıcısı tarafından başvurulduğundan ve Benzersiz Tanımlayıcı Türü dizindeki öğeyi doğrudan bulmak için kullanıldığından, bunu doğru şekilde ayarlama önemlidir.  Dizin değeri aslında ASCII bayt dizisi olarak depoladığında bunu Dize olarak ayarlamak eşitlemenin düzgün çalışmasını engeller. |
| Ayırt edici ad |Her bir kaydın ayırt edici adını içeren özniteliğin öznitelik adını girin.  Active Directory'de, bu genellikle distinguishedName’dir.  Diğer LDAP uygulamalarında, entryDN veya benzer bir şey olabilir.  distinguishedName varsayılandır. <br><br>Not: Yalnızca ayırt edici adı içeren bir öznitelik yoksa, adspath özniteliği kullanılabilir.  Yolun "LDAP://<server>/" kısmı, nesnenin yalnızca ayırt edici adı bırakılarak otomatik olarak çıkarılır. |
| Kapsayıcı adı |Kapsayıcı kaydındaki adı içeren özniteliğin öznitelik adını girin.  Bu özniteliğin değeri, Active Directory’den aktarılırken ya da eşitleme öğeleri eklenirken Kapsayıcı Hiyerarşisi’nde görüntülenir.  Varsayılan addır. <br><br>Not: Farklı kapsayıcılar kendi adları için farklı öznitelikler kullanıyorsa, birden çok kapsayıcı adı özniteliği noktalı virgülle ayrılarak belirtilebilir.  Bir kapsayıcı nesnesinde bulunan ilk kapsayıcı adı özniteliği, adını görüntülemek için kullanılır. |
| Güvenlik grubu adı |Güvenlik grubu kaydındaki adı içeren özniteliğin öznitelik adını girin.  Bu özniteliğin değeri, Active Directory’den aktarılırken ya da eşitleme öğeleri eklenirken Güvenlik Grubu listesinde görüntülenir.  Varsayılan addır. |
| Kullanıcılar |Aşağıdaki öznitelikler dizinde kullanıcı bilgileri aramak, görüntülemek, içeri aktarmak ve eşitlemek için kullanılır. |
| Kullanıcı adı |Kullanıcı kaydındaki kullanıcı adını içeren özniteliğin öznitelik adını girin.  Bu öznitelik değeri Multi-Factor Auth Sunucusu kullanıcı adı olarak kullanılır.  Birinciye yedek olarak ikinci bir öznitelik belirtilebilir.  İkinci öznitelik, yalnızca ilk öznitelik kullanıcı için bir değer içermiyorsa kullanılır.  userPrincipalName ve sAMAccountName varsayılanlardır. |
| Ad |Kullanıcı kaydındaki adı içeren özniteliğin öznitelik adını girin.  givenName varsayılandır. |
| Soyadı |Kullanıcı kaydındaki soyadını içeren özniteliğin öznitelik adını girin.  sn varsayılandır. |
| E-posta adresi |Kullanıcı kaydındaki e-posta adresini içeren özniteliğin öznitelik adını girin.  E-posta adresi kullanıcıya karşılama ve güncelleştirme e-postaları göndermek için kullanılır.  mail varsayılandır. |
| Kullanıcı grubu |Kullanıcı kaydındaki kullanıcı grubunu içeren özniteliğin öznitelik adını girin.  Kullanıcı grubu, aracıda ve Multi-Factor Auth Sunucusu Yönetim Portalı’ndaki raporlarında bulunan kullanıcıları filtrelemek için kullanılabilir. |
| Açıklama |Kullanıcı kaydındaki açıklamayı içeren özniteliğin öznitelik adını girin.  Açıklama yalnızca arama için kullanılır.  description varsayılandır. |
| Sesli arama dili |Kullanıcıya yönelik sesli aramalar için kullanılacak dilin kısa adını içeren özniteliğin öznitelik adını girin. |
| SMS metin dili |Kullanıcıya yönelik SMS iletileri için kullanılacak dilin kısa adını içeren özniteliğin öznitelik adını girin. |
| Telefon uygulama dili |Kullanıcıya yönelik telefon uygulama metin iletileri için kullanılacak dilin kısa adını içeren özniteliğin öznitelik adını girin. |
| OATH belirteci dili |Kullanıcıya yönelik OATH belirteci metin iletileri için kullanılacak dilin kısa adını içeren özniteliğin öznitelik adını girin. |
| Telefonlar |Aşağıdaki öznitelikler kullanıcı telefon numaralarını içeri aktarmak ya da eşitlemek için kullanılır.  Telefon türü için öznitelik adı belirtilmezse, Active Directory’den aktarırken ya da eşitleme öğeleri eklerken telefon türü kullanılmaz. |
| İş |Kullanıcı kaydındaki iş telefonu numarasını içeren özniteliğin öznitelik adını girin.  telephoneNumber varsayılandır. |
| Ev |Kullanıcı kaydındaki ev telefonu numarasını içeren özniteliğin öznitelik adını girin.  homePhone varsayılandır. |
| Çağrı cihazı |Kullanıcı kaydındaki çağrı cihazı numarasını içeren özniteliğin öznitelik adını girin.  pager varsayılandır. |
| Cep telefonu |Kullanıcı kaydındaki cep telefonu numarasını içeren özniteliğin öznitelik adını girin.  mobile varsayılandır. |
| Faks |Kullanıcı kaydındaki faks numarasını içeren özniteliğin öznitelik adını girin.  facsimileTelephoneNumber varsayılandır. |
| IP telefonu |Kullanıcı kaydındaki IP telefonu numarasını içeren özniteliğin öznitelik adını girin.  ipPhone varsayılandır. |
| Özel |Kullanıcı kaydındaki özel telefonu numarasını içeren özniteliğin öznitelik |
| adını girin.  Varsayılan boştur. | |
| Dahili numara |Kullanıcı kaydındaki telefon numarası dahili numarasını içeren özniteliğin öznitelik adını girin.  Dahili numara alanının değeri yalnızca birincil telefon numarası için dahili numara olarak kullanılır.  Varsayılan boştur. <br><br>Not: Dahili numara özniteliği belirtilmezse, dahili numaralar telefon özniteliğinin parçası olarak eklenebilir.  Ayrıştırmak için dahili numaranın önünde “x” yer almalıdır.  Örneğin 555-123-4567 x890, telefon numarası olarak 555-123-4567 ve dahili numara olarak 890’ı ifade eder. |
| Varsayılanları Geri Yükle düğmesi |Tüm öznitelikleri varsayılan değerlerine geri döndürmek için Varsayılanları Geri Yükle düğmesine tıklayın.  Varsayılanlar normal Active Directory ya da ADAM şemasıyla düzgün çalışmalıdır. |

Öznitelikleri düzenlemek için, Öznitelikler sekmesinde Düzenle düğmesine tıklamanız yeterlidir.  Bu, öznitelikleri düzenlemenize olanak sağlayan bir pencere getirir.

![Öznitelikleri Düzenleme](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## Eşitleme
Eşitleme, Azure Multi-Factor kullanıcı veritabanını Active Directory ya da başka bir Basit Dizin Erişimi Protokolü (LDAP), Basit Dizin Erişimi Protokolü dizinindeki kullanıcılarla eşitlenmiş tutar.  İşlem kullanıcıları el ile Active Directory'den içeri aktarmaya benzer, ancak işlemek üzere Active Directory kullanıcısı ve güvenlik grubu değişikliklerini düzenli aralıklarla yoklar.  Ayrıca, kapsayıcı ya da güvenlik grubundan kaldırılan kullanıcıların devre dışı bırakılmasını ya da kaldırılmasını ve Active Directory’den silinen kullanıcıların kaldırılmasını sağlar.

Multi-Factor Auth ADSync hizmeti, Active Directory’nin düzenli yoklamasını gerçekleştiren bir Windows hizmetidir.  Bu, Azure AD Sync ya da Azure AD Connect ile karıştırılmamalıdır.  Multi-Factor Auth ADSync, aynı kod tabanıyla oluşturulmakla birlikte, Azure Multi-Factor Authentication Sunucusu’na özeldir.  Bu, Durduruldu durumunda iken yüklenir ve çalışmak üzere yapılandırıldığında Multi-Factor Auth Sunucusu hizmeti tarafından başlatılır.  Çok sunuculu Multi-Factor Auth Sunucusu yapılandırmanız varsa, Multi-Factor Auth ADSync yalnızca tek bir sunucuda çalışabilir.

Multi-Factor Auth ADSync hizmeti, değişiklikleri verimli şekilde yoklamak üzere Microsoft tarafından sağlanan DirSync LDAP sunucu eklentisi kullanır.  Bu DirSync denetimi çağıranı, "directory get changes" hakkına ve DS-Replication-Get-Changes genişletilmiş denetim erişimi hakkına sahip olmalıdır.  Varsayılan olarak, bu haklar etki alanı denetleyicilerindeki Yönetici ve LocalSystem hesaplarına atanır.  Multi-Factor Auth AdSync hizmeti varsayılan olarak LocalSystem olarak çalışmak üzere yapılandırılmıştır.  Bu nedenle en basiti hizmeti bir etki alanı denetleyicisinde çalıştırmaktır.  Her zaman tam eşitleme gerçekleştirmek üzere yapılandırırsanız, hizmet daha az izne sahip bir hesap olarak çalışabilir.  Bu daha az verimlidir, ancak daha az hesap ayrıcalığı gerektirir.

LDAP kullanmak üzere yapılandırılırsa ve LDAP dizini DirSync denetimini destekliyorsa, kullanıcı ve güvenlik grubu değişiklikleri için yoklama işlemi Active Directory’de yapıldığıyla aynı şekilde işler.  LDAP dizini DirSync denetimini desteklemiyorsa, her döngü sırasında tam eşitleme gerçekleştirilir.

![Eşitleme](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Eşitleme sekmesindeki her bir ayar hakkında ek bilgi için aşağıdaki tabloyu kullanın.

| Özellik | Açıklama |
| --- | --- |
| Active Directory ile eşitlemeyi etkinleştir |İşaretlendiğinde, Multi-Factor Auth Sunucusu hizmeti düzenli olarak Active Directory değişikliklerini yoklamak için başlatılır. <br><br>Not: Multi-Factor Auth Sunucusu hizmetinin değişiklikleri işlemeye başlaması için, en az bir eşitleme öğesinin eklenmesi ve Şimdi Eşitle işlemi gerçekleştirilmesi gerekir. |
| Şu aralıkta eşitle |Multi-Factor Auth Sunucusu hizmetinin değişiklikleri yoklama ile işleme arasında bekleyeceği zaman aralığı. <br><br> Not: Belirtilen aralık her döngünün başlangıcı arasındaki süredir.  Değişiklikleri işleme zamanı aralığı aşarsa, hizmet hemen tekrar yoklama yapar. |
| Artık Active Directory'de olmayan kullanıcıları kaldır |İşaretlendiğinde, Multi-Factor Auth Sunucusu hizmeti Active Directory silinen kullanıcı küçük görüntülerini işler ve ilgili Multi-Factor Auth Sunucusu kullanıcısını kaldırır. |
| Her zaman tam eşitleme gerçekleştir |İşaretlendiğinde, Multi-Factor Auth Sunucusu hizmeti her zaman tam eşitleme gerçekleştirir.  İşaretlenmediğinde, Multi-Factor Auth Sunucusu hizmeti yalnızca değiştirilmiş kullanıcılar sorgulayarak artımlı eşitleme gerçekleştirir.  Varsayılan olarak işaretli değildir. <br><br> Not: İşaretlenmediğinde, artımlı eşitleme yalnızca dizin DirSync denetimini desteklediğinde ve dizine bağlanmak için kullanılan hesap DirSync artımlı sorguları gerçekleştirmek için uygun izinlere sahip olduğunda gerçekleştirilebilir.  Hesap uygun izinlere sahip değilse veya eşitlemeye birden çok etki alanı dahilse, tam eşitleme gerçekleştirilmesi önerilir. |
| X değerinden fazla kullanıcı devre dışı bırakıldığında ya da kaldırıldığında yönetici onayı iste |Eşitleme öğeleri, artık öğenin kapsayıcısı ya da güvenlik grubunun artık üyesi olmayan kullanıcıları devre dışı bırakacak veya kaldıracak şekilde yapılandırılabilir.  Önlem olarak, devre dışı bırakılacak veya silinecek kullanıcı sayısı eşiği aştığında, yönetici onayı gerekli olabilir.  İşaretlendiğinde, belirtilen eşik için onay gerekli olacaktır.  Varsayılan değer 5’tir ve aralık 1 - 999’dur. <br><br> Onay, ilk olarak yöneticilere bir e-posta bildirimi gönderilmesiyle sağlanır. E-posta bildirimi, kullanıcıları devre dışı bırakma ve kaldırmayı gözden geçirme ve onaylama için yönergeler sağlar.  Multi-Factor Auth Sunucusu kullanıcı arabirimi başlatıldığında, onay ister. |

**Şimdi Eşitle** düğmesi belirtilen eşitleme öğeleri için tam eşitleme çalıştırmanıza olanak sağlar.  Eşitleme öğeleri eklendiğinde, değiştirildiğinde, kaldırıldığında ve yeniden sıralandığında her seferde tam eşitleme gereklidir.  Ayrıca, artımlı değişiklikler için hizmetin yoklama yapacağı başlangıç noktasını ayarladığından, Multi-Factor Auth AdSync hizmeti çalışır duruma geçmeden önce de gereklidir.  Eşitleme öğelerinde değişiklik yapıldıysa ve bir tam eşitleme gerçekleştirilmediyse, başka bir bölüme geçerken ya da kullanıcı arabirimini kapatırken sizden Şimdi Eşitleme yapmanız istenir.

**Kaldır** düğmesi yöneticinin bir ya da daha fazla eşitleme öğesini Multi-Factor Auth Sunucusu eşitleme öğesi listesinden silmesini sağlar.

> [!WARNING]
> Eşitleme öğesi kaydı kaldırıldıktan sonra kurtarılamaz. Yanlışlıkla sildiyseniz, eşitleme öğesi kaydını yeniden eklemeniz gerekir.
> 
> 

Eşitleme öğesi veya eşitleme öğeleri Multi-Factor Auth Sunucusu’ndan kaldırıldı.  Multi-Factor Auth Sunucusu artık eşitleme öğelerini işlemeyecek.

Yukarı Taşı ve Aşağı Taşı düğmeleri yöneticinin eşitleme öğelerinin sırasını değiştirmesini sağlar.  Aynı kullanıcı birden fazla eşitleme öğesinin üyesi olabileceğinden (örneğin, kapsayıcı ve güvenlik grubu), sıra önemlidir.  Kullanıcıya eşitleme sırasında uygulanan ayarlar, kullanıcının ilişkilendirildiği, listedeki ilk eşitleme öğesinden gelir.  Bu nedenle, eşitleme öğeleri öncelik sırasına koyulmalıdır.

> [!TIP]
> Eşitleme öğeleri kaldırıldıktan sonra bir tam eşitleme gerçekleştirilmelidir.  Eşitleme öğeleri sıralandıktan sonra bir tam eşitleme gerçekleştirilmelidir.  Tam eşitleme gerçekleştirmek için Şimdi Eşitle düğmesine tıklayın.
> 
> 

## Multi-Factor Auth Sunucuları
Yedek RADIUS proxy, LDAP proxy olarak hizmet etmesi ya da IIS Kimlik Doğrulaması için ek Multi-Factor Auth Sunucuları kurulabilir. Eşitleme yapılandırması tüm aracılar arasında paylaşılır. Ancak, bu aracılardan yalnızca birinde Multi-Factor Auth Sunucusu hizmeti çalışıyor olabilir. Bu sekme, eşitleme için etkinleştirilmesi gereken Multi-Factor Auth Sunucusu’nu seçmenizi sağlar.

![Multi-Factor Auth Sunucuları](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)

<!--HONumber=Sep16_HO3-->


