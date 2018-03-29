---
title: Etki alanına katılmış Hdınsight kümelerini - Azure yönetme | Microsoft Docs
description: Etki alanına katılmış Hdınsight kümeleri yönetmeyi öğrenin
services: hdinsight
documentationcenter: ''
author: bprakash
manager: jhubbard
editor: cgronlun
tags: ''
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2018
ms.author: bhanupr
ms.openlocfilehash: 44202541557a7513e0068f52289a637f6e48f43f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Etki alanına katılmış Hdınsight kümelerini yönetme
Kullanıcılar ve roller etki alanına katılmış ve etki alanına katılmış Hdınsight kümelerini yönetme konusunda bilgi edinin.

## <a name="access-the-clusters-with-enterprise-security-package"></a>Kurumsal güvenlik paketi kümeleriyle erişin.

Kurumsal güvenlik (daha önce Hdınsight Premium olarak da bilinir) paketi burada kimlik doğrulaması Active Directory ve yetkilendirme Apache bırakabilmenizi ve depolama ACL'ler (ADLS ACL'ler) tarafından gerçekleştirilir küme, çok kullanıcılı erişim sağlar. Yetkilendirme birden çok kullanıcı arasında güvenli sınırları sağlar ve yalnızca ayrıcalıklı kullanıcıların yetkilendirme ilkelerine bağlı olarak veri erişimi sağlar.

Güvenlik ve kullanıcı yalıtımı Kurumsal güvenlik paketi ile bir Hdınsight kümesi için önemlidir. Bu gereksinimleri karşılamak üzere Kurumsal güvenlik paketi kümeyle SSH erişimi engellenir. Aşağıdaki tabloda, her küme türü için önerilen erişim yöntemleri gösterilmektedir:

