---
title: 'Azure Databricks için Excel, Python veya R bağlanma '
description: Azure Databricks, Excel, Python veya r bağlanmak için Simba sürücü kullanmayı öğrenin
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: mamccrea
ms.openlocfilehash: c57550a8b683ad8f184884374c4f09216417fc40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60236271"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Azure Databricks için Excel, Python veya R bağlanma

Bu makalede, Microsoft Excel, Python veya R dili ile Azure Databricks bağlanmak için Databricks ODBC sürücüsü kullanmayı öğrenin. Bağlantıyı oluşturduktan sonra Azure databricks'te verileri Excel, Python veya R istemcilerden erişebilirsiniz. İstemciler, verilerinde daha fazla analiz için de kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma alanı, bir Spark kümesi ve bir örnek veri kümenizle ilişkili olmalıdır. Bu Önkoşullar zaten yoksa, en hızlı başlangıcı tamamlamak [Azure portalını kullanarak Azure Databricks'te Spark işini çalıştırma](quickstart-create-databricks-workspace-portal.md).

* Databricks ODBC sürücüsünü indir [Databricks sürücüsü indirme sayfası](https://databricks.com/spark/odbc-driver-download). Azure Databricks için bağlanmak istediğiniz uygulamaya bağlı olarak 32 bit veya 64-bit sürümü yükleyin. Örneğin, Excel'den bağlanmak için sürücünün 32-bit sürümünü yükleyin. R ve Python bağlanmak için sürücünün 64-bit sürümünü yükleyin.

* Databricks içinde bir kişisel erişim belirteci ayarlayın. Yönergeler için [belirteç Yönetim](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="set-up-a-dsn"></a>Bir DSN ayarlayın

Bir veri kaynağı adı (DSN) belirli bir veri kaynağına hakkındaki bilgileri içerir. ODBC sürücüsü, bir veri kaynağına bağlanmak için bu DSN gerekir. Bu bölümde, Databricks ODBC sürücüsü ile Azure Databricks, Microsoft Excel, Python veya r gibi istemcilerden bağlanmak için kullanılan bir DSN'ye ayarlayın

1. Azure Databricks çalışma alanından, Databricks kümesine gidin.

    ![Açık Databricks kümesine](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "açık Databricks kümesine")

2. Altında **yapılandırma** sekmesinde **JDBC/ODBC** sekmesini ve değerlerini kopyalayın **sunucusu konak adı** ve **HTTP yolu**. Bu makaledeki adımları tamamlayabilmeniz için bu değerlere ihtiyacınız olur.

    ![Databricks yapılandırmasını alma](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "alma Databricks yapılandırma")

3. Bilgisayarınızda Başlat **ODBC veri kaynakları** uygulamaya bağlı olarak uygulama (32 bit veya 64-bit). Excel'den bağlanmak için 32 bit sürümü kullanın. R ve Python bağlanmak için 64-bit sürümü kullanın.

    ![ODBC başlatma](./media/connect-databricks-excel-python-r/launch-odbc-app.png "ODBC uygulama başlatma")

4. Altında **Kullanıcı DSN** sekmesinde **Ekle**. İçinde **yeni veri kaynağı oluştur** iletişim kutusunda **Simba Spark ODBC sürücüsü**ve ardından **son**.

    ![ODBC başlatma](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "ODBC uygulama başlatma")

5. İçinde **Simba Spark ODBC sürücüsü** iletişim kutusunda, aşağıdaki değerleri sağlayın:

    ![DSN yapılandırma](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "DSN yapılandırın")

    Aşağıdaki tabloda, iletişim kutusunda sağlanacak değerlerin hakkında bilgiler sağlar.
    
    |Alan  | Değer  |
    |---------|---------|
    |**Veri kaynağı adı**     | Veri kaynağı için bir ad sağlayın.        |
    |**Konaklarında**     | Databricks çalışma için kopyaladığınız değeri sağlamak *sunucusu konak adı*.        |
    |**Bağlantı Noktası**     | Girin *443*.        |
    |**Kimlik doğrulaması** > **mekanizması**     | Seçin *kullanıcı adı ve parola*.        |
    |**Kullanıcı adı**     | Girin *belirteci*.        |
    |**Parola**     | Databricks çalışma alanından kopyalanan belirteç değeri girin. |
    
    DSN Kurulum iletişim kutusunda aşağıdaki ek adımları gerçekleştirin.
    
    * Tıklayın **HTTP OPTIONS**. Açılan iletişim kutusunda, değeri yapıştırın *HTTP yolu* , Databricks çalışma alanından kopyalar. **Tamam** düğmesine tıklayın.
    * Tıklayın **SSL seçeneklerini**. Açılan iletişim kutusunda seçin **SSL'yi etkinleştir** onay kutusu. **Tamam** düğmesine tıklayın.
    * Tıklayın **Test** Azure Databricks bağlantısını test etmek için. Tıklayın **Tamam** yapılandırmayı kaydetmek için.
    * İçinde **ODBC Veri Kaynağı Yöneticisi** iletişim kutusu, tıklayın **Tamam**.

Şimdi ayarlayın, DSN sahipsiniz. Sonraki bölümde, Azure Databricks, Excel, Python veya r bağlanmak için bu DSN kullanın

## <a name="connect-from-microsoft-excel"></a>Microsoft Excel kullanarak bağlanma

Bu bölümde, verileri Azure Databricks'ten Microsoft Excel'e kullanarak daha önce oluşturduğunuz DSN çekin. Başlamadan önce Microsoft Excel yüklü olduğundan emin olun. Excel'den deneme sürümünü kullanabilir [Microsoft Excel deneme bağlantı](https://products.office.com/excel).

1. Boş bir çalışma kitabını Excel'de açın. Gelen **veri** Şerit ye **Veri Al**. Tıklayın **diğer kaynaklardan** ve ardından **gelen ODBC**.

    ![Excel'den ODBC başlatma](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "Excel'den ODBC başlatma")

2. İçinde **gelen ODBC** iletişim kutusunda, daha önce oluşturduğunuz DSN seçin ve ardından **Tamam**.

    ![DSN seçin](./media/connect-databricks-excel-python-r/excel-select-dsn.png "DSN seçin")

3. Kimlik bilgileri istenirse, kullanıcı adı girin **belirteci**. Parola için Databricks çalışma alanından alınan belirteç değeri sağlayın.

    ![Databricks için kimlik bilgilerini sağlamanız](./media/connect-databricks-excel-python-r/excel-databricks-token.png "DSN seçin")

4. Gezgin penceresinden Databricks'te Excel'e yükleyin ve ardından istediğiniz tabloyu seçin **yük**. 

    ![DTA Excel'e Yükle](./media/connect-databricks-excel-python-r/excel-load-data.png "yük dta Excel'e")

Excel çalışma kitabınızda veri oluşturduktan sonra analitik işlemlerini gerçekleştirebilirsiniz.

## <a name="connect-from-r"></a>R ' bağlanma

> [!NOTE]
> Bu bölümde, Azure Databricks ile masaüstünüzde çalışan bir R Studio istemci tümleştirme hakkında bilgi sağlar. Azure Databricks kümesinde kendi R Studio kullanımı hakkında yönergeler için bkz: [Azure Databricks üzerinde R Studio](https://docs.azuredatabricks.net/spark/latest/sparkr/rstudio.html).

Bu bölümde, Azure Databricks'te kullanılabilir başvuru verileriyle R dili IDE kullanın. Başlamadan önce bilgisayarda yüklü olması gerekir.

* R dili için bir IDE. Bu makalede, RStudio Desktop için kullanır. Buradan yükleyebilirsiniz [R Studio indirme](https://www.rstudio.com/products/rstudio/download/).
* RStudio Desktop için IDE'nizi kullanırsanız, Microsoft R Client'ndan yüklemeye [ https://aka.ms/rclient/ ](https://aka.ms/rclient/). 

RStudio açın ve aşağıdaki adımları uygulayın:

- Başvuru `RODBC` paket. Bu, daha önce oluşturduğunuz DSN'ı kullanarak Azure Databricks'e bağlamanızı sağlar.
- DSN kullanarak bağlantı kurun.
- Azure databricks'te veriler üzerinde bir SQL sorgusunu çalıştırın. Aşağıdaki kod parçacığında *radio_sample_data* Azure Databricks'te zaten bir tablodur.
- Çıktıyı doğrulamak için bir sorgu üzerindeki bazı işlemler gerçekleştirin. 

Aşağıdaki kod parçacığını bu görevleri gerçekleştirir:

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

## <a name="connect-from-python"></a>Python'dan bağlanma

Bu bölümde, Azure Databricks'te kullanılabilir başvuru verileriyle (örneğin, IDLE) bir Python IDE kullanın. Başlamadan önce aşağıdaki önkoşulları tamamlayın:

* Python'dan yükleme [burada](https://www.python.org/downloads/). Python'ı bu bağlantıdan yükleyerek, boşta yükler.

* Bilgisayarda bir komut isteminden yükleme `pyodbc` paket. Şu komutu çalıştırın:

      pip install pyodbc

Boşta açın ve aşağıdaki adımları uygulayın:

- İçeri aktarma `pyodbc` paket. Bu, daha önce oluşturduğunuz DSN'ı kullanarak Azure Databricks'e bağlamanızı sağlar.
- Daha önce oluşturduğunuz DSN kullanarak bağlantı kurun.
-  Oluşturduğunuz bağlantıyı kullanarak bir SQL sorgusunu çalıştırın. Aşağıdaki kod parçacığında *radio_sample_data* Azure Databricks'te zaten bir tablodur.
- Çıktıyı doğrulama için sorgu işlemleri.

Aşağıdaki kod parçacığını bu görevleri gerçekleştirir:

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

* Burada Azure Databricks'e verileri içeri aktarabilirsiniz kaynaklardan hakkında bilgi edinmek için bkz: [Azure Databricks için veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)


