---
title: Ambari Azure İzleyici ile küme kullanılabilirliği izleme günlükleri
description: Ambari kullanmayı öğrenin ve küme durumunu ve kullanılabilirliğini izlemek için Azure İzleyici günlüğe kaydeder.
keywords: ambari, izleme, log analytics, uyarı, kullanılabilirlik, sistem durumu izleme
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: tyfox
ms.openlocfilehash: 459de569916af14b0efea0ff08b92e5c93ed2369
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64718893"
---
# <a name="how-to-monitor-cluster-availability-with-ambari-and-azure-monitor-logs"></a>Ambari Azure İzleyici ile küme kullanılabilirliği izleme günlükleri

HDInsight kümeleri hem sistem durumu bilgileri bir bakışta ve önceden tanımlanmış uyarılar sunan Apache Ambari, hem de sorgulanabilir ölçümlerini ve günlüklerini yanı sıra, yapılandırılabilir uyarı sağlayan, Azure İzleyici logs tümleştirmesi içerir.

Bu belge, kümenizi izlemek için bu araçları kullanmayı gösterir ve Ambari uyarı yapılandırma, izleme düğümü kullanılabilirlik oranı ve bir veya daha fazla düğümünden bir sinyal alınmadı, tetiklenen Azure İzleyici uyarı oluşturmaya ilişkin bazı örnekler gösterilmektedir beş saat.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>Pano

