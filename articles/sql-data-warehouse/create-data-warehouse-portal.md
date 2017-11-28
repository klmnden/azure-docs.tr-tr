---
title: "Azure portalı bir Azure SQL data warehouse - oluşturma | Microsoft Docs"
description: "Azure SQL Data Warehouse için Azure portalında bir SQL server, sunucu düzeyinde güvenlik duvarı kuralı ve bir veri ambarı oluşturun. Ardından sorgu."
keywords: "SQL veri ambarı öğretici, bir SQL data warehouse oluşturma"
services: sql-database
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: Active
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: barbkess
ms.openlocfilehash: 3a3d077aeb705a996ea82fe7b5e390112712182b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="create-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Azure portalında bir Azure SQL data warehouse oluşturma

Bu hızlı başlangıç Öğreticisi Azure SQL Data Warehouse hizmetini kullanarak bir veri ambarı oluşturur ve AdventureWorksDW örnek verilerle başlatır. Ardından, veri ambarı'na bağlanmak ve verileri bir sorgu çalıştırın. Öğretici kullanır [Azure portal](https://portal.azure.com) ve [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bu hızlı başlangıç başlamadan önce indirin ve en yeni sürümünü yüklemek [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-data-warehouse"></a>Veri ambarı oluşturma

Bir Azure SQL veri ambarı tanımlanan bir dizi ile oluşturulan [işlem kaynaklarını](performance-tiers.md). Veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL mantıksal sunucusuna](../sql-database/sql-database-features.md). 

AdventureWorksDW örnek verileri içeren bir SQL data warehouse oluşturmak için aşağıdaki adımları izleyin. 

1. Tıklatın **yeni** Azure portalının sol üst köşesindeki düğmesi.

2. Seçin **veritabanları** gelen **yeni** sayfasında ve seçin **SQL Data Warehouse** altında **öne çıkan** üzerinde **yeni**sayfası.

    ![boş data warehouse oluşturma](media/load-data-from-azure-blob-storage-using-polybase/create-empty-data-warehouse.png)

3. Aşağıdaki bilgiler ile SQL Data Warehouse formu doldurun:   

    | Ayar | Önerilen değer | Açıklama | 
    | ------- | --------------- | ----------- | 
    | **Veritabanı adı** | mySampleDataWarehouse | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](/sql/relational-databases/databases/database-identifiers). Not: bir veri ambarı veritabanı türüdür.| 
    | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
    | **Kaynak seçin** | Örnek | Örnek veritabanını yüklemek için belirtir. Not, bir veri ambarı veritabanı türüdür. |
    | **Örnek Seç** | AdventureWorksDW | AdventureWorksDW örnek veritabanını yüklemek için belirtir.  |

    ![Data warehouse oluşturma](media/create-data-warehouse-portal/select-sample.png)

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. Doldurmak **yeni sunucu form** aşağıdaki bilgilerle: 

    | Ayar | Önerilen değer | Açıklama | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
    | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
    | **Parola** | Geçerli bir parola | Parolanız en az sekiz karakter olmalıdır ve aşağıdaki kategoriden üçünden karakterler içermelidir: büyük harf karakterler, küçük harfler, sayılar ve alfasayısal olmayan karakter. |
    | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

    ![Veritabanı sunucusu oluşturma](media/load-data-from-azure-blob-storage-using-polybase/create-database-server.png)

5. **Seç**'e tıklayın.

6. Tıklatın **performans katmanı** veri ambarı esneklik veya işlem için optimize edilmiştir ve sayısı, veri ambarı birimlerini olup olmadığını belirtmek için. 

7. Bu öğretici için seçin **esneklik için en iyi duruma getirilmiş** hizmet katmanı. Varsayılan olarak, kaydırıcıyı kümesine **DW400**.  Yukarı ve aşağı nasıl çalıştığını görmek için taşımayı deneyin. 

    ![Performans yapılandırın](media/load-data-from-azure-blob-storage-using-polybase/configure-performance.png)

8. **Uygula**'ya tıklayın.

9. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

    ![Oluştur'u tıklatın](media/load-data-from-azure-blob-storage-using-polybase/click-create.png)

10. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](media/load-data-from-azure-blob-storage-using-polybase/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL veri ambarı hizmeti, sunucu düzeyinde-dış uygulamaları ve araçları sunucu veya sunucu üzerindeki herhangi bir veritabanına bağlanma engelleyen bir güvenlik duvarı oluşturur. Bağlantıyı etkinleştirmek için belirli IP adresleri için bağlantıyı etkinleştirmek güvenlik duvarı kuralı ekleyebilirsiniz.  Oluşturmak için bu adımları bir [sunucu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) , istemcinin IP adresi için. 

> [!NOTE]
> SQL veri ambarı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Şirket ağı içinde bağlanması çalışıyorsanız, bağlantı noktası 1433 üzerinden giden trafik, ağınızın güvenlik duvarı tarafından izin verilmeyebilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınız için genel bakış sayfası açılır ve tam sunucu adını gösteren (gibi **mynewserver 20171113.database.windows.net**) ve diğer yapılandırmalar için seçenekler sağlar. 

2. Sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanmak için bu tam sunucu adını kopyalayın. Ardından sunucu ayarları'nı açmak için sunucu adına tıklayın.

   ![sunucu adını bulma](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png) 

3. Sunucu Ayarları'nı açmak için sunucu adına tıklayın.

   ![Sunucu ayarları](media/load-data-from-azure-blob-storage-using-polybase/server-settings.png) 

5. Tıklatın **güvenlik duvarı ayarlarını göster**. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

   ![sunucu güvenlik duvarı kuralı](media/load-data-from-azure-blob-storage-using-polybase/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet** düğmesine tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Şimdi, SQL server ve bu IP adresini kullanarak kendi veri ambarlarında bağlanabilirsiniz. Bağlantı, SQL Server Management Studio veya tercih ettiğiniz başka bir aracı çalışır. Bağlandığınızda, daha önce oluşturduğunuz ServerAdmin hesabı kullanın.  

> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tıklatın **OFF** bu sayfa ve ardından **kaydetmek** tüm Azure Hizmetleri için Güvenlik Duvarı'nı devre dışı bırakmak için.

## <a name="get-the-fully-qualified-server-name"></a>Tam sunucu adını Al

Tam sunucu adını, SQL server için Azure portalında alın. Daha sonra tam adı sunucuya bağlanırken kullanacaksınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın. Bu örnekte, tam mynewserver 20171113.database.windows.net addır. 

    ![bağlantı bilgileri](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)  

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu Yöneticisi olarak sunucuya bağlanın

Bu bölümde kullanan [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) Azure SQL sunucunuza bir bağlantı kurmak için (SSMS).

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | Adı, şunun gibi olmalıdır: **mynewserver 20171113.database.windows.net**. |
   | Kimlik Doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırdığımız tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Bu, sunucuyu oluştururken belirttiğiniz hesaptır. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Bu, sunucuyu oluştururken belirttiğiniz paroladır. |

    ![sunucuya bağlan](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

5. Nesne Gezgini'nde genişletin **veritabanları**. Ardından **mySampleDatabase** , yeni veritabanı nesneleri görüntülemek için.

    ![Veritabanı nesneleri](media/create-data-warehouse-portal/connected.png) 

## <a name="run-some-queries"></a>Bazı sorgular çalıştırın

SQL veri ambarı T-SQL sorgu dili olarak kullanır. Bir sorgu penceresi açın ve bazı T-SQL sorgularını çalıştırmak için aşağıdaki adımları kullanın.

1. Sağ **mySampleDataWarehouse** seçip **yeni sorgu**.  Yeni bir sorgu penceresi açılır.
2. Sorgu penceresinde, veritabanlarının listesini görmek için aşağıdaki komutu girin.

    ```sql
    SELECT * FROM sys.databases
    ```

3. **Yürüt**’e tıklayın.  İki veritabanı sorgusu sonuçlarını gösterir: **ana** ve **mySampleDataWarehouse**.

    ![Sorgu veritabanları](media/create-data-warehouse-portal/query-databases.png)

4. Bazı veri aramak için üç alt evde sahip Adams, son adı ile müşteriler sayısını görmek için aşağıdaki komutu kullanın. Sonuçlar altı müşteriler listeler. 

    ```sql
    SELECT LastName, FirstName FROM dbo.dimCustomer
    WHERE LastName = 'Adams' AND NumberChildrenAtHome = 3;
    ```

    ![Sorgu dbo.dimCustomer](media/create-data-warehouse-portal/query-customer.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşlem kaynakları ve veri ambarına yüklenen veriler için ücretlendirilirsiniz. Bunlar ayrı ayrı faturalandırılır. 

- Veri deposunda tutmak istiyorsanız, veri ambarı kullanmadığınızda işlem duraklatabilirsiniz. Göre işlem duraklatma ücret veri depolama için yalnızca olacaktır ve verilerle çalışmak hazır olduğunda işlem devam edebilirsiniz.
- Gelecekteki ücretleri kaldırmak istiyorsanız, veri ambarı silebilirsiniz. 

İstediğiniz kaynakları temizlemek için aşağıdaki adımları izleyin.

1. Oturum açma [Azure portal](https://portal.azure.com), veri Ambarınızı'ı tıklatın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlem duraklatmak için tıklatın **duraklatma** düğmesi. Veri ambarı duraklatıldığında göreceğiniz bir **Başlat** düğmesi.  İşlem devam etmek için tıklatın **Başlat**.

2. Veri ambarı, işlem ya da depolama için sizden ücret olmaz şekilde kaldırmak için tıklatın **silmek**.

3. Oluşturduğunuz SQL server kaldırmak için tıklatın **mynewserver 20171113.database.windows.net** önceki görüntü ve ardından **silmek**.  Bu dikkatli olun sunucuyu silmek sunucuya atanmış tüm veritabanlarını silecek.

4. Kaynak grubu kaldırmak için tıklatın **myResourceGroup**ve ardından **kaynak grubu Sil**.


## <a name="next-steps"></a>Sonraki adımlar

Artık bir veritabanınız olduğuna göre, sık kullandığınız araçlarla bağlanabilir ve sorgulayabilirsiniz. Aşağıdan aracınızı seçerek daha fazla bilgi edinin:

- [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Visual Studio Code](../sql-database/sql-database-connect-query-vscode.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [.NET](../sql-database/sql-database-connect-query-dotnet.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [PHP](../sql-database/sql-database-connect-query-php.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Node.js](../sql-database/sql-database-connect-query-nodejs.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Java](../sql-database/sql-database-connect-query-java.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Python](../sql-database/sql-database-connect-query-python.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Ruby](../sql-database/sql-database-connect-query-ruby.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)