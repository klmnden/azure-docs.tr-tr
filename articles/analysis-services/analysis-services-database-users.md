---
title: "Veritabanı rolleri ve Azure Analysis Services kullanıcıları yönetme | Microsoft Docs"
description: "Veritabanı rolleri ve kullanıcıların Azure Analysis Services sunucusunda yönetmeyi öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d0bc7d7514f111b4bbde33bd60ae21264bd797fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="manage-database-roles-and-users"></a>Veritabanı rolleri ve kullanıcıları yönetme

Model veritabanı düzeyinde tüm kullanıcıların bir role ait olmalıdır. Rolleri model veritabanı için belirli izinlerine sahip olan kullanıcıların tanımlar. Herhangi bir kullanıcı veya güvenlik grubu role eklenen bir hesap Azure AD kiracısı sunucu ile aynı abonelikte olması gerekir.

Rolleri tanımlama nasıl kullandığınız aracı bağlı olarak farklı olan, ancak etkisi aynıdır.

Rol izinleri şunlardır:
*  **Yönetici** -kullanıcınız veritabanı için tam izinleri. Yönetici izinlerine sahip veritabanı rolleri, sunucu yöneticisinden farklıdır.
*  **İşlem** -kullanıcılar bağlanmak ve veritabanı üzerinde işlem işlemleri ve model veritabanı verileri analiz edin.
*  **Okuma** -bağlanmak ve model veritabanı verileri çözümlemek için bir istemci uygulaması kullanıcılar kullanabilir.

