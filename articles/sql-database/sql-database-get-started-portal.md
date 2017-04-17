---
title: "Azure portalı: SQL veritabanı oluşturma | Microsoft Docs"
description: "Azure portalında SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturmayı öğrenin. Ayrıca Azure portalını kullanarak bir Azure SQL veritabanında sorgu yürütmeyi öğrenirsiniz."
keywords: "sql veritabanı öğreticisi, sql veritabanı oluşturma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 04/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 58af25d90b419b3ddb986118a8c9ba3b42aa95a6
ms.lasthandoff: 04/12/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Azure portalında Azure SQL veritabanı oluşturma

Bu hızlı başlangıç öğreticisinde, Azure’da SQL veritabanı oluşturma adımlar gösterilmektedir. Azure SQL Veritabanı, bulutta yüksek oranda kullanılabilir SQL Server veritabanlarını çalıştırıp ölçeklendirmenize olanak tanıyan bir “Hizmet Olarak Veritabanı” teklifidir. Bu hızlı başlangıç, Azure portalını kullanarak bir SQL veritabanı oluşturma işlemini göstermektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) içinde oluşturulur. 

Adventure Works LT örnek verilerini içeren bir SQL veritabanı oluşturmak için bu adımları izleyin. 

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Veritabanları** penceresinden **SQL Veritabanı**’nı seçin.

    ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:     
   - Veritabanı adı: **mySampleDatabase**
   - Kaynak grubu: **myResourceGroup**
   - Kaynak: **Örnek (AdventureWorksLT)**

4. Yeni veritabanınıza yönelik bir sunucu oluşturup yapılandırmak için **Sunucu**’ya tıklayın. Genel olarak benzersiz bir sunucu adı belirterek **Yeni sunucu formu**’nu doldurun, Sunucu yönetici oturumu için bir ad sağlayın ve ardından seçtiğiniz parolayı belirtin. 

    ![create database-server](./media/sql-database-get-started-portal/create-database-server.png)
5. **Seç**'e tıklayın.

6. Yeni veritabanınıza ait hizmet katmanını ve performans düzeyini belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Bu hızlı başlangıç için **20 DTU** ve **250** GB depolama alanı seçin

    ![create database-s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. **Uygula**'ya tıklayın.  

8. Veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer. 

9. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

    ![bildirim](./media/sql-database-get-started-portal/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, güvenlik duvarını belirli IP adreslerine açmaya yönelik bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını engelleyen sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. SQL Veritabanı güvenlik duvarı üzerinden yalnızca IP adresinize yönelik dış bağlantıları etkinleştirmek üzere istemcinizin IP adresi için bir [SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturmak için bu adımları izleyin. 

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**’na tıklayın ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. Veritabanınız için bir genel bakış sayfası açılır ve tam sunucu adı (örneğin **mynewserver20170327.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur.

      ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Önceki görüntüde gösterildiği gibi araç çubuğundaki **sunucu güvenlik duvarı ayarla** öğesine tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

3. Araç çubuğundaki **İstemci IP’si ekle** öğesine tıklayın ve ardından **Kaydet**’e tıklayın. Geçerli IP adresiniz için bir sunucu düzeyi güvenlik duvarı kuralı oluşturulur.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Artık SQL Server Management Studio’yu veya seçtiğiniz başka bir aracı kullanarak, daha önce oluşturduğunuz Sunucu yönetici hesabıyla bu IP adresinden SQL Veritabanı sunucusuna ve sunucuya ait veritabanlarına bağlanabilirsiniz.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamazsınız.
>

## <a name="query-the-sql-database"></a>SQL veritabanını sorgulama

SQL veritabanımız oluşturulurken **AdventureWorksLT** örnek veritabanı ile doldurulmuştur (bu hızlı başlangıcın başındaki Kullanıcı Arabirimi Oluşturma bölümünde belirlediğimiz seçeneklerden biriydi). Şimdi de verileri sorgulamak için Azure portalındaki yerleşik sorgu aracını kullanalım. 

1. Veritabanınız için SQL Veritabanı sayfasında, araç çubuğundaki **Araçlar**’a tıklayın. **Araçlar** sayfası açılır.

     ![araçlar menüsü](./media/sql-database-get-started-portal/tools-menu.png) 

2. **Sorgu düzenleyicisi (önizleme)**’ye tıklayın, **Önizleme koşulları** onay kutusuna tıklayın ve ardından **Tamam**’a tıklayın. Sorgu düzenleyicisi sayfası açılır.

3. **Oturum aç**’a tıklayın ve istendiğinde **SQL Server kimlik doğrulamasını** seçin ve daha önce oluşturduğunuz sunucu yöneticisi oturum açma bilgilerini ve parolayı girin.

    ![oturum açma](./media/sql-database-get-started-portal/login.png) 

4. Oturum açmak için **Tamam**’a tıklayın.

5. Kimlik doğrulaması yaptıktan sonra sorgu düzenleyici bölmesine aşağıdaki sorguyu yazın.

   ```
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. **Çalıştır**’a tıklayın ve **Sonuçlar** bölmesindeki sorgu sonuçlarını gözden geçirin.

    ![sorgu düzenleyicisi sonuçları](./media/sql-database-get-started-portal/query-editor-results.png)

7. **Sorgu düzenleyicisi** sayfasını ve **Araçlar** sayfasını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıca göre belirlenir. Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, Azure portalda bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myResourceGroup**’a tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio kullanarak bağlanmak ve sorgu yürütmek için, bkz. [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- Visual Studio Code’u kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).

