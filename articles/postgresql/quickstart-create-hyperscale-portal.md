---
title: PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı hızlı başlangıç
description: 'Hızlı Başlangıç: oluşturma ve sorgulama için Azure veritabanı tablolarında PostgreSQL hiper ölçekli (Citus) (Önizleme) dağıtılmış.'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/06/2019
ms.openlocfilehash: 4271d94f07125a870cc4aa859b01db819d583f40
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406442"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-preview-in-the-azure-portal"></a>Hızlı Başlangıç: PostgreSQL - Azure portalında hiper ölçekli (Citus) (Önizleme) için Azure veritabanı oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu hızlı başlangıçta, PostgreSQL - hiper ölçekli (Citus) (Önizleme) için Azure veritabanı oluşturma işlemini göstermektedir Azure portalını kullanarak sunucu grubu. Dağıtılmış veriler inceleyeceksiniz: parçalama tabloları düğümlerde, örnek veri alma ve çalışan birden çok düğümde yürütülen sorgular.

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
   > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu hızlı başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

5. Tıklayın **yapılandırma sunucusu grubunu**. Ayarları içeren bölüm değiştirmeden bırakın ve tıklayın **Kaydet**.
6. Tıklayın **gözden geçir + Oluştur** ardından **Oluştur** sunucuyu sağlamak için. Sağlama birkaç dakika sürer.
7. Sayfa dağıtımını izlemek için yönlendirir. Canlı durumu değiştiğinde **devam ettiği dağıtımıdır** için **dağıtımınız tamamlandıktan**, tıklayın **çıkışları** sayfasının sol menü öğesi.
8. Çıkış sayfası değeri panoya kopyalamak için yanında bir düğme olan bir düzenleyici ana bilgisayar adı içerir. Daha sonra kullanmak için bu bilgileri kaydedin.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Azure sunucu düzeyinde bir güvenlik duvarı (Citus) (Önizleme) hizmeti kullandığı PostgreSQL hiper ölçekli veritabanı. Varsayılan olarak, düzenleyici düğüm ve içinde herhangi bir veritabanına bağlanmasını tüm dış uygulama ve araçların güvenlik duvarı engeller. Biz, belirli bir IP adresi aralığı için güvenlik duvarını açmak üzere bir kural eklemeniz gerekir.

1. Gelen **çıkışları** daha önce kopyaladığınız Düzenleyici düğüm ana bilgisayar bölümü tıklatın geri **genel bakış** menü öğesi.

2. Dağıtımınızın ölçeklendirme grubun adı "sg-" ile önek alacaktır. Kaynak listesinde bulun ve tıklatın.

3. Tıklayın **Güvenlik Duvarı** altında **güvenlik** sol menüdeki.

4. Bağlantıya tıklayın **+ geçerli istemci IP adresi için Güvenlik Duvarı Kuralı Ekle**. Son olarak, tıklayın **Kaydet** düğmesi.

5. **Kaydet**’e tıklayın.

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
   >

## <a name="connect-to-the-database-using-psql-in-cloud-shell"></a>Cloud Shell'de psql kullanarak veritabanına bağlanma

Şimdi PostgreSQL sunucusu için Azure Veritabanına bağlanmak üzere [psql](https://www.postgresql.org/docs/current/app-psql.html) komut satırı yardımcı programını kullanalım.
1. Sol gezinme bölmesindeki terminal simgesiyle Azure Cloud Shell’i başlatın.

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/quickstart-create-hyperscale-portal/psql-cloud-shell.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/quickstart-create-hyperscale-portal/psql-bash.png)

3. Cloud Shell isteminde, psql komutlarını kullanarak PostgreSQL için Azure Veritabanı sunucunuza bağlanın. Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
   ```bash
   psql --host=<myserver> --username=myadmin --dbname=citus
   ```

   Örneğin, aşağıdaki komut adlı varsayılan veritabanına bağlanır **citus** PostgreSQL sunucunuzda **demosunucum.postgres.Database.Azure.com** erişim kimlik bilgilerini kullanarak. İstendiğinde sunucu yönetici parolanızı girin.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --username=myadmin --dbname=citus
   ```

## <a name="create-and-distribute-tables"></a>Oluşturma ve tabloları dağıtma

Psql kullanarak hiper ölçekli Düzenleyici düğüme yeniden bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz.

Hiper ölçekli içinde sunucuları var. Tablo üç tür şunlardır:

- (Performans ve paralelleştirme için ölçeklendirme yardımcı olmak için dağılmış) dağıtılmış ya da parçalı tablo
- Başvuru tabloları (birden çok kopyası tutulur)
- Yerel tablolar (genellikle iç yönetici tabloları için kullanılır)

Bu hızlı başlangıçta öncelikle dağıtılmış tablolar ve bunlarla tanıdık alma odaklanacağız.

Veri modeli ile çalışmak için yapmamız basittir: GitHub kullanıcı ve olay verileri. Olayları çatal oluşturma dahil, git yürütmeleri kuruluş ve daha fazlası ile ilgili.

Psql şimdi bağlandıktan sonra tablomuz oluşturun. Çalıştırın psql konsolunda:

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

`payload` Alanını `github_events` JSONB veri türüne sahip. JSONB JSON veri türü ikili biçimde Postgres kullanılıyor. Bu, tek bir sütunda daha esnek bir şema depolamak kolaylaştırır.

Postgres oluşturabileceğiniz bir `GIN` dizini, her anahtar ve değer içindeki dizin bu tür. Bir dizin ile hızlı olur ve çeşitli koşullar ile yükü sorgulamak kolay. Yeni bir ubuntu ve verilerimizi yüklediğimiz önce birkaç dizinleri oluşturun. Psql:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Sonraki biz Postgres tabloların Düzenleyici düğümde alın ve hiper ölçekli bir parçaya bilgi çalışanları boyunca onları. Bunu yapmak için biz parça anahtarı belirterek her tablo için bir sorgu çalıştıracaksınız üzerinde. Geçerli örnekte parça olayları ve kullanıcıların tablo üzerinde geçeriz `user_id`:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

Verileri yüklemek hazırız. İki örnek dosyalarını indirme [kullanıcıları.csv](https://examples.citusdata.com/users.csv) ve [events.csv](https://examples.citusdata.com/events.csv). Dosyalar'ı indirdikten sonra indirilen dosyaları içeren dizinden psql çalıştırmak dikkatli, psql kullanarak veritabanına bağlanın. İle veri yükleme `\copy` komutu:

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
