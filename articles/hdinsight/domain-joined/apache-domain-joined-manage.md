---
title: Kurumsal güvenlik Enterprise - Azure HDInsight kümelerini yönetme
description: Kurumsal güvenlik paketi ile HDInsight kümelerini nasıl yöneteceğinizi öğrenin.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 08/24/2018
ms.openlocfilehash: 951bd74c67c77c944a17e41646c4fe49ef46b33f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128322"
---
# <a name="manage-hdinsight-clusters-with-enterprise-security-package"></a>Kurumsal güvenlik paketi ile HDInsight kümelerini yönetme
Kullanıcılar ve roller HDInsight Kurumsal güvenlik paketi (ESP) ve ESP kümelerini nasıl yöneteceğinizi öğrenin.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için VSCode kullanma

Yönetilen Apache Ambari kullanıcı adı kullanarak normal bir küme bağlantısını, ayrıca etki alanı kullanıcı adı kullanarak bir güvenlik Apache Hadoop kümesi bağlantı (örneğin: user1@contoso.com).
1. Seçerek komut paletini açın **CTRL + SHIFT + P**yazıp enter **HDInsight: Küme bağlantı**.

   ![bağlantı kümesi komutu](./media/apache-domain-joined-manage/link-cluster-command.png)

2. HDInsight girin küme URL'si, kullanıcı adı -> giriş -> giriş parola seçin -> küme türü -> bunu gösterir başarı bilgisi doğrulama geçirilmiş.
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-domain-joined-manage/link-cluster-process.png)

   > [!NOTE]  
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanılır. 
   
3. Komutunu kullanarak bir bağlı küme görebilirsiniz **listesi küme**. Artık bağlantılı bu küme için bir komut gönderebilirsiniz.

   ![bağlı küme](./media/apache-domain-joined-manage/linked-cluster.png)

4. Ayrıca, girişini yaparak tarafından bir küme kesebilir **HDInsight: Bir küme bağlantısını** komut paletinden.

## <a name="use-intellij-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için IntelliJ kullanma