Ambari Pano tıklayarak erişilebilir **Ambari giriş** bağlantısını **küme panoları** HDInsight genel bakış dikey penceresinde gösterildiği Azure Portalı'nda aşağıdaki bölümü. Alternatif olarak, bir tarayıcıda aşağıdaki URL'yi girerek erişilebileceğini [https://\<clustername\>. azurehdinsight.net](https://clustername.azurehdinsight.net/)

![HDInsight kaynak portal görünümü](media/hdinsight-cluster-availability/portal-overview.png)

Ardından bir küme oturum açma kullanıcı adı ve parola istenir. Kimlik bilgilerini girin, kümeyi oluştururken seçtiğiniz.

Ardından birkaç HDInsight kümenizin sistem durumu hızlı bir genel bakış vermek için ölçümleri Göster pencere öğeleri içeren Ambari panoya alınır. Bu pencere öğeleri Canlı DataNodes (çalışan düğümleri) ve JournalNodes (zookeeper düğümü) sayısı gibi ölçümleri Göster NameNodes (baş düğüm) çalışma süresi, iyi ölçümleri belirli küme türleri için özel olarak ister Spark ve Hadoop kümeleri için YARN ResourceManager çalışma süresi.

![Ambari Panosu](media/hdinsight-cluster-availability/ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Konakları – tek düğüm durumunu görüntüleme

Ayrıca, tek tek düğümleri için durum bilgisini de görüntüleyebilirsiniz. Tıklayın **konakları** kümedeki tüm düğümlerin listesini görüntülemek ve her düğüm hakkındaki temel bilgileri görmek için sekmesinde. Her düğüm adı solundaki yeşil onay düğümde çalışır durumda tüm bileşenleri gösterir. Bir bileşenin bir düğüm üzerinde çalışmıyorsa, yeşil onay işareti yerine kırmızı bir uyarı üçgen görürsünüz.

![Görüntülemek Ambari konakları](media/hdinsight-cluster-availability/ambari-hosts.png)

Ardından tıklayabilirsiniz **adı** görüntülemek için bir düğüm, belirli bir düğüm için konak ölçümlerini ayrıntılı. Bu görünüm, her ayrı ayrı bileşen durumu/kullanılabilirliğini gösterir.

![Ambari konakları tek düğüm görünümü](media/hdinsight-cluster-availability/ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Ambari uyarıları

Ambari, ayrıca belirli olaylar bildirim sağlayan birkaç yapılandırılabilir uyarı sunar. Uyarı tetiklendiğinde uyarı sayısını içeren bir kırmızı rozet Ambari, sol üst köşede gösterilir. Bu rozet tıklayarak geçerli uyarıların bir listesi gösterilir.

![Ambari uyarı sayısı](media/hdinsight-cluster-availability/ambari-alerts.png)

Uyarı tanımları ve bunların durumlarını listesini görüntülemek için tıklayın **uyarılar** sekmesinde, aşağıda gösterildiği gibi.

![Görüntülemek Ambari uyarılar tanımları](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari sunar, kullanılabilirlikle ilgili pek çok önceden tanımlanmış uyarılar dahil olmak üzere:

| Uyarı adı                        | Açıklama                                                                                                                                                                           |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DataNode sistem durumu özeti           | Sağlıksız DataNodes varsa bu hizmet düzeyi uyarı tetiklenir                                                                                                                |
| NameNode yüksek kullanılabilirlik sistem durumu | Etkin NameNode ya da bekleme NameNode çalışmıyorsa, bu hizmet düzeyi uyarısı tetiklenir.                                                                              |
| Yüzde JournalNodes kullanılabilir    | Sayısını JournalNodes aşağı kümedeki kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bunu JournalNode işlem denetimlerin sonuçlarını toplar. |
| Yüzde DataNodes kullanılabilir       | Sayısını DataNodes aşağı kümedeki kritik yapılandırılan eşik değerinden büyükse, bu uyarı tetiklenir. Bunu DataNode işlem denetimlerin sonuçlarını toplar.       |

Ambari, tam listesini kullanılabilirlik kümesinin bulunabilir, Yardım İzleyici uyarılarına [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui),

Bir uyarının ayrıntılarını görüntülemek veya ölçütlerini değiştirmek için tıklayın **adı** uyarı. Ele **sistem durumu özetini DataNode** örnek olarak. 'Uyarı' veya 'kritik' uyarı ve ölçütler için denetleme aralığı tetikleyecek belirli bir ölçüte yanı sıra, uyarının açıklamasını görebilirsiniz. Yapılandırmayı düzenlemek için tıklayın **Düzenle** yapılandırma kutusunun sağ üst köşesindeki düğme.

![Ambari uyarı yapılandırma](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Burada, açıklama ve daha da önemlisi, onay düzenleyebilirsiniz aralığı ve eşikler için uyarı veya kritik uyarılar.

![Ambari uyarı yapılandırma düzenleme görünümü](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

Bu örnekte, bir kritik uyarı ve 1 sağlıksız DataNode yalnızca tetikleyici bir uyarı tetiklemek 2 sağlıksız DataNodes yapabilir. Tıklayın **Kaydet** işiniz bittiğinde düzenleme.

### <a name="email-notifications"></a>E-posta bildirimleri

Ayrıca isteğe bağlı olarak, Ambari uyarılar için e-posta bildirimleri de yapılandırabilirsiniz. Bunu yapmak için üzerinde ne zaman **uyarılar** sekmesinde **eylemleri** sol, ardından düğmesini **bildirimleri yönetme.**

![Ambari bildirimleri eylem yönetme](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Uyarı bildirimlerini yönetmek için bir iletişim kutusu açılır. Tıklayın **+** Ambari sunucusu ayrıntıları e-postaların gönderileceği e-posta ile sağlamak için gerekli alanları doldurun ve iletişim alt kısmındaki.

> [!TIP]
> Ambari ayarı e-posta bildirimleri birçok HDInsight kümelerini yönetirken tek bir yerde uyarıları almak için en iyi yolu olabilir.

## <a name="azure-monitor-logs-integration"></a>Azure İzleyici tümleştirmesi günlüğe kaydeder.

Azure İzleyici toplanacağı ve birleşik bir izleme deneyimi elde etmek için tek bir yerde toplanır için HDInsight kümeleri gibi birden çok kaynaklar tarafından oluşturulan etkinleştirir verilerini günlüğe kaydeder.

Bir önkoşul olarak toplanan verileri depolamak için Log Analytics çalışma gerekir. Zaten bir oluşturmadıysanız, buradaki yönergeleri izleyebilirsiniz: [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>HDInsight Azure İzleyici günlüklerine tümleştirmesini etkinleştirme

Portal'da HDInsight kümesi kaynak sayfasından tıklayın **Operations Management Suite** dikey penceresi. ' A tıklayarak **etkinleştirme** ve açılan listeden Log Analytics çalışma alanınızı seçin.

![HDInsight Operations Management Suite dikey penceresi](media/hdinsight-cluster-availability/portal-enable-oms.png)

### <a name="query-metrics-and-logs-tables-in-the-logs-blade"></a>Günlükleri dikey penceresinde, ölçüm ve günlükleri tabloları sorgulama

Azure İzleyici günlük tümleştirmesi etkinleştirildiğinde (Bu işlem birkaç dakika sürebilir), gidin, **Log Analytics çalışma alanı** kaynak ve tıklayın **günlükleri** dikey penceresi

![Log Analytics çalışma alanı günlükleri dikey penceresi](media/hdinsight-cluster-availability/portal-logs.png)

**Günlükleri** dikey penceresinde, örnek sorguları bir dizi gibi listelenir:

| Sorgu adı                      | Açıklama                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Bilgisayarları kullanılabilirlik Bugün    | Grafik günlükleri, her saat gönderme bilgisayar sayısı                     |
| Liste sinyal                 | Son bir saat gelen tüm bilgisayar sinyalleri listesi                           |
| Her bilgisayarın son sinyal | Her bilgisayar tarafından gönderilen son sinyal Göster                             |
| Bilgisayarların kullanılamamasının           | Son 5 saat içinde bir sinyal gönderin, bilinen tüm bilgisayarları listeleyin |
| Kullanılabilirlik oranı               | Bağlanan her bilgisayar kullanılabilirlik oranını hesaplayın                |

Örnek olarak, çalıştırma **kullanılabilirlik oranı** tıklayarak örnek sorgu **çalıştırma** yukarıdaki ekran görüntüsünde gösterildiği gibi bu sorgu üzerinde. Bu işlem, yüzde olarak kümedeki her düğüm kullanılabilirlik oranını gösterir. Ölçümleri aynı Log Analytics çalışma alanına göndermek birden çok HDInsight kümesi etkinleştirilirse, görüntülenen o kümedeki tüm düğümler için kullanılabilirlik oranı görürsünüz.

![Günlük analizi çalışma alanı günlükleri dikey penceresinde 'kullanılabilirlik oranı' örnek sorgu](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE] 
> Kümenizi doğru kullanılabilirlik fiyatları üzerinden görmeden önce en az 24 saat için çalıştırmanız gerekir böylece kullanılabilirlik ücretinin bir 24 saatlik dönem boyunca ölçülür.

Tıklayarak, bu tabloda paylaşılan bir panoya sabitleyebilirsiniz **PIN** sağ üst köşedeki. Yazılabilir panoları yoksa nasıl oluşturulacağını görebilirsiniz burada: [Azure portalında panolarını oluşturma ve paylaşma](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-a-dashboard-and-manage-access-control).

### <a name="azure-monitor-alerts"></a>Azure uyarıları izleme

Azure İzleyici uyarılarını ne zaman tetikleyecek bir ölçüm değeri ayarlayabilirsiniz veya bir sorgunun sonuçlarını belirli koşullara uygun. Örneğin, bir veya daha fazla düğüm henüz gönderildiğinde bir sinyal 5 saat içinde e-posta göndermek için bir uyarı oluşturalım (yani kullanılamaz olduğu varsayılmıştır).

Gelen **günlükleri** dikey penceresinde çalıştırın **bilgisayarların kullanılamamasının** tıklayarak örnek sorgu **çalıştırma** aşağıda gösterildiği gibi bu sorgu üzerinde.

![Günlük analizi çalışma alanı günlükleri dikey penceresinde 'bilgisayarların kullanılamamasının' örnek sorgu](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Tüm düğümlerin kullanılabilir olmasını, bu sorgu şu an için 0 sonuç döndürmesi gerekir. Tıklayın **yeni uyarı kuralı** Uyarınız bu sorgu için yapılandırmaya başlayın.

![Log Analytics çalışma alanı yeni uyarı kuralı](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Bir uyarıya üç bileşeni vardır: *kaynak* (Bu durumda Log Analytics çalışma), kuralı oluşturulacağı *koşul* uyarıyı tetikleyecek ve *Eylem grupları*  uyarısı tetiklendiğinde ne olacağını belirler.

Tıklayın **koşul başlık**sinyal mantığını yapılandırmayı tamamlamak için aşağıda gösterildiği gibi.

![Uyarı kuralı koşulu](media/hdinsight-cluster-availability/portal-condition-title.png)

Bu açılır **sinyal mantığını yapılandırma** dikey penceresi.

Ayarlama **uyarı mantığı** gibi bölümünde:

*Temel: Sonuç, koşul sayısı: Eşik değerinden daha büyük: 0.*

Sonuç sayısı 0'dan hiç olmadığı kadar büyükse, bu sorgu kullanılamaz düğümleri yalnızca sonuç döndürür. bu yana uyarı harekete.

İçinde **göre Evaluated** bölümünde, **süresi** ve **sıklığı** ne sıklıkta, kullanılamayan düğümleri denetlemek istediğinize bağlı.

Bu uyarı amacıyla emin olmak istediğiniz Not **dönem sıklığı =.** Süresi, sıklığı ve uyarı diğer parametreler hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Tıklayın **Bitti** işiniz bittiğinde sinyal mantığını yapılandırma.

![Uyarı kuralı sinyal mantığını yapılandırma](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Var olan bir eylem grubu zaten yoksa tıklayın **Yeni Oluştur** altında **Eylem grupları** bölümü.

![Uyarı kuralı yeni eylem grubu](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Bu açılır **eylem grubu Ekle** dikey penceresi. Seçin bir **eylem grubu adı**, **kısa ad**, **abonelik**, ve **kaynak grubu.** Altında **eylemleri** bölümünde, seçin bir **eylem adı** seçip **e-posta/SMS/anında iletme/ses** olarak **eylem türü.**

> [!NOTE]
> Bir e-posta/SMS/anında iletme/ses, bir Azure işlevi, mantıksal uygulama, Web kancası, ITSM ve Otomasyon Runbook'u gibi yanında bir uyarı tetikleyebilir çeşitli eylemler vardır. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Bu açılır **e-posta/SMS/anında iletme/ses** dikey penceresi. Seçin bir **adı** alıcı **denetleyin** **e-posta** kutusuna ve gönderilen uyarı istediğiniz bir e-posta adresini yazın. Tıklayın **Tamam** içinde **e-posta/SMS/anında iletme/ses** dikey penceresinde, ardından **eylem grubu Ekle** eylem grubunuzu yapılandırmayı tamamlamak için dikey.

![Eylem grubu uyarı kuralı Ekle](media/hdinsight-cluster-availability/portal-add-action-group.png)

Bu dikey pencereleri kapattıktan sonra eylem grubu altında listelenen görmelisiniz **Eylem grupları** bölümü. Son olarak, tamamlamak **uyarı ayrıntılarını** yazarak bölümü bir **uyarı kuralı adı** ve **açıklama** seçip bir **önem derecesi**.
Tıklayın **uyarı kuralı oluştur** tamamlanması.

![Son uyarı kuralı oluşturma](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> Belirtme olanağı **önem derecesi** birden çok uyarı oluştururken kullanılabilecek güçlü bir araçtır. Örneğin, aşağı tek bir baş düğüm aşması durumunda bir uyarı (önem derecesi 1) yükseltmek için bir uyarı oluşturabilir ve kritik (önem derecesi 0) hem de düğümleri head olası olayı yükselten başka bir uyarı aşağı git.

Bu uyarı için bir koşul karşılandığında uyarı tetiklenir ve bunun gibi uyarı ayrıntılarını içeren bir e-posta alırsınız:

![Azure İzleyici uyarı e-posta](media/hdinsight-cluster-availability/alert-email.png)

Ayrıca, giderek önem derecesine göre gruplandırılmış başlatıldı tüm uyarıları görüntüleyebilirsiniz **uyarılar** dikey penceresinde, **Log Analytics çalışma alanı**.

![Log Analytics çalışma alanı uyarıları](media/hdinsight-cluster-availability/portal-alerts.png)

Önem derecesi gruplandırma'yı tıklatarak (yani **önem derecesi 1** vurgulanan yukarıda) tüm uyarıları gibi aşağıda başlatıldı, önem derecesi için kayıtları gösterir:

![Log Analytics çalışma alanını önem derecesi 1 uyarıları](media/hdinsight-cluster-availability/portal-alerts-sev-1.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Kullanılabilirliği ve güvenilirliği HDInsight Apache Hadoop kümelerini](hdinsight-high-availability-linux.md)
