---
title: Kullanıcıları Ambari Views - Azure HDInsight için yetkilendirme
description: Etki alanına katılmış HDInsight kümeleri Ambari kullanıcı ve grup izinlerini yönetmek nasıl.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 7734848035eeab3c38a57b9999f8cda77f6a4e24
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107165"
---
# <a name="authorize-users-for-ambari-views"></a>Kullanıcıları Ambari Views için yetkilendirme

[Etki alanına katılmış HDInsight kümeleri](./domain-joined/apache-domain-joined-introduction.md) Azure Active Directory tabanlı kimlik doğrulaması dahil kurumsal sınıf özellikler sunar. Yapabilecekleriniz [yeni kullanıcıları eşitleme](hdinsight-sync-aad-users-to-cluster.md) belirli kullanıcılarla belirli eylemleri gerçekleştirmek izin verme kümesine erişim sağlanan Azure AD gruplarına eklenebilir. Kullanıcılar, gruplar ve izinler Ambari ile çalışma, etki alanına katılmış HDInsight kümesi ve standart HDInsight kümesi için desteklenir.

Active Directory Kullanıcıları küme düğümlerine, etki alanı kimlik bilgilerini kullanarak oturum açabilir. Bunlar ayrıca, etki alanı kimlik küme etkileşim Hue, Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri gibi diğer onaylanmış uç noktaların kimlik doğrulaması için kullanabilirsiniz.

> [!WARNING]
> Linux tabanlı HDInsight kümenizdeki Ambari bekçi (hdinsightwatchdog) parolasını değiştirmeyin. Parola değiştirme betik eylemlerini kullanın veya kümenizle ölçeklendirme işlemleri gerçekleştirme olanağı keser.

Zaten yapmadıysanız, izleyin [bu yönergeleri](./domain-joined/apache-domain-joined-configure.md) yeni bir etki alanına katılmış kümesi sağlamak için.

## <a name="access-the-ambari-management-page"></a>Ambari Yönetim sayfasına erişin

Ulaşmak için **Ambari Yönetim sayfası** üzerinde [Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md), Gözat **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Küme Yöneticisi kullanıcı adı ve kümeyi oluştururken tanımladığınız parola girin. Ardından, Ambari panodan seçin **yönetme Ambari** altında **yönetici** menüsü:

![Ambari ile yönetme](./media/hdinsight-authorize-users-to-ambari/manage-ambari.png)

## <a name="grant-permissions-to-hive-views"></a>Hive görünümleri izinler

Ambari ile örnekleri görüntüle ve Hive Tez, diğerlerinin yanı sıra gelir. Bir veya daha fazla Hive görünümü örneğine erişimi vermek için Git **Ambari Yönetim sayfası**.

1. Yönetim sayfasından seçin **görünümleri** altında bağlantı **görünümleri** Soldaki menü başlığı.

    ![Görünüm bağlantısı](./media/hdinsight-authorize-users-to-ambari/views-link.png)

2. Görünüm sayfasında genişletin **HIVE** satır. Hive hizmet kümeye eklendiğinde, oluşturulan bir varsayılan Hive görünümü yoktur. Gerektiğinde daha fazla Hive görünümü örneği de oluşturabilirsiniz. Bir Hive görünümü seçin:

    ![Görünümleri - Hive görünümü](./media/hdinsight-authorize-users-to-ambari/views-hive-view.png)

3. Görünümü sayfasının en altına doğru kaydırın. Altında *izinleri* bölümünde, etki alanı kullanıcıları görünümüne izinlerini vermek için iki seçenek vardır:

**Bu kullanıcılara izin vermek** ![bu kullanıcılara izin verin](./media/hdinsight-authorize-users-to-ambari/add-user-to-view.png)

**Bu gruplar için izin verme** ![bu gruplara izin verme](./media/hdinsight-authorize-users-to-ambari/add-group-to-view.png)

4. Bir kullanıcı eklemek için seçin **Kullanıcı Ekle** düğmesi.

    * Kullanıcı adı ve yazmaya önceden tanımlanmış adları aşağı açılan listesini görürsünüz.

    ![Kullanıcı yanı](./media/hdinsight-authorize-users-to-ambari/user-autocomplete.png)

    * Seçin veya yazdıktan, kullanıcı adı. Bu kullanıcı adı yeni bir kullanıcı eklemek için seçin **yeni** düğmesi.

    * Değişikliklerinizi kaydetmek için seçmeniz **mavi onay kutusu**.

    ![Girilen kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-entered.png)

5. Bir grup eklemek için seçin **Grup Ekle** düğmesi.

    * Grup adını yazmaya başlayın. Mevcut bir grubu adını seçerek ya da yeni bir grup ekleme işlemini kullanıcı ekleme aynıdır.
    * Değişikliklerinizi kaydetmek için seçmeniz **mavi onay kutusu**.

    ![Girilen grubu](./media/hdinsight-authorize-users-to-ambari/group-entered.png)

Bu görünümü kullanmak için bir kullanıcıya izin atamak istediğiniz, ancak bunları ek izinlere sahip bir grubun bir üyesi olmasını istemediğiniz bir görünüme doğrudan kullanıcı ekleme yararlı olur. Yönetim yükünü azaltmak için gruplarına izinler atamak daha basit olabilir.

