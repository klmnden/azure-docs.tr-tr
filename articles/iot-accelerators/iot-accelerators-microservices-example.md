---
title: Değiştirme ve bir mikro hizmet dağıtmanız | Microsoft Docs
description: Bu öğreticide, değiştirme ve Uzaktan izleme mikro hizmet dağıtmanız gösterilir
services: ''
suite: iot-suite
author: giyeh
manager: hegate
ms.author: giyeh
ms.service: iot-suite
ms.date: 04/19/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3d79c085d10515183a5ddcc12ecac503915eb2e2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="customize-and-redeploy-a-microservice"></a>Özelleştirme ve bir mikro hizmet yeniden dağıtın

Bu öğretici Uzaktan izleme çözümünde mikro birini düzenlemek, mikro hizmet görüntüsünü oluşturmak, docker hub'ınıza görüntüyü dağıtmak ve Uzaktan izleme çözümünde kullanın gösterilmektedir. Bu kavram tanıtmak için öğretici temel senaryo burada mikro hizmet API çağrısı ve durum iletiden "Canlı ve iyi" değiştirmek "Yeni Made burada düzenler için!" kullanır

Uzaktan izleme çözümü docker hub'dan çekilen docker görüntüleri kullanılarak oluşturulan mikro kullanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Düzenle ve Uzaktan izleme çözümünde mikro hizmet oluşturma
> * Docker yansıması oluştur
> * Docker görüntü docker hub'ına anında iletme
> * Yeni docker görüntüsü isteme
> * Değişiklikleri Görselleştirme 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izleyin, gerekir:

>[!div class="checklist"]
> * [Uzaktan izleme önceden yapılandırılmış çözümü yerel olarak dağıtma](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Docker hesabı](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - API yanıtını görüntülemek için gerekli

## <a name="call-the-api-and-view-response-status"></a>Arama API ve görünüm yanıt durumu

Bu bölümünde, varsayılan IOT hub Yöneticisi mikro API çağrısı. API mikro hizmet özelleştirerek değiştirmek daha sonra bir durum iletisi döndürür.

1. Uzaktan izleme çözümü makinenizde yerel olarak çalışır durumda olduğundan emin olun.
2. Postman indirdiğiniz bulun ve açın.
3. Postman içinde GET aşağıdakileri girin: http://localhost:8080/iothubmanager/v1/status.
4. Dönüş görüntülemek ve görmeniz gerekir, "Durum": "Tamam: Canlı ve iyi".
![Canlı ve iyi Postman iletisi](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Durumu değiştirmek ve görüntü oluşturma

"Yeni düzenlemeleri burada yapılan!" için IOT Hub Yöneticisi mikro hizmet durum iletisini şimdi Değiştir ve bu yeni durum docker görüntüsüyle yeniden derleyin. Burada sorunları çalıştırırsanız başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Terminalinizde açık olduğundan emin olun ve Uzaktan izleme çözümü kopyaladığınız dizine geçin. 
2. Dizininize değiştirme ".. azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/WebService/v1/Controllers".
3. Herhangi bir metin düzenleyicisi veya istediğiniz IDE StatusController.cs açın. 
4. Aşağıdaki kodu bulun:

    ```javascript
    return new StatusApiModel(true, "Alive and well");
    ```

    aşağıdaki kodla değiştirin ve dosyayı kaydedin.

    ```javascript
    return new StatusApiModel(true, "New Edits Made Here!");
    ```

5. Terminalinizde için geri dönün, ancak şimdi aşağıdaki dizine geçin: "... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/scripts/docker".
6. Yeni docker görüntünüzü oluşturmak için şunu yazın

    ```cmd/sh
    sh build
    ```

7. Yeni görüntünüzü başarıyla oluşturuldu doğrulamak için aşağıdakileri yazın

    ```cmd/sh
    docker images 
    ```

Depo "azureiotpcs/iothub-manager-dotnet" olmalıdır.

![Başarılı docker görüntüsü](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Etiket ve anında iletme resmi
Yeni docker görüntünüzü bir docker hub'a anında önce Docker görüntülerinizi etiketlenmesi bekliyor. Burada sorunları çalıştırırsanız başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Yazarak oluşturulan docker görüntüsü Görüntü Kimliğini bulun:

    ```cmd/sh
    docker images
    ```

2. "Test" türüyle görüntünüzü etiketlemek için

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Yeni etiketli görüntünüzü docker hub'ınıza göndermek için yazın

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Internet tarayıcınızı açın ve gidin, [docker hub'a](https://hub.docker.com/) ve oturum açın.
5. Şimdi, docker hub'ına yeni basılmış docker görüntünüzü görmeniz gerekir.
![Docker hub'a docker görüntüde](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Uzaktan izleme çözümünüz güncelleştir
Şimdi, yerel docker-docker hub'ınızı yeni, docker görüntüden çekmesini compose.yml güncelleştirmeniz gerekir. Burada sorunları çalıştırırsanız başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Terminale geri dönün ve şu dizine değiştirin: "... Azure-iot-PCS-Remote-Monitoring-dotnet/scripts/Local".
2. Docker-compose.yml herhangi bir metin düzenleyicisi veya istediğiniz IDE açın.
3. Aşağıdaki kodu bulun:

    ```docker
    image: azureiotpcs/pcs-auth-dotnet:testing
    ```

    ve aşağıdaki görüntü gibi arayın ve kaydetmek için değiştirin.

    ```cmd/sh
    image: [docker ID]/pcs-auth-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Yeni yanıt durumunu görüntüleme
Uzaktan izleme çözümünü yerel bir örneğini yükleyecek ve yeni bir durum yanıtı Postman içinde görüntüleyerek sonlandırın.

1. Terminalinizde için geri dönün ve şu dizine değiştirin: "... Azure-iot-PCS-Remote-Monitoring-dotnet/scripts/Local".
2. Yerel Uzaktan izleme çözümü örneğiniz terminale aşağıdaki komutu yazarak başlatın:

    ```cmd/sh
    docker-compose up
    ```

3. Postman indirdiğiniz bulun ve açın.
4. Aşağıdaki isteği Get Postman içinde girin: http://localhost:8080/iothubmanager/v1/status. Artık görmelisiniz, "Durum": "Tamam: yeni düzenlemeleri yapılan burada!".

![Yeni burada yapılan düzenlemeleri postman iletisi](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Sorun giderme

Sorunla çalıştırıyorsanız, yerel makinede kapsayıcıları ve docker görüntüleri kaldırmayı deneyin.

1. Tüm kapsayıcıları kaldırmak için önce tüm çalışan kapsayıcılar durdurmak gerekir. Terminal türü açın

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Tüm görüntüleri kaldırmak için terminal açın ve şunu yazın 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Olup olmadığını herhangi bir kapsayıcıdaki makinede yazarak kontrol edebilirsiniz

    ```cmd/sh
    docker ps -aq 
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız, hiçbir şey gösterilmesi gerekir.

4. Olup olmadığını tüm görüntüleri makinede yazarak kontrol edebilirsiniz

    ```cmd/sh
    docker images
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız, hiçbir şey gösterilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gördüğünüz nasıl yapılır:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Düzenle ve Uzaktan izleme çözümünde mikro hizmet oluşturma
> * Docker yansıması oluştur
> * Docker görüntü docker hub'ına anında iletme
> * Yeni docker görüntüsü isteme
> * Değişiklikleri Görselleştirme 

Denemek için İleri şey [Uzaktan izleme çözümünde aygıt benzeticisi mikro özelleştirme](iot-accelerators-remote-monitoring-test.md)

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

