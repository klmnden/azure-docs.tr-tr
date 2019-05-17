---
title: – Hiper ölçekli (Citus) (Önizleme) PostgreSQL için Azure veritabanı ile gerçek zamanlı bir Panoda tasarım Öğreticisi
description: Bu öğreticide, PostgreSQL hiper ölçekli (Citus) (Önizleme) için Azure veritabanı üzerinde dağıtılmış tablolar sorgu oluşturmak ve doldurmak gösterilir.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: a5e4b2073a29785ee851b2733c12d6331afe59d8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791327"
---
# <a name="tutorial-design-a-real-time-analytics-dashboard-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Öğretici: Gerçek zamanlı analiz Panosu PostgreSQL hiper ölçekli (Citus) (Önizleme) kullanarak Azure veritabanı tasarlama

Bu öğreticide, PostgreSQL - öğrenmek için hiper ölçekli (Citus) (Önizleme) için Azure veritabanı kullanan nasıl yapılır:

> [!div class="checklist"]
> * Bir hiper ölçekli (Citus) sunucu grubu oluşturma
> * Bir şema oluşturmak için psql yardımcı programı kullanın.
> * Düğümlerde parça tabloları
> * Örnek veri oluşturma
> * Toplamaları gerçekleştirin
> * Ham ve toplu veri sorgulama
> * Verileri süresi dolacak

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Bir şema oluşturmak için psql yardımcı programı kullanın.

-PostgreSQL için Azure veritabanı'na bağlandıktan sonra psql, kullanarak hiper ölçekli (Citus) (Önizleme) bazı temel görevleri tamamlayabilirsiniz. Bu öğreticide daha sonra bu verilere dayalı gerçek zamanlı panolar sağlamak için verileri sıralı web analizden trafik verileri almak aracılığıyla açıklanmaktadır.

Tüm ham web trafiği verilerimiz tüketen bir tablo oluşturalım. Terminal psql aşağıdaki komutları çalıştırın:

```sql
CREATE TABLE http_request (
  site_id INT,
  ingest_time TIMESTAMPTZ DEFAULT now(),

  url TEXT,
  request_country TEXT,
  ip_address TEXT,

  status_code INT,
  response_time_msec INT
);
```

Bunu bizim dakikalık toplamalar barındıracak bir tablo ve bizim son paketi konumu tutan bir tablo oluşturmak için dağıtacağız. Psql de aşağıdaki komutları çalıştırın:

```sql
CREATE TABLE http_request_1min (
  site_id INT,
  ingest_time TIMESTAMPTZ, -- which minute this row represents

  error_count INT,
  success_count INT,
  request_count INT,
  average_response_time_msec INT,
  CHECK (request_count = error_count + success_count),
  CHECK (ingest_time = date_trunc('minute', ingest_time))
);

CREATE INDEX http_request_1min_idx ON http_request_1min (site_id, ingest_time);

CREATE TABLE latest_rollup (
  minute timestamptz PRIMARY KEY,

  CHECK (minute = date_trunc('minute', minute))
);
```

Artık bu psql komut ile Tablo listesinde yeni oluşturulan tablolarda görebilirsiniz:

```postgres
\dt
```

## <a name="shard-tables-across-nodes"></a>Düğümlerde parça tabloları

Bir hiper ölçekli dağıtımı kullanıcı tarafından atanan bir sütun değerine göre farklı düğümlerde tablo satırları depolar. Bu "dağıtım sütunu" veri düğümleri arasında parçalı nasıl işaretler.

Dağıtım sütunu olarak ayarlayalım\_kimliği, parça anahtarı. Bu işlevler, psql çalıştırın:

  ```sql
SELECT create_distributed_table('http_request',      'site_id');
SELECT create_distributed_table('http_request_1min', 'site_id');
```

## <a name="generate-sample-data"></a>Örnek veri oluşturma

Artık sunucu grubumuz bazı verilerin alımı hazır olmanız gerekir. Aşağıdaki yerel olarak çalıştırıyoruz bizim `psql` sürekli olarak veri eklemek için bağlantı.

