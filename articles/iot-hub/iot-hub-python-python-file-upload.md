---
title: Python ile Azure IOT Hub'ına aygıtlardan dosyaları karşıya yükleme | Microsoft Docs
description: Python için Azure IOT cihaz SDK'sını kullanarak buluta bir aygıttan dosyaları karşıya yükleme yapma. Karşıya yüklenen dosyaların bir Azure depolama blob kapsayıcısında depolanır.
services: iot-hub
documentationcenter: python
author: kgremban
manager: timlt
editor: ''
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: kgremban
ms.openlocfilehash: 7f64783f5e1c79436b671ef98f30f5e3594b94e6
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>IOT Hub ile bulut cihazınızdan dosyaları karşıya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici nasıl kullanılacağını izleyen [dosya karşıya yükleme özellikleri IOT Hub'ın](iot-hub-devguide-file-upload.md) bir dosyayı karşıya yüklemeyi [Azure blob depolama](../storage/index.yml). Öğretici şunların nasıl yapıldığını gösterir için:

- Güvenli bir şekilde bir dosya yüklemek için bir depolama kapsayıcısı sağlar.
- IOT hub'ınız aracılığıyla bir dosyayı karşıya yüklemek için Python istemcisini kullanın.

[IOT Hub ile çalışmaya başlama](iot-hub-node-node-getstarted.md) öğretici, IOT hub'ı temel cihaz bulut Mesajlaşma işlevlerini gösterir. Ancak, bazı senaryolarda aygıtlarınızı IOT hub'ı kabul görece küçük bir cihaz bulut iletilerini göndermek verileri kolayca eşlenemiyor. Bir aygıttan upland dosyalara ihtiyacınız olduğunda, güvenlik ve güvenilirlik IOT Hub'ının kullanmaya devam edebilirsiniz.

> [!NOTE]
> IOT Hub Python SDK'sı şu anda yalnızca destekler karakter tabanlı dosyaları gibi yükleme **.txt** dosyaları.

Bu öğreticinin sonunda Python konsol uygulamasını çalıştırın:

* **FileUpload.py**, Python cihaz SDK'sını kullanarak depolama için bir dosya yükler.

> [!NOTE]
> IOT Hub, birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla (C, .NET, Javascript, Python ve Java dahil) dilleri destekler. Başvurmak [Azure IOT Geliştirme Merkezi] Cihazınızı Azure IOT Hub'ına bağlamak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x][lnk-python-download]. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz][lnk-install-pip] gerekebilir.
* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi][lnk-visual-c-redist].
* Etkin bir Azure hesabı. (Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)


[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Bir aygıt uygulamasından bir dosyayı karşıya yüklemek

Bu bölümde, bir dosyayı karşıya yüklemeyi IOT hub'ına cihaz uygulaması oluşturun.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-iothub-aygıt-client** paketi:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **FileUpload.py** çalışma klasörünüzdeki dosya.

1. Aşağıdakileri ekleyin `import` deyimleri ve değişkenleri başlangıcında **FileUpload.py** dosya. Değiştir `deviceConnectionString` cihazınızın IOT hub bağlantı dizesine sahip:

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name on storage after upload]"
    ```

1. İçin bir geri çağırma oluşturun **upload_blob** işlevi:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. İstemci bağlanma ve dosyayı karşıya yüklemek için aşağıdaki kodu ekleyin. De dahil `main` yordamı:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Kaydet ve Kapat **UploadFile.py** dosya.

1. Örnek bir metin dosyası için çalışma klasörü kopyalamak ve yeniden adlandırmak `sample.txt`.

    > [!NOTE]
    > IOT Hub Python SDK'sı şu anda yalnızca destekler karakter tabanlı dosyaları gibi yükleme **.txt** dosyaları.


## <a name="run-the-application"></a>Uygulamayı çalıştırma

Şimdi uygulamayı çalıştırmak hazırsınız.

1. Çalışma klasöründeki bir komut isteminde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python FileUpload.py
    ```

1. Aşağıdaki ekran görüntüsünde çıktısını gösterir **dosya yükleme** uygulama:

    ![Simulated-device uygulamadan çıktı](./media/iot-hub-python-python-file-upload/1.png)

1. Yüklenen dosya yapılandırdığınız depolama kapsayıcısı içinde görüntülemek için portalı kullanabilirsiniz:

    ![Yüklenen dosya](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, dosya yüklemelerini basitleştirmek için IOT hub'ı dosya karşıya yükleme becerilerini kullanacak öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [IOT hub'ı program aracılığıyla oluşturma][lnk-create-hub]
* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

<!-- Links -->
[Azure IOT Geliştirme Merkezi]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
