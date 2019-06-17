---
title: Veritabanı rolleri ve kullanıcıları Azure Analysis Services yönetme | Microsoft Docs
description: Veritabanı rolleri ve kullanıcıların bir Azure Analysis Services sunucusu'nı yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 462625ce61f4538aa0769667648e07cc6307cbb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61023640"
---
# <a name="manage-database-roles-and-users"></a>Veritabanı rolleri ve kullanıcıları yönetme

Model veritabanı düzeyinde tüm kullanıcıları bir role ait olmalıdır. Model veritabanı için belirli izinlere sahip kullanıcı rolleri tanımlar. Herhangi bir kullanıcı veya güvenlik grubu role eklenen bir hesabı Azure AD kiracısı sunucu ile aynı abonelikte olması gerekir. 

Kullandığınız araç bağlı olarak farklı rolleri nasıl tanımladığınızı ancak etkisi aynıdır.

Rol izinleri şunlardır:
*  **Yönetici** -kullanıcılar, veritabanı için tam izinlere sahip. Yönetici izinlerine sahip veritabanı rolleri, sunucu yöneticilerinin farklıdır.
*  **İşlem** -kullanıcılar bağlanmak ve veritabanını işlem işlemleri ve model veritabanı verileri analiz edin.
*  **Okuma** -kullanıcıların bağlanın ve model veritabanı verileri analiz etmek için bir istemci uygulamasını kullanabilir.

