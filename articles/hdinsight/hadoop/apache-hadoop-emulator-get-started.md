---
title: "Bir Hadoop korumalı - öykünücüsü - Azure Hdınsight kullanarak öğrenin | Microsoft Docs"
description: "Hadoop ekosistemine kullanma hakkında öğrenmeye başlamak için bir Azure sanal makinesi üzerinde Hortonworks bir Hadoop korumalı ayarlayabilirsiniz. "
keywords: "hadoop öykünücüsü, hadoop korumalı alan"
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: d7df18a80470beb8dc25cf6add6b7a61f45dcfe7
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Bir Hadoop korumalı bir sanal makinede bir öykünücü kullanmaya başlama

Hadoop ekosistemi hakkında bilgi edinmek için sanal makinedeki Hortonworks Hadoop sandbox yüklemeyi öğrenin. Korumalı alan Hadoop, Hadoop dağıtılmış dosya sistemi (HDFS) ve iş gönderme hakkında bilgi edinmek için bir yerel geliştirme ortamı sağlar. Hadoop ile hakkında bilgi sahibi olduktan sonra Hdınsight kümesi oluşturarak Azure üzerinde Hadoop kullanmaya başlayabilirsiniz. Kullanmaya başlama hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Ön koşullar
* [Oracle VirtualBox](https://www.virtualbox.org/). Buradan yükleyip [burada](https://www.virtualbox.org/wiki/Downloads).



## <a name="download-and-install-the-virtual-machine"></a>Sanal makine yükleyip
1. Gözat [Hortonworks indirmeleri](http://hortonworks.com/downloads/#sandbox).

2. Tıklatın **İNDİRMEK için VIRTUALBOX** son Hortonworks korumalı alan bir VM üzerinde karşıdan yüklemek için. Yükleme başlamadan önce Hortonworks ile kaydetmeniz istenir. Ağ hızınıza bağlı olarak indirmek için bir ila iki saat sürer.
   
    ![İçin bağlantı görüntüsünü karşıdan yüklemek için VirtualBox Hortonworks korumalı alan](./media/apache-hadoop-emulator-get-started/download-sandbox.png)
3. Aynı web sayfasından tıklatın **sanal kutusundaki içeri aktarma** yükleme yönergeleri için sanal makine içeren PDF'yi indirmek için bağlantı.

Eski bir HDP sürüm korumalı alan indirmek için arşiv genişletin:

![Hortonworks Sandbox arşiv](./media/apache-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a>Sanal makineyi Başlat

1. Oracle VM VirtualBox açın.
2. Gelen **dosya** menüsünde tıklatın **alma Gereci**ve ardından Hortonworks Sandbox görüntüsünü belirtin.
1. Hortonworks korumalı alan seçin, **Başlat**ve ardından **Normal başlangıç**. Sanal makine önyükleme işlemi tamamlandıktan sonra oturum açma yönergeleri görüntüler.
   
    ![Normal Başlat](./media/apache-hadoop-emulator-get-started/normal-start.png)
2. Bir web tarayıcısı açın ve görüntülenen URL'ye (genellikle http://127.0.0.1:8888) gidin.

## <a name="set-sandbox-passwords"></a>Korumalı alan parola ayarlama

1. Gelen **başlama** Hortonworks Sandbox sayfasında, adım **görünüm Gelişmiş Seçenekler**. SSH kullanarak korumalı alan oturum açmak için bu sayfadaki bilgileri kullanın. Sağlanan adını ve parolayı kullanın.
   
   > [!NOTE]
   > Bir SSH istemcisi yüklü değilse sanal makine tarafından verilen web tabanlı SSH kullanabilirsiniz **http://localhost:4200 /**.
   > 
   
    SSH, bağlanırken ilk kez kök hesabının parolasını değiştirmeniz istenir. SSH kullanarak oturum açışınızda kullanacağınız yeni bir parola girin.

2. Oturum açtıktan sonra aşağıdaki komutu girin:
   
        ambari-admin-password-reset
   
    İstendiğinde, Ambari yönetici hesabı için bir parola sağlayın. Ambari Web kullanıcı arabirimini eriştiğinizde bu kullanılır.

## <a name="use-hive-commands"></a>Hive komutları kullanın

1. Korumalı bir SSH bağlantısından Hive kabuğunu başlatmak için aşağıdaki komutu kullanın:
   
        hive
2. Kabuk başladıktan sonra sandbox ile sağlanan tabloları görüntülemek için aşağıdakileri kullanın:
   
        show tables;
3. 10 satırları almak için aşağıdakini kullanın `sample_07` tablosu:
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Hortonworks Sandbox ile kullanmayı öğrenin](../hdinsight-hadoop-emulator-visual-studio.md)
* [Hortonworks Sandbox ipleri öğrenme](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop Öğreticisi - HDP ile çalışmaya başlama](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

