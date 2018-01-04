---
title: "Azure Active Directory oturum açma etkinliği raporu API Başvurusu | Microsoft Docs"
description: "Azure Active Directory oturum açma etkinliği raporu API Başvurusu"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: e213e6fcf10e98cb8e4344692475eb8d41d1afb5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory oturum açma etkinliği raporu API Başvurusu
Bu konuda, Azure Active Directory hakkındaki konuları API raporlama koleksiyonu bir parçasıdır.  
Azure AD raporlama kodu veya ilgili araçları kullanarak oturum açma etkinliği rapor verilerine erişim sağlayan bir API ile sağlar.
Hakkında başvuru bilgileri sağlamak için bu konunun kapsamı olan **oturum açma etkinliği raporu API işleminde**.

Bkz.:

* [Oturum açma etkinliklerini](active-directory-reporting-azure-portal.md#activity-reports) daha fazla kavramsal bilgi için
* [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) raporlama API'si hakkında daha fazla bilgi için.


## <a name="who-can-access-the-api-data"></a>API veri erişebilecek mi?
* Kullanıcılar ve Güvenlik Yöneticisi veya güvenlik okuyucu rolü hizmet asıl adı
* Genel Yöneticiler
* API erişme yetkisi olan herhangi bir uygulama (app yetkilendirme olabilir Kurulum genel yöneticinin iznine dayanılarak yalnızca)

Oturum açma olayları gibi güvenlik API'leri erişmek bir uygulama için erişim yapılandırmak için güvenlik okuyucu rolüne uygulamalara hizmet sorumlusu eklemek için aşağıdaki PowerShell kullanın

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Ön koşullar
Bu rapor raporlama API aracılığıyla erişmek için şunlara sahip olmalısınız:

* Bir [Azure Active Directory Premium P1 veya P2 edition](active-directory-editions.md)
* Tamamlanan [Azure AD raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>API erişme
Bu API aracılığıyla da erişebilirsiniz [Graph Explorer'a](https://graphexplorer2.cloudapp.net) veya program aracılığıyla, örneğin, PowerShell kullanarak. AAD grafik REST çağrılarında kullanılan OData filtresi sözdizimi doğru şekilde yorumlamaya PowerShell sırayla backtick kullanmanız gerekir (diğer adıyla: Vurgu) "$ karakterini atlamak için" karakter. Backtick karakter gören [PowerShell'in kaçış karakteri](https://technet.microsoft.com/library/hh847755.aspx), sabit bir $ karakteri yorumu yapın ve PowerShell değişken adı olarak kafa karıştırıcı önlemek PowerShell izin verme (IE: $filter).

Bu konunun odak Graph Explorer'a noktasıdır. Bu PowerShell örnek için bkz [PowerShell Betiği](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>API uç noktası
Bu API aşağıdaki temel URI'yi kullanarak erişebilirsiniz:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Veri hacmi nedeniyle, bu API bir milyon kayıtları döndürülen bir sınıra sahiptir. 

Bu çağrı verileri toplu olarak döndürür. Her toplu işlem en fazla 1000 kaydı var.  
Sonraki toplu kayıt almak için sonraki bağlantıyı kullanın. Alma [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) ilk döndürülen kayıt kümesi bilgileri. Atla belirteci sonucu sonunda ayarlanır.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Desteklenen filtreler
Bir API tarafından döndürülen kayıt sayısını daraltabilirsiniz bir filtre formda çağırın.  
Oturum açma için API ilgili verileri, aşağıdaki filtreleri desteklenir:

* **$top =\<döndürülecek kayıtları numara\>**  - döndürülen kayıt sayısını sınırlamak için. Bu pahalı bir işlemdir. Binlerce nesneyi dönmek istiyorsanız, bu filtre kullanmamanız gerekir.  
* **$filter =\<filtre ifadesi\>**  -, desteklenen filtre alanları temel alarak çok önem verdiğiniz kayıtları türünü belirtmek için

## <a name="supported-filter-fields-and-operators"></a>Desteklenen filtre alanları ve işleçleri
İlgilendiğiniz kayıtları türünü belirtmek için bir ya da aşağıdaki filtre alanlarını bileşimini içerebilir bir filtre ifadesi oluşturabilirsiniz:

* [signinDateTime](#signindatetime) -bir tarih veya tarih aralığı tanımlar
* [UserId](#userid) -belirli bir tanımlar kullanıcı tabanlı kullanıcının kimliği
* [userPrincipalName](#userprincipalname) -belirli bir tanımlar kullanıcı tabanlı kullanıcının kullanıcı asıl adı (UPN)
* [AppID](#appid) -belirli bir tanımlar uygulaması tabanlı uygulamanın Kimliğini
* [appDisplayName](#appdisplayname) -belirli bir tanımlar uygulaması tabanlı uygulamanın görünen adı
* [loginStatus](#loginStatus) -giriş durumunu tanımlar (başarı / hata)

> [!NOTE]
> Graph Explorer'a, sizin için her harf ayrımına gerek kullanıldığında, filtre alanları.
> 
> 

Döndürülen veri kapsamını daraltmak için desteklenen filtreler ve filtre alanları birleşimlerini oluşturabilirsiniz. Örneğin, aşağıdaki deyim 1 Temmuz 2016 Temmuz 6 2016 arasındaki üst 10 kayıtları döndürür:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**İşleçler desteklenen**: eq, ge, le, gt, lt

**Örnek**:

Belirli bir tarih kullanma

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Bir tarih aralığı kullanma    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notlar**:

Datetime parametresi UTC biçiminde olmalıdır 

- - -
### <a name="userid"></a>Kullanıcı Kimliği
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notlar**:

UserId değeri bir dize değeridir

- - -
### <a name="userprincipalname"></a>userPrincipalName
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notlar**:

UserPrincipalName değeri bir dize değeridir

- - -
### <a name="appid"></a>AppID
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notlar**:

AppID değerini bir dize değeridir

- - -
### <a name="appdisplayname"></a>appDisplayName
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notlar**:

Bir dize değeri appDisplayName değeri

- - -
### <a name="loginstatus"></a>loginStatus
**İşleçler desteklenen**: eq

**Örnek**:

    $filter=loginStatus+eq+'1'  


**Notlar**:

LoginStatus için iki seçenek vardır: 0 - başarılı, 1 - hata

- - -
## <a name="next-steps"></a>Sonraki adımlar
* Oturum açma filtrelenmiş etkinlikleri örneklerini görmek istiyor musunuz? Kullanıma [Azure Active Directory oturum açma etkinliği raporu API örnekleri](active-directory-reporting-api-sign-in-activity-samples.md).
* Azure AD raporlama API'si hakkında daha fazla bilgi istiyor musunuz? Bkz: [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).