## <a name="grant-permissions-to-tez-views"></a>Tez görünümlere izin ver

Örnekleri görüntüle Tez izlemek ve Hive sorguları ve Pig betikleri tarafından gönderilen tüm Tez işlerinin hatalarını ayıklamak kullanıcıların. Küme sağlandığında oluşturan bir varsayılan Tez görünümü örneği yok.

Kullanıcılar ve gruplar Tez görünümü örneğine atamak için genişletme **TEZ** daha önce açıklandığı gibi görünüm sayfasında satır.

![Görünümleri - Tez görüntüle](./media/hdinsight-authorize-users-to-ambari/views-tez-view.png)

Kullanıcı veya grup eklemek için önceki bölümdeki adımları 3-5'i tekrarlayın.

## <a name="assign-users-to-roles"></a>Kullanıcı rollerine atama

Kullanıcılar ve gruplar, erişim izinleri azalan sırayla yeniden listelenmiş için beş güvenlik rolleri vardır:

* Küme Yöneticisi
* Küme işleci
* Hizmet Yöneticisi
* Hizmet işleci
* Küme kullanıcısı

Rolleri yönetmek için Git **Ambari Yönetim sayfası**, ardından **rolleri** içinde bağlantı *kümeleri* Soldaki menü grubu.

![Rolleri menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-link.png)

Her role verilen izinlere listesini görmek için üzerinde mavi bir soru işareti yanındaki tıklayın **rolleri** Tablo üst bilgi roller sayfasında.

![Rolleri menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-permissions.png)

Bu sayfada, kullanıcılar ve gruplar için rolleri yönetmek için kullanabileceğiniz iki farklı görünümleri bulunur: blok ve listesi.

### <a name="block-view"></a>Blok görünümü

Blok görünümü her rol kendi satırında görüntüler ve sağlar **bu kullanıcılara roller atama** ve **bu gruplara rol atama** daha önce açıklandığı gibi seçenekleri.

![Rolleri görünümünü engelleme](./media/hdinsight-authorize-users-to-ambari/roles-block-view.png)

### <a name="list-view"></a>Liste Görünümü

Liste görünümüne iki kategoride hızlı düzenleme özellikleri sağlar: kullanıcılar ve gruplar.

* Kullanıcıların kategorisini liste görünümü açılır listeden bir rol için her bir kullanıcı seçmenize olanak sağlar, tüm kullanıcıların listesini görüntüler.

    ![Liste Görünümü - Kullanıcı rolleri](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

* Tüm grupları ve her gruba atanan rolü liste görünümüne Grup kategorisini görüntüler. Bizim örneğimizde, belirtilen Azure AD gruplarındaki gruplarının listesini eşitlenir **erişim kullanıcı grubu** kümenin etki alanı ayarlarını özelliği. Bkz: [bir etki alanına katılmış HDInsight kümesi oluşturma](/domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-a-domain-joined-hdinsight-cluster).

    ![Liste Görünümü - grupları rolleri](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    Yukarıdaki görüntüde, "hiveusers" grubu olarak atanır *küme kullanıcı* rol. Bu, kullanıcılara görüntüleyebilir, ancak hizmet yapılandırması ve küme ölçümleri değiştirilmemesi için bu grubun salt okunur bir roldür.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Ambari için yalnızca görüntüleme bir kullanıcı olarak oturum açın

Hive ve Tez görünümlerine "hiveuser1 kullanıcısının" izinleri size Azure AD etki alanı kullanıcı atadınız. Ambari Web kullanıcı arabirimini Başlat ve bu kullanıcının etki alanı kimlik bilgilerini (Azure AD kullanıcı adı e-posta biçimini ve parola) girin, kullanıcı Ambari görünümleri sayfasına yönlendirilir. Buradan, kullanıcının herhangi bir erişilebilir görünüm seçebilirsiniz. Kullanıcı sitesinin Pano, hizmetleri, konaklar, uyarılar veya yönetim sayfaları dahil olmak üzere herhangi bir bölümünü ziyaret edin olamaz.

![Kullanıcı yalnızca görünümlerle](./media/hdinsight-authorize-users-to-ambari/user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Ambari için küme kullanıcı olarak oturum açın

Azure AD etki alanı kullanıcı "hiveuser2" biz atadığınız *küme kullanıcı* rol. Bu rol, Pano ve tüm menü öğelerinin erişebilir. Küme kullanıcısı yönetici değerinden daha az izin verilen seçenekleri vardır. Örneğin, hiveuser2 hizmetlerinin her biri için yapılandırmaları görüntüleyebilirsiniz, ancak onları düzenleyemezsiniz.

![Küme kullanıcı rolüne sahip kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* [İçinde etki alanına katılmış HDInsight Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)
* [Etki alanına katılmış HDInsight kümelerini yönetme](./domain-joined/apache-domain-joined-manage.md)
* [HDInsight, Hadoop ile Hive görünümünü kullanma](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Azure AD kullanıcılarının kümeye Eşitle](hdinsight-sync-aad-users-to-cluster.md)
