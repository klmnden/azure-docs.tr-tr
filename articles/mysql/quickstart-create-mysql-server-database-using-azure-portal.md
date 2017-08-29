---
title: "Hızlı Başlangıç: MySQL için Azure Veritabanı sunucusu Oluşturma - Azure portalı | Microsoft Docs"
description: "Bu makalede, Azure portalını kullanarak örnek bir MySQL için Azure Veritabanı sunucusunu yaklaşık beş dakika içinde hızlıca oluşturma adımları verilmektedir."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 829c7e73cbf22d866bbd6fd54edc7a954ad7174c
ms.contentlocale: tr-tr
ms.lasthandoff: 08/16/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı, bulutta yüksek oranda kullanılabilir olan MySQL veritabanları çalıştırmanızı, yönetmenizi ve ölçeklendirmenizi sağlayan ve yönetilen bir hizmettir. Bu Hızlı Başlangıçta, Azure portalını kullanarak yaklaşık beş dakikada nasıl MySQL için Azure Veritabanı sunucusu oluşturacağınız gösterilir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="create-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma
MySQL için Azure Veritabanı sunucusu, tanımlı bir dizi [işlem ve depolama kaynağı](./concepts-compute-unit-and-storage.md) ile oluşturulur. Sunucu, [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) içinde oluşturulur.

MySQL için Azure veritabanı sunucusu oluşturmak üzere şu adımları uygulayın:

1. Azure portalının sol üst köşesinde bulunan **Yeni** (+) düğmesine tıklayın.

