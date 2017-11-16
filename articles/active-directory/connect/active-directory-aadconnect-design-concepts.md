---
title: "Azure AD Connect: Tasarım kavramları | Microsoft Docs"
description: "Bu konuda belirli uygulama tasarım alanları ayrıntıları"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 53a0f766de9db7e6ee48b6659aad378620c0d727
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Tasarım kavramları
Azure AD Connect uygulama tasarımı sırasında zorlayıcı alanları tanımlamak için bu konunun amacı budur. Bu konuda derinlemesine belirli alanlara ise ve bu kavramları diğer konularında kısaca açıklanmıştır.

## <a name="sourceanchor"></a>sourceAnchor
SourceAnchor özniteliği olarak tanımlanır *bir nesne ömrü boyunca sabit bir öznitelik*. Ayrıca aynı nesne şirket içi olarak ve Azure AD'de bir nesneyi benzersiz olarak tanımlar. Öznitelik olarak da adlandırılır **İmmutableıd** ve iki ad birbirinin yerine kullanılır.

Sözcüğünü değişmez, "değiştirilemez", bu konuya önemlidir. Bu özniteliğin değeri ayarlandıktan sonra değiştirilemez, senaryonuza destekleyen bir tasarım seçmek önemlidir.

Özniteliği aşağıdaki senaryolar için kullanılır:

* Yeni bir eşitleme altyapısı sunucusu yerleşik veya bir olağanüstü durum kurtarma senaryosunda sonra yeniden, bu öznitelik varolan nesneleri nesneleri şirket içi Azure AD'de bağlar.
* Eşitlenmiş kimlik modeli için bir yalnızca bulut kimlik taşırsanız, ardından bu özniteliği "sabit match" var olan nesneleri nesnelere Azure AD ile şirket içi nesneleri sağlar.
* Federasyon ve ardından bu özniteliği ile birlikte kullanıyorsanız, **userPrincipalName** talep kümesinde bir kullanıcıyı benzersiz şekilde tanımlamak için kullanılır.

Kullanıcılara ilgili olarak bu konu hakkında sourceAnchor yalnızca alınmaktadır. Tüm nesne türleri için aynı kurallar geçerlidir, ancak bunu yalnızca kullanıcılar için bu sorun genellikle bir konudur.

### <a name="selecting-a-good-sourceanchor-attribute"></a>İyi sourceAnchor özniteliği seçme
Öznitelik değeri aşağıdaki kurallara uymalıdır:

* Daha az 60 karakter uzunluğunda olabilir
  * Karakter olmaması, a-z, A-Z veya 0-9 kodlanmış ve 3 karakter olarak sayılır
* Bir özel karakter içeremez: &#92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Genel olarak benzersiz olmalıdır
* Bir dize, tamsayı veya ikili olmalıdır
* Kullanıcı adı, bu değişiklikleri dayanmalıdır değil
* Değil büyük küçük harfe duyarlı ve örneğe göre farklılık gösterebilir değerleri kaçının
* Nesne oluşturulduğunda atanmalıdır

Seçili sourceAnchor dize türünde değilse, Azure AD Connect Base64Encode hiçbir özel karakter emin olmak için öznitelik değeri görüntülenir. ADFS değerinden başka bir federasyon sunucusu kullanıyorsanız Base64Encode öznitelik edebilir sunucunuz emin olun.

SourceAnchor özniteliği büyük/küçük harf duyarlıdır. "JohnDoe" değerini "johndoe" ile aynı değil. Ancak, iki farklı nesneler arasındaki fark durumda yalnızca sahip olmamalıdır.

Tek bir ormana sahibim şirket içi, daha sonra kullanmanız gereken özniteliği ise **objectGUID**. Ayrıca Azure AD Connect'i hızlı ayarları kullandığınızda kullanılan öznitelik ve aynı zamanda DirSync tarafından kullanılan öznitelik budur.

Birden çok ormanınız varsa ve kullanıcı etki alanları ve ormanlar arasında sonra taşımayın **objectGUID** bile bu durumda kullanmak için iyi bir özniteliktir.

Kullanıcıların etki alanları ve ormanlar arasında taşırsanız, taşıma işlemi sırasında kullanıcılarla değişmeyen veya taşınabilir bir özniteliği bulmalıdır. Önerilen yaklaşım yapay bir öznitelik uygulamaktır. Bir GUID gibi görünen bir şey tutan bir öznitelik uygun olacaktır. Nesne oluşturma sırasında yeni bir GUID oluşturulur ve kullanıcı damgalı. Özel eşitleme kuralı göre bu değer oluşturmak için eşitleme altyapısı sunucusu oluşturulabilir **objectGUID** ve EKLER seçili özniteliğinde güncelleştirin. Nesneyi taşıdığınızda, bu değer içeriğini de kopyaladığınızdan emin olun.

