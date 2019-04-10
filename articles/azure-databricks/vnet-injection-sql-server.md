---
title: Sorgu bir SQL Server Linux Docker kapsayıcısı içinde bir sanal ağdan bir Azure Databricks not defteri
description: Bu makalede, Azure Databricks, sanal ağ olarak da bilinen sanal ağa ekleme dağıtmayı açıklar.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 345e07fac30f4ad0c8e9918cb8a1ff0fb8aeb811
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288959"
---
# <a name="tutorial-query-a-sql-server-linux-docker-container-in-a-virtual-network-from-an-azure-databricks-notebook"></a>Öğretici: Sorgu bir SQL Server Linux Docker kapsayıcısı içinde bir sanal ağdan bir Azure Databricks not defteri

Bu öğreticide bir sanal ağdaki bir SQL Server Linux Docker kapsayıcısı ile Azure Databricks tümleştirme öğretir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Databricks çalışma alanı bir sanal ağa dağıtma
> * Linux sanal makinesi ortak bir ağ yükleme
> * Docker'ı yükleme
> * Microsoft SQL Server Linux docker kapsayıcısını yükleme
> * SQL Server JDBC bir Databricks Not Defteri kullanarak sorgulama

## <a name="prerequisites"></a>Önkoşullar

* Oluşturma bir [Databricks çalışma alanı bir sanal ağdaki](quickstart-create-databricks-workspace-vnet-injection.md).

* Yükleme [Windows için Ubuntu](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).

* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)’yu indirin.

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

1. Azure portalında simgesini seçin **sanal makineler**. Ardından, **+ Ekle**.

    ![Yeni bir Azure sanal makine ekleyin](./media/vnet-injection-sql-server/add-virtual-machine.png)

2. Üzerinde **Temelleri** sekmesinde, Ubuntu Server 16.04 LTS seçin. Bir VCPU ve 2 GB RAM B1ms VM boyutunu değiştirin. Bir Linux SQL Server Docker kapsayıcısı için en düşük gereksinimi 2 GB'dir. Bir yönetici kullanıcı adı ve parola seçin.

    ![Temel bilgiler sekmesinde yeni sanal makine yapılandırması](./media/vnet-injection-sql-server/create-virtual-machine-basics.png)

3. Gidin **ağ** sekmesi. Sanal ağ ve Azure Databricks kümenizi içeren ortak bir alt ağ seçin. Seçin **gözden geçir + Oluştur**, ardından **Oluştur** sanal makine dağıtma.

    ![Yeni sanal makine yapılandırması, Ağ sekmesi](./media/vnet-injection-sql-server/create-virtual-machine-networking.png)

4. Dağıtım tamamlandığında, sanal makineye gidin. Sanal ağ/alt ağ içinde ve genel IP adresi fark **genel bakış**. Seçin **genel IP adresi**

    ![Sanal makineye genel bakış](./media/vnet-injection-sql-server/virtual-machine-overview.png)

5. Değişiklik **atama** için **statik** girin bir **DNS ad etiketi**. Seçin **Kaydet**ve sanal makineyi yeniden başlatın.

    ![Genel IP adresi yapılandırması](./media/vnet-injection-sql-server/virtual-machine-staticip.png)

6. Seçin **ağ** sekmesinde altında **ayarları**. Azure Databricks dağıtım sırasında oluşturulmuş olan ağ güvenlik grubu sanal makineyle ilişkili olduğuna dikkat edin. Seçin **gelen bağlantı noktası kuralı Ekle**.