Bir tablosal model projesi oluştururken, rolleri oluşturma ve SSDT'de Rol Yöneticisi'ni kullanarak bu rollere kullanıcılar veya gruplar ekleyin. Bir sunucuya dağıtıldığında, SSMS, kullandığınız [Analiz Hizmetleri PowerShell cmdlet'leri](/sql/analysis-services/powershell/analysis-services-powershell-reference), veya [Tablosal Model betik dili](https://msdn.microsoft.com/library/mt614797.aspx) roller veya kullanıcı üyeleri eklemek veya kaldırmak için (TMSL).

> [!NOTE]
> Güvenlik grupları olmalıdır `MailEnabled` özelliğini `True`.

## <a name="to-add-or-manage-roles-and-users-in-ssdt"></a>Eklemek veya roller ve kullanıcılar ssdt'de yönetmek için  
  
1.  SSDT > **Tablosal Model Gezgini**, sağ **rolleri**.  
  
2.  **Rol Yöneticisi**'nde **Yeni**'ye tıklayın.  
  
3.  Rolü için bir ad yazın.  
  
     Varsayılan olarak, varsayılan rolün adını, her yeni bir rol için artımlı olarak numaralandırılmıştır. Örneğin, finans yöneticileri veya İnsan Kaynakları uzmanlarıyla üye türü açıkça tanımlayan bir ad yazın önerilir.  
  
4.  Aşağıdaki izinlerden birini seçin:  
  
    |İzin|Açıklama|  
    |----------------|-----------------|  
    |**Yok.**|Üyeleri model şeması değiştirilemez ve verileri sorgulayamaz.|  
    |**Okuma**|Üyeler (satır filtreleri temel alarak) verileri sorgulayabilirsiniz ancak model şeması değiştiremezsiniz.|  
    |**Okuma ve işleme**|Üye verileri (bağlı olarak satır düzeyi filtreleri) ve çalıştırma işlemi ve işlemin tüm işlemleri sorgulayabilirsiniz ancak model şeması değiştirilemez.|  
    |**İşlem**|Üye işlemi ve işlemin tüm işlemleri de çalıştırabilirsiniz. Model şeması değiştirilemez ve verileri sorgulayamaz.|  
    |**Yönetici**|Üyeleri model şeması değiştirebilir ve tüm verileri sorgulayın.|   
  
5.  Rol kullanıyorsanız oluşturma okuma veya okuma ve işleme izni bir DAX formülü kullanarak satır filtreleri ekleyebilirsiniz. Tıklayın **satır filtreleri** sekmesinde, bir tablo seçin, sonra tıklayın **DAX filtresi** alan ve bir DAX formülü yazın.
  
6.  Tıklayın **üyeleri** > **harici Ekle**.  
  
8.  İçinde **harici Üye Ekle**, kullanıcıları veya grupları kiracınızda Azure AD tarafından e-posta adresi girin. Sonra Tamam'a tıklayın ve rolleri ve Rol üyeleri Tablosal Model Gezgini'nde görünür Rol Yöneticisi'ni kapatın. 
 
     ![Rolleri ve kullanıcıları Tablosal Model Gezgini'nde](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Azure Analysis Services sunucunuza dağıtabilirsiniz.


## <a name="to-add-or-manage-roles-and-users-in-ssms"></a>Eklemek veya roller ve kullanıcılar ssms'de yönetmek için

Roller ve kullanıcılar için bir dağıtılan model veritabanı eklemek için sunucuya olarak sunucu yöneticisi veya yönetici izinlerine sahip bir veritabanı rolü zaten bağlı gerekir.

1. Nesne Exporer içinde sağ **rolleri** > **yeni rol**.

2. İçinde **Rol Oluştur**, bir rol adı ve açıklama girin.

3. Bir izin seçin.

   |İzin|Açıklama|  
   |----------------|-----------------|  
   |**Tam Denetim (Yönetici)**|Üyeleri, model şeması değiştirebilirsiniz işlemek ve tüm verileri sorgulayabilir.| 
   |**İşlem veritabanı**|Üye işlemi ve işlemin tüm işlemleri de çalıştırabilirsiniz. Model şeması değiştirilemez ve verileri sorgulayamaz.|  
   |**Okuma**|Üyeler (satır filtreleri temel alarak) verileri sorgulayabilirsiniz ancak model şeması değiştiremezsiniz.|  
  
4. Tıklayın **üyelik**, ardından bir kullanıcıyı veya grubu girin, ' % s'kiracınızın Azure AD e-posta adresi.

     ![Kullanıcı ekle](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Oluşturduğunuz rolü okuma izni varsa, bir DAX formülü kullanarak satır filtreleri ekleyebilirsiniz. Tıklayın **satır filtreleri**, bir tablo seçin ve ardından bir DAX formülüne yazın **DAX filtresi** alan. 

## <a name="to-add-roles-and-users-by-using-a-tmsl-script"></a>TMSL betik kullanarak rolleri ve kullanıcıları ekleme

XMLA penceresinde SSMS veya PowerShell kullanarak bir TMSL betiği çalıştırabilirsiniz. Kullanım [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) komut ve [rolleri](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) nesne.

**Örnek TMSL betiği**

Bu örnekte, dış B2B kullanıcısı ve grubu SalesBI veritabanı için Okuma izinlerine sahip analist rolüne eklenir. Dış kullanıcı ve Grup aynı Azure AD kiracısında olması gerekir.

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

## <a name="to-add-roles-and-users-by-using-powershell"></a>PowerShell kullanarak rolleri ve kullanıcıları ekleme

[SqlServer](/sql/analysis-services/powershell/analysis-services-powershell-reference) göreve özel veritabanı yönetimi cmdlet'leri ve bir Tablosal Model betik dili (TMSL) sorgu veya betik kabul genel amaçlı Invoke-ASCmd cmdlet'i modülü sağlar. Aşağıdaki cmdlet, veritabanı rolleri ve kullanıcıları yönetmek için kullanılır.
  
|Cmdlet|Açıklama|
|------------|-----------------| 
|[RoleMember ekleyin](/sql/analysis-services/powershell/analysis-services-powershell-reference)|Üye veritabanı rolüne ekleyin.| 
|[Remove-RoleMember](/sql/analysis-services/powershell/analysis-services-powershell-reference)|Üye veritabanı rolden kaldırma.|   
|[Çağırma ASCmd](/sql/analysis-services/powershell/analysis-services-powershell-reference)|TMSL betiğini yürütün.|

## <a name="row-filters"></a>Satır filtreleri  

Bir tablodaki satırları belirli bir rolü üyeleri tarafından sorgulanabilir satır filtrelerini tanımlayın. Satır filtreleri, DAX formülleri kullanarak modeldeki her tablo için tanımlanır.  
  
Satır filtreleri, yalnızca rolleri okuma ve okuma için tanımlanabilir ve işlem izinleri. Belirli bir tablo için bir satır filtresi tanımlanmazsa başka bir tablodan çapraz filtreleme uygulanır sürece varsayılan olarak, tablodaki tüm satırları üyeleri sorgulayabilirsiniz.
  
 Satır filtreleri, belirli bir rolü üyeleri tarafından sorgulanabilir satırları tanımlamak için bir TRUE/FALSE değeri değerlendirilmelidir bir DAX formülü gerektirir. DAX formülü bulunmayan satırları sorgulanamıyor. Örneğin, aşağıdaki satır ile Müşteriler tablosunu ifadeyi filtreler *müşteriler [Country] = "ABD" =* , satış rolünün üyeleri, yalnızca ABD müşterileri görebilir.  
  
Satır filtreleri belirtilen satırları ve ilişkili satırları uygulanır. Bir tabloda birden çok ilişki varsa, güvenlik etkin ilişki için filtre uygulayın. Satır filtreleri ilişkili tablolar için örneğin tanımlanan diğer satır filtreleri ile kesiştiğinden:  
  
|Tablo|DAX ifadesi|  
|-----------|--------------------|  
|Bölge|= Bölge [Country] = "Tr"|  
|ProductCategory|= ProductCategory [Name] = "Bisiklet"|  
|İşlemler|= İşlemleri [yıl] 2016 =|  
  
 Üyeleri burada müşteri ABD'de, bisiklet ürün kategorisi olan ve 2016 yıldır veri satırı sorgulayabilirsiniz net etkisidir. Kullanıcılar bu izinler veren başka bir rol üyesi oldukları sürece, bisiklet veya işlemleri değil 2016'da olmayan işlemler işlemler ABD dışında sorgulayamaz.
  
 Filtre kullanabileceğiniz *=FALSE()* tablonun tamamını tüm satırlara erişimi reddetmek için.

## <a name="next-steps"></a>Sonraki adımlar

  [Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)   
  [Azure Analysis Services PowerShell ile yönetme](analysis-services-powershell.md)  
  [Tablosal Model betik dili (TMSL) başvurusu](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

