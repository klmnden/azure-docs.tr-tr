---
title: Azure SQL veritabanını dinamik veri maskeleme | Microsoft docs
description: SQL veritabanı dinamik veri maskeleme ayrıcalıklı olmayan kullanıcılara maskeleyerek gizli verilerin açığa sınırlar.
services: sql-database
author: ronitr
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 04/01/2018
ms.author: ronitr
ms.openlocfilehash: 4d948786d70ffc1350825b04dbddc128cf89466e
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="sql-database-dynamic-data-masking"></a>SQL veritabanı dinamik veri maskeleme

SQL veritabanı dinamik veri maskeleme ayrıcalıklı olmayan kullanıcılara maskeleyerek gizli verilerin açığa sınırlar. 

Dinamik veri maskeleme müşterilerin uygulama katmanını çok az etkileyerek hassas verilerin ne kadarının gösterileceğini belirlemelerini sağlar ve hassas verilere yetkisiz erişimin engellenmesine yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

Örneğin, çağrı merkezinde bir temsilcisiyle arayanlar, kredi kartı numarası birkaç haneli tanımlayabilir, ancak veri öğelerden tam olarak temsilcisiyle açılmamalıdır değil. Maskeleri tüm ancak sonuç herhangi kredi kartı numarasının son dört rakamı herhangi bir sorgu kümesi bir maskeleme kuralı tanımlanabilir. Bir geliştirici uyumluluk düzenlemeleri ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilmesi başka bir örnek olarak, kişisel bilgileri (PII) verileri korumak için uygun veri maskesi tanımlanabilir.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL veritabanı dinamik veri maskeleme temelleri
Dinamik veri dinamik veri maskeleme işlemi SQL veritabanı yapılandırma dikey veya ayarları dikey penceresini seçerek Azure portalındaki ilke maskeleme ayarlayın.

### <a name="dynamic-data-masking-permissions"></a>Dinamik veri maskeleme izinleri
Dinamik veri maskeleme Azure veritabanı yönetici, sunucu yöneticisi veya güvenlik yetkilisi rolleri tarafından yapılandırılabilir.

### <a name="dynamic-data-masking-policy"></a>Dinamik veri maskeleme İlkesi
* **Maskeleme işlemi için hariç tutulan SQL kullanıcıları** - kümesi SQL kullanıcılarının veya SQL'de maskelenmemiş Veri Al AAD kimlikleri sorgu sonuçları. Yönetici ayrıcalıklarına sahip kullanıcılar maskeleme işlemi her zaman hariç tutulur ve özgün veriler herhangi bir maskesi olmadan bakın.
* **Kuralları maskeleme** -bir dizi maskelenecek belirlenmiş alanları tanımlayan kurallar ve kullanılan maskeleme işlevi. Veritabanı şema adı, tablo adını ve sütun adını kullanarak belirtilen alanlar tanımlanabilir.
* **İşlevler maskeleme** -bir dizi farklı senaryolar için veri riskini kontrol yöntem.

| Maskeleme işlevi | Maskeleme mantığı |
| --- | --- |
| **Varsayılan** |**Tam maskeleme göre belirlenen alanlarının veri türleri**<br/><br/>• Kullanım XXXX veya alan boyutu 4'ten az karakter dizesi veri türleriyle (nchar, ntext, nvarchar) için ise daha az Xs.<br/>• (Bigint, bit, ondalık, int, para, sayısal, smallint, küçük para, Mini tamsayı, kayan nokta, gerçek) sayısal veri türleri için sıfır değeri kullanın.<br/>• 01-01-1900 tarih/saat veri türleri (tarihi, datetime2, datetime, datetimeoffset, smalldatetime, saat) kullanın.<br/>• SQL variant, geçerli türünün varsayılan değeri için kullanılır.<br/>• XML belge için <masked/> kullanılır.<br/>• Özel veri türleri için boş bir değer kullanın (zaman damgası tablo, hierarchyid, GUID, ikili, görüntü, değişken ikili uzamsal türler). |
| **Kredi kartı** |**Belirtilen alanları son dört rakamı gösterir yöntemi maskeleme** ve kredi kartı biçiminde öneki olarak bir sabit dize ekler.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-posta** |**İlk harfi kullanıma sunar ve etki alanı ile XXX.com değiştirir yöntemi maskeleme** bir sabit dize öneki biçiminde bir e-posta adresi kullanarak.<br/><br/>aXX@XXXX.com |
| **Rastgele sayı** |**Rastgele bir sayı oluşturur yöntemi maskeleme** göre seçilen sınırları ve gerçek veri türleri. Ardından maskeleme işlevi belirlenen sınırlar eşitse, sabit bir sayıdır.<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Özel metin** |**İlk ve son karakterler sunan yöntemi maskeleme** ve ortadaki özel doldurma dize ekler. Özgün dizeye gösterilen önek ve sonek kısa ise yalnızca doldurma dize kullanılır. <br/>önek [doldurma] sonek<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Maskelenmesi önerilen alanlar
DDM önerileri altyapısı belirli alanları veritabanınızdan maskeleme için iyi adaylar olabilir olası hassas alanları olarak işaretler. Dinamik veri maskeleme dikey penceresinde Portalı'nda, veritabanınız için önerilen sütunları görürsünüz. Yapmanız gereken tek şey tıklatın **Maskesi Ekle** bir veya daha fazla sütun için ve ardından **kaydetmek** bu alanlar için bir maskesi uygulamak için.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dinamik veri maskeleme Powershell cmdlet'lerini kullanarak, veritabanınız için ayarlama
Bkz: [Azure SQL veritabanı cmdlet'leri](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>REST API kullanarak veritabanınız için dinamik veri maskeleme ayarlama
Bkz: [işlemleri Azure SQL veritabanları için](https://msdn.microsoft.com/library/dn505719.aspx).