7. SSH için 22 numaralı bağlantı noktasını açmak için bir kural ekleyin. Aşağıdaki ayarları kullanın:
    
    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Kaynak|IP Adresleri|IP adresleri, belirli bir kaynak IP adresine izin verilir veya bu kural tarafından reddedildi'ten gelen trafiği belirtir.|
    |Kaynak IP adresleri|< genel IP\>|Girin genel IP adresi. Genel IP adresi ziyaret ederek bulabilirsiniz [bing.com](https://www.bing.com/) ve arama **"IP Adresim"**.|
    |Kaynak bağlantı noktası aralıkları|*|Herhangi bir bağlantı noktasına gelen trafiğe izin verin.|
    |Hedef|IP Adresleri|IP adresleri için belirli bir kaynak IP adresine izin verilir veya bu kural tarafından reddedildi, giden trafiği belirtir.|
    |Hedef IP adresleri|< VM'nin genel IP\>|Sanal makinenizin genel IP adresini girin. Bunu şirket bulabilirsiniz **genel bakış** sayfasında, sanal makinenizin.|
    |Hedef bağlantı noktası aralıkları|22|SSH için 22 numaralı bağlantı noktasını açın.|
    |Öncelik|290|Kural bir öncelik verin.|
    |Ad|SSH-databricks-öğretici-vm|Kural, bir ad verin.|


    ![Bağlantı noktası 22 için gelen Güvenlik Kuralı Ekle](./media/vnet-injection-sql-server/open-port.png)

8. 1433 numaralı bağlantı noktasını aşağıdaki ayarlara sahip SQL açmak için bir kural ekleyin:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Kaynak|IP Adresleri|IP adresleri, belirli bir kaynak IP adresine izin verilir veya bu kural tarafından reddedildi'ten gelen trafiği belirtir.|
    |Kaynak IP adresleri|10.179.0.0/16|Sanal ağınız için adres aralığını girin.|
    |Kaynak bağlantı noktası aralıkları|*|Herhangi bir bağlantı noktasına gelen trafiğe izin verin.|
    |Hedef|IP Adresleri|IP adresleri için belirli bir kaynak IP adresine izin verilir veya bu kural tarafından reddedildi, giden trafiği belirtir.|
    |Hedef IP adresleri|< VM'nin genel IP\>|Sanal makinenizin genel IP adresini girin. Bunu şirket bulabilirsiniz **genel bakış** sayfasında, sanal makinenizin.|
    |Hedef bağlantı noktası aralıkları|1433|SQL Server için 22 numaralı bağlantı noktasını açın.|
    |Öncelik|300|Kural bir öncelik verin.|
    |Ad|SQL-databricks-öğretici-vm|Kural, bir ad verin.|

    ![Bağlantı noktası 1433 için gelen Güvenlik Kuralı Ekle](./media/vnet-injection-sql-server/open-port2.png)

## <a name="run-sql-server-in-a-docker-container"></a>SQL Server'ı bir Docker kapsayıcısında çalıştırma

1. Açık [Ubuntu için Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab), ya da sanal makineye SSH sağlayacak herhangi bir aracı. Sanal makinenizi Azure portal ve select gidin **Connect** bağlanmak için SSH komutunu alınamıyor.

    ![Sanal makineye bağlanma](./media/vnet-injection-sql-server/vm-ssh-connect.png)

2. Ubuntu terminalinizde komutu girin ve sanal makine yapılandırıldığında oluşturduğunuz yönetici parolasını girin.

    ![Ubuntu terminal SSH oturum açma](./media/vnet-injection-sql-server/vm-login-terminal.png)

3. Sanal makinede Docker yüklemek için aşağıdaki komutu kullanın.

    ```bash
    sudo apt-get install docker.io
    ```

    Aşağıdaki komutla Docker yüklenmesini doğrulayın:

    ```bash
    sudo docker --version
    ```

4. Görüntü yükleyin.

    ```bash
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

    Görüntüleri denetleyin.

    ```bash
    sudo docker images
    ```

5. Kapsayıcı görüntüsünü çalıştırın.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password1234' -p 1433:1433 --name sql1  -d mcr.microsoft.com/mssql/server:2017-latest
    ```

    Kapsayıcının çalışır durumda olduğunu doğrulayın.

    ```bash
    sudo docker ps -a
    ```

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. SQL Server Management Studio'yu açın ve sunucu adını ve SQL kimlik doğrulaması kullanarak sunucuya bağlanın. Oturum açma kullanıcı adı olan **SA** ve parola Docker komutunda ayarlanan paroladır. Örnek komut parola `Password1234`.

    ![SQL Server Management Studio kullanarak SQL Server'a bağlanma](./media/vnet-injection-sql-server/ssms-login.png)

2. Başarıyla bağlandıktan sonra seçin **yeni sorgu** tablo, bir veritabanı oluşturmak için aşağıdaki kod parçacığını girin ve bazı kayıtları tablosuna ekleyin.

    ```SQL
    CREATE DATABASE MYDB;
    GO
    USE MYDB;
    CREATE TABLE states(Name VARCHAR(20), Capitol VARCHAR(20));
    INSERT INTO states VALUES ('Delaware','Dover');
    INSERT INTO states VALUES ('South Carolina','Columbia');
    INSERT INTO states VALUES ('Texas','Austin');
    SELECT * FROM states
    GO
    ```

    ![SQL Server veritabanı oluşturmak için sorgu](./media/vnet-injection-sql-server/create-database.png)

## <a name="query-sql-server-from-azure-databricks"></a>Sorgu SQL Server'dan Azure Databricks

1. Azure Databricks çalışma alanınıza gidin ve ön koşulların bir parçası bir küme oluşturduğunuzu doğrulayın. Ardından, **bir not defteri oluşturma**. Not defterini seçme gibi bir ad vermek *Python* dil ve oluşturduğunuz kümeyi seçin.

    ![Yeni bir Databricks not defteri ayarları](./media/vnet-injection-sql-server/create-notebook.png)

2. SQL Server sanal makinesinin iç IP adresi için ping işlemi yapmak için aşağıdaki komutu kullanın. Bu ping başarılı olması gerekir. Aksi durumda, kapsayıcının çalıştığını ve ağ güvenlik grubu (NSG) yapılandırmasını gözden geçirmek doğrulayın.

    ```python
    %sh
    ping 10.179.64.4
    ```

    Ayrıca, gözden geçirmek için nslookup komutunu da kullanabilirsiniz.

    ```python
    %sh
    nslookup databricks-tutorial-vm.westus2.cloudapp.azure.com
    ```

3. SQL Server başarıyla işten sonra tablo ve veritabanı sorgulayabilirsiniz. Aşağıdaki python kodu çalıştırın:

    ```python
    jdbcHostname = "10.179.64.4"
    jdbcDatabase = "MYDB"
    userName = 'SA'
    password = 'Password1234'
    jdbcPort = 1433
    jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, userName, password)

    df = spark.read.jdbc(url=jdbcUrl, table='states')
    display(df)
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Azure Databricks çalışma ve tüm ilgili kaynakları silin. İşin silinmesi, gereksiz faturalandırma önler. Azure Databricks çalışma gelecekte kullanmayı planlıyorsanız, kümeyi durdurun ve daha sonra yeniden başlatın. Size bu Azure Databricks çalışma alanını kullanmaya devam etmeyecekseniz, aşağıdaki adımları kullanarak bu öğreticide oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve ardından oluşturduğunuz kaynak grubunun adına tıklayın.

2. Kaynak grubu sayfanızda seçin **Sil**, adını, metin kutusuna silinecek ve ardından kaynak **Sil** yeniden.

## <a name="next-steps"></a>Sonraki adımlar

Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Öğretici: Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek](databricks-extract-load-sql-data-warehouse.md)
