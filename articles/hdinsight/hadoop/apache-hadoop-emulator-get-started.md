---
title: Bir Apache Hadoop korumalı alanı - öykünücüsü - Azure HDInsight'ı kullanarak bilgi edinin
description: 'Apache Hadoop ekosistemindeki kullanma hakkında öğrenmeye başlayın için bir Azure sanal makinesinde Hortonworks Hadoop korumalı alan ayarlayabilirsiniz. '
keywords: hadoop öykünücüsü, hadoop korumalı alanı
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: hrasheed
ms.openlocfilehash: 2cb99cfe765e1d3444f362e591812f5088c78c0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393145"
---
# <a name="get-started-with-an-apache-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Bir Apache Hadoop korumalı alanı, bir sanal makinede bir öykünücü ile çalışmaya başlama

Apache Hadoop korumalı alanı Hortonworks Hadoop ekosistemi hakkında bilgi edinmek için bir sanal makineye yükleme konusunda bilgi edinin. Korumalı alan, Hadoop, Hadoop dağıtılmış dosya sistemi (HDFS) ve iş gönderme hakkında bilgi edinmek için bir yerel geliştirme ortamı sağlar. Hadoop ile ilgili bilgi sahibi olduktan sonra bir HDInsight kümesi oluşturarak Azure üzerinde Hadoop kullanmaya başlayabilirsiniz. Nasıl başlayacağınızı hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Önkoşullar
* [Oracle VirtualBox](https://www.virtualbox.org/). Buradan indirip [burada](https://www.virtualbox.org/wiki/Downloads).


## <a name="download-and-install-the-virtual-machine"></a>Sanal makineye yükleyip
1. Gözat [Cloudera indirir](https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html).

2. Tıklayın **VIRTUALBOX** altında **yükleme türünü seç** en son Hortonworks korumalı alanı bir VM'de indirilemedi. Oturum açın veya ürün ilgi formu doldurun.

1. Düğmesini **HDP korumalı alanı (en son)** karşıdan yükleme işlemini başlatın.

Korumalı alan ayarlama hakkında yönergeler için bkz: [korumalı alan dağıtım ve Yükleme Kılavuzu](https://hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/1/).

Eski bir HDP sürüm korumalı alan indirmek için altındaki bağlantıları görmek **eski sürümleri**.

## <a name="start-the-virtual-machine"></a>Sanal makineyi Başlat

1. Oracle VM VirtualBox açın.
2. Gelen **dosya** menüsünde, tıklayın **alma Gereci**ve ardından Hortonworks korumalı alanı görüntüsünü belirtin.
1. Hortonworks korumalı alanı seçip **Başlat**, ardından **Normal başlangıç**. Sanal makinenin önyükleme işlemi tamamlandıktan sonra oturum açma yönergeleri görüntüler.

    ![Normal Başlat](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Bir web tarayıcısı açın ve görüntülenen URL'ye gidin (genellikle `http://127.0.0.1:8888`).

## <a name="set-sandbox-passwords"></a>Korumalı alan parola ayarlama

1. Gelen **başlama** select Hortonworks Sandbox sayfası adımında **görünüm Gelişmiş Seçenekler**. Bilgiler, SSH kullanarak korumalı alan oturum açmak için bu sayfada kullanın. Sağlanan adını ve parolayı kullanın.

   > [!NOTE]
   > Bir SSH istemcisi yüklü değilse, sanal makine tarafından verilen web tabanlı SSH kullanabilirsiniz **http://localhost:4200/** .

    SSH kullanarak bağlandığınız ilk kez kök hesabın parolasını değiştirmeniz istenir. SSH kullanarak oturum açışınızda kullanacağınız yeni bir parola girin.

2. Oturum açtıktan sonra aşağıdaki komutu girin:

        ambari-admin-password-reset

    İstendiğinde, Ambari yönetici hesabı için bir parola sağlayın. Ambari Web kullanıcı arabirimini eriştiğinde kullanılır.

## <a name="use-hive-commands"></a>Hive komutlarını kullanma

1. Korumalı bir SSH bağlantısından, Hive kabuğunu başlatmak için aşağıdaki komutu kullanın:

        hive
2. Kabuk başlatıldıktan sonra korumalı alan ile sağlanan tabloları görüntülemek için aşağıdakileri kullanın:

        show tables;
3. 10 satırlardan almak için aşağıdakini kullanın `sample_07` tablosu:

        select * from sample_07 limit 10;

## <a name="next-steps"></a>Sonraki adımlar
* [Hortonworks korumalı alanı ile Visual Studio kullanmayı öğrenin](../hdinsight-hadoop-emulator-visual-studio.md)
* [Hortonworks korumalı alanı işin öğrenme](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop Öğreticisi - HDP ile çalışmaya başlama](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

