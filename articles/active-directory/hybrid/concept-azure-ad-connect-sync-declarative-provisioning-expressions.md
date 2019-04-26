---
title: 'Azure AD Connect: Bildirim temelli sağlama ifadeleri | Microsoft Docs'
description: Bildirim temelli sağlama ifadelerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/18/2017
ms.date: 11/08/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: cdc7c9dba49bf37db1f039d43b0450c65884c74b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60245497"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect eşitleme: Bildirim Temelli Sağlama İfadelerini Anlama
Azure AD Connect eşitleme ilk Forefront Identity Manager 2010'da tanıtılan bildirim temelli sağlama üzerinde oluşturur. Tam kimlik tümleştirme iş mantığınızı derlenmiş kod yazmak zorunda kalmadan olanak tanır.

Öznitelik akışlarında kullanılan bir ifade dili bildirim temelli sağlama, önemli bir parçasıdır. Kullanılan dil, Microsoft® Visual Basic® Applications (VBA) için bir alt kümesidir. VBScript deneyimi olan kullanıcılar da bunu algılar ve bu dil Microsoft Office'de kullanılır. Bildirim temelli sağlama ifade dili işlevleri yalnızca kullanıyor ve yapılandırılmış bir dil değil. Deyimleri veya yöntemler yoktur. Bunun yerine işlevler için hızlı program akışı iç içe geçirilmiştir.

Daha fazla ayrıntı için [Hoş Geldiniz Office 2013 için dil başvurusu uygulamaları için Visual Basic](https://msdn.microsoft.com/library/gg264383.aspx).

Öznitelikleri kesin türlü yapılır. Bir işlev yalnızca doğru türde öznitelikleri kabul eder. Büyük küçük harfe duyarlıdır. İşlev adları ve öznitelik adları hem doğru büyük/küçük harf olmalıdır veya bir hata oluşturulur.

## <a name="language-definitions-and-identifiers"></a>Dil tanımları ve tanımlayıcıları
- İşlevler, bağımsız değişkenleri köşeli ayraçlar içindeki arkasından bir ada sahip: FunctionName (1 bağımsız değişken, bağımsız değişken N).
- Öznitelik, köşeli ayraç tanımlanır: [attributeName]
- Parametreleri yüzde işaretleri tarafından tanımlanır: % ParameterName %
- Dize sabitleri, tırnak işareti içine: Örneğin, "Contoso" (Not: düz tırnak kullanmanız gerekir "" ve akıllı tırnaklar değil "")
- Sayısal değerleri tırnak işaretleri olmadan ifade ve ondalık olması bekleniyor. Onaltılık değerler önekiyle & h Örneğin, 98052 & HFF
- Boole değerleri sabitleriyle gösterilir: TRUE, False.
- Yerleşik sabitlerini ve sabit değerleri yalnızca adla belirtilir: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>İşlevler
Bildirim temelli sağlama, öznitelik değerleri dönüştürme etme olanağını etkinleştirmek için birçok işlevlerini kullanır. Bu işlevler bir işlev sonucu başka bir işleve geçirilen şekilde yuvalanabilir.

`Function1(Function2(Function3()))`

İşlevlerin tam listesi bulunabilir [işlev başvurusu](reference-connect-sync-functions-reference.md).

### <a name="parameters"></a>Parametreler
Bir parametre, bir bağlayıcı veya PowerShell kullanarak bir yönetici tarafından tanımlanır. Parametreleri genellikle sistem için farklı değerlere sahip, etki alanı kullanıcı adı, örneğin bulunur. Bu parametreleri öznitelik akışları kullanılabilir.

Active Directory Bağlayıcısı gelen eşitleme kuralları için aşağıdaki parametreleri sağlanan:

| Parametre Adı | Açıklama |
| --- | --- |
| Domain.Netbios |Şu anda içeri aktarılan örnek FABRIKAMSALES etki alanının NetBIOS biçimi |
| Domain.FQDN |Şu anda içeri aktarılan örnek sales.fabrikam.com etki alanının FQDN'si biçimi |
| Domain.LDAP |Şu anda içeri aktarılan örnek DC etki alanının LDAP biçimi Satışlar, DC = fabrikam, DC = com |
| Forest.Netbios |Şu anda içeri aktarılan orman adının örneğin FABRIKAMCORP NetBIOS biçimi |
| Forest.FQDN |Şu anda içeri aktarılan orman adının örneğin fabrikam.com FQDN biçimi |
| Forest.LDAP |LDAP biçimi şu anda içeri aktarılan orman adı, örneğin, DC = fabrikam, DC = com |

Sistem şu anda çalışan bağlayıcı tanımlayıcısını almak için kullanılan aşağıdaki parametre sağlar:  
`Connector.ID`

Kullanıcının bulunduğu etki alanının NetBIOS adıyla meta veri deposu özniteliği etki alanı dolduran bir örnek aşağıda verilmiştir:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>İşleçler
Aşağıdaki işleçleri kullanabilirsiniz:

- **Karşılaştırma**: <, < =, <>, =, >, > =
- **Matematik**: +, -, \*, -
- **Dize**: & (Birleştir)
- **Mantıksal**: & & (ve), || (veya)
- **Değerlendirme sırası**:)

İşleçler, soldan sağa doğru değerlendirilir ve aynı değerlendirme önceliğe sahip. Diğer bir deyişle, \* (çarpanı) (çıkarma) önce - değerlendirilmez. 2\*(5 + 3) 2 ile aynı değil\*5 + 3. Köşeli ayraç (), soldan sağa Değerlendirme sırasını uygun olmadığı durumlarda Değerlendirme sırasını değiştirmek için kullanılır.

## <a name="multi-valued-attributes"></a>Birden çok değerli öznitelikler
İşlevleri tek değerli hem birden çok değerli öznitelikler üzerinde çalışabilir. İşlevi, birden çok değerli öznitelikler için her bir değerin üzerinde çalışır ve aynı işlevi her değeri için de geçerlidir.

Örneğin:  
`Trim([proxyAddresses])` Bir kırpma proxyAddress özniteliği her değerin yapın.  
`Word([proxyAddresses],1,"@") & "@contoso.com"` Her bir değeri ile bir @-sign, etki alanı ile değiştirin @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` SIP-adresi arayın ve değerleri kaldırın.

## <a name="next-steps"></a>Sonraki adımlar
- Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).
- Bkz: nasıl bildirim temelli sağlama olan kullanılan kullanıma hazır olarak [varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md).
- İçinde bildirim temelli sağlama kullanarak pratik değişiklik yapılacağını görmek [Varsayılan yapılandırmada bir değişiklik yapmak nasıl](how-to-connect-sync-change-the-configuration.md).

**Genel bakış konuları**

- [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

**Başvuru konuları**

- [Azure AD Connect eşitlemesi: İşlevler başvurusu](reference-connect-sync-functions-reference.md)


