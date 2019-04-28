---
title: 'Öğretici: Azure portalını kullanarak PostgreSQL için Azure veritabanı tasarlama'
description: Bu öğretici, Azure portalını kullanarak ilk PostgreSQL için Azure Veritabanınızı nasıl tasarlayacağınızı gösterir.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 03/20/2018
ms.openlocfilehash: aed539484ac01d1b18b8374ffb57456364f9bd2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61092111"
---
# <a name="tutorial-design-an-azure-database-for-postgresql-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak PostgreSQL için Azure veritabanı tasarlama

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Azure portalını kullanarak, sunucunuzu kolayca yönetebilir ve bir veritabanı tasarlayabilirsiniz.

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programını kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

## <a name="prerequisites"></a>Önkoşullar
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
   ![PostgreSQL için Azure Veritabanı - Veritabanı oluşturma](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3. Yeni sunucu ayrıntıları formunu aşağıdaki bilgilerle doldurun:

   ![Sunucu oluşturma](./media/tutorial-design-database-using-azure-portal/2-create.png)

   - Sunucu adı: **demosunucum** (bir sunucu DNS adıyla eşleşir ve bu nedenle sunucunun genel olarak benzersiz olması gerekir) 
   - Abonelik: Birden fazla aboneliğiniz varsa kaynağın mevcut olduğu ve faturalandırıldığı uygun aboneliği seçin.
   - Kaynak grubu: **myresourcegroup**
   - Tercih ettiğiniz sunucu yöneticisi oturum açma adı ve parolası
   - Location
   - PostgreSQL Sürümü

   > [!IMPORTANT]
   > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu öğreticinin sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

4. Yeni sunucunuza ait fiyatlandırma katmanını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Bu öğreticide, seçin **genel amaçlı**, **5. nesil** işlem nesli, 2 **sanal çekirdekler**, 5 GB **depolama** ve 7 gün  **Yedekleme bekletme süresi**. Sunucunuzun otomatik yedeklemelerini coğrafi olarak yedekli depolamada depolamak için **Coğrafi Olarak Yedekli** yedekleme seçeneğini belirleyin.
   ![PostgreSQL için Azure Veritabanı - fiyatlandırma katmanını seçme](./media/tutorial-design-database-using-azure-portal/2-pricing-tier.png)

5. **Tamam**’a tıklayın.

6. Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

7. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
   ![PostgreSQL için Azure Veritabanı - Bildirimleri inceleme](./media/tutorial-design-database-using-azure-portal/3-notifications.png)

   > [!TIP]
   > Dağıtımlarınızın kolayca izlenmesini sağlamak için **Panoya sabitle** seçeneğini işaretleyin.

   Varsayılan olarak, **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere geliştirilmiş varsayılan bir veritabanıdır. 

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

PostgreSQL için Azure Veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı kullanır. Varsayılan olarak bu güvenlik duvarı, belirli bir IP adresi aralığı için güvenlik duvarını açmak üzere bir güvenlik duvarı kuralı oluşturulmadıkça, tüm dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1. Dağıtım tamamlandıktan sonra, sol taraftaki menüden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için **demosunucum** adını yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.

   ![PostgreSQL için Azure Veritabanı - Sunucu arama](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. Sunucu sayfasında **Bağlantı güvenliği**’ni seçin. 

3. **Kural Adı** altında metin kutusuna tıklayın ve IP aralığını bağlantı için beyaz listeye alacak yeni bir güvenlik duvarı kuralı ekleyin. IP aralığınızı girin. **Kaydet**’e tıklayın.

   ![PostgreSQL için Azure Veritabanı - Güvenlik Duvarı Kuralı Oluşturma](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4. **Kaydet**’e ve ardından **X** düğmesine tıklayarak **Bağlantı güvenliği** sayfasını kapatın.

   > [!NOTE]
   > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
   >

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

PostgreSQL sunucusu için Azure Veritabanını oluşturduğunuzda, varsayılan **postgres** veritabanı da oluşturulmuştur. Veritabanı sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve kısa süre önce oluşturduğunuz sunucuyu aratın.

   ![PostgreSQL için Azure Veritabanı - Sunucu arama](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2. **demosunucum** sunucu adına tıklayın.

3. Sunucunun **Genel Bakış** sayfasını seçin. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.

   ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Cloud Shell’de psql’i kullanarak PostgreSQL veritabanına bağlanma

Şimdi PostgreSQL sunucusu için Azure Veritabanına bağlanmak üzere [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) komut satırı yardımcı programını kullanalım. 
1. Sol gezinme bölmesindeki terminal simgesiyle Azure Cloud Shell’i başlatın.

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. Cloud Shell isteminde, psql komutlarını kullanarak PostgreSQL için Azure Veritabanı sunucunuza bağlanın. Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Örneğin aşağıdaki komut, erişim kimlik bilgilerini kullanarak **mydemoserver.postgres.database.azure.com** PostgreSQL sunucunuzda **postgres** adlı varsayılan veritabanına bağlanır. İstendiğinde sunucu yönetici parolanızı girin.

   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

## <a name="create-a-new-database"></a>Yeni veritabanı oluşturma
Sunucuya bağlandıktan sonra, istemde boş bir veritabanı oluşturun.
```bash
CREATE DATABASE mypgsqldb;
```

İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün.
```bash
\c mypgsqldb
```
## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma
Artık PostgreSQL için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayabilirsiniz:

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Bu SQL kodunu kullanarak stok bilgilerini izleyen bir tablo oluşturalım:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Şimdi şunu yazarak tablo listesinde yeni oluşturulan tabloyu görebilirsiniz:
```sql
\dt
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme
Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz stok tablosunda artık iki satırlık örnek verileriniz vardır.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme
Stok veritabanı tablosundan bilgileri almak için şu sorguyu yürütün. 
```sql
SELECT * FROM inventory;
```

Tablodaki verileri de güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Verileri alırken güncelleştirilmiş değerleri görebilirsiniz.
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a>Verileri önceki bir noktaya geri yükleme
Bu tabloyu yanlışlıkla sildiğinizi düşünün. Bu, kolayca kurtaramayacağınız bir durumdur. PostgreSQL için Azure Veritabanı sunucunuzun yedeğinin olduğu herhangi bir noktaya dönerek (yapılandırdığınız yedekleme bekletme dönemine göre belirlenir) bu noktayı yeni bir sunucuya geri yükleyebilirsiniz. Bu yeni sunucuyu silinen verilerinizi kurtarmak için kullanabilirsiniz. Aşağıdaki adımlar, **demosunucum** sunucusunu envanter tablosu eklenmeden önceki bir noktaya geri yükler.

1. Sunucunuza yönelik PostgreSQL için Azure Veritabanı **Genel Bakış** sayfasında araç çubuğundaki **Geri Yükle** seçeneğine tıklayın. **Geri Yükle** sayfası açılır.

   ![Azure portalı - Geri yükleme formu seçenekleri](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)

2. **Geri Yükleme** formunu gerekli bilgiler ile doldurun:

   ![Azure portalı - Geri yükleme formu seçenekleri](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)

   - **Geri yükleme noktası**: Bir-sunucu değiştirilmeden önce gerçekleşen belirli bir noktaya seçin
   - **Hedef sunucu**: Geri yüklemek istediğiniz yeni bir sunucu adı sağlayın
   - **Konum**: Bölgeyi seçemezsiniz, varsayılan olarak, kaynak sunucuyla aynıdır
   - **Fiyatlandırma katmanı**: Bir sunucuyu geri yüklerken bu değeri değiştiremezsiniz. Kaynak sunucuyla aynıdır. 
3. Sunucuyu, [tablo silinmeden önceki belirli bir noktaya geri yüklemek için](./howto-restore-server-portal.md) **Tamam**’a tıklayın. Sunucunun farklı bir zaman noktasına geri yüklenmesi, [fiyatlandırma katmanınızın](./concepts-pricing-tiers.md) bekletme dönemi içinde olmak şartıyla, belirttiğiniz zaman noktasından itibaren özgün sunucu ile aynı yeni bir kopya sunucu oluşturur.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, aşağıdakileri yapmak için Azure portalını ve diğer yardımcı programları nasıl kullanacağınızı öğrendiniz:
> [!div class="checklist"]
> * PostgreSQL için Azure Veritabanı sunucusu oluşturma
> * Sunucu güvenlik duvarını yapılandırma
> * Veritabanı oluşturmak için [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programını kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> * Verileri geri yükleme

Ardından, benzer görevleri yapmak için Azure CLI'yı kullanmayı öğrenmek için şu öğreticiyi gözden geçirin: [PostgreSQL için Azure CLI kullanarak ilk Azure veritabanınızı tasarlama](tutorial-design-database-using-azure-cli.md)
