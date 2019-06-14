---
title: 'Azure AD Connect: Tasarım kavramları | Microsoft Docs'
description: Belirli uygulama tasarım alanları bu konuda ayrıntıları
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 311ba489073805fdb034b435ab9e5e1ddc2c4e3c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60382309"
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Tasarım kavramları
Bu belgenin amacı, Azure AD Connect uygulama tasarım sırasında düşündüğünüz alanlarını açıklayan sağlamaktır. Bu belge belirli alanları ayrıntılı bir bakış ve diğer belgelerde Bu kavramlar kısaca açıklanmaktadır.

## <a name="sourceanchor"></a>sourceAnchor
SourceAnchor özniteliği olarak tanımlı *nesne yaşam süresi boyunca sabit olan bir öznitelik*. Bu, bir nesne olarak aynı nesne şirket içi ve Azure AD'de benzersiz olarak tanımlar. Öznitelik olarak da adlandırılır **Immutableıd** ve iki ad birbirinin yerine kullanılır.

Sözcüğü değişmezse, olan "değiştirilemez", bu belge için önemlidir. Bu özniteliğin değeri, ayarlandıktan sonra değiştirilemez olduğundan, senaryonuz destekleyen bir tasarım seçmek önemlidir.

Öznitelik aşağıdaki senaryolar için kullanılır:

* Yeni bir eşitleme altyapısı sunucusu yerleşik veya bir olağanüstü durum kurtarma senaryosuna sonra yeniden, bu öznitelik mevcut nesneleri nesnelerini şirket içi ile Azure AD'de bağlar.
* Bir eşitleme kimlik modeli için bir yalnızca bulut kimlikten taşırsanız, bu öznitelik şirket içi nesneleri ile Azure AD'de nesneleri "sabit match" varolan nesnelere sağlar.
* Federasyon ve ardından bu öznitelik ile birlikte kullanıyorsanız **userPrincipalName** talepte bir kullanıcıyı benzersiz şekilde tanımlamak için kullanılır.

Bu konu yalnızca sourceAnchor hakkında konuşuyor ve kendisinden kullanıcılara bağlantılı olarak. Tüm nesne türleri için aynı kuralları geçerlidir, ancak bu sorun genellikle bir konudur yalnızca kullanıcılara yöneliktir.

### <a name="selecting-a-good-sourceanchor-attribute"></a>İyi sourceAnchor özniteliği seçme
Öznitelik değeri, şu kurallara uymalıdır:

* 60'dan az karakter uzunluğunda
  * Karakter olmaması a-z, A-Z veya 0-9 kodlanmış ve 3 karakter sayılır
* Bir özel karakter içermemelidir: &#92; ! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " \@ _
* Genel olarak benzersiz olmalıdır
* Bir dize, tamsayı veya ikili olmalıdır
* Değiştirebilirsiniz çünkü kullanıcının adına dayalı
* Değil büyük/küçük harfe ve büyük küçük harfle değişebilir değerler kaçının
* Nesne oluşturulduğunda atanmalıdır

Seçili sourceAnchor türü dizesi değilse, Azure AD Connect Base64Encode özel karakterler olmadığından emin olmak için öznitelik değeri görüntülenir. ADFS değerinden başka bir federasyon sunucusu kullanıyorsanız, sunucunuzun Base64Encode öznitelik ayrıca emin olun.

SourceAnchor özniteliği, büyük/küçük harf duyarlıdır. "JohnDoe" değerini "johndoe" ile aynı değil. Ancak, yalnızca bir servis talebi fark ile iki farklı nesne olmamalıdır.

Şirket içi, ardından kullanması gereken öznitelik ise tek bir ormana sahibim **objectGUID**. Ayrıca Azure AD Connect'i hızlı ayarları kullandığınızda kullanılan öznitelik ve aynı zamanda DirSync tarafından kullanılan öznitelik budur.

Birden çok ormanınız ve kullanıcıların ormanları ve etki alanları arasında ardından taşımayın **objectGUID** bile bu durumda kullanmak iyi bir özniteliktir.

Kullanıcılar ormanları ve etki alanları arasında taşırsanız, taşıma işlemi sırasında kullanıcılarla değişmez veya taşınabilir bir özniteliği bulmalıdır. Yapay bir öznitelik tanıtmak önerilen bir yaklaşımdır. Bir GUID gibi görünen bir şey tutabilen bir özniteliği uygun olacaktır. Nesne oluşturma sırasında yeni bir GUID oluşturulur ve kullanıcıya damgası. Eşitleme altyapısı sunucusuna bağlı bu değeri oluşturmak için özel bir eşitleme kuralı oluşturulabilir **objectGUID** ve seçili özniteliğinde EKLER güncelleştirin. Nesne taşıdığınızda, ayrıca bu değer içeriğini kopyalayın emin olun.

