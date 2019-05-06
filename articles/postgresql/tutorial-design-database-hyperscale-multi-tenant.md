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
ms.date: 05/06/2019
ms.openlocfilehash: b135baf73e21cd524b6e8fad35452362f36cf0c0
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65080810"
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
   - Sunucu grubu adı: sunucu alt etki alanı için kullanılacak yeni bir sunucu grubu için benzersiz bir ad girin.
   - Yönetici kullanıcı adı: benzersiz bir kullanıcı adı girin, veritabanına bağlanmak için daha sonra kullanılacak.
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

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/tutorial-design-database-hyperscale-multi-tenant/psql-cloud-shell.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/tutorial-design-database-hyperscale-multi-tenant/psql-bash.png)

3. Cloud Shell isteminde, psql komutlarını kullanarak PostgreSQL için Azure Veritabanı sunucunuza bağlanın. Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Örneğin, aşağıdaki komut adlı varsayılan veritabanına bağlanır **citus** PostgreSQL sunucunuzda **demosunucum.postgres.Database.Azure.com** erişim kimlik bilgilerini kullanarak. İstendiğinde sunucu yönetici parolanızı girin.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

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

Paylaşılan coğrafi bilgileri tutmak için bir tablo oluşturun. Bu, psql çalıştırın:

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

Bu örnek verilerle yükleyin. Bu dizin içinde psql içinde veri kümesi indirdiğiniz çalıştırmayı unutmayın.

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
