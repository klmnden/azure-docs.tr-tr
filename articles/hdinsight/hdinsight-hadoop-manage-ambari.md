---
title: "İzleme ve Azure Hdınsight Ambari Web kullanıcı arabirimini kullanarak yönetme | Microsoft Docs"
description: "Linux tabanlı Hdınsight kümelerini yönetmek ve izlemek için Ambari kullanmayı öğrenin. Bu belgede, Ambari Web kullanıcı arabirimini Hdınsight kümeleri ile dahil kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2017
ms.author: larryfr
ms.openlocfilehash: ec6e6d07b0933504ffee17912aac9ee3ef937688
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a>Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari, yönetim ve kolay bir web kullanıcı Arabirimi ve REST API kullanmayı sağlayarak Hadoop kümesi izleme basitleştirir. Ambari, Linux tabanlı Hdınsight kümelerine dahil ve kümeyi izlemek ve yapılandırma değişiklikleri yapmak için kullanılır.

Bu belgede, Ambari Web kullanıcı arabirimini bir Hdınsight kümesini kullanmayı öğrenin.

## <a id="whatis"></a>Ambari nedir?

[Apache Ambari](http://ambari.apache.org) kullanımı kolay web kullanıcı Arabirimi sağlayarak Hadoop yönetimini basitleştirir. Ambari kullanabileceğiniz oluşturmak, yönetmek ve Hadoop kümelerini izleme. Geliştiriciler tümleştirebilir Bu yetenekler uygulamalarına kullanarak [Ambari REST API'leri](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari Web kullanıcı Arabirimi, varsayılan olarak Linux işletim sistemini Hdınsight kümeleri ile sağlanır.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). 

## <a name="connectivity"></a>Bağlantı

Ambari Web kullanıcı arabirimini HTTPS://CLUSTERNAME.azurehdidnsight.net, konumunda Hdınsight kümenize kullanılabilir olduğu **CLUSTERNAME** kümenizin adıdır.

> [!IMPORTANT]
> Hdınsight üzerinde Ambari bağlanma HTTPS gerektirir. Kimlik doğrulaması için istendiğinde, Yönetici hesap adı ve küme oluştururken verdiğiniz parolası'nı kullanın.

## <a name="ssh-tunnel-proxy"></a>SSH tüneli (proxy)

Kümeniz için Ambari doğrudan Internet üzerinden erişilebilir olsa da, bazı bağlantılar Ambari Web kullanıcı arabirimini (Jobtracker'a gibi) internet'te sunulmaz. Bu hizmetlere erişmek için bir SSH tüneli oluşturmanız gerekir. Daha fazla bilgi için bkz: [kullanım SSH tünel Hdınsight ile](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari Web kullanıcı Arabirimi

> [!WARNING]
> Ambari Web kullanıcı arabirimini tüm özelliklerini Hdınsight üzerinde desteklenir. Daha fazla bilgi için bkz: [desteklenmeyen işlemleri](#unsupported-operations) bu belgenin bölüm.

Ambari Web kullanıcı Arabirimi için bağlanırken sayfasına kimlik doğrulaması yapmak istenir. Küme oluşturma sırasında kullanılan parola ve küme yönetici kullanıcı (varsayılan Yönetici) kullanın.

Sayfayı açtığında çubuğunun üstündeki unutmayın. Bu çubuğu, aşağıdaki bilgileri ve denetimleri içerir:

![ambari nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari logosu** -küme izlemek için kullanılan Pano açar.

* **Küme adı # ops** -devam eden ambarı işlemler sayısını görüntüler. Küme adı seçerek veya **# ops** arka plan işlemleri listesini görüntüler.

* **# Uyarıları** -görüntüler uyarı veya kritik uyarılar, küme.

* **Pano** -Pano görüntüler.

* **Hizmetleri** -kümedeki hizmetleri için bilgi ve yapılandırma ayarları.

* **Ana bilgisayar** -kümedeki düğümler için bilgileri ve yapılandırma ayarları.

* **Uyarıları** -günlük bilgi, uyarı ve kritik uyarılar.

* **Yönetici** -küme, hizmet hesabı bilgilerini ve Kerberos güvenlik yüklü yazılım yığını/Hizmetleri.

* **Yönetici düğmesi** -ambarı yönetimi, kullanıcı ayarlarını ve oturum kapatma.

## <a name="monitoring"></a>İzleme

### <a name="alerts"></a>Uyarılar

Aşağıdaki liste, Ambari tarafından kullanılan ortak uyarı durumlarını içerir:

* **TAMAM**
* **Uyarı**
* **KRİTİK**
* **BİLİNMEYEN**

Dışındaki uyarıları **Tamam** neden **# uyarıları** uyarı sayısını görüntülemek için sayfanın üstündeki girişi. Bu girişi seçerseniz, uyarıları ve bunların durumunu görüntüler.

Uyarıları görüntülenebilir birkaç varsayılan gruplar halinde düzenlenir **uyarıları** sayfası.

![Uyarılar sayfası](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Grupları kullanarak yönetebileceğiniz **Eylemler** menü ve seçerek **uyarı grupları yönet**.

![Uyarı gruplar iletişim yönetme](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Ayrıca, uyarı yöntemleri yönetmek ve uyarı bildirimleri oluşturma **Eylemler** seçerek menü __yönetmek uyarı bildirimleri__. Geçerli herhangi bir bildirim görüntülenir. Ayrıca, buradan bildirimler de oluşturabilirsiniz. Bildirimler, üzerinden gönderilebilir **e-posta** veya **SNMP** belirli uyarı önem birleşimleri olduğunda. Örneğin, bir e-posta uyarıları biri gönderebilirsiniz **YARN varsayılan** Grup ayarlanmış **kritik**.

![Uyarı iletişim kutusu oluşturma](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Son olarak, seçme __uyarı ayarlarını Yönet__ gelen __Eylemler__ menüsü bir bildirim gönderilmeden önce bir uyarı ortaya gerekir sayısı ayarlamanıza olanak sağlar. Bu ayar, bildirimleri geçici hataları önlemek için kullanılabilir.

### <a name="cluster"></a>Küme

**Ölçümleri** Pano sekmesi kolaylaştıran bir bakışta, küme durumunu izlemek pencere öğeleri, bir dizi içerir. Birkaç pencere öğeleri gibi **CPU kullanımı**, tıklatıldığında ek bilgileri sağlayın.

![Pano ölçümlerle](./media/hdinsight-hadoop-manage-ambari/metrics.png)

**Heatmaps** sekmesi ölçümleri yeşilden kırmızı giderek renkli heatmaps olarak görüntüler.

![Pano heatmaps ile](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Küme içindeki düğümler üzerinde daha fazla bilgi için seçin **ana**. Daha sonra ilgilendiğiniz belirli düğümünü seçin.

![ana bilgisayar ayrıntıları](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Hizmetler

**Hizmetleri** kenar Panoda küme üzerinde çalışan hizmetleri durumunu hızlı fikir sağlar. Çeşitli simgeleri durumu veya alınması gereken eylemleri belirtmek için kullanılır. Örneğin, bir hizmet geri dönüştürülmesi gerekiyorsa sarı geri dönüşüm simgesi görüntülenir.

![Hizmetleri yan çubuğu](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> Görüntülenen hizmetler Hdınsight küme türleri ve sürümleri arasında farklılık gösterir. Burada görüntülenen hizmetler, kümeniz için görüntülenen hizmetler farklı olabilir.

Bir hizmet seçmeden hizmette daha ayrıntılı bilgiler görüntüler.

![Hizmet özet bilgileri](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Hızlı bağlantılar

Bazı Hizmetleri görünen bir **hızlı bağlantılar** sayfanın üst kısmındaki bağlantı. Bu hizmete özgü web Uı'lar, gibi erişmek için kullanılabilir:

* **İş Geçmişi** -MapReduce iş geçmişi.
* **Resource Manager** -YARN ResourceManager UI.
* **İş** -Hadoop dağıtılmış dosya sistemi (HDFS) iş UI.
* **Oozie Web kullanıcı Arabirimi** -Oozie UI.

Aşağıdaki bağlantılardan birini seçerek yeni bir sekmede seçilen sayfasını görüntüler, tarayıcınızda açılır.

> [!NOTE]
> Seçme **hızlı bağlantılar** girişi bir hizmet için "Sunucu bulunamadı" hata döndürebilir. Bu hatayla karşılaşırsanız, kullanırken SSH tüneli kullanmalısınız **hızlı bağlantılar** bu hizmet için girişi. Bilgi için bkz: [kullanım SSH tünel Hdınsight ile](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Yönetim

### <a name="ambari-users-groups-and-permissions"></a>Ambari kullanıcılar, gruplar ve izinler

Kullanıcılar, gruplar ve izinler ile çalışma kullanılırken desteklenir bir [etki alanına katılmış](./domain-joined/apache-domain-joined-introduction.md) Hdınsight kümesi. Etki alanına katılmış bir kümede ambarı Yönetimi kullanıcı arabirimini kullanma hakkında daha fazla bilgi için bkz: [etki alanına katılmış Hdınsight kümelerini yönetme](./domain-joined/apache-domain-joined-introduction.md).

> [!WARNING]
> Linux tabanlı Hdınsight kümenizdeki Ambari izleme (hdinsightwatchdog) parolasını değiştirmeyin. Parola değiştirme betik eylemleri kullanın veya kümeniz ile ölçeklendirme işlemleri olanağı keser.

### <a name="hosts"></a>Ana bilgisayarlar

**Ana** sayfası, kümedeki tüm konaklarda listeler. Ana bilgisayarları yönetmek için aşağıdaki adımları izleyin.

![ana sayfa](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Ekleme, yetkisini alma ve bir ana bilgisayar recommissioning Hdınsight kümeleri ile kullanılmamalıdır.

1. Yönetmek istediğiniz konağı seçin.

2. Kullanım **Eylemler** menü gerçekleştirmek istediğiniz eylemi seçin:

   * **Tüm bileşenleri başlatın** -konakta tüm bileşenleri başlatın.

   * **Tüm bileşenleri Durdur** -konakta tüm bileşenleri durdurun.

   * **Tüm bileşenleri yeniden** - Dur ve tüm bileşenleri konakta başlatın.

   * **Bakım modunu açmak** -ana bilgisayar için uyarıları bastırır. Uyarıları oluşturan eylemleri gerçekleştiriyorsanız Bu mod etkinleştirilmelidir. Örneğin, bir Hizmeti'ni durdurma ve başlatma.

   * **Bakım modunu devre dışı bırakmak** -normal uyarmak için ana döndürür.

   * **Durdur** -durakları DataNode veya ana bilgisayarda NodeManagers.

   * **Başlat** -DataNode başlatır veya ana bilgisayarda NodeManagers.

   * **Yeniden** -durur ve ana bilgisayarda DataNode veya NodeManagers başlatır.

   * **Yetkisini** -kümeden bir ana bilgisayarı kaldırır.

     > [!NOTE]
     > Bu eylem, Hdınsight kümelerinde kullanmayın.

   * **Recommission** -önceden yetkisi alınmış bir konak kümesine ekler.

     > [!NOTE]
     > Bu eylem, Hdınsight kümelerinde kullanmayın.

### <a id="service"></a>Hizmetleri

Gelen **Pano** veya **Hizmetleri** sayfasında, kullanmak **Eylemler** tüm hizmetlerini durdurmak ve başlatmak için hizmetler listesinin altındaki düğmesini.

![Hizmet eylemleri](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Sırada **Hizmet Ekle** listelenen bu menüde, bu hizmetleri Hdınsight kümesine eklemek için kullanılmamalıdır. Küme hazırlama sırasında bir betik eylemi kullanarak yeni hizmetlerin eklenmesi gerekir. Betik eylemleri kullanma hakkında daha fazla bilgi için bkz: [özelleştirme Hdınsight kümeleri betik eylemleri kullanılarak](hdinsight-hadoop-customize-cluster-linux.md).

Sırada **Eylemler** düğmesi tüm hizmetleri yeniden başlatın, başlatma, durdurma veya belirli bir hizmeti yeniden başlatmak istediğiniz sıklığı. Bireysel bir hizmet eylemleri gerçekleştirmek için aşağıdaki adımları kullanın:

1. Gelen **Pano** veya **Hizmetleri** sayfasında, bir hizmet seçin.

2. Üstünden **Özet** sekmesinde, kullanmak **hizmet eylemleri** düğmesini tıklatın ve gerçekleştirilecek eylemi seçin. Bu, tüm düğümlerde hizmetini yeniden başlatır.

    ![hizmet eylemi](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Küme çalışırken bazı hizmetleri yeniden uyarılar oluşturabilir. Uyarıları önlemek için kullanabileceğiniz **hizmet eylemleri** etkinleştirmek için düğmeyi **Bakım modu** yeniden gerçekleştirmeden önce hizmeti.

3. Bir eylem seçildikten sonra **# op** arka plan işlemi gerçekleşmekte olduğunu göstermek için sayfa artışlarla üstündeki girişi. Görüntüleyecek şekilde yapılandırılmış olsa arka plan işlemleri listesi görüntülenir.

   > [!NOTE]
   > Etkinleştirmiş olmanız durumunda **Bakım modu** kullanarak devre dışı bırakmak hizmet için unutmayın **hizmet eylemleri** işlemi tamamlandıktan sonra düğmesine tıklayın.

Bir hizmeti yapılandırmak için aşağıdaki adımları kullanın:

1. Gelen **Pano** veya **Hizmetleri** sayfasında, bir hizmet seçin.

2. Seçin **yapılandırmalar** sekmesi. Geçerli yapılandırması görüntülenir. Önceki yapılandırmaların listesi de görüntülenir.

    ![Yapılandırmaları](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Yapılandırmasını değiştirin ve ardından görüntülenen alanları kullanın **kaydetmek**. Veya önceki bir yapılandırma seçin ve ardından **geçerli hale** önceki ayarlarına geri almak için.

## <a name="ambari-views"></a>Ambari görünümleri

Ambari görünümleri izin Ambari Web kullanıcı arabirimini kullanarak kullanıcı Arabirimi öğeleri takın geliştiricilerin [Ambari görünümleri Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). Hdınsight Hadoop küme türü ile aşağıdaki görünümleri sağlar:

* Yarn sıra Yöneticisi: sıra yöneticisini görüntüleme ve değiştirme YARN sıralar için basit bir kullanıcı Arabirimi sağlar.

* Hive görünümü: Hive görünümü doğrudan web tarayıcınızdan Hive sorguları çalıştırmanıza olanak sağlar. Sorguları Kaydet, sonuçları görüntülemek, küme depolama alanına sonuçları kaydetmek veya sonuçları yerel sisteminize indirin. Hive görünümleri kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanım Hive görünümleri](hadoop/apache-hadoop-use-hive-ambari-view.md).

* Tez görünümü: Tez View daha iyi anlamak ve işleri en iyi duruma getirmek sağlar. Tez işlerinde nasıl yürütülür ve hangi kaynaklara kullanılan bilgileri görüntüleyebilirsiniz.

## <a name="unsupported-operations"></a>Desteklenmeyen işlemler

Hdınsight üzerinde aşağıdaki ambarı işlemleri desteklenmez:

* __Ölçümleri Toplayıcı hizmeti taşıma__. Bilgileri ölçümleri Toplayıcısı hizmeti görüntülerken, hizmet eylemleri menüsünden kullanılabilir eylemleri biri __taşıma ölçümleri Toplayıcı__. Bu, Hdınsight ile desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) Hdınsight ile.