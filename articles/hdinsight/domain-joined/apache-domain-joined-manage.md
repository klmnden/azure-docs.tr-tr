---
title: Etki alanına katılmış HDInsight kümeleri - Azure'ı yönetme
description: Etki alanına katılmış HDInsight kümelerini yönetme hakkında bilgi edinin
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2018
ms.openlocfilehash: 494049cffe77e23c33528747e04bf96065fac2e2
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051613"
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Etki alanına katılmış HDInsight kümelerini yönetme
Kullanıcılar ve roller etki alanına katılmış HDInsight yanı sıra, etki alanına katılmış HDInsight kümelerini nasıl yöneteceğinizi öğrenin.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Etki alanına katılmış kümeye bağlamak için VSCode kullanma

Ambari yönetilen kullanıcı adı kullanarak normal bir küme bağlantısını, ayrıca etki alanı kullanıcı adı kullanarak bir güvenlik hadoop kümesini bağlamak (örneğin: user1@contoso.com).
1. Seçerek komut paletini açın **CTRL + SHIFT + P**yazıp enter **HDInsight: küme bağlantı**.

   ![bağlantı kümesi komutu](./media/apache-domain-joined-manage/link-cluster-command.png)

2. HDInsight girin küme URL'si, kullanıcı adı -> giriş -> giriş parola seçin -> küme türü -> bunu gösterir başarı bilgisi doğrulama geçirilmiş.
   
   ![bağlantı kümesi iletişim kutusu](./media/apache-domain-joined-manage/link-cluster-process.png)

   > [!NOTE]
   > Küme hem Azure aboneliğinizde oturum ve bir kümeye bağlı bağlı kullanıcı adı ve parola kullanılır. 
   
3. Komutunu kullanarak bir bağlı küme görebilirsiniz **listesi küme**. Artık bağlantılı bu küme için bir komut gönderebilirsiniz.

   ![bağlı küme](./media/apache-domain-joined-manage/linked-cluster.png)

