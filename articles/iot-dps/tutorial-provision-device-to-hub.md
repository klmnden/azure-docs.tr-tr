---
title: Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak cihaz sağlama | Microsoft Docs
description: Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak tek bir IoT hub’a cihazınızı sağlama
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/12/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9f151a8fbcdc20124467a1db290f6a05f574e4fe
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak bir IoT hub’a cihazı sağlama

Önceki öğreticide, Cihaz Sağlama hizmetine bağlanmak için bir cihazın nasıl ayarlanacağını öğrendiniz. Bu öğreticide, ootmatik sağlama ve **_kayıt listelerini_** kullanarak, cihazınızı tek bir IoT hub’a sağlamak için bu hizmetin nasıl kullanılacağını öğreneceksiniz. Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Cihazı kaydedin
> * Cihazı başlatın
> * Cihaz kayıtlı olduğunu doğrulayın

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, [Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak bir cihazı sağlamak üzere ayarlama](./tutorial-set-up-device.md) öğreticisinde açıklandığı gibi cihazınızı yapılandırdığınızdan emin olun.

Otomatik sağlama işlemini bilmiyorsanız, devam etmeden önce [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun.

<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Cihazı kaydedin

Bu adım, cihazın benzersiz güvenlik yapılarının Cihaz Sağlama Hizmeti’ne eklenmesini kapsar. Bu güvenlik yapıları, aşağıdaki gibi cihazın [Kanıtlama mekanizması](concepts-device.md#attestation-mechanism)’nı temel alır:

- TPM tabanlı cihazlar için şunlar gerekir:
    - TPM yongası veya simülasyonu için benzersiz olan, TPM yonga üreticisinden alınan *Onay Anahtarı*.  Daha fazla bilgi için [TPM Onay Anahtarını Anlama](https://technet.microsoft.com/library/cc770443.aspx) bölümünü okuyun.
    - Ad alanındaki/kapsamdaki bir cihazı benzersiz şekilde tanımlamak için kullanılan *Kayıt Kimliği*. Bu kimlik, cihaz kimliğiyle aynı olabilir veya olmayabilir. Kimlik her cihaz için zorunludur. TPM tabanlı cihazlar için kayıt kimliği, TPM’den türetilebilir; örneğin, TPM Onay Anahtarının SHA-256 karması.

    [![Portalda TPM için kayıt bilgileri](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png#lightbox)  

- X.509 tabanlı cihazlar için şunlar gerekir:
    - *.pem* veya *.cer* dosyası şeklinde [X.509 yongasına ya da simülasyonuna düzenlenen sertifika](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx). Bireysel kayıt için, X.509 sisteminiz için cihaz başına *imzalayan sertifikayı* kullanmanız, kayıt grupları içinse *kök sertifika* kullanmanız gerekir. 

    [![Portalda X.509 kanıtı için tek kayıt ekleme](./media/tutorial-provision-device-to-hub/individual-enrollment.png)](./media/tutorial-provision-device-to-hub/individual-enrollment.png#lightbox)

Cihaz Sağlama Hizmeti’ne cihazı kaydetmenin iki yolu vardır:

- **Kayıt Grupları** Bu, belirli bir kanıtlama mekanizmasını paylaşan bir aygıt grubunu temsil eder. İstenen bir ilk yapılandırmayı paylaşan çok sayıda cihaz için veya aynı kiracıya giden tüm cihazlar için bir kayıt grubu kullanılmasını öneririz.

    [![Portalda X.509 kanıtı için grup kaydı ekleme](./media/tutorial-provision-device-to-hub/group-enrollment.png)](./media/tutorial-provision-device-to-hub/group-enrollment.png#lightbox)

- **Bireysel Kayıtlar** Bu, Cihaz Sağlama Hizmeti’ne kaydolabilecek tek bir cihaz için bir girdiyi temsil eder. Bireysel kayıtlar, kanıtlama mekanizması olarak x509 sertifikalarını veya SAS belirteçlerini (gerçek ya da sanal TPM’de) kullanabilir. Benzersiz ilk yapılandırma gerektiren cihazlar için veya kanıtlama mekanizması olarak TPM ya da sanal TPM aracılığıyla yalnızca SAS belirteçlerini kullanabilen cihazlar için bireysel kayıtların kullanılmasını öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

Şimdi cihazın kanıtlama mekanizmasına dayalı olarak gerekli güvenlik yapılarını kullanarak Cihaz Sağlama Hizmeti örneğinize cihazı kaydedersiniz: 

1. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar** düğmesine tıklayın ve Cihaz Sağlama hizmetinizi açın.

2. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Kayıtları yönet**’i seçin. Cihaz kurulumunuza göre **Bireysel Kayıtlar** sekmesini veya **Kayıt Grupları** sekmesini seçin. Üstteki **Ekle** düğmesine tıklayın. Kimlik kanıtlama *Mekanizması* olarak **TPM** veya **X.509** seçeneğini belirleyin ve önceden açıklandığı şekilde uygun güvenlik yapılarını girin. Yeni bir **IoT Hub cihaz kimliği** girebilirsiniz. Tamamlandığında **Kaydet** düğmesine tıklayın. 

3. Cihaz başarıyla kaydedildiğinde cihazın portalda şu şekilde görüntülendiğini görmeniz gerekir:

    ![Portalda başarılı TPM kaydı](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)

Kayıttan sonra sağlama hizmeti, cihazın önyüklenmesini ve daha sonra buna bağlanmasını bekler. Cihazınız ilk kez önyüklendiğinde, istemci SDK’sı kitaplığı, cihazdaki güvenlik yapılarını ayıklamak için yonganız ile etkileşime geçer ve Cihaz Sağlama hizmetinize kaydı doğrular. 

## <a name="start-the-device"></a>Cihazı başlatın

Bu noktada, aşağıdaki kurulum cihaz kaydı için hazırdır:

1. Cihazınız veya cihaz grubunuz, Cihaz Sağlama hizmetinize kaydolur ve 
2. Cihazınız, yapılandırılan kanıtlama mekanizmasına hazır olur ve Cihaz Sağlama Hizmeti istemci SDK’sı kullanılarak uygulamadan cihaza erişilebilir.

İstemci uygulamanızın, Cihaz Sağlama hizmetinize kaydı başlatmasını sağlamak için cihazı başlatın.  

## <a name="verify-the-device-is-registered"></a>Cihaz kayıtlı olduğunu doğrulayın

Cihazınız önyüklendikten sonra aşağıdaki eylemler gerçekleşmelidir. Daha fazla bilgi için [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) TPM simülatör örnek uygulamasına bakın. 

1. Cihaz, Cihaz Sağlama hizmetinize bir kayıt isteği gönderir.
2. TPM cihazları için Cihaz Sağlama Hizmeti bir kayıt sınaması gönderir ve cihazınız da buna yanıt verir. 
3. Kayıt başarılı olduğunda Cihaz Sağlama hizmeti, IoT hub URI’sini, cihaz kimliğini ve şifrelenmiş anahtarı cihaza geri gönderir. 
4. Sonra cihazdaki IoT Hub istemci uygulaması, hub’ınıza bağlanır. 
5. Hub’a başarılı bağlantı gerçekleştiğinde, IoT hub’ın **Cihaz Gezgini**’nde cihazın görünmesi gerekir. 

    ![Portalda hub ile başarılı bağlantı](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihazı kaydedin
> * Cihazı başlatın
> * Cihaz kayıtlı olduğunu doğrulayın

Yük dengeli hublar arasında birden çok cihaz sağlamayı öğrenmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IoT hubları arasında cihaz sağlama](./tutorial-provision-multiple-hubs.md)
