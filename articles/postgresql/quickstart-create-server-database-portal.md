---
title: "Azure portal: PostgreSQL sunucu için bir Azure veritabanı oluşturma | Microsoft Docs"
description: "Hızlı Başlangıç Kılavuzu oluşturma ve Azure veritabanı PostgreSQL server için Azure portalı kullanıcı arabirimini kullanarak yönetme."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/10/2017
ms.openlocfilehash: 3a76e816f9b1fa484789f548899d7e8e7043febb
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Azure portalında bir Azure veritabanı PostgreSQL sunucu için oluşturma

Azure veritabanı PostgreSQL için çalıştırın, yönetmek ve yüksek oranda kullanılabilir PostgreSQL veritabanları bulutta ölçeklendirmek için kullandığınız yönetilen bir hizmettir. Bu Hızlı Başlangıç, PostgreSQL sunucu için bir Azure veritabanı yaklaşık beş dakika içinde Azure portalını kullanarak oluşturulacağını gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve gidin [portal](https://portal.azure.com/). Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

Bir PostgreSQL server Azure veritabanı oluşturmak için aşağıdaki adımları uygulayın:
1. Portalın sol üst köşesinde bulunan **Yeni** düğmesini (+) seçin.

2. Seçin **veritabanları** > **PostgreSQL için Azure veritabanı**.

    !["PostgreSQL için Azure veritabanı" seçeneği](./media/quickstart-create-database-portal/1-create-database.png)

3. Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Sunucu adı |*mypgserver-20170401*|Azure veritabanınız PostgreSQL sunucu için tanımlayan benzersiz bir ad. Etki alanı adı *postgres.database.azure.com* sağladığınız sunucu adı eklenir. Sunucu, yalnızca küçük harf, sayı ve tire (-) karakterini içerebilir. En az 3-63 karakter içermelidir.
    Abonelik|Aboneliğiniz|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden çok aboneliğiniz varsa, kaynak için fatura abonelik seçin.
    Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya mevcut bir aboneliğiniz.
    Sunucu yöneticisi oturum açma |*mylogin*| Sunucuya bağlandığınızda kullanılacak kendi oturum açma hesabı. Yönetici oturum açma adı olamaz **azure_superuser,** **azure_pg_admin,** **yönetici,** **yönetici,** **kök,** **konuk,** veya **ortak.** İle başlayamaz **pg_**.
    Parola |Tercih ettiğiniz | Sunucu yönetici hesabı için yeni bir parola. 8 ila 128 karakter arası içermelidir. Parolanızı aşağıdaki kategoriden üçünden karakterler içermelidir: İngilizce büyük harfler, küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vs.).
    Konum|Kullanıcılarınız için en yakın bölgeyi| Kullanıcılarınız için en yakın konumu.
    PostgreSQL sürüm|En son sürümü| En son sürümünü belirli gereksinimlere sahip değilse.
    Fiyatlandırma katmanı | **Temel**, **50 İşlem Birimi** **50 GB** | Yeni veritabanınızın hizmet katmanı ve performans düzeyi. Seçin **fiyatlandırma katmanı**. Ardından, **temel** sekmesi. Sol sonuna seçme **işlem birimleri** Bu Hızlı Başlangıç için en düşük miktarı değerine ayarlamak için kaydırıcıyı. Fiyatlandırma katmanı seçimi kaydetmeyi seçin **Tamam**. Daha fazla bilgi için aşağıdaki ekran görüntüsüne bakın. 
    Panoya sabitle | İşaretli | Ön pano sayfası portalınızdaki sunucunuzun kolay izlemeyi etkinleştirir.

    > [!IMPORTANT]
    > Sunucu Yöneticisi oturum açma ve burada belirttiğiniz parola, sunucu ve veritabanlarını Bu hızlı başlangıç devamındaki oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

    !["Fiyatlandırma katmanı" bölmesi](./media/quickstart-create-database-portal/2-service-tier.png)

4. Sunucuyu sağlamak için **Oluştur**’u seçin. Hazırlama işlemi 20 dakika kadar sürebilir.

5. Araç çubuğunda seçin **bildirimleri** dağıtım işlemi izlemek için simge.

    !["Bildirimler" bölmesi](./media/quickstart-create-database-portal/3-notifications.png)
   
  Varsayılan olarak, bir **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere tasarlanmıştır ve varsayılan bir veritabanı bir veritabanıdır. 

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

