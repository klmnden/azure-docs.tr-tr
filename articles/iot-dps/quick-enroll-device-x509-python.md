---
title: "Python kullanarak X.509 cihazlarını Azure Cihaz Sağlama Hizmeti'ne kaydetme | Microsoft Docs"
description: "Azure Hızlı Başlangıç - Python sağlama hizmeti SDK'sını kullanarak X.509 cihazlarını Azure IoT Hub Cihazı Sağlama Hizmeti'ne kaydetme"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 01/25/2018
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9cd16ddcd5dfbcd68be37e2988e57604082a338f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="enroll-x509-devices-to-iot-hub-device-provisioning-service-using-python-provisioning-service-sdk"></a>Python sağlama hizmeti SDK'sını kullanarak X.509 cihazlarını IoT Hub Cihazı Sağlama Hizmeti'ne kaydetme
[!INCLUDE [iot-dps-selector-quick-enroll-device-x509](../../includes/iot-dps-selector-quick-enroll-device-x509.md)]

Bu adımlar örnek Python uygulamasından yardım alarak ve [Python Sağlama Hizmeti SDK'sını](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client) kullanarak bir grup X.509 sanal cihazını programlı bir şekilde Azure IoT Hub Cihazı Sağlama Hizmeti'ne kaydetmeyi göstermektedir. Java Hizmeti SDK'sı hem Windows hem de Linux makinelerinde çalışır, ancak bu makalede, kayıt işlemlerinin gösterilmesi için Windows geliştirme makinesi kullanılır.

Devam etmeden önce [IoT Hub Cihaz Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) adımlarını tamamladığınızdan emin olun.

> [!NOTE]
> Bu hızlı başlangıç yalnızca **Kayıt Grubunu** destekler. _Python Sağlama Hizmeti SDK'sı_ aracılığıyla **Bireysel Kayıt** hala yapım aşamasında.
> 

<a id="prepareenvironment"></a>

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. [Python 2.x veya 3.x](https://www.python.org/downloads/) sürümünü indirip yükleyin. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python'u eklediğinizden emin olun. 

1. Aşağıdaki seçeneklerden birini belirleyin:

    - **Azure IoT Python SDK'sını** oluşturup derleyin. Python paketlerini derlemek için [bu yönergeleri](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) uygulayın. Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketini](http://www.microsoft.com/download/confirmation.aspx?id=48145) ayrıca yükleyin.

    - [Python paket yönetim sistemi olan *pip*’i yükleyin](https://pip.pypa.io/en/stable/installing/) veya yükseltin ve şu komut aracılığıyla paketi yükleyin:

        ```cmd/sh
        pip install azure-iothub-provisioningserviceclient
        ```

1. Sağlama hizmetinize yüklenmiş ve doğrulanmış bir ara veya kök CA X.509 sertifikasını içeren bir .pem dosyasına ihtiyacınız vardır. **Azure IoT C SDK'sı** X.509 sertifika zinciri oluşturmanıza, bu zincirdeki bir kök veya ara sertifikayı yüklemenize ve sertifikayı doğrulamak için hizmette sahip olma onayı gerçekleştirmenize yardımcı olabilecek araçlar içerir. Bu araçları kullanmak için [Azure IoT C SDK'sını](https://github.com/Azure/azure-iot-sdk-c) kopyalayın ve makinenizde [azure-iot-sdk-c\tools\CACertificates\CACertificateOverview.md](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) içindeki adımları uygulayın.


## <a name="modify-the-python-sample-code"></a>Python örnek kodunu değiştirme

Bu bölümde örnek koda X.509 cihazınızın sağlama ayrıntılarını nasıl ekleyeceğiniz gösterilir. 

1. Metin düzenleyicisi kullanarak yeni bir **EnrollmentGroup.py** dosyası oluşturun.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini **EnrollmentGroup.py** dosyasının başlangıcına ekleyin. `dpsConnectionString` öğesini, **Azure Portal** üzerinde **Cihaz Sağlama Hizmeti**’nizdeki **Paylaşılan erişim ilkeleri** altında bulunan bağlantı dizenizle değiştirin. Sertifika yer tutucusunu, daha önce [Ortamı hazırlama](quick-enroll-device-x509-python.md#prepareenvironment) konusunda oluşturulan sertifika ile değiştirin. Son olarak, benzersiz bir `registrationid` oluşturun ve yalnızca küçük harf alfasayısal karakterler ve kısa çizgiler içerdiğinden emin olun.  
   
    ```python
    from provisioningserviceclient import ProvisioningServiceClient
    from provisioningserviceclient.models import EnrollmentGroup, AttestationMechanism

    CONNECTION_STRING = "{dpsConnectionString}"

    SIGNING_CERT = """-----BEGIN CERTIFICATE-----
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    -----END CERTIFICATE-----"""

    GROUP_ID = "{registrationid}"
    ```

1. Grup kaydı oluşturmasını uygulamak için aşağıdaki işlevi ve işlev çağrısını ekleyin:
   
    ```python
    def main():
        print ( "Initiating enrollment group creation..." )

        psc = ProvisioningServiceClient.create_from_connection_string(CONNECTION_STRING)
        att = AttestationMechanism.create_with_x509_signing_certs(SIGNING_CERT)
        eg = EnrollmentGroup.create(GROUP_ID, att)

        eg = psc.create_or_update(eg)
    
        print ( "Enrollment group created." )

    if __name__ == '__main__':
        main()
    ```

1. **EnrollmentGroup.py** dosyasını kaydedip kapatın.
 

## <a name="run-the-sample-group-enrollment"></a>Örnek grup kaydını çalıştırma

1. Bir komut istemi açın ve betiği çalıştırın.

    ```cmd/sh
    python EnrollmentGroup.py
    ```

1. Kaydın başarılı olup olmadığını görmek için çıktıyı gözlemleyin.

1. Azure portalında sağlama hizmetinize gidin. **Kayıtları yönetme**'ye tıklayın. X.509 cihaz grubunuzun daha önce oluşturulmuş `registrationid` adıyla **Kayıt Grupları** sekmesi altında göründüğüne dikkat edin. 

    ![Portalda X.509 kaydının başarılı olup olmadığını doğrulama](./media/quick-enroll-device-x509-python/1.png)  


## <a name="clean-up-resources"></a>Kaynakları temizleme
Java hizmeti örneğini keşfetmeye devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. Makinenizdeki Java örnek çıktı penceresini kapatın.
1. Makinenizde _X509 Cert Generator_ penceresini kapatın.
1. Azure portalındaki Cihaz Sağlama hizmetine gidin, **Kayıtları yönetme**'ye tıklayıp **Kayıt Grupları** sekmesini seçin. Bu Hızlı Başlangıç adımlarını kullanarak kaydettiğiniz X.509 cihazlarının *GRUP ADI* değerini seçip dikey pencerenin en üstünde bulunan **Sil** düğmesine tıklayın.  


## <a name="next-steps"></a>Sonraki adımlar
Bu Hızlı Başlangıçta sanal X.509 cihazlarından oluşan bir grubu Cihaz Sağlama hizmetinize kaydettiniz. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin. 

> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)
