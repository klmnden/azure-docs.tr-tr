---
title: "Azure Active Directory denetim API Başvurusu | Microsoft Docs"
description: "Azure Active Directory denetim API'si ile çalışmaya nasıl başlayacağınız"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5cdf80ff1cc49b1582302d411ee6fcc8f193c021
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory denetim API Başvurusu
Bu konuda, Azure Active Directory hakkındaki konuları API raporlama koleksiyonu bir parçasıdır.  
Azure AD raporlama kodu veya ilgili araçları kullanarak denetim veri erişmenizi sağlayan bir API ile sağlar.
Hakkında başvuru bilgileri sağlamak için bu konunun kapsamı olan **API denetim**.

Bkz.:

* [Denetim günlükleri](active-directory-reporting-azure-portal.md#activity-reports) daha fazla kavramsal bilgi için

* [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) raporlama API'si hakkında daha fazla bilgi için.


İçin:

- Sık sorulan sorular, okuma bizim [SSS](active-directory-reporting-faq.md) 

- Sorunları, lütfen [bir destek bileti dosya](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Güvenlik Yöneticisi veya Güvenlik Okuyucusu rolündeki kullanıcılar
* Genel Yöneticiler
* API erişme yetkisi olan herhangi bir uygulama (app yetkilendirme olabilir Kurulum genel yöneticinin iznine dayanılarak yalnızca)

## <a name="prerequisites"></a>Önkoşullar
Bu rapor raporlama API'si erişmek için sahip olmanız gerekir:

* Bir [Azure Active Directory ücretsiz ya da daha iyi edition](active-directory-editions.md)
* Tamamlanan [Azure AD raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>API erişme
Bu API aracılığıyla da erişebilirsiniz [Graph Explorer'a](https://graphexplorer2.cloudapp.net) veya program aracılığıyla, örneğin, PowerShell kullanarak. AAD grafik REST çağrılarında kullanılan OData filtresi sözdizimi doğru şekilde yorumlamaya PowerShell sırayla backtick kullanmanız gerekir (diğer adıyla: Vurgu) "$ karakterini atlamak için" karakter. Backtick karakter gören [PowerShell'in kaçış karakteri](https://technet.microsoft.com/library/hh847755.aspx), sabit bir $ karakteri yorumu yapın ve PowerShell değişken adı olarak kafa karıştırıcı önlemek PowerShell izin verme (IE: $filter).

Bu konunun odak Graph Explorer'a noktasıdır. Bu PowerShell örnek için bkz [PowerShell Betiği](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API Endpoint
Bu API aşağıdaki URI'ı kullanarak erişebilirsiniz:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

(OData sayfalandırma kullanarak) Azure AD denetim API'si tarafından döndürülen kayıt sayısı sınırı yoktur.
Bekletme sınırları veri raporlama, kullanıma [raporlama bekletme ilkeleri](active-directory-reporting-retention.md).

Bu çağrı verileri toplu olarak döndürür. Her toplu işlem en fazla 1000 kaydı var.  
Sonraki toplu kayıt almak için sonraki bağlantıyı kullanın. Döndürülen kayıtları ilk kümesinden skiptoken bilgi alın. Atla belirteci sonucu sonunda ayarlanır.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Desteklenen filtreler
Bir API tarafından döndürülen kayıt sayısını daraltabilirsiniz bir filtre formda çağırın.  
Oturum açma için API ilgili verileri, aşağıdaki filtreleri desteklenir:

* **$top =\<döndürülecek kayıtları numara\>**  - döndürülen kayıt sayısını sınırlamak için. Bu pahalı bir işlemdir. Binlerce nesneyi dönmek istiyorsanız, bu filtre kullanmamanız gerekir.     
* **$filter =\<filtre ifadesi\>**  -, desteklenen filtre alanları temel alarak çok önem verdiğiniz kayıtları türünü belirtmek için

## <a name="supported-filter-fields-and-operators"></a>Desteklenen filtre alanları ve işleçleri
İlgilendiğiniz kayıtları türünü belirtmek için bir ya da aşağıdaki filtre alanlarını bileşimini içerebilir bir filtre ifadesi oluşturabilirsiniz:

* [Tarihi](#activitydate) -bir tarih veya tarih aralığı tanımlar
* [Kategori](#category) -filtre uygulamak istediğiniz kategori tanımlar.
* [activityStatus](#activitystatus) -bir etkinlik durumunu tanımlar
* [activityType](#activitytype) -bir etkinlik türünü tanımlar
* [Etkinlik](#activity) -dize olarak etkinliğini tanımlar  
* [Aktör/adı](#actorname) -aktör aktör'ın adı biçiminde tanımlar
* [Aktör/objectID](#actorobjectid) -aktör aktör'ın kimliği formunda tanımlar   
* [Aktör/upn](#actorupn) -aktör aktör'ın kullanıcı asıl adı (UPN) biçiminde tanımlar 
* [Hedef/adı](#targetname) -hedef aktör'ın adı biçiminde tanımlar
* [Hedef/objectID](#targetobjectid) -hedef hedefin kimliği formunda tanımlar  
* [Hedef/upn](#targetupn) -aktör aktör'ın kullanıcı asıl adı (UPN) biçiminde tanımlar   

- - -
### <a name="activitydate"></a>activityDate
**İşleçler desteklenen**: eq, ge, le, gt, lt

**Örnek**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Notlar**:

DateTime UTC biçiminde olmalıdır

- - -
### <a name="category"></a>category

**Desteklenen değerler**:

| Kategori                         | Değer     |
| :--                              | ---       |
| Çekirdek Dizin                   | Dizin |
| Self Servis Parola Yönetimi | SSPR      |
| Self Servis Grup Yönetimi    | SSGM      |
| Hesap Sağlama             | Sync      |
| Otomatik Parola Geçişi      | Otomatik Parola Geçişi |
| Kimlik Koruması              | IdentityProtection |
| Davetli Kullanıcılar                    | Davetli Kullanıcılar |
| MIM Hizmeti                      | MIM Hizmeti |



**İşleçler desteklenen**: eq

**Örnek**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Desteklenen değerler**:

| Etkinlik Durumu | Değer |
| :--             | ---   |
| Başarılı         | 0     |
| Hata         | - 1   |

**İşleçler desteklenen**: eq

**Örnek**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=activityType eq 'User'    

**Notlar**:

büyük küçük harfe duyarlı

- - -
### <a name="activity"></a>etkinlik
**İşleçler desteklenen**: eq, içerir, startsWith

**Örnek**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Notlar**:

büyük küçük harfe duyarlı

- - -
### <a name="actorname"></a>Aktör/adı
**İşleçler desteklenen**: eq, içerir, startsWith

**Örnek**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Notlar**:

Büyük küçük harfe duyarsızdır

- - -
### <a name="actorobjectid"></a>actor/objectId
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>Hedef/adı
**İşleçler desteklenen**: eq, içerir, startsWith

**Örnek**:

    $filter=targets/any(t: t/name eq 'some name')    

**Notlar**:

Büyük küçük harfe duyarsızdır

- - -
### <a name="targetupn"></a>Hedef/upn
**İşleçler desteklenen**: eq, startsWith

**Örnek**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Notlar**:

* Büyük küçük harfe duyarsızdır
* Tam ad alanını Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity sorgulanırken eklemeniz gerekir

- - -
### <a name="targetobjectid"></a>target/objectId
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>Aktör/upn
**İşleçler desteklenen**: eq, startsWith

**Örnek**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Notlar**:

* Büyük küçük harfe duyarsızdır 
* Tam ad alanını Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity sorgulanırken eklemeniz gerekir

- - -
## <a name="next-steps"></a>Sonraki Adımlar
* Filtrelenmiş sistem faaliyetleri örneklerini görmek istiyor musunuz? Kullanıma [Azure Active Directory denetim API'si örnekleri](active-directory-reporting-api-audit-samples.md).
* Azure AD raporlama API'si hakkında daha fazla bilgi istiyor musunuz? Bkz: [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).

