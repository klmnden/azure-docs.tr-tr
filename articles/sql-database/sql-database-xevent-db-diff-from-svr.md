---
title: "SQL veritabanı olaylar genişletilmiş | Microsoft Docs"
description: "Azure SQL veritabanında genişletilmiş olaylar (Xevent'ler) ve nasıl olay oturumları biraz Microsoft SQL Server'ın olay oturumlarından farklılıklar açıklanmıştır."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 3b28cf15-f820-4b3c-8310-908d6d5b9d0c
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 7e5da1c32484b0b94d2ad32ead6bb7c28f9744aa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="extended-events-in-sql-database"></a>SQL veritabanında genişletilmiş olaylar
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Bu konuda Microsoft SQL Server Genişletilmiş olaylara göre nasıl genişletilmiş olaylar Azure SQL veritabanında biraz farklı uygulamasıdır açıklanmaktadır.

- SQL Database V12 genişletilmiş olaylar özelliği ikincide Takvim 2015 yarısı kazanılan.
- SQL Server 2008'den beri genişletilmiş olaylar oluşturdu.
- SQL veritabanı üzerinde Genişletilmiş olaylar özellik kümesini sağlam bir SQL Server'ın özellikleri alt kümesidir.

*Xevent'ler* 'için genişletilmiş olayları' bazen kullanılan bir resmi olmayan takma blogları ve diğer resmi olmayan konumlara adıdır.

Azure SQL Database ve Microsoft SQL Server için genişletilmiş olaylar hakkında ek bilgi şu adresten edinilebilir:

