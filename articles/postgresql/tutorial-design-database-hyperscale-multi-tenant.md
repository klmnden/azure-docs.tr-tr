---
title: PostgreSQL hiper ölçekli (Citus) (Önizleme) Azure veritabanı ile çok kiracılı veritabanı tasarlama Öğreticisi
description: Bu öğreticide, PostgreSQL hiper ölçekli (Citus) (Önizleme) için Azure veritabanı üzerinde dağıtılmış tablolar sorgu oluşturmak ve doldurmak gösterilir.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: 73d7aebf3dbff59320e0ef92cbd54811503c71b4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65757623"
---
# <a name="tutorial-design-a-multi-tenant-database-by-using-azure-database-for-postgresql--hyperscale-citus-preview"></a>Öğretici: Azure veritabanını PostgreSQL hiper ölçekli (Citus) (Önizleme) kullanarak çok kiracılı veritabanı tasarlama

Bu öğreticide, PostgreSQL - öğrenmek için hiper ölçekli (Citus) (Önizleme) için Azure veritabanı kullanan nasıl yapılır:

> [!div class="checklist"]
> * Bir hiper ölçekli (Citus) sunucu grubu oluşturma
> * Bir şema oluşturmak için psql yardımcı programı kullanın.
> * Düğümlerde parça tabloları
> * Örnek verileri ekleme
> * Kiracı verileri Sorgulama
> * Kiracılar arasında veri paylaşımı
> * Şema Kiracı başına özelleştirme

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Bir şema oluşturmak için psql yardımcı programı kullanın.

-PostgreSQL için Azure veritabanı'na bağlandıktan sonra psql, kullanarak hiper ölçekli (Citus) (Önizleme) bazı temel görevleri tamamlayabilirsiniz. Bu öğreticide, kampanyaları izlemek reklamcılar sağlayan bir web uygulaması oluşturma işleminde açıklanmaktadır.

Birden çok şirket, bir uygulama kullanma, Haydi şirketleri ve kendi kampanyalar için başka tutmak üzere bir tablo oluşturun. Psql konsolunda şu komutları çalıştırın:

```sql
CREATE TABLE companies (
  id bigserial PRIMARY KEY,
  name text NOT NULL,
  image_url text,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL
);

CREATE TABLE campaigns (
  id bigserial,
  company_id bigint REFERENCES companies (id),
  name text NOT NULL,
  cost_model text NOT NULL,
  state text NOT NULL,
  monthly_budget bigint,
  blacklisted_site_urls text[],
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id)
);
```

Her kampanya reklam çalıştırmak ödeme yaparsınız. Psql yukarıdaki koddan sonra aşağıdaki kodu çalıştırarak bir tablo reklam için de ekleyin:

```sql
CREATE TABLE ads (
  id bigserial,
  company_id bigint,
  campaign_id bigint,
  name text NOT NULL,
  image_url text,
  target_url text,
  impressions_count bigint DEFAULT 0,
  clicks_count bigint DEFAULT 0,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, campaign_id)
    REFERENCES campaigns (company_id, id)
);
```

Son olarak, tıklama ve her bir ad izlenimler hakkındaki istatistiklerdir izleriz:

```sql
CREATE TABLE clicks (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  clicked_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_click_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);

CREATE TABLE impressions (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  seen_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_impression_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);
```

Artık psql Tablo listesinde yeni oluşturulan tablolar çalıştırarak görebilirsiniz:

```postgres
\dt
```

Çok kiracılı uygulamaları şirket kimliği tüm birincil ve yabancı anahtarları dahil neden olan yalnızca Kiracı başına benzersizlik

## <a name="shard-tables-across-nodes"></a>Düğümlerde parça tabloları

Bir hiper ölçekli dağıtımı kullanıcı tarafından atanan bir sütun değerine göre farklı düğümlerde tablo satırları depolar. Kiracı bu "dağıtım sütunu" işaretlerini, hangi satırların sahip.

Dağıtım sütunu şirket olacak şekilde ayarlayalım\_kimliği, Kiracı tanımlayıcısı. Bu işlevler, psql çalıştırın:

```sql
SELECT create_distributed_table('companies',   'id');
SELECT create_distributed_table('campaigns',   'company_id');
SELECT create_distributed_table('ads',         'company_id');
SELECT create_distributed_table('clicks',      'company_id');
SELECT create_distributed_table('impressions', 'company_id');
```

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Şimdi, psql dışında normal komut satırında, örnek veri kümelerini indirin:

```bash
for dataset in companies campaigns ads clicks impressions geo_ips; do
  curl -O https://examples.citusdata.com/mt_ref_arch/${dataset}.csv
done
```

Geri psql içinde toplu veri yükleme. Veri dosyalarını karşıdan yüklediğiniz aynı dizinde psql çalıştırmayı unutmayın.

```sql
\copy companies from 'companies.csv' with csv
\copy campaigns from 'campaigns.csv' with csv
\copy ads from 'ads.csv' with csv
\copy clicks from 'clicks.csv' with csv
\copy impressions from 'impressions.csv' with csv
```

Bu veriler artık çalışan düğümlere yayılır.

## <a name="query-tenant-data"></a>Kiracı verileri Sorgulama

