---
title: "Azure portalı: PostgreSQL için Azure Veritabanı oluşturma | Microsoft Belgeleri"
description: "Azure portalı kullanıcı arabirimini kullanarak PostgreSQL için Azure Veritabanı oluşturma ve yönetmeye yönelik hızlı başlangıç kılavuzu."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/04/2017
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: e89058a500aeb542ae4c7dc6bb4a9b9ae54db7f2
ms.contentlocale: tr-tr
ms.lasthandoff: 08/09/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Azure portalında PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Bu hızlı başlangıçta, Azure portalını kullanarak nasıl PostgreSQL için Azure Veritabanı sunucusu oluşturacağınız gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

PostgreSQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:
1.  Azure portalının sol üst köşesinde bulunan **Yeni** (+) düğmesine tıklayın.
2.  **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **PostgreSQL için Azure Veritabanı**’nı seçin.
 ![PostgreSQL için Azure Veritabanı - Veritabanı oluşturma](./media/quickstart-create-database-portal/1-create-database.png)

3.  Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Sunucu adı |*mypgserver-20170401*|Azure veritabanınızı PostgreSQL sunucusuna tanıtan benzersiz bir ad seçin. *postgres.database.azure.com* etki alanı adı, uygulamaların bağlanması için sağladığınız sunucu adına eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 63 karakterden oluşmalıdır.
    Abonelik|*Aboneliğiniz*|Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin.
    Kaynak Grubu|*myresourcegroup*| Yeni bir kaynak grubu adı oluşturabilir veya mevcut bir aboneliğinizi kullanabilirsiniz.
    Sunucu yöneticisi oturum açma |*mylogin*| Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınızı yapın. Yönetici oturum açma adı 'azure_superuser', 'azure_pg_admin', 'admin', 'administrator', 'root', 'guest' veya 'public' olamaz ve 'pg_' ile başlayamaz.
    Parola |*Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola oluşturun. 8 ila 128 karakter arası içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır – İngilizce büyük ve küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Konum|*Kullanıcılarınıza en yakın bölge*| Kullanıcılarınız için en yakın konumu seçin.
    PostgreSQL Sürümü|*En son sürümü seçin*| Belirli gereksinimleriniz yoksa en son sürümü seçin.
    Fiyatlandırma Katmanı | **Temel**, **50 İşlem Birimi** **50 GB** | Yeni veritabanınıza ait hizmet katmanını ve performans düzeyini belirtmek için **Fiyatlandırma katmanı**’na tıklayın. En üstteki sekmede Temel katmanı seçin. Bu hızlı başlangıç için, İşlem Birimleri kaydırıcısının sol ucuna tıklayarak değeri mümkün olan en düşük değere ayarlayın. Fiyatlandırma katmanını kaydetmek için **Tamam**’a tıklayın. Aşağıdaki ekran görüntüsüne bakın.
    | Panoya sabitle | İşaretli | Azure Portal'ın ön pano sayfasında sunucunuzun kolayca izlenmesine izin vermek için **Panoya Sabitle** seçeneğini işaretleyin.

  > [!IMPORTANT]
  > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu hızlı başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.

    ![PostgreSQL için Azure Veritabanı - fiyatlandırma katmanını seçme](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika, en fazla 20 dakika alır.

5.  Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
 ![PostgreSQL için Azure Veritabanı - Bildirimleri inceleme](./media/quickstart-create-database-portal/3-notifications.png)
   
  Varsayılan olarak, **postgres** veritabanı sunucunuz altında oluşturulur. [Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) veritabanı; kullanıcılar, yardımcı programlar ve üçüncü taraf uygulamalar tarafından kullanılmak üzere geliştirilmiş varsayılan bir veritabanıdır. 

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

PostgreSQL için Azure Veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, belirli IP adresleri için güvenlik duvarını açmak üzere bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1.  Dağıtım tamamlandıktan sonra sunucunuzu bulun. Gerekirse arama yapabilirsiniz. Örneğin, sol taraftaki menüden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için sunucu adını (ör. mypgserver-20170401) yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun Genel bakış sayfası açılır ve daha fazla yapılandırma seçeneği sunulur.
 
    ![PostgreSQL için Azure Veritabanı - Sunucu arama ](./media/quickstart-create-database-portal/4-locate.png)

2.  Sunucu sayfasında **Bağlantı Güvenliği**’ni seçin. 
    ![PostgreSQL için Azure Veritabanı - Güvenlik Duvarı Kuralı Oluşturma](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  **Güvenlik Duvarı Kuralları** başlığı altında, güvenlik duvarı kuralı oluşturmaya başlamak için **Kural Adı** sütunundaki boş metin kutusuna tıklayın. 

    Bu hızlı başlangıçta, tüm sütunlardaki metin kutularını aşağıdaki değerlerle doldurarak tüm IP adreslerinin sunucuya bağlanmasına izin verelim:

    Kural Adı | Başlangıç IP’si | Bitiş IP’si 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Bağlantı güvenliği sayfasının üst araç çubuğunda **Kaydet**’e tıklayın. Ardından **X**’e tıklayarak Bağlantı Güvenliği sayfasını kapatın.

    > [!NOTE]
    > Azure PostgreSQL sunucusu, 5432 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 5432 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 5432 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
  >

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma

PostgreSQL sunucusu için Azure Veritabanımızı oluşturduğumuzda, **postgres** adlı varsayılan veritabanı da oluşturulur. Veritabanı sunucusuna bağlanmak için tam sunucu adını ve yönetici oturum açma kimlik bilgilerini hatırlamanız gerekir. Bu değerleri hızlı başlangıç makalesinde daha önce not almış olabilirsiniz. Almadıysanız, Azure portalında sunucuya Genel Bakış sayfasında sunucu adını ve oturum açma bilgilerini kolayca bulabilirsiniz.

1. Sunucunuzun **Genel Bakış** sayfasını açın. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin.
    İmlecinizi her bir alanın üzerine getirin, metnin sağ tarafında Kopyala simgesi görünür. Değerleri kopyalamak için gerektiği şekilde Kopyala simgesine tıklayın.

 ![PostgreSQL için Azure Veritabanı - Sunucu Yöneticisi Oturum Açma](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Cloud Shell’de psql’i kullanarak PostgreSQL veritabanına bağlanma

PostgreSQL sunucusu için Azure veritabanınıza çeşitli uygulamalar kullanarak bağlanabilirsiniz. İlk olarak sunucuya nasıl bağlanılacağını göstermek için psql komut satırı yardımcı programını kullanalım.  Burada açıklandığı şekliyle bir web tarayıcısını ve Azure Cloud Shell’i herhangi bir ek yazılım yüklemeniz gerekmeden kullanabilirsiniz. Kendi makinede yerel olarak yüklü psql yardımcı programı varsa, oradan da bağlanabilirsiniz.

1. Sol gezinme bölmesindeki terminal simgesiyle Azure Cloud Shell’i başlatın.

   ![PostgreSQL için Azure Veritabanı - Azure Cloud Shell terminal simgesi](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Azure Cloud Shell, tarayıcınızda açılarak bash kabuk komutları yazmanıza imkan tanır.

   ![PostgreSQL için Azure Veritabanı - Azure Shell Bash İstemi](./media/quickstart-create-database-portal/8-bash.png)

3. Cloud Shell isteminde, yeşil istemde psql komut satırını yazarak PostgreSQL için Azure Veritabanı sunucunuzdaki bir veritabanına bağlanın.

    Aşağıdaki biçim, [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) yardımcı programıyla PostgreSQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Örneğin, aşağıdaki komut bir örnek sunucuya bağlanır:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    --host | *sunucu adı* | PostgreSQL için Azure Veritabanını oluştururken kullandığınız sunucu adı değerini belirtin. Gösterilen örnek sunucumuz: mypgserver-20170401.postgres.database.azure.com. Örnekte gösterildiği gibi tam etki alanı adını kullanın (\*. postgres.database.azure.com). Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için önceki bölümü takip edin. 
    --port | **5432** | PostgreSQL Azure veritabanına bağlanırken her zaman bağlantı noktası olarak 5432 kullanın. 
    --username | *sunucu yöneticisi oturum açma adı* |PostgreSQL için Azure Veritabanını oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adını yazın. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için önceki bölümü takip edin.  Biçim şöyledir: *username@servername*.
    --dbname | **postgres** | İlk bağlantı için, sistem tarafından oluşturulan varsayılan veritabanı adını (*postgres*) kullanın. Daha sonra kendi veritabanınızı oluşturun.

    Kendi parametre değerleriniz ile psql komutu çalıştırdıktan sonra sunucu yöneticisi parolasını yazmanız istenir. Bu, sunucuyu oluştururken belirttiğiniz paroladır. 

    psql parametresi |Önerilen değer|Açıklama
    ---|---|---
    password | *yönetici parolanız* | Yazılan parola karakterlerinin bash isteminde gösterilmeyeceğini unutmayın. Kimlik doğrulaması yapmak ve bağlanmak için tüm karakterleri yazdıktan sonra enter tuşuna basın.

4.  Sunucuya bağlandıktan sonra, aşağıdaki komutu yazarak istemde boş bir veritabanı oluşturun.
    ```bash
    CREATE DATABASE mypgsqldb;
    ```

5.  İstemde, bağlantıyı yeni oluşturulan **mypgsqldb** veritabanına geçirmek için aşağıdaki komutu yürütün.
    ```bash
    \c mypgsqldb
    ```

6.  psql’den çıkmak için \q yazıp ENTER tuşuna basın. Artık Azure Cloud Shell’i kapatabilirsiniz.

Böylece Azure Veritabanını PostgreSQL’ye bağladınız ve boş bir kullanıcı veritabanı oluşturdunuz. Başka bir ortak araç olan pgAdmin’i kullanarak bağlanmak için sonraki bölüme geçin.

## <a name="connect-to-postgresql-database-using-pgadmin"></a>pgAdmin’i kullanarak PostgreSQL veritabanına bağlanma

_pgAdmin_ GUI aracını kullanarak Azure PostgreSQL sunucusuna bağlanmak için
1.  İstemci bilgisayarınızda _pgAdmin_ uygulamasını başlatın. _pgAdmin_’i http://www.pgadmin.org/ adresinden yükleyebilirsiniz.
2.  **Hızlı Bağlantılar** menüsünden **Yeni Sunucu Ekle**’yi seçin.
3.  **Oluştur - Sunucu** iletişim kutusunun **Genel** sekmesinde, sunucu için **Azure PostgreSQL Sunucusu** gibi benzersiz ve kolay bir ad girin.
![pgAdmin aracı - oluştur - sunucu](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  **Oluştur - Sunucu** iletişim kutusunun **Bağlantı** sekmesinde belirtilen ayarları kullanın ve **Kaydet**’e tıklayın.
   ![pgAdmin - Oluştur - Sunucu](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)
    - **Ana Bilgisayar Adı/Adresi**: mypgserver-20170401.postgres.database.azure.com 
        - Tam sunucu adı.
    - **Bağlantı noktası:** 5432
        - Bu veritabanı sunucusu tarafından kullanılan bağlantı noktası numarası 5432'dir.
    - **Bakım Veritabanı**: postgres 
        - Sistem tarafından oluşturulan varsayılan veritabanı adı.
    - **Kullanıcı Adı:** mylogin@mypgserver-20170401 
        - Bu hızlı başlangıçta daha önce elde edilen Sunucu yöneticisi oturum açma bilgileri (user@mypgserver).
    - **Parola**: Bu hızlı başlangıçta daha önce sunucuyu oluştururken seçtiğiniz parola.
    - **SSL Modu**: Gerekli kıl
        - Tüm Azure PostgreSQL sunucuları, varsayılan olarak SSL’yi zorunla tutma seçeneği AÇIK konumundayken oluşturulur. SSL’yi zorunlu tutma seçeneğini KAPALI konumuna getirmek için [SSL’yi zorunlu tutma](./concepts-ssl-connection-security.md) bölümündeki ayrıntıları inceleyin.
5.  **Kaydet** düğmesine tıklayın.
6.  Tarayıcı sol bölmesinde **Sunucu Grupları**’nı genişletin. **Azure PostgreSQL Sunucusu** adlı sunucunuzu seçin.
7.  Bağlandığınız **Sunucu**’yu ve ardından altındaki **Veritabanları**’nı seçin. 
8.  Veritabanı oluşturmak için **Veritabanları**’na sağ tıklayın.
9.  Veritabanı adı için **mypgsqldb**’yi ve sahibi için sunucu yöneticisi oturum açma bilgileri olarak **mylogin**’i seçin.
10. Boş bir veritabanı oluşturmak için **Kaydet**’e tıklayın.
11. **Tarayıcı**’da **Sunucu**’yu genişletin. Oluşturduğunuz sunucuyu genişletin ve altındaki **mypgsqldb** veritabanını bulun.
 ![pgAdmin - Oluştur - Veritabanı](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
[Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silerek hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin.

> [!TIP]
> Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1.  Azure portalında sol taraftaki menüden, **Kaynak grupları**’na ve ardından **myresourcegroup**’a tıklayın.
2.  Kaynak grubu sayfanızda **Sil**’e tıklayın, metin kutusuna **myresourcegroup** yazıp Sil’e tıklayın.

Yalnızca yeni oluşturulan sunucuyu silmek istiyorsanız:
1.  Azure portalında sol taraftaki menüden PostgreSQL sunucuları’na tıklayın ve kısa süre önce oluşturduğunuz sunucuyu aratın
2.  Genel bakış sayfasında, ![PostgreSQL için Azure Veritabanı - Sunucuyu silme](./media/quickstart-create-database-portal/12-delete.png) üst bölmesinde Sil düğmesine tıklayın
3.  Silmek istediğiniz sunucu adını onaylayın ve altındaki etkilenen veritabanlarını gösterin. Metin kutusuna **mypgserver-20170401** yazıp Sil’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)