2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **MySQL için Azure Veritabanı**’nı seçin. Yeni sayfa arama kutusuna **MySQL** yazarak da hizmeti bulabilirsiniz.
![Azure portalı - yeni - veritabanı - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

    **Ayar** | **Önerilen değer** | **Alan Açıklaması** 
    ---|---|---
    Sunucu adı | myserver4demo | Azure veritabanınızı MySQL sunucusuna tanıtan benzersiz bir ad seçin. *mysql.database.azure.com* etki alanı adı, uygulamaların bağlanması için sağladığınız sunucu adına eklenir. Sunucu adı yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 63 karakterden oluşmalıdır.
    Abonelik | Aboneliğiniz | Sunucunuz için kullanmak istediğiniz Azure aboneliği. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin.
    Kaynak grubu | myresourcegroup | Yeni bir kaynak grubu adı oluşturabilir veya mevcut bir aboneliğinizi kullanabilirsiniz.
    Sunucu yöneticisi oturum açma | myadmin | Sunucuya bağlanırken kullanılacak kendi oturum açma hesabınızı oluşturun. Yönetici oturum açma adı 'azure_superuser', 'admin', 'administrator', 'root', 'guest' veya 'public' olamaz.
    Parola | *Tercih ettiğiniz* | Sunucu yönetici hesabı için yeni bir parola oluşturun. 8 ila 128 karakter arası içermelidir. Parolanız şu üç kategoride yer alan karakterlerden oluşmalıdır – İngilizce büyük ve küçük harfler, sayılar (0-9) ve alfasayısal olmayan karakterler (!, $, #, %, vb.).
    Parolayı onayla | *Tercih ettiğiniz*| Yönetici hesabı parolasını onaylayın.
    Konum | *Kullanıcılarınıza en yakın bölge*| Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
    Sürüm | *En son sürümü seçin*| Belirli gereksinimleriniz yoksa en son sürümü seçin.
    Fiyatlandırma Katmanı | **Temel**, **50 İşlem Birimi** **50 GB** | Yeni veritabanınıza ait hizmet katmanını ve performans düzeyini belirtmek için **Fiyatlandırma katmanı**’na tıklayın. En üstteki sekmedeki **Temel katmanı**’nı seçin. Bu Hızlı Başlangıç için, **İşlem Birimleri** kaydırıcısının sol ucuna tıklayarak değeri mümkün olan en düşük değere ayarlayın. Fiyatlandırma katmanını kaydetmek için **Tamam**’a tıklayın. Aşağıdaki ekran görüntüsüne bakın.
    Panoya sabitle | İşaretli | Azure Portal'ın ön pano sayfasında sunucunuzun kolayca izlenmesine izin vermek için **Panoya Sabitle** seçeneğini işaretleyin.

    > [!IMPORTANT]
    > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu Hızlı Başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.
    > 

    ![Azure portalı - gerekli form girişlerini yaparak MySQL oluşturma](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Sunucuyu sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika, en fazla 20 dakika alır.
   
5.  Araç çubuğunda **Bildirimler**’e (zil simgesi) tıklayarak dağıtım işlemini izleyin.

## <a name="configure-a-server-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı oluşturma

MySQL için Azure Veritabanı hizmeti, sunucu düzeyinde bir güvenlik duvarı oluşturur. Bu güvenlik duvarı, belirli IP adresleri için güvenlik duvarını açmak üzere bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ve sunucu üzerindeki herhangi bir veritabanına bağlanmasını engeller. 

1.  Dağıtım tamamlandıktan sonra sunucunuzu bulun. Gerekirse arama yapabilirsiniz. Örneğin, sol taraftaki menüden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için sunucu adını (*myserver4demo* örneğindeki gibi) yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun **Genel bakış** sayfası açılır ve daha fazla yapılandırma seçenekleri sunulur.

2. Sunucu sayfasında **Bağlantı güvenliği**’ni seçin.

3.  **Güvenlik Duvarı kuralları** başlığı altında, güvenlik duvarı kuralı oluşturmaya başlamak için **Kural Adı** sütunundaki boş metin kutusuna tıklayın. 

    Bu Hızlı Başlangıçta, tüm sütunlardaki metin kutularını aşağıdaki değerlerle doldurarak tüm IP adreslerinin sunucuya bağlanmasına izin verelim:

    Kural Adı | Başlangıç IP’si | Bitiş IP’si 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. **Bağlantı güvenliği** sayfasının üst araç çubuğunda **Kaydet**’e tıklayın. Biraz bekleyin ve devam etmeden önce bağlantı güvenliği güncelleştirmesinin başarıyla tamamlandığını gösteren bildirime dikkat edin.

    > [!NOTE]
    > MySQL için Azure Veritabanı bağlantıları 3306 bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 3306 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 3306 numaralı bağlantı noktasını açmadığı sürece sunucunuza bağlanamazsınız.
    > 

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma
Veritabanı sunucusuna bağlanmak için tam sunucu adını ve yönetici oturum açma kimlik bilgilerini hatırlamanız gerekir. Bu değerleri Hızlı Başlangıç makalesinde daha önce not almış olabilirsiniz. Aksi takdirde, Azure portalındaki sunucuya **Genel Bakış** sayfasında veya **Özellikler** sayfasında sunucu adını ve oturum açma bilgilerini kolayca bulabilirsiniz.

1. Sunucunuzun **Genel Bakış** sayfasını açın. **Sunucu adını** ve **Sunucu yöneticisi oturum açma adını** not edin. 
    İmlecinizi her bir alanın üzerine getirin, metnin sağ tarafında Kopyala simgesi görünür. Değerleri kopyalamak için gerektiği şekilde Kopyala simgesine tıklayın.

    Bu örnekte sunucu adı *myserver4demo.mysql.database.azure.com*, sunucu yöneticisi oturum açma bilgileri ise *myadmin@myserver4demo* şeklindedir.

## <a name="connect-to-mysql-using-mysql-command-line-tool"></a>MySQL komut satırı aracını kullanarak sunucuya bağlanma
MySQL sunucusu için Azure veritabanınıza çeşitli uygulamalar kullanarak bağlanabilirsiniz. İlk olarak sunucuya nasıl bağlanılacağını göstermek için [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) komut satırı aracını kullanalım.  Burada açıklandığı şekliyle bir web tarayıcısını ve Azure Cloud Shell’i herhangi bir ek yazılım yüklemeniz gerekmeden kullanabilirsiniz. Kendi makinede yerel olarak yüklü mysql yardımcı programı varsa, oradan da bağlanabilirsiniz.

1. Azure portalı web sayfasının sağ üst tarafındaki terminal simgesi aracılığıyla ( >_ ) Azure Cloud Shell’i başlatın.

2. Azure Cloud Shell, tarayıcınızda açılarak bash kabuk komutları yazmanıza imkan tanır.

    ![Komut istemi - mysql komut satırı örneği](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. Cloud Shell isteminde, yeşil istemde mysql komut satırını yazarak MySQL için Azure Veritabanı sunucunuza bağlanın.

    Aşağıdaki biçim, mysql yardımcı programıyla MySQL için Azure Veritabanı sunucusuna bağlanmak amacıyla kullanılır:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Örneğin, aşağıdaki komut örnek sunucumuza bağlanır:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    mysql parametresi |Önerilen değer|Açıklama
    ---|---|---
    --host | *sunucu adı* | MySQL için Azure Veritabanını oluştururken kullandığınız sunucu adı değerini belirtin. Gösterilen örnek sunucumuz: myserver4demo.mysql.database.azure.com. Örnekte gösterildiği gibi tam etki alanı adını (\*.mysql.database.azure.com) kullanın. Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. 
    --kullanıcı | *sunucu yöneticisi oturum açma adı* |MySQL için Azure Veritabanını oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adını yazın. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin.  Biçim şöyledir: *username@servername*.
    --parola | *istenene kadar bekleyin* | Komutu girdikten sonra "Parolayı girmeniz" istenir. İstendiğinde, sunucuyu oluştururken belirttiğiniz parolayı yazın.  Yazılan parola karakterlerinin yazılırken bash isteminde gösterilmeyeceğini unutmayın. Kimlik doğrulaması yapmak ve bağlanmak için tüm karakterleri yazdıktan sonra enter tuşuna basın.

   Bağlantı kurulduğunda komut yazmanız için mysql yardımcı programı tarafından bir `mysql>` istemi görüntülenir. 

    Örnek mysql çıktısı:
    ```bash
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Güvenlik duvarı Azure Cloud Shell IP adreslerine izin verecek biçimde yapılandırılmamışsa aşağıdaki hata oluşur:
    >
    > HATA 2003 (28000): 123.456.789.0 IP adresli istemcinin sunucuya erişmesine izin verilmiyor.
    >
    > Hatayı gidermek için sunucu yapılandırmasının makalenin *Sunucu düzeyinde güvenlik duvarı kuralı yapılandırma* bölümünde yer alan adımlarla eşleştiğinden emin olun.

4. Bağlantının çalıştığından emin olmak için sunucu durumunu görüntüleyin. Bağlantı kurulduktan sonra mysql> istemine `status` yazın.
    ```sql
    status
    ```

   > [!TIP]
   > Ek komutlar için bkz. [MySQL 5.7 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Aşağıdaki komutu yazarak mysql>istemde boş bir veritabanı oluşturun:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Komutun tamamlanması birkaç dakika sürebilir. 

    MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Sınırsız sayıda veritabanı oluşturabilirsiniz, ancak birden fazla veritabanı aynı sunucu kaynağını paylaşır. 

6. Aşağıdaki komutu yazarak mysql>istemde veritabanlarını listeleyin:

    ```sql
    SHOW DATABASES;
    ```

7.  Mysql aracından çıkmak için `\q` yazıp ENTER tuşuna basın. İşlem tamamlanınca Azure Cloud Shell’i kapatabilirsiniz.

Böylece Azure Veritabanını MySQL’ye bağladınız ve boş bir kullanıcı veritabanı oluşturdunuz. Benzer bir alıştırmayı tekrarlayarak aynı sunucuya farklı bir yaygın araç olan MySQL Workbench ile bağlanmak için bir sonraki bölüme devam edin.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>MySQL Workbench GUI aracını kullanarak sunucuya bağlanma
MySQL Workbench GUI aracını kullanarak Azure MySQL sunucusuna bağlanmak için:

1.  İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2.  **Setup New Connection** (Yeni Bağlantı Oluştur) iletişim kutusundaki **Parameters** (Parametreler) sekmesine aşağıdaki bilgileri girin:

    ![yeni bağlantı oluştur](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Ayar** | **Önerilen değer** | **Alan Açıklaması** |
    |---|---|---|
    |   Bağlantı Adı | Tanıtım Bağlantısı | Bu bağlantı için bir etiket belirtin. |
    | Bağlantı Yöntemi | Standart (TCP/IP) | Standart (TCP/IP) yeterlidir. |
    | Ana Bilgisayar Adı | *sunucu adı* | MySQL için Azure Veritabanını oluştururken kullandığınız sunucu adı değerini belirtin. Gösterilen örnek sunucumuz: myserver4demo.mysql.database.azure.com. Örnekte gösterildiği gibi tam etki alanı adını (\*.mysql.database.azure.com) kullanın. Sunucu adınızı anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin.  |
    | Bağlantı noktası | 3306 | MySQL Azure veritabanına bağlanırken her zaman bağlantı noktası olarak 3306 kullanın. |
    | Kullanıcı adı |  *sunucu yöneticisi oturum açma adı* | MySQL için Azure Veritabanını oluştururken girdiğiniz sunucu yöneticisi oturum açma kullanıcı adını yazın. Bizim örnek kullanıcı adımız myadmin@myserver4demo. Kullanıcı adını anımsamıyorsanız bağlantı bilgilerini almak için bir önceki bölümdeki adımları izleyin. Biçim şöyledir: *username@servername*.
    | Parola | parolanız | Parolayı kaydetmek için Kasada Depola... düğmesine tıklayın. |

    Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın. Bağlantıyı kaydetmek için Tamam’a tıklayın. 

    > [!NOTE]
    > SSL, sunucunuzda varsayılan olarak zorunlu kılınmıştır ve başarıyla bağlanması için ek yapılandırma gerektirir. Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md).  Bu Hızlı Başlangıç için SSL’yi devre dışı bırakmak isterseniz, Azure portalına gidin ve Bağlantı güvenliği sayfasına tıklayarak SSL bağlantısını zorunlu kıl iki durumlu düğmesini devre dışı bırakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silerek ya da diğer kaynakların olduğu gibi kalmasını istiyorsanız ilgili sunucu kaynağını silerek daha önce hızlı başlangıçta oluşturduğunuz kaynakları temizleyin.

> [!TIP]
> Bu koleksiyondaki diğer Hızlı Başlangıçlar, bu Hızlı Başlangıcı temel alır. Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

Yeni oluşturulan sunucu dahil olmak üzere kaynak grubunun tamamını silmek için:
1.  Azure portalında kaynak grubunuzu bulun. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından kaynak grubunuzun adına (örneğimizdeki **myresourcegroup** gibi) tıklayın.
2.  Kaynak grubunuzun sayfasında **Sil**’e tıklayın. Sonra kaynak grubunuzun adını (örneğimizdeki **myresourcegroup** gibi) metin kutusuna yazarak silmeyi onaylayın ve **Sil**’e tıklayın.

Veya bunun yerine, yeni oluşturulan sunucuyu silin:
1.  Sunucunuz açık değilse Azure portalında sunucuyu bulun. Azure portalının sol tarafındaki menüden **Tüm kaynaklar**’a tıklayın ve oluşturduğunuz sunucuyu arayın.
2.  **Genel Bakış** sayfasının üst bölmesindeki **Sil** düğmesine tıklayın.
![MySQL için Azure Veritabanı - Sunucuyu silme](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Silmek istediğiniz sunucu adını onaylayın ve altındaki etkilenen veritabanlarını gösterin. Metin kutusuna sunucu adını (örneğimizdeki **myserver4demo** gibi) yazın ve **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk MySQL için Azure Veritabanınızı tasarlama](./tutorial-design-database-using-portal.md)


