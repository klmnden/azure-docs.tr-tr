---
title: Yerel Service Fabric kümesinde Java uygulamasının hatasını ayıklama | Microsoft Docs
description: Bu öğreticide, yerel kümede çalıştırılan bir Service Fabric Java uygulamasından nasıl hata ayıklama yapılacağını ve günlüklerin alınacağını öğreneceksiniz.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: mfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: c5ff1a0373fcce339bea2b235d86f20dc861a15c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224055"
---
# <a name="tutorial-debug-a-java-application-deployed-on-a-local-service-fabric-cluster"></a>Öğretici: Yerel bir Service Fabric kümesinde dağıtılan bir Java uygulamasının hatasını ayıklama

Bu öğretici, bir dizinin ikinci bölümüdür. Eclipse for the Service Fabric uygulaması kullanılarak uzak bir hata ayıklayıcının nasıl ekleneceğini öğreneceksiniz. Ayrıca çalıştırılan uygulamalardaki günlüklerin geliştirici için uygun bir konuma nasıl yeniden yönlendirileceğini de öğreneceksiniz.

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Java Service Fabric Güvenilir Hizmetler uygulaması derleme](service-fabric-tutorial-create-java-app.md)
> * Yerel kümede uygulamayı dağıtma ve uygulamanın hatasını ayıklama
> * [Azure kümesine uygulama dağıtma](service-fabric-tutorial-java-deploy-azure.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-java-elk.md)
> * [CI/CD ayarlama](service-fabric-tutorial-java-jenkins.md)


Serinin ikinci bölümünde şunları öğrenirsiniz:
> [!div class="checklist"]
> * Eclipse kullanarak Java uygulamasının hatasını ayıklama
> * Günlükleri yapılandırılabilir bir konuma yeniden yönlendirme


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* [Mac](service-fabric-get-started-mac.md) veya [Linux](service-fabric-get-started-linux.md) için geliştirme ortamınızı ayarlayın. Eclipse eklentisini, Gradle’ı, Service Fabric SDK’yı ve Service Fabric CLI’yı (sfctl) yükleme yönergelerini izleyin.

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-java-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

[Derleme ve dağıtma](service-fabric-tutorial-create-java-app.md#deploy-application-to-local-cluster) uygulamaya yerel geliştirme kümesi.

## <a name="debug-java-application-using-eclipse"></a>Eclipse kullanarak Java uygulamasının hatasını ayıklama

1. Makinenizde Eclipse IDE’yi açın ve **Dosya -> İçeri Aktar....** seçeneklerine tıklayın.

2. Açılan pencerede **Genel -> Mevcut Projeleri Çalışma Alanına** seçeneğini belirleyin ve İleri düğmesine basın.

3. Projeleri İçeri Aktar penceresinde **Kök dizini seçin** seçeneğini belirleyin ve **Oylama** dizinini seçin. Birinci öğretici serisini izlediyseniz **Oylama** dizini, **Eclipse çalışma alanı** dizinindedir.

4. Hatasını ayıklamak istediğiniz hizmetin entryPoint.sh öğesini güncelleştirin; böylece hizmet, uzaktan hata ayıklama parametreleriyle Java işlemini başlatır. Bu öğretici için durum bilgisi olmayan ön uç kullanılır: *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh*. Bu örnekte, hata ayıklama için 8001 numaralı bağlantı noktası ayarlanmıştır.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingWeb.jar
    ```

5. Hatası ayıklanmakta olan hizmet için çoğaltma sayısını veya örnek sayısını ayarlayarak Uygulama Bildirimini güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmaları önler. Örneğin, şu işlemleri izleyerek durum bilgisi olmayan hizmetler için ``InstanceCount="1"`` ayarını yapın ve durum bilgisi olan hizmetler için hedefi ve en az çoğaltma kümesi boyutlarını 1 olarak ayarlayın: ``TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

6. Eclipse IDE’de **Çalıştır -> Hata Ayıklama Yapılandırmaları -> Uzak Java Uygulaması** seçeneklerini belirleyin, **Yeni** düğmesine basın, aşağıdaki şekilde özellikleri ayarlayın ve **Uygula**’ya tıklayın.

    ```
    Name: Voting
    Project: Voting
    Connection Type: Standard
    Host: localhost
    Port: 8001
    ```

7. *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java* dosyasının 109. satırına bir kesme noktası ekleyin.

8. Paket Gezgini’nde **Oylama** projesine sağ tıklayın ve **Service Fabric -> Uygulama Yayımla ...** seçeneklerine tıklayın.

9. **Uygulama Yayımla** penceresinde açılan listeden **Local.json** seçeneğini belirleyin ve **Yayımla**’ya tıklayın.

10. Eclipse IDE’de **Çalıştır -> Hata Ayıklama Yapılandırmaları -> Uzak Java Uygulaması** seçeneklerini belirleyin, oluşturduğunuz **Oylama** yapılandırmasına ve sonra **Hata Ayıklama**’ya tıklayın.

11. Web tarayıcısı ve erişim Git **8080**. Bu kesme noktasını otomatik olarak ulaşırsınız ve Eclipse girin **hata ayıklama perspektifi**.

Artık herhangi bir Service Fabric Eclipse uygulamada hata ayıklamak için aynı adımları uygulayabilirsiniz.

## <a name="redirect-application-logs-to-custom-location"></a>Uygulama günlüklerini özel konuma yeniden yönlendirme

Aşağıdaki adımlarda, varsayılan */var/log/syslog* konumundaki uygulama günlüklerinin özel bir konuma nasıl yeniden yönlendirileceği gösterilmektedir.

1. Şu anda yalnızca Service Fabric Linux kümelerinde çalıştırılan uygulamalar tek bir günlük dosyasının seçilmesini destekler. Uygulama günlükleri her zaman Git şekilde ayarlamak için */tmp/mysfapp0.0.log*, voting/votingapplication/votingwebpkg/Code/Logging.Properties konumunda Logging.Properties adlı bir dosya oluşturun *Voting/VotingApplication/VotingWebPkg/Code/logging.properties*  ve aşağıdaki içeriği ekleyin.

    ```
    handlers = java.util.logging.FileHandler

    java.util.logging.FileHandler.level = ALL
    java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter

    # This value specifies your custom location.
    # You will have to ensure this path has read and write access by the process running the SF Application
    java.util.logging.FileHandler.pattern = /tmp/mysfapp0.0.log
    ```

2. Java yürütme komutu için *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh* konumuna aşağıdaki parametreyi ekleyin:

    ```bash
    -Djava.util.logging.config.file=logging.properties
    ```

    Aşağıdaki örnek, önceki bölümde örnek bir yürütme hata ayıklayıcısı ile bağlanmış, yürütme benzer gösterir.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=logging.properties -jar VotingWeb.jar
    ```

Bu aşamada, Service Fabric Java uygulamalarınızı geliştirirken nasıl hata ayıklaması yapacağınızı ve uygulama günlüklerinize erişeceğinizi öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Eclipse kullanarak Java uygulamasının hatasını ayıklama
> * Günlükleri yapılandırılabilir bir konuma yeniden yönlendirme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Uygulamayı Azure'a dağıtma](service-fabric-tutorial-java-deploy-azure.md)