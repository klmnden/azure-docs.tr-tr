---
title: Azure AD grafik API'si için hızlı başlangıç | Microsoft Docs
description: Azure Active Directory grafik API'sini Azure AD OData REST API uç noktaları yoluyla programlı erişim sağlar. Uygulamaları gerçekleştirmek için Azure AD grafik API'si kullanabilir oluşturma, okuma, güncelleştirme ve (CRUD) işlemleridir dizin verileri ve nesneleri silme.
services: active-directory
documentationcenter: n/a
author: mtillman
manager: mtillman
editor: ''
tags: ''
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: d195d808e07b872c11379f13b6e89794da39f70e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="quickstart-for-the-azure-ad-graph-api"></a>Azure AD grafik API'si için hızlı başlangıç
Azure Active Directory (AD) grafik API'si Azure AD OData REST API uç noktaları yoluyla programlı erişim sağlar. Uygulamaları gerçekleştirmek için Azure AD grafik API'si kullanabilir oluşturma, okuma, güncelleştirme ve (CRUD) işlemleridir dizin verileri ve nesneleri silme. Örneğin, yeni bir kullanıcı oluşturmak, görüntülemek veya kullanıcının özelliklerini güncelleştirmek, kullanıcı parolasını değiştirmeniz, rol tabanlı erişim, Grup üyeliğini denetlemek için Azure AD Graph API kullanabilirsiniz devre dışı bırakın ya da kullanıcıyı silin. Azure AD Graph API özellikler ve uygulama senaryoları hakkında daha fazla bilgi için bkz: [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) ve [Azure AD Graph API Önkoşullar](https://msdn.microsoft.com/library/hh974476.aspx). 

> [!IMPORTANT]
> Azure Active Directory kaynaklarına erişmek için Azure AD Graph API'si yerine [Microsoft Graph](https://developer.microsoft.com/graph) kullanmanız önemle tavsiye edilir. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API’sinin hala uygun olabileceği senaryo sayısı çok sınırlıdır; daha fazla bilgi için Office Geliştirici Merkezi’ndeki [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog gönderisine bakın.
> 
> 

## <a name="how-to-construct-a-graph-api-url"></a>Grafik API'si URL'nin nasıl oluşturulacağı
Grafik API'si, dizin verilerini ve (diğer bir deyişle, kaynakları veya varlıklar) karşı CRUD işlemleri gerçekleştirmek istediğiniz nesnelere erişmek için açık veri (OData) protokolünü temel URL'leri kullanabilirsiniz. Grafik API'si kullanılan URL'leri dört ana bölümden oluşur: hizmet kök, Kiracı tanıtıcısı, kaynak yol ve sorgu dizesi seçenekleri: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Aşağıdaki URL örneği ele: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Hizmet kök**: Azure AD Graph API, hizmet her zaman köküdür https://graph.windows.net.
* **Kiracı tanımlayıcı**: Bu bölümde yukarıdaki örnekte, bir doğrulanmış (kayıtlı) etki alanı adı olabilir contoso.com. Bu aynı zamanda bir kiracı nesne kimliği veya "myorganization" veya "benim" diğer adı olabilir. Daha fazla bilgi için bkz: [adresleme varlıkları ve Azure AD Graph API işlemleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview).
* **Kaynak yolu**: Bu bölümde bir URL'ye (kullanıcılar, gruplar, belirli bir kullanıcı veya bir özel grup, vb. ile) bulunulması için kaynak tanımlar Yukarıdaki örnekte, kaynak adresi için üst düzey "grupları" değil. Örneğin belirli bir varlık karşılayabilirsiniz "kullanıcılar / {objectID}" veya "kullanıcıların/userPrincipalName".
* **Sorgu parametreleri**: kaynak yolu bölümü sorgu parametreleri bölümünden bir soru işareti (?) ayırır. Azure AD Graph API de tüm istekler "api-version" sorgu parametresi gereklidir. Azure AD Graph API ayrıca aşağıdaki OData sorgu seçeneklerini destekler: **$filter**, **$orderby**, **$expand**, **$top**ve **$format**. Aşağıdaki sorgu seçenekleri şu anda desteklenmiyor: **$count**, **$inlinecount**, ve **$skip**. Daha fazla bilgi için bkz: [desteklenen sorguları, filtreleri ve Azure AD Graph API disk belleği seçeneklerinde](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Grafik API'si sürümleri
"Api-version" Sorgu parametresinde bir grafik API'si isteği sürümü belirtin. 1.5 ve daha sonraki sürümü için bir sayısal sürüm değeri kullanın; API sürümü 1.6 =. Önceki sürümler için YYYY-AA-GG biçimine aynılarını bir tarih dizesi kullanın; Örneğin, API sürümü 2013-11-08 =. Aşağıdaki dizeyi "beta"; Önizleme özellikleri için kullanın Örneğin, API sürümü beta =. Grafik API'si sürümleri arasındaki farklar hakkında daha fazla bilgi için bkz: [Azure AD Graph API sürümü oluşturma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Grafik API'si meta verileri
Aşağıdaki URL'yi iade Azure AD Graph API meta veri dosyası, URL için örnek Kiracı tanımlayıcıda sonra "$metadata" kesimi eklemek için bir tanıtım şirket için meta verileri döndürür: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Bu URL meta verileri görmek için web tarayıcınızın adres çubuğuna girebilirsiniz. Döndürülen CSDL meta veri belgesi varlıkları ve karmaşık türler, özellikleri ve işlevleri ve grafik, istenen API sürümü tarafından kullanıma sunulan eylemleri açıklar. API sürümü parametrenin atlanması en son sürümü için meta verileri döndürür.

## <a name="common-queries"></a>Genel sorgular
[Azure AD Graph API Genel sorgular](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) dizininizde en üst düzey kaynaklara erişmek için kullanılan sorguları ve dizininizde işlemlerini gerçekleştirmek için sorguları da dahil olmak üzere Azure AD grafik ile kullanılan genel sorgular listeler.

Örneğin, `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` döndürür şirket bilgileri dizin contoso.com.

Veya `https://graph.windows.net/contoso.com/users?api-version=1.6` contoso.com directory tüm kullanıcı nesneleri listeler.

## <a name="using-the-azure-ad-graph-explorer"></a>Azure AD Graph Explorer'a kullanma
Uygulamanızı oluşturmak gibi dizin verileri sorgulamak için Azure AD grafik API'si için Azure AD Graph Explorer'a kullanabilirsiniz.

Aşağıdaki ekran görüntüsünde gördüğünüz Azure AD Graph Explorer'a gidin, oturum açma ve girin olan çıktısı şöyledir `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` oturum açmış kullanıcının dizindeki tüm kullanıcıları görüntülemek için:

![Azure AD grafik api'si Gezgini](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Azure AD Graph Explorer'a yük**: aracı yüklemek için gidin [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/). Tıklatın **oturum açma** ve Azure AD grafik Explorer'ı kiracınız karşı çalıştırmak için Azure AD hesabı kimlik bilgilerinizi ile oturum açın. Kendi Kiracı karşı Azure AD Graph Explorer'a çalıştırırsanız, siz veya yöneticiniz oturum açma sırasında onaylaması gerekir. Bir Office 365 aboneliğiniz varsa, otomatik olarak Azure AD kiracısı gerekir. Office 365'e oturum açmak için kullandığınız kimlik bilgilerinin, aslında, Azure AD hesapları ve Azure AD grafik Gezgini ile bu kimlik bilgileri kullanabilirsiniz.

**Bir sorgu çalıştırın**: bir sorguyu çalıştırmak için istek metin kutusuna sorgunuzu yazın ve **almak** veya **girin** anahtarı. Sonuçlar yanıt kutusunda görüntülenir. Örneğin, `https://graph.windows.net/myorganization/groups?api-version=1.6` oturum açmış kullanıcının dizinindeki tüm Grup nesnelerini listeler.

Aşağıdaki özellikler ve Azure AD Graph Explorer'a sınırlamaları dikkat edin:

* Kaynak üzerinde otomatik tamamlama özelliğini ayarlar. Bu işlev görmek için (şirket URL'si göründüğü) istek metin kutusuna tıklayın. Açılır listeden ayarlanmış bir kaynak seçebilirsiniz.
* İstek geçmişi.
* "Bana" destekler ve "myorganization" adresleme diğer adları. Örneğin, kullanabileceğiniz `https://graph.windows.net/me?api-version=1.6` oturum açmış olan kullanıcının kullanıcı nesnesi döndürmek için veya `https://graph.windows.net/myorganization/users?api-version=1.6` tüm kullanıcılar oturum açmış kullanıcının dizinde dönün.
* Kendi dizini kullanılarak karşı tam CRUD işlemleri destekleyen `POST`, `GET`, `PATCH` ve `DELETE`.
* Yanıt Üstbilgileri bölümü. Bu bölümde, sorgu çalıştırırken oluşan sorunları gidermenize yardımcı olması için kullanılabilir.
* Genişletme ve daraltma özellikleriyle yanıt için bir JSON Görüntüleyici.
* Küçük resim fotoğrafı karşıya veya görüntüleme için destek yok.

## <a name="using-fiddler-to-write-to-the-directory"></a>Dizine yazmak için fiddler'ı kullanma
Bu Hızlı Başlangıç Kılavuzu amaçlar için fiddler'ı Web hata ayıklayıcı uygulama 'yazma işlemlerinin Azure AD dizininizi karşı' gerçekleştirmek için kullanabilirsiniz. Örneğin, almak ve (hangi Azure AD grafik Gezgini ile mümkün değildir) bir kullanıcının profil fotoğrafınız yükleyin. Daha fazla bilgi için ve fiddler'ı yüklemek için bkz: [ http://www.telerik.com/fiddler ](http://www.telerik.com/fiddler).

Aşağıdaki örnekte, yeni bir güvenlik grubu 'MyTestGroup' Azure AD dizininizi oluşturmak için fiddler'ı Web hata ayıklayıcı kullanın.

**Bir erişim belirteci edinmek**: Azure AD grafik erişmek için istemcileri ilk Azure AD'ye kimliğini başarıyla doğrulamak için gereklidir. Daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).

**Oluşturma ve bir sorgu çalıştırın**: aşağıdaki adımları tamamlayın:

1. Fiddler Web hata ayıklayıcı açın ve geçiş **Oluşturucu** sekmesi.
2. Yeni bir güvenlik grubu oluşturmak istediğiniz beri seçin **Post** aşağı açılır menüden HTTP yöntemi. Bir grup nesnesi işlemleri ve izinleri hakkında daha fazla bilgi için bkz: [grup](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) içinde [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Yanındaki alanda **Post**, aşağıdaki istek URL'sindeki türle: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > {Mytenantdomain} yerine kendi Azure AD dizini ile etki alanı adını gerekir.
   > 
   > 
4. Hemen altına Post aşağı alanında aşağıdaki HTTP üst bilgisine yazın:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Yedek, &lt;erişim belirtecinizi&gt; Azure AD dizininiz için erişim belirteci ile.
   > 
   > 
5. İçinde **istek gövdesinde** alan, aşağıdaki JSON yazın:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Grupları oluşturma hakkında daha fazla bilgi için bkz: [Grup Oluştur](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Azure AD varlıkları ve grafik tarafından sunulan türleri hakkında daha fazla bilgi ve bunlar üzerinde grafik ile gerçekleştirilebilir işlemleri hakkında bilgi için bkz: [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Azure AD grafik API'si](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Daha fazla bilgi edinmek [Azure AD grafik API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