Azure veritabanı PostgreSQL için sunucu düzeyinde güvenlik duvarı oluşturur. Belirli IP adresleri için Güvenlik Duvarı'nı açmak için bir kural oluşturmadan, harici uygulamalar ve araçları sunucu ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1. Dağıtım tamamlandıktan sonra sunucunuzu bulun. Gerekirse arama yapabilirsiniz. Örneğin, sol taraftaki menüsünden seçin **tüm kaynakları**. Örnek gibi sunucu adınızı yazın **mypgserver 20170401**, yeni oluşturulan sunucunuz için aranacak. Arama sonuç listesinden sunucunuzun adını seçin. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.
 
    ![Sunucu adı arama](./media/quickstart-create-database-portal/4-locate.png)

2. Sunucu sayfasında **Bağlantı güvenliği**’ni seçin.

    !["Bağlantı güvenliği" ayarını](./media/quickstart-create-database-portal/5-firewall-2.png)

3. Altında **güvenlik duvarı kuralları** başlık, buna **kural adı** boş bir metin kutusunda güvenlik duvarı kuralı oluşturmaya başlamak için sütun seçin. 

    Bu Hızlı Başlangıç için şimdi Server'a tüm IP adreslerini verin. Metin kutusuna her sütunda aşağıdaki değerlerle doldurun:

    Kural adı | Başlangıç IP’si | Bitiş IP’si 
    ---|---|---
    AllowAllIps | 0.0.0.0 | 255.255.255.255

4. **Bağlantı güvenliği** sayfasının üst araç çubuğunda **Kaydet**’i seçin. Devam etmeden önce bağlantı güvenlik güncelleştirmesi başarıyla tamamlandığını belirten bildirim görünene kadar bekleyin.

    > [!NOTE]
    > PostgreSQL sunucusu için Azure Veritabanınıza yönelik bağlantılar 5432 bağlantı noktası üzerinden iletişim kurar. Şirket ağı içinde bağlanması çalışırsanız, bağlantı noktası 5432 üzerinden giden trafik, ağınızın güvenlik duvarı tarafından izin verilmeyebilir. BT departmanınız 5432 bir bağlantı noktası açar sürece bu durumda, sunucunuza bağlanamıyor.
    >

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

Azure veritabanınızı PostgreSQL sunucusu oluşturduğunuzda, varsayılan bir veritabanı adlı **postgres** oluşturulur. Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgilerinizi gerekir. Bu değerleri Hızlı Başlangıç makalesinde daha önce not almış olabilirsiniz. Almadıysanız, sunucu adını ve oturum açma bilgileri sunucuda kolayca bulabilirsiniz **genel bakış** portalında sayfası.