```sql
DO $$
  BEGIN LOOP
    INSERT INTO http_request (
      site_id, ingest_time, url, request_country,
      ip_address, status_code, response_time_msec
    ) VALUES (
      trunc(random()*32), clock_timestamp(),
      concat('http://example.com/', md5(random()::text)),
      ('{China,India,USA,Indonesia}'::text[])[ceil(random()*4)],
      concat(
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2)
      )::inet,
      ('{200,404}'::int[])[ceil(random()*2)],
      5+trunc(random()*150)
    );
    COMMIT;
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

Sorgu, saniyede yaklaşık sekiz satır ekler. Satırları, dağıtım sütunu tarafından belirtildiği gibi farklı çalışan düğümlerinde depolandığından `site_id`.

   > [!NOTE]
   > Çalışan veri nesil sorguyu bırakın ve bu öğreticinin geri kalan komutlar ikinci psql bağlantısı.
   >

## <a name="query"></a>Sorgu

Barındırma seçeneği büyük ölçekli paralel hızı için sorguları işlemek üzere birden çok düğüm sağlar. Veritabanı örneği için çalışan düğümlerindeki toplam ve sayı gibi toplamalar hesaplar ve son bir yanıt sonuçları birleştirir.

Aşağıda, web isteklerini birkaç istatistikleri birlikte dakikada saymak için bir sorgu verilmiştir.
Psql çalıştırmayı deneyin ve sonuçları inceleyin.

```sql
SELECT
  site_id,
  date_trunc('minute', ingest_time) as minute,
  COUNT(1) AS request_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
  SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
FROM http_request
WHERE date_trunc('minute', ingest_time) > now() - '5 minutes'::interval
GROUP BY site_id, minute
ORDER BY minute ASC;
```

## <a name="rolling-up-data"></a>Veri alınıyor

Önceki sorguya erken aşamalarında düzgün çalışır, ancak, verilerin ölçeği performansı düşürür. Dağıtılmış işlem ile bile, işlem daha da sürekli olarak yeniden hesaplamak üzere verileri önceden daha hızlıdır.

Ham verileri birleşik bir tabloya çalışırken düzenli olarak ile hızlı bizim Pano kaldığından emin olabilirsiniz. Toplama süresi ile denemeler yapabilirsiniz. Dakika başına toplama tablo kullandık, ancak bunun yerine 5, 15 veya 60 dakika içinde veri uğratabilir.

Bu döküm daha kolay çalıştırmak için bunu plpgsql işleve koyun dağıtacağız. Psql oluşturmak için şu komutları çalıştırın `rollup_http_request` işlevi.

```sql
-- initialize to a time long ago
INSERT INTO latest_rollup VALUES ('10-10-1901');

-- function to do the rollup
CREATE OR REPLACE FUNCTION rollup_http_request() RETURNS void AS $$
DECLARE
  curr_rollup_time timestamptz := date_trunc('minute', now());
  last_rollup_time timestamptz := minute from latest_rollup;
BEGIN
  INSERT INTO http_request_1min (
    site_id, ingest_time, request_count,
    success_count, error_count, average_response_time_msec
  ) SELECT
    site_id,
    date_trunc('minute', ingest_time),
    COUNT(1) as request_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
    SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
  FROM http_request
  -- roll up only data new since last_rollup_time
  WHERE date_trunc('minute', ingest_time) <@
          tstzrange(last_rollup_time, curr_rollup_time, '(]')
  GROUP BY 1, 2;

  -- update the value in latest_rollup so that next time we run the
  -- rollup it will operate on data newer than curr_rollup_time
  UPDATE latest_rollup SET minute = curr_rollup_time;
END;
$$ LANGUAGE plpgsql;
```

Yerinde bizim işlevi ile verileri geri yürütün:

```sql
SELECT rollup_http_request();
```

Ve önceden toplanmış bir formda verilerimizi ile biz önceden olduğu gibi aynı raporu almak için toplama tablosunu sorgulayabilirsiniz. Aşağıdaki sorguyu çalıştırın:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Süresi dolacak eski veriler

Toplamaları sorguları hızlandırmak, ancak biz yine de sınırsız depolama maliyetleri önlemek için eski verileri süresi gerekir. Ne kadar süreyle her ayrıntı düzeyi için verileri ve standart sorgu süresi dolmuş verilerini silmek için olacağına karar verirsiniz. Aşağıdaki örnekte, bir gün için ham verileri korumak ve toplamalar bir ay boyunca dakika başına verdik:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

Üretim ortamında, bu sorguları bir işlevde kaydırma ve içinde bir cron işinin dakikada çağrı.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayacağınızı düşünüyorsanız, sunucu grubunu silin. Tuşuna *Sil* düğmesine *genel bakış* sunucu grubunuz için sayfa. Açılan sayfada istendiğinde sunucu grubu adını onaylayın ve en son tıklayın *Sil* düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir hiper ölçekli (Citus) sunucu grubu sağlama öğrendiniz. Psql ile bağlı, oluşturulan bir şema ve veri dağıtılmış. Veri hem ham biçimde öğrendiniz düzenli olarak bu verileri bir araya getirerek, toplanmış olan tabloları sorgulama ve eski verileri süresi dolacak.

Ardından, Hiper ölçekli kavramları hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Hiper ölçekli düğüm türleri](https://aka.ms/hyperscale-concepts)