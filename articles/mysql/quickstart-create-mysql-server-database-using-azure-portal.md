---
title: "Hızlı Başlangıç: MySQL için Azure Veritabanı sunucusu Oluşturma - Azure portalı | Microsoft Docs"
description: "Bu makalede, Azure portalını kullanarak örnek bir MySQL için Azure Veritabanı sunucusunu yaklaşık beş dakika içinde hızlıca oluşturma adımları verilmektedir."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: hero-article
ms.date: 06/14/2017
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dba50b369fb87d5f6d5118038c75392bd719cc10
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---

<a id="create-an-azure-database-for-mysql-server-using-azure-portal" class="xliff"></a>

# Azure portalını kullanarak MySQL için Azure Veritabanı sunucusu oluşturma
Bu makalede, Azure portalını kullanarak bir MySQL için Azure Veritabanı sunucusunu yaklaşık beş dakika içinde oluşturma adımları gösterilmektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

<a id="log-in-to-azure" class="xliff"></a>

## Azure'da oturum açma
Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

<a id="create-azure-database-for-mysql-server" class="xliff"></a>

## MySQL için Azure Veritabanı sunucusu oluşturma
1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Yeni** sayfasından **Veritabanları**’nı seçin ve **Veritabanları** sayfasından **MySQL için Azure Veritabanı**’nı seçin. Yeni sayfa arama kutusuna **MySQL** yazarak da hizmeti bulabilirsiniz.
![Azure portalı - yeni - veritabanı - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Yeni sunucu ayrıntıları formunu, bir önceki resimde gösterildiği gibi aşağıdaki bilgilerle doldurun:

| **Ayar** | **Önerilen değer** | **Alan Açıklaması** |
|---|---|---|
| *Sunucu adı* | myserver4demo  | Sunucu adı genel olarak benzersiz olmalıdır. |
| *Abonelik* | mysubscription | Açılan listeden aboneliğinizi seçin. |
| *Kaynak grubu* | myresourcegroup | Bir kaynak grubu oluşturun veya mevcut olanlardan birini kullanın. |
| *Sunucu yöneticisi oturum açma bilgileri* | myadmin | MySQL altyapısında yönetici olacak bir hesap adı belirtin. |
| *Parola* |  | Güçlü bir yönetici hesabı parolası belirleyin. |
| *Parolayı onayla* |  | Yönetici hesabı parolasını onaylayın. |
| *Konum* |  | Kullanılabilir bir bölge seçin. |
| *Sürüm* | 5.7 | En son sürümü seçin. |
| *Fiyatlandırma Katmanı* | Temel, 50 İşlem Birimi, 50 Depolama (GB)  | **Fiyatlandırma katmanı**, **İşlem Birimleri**, **Depolama (GB)** öğelerini seçip **Tamam**’a tıklayın. |
| *Panoya Sabitle* | İşaretli | Sunucuyu daha sonra kolayca bulabilmeniz için bu kutunun işaretlenmesi önerilir |

   Yeni veritabanınıza ait fiyatlandırma katmanını ve performans düzeyini belirtmek için **Fiyatlandırma Katmanı**’na tıklayın. Bu hızlı başlangıç için Temel Katman’ı, 50 İşlem Birimi’ni ve 50 GB dahili depolamayı seçin. Ardından **Tamam**’a tıklayarak fiyatlandırma katmanını kaydedin.
   
   ![Azure portalı - gerekli form girişlerini yaparak MySQL oluşturma](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

   Sonra, **Oluştur**’a tıklayın. Bir veya iki dakika içinde, bulutta MySQL sunucusu için yeni bir Azure Veritabanı çalışmaya başlayacaktır. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** (zil simgesi) düğmesine tıklayın.

<a id="configure-the-firewall" class="xliff"></a>

## Güvenlik duvarını yapılandırma
MySQL için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını yapılandırın ve istemcinin genel ağ IP adresini (veya aralığını) güvenilir listeye ekleyin.

1. Dağıtım tamamlandıktan sonra, sol taraftaki menüden **Tüm Kaynaklar**’a tıklayın ve yeni oluşturduğunuz sunucuyu aramak için **myserver4demo** adını yazın. Arama sonucunda listelenen sunucu adına tıklayın. Sunucunuzun Genel bakış sayfası açılır ve daha fazla yapılandırma seçeneği sunulur.

2. Sunucu dikey penceresinde, **Bağlantı Güvenliği**’ni seçin.

3. **IP'mi Ekle**’ye tıklayarak yerel bilgisayarınızın IP adresini ekleyin veya bir IP aralığı yapılandırın. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
  ![Azure portalı - güvenlik duvarı kuralı ekleme ve kaydetme](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma
Azure portalında Azure MySQL sunucunuzun tam etki alanı adını alın. **mysql.exe** komut satırı aracını kullanarak sunucunuza bağlanmak için tam etki alanı adını kullanırsınız.

1.  [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a ve MySQL için Azure Veritabanı sunucusuna tıklayın.

2.  **Özellikler**'e tıklayın. **SUNUCU ADI** ve **SUNUCU YÖNETİCİSİ OTURUM AÇMA BİLGİLERİ** değerlerini not edin.
Bu örnekte sunucu adı *myserver4demo.mysql.database.azure.com*, sunucu yöneticisi oturum açma bilgileri ise *myadmin@myserver4demo* şeklindedir.

<a id="connect-to-the-server-using-mysqlexe-command-line-tool" class="xliff"></a>

## mysqlexe komut satırı aracını kullanarak sunucuya bağlanma
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kullanın. Azure Cloud Shell’i kullanarak mysql komut satırı aracını tarayıcıdan çalıştırabilir veya yerel olarak yüklenmiş mysql araçlarını kullanarak kendi makinenizden başlatabilirsiniz. Azure Cloud Shell’i başlatmak için bu makaledeki bir kod bloğu üzerinde `Try It` düğmesine tıklayın veya [Azure portalını](https://portal.azure.com) ziyaret ederek sağ üstteki araç çubuğunda `>_` simgesine tıklayın. 

1. Bağlanma komutunu yazın:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Bağlantının çalıştığından emin olmak için sunucu durumunu görüntüleyin. Bağlantı kurulduktan sonra mysql> istemine `status` yazın.
```sql
status
```

   ![Komut istemi - mysql komut satırı örneği](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

   > [!TIP]
   > Ek komutlar için bkz. [MySQL 5.7 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

3. mysql> istemine `CREATE DATABASE` yazarak boş bir veritabanı oluşturun.

   ```sql
   CREATE DATABASE quickstartdb;
   ```

   MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanınız olabilir. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Sınırsız sayıda veritabanı oluşturabilirsiniz, ancak birden fazla veritabanı aynı sunucu kaynağını paylaşır.  

4. mysql> istemine `SHOW DATABASES` yazarak veritabanlarını listeleyin.

   ```sql
   SHOW DATABASES;
   ```

<a id="connect-to-the-server-using-the-mysql-workbench-gui-tool" class="xliff"></a>

## MySQL Workbench GUI aracını kullanarak sunucuya bağlanma
1.  İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2.  **Setup New Connection** (Yeni Bağlantı Oluştur) iletişim kutusundaki **Parameters** (Parametreler) sekmesine aşağıdaki bilgileri girin:

   ![yeni bağlantı oluştur](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

| **Ayar** | **Önerilen değer** | **Alan Açıklaması** |
|---|---|---|
|   *Bağlantı Adı* | Tanıtım Bağlantısı| Bu bağlantı için bir etiket belirtin. |
| *Bağlantı Yöntemi* | Standart (TCP/IP) | Standart (TCP/IP) yeterlidir. |
| *Ana Bilgisayar Adı* | myserver4demo.mysql.database.azure.com | Sunucunuzun tam sunucu adını kullanın. |
| *Bağlantı Noktası* | 3306 | Varsayılan 3306 bağlantı noktasını kullanın. |
| *Kullanıcı Adı* | myadmin@myserver4demo  | Daha önce not ettiğiniz sunucu yöneticisi oturum açma bilgilerini @ karakteri ve sunucu adı ile birlikte kullanın. |
| *Parola* | parolanız | Parolayı kaydetmek için Kasada Depola... düğmesine tıklayın. |

Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın. Bağlantıyı kaydetmek için Tamam’a tıklayın. 

> [!NOTE]
> SSL, sunucunuzda varsayılan olarak zorunlu kılınmıştır ve başarıyla bağlanması için ek yapılandırma gerektirir. Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md).  Bu hızlı başlangıç için SSL’yi devre dışı bırakmak isterseniz, Azure portalına gidin ve Bağlantı güvenliği sayfasına tıklayarak SSL bağlantısını zorunlu kıl iki durumlu düğmesini devre dışı bırakın.

<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme
[Azure kaynak grubunu](../azure-resource-manager/resource-group-overview.md) silerek hızlı başlangıçta oluşturduğunuz tüm kaynakları temizleyin.

> [!TIP]
> Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1.  Azure portalında sol taraftaki menüden, **Kaynak grupları**’na ve ardından **myresourcegroup**’a tıklayın.
2.  Kaynak grubu sayfanızda **Sil**’e tıklayın, metin kutusuna **myresourcegroup** yazıp Sil’e tıklayın.

Yeni oluşturulan sunucuyu silmek isterseniz:
1.  Azure portalında sol taraftaki menüden PostgreSQL sunucuları’na tıklayın ve kısa süre önce oluşturduğunuz sunucuyu aratın
2.  Genel bakış sayfasında ![MySQL için Azure Veritabanı - Sunucuyu silme](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png) üst bölmesinde Sil düğmesine tıklayın
3.  Silmek istediğiniz sunucu adını onaylayın ve altındaki etkilenen veritabanlarını gösterin. Metin kutusuna **myserver4demo** yazıp Sil’e tıklayın.


<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk MySQL için Azure Veritabanınızı tasarlama](./tutorial-design-database-using-portal.md)


