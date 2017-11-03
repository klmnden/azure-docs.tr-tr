---
title: "Kullanıcıları yetkilendirmek için Ambari görünümleri - Azure Hdınsight | Microsoft Docs"
description: "Etki alanına katılmış Hdınsight kümeleri Ambari kullanıcı ve grup izinlerini yönetmek nasıl."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: ad9aa6aee0a9f6407da6e9f45df71f8feb8b1500
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authorize-users-for-ambari-views"></a>Ambari görünümleri için Kullanıcıları yetkilendirmek

[Etki alanına katılmış Hdınsight kümeleri](hdinsight-domain-joined-introduction.md) Azure Active Directory tabanlı kimlik doğrulaması dahil Kurumsal düzeyde özellikler sunar. Yeni kullanıcılar eşitleyebilirsiniz
<!-- [synchronize new users](hdinsight-sync-aad-users-to-cluster.md) --> added to Azure AD groups that have been provided access to the cluster, allowing those specific users to perform certain actions. Currently, working with users, groups, and permissions in Ambari is only supported when using a domain-joined HDInsight cluster.

Active Directory kullanıcıları kendi etki alanı kimlik bilgilerini kullanarak küme düğümlerine oturum açabilir. Küme etkileşimleri ton, Ambari görünümleri, ODBC, JDBC, PowerShell ve REST API'leri gibi diğer onaylı uç noktaları ile kimlik doğrulaması yapmak için etki alanı kimlik bilgilerini de kullanabilirsiniz.

> [!WARNING]
> Linux tabanlı Hdınsight kümenizdeki Ambari izleme (hdinsightwatchdog) parolasını değiştirmeyin. Parola değiştirme betik eylemleri kullanın veya kümeniz ile ölçeklendirme işlemleri olanağı keser.

Zaten yapmadıysanız, izleyin [bu yönergeleri](hdinsight-domain-joined-configure.md) yeni bir etki alanına katılmış kümesi sağlamak için.

## <a name="access-the-ambari-management-page"></a>Erişim Ambari Yönetim sayfası

Almak için **Ambari Yönetim sayfasında** üzerinde [Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md), Gözat  **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Küme Yönetici kullanıcı adı ve küme oluştururken tanımladığınız parolayı girin. Ardından, Ambari panodan seçin **yönetmek Ambari** altında **yönetici** menüsü:

![Ambari yönetme](./media/hdinsight-authorize-users-to-ambari/manage-ambari.png)

## <a name="grant-permissions-to-hive-views"></a>Hive görünümleri izinleri

Ambari görünüm örnekleriyle Hive ve Tez, diğerleriyle birlikte gelir. Bir veya daha fazla Hive görünümü örneği erişim vermek için Git **Ambari Yönetim sayfasında**.

1. Yönetim sayfasında seçin **görünümleri** altında bağlantı **görünümleri** sol menü başlığı.

    ![Görünümleri bağlantı](./media/hdinsight-authorize-users-to-ambari/views-link.png)

2. Görünümleri sayfasında genişletin **HIVE** satır. Hive hizmet kümeye eklendiğinde, oluşturulan bir varsayılan Hive görünüm yok. Gerektiğinde daha fazla Hive görünümü örnekleri de oluşturabilirsiniz. Bir Hive görünümü seçin:

    ![Görünümleri - Hive görünümü](./media/hdinsight-authorize-users-to-ambari/views-hive-view.png)

3. View sayfasının en altına doğru kaydırın. Altında *izinleri* bölümünde, izinlerini görüntülemek için etki alanı kullanıcıları verme için iki seçenek vardır:

**Bu kullanıcılara izin vermek** ![bu kullanıcılara izin ver](./media/hdinsight-authorize-users-to-ambari/add-user-to-view.png)

**İzni bu gruplara** ![bu gruplara izin verin](./media/hdinsight-authorize-users-to-ambari/add-group-to-view.png)

4. Bir kullanıcı eklemek için seçin **Kullanıcı Ekle** düğmesi.

    * Kullanıcı adı ve yazarak başlangıç önceden tanımlanmış adları açılan listesini görürsünüz.

    ![Kullanıcı autocompletes](./media/hdinsight-authorize-users-to-ambari/user-autocomplete.png)

    * Seçin veya son yazarak, kullanıcı adı. Bu kullanıcı adı yeni bir kullanıcı eklemek için seçin **yeni** düğmesi.

    * Değişikliklerinizi kaydetmek için seçin **mavi onay kutusu**.

    ![Girilen kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-entered.png)

5. Bir grup eklemek için seçin **Grup Ekle** düğmesi.

    * Grup adını yazmaya başlayın. Var olan bir grup adını seçmek veya yeni bir grup ekleme işlemi kullanıcı ekleme aynıdır.
    * Değişikliklerinizi kaydetmek için seçin **mavi onay kutusu**.

    ![Girilen grubu](./media/hdinsight-authorize-users-to-ambari/group-entered.png)

Bu görünümü kullanmak için bir kullanıcı için izinleri atamak istiyor, ancak ek izinlere sahip bir grubun üyesi olacak şekilde istemediğinizden doğrudan bir görünüm için kullanıcı ekleme yararlıdır. Yönetim yükünü azaltmak için gruplarına izinler atamak daha basit olabilir.

