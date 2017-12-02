---
title: "Etki alanına katılmış Hdınsight kümelerini - Azure yönetme | Microsoft Docs"
description: "Etki alanına katılmış Hdınsight kümeleri yönetmeyi öğrenin"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 0fc32960fc2f1ae69315dbfd6bfb8c34c4adc0fa
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Etki alanına katılmış Hdınsight kümelerini yönetme
Kullanıcılar ve roller etki alanına katılmış ve etki alanına katılmış Hdınsight kümelerini yönetme konusunda bilgi edinin.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Kullanıcıları etki alanına katılmış Hdınsight kümeleri
Küme oluşturma sırasında oluşturulan iki kullanıcı hesapları olmayan etki alanına katılmış bir Hdınsight kümesi vardır:

* **Ambari yönetici**: Bu hesap olarak da bilinen, *Hadoop kullanıcı* veya *HTTP kullanıcı*. Bu hesap için Ambari https:// sırasında oturum açmak için kullanılan&lt;clustername >. azurehdinsight.net. Ayrıca, Ambari görünümleri sorguları çalıştırmak için harici araçlar (yani PowerShell, Templeton, Visual Studio) aracılığıyla işleri çalıştırıp BI Araçları (yani Excel, Powerbı veya Tableau) ve Hive ODBC sürücüsü ile kimlik doğrulaması için de kullanılabilir.
* **SSH kullanıcı**: Bu hesap ile SSH kullanılabilir ve sudo komutları yürütün. Linux VM'ler için kök ayrıcalıklarına sahiptir.

Bir etki alanına katılmış Hdınsight kümesi üç Ambari yönetici yanı sıra yeni kullanıcılar ve SSH kullanıcı sahiptir.

* **Bırakabilmenizi yönetici**: Bu hesap yerel Apache bırakabilmenizi yönetici hesabıdır. Bir active directory etki alanı kullanıcısı değil. Bu hesap ilkeleri kurulumu ve diğer kullanıcıların yöneticileri veya temsilci olarak atanan Yöneticiler (kullanıcılarla ilkeleri yönetebilmeniz için) yapmak için kullanılabilir. Varsayılan olarak, kullanıcı adı olan *yönetici* ve parola Ambari yönetici parolası ile aynıdır. Parola bırakabilmenizi Ayarları sayfasından güncelleştirilebilir.
* **Küme Yöneticisi etki alanı kullanıcısı**: Ambari ve bırakabilmenizi gibi Hadoop Küme Yöneticisi olarak atanmış bir active directory etki alanı kullanıcısı bu hesabıdır. Küme oluşturma sırasında bu kullanıcının kimlik bilgilerini sağlamanız gerekir. Bu kullanıcı aşağıdaki ayrıcalıklara sahiptir:

  * Makine etki alanına ve küme oluşturma sırasında belirttiğiniz OU içinde yerleştirin.
  * Küme oluşturma sırasında belirttiğiniz OU içinde hizmet asıl adı oluşturun.
  * Geriye doğru DNS girdilerini oluşturun.

    Diğer AD kullanıcılar da bu ayrıcalıklarına sahip unutmayın.

    Bırakabilmenizi tarafından yönetilmeyen ve bu nedenle güvenli olmayan bazı bitiş noktaları (örneğin, Templeton) kümedeki vardır. Bu uç noktaları, küme yönetim etki alanı kullanıcısı dışındaki tüm kullanıcılar için kilitlendiğini.
* **Normal**: küme oluşturma sırasında birden çok active directory grupları sağlayabilir. Bu gruplardaki kullanıcıların bırakabilmenizi ve Ambari senkronize edilir. Bu kullanıcılar etki alanı kullanıcıları ve yalnızca bırakabilmenizi yönetilen uç noktaları (örneğin, Hiveserver2) erişebilir. RBAC ilkeleri ve bu kullanıcılara uygulanabilir denetim sağlar.

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
2. Hdınsight kümenize bir dikey pencerede açın. Bkz: [listesi ve Göster kümeleri](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
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
* Etki alanına katılmış Hdınsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
