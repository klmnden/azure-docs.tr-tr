---
title: Python ile Azure IOT Hub'ına cihazlardan dosya yükleme | Microsoft Docs
description: Python için Azure IOT cihaz SDK'sını kullanarak bulutta bir CİHAZDAN dosyaları karşıya yükleme. Karşıya yüklenen dosyaları bir Azure depolama blob kapsayıcısında depolanır.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: kgremban
ms.openlocfilehash: 23b0a2ac8e0264ddc1592479759cc8398d9ef5f8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621276"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Cihazınızı IOT Hub ile buluta dosyaları karşıya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu makalede nasıl kullanılacağını gösterir [dosya karşıya yükleme özellikleri IOT Hub'ın](iot-hub-devguide-file-upload.md) bir dosyayı karşıya yüklemek için [Azure blob depolama](../storage/index.yml). Öğretici şunların nasıl yapıldığını göstermektedir:

* Bir dosya karşıya yükleme için güvenli bir şekilde bir depolama kapsayıcısı sağlar.

* IOT hub'ınız aracılığıyla bir dosyayı karşıya yüklemek için Python istemcisini kullanın.

[Telemetri gönderir bir CİHAZDAN bir IOT hub'ına](quickstart-send-telemetry-python.md) hızlı başlangıç, IOT hub'ı temel cihaz-bulut Mesajlaşma işlevlerini gösterir. Ancak, bazı senaryolarda cihazlarınızı IOT hub'ı kabul görece küçük bir CİHAZDAN buluta ileti gönderme verileri kolayca eşlenemiyor. Bir CİHAZDAN upland dosyalara ihtiyacınız olduğunda, güvenlik ve güvenilirlik IOT hub'ı kullanmaya devam edebilirsiniz.

> [!NOTE]
> IOT Hub Python SDK'sı şu anda yalnızca destekler karakter tabanlı dosyaları gibi karşıya **.txt** dosyaları.

Bu öğreticinin sonunda bir Python konsol uygulaması çalıştırın:

* **FileUpload.py**, Python cihaz SDK'sı kullanarak depolama için bir dosya yükler.

> [!NOTE]
> IOT hub'ı, çok sayıda cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla (C, .NET, Javascript, Python ve Java dahil) dilleri destekler. Başvurmak [Azure IOT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot) Cihazınızı Azure IOT Hub'ına bağlanmak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2.x veya 3.x](https://www.python.org/downloads/). Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*'yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.

* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi](https://www.microsoft.com/download/confirmation.aspx?id=48145).

* Etkin bir Azure hesabı. Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.

* IOT hub, Azure hesabınızda, karşıya dosya yükleme özelliğiyle test etmek için bir cihaz kimliği. 

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir cihaz uygulamasından bir dosyayı karşıya yükleyin

Bu bölümde, IOT hub'ına bir dosyayı karşıya yüklemek için cihaz uygulaması oluşturun.

1. Komut isteminizde yüklemek için aşağıdaki komutu çalıştırın **azure-iothub-cihaz-client** paket:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Bir metin düzenleyicisi kullanarak, blob depolama alanına yükleyeceği bir test dosyası oluşturun.

    > [!NOTE]
    > IOT Hub Python SDK'sı şu anda yalnızca destekler karakter tabanlı dosyaları gibi karşıya **.txt** dosyaları.

3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **FileUpload.py** çalışma klasöründeki dosya.

4. Aşağıdaki `import` deyimlerini ve değişkenlerini başlangıcında **FileUpload.py** dosya. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

5. Dosyanızda değiştirin `[Device Connection String]` cihazınızın IOT hub bağlantı dizesine sahip. Değiştirin `[Full path to file]` ile oluşturduğunuz test dosyası veya Cihazınızda karşıya yüklemek istediğiniz herhangi bir dosya yolu. Değiştirin `[File name for storage]` ile blob depolama alanına karşıya yüklendikten sonra bir dosyaya vermek istediğiniz adı. 

6. İçin bir geri çağırma oluşturun **upload_blob** işlevi:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

7. İstemci bağlanma ve dosyanın karşıya yüklemek için aşağıdaki kodu ekleyin. De `main` yordam:

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

8. Kaydet ve Kapat **UploadFile.py** dosya.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak hazırsınız.

1. Çalışma klasöründeki bir komut isteminde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python FileUpload.py
    ```

2. Aşağıdaki ekran görüntüsünde çıktısında **FileUpload** uygulama:

    ![Simulated-device uygulama çıktısı](./media/iot-hub-python-python-file-upload/1.png)

3. Karşıya yüklenen dosya yapılandırdığınız depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

    ![Karşıya yüklenen dosya](./media/iot-hub-python-python-file-upload/2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlardan karşıya dosya yükleme işlemleri basitleştirmek için dosya karşıya yükleme özellikleri IOT hub'ı kullanmayı öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlamalı IOT hub oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)