Değişmez bildiğiniz varolan bir özniteliğe çekme başka bir çözümdür. Yaygın olarak kullanılan öznitelikler içerir **EmployeeID**. Harfleri içeren bir öznitelik düşünüyorsanız, hiçbir fırsat (küçük harf ve büyük harflerle) çalışması için özniteliğinin değeri değiştirebilirsiniz olduğundan emin olun. Kullanılmaması gereken hatalı öznitelikler özniteliklerle kullanıcı adını içerir. Marriage veya divorce adı değiştirmek için bu öznitelik için izin verilmiyor bekleniyor. Bu ayrıca nedenlerinden biri neden olduğu gibi öznitelikleri **userPrincipalName**, **posta**, ve **targetAddress** bile Azure AD Connect Yükleme Sihirbazı'nda seçmek mümkün değildir. Bu öznitelikler de içeren "@" sourceAnchor izin verilmeyen bir karakter.

### <a name="changing-the-sourceanchor-attribute"></a>SourceAnchor özniteliği değiştirme
SourceAnchor özniteliği değeri, nesnenin Azure AD'de oluşturulan ve kimliğini eşitlenmiş sonra değiştirilemez.

Bu nedenle, Azure AD Connect'e aşağıdaki kısıtlamalar geçerlidir:

* SourceAnchor özniteliği yalnızca ilk yükleme sırasında ayarlanabilir. Yükleme Sihirbazı'nı yeniden çalıştırın, bu seçenek salt okunurdur. Ardından bu ayarı değiştirmek gerekirse, yeniden yükleyin ve gerekir kaldırın.
* Başka bir Azure AD Connect sunucusu yüklerseniz, daha önce kullanılan aynı sourceAnchor özniteliği seçmeniz gerekir. Daha önce DirSync kullanarak ve Azure AD Connect'e taşıma durumunda kullanmalısınız **objectGUID** , DirSync tarafından kullanılan öznitelik olduğundan.
* SourceAnchor için değer sonra değiştirilirse Azure ad, ardından Azure AD Connect eşitleme bir hata oluşturur ve daha fazla değişiklik üzerinde sorun çözüldükten önce nesne ve sourceAnchor değiştirildiğinde, kaynak dizininde izin vermiyor nesnesi verildi.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>MsDS-ConsistencyGuid sourceAnchor kullanma
Varsayılan olarak, Azure AD Connect (sürüm 1.1.486.0 ve daha eski) objectGUID sourceAnchor özniteliği olarak kullanır. Sistem tarafından oluşturulan objectguıd'dir. Değerini oluştururken AD nesnelerini şirket içi belirtemezsiniz. Bölümünde açıklandığı gibi [sourceAnchor](#sourceanchor), sourceAnchor değeri belirtmek için gereken olduğu senaryo vardır. Senaryolar için uygun olup olmadığını sourceAnchor özniteliği olarak yapılandırılabilir bir AD özniteliği (örneğin, msDS-ConsistencyGuid) kullanmanız gerekir.

Azure AD Connect (sürüm 1.1.524.0 ve sonra) şimdi sourceAnchor özniteliği olarak msDS-ConsistencyGuid kullanımını kolaylaştırır. Bu özelliği kullanıldığında, Azure AD Connect eşitleme kuralları otomatik olarak yapılandırır:

1. MsDS-ConsistencyGuid sourceAnchor özniteliği olarak kullanıcı nesneleri için kullanın. ObjectGUID diğer nesne türleri için kullanılır.

2. Verilen herhangi için şirket içi AD kullanıcı nesnesi, msDS-ConsistencyGuid özniteliği msDS-ConsistencyGuid özniteliği şirket içi Active Directory'de objectGUID değerini geri doldurulan, Azure AD Connect yazma değildir. MsDS-ConsistencyGuid özniteliği doldurulduktan sonra Azure AD Connect sonra Azure AD nesne dışarı aktarır.

