---
title: "Performans ayarlama ve MySQL için Azure veritabanında veritabanı bakımı için kullanım sys_schema nasıl"
description: "Bu makalede sys_schema performans sorunları bulmak ve Azure veritabanında veritabanı için MySQL sürdürmek için nasıl kullanılacağını açıklar."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 2e5b6b859df06d686a97fc1b134da8d66df6783e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-use-sysschema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>MySQL için Azure veritabanında performans ayarlama ve veritabanı bakımı için sys_schema kullanma

MySQL performance_schema, ilk MySQL 5. 5'da, kullanılabilir bellek ayırma, depolanan programlar, meta veri kilitleme, vb. gibi birçok önemli sunucu kaynakları için araçları sağlar. Ancak, 80'den fazla tablo performance_schema içerir ve genellikle gerekli bilgileri alma INFORMATION_SCHEMA tablolardan yanı sıra performance_schema içindeki tabloları birleştirme gerektirir. Hem performance_schema hem de INFORMATION_SCHEMA oluşturmak, sys_schema güçlü bir koleksiyonunu sağlar [kullanıcı dostu görünümleri](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) salt okunur bir veritabanında ve Azure veritabanı'nda sürüm 5.7 MySQL için tam olarak etkinleştirilmedi.

![sys_schema görünümleri](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

Sys_schema 52 görünüm vardır ve her görünüm aşağıdaki önekinden birini içerir:

- Host_summary veya g/ç: g/ç, gecikme ilgili.
- InnoDB: InnoDB arabellek durumu ve kilitler.
- Bellek: Ana bilgisayar ve kullanıcı bellek kullanımı.
- Şema: Şema ile ilgili bilgileri otomatik artış, dizinler, vb. gibi.
- Deyimi: SQL deyimleri bilgi; tam tablo taraması veya uzun sorgu süresi sonuçlandı deyimi olabilir.
- Kullanıcı: Kaynaklar tüketilen ve kullanıcılara göre gruplandırılmış. Dosya g/ç, bağlantıları ve bellek örnektir.
- Bekleme: ana bilgisayar veya kullanıcı tarafından gruplandırılmış olayları bekleyin.

Şimdi sys_schema bazı yaygın kullanım desenlerini bakalım. Başından itibaren biz kullanım desenlerini iki kategoriler halinde gruplandırabilirsiniz: **performans ayarlaması** ve **veritabanı bakım**.

## <a name="performance-tuning"></a>Performans ayarı

### <a name="sysusersummarybyfileio"></a>*sys.user_summary_by_file_io*

G/ç veritabanında en pahalı bir işlemdir. Biz sorgulayarak ortalama g/ç gecikmesi bulabilirsiniz *sys.user_summary_by_file_io* görünümü. Varsayılan 125 sağlanan depolama Alanım g/ç gecikmesi yaklaşık 15 saniye GB'dir.

![g/ç gecikmesi: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

Azure veritabanı MySQL için sağlanan depolama 1 TB'yükseltilince g/ç depolama göre ölçeklendirir çünkü 571 26 X performans artışı temsil eden ms için my GÇ gecikmesini azaltır!

![g/ç gecikmesi: 1TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschematableswithfulltablescans"></a>*sys.schema_tables_with_full_table_scans*

Dikkatli planlama rağmen çok sayıda sorgu hala tam tablo taramasına neden olabilir. Dizinler ve bunları en iyi duruma getirme türleri hakkında ek bilgi için bu makalede başvurabilir: [sorgu performans sorunlarını gidermek nasıl](./howto-troubleshoot-query-performance.md). Tam tablo tarama yoğun bir kaynak ve veritabanı performansını düşürebilir. Tam tablo taraması tablolarla bulmak için en hızlı yolu sorgu *sys.schema_tables_with_full_table_scans* görünümü.

![tam tablo tarama](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysusersummarybystatementtype"></a>*sys.user_summary_by_statement_type*

Veritabanı performans sorunlarını gidermek için bu veritabanınızı içinde gerçekleştiği ve kullanarak olayları tanımlamak faydalı olabilir *sys.user_summary_by_statement_type* görünüm eli yalnızca yapabilir.

![deyimi tarafından özeti](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

Bu örnekte, 53 44579 kez slog sorgu günlüğü temizleme dakika Azure veritabanı MySQL için harcanan. Uzun bir süre ve birçok IOs olmasıdır. Bu etkinlik tarafından devre dışı bırakın, yavaş sorgu günlüğü azaltın veya yavaş sorgu oturum açma Azure portal sıklığını azaltın.

## <a name="database-maintenance"></a>Veritabanı bakımı

### <a name="sysinnodbbufferstatsbytable"></a>*sys.innodb_buffer_stats_by_table*

InnoDB arabellek havuzu bellekte bulunur ve DBMS ve depolama alanı arasındaki ana önbellek mekanizmadır. InnoDB arabellek havuzu boyutu performans katmanına bağlıdır ve farklı bir Ürün SKU seçilir sürece değiştirilemez. Bellekte gibi işletim sistemi ile eski sayfaları taze veriler için yer açmak için takas. Hangi tabloları InnoDB arabellek havuzu belleğin çoğunu tüketen çıkışı bulmak için Sorgulayabileceğiniz *sys.innodb_buffer_stats_by_table* görünümü.

![InnoDB arabellek durumu](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

Yukarıdaki grafikte Sistem tabloları ve görünümleri, her mysqldatabase033 veritabanı tablosunda dışında WordPress Sitelerim birini barındıran, 16 KB veya veri bellekte 1 sayfa kapladığı olduğunu açıktır.

### <a name="sysschemaunusedindexes--sysschemaredundantindexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Dizinler için okuma performansı artırmak için harika Araçlar olsa da, ekler ve depolama için ek ücrete neden. *Sys.schema_unused_indexes* ve *sys.schema_redundant_indexes* kullanılmayan veya yinelenen dizinleri fikir sağlar.

![kullanılmayan dizinler](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![yedek dizinler](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Sonuç

Özet olarak, sys_schema hem performans ayarlama ve veritabanı bakımı için harika bir araçtır. Bu özellik Azure veritabanı'nda MySQL için yararlanmak emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
- Eş sorularınıza en ilgili bulma veya yeni bir soru/yanıt göndermek için ziyaret [MSDN Forumu](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure-database-mysql).
