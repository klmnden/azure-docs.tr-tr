---
title: Etki alanına katılmış Hdınsight'ta - Azure Hive ilkelerini yapılandırma
description: Öğrenin ....
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: bd99e5fda80663b37c60d972742b16c27b92cf55
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31592684"
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight"></a>Etki alanına katılmış Hdınsight'ta Hive ilkelerini yapılandırma
Hive için Apache Ranger ilkelerini yapılandırmayı öğrenin. Bu makalede hivesampletable erişimini kısıtlamak için iki Ranger ilkesi oluşturacaksınız. hivesampletable, HDInsight kümelerine sahiptir. İlkeleri yapılandırdıktan sonra Excel ve ODBC sürücüsünü kullanarak HDInsight’taki Hive tablolarına bağlanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Etki alanına katılmış HDInsight kümesi. Bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013’ün tek başına sürümü veya Office 2010 Professional Plus yüklü iş istasyonu.

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma
**Ranger Yönetici Arabirimine bağlanmak için**

1. Bir tarayıcıdan Ranger Yönetici Arabirimine bağlanın. URL: https://&lt;KümeAdı>.azurehdinsight.net/Ranger/.

   > [!NOTE]
   > Ranger’ın kimlik bilgileri Hadoop kümesinin kimlik bilgilerinden farklıdır. Önbelleğe alınan Hadoop kimlik bilgilerini kullanarak tarayıcıları önlemek için bırakabilmenizi yönetici UI bağlanmak için yeni InPrivate tarayıcı penceresini kullanın.
   >
   >