Sunucunuzun **Genel Bakış** sayfasını açın. Not **sunucu adı** ve **sunucu yönetici oturum açma adı**. İmleç her bir alan getirin ve metnin sağ Kopyala simgesi görünür. Kopya sembol değerleri kopyalamak için gerektiği şekilde seçin.

 ![Sunucu "Genel bakış" sayfası](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-the-postgresql-database-by-using-psql-in-cloud-shell"></a>Bulut Kabuğu'nda psql kullanarak PostgreSQL veritabanına bağlan

PostgreSQL sunucusu için Azure veritabanınıza çeşitli uygulamalar kullanarak bağlanabilirsiniz. İlk olarak sunucuya nasıl bağlanılacağını göstermek için psql komut satırı yardımcı programını kullanalım. Bir web tarayıcısı ve Azure bulut Kabuk gerek olmadan burada açıklandığı gibi ek yazılım yüklemek için kullanabilirsiniz. Kendi makinede yerel olarak yüklü psql yardımcı programı varsa, oradan da bağlanabilirsiniz.

1. Üst Gezinti Bölmesi'nde bulut Kabuğu'nu açmak için terminal simgeyi seçin.

   ![Azure bulut Kabuk terminal simgesi](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Bulut Kabuğu'nu Bash Kabuk komutları yazabileceğiniz tarayıcınızda açar.

   ![Bulut Kabuk Bash istemi](./media/quickstart-create-database-portal/8-bash.png)

3. Azure veritabanınızı PostgreSQL sunucusu için veritabanında psql komut satırı yazarak bulut Kabuk isteminde bağlayın.

    PostgreSQL sunucusu için bir Azure veritabanına bağlanmak için [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programı, aşağıdaki biçimi kullanın:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Örneğin, aşağıdaki komut bir örnek sunucuya bağlanır:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    --host | Sunucu adı | Daha önce PostgreSQL sunucu için Azure veritabanı oluştururken kullandığınız sunucu adı değeri. Gösterilen bizim örnek sunucu **mypgserver 20170401.postgres.database.azure.com.** Tam etki alanı adını kullanın (**\*. postgres.database.azure.com**) örnekte gösterildiği gibi. Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. 
    --port | 5432 | PostgreSQL server için Azure veritabanına bağlanırken kullanılacak bağlantı noktası. 
    --username | Sunucu yönetici oturum açma adı |Daha önce PostgreSQL sunucu için Azure veritabanı oluşturduğunuzda, sağladığınız sunucu yönetici oturum açma kullanıcı adı. Kullanıcı adınızı hatırlamıyorsanız bağlantı bilgilerini almak için önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    --dbname | *postgres* | Varsayılan, ilk bağlantı için oluşturulmuş sistem tarafından oluşturulan veritabanı adı. Daha sonra kendi veritabanı oluşturun.

    Sonra kendi parametre değerleri ile psql komutunu çalıştırın, sunucu yöneticisi parolasını girmeniz istenir. Bu parola sunucu oluşturduğunuzda, verdiğiniz adla aynı değil. 

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    password | Yönetici parolası | Yazılan parola karakterlerini bash satırına gösterilmeyen. Tüm karakterleri yazdıktan sonra seçin **Enter** kimlik doğrulaması ve bağlanmak için anahtarı.

    Bağlandıktan sonra psql yardımcı programı sql komutlarını girildiği postgres istemini görüntüler. Bulut Kabuğu'nda psql PostgreSQL server sürümü için Azure veritabanına sürümünden farklı bir sürüm olabileceğinden ilk bağlantı çıktıda bir uyarı görüntülenebilir. 
    
    Örnek psql çıktısı:
    ```bash
    psql (9.5.7, server 9.6.2)
    WARNING: psql major version 9.5, server major version 9.6.
        Some psql features might not work.
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
    Type "help" for help.
   
    postgres=> 
    ```

    > [!TIP]
    > Güvenlik Duvarı bulut Kabuk IP adresine izin vermek için yapılandırılmamışsa, aşağıdaki hata oluşur:
    > 
    > "psql: Önemli: pg_hba.conf giriş ana"138.91.195.82"kullanıcı"mylogin"veritabanı"postgres", önemli SSL: SSL bağlantısı gereklidir. SSL seçenekleri belirtin ve yeniden deneyin.
    > 
    > Hatayı gidermek için sunucu yapılandırması, bu makalenin "Sunucu düzeyinde güvenlik duvarı kuralı yapılandırma" bölümündeki adımları eşleştiğinden emin olun.

4. Aşağıdaki komutu yazarak istemde boş bir veritabanı oluşturun:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Komutun tamamlanması için birkaç dakika sürebilir. 

5. İsteminde bağlantıları yeni oluşturulan veritabanına geçiş yapmak için aşağıdaki komutu yürütün **mypgsqldb**:
    ```bash
    \c mypgsqldb
    ```

6. Tür `\q`ve ardından **Enter** psql çıkmak için anahtar. Tamamlanmış sonra bulut Kabuk kapatabilirsiniz.

Şimdi PostgreSQL server için Azure veritabanı bağlandınız ve boş kullanıcı veritabanı oluşturuldu. Başka bir ortak aracı, pgAdmin kullanarak bağlanmak için sonraki bölüme geçin.

## <a name="connect-to-the-postgresql-database-by-using-pgadmin"></a>PgAdmin kullanarak PostgreSQL veritabanına bağlan

GUI aracı pgAdmin kullanarak Azure PostgreSQL sunucuya bağlanmak için:
1. İstemci bilgisayarınızda pgAdmin uygulamasını açın. Gelen pgAdmin yükleyebilirsiniz [pgAdmin Web sitesi](http://www.pgadmin.org/).

2. Pano sayfasındaki altında **hızlı bağlantılar** bölümünde, select **yeni sunucu Ekle** simgesi.

3. İçinde **Create - Server** iletişim kutusundaki **genel** sekmesinde, sunucu için benzersiz bir kolay ad girin **Azure PostgreSQL sunucusu**.

    !["Genel" sekmesindeki](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)

4. İçinde **Create - Server** iletişim kutusundaki **bağlantı** sekmesinde ayarları belirtildiği şekilde kullanın ve ardından **kaydetmek**.

   !["Bağlantı" sekmesi](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    pgAdmin parametresi |Önerilen değer|Açıklama
    ---|---|---
    Konak Adı/Adresi | Sunucu adı | Daha önce PostgreSQL sunucu için Azure veritabanı oluştururken kullandığınız sunucu adı değeri. Bizim örnek sunucu **mypgserver 20170401.postgres.database.azure.com.** Tam etki alanı adını kullanın (**\*. postgres.database.azure.com**) örnekte gösterildiği gibi. Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. 
    Bağlantı noktası | 5432 | PostgreSQL server için Azure veritabanına bağlanırken kullanılacak bağlantı noktası. 
    Bakım veritabanı | *postgres* | Varsayılan sistem tarafından oluşturulan veritabanı adı.
    Kullanıcı adı | Sunucu yönetici oturum açma adı | Daha önce PostgreSQL sunucu için Azure veritabanı oluşturduğunuzda, sağladığınız sunucu yönetici oturum açma kullanıcı adı. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    Parola | Yönetici parolası | Parola, seçtiğiniz bu Quickstart önceki sunucu oluşturduğunuzda.
    Rol | Boş bırakın | Rol adı bu noktada sağlamaya gerek yoktur. Alanı boş bırakın.
    SSL modu | Gerekli | Varsayılan olarak, tüm Azure PostgreSQL sunucularının SSL ile oluşturulan zorlamayı açık. SSL zorlamayı devre dışı bırakmak için bkz: [Zorla SSL](./concepts-ssl-connection-security.md).
    
5. **Kaydet**’i seçin.

6. İçinde **tarayıcı** sol bölmesinde **sunucuları** düğümü. Örneğin, sunucunuzun seçin **Azure PostgreSQL sunucusu**. Bağlanmak için tıklatın.

7. Sunucu düğümünü ve ardından onun altındaki **Veritabanları**’nı genişletin. Var olan içermelidir *postgres* veritabanı ve herhangi bir yeni oluşturulan kullanıcı veritabanı gibi **mypgsqldb**, önceki bölümde oluşturduğumuz. Sunucu başına birden çok veritabanı için PostgreSQL Azure veritabanı oluşturabilirsiniz dikkat edin.

8. Sağ **veritabanları**, seçin **oluşturma** menüsüne ve ardından **veritabanı**.

9. Tercih ettiğiniz bir veritabanı adı yazın **veritabanı** gibi alan **mypgsqldb**, örnekte gösterildiği gibi.

10. Seçin **sahibi** liste kutusundan veritabanı için. Bizim örneğimizde gibi yönetici oturum açma adı sunucunuzu seçin **mylogin**.

11. Seçin **kaydetmek** yeni bir boş veritabanı oluşturmak için.

12. İçinde **tarayıcı** bölmesinde veritabanları listesinde, sunucu adı altında oluşturulan veritabanı bakın.

    !["Tarayıcı" bölmesi](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı Başlangıç iki yoldan biriyle oluşturduğunuz kaynakları temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynaklar korumanız istiyorsanız, yalnızca tek bir sunucu kaynak silin.

> [!TIP]
> Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmek düşünmüyorsanız bu hızlı başlangıç portalında tarafından oluşturulan kaynakları silmek için aşağıdaki adımları izleyin.

Yeni oluşturulan server dahil olmak üzere tüm kaynak grubunu silmek için:
1. Kaynak grubunuzun portalda bulun. Sol taraftaki menüden seçin **kaynak grupları**. Bizim örneğimizde gibi kaynak grubunuzun adını seçip **myresourcegroup**.

2. Kaynak grubunuzun sayfasında **Sil**’i seçin. Bizim örneğimizde gibi kaynak grubunuzun adını yazın **myresourcegroup**, silmeyi onaylamak için metin kutusundaki. **Sil**’i seçin.

Yalnızca yeni oluşturulan sunucusunu silmek için:
1. Açık sahip değilseniz, sunucunuzun Portalı'nda bulun. Sol taraftaki menüden seçin **tüm kaynakları**. Ardından, oluşturduğunuz sunucuyu arayın.

2. **Genel Bakış** sayfasında **Sil**’i seçin.

    !["Sil" düğmesini](./media/quickstart-create-database-portal/12-delete.png)

3. Silmek istediğiniz sunucu adını doğrulayın ve altındaki etkilenen veritabanlarını görüntüleyin. Bizim örneğimizde gibi metin kutusuna sunucunuzun adını yazın **mypgserver 20170401**. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