>[!NOTE]
> Bir kez bir şirket içi AD nesne (yani AD bağlayıcı alanına içeri aktarılıp, meta veri deposuna öngörülen) Azure AD Connect içine aktarılır, sourceAnchor değerini artık değiştiremezsiniz. SourceAnchor değeri belirtmek için bir şirket içi verilen AD nesne, Azure AD Connect içeri aktarılmadan önce kendi msDS-ConsistencyGuid özniteliği yapılandırın.

### <a name="permission-required"></a>İzin gerekiyor
Bu özelliğin çalışması için şirket içi Active Directory ile eşitlemek için kullanılan AD DS hesap şirket içi Active Directory'de msDS-ConsistencyGuid öznitelik yazma izni verilmesi gerekir.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>ConsistencyGuid - yeni yükleme nasıl etkinleştirileceğini
Yeni yükleme sırasında sourceAnchor olarak ConsistencyGuid kullanımını etkinleştirebilirsiniz. Bu bölümde, hızlı ve özel yükleme ayrıntıları ele alınmaktadır.

  >[!NOTE]
  > Azure AD Connect yalnızca yeni sürümlerini (1.1.524.0 ve sonra) yeni yüklemesi sırasında sourceAnchor olarak ConsistencyGuid kullanımını destekler.

### <a name="how-to-enable-the-consistencyguid-feature"></a>ConsistencyGuid özelliğini etkinleştirme
Şu anda bu özellik yalnızca yeni Azure AD Connect yüklemesi sırasında yalnızca etkinleştirilebilir.

#### <a name="express-installation"></a>Hızlı yükleme
Azure AD Connect ile hızlı mod yüklerken, Azure AD Connect Sihirbazı otomatik olarak aşağıdaki mantık kullanılarak sourceAnchor özniteliği olarak kullanmak için en uygun AD özniteliği belirler:

* İlk olarak, Azure AD Connect Sihirbazı'nı (varsa) önceki Azure AD Connect yüklemesi sourceAnchor özniteliği olarak kullanılan AD öznitelik almak için Azure AD kiracınıza sorgular. Bu bilgiler varsa, Azure AD Connect aynı AD özniteliğini kullanır.

  >[!NOTE]
  > Azure AD Connect yalnızca yeni sürümlerini (1.1.524.0 ve sonra) sourceAnchor özniteliği hakkında bilgileri depolar Azure AD kiracınızda yükleme sırasında kullanılır. Azure AD Connect eski sürümlerinde yoktur.

* Kullanılan sourceAnchor özniteliği hakkında bilgi kullanılabilir değilse, sihirbaz, şirket içi Active Directory'de msDS-ConsistencyGuid öznitelik durumunu denetler. Öznitelik dizinde herhangi bir nesne üzerinde yapılandırılmazsa sihirbaz msDS-ConsistencyGuid sourceAnchor özniteliği olarak kullanır. Öznitelik dizininde bir veya daha fazla nesnelerde yapılandırılmışsa, sihirbazın öznitelik diğer uygulamalar tarafından kullanılıyor ve sourceAnchor özniteliği olarak uygun değil sonucuna...

* Bu durumda sihirbazın geri sourceAnchor özniteliği olarak objectGUID kullanmaya döner.

* SourceAnchor özniteliği karar sonra sihirbaz, Azure AD kiracınızda bilgileri depolar. Bilgileri, gelecekteki Azure AD Connect yüklemesi tarafından kullanılır.

Hızlı yükleme işlemi tamamlandıktan sonra Sihirbazı hangi öznitelik olarak kaynak bağlantısı özniteliği çekilen bildirir.