Tablo modeli projesi oluştururken, roller oluşturabilir ve SSDT içinde rol Yöneticisi'ni kullanarak bu rollere kullanıcıları veya grupları ekleyin. Bir sunucuya dağıtıldığında, SSMS, kullandığınız [Analiz Hizmetleri PowerShell cmdlet'leri](https://msdn.microsoft.com/library/hh758425.aspx), veya [tablolu modeli komut dosyası dili](https://msdn.microsoft.com/library/mt614797.aspx) rolleri ve kullanıcı üye eklemek veya kaldırmak için (TMSL).

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a>Eklemek veya roller ve SSDT kullanıcıları yönetmek için  
  
1.  Ssdt'de > **tablolu Model Gezgini**, sağ **rolleri**.  
  
2.  **Rol Yöneticisi**'nde **Yeni**'ye tıklayın.  
  
3.  Rolü için bir ad yazın.  
  
     Varsayılan olarak, varsayılan rol adını artımlı olarak her yeni rol için numaralandırılır. Üye türü, örneğin, finans yöneticileri veya İnsan Kaynakları uzmanlarıyla açıkça tanımlayan bir ad yazın önerilir.  
  
4.  Aşağıdaki izinlerden birini seçin:  
  
    |İzin|Açıklama|  
    |----------------|-----------------|  
    |**Yok**|Üyeleri olan model şeması değiştirilemez ve verileri sorgulanamıyor.|  
    |**Okuma**|Üyeler (satır filtreleri bağlı olarak) verileri sorgulayabilir ancak model şeması değiştiremezsiniz.|  
    |**Okuma ve işlem**|Üye verileri (temel alınarak satır düzeyi filtreleri) ve çalıştırma işlemi ve işlem tüm işlemleri sorgulama yapabilirsiniz ancak model şeması değiştiremezsiniz.|  
    |**İşlem**|Üye işlemi ve işlem tüm işlemleri çalıştırabilirsiniz. Model şeması değişiklik yapılamaz ve veri sorgulayamıyor.|  
    |**Yönetici**|Üyeleri olan model şeması değiştirmek ve tüm verileri sorgulayabilirsiniz.|   
  
5.  Rol kullanıyorsanız oluşturma okuma veya okuma ve işlem izni bir DAX formülü kullanarak satır filtrelerini ekleyebilirsiniz. Tıklatın **satır filtrelerini** sekmesinde, bir tablo seçin, sonra tıklatın **DAX filtre** alan ve bir DAX formülü girin.
  
6.  Tıklatın **üyeleri** > **dış eklemek**.  
  
8.  İçinde **dış Üye Ekle**, kullanıcıları veya grupları, Azure AD kiracınızda tarafından e-posta adresi girin. Tamam'ı tıklatın ve Rol Yöneticisi'ni kapatın rolleri ve Rol üyeleri tablolu Model Gezgini'nde görünür. 
 
     ![Rol ve kullanıcı tablolu Model Gezgini](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Azure Analysis Services sunucusuna dağıtır.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>Eklemek veya roller ve SSMS kullanıcıları yönetmek için
Rol ve kullanıcı bir dağıtılan modeli veritabanına eklemek için Sunucu Yöneticisi veya yönetici izinlerine sahip bir veritabanı rolü zaten sunucusuna bağlı olmanız gerekir.

1. Nesne Exporer içinde sağ **rolleri** > **yeni rol**.

2. İçinde **Rol Oluştur**, bir rol adı ve açıklama girin.

3. Bir izin seçin.
   |İzin|Açıklama|  
   |----------------|-----------------|  
   |**Tam Denetim (Yönetici)**|Üyeleri olan model şeması değiştirebilirsiniz işlemek ve tüm verileri sorgulayabilir.| 
   |**İşlem veritabanı**|Üye işlemi ve işlem tüm işlemleri çalıştırabilirsiniz. Model şeması değişiklik yapılamaz ve veri sorgulayamıyor.|  
   |**Okuma**|Üyeler (satır filtreleri bağlı olarak) verileri sorgulayabilir ancak model şeması değiştiremezsiniz.|  
  
4. Tıklatın **üyelik**, sonra bir kullanıcı veya grubu girin kiracınızda Azure AD tarafından e-posta adresi.

     ![Kullanıcı ekle](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Oluşturmakta olduğunuz rol okuma iznine sahipse, bir DAX formülü kullanarak satır filtrelerini ekleyebilirsiniz. Tıklatın **satır filtrelerini**, bir tablo seçin ve ardından bir DAX formülü yazın **DAX filtre** alan. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>TMSL komut dosyası kullanarak rolleri ve kullanıcılar ekleme
SSMS veya PowerShell kullanarak XMLA penceresinde TMSL komut dosyası çalıştırabilirsiniz. Kullanım [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) komut ve [rolleri](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) nesnesi.

**Örnek TMSL komut dosyası**

Bu örnekte, B2B dış kullanıcı ve Grup SalesBI veritabanı için Okuma izinlerine sahip analist rol eklenir. Dış kullanıcı ve Grup aynı Kiracı Azure AD olmalıdır.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="to-add-roles-and-users-by-using-powershell"></a>PowerShell kullanarak rolleri ve kullanıcılar ekleme
[SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) görev özgü veritabanı yönetimi cmdlet'leri ve tablo modeli komut dosyası dili (TMSL) sorgu veya betik kabul genel amaçlı Invoke-ASCmd cmdlet modülü sağlar. Aşağıdaki cmdlet, veritabanı rolleri ve kullanıcıları yönetmek için kullanılır.
  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[Ekleme RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Bir veritabanı rolüne üye ekleme.| 
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Üye veritabanı rolden kaldırır.|   
|[Çağırma ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|TMSL betiğini yürütün.|

## <a name="row-filters"></a>Satır filtreleri  
Hangi satır bir tablodaki belirli bir rol üyeleri tarafından sorgulanabilir satır filtrelerini tanımlayın. Satır filtrelerini DAX formülleri kullanarak modeldeki her tablo için tanımlanır.  
  
Satır filtreleri, yalnızca rollerini okuma ve okuma için tanımlanabilir ve işlem izinleri. Belirli bir tablo için bir satır filtresi tanımlı değilse, varsayılan olarak, başka bir tablodan çapraz filtreleme uygulanır sürece tablodaki tüm satırları üyeleri sorgulayabilirsiniz.
  
 Satır filtrelerini bu belirli rolünün üyeleri tarafından sorgulanabilir satırları tanımlamak için bir TRUE/FALSE değeri değerlendirilmelidir bir DAX formülü gerektirir. DAX formülü dahil edilmeyen satırları sorgulanamıyor. Örneğin, aşağıdaki satırı Müşteriler tablosuyla ifadeyi filtreler *müşteriler [Country] = "ABD" =*, satış rolünün üyeleri yalnızca ABD'deki müşteriler bakın.  
  
Belirtilen satırları ve ilişkili satırları satır filtreleri uygulayın. Bir tabloda birden çok ilişki olduğunda, güvenlik etkin ilişki için filtre uygulayın. Satır filtrelerini diğer satır filtreleri ilişkili tablolar için örneğin tanımlanmış kesiştiğinden:  
  
|Tablo|DAX ifadesi|  
|-----------|--------------------|  
|Bölge|= Bölge [Country] = "Tr"|  
|ProductCategory|= ProductCategory [Name] "Bisiklet" =|  
|İşlemler|= İşlemleri [yıl] 2016 =|  
  
 Üyeleri burada müşteri ABD'de, ürün kategorisi bisiklet olduğu ve yıl 2016 veri satırı sorgulayabilirsiniz net etkisidir. Kullanıcılar, bu izinler veren başka bir rolün üyesi olmadığı sürece olmayan bisiklet ya da işlemleri 2016'da değil işlemleri işlemleri ABD dışında sorgulayamıyor.
  
 Filtre kullanabilirsiniz *=FALSE()*, tüm bir tabloyu tüm satırları erişimini.

## <a name="next-steps"></a>Sonraki adımlar
  [Sunucu yöneticileri yönetin](analysis-services-server-admins.md)   
  [Azure Analysis Services PowerShell ile yönetme](analysis-services-powershell.md)  
  [Tablo modeli komut dosyası dili (TMSL) başvurusu](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

