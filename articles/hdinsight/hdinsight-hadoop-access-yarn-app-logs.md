---
title: "Erişim Hadoop YARN uygulama günlüklerini program aracılığıyla - Azure | Microsoft Docs"
description: "Access uygulaması Hdınsight Hadoop kümesinde programlı olarak günlüğe kaydeder."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0198d6c9-7767-4682-bd34-42838cf48fc5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 90323af4a1f4526ab9b26811c8679337076112d1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Windows tabanlı Hdınsight üzerinde erişim YARN uygulama günlükleri
Bu konuda, Azure hdınsight'ta Hadoop Windows tabanlı kümede bitirdikten YARN (henüz başka bir kaynak Uzlaştırıcı) uygulamaları için günlüklere erişmek açıklanmaktadır

> [!IMPORTANT]
> Bu belgedeki bilgiler yalnızca Windows tabanlı Hdınsight kümeleri için geçerlidir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı Hdınsight kümelerinde YARN erişme hakkında bilgi günlükleri için bkz: [erişim YARN uygulama günlüklerini hdınsight'ta Linux tabanlı Hadoop üzerinde](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Ön koşullar
* Bir Windows tabanlı Hdınsight kümesi.  Bkz: [Hdınsight oluşturma Windows tabanlı Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN zaman çizelgesi sunucu
<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN zaman çizelgesi sunucu</a> de framework özgü olarak uygulama bilgilerini iki farklı arabirimleri aracılığıyla tamamlanmış uygulamaları genel bilgiler sağlar. Bu avantajlar şunlardır:

* 3.1.1.374 sürümüyle etkin ya da daha yüksek depolama ve alma Hdınsight kümelerinde genel uygulama bilgilerinin açıldı.
* Zaman Çizelgesi sunucunun çerçeveye özel uygulama bilgileri bileşeninin Hdınsight kümelerinde şu anda kullanılabilir değil.

Uygulamalar hakkında genel bilgiler aşağıdaki sıralar veri içerir:

* Uygulama kimliği, uygulamanın benzersiz tanıtıcısı
* Uygulama başlatan kullanıcının
* Uygulama tamamlamak için yapılan deneme hakkında bilgi
* Belirtilen uygulama girişimleri tarafından kullanılan kapsayıcıları

Hdınsight kümelerinde bu bilgileri varsayılan Azure depolama hesabınızın varsayılan kapsayıcı geçmişi deposunda için Azure Resource Manager tarafından depolanır. Bu genel veriler tamamlanmış uygulamalar üzerinde bir REST API'si alınabilir:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>YARN uygulamalar ve günlükleri
YARN kaynak Yönetimi'nden uygulama zamanlama/izleme ayrıştırarak birden fazla programlama modeli (bunlardan biri olan MapReduce) destekler. Bu genel yapılır *ResourceManager* (RM) çalışan-düğüm başına *NodeManagers* (NMs) ve uygulama başına *ApplicationMasters* (AMs). Uygulama başına AM RM ile uygulamanızı çalıştırmak için kaynakları (CPU, bellek, disk, ağ) anlaşır. RM olarak verilen bu kaynaklara erişim izni NMs çalışır *kapsayıcıları*. AM RM tarafından atanmış kapsayıcıları ilerlemesini izlemek için sorumludur Bir uygulama, uygulama doğasına bağlı olarak çok sayıda kapsayıcı gerektirebilir.

Ayrıca, her uygulama katları oluşabilir *uygulama denemeleri* kilitlenme varlığında veya bir AM arasındaki iletişimi kaybı nedeniyle tamamlamak için ve bir RM Bu nedenle, kapsayıcıları belirli bir uygulama girişimi verilir. Bir fikir bağlam YARN uygulama tarafından gerçekleştirilen çalışmanın temel birim için bir kapsayıcı sağlar ve bir kapsayıcı bağlamı içinde yapılan tüm iş kapsayıcı ayrılmış olan tek çalışan düğümünde gerçekleştirilir. Bkz: [YARN kavramları] [ YARN-concepts] daha ayrıntılı başvuru.

Uygulama (ve ilişkili kapsayıcı günlükleri) sorunlu Hadoop uygulamalarında hata ayıklama de önemlidir. YARN toplama, toplama ve uygulama günlükleri ile depolamak için iyi bir çerçeve sağlar [günlük toplama] [ log-aggregation] özelliği. Günlük toplama özellik bir çalışan düğümdeki tüm kapsayıcıları arasında günlükleri toplar ve bir uygulama bittikten sonra bir toplu günlük dosyası olarak çalışan düğümü başına varsayılan dosya sistemi üzerinde saklar gibi erişen uygulama günlüklerini daha belirleyici hale getirir. Uygulamanızın yüzlerce veya binlerce kapsayıcıların kullanabilir, ancak günlükleri tek alt düğüm üzerinde çalışan tüm kapsayıcıları için her zaman tek uygulamanız tarafından kullanılan çalışan düğümü başına bir günlük dosyasında oluşan bir dosyaya toplanacak. Günlük toplama Hdınsight kümelerinde varsayılan olarak etkinleştirilmiştir (sürüm 3.0 ve üstü), ve toplanan günlükleri varsayılan kapsayıcı kümenizin şu konumda bulunabilir:

    wasb:///app-logs/<user>/logs/<applicationId>

Bu konumda bulunan *kullanıcı* uygulama başlatan kullanıcı adı ve *ApplicationId* YARN RM tarafından atanan bir uygulamanın benzersiz tanımlayıcısı değil

Bunlar yazıldığı şekilde toplanmış günlükleri doğrudan okunabilir olmayan bir [TFile][T-file], [ikili biçimi] [ binary-format] kapsayıcı dizini. YARN uygulamalar veya ilgi kapsayıcıları için düz metin olarak bu günlükler dökümü CLI araçlarını sağlar. Aşağıdaki YARN birini çalıştırarak düz metin (için RDP bağlandıktan sonra) doğrudan küme düğümlerinde komutları gibi bu günlükleri görüntüleyebilirsiniz:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager kullanıcı Arabirimi
YARN ResourceManager UI küme headnode üzerinde çalışır ve Azure portalı panosunun erişilebilir:

1. [Azure portalda](https://portal.azure.com/) oturum açın.
2. Sol menüsünde **Gözat**, tıklatın **Hdınsight kümeleri**, YARN uygulama günlüklerine erişmek istediğiniz Windows tabanlı bir kümesine tıklayın.
3. Üst menüsünde **Pano**. Yeni bir tarayıcıda açılan bir sayfa görürsünüz adlı sekmesini **Hdınsight sorgu konsol**.
4. Gelen **Hdınsight sorgu konsol**, tıklatın **Yarn kullanıcı Arabiriminde**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
