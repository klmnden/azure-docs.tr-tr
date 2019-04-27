---
title: Kurumsal güvenlik paketi - Azure ile HDInsight Hive ilkelerini yapılandırma
description: Kurumsal güvenlik paketi ile bir Azure HDInsight hizmetinde Hive için Apache Ranger ilkelerini yapılandırmayı öğrenin.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8effa84c9d7adc14060fb00fae9915a04c1d04cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60536615"
---
# <a name="configure-apache-hive-policies-in-hdinsight-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile HDInsight içinde Apache Hive ilkelerini yapılandırma
Apache Hive için Apache Ranger ilkelerini yapılandırmayı öğrenin. Bu makalede hivesampletable erişimini kısıtlamak için iki Ranger ilkesi oluşturacaksınız. hivesampletable, HDInsight kümelerine sahiptir. İlkeleri yapılandırdıktan sonra Excel ve ODBC sürücüsünü kullanarak HDInsight’taki Hive tablolarına bağlanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Kurumsal güvenlik paketi ile HDInsight kümesi. Bkz: [yapılandırma HDInsight kümeleri ile ESP](apache-domain-joined-configure.md).
* Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013’ün tek başına sürümü veya Office 2010 Professional Plus yüklü iş istasyonu.

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma
**Ranger Yönetici Arabirimine bağlanmak için**

1. Bir tarayıcıdan Ranger Yönetici Arabirimine bağlanın. URL: https://&lt;KümeAdı>.azurehdinsight.net/Ranger/.

   > [!NOTE]  
   > Ranger, Apache Hadoop kümesi farklı kimlik bilgileri kullanır. Önbelleğe alınmış Hadoop kimlik bilgilerini kullanarak tarayıcıları önlemek için Ranger yönetici Arabirimine bağlanmak için yeni bir InPrivate tarayıcı penceresinden kullanın.

