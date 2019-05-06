---
title: – Hiper ölçekli (Citus) (Önizleme) PostgreSQL için Azure veritabanı ile gerçek zamanlı bir Panoda tasarım Öğreticisi
description: Bu öğreticide, PostgreSQL hiper ölçekli (Citus) (Önizleme) için Azure veritabanı üzerinde dağıtılmış tablolar sorgu oluşturmak ve doldurmak gösterilir.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/06/2019
ms.openlocfilehash: 7324ab1d7aa6e42100c9c6760c17b0ea6445f21d
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080747"
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

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-an-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
3. Dağıtım seçeneği için tıklatın **Oluştur** düğmesini **hiper ölçekli (Citus) sunucu grubu - Önizleme.**
4. Yeni sunucu ayrıntıları formunu aşağıdaki bilgilerle doldurun:
   - Kaynak grubu: tıklayın **Yeni Oluştur** Bu alan için metin kutusunun altında bağlantı. Gibi bir ad girin **myresourcegroup**.
   - Sunucu grubu adı: **demosunucum** (genel olarak benzersiz olması için DNS adıyla eşleşir ve gerekli olan sunucu adı).
   - Yönetici kullanıcı adı: **myadmin** (bunu daha sonra veritabanına bağlanmak için kullanılacak).
   - Parola: en az sekiz karakter uzunluğunda olmalıdır ve – İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakter şu kategorilerin üçünden karakterler içermelidir (!, $, #, %, vs.)
   - Konum: bunları verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.

   > [!IMPORTANT]
   > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu öğreticinin sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

5. Tıklayın **yapılandırma sunucusu grubunu**. Ayarları içeren bölüm değiştirmeden bırakın ve tıklayın **Kaydet**.
6. Tıklayın **gözden geçir + Oluştur** ardından **Oluştur** sunucuyu sağlamak için. Sağlama birkaç dakika sürer.
7. Sayfa dağıtımını izlemek için yönlendirir. Canlı durumu değiştiğinde **devam ettiği dağıtımıdır** için **dağıtımınız tamamlandıktan**, tıklayın **çıkışları** sayfasının sol menü öğesi.
8. Çıkış sayfası değeri panoya kopyalamak için yanında bir düğme olan bir düzenleyici ana bilgisayar adı içerir. Daha sonra kullanmak için bu bilgileri kaydedin.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

PostgreSQL için Azure Veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı kullanır. Varsayılan olarak, sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını tüm dış uygulama ve araçların güvenlik duvarı engeller. Biz, belirli bir IP adresi aralığı için güvenlik duvarını açmak üzere bir kural eklemeniz gerekir.

1. Gelen **çıkışları** daha önce kopyaladığınız Düzenleyici düğüm ana bilgisayar bölümü tıklatın geri **genel bakış** menü öğesi.

2. Ölçeklendirme grubu dağıtımınız kaynakları için listede bulun ve tıklatın. (Adı "sg-" ile önek alacaktır.)

3. Tıklayın **Güvenlik Duvarı** altında **güvenlik** sol menüdeki.

4. Bağlantıya tıklayın **+ geçerli istemci IP adresi için Güvenlik Duvarı Kuralı Ekle**. Son olarak, tıklayın **Kaydet** düğmesi.

5. **Kaydet**’e tıklayın.

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Cloud Shell'de psql kullanarak veritabanına bağlanma

Şimdi PostgreSQL sunucusu için Azure Veritabanına bağlanmak üzere [psql](https://www.postgresql.org/docs/current/app-psql.html) komut satırı yardımcı programını kullanalım.
1. Sol gezinme bölmesindeki terminal simgesiyle Azure Cloud Shell’i başlatın.

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/tutorial-design-database-hyperscale-realtime/psql-cloud-shell.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/tutorial-design-database-hyperscale-realtime/psql-bash.png)

3. Cloud Shell isteminde, psql komutlarını kullanarak PostgreSQL için Azure Veritabanı sunucunuza bağlanın. Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Örneğin, aşağıdaki komut adlı varsayılan veritabanına bağlanır **citus** PostgreSQL sunucunuzda **demosunucum.postgres.Database.Azure.com** erişim kimlik bilgilerini kullanarak. İstendiğinde sunucu yönetici parolanızı girin.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

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

Bunu bizim dakikalık toplamalar barındıracak bir tablo ve bizim son paketi konumu tutan bir tablo oluşturmak için dağıtacağız. Psql de aşağıdaki komutu çalıştırın:

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
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

Sorgu, bir satır ikinci yaklaşık her çeyrek ekler. Satırları, dağıtım sütunu tarafından belirtildiği gibi farklı çalışan düğümlerinde depolandığından `site_id`.

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

## <a name="performing-rollups"></a>Toplamaları gerçekleştirme

Yukarıdaki sorguda erken aşamalarında düzgün çalışır, ancak performansını, verilerin ölçeği azaltır. Hatta dağıtılmış işlem ile bu verileri tekrar tekrar yeniden hesapla daha önceden işlem daha hızlıdır.

Ham verileri birleşik bir tabloya çalışırken düzenli olarak ile hızlı bizim Pano kaldığından emin olabilirsiniz. Bu durumda biz bir dakika toplama tablomuza toplanır, ancak aynı zamanda 5 dakika, 15 dakikada bir saatlik toplamalar sahip ve benzeri.

Bu döküm sürekli çalıştıracağız çünkü bunu gerçekleştirmek için bir işlev oluşturma dağıtacağız. Psql oluşturmak için şu komutları çalıştırın `rollup_http_request` işlevi.

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

Ve önceden toplanmış bir formda verilerimizi ile biz önceden olduğu gibi aynı raporu almak için toplama tablosunu sorgulayabilirsiniz. Aşağıdakileri çalıştırın:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Süresi dolacak eski veriler

Toplamaları sorguları hızlandırmak, ancak biz yine de sınırsız depolama maliyetleri önlemek için eski verileri süresi gerekir. Yalnızca ne kadar süreyle her ayrıntı düzeyi için verileri ve standart sorgu süresi dolmuş verilerini silmek için olacağına karar verirsiniz. Aşağıdaki örnekte, bir gün için ham verileri korumak ve toplamalar bir ay boyunca dakika başına verdik:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

Üretim ortamında, bu sorguları bir işlevde Kaydır ve cron işinde dakikada çağırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayacağınızı düşünüyorsanız, sunucu grubunu silin. Tuşuna *Sil* düğmesine *genel bakış* sunucu grubunuz için sayfa. Açılan sayfada istendiğinde sunucu grubu adını onaylayın ve en son tıklayın *Sil* düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir hiper ölçekli (Citus) sunucu grubu sağlama öğrendiniz. Psql ile bağlı, oluşturulan bir şema ve veri dağıtılmış. Veri hem ham biçimde öğrendiniz düzenli olarak bu verileri bir araya getirerek, toplanmış olan tabloları sorgulama ve eski verileri süresi dolacak.

Ardından, Hiper ölçekli kavramları hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Hiper ölçekli düğüm türleri](https://aka.ms/hyperscale-concepts)