---
title: Azure AD Graph API'SİNİN nasıl kullanılacağı
description: Azure Active Directory (Azure AD) Graph API, OData REST API uç noktaları aracılığıyla Azure AD'ye programlı erişim sağlar. Uygulamalar Azure AD Graph API'si gerçekleştirmek için kullanabileceğiniz oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri dizin verileri ve nesneleri.
services: active-directory
documentationcenter: n/a
author: CelesteDG
manager: mtillman
editor: ''
tags: ''
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: sureshja
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2dfbf920a0e1fc002f3bcbe90164e1fd13a0b978
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411505"
---
# <a name="how-to-use-the-azure-ad-graph-api"></a>Nasıl yapılır: Azure AD Graph API’sini kullanma

Azure Active Directory (Azure AD) Graph API, OData REST API uç noktaları aracılığıyla Azure AD'ye programlı erişim sağlar. Uygulamalar Azure AD Graph API'si gerçekleştirmek için kullanabileceğiniz oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri dizin verileri ve nesneleri. Örneğin, Azure AD Graph API, yeni kullanıcı oluşturma, görüntüleme veya kullanıcının özelliklerini güncelleştirmek, kullanıcının parolasını değiştirmek için rol tabanlı erişim için grubu üyeliğini denetleyin kullanabileceğiniz devre dışı bırakın ya da kullanıcı silinemiyor. Azure AD Graph API'si özellikleri ve uygulama senaryoları hakkında daha fazla bilgi için bkz. [Azure AD Graph API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) ve [Azure AD Graph API önkoşulları](https://msdn.microsoft.com/library/hh974476.aspx).

