---
title: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme | Microsoft Docs
description: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: celested
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6fe74852824c10d24729f785e5e33a17b793161
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411343"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme

Bugün, çoklu oturum açma (SSO) ile Azure AD uygulama galerisinde yanı sıra özel uygulamalar önceden tümleştirilmiş iki uygulama da dahil olmak üzere çoğu kuruluş uygulamaları Azure Active Directory (Azure AD) destekler. Bir kullanıcı, uygulamanın SAML 2.0 protokolünü kullanarak Azure AD üzerinden kimliğini doğrular, Azure AD belirteç (bir HTTP POST) aracılığıyla uygulamaya gönderir. Ve daha sonra uygulama doğrular ve belirteci yerine bir kullanıcı adı ve parola bilgilerini isteyen kullanıcının oturumunu açmak için kullanır. Bu SAML belirteçleri olarak bilinen kullanıcı ile ilgili bilgiler içeren *talep*.

A *talep* bildiren bir kimlik sağlayıcısı, sorunu bu kullanıcı için belirteç içinde bir kullanıcı hakkında bilgi. İçinde [SAML belirteci](https://en.wikipedia.org/wiki/SAML_2.0), bu veriler genellikle SAML özniteliği deyimde yer alır. Kullanıcının benzersiz kimliği genellikle SAML ad tanımlayıcısı da bilinen konu temsil edilir.

Varsayılan olarak, Azure AD SAML belirteci içeren uygulamanıza sorunları bir `NameIdentifier` kullanıcının benzersiz şekilde tanımlayabilir, Azure AD'de kullanıcının kullanıcı adını (diğer adıyla kullanıcı asıl adı) değerini talep. SAML belirtecindeki Ayrıca, kullanıcının e-posta adresi, ad ve soyadını içeren ek talepleri de içerir.

Görüntülemek veya uygulamaya SAML belirtecinde verilen talepleri düzenlemek için Azure portalında uygulama açın. Açılacağını **kullanıcı öznitelikleri ve talepler** bölümü.

![Kullanıcı öznitelikleri ve talepler bölümü](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir neden iki olası nedeni vardır:

* Uygulama gerektiriyorsa `NameIdentifier` veya Azure AD'de depolanan kullanıcı adı (veya kullanıcı asıl adı) dışında bir şey olduğu iddia edilen Nameıd.
* Uygulamayı farklı bir URI'leri talep kümesi gerektirir veya talep değerleri hedefine yazıldı.

## <a name="editing-nameid"></a>Nameıd düzenleme

(Ad tanımlayıcı değeri) Nameıd düzenlemek için:

1. Açık **ad tanımlayıcı değeri** sayfası.
1. Öznitelik veya dönüştürme için bir öznitelik uygulamak istediğiniz seçin. İsteğe bağlı olarak istediğiniz kendisi için Nameıd talebi biçimi belirtebilirsiniz.

   ![Nameıd (ad tanımlayıcısı) değerini Düzenle](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>Nameıd biçimi

Ardından SAML isteğinde belirli bir biçime sahip öğeyi NameIDPolicy içeriyorsa, Azure AD talep biçiminde dokunmaz.

SAML isteğini NameIDPolicy için bir öğe içermiyorsa, Azure AD, belirttiğiniz biçimde Nameıd verecek. Hiçbir biçimi belirttiyseniz, Azure AD seçili talep kaynağı ile ilişkili varsayılan kaynak biçimi kullanır.

Gelen **belirleyin adı tanımlayıcı biçimi** açılır listesinde, aşağıdaki seçeneklerden birini seçebilirsiniz.

| Nameıd biçimi | Açıklama |
|---------------|-------------|
| **Varsayılan** | Azure AD, varsayılan kaynak biçimini kullanır. |
| **Kalıcı** | Azure AD kalıcı Nameıd biçimi kullanır. |
| **EmailAddress** | Azure AD EmailAddress Nameıd biçimi kullanır. |
| **Belirtilmemiş** | Azure AD belirtilmemiş Nameıd biçimi kullanır. |
| **Geçici** | Azure AD, geçici Nameıd biçimi kullanır. |

NameIDPolicy özniteliği hakkında daha fazla bilgi için bkz. [tek oturum açma SAML Protokolü](single-sign-on-saml-protocol.md).

### <a name="attributes"></a>Öznitelikler

İstenen kaynağı seçin `NameIdentifier` (veya Nameıd) talep. Aşağıdaki seçenekler arasından seçim.

| Ad | Açıklama |
|------|-------------|
| Email | Kullanıcının e-posta adresi |
| userprincipalName | Kullanıcının kullanıcı asıl adı (UPN) |
| onpremisessamaccount | Şirket içi ad'nizden Azure AD'ye eşitlenen SAM hesabı adı |
| Nesne Kimliği | Azure AD'de kullanıcının nesne kimliği |
| EmployeeID | EmployeeID kullanıcının |
| Dizin genişletmeleri | Dizin genişletmeleri [şirket içi Azure AD Connect Sync kullanarak Active Directory eşitlendi](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| 1-15 uzantı öznitelikleri | Şirket içinde Azure AD'ye şemayı genişletmek için kullanılan uzantı öznitelikleri |

Daha fazla bilgi için bkz. [Tablo 3: Kaynak başına geçerli kimlik değerleri](active-directory-claims-mapping.md#table-3-valid-id-values-per-source).

### <a name="special-claims---transformations"></a>Özel talepler - dönüşümleri

Talep dönüştürmeleri işlevleri de kullanabilirsiniz.

| İşlev | Açıklama |
|----------|-------------|
| **ExtractMailPrefix()** | Etki alanı soneki, e-posta adresi veya kullanıcı asıl adını kaldırır. Bu aracılığıyla geçirilen kullanıcı adı, yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" yerine joe_smith@contoso.com). |
| **Join()** | Bir öznitelik ile doğrulanmış bir etki alanına katılır. Seçili kullanıcı tanıtıcı değeri bir etki alanı varsa, seçili doğrulanmış etki alanına eklemek için kullanıcı adı ayıklayacaksınız. Örneğin, e-posta seçerseniz (joe_smith@contoso.com) kullanıcı tanıtıcı değeri ve doğrulanmış etki alanı olarak select contoso.onmicrosoft.com olarak bu sonuçlanır joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Seçilen özniteliğin karakterleri, küçük harf karakterlere dönüştürür. |
| **ToUpper()** | Seçilen özniteliğin karakterleri büyük harf karakterlere dönüştürür. |

## <a name="adding-application-specific-claims"></a>Uygulamaya özgü talep ekleme

Uygulamaya özgü talep eklemek için:

1. İçinde **kullanıcı öznitelikleri ve talepler**seçin **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** sayfası.
1. Girin **adı** talepler. Değer kesinlikle SAML spec başına bir URI düzeni izleyin gerekmez. Bir URI düzeni ihtiyacınız varsa, bu koyabilirsiniz **Namespace** alan.
1. Seçin **kaynak** talep nerede bulunacağını değerini almak için. Talep olarak yayma önce kullanıcı özniteliği için bir dönüştürme uygulamak ya da kaynak öznitelik açılan listeden bir kullanıcı özniteliğini seçin.

### <a name="application-specific-claims---transformations"></a>Uygulamaya özgü talep - dönüşümleri

Talep dönüştürmeleri işlevleri de kullanabilirsiniz.

| İşlev | Açıklama |
|----------|-------------|
| **ExtractMailPrefix()** | Etki alanı soneki, e-posta adresi veya kullanıcı asıl adını kaldırır. Bu aracılığıyla geçirilen kullanıcı adı, yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" yerine joe_smith@contoso.com). |
| **Join()** | İki öznitelik birleştirerek yeni bir değer oluşturur. İsteğe bağlı olarak, iki öznitelikleri arasında bir ayırıcı kullanabilirsiniz. |
| **ToLower()** | Seçilen özniteliğin karakterleri, küçük harf karakterlere dönüştürür. |
| **ToUpper()** | Seçilen özniteliğin karakterleri büyük harf karakterlere dönüştürür. |
| **Contains()** | Giriş belirtilen değerle eşleşiyorsa, öznitelik veya sabiti çıkarır. Aksi takdirde, eşleşme yoksa, başka bir çıkış belirtebilirsiniz.<br/>Örneğin, bir talep değeri olduğu kullanıcının e-posta adresi etki alanını içeriyorsa yayma istiyorsanız "@contoso.com", kullanıcı asıl adı çıktısını almak istediğiniz Aksi takdirde. Bunu yapmak için aşağıdaki değerleri yapılandırırsınız:<br/>*Parametre 1(input)*: user.email<br/>*Değer*: "@contoso.com"<br/>Parametre 2 (çıkış): user.email<br/>Parametre 3 (eşleşme yoksa çıkış): user.userprincipalname |
| **EndWith()** | Belirtilen değerle giriş sona ererse, öznitelik veya sabiti çıkarır. Aksi takdirde, eşleşme yoksa, başka bir çıkış belirtebilirsiniz.<br/>EmployeeID "000" ile bitiyorsa değerin kullanıcının EmployeeID olduğu bir talep yayma istiyorsanız, örneğin, aksi takdirde uzantısı özniteliği çıkış istediğiniz. Bunu yapmak için aşağıdaki değerleri yapılandırırsınız:<br/>*Parametre 1(input)*: user.employeeid<br/>*Değer*: "000"<br/>Parametre 2 (çıkış): user.employeeid<br/>Parametre 3 (eşleşme yoksa çıkış): user.extensionattribute1 |
| **StartWith()** | Giriş belirtilen değerle başlayıp başlamadığını öznitelik veya sabiti çıkarır. Aksi takdirde, eşleşme yoksa, başka bir çıkış belirtebilirsiniz.<br/>Ülke "ABD" ile başlıyorsa değerin kullanıcının EmployeeID olduğu bir talep yayma istiyorsanız, örneğin, aksi takdirde uzantısı özniteliği çıkış istediğiniz. Bunu yapmak için aşağıdaki değerleri yapılandırırsınız:<br/>*Parametre 1(input)*: Resource.country<br/>*Değer*: "BİZE"<br/>Parametre 2 (çıkış): user.employeeid<br/>Parametre 3 (eşleşme yoksa çıkış): user.extensionattribute1 |
| **Extract() - After matching** | Belirtilen değerle eşleşen sonra alt dizeyi döndürür.<br/>Örneğin, girdinin değer "Finance_BSimon" ise, eşleşen değeri olan "Finance_" sonra "BSimon" talebin çıkışı yapılır. |
| **Extract() - Before matching** | Belirtilen değerle eşleşen kadar alt dizeyi döndürür.<br/>Örneğin, girdinin değer "BSimon_US" ise, eşleşen değeri olan "_US" sonra "BSimon" talebin çıkışı yapılır. |
| **Extract() - Between matching** | Belirtilen değerle eşleşen kadar alt dizeyi döndürür.<br/>Örneğin, girdinin değer "Finance_BSimon_US" ise, ilk eşleşen değeri olan "Finance_", ikinci eşleşen değeri olan "_US" ve ardından "BSimon" talebin çıkışı yapılır. |
| **ExtractAlpha() - Prefix** | Dize öneki alfabetik bölümünü döndürür.<br/>Girdinin değer "BSimon_123" ise, örneğin, ardından "BSimon" döndürür. |
| **ExtractAlpha() - soneki** | Dize soneki alfabetik bölümünü döndürür.<br/>Girdinin değer "123_Simon" ise, örneğin, ardından "BSimon" döndürür. |
| **ExtractNumeric() - Prefix** | Dize öneki sayısal bölümü döndürür.<br/>Girdinin değer "123_BSimon" ise, örneğin, ardından "123" döndürür. |
| **ExtractNumeric() - Suffix** | Dizesinin soneki sayısal bölümü döndürür.<br/>Girdinin değer "BSimon_123" ise, örneğin, ardından "123" döndürür. |
| **IfEmpty()** | Giriş null veya boş ise, öznitelik veya sabiti çıkarır.<br/>Örneğin, EmployeeID belirli bir kullanıcı için boş ise bir extensionattribute içinde depolanan bir öznitelik çıkış istiyorsanız. Bunu yapmak için aşağıdaki değerleri yapılandırırsınız:<br/>Parametre 1(input): user.employeeid<br/>Parametre 2 (çıkış): user.extensionattribute1<br/>Parametre 3 (eşleşme yoksa çıkış): user.employeeid |
| **IfNotEmpty()** | Giriş null veya boş değilse, öznitelik veya sabiti çıkarır.<br/>Örneğin, EmployeeID belirli bir kullanıcı için boş değilse bir extensionattribute içinde depolanan bir öznitelik çıkış istiyorsanız. Bunu yapmak için aşağıdaki değerleri yapılandırırsınız:<br/>Parametre 1(input): user.employeeid<br/>Parametre 2 (çıkış): user.extensionattribute1 |

Ek dönüşümleri gerekiyorsa, içinde fikrinizi gönderdiğinizde [Azure AD'de geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599) altında *SaaS uygulaması* kategorisi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Azure AD uygulama galerisinde bulunmayan uygulamalar çoklu oturum açmayı yapılandırın](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](howto-v1-debug-saml-sso-issues.md)
