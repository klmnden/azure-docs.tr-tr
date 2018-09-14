---
title: Azure SQL veritabanı dinamik veri maskeleme | Microsoft docs
description: SQL veritabanı dinamik veri maskeleme hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar
services: sql-database
author: ronitr
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: ronitr
ms.reviewer: vanto
ms.openlocfilehash: 92be9a54e5309670b51baa371d26406676778737
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542100"
---
# <a name="sql-database-dynamic-data-masking"></a>SQL veritabanı dinamik veri maskeleme

SQL veritabanı dinamik veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar. 

Dinamik veri maskeleme müşterilerin uygulama katmanını çok az etkileyerek hassas verilerin ne kadarının gösterileceğini belirlemelerini sağlar ve hassas verilere yetkisiz erişimin engellenmesine yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.

Örneğin, çağrı merkezindeki temsilcisiyle çağıranlar, kredi kartı numarası birkaç basamak tarafından tanımlamaya başlayabilir, ancak veri öğelerin tam olarak temsilcisiyle sunulmamalıdır. Tüm maskeleri ancak sonuç tüm kredi kartı numarasının son dört rakamı herhangi bir sorgu kümesi bir maskeleme kuralı tanımlanabilir. Bir geliştirici uyumluluk düzenlemelerini ihlal etmeden sorun giderme amacıyla üretim ortamlarında sorgulayabilmesi başka bir örnek olarak, kişisel bilgileri (PII) verileri korumak için uygun veri maskesi tanımlanabilir.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL veritabanı dinamik veri maskeleme temelleri
Dinamik veri maskeleme işlemi SQL veritabanının yapılandırma dikey penceresinde ya da ayarlar dikey penceresi seçerek Azure portalında Bu ilkeyi maskeleme dinamik bir veri ayarlayın.

### <a name="dynamic-data-masking-permissions"></a>Dinamik veri maskeleme izinleri
Dinamik veri maskeleme, Azure veritabanı yöneticisi, sunucu yöneticisi veya güvenlik yöneticisi rolü tarafından yapılandırılabilir.

### <a name="dynamic-data-masking-policy"></a>Dinamik veri maskeleme İlkesi
* **Maskeleme işlemi hariç tutulan SQL kullanıcıları** - kümesi SQL kullanıcıları veya SQL maskelenmemiş veri alma AAD kimlikleri sorgu sonuçları. Yönetici ayrıcalıklarına sahip kullanıcılar maskeleme işlemi her zaman hariç tutulur ve özgün veriler olmadan herhangi bir maskesi bakın.
* **Maskeleme kuralları** -bir dizi gizlenmeye belirlenen alanları tanımlayan kuralları ve kullanılan maskeleme işlevi. Veritabanı şema adı, tablo adını ve sütun adını kullanarak belirtilen alanlar tanımlanabilir.
* **İşlevleri maskeleme** -bir dizi farklı senaryolar için verilerin açığa denetleyen yöntemleri.

| Maskeleme işlevi | Maskeleme mantığı |
| --- | --- |
| **Varsayılan** |**Belirtilen alanları veri türlerine göre tam maskeleme**<br/><br/>• XXXX kullanın veya alan boyutu 4'ten az karakter dizesi veri türleriyle (n karakter, n metin, n değişken karakter) için ise daha az Xs.<br/>• Sıfır değeri (bigint, bit, decimal, int, para, sayısal, tamsayı, küçük para, Mini tamsayı, kayan, gerçek) sayısal veri türleri için kullanın.<br/>• 01-01-1900 kullanılmak tarih/saat veri türleri (tarih, datetime2, datetime, datetimeoffset, smalldatetime, süre).<br/>• SQL varyantı, varsayılan değer geçerli bir tür için kullanılır.<br/>XML belge • <masked/> kullanılır.<br/>• Özel veri türleri için boş bir değer kullanın (zaman damgası tablo, HierarchyId, GUID, ikili dosya, görüntü, varbinary uzamsal türler). |
| **Kredi kartı** |**Son dört rakamı atanan alanların sunan yöntemi maskeleme** ve kredi kartı biçiminde bir ön ek olarak bir sabit dizesi ekler.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-posta** |**İlk harfi ve etki alanı ile XXX.com değiştirir yöntemi maskeleme** bir sabit dize öneki biçiminde bir e-posta adresi kullanarak.<br/><br/>aXX@XXXX.com |
| **Rastgele sayı** |**Rastgele bir sayı oluşturan yöntemi maskeleme** göre gerçek veri türleri ve seçili sınırlar. Belirlenen sınırlar eşitse maskeleme işlevi olan bir sabit sayıdır.<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Özel metin** |**İlk ve son karakterler sunan yöntemi maskeleme** ve ortada bir özel doldurma dizesi ekler. Özgün dizeye sunulan önek ve sonek kısaysa, yalnızca doldurma dizesi kullanılır. <br/>ön ek [iç boşluk] son eki<br/><br/>![Gezinti Bölmesi](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Maskelenmesi önerilen alanlar
DDM hizmetinin öneriler motoru, veritabanınızdaki belirli alanları maskeleme için iyi adaylar olabilir hassas olabilecek alanlar olarak işaretler. Portalı'nda dinamik veri maskeleme dikey penceresinde, veritabanınız için önerilen sütunlar görürsünüz. Tek yapmak için ihtiyacınız olan tıklayın **maske Ekle** bir veya birden çok sütunun ve ardından **Kaydet** bu alanlar için bir maske uygulamak için.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dinamik veri maskeleme veritabanınız için Powershell cmdlet'lerini kullanarak ayarlama
Bkz: [Azure SQL veritabanı cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.sql).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dinamik veri maskelemeyi REST API kullanarak veritabanınızı ayarlayın
Bkz: [işlemleri için Azure SQL veritabanı](https://msdn.microsoft.com/library/dn505719.aspx).

