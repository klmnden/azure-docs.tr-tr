---
title: Performans ayarlama ve MySQL için Azure veritabanı'nda veritabanı bakımı için kullanım sys_schema nasıl
description: Bu makalede, performans sorunlarını bulun ve MySQL için Azure veritabanı sürdürmek için sys_schema kullanmayı açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/01/2018
ms.openlocfilehash: 993c77056c09c1dc21d5317ddbfe8e937341718d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61422385"
---
# <a name="how-to-use-sysschema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı performans ayarlama ve veritabanı bakımı için sys_schema kullanma

MySQL performance_schema, ilk MySQL 5. 5'da, kullanılabilir bellek ayırma, saklı programlar, kilitleme, vb. meta veri gibi çok sayıda önemli sunucu kaynakları için izleme sağlar. Ancak, performance_schema 80'den fazla tabloları içeren ve çoğunlukla gerekli bilgileri alınıyor information_schema tablolardan yanı sıra performance_schema tabloları birleştirme gerektirir. Performance_schema hem information_schema oluşturmak, sys_schema güçlü bir koleksiyonunu sağlayan [kullanıcı dostu görünümleri](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) salt okunur bir veritabanında ve sürüm 5.7 MySQL için Azure veritabanı'nda tam olarak etkinleştirilir.

![sys_schema görünümleri](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

İçinde sys_schema 52 görünüm vardır ve her görünüm aşağıdaki ön ekleri birine sahiptir:

- Host_summary veya GÇ: G/ç gecikme süreleriyle ilgili.
- Innodb: Innodb arabellek durumu ve kilitler.
- Bellek: Kullanıcılar ve ana bilgisayar bellek kullanımı.
- Şema: Şema ile ilgili bilgiler için otomatik artırma, dizinler, vb. gibi.
- deyimi: SQL deyimleri bilgiler; Bu, tam bir tablo taraması veya uzun sorgu süresi sonuçlanan ifade olabilir.
- Kullanıcı: Tüketilen ve kullanıcılara göre gruplandırılmış kaynaklar. Dosya g/ç, bağlantılar ve bellek verilebilir.
- Bekleme süresi: Ana bilgisayar veya kullanıcı tarafından gruplandırılmış olay bekleyin.

Artık sys_schema bazı ortak kullanım desenleri bakalım. Başlangıç olarak, biz kullanım düzenlerine iki kategoride Grup: **Performans ayarlama** ve **veritabanı bakım**.

## <a name="performance-tuning"></a>Performans ayarlama

### <a name="sysusersummarybyfileio"></a>*sys.user_summary_by_file_io*

GÇ veritabanında en pahalı bir işlemdir. İçin ortalama g/ç gecikme süresi sorgulayarak bulabiliriz *sys.user_summary_by_file_io* görünümü. Varsayılan 125 GB sağlanan depolama alanı, my g/ç gecikme süresi yaklaşık 15 saniyedir.

![GÇ gecikmesi: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

MySQL için Azure veritabanı, 1 TB'ye kadar sağlanan depolama artırma sonra depolama göre g/ç ölçeklendirilir çünkü 571 ms ile my g/ç gecikme süresini azaltır.

![GÇ gecikmesi: 1TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschematableswithfulltablescans"></a>*sys.schema_tables_with_full_table_scans*

Çok sayıda sorgu, dikkatli planlama rağmen yine de tam tablo taramaya sonuçlanabilir. Dizinler ve bunları en iyi duruma getirme türleri hakkında ek bilgi için bu makalede başvurabilir: [Sorgu performansı sorunlarını giderme](./howto-troubleshoot-query-performance.md). Tam tablo taramasından kaynak kullanımı yoğun ve veritabanınızın performansı düşürebilir. En hızlı yolu tam tablo taraması tablolarla bulmak için sorgu *sys.schema_tables_with_full_table_scans* görünümü.

![tam tablo tarama](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysusersummarybystatementtype"></a>*sys.user_summary_by_statement_type*

Veritabanı performans sorunlarını gidermek için veritabanınızın içinde gerçekleşen işlemleri ve kullanarak olayları tanımlamak avantajlı olabilir *sys.user_summary_by_statement_type* görünümü ux'in yalnızca yapabilir.

![deyimi tarafından özeti](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

Bu örnekte MySQL için Azure veritabanı 53 44579 kez slog sorgu günlüğü temizleme dakika ayırıyor. Uzun ve çok sayıda IOs olmasıdır. Bu etkinlik tarafından ya da devre dışı bırakma, yavaş sorgu günlüğü azaltmak veya yavaş sorgu, Azure portalında oturum açma sıklığını azaltın.

## <a name="database-maintenance"></a>Veritabanı bakımı

### <a name="sysinnodbbufferstatsbytable"></a>*sys.innodb_buffer_stats_by_table*

Innodb arabellek havuzu bellekte yer alıyor ve DBMS ve depolama alanı arasındaki ana önbellek mekanizmadır. Innodb arabellek havuzunun boyutunu, performans katmanına bağlıdır ve farklı bir ürünü SKU seçilir sürece değiştirilemez. Bellekte gibi işletim sistemi ile eski sayfaları kullanıma sahip olabileceksiniz veri yer açmak için değiştirilir. Innodb arabellek havuzu bellek birçok tabloları kullanma kullanıma bulmak için bunları sorgulayabilirsiniz *sys.innodb_buffer_stats_by_table* görünümü.

![Innodb arabellek durumu](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

Yukarıdaki grafikte Sistem tabloları ve görünümleri, her mysqldatabase033 veritabanı tablosunda dışında WordPress sitelerimi barındıran, 16 KB veya bellek veri 1 sayfa kapladığı olduğunu açıktır.

### <a name="sysschemaunusedindexes--sysschemaredundantindexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Dizinler için okuma performansını artırmak için harika Araçlar olsa da, bunlar ekler ve depolama için ek ücret yansıtılmaz. *Sys.schema_unused_indexes* ve *sys.schema_redundant_indexes* kullanılmayan veya yinelenen dizin hakkında ayrıntılı bilgiler sağlar.

![kullanılmayan dizin](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![yedekli dizinleri](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Sonuç

Özet olarak, sys_schema hem performans ayarlama ve veritabanı bakımı için harika bir araçtır. MySQL için Azure veritabanı'nda bu özelliğin avantajlarından aldığınızdan emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
- Eş en ilgili sorularınıza yanıt bulun veya yeni bir soru/yanıt post için şurayı ziyaret edin [MSDN Forumu](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
