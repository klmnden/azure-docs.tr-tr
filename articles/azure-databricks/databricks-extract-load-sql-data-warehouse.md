---
title: 'Öğretici: Azure Databricks kullanarak ETL işlemleri yapma'
description: Data Lake Store'dan Azure Databricks'e veri ayıklamayı, verileri dönüştürmeyi ve sonra da Azure SQL Veri Ambarı'na yüklemeyi öğrenin.
services: azure-databricks
author: nitinme
ms.author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 07/26/2018
ms.openlocfilehash: cf71eb5e227003f7b9ee0c395d0bc04538e64cfa
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024895"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Öğretici: Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme

Bu öğreticide, Azure Databricks kullanarak ETL (verileri ayıklama, dönüştürme ve yükleme) işlemi yaparsınız. Verileri Azure Data Lake Store'dan Azure Databricks'e ayıklar, Azure Databricks'te veriler üzerinde dönüştürmeler çalıştırır ve ardından dönüştürülmüş verileri Azure SQL Veri Ambarı'na yüklersiniz. 

Bu öğreticideki adımlarda, verileri Azure Databricks'e aktarmak üzere Azure Databricks için SQL Veri Ambarı bağlayıcısı kullanılır. Bu bağlayıcı da, Azure Databricks kümesiyle Azure SQL Veri Ambarı arasında aktarılan veriler için geçici depolama alanı olarak Azure Blob Depolama'yı kullanır.

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Data Lake Store ve SQL Veri Ambarı ile Azure Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Data Lake Store ve SQL Veri Ambarı ile Azure Databricks")

Bu öğretici aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Data Lake Store hesabı oluşturma
> * Azure Data Lake Store'a veri yükleme
> * Azure Databricks’te not defteri oluşturma
> * Data Lake Store'dan verileri ayıklama
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimlerin karşılandığından emin olun:
- Bir Azure SQL Veri Ambarı oluşturun, sunucu düzeyi güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanın. [Hızlı başlangıç: Azure SQL Veri Ambarı oluşturma](../sql-data-warehouse/create-data-warehouse-portal.md) başlığı altındaki yönergeleri izleyin
- Azure SQL Veri Ambarı için veritabanı ana anahtarı oluşturun. [Veritabanı Ana Anahtarı oluşturma](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) başlığı altındaki yönergeleri izleyin.
- Azure Blob depolama hesabı ve bu hesabın içinde bir kapsayıcı oluşturun. Ayrıca, depolama hesabına erişmek için erişim anahtarını alın. [Hızlı başlangıç: Azure Blob depolama hesabı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md) başlığı altındaki yönergeleri izleyin.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz. 

1. Azure portalında **Kaynak oluşturun** > **Veri ve Analiz** > **Azure Databricks** seçeneklerini belirleyin.

    ![Azure portalında Databricks](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks on Azure portal")

3. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/databricks-extract-load-sql-data-warehouse/create-databricks-workspace.png "Create an Azure Databricks workspace")

    Aşağıdaki değerleri sağlayın: 
     
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Doğu ABD 2**’yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