- [Hızlı Başlangıç: SQL Server Genişletilmiş olaylar](http://msdn.microsoft.com/library/mt733217.aspx)
- [Genişletilmiş olaylar](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Ön koşullar

Bu konu, zaten bazı bilgisine sahip varsayar:

- [Azure SQL veritabanı hizmetinin](https://azure.microsoft.com/services/sql-database/).
- [Olaylar Genişletilmiş](http://msdn.microsoft.com/library/bb630282.aspx) Microsoft SQL Server'da.

- Belgelerimizi genişletilmiş olaylar hakkında toplu SQL Server ve SQL veritabanı için geçerlidir.

Olay dosyası olarak seçerken aşağıdaki öğeleri önceki maruz yararlıdır [hedef](#AzureXEventsTargets):

- [Azure depolama hizmeti](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Azure Storage ile Azure PowerShell'i kullanarak](../storage/common/storage-powershell-guide-full.md) -PowerShell ve Azure depolama hizmeti hakkında kapsamlı bilgi sağlar.

## <a name="code-samples"></a>Kod örnekleri

İlgili Konular iki kod örnekleri sağlar:


- [Halka arabelleği hedef kod SQL veritabanında genişletilmiş olaylar](sql-database-xevent-code-ring-buffer.md)
    - Kısa basit Transact-SQL komut dosyası.
    - Biz halka arabelleği hedefle bittiğinde, kaynaklarını alter bırak yürüterek serbest bırakmalısınız, kod örnek konudaki vurgulamak `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` deyimi. Halka arabelleği ile başka bir örneği daha sonra ekleyebilirsiniz `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [SQL veritabanı genişletilmiş olaylar için olay dosya hedef kodu](sql-database-xevent-code-event-file.md)
    - 1 bir Azure depolama kapsayıcısı oluşturmak için PowerShell aşamasıdır.
    - Aşama 2 Azure Storage kapsayıcısını kullanan Transact-SQL ' dir.

## <a name="transact-sql-differences"></a>Transact-SQL farklılıkları


- Çalıştırdığınızda [oluşturma olay OTURUMU](http://msdn.microsoft.com/library/bb677289.aspx) kullandığınız SQL Server üzerinde komut **ON SERVER** yan tümcesi. Ancak SQL veritabanında kullandığınız **ON veritabanı** yan tümcesi yerine.


- **ON veritabanı** yan tümcesi de uygulanır [ALTER olay OTURUMU](http://msdn.microsoft.com/library/bb630368.aspx) ve [bırakma olay OTURUMU](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL komutları.


- Olay oturumu seçeneği, dahil etmek için en iyi uygulamadır **STARTUP_STATE ON =** içinde **oluşturma olay OTURUMU** veya **ALTER olay OTURUMU** deyimleri.
    - **ON =** değeri, bir yük devretme nedeniyle mantıksal veritabanı yapılandırılmasına sonra otomatik yeniden başlatma destekler.

## <a name="new-catalog-views"></a>Yeni Katalog görünümleri

Genişletilmiş olaylar özelliği birkaçı tarafından desteklenir [Katalog görünümleri](http://msdn.microsoft.com/library/ms174365.aspx). Katalog görünümleri hakkında size bildirmek *meta verileri veya tanımları* kullanıcı tarafından oluşturulan olay oturumlarının geçerli veritabanında. Görünümleri etkin olay oturumları örnekleri hakkında bilgi döndürmeyin.

| Adı<br/>Katalog görünümü | Açıklama |
|:--- |:--- |
| **sys.database_event_session_actions** |Her bir olay oturumu olayda her eylem için bir satır döndürür. |
| **sys.database_event_session_events** |Her olay için bir satır, bir olay oturumunda döndürür. |
| **sys.database_event_session_fields** |Olayları ve hedefleri açık olarak ayarlanıp özelleştirme-mümkün her sütun için bir satır döndürür. |
| **sys.database_event_session_targets** |Her olay hedefi için bir satır için bir olay oturumu döndürür. |
| **sys.database_event_sessions** |SQL veritabanı veritabanındaki her olay oturumu için bir satır döndürür. |

Microsoft SQL Server'da benzer Katalog görünümleri içeren adları sahip *klasöründe\_*  yerine *.database\_*. Ad deseni benzer **sys.server_event_%**.

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Yeni dinamik yönetim görünümlerini [(Dmv'leri)](http://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL veritabanı sahip [dinamik yönetim görünümlerini (Dmv'leri)](http://msdn.microsoft.com/library/bb677293.aspx) genişletilmiş olaylar destekler. Dmv'leri hakkında size bildirmek *etkin* olay oturumları.

| DMV adı | Açıklama |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Olay oturumu eylemler hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_events** |Oturum olaylar hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_object_columns** |Bir oturuma bağlı olan nesneleri için yapılandırma değerlerini gösterir. |
| **sys.dm_xe_database_session_targets** |Oturum hedefleri hakkında bilgi verir. |
| **sys.dm_xe_database_sessions** |Bir satır kapsamlıdır her bir olay oturumu için geçerli veritabanına döndürür. |

Microsoft SQL Server'da benzer Katalog görünümleri olmadan adlı  *\_veritabanı* adı kısmını gibi:

- **sys.dm_xe_sessions**, adı yerine<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>Dmv'leri ortak
Genişletilmiş olaylar için Azure SQL Database ve Microsoft SQL Server için ortak olan ek Dmv'leri vardır:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Olaylar, Eylemler ve hedefleri genişletilmiş kullanılabilir Bul

Basit bir SQL çalıştırabilirsiniz **seçin** kullanılabilir olaylar, eylemleri ve hedef listesini elde etmek için.

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

## <a name="targets-for-your-sql-database-event-sessions"></a>SQL veritabanı olay oturumları için hedefleri

SQL veritabanı üzerinde olay oturumları sonuçlarından yakalayabilirsiniz hedefleri şunlardır:

- [Halka arabelleği hedef](http://msdn.microsoft.com/library/ff878182.aspx) -kısaca bellekte olay verileri tutar.
- [Olay sayaç hedef](http://msdn.microsoft.com/library/ff878025.aspx) -genişletilmiş olaylar oturumu sırasında oluşan tüm olaylar sayar.
- [Olay dosya hedef](http://msdn.microsoft.com/library/ff878115.aspx) -bir Azure depolama kapsayıcısı için tam arabellek yazar.

[Olay izleme için Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API SQL veritabanı üzerinde Genişletilmiş olaylar için kullanılabilir değil.

## <a name="restrictions"></a>Kısıtlamaları

Birkaç SQL veritabanının bulut ortamı befitting güvenlikle ilgili farklar vardır:

- Genişletilmiş olaylar tek Kiracı yalıtımı model üzerinde kurulan. Bir veritabanında bir olay oturumu başka bir veritabanından veri ve olayları erişemez.
- Veremez bir **oluşturma olay OTURUMU** deyimi bağlamında **ana** veritabanı.

## <a name="permission-model"></a>İzni modeli

Sahip olmanız gerekir **denetim** izin vermek için veritabanında bir **oluşturma olay OTURUMU** deyimi. Veritabanı sahibi (dbo) sahip **denetim** izni.

### <a name="storage-container-authorizations"></a>Depolama kapsayıcısı yetkilerini

Azure depolama kapsayıcısının belirtmelisiniz için oluşturduğunuz SAS belirteci **rwl** izinler için. **Rwl** değeri, aşağıdaki izinleri sağlar:

- Okuma
- Yazma
- Liste

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Yoğun kullanımını genişletilmiş olaylar için genel sistem sağlıklı olandan daha etkin bellek burada birikebilir senaryo vardır. Bu nedenle Azure SQL veritabanı sistem dinamik olarak ayarlar ve bir olay oturumu tarafından toplanabilir etkin bellek miktarını sınırları ayarlar. Birçok faktöre dinamik hesaplama gidin.

En fazla bellek zorlanmış bildiren bir hata iletisi alırsanız, bazı düzeltici eylemleri şunlardır:

- Daha az sayıda eşzamanlı olay oturumları çalıştırın.
- Aracılığıyla, **oluşturma** ve **ALTER** deyimleri olay oturumları için sizin belirttiğiniz bellek miktarını azaltmak **MAX\_bellek** yan tümcesi.

### <a name="network-latency"></a>Ağ gecikmesi

**Olay dosyası** hedef ağ gecikmesi veya hatalar için Azure Storage bloblarını kalıcı veriler deneyimi. SQL veritabanı diğer olaylarla tamamlamak ağ iletişimi için beklerken gecikebilir. Bu gecikme, iş yükü yavaşlatabilir.

- Bu performans riski azaltmak için ayarı kaçının **EVENT_RETENTION_MODE** için seçenek **NO_EVENT_LOSS** olay oturumu tanımlarında.

## <a name="related-links"></a>İlgili bağlantılar

- [Azure Storage ile Azure PowerShell'i kullanma](../storage/common/storage-powershell-guide-full.md).
- [Azure depolama cmdlet'leri](http://msdn.microsoft.com/library/dn806401.aspx)
- [Azure Storage ile Azure PowerShell'i kullanarak](../storage/common/storage-powershell-guide-full.md) -PowerShell ve Azure depolama hizmeti hakkında kapsamlı bilgi sağlar.
- [BLOB depolama alanından .NET kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Olay OTURUMU (Transact-SQL) oluşturma](http://msdn.microsoft.com/library/bb677289.aspx)
- [Microsoft SQL Server'da genişletilmiş olaylar hakkında Jonathan Kehayias blog yazılarını](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- Azure *hizmet güncelleştirmeleri* Web sayfası, Azure SQL veritabanı parametresi tarafından daraltıldığı:
    - [https://Azure.microsoft.com/Updates/?Service=SQL-Database](https://azure.microsoft.com/updates/?service=sql-database)


Aşağıdaki bağlantılarda diğer kod örnek konuları genişletilmiş olaylar için kullanılabilir. Ancak, örnek Microsoft SQL Server Azure SQL veritabanına karşı hedefler olup olmadığını görmek için herhangi bir örnek düzenli olarak denetlemeniz gerekir. Ardından, küçük değişiklikler örneği çalıştırmak için gerekli olup olmadığını karar verebilirsiniz.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
