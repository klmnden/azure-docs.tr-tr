---
title: "Hızlı Başlangıç: MySQL sunucusu için Azure Veritabanı Oluşturma - Azure portalı | Microsoft Docs"
description: "Bu makalede, Azure portalını kullanarak MySQL sunucusu için örnek bir Azure Veritabanını yaklaşık beş dakika içinde hızlıca oluşturma adımları verilmektedir."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 25bfd2c6c25ddb8747dec58fdc68f904f81127fa
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Azure portalını kullanarak MySQL sunucusu için Azure Veritabanı oluşturma

Bu makalede, Azure portalını kullanarak MySQL sunucusu için örnek bir Azure Veritabanını yaklaşık beş dakika içinde oluşturma adımları gösterilmektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

![Azure portalı - oturum açma ve pano](./media/quickstart-create-mysql-server-database-using-azure-portal/1_portal-login.png)

## <a name="create-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı oluşturma

1. **Veritabanları** > **MySQL**’e gidin. **Veritabanları** kategorisi altında MySQL Sunucusu için Azure Veritabanını bulamazsanız, kullanılabilir tüm veritabanı hizmetlerini görüntülemek için **Tümünü gör**’ü seçin. Ayrıca arama kutusuna **MySQL** yazarak hizmeti hızlıca bulabilirsiniz.
![Azure portalı - yeni - veritabanı - MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

2. **MySQL** simgesine ve ardından **Oluştur**’a tıklayın.
Bu örnekte MySQL için Azure Veritabanı sayfasını aşağıdaki bilgilerle doldurun:

| **Form Alanı** | **Alan Açıklaması** |
|----------------|-----------------------|
| *Sunucu adı* | mysqlserver4demo (sunucu adı genel olarak benzersiz) |
| *Abonelik* | MySQLaaS (açılır listeden seçin) |
| *Kaynak grubu* | myresource (bir kaynak grubu oluşturun veya var olanlardan birini kullanın) |
| *Sunucu yöneticisi oturum açma bilgileri* | myadmin (yönetici hesabı adını ayarlayın) |
| *Parola* | yönetici hesabı parolasını ayarlayın |
| *Parolayı onayla* | yönetici hesabı parolasını onaylayın |
| *Konum* | Kuzey Avrupa (**Kuzey Avrupa** ile **Batı ABD** arasından seçim yapın) |
| *Sürüm* | 5.6 (MySQL sunucusu sürümünü seçin) |
| *Performansı yapılandır* | Temel (**Performans katmanı**, **İşlem Birimleri**, **Depolama**’yı seçin ve ardından **Tamam**’a tıklayın) |

![Azure portalı - gerekli form girişlerini yaparak MySQL oluşturma](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

Birkaç dakika sonra MySQL sunucusu için Azure Veritabanı sağlanır ve çalışır duruma gelir. Dağıtım işlemini izlemek için izleyicideki araç çubuğunda **Bildirimler** düğmesine (zil simgesi) tıklayabilirsiniz.

> [!TIP]
> Azure hizmetlerini aynı bölgeye yerleştirmeniz ve size en yakın konumu seçmeniz önerilir. Ayrıca, dağıtımlarınızın kolayca izlenmesini sağlamak için **Panoya sabitle** seçeneğini işaretleyin.

## <a name="configure-the-firewall"></a>Güvenlik duvarını yapılandırma
İstemcinizden MySQL için Azure Veritabanı’na ilk kez bağlanmadan önce, güvenlik duvarını yapılandırmanız ve istemcinin genel ağ IP adresini (veya IP adresi aralığını) güvenilir listeye eklemeniz gerekir.

1. Yeni oluşturduğunuz sunucuya ve ardından **Ayarlar**’a tıklayın.
  ![Azure portalı - MySQL - Ayarlar düğmesi](./media/quickstart-create-mysql-server-database-using-azure-portal/4_server-settings.png)

2. **GENEL** bölümünün altında **Güvenlik duvarı ayarları**’na tıklayın. **IP Adresimi Ekle**’ye tıklayarak yerel bilgisayarınızın IP adresini ekleyebilir veya bir IP aralığı yapılandırabilirsiniz. Kuralları oluşturduktan sonra **Kaydet**’e tıklamayı unutmayın.
  ![Azure portalı - güvenlik duvarı kuralı ekleme ve kaydetme](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
Azure portalında Azure MySQL sunucunuzun tam etki alanı adını alın. **mysql.exe** komut satırı aracını kullanarak sunucunuza bağlanmak için tam etki alanı adını kullanırsınız.

1.    [Azure portalı](https://portal.azure.com/)’nda soldaki menüden **Tüm kaynaklar**’a ve MySQL sunucusu için Azure Veritabanı’na tıklayın.

2.    **Özellikler**'e tıklayın. **SERVER NAME** ve **SERVER ADMIN LOGIN** değerlerini not edin.
Bu örnekte sunucu adı *mysql4doc.database.windows.net*, sunucu yöneticisi oturum açma bilgileri ise *mysqladmin@mysql4doc* şeklindedir.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>mysqlexe komut satırı aracını kullanarak sunucuya bağlanma
Bir MySQL sunucusu içinde birden fazla veritabanı oluşturabilirsiniz. Sınırsız sayıda veritabanı oluşturabilirsiniz, ancak birden fazla veritabanı aynı sunucu kaynağını paylaşır.  **mysql.exe** komut satırı aracını kullanarak sunucunuza bağlanmak için, portalda **Azure Cloud Shell**’i açıp aşağıdaki bilgileri girin:

1. **mysql** komut satırı aracını kullanarak sunucuya bağlanın:
```dos
 mysql -h mysqlserver4demo.database.windows.net -u myadmin@mysqlserver4demo -p
```

2. Sunucu durumunu görüntüleyin:
```dos
 mysql> status
```
  ![Komut istemi - mysql komut satırı örneği](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

> [!TIP]
> Ek komutlar için bkz. [MySQL 5.6 Başvuru Kılavuzu - Bölüm 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>MySQL Workbench GUI aracını kullanarak sunucuya bağlanma
1.    İstemci bilgisayarınızda MySQL Workbench uygulamasını başlatın. MySQL Workbench uygulamasını [buradan](https://dev.mysql.com/downloads/workbench/) indirip yükleyebilirsiniz.

2.    **Yeni Bağlantı Oluştur** iletişim kutusundaki **Parametreler** sekmesine aşağıdaki bilgileri girin:

| **Parametreler** | **Açıklama** |
|----------------|-----------------|
|    *Bağlantı Adı* | Bu bağlantı için bir ad belirtin (herhangi bir ad olabilir) |
| *Bağlantı Yöntemi* | Standart (TCP/IP) seçeneğini belirleyin |
| *Ana Bilgisayar Adı* | mycliserver.database.windows.net (daha önce not aldığınız SERVER NAME değeri) |
| *Bağlantı Noktası* | 3306 |
| *Kullanıcı Adı* | myadmin@mycliserver (daha önce not aldığınız SERVER ADMIN LOGIN değeri) |
| *Parola* | Yönetici hesabı parolasını kasada depolayabilirsiniz |

![yeni bağlantı oluştur](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

3.    Tüm parametrelerin doğru yapılandırılıp yapılandırılmadığını test etmek için **Bağlantıyı Sına**’ya tıklayın.

4.    Şimdi, yeni oluşturduğunuz bağlantıya tıklayarak sunucuya başarıyla bağlanabilirsiniz.

> SSL, başarıyla bağlanması için ek yapılandırma gerektiren sunucunuzda varsayılan olarak uygulanır. Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md).  Bu hızlı başlangıçta SSL’yi devre dışı bırakmak istiyorsanız, SSL uygulamasını devre dışı bırakmak için portalda “Bağlantı güvenliği” bölümüne gidebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir hızlı başlangıç kılavuzu/öğretici için gereksinim duymuyorsanız, aşağıdakileri yaparak bu kaynakları silebilirsiniz:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myresource**’a tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myresource** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MySQL veritabanı için ilk Azure Veritabanınızı tasarlama](./tutorial-design-database-using-portal.md)


