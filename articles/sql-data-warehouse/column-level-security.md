---
title: Azure SQL veri ambarı sütun düzeyinde güvenlik | Microsoft Docs
description: Sütun düzeyi güvenlik (CLS), kullanıcının yürütme bağlamı veya grup üyeliklerini temel veritabanı tablo sütunları erişimi denetlemek müşterilerin sağlar. Tasarım ve uygulama güvenlik kodlama CLS basitleştirir. CLS sütun erişim kısıtlamaları uygulamak sağlar.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: aa91bd586e064239d0e05c754427947963c9ee3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082819"
---
# <a name="column-level-security"></a>Sütun düzeyinde güvenlik
Sütun düzeyi güvenlik (CLS), kullanıcının yürütme bağlamı veya grup üyeliklerini temel veritabanı tablo sütunları erişimi denetlemek müşterilerin sağlar.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]

Tasarım ve uygulama güvenlik kodlama CLS basitleştirir. CLS, hassas verileri korumak için sütun erişim kısıtlamaları uygulamak sağlar. Örneğin, belirli kullanıcılara yalnızca belirli departmanı için uygun bir tablonun sütunlarını eriştiğinden emin olun. Veritabanı katmanı bulunur yerine koy verileri başka bir uygulama katmanı kullanarak erişimi kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanına erişim kısıtlamalarını uygular. Bu kısıtlama, genel güvenlik sistem'ın yüzey alanını azaltarak daha güvenli ve sağlam güvenlik sağlar. Ayrıca, CLS Ayrıca kullanıcıların erişim kısıtlamalarını izlenmesi için sütunları filtrelemek için görünümler tanıtımı için ihtiyacını ortadan kaldırır.

CLS ile uygulayabileceğine [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) T-SQL deyimi. Bu mekanizma ile hem SQL hem de Azure Active Directory (AAD) kimlik doğrulaması desteklenir.

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
Aşağıdaki örnek, 'TestUser', 'Üyelik' tablosunun 'SSN' sütunu erişimini kısıtlamak gösterilmektedir:

Sosyal Güvenlik numaraları depolamak için kullanılan SSN sütunuyla 'Üyelik' tablo oluşturun:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

'TestUser' izin hassas veriler içeren SSN sütun dışındaki tüm sütunları erişmek için:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

'TestUser' SSN sütun içeriyorlarsa başarısız olacağından yürütülen sorgular:

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Kullanım Örnekleri
CLS bugün nasıl kullanıldığını ilişkin bazı örnekler:
- Finansal hizmet şirketi yalnızca hesap yöneticileri müşteri sosyal güvenlik numarası (SSN), telefon numaraları ve diğer kişisel bilgileri (PII) erişmesini sağlar.
- Sağlık Hizmetleri Sağlayıcısı yalnızca Doktorlar ve nurses fatura departmanı üyeleri sağlanmıyor hassas sağlık kayıtlarına erişim sağlamak bu verileri görüntüleme olanak sağlar.
