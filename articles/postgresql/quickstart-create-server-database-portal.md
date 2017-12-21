---
title: "Azure Portal: PostgreSQL için Azure Veritabanı sunucusu oluşturma | Microsoft Docs"
description: "Azure Portal kullanıcı arabirimini kullanarak PostgreSQL için Azure Veritabanı sunucusu oluşturma ve yönetmeye yönelik hızlı başlangıç kılavuzu."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/03/2017
ms.openlocfilehash: b78009a4b2683bb7ee881808ddbbc792d66dea6c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Azure portalında PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu Hızlı Başlangıçta, Azure Portal kullanarak yaklaşık beş dakika içinde PostgreSQL için Azure Veritabanı sunucusunun nasıl oluşturulduğu gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
Web tarayıcınızı açın ve [portala](https://portal.azure.com/) gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

PostgreSQL için Azure Veritabanı sunucusu oluşturmak için şu adımları uygulayın:
1. Portalın sol üst köşesinde bulunan **Yeni** düğmesini (+) seçin.

2. **Veritabanları** > **PostgreSQL için Azure Veritabanı**'nı seçin.

    !["PostgreSQL için Azure Veritabanı" seçeneği](./media/quickstart-create-database-portal/1-create-database.png)

3. Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Sunucu adı |*mypgserver-20170401*|PostgreSQL için Azure Veritabanı sunucunuzu tanıtan benzersiz bir ad. Girdiğiniz sunucu adına *postgres.database.azure.com* etki alanı adı eklenir. Sunucunuz yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. En az 3, en çok 63 karakterden oluşmalıdır.
    Abonelik|Aboneliğiniz|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalandırıldığı aboneliği seçin.
    Kaynak grubu|*myresourcegroup*| Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Sunucu yöneticisi oturum açma |*mylogin*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınız. Yönetici oturum açma adı **azure_superuser,** **azure_pg_admin,** **admin,** **administrator,** **root,** **guest,** veya **public** olamaz. Bu ad **pg_** ile başlayamaz.
    Parola |Tercih ettiğiniz | Sunucu yönetici hesabı için yeni bir parola. 8 ila 128 karakter arası içermelidir. Parolanız şu kategorilerden üçünde yer alan karakterlerden oluşmalıdır: İngilizce büyük harfler, İngilizce küçük harfler, sayılar (0 - 9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Konum|Kullanıcılarınıza en yakın bölge| Kullanıcılarınız için en yakın olan konum.
    PostgreSQL sürümü|En son sürüm| Belirli gereksinimleriniz olmadığı sürece, en son sürüm.
    Fiyatlandırma katmanı | **Temel**, **50 İşlem Birimi** **50 GB** | Yeni veritabanınızın hizmet katmanı ve performans düzeyi. **Fiyatlandırma katmanı**'nı seçin. Sonra **Temel** sekmesini seçin. Ardından bu Hızlı Başlangıç için, **İşlem Birimleri** kaydırıcısının sol ucunu seçerek değeri mümkün olan en düşük değere ayarlayın. Fiyatlandırma katmanı seçimini kaydetmek için **Tamam**’ı seçin. Daha fazla bilgi için aşağıdaki ekran görüntüsüne bakın. 
    Panoya sabitle | İşaretli | Portalınızın ön pano sayfasında sunucunuzun kolayca izlenmesine izin verir.

    > [!IMPORTANT]
    > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu Hızlı Başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

    !["Fiyatlandırma katmanı" bölmesi](./media/quickstart-create-database-portal/2-service-tier.png)

4. Sunucuyu sağlamak için **Oluştur**’u seçin. Hazırlama işlemi 20 dakika kadar sürebilir.

5. Araç çubuğunda, dağıtım sürecini izlemek için **Bildirimler** simgesini seçin.

    !["Bildirimler" bölmesi](./media/quickstart-create-database-portal/3-notifications.png)
   
  Varsayılan olarak, sunucunuzun altında bir **postgres** veritabanı oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamaları tarafından kullanılmak üzere geliştirilmiş, varsayılan bir veritabanıdır. 

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

PostgreSQL için Azure Veritabanı, sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, belirli IP adresleri için güvenlik duvarını açmak üzere bir kural oluşturmadığınız sürece, dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1. Dağıtım tamamlandıktan sonra sunucunuzu bulun. Gerekirse arama yapabilirsiniz. Örneğin, soldaki menüden **Tüm kaynaklar**'ı seçin. Yeni oluşturulan sunucunuzu aramak için sunucunuzun adını (örnekte **mypgserver-20170401**) yazın. Arama sonuçları listesinden sunucunuzun adını seçin. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.
 
    ![Sunucu adı araması](./media/quickstart-create-database-portal/4-locate.png)

2. Sunucu sayfasında **Bağlantı güvenliği**’ni seçin.

    !["Bağlantı güvenliği" ayarı](./media/quickstart-create-database-portal/5-firewall-2.png)

3. **Güvenlik duvarı kuralları** başlığı altında, **Kural Adı** sütunundaki boş metin kutusunu seçerek güvenlik duvarı kuralı oluşturmaya başlayın. 

    Bu Hızlı Başlangıç için, sunucuda tüm IP adreslerine izin verin. Her sütundaki metin kutusunu aşağıdaki değerlerle doldurun:

    Kural adı | Başlangıç IP’si | Bitiş IP’si 
    ---|---|---
    AllowAllIps | 0.0.0.0 | 255.255.255.255

4. **Bağlantı güvenliği** sayfasının üst araç çubuğunda **Kaydet**’i seçin. Devam etmeden önce bağlantı güvenliği güncelleştirmesinin başarıyla tamamlandığını belirten bildirim görünene kadar bekleyin.

    > [!NOTE]
    > PostgreSQL sunucusu için Azure Veritabanınıza yönelik bağlantılar 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
    >

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

PostgreSQL için Azure Veritabanı sunucunuzu oluşturduğunuzda, **postgres** adlı varsayılan veritabanı da oluşturulur. Veritabanı sunucunuza bağlanmak için tam sunucu adını ve yönetici oturum açma kimlik bilgileri gerekir. Bu değerleri Hızlı Başlangıç makalesinde daha önce not almış olabilirsiniz. Almadıysanız, portaldaki sunucuya **Genel Bakış** sayfasında sunucu adını ve oturum açma bilgilerini kolayca bulabilirsiniz.

Sunucunuzun **Genel Bakış** sayfasını açın. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not alın. İmlecinizi her alanın üzerine getirin; metnin sağ tarafında Kopyala simgesi görünür. Değerleri kopyalamak için gerektiği gibi Kopyala simgesini seçin.

 ![Sunucu "Genel Bakış" sayfası](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-the-postgresql-database-by-using-psql-in-cloud-shell"></a>Cloud Shell’de psql’yi kullanarak PostgreSQL Veritabanı'na bağlanma

PostgreSQL sunucusu için Azure veritabanınıza çeşitli uygulamalar kullanarak bağlanabilirsiniz. İlk olarak sunucuya nasıl bağlanılacağını göstermek için psql komut satırı yardımcı programını kullanalım. Burada açıklandığı şekliyle bir web tarayıcısını ve Azure Cloud Shell’i herhangi bir ek yazılım yüklemeniz gerekmeden kullanabilirsiniz. Kendi makinede yerel olarak yüklü psql yardımcı programı varsa, oradan da bağlanabilirsiniz.

1. Üst gezinti bölmesinde, Cloud Shell'i açmak için terminal simgesini seçin.

   ![Azure Cloud Shell terminal simgesi](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Cloud Shell, tarayıcınızda açılarak Bash kabuk komutları yazmanıza olanak verir.

   ![Cloud Shell Bash istemi](./media/quickstart-create-database-portal/8-bash.png)

3. Cloud Shell isteminde, psql komut satırını yazarak PostgreSQL için Azure Veritabanı sunucunuzdaki bir veritabanına bağlanın.

    [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanırken aşağıdaki biçimi kullanın:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Örneğin, aşağıdaki komut bir örnek sunucuya bağlanır:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    --host | Sunucu adı | PostgreSQL için Azure Veritabanı sunucusunu oluştururken kullandığınız sunucu adı değeri. Gösterilen örnek sunucu: **mypgserver-20170401.postgres.database.azure.com.** Örnekte gösterildiği gibi tam etki alanı adını kullanın (**\*. postgres.database.azure.com**). Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. 
    --port | 5432 | PostgreSQL için Azure Veritabanı sunucusuna bağlanırken kullanılacak bağlantı noktası. 
    --username | Sunucu yöneticisi oturum açma adı |PostgreSQL için Azure Veritabanı sunucusunu oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adı. Kullanıcı adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    --dbname | *postgres* | İlk bağlantı için sistem tarafından oluşturulan, varsayılan veritabanı adı. Daha sonra kendi veritabanınızı oluşturun.

    Kendi parametre değerleriniz ile psql komutunu çalıştırdıktan sonra sunucu yöneticisi parolasını girmeniz istenir. Bu parola, sunucuyu oluştururken belirttiğiniz parolayla aynıdır. 

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    password | Yönetici parolanız | Yazılan parola karakterleri Bash isteminde gösterilmez. Tüm karakterleri yazdıktan sonra, kimlik doğrulaması yapmak ve bağlanmak için **Enter** tuşuna seçin.

    Bağlantı kurduktan sonra, psql yardımcı programı sql komutlarını girdiğiniz bir postgres istemi görüntüler. Cloud Shell’deki psql ile PostgreSQL için Azure Veritabanı sunucu sürümü farklı olabileceğinden, ilk bağlantı çıkışında bir uyarı gösterilebilir. 
    
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
    > Güvenlik duvarı Cloud Shell IP adreslerine izin verecek biçimde yapılandırılmamışsa aşağıdaki hata oluşur:
    > 
    > "psql: FATAL:  "138.91.195.82" konağı, "mylogin" kullanıcısı, "postgres" veritabanı için pg_hba.conf girdisi yok, FATAL üzerinde SSL: SSL bağlantısı gerekiyor. SSL seçeneklerini belirtin ve yeniden deneyin.
    > 
    > Hatayı gidermek için sunucu yapılandırmasının bu makaledeki “Sunucu düzeyinde güvenlik duvarı kuralı yapılandırma” bölümünde yer alan adımlarla eşleştiğinden emin olun.

4. Aşağıdaki komutu yazarak istemde boş bir veritabanı oluşturun:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```
    Komutun bitmesi birkaç dakika sürebilir. 

5. İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün:
    ```bash
    \c mypgsqldb
    ```

6. `\q` yazın ve ardından **Enter** tuşunu seçerek psql'den çıkın. Bitirdikten sonra Cloud Shell’i kapatabilirsiniz.

Artık PostgreSQL için Azure Veritabanı sunucusuna bağlandınız ve boş bir kullanıcı veritabanı oluşturdunuz. Başka bir ortak araç olan pgAdmin’i kullanarak bağlanmak için sonraki bölüme geçin.

## <a name="connect-to-the-postgresql-database-by-using-pgadmin"></a>pgAdmin’i kullanarak PostgreSQL Veritabanı'na bağlanma

pgAdmin GUI aracını kullanarak Azure PostgreSQL sunucusuna bağlanmak için:
1. İstemci bilgisayarınızda pgAdmin uygulamasını açın. pgAdmin'i [pgAdmin web sitesinden](http://www.pgadmin.org/) yükleyebilirsiniz.

2. Pano sayfasındaki **Hızlı Bağlantılar** bölümünün altında **Yeni Sunucu Ekle** simgesini seçin.

3. **Oluştur - Sunucu** iletişim kutusunun **Genel** sekmesinde, sunucu için **Azure PostgreSQL Sunucusu** gibi benzersiz ve kolay bir ad girin.

    !["Genel" sekmesi](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)

4. **Oluştur - Sunucu** iletişim kutusunun **Bağlantı** sekmesinde belirtilen ayarları kullanın ve **Kaydet**’i seçin.

   !["Bağlantı" sekmesi](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)

    pgAdmin parametresi |Önerilen değer|Açıklama
    ---|---|---
    Konak Adı/Adresi | Sunucu adı | PostgreSQL için Azure Veritabanı sunucusunu oluştururken kullandığınız sunucu adı değeri. Örnek sunucumuz: **mypgserver-20170401.postgres.database.azure.com.** Örnekte gösterildiği gibi tam etki alanı adını kullanın (**\*. postgres.database.azure.com**). Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. 
    Bağlantı noktası | 5432 | PostgreSQL için Azure Veritabanı sunucusuna bağlanırken kullanılacak bağlantı noktası. 
    Bakım veritabanı | *postgres* | Sistem tarafından oluşturulan varsayılan veritabanı adı.
    Kullanıcı adı | Sunucu yöneticisi oturum açma adı | PostgreSQL için Azure Veritabanı sunucusunu oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adı. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    Parola | Yönetici parolanız | Bu Hızlı Başlangıçta daha önce sunucuyu oluştururken seçtiğiniz parola.
    Rol | Boş bırakın | Bu noktada bir rol adı sağlamanız gerekmez. Alanı boş bırakın.
    SSL modu | Gerekli | Tüm Azure PostgreSQL sunucuları, varsayılan olarak SSL’yi zorunla tutma seçeneği açık konumundayken oluşturulur. SSL'yi zorunlu tutma seçeneğini kapatmak için, bkz. [SSL'yi zorunlu tutma](./concepts-ssl-connection-security.md).
    
5. **Kaydet**’i seçin.

6. Sol taraftaki **Tarayıcı** bölmesinde **Sunucular** düğümünü genişletin. Sunucunuzu seçin; örneğin, **Azure PostgreSQL Sunucusu**. Bu sunucuya bağlanmak için tıklayın.

7. Sunucu düğümünü ve ardından onun altındaki **Veritabanları**’nı genişletin. Listenin mevcut *postgres* veritabanınızı ve bir önceki bölümde oluşturulan **mypgsqldb** gibi yeni oluşturulan tüm kullanıcı veritabanlarını içermesi gerekir. PostgreSQL için Azure Veritabanı ile sunucu başına birden çok veritabanı oluşturabildiğinize dikkat edin.

8. **Veritabanları**’na sağ tıklayın, **Oluştur** menüsünü seçin ve sonra da **Veritabanı**’nı seçin.

9. **Veritabanı** alanına, örnekte gösterildiği gibi **mypgsqldb** benzeri tercih ettiğiniz bir veritabanı adı yazın.

10. Liste kutusundan veritabanı için **Sahip**’i seçin. Sunucu yöneticinizin oturum açma adını (örnekteki **mylogin** gibi) seçin.

11. Yeni ve boş bir veritabanı oluşturmak için **Kaydet**’i seçin.

12. **Tarayıcı** bölmesinde, sunucu adınızın altındaki veritabanları listesinde oluşturduğunuz veritabanına bakın.

    !["Tarayıcı" bölmesi](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki yoldan biriyle temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silebilirsiniz. Diğer kaynakları olduğu gibi korumak istiyorsanız tek bir sunucu kaynağını silin.

> [!TIP]
> Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, portalda bu hızlı başlangıç ile oluşturulmuş olan kaynakları silmek için aşağıdaki adımları izleyin.

Yeni oluşturulan sunucu dahil olmak üzere kaynak grubunun tamamını silmek için:
1. Portalda kaynak grubunuzu bulun. Soldaki menüden **Kaynak grupları**'nı seçin. Ardından, kaynak grubunuzun adını (örnekteki **myresourcegroup** gibi) seçin.

2. Kaynak grubunuzun sayfasında **Sil**’i seçin. Kaynak grubunuzun adını (örnekteki **myresourcegroup** gibi) metin kutusuna yazarak silmeyi onaylayın. **Sil**’i seçin.

Yalnızca yeni oluşturulan sunucuyu silmek için:
1. Sunucunuz açık değilse portalda sunucuyu bulun. Soldaki menüden **Tüm kaynaklar**'ı seçin. Ardından, oluşturduğunuz sunucuyu arayın.

2. **Genel Bakış** sayfasında **Sil**’i seçin.

    !["Sil" düğmesi](./media/quickstart-create-database-portal/12-delete.png)

3. Silmek istediğiniz sunucu adını onaylayın ve altındaki etkilenen veritabanlarını görüntüleyin. Metin kutusuna sunucunuzun adını (örnekteki **mypgserver-20170401** gibi) yazın. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