Ambari yönetilen kullanıcı adı kullanarak normal bir küme bağlantısını, ayrıca etki alanı kullanıcı adı kullanarak bir güvenlik hadoop kümesini bağlamak (örneğin: user1@contoso.com). 
1. Tıklayın **küme bağlantı** gelen **Azure Gezgini**.

   ![bağlantı kümesi bağlam menüsü](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Girin **küme adı**, **kullanıcı adı** ve **parola**. Kullanıcı adı ve parola kimlik doğrulama hatası geldiyseniz denetlemek gerekir. İsteğe bağlı olarak, depolama hesabı depolama anahtarı ekleyin ardından depolama kapsayıcısından bir kapsayıcıyı seçin. Depolama Gezgini'nde soldaki ağaçtan depolama bilgilerini içindir
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-domain-joined-manage/link-a-cluster-dialog.png)

   > [!NOTE]  
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlantılı depolama anahtarı, kullanıcı adı ve parola kullanın.
   > ![Depolama Gezgini'nde Intellij](./media/apache-domain-joined-manage/storage-explorer-in-IntelliJ.png)

   
3. Bağlı küme gördüğünüz **HDInsight** giriş bilgileri doğru olması durumunda düğümü. Artık bağlantılı bu kümeye bir uygulama gönderebilir.

   ![bağlı küme](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. Ayrıca, bir kümeden kesebilir **Azure Gezgini**.
   
   ![bağlantısız küme](./media/apache-domain-joined-manage/unlink.png)

## <a name="use-eclipse-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için Eclipse kullanma

Ambari yönetilen kullanıcı adı kullanarak normal bir küme bağlantısını, ayrıca etki alanı kullanıcı adı kullanarak bir güvenlik hadoop kümesini bağlamak (örneğin: user1@contoso.com).
1. Tıklayın **küme bağlantı** gelen **Azure Gezgini**.

   ![bağlantı kümesi bağlam menüsü](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Girin **küme adı**, **kullanıcı adı** ve **parola**, ardından kümesini bağlamak için Tamam düğmesine tıklayın. İsteğe bağlı olarak, depolama hesabı, depolama anahtarını girin ve sonra soldaki ağaç görünümünde çalışmak Depolama Gezgini'ni depolama kapsayıcısı seçin
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-domain-joined-manage/link-cluster-dialog.png)
   
   > [!NOTE]  
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlantılı depolama anahtarı, kullanıcı adı ve parola kullanın.
   > ![Depolama Gezgini'nde Eclipse](./media/apache-domain-joined-manage/storage-explorer-in-Eclipse.png)

3. Bağlı küme gördüğünüz **HDInsight** giriş bilgileri doğru olması durumunda Tamam düğmesine tıklandıktan sonra düğüm. Artık bağlantılı bu kümeye bir uygulama gönderebilir.

   ![bağlı küme](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. Ayrıca, bir kümeden kesebilir **Azure Gezgini**.
   
   ![bağlantısız küme](./media/apache-domain-joined-manage/unlink.png)

## <a name="access-the-clusters-with-enterprise-security-package"></a>Kurumsal güvenlik paketi kümeleriyle erişin.

Kurumsal güvenlik paketi (daha önce HDInsight Premium da bilinir), burada kimlik doğrulaması Active Directory ve Apache Ranger'ı ve depolama ACL'leri (ADLS ACL'ler) tarafından yetkilendirme gerçekleştirilir kümedeki birden çok kullanıcı erişim sağlar. Yetkilendirme, birden çok kullanıcı arasında güvenli sınırları sağlar ve yalnızca ayrıcalıklı kullanıcıların yetkilendirme ilkelerine bağlı olarak veri erişimi sağlar.

Güvenlik ve kullanıcı yalıtımı Kurumsal güvenlik paketi ile bir HDInsight kümesi için önemlidir. Bu gereksinimleri karşılamak için Kurumsal güvenlik paketi şu cmdlet'le kümeye SSH erişimi engellenir. Aşağıdaki tabloda, her küme türü için önerilen erişim yöntemleri gösterilmektedir:

|İş yükü|Senaryo|Erişim yöntemi|
|--------|--------|-------------|
|Apache Hadoop|Hive – etkileşimli iş/sorgular  |<ul><li>[Beeline](#beeline)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Etkileşimli iş/sorgular, PySpark etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Livy ile Zeppelin](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Apache Spark|Spark gönderme, PySpark batch senaryoları|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Etkileşimli sorgu (LLAP)|Etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Herhangi biri|Özel uygulama yükleme|<ul><li>[Betik eylemleri](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|

   > [!NOTE]  
   > Jupyter yüklü/desteklenir Kurumsal güvenlik paketi değil.

Standart API'lerini kullanarak, güvenlik açısından yardımcı olur. Ayrıca, aşağıdaki avantajlardan yararlanabilirsiniz:

1.  **Yönetim** – kodunuzu yönetmenizi ve standart API'lerini kullanarak işleri otomatik hale getirin – Livy vb. HS2.
2.  **Denetim** – SSH ile denetlemek için hangi kullanıcıların SSH bir yolu yoktur kümeye vardı. Standart uç noktaları aracılığıyla işleri oluşturulduğunda kullanıcı bağlamında yürütülen gibi bu durum gideremezsiniz. 



### <a name="beeline"></a>Beeline kullanma 
Beeline makinenizde yükleyin ve genel internet üzerinden bağlanma, aşağıdaki parametreleri kullanın: 

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Beeline yerel olarak yüklü olması ve bir Azure sanal ağ üzerinden bağlanma, aşağıdaki parametreleri kullanın: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Bir baş düğüm tam etki alanı adını bulmak için yönetme belgenin Ambari REST API'yi kullanarak HDInsight bilgileri kullanın.














## <a name="users-of-hdinsight-clusters-with-esp"></a>ESP HDInsight kümeleriyle kullanıcıları
ESP HDInsight küme, küme oluşturma sırasında oluşturulan iki kullanıcı hesapları vardır:

* **Ambari Yöneticisi**: Bu hesap olarak da bilinir, *Hadoop kullanıcısıyla* veya *HTTP kullanıcısı*. Bu hesap için Ambari https:// oturum açmak için kullanılan&lt;clustername >. azurehdinsight.net. Ayrıca, Hive ODBC sürücüsünü ve BI Araçları (örneğin, Excel, Power BI veya Tableau) kimlik doğrulama Ambari görünümleri sorguları çalıştırmak ve dış Araçlar (örneğin, PowerShell, templeton da, Visual Studio) aracılığıyla işleri yürütmek için de kullanılabilir.

Bir HDInsight kümesi ile ESP Ambari Yöneticisi ek olarak üç yeni kullanıcıları içeren

* **Ranger yönetici**:  Bu hesap yerel Apache Ranger yönetici hesabıdır. Bir active directory etki alanı kullanıcısı değil. Bu hesap ilkeleri ayarlayın ve (söz konusu kullanıcıların ilkeleri yönetebilmeniz için) diğer kullanıcılara yönetici veya yönetici temsilcileri yapmak için kullanılabilir. Varsayılan olarak, kullanıcı adı: *yönetici* ve parola Ambari Yöneticisi parolasını ile aynıdır. Parola ayarları sayfasından Ranger güncelleştirilebilir.
* **Küme Yöneticisi etki alanı kullanıcısı**: Ambari ve Ranger gibi Hadoop Küme Yöneticisi olarak atanmış bir active directory etki alanı kullanıcı hesabıdır. Küme oluşturma sırasında bu kullanıcının kimlik bilgilerini sağlamanız gerekir. Bu kullanıcıya aşağıdaki ayrıcalıklara sahiptir:

  * Makine etki alanına ve küme oluşturma sırasında belirttiğiniz OU içinde yerleştirin.
  * Küme oluşturma sırasında belirttiğiniz OU içinde hizmet sorumluları oluşturma.
  * Ters DNS girişleri oluşturma.

    Diğer AD kullanıcılar da bu ayrıcalıklara sahip olmadığını unutmayın.

    Ranger tarafından yönetilmez ve bu nedenle güvenli olmayan bazı uç noktalar (örneğin, templeton olarak da) kümesi içinde vardır. Bu uç noktaları, Küme Yöneticisi etki alanı kullanıcısı dışındaki tüm kullanıcılar için kilitlendiğini.
* **Normal**: Küme oluşturma sırasında birden çok active directory grupları sağlayabilirsiniz. Bu gruplardaki kullanıcılar için Ranger'ı ve Ambari eşitlenir. Bu kullanıcılar, etki alanı kullanıcıları olan ve yalnızca Ranger yönetilen uç noktalar (örneğin, Hiveserver2) erişebilir. RBAC ilkelerini ve bu kullanıcılar için uygun denetim sağlar.

## <a name="roles-of-hdinsight-clusters-with-esp"></a>HDInsight kümeleri ESP ile roller
Kurumsal güvenlik paketi HDInsight aşağıdaki roller vardır:

* Küme Yöneticisi
* Küme işleci
* Hizmet Yöneticisi
* Hizmet işleci
* Küme kullanıcısı

**Bu rollerden izinler görmek için**

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. Mavi soru işareti ' izinleri görmek için tıklayın:

    ![ESP HDInsight rol izinleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Ambari Yönetimi kullanıcı arabirimini açın

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. HDInsight kümenizi açın.
3. Tıklayın **Pano** Ambari'yi açmak için üstteki menüden.
4. İçin Ambari Küme Yöneticisi etki alanı kullanıcı adı ve parola kullanarak oturum açın.
5. Tıklayın **yönetici** açılan menüsünde, üst, sağ üst köşedeki ve ardından **yönetme Ambari**.

    ![ESP HDInsight Ambari yönetme](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Kullanıcı arabirimini şuna benzer:

    ![ESP HDInsight Ambari Yönetimi kullanıcı Arabirimi](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı kullanıcıları listeleyin
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **kullanıcılar**. HDInsight kümesi için Active Directory'nizden eşitlenen tüm kullanıcıları görürsünüz.

    ![ESP HDInsight Ambari yönetim kullanıcı Arabirimi kullanıcıları listeleme](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı grupları listeleme
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **grupları**. HDInsight kümesi için Active Directory'nizden eşitlenen tüm grupları göreceksiniz.

    ![ESP HDInsight Ambari yönetim kullanıcı Arabirimi grupları Listele](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive görünümleri izinlerini yapılandırma
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **görünümleri**.
3. Tıklayın **HIVE** ayrıntıları göstermek için.

    ![ESP HDInsight Ambari yönetim kullanıcı Arabirimi Hive görünümleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Tıklayın **Hive görünümü** Hive Görüşnümleri yapılandırmak için bağlantı.
5. Ekranı aşağı kaydırarak **izinleri** bölümü.

    ![ESP HDInsight Ambari Yönetimi kullanıcı Arabirimi Hive görünümleri izinlerini yapılandırma](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Tıklayın **Kullanıcı Ekle** veya **Grup Ekle**ve ardından kullanıcıları veya Hive görünümleri kullanabileceğiniz grupları belirtin.

## <a name="configure-users-for-the-roles"></a>Kullanıcı rolleri için yapılandırma
 Rolleri ve izinlerini listesini görmek için ESP ile roller, HDInsight kümeleri bakın.

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. Tıklayın **Kullanıcı Ekle** veya **Grup Ekle** kullanıcılar ve gruplar farklı rollere atamak için.

## <a name="next-steps"></a>Sonraki adımlar
* Kurumsal güvenlik paketi ile bir HDInsight kümesi yapılandırmak için bkz: [yapılandırma HDInsight kümeleri ile ESP](apache-domain-joined-configure.md).
* Hive ilkelerini ve Hive sorgularını çalıştırmak için bkz [HDInsight için Apache Hive ilkelerini kümeleri ile ESP](apache-domain-joined-run-hive.md).