## <a name="grant-permissions-to-tez-views"></a>Tez görünümleri izinleri

Tez görünüm örneklerini izlemek ve Hive sorguları ve Pig betikleri tarafından gönderilen tüm Tez işlerinde hata ayıklamak kullanıcıların izin verin. Küme sağlandığında oluşturan bir varsayılan Tez görünümü örneği yoktur.

Kullanıcılar ve gruplar Tez görünümü örneğine atamak için Genişlet **TEZ** daha önce açıklandığı gibi görünümleri sayfasında satır.

![Görünümleri - Tez görünümü](./media/hdinsight-authorize-users-to-ambari/views-tez-view.png)

Kullanıcılar veya gruplar eklemek için 3-5 önceki bölümdeki adımları yineleyin.

## <a name="assign-users-to-roles"></a>Kullanıcıları rollere atama

Kullanıcılar ve gruplar, erişim izinleri azalan sırada listelenen için beş güvenlik rolleri vardır:

* Küme Yöneticisi
* Küme işleci
* Hizmet Yöneticisi
* Hizmet işleci
* Küme kullanıcı

Rolleri yönetmek için şu adrese gidin **Ambari Yönetim sayfasında**seçeneğini belirleyip **rolleri** içinde bağlantı *kümeleri* sol menü grubu.

![Rolleri menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-link.png)

Her rol için verilen izinleri listesini görmek için mavi bir soru işareti üzerinde yanındaki tıklayın **rolleri** rolleri sayfasında tablo üstbilgisi.

![Rolleri menü bağlantısı](./media/hdinsight-authorize-users-to-ambari/roles-permissions.png)

Bu sayfada, kullanıcılar ve gruplar için rolleri yönetmek için kullanabileceğiniz iki farklı görünümleri vardır: blok ve listesi.

### <a name="block-view"></a>Blok görünümü

Blok görünümü her rol kendi satırında görüntüler ve sağlar **bu kullanıcılara roller atama** ve **bu gruplara Rolleri Ata** daha önce açıklandığı gibi seçenekleri.

![Rolleri görünümünü engelleme](./media/hdinsight-authorize-users-to-ambari/roles-block-view.png)

### <a name="list-view"></a>Liste Görünümü

Liste Görünümü iki kategoride hızlı düzenleme yetenekleri sağlar: kullanıcılar ve gruplar.

* Liste Görünümü kullanıcılar kategorisini açılan listede her kullanıcı için bir rol seçin sağlayan tüm kullanıcıların listesini görüntüler.

    ![Liste Görünümü - Kullanıcı rolleri](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

* Liste görünümüne Grup kategorisini tüm gruplarının ve her grup için atanan rolü görüntüler. Bizim örneğimizde, grupları listesi belirtilen Azure AD grupları eşitlenen **erişim kullanıcı grubu** kümenin etki alanı ayarları özelliği. Bkz: [oluşturma Hdınsight kümesi](hdinsight-domain-joined-configure.md#create-hdinsight-cluster).

    ![Rolleri liste görünümü - grupları](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    Yukarıdaki resimde "hiveusers" grubu atanır *küme kullanıcı* rol. Bu, bu grubun görüntüleyebilir, ancak hizmet yapılandırması ve küme ölçümlerini değiştirmemeniz kullanıcıların sağlayan salt okunur bir roldür.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Ambari için salt okunur bir kullanıcı olarak oturum açmak

Biz bizim Azure AD etki alanı kullanıcı "hiveuser1" Hive ve Tez görünümlerine atanan izinlere sahip olmanız. Ambari Web kullanıcı arabirimini Başlat ve bu kullanıcının etki alanı kimlik bilgileri (e-posta biçimi ve parolayı Azure AD kullanıcı adı) girin, kullanıcı Ambari görünümleri sayfasına yönlendirilir. Buradan, kullanıcının herhangi bir erişilebilir görünüm seçebilirsiniz. Kullanıcı sitesinin Pano, hizmetleri, konaklar, uyarıları veya yönetici sayfalar dahil olmak üzere herhangi bir bölümünü ziyaret edemiyor.

![Kullanıcı yalnızca görünümlerle](./media/hdinsight-authorize-users-to-ambari/user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Ambari için bir küme kullanıcı olarak oturum açın

Azure AD etki alanı kullanıcı "hiveuser2" biz atadığınız *küme kullanıcı* rol. Bu Pano ve tüm menü öğelerini erişebilen rolüdür. Bir küme kullanıcı bir yönetici'den daha az izin verilen seçeneğe sahiptir. Örneğin, hiveuser2 hizmetlerinin her biri için yapılandırmaları görüntüleyebilirsiniz, ancak bunları düzenleyemezsiniz.

![Küme kullanıcı rolüne sahip kullanıcı](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Etki alanına katılmış Hdınsight'ta Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md)
* [Etki alanına katılmış Hdınsight kümelerini yönetme](hdinsight-domain-joined-manage.md)
* [Hdınsight'ta Hadoop ile Hive görünümünü kullanın](hdinsight-hadoop-use-hive-ambari-view.md)

<!-- * [Synchronize Azure AD users to the cluster](hdinsight-sync-aad-users-to-cluster.md) -->
