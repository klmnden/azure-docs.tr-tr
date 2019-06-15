---
title: Azure SQL veritabanı dinamik veri maskeleme | Microsoft docs
description: SQL veritabanı dinamik veri maskeleme hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 03/04/2019
ms.openlocfilehash: 1db1535779d180994c9ce4350d11f4c696da9e3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64721541"
---
# <a name="sql-database-dynamic-data-masking"></a>SQL veritabanı dinamik veri maskeleme

SQL veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar. 

Dinamik veri maskeleme müşterilerin uygulama katmanını çok az etkileyerek hassas verilerin ne kadarının gösterileceğini belirlemelerini sağlar ve hassas verilere yetkisiz erişimin engellenmesine yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

Örneğin, çağrı merkezindeki temsilcisiyle çağıranlar, kredi kartı numarası birkaç basamak tarafından tanımlamaya başlayabilir, ancak veri öğelerin tam olarak temsilcisiyle sunulmamalıdır. Tüm maskeleri ancak sonuç tüm kredi kartı numarasının son dört rakamı herhangi bir sorgu kümesi bir maskeleme kuralı tanımlanabilir. Bir geliştirici uyumluluk düzenlemelerini ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilmesi başka bir örnek olarak, kişisel bilgileri (PII) verileri korumak için uygun veri maskesi tanımlanabilir.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL veritabanı dinamik veri maskeleme temelleri

Dinamik veri maskeleme işlemi SQL veritabanının yapılandırma dikey penceresinde ya da ayarlar dikey penceresi seçerek Azure portalında Bu ilkeyi maskeleme dinamik bir veri ayarlayın.

### <a name="dynamic-data-masking-permissions"></a>Dinamik veri maskeleme izinleri

Dinamik veri maskeleme, Azure SQL veritabanı yöneticisi, sunucu yöneticisi tarafından yapılandırılabilir veya [SQL Güvenlik Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rolleri.

### <a name="dynamic-data-masking-policy"></a>Dinamik veri maskeleme İlkesi

* **Maskeleme işlemi hariç tutulan SQL kullanıcıları** - kümesi SQL kullanıcıları veya SQL maskelenmemiş veri alma AAD kimlikleri sorgu sonuçları. Yönetici ayrıcalıklarına sahip kullanıcılar maskeleme işlemi her zaman hariç tutulur ve özgün veriler olmadan herhangi bir maskesi bakın.
* **Maskeleme kuralları** -bir dizi gizlenmeye belirlenen alanları tanımlayan kuralları ve kullanılan maskeleme işlevi. Veritabanı şema adı, tablo adını ve sütun adını kullanarak belirtilen alanlar tanımlanabilir.
* **İşlevleri maskeleme** -bir dizi farklı senaryolar için verilerin açığa denetleyen yöntemleri.

| Maskeleme işlevi | Maskeleme mantığı |
| --- | --- |
| **Varsayılan** |**Belirtilen alanları veri türlerine göre tam maskeleme**<br/><br/>• XXXX kullanın veya alan boyutu 4'ten az karakter dizesi veri türleriyle (n karakter, n metin, n değişken karakter) için ise daha az Xs.<br/>• Sıfır değeri (bigint, bit, decimal, int, para, sayısal, tamsayı, küçük para, Mini tamsayı, kayan, gerçek) sayısal veri türleri için kullanın.<br/>• 01-01-1900 kullanılmak tarih/saat veri türleri (tarih, datetime2, datetime, datetimeoffset, smalldatetime, süre).<br/>• SQL varyantı, varsayılan değer geçerli bir tür için kullanılır.<br/>XML belge • \<maskelenmiş / > kullanılır.<br/>• Özel veri türleri için boş bir değer kullanın (zaman damgası tablo, HierarchyId, GUID, ikili dosya, görüntü, varbinary uzamsal türler). |
| **Kredi kartı** |**Son dört rakamı atanan alanların sunan yöntemi maskeleme** ve kredi kartı biçiminde bir ön ek olarak bir sabit dizesi ekler.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-posta** |**İlk harfi ve etki alanı ile XXX.com değiştirir yöntemi maskeleme** bir sabit dize öneki biçiminde bir e-posta adresi kullanarak.<br/><br/>aXX@XXXX.com |
| **Rastgele sayı** |**Rastgele bir sayı oluşturan yöntemi maskeleme** göre gerçek veri türleri ve seçili sınırlar. Belirlenen sınırlar eşitse maskeleme işlevi olan bir sabit sayıdır.<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Özel metin** |**İlk ve son karakterler sunan yöntemi maskeleme** ve ortada bir özel doldurma dizesi ekler. Özgün dizeye sunulan önek ve sonek kısaysa, yalnızca doldurma dizesi kullanılır. <br/>ön ek [iç boşluk] son eki<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Maskelenmesi Önerilen alanlar

DDM hizmetinin öneriler motoru, veritabanınızdaki belirli alanları maskeleme için iyi adaylar olabilir hassas olabilecek alanlar olarak işaretler. Portalı'nda dinamik veri maskeleme dikey penceresinde, veritabanınız için önerilen sütunlar görürsünüz. Tek yapmak için ihtiyacınız olan tıklayın **maske Ekle** bir veya birden çok sütunun ve ardından **Kaydet** bu alanlar için bir maske uygulamak için.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dinamik veri maskeleme veritabanınız için PowerShell cmdlet'lerini kullanarak ayarlama

Bkz: [Azure SQL veritabanı cmdlet'leri](https://docs.microsoft.com/powershell/module/az.sql).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dinamik veri maskelemeyi REST API kullanarak veritabanınızı ayarlayın

Bkz: [işlemleri için Azure SQL veritabanı](https://docs.microsoft.com/rest/api/sql/).