|İş yükü|Senaryo|Erişim yöntemi|
|--------|--------|-------------|
|Hadoop|Hive – etkileşimli işleri/sorgular |<ul><li>[Beeline](#beeline)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Etkileşimli işleri/sorguları, PySpark etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Livy ile Zeppelin](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Batch senaryoları – Spark gönderme, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Etkileşimli sorgu (LLAP)|Etkileşimli|<ul><li>[Beeline](#beeline)</li><li>[Hive görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Visual Studio Araçları](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Herhangi biri|Özel bir uygulama yükleyin|<ul><li>[Betik eylemleri](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|


Standart API'lerini kullanarak, güvenlik açısından yardımcı olur. Ayrıca, aşağıdaki faydaları alın:

1.  **Yönetim** – kodunuzu yönetmek ve standart API'lerini kullanarak işleri otomatikleştirmek – Livy vb. HS2.
2.  **Denetim** – SSH ile denetlemek için hangi kullanıcıların SSH yolu yoktur kümeye vardı. Kullanıcı bağlamında yürütülmesi gibi işleri standart uç noktaları oluşturulan olduğunda bu durum olmayacaktır. 



### <a name="beeline"></a>Beeline kullanın 
Beeline makinenize, yükleyin ve ortak internet üzerinden bağlanma, aşağıdaki parametreleri kullanın: 

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Yerel olarak yüklenmiş Beeline varsa ve bir Azure sanal ağ üzerinden bağlanma, aşağıdaki parametreleri kullanın: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Bir headnode tam etki alanı adını bulmak için yönetmek Ambari REST API belge kullanarak Hdınsight'ta bilgileri kullanın.














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Kullanıcıları etki alanına katılmış Hdınsight kümeleri
Küme oluşturma sırasında oluşturulan iki kullanıcı hesapları olmayan etki alanına katılmış bir Hdınsight kümesi vardır:

* **Ambari yönetici**: Bu hesap olarak da bilinen, *Hadoop kullanıcı* veya *HTTP kullanıcı*. Bu hesap için Ambari https:// sırasında oturum açmak için kullanılan&lt;clustername >. azurehdinsight.net. Ayrıca, Ambari görünümleri sorguları çalıştırmak için harici araçlar (örneğin, PowerShell, Templeton, Visual Studio) aracılığıyla işleri çalıştırıp BI Araçları (örneğin, Excel, Powerbı veya Tableau) ve Hive ODBC sürücüsü ile kimlik doğrulaması için de kullanılabilir.

Bir etki alanına katılmış Hdınsight kümesi Ambari yönetici yanı sıra üç yeni kullanıcılar sahiptir

* **Bırakabilmenizi yönetici**: Bu hesap yerel Apache bırakabilmenizi yönetici hesabıdır. Bir active directory etki alanı kullanıcısı değil. Bu hesap ilkeleri kurulumu ve diğer kullanıcıların yöneticileri veya temsilci olarak atanan Yöneticiler (kullanıcılarla ilkeleri yönetebilmeniz için) yapmak için kullanılabilir. Varsayılan olarak, kullanıcı adı olan *yönetici* ve parola Ambari yönetici parolası ile aynıdır. Parola bırakabilmenizi Ayarları sayfasından güncelleştirilebilir.
* **Küme Yöneticisi etki alanı kullanıcısı**: Ambari ve bırakabilmenizi gibi Hadoop Küme Yöneticisi olarak atanmış bir active directory etki alanı kullanıcısı bu hesabıdır. Küme oluşturma sırasında bu kullanıcının kimlik bilgilerini sağlamanız gerekir. Bu kullanıcı aşağıdaki ayrıcalıklara sahiptir:

  * Makine etki alanına ve küme oluşturma sırasında belirttiğiniz OU içinde yerleştirin.
  * Küme oluşturma sırasında belirttiğiniz OU içinde hizmet asıl adı oluşturun.
  * Geriye doğru DNS girdilerini oluşturun.

    Diğer AD kullanıcılar da bu ayrıcalıklarına sahip unutmayın.

    Bırakabilmenizi tarafından yönetilmeyen ve bu nedenle güvenli olmayan bazı bitiş noktaları (örneğin, Templeton) kümedeki vardır. Bu uç noktaları, küme yönetim etki alanı kullanıcısı dışındaki tüm kullanıcılar için kilitlendiğini.
* **Normal**: küme oluşturma sırasında birden çok active directory grupları sağlayabilir. Bu gruplardaki kullanıcıların bırakabilmenizi ve Ambari eşitlenir. Bu kullanıcılar etki alanı kullanıcıları ve yalnızca bırakabilmenizi yönetilen uç noktaları (örneğin, Hiveserver2) erişimi. RBAC ilkeleri ve bu kullanıcılara uygulanabilir denetim sağlar.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Etki alanına katılmış Hdınsight kümeleri rolleri
Etki alanına katılmış Hdınsight aşağıdaki rolleri vardır:

* Küme Yöneticisi
* Küme işleci
* Hizmet Yöneticisi
* Hizmet işleci
* Küme kullanıcı

**Bu rolleri izinleri görmek için**

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [ambarı Yönetimi kullanıcı arabirimini açın](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. İzinleri görmek üzere mavi soru işareti tıklatın:

    ![Etki alanına katılmış Hdınsight rolleri izinleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Ambari Yönetimi kullanıcı arabirimini açın

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Hdınsight kümenizi açın. Bkz: [listesi ve Göster kümeleri](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Tıklatın **Pano** Ambari'yi açmak için üstteki menüden.
4. Ambari için Küme Yöneticisi etki alanı kullanıcı adı ve parola kullanarak oturum açın.
5. Tıklatın **yönetici** üst açılır menüsünden sağ köşesindeki ve ardından **yönetmek Ambari**.

    ![Etki alanına katılmış Hdınsight Ambari yönetme](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Kullanıcı arabirimini şuna benzer:

    ![Etki alanına katılmış Hdınsight Ambari yönetim kullanıcı Arabirimi](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı kullanıcıları listele
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [ambarı Yönetimi kullanıcı arabirimini açın](#open-the-ambari-management-ui).
2. Sol menüden **kullanıcılar**. Hdınsight kümesine Active Directory'nizden eşitlenen tüm kullanıcılar göreceksiniz.

    ![Etki alanına katılmış Hdınsight Ambari yönetim kullanıcı Arabirimi kullanıcıları listele](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Active Directory'nizden eşitlenen etki alanı grupları listesi
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [ambarı Yönetimi kullanıcı arabirimini açın](#open-the-ambari-management-ui).
2. Sol menüden **grupları**. Hdınsight kümesine Active Directory'nizden eşitlenen tüm grupları göreceksiniz.

    ![Etki alanına katılmış Hdınsight Ambari yönetim kullanıcı Arabirimi listesi grupları](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive görünümleri izinlerini yapılandırma
1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [ambarı Yönetimi kullanıcı arabirimini açın](#open-the-ambari-management-ui).
2. Sol menüden **görünümleri**.
3. Tıklatın **HIVE** ayrıntılarını görüntülemek için.

    ![Etki alanına katılmış Hdınsight Ambari yönetim kullanıcı Arabirimi Hive görünümleri](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Tıklatın **Hive görünümü** Hive görünümleri yapılandırmak için bağlantı.
5. Ekranı aşağı kaydırarak **izinleri** bölümü.

    ![Etki alanına katılmış Hdınsight ambarı Yönetimi UI Hive görünümleri izinleri yapılandırma](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Tıklatın **Kullanıcı Ekle** veya **Grup Ekle**ve ardından kullanıcılar veya Hive görünümleri grupları belirtin.

## <a name="configure-users-for-the-roles"></a>Kullanıcı rolleri için yapılandırma
 Rolleri ve izinlerini listesini görmek için bkz: [rolleri, etki alanına katılmış Hdınsight kümeleri](#roles-of-domain---joined-hdinsight-clusters).

1. Ambari Yönetimi kullanıcı arabirimini açın.  Bkz: [ambarı Yönetimi kullanıcı arabirimini açın](#open-the-ambari-management-ui).
2. Sol menüden **rolleri**.
3. Tıklatın **Kullanıcı Ekle** veya **Grup Ekle** kullanıcılar ve gruplar farklı rollere atamak için.

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış HDInsight kümesini yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
