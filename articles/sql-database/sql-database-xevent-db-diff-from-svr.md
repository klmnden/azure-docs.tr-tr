---
title: Genişletilmiş olaylar SQL veritabanı'nda | Microsoft Docs
description: Azure SQL veritabanı'nda genişletilmiş olaylar (Xevent'ler), ve olay oturumları biraz Microsoft SQL Server'daki olay oturumlarından farklılıklar açıklanmıştır.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: 7f742b094575b78f453fb735b23cc5319a27fa7e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65206648"
---
# <a name="extended-events-in-sql-database"></a>SQL veritabanı'nda genişletilmiş olaylar
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Bu konu Azure SQL veritabanı'nda genişletilmiş olaylar uygulanmasının biraz daha farklı olmasına nasıl Microsoft SQL Server Genişletilmiş olaylar ile karşılaştırıldığında açıklar.

- SQL veritabanı V12 genişletilmiş olaylar özellik ikinci yarısında Takvim 2015 kavuştu.
- SQL Server 2008'den itibaren genişletilmiş olaylar oluşturdu.
- SQL veritabanı'nda genişletilmiş olaylar özellik kümesi, SQL Server'ın özellikleri hakkında güçlü bir alt kümesidir.

*Xevent'ler* blogları ve diğer resmi olmayan konumlara 'için genişletilmiş olaylar' bazen kullanılan resmi olmayan bir takma ad kullanılabilir.

Azure SQL veritabanı ve Microsoft SQL Server için genişletilmiş olaylar hakkında ek bilgi şuradan ulaşabilirsiniz:

- [Hızlı Başlangıç: SQL Server Genişletilmiş olaylar](https://msdn.microsoft.com/library/mt733217.aspx)
- [Genişletilmiş Olaylar](https://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Önkoşullar

Bu konuda, biraz bilgi zaten sahip olduğunuz varsayılır:

- [Azure SQL veritabanı hizmeti](https://azure.microsoft.com/services/sql-database/).
- [Genişletilmiş olaylar](https://msdn.microsoft.com/library/bb630282.aspx) Microsoft SQL Server.

- Belgelerimizi genişletilmiş olaylar hakkında toplu SQL Server ve SQL veritabanı için geçerlidir.

Olay dosyası olarak seçerken aşağıdaki öğeleri önceki maruz kalma riskinizi yararlı [hedef](#AzureXEventsTargets):

- [Azure depolama hizmeti](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Azure PowerShell kullanarak Azure depolama ile](../storage/common/storage-powershell-guide-full.md) -PowerShell ve Azure depolama hizmeti hakkında kapsamlı bilgi sağlar.

## <a name="code-samples"></a>Kod örnekleri

İlgili Konular iki kod örnekleri sağlar:


- [Halka arabelleği hedef kodu için SQL veritabanı'nda genişletilmiş olaylar](sql-database-xevent-code-ring-buffer.md)
    - Kısa basit Transact-SQL betiği.
    - Biz bir halka arabelleği hedef ile işiniz bittiğinde, kaynaklarını alter bırak yürüterek serbest bırakmalısınız, kod örnek konusunda vurgulamak `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` deyimi. Halka arabelleği ile başka bir örneği daha sonra ekleyebilirsiniz `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [SQL veritabanı'nda genişletilmiş olaylar için olay dosyası hedef kodu](sql-database-xevent-code-event-file.md)
    - 1\. Aşama, bir Azure depolama kapsayıcısı oluşturmak için powershell'dir.
    - 2\. Aşama, Azure depolama kapsayıcısını kullanan Transact-SQL ' dir.

## <a name="transact-sql-differences"></a>Transact-SQL farklılıkları


- Yürüttüğünüzde [olay OTURUMU oluşturma](https://msdn.microsoft.com/library/bb677289.aspx) komutunu SQL Server'da kullandığınız **ON SERVER** yan tümcesi. Ancak, kullandığınız SQL veritabanı'nda **üzerinde veritabanı** yan tümcesi bunun yerine.


- **Üzerinde veritabanı** yan tümcesi de uygulanır [ALTER olay OTURUMU](https://msdn.microsoft.com/library/bb630368.aspx) ve [bırakın olay oturumunu](https://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL komutlarını.


- Olay oturumu seçeneği, dahil etmek için en iyi uygulamadır **STARTUP_STATE = ON** içinde **olay OTURUMU oluşturma** veya **ALTER olay OTURUMU** deyimleri.
    - **= ON** değeri bir yeniden yapılandırma nedeniyle yük devretme mantıksal veritabanının sonra otomatik yeniden başlatma destekler.

## <a name="new-catalog-views"></a>Yeni Katalog görünümleri

Genişletilmiş olaylar özelliği birkaç tarafından desteklenen [Katalog görünümleri](https://msdn.microsoft.com/library/ms174365.aspx). Katalog görünümleri hakkında size bilgi *meta verileri veya tanımları* geçerli veritabanında kullanıcı tarafından oluşturulan olay oturumu. Görünümler, etkin olay oturumları örnekleri hakkında bilgi gitmez.

| Adı<br/>Katalog görünümü | Açıklama |
|:--- |:--- |
| **sys.database_event_session_actions** |Her bir olay oturumuna olayının üzerinde her eylem için bir satır döndürür. |
| **sys.database_event_session_events** |Bir olay oturumunda her olay için bir satır döndürür. |
| **sys.database_event_session_fields** |Olayları ve hedefler üzerinde açık olarak özelleştirilebilir her sütun için bir satır döndürür. |
| **sys.database_event_session_targets** |Her olay hedefi için bir satır için bir olay oturumu döndürür. |
| **sys.database_event_sessions** |SQL veritabanı veritabanındaki her bir olay oturumu için bir satır döndürür. |

Microsoft SQL Server'da benzer Katalog görünümleri içeren adları sahip *klasöründe\_*  yerine *.database\_* . Ad deseni benzer **sys.server_event_%** .

## <a name="new-dynamic-management-views-dmvshttpsmsdnmicrosoftcomlibraryms188754aspx"></a>Yeni dinamik yönetim görünümlerini [(Dmv'ler)](https://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL veritabanı olan [dinamik yönetim görünümlerini (Dmv'ler)](https://msdn.microsoft.com/library/bb677293.aspx) genişletilmiş olaylar destekler. Dmv'leri hakkında size bilgi *etkin* olay oturumları.

| DMV adı | Açıklama |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Olay oturumu eylemler hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_events** |Oturum olayları hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_object_columns** |Bir oturuma bağlı nesneler için yapılandırma değerlerini gösterir. |
| **sys.dm_xe_database_session_targets** |Oturum hedefleri hakkında bilgi döndürür. |
| **sys.dm_xe_database_sessions** |Bir satır kapsamı her bir olay oturumu için geçerli veritabanına döndürür. |

Microsoft SQL Server'da benzer Katalog görünümleri olmadan adlı  *\_veritabanı* adı kısmı gibi:

- **sys.dm_xe_sessions**, adı yerine<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>Ortak Dmv'ler
Genişletilmiş olaylar için Azure SQL veritabanı ve Microsoft SQL Server için ortak olan ek DMV vardır:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

  <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Genişletilmiş olayları, eylemleri ve hedefleri kullanılabilir Bul

Basit bir SQL çalıştırabileceğiniz **seçin** kullanılabilir olayları, eylemleri ve hedef listesi elde etmek için.

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>İçin SQL veritabanı olayı oturumlarına hedefleri

SQL veritabanı, etkinlik oturumlarına sonuçlardan yakalayabilirsiniz hedefleri şunlardır:

- [Halka arabelleği hedefine](https://msdn.microsoft.com/library/ff878182.aspx) -kısaca bellekte olay verileri tutar.
- [Sayaç hedefi olay](https://msdn.microsoft.com/library/ff878025.aspx) -bir genişletilmiş olaylar oturumu sırasında gerçekleşen tüm olayları sayar.
- [Olay dosya hedefine](https://msdn.microsoft.com/library/ff878115.aspx) -bir Azure depolama kapsayıcısı için tüm arabellekler yazar.

[Olay izleme için Windows (ETW)](https://msdn.microsoft.com/library/ms751538.aspx) API SQL veritabanı'nda genişletilmiş olaylar için uygun değildir.

## <a name="restrictions"></a>Kısıtlamalar

Birkaç SQL veritabanı'nın bulut ortamı befitting güvenlikle ilgili farklar vardır:

- Genişletilmiş olaylar tek kiracılı yalıtım modeline bulunmuş. Bir olay oturumuna bir veritabanındaki verileri veya olayları başka bir veritabanından erişemez.
- Veremez bir **olay OTURUMU oluşturma** deyimi bağlamında **ana** veritabanı.

## <a name="permission-model"></a>Bir izin modeli

Olmalıdır **denetimi** veritabanı izinlerini vermek için bir **olay OTURUMU oluşturma** deyimi. Veritabanı sahibinin (dbo) **denetimi** izni.

### <a name="storage-container-authorizations"></a>Depolama kapsayıcısı yetkilendirmeleri

Azure depolama kapsayıcınızda belirtmelisiniz için SAS belirteci oluşturmak **rwl** izinleri. **Rwl** değeri, aşağıdaki izinleri sağlar:

- Okuma
- Yazma
- Liste

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Genişletilmiş olaylar yoğun kullanımı için genel olarak sistemin iyi durumda olandan daha etkin bir bellek burada birikebilir senaryo vardır. Bu nedenle Azure SQL veritabanı sistem dinamik olarak ayarlar ve bir olay oturumu tarafından toplanabilir active bellek miktarını sınırlar ayarlar. Pek çok etken dinamik hesaplaması gidin.

En fazla bellek zorlandı bildiren bir hata iletisi alırsanız, uygulayabileceğiniz bazı düzeltici eylemler şunlardır:

- Daha az eş zamanlı etkinlik oturumları çalıştırın.
- Aracılığıyla, **Oluştur** ve **ALTER** olay oturumları için deyimleri, belirttiğiniz bellek miktarını azaltmak **MAX\_bellek** yan tümcesi.

### <a name="network-latency"></a>Ağ gecikmesi

**Olay dosyası** hedef ağ gecikmesi ya da hataları kalıcı veriler Azure depolama bloblarına yaşayabilir. Bunlar ağ iletişimi tamamlamak beklerken SQL veritabanı'nda diğer olayları gecikebilir. Bu gecikme, iş yükünüz yavaşlatabilir.

- Bu performans riski azaltmak için ayar önlemek **EVENT_RETENTION_MODE** seçeneğini **NO_EVENT_LOSS** olay oturumu tanımlarındaki.

## <a name="related-links"></a>İlgili bağlantılar

- [Azure PowerShell'i Azure Depolama'yla kullanma](../storage/common/storage-powershell-guide-full.md).
- [Azure depolama cmdlet'leri](https://docs.microsoft.com/powershell/module/Azure.Storage)
- [Azure PowerShell kullanarak Azure depolama ile](../storage/common/storage-powershell-guide-full.md) -PowerShell ve Azure depolama hizmeti hakkında kapsamlı bilgi sağlar.
- [Net'ten BLOB storage kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/ms189522.aspx)
- [CREATE EVENT SESSION (Transact-SQL)](https://msdn.microsoft.com/library/bb677289.aspx)
- [Microsoft SQL Server Genişletilmiş olaylar hakkında Jonathan Kehayias blog gönderileri](https://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- Azure *hizmet güncelleştirmeleri* Web sayfasını, Azure SQL veritabanı parametresi tarafından daraltıldığı:
    - [https://azure.microsoft.com/updates/?service=sql-database](https://azure.microsoft.com/updates/?service=sql-database)


Diğer kod örnek konuları genişletilmiş olaylar için aşağıdaki bağlantılarda kullanılabilir. Ancak, Microsoft Azure SQL veritabanı ve SQL Server örneği mı hedeflediği görmek için herhangi bir örnek düzenli olarak işaretlemeniz gerekir. Ardından, küçük değişiklikler örneği çalıştırmak için gerekli olup olmadığını karar verebilirsiniz.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