![SourceAnchor için çekildi AD özniteliği Sihirbazı sizi bilgilendirir](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Özel yükleme
Azure AD Connect özel moduyla yüklerken, Azure AD Connect Sihirbazı sourceAnchor özniteliği yapılandırırken iki seçenek sunar:

![Özel yükleme - sourceAnchor yapılandırma](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Ayar | Açıklama |
| --- | --- |
| Kaynak bağlantısını benim için Azure yönetsin | Azure AD’nin sizin için özniteliği seçmesini istiyorsanız bu seçeneği belirleyin. Bu seçeneği belirlerseniz, Azure AD Connect Sihirbazı aynı durum geçerlidir [sourceAnchor özniteliği seçme mantığı Express yüklemesi sırasında kullanılan](#express-installation). Express yüklemesine benzer, sihirbazın, özel yükleme tamamlandıktan sonra kaynak bağlantısı özniteliği olarak hangi öznitelik çekilen bildirir. |
| Belirli bir öznitelik | SourceAnchor özniteliği olarak mevcut bir AD özniteliğini belirtmek istiyorsanız bu seçeneği belirleyin. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>ConsistencyGuid - var olan dağıtım nasıl etkinleştirileceğini
Kaynak bağlantısı özniteliği olarak objectGUID kullanan mevcut bir Azure AD Connect dağıtımınız varsa, bunun yerine ConsistencyGuid kullanmaya geçiş yapabilirsiniz.

>[!NOTE]
> Azure AD Connect yalnızca yeni sürümlerini (1.1.552.0 ve sonra) kaynak bağlantısı özniteliği olarak ConsistencyGuid objectGUID geçiş destekler.

ObjectGUID ConsistencyGuid için kaynak bağlantısı özniteliği olarak değiştirmek için:

1. Azure AD Connect Sihirbazı'nı başlatın ve tıklayın **yapılandırma** görevleri ekranına gidin.

2. Seçin **yapılandırma kaynak bağlantısı** görev seçeneğini ve tıklayın **sonraki**.

   ![Var olan dağıtım - adım 2 ConsistencyGuid etkinleştir](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Azure AD yönetici kimlik bilgilerinizi girin ve tıklayın **sonraki**.

4. Azure AD Connect Sihirbazı şirket içi Active Directory'de msDS-ConsistencyGuid öznitelik durumunu inceler. Öznitelik, dizinde herhangi bir nesne üzerinde yapılandırılmamışsa, Azure AD Connect'i başka bir uygulama öznitelik kullanmakta olduğu ve kaynak bağlantısı özniteliği olarak kullanmak güvenlidir sonlanır. Tıklatın **sonraki** devam etmek için.

   ![Var olan dağıtım - 4. adım ConsistencyGuid etkinleştir](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. İçinde **yapılandırma için hazır** ekranında **yapılandırma** yapılandırma değişikliği yapma.

   ![Var olan dağıtım - 5. adım ConsistencyGuid etkinleştir](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. Yapılandırma tamamlandıktan sonra sihirbazın bu msDS-ConsistencyGuid artık kaynak bağlantısı özniteliği olarak kullanılan gösterir.

   ![Var olan dağıtım - adım 6 ConsistencyGuid etkinleştir](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Öznitelik dizininde bir veya daha fazla nesnelerde yapılandırılmışsa (4. adım) Çözümleme sırasında sihirbazın özniteliği başka bir uygulama tarafından kullanılıyor ve aşağıdaki çizimde gösterildiği gibi bir hata döndürür sonlanır. Daha önce ConsistencyGuid özelliğini etkinleştirdiyseniz, bu hata ayrıca oluşabilir Birincil Azure AD Connect'in üzerinde sunucu ve hazırlama sunucunuz aynı çalışıyorsunuz.

![Var olan dağıtım - hata ConsistencyGuid etkinleştir](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Öznitelik varolan diğer uygulamalar tarafından kullanılmadığından eminseniz, Azure AD Connect Sihirbazı yeniden başlatarak hata gizleyebilirsiniz **/SkipLdapSearchcontact** belirtilen. Bunu yapmak için komut isteminde aşağıdaki komutu çalıştırın:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>AD FS veya üçüncü taraf Federasyon yapılandırma üzerinde etkisi
Azure AD Connect kullanıyorsanız yönetmek için AD FS dağıtımı şirket içi, Azure AD Connect sourceAnchor aynı AD özniteliğini kullanmak için talep kuralları otomatik olarak güncelleştirir. Bu, ADFS tarafından oluşturulan İmmutableıd talep Azure AD'ye aktarılan sourceAnchor değerleri ile tutarlı olmasını sağlar.

AD FS Azure AD Connect dışında yönettiğiniz veya üçüncü taraf federasyon sunucuları için kimlik doğrulaması kullanıyorsanız, makale bölümde açıklandığı gibi Azure AD'ye aktarılan sourceAnchor değerleri ile tutarlı olacak şekilde İmmutableıd talep için talep kurallarını el ile güncelleştirmeniz gerekir [Değiştir AD FS talep kuralları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). Yükleme tamamlandıktan sonra Sihirbazı'nı aşağıdaki uyarıyı döndürür:

![Üçüncü taraf Federasyon yapılandırma](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Yeni dizinler için var olan dağıtım ekleme
ConsistencyGuid özelliği etkinleştirilmiş Azure AD Connect dağıttıysanız ve şimdi başka bir dizin dağıtımına eklemek istediğiniz varsayalım. Dizin eklemeyi denediğinizde, Azure AD Connect Sihirbazı directory mSDS-ConsistencyGuid özniteliğinde durumunu denetler. Öznitelik dizininde bir veya daha fazla nesnelerde yapılandırılmışsa sihirbaz öznitelik diğer uygulamalar tarafından kullanılıyor ve aşağıdaki çizimde gösterildiği gibi bir hata döndürür sonlanır. Öznitelik mevcut uygulamalar tarafından kullanılmadığından eminseniz, hatayı bastırmak hakkında bilgi için desteğe başvurun gerekir.

![Yeni dizinler için var olan dağıtım ekleme](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD'de oturum açma
Şirket içi dizininizi Azure AD ile tümleştirme sırasında önemlidir şekilde kullanıcı eşitleme ayarlarını nasıl etkileyebileceğini anlaması için kimliğini doğrular. Azure AD, kullanıcının kimliğini doğrulamak için userPrincipalName (UPN) kullanır. Ancak, kullanıcılarınızın eşitlediğinizde, userPrincipalName değeri için dikkatle kullanılacak öznitelik seçmeniz gerekir.

### <a name="choosing-the-attribute-for-userprincipalname"></a>UserPrincipalName özniteliği seçme
Sağlamak için öznitelik seçerken Azure birinde kullanılacak UPN değerini sağlamalısınız

* Biçimi olmalıdır UPN sözdizimi (RFC 822) öznitelik değerlerini uygunusername@domain
* Değerleri soneki Azure AD'de doğrulanmış özel etki alanlarını birine eşleşen

Hızlı Ayarları'nda, varsayılan özniteliğinin userPrincipalName seçimdir. UserPrincipalName özniteliğinin değeri içermiyorsa, kullanıcılarınızın Azure oturumu açın istediğiniz sonra seçmeniz gerekir **özel yükleme**.

### <a name="custom-domain-state-and-upn"></a>Özel etki alanı durumu ve UPN
UPN soneki doğrulanmış bir etki alanı olduğundan emin olmak önemlidir.

John, contoso.com ormanındaki bir kullanıcıdır. Şirket içi UPN kullanmak için John istediğiniz john@contoso.com kullanıcılar, Azure AD directory contoso.onmicrosoft.com eşitlendiğinden sonra Azure'da oturum açmak için. Bunu yapmak için ekleme ve contoso.com özel bir etki alanı kullanıcıları eşitlemeye başlamadan önce Azure AD'de doğrulama gerekir. John, örneğin contoso.com, UPN sonekinin Azure AD'de doğrulanmış bir etki alanı eşleşmiyorsa, Azure AD ile contoso.onmicrosoft.com UPN soneki değiştirir.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Yönlendirilebilir olmayan şirket içi etki alanları ve Azure AD için UPN
Bazı kuruluşlar contoso.local veya contoso gibi basit tek etiketli etki alanları gibi yönlendirilebilir olmayan etki alanları sahiptir. Azure AD'de yönlendirilemeyen bir etki alanını doğrulamak mümkün değildir. Azure AD Connect yalnızca bir doğrulanmış etki alanı için Azure AD'ye eşitleyebilirsiniz. Azure AD dizini oluşturduğunuzda, örneğin, contoso.onmicrosoft.com Azure AD için varsayılan etki alanı haline gelir yönlendirilebilir bir etki alanı oluşturur. Bu nedenle, diğer yönlendirilebilir etki alanında böyle bir senaryo için varsayılan onmicrosoft.com etki alanı eşitleme istemediğiniz durumda doğrulamak gerekli olur.

Okuma [özel etki alanı adınızı Azure Active Directory'ye ekleme](../active-directory-domains-add-azure-portal.md) ekleme ve etki alanı doğrulama hakkında daha fazla bilgi için.

Azure AD Connect yönlendirilebilir olmayan etki alanı ortamında çalıştırıyorsanız ve uygun şekilde express ayarlarla devam giderek uyar algılar. Yönlendirilemeyen bir etki alanında çalışıyorsanız, ardından bu kullanıcıların UPN yönlendirilemeyen sonekleri çok olabilir. Örneğin, contoso.local altında çalıştırıyorsanız, Azure AD Connect hızlı ayarları kullanarak yerine özel ayarlar kullanmanızı önerir. Özel ayarlarını kullanarak, kullanıcıların Azure AD ile eşitlenir sonra Azure'da oturum açmak için UPN kullanılması gereken özniteliği belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
