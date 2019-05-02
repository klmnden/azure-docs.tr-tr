---
title: İzleme ve Azure HDInsight'ın Ambari Web kullanıcı arabirimini kullanarak yönetme
description: Linux tabanlı HDInsight kümelerini yönetmek ve izlemek için Ambari kullanmayı öğrenin. Bu belgede, dahil olan HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 1659ab72620b6bf91eb932f8414a0f6600350e37
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714477"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-web-ui"></a>Apache Ambari Web kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari, yönetim ve bir kolayca web UI ve REST API'si kullanma sağlayarak bir Apache Hadoop kümesini izleme basitleştirir. Ambari, Linux tabanlı HDInsight kümelerine dahil ve küme izleme ve yapılandırma değişiklikleri yapmak için kullanılır.

Bu belgede, Ambari Web kullanıcı arabirimini bir HDInsight kümesi ile kullanma konusunda bilgi edinin.

## <a id="whatis"></a>Apache Ambari nedir?

[Apache Ambari](https://ambari.apache.org) bir kolayca kullanıma web kullanıcı Arabirimi sağlayarak Hadoop yönetimini basitleştirir. Ambari, yönetmek ve Hadoop kümeleri izlemek için kullanabilirsiniz. Geliştiriciler tümleştirilebilir yeteneklere uygulamalarına kullanarak [Ambari REST API'lerini](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari Web kullanıcı Arabirimi, varsayılan olarak Linux işletim sistemini HDInsight kümeleri ile sağlanır.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). 

## <a name="connectivity"></a>Bağlantı

Ambari Web kullanıcı arabirimini konumunda HDInsight kümenize kullanılabilir HTTPS://CLUSTERNAME.azurehdinsight.netburada **CLUSTERNAME** kümenizin adıdır.

> [!IMPORTANT]  
> HDInsight üzerinde Ambari bağlanma HTTPS gerektirir. Kimlik doğrulaması için istendiğinde, yönetici hesabı adını ve kümeyi oluştururken belirttiğiniz parolayı kullanın.

## <a name="ssh-tunnel-proxy"></a>SSH tüneli (proxy)

Kümeniz için Ambari doğrudan Internet üzerinden erişilebilir olsa da, bazı bağlantılar Ambari Web kullanıcı arabirimini (gibi Jobtracker'a) internet'te sunulmaz. Bu hizmetlere erişmek için bir SSH tüneli oluşturmanız gerekir. Daha fazla bilgi için [SSH tünel oluşturmayı kullanma HDInsight ile](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari Web UI

> [!WARNING]  
> Ambari Web kullanıcı arabirimi tüm özellikleri, HDInsight üzerinde desteklenir. Daha fazla bilgi için [desteklenmeyen işlem](#unsupported-operations) bu belgenin bölüm.

Ambari Web kullanıcı Arabirimine bağlanırken sayfasına kimlik doğrulaması istenir. Kümenin yönetici kullanıcı (varsayılan Yönetici) ve küme oluşturma sırasında kullandığınız parolayı kullanın.

Sayfa açıldığında, üst çubuk unutmayın. Bu çubuk, aşağıdaki bilgileri ve denetimler içerir:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari logosu** -kümesini izlemek için kullanılan Pano açılır.

* **Küme adı # ops** -Ambari devam eden işlemlerin sayısını görüntüler. Küme adını seçerek veya **# ops** arka plan işlemleri listesini görüntüler.

* **# Uyarıları** -görüntüler uyarı veya kritik uyarı varsa küme.

* **Pano** -panoyu görüntüler.

* **Hizmetleri** -kümedeki hizmetlerin bilgilerine ve yapılandırma ayarları.

* **Konaklar** -küme içindeki düğümler için bilgileri ve yapılandırma ayarları.

* **Uyarılar** -günlük bilgi, uyarı ve kritik uyarılar.

* **Yönetici** -küme, hizmet hesabı bilgilerini ve Kerberos güvenlik yüklü yazılım yığını/Hizmetleri.

* **Yönetici düğmesi** -Ambari yönetimi, kullanıcı ayarlarını ve oturum kapatma.

## <a name="monitoring"></a>İzleme

### <a name="alerts"></a>Uyarılar

Aşağıdaki liste, Ambari tarafından kullanılan ortak uyarı durumları içerir:

* **TAMAM**
* **Uyarı**
* **KRİTİK**
* **BİLİNMİYOR**

Dışında uyarıları **Tamam** neden **# uyarıları** uyarıların sayısını görüntülemek için sayfanın üst kısmındaki girişi. Bu girişi seçerseniz, uyarıları ve durumlarını görüntüler.

Uyarıları görüntülenebilir birkaç varsayılan gruplar halinde düzenlenir **uyarılar** sayfası.

![Uyarılar sayfası](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Grupları kullanarak yönetebileceğiniz **eylemleri** menü ve seçerek **uyarı grupları yönet**.

![Uyarı gruplar iletişim yönetme](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Ayrıca, uyarı yöntemleri yönetebilir ve uyarı bildirimleri oluşturma **eylemleri** menüsünü seçerek __Uyarı bildirimlerini yönetme__. Geçerli herhangi bir bildirim görüntülenir. Burada bildirimleri de oluşturabilirsiniz. Bildirimleri aracılığıyla gönderilebilir **e-posta** veya **SNMP** belirli uyarı/önem derecesine bileşimleri olduğunda. Örneğin, tüm uyarıları, bir e-posta iletisi gönderebilirsiniz **YARN varsayılan** grup kümesine **kritik**.

![Uyarı iletişim kutusu oluşturma](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Son olarak, seçerek __uyarı ayarlarını Yönet__ gelen __eylemleri__ menü bildirim gönderilmeden önce bir uyarı nkarakterler sayısını ayarlamanızı sağlar. Bu ayar, geçici hatalar için bildirimleri önlemek için kullanılabilir.

### <a name="cluster"></a>Küme

**Ölçümleri** Pano sekmesi bir bakışta, küme durumunu izlemek kolaylaştıran pencere öğeleri bir dizi içerir. Çeşitli pencere öğeleri gibi **CPU kullanımı**, tıklandığında ek bilgileri sağlayın.

![ölçümleri içeren Pano](./media/hdinsight-hadoop-manage-ambari/metrics.png)

**Isı haritaları** sekmesi ölçümleri yeşilden kırmızıya giderek renkli ısı haritaları olarak görüntüler.

![Isı haritaları içeren Pano](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Küme içindeki düğümler hakkında daha fazla bilgi için seçin **konakları**. Sonra ilgilendiğiniz belirli bir düğüm seçin.

![ana bilgisayar ayrıntıları](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Hizmetler

**Hizmetleri** Panoda kenar küme üzerinde çalışan hizmetlerin durumunu ilişkin hızlı Öngörüler sağlar. Çeşitli simge durumu veya alınması gereken eylemleri belirtmek için kullanılır. Örneğin, bir hizmeti geri dönüştürülecek gerekiyorsa bir sarı geri dönüşüm simgesi görüntülenir.

![Hizmetleri kenar çubuğu](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]  
> Görüntülenen Hizmetleri, HDInsight küme türleri ve sürümleri arasında farklılık gösterir. Burada görüntülenen Hizmetleri kümeniz için görüntülenen hizmetler farklı olabilir.

Bir hizmeti seçtiğinizde, hizmette daha ayrıntılı bilgileri görüntüler.

![Hizmet özet bilgileri](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Hızlı bağlantılar

Bazı hizmetler görünen bir **hızlı bağlantılar** sayfanın üstündeki bağlantısı. Bu, hizmete özgü web kullanıcı arabirimleri, gibi erişmek için kullanılabilir:

* **İş Geçmişi** -MapReduce iş geçmişi.
* **Resource Manager** - YARN ResourceManager UI.
* **NameNode** -Hadoop dağıtılmış dosya sistemi (HDFS) NameNode kullanıcı Arabirimi.
* **Oozie Web kullanıcı Arabirimi** -Oozie kullanıcı Arabirimi.

Bu bağlantılardan birini seçerek seçili sayfasını görüntüler, tarayıcınızda yeni bir sekmede açılır.

> [!NOTE]  
> Seçme **hızlı bağlantılar** girişi için bir hizmet, "Sunucu bulunamadı" hatası döndürebilir. Bu hatayla karşılaşırsanız, kullanırken bir SSH tüneli kullanmalısınız **hızlı bağlantılar** bu hizmet için giriş. Bilgi için [SSH tünel oluşturmayı kullanma HDInsight ile](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Yönetim

### <a name="ambari-users-groups-and-permissions"></a>Ambari kullanıcıları, grupları ve izinleri

Kullanıcıları, grupları ve izinleri ile çalışma kullanırken desteklenen bir [etki alanına katılmış](./domain-joined/apache-domain-joined-introduction.md) HDInsight kümesi. Etki alanına katılmış bir küme üzerinde yönetim Ambari UI'ı kullanma hakkında daha fazla bilgi için bkz. [etki alanına katılmış HDInsight kümelerini yönetme](./domain-joined/apache-domain-joined-introduction.md).

> [!WARNING]  
> Linux tabanlı HDInsight kümenizdeki Ambari bekçi (hdinsightwatchdog) parolasını değiştirmeyin. Parola değiştirme betik eylemlerini kullanın veya kümenizle ölçeklendirme işlemleri gerçekleştirme olanağı keser.

### <a name="hosts"></a>Ana bilgisayarlar

**Konakları** sayfası, kümedeki tüm Konaklara listeler. Ana bilgisayarları yönetmek için aşağıdaki adımları izleyin.

![ana sayfa](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]  
> Ekleme, kullanımdan kaldırma ve ana bilgisayar recommissioning HDInsight kümeleri ile kullanılmamalıdır.

1. Yönetmek istediğiniz konağı seçin.

2. Kullanım **eylemleri** menüsünde gerçekleştirmek istediğiniz eylemi seçin:

   * **Tüm bileşenleri başlatın** -konakta tüm bileşenleri başlatın.

   * **Tüm bileşenleri Durdur** -konakta tüm bileşenleri durdur.

   * **Tüm bileşenleri yeniden Başlat** Stop - ve konakta tüm bileşenleri başlatın.

   * **Bakım Modu'nu** -ana bilgisayar için uyarıları bastırır. Uyarıları oluşturan eylemler gerçekleştiriyorsa Bu mod etkinleştirilmelidir. Örneğin, bir Hizmeti'ni durdurma ve başlatma.

   * **Bakım modunu devre dışı bırakmak** -normal uyarmak için ana döndürür.

   * **Durdur** -durakları DataNode veya konaktaki NodeManagers.

   * **Başlangıç** -DataNode başlatır veya konaktaki NodeManagers.

   * **Yeniden** -durur ve konaktaki DataNode veya NodeManagers başlatır.

   * **Yetkisini** -bir ana bilgisayar kümeden kaldırır.

     > [!NOTE]  
     > Bu eylem, HDInsight kümelerinde kullanmayın.

   * **Recommission** -daha önce yetkisi alınmış bir konak kümesine ekler.

     > [!NOTE]  
     > Bu eylem, HDInsight kümelerinde kullanmayın.

### <a id="service"></a>Hizmetleri

Gelen **Pano** veya **Hizmetleri** sayfasında **eylemleri** altındaki tüm hizmetlerini durdurmak ve başlatmak için hizmetler listesinden düğmesi.

![Hizmet eylemleri](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]  
> Sırada **Hizmet Ekle** listelenen bu menüde, hizmetleri, HDInsight kümesine eklemek için kullanılmamalıdır. Küme hazırlama sırasında bir betik eylemi kullanarak yeni hizmetler eklenmesi gerekir. Betik eylemlerini kullanarak daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemlerini kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

Sırada **eylemleri** düğmesi tüm hizmetleri yeniden başlatın, genellikle başlatma, durdurma veya belirli bir hizmeti yeniden istiyor. Bir bireysel hizmet eylemleri gerçekleştirmek için aşağıdaki adımları kullanın:

1. Gelen **Pano** veya **Hizmetleri** sayfasında, bir hizmet seçin.

2. Üst kısmından **özeti** için sekmesinde, kullanın **hizmet eylemleri** düğmesini tıklatın ve gerçekleştirilecek eylemi seçin. Bu, tüm düğümlerde hizmetini yeniden başlatır.

    ![hizmet eylemi](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]  
   > Küme çalışırken bazı hizmetlerin yeniden uyarılar oluşturabilir. Uyarıları önlemek için kullanabileceğiniz **hizmet eylemleri** etkinleştirmek için düğmeye **Bakım modu** hizmeti yeniden başlatmayı gerçekleştirmeden önce.

3. Bir eylem seçildikten sonra **# op** giriş üst arka plan işlemi oluştuğunu göstermek için sayfa artırır. Arka plan işlemleri listesini görüntülemek için yapılandırılmışsa görüntülenir.

   > [!NOTE]  
   > Etkinleştirilirse, **Bakım modu** kullanarak devre dışı bırakmak için hizmete unutmayın **hizmet eylemleri** işlemi tamamlandıktan sonra düğme.

Bir hizmeti yapılandırmak için aşağıdaki adımları kullanın:

1. Gelen **Pano** veya **Hizmetleri** sayfasında, bir hizmet seçin.

2. Seçin **yapılandırmaları** sekmesi. Geçerli yapılandırmayı görüntülenir. Önceki yapılandırmaların listesi de görüntülenir.

    ![Yapılandırmaları](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Yapılandırmayı değiştirebilir ve ardından görüntülenen alanları **Kaydet**. Veya önceki bir yapılandırma seçin ve ardından **geçerli hale** önceki ayarlara geri almak için.

## <a name="ambari-views"></a>Ambari görünümleri

Ambari görünümlerini izin Ambari Web kullanıcı arabirimini kullanarak kullanıcı Arabirimi öğeleri takın geliştiricilerin [Apache Ambari görünümleri Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight Hadoop küme türleri ile aşağıdaki görünümleri sağlar:


* Hive görünümü: Hive görünümü doğrudan web tarayıcınızdan Hive sorguları çalıştırmanıza olanak sağlar. Sorguları kaydedebilir, sonuçları görüntülemek, sonuçları küme depolama alanına kaydedin veya sonuçları, yerel bir sisteme indirme. Hive görünümleri kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile kullanmak Apache Hive görünümleri](hadoop/apache-hadoop-use-hive-ambari-view.md).

* Tez görünümü: Tez görünümü daha iyi anlamanıza ve iyileştirme işleri olanak sağlar. Tez işlerinde nasıl yürütülür ve hangi kaynakların kullanılan bilgileri görüntüleyebilirsiniz.

## <a name="unsupported-operations"></a>Desteklenmeyen işlemleri

HDInsight üzerinde aşağıdaki Ambari işlemleri desteklenmez:

* __Ölçümleri Toplayıcı hizmetinin taşıma__. Ölçümleri Toplayıcısı hizmeti bilgileri görüntülerken, hizmet Eylemler menüsünden kullanılabilir eylemler biri olan __taşıma ölçümleri Toplayıcı__. Bu, HDInsight ile desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl kullanacağınızı öğrenin [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) HDInsight ile.