Çekme değiştiremezsiniz tanıdığınız varolan bir özniteliği başka bir çözümdür. Yaygın olarak kullanılan öznitelikler içerir **EmployeeID**. Harf içeren bir öznitelik göz önünde bulundurun, yoktur (büyük harf ve küçük harf) çalışması konusunda hiç olasılık özniteliğin değerini değiştirebilirsiniz emin olun. Kullanılmamalıdır hatalı öznitelikleri özniteliklerle kullanıcı adını içerir. Marriage veya divorce adını değiştirmek için bu öznitelik için izin verilmiyor bekleniyor. Bu da bir nedeni budur gibi öznitelikleri **userPrincipalName**, **posta**, ve **targetAddress** Azure AD Connect yükleme seçmek bile mümkün olmayan Sihirbaz. Bu öznitelikleri de içeren "\@" sourceAnchor içinde izin verilmeyen bir karakter.

### <a name="changing-the-sourceanchor-attribute"></a>SourceAnchor özniteliği değiştirme
SourceAnchor özniteliği değeri kimlik eşleştirilir ve Azure AD'de oluşturulduktan sonra değiştirilemez.

Bu nedenle, Azure AD Connect'e aşağıdaki kısıtlamalar uygulanır:

* SourceAnchor özniteliği, yalnızca ilk yükleme sırasında ayarlanabilir. Bu seçenek, Yükleme Sihirbazı'nı yeniden çalıştırın, salt okunur. Ardından bu ayarı değiştirmeniz gerekirse, yeniden yükleyin ve gerekir kaldırın.
* Başka bir Azure AD Connect sunucusu yüklerseniz, daha önce kullanılan aynı sourceAnchor özniteliği seçmeniz gerekir. Daha önce DirSync kullanarak ve Azure AD Connect'e taşıma durumunda kullanmanız gerekir **objectGUID** , DirSync tarafından kullanılan öznitelik olduğundan.
* Azure ad, ardından Azure AD Connect'i eşitleme bir hata oluşturur ve başka herhangi bir değişiklik üzerinde sorun düzeltilmiştir önce nesne ve sourceAnchor değiştirildiğinde, Kaynak Yöneticisi yeniden izin vermiyor sourceAnchor değeri sonra değiştirilirse nesnesi verildi y.

## <a name="using-ms-ds-consistencyguid-as-sourceanchor"></a>MS-DS-Consistencyguid'i sourceAnchor olarak kullanma
Varsayılan olarak, Azure AD Connect (sürüm 1.1.486.0 yaşındaki) sourceAnchor özniteliği olarak objectGUID kullanır. Sistem tarafından oluşturulan objectguıd'dir. Değerini oluştururken şirket AD nesnelerini belirtemezsiniz. Bölümünde açıklandığı gibi [sourceAnchor](#sourceanchor), sourceAnchor belirtmeniz gereken senaryolar da vardır. Senaryoları için geçerli olan yapılandırılabilir bir AD özniteliğini kullanmanız gerekir (örneğin, ms-DS-ConsistencyGuid) sourceAnchor özniteliği olarak.

Azure AD Connect'i (sürüm 1.1.524.0 ve sonra) artık sourceAnchor özniteliği olarak ms-DS-ConsistencyGuid kullanımını kolaylaştırır. Bu özelliği kullanırken, Azure AD Connect eşitleme kuralları otomatik olarak yapılandırır:

1. MS-DS-ConsistencyGuid sourceAnchor özniteliği olarak kullanıcı nesneleri için kullanın. ObjectGUID diğer nesne türleri için kullanılır.

2. Verilen herhangi için şirket içi AD kullanıcı nesnesi, ms-DS-ConsistencyGuid özniteliği değil doldurulmuş, Azure AD Connect yazma ms-DS-ConsistencyGuid özniteliği şirket içi Active Directory'de, objectGUID değere döner. Ms-DS-ConsistencyGuid özniteliği doldurulduktan sonra Azure AD Connect nesneyi ardından Azure AD'ye verir.

>[!NOTE]
> Bir kez bir şirket içi AD nesnesi (yani AD bağlayıcı alanına içeri aktarılıp, meta veri deposuna öngörülen) Azure AD Connect içine alınır, artık sourceAnchor değerine değiştirilemiyor. SourceAnchor değeri belirtmek için bir şirket içi verilen AD nesne, Azure AD Connect içeri aktarılmadan önce ms-DS-ConsistencyGuid özniteliği yapılandırın.

### <a name="permission-required"></a>İzin gerekiyor
Bu özelliğin çalışması için şirket içi Active Directory ile eşitlemek için kullanılan AD DS hesabı şirket içi Active Directory'de ms-DS-ConsistencyGuid özniteliği yazma izni verilmesi gerekir.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>Yeni yükleme - consistencyguid içinde özellik etkinleştirme
Yeni yükleme sırasında consistencyguid içinde kullanımını sourceAnchor olarak etkinleştirebilirsiniz. Bu bölüm, Express hem de özel yükleme ayrıntıları kapsar.

  >[!NOTE]
  > Azure AD Connect yalnızca yeni sürümlerini (1.1.524.0 ve sonra) yeni bir yükleme sırasında sourceAnchor olarak consistencyguid içinde kullanımını destekler.

### <a name="how-to-enable-the-consistencyguid-feature"></a>Consistencyguid içinde özelliğini etkinleştirme
Şu anda bu özellik yalnızca yeni Azure AD Connect yüklemesi sırasında yalnızca etkinleştirilebilir.

#### <a name="express-installation"></a>Hızlı yükleme
Azure AD Connect ile hızlı mod yüklerken, Azure AD Connect Sihirbazı otomatik olarak aşağıdaki mantık kullanılarak sourceAnchor özniteliği olarak kullanmak için en uygun AD özniteliği belirler:

* İlk olarak, Azure AD Connect Sihirbazı'nı (varsa) önceki Azure AD Connect yüklemesi sourceAnchor özniteliği olarak kullanılan AD özniteliği almak için Azure AD kiracınıza sorgular. Bu bilgiler varsa, Azure AD Connect aynı AD özniteliğini kullanır.

  >[!NOTE]
  > Azure AD Connect yalnızca yeni sürümlerini (1.1.524.0 ve sonra) sourceAnchor özniteliği hakkında bilgi depolamak Azure AD kiracınızda yüklemesi sırasında kullanılır. Azure AD Connect'in eski sürümlerinde yoktur.

* Sihirbaz, kullanılan sourceAnchor özniteliği hakkında bilgi kullanılabilir değilse, şirket içi Active directory'nizde ms-DS-ConsistencyGuid özniteliği durumunu denetler. Öznitelik dizinindeki herhangi bir nesne üzerinde yapılandırılmazsa sihirbaz ms-DS-ConsistencyGuid sourceAnchor özniteliği olarak kullanır. Öznitelik, dizinde veya daha fazla nesne yapılandırılmışsa, sihirbaz öznitelik diğer uygulamalar tarafından kullanılıyor ve sourceAnchor özniteliği olarak uygun değil sonucuna...

* Bu durumda, sihirbaz geri sourceAnchor özniteliği olarak objectGUID kullanmaya döner.

* SourceAnchor özniteliği karar sonra sihirbaz, Azure AD kiracınızda bilgileri depolar. Bilgiler, ileride Azure AD Connect yüklemesi tarafından kullanılır.

Hızlı yükleme tamamlandıktan sonra sihirbaz kaynak bağlayıcı özniteliği olarak hangi özniteliğin seçildiğini size bildirir.