2. Küme yöneticisi etki alanı kullanıcı adı ve parolasını kullanarak oturum açın:

    ![HDInsight etki alanına katılmış Ranger ana sayfası](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Ranger şu an için yalnızca Yarn ve Hive ile birlikte çalışmaktadır.

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma
Bkz: [bir etki alanına katılmış Hdınsight kümesi oluşturma](apache-domain-joined-configure-using-azure-adds.md#create-a-domain-joined-hdinsight-cluster), hiveruser1 ve hiveuser2 nasıl oluşturulacağı hakkında bilgi için. Bu öğreticide iki kullanıcı hesabını kullanın.

## <a name="create-ranger-policies"></a>Ranger ilkelerini oluşturma
Bu bölümde, hivesampletable erişmek için iki bırakabilmenizi ilkeleri oluşturun. Farklı sütun kümelerine select izni vereceksiniz. Her iki kullanıcı kullanılarak oluşturulan [bir etki alanına katılmış Hdınsight kümesi oluşturma](apache-domain-joined-configure-using-azure-adds.md#create-a-domain-joined-hdinsight-cluster). Bir sonraki bölümde ise iki ilkeyi Excel'de test edeceksiniz.

**Ranger ilkeleri oluşturmak için**

1. Ranger Yönetici Arabirimini açın. Bkz. [Apache Ranger Yönetici Arabirimine bağlanma](#connect-to-apache-ranager-admin-ui).
2. ****Hive**’ın altındaki &lt;KümeAdı>_hive** öğesine tıklayın. Önceden yapılandırılmış iki ilke göreceksiniz.
3. **Add New Policy**’ye tıklayıp aşağıdaki değerleri girin:

   * Policy name: read-hivesampletable-all
   * Hive Database: default
   * table: hivesampletable
   * Hive column: *
   * Select User: hiveuser1
   * Permissions: select

     ![HDInsight etki alanına katılmış Ranger Hive ilkesi yapılandırma](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

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
 | User Name | Girin hiveuser1@contoso158.onmicrosoft.com. Farklı ise, etki alanı adını güncelleştirin. |
 | Parola | hiveuser1 kullanıcısının parolasını girin. |

Veri kaynağını kaydetmeden önce **Test**’e tıklayın.

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma
Son bölümünde iki ilke yapılandırdınız.  hiveuser1 tüm sütunlarda select iznine, hiveuser2 ise iki sütunda select iznine sahiptir. Bu bölümde verileri Excel'e aktarmak için iki kullanıcının kimliğine bürüneceksiniz.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.
2. **Veri** sekmesinde **Diğer Veri Kaynaklarından**’a ve ardından **Veri Bağlantı Sihirbazı’ndan** öğesine tıklayarak **Veri Bağlantı Sihirbazı**’nı açın.

    ![Veri bağlantı sihirbazını açın][img-hdi-simbahiveodbc.excel.dataconnection]
3. Veri kaynağı olarak **ODBC DSN**’yi seçip **İleri**’ye tıklayın.
4. ODBC veri kaynaklarından bir önceki adımda oluşturduğunuz veri kaynağı adını seçip **İleri**’ye tıklayın.
5. Sihirbaz kümeye için parolayı yeniden girin ve ardından **Tamam**. **Veritabanı ve Tablo Seç** iletişim kutusunun açılmasını bekleyin. Bu işlem birkaç saniye sürebilir.
6. **hivesampletable**’ı seçip **İleri**’ye tıklayın.
7. **Son**'a tıklayın.
8. **Verileri İçeri Aktar** iletişim kutusunda sorguyu değiştirebilir veya belirtebilirsiniz. Bunun için **Özellikler**’e tıklayın. Bu işlem birkaç saniye sürebilir.
9. **Tanım** sekmesine tıklayın. Komut metni şu şekildedir:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Tanımladığınız Ranger ilkelerine göre hiveuser1 kullanıcısı tüm sütunlarda select iznine sahiptir.  Bu nedenle bu sorgu hiveuser1'ın kimlik bilgileriyle çalışır, ancak bu sorgu hiveuser2'ın kimlik bilgileri ile çalışmaz.

   ![Bağlantı Özellikleri][img-hdi-simbahiveodbc-excel-connectionproperties]
10. **Tamam**’a tıklayarak Bağlantı Özellikleri iletişim kutusunu kapatın.
11. **Tamam**’a tıklayarak **Verileri İçeri Aktar** iletişim kutusunu kapatın.  
12. hiveuser1 kullanıcısının parolasını tekrar girin ve **Tamam**’a tıklayın. Verilerin Excel’e aktarılması birkaç saniye sürer. İşlem tamamlandığında 11 veri sütunu göreceksiniz.

Son bölümünde oluşturduğunuz ikinci ilkesini (okuma hivesampletable devicemake) sınamak için

1. Excel'de yeni bir sayfa ekleyin.
2. Verileri içeri aktarmak için son yordamı uygulayın.  Yaptığınız tek değişiklik, hiveuser1 yerine 's hiveuser2'in kimlik bilgilerini kullanmaktır. Hiveuser2 yalnızca iki sütun görmek için izne sahip olduğundan bu başarısız olur. Şu hatayı alacaksınız:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Verileri içe aktarmak için aynı yordamı uygulayın. Bu sefer hiveuser2 kullanıcısının kimlik bilgilerini kullanın ve select deyimi için:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    yerine şunu yazın:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    İşlem tamamlandığında iki veri sütununun içe aktarıldığını göreceksiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış HDInsight kümesini yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Bir etki alanına katılmış Hdınsight kümesi için bkz. [yönetmek etki alanına katılmış Hdınsight kümeleri](apache-domain-joined-manage.md).
* Etki alanına katılmış Hdınsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Hive JDBC kullanarak Hive’a bağlanmak için bkz. [Hive JDBC kullanarak Azure HDInsight üzerindeki Hive’a bağlanma](../hadoop/apache-hadoop-connect-hive-jdbc-driver.md)
* Hive ODBC kullanarak Excel ile Hadoop arasında bağlantı kurmak için bkz. [Microsoft Hive ODBC sürücüsü kullanarak Excel ile Hadoop arasında bağlantı kurma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
* Power Query kullanarak Excel ile Hadoop arasında bağlantı kurmak için bkz. [Power Query kullanarak Excel ile Hadoop arasında bağlantı kurma](../hadoop/apache-hadoop-connect-excel-power-query.md)
