---
title: Linux tabanlı HDInsight üzerinde - Azure erişim Apache Hadoop YARN uygulama günlüklerine
description: Komut satırı ve bir web tarayıcısı kullanarak bir Linux tabanlı HDInsight (Apache Hadoop) kümesinde YARN uygulama günlüklerine erişmeyi öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.openlocfilehash: c0c5ecfba97c61288d08681006645eab0bdd23f2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059469"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-linux-based-hdinsight"></a>Linux tabanlı HDInsight üzerinde erişim Apache Hadoop YARN uygulama günlüklerine

İçin günlüklere nasıl erişeceğinizi öğrenin [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) (henüz başka bir Resource Negotiator) uygulamaları bir [Apache Hadoop](https://hadoop.apache.org/) Azure HDInsight kümesinde.

## <a name="YARNTimelineServer"></a>YARN Timeline sunucusu

[Apache Hadoop YARN Timeline sunucusu](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) tamamlanmış uygulamalar genel bilgiler sağlar

YARN Timeline sunucusu, aşağıdaki veri türünü içerir:

* Uygulama kimliği, uygulamanın benzersiz tanımlayıcısı
* Uygulamayı başlatan kullanıcının
* Uygulama tamamlamak için atılan hakkında bilgi
* Belirtilen uygulama girişimleri tarafından kullanılan kapsayıcıları

## <a name="YARNAppsAndLogs"></a>YARN uygulama ve günlükleri

YARN birden fazla programlama modelini destekler ([Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) bunlardan biri olan) kaynak yönetimi uygulama zamanlama/izleme ayırma tarafından. YARN kullandığı genel *ResourceManager* (RM) çalışan-düğüm başına *NodeManagers* (NMs) ve uygulama başına *ApplicationMasters* (AMs). Uygulama başına AM RM ile uygulamanızı çalıştırmak için kaynaklar (CPU, bellek, disk, ağ) görüşür. RM olarak verilen bu kaynaklara erişim izni için NMs çalışır *kapsayıcıları*. AM RM tarafından atanmış kapsayıcıları ilerlemesini izlemek için sorumludur Bir uygulamayı uygulamanın doğasına bağlı olarak çok sayıda kapsayıcı olması gerekebilir.

Her uygulama birden çok oluşabilir *uygulama girişimi*. Bir uygulama başarısız olursa, yeni bir deneme gerçekleştirilmesi. Her girişimde bir kapsayıcıda çalışır. Bir anlamda, temel birim YARN uygulama tarafından gerçekleştirilen iş bağlamı bir kapsayıcı sağlar. Bir kapsayıcı bağlamında yapılır tüm iş kapsayıcı ayrılmış olan tek bir çalışan düğümü üzerinde gerçekleştirilir. Bkz: [Apache Hadoop YARN kavramları](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html) daha ayrıntılı başvuru.

Uygulama günlükleri (ve ilişkili kapsayıcı günlüklerini) sorunlu Hadoop uygulamalarında hata ayıklama içinde önemlidir. YARN toplama, toplama ve uygulama günlükleriyle depolamak için iyi bir çerçeve sağlar [günlük toplama](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/) özelliği. Günlük toplama özelliği erişen uygulama günlükleri daha kararlı hale getirir. Bir çalışan düğümü üzerindeki tüm kapsayıcılar arasında günlükleri toplar ve bunları çalışan düğümü başına bir toplu günlük dosyası olarak depolar. Bir uygulama bittikten sonra günlük varsayılan dosya sistemi üzerinde depolanır. Uygulamanız, yüzlerce veya binlerce kullanabilir, ancak günlükler için tek çalışan düğümü üzerinde çalıştıran tüm kapsayıcıları tek bir dosyaya her zaman toplanır. Bu nedenle var. yalnızca uygulamanız tarafından kullanılan çalışan düğümü başına 1 günlük Günlük toplama, HDInsight kümeleri sürüm 3.0 ve sonraki varsayılan olarak etkindir. Toplanan günlükler kümenin varsayılan depolama alanı bulunur. Aşağıdaki yol günlükleri HDFS yoludur:

    /app-logs/<user>/logs/<applicationId>

Yoluna `user` uygulamayı başlatan kullanıcının adıdır. `applicationId` YARN RM tarafından bir uygulamaya atanan benzersiz tanımlayıcısı

Bunlar yazıldığı şekilde toplanan günlükler doğrudan okunabilir olmayan bir [TFile][T-file], [ikili biçimi] [ binary-format] kapsayıcı tarafından dizini oluşturulmuş. Bu günlükler, uygulamalar veya ilgi kapsayıcılar için düz metin olarak görüntülemek için YARN ResourceManager günlükleri veya CLI araçlarını kullanın.

## <a name="yarn-cli-tools"></a>YARN CLI araçları

YARN CLI araçları kullanmak için HDInsight kümesine SSH kullanarak bağlanmanız gerekir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Bu günlükler, düz metin olarak aşağıdaki komutlardan birini çalıştırarak görüntüleyebilirsiniz:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Belirtin &lt;ApplicationId >, &lt;kullanıcı-kim-başlatıldı--uygulama >, &lt;Containerıd >, ve &lt;alt düğüm adresi > şu komutları çalıştırarak bilgi.

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi

YARN ResourceManager kullanıcı Arabirimi kümesi baş düğümünde çalışır. Ambari web kullanıcı Arabirimi erişilebilir. YARN günlükleri görüntülemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızda gidin https://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME HDInsight kümenizin adıyla değiştirin.
2. Hizmetleri soldaki listeden seçin **YARN**.

    ![Seçili yarn hizmeti](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Gelen **hızlı bağlantılar** açılır listesinde, bir küme baş düğümleri seçin ve ardından **ResourceManager günlük**.

    ![Yarn hızlı bağlantılar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    YARN günlükleri yönelik bağlantıların bir listesi sunulur.

[YARN-timeline-server]:https://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