4. Hesabın oluşturulması birkaç dakika sürer. Hesap oluşturma sırasında portal sağ tarafta **Azure Databricks için dağıtım gönderiliyor** kutucuğunu gösterir. Kutucuğu görmek için panonuzu sağa kaydırmanız gerekebilir. Ayrıca, ekranın üst kısmında gösterilen bir ilerleme çubuğu vardır. İlerleme durumu için her iki alanı da izleyebilirsiniz.

    ![Databricks dağıtım kutucuğu](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure’da Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * Bu makale için **4.0** çalışma zamanıyla bir küme oluşturun. 
    * **\_\_ dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.
    
    **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma

Bu bölümde, Azure Data Lake Store hesabı oluşturur ve bir Azure Active Directory hizmet sorumlusunu bu hesapla ilişkilendirirsiniz. Bu öğreticinin devamında, Azure Data Lake Store'a erişmek için Azure Databricks'te bu hizmet sorumlusunu kullanacaksınız. 

1. [Azure portalında](https://portal.azure.com) **Kaynak oluşturun** > **Depolama** > **Data Lake Store**'u seçin.
3. **Yeni Data Lake Store** dikey penceresinde, aşağıdaki ekran görüntüsünde gösterilen değerleri sağlayın:
   
    ![Yeni bir Azure Data Lake Store hesabı oluşturma](./media/databricks-extract-load-sql-data-warehouse/create-new-datalake-store.png "Create a new Azure Data Lake account")

    Aşağıdaki değerleri sağlayın: 
     
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Ad**     | Data Lake Store hesabı için benzersiz bir ad girin.        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Bu öğretici için, Azure Databricks çalışma alanını oluştururken kullandığınız kaynak grubunu kullanın.  |
    |**Konum**     | **Doğu ABD 2**’yi seçin.  |
    |**Fiyatlandırma paketi**     |  **Kullandıkça öde**'yi seçin. |
    | **Şifreleme Ayarları** | Varsayılan ayarları koruyun. |

    **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

Şimdi Azure Active Directory hizmet sorumlusu oluşturur ve bunu daha önce oluşturduğunuz Data Lake Store hesabıyla ilişkilendirirsiniz.

### <a name="create-an-azure-active-directory-service-principal"></a>Azure Active Directory hizmet sorumlusu oluşturma
   
1. [Azure portalında](https://portal.azure.com) **Tüm hizmetler**'i seçin ve ardından **Azure Active Directory** için arama yapın.

2. **Uygulama kayıtları**'nı seçin.

   ![uygulama kayıtlarını seçme](./media/databricks-extract-load-sql-data-warehouse/select-app-registrations.png)

3. **Yeni uygulama kaydı**’nı seçin.

   ![uygulama ekleme](./media/databricks-extract-load-sql-data-warehouse/select-add-app.png)

4. Uygulama için bir ad ve URL sağlayın. Oluşturmak istediğiniz uygulama türü olarak **Web uygulaması / API**'yi seçin. Oturum açma URL'sini belirtin ve ardından **Oluştur**'u seçin.

   ![uygulamayı adlandırma](./media/databricks-extract-load-sql-data-warehouse/create-app.png)

Azure Databricks'ten Data Lake Store hesabına erişmek için, oluşturduğunuz Azure Active Directory hizmet sorumlusunun aşağıdaki değerlerine ihtiyacınız vardır:
- Uygulama Kimliği
- Kimlik doğrulama anahtarı
- Kiracı Kimliği

Aşağıdaki bölümlerde, daha önce oluşturduğunuz Azure Active Directory hizmet sorumlusunun bu değerlerini alırsınız.

### <a name="get-application-id-and-authentication-key-for-the-service-principal"></a>Hizmet sorumlusunun uygulama kimliğini ve kimlik doğrulama anahtarını alma

Programlamayla oturum açılırken, uygulamanızın kimliği ve kimlik doğrulama anahtarı gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Azure Active Directory'deki **Uygulama kayıtları**'nda uygulamanızı seçin.

   ![uygulama seçme](./media/databricks-extract-load-sql-data-warehouse/select-app.png)

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Bazı [örnek uygulamalar](#log-in-as-the-application) istemci kimliği olarak bu değere başvurur.

   ![istemci kimliği](./media/databricks-extract-load-sql-data-warehouse/copy-app-id.png)

3. Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

   ![ayarları seçme](./media/databricks-extract-load-sql-data-warehouse/select-settings.png)

4. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

   ![anahtarları seçme](./media/databricks-extract-load-sql-data-warehouse/select-keys.png)

5. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

   ![anahtarı kaydetme](./media/databricks-extract-load-sql-data-warehouse/save-key.png)

   Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Bu değeri kopyalayın çünkü daha sonra anahtarı alamazsınız. Uygulama olarak uygulama kimliğiyle oturum açarken anahtar değerini sağlamanız gerekir. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![kaydedilen anahtar](./media/databricks-extract-load-sql-data-warehouse/copy-key.png)

### <a name="get-tenant-id"></a>Kiracı kimliğini alma

Programlamayla oturum açılırken, kimlik doğrulama isteğinizle birlikte kiracı kimliğini geçirmeniz gerekir.

1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/databricks-extract-load-sql-data-warehouse/select-active-directory.png)

1. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

   ![Azure AD özelliklerini seçme](./media/databricks-extract-load-sql-data-warehouse/select-ad-properties.png)

1. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

   ![kiracı kimliği](./media/databricks-extract-load-sql-data-warehouse/copy-directory-id.png) 

## <a name="upload-data-to-data-lake-store"></a>Data Lake Store'a veri yükleme

Bu bölümde, örnek veri dosyasını Data Lake Store'a yüklersiniz. Bu filtreyi daha sonra Azure Databricks'te bazı dönüştürmeleri çalıştırmak için kullanacaksınız. Bu öğreticide kullandığınız örnek veriler (**small_radio_json.json**) bu [Github deposunda](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) bulunabilir.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Data Lake Store hesabını seçin.

2. **Genel Bakış** sekmesinde **Veri Gezgini**'ne tıklayın.

    ![Veri Gezgini'ni açma](./media/databricks-extract-load-sql-data-warehouse/open-data-explorer.png "Veri Gezgini'ni açma")

3. Veri Gezgini'nin içinde **Karşıya Yükle**'ye tıklayın.

    ![Karşıya Yükle seçeneği](./media/databricks-extract-load-sql-data-warehouse/upload-to-data-lake-store.png "Karşıya Yükle seçeneği")

4. **Dosyaları karşıya yükle** alanında, örnek veri dosyanızın konumuna göz atın ve ardından **Seçili dosyaları ekle**'yi seçin.

    ![Karşıya Yükle seçeneği](./media/databricks-extract-load-sql-data-warehouse/upload-data.png "Karşıya Yükle seçeneği")

5. Bu öğreticide, veri dosyasını Data Lake Store'un köküne yüklediniz. Dolayısıyla, dosya artık `adl://<YOUR_DATA_LAKE_STORE_ACCOUNT_NAME>.azuredatalakestore.net/small_radio_json.json` konumunda kullanılabilir.

## <a name="associate-service-principal-with-azure-data-lake-store"></a>Hizmet sorumlusunu Azure Data Lake Store ile ilişkilendirme

Bu bölümde, Azure Data Lake Store hesabındaki verileri oluşturduğunuz Azure Active Directory hizmet sorumlusuyla ilişkilendirirsiniz. Bu şekilde Azure Databricks'ten Data Lake Store hesabına erişebildiğinizden emin olursunuz. Bu makaledeki senaryo için Data Lake Store içindeki verileri okuyarak SQL Veri Ambarı'ndaki bir tabloyu dolduracaksınız. [Data Lake Store'da Erişim Denetimine Genel Bakış](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions) içeriğine göre Data Lake Store'daki bir dosyada okuma yetkisine sahip olmak için şu izinlere sahip olmanız gerekir:

- Dosyaya giden klasör yapısındaki tüm klasörlerde **Yürütme** izinleri.
- Dosyanın kendisinde **Okuma** izinleri.

Bu izinleri vermek için aşağıdaki adımları gerçekleştirin.

1. [Azure portalda](https://portal.azure.com) oluşturduğunuz Data Lake Store hesabını ve ardından **Veri Gezgini**'ni seçin.

    ![Veri Gezgini'ni Başlat](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-data-explorer.png "Veri Gezgini'ni Başlat")

2. Bu senaryoda örnek veriler klasör yapısının kökünde olduğundan yalnızca kök klasörde **Yürütme** izinleri atamanız gerekir. Bunun için veri gezgini kök dizininde **Erişim**'i seçin.

    ![Klasöre ACL ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-1.png "Klasöre ACL ekleme")

3. **Erişim** bölümünde **Ekle**'yi seçin.

    ![Klasöre ACL ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-2.png "Klasöre ACL ekleme")

4. **İzin atayın** bölümünde **Kullanıcı veya grup seç**'e tıklayın ve önceden oluşturduğunuz Azure Active Directory hizmet sorumlusunu arayın.

    ![Data Lake Store erişimi ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Data Lake Store erişimi ekleme")

    Atamak istediğiniz AAD hizmet sorumlusunu seçin ve **Seç**'e tıklayın.

5. **İzin atayın** bölümünde **İzinleri seçin** > **Yürütme**'ye tıklayın. Diğer varsayılan değerleri bırakın ve önce **İzinleri seçin**, sonra **İzin atayın** bölümünde **Tamam**'ı seçin.

    ![Data Lake Store erişimi ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-4.png "Data Lake Store erişimi ekleme")

6. Veri Gezgini'ne dönün ve okuma izni atamak istediğiniz dosyaya tıklayın. **Dosya Önizleme** bölümünde **Erişim**'i seçin.

    ![Data Lake Store erişimi ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-1.png "Data Lake Store erişimi ekleme")

7. **Erişim** bölümünde **Ekle**'yi seçin. **İzin atayın** bölümünde **Kullanıcı veya grup seç**'e tıklayın ve önceden oluşturduğunuz Azure Active Directory hizmet sorumlusunu arayın.

    ![Data Lake Store erişimi ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-folder-3.png "Data Lake Store erişimi ekleme")

    Atamak istediğiniz AAD hizmet sorumlusunu seçin ve **Seç**'e tıklayın.

8. **İzin atayın** bölümünde **İzinleri seçin** > **Okuma**'yı seçin. Önce **İzinleri seçin**, sonra **İzin atayın** bölümünde **Tamam**'ı seçin.

    ![Data Lake Store erişimi ekleme](./media/databricks-extract-load-sql-data-warehouse/add-adls-access-file-2.png "Data Lake Store erişimi ekleme")

    Hizmet sorumlusu artık Azure Data Lake Store'daki örnek veri dosyasını okumak için yeterli izinlere sahip.

## <a name="extract-data-from-data-lake-store"></a>Data Lake Store'dan verileri ayıklama

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturur ve ardından verileri Data Lake Store'dan Azure Databricks'e ayıklamak için kod parçacıkları çalıştırırsınız.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks’te not defteri oluşturma](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Create notebook in Databricks")

2. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’u seçin.

3. Aşağıdaki kod parçacığını boş bir kod hücresine ekleyin ve yer tutucu değerleri daha önce Azure Active Directory hizmet sorumlusu için kaydettiğiniz değerlerle değiştirin.

        spark.conf.set("dfs.adls.oauth2.access.token.provider.type", "ClientCredential")
        spark.conf.set("dfs.adls.oauth2.client.id", "<APPLICATION-ID>")
        spark.conf.set("dfs.adls.oauth2.credential", "<AUTHENTICATION-KEY>")
        spark.conf.set("dfs.adls.oauth2.refresh.url", "https://login.microsoftonline.com/<TENANT-ID>/oauth2/token")

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

4. Artık Azure Databricks'te veri çerçevesi olarak Data Lake Store'daki örnek json dosyasını yükleyebilirsiniz. Aşağıdaki kod parçacığını yeni bir kod hücresine geçirin, yer tutucu değeri değiştirin ve ardından **SHIFT + ENTER** tuşlarına basın.

        val df = spark.read.json("adl://<DATA LAKE STORE NAME>.azuredatalakestore.net/small_radio_json.json")

5. Veri çerçevesinin içeriğini görmek için aşağıdaki kod parçacığını çalıştırın.

        df.show()

    Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
        +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
        | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
        | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
        | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
        ...
        ...

Artık verileri Azure Data Lake Store'dan Azure Databricks'e ayıkladınız.

## <a name="transform-data-in-azure-databricks"></a>Azure Databricks'te verileri dönüştürme

Ham örnek veriler (**small_radio_json.json**) radyo istasyonunun hedef kitlesini yakalar ve çeşitli sütunları vardır. Bu bölümde, veri kümesinden yalnızca belirli sütunları içeri almak için verileri dönüştürürsünüz. 

1. Oluşturduğunuz veri çerçevesinden yalnızca *firstName*, *lastName*, *gender*, *location* ve *level* sütunlarını alarak başlayın.

        val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
        specificColumnsDf.show()

    Aşağıdaki kod parçacığında gösterildiği gibi bir çıkış alırsınız:

        +---------+----------+------+--------------------+-----+
        |firstname|  lastname|gender|            location|level|
        +---------+----------+------+--------------------+-----+
        | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |     Tess|  Townsend|     F|Nashville-Davidso...| free|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
        |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Tess|  Townsend|     F|Nashville-Davidso...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |     Liam|     Watts|     M|New York-Newark-J...| paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        |     Alan|     Morse|     M|Chicago-Napervill...| paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
        +---------+----------+------+--------------------+-----+

2.  Bu verilerde başka dönüştürmeler de yaparak **level** sütununun adını **subscription_type** olarak değiştirebilirsiniz.

        val renamedColumnsDf = specificColumnsDf.withColumnRenamed("level", "subscription_type")
        renamedColumnsDf.show()

    Aşağıdaki kod parçacığında gösterildiği gibi bir çıkış alırsınız.

        +---------+----------+------+--------------------+-----------------+
        |firstname|  lastname|gender|            location|subscription_type|
        +---------+----------+------+--------------------+-----------------+
        | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
        |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |     Liam|     Watts|     M|New York-Newark-J...|             paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
        |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
        |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
        +---------+----------+------+--------------------+-----------------+

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

Bu bölümde, dönüştürülen verileri Azure SQL Veri Ambarı'na yüklersiniz. Azure Databricks için Azure SQL Veri Ambarı bağlayıcısını kullanıp veri çerçevesini bir tablo olarak doğrudan SQL veri ambarına yükleyebilirsiniz.

Daha önce de belirtildiği gibi, SQL veri ambarı bağlayıcısı verileri Azure Databricks ile Azure SQL Veri Ambarı arasında aktarmak üzere karşıya yüklemek için geçici depolama alanı konumu olarak Azure Blob Depolama'yı kullanır. Bu nedenle, depolama hesabına bağlanmak için kullanılacak yapılandırmayı sağlayarak başlarsınız. Bu makalenin önkoşullarından biri olarak hesabı önceden oluşturmuş olmalısınız.

1. Azure Databricks'ten Azure Depolama hesabına erişmek için yapılandırmayı sağlayın.

        val blobStorage = "<STORAGE ACCOUNT NAME>.blob.core.windows.net"
        val blobContainer = "<CONTAINER NAME>"
        val blobAccessKey =  "<ACCESS KEY>"

2. Verileri Azure Databricks ile Azure SQL Veri Ambarı arasında taşırken kullanılacak geçici klasörü belirtin.

        val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"

3. Aşağıdaki kod parçacığını çalıştırarak Azure Blob depolama erişim anahtarlarını yapılandırmada depolayın. Bu sayede erişim anahtarını not defterinde düz metin olarak saklamanız gerekmez.

        val acntInfo = "fs.azure.account.key."+ blobStorage
        sc.hadoopConfiguration.set(acntInfo, blobAccessKey)

4. Azure SQL Veri Ambarı örneğine bağlanmak için değerleri sağlayın. Önkoşulların bir parçası olarak SQL veri ambarını oluşturmuş olmalısınız.

        //SQL Data Warehouse related settings
        val dwDatabase = "<DATABASE NAME>"
        val dwServer = "<DATABASE SERVER NAME>" 
        val dwUser = "<USER NAME>"
        val dwPass = "<PASSWORD>"
        val dwJdbcPort =  "1433"
        val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
        val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
        val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass

5. Dönüştürülmüş veri çerçevesi **renamedColumnsDf**'yi SQL veri ambarına tablo olarak yüklemek için aşağıdaki kod parçacığını çalıştırın. Bu kod parçacığı SQL veritabanında **SampleTable** adlı bir tablo oluşturur. Azure SQL DW için ana anahtar gerekir.  SQL Server Management Studio'da "CREATE MASTER KEY;" komutunu yürüterek bir ana anahtar oluşturabilirsiniz.

        spark.conf.set(
          "spark.sql.parquet.writeLegacyFormat",
          "true")
        
        renamedColumnsDf.write
            .format("com.databricks.spark.sqldw")
            .option("url", sqlDwUrlSmall) 
            .option("dbtable", "SampleTable")
            .option( "forward_spark_azure_storage_credentials","True")
            .option("tempdir", tempDir)
            .mode("overwrite")
            .save()

6. SQL veritabanına bağlanın ve **SampleTable** tablosunu gördüğünüzü doğrulayın.

    ![Örnek tabloyu doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Örnek tabloyu doğrulama")

7. Tablonun içindekileri doğrulamak için bir seçme sorgusu çalıştırın. **renamedColumnsDf** veri çerçevesiyle aynı verileri içermelidir.

    ![Örnek tablo içeriğini doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Örnek tablo içeriğini doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi çalıştırdıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi oluştururken **__ dakika etkinsizlik süresinden sonra sonlandır** onay kutusunu seçtiyseniz, kümeyi kendiniz sonlandırmazsanız küme otomatik olarak durdurulur. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Data Lake Store hesabı oluşturma
> * Azure Data Lake Store'a veri yükleme
> * Azure Databricks’te not defteri oluşturma
> * Data Lake Store'dan verileri ayıklama
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure Event Hubs kullanarak Azure Databricks'e gerçek zamanlı veri akışı yapmayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Event Hubs kullanarak Azure Databricks’e veri akışı sağlama](databricks-stream-from-eventhubs.md)
