---
title: "Azure IOT Hub cihaz sağlama hizmeti ile bir cihaz hazırlayın | Microsoft Docs"
description: "Cihazınızı Azure IOT Hub cihaz sağlama hizmeti ile tek bir IOT hub'ına sağlama"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: bf50699d2dc67294d554ba15713254a8b88d8ade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaz sağlama hizmeti ile bir IOT hub'ına cihaz hazırlayın

Önceki öğreticide, bir aygıtı Aygıt sağlama hizmete bağlanmak için ayarlama öğrendiniz. Bu öğreticide, tek bir IOT hub aygıtınıza sağlamak için bu hizmeti kullanmak nasıl öğreneceksiniz kullanarak  **_kayıt listeleri_**. Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Cihaz kaydı
> * Cihaz Başlat
> * Cihaz kayıtlı doğrulayın

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce Cihazınızı yapılandırdığınızdan emin olun ve kendi *donanım güvenlik modülü* öğreticide anlatıldığı gibi [Azure IOT Hub cihaz sağlama hizmeti ile sağlamak için bir aygıtı ayarlama](./tutorial-set-up-device.md).


<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Cihaz kaydı

Bu adım, cihazın benzersiz güvenlik yapıları sağlama cihazı hizmetine ekleme içerir. Bu güvenlik yapıtların aşağıdaki gibidir:

- TPM tabanlı cihazlar için:
    - *Onay anahtarını* her TPM yongası veya benzetimi benzersiz. Okuma [anlamak TPM onay anahtarını](https://technet.microsoft.com/library/cc770443.aspx) daha fazla bilgi için.
    - *Kayıt kimliği* ad/kapsamında bir cihazı benzersiz olarak tanımlamak için kullanılır. Bu olabilir veya cihaz kimliği ile aynı olamaz Kimliği her aygıt için zorunludur. TPM tabanlı cihazlar için kayıt kimliği TPM kendisini, örneğin, bir SHA-256 karmasını TPM onay anahtarını türetilmiş olmalıdır.

    ![Portal TPM için kayıt bilgileri](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- X.509 tabanlı cihazlar için:
    - [X.509 sertifikası](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) yonga veya ya da biçiminde benzetimi bir *.pem* veya *.cer* dosya. Tek tek kayıt için kullanmanız gerekir. *imzalayan sertifika* X.509 sisteminiz için kayıt gruplarının karşın, kullanmanız gereken *kök sertifika*.

    ![Portal X.509 için kayıt bilgileri](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)


Cihaz sağlama hizmetine cihazı kaydetmek için iki yolu vardır:

- **Kayıt grupları** bu belirli kanıtlama mekanizması paylaşan aygıtları grubunu temsil eder. Çok sayıda istenen ilk yapılandırma paylaşmak, cihazlar için veya cihazlar için bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz.

    ![X.509 portalı için kayıt grupları](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Tek tek kayıtları** bu cihaz sağlama hizmeti ile kaydedebilir tek bir cihaz için bir giriş temsil eder. Tek tek kayıtları ya da x509 kullanabilir sertifikalar veya SAS belirteçleri (gerçek veya sanal TPM'de) kanıtlama mekanizmaları. Benzersiz başlangıç yapılandırmasını gerektiren cihazlar için veya yalnızca TPM ya da sanal TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak kullanabileceğiniz cihazlar için tek tek kayıtları kullanmanızı öneririz. Tek tek kayıtları, belirtilen istenen IOT hub cihaz kimliği olabilir.

Cihaz portalında kaydetme adımları şunlardır:

1. HSM aygıtınızda için güvenlik yapıları unutmayın. Başlıklı bölümde belirtilen API'lerini kullanmanız gerekebilir [ayıklamak güvenlik yapıları](./tutorial-set-up-device.md#extractsecurity) önceki öğreticinin bir geliştirme ortamı.

1. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve Cihaz Sağlama hizmetinizi açın.

1. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Kayıtları yönet**’i seçin. Şunlardan birini seçin **tek tek kayıtları** sekmesini veya **kayıt grupları** sekmesi, cihaz Kurulumu göredir. Tıklatın **Ekle** üstündeki düğmesi. Seçin **TPM** veya **X.509** kimlik kanıtı olarak *mekanizması*ve daha önce açıklandığı gibi uygun güvenlik yapıları girin. Yeni bir girebilirsiniz **IOT Hub cihaz kimliği**. Tamamlandığında **Kaydet** düğmesine tıklayın. 

1. Cihaz başarıyla kaydedildiğinde görüntülenen aşağıdaki olarak Portalı'nda görmeniz gerekir:

    ![TPM kayıt Portalı'nda](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)


## <a name="start-the-device"></a>Cihaz Başlat

Bu noktada, aşağıdaki Kurulum cihaz kaydı için hazır:

1. Cihaz veya cihaz grubu cihaz sağlamayı hizmetinize kaydedilen ve 
2. Cihazınızı HSM yonga yapılandırıldı ve cihaz sağlama hizmeti istemci SDK'sını kullanarak uygulama üzerinden erişilebilir ile hazırdır.

Cihazı Cihaz sağlamayı hizmetiniz ile kaydı'nı başlatmak istemci uygulamanızı başlatın.  


## <a name="verify-the-device-is-registered"></a>Cihaz kayıtlı doğrulayın

Bir kez, cihaz önyüklemesinde, aşağıdaki eylemleri gerçekleşmesi. TPM simulator örnek bir uygulama bkz [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) daha fazla ayrıntı için. 

1. Aygıtın aygıt sağlama hizmetinize bir kayıt isteği gönderir.
2. TPM aygıtları için aygıt hizmeti sağlama Cihazınızı yanıt vereceği bir kayıt sınama geri gönderir. 
3. Başarılı kaydı üzerinde aygıt hizmeti sağlama IOT hub'ı URI, cihaz kimliği ve şifrelenmiş anahtar cihaza geri gönderir. 
4. IOT Hub istemci uygulaması cihazda ardından hub'ınıza bağlanır. 
5. Hub'ına başarılı bağlantı üzerine IOT hub ' ın görünür aygıt görmelisiniz **aygıt Explorer**. 

    ![Hub'ına Portalı'nda başarılı bağlantı](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihaz kaydı
> * Cihaz Başlat
> * Cihaz kayıtlı doğrulayın

Yük dengeli hubs arasında birden çok aygıt sağlama konusunda bilgi almak için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IOT hub'ları aygıtlarda sağlama](./tutorial-provision-multiple-hubs.md)
