---
title: "Azure AD Connect: Bildirim temelli hazırlama ifadelerini | Microsoft Docs"
description: "Bildirim temelli hazırlama ifadelerini açıklanmaktadır."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 83fe949468a67318c766f0070498c35300af4deb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect eşitleme: bildirim temelli hazırlama ifadeleri anlama
Azure AD Connect eşitleme ilk Forefront Identity Manager 2010'da sunulan bildirim temelli hazırlama üzerinde oluşturur. Derlenmiş kod yazmak zorunda kalmadan, tam kimlik tümleştirme iş mantığı uygulamanız imkan tanır.

Öznitelik akışlarında kullanılan ifade dili bildirim temelli hazırlama, önemli bir parçasıdır. Kullanılan dili, Microsoft® Visual Basic® for Applications (VBA) bir alt kümesidir. Bu dil Microsoft Office'de kullanılır ve VBScript deneyimi olan kullanıcılar da onu tanımaz. Bildirim temelli hazırlama ifade dili yalnızca işlevler kullanılarak ve yapılandırılmış bir dil değil. Yöntemleri veya deyimleri yok. Express program akışı işlevleri yerine iç içe.

Daha fazla ayrıntı için bkz: ['na Hoş Geldiniz Office 2013 için dil başvurusu uygulamalar için Visual Basic](https://msdn.microsoft.com/library/gg264383.aspx).

Öznitelikleri kesin türü belirtilmiş. Bir işlev yalnızca doğru türde öznitelikleri kabul eder. Ayrıca, büyük küçük harfe duyarlı değildir. Bir hata oluşturulur veya işlev adları ve öznitelik adları doğru büyük/küçük harf olmalıdır.

## <a name="language-definitions-and-identifiers"></a>Dil tanımları ve tanımlayıcıları
* İşlev bağımsız değişkenleri köşeli adından sonra sahip: FunctionName (1 bağımsız değişkeni, değişken N).
* Öznitelikleri köşeli tarafından tanımlanır: [attributeName]
* Parametreleri yüzde işaretleri tarafından tanımlanır: % ParameterName %
* Dize sabitleri tırnak içine alınmış: Örneğin, "Contoso" (Not: düz tırnak işaretleri kullanmanız gerekir "" tırnak akıllı değil ve "")
* Sayısal değerler tırnak işaretleri olmadan ifade ve ondalık olması bekleniyor. Onaltılık değerler öneki ile & H. Örneğin, 98052 & HFF
* Boole değerleri sabitler ile ifade edilir: True, False.
* Yerleşik sabit ve değişmez değerleri yalnızca kendi adı ile ifade edilir: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>İşlevler
Bildirim temelli hazırlama, öznitelik değerleri dönüştürme etme olanağını etkinleştirmek için birçok işlevini kullanır. Bir işlevin sonuç başka bir işleve geçirilen şekilde bu işlevleri iç içe.

`Function1(Function2(Function3()))`

İşlevlerin tam listesi bulunabilir [işlev başvurusu](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametreler
Bir parametre bir bağlayıcı veya PowerShell kullanan bir yönetici tarafından tanımlanır. Parametreler genellikle sistem için farklı değerler içeren, örneğin, kullanıcı etki alanı adı bulunur. Bu parametreler öznitelik akışları kullanılabilir.

Active Directory Bağlayıcısı gelen eşitleme kuralları için aşağıdaki parametreleri sağlanan:

| Parametre Adı | Yorum |
| --- | --- |
| Domain.Netbios |Şu anda alınmakta, örneğin FABRIKAMSALES etki alanının NetBIOS biçimi |
| Domain.FQDN |Şu anda içeri aktarılmakta olan, etki alanı örneğin sales.fabrikam.com FQDN biçimi |
| Domain.LDAP |Şu anda içeri aktarılmakta olan, etki alanı örneğin DC LDAP biçimi Satışlar, DC = fabrikam, DC = com = |
| Forest.Netbios |NetBIOS adının biçimi şu anda alınmakta orman örneğin FABRIKAMCORP |
| Forest.FQDN |Şu anda alınmakta orman adının örneğin fabrikam.com FQDN biçimi |
| Forest.LDAP |Şu anda alınmakta orman adının örneğin DC LDAP biçimi fabrikam, DC = com = |

Sistem şu anda çalışan bağlayıcı tanıtıcısı almak için kullanılan aşağıdaki parametre sağlar:  
`Connector.ID`

Aşağıda, kullanıcının bulunduğu etki alanının NetBIOS adı ile meta veri deposu özniteliği etki alanı dolduran bir örnek verilmiştir:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>İşleçler
Aşağıdaki işleçleri kullanılabilir:

* **Karşılaştırma**: <, < =, <>, =, >, > =
* **Matematik**: +, -, \*, -
* **Dize**: & (Birleştir)
* **Mantıksal**: & & (ve) || (veya)
* **Değerlendirme sırası**:)

İşleçler soldan sağa değerlendirilir ve aynı değerlendirme önceliğe sahip. Diğer bir deyişle, \* (çarpanı) (önce - çıkarma) değerlendirilmez. 2\*(5 + 3) 2 ile aynı değil\*5 + 3. Köşeli ayraçlar () sağ değerlendirme sırası sola uygun olmadığı durumlarda Değerlendirme sırasını değiştirmek için kullanılır.

## <a name="multi-valued-attributes"></a>Birden çok değerli öznitelikleri
İşlevler, hem tek değerli ve birden çok değerli öznitelikleri üzerinde çalışabilir. Birden çok değerli öznitelikler, işlevi her değer çalışır ve her değere aynı işlevi uygular.

Örneğin:  
`Trim([proxyAddresses])`Kırpma proxyAddress özniteliğinde her değerin yapın.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Her değere sahip bir @-sign, etki alanı ile Değiştir @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`SIP-adresini bakın ve değerleri kaldırın.

## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Bkz: nasıl bildirim temelli hazırlama kullanılan out-of-box içinde olduğu [varsayılan yapılandırmayı anlama](active-directory-aadconnectsync-understanding-default-configuration.md).
* Bkz. bildirim temelli sağlamayı kullanarak pratik değişiklik yapmak [varsayılan yapılandırması ile değişiklik yapmak nasıl](active-directory-aadconnectsync-change-the-configuration.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

**Başvuru konuları**

* [Azure AD Connect eşitleme: işlevleri başvurusu](active-directory-aadconnectsync-functions-reference.md)

