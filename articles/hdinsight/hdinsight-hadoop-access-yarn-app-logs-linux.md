---
title: Erişim Hadoop YARN uygulama günlüklerini Linux tabanlı Hdınsight üzerinde - Azure | Microsoft Docs
description: YARN uygulama günlükleri komut satırı ve bir web tarayıcısı kullanarak bir Linux tabanlı Hdınsight (Hadoop) kümesine erişim öğrenin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: larryfr
ms.openlocfilehash: 67732b3a4c63408d8fc6ab5ed75b40a0eb043167
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Linux tabanlı Hdınsight üzerinde erişim YARN uygulama günlükleri

Azure Hdınsight Hadoop kümesinde YARN (henüz başka bir kaynak Uzlaştırıcı) uygulamaları için günlüklere erişmek öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN zaman çizelgesi sunucu

[YARN zaman çizelgesi sunucu](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) tamamlanmış uygulamaların ve iki farklı arabirimleri aracılığıyla çerçeveye özel uygulama bilgilerini hakkında genel bilgiler sağlar. Bu avantajlar şunlardır:

* 3.1.1.374 sürümüyle etkin ya da daha yüksek depolama ve alma Hdınsight kümelerinde genel uygulama bilgilerinin açıldı.
* Zaman Çizelgesi sunucunun çerçeveye özel uygulama bilgileri bileşeninin Hdınsight kümelerinde şu anda kullanılabilir değil.

Uygulamalar hakkında genel bilgiler aşağıdaki veri türünü içerir:

* Uygulama kimliği, uygulamanın benzersiz tanıtıcısı
* Uygulama başlatan kullanıcının
* Uygulama tamamlamak için yapılan deneme hakkında bilgi
* Belirtilen uygulama girişimleri tarafından kullanılan kapsayıcıları

## <a name="YARNAppsAndLogs"></a>YARN uygulamalar ve günlükleri

YARN kaynak Yönetimi'nden uygulama zamanlama/izleme ayrıştırarak birden fazla programlama modeli (bunlardan biri olan MapReduce) destekler. YARN kullanan bir genel *ResourceManager* (RM) çalışan-düğüm başına *NodeManagers* (NMs) ve uygulama başına *ApplicationMasters* (AMs). Uygulama başına AM RM ile uygulamanızı çalıştırmak için kaynakları (CPU, bellek, disk, ağ) anlaşır. RM olarak verilen bu kaynaklara erişim izni NMs çalışır *kapsayıcıları*. AM RM tarafından atanmış kapsayıcıları ilerlemesini izlemek için sorumludur Bir uygulama, uygulama doğasına bağlı olarak çok sayıda kapsayıcı gerektirebilir.

Her uygulama katları oluşabilir *uygulama denemeleri*. Bir uygulama başarısız olursa, yeni bir girişim denenen. Her girişimde bir kapsayıcıda çalışır. Bir anlamda bağlam YARN uygulama tarafından gerçekleştirilen çalışmanın temel birim için bir kapsayıcı sağlar. Bir kapsayıcı bağlamı içinde yapılan tüm iş kapsayıcı ayrılmış olan tek çalışan düğümünde gerçekleştirilir. Bkz: [YARN kavramları] [ YARN-concepts] daha ayrıntılı başvuru.

Uygulama (ve ilişkili kapsayıcı günlükleri) sorunlu Hadoop uygulamalarında hata ayıklama de önemlidir. YARN toplama, toplama ve uygulama günlükleri ile depolamak için iyi bir çerçeve sağlar [günlük toplama] [ log-aggregation] özelliği. Günlük toplama özellik erişimi uygulama günlüklerini daha belirleyici hale getirir. Bir alt düğüm üzerindeki tüm kapsayıcıları arasında günlükleri toplar ve çalışan düğümü başına bir toplu günlük dosyası olarak depolar. Bir uygulama bittikten sonra günlük varsayılan dosya sistemi üzerinde depolanır. Uygulamanızın yüzlerce veya binlerce kapsayıcıların kullanabilir, ancak günlükler tek alt düğüm üzerinde çalışan tüm kapsayıcıları için her zaman tek bir dosyaya toplanır. Bu nedenle var. yalnızca uygulamanız tarafından kullanılan çalışan düğümü başına 1 günlük Günlük toplama Hdınsight kümeleri sürüm 3.0 ve üstü varsayılan olarak etkindir. Toplanan günlükleri kümenin varsayılan depolama bulunur. Aşağıdaki yola günlüklerine HDFS yol şudur:

    /app-logs/<user>/logs/<applicationId>

Yolda `user` uygulama başlatan kullanıcının adıdır. `applicationId` YARN RM tarafından atanan bir uygulama için benzersiz tanımlayıcısı

Bunlar yazıldığı şekilde toplanmış günlükleri doğrudan okunabilir olmayan bir [TFile][T-file], [ikili biçimi] [ binary-format] kapsayıcı dizini. Bu günlükler uygulamalar veya ilgi kapsayıcıları için düz metin olarak görüntülemek için YARN ResourceManager günlükleri veya CLI araçlarını kullanın.

## <a name="yarn-cli-tools"></a>YARN CLI araçlarını

YARN CLI araçlarını kullanmak için önce SSH kullanarak Hdınsight kümesine bağlanmanız gerekir. Bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

Düz metin olarak aşağıdaki komutlardan birini çalıştırarak bu günlükleri görüntüleyebilirsiniz:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Belirtin &lt;ApplicationId >, &lt;kullanıcı-kim-başlatıldı--uygulama >, &lt;Containerıd >, ve &lt;alt düğüm adresi > Bu komutlar çalıştırıldığında, bilgi.

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi

YARN ResourceManager UI küme headnode üzerinde çalışır. Ambari web kullanıcı Arabirimi erişilir. YARN günlüklerini görüntülemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızda gidin https://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME Hdınsight kümenizin adıyla değiştirin.
2. Sol taraftaki Hizmetleri listesinden seçin **YARN**.

    ![Seçili yarn hizmeti](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Gelen **hızlı bağlantılar** açılan listesinde, küme baş düğümler birini seçin ve ardından **ResourceManager günlük**.

    ![Yarn hızlı bağlantılar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    YARN günlüklerini bağlantılar listesiyle birlikte sunulur.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