Uygulama için tek bir kiracının verileri istediğinde, veritabanının tek çalışan düğümü üzerinde sorgu yürütebilirsiniz. Tek kiracılı sorgular tek Kiracı kimliğine göre filtrele Örneğin, aşağıdaki filtreleri sorgu `company_id = 5` reklamlar ve izlenimler için. Psql sonuçları görmek için çalıştırmayı deneyin.

```sql
SELECT a.campaign_id,
       RANK() OVER (
         PARTITION BY a.campaign_id
         ORDER BY a.campaign_id, count(*) desc
       ), count(*) as n_impressions, a.id
  FROM ads as a
  JOIN impressions as i
    ON i.company_id = a.company_id
   AND i.ad_id      = a.id
 WHERE a.company_id = 5
GROUP BY a.campaign_id, a.id
ORDER BY a.campaign_id, n_impressions desc;
```

## <a name="share-data-between-tenants"></a>Kiracılar arasında veri paylaşımı

Şimdiye kadar tüm tabloları tarafından dağıtılmış `company_id`, ancak bazı veriler doğal olarak "tüm özellikle kiracıda değil" ve paylaşılabilir. Örneğin, örnek ad platformu tüm şirketlerin IP adreslerine göre kendi hedef kitle için coğrafi bilgi almak isteyebilirsiniz.

Paylaşılan coğrafi bilgileri tutmak için bir tablo oluşturun. Psql aşağıdaki komutları çalıştırın:

```sql
CREATE TABLE geo_ips (
  addrs cidr NOT NULL PRIMARY KEY,
  latlon point NOT NULL
    CHECK (-90  <= latlon[0] AND latlon[0] <= 90 AND
           -180 <= latlon[1] AND latlon[1] <= 180)
);
CREATE INDEX ON geo_ips USING gist (addrs inet_ops);
```

Sonraki olun `geo_ips` "başvuru tablosu" her çalışan düğümü üzerinde bir tablonun kopyasını saklamak için.

```sql
SELECT create_reference_table('geo_ips');
```

Bu örnek verilerle yükleyin. Veri kümesi indirdiğiniz psql dizini içinde bu komutu çalıştırmak unutmayın.

```sql
\copy geo_ips from 'geo_ips.csv' with csv
```

Coğrafi tıklama tabloyla birleştirme\_IP'ler tüm düğümlerinde verimlidir.
İşte ad tıklanan herkes konumlarını bulmak için bir birleştirme
290. Psql sorgu çalıştırmayı deneyin.

```sql
SELECT c.id, clicked_at, latlon
  FROM geo_ips, clicks c
 WHERE addrs >> c.user_ip
   AND c.company_id = 5
   AND c.ad_id = 290;
```

## <a name="customize-the-schema-per-tenant"></a>Şema Kiracı başına özelleştirme

Her Kiracı, başkaları tarafından gerekmeyen özel bilgileri depolamak için gerekebilir. Ancak, tüm kiracılar ortak altyapı bir aynı veritabanı şeması ile paylaşın. Ek veriler gidebilecekleri?

Bir kandırma açık uçlu bir sütuna PostgreSQL'ın gibi kullanmaktır JSONB.  Bizim şema JSONB alana sahip `clicks` adlı `user_data`.
(Örneğin şirket beş), şirket sütunu kullanıcı bir mobil cihazda olup olmadığını izlemek için kullanabilirsiniz.

Kimin daha fazla tıkladığında bulmak için bir sorgu aşağıdadır: Mobil veya Geleneksel ziyaretçi.

```sql
SELECT
  user_data->>'is_mobile' AS is_mobile,
  count(*) AS count
FROM clicks
WHERE company_id = 5
GROUP BY user_data->>'is_mobile'
ORDER BY count DESC;
```

Biz bu sorgu için tek bir şirket oluşturarak en iyi duruma getirebilirsiniz bir [kısmi dizin](https://www.postgresql.org/docs/current/static/indexes-partial.html).

```sql
CREATE INDEX click_user_data_is_mobile
ON clicks ((user_data->>'is_mobile'))
WHERE company_id = 5;
```

Genellikle daha oluşturabiliriz bir [GIN dizinlerini](https://www.postgresql.org/docs/current/static/gin-intro.html) her anahtar ve sütundaki değeri.

```sql
CREATE INDEX click_user_data
ON clicks USING gin (user_data);

-- this speeds up queries like, "which clicks have
-- the is_mobile key present in user_data?"

SELECT id
  FROM clicks
 WHERE user_data ? 'is_mobile'
   AND company_id = 5;
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayacağınızı düşünüyorsanız, sunucu grubunu silin. Tuşuna *Sil* düğmesine *genel bakış* sunucu grubunuz için sayfa. Açılan sayfada istendiğinde sunucu grubu adını onaylayın ve en son tıklayın *Sil* düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir hiper ölçekli (Citus) sunucu grubu sağlama öğrendiniz. Psql ile bağlı, oluşturulan bir şema ve veri dağıtılmış. Veri hem içinde hem de kiracılar arasında ve Kiracı başına şema özelleştirmek için öğrendiniz.

Ardından, Hiper ölçekli kavramları hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Hiper ölçekli düğüm türleri](https://aka.ms/hyperscale-concepts)
