---
title: "Azure portalını kullanarak PostgreSQL için ilk Azure veritabanınızı tasarım | Microsoft Docs"
description: "Bu öğretici, Azure Portalı'nı kullanarak ilk Azure veritabanınızı PostgreSQL için tasarım kullanmayı gösterir."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 11/03/2017
ms.openlocfilehash: 1a210f813319a4f21c7c246002c968b8093f8a4e
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-the-azure-portal"></a>İlk Azure veritabanınız için Azure portalını kullanarak PostgreSQL tasarlama

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure Portalı'nı kullanarak, kolayca sunucunuzu yönetin ve bir veritabanı tasarlayın.

Bu öğreticide, bilgi edinmek için Azure portalını kullanın nasıl yapılır:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) bir veritabanı oluşturmak için yardımcı programı
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="prerequisites"></a>Ön koşullar
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1.  Tıklatın **+ yeni** düğme Azure portalında sol üst köşesinde bulundu.
2.  **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
 ![PostgreSQL için Azure Veritabanı - Veritabanı oluşturma](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3.  Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:
    - Sunucu adı: **mypgserver-20170401** (bir sunucu DNS adıyla eşleşir ve bu nedenle sunucunun genel olarak benzersiz olması gerekir) 
    - Abonelik: Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin.
    - Kaynak grubu: **myresourcegroup**
    - Tercih ettiğiniz sunucu yöneticisi oturum açma adı ve parolası
    - Konum
    - PostgreSQL Sürümü

  > [!IMPORTANT]
  > Sunucu Yöneticisi oturum açma ve burada belirttiğiniz parola, sunucu ve veritabanlarını Bu hızlı başlangıç devamındaki oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

4.  Yeni veritabanınıza ait hizmet katmanını ve performans düzeyini belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Bu Hızlı Başlangıç için seçin **temel** katmanı, **50 işlem birimleri** ve **50 GB** dahil depolama.
 ![PostgreSQL için Azure Veritabanı - hizmet katmanını seçme](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)
5.  **Tamam**’a tıklayın.
6.  Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

  > [!TIP]
  > Dağıtımlarınızın kolayca izlenmesini sağlamak için **Panoya sabitle** seçeneğini işaretleyin.

7.  Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
 ![PostgreSQL için Azure Veritabanı - Bildirimleri inceleme](./media/tutorial-design-database-using-azure-portal/3-notifications.png)
   
  Varsayılan olarak, **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere geliştirilmiş varsayılan bir veritabanıdır. 

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Azure veritabanı PostgreSQL hizmeti için sunucu düzeyinde bir Güvenlik Duvarı'nı kullanır. Varsayılan olarak, sunucu ve sunucudaki tüm veritabanları için bir güvenlik duvarı kuralı belirli bir IP adresi aralığı için Güvenlik Duvarı'nı açmak için yapılandırılmadığı sürece bağlanmasını tüm dış uygulamaları ve araçları bu güvenlik duvarı önler. 

1.  Dağıtım tamamlandıktan sonra, sol taraftaki menünden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için **mypgserver-20170401** adını yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.
 
 ![PostgreSQL için Azure Veritabanı - Sunucu arama ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  Sunucu sayfasında **Bağlantı güvenliği**’ni seçin. 
3.  **Kural Adı** altında metin kutusuna tıklayın ve IP aralığını bağlantı için beyaz listeye alacak yeni bir güvenlik duvarı kuralı ekleyin. Şimdi bu öğreticide, yazarak tüm IP'ler izin **kural adı AllowAllIps =**, **başlangıç IP 0.0.0.0 =** ve **bitiş IP 255.255.255.255 =** ve ardından **Kaydet** . Ağınızdan bağlanabilmesi için daha küçük bir IP aralığını kapsayan bir belirli güvenlik duvarı kuralı ayarlayabilirsiniz.
 
 ![PostgreSQL için Azure Veritabanı - Güvenlik Duvarı Kuralı Oluşturma](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  Tıklatın **kaydetmek** ve ardından **X** kapatmak için **bağlantıları güvenlik** sayfası.

  > [!NOTE]
  > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. BT departmanınız 5432 bir bağlantı noktası açar sürece bu durumda, Azure SQL veritabanı sunucusuna bağlanamıyor.
  >


## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Azure veritabanı PostgreSQL sunucusu, varsayılan için oluşturduğunuzda **postgres** veritabanı da oluşturuldu. Veritabanı sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

1. Azure portalında sol taraftaki menüden **tüm kaynakları** ve yeni oluşturduğunuz sunucu araması **mypgserver 20170401**.

  ![PostgreSQL için Azure Veritabanı - Sunucu arama ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. **mypgserver-20170401** sunucu adına tıklayın.

4. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.

 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Cloud Shell’de psql’i kullanarak PostgreSQL veritabanına bağlanma

Şimdi kullanalım [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) PostgreSQL server için Azure veritabanına bağlanmak için komut satırı yardımcı programı. 
1. Sol gezinme bölmesindeki terminal simgesiyle Azure Cloud Shell’i başlatın.

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. Cloud Shell isteminde, psql komutlarını kullanarak PostgreSQL için Azure Veritabanı sunucunuza bağlanın. Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mypgserver-20170401.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. İstendiğinde sunucu yönetici parolanızı girin.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a>Yeni veritabanı oluşturun
Sunucuya bağlandıktan sonra, istemde boş bir veritabanı oluşturun.
```bash
CREATE DATABASE mypgsqldb;
```

İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün.
```bash
\c mypgsqldb
```
## <a name="create-tables-in-the-database"></a>Veritabanında tabloları oluşturma
PostgreSQL için Azure veritabanına bağlanmak nasıl bildiğinize göre bazı temel görevleri tamamlayın:

İlk olarak, bir tablo oluşturun ve bazı verilerle yükleyin. Bu SQL kodu kullanarak Envanter bilgilerini izleyen bir tablo oluşturalım:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Yazarak Tablo listesinde yeni oluşturulan tabloda şimdi görebilirsiniz:
```sql
\dt
```

## <a name="load-data-into-the-tables"></a>Veri tablolarına yükleme
Bir tablo sahip olduğunuza göre bazı veriler içine ekleyin. Açık komut istemi penceresinde, bazı veri satırı eklemek için aşağıdaki sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Şimdi örnek verilerin daha önce oluşturduğunuz stok tabloya iki satır var.

## <a name="query-and-update-the-data-in-the-tables"></a>Sorgulamak ve tablolarındaki verileri güncelleyin
Stok veritabanı tablosundan bilgi almak için aşağıdaki sorguyu çalıştırın. 
```sql
SELECT * FROM inventory;
```

Ayrıca, tablodaki verileri güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Veri aldığınızda, güncelleştirilmiş değerleri görebilirsiniz.
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a>Veri zamandaki önceki bir noktaya geri yükleme
Bu tablo yanlışlıkla silinmiş düşünün. Bu durum, kolayca kurtaramazsınız şeydir. Azure veritabanı PostgreSQL için tüm noktası zamanında (olarak 7 gün (Temel) ve 35 gün (standart) son yukarı) için geri dönün ve bu noktası zaman içinde yeni bir sunucuya geri yüklemek sağlar. Bu yeni sunucu silinen verilerinizi kurtarmak için kullanabilirsiniz. Aşağıdaki adımlar geri yükleme **mypgserver 20170401** Stok tablosu eklenmeden önce bir noktasına sunucu.

1.  Azure veritabanı PostgreSQL için **genel bakış** sayfasında sunucunuz için **geri** araç çubuğunda. **Geri** sayfası açılır.
  ![Azure portal - geri yükleme form seçenekleri](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)
2.  Doldurmak **geri** form gerekli bilgileri:

  ![Azure portal - geri yükleme form seçenekleri](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - **Geri yükleme noktası**: bir nokta sunucu değiştirilmeden önce oluşan zaman seçin
  - **Hedef sunucu**: geri yüklemek istediğiniz yeni bir sunucu adı sağlayın
  - **Konum**: bölge seçemezsiniz, varsayılan olarak kaynak sunucuyla aynı.
  - **Fiyatlandırma katmanı**: bir sunucu geri yüklerken bu değer değiştirilemez. Kaynak sunucu ile aynı. 
3.  Tıklatın **Tamam** [bir nokta zaman için sunucunun geri](./howto-restore-server-portal.md) tablo silinmeden önce. Sunucuyu zamanında farklı bir noktaya geri yükleme oluşturur yinelenen yeni bir sunucu noktası itibariyle özgün sunucusu olarak belirttiğiniz süre için saklama dönemi içinde olmasını sağlanan, [hizmet katmanı](./concepts-service-tiers.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalı ve diğer yardımcı programlarını kullanın öğrendiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu Güvenlik Duvarı'nı yapılandırma
> * Kullanım [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) bir veritabanı oluşturmak için yardımcı programı
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Ardından, benzer görevleri gerçekleştirmek için Azure CLI kullanmayı öğrenmek için bu öğreticiyi gözden geçirin: [PostgreSQL için Azure CLI kullanarak ilk Azure veritabanınızı tasarlama](tutorial-design-database-using-azure-cli.md)
