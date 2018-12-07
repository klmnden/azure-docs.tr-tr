---
title: Hadoop YARN uygulama program aracılığıyla - Azure günlüklerine erişme
description: Uygulama program aracılığıyla bir HDInsight Hadoop kümesinde günlüklerine erişme.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: da105be19f7d546e530298f87974fe7f3f78989f
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012224"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-windows-based-hdinsight"></a>Windows tabanlı HDInsight üzerinde erişim Apache Hadoop YARN uygulama günlüklerine
Bu belge için günlüklere nasıl erişeceğinizi açıklar [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) Windows tabanlı bir Apache Hadoop üzerine tamamlanmış uygulamalar Azure HDInsight küme

> [!IMPORTANT]
> Bu belgedeki bilgiler yalnızca Windows tabanlı HDInsight kümeleri için geçerlidir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı HDInsight kümelerinde günlüklerini YARN erişme hakkında bilgi için bkz. [erişim Apache Hadoop YARN uygulama günlüklerine Apache Hadoop'ta Linux tabanlı HDInsight üzerinde](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Önkoşullar
* Bir Windows tabanlı HDInsight kümesi.  Bkz: [oluşturma Windows tabanlı Apache Hadoop kümeleri HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN Timeline sunucusu
<a href="http://hadoop.apache.org/docs/r2.4.1/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Apache Hadoop YARN Timeline sunucusu</a> genel bilgiler tamamlanmış uygulamaları iki farklı arabirimler üzerinden de olarak çerçeveye özgü uygulama bilgileri sağlar. Bu avantajlar şunlardır:

* Depolama ve HDInsight kümelerinde genel uygulama bilgilerin alınmasını 3.1.1.374 sürümüyle etkin ya da daha yüksek olmuştur.
* HDInsight kümelerinde çerçeveye özgü uygulama bilgileri bileşenini Timeline sunucusu şu anda kullanılamıyor.

Veri şu tür uygulamalar hakkında genel bilgiler içerir:

* Uygulama kimliği, uygulamanın benzersiz tanımlayıcısı
* Uygulamayı başlatan kullanıcının
* Uygulama tamamlamak için atılan hakkında bilgi
* Belirtilen uygulama girişimleri tarafından kullanılan kapsayıcıları

HDInsight kümeleriniz üzerinde bu bilgiler, Azure Resource Manager tarafından depolanır. Bilgiler, geçmişi deponun kümenin varsayılan depolama alanı için kaydedilir. Tamamlanan uygulamaların genel bu veriler, bir REST API aracılığıyla alınabilir:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN uygulama ve günlükleri
YARN kaynak yönetimi uygulama zamanlama/izleme ayrılarak birden çok programlama modelini destekler. YARN kullandığı genel *ResourceManager* (RM) çalışan-düğüm başına *NodeManagers* (NMs) ve uygulama başına *ApplicationMasters* (AMs). Uygulama başına AM RM ile uygulamanızı çalıştırmak için kaynaklar (CPU, bellek, disk, ağ) görüşür. RM olarak verilen bu kaynaklara erişim izni için NMs çalışır *kapsayıcıları*. AM RM tarafından atanmış kapsayıcıları ilerlemesini izlemek için sorumludur Bir uygulamayı uygulamanın doğasına bağlı olarak çok sayıda kapsayıcı olması gerekebilir.

* Her uygulama birden çok oluşabilir *uygulama girişimi*. 
* Kapsayıcılar, bir uygulamanın belirli bir girişimi verilir. 
* Bir kapsayıcı temel iş birimidir için bağlam sağlar. 
* Bir kapsayıcı bağlamında yapılır iş kapsayıcısı için ayrılan tek bir çalışan düğümü üzerindeki gerçekleştirilir. 

Daha fazla bilgi için [Apache Hadoop YARN kavramları][YARN-concepts].

Uygulama günlükleri (ve ilişkili kapsayıcı günlüklerini) sorunlu Hadoop uygulamalarında hata ayıklama içinde önemlidir. YARN toplama, toplama ve uygulama günlükleriyle depolamak için iyi bir çerçeve sağlar [günlük toplama] [ log-aggregation] özelliği. Günlük toplama özelliği, bir çalışan düğümü üzerindeki tüm kapsayıcılar arasında günlükleri toplar ve uygulama bittikten sonra bir toplu günlük dosyası çalışan düğümü başına varsayılan dosya sistemi olarak depolar gibi erişen uygulama günlükleri daha kararlı hale getirir. Uygulamanız, yüzlerce veya binlerce kullanabilir, ancak tek tek bir dosyada uygulamanız tarafından kullanılan çalışan düğümü başına kaynaklanan bir dosyaya tek bir alt düğüm üzerinde çalışan tüm kapsayıcılar için günlükleri toplanır. Günlük toplama HDInsight kümelerinde varsayılan olarak etkinleştirilmiştir (sürüm 3.0 ve üstü), ve toplanan günlükler varsayılan kapsayıcı kümenizin şu konumda bulunabilir:

    wasb:///app-logs/<user>/logs/<applicationId>

O konumda *kullanıcı* uygulamayı başlatan kullanıcının adıdır ve *ApplicationId* YARN RM tarafından atanmış bir uygulamanın benzersiz tanımlayıcısı aynıdır

Bunlar yazıldığı şekilde toplanan günlükler doğrudan okunabilir olmayan bir [TFile][T-file], [ikili biçimi] [ binary-format] kapsayıcı tarafından dizini oluşturulmuş. YARN CLI araçları Bu günlükler, uygulamalar veya ilgi kapsayıcılar için düz metin olarak dökümü sağlar. Aşağıdaki YARN birini çalıştırarak düz metin (RDP bağlanmak sonra) doğrudan küme düğümlerinde komutları gibi bu günlükleri görüntüleyebilirsiniz:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi
YARN ResourceManager kullanıcı Arabirimi, küme baş düğümüne üzerinde çalışır ve Azure portal panosunda erişilebilir:

1. [Azure portalda](https://portal.azure.com/) oturum açın.
2. Sol menüsünde **Gözat**, tıklayın **HDInsight kümeleri**, YARN uygulama günlüklerine erişmek için Windows tabanlı bir kümeye tıklayın.
3. Üst menüsünde **Pano**. Yeni bir tarayıcıda açılan bir sayfa görürsünüz sekmesi adı verilen **HDInsight sorgu Konsolu**.
4. Gelen **HDInsight sorgu Konsolu**, tıklayın **Yarn UI**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
