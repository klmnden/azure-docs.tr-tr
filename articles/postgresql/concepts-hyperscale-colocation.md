---
title: PostgreSQL için Azure veritabanı sunucusu kavramları
description: Bu makalede, konuları ve PostgreSQL sunucuları için Azure veritabanı'nı yönetme ve yapılandırma için yönergeler sağlar.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: d03cfd49887adf1f6a4650e374d3e13eeca735a4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077478"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda ortak yerleşim tablosu

Birlikte bulundurma, ilgili bilgileri aynı düğümler üzerinde birlikte depolama anlamına gelir. Tüm gerekli veri herhangi bir ağ trafiği kullanılabilir olduğunda hızlı sorgular gidebilirsiniz. Farklı düğümlere ilgili verileri birlikte bulundurma, verimli bir şekilde her bir düğümde paralel olarak çalıştırmak sorguları sağlar.

## <a name="data-colocation-for-hash-distributed-tables"></a>Karma dağıtılmış tablo için verileri birlikte bulundurma

Karma dağıtım sütunundaki değerin parça'nın karma aralığı içinde kalırsa hiper ölçekli bir parçada satır depolanır. Aynı karma aralıkla parçalar, her zaman aynı düğümde yerleştirilir. Satırları eşit dağıtım sütun değerleri ile aynı düğümde çok tabloda her zaman olur.

![Parçalar](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Birlikte bulundurma pratik bir örneği

Çok kiracılı web analizi SaaS parçası olabilecek aşağıdaki tablolarda göz önünde bulundurun:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Artık bir müşteriye dönük Pano tarafından gibi verilen sorgulara yanıt vermek istiyoruz: "İle başlayan tüm sayfalar için geçtiğimiz hafta ziyaret sayısı dönüş ' / blog' kiracısında altı."

Verilerimizi bir tek sunuculu dağıtım seçeneği, biz kolayca sorgumuzu zengin SQL tarafından sunulan bir ilişkisel işlemleri ifade edebilir:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Sürece [çalışma kümesi](https://en.wikipedia.org/wiki/Working_set) bu sorgu belleğe sığamayacak için bir tek sunuculu uygun bir çözüm tablodur. Ancak, veri modeli ile hiper ölçekli dağıtım seçeneği ölçeklendirmenin fırsatları düşünelim.

### <a name="distributing-tables-by-id"></a>Tablo Kimliği ile dağıtma

Tek sunuculu sorguları, kiracılar ve her Kiracı için depolanan verileri sayısı arttıkça yavaşlamasıdır başlatın. Yüksek çalışma kümesi bellekte sığdırma durdurur ve CPU bir performans sorunu haline gelir.

Bu durumda, parça veri hiper ölçekli kullanarak birçok düğümlere gerçekleştirebiliriz. Parçalama dağıtım sütunu olduğunda sağlamak için ihtiyacımız ilk ve en önemli seçimi. Naïve yönteminizi kullanarak başlayalım `event_id` olay tablosu ve `page_id` için `page` tablosu:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Verileri farklı çalışanlar arasında dağıtılır, biz tek bir PostgreSQL düğümde yaptığınız gibi biz bir birleştirme yapamazsınız. Bunun yerine, size iki sorguları göndermek amacıyla gerekir:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Daha sonra iki adımı sonuçlardan uygulama tarafından birleştirilmesi gerekir.

Sorguları çalıştıran düğümlere dağılmış parçalardaki veri başvurmanız gerekir.

![verimli sorgular](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

Bu durumda veri dağıtım önemli engelleri oluşturur:

-   Birden çok sorgu çalıştıran, her parça sorgulama gelen ek yükü
-   S1 yükü çok sayıda istemciye döndürüyor
-   S2 büyük
-   Birden fazla adım sorgular yazmaya gerek uygulamada değişiklikler gerektirir.

Veri dağınık olduğundan, sorgu paralelleştirilebilir. Sorgusu yapan iş miktarını önemli ölçüde birçok parça sorgulama yükü büyükse, ancak bunu yalnızca yararlıdır.

### <a name="distributing-tables-by-tenant"></a>Tabloları Kiracı tarafından dağıtma

Hiper ölçekli aynı dağıtım sütun değerine sahip satırları aynı düğümde olması garanti. Başlıyorum, bizim tablolarla oluşturabiliriz `tenant_id` dağıtım sütunu.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Artık hiper ölçekli (S1) yapmadan özgün tek sunuculu sorgu yanıt verebilir:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Filtre ve Kiracı üzerinde birleştirme nedeniyle hiper ölçekli tüm sorgu belirli bir kiracı için veri içeren birlikte parçalar kümesini kullanarak yanıtlanması gereken olduğunu bilir. Tek bir PostgreSQL düğüm, tek bir adımda sorgu yanıt verebilir.

![daha iyi sorgu](media/concepts-hyperscale-colocation/colocation-better-query.png)

Bazı durumlarda, sorgular ve tablo şemalarını Kiracı kimliği içinde benzersiz kısıtlamalar dahil etmek ve koşullar katılmak için değiştirilmesi gerekir. Ancak, bu genellikle basit farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

- İçinde Kiracı verilerini nasıl birlikte bkz [çok kiracılı Öğreticisi](tutorial-design-database-hyperscale-multi-tenant.md)
