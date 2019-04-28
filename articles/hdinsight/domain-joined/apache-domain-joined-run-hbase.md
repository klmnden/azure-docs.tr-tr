---
title: Kurumsal güvenlik paketi - Azure ile HDInsight, Apache HBase ilkelerini yapılandırma
description: Hbase'de Kurumsal güvenlik paketi ile Azure HDInsight için Apache Ranger ilkelerini yapılandırmayı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: tutorial
ms.date: 02/01/2019
ms.openlocfilehash: 61d3256ca169952ab3dda76914293a06a044d6eb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113162"
---
# <a name="tutorial-configure-apache-hbase-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Öğretici: Kurumsal güvenlik paketi (Önizleme) ile HDInsight, Apache HBase ilkelerini yapılandırma

Kurumsal güvenlik paketi (ESP) Apache HBase kümeleri için Apache Ranger ilkelerini yapılandırmayı öğrenin. ESP kümeleri bir etki alanına bağlıdır ve kullanıcıların etki alanı kimlik bilgileriyle kimlik doğrulaması yapmasına olanak sağlar. Bu öğreticide, bir HBase tablosundaki farklı sütun ailesi için erişimi kısıtlamak için iki Ranger ilkesi oluşturun.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Etki alanı kullanıcılarını oluşturma
> * Ranger ilkelerini oluşturma
> * Tablo bir HBase kümesi oluşturma
> * Ranger ilkelerini test etme

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* [Azure Portal](https://portal.azure.com/) oturum açın.

* Oluşturma bir [HDInsight HBase küme Kurumsal güvenlik paketi ile](apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma

1. Bir tarayıcıdan, `https://<ClusterName>.azurehdinsight.net/Ranger/` URL’sini kullanarak Ranger Yönetici kullanıcı arabirimine bağlanın. Değiştirmeyi unutmayın `<ClusterName>` , HBase kümesi adı.

    > [!NOTE]  
    > Ranger kimlik bilgileri Hadoop kümesi kimlik bilgileriyle aynı değildir. Tarayıcıların ön belleğe alınmış Hadoop kimlik bilgilerini kullanmasını önlemek için Ranger Yönetici Arabirimine yeni bir InPrivate tarayıcı penceresinden bağlanın.

2. Azure Active Directory (AD) yönetici kimlik bilgilerinizi kullanarak oturum açın. Azure AD yönetici kimlik bilgileri HDInsight küme kimlik bilgileri veya Linux HDInsight düğümü SSH kimlik bilgileriyle aynı değildir.

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma

Ziyaret [Kurumsal güvenlik paketi ile bir HDInsight kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds)nasıl oluşturulacağını öğrenmek için **sales_user1** ve **marketing_user1** etki alanı kullanıcıları. Bir üretim senaryosunda, etki alanı kullanıcıları Active Directory kiracınızdan gelir.

## <a name="create-hbase-tables-and-import-sample-data"></a>HBase tabloları oluşturmak ve örnek verileri içeri aktarma

HBase kümelerine bağlanmak ve daha sonra kullanmak için SSH kullanabilirsiniz [Apache HBase Kabuğu](https://hbase.apache.org/0.94/book/shell.html) HBase tabloları oluşturmak için veri ve sorgu veri ekleyin. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="to-use-the-hbase-shell"></a>HBase kabuğunu kullanmak için

1. SSH'den aşağıdaki HBase komutu çalıştırın:
   
    ```bash
    hbase shell
    ```

2. Bir HBase tablosu oluşturmayı `Customers` iki sütun ailesi ile: `Name` ve `Contact`.

    ```hbaseshell   
    create 'Customers', 'Name', 'Contact'
    list
    ```
3. Bazı verileri ekleyin:
    
    ```hbaseshell   
    put 'Customers','1001','Name:First','Alice'
    put 'Customers','1001','Name:Last','Johnson'
    put 'Customers','1001','Contact:Phone','333-333-3333'
    put 'Customers','1001','Contact:Address','313 133rd Place'
    put 'Customers','1001','Contact:City','Redmond'
    put 'Customers','1001','Contact:State','WA'
    put 'Customers','1001','Contact:ZipCode','98052'
    put 'Customers','1002','Name:First','Robert'
    put 'Customers','1002','Name:Last','Stevens'
    put 'Customers','1002','Contact:Phone','777-777-7777'
    put 'Customers','1002','Contact:Address','717 177th Ave'
    put 'Customers','1002','Contact:City','Bellevue'
    put 'Customers','1002','Contact:State','WA'
    put 'Customers','1002','Contact:ZipCode','98008'
    ```
4. Tablonun içindekileri görüntüleyin:
    
    ```hbaseshell
    scan 'Contacts'
    ```
    ![HDInsight Hadoop HBase kabuğu](./media/apache-domain-joined-run-hbase/hbase-shell-scan-table.png)

## <a name="create-ranger-policies"></a>Ranger ilkelerini oluşturma

Ranger ilke oluşturmak **sales_user1** ve **marketing_user1**.

1. **Ranger Yönetici Arabirimini** açın. Tıklayın  **\<ClusterName > _hbase** altında **HBase**.

   ![Apache Ranger Yönetici Arabirimi](./media/apache-domain-joined-run-hbase/apache-ranger-admin-login.png)

2. **Liste, ilkeleri** bu küme için oluşturulan tüm Ranger ilkelerini ekranı görüntüler. Bir önceden yapılandırılmış ilke listelenebilir. Tıklayın **yeni ilke Ekle**.

    ![Apache Ranger Yönetici Arabirimi Oluşturma İlkesi](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policies-list.png)

3. Üzerinde **ilke Oluştur** ekranında, aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  sales_customers_name_contact   |
   |HBase tablo   |  Müşteriler |
   |HBase sütun ailesi   |  İlgili kişi adı |
   |HBase sütun   |  * |
   |Grup Seçin  | |
   |Kullanıcı Seçin  | sales_user1 |
   |İzinler  | Okuma |

   Konu adında şu joker karakterler bulunabilir:

   * `*` karakter sıfır veya daha fazla oluşumunu gösterir.
   * `?` tek karakter gösterir.

   ![Apache Ranger Yönetici Arabirimi Oluşturma İlkesi](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-sales.png)

   >[!NOTE]
   >**Select User** için bir etki alanı kullanıcısı otomatik olarak doldurulmazsa, Ranger’ın Azure AD ile eşitlenmesi için birkaç dakika bekleyin.

4. **Add**’e tıklayarak ilkeyi kaydedin.

5. **Add New Policy**’ye tıklayıp aşağıdaki değerleri girin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |İlke Adı  |  marketing_customers_contact   |
   |HBase tablo   |  Müşteriler |
   |HBase sütun ailesi   |  İletişim |
   |HBase sütun   |  * |
   |Grup Seçin  | |
   |Kullanıcı Seçin  | marketing_user1 |
   |İzinler  | Okuma |

   ![Apache Ranger Yönetici Arabirimi Oluşturma İlkesi](./media/apache-domain-joined-run-hbase/apache-ranger-hbase-policy-create-marketing.png)  

6. **Add**’e tıklayarak ilkeyi kaydedin.

## <a name="test-the-ranger-policies"></a>Ranger ilkelerini test etme

Yapılandırılmış, Ranger ilkelerine bağlı **sales_user1** tüm sütunlar için verileri hem de görüntüleyebilirsiniz `Name` ve `Contact` sütun ailesi. **Marketing_user1** verileri yalnızca görüntüleyebilir `Contact` sütun ailesi.

### <a name="access-data-as-salesuser1"></a>Sales_user1 olarak veri erişimi

1. Kümeye yeni bir SSH bağlantısı açın. Küme oturum açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. İstenen kullanıcı bağlamında değiştirmek için kinit komutunu kullanın.

   ```bash
   kinit sales_user1
   ```

2. Hbase kabuğunu açabilir ve tablo tarama `Customers`.

   ```hbaseshell
   hbase shell
   scan `Customers`
   ```

3. Satış kullanıcı tüm sütunları görüntüleyebilirsiniz fark `Customers` iki sütun da dahil olmak üzere tablo `Name` beş sütunları yanı sıra, sütun ailesi `Contact` sütun ailesi.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1001                              column=Name:First, timestamp=1548894871561, value=Alice
     1001                              column=Name:Last, timestamp=1548894871707, value=Johnson
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
     1002                              column=Name:First, timestamp=1548894897419, value=Robert
     1002                              column=Name:Last, timestamp=1548894897487, value=Stevens
    2 row(s) in 0.1000 seconds
    ```

### <a name="access-data-as-marketinguser1"></a>Marketing_user1 olarak veri erişimi

1. Kümeye yeni bir SSH bağlantısı açın. Olarak oturum açmak için aşağıdaki komutu kullanın **marketing_user1**:

   ```bash
   ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
   ```

1. Kinit komutunu istenen kullanıcı bağlamına geçin.

   ```bash
   kinit marketing_user1
   ```

2. Hbase kabuğunu açabilir ve tablo tarama `Customers`:

    ```hbaseshell
    hbase shell
    scan `Customers`
    ```

3. Pazarlama kullanıcı yalnızca beş sütunlarını görüntüleme fark `Contact` sütun ailesi.

    ```hbaseshell
    ROW                                COLUMN+CELL
     1001                              column=Contact:Address, timestamp=1548894873820, value=313 133rd Place
     1001                              column=Contact:City, timestamp=1548895061523, value=Redmond
     1001                              column=Contact:Phone, timestamp=1548894871759, value=333-333-3333
     1001                              column=Contact:State, timestamp=1548895061613, value=WA
     1001                              column=Contact:ZipCode, timestamp=1548895063111, value=98052
     1002                              column=Contact:Address, timestamp=1548894899174, value=717 177th Ave
     1002                              column=Contact:City, timestamp=1548895103129, value=Bellevue
     1002                              column=Contact:Phone, timestamp=1548894897524, value=777-777-7777
     1002                              column=Contact:State, timestamp=1548895103231, value=WA
     1002                              column=Contact:ZipCode, timestamp=1548895104804, value=98008
    2 row(s) in 0.0730 seconds
    ```

9. Ranger kullanıcı arabiriminden denetim erişimi olaylarını görüntüleyin.

   ![Ranger Kullanıcı Arabirimi Denetim İlkesi](./media/apache-domain-joined-run-hbase/apache-ranger-admin-audit.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımlarla oluşturduğunuz HBase kümesi silin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. İçinde **arama** kutusunun üstündeki türü **HDInsight**. 
1. Seçin **HDInsight kümeleri** altında **Hizmetleri**.
1. Görüntülenen listede HDInsight kümelerinin, tıklayın **...**  yanında, Bu öğretici için oluşturduğunuz küme. 
1. **Sil**'e tıklayın. **Evet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Apache HBase ile çalışmaya başlama](../hbase/apache-hbase-tutorial-get-started-linux.md)
