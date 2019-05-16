---
title: PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı hızlı başlangıç
description: 'Hızlı Başlangıç: oluşturma ve sorgulama için Azure veritabanı tablolarında PostgreSQL hiper ölçekli (Citus) (Önizleme) dağıtılmış.'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/14/2019
ms.openlocfilehash: efc3801ab03f739761a41bec754f975fe43dcd8e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65757495"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Hızlı Başlangıç: PostgreSQL - Azure portalında hiper ölçekli (Citus) (Önizleme) için Azure veritabanı oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu hızlı başlangıçta, PostgreSQL - hiper ölçekli (Citus) (Önizleme) için Azure veritabanı oluşturma işlemini göstermektedir Azure portalını kullanarak sunucu grubu. Dağıtılmış veriler hakkında bilgi edineceksiniz: parçalama tabloları düğümlerde, örnek veri alma ve çalışan birden çok düğümde yürütülen sorgular.

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="create-and-distribute-tables"></a>Oluşturma ve tabloları dağıtma

Psql kullanarak hiper ölçekli Düzenleyici düğüme yeniden bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz.

Hiper ölçekli içinde sunucuları var. Tablo üç tür şunlardır:

- (Performans ve paralelleştirme için ölçeklendirme yardımcı olmak için dağılmış) dağıtılmış ya da parçalı tablo
- Başvuru tabloları (birden çok kopyası tutulur)
- Yerel tablolar (genellikle iç yönetici tabloları için kullanılır)

Bu hızlı başlangıçta, öncelikli olarak dağıtılmış tablolar ve bunlarla tanıdık alma odaklanacağız.

Veri modeli ile çalışmak için yapmamız basittir: GitHub kullanıcı ve olay verileri. Olayları çatal oluşturma dahil, git yürütmeleri kuruluş ve daha fazlası ile ilgili.

Psql bağlandıktan sonra tablomuz oluşturalım. Çalıştırın psql konsolunda:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

`payload` Alanını `github_events` JSONB veri türüne sahip. JSONB JSON veri türü ikili biçimde Postgres kullanılıyor. Veri türü, tek bir sütunda esnek bir şema depolamak kolaylaştırır.

Postgres oluşturabileceğiniz bir `GIN` her anahtar ve değer içindeki dizinini oluşturacak bu tür dizini. Bir dizin ile hızlı olur ve çeşitli koşullar ile yükü sorgulamak kolay. Yeni bir ubuntu ve verilerimizi yüklediğimiz önce birkaç dizinleri oluşturun. Psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Sonraki biz Postgres tabloların Düzenleyici düğümde alın ve hiper ölçekli bir parçaya bilgi çalışanları boyunca onları. Bunu yapmak için biz parça anahtarı belirterek her tablo için bir sorgu çalıştıracaksınız üzerinde. Geçerli örnekte parça olayları ve kullanıcıların tablo üzerinde geçeriz `user_id`:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

Verileri yüklemek hazırız. Psql yine de, dosyaları indirmek için kabuk:

```sql
\! curl -O https://examples.citusdata.com/users.csv
\! curl -O https://examples.citusdata.com/events.csv
```

Ardından, verileri dosyalarından dağıtılmış tablolara yükleyin:

```sql
\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Sorgu çalıştırma

Artık eğlenceli bir süredir gerçekten bazı sorguları çalıştıran bir parçası. Basit bir ile başlayalım `count (*)` yüklendik veri miktarını görmek için:

```sql
SELECT count(*) from github_events;
```

Sorunsuz şekilde çalışan. Size bu tür bir bit toplama işlemleri geri dönüp ancak şimdilik birkaç sorgu bakalım. İçinde JSONB `payload` sütunu var. veri iyi bir bit, ancak olay türüne göre değişir. `PushEvent` Gönderim için ayrı işleme sayısını içeren bir boyut olayları içerir. Bu işleme saat başına toplam sayısını bulmak için kullanabiliriz:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Şu ana kadar sorgular github dahil\_olayları özel olarak, ancak bu bilgiler github ile birleştirdik\_kullanıcılar. Parçalı Biz bu yana hem kullanıcılar hem de aynı tanımlayıcıyı olaylarına (`user_id`), kullanıcı kimliklerini eşleşen her iki tablonun satırlarını olacaktır [birlikte](https://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) aynı veritabanı düğümleri ve kolayca katılabilir.

Üzerinde katılırsanız `user_id`, Hiper ölçekli gönderebilir birleştirme yürütme Paralel yürütme için parçalar halinde çalışan düğümlerinde. Örneğin, en yüksek sayıda depo oluşturan kullanıcıların bulalım:

```sql
SELECT login, count(*)
FROM github_events ge
JOIN github_users gu
ON ge.user_id = gu.user_id
WHERE event_type = 'CreateEvent' AND
      payload @> '{"ref_type": "repository"}'
GROUP BY login
ORDER BY count(*) DESC;
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayacağınızı düşünüyorsanız, sunucu grubunu silin. Tuşuna **Sil** düğmesine **genel bakış** sunucu grubunuz için sayfa. Açılan sayfada istendiğinde sunucu grubu adını onaylayın ve en son tıklayın **Sil** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir hiper ölçekli (Citus) sunucu grubu sağlama öğrendiniz. Psql ile bağlı, oluşturulan bir şema ve veri dağıtılmış.

Ardından, ölçeklenebilir çok kiracılı uygulamalar oluşturmak için bir öğreticiyi izleyin.
> [!div class="nextstepaction"]
> [Çok Kiracılı veritabanı tasarlama](https://aka.ms/hyperscale-tutorial-multi-tenant)