2. Küme yöneticisi etki alanı kullanıcı adı ve parolasını kullanarak oturum açın:

    ![HDInsight ESP Ranger ana sayfası](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Ranger şu an için yalnızca Yarn ve Hive ile birlikte çalışmaktadır.

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma
Bkz: [HDInsight küme oluşturma ile ESP](apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp), hiveruser1 ve hiveuser2 kullanıcılarını oluşturma hakkında daha fazla bilgi için. Bu öğreticide iki kullanıcı hesaplarını kullandığınız.

## <a name="create-ranger-policies"></a>Ranger ilkelerini oluşturma
Bu bölümde hivesampletable erişimi için iki Ranger ilkesi oluşturun. Farklı sütun kümelerine select izni vereceksiniz. Her iki kullanıcı kullanılarak oluşturulan [HDInsight küme oluşturma ile ESP](apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp). Bir sonraki bölümde ise iki ilkeyi Excel'de test edeceksiniz.

**Ranger ilkeleri oluşturmak için**

1. Ranger Yönetici Arabirimini açın. Bkz. Apache Ranger yönetici Arabirimine bağlanma.
2. **Hive**’ın altındaki **&lt;KümeAdı>_hive** öğesine tıklayın. Önceden yapılandırılmış iki ilke göreceksiniz.
3. **Add New Policy**’ye tıklayıp aşağıdaki değerleri girin:

   * Policy name: read-hivesampletable-all
   * Hive Database: default
   * table: hivesampletable
   * Hive column: *
   * Select User: hiveuser1
   * Permissions: select

     ![HDInsight ESP Ranger Hive ilkesi yapılandırma](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

     > [!NOTE]  
     > Select User alanında etki alanı kullanıcısı yoksa Ranger’ın AAD ile eşitlenmesi için birkaç dakika bekleyin.
     >
     >
4. **Add**’e tıklayarak ilkeyi kaydedin.
5. Son iki adımı tekrarlayarak aşağıdaki özelliklere sahip yeni bir ilke oluşturun:

   * Policy name: read-hivesampletable-devicemake
   * Hive Database: default
   * table: hivesampletable
   * Hive column: clientid, devicemake
   * Select User: hiveuser2
   * Permissions: select

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma
Talimatlara [Hive ODBC veri kaynağı oluşturma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md) sayfasından erişebilirsiniz.  

 | Özellik  |Açıklama |
 | --- | --- |
 | Data Source Name | Veri kaynağınız için bir ad verin |
 | Host | &lt;HDInsightKümesiAdı>.azurehdinsight.net yazın. Örnek: HDIKumesi.azurehdinsight.net |
 | Bağlantı noktası | **443** yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
 | Database | **Default**’u kullanın. |
 | Hive Server Type | **Hive Server 2**’yi seçin |
 | Mechanism | **Azure HDInsight Service**’i seçin |
 | HTTP Path | Boş bırakın. |
 | User Name | hiveuser1@contoso158.onmicrosoft.com yazın. Etki alanı adı farklıysa güncelleştirin. |
 | Parola | hiveuser1 kullanıcısının parolasını girin. |

Veri kaynağını kaydetmeden önce **Test**’e tıklayın.

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma
Son bölümünde iki ilke yapılandırdınız.  hiveuser1 tüm sütunlarda select iznine, hiveuser2 ise iki sütunda select iznine sahiptir. Bu bölümde verileri Excel'e aktarmak için iki kullanıcının kimliğine bürüneceksiniz.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.
2. **Veri** sekmesinde **Diğer Veri Kaynaklarından**’a ve ardından **Veri Bağlantı Sihirbazı’ndan** öğesine tıklayarak **Veri Bağlantı Sihirbazı**’nı açın.

    ![Veri bağlantı sihirbazını açın][img-hdi-simbahiveodbc.excel.dataconnection]
3. Veri kaynağı olarak **ODBC DSN**’yi seçip **İleri**’ye tıklayın.
4. ODBC veri kaynaklarından bir önceki adımda oluşturduğunuz veri kaynağı adını seçip **İleri**’ye tıklayın.
5. Sihirbaza kümenin parolasını tekrar girin ve ardından **Tamam**. **Veritabanı ve Tablo Seç** iletişim kutusunun açılmasını bekleyin. Bu işlem birkaç saniye sürebilir.
6. **hivesampletable**’ı seçip **İleri**’ye tıklayın.
7. **Son**'a tıklayın.
8. **Verileri İçeri Aktar** iletişim kutusunda sorguyu değiştirebilir veya belirtebilirsiniz. Bunun için **Özellikler**’e tıklayın. Bu işlem birkaç saniye sürebilir.
9. **Tanım** sekmesine tıklayın. Komut metni şu şekildedir:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Tanımladığınız Ranger ilkelerine göre hiveuser1 kullanıcısı tüm sütunlarda select iznine sahiptir.  Bu nedenle bu sorgu hiveuser1 kullanıcısının kimlik bilgileriyle çalışır ancak hiveuser2 kullanıcısının kimlik bilgileriyle bu sorgu çalışmaz.

   ![Bağlantı Özellikleri][img-hdi-simbahiveodbc-excel-connectionproperties]
10. **Tamam**’a tıklayarak Bağlantı Özellikleri iletişim kutusunu kapatın.
11. **Tamam**’a tıklayarak **Verileri İçeri Aktar** iletişim kutusunu kapatın.  
12. hiveuser1 kullanıcısının parolasını tekrar girin ve **Tamam**’a tıklayın. Verilerin Excel’e aktarılması birkaç saniye sürer. İşlem tamamlandığında 11 veri sütunu göreceksiniz.

Son bölümde oluşturduğunuz ikinci ilkeyi (read-hivesampletable-devicemake) test etmek için

1. Excel'de yeni bir sayfa ekleyin.
2. Verileri içeri aktarmak için son yordamı uygulayın.  Yaptığınız tek değişiklik hiveuser1 yerine kullanıcısının hiveuser2 kullanıcısının kimlik bilgilerini kullanmaktır. Hiveuser2 yalnızca iki sütunu görme izni olduğu için başarısız olur. Şu hatayı alacaksınız:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Verileri içe aktarmak için aynı yordamı uygulayın. Bu sefer hiveuser2 kullanıcısının kimlik bilgilerini kullanın ve select deyimi için:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    yerine şunu yazın:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    İşlem tamamlandığında iki veri sütununun içe aktarıldığını göreceksiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kurumsal güvenlik paketi ile bir HDInsight kümesi yapılandırmak için bkz: [yapılandırma HDInsight kümeleri ile ESP](apache-domain-joined-configure.md).
* Bir HDInsight kümesi ile ESP yönetmek için bkz: [yönetme HDInsight kümeleri ile ESP](apache-domain-joined-manage.md).
* ESP ile HDInsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Bağlanma Hive JDBC kullanarak Hive için bkz: [Apache Hive üzerindeki Azure HDInsight Hive JDBC sürücüsü kullanarak bağlanma](../hadoop/apache-hadoop-connect-hive-jdbc-driver.md)
* Excel için Hadoop Hive ODBC kullanarak bağlanmak için bkz: [Microsoft Hive ODBC sürücüsü ile Apache Hadoop Excel'e bağlanma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
* Excel için Power Query kullanarak Hadoop bağlanmak için bkz: [Power Query kullanarak Apache Hadoop Excel'e bağlanma](../hadoop/apache-hadoop-connect-excel-power-query.md)
