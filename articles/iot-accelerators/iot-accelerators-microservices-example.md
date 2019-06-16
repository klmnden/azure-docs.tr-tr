---
title: Değiştirin ve yeniden bir mikro hizmet - Azure | Microsoft Docs
description: Bu öğreticide değiştirmek ve bir mikro hizmet Uzaktan izleme yeniden dağıtma işlemi gösterilmektedir
author: dominicbetts
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: 1552c54afe2195d58a032e9cc7bfa5aa70c844b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447650"
---
# <a name="customize-and-redeploy-a-microservice"></a>Bir mikro hizmeti özelleştirme ve yeniden dağıtma

Bu öğreticide birini düzenlemek gösterilir [mikro Hizmetler](https://azure.com/microservices) Uzaktan izleme çözümünde, mikro hizmet görüntüsü oluşturun, görüntüyü docker hub'ınızı dağıtmak ve sonra Uzaktan izleme çözümünde kullanabilirsiniz. Bu kavram, öğretici temel senaryo burada bir mikro hizmet API çağrısı ve durum ileti "Etkin tutma ve iyi" değiştirin "Yeni Made burada düzenlemeleri için!" kullanır

Uzaktan izleme çözümü, bir docker hub'ından çekilir docker görüntüleri kullanılarak oluşturulan bir mikro hizmetler kullanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Uzaktan izleme çözümünde bir mikro hizmet oluşturun ve düzenleyin
> * Bir docker görüntüsü oluşturun
> * Bir docker görüntüsünü docker hub'ına gönderme
> * Yeni docker görüntüsünü çekme
> * Değişiklikleri görselleştirin 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi uygulamak için ihtiyacınız vardır:

>[!div class="checklist"]
> * [Uzaktan izleme çözüm Hızlandırıcısını yerel olarak dağıtma](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Bir Docker hesabı](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - API yanıtı görüntülemek için gerekli

## <a name="call-the-api-and-view-response-status"></a>Arama API'si ve görünümü yanıt durumu

Bu bölümünde varsayılan IOT hub Yöneticisi mikro hizmet API çağrısı. API, daha sonra mikro hizmet özelleştirerek değiştiren bir durum iletisi döndürür.

1. Uzaktan izleme çözümü makinenizde yerel olarak çalışır durumda olduğundan emin olun.
2. Postman indirdiğiniz bulun ve açın.
3. Postman içinde GET aşağıdakileri girin: `http://localhost:8080/iothubmanager/v1/status`.
4. Görünümü "Durum" dönüş ve görmeniz gerekir: "Tamam: etkin ve iyi".

    ![Canlı ve iyi Postman iletisi](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Durumu değiştirmek ve görüntüyü oluşturma

"Yeni düzenlemeler burada yapılan!" için IOT Hub Yöneticisi mikro hizmet durum iletisi şimdi Değiştir ve ardından bu yeni durum docker görüntüsünü yeniden derleyin. Burada sorun yaşarsanız, başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Terminalinizi açık olduğundan emin olun ve Uzaktan izleme çözümü kopyaladığınız dizine geçin. 
1. Dizininizi "azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/Services için" değiştirin.
1. Herhangi bir metin düzenleyicisinde ya da istediğiniz gibi IDE StatusService.cs açın. 
1. Aşağıdaki kodu bulun:

    ```csharp
    var result = new StatusServiceModel(true, "Alive and well!");
    ```

    aşağıdaki kodla değiştirin ve dosyayı kaydedin.

    ```csharp
    var result = new StatusServiceModel(true, "New Edits Made Here!");
    ```

5. Terminalinizi için geri dönün, ancak artık aşağıdaki dizine geçin: "azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/scripts/docker".
6. Yeni docker görüntünüzü derlemek için aşağıdakileri yazın

    ```sh
    sh build
    ```
    
    veya Windows üzerinde:
    
    ```cmd
    ./build.cmd
    ```

7. Yeni görüntünüzü başarıyla oluşturuldu doğrulamak için aşağıdakileri yazın

    ```cmd/sh
    docker images 
    ```

Depo "azureiotpcs/iothub-manager-dotnet" olmalıdır.

![Başarılı bir docker görüntüsü](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Görüntüyü etiketleme ve gönderme
Yeni docker görüntüsünü docker hub'a gönderebilmek için önce Docker görüntülerinizi etiketlemek için bekliyor. Burada sorun yaşarsanız, başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Oluşturduğunuz yazarak docker görüntüsünü görüntü Kimliğini bulun:

    ```cmd/sh
    docker images
    ```

2. "Test" türüyle, görüntüyü etiketlemek için

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Docker hub'ınıza yeni etiketli görüntünüzü itme için şunu yazın

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Bir internet tarayıcısı açın ve gidin, [docker hub](https://hub.docker.com/) ve oturum açın.
5. Artık docker hub'ında yeni gönderilen docker görüntünüzü görmeniz gerekir.
![Docker görüntüsünü docker hub'a](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Uzaktan izleme çözümünüzü güncelleştir
Şimdi, yerel docker-docker hub, yeni docker görüntüsünü çekmek için compose.yml güncelleştirmeniz gerekir. Burada sorun yaşarsanız, başvurmak bizim [sorun giderme](#Troubleshoot) bölümü.

1. Terminale geri dönün ve aşağıdaki dizine geçin: "azure-iot-pcs-remote-monitoring-dotnet/services/scripts/local".
2. Docker-compose.yml herhangi bir metin düzenleyicisinde ya da istediğiniz gibi IDE açın.
3. Aşağıdaki kodu bulun:

    ```yml
    image: azureiotpcs/iothub-manager-dotnet:testing
    ```

    ve aşağıdaki gibi görünür ve kaydetmek için değiştirin.

    ```yml
    image: [docker ID]/iothub-manager-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Yeni yanıt durumunu görüntüleyin
Uzaktan izleme çözümünü yerel bir örneğini dağıtarak ve Postman içinde yeni durum yanıt görüntüleyerek sonlandırın.

1. Terminalinizi için geri dönün ve aşağıdaki dizine geçin: "azure-iot-pcs-remote-monitoring-dotnet/scripts/local".
2. Uzaktan izleme çözümü, yerel örneği, terminale aşağıdaki komutu yazarak başlatın:

    ```cmd/sh
    docker-compose up
    ```

3. Postman indirdiğiniz bulun ve açın.
4. Postman içinde aşağıdaki isteği Get girin: `http://localhost:8080/iothubmanager/v1/status`. Artık görmelisiniz, "Durum": "TAMAM: Burada yapılan yeni düzenlemeler! ".

![Yeni burada yaptığınız düzenlemeleri postman iletisi](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Sorun giderme

Bir sorunla karşılaşırsanız çalıştırıyorsanız, yerel makinede kapsayıcıları ve docker görüntüleri kaldırmayı deneyin.

1. Tüm kapsayıcıları kaldırmanız için önce tüm çalışan kapsayıcıları durdurmak gerekecektir. Tür ve terminal açın

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Tüm görüntüleri kaldırmak için türü ve terminal açın 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Varsa tüm kapsayıcıları makinede yazarak denetleyebilirsiniz.

    ```cmd/sh
    docker ps -aq 
    ```

    Tüm kapsayıcıları başarıyla kaldırılırsa, hiçbir şey gösterilmesi gerekir.

4. Varsa herhangi bir görüntü makinede yazarak denetleyebilirsiniz.

    ```cmd/sh
    docker images
    ```

    Tüm kapsayıcıları başarıyla kaldırılırsa, hiçbir şey gösterilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gördüğünüz nasıl yapılır:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Uzaktan izleme çözümünde bir mikro hizmet oluşturun ve düzenleyin
> * Bir docker görüntüsü oluşturun
> * Bir docker görüntüsünü docker hub'ına gönderme
> * Yeni docker görüntüsünü çekme
> * Değişiklikleri görselleştirin 

Denemek için bir sonraki şey [Uzaktan izleme çözümündeki cihaz simülatörü mikro özelleştirme](iot-accelerators-microservices-example.md)

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

