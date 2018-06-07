---
title: Excel, Python veya R Azure Databricks bağlanma | Microsoft Docs
description: Excel, Python veya r Azure Databricks bağlanmak için Simba sürücü kullanmayı öğrenin
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bbf75a03fb771aa415a26e151614cecfaa14c485
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598887"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Excel, Python veya R Azure Databricks Bağlan

Bu makalede, Microsoft Excel, Python veya R dili ile Azure Databricks bağlanmak için Databricks ODBC sürücüsü kullanmayı öğrenin. Bağlantıyı kurduktan sonra Excel, Python veya R istemcilerden Azure Databricks verilerde erişebilir. İstemcilerin, ek verileri çözümlemek için de kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma alanı, Spark kümesi ve örnek veri kümenizle ilişkilendirilmiş olması gerekir. Bu Önkoşullar zaten yoksa, en hızlı başlangıç tamamlamak [Azure Azure portalını kullanarak Databricks üzerinde bir Spark çalıştırın](quickstart-create-databricks-workspace-portal.md).

* Databricks ODBC sürücüsünü karşıdan [Databricks sürücü indirme sayfası](https://databricks.com/spark/odbc-driver-download). Azure Databricks bağlanmak istediğiniz uygulamaya bağlı olarak 32 bit veya 64-bit sürümünü yükleyin. Örneğin, Excel'den bağlanmak için sürücü 32-bit sürümünü yükleyin. R ve Python bağlanmak için sürücü 64-bit sürümünü yükleyin.

* Kişisel erişim belirteci Databricks olarak ayarlayın. Yönergeler için bkz: [belirteci Yönetim](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="set-up-a-dsn"></a>Bir DSN ayarlama

Bir veri kaynağı adı (DSN) belirli bir veri kaynağına hakkındaki bilgileri içerir. ODBC sürücüsü bir veri kaynağına bağlanmak için bu DSN gerekir. Bu bölümde, Databricks ODBC sürücüsü ile Microsoft Excel, Python veya r gibi istemcilerden Azure Databricks bağlanmak için kullanılan bir DSN ayarlayın

1. Azure Databricks çalışma alanından Databricks kümeye gidin.

    ![Açık Databricks küme](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "açık Databricks küme")

2. Altında **yapılandırma** sekmesini tıklatın, **JDBC/ODBC** sekmesinde ve değerlerini kopyalayın **sunucusu konak adı** ve **HTTP yolu**. Bu makaledeki adımları tamamlamak için bu değerleri gerekir.

    ![Databricks yapılandırmasını almak](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "almak Databricks yapılandırma")

3. Bilgisayarınızda Başlat **ODBC veri kaynakları** uygulamaya bağlı olarak uygulama (32 bit veya 64 bit). Excel'den bağlanmak için 32-bit sürümünü kullanın. R ve Python bağlanmak için 64-bit sürümünü kullanın.

    ![ODBC başlatma](./media/connect-databricks-excel-python-r/launch-odbc-app.png "ODBC uygulama başlatma")

4. Altında **Kullanıcı DSN** sekmesini tıklatın, **Ekle**. İçinde **yeni veri kaynağı oluştur** iletişim kutusunda **Simba Spark ODBC sürücüsü**ve ardından **son**.

    ![ODBC başlatma](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "ODBC uygulama başlatma")

5. İçinde **Simba Spark ODBC sürücüsü** iletişim kutusunda, aşağıdaki değerleri sağlayın:

    ![DSN yapılandırma](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "DSN yapılandırın")

    Aşağıdaki tabloda, iletişim kutusunda sağlamak için değerleri hakkında bilgi sağlar.
    
    |Alan  | Değer  |
    |---------|---------|
    |**Veri kaynağı adı**     | Veri kaynağı için bir ad sağlayın.        |
    |**Konak**     | Databricks çalışma alanından için kopyaladığınız değeri sağlamak *sunucusu konak adı*.        |
    |**Bağlantı Noktası**     | Girin *443*.        |
    |**Kimlik doğrulama** > **mekanizması**     | Seçin *kullanıcı adı ve parola*.        |
    |**Kullanıcı adı**     | Girin *belirteci*.        |
    |**Parola**     | Databricks çalışma alanından kopyalanan belirteç değerini girin. |
    
    DSN Kurulumu iletişim kutusunda aşağıdaki ek adımları gerçekleştirin.
    
    * Tıklatın **HTTP OPTIONS**. Açılan iletişim kutusunda, değeri yapıştırın *HTTP yolu* Databricks çalışma alanından kopyaladığınız. **Tamam**’a tıklayın.
    * Tıklatın **SSL seçeneklerini**. Açılan iletişim kutusunda seçin **SSL'yi etkinleştir** onay kutusu. **Tamam**’a tıklayın.
    * Tıklatın **Test** Azure Databricks test edin. Tıklatın **Tamam** yapılandırmayı kaydetmek için.
    * İçinde **ODBC Veri Kaynağı Yöneticisi** iletişim kutusu, tıklatın **Tamam**.

Artık ayarlamak, DSN var. Sonraki bölümlerde, Excel, Python veya r Azure Databricks bağlanmak için bu DSN kullanın

## <a name="connect-from-microsoft-excel"></a>Microsoft Excel kullanarak bağlan

Bu bölümde, verileri Azure Databricks Microsoft Excel'e daha önce oluşturduğunuz DSN kullanarak çeker. Başlamadan önce bilgisayarınızda Microsoft Excel yüklü olduğundan emin olun. Excel'den deneme sürümünü kullanabilirsiniz [Microsoft Excel deneme bağlantı](https://products.office.com/excel).

1. Boş bir çalışma kitabını Excel'de açın. Gelen **veri** Şerit, tıklatın **Veri Al**. Tıklatın **diğer kaynaklardan** ve ardından **gelen ODBC**.

    ![ODBC Excel'den başlatma](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "Excel ODBC'den başlatma")

2. İçinde **gelen ODBC** iletişim kutusunda, daha önce oluşturduğunuz DSN seçin ve ardından **Tamam**.

    ![DSN seçin](./media/connect-databricks-excel-python-r/excel-select-dsn.png "DSN seçin")

3. Kimlik bilgilerini istenirse, kullanıcı adı **belirteci**. Parola için Databricks çalışma alanı'ndan alınan belirteç değeri sağlayın.

    ![Kimlik bilgilerini sağlamak için Databricks](./media/connect-databricks-excel-python-r/excel-databricks-token.png "DSN seçin")

4. Gezgin penceresinden Excel'e yükleyin ve ardından istediğiniz Databricks içinde tabloyu seçin **yük**. 

    ![Excel'e DTA yük](./media/connect-databricks-excel-python-r/excel-load-data.png "yük dta Excel'e")

Excel çalışma kitabınızı verileri bulduktan sonra analitik işlemlerini gerçekleştirebilirsiniz.

## <a name="connect-from-r"></a>R Bağlan

Bu bölümde, başvuru verileri Azure Databricks kullanılabilir bir R dilini IDE kullanın. Başlamadan önce bilgisayarda yüklü olması gerekir.

* Bir IDE R dil için. Bu makalede Rstudio'dan Masaüstü için kullanır. Şuradan yükleyebilirsiniz [R Studio indirme](https://www.rstudio.com/products/rstudio/download/).
* IDE'yi Masaüstü için Rstudio'dan kullanıyorsanız, Microsoft R istemciden de yüklemeniz [ http://aka.ms/rclient/ ](http://aka.ms/rclient/). 

Rstudio'dan açın ve aşağıdaki adımları uygulayın:

- Başvuru `RODBC` paket. Bu, daha önce oluşturduğunuz DSN kullanarak Azure Databricks bağlanmanızı sağlar.
- DSN kullanarak bağlantı kurun.
- Azure Databricks veriler üzerinde bir SQL sorgusunu çalıştırın. Aşağıdaki kod parçacığında, *radio_sample_data* Azure Databricks zaten bir tablo.
- Bazı çıktı doğrulamak için sorgu işlemleri. 

Aşağıdaki kod parçacığında, şu görevleri gerçekleştirir:

    # reference the 'RODBC' package
    require(RODBC)
    
    # establish a connection using the DSN you created earlier
    conn <- odbcConnect("<ENTER DSN NAME HERE>")
    
    # run a SQL query using the connection you created
    res <- sqlQuery(conn, "SELECT * FROM radio_sample_data")
    
    # print out the column names in the query output
    names(res) 
        
    # print out the number of rows in the query output
    nrow (res)

## <a name="connect-from-python"></a>Python Bağlan

Bu bölümde, başvuru verileri Azure Databricks kullanılabilir (örneğin, boşta) bir Python IDE kullanın. Başlamadan önce aşağıdaki önkoşulları tamamlayın:

* Gelen Python yüklemek [burada](https://www.python.org/downloads/). Bu bağlantıdan Python yükleme boşta yükler.

* Bilgisayarda bir komut isteminden yükleme `pyodbc` paket. Şu komutu çalıştırın:

      pip install pyodbc

Boşta açın ve aşağıdaki adımları uygulayın:

- İçeri aktarma `pyodbc` paket. Bu, daha önce oluşturduğunuz DSN kullanarak Azure Databricks bağlanmanızı sağlar.
- Daha önce oluşturduğunuz DSN kullanarak bağlantı kurun.
-  Oluşturduğunuz bağlantıyı kullanarak bir SQL sorgusunu çalıştırın. Aşağıdaki kod parçacığında, *radio_sample_data* Azure Databricks zaten bir tablo.
- Çıktı doğrulamak için sorgu işlemleri.

Aşağıdaki kod parçacığında, şu görevleri gerçekleştirir:

```python
# import the `pyodbc` package:
import pyodbc

# establish a connection using the DSN you created earlier
conn = pyodbc.connect("DSN=<ENTER DSN NAME HERE>", autocommit = True)

# run a SQL query using the connection you created
cursor = conn.cursor()
cursor.execute("SELECT * FROM radio_sample_data")

# print the rows retrieved by the query.
for row in cursor.fetchall():
    print(row)

```

## <a name="next-steps"></a>Sonraki adımlar

* Burada Azure Databricks veri alabilirsiniz kaynaklardan hakkında bilgi edinmek için [Azure Databricks için veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)