4. Ayrıca, girişini yaparak tarafından bir küme kesebilir **HDInsight: küme bağlantısını** komut paletinden.

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
|Hadoop|Hive – etkileşimli iş/sorgular |<ul><li>[Beeline](#beeline)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Etkileşimli iş/sorgular, PySpark etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Livy ile Zeppelin](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Spark gönderme, PySpark batch senaryoları|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
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














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Kullanıcıları etki alanına katılmış HDInsight kümeleri
Küme oluşturma sırasında oluşturulan iki kullanıcı hesabını değil etki alanına katılmış bir HDInsight kümesine sahiptir:

* **Ambari Yöneticisi**: Bu hesap olarak da bilinir, *Hadoop kullanıcısıyla* veya *HTTP kullanıcısı*. Bu hesap için Ambari https:// oturum açmak için kullanılan&lt;clustername >. azurehdinsight.net. Ayrıca, Hive ODBC sürücüsünü ve BI Araçları (örneğin, Excel, Powerbı veya Tableau) kimlik doğrulama Ambari görünümleri sorguları çalıştırmak ve dış Araçlar (örneğin, PowerShell, templeton da, Visual Studio) aracılığıyla işleri yürütmek için de kullanılabilir.

Ambari Yöneticisi ek olarak üç yeni kullanıcı bir etki alanına katılmış HDInsight kümesine sahip

* **Ranger yönetici**: Bu hesap yerel Apache Ranger yönetici hesabıdır. Bir active directory etki alanı kullanıcısı değil. Bu hesap ilkeleri ayarlayın ve (söz konusu kullanıcıların ilkeleri yönetebilmeniz için) diğer kullanıcılara yönetici veya yönetici temsilcileri yapmak için kullanılabilir. Varsayılan olarak, kullanıcı adı: *yönetici* ve parola Ambari Yöneticisi parolasını ile aynıdır. Parola ayarları sayfasından Ranger güncelleştirilebilir.
* **Küme Yöneticisi etki alanı kullanıcısı**: Bu hesap, Ambari ve Ranger gibi Hadoop Küme Yöneticisi olarak atanmış bir active directory etki alanı kullanıcısı. Küme oluşturma sırasında bu kullanıcının kimlik bilgilerini sağlamanız gerekir. Bu kullanıcıya aşağıdaki ayrıcalıklara sahiptir:

  * Makine etki alanına ve küme oluşturma sırasında belirttiğiniz OU içinde yerleştirin.
  * Küme oluşturma sırasında belirttiğiniz OU içinde hizmet sorumluları oluşturma.
  * Ters DNS girişleri oluşturma.

    Diğer AD kullanıcılar da bu ayrıcalıklara sahip olmadığını unutmayın.

    Ranger tarafından yönetilmez ve bu nedenle güvenli olmayan bazı uç noktalar (örneğin, templeton olarak da) kümesi içinde vardır. Bu uç noktaları, Küme Yöneticisi etki alanı kullanıcısı dışındaki tüm kullanıcılar için kilitlendiğini.
* **Normal**: küme oluşturma sırasında birden çok active directory grupları sağlayabilirsiniz. Bu gruplardaki kullanıcılar için Ranger'ı ve Ambari eşitlenir. Bu kullanıcılar, etki alanı kullanıcıları olan ve yalnızca Ranger yönetilen uç noktalar (örneğin, Hiveserver2) erişebilir. RBAC ilkelerini ve bu kullanıcılar için uygun denetim sağlar.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Etki alanına katılmış HDInsight kümelerinin rolleri
Etki alanına katılmış HDInsight aşağıdaki roller vardır:

* Küme Yöneticisi
* Küme işleci
* Hizmet Yöneticisi
* Hizmet işleci
* Küme kullanıcısı

**Bu rollerden izinler görmek için**

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. Mavi soru işareti ' izinleri görmek için tıklayın:

    ![Etki alanına katılmış HDInsight rol izinleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Ambari Yönetimi kullanıcı arabirimini açın

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. HDInsight kümenizi açın. Bkz: [kümeleri Listele ve Göster](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Tıklayın **Pano** Ambari'yi açmak için üstteki menüden.
4. İçin Ambari Küme Yöneticisi etki alanı kullanıcı adı ve parola kullanarak oturum açın.
5. Tıklayın **yönetici** açılan menüsünde, üst, sağ üst köşedeki ve ardından **yönetme Ambari**.

    ![Etki alanına katılmış HDInsight Ambari yönetme](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Kullanıcı arabirimini şuna benzer:

    ![Etki alanına katılmış HDInsight Ambari Yönetimi kullanıcı Arabirimi](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı kullanıcıları listeleyin
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **kullanıcılar**. HDInsight kümesi için Active Directory'nizden eşitlenen tüm kullanıcıları görürsünüz.

    ![Etki alanına katılmış HDInsight Ambari yönetim kullanıcı Arabirimi kullanıcıları listeleme](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı grupları listeleme
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **grupları**. HDInsight kümesi için Active Directory'nizden eşitlenen tüm grupları göreceksiniz.

    ![Etki alanına katılmış HDInsight Ambari yönetim kullanıcı Arabirimi grupları Listele](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive görünümleri izinlerini yapılandırma
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **görünümleri**.
3. Tıklayın **HIVE** ayrıntıları göstermek için.

    ![Etki alanına katılmış HDInsight Ambari yönetim kullanıcı Arabirimi Hive görünümleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Tıklayın **Hive görünümü** Hive Görüşnümleri yapılandırmak için bağlantı.
5. Ekranı aşağı kaydırarak **izinleri** bölümü.

    ![Etki alanına katılmış HDInsight Ambari yönetim kullanıcı Arabirimi Hive görünümleri izinleri yapılandırma](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Tıklayın **Kullanıcı Ekle** veya **Grup Ekle**ve ardından kullanıcıları veya Hive görünümleri kullanabileceğiniz grupları belirtin.

## <a name="configure-users-for-the-roles"></a>Kullanıcı rolleri için yapılandırma
 Rolleri ve izinlerini listesini görmek için bkz: [rolleri, etki alanına katılmış HDInsight kümeleri](#roles-of-domain---joined-hdinsight-clusters).

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [Ambari UI Yönetimi](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. Tıklayın **Kullanıcı Ekle** veya **Grup Ekle** kullanıcılar ve gruplar farklı rollere atamak için.

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış HDInsight kümesini yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
