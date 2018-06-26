---
title: Azure SQL Data Warehouse sütun düzeyinde güvenlik | Microsoft Docs
description: Sütun düzeyi güvenlik (CLS) kullanıcının yürütme bağlamı veya grup üyeliklerini temel alan veritabanı tablo sütunları erişimi denetlemek müşterilerin sağlar. CLS tasarım ve uygulamanızda güvenlik kodlama basitleştirir. CLS sütun erişim kısıtlamalarını uygulamak sağlar.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 06/15/2018
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: 5a916132f705f3c517ee6789b61a3972b2445b62
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938519"
---
# <a name="column-level-security"></a>Sütun düzeyinde güvenlik 
Sütun düzeyi güvenlik (CLS) kullanıcının yürütme bağlamı veya grup üyeliklerini temel alan veritabanı tablo sütunları erişimi denetlemek müşterilerin sağlar.  

CLS tasarım ve uygulamanızda güvenlik kodlama basitleştirir. CLS önemli verileri korumak için sütun erişim kısıtlamalarını uygulamak sağlar. Örneğin, belirli kullanıcıları yalnızca kendi departmanı ile ilgili bir tablodaki belirli sütunların eriştiğinden emin olun. Veritabanı katmanı bulunur yerine başka bir uygulama katmanındaki veriler koymadan erişim kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı erişim kısıtlamaları geçerlidir. Bu güvenlik sisteminizde daha güvenli ve sağlam genel güvenlik sisteminizi'nın yüzey alanını azaltarak hale getirir. Ayrıca, bu da kullanıcılara erişim kısıtlamalarını izlenmesi için sütunları filtrelemek için görünümler sunarak gereğini ortadan kaldırır. 

CLS ile uygulama [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL ifadesi. Bu mekanizma ile hem SQL hem de Azure Active Directory (AAD) kimlik doğrulama desteklenir.

![CLS](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sözdizimi 

```sql
GRANT <permission> [ ,...n ] ON    
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]   
    TO <database_principal> [ ,...n ]    
    [ WITH GRANT OPTION ]   
    [ AS <database_principal> ]   
<permission> ::=   
    SELECT 
  | UPDATE  
<database_principal> ::=    
      Database_user    
    | Database_role    
    | Database_user_mapped_to_Windows_User    
    | Database_user_mapped_to_Windows_Group    
```

## <a name="example"></a>Örnek 
Aşağıdaki örnek 'TestUser' 'Üyelik' tablosunun 'SSN' sütunu erişimini kısıtlamak nasıl gösterir: 

'Üyelik' tablo sosyal güvenlik numarası depolamak için kullanılan SSN sütunla oluşturun:

```sql
CREATE TABLE Membership   
  (MemberID int IDENTITY,   
   FirstName varchar(100) NULL,   
   SSN char(9) NOT NULL, 
   LastName varchar(100) NOT NULL,   
   Phone varchar(12) NULL,   
   Email varchar(100) NULL);  
```

'TestUser' izin hassas verileri içeren SSN sütun dışındaki tüm sütunları erişmek için: 

```sql  
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;   
``` 

Sorgular: SSN sütun içeriyorlarsa 'TestUser' başarısız

```sql  
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'. 
``` 

## <a name="use-cases"></a>Kullanım örnekleri
CLS bugün nasıl kullanıldığını ilişkin bazı örnekler: 
- Finansal Hizmetler kesin yalnızca hesap yöneticileri müşteri sosyal güvenlik numarası (SSN), telefon numaraları ve diğer kişisel bilgileri (PII) erişimi olmasını sağlar.
- Sağlık sağlayıcısı yalnızca Doktorlar ve nurses hassas tıbbi kayıtları fatura departmanı üyeleri sağlanmıyor erişimi bu verileri görüntüleme izin verir.