![AD özniteliği için sourceAnchor çekilen Sihirbazı bildirir](./media/plan-connect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Özel yükleme
Azure AD Connect özel moduyla yüklerken, Azure AD Connect Sihirbazı sourceAnchor özniteliği yapılandırırken iki seçenek sunar:

![Özel yükleme - sourceAnchor yapılandırma](./media/plan-connect-design-concepts/consistencyGuid-02.png)

| Ayar | Açıklama |
| --- | --- |
| Kaynak bağlantısını benim için Azure yönetsin | Azure AD’nin sizin için özniteliği seçmesini istiyorsanız bu seçeneği belirleyin. Bu seçeneği belirlerseniz Azure AD Connect Sihirbazı aynısı geçerlidir [Express yüklemesi sırasında kullanılan sourceAnchor özniteliği seçim mantığını](#express-installation). Express yüklemesine benzer, sihirbaz, özel yükleme tamamlandıktan sonra kaynak bağlayıcı özniteliği olarak hangi özniteliğin seçildiğini size bildirir. |
| Belirli bir öznitelik | SourceAnchor özniteliği olarak mevcut bir AD özniteliğini belirtmek istiyorsanız bu seçeneği belirleyin. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>-Mevcut dağıtım consistencyguid içinde özelliğini etkinleştirme
Kaynak bağlantısı özniteliği olarak objectGUID kullanan mevcut bir Azure AD Connect dağıtımı varsa, bunun yerine consistencyguid içinde geçiş yapabilirsiniz.

>[!NOTE]
> Azure AD Connect yalnızca yeni sürümlerini (1.1.552.0 ve sonrası) consistencyguid içinde kaynak bağlayıcı özniteliği olarak objectGUID geçiş desteği.

ObjectGUID consistencyguid içinde için kaynak bağlantısı özniteliği olarak değiştirmek için:

1. Azure AD Connect Sihirbazı'nı başlatır ve **yapılandırma** görevleri ekranına gitmek için.

2. Seçin **kaynak bağlantısını yapılandır** görev seçeneğini ve tıklayın **sonraki**.

   ![Var olan dağıtım - 2. adım consistencyguid içinde etkinleştir](./media/plan-connect-design-concepts/consistencyguidexistingdeployment01.png)

3. Azure AD yönetici kimlik bilgilerinizi girin ve tıklayın **sonraki**.

4. Azure AD Connect Sihirbazı, şirket içi Active directory'nizde ms-DS-ConsistencyGuid özniteliği durumunu analiz eder. Öznitelik dizinindeki herhangi bir nesne üzerinde yapılandırılmamışsa, Azure AD Connect'i başka bir uygulama özniteliğini kullanıyor ve kaynak bağlayıcı özniteliği olarak kullanmak üzere güvenli olup burada sona eriyor. Tıklayın **sonraki** devam etmek için.

   ![Var olan dağıtım - 4. adım consistencyguid içinde etkinleştir](./media/plan-connect-design-concepts/consistencyguidexistingdeployment02.png)

5. İçinde **yapılandırmaya hazır** ekranında **yapılandırma** yapılandırma değişikliği yapma.

   ![Var olan dağıtım - 5. adım consistencyguid içinde etkinleştir](./media/plan-connect-design-concepts/consistencyguidexistingdeployment03.png)

6. Yapılandırma tamamlandıktan sonra ms-DS-ConsistencyGuid artık kaynak bağlayıcı özniteliği olarak kullanılan sihirbaz gösterir.

   ![Var olan dağıtım - 6. adım consistencyguid içinde etkinleştir](./media/plan-connect-design-concepts/consistencyguidexistingdeployment04.png)

Öznitelik veya daha fazla nesne dizinde yapılandırılmışsa (4. adım) analiz sırasında sihirbaz özniteliği başka bir uygulama tarafından kullanılır ve aşağıdaki diyagramda gösterildiği gibi bir hata döndürür sona eriyor. Bu hata consistencyguid içinde özelliği daha önce etkinleştirdiyseniz de oluşabilir birincil, Azure AD Connect'i hazırlama sunucunuzdaki aynı yapmaya çalışıyorsunuz Server.

![Var olan dağıtım - hata consistencyguid içinde etkinleştir](./media/plan-connect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Öznitelik mevcut diğer uygulamalar tarafından kullanılmadığından eminseniz, Azure AD Connect Sihirbazı yeniden başlatarak hata gizleyebilirsiniz **/SkipLdapSearch** belirtilen anahtar. Bunu yapmak için komut isteminde aşağıdaki komutu çalıştırın:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>AD FS'den veya üçüncü taraf Federasyon yapılandırması üzerindeki etki
Azure AD Connect kullanıyorsanız yönetmek için AD FS dağıtımı şirket, Azure AD Connect otomatik olarak aynı AD özniteliği sourceAnchor kullanmak için talep kurallarını güncelleştirir. Bu, AD FS tarafından oluşturulan Immutableıd talebi Azure AD'ye aktarılan sourceAnchor değerlerle tutarlı olmasını sağlar.

AD FS'yi Azure AD Connect dışında yönetiyorsanız veya üçüncü taraf federasyon sunucuları için kimlik doğrulaması kullanıyorsanız, açıklandığı gibi Azure AD'ye aktarılan sourceAnchor değerlerle tutarlı olması Immutableıd talebi için talep kurallarını el ile güncelleştirmeniz gerekir makale bölümünde [Değiştir AD FS talep kuralları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). Yükleme tamamlandıktan sonra sihirbaz, aşağıdaki uyarıyı döndürür:

![Üçüncü taraf Federasyon yapılandırması](./media/plan-connect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Yeni dizinler için mevcut dağıtım ekleme
Azure AD Connect consistencyguid içinde özelliği etkin dağıttığınız ve artık dağıtıma başka bir dizin eklemek istediğiniz varsayalım. Azure AD Connect Sihirbazı, dizine eklemeyi denediğinizde, ms-DS-ConsistencyGuid özniteliği dizinde durumunu denetler. Öznitelik, dizinde veya daha fazla nesne yapılandırılmışsa, sihirbaz öznitelik diğer uygulamalar tarafından kullanılır ve aşağıdaki diyagramda gösterildiği gibi bir hata döndürür sona eriyor. Öznitelik mevcut uygulamalar tarafından kullanılmadığından eminseniz, Azure AD Connect Sihirbazı yeniden başlatarak hata gizleyebilirsiniz **/SkipLdapSearch** yukarıda açıklandığı gibi belirtilen bir anahtar veya bağlantı gerekir Daha fazla bilgi için destek.

![Yeni dizinler için mevcut dağıtım ekleme](./media/plan-connect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD oturum açma
Şirket içi dizininizi Azure AD ile tümleştirme sırasında önemli olduğu şekilde kullanıcı eşitleme ayarlarını nasıl etkileyebileceğini anlaması için kimlik doğrulaması yapar. Azure AD userPrincipalName (UPN) kullanıcının kimliğini doğrulamak için kullanır. Bununla birlikte, kullanıcılarınızın eşitlediğinizde, dikkatli bir şekilde userPrincipalName değeri için kullanılacak özniteliği seçmeniz gerekir.

### <a name="choosing-the-attribute-for-userprincipalname"></a>UserPrincipalName özniteliği seçme
Azure birinde kullanılacak UPN değeri sağlamak için öznitelik seçerken emin olun

* Öznitelik değerleri, UPN sözdizimi (RFC 822) uygun biçimde kullanıcı adı olmalıdır\@etki alanı
* Azure AD'de doğrulanmış özel etki alanlarından biri için değerleri soneki eşleşir

Express ayarlarında userPrincipalName özniteliği için varsayılan seçim olan. UserPrincipalName özniteliği değeri içermiyorsa, Azure'da oturum açma, kullanıcılarınızın istediğiniz, sonra da seçmeniz gerekir **özel yükleme**.

### <a name="custom-domain-state-and-upn"></a>Özel etki alanı durumu ve UPN
UPN soneki doğrulanmış bir etki alanı olduğundan emin olmak önemlidir.

John, contoso.com içindeki bir kullanıcıdır. John, şirket içi UPN john kullanmak istediğiniz\@contoso.com için Azure AD directory contoso.onmicrosoft.com kullanıcılar eşitlendikten sonra Azure'da oturum açmak için. Bunu yapmak için ekleme ve contoso.com, kullanıcıları eşitlemeye başlamadan önce Azure AD'de özel etki alanı doğrulama gerekir. John, örneğin contoso.com, UPN sonekini Azure AD'de doğrulanmış bir etki alanı ile eşleşmiyorsa Azure AD ile contoso.onmicrosoft.com UPN soneki değiştirir.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Yönlendirilebilir olmayan şirket içi etki alanları ve Azure AD için UPN
Bazı kuruluşların contoso.local veya contoso gibi basit bir tek etiketli etki alanları gibi yönlendirilemeyen etki alanları vardır. Azure AD'de yönlendirilemeyen bir etki alanı doğrulayamadı değildir. Azure AD Connect yalnızca doğrulanmış bir etki alanı için Azure AD'ye eşitleyebilirsiniz. Azure AD dizini oluşturduğunuzda, varsayılan etki alanı contoso.onmicrosoft.com gibi Azure AD için duruma yönlendirilebilir bir etki alanı oluşturur. Bu nedenle, varsayılan onmicrosoft.com etki alanı için eşitlemeyi istemediğiniz durumlarda diğer yönlendirilebilir bir etki alanında böyle bir senaryo doğrulamak gerekli hale gelir.

Okuma [Azure Active Directory'ye özel etki alanı adınızı ekleme](../active-directory-domains-add-azure-portal.md) ekleme ve etki alanlarını doğrulama hakkında daha fazla bilgi için.

Azure AD Connect yönlendirilemeyen etki alanı ortamında çalıştırıyorsanız ve hızlı ayarlar ile devam yapmalarını uygun şekilde uyar algılar. Yönlendirilemeyen bir etki alanında çalışıyorsanız, ardından bunu, kullanıcıların UPN yönlendirilemeyen sonekleri çok olabilir. Örneğin, contoso.local altında çalıştırıyorsanız, Azure AD Connect hızlı ayarları kullanarak yerine özel ayarlar kullanmanızı önerir. Özel ayarlarını kullanarak UPN Azure AD'ye eşitlenmiş kullanıcıları sonra Azure'da oturum açmak için kullanılması gereken özniteliği belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