Bu makale, Azure AD Graph API için geçerlidir. Microsoft Graph API için ilgili benzer bilgileri için bkz. [Microsoft Graph API'sini](https://developer.microsoft.com/graph/docs/concepts/use_the_api).

> [!IMPORTANT]
> Azure Active Directory kaynaklarına erişmek için Azure AD Graph API'si yerine [Microsoft Graph](https://developer.microsoft.com/graph) kullanmanız önemle tavsiye edilir. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API’sinin hala uygun olabileceği senaryo sayısı çok sınırlıdır; daha fazla bilgi için Office Geliştirici Merkezi’ndeki [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog gönderisine bakın.

## <a name="how-to-construct-a-graph-api-url"></a>Graph API URL'si oluşturmak nasıl

Graph API'si, dizin verilerini ve karşı CRUD işlemleri gerçekleştirmek istediğiniz nesneleri (diğer bir deyişle, kaynakları veya varlıklar) erişmek için açık veri (OData) protokolünü temel alan URL'leri kullanabilirsiniz. Graph API'si kullanılan URL'leri dört ana bölümden oluşur: hizmet kök, Kiracı tanımlayıcısı, kaynak yolu ve sorgu dizesi seçeneğimiz: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Aşağıdaki URL örneği Al: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Hizmet kök**: Azure AD Graph API, hizmet kök her zaman olduğu https://graph.windows.net.
* **Kiracı tanımlayıcısı**: Bu bölümde, önceki örnekte, bir (kayıtlı) doğrulanmış bir etki alanı adı olabilir contoso.com. Bu da bir kiracı nesne kimliği veya "myorganization" veya "me" diğer ad olabilir. Daha fazla bilgi için [adresleme varlıkları ve Azure AD Graph API işlemlerinde](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview).
* **Kaynak yolu**: Bu bölümde bir URL'ye (kullanıcılar, gruplar, belirli bir kullanıcıya veya bir belirli bir gruba, vb. ile) bulunulması için kaynak tanımlar Yukarıdaki örnekte, üst düzey "grupları" kaynak adresi için olduğu. Örneğin belirli bir varlığa karşılayabilirsiniz "kullanıcıların / {objectID}" veya "kullanıcıların/userPrincipalName".
* **Sorgu parametreleri**: Kaynak yolu bölümü sorgu parametreleri bölümünden bir soru işareti (?) ayırır. Azure AD Graph API'si de tüm istekler "api-version" sorgu parametresi gereklidir. Azure AD Graph API, aşağıdaki OData sorgu seçeneklerini de destekler: **$filter**, **$orderby**, **$expand**, **$top**ve **$format**. Aşağıdaki sorgu seçenekleri şu anda desteklenmiyor: **$count**, **$inlinecount**, ve **$skip**. Daha fazla bilgi için [desteklenen sorgular, filtreler ve sayfalandırma seçenekleri Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API sürümleri

"Api-version" sorgu parametresi'de bir Graph API isteği için sürüm belirtin. İçin sürüm 1.5 ve sonrası, sayısal sürüm değeri kullanın. API sürüm 1.6 =. Önceki sürümlerde, YYYY-AA-GG biçimine aynılarını bir tarih dizesini kullanın. Örneğin, api sürümü = 2013-11-08. Önizleme özellikleri için "beta"; dize kullanın. Örneğin, api sürümü beta =. Graph API sürümleri arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure AD Graph API sürümü oluşturma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API meta verileri

Aşağıdaki URL'yi Azure AD Graph API meta veri dosyası iade, URL'si örneğin Kiracı tanımlayıcısı sonra "$metadata" kesimi eklemek için bir tanıtım şirket için meta verileri getirir: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Bu URL'yi meta verileri görmek için bir web tarayıcısının Adres çubuğuna girebilirsiniz. Döndürülen CSDL meta veri belgesi, varlıkları ve karmaşık türler, kendi özellikleri ve işlevleri ve istediğiniz Graph API sürümü tarafından kullanıma sunulan eylemleri açıklar. Api-version parametresi atlama en son sürümü için meta verileri döndürür.

## <a name="common-queries"></a>Genel sorgular

[Azure AD Graph API ortak sorgular](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) dizininizde en üst düzey kaynaklara erişmek için kullanılan sorguları ve dizininizde işlemleri gerçekleştirmek için sorguları da dahil olmak üzere Azure AD Graph ile kullanılan yaygın sorguları listeler.

Örneğin, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` şirket directory contoso.com için ilgili bilgi döndürür.

Veya `https://graph.windows.net/contoso.com/users?api-version=1.6` contoso.com directory tüm kullanıcı nesnelerini listeler.

## <a name="using-the-azure-ad-graph-explorer"></a>Azure AD Graph Gezgini kullanma
Azure AD Graph API'si için Azure AD Graph Gezgini, uygulamanızı oluştururken dizin verileri sorgulamak için kullanabilirsiniz.

Aşağıdaki anlık görüntüde gördüğünüz Azure AD Graph Explorer'a gidin, oturum açın ve girin olsaydı çıktıdır `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` oturum açmış kullanıcının dizindeki tüm kullanıcıları görüntülemek için:

![Azure AD grafik api'si Gezgini](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Azure AD Graph Gezgini yük**: Aracı'nı yüklemek için gidin [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/). Tıklayın **oturum açma** ve kiracınızda Azure AD Graph Gezgini çalıştırmak için Azure AD hesabı kimlik bilgileri ile oturum açın. Azure AD Graph Gezgini karşı kendi kiracınızı çalıştırırsanız, siz veya yöneticiniz oturum açma sırasında onayı gerekir. Bir Office 365 aboneliğiniz varsa, otomatik olarak bir Azure AD kiracısına sahip. Office 365'te oturum açmak için kullandığınız kimlik bilgileri, aslında, Azure AD hesapları ve Azure AD Graph Gezgini ile bu kimlik bilgilerini kullanabilirsiniz.

**Bir sorgu çalıştırdığınızda**: Bir sorguyu çalıştırmak için sorgu isteği metin kutusuna yazın ve **alma** veya **girin** anahtarı. Sonuçlar yanıt kutusunda görüntülenir. Örneğin, `https://graph.windows.net/myorganization/groups?api-version=1.6` oturum açan kullanıcının dizinde tüm Grup nesneleri listeler.

Aşağıdaki özellikler ve Azure AD Graph Gezgini sınırlamaları unutmayın:

* Kaynak üzerindeki Otomatik Tamamla özelliği ayarlar. Bu işlevi görmek için (burada şirket URL'si görünür) istek metin kutusuna tıklayın. Açılır listeden ayarlanmış bir kaynak seçebilirsiniz.
* İstek geçmişi.
* "Me" destekler ve "myorganization" adresleme diğer adları. Örneğin, kullanabileceğiniz `https://graph.windows.net/me?api-version=1.6` oturum açmış kullanıcının kullanıcı nesneyi döndürmek için veya `https://graph.windows.net/myorganization/users?api-version=1.6` oturum açan kullanıcının dizinde tüm kullanıcılarınızı döndürmesini istiyorsanız.
* Kendi dizini kullanılarak karşı tam CRUD işlemleri destekleyen `POST`, `GET`, `PATCH` ve `DELETE`.
* Yanıt üst bilgileri bölümü. Bu bölümde, sorgu çalıştırırken oluşan sorunları gidermenize yardımcı olması için kullanılabilir.
* JSON Görüntüleyicisi Genişlet ve Daralt özellikleriyle yanıtı.
* Küçük resim fotoğrafı karşıya veya görüntüleme için destek yok.

## <a name="using-fiddler-to-write-to-the-directory"></a>Dizine yazmak için Fiddler'ı kullanma

Bu Hızlı Başlangıç Kılavuzu amaçları için 'Azure AD dizininizi karşı yazma işlemleri' gerçekleştiren uygulama Fiddler Web hata ayıklayıcıyı kullanabilirsiniz. Örneğin, alın ve (hangi Azure AD Graph Gezgini ile mümkün değildir) bir kullanıcının profil fotoğrafı karşıya yükleyin. Daha fazla bilgi ve fiddler'ı yüklemek için bkz: [ https://www.telerik.com/fiddler ](https://www.telerik.com/fiddler).

Aşağıdaki örnekte, Azure AD dizininizde yeni bir güvenlik grubu 'MyTestGroup' oluşturmak için fiddler'ı Web hata ayıklayıcıyı kullanın.

**Bir erişim belirteci edinmek**: Azure AD Graph erişmek için öncelikle Azure AD'ye kimliğini başarıyla doğrulamak için istemcileri gereklidir. Daha fazla bilgi için [Azure AD için kimlik doğrulama senaryoları](authentication-scenarios.md).

**Oluşturma ve bir sorgu çalıştırdığınızda**: Aşağıdaki adımları tamamlayın:

1. Fiddler'ı Web Hata Ayıklayıcısı'nı açın ve geçiş **Oluşturucusu** sekmesi.
2. Bu yana yeni bir güvenlik grubu oluşturmak istediğiniz seçin **Post** aşağı açılır menüden HTTP yöntemi. Bir grup nesnede işlemleri ve izinleri hakkında daha fazla bilgi için bkz. [grubu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) içinde [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. İleri'alanında **Post**, aşağıdaki istek URL'sindeki: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > Kendi Azure AD dizini ile etki alanı adı yerine, {mytenantdomain} gerekir.

4. Post aşağı açılır hemen altındaki alanda aşağıdaki HTTP üst bilgisi yazın:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Yedek, &lt;erişim belirtecinizin&gt; ile Azure AD dizininiz için erişim belirteci.

5. İçinde **istek gövdesi** alan, aşağıdaki JSON yazın:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Grupları oluşturma hakkında daha fazla bilgi için bkz. [Grup Oluştur](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Azure AD varlıkları ve Graph tarafından kullanıma sunulan türleri hakkında daha fazla bilgi ve grafik ile bunlar üzerinde gerçekleştirilen işlemler hakkında daha fazla bilgi için bkz. [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure AD Graph API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Daha fazla bilgi edinin [Azure AD Graph API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
