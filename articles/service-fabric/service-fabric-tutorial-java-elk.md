---
title: Azure'da ELK kullanarak Service Fabric’teki uygulamalarınızı izleme | Microsoft Docs
description: Bu öğreticide, ELK’nın nasıl ayarlanacağını ve Service Fabric uygulamalarınızın nasıl izleneceğini öğrenin.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
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
ms.openlocfilehash: 689207339db0250d42fc64c33f43c42c18317d41
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388732"
---
# <a name="tutorial-monitor-your-service-fabric-applications-using-elk"></a>Öğretici: ELK kullanarak Service Fabric uygulamalarınızı izleme

Bu öğretici, bir serinin dördüncü bölümüdür. Azure’da çalıştırılan Service Fabric uygulamalarını izlemek için ELK’nın (Elasticsearch, Logstash ve Kibana) nasıl kullanılacağını gösterir.

Serinin dördüncü kısmında öğrenecekleriniz:
> [!div class="checklist"]
> * Azure’da ELK sunucusunu ayarlama
> * Event Hubs’tan günlükleri almak için Logstash’i yapılandırma
> * Kibana’da platform ve uygulama günlüklerini görselleştirme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Java Service Fabric Güvenilir Hizmetler uygulaması derleme](service-fabric-tutorial-create-java-app.md)
> * [Yerel kümede uygulamayı dağıtma ve uygulamanın hatasını ayıklama](service-fabric-tutorial-debug-log-local-cluster.md)
> * [Azure kümesine uygulama dağıtma](service-fabric-tutorial-java-deploy-azure.md)
> * Uygulama için izleme ve tanılamayı ayarlama
> * [CI/CD ayarlama](service-fabric-tutorial-java-jenkins.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* Günlükleri, [ikinci kısımda](service-fabric-tutorial-debug-log-local-cluster.md) belirtilen konuma göndermek için uygulamanızı ayarlayın.
* [Üçüncü kısmı](service-fabric-tutorial-java-deploy-azure.md) tamamlayın ve çalıştırılan bir Service Fabric kümesini, günlükler Event Hubs’a gönderilecek şekilde yapılandırın.
* 'Dinleme' iznine ve üçüncü serideki ilişkilendirilmiş birincil anahtara sahip olan Event Hubs’taki ilke.

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-java-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

## <a name="create-an-elk-server-in-azure"></a>Azure’da ELK sunucusu oluşturma

Bu öğretici için önceden yapılandırılmış bir ELK ortamı kullanabilirsiniz ve varsa, **Logstash ayarlama** bölümüne atlayın. Ancak yoksa, Azure’da aşağıdaki adımlarla bir tane oluşturulabilir.

1. Azure’da [Bitnami](https://ms.portal.azure.com/#create/bitnami.elk4-6) tarafından Onaylı bir ELK oluşturun. Öğreticinin amacı doğrultusunda, bu sunucunun oluşturulması için izlenecek belirli bir belirtim yoktur.

2. Azure portalında kaynağınıza gidin ve **Destek + Sorun Giderme** bölümündeki **Önyükleme Tanılaması** sekmesine girin. Ardından **Seri Günlüğü** sekmesine tıklayın.

    ![Önyükleme Tanılamaları](./media/service-fabric-tutorial-java-elk/bootdiagnostics.png)
3. Kibana örneğine erişmek için, günlüklerde parola araması yapılması gerekir. Aşağıdaki kod parçacığına benzer:

    ```bash
    [   25.932766] bitnami[1496]: #########################################################################
    [   25.948656] bitnami[1496]: #                                                                       #
    [   25.962448] bitnami[1496]: #        Setting Bitnami application password to '[PASSWORD]'           #
    [   25.978137] bitnami[1496]: #        (the default application username is 'user')                   #
    [   26.004770] bitnami[1496]: #                                                                       #
    [   26.029413] bitnami[1496]: #########################################################################
    ```

4. Oturum açma ayrıntılarına erişmek için Azure portalında sunucunun Genel Bakış sayfasındaki bağlan düğmesine basın.

    ![VM Bağlantısı](./media/service-fabric-tutorial-java-elk/vmconnection.png)

5. Aşağıdaki komutu kullanarak ELK görüntüsünü barındıran sunucuda SSH

    ```bash
    ssh [USERNAME]@[CONNECTION-IP-OF-SERVER]

    Example: ssh testaccount@104.40.63.157
    ```

## <a name="set-up-elk"></a>ELK ayarlama

1. Birinci adım, ELK ortamının yüklenmesidir

    ```bash
    sudo /opt/bitnami/use_elk
    ```

2. Mevcut bir ortamı kullanıyorsanız, Logstash hizmetini durdurmak için aşağıdaki komutu çalıştırmanız gerekir

    ```bash
    sudo /opt/bitnami/ctlscript.sh stop logstash
    ```

3. Event Hubs için Logstash eklentisini yüklemek amacıyla aşağıdaki komutu çalıştırın.

    ```bash
    logstash-plugin install logstash-input-azureeventhub
    ```

4. Oluşturma veya aşağıdaki içeriklerle mevcut Logstash yapılandırma dosyanızı değiştirin: Dosyayı oluşturuyorsanız, oluşturulacak sahip ```/opt/bitnami/logstash/conf/access-log.conf``` Azure'da ELK Bitnami görüntüsünün kullanıyorsanız.

    ```json
    input
    {
        azureeventhub
        {
            key => "[RECEIVER-POLICY-KEY-FOR-EVENT-HUB]"
            username => "[RECEIVER-POLICY-NAME]"
            namespace => "[EVENTHUB-NAMESPACENAME]"
            eventhub => "[EVENTHUB-NAME]"
            partitions => 4
        }
    }

    output {
         elasticsearch {
             hosts => [ "127.0.0.1:9200" ]
         }
     }
    ```

5. Yapılandırmayı doğrulamak için aşağıdaki komutu çalıştırın:

    ```bash
    /opt/bitnami/logstash/bin/logstash -f /opt/bitnami/logstash/conf/ --config.test_and_exit
    ```

6. Logstash hizmetini başlatın

    ```bash
    sudo /opt/bitnami/ctlscript.sh start logstash
    ```

7. Verileri aldığınızdan emin olmak için Elasticsearch’ü denetleyin

    ```bash
    curl 'localhost:9200/_cat/indices?v'
    ```

8. Kibana panonuza erişin **http:\//SERVER-IP** ve Kibana için kullanıcı adı ve parola girin. Azure’da ELK görüntüsünü kullandıysanız varsayılan kullanıcı adı 'user' ve parola da **Önyükleme Tanılaması**’ndan alınan paroladır.

    ![Kibana](./media/service-fabric-tutorial-java-elk/kibana.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure’da ELK sunucusunu çalışır duruma getirme
> * Service Fabric kümenizden tanılama bilgilerini alacak şekilde sunucuyu yapılandırma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Jenkins ile CI/CD ayarlama](service-fabric-tutorial-java-jenkins.md)
