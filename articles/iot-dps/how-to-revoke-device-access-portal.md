---
title: Azure IOT Hub cihaz hizmeti sağlama aygıttan disenroll nasıl
description: Azure IOT Hub cihaz sağlama hizmeti aracılığıyla sağlama önlemek için bir aygıt disenroll nasıl
author: bryanla
ms.author: bryanla
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 4b6c948765575c92c962999fe394ffaf1a0d22f3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628853"
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaz hizmeti sağlama aygıttan disenroll nasıl

Cihaz kimlik bilgilerinin doğru yönetim IOT çözümleri gibi yüksek profilli sistemler için önemlidir. Bu sistemlere en iyi yöntem cihazlar için erişimi iptal etme NET bir plana yapmaktır zaman kullanıcıların kimlik bilgilerini bir paylaşılan erişim imzaları (SAS) belirteci ya da bir X.509 sertifikası tehlikeye. 

Cihaz sağlama hizmet kaydolmayı sağlayan olması bir aygıt [otomatik sağlanan](concepts-auto-provisioning.md). Sağlanan bir aygıtı ilk almasına izin vererek IOT Hub ile kayıtlı biridir [cihaz çifti](~/articles/iot-hub/iot-hub-devguide-device-twins.md) durum ve telemetri verileri raporlama başlayın. Bu makale, gelecekte tekrar sağlanan önleme, sağlama hizmet örneği bir aygıttan disenroll açıklar.

> [!NOTE] 
> Yeniden deneme ilkesi için erişimi iptal aygıtların farkında olun. Örneğin, bir sonsuz yeniden deneme ilkesi olan bir aygıtı sağlama Hizmeti'ne kaydolmak sürekli olarak deneyebilirsiniz. Bu durum Hizmet kaynağı tüketir ve büyük olasılıkla performansı etkiler.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Bir tek tek kayıt girişi kullanarak kara liste cihazları

Tek tek kayıtları tek bir aygıta uygulamak ve X.509 sertifikaları ya da SAS belirteçlerinde (gerçek veya sanal TPM) kanıtlama mekanizması olarak kullanabilirsiniz. (Bunların kanıtlama mekanizması yalnızca tek bir kayıt yoluyla sağlanabilir gibi SAS belirteci kullanan aygıtları.) Tek bir kayıt cihaza kara listeye devre dışı bırakın ya da kayıt girişi silmek. 

Cihaz kayıt girdisini devre dışı bırakarak geçici olarak kara listeye: 

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde cihazınızın kara listeye istediğiniz sağlama hizmeti seçin.
3. Sağlama hizmetinizi seçin **kayıtlarını yönetme**ve ardından **tek tek kayıtları** sekmesi.
4. Kara listeye istediğiniz cihaz kayıt girişini seçin. 
5. Alt kısmına kaydırın ve seçin **devre dışı** üzerinde **etkinleştirmek girişi** geçiş yapın ve ardından **kaydetmek**.  

   [![Tek tek kayıt girişi portalında devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png#lightbox)  

Cihaz kayıt girişi silerek kalıcı olarak kara listeye:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde cihazınızın kara listeye istediğiniz sağlama hizmeti seçin.
3. Sağlama hizmetinizi seçin **kayıtlarını yönetme**ve ardından **tek tek kayıtları** sekmesi.
4. Kara listeye istediğiniz cihaz kayıt girişi yanındaki onay kutusunu seçin. 
5. Seçin **silmek** penceresi ve ardından üstündeki **Evet** kayıt kaldırmak istediğinizi onaylamak için. 

   ![Tek tek kayıt girişi portalında Sil](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Yordamı tamamladıktan sonra tek tek kayıtları listesinden kaldırılır, giriş görmeniz gerekir.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Bir kayıt grubunu kullanarak bir X.509 ara veya kök CA sertifikasını kara

X.509 sertifikaları genellikle bir güven sertifikası zinciri düzenlenir. Bir Sertifika zincirindeki herhangi bir aşamada güvenliği tehlikeye girdiğinde, güven bozulur. Bu sertifikayı içeren zincir sağlama cihazları aşağı aygıt hizmeti sağlama engellemek için sertifika kara listede gerekir. X.509 sertifikaları sağlama hizmeti ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [X.509 sertifikalarını](./concepts-security.md#x509-certificates). 

Bir giriş aynı tarafından ara imzalı X.509 sertifikalarının ortak bir kanıtlama mekanizması paylaşma veya kök CA cihazlar için bir kayıt grubudur. Kayıt Grup giriş ara ile ilişkili X.509 sertifikası ile yapılandırılmış veya kök CA. Giriş kendi sertifika zincirinde Bu sertifika ile cihazları tarafından paylaşılan tüm yapılandırma değerlerini, twin durumu ve IOT hub bağlantısı gibi ile de yapılandırılabilir. Sertifika kara listeye devre dışı bırakın ya kayıt grubunu silin.

Sertifika, kayıt grubu devre dışı bırakarak geçici olarak kara listeye: 

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde imzalama sertifikası kara listeye istediğiniz sağlama hizmeti seçin.
3. Sağlama hizmetinizi seçin **kayıtlarını yönetme**ve ardından **kayıt grupları** sekmesi.
4. Kara listeye istediğiniz sertifikayı kullanarak kayıt grubunu seçin.
5. Seçin **devre dışı** üzerinde **etkinleştirmek girişi** geçiş yapın ve ardından **kaydetmek**.  

   ![Kayıt Portalı'nda grup giriş devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Sertifika kayıt grubunu silerek kalıcı olarak kara listeye:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde cihazınızın kara listeye istediğiniz sağlama hizmeti seçin.
3. Sağlama hizmetinizi seçin **kayıtlarını yönetme**ve ardından **kayıt grupları** sekmesi.
4. Kara listeye istediğiniz sertifika için kayıt grubunun yanındaki onay kutusunu seçin. 
5. Seçin **silmek** penceresi ve ardından üstündeki **Evet** kayıt grubunu kaldırmak istediğinizi onaylamak için. 

   ![Kayıt Portalı'nda grup giriş silme](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Yordamı tamamladıktan sonra kayıt gruplar listesinden kaldırılır, giriş görmeniz gerekir.  

> [!NOTE]
> Bir sertifika için bir kayıt grubunu silerseniz, kendi sertifika zincirinde sertifikanız aygıtları hala etkin kayıt grubu kök sertifikası veya başka bir ara sertifika kendi sertifikasında yukarıya için kaydol olabilir zinciri var.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Bir kayıt grubundaki kara liste belirli cihazlar

X.509 kanıtlama mekanizması uygulayan aygıtlar, kimlik doğrulaması için cihazın sertifika zinciri ve özel anahtarı kullanın. Bir aygıt bağlar ve cihaz sağlama hizmeti ile kimlik doğrulaması, hizmeti cihazın kimlik eşleşen tek bir kayıt için ilk arar. Hizmeti daha sonra cihaz sağlanan olup olmadığını belirlemek için kayıt grupları arar. Hizmet cihaz devre dışı bırakılmış bir bireysel kaydını bulursa, cihaz bağlanmasını engeller. Hizmetin bir etkin kayıt grubu için bir ara veya kök CA aygıtın sertifika zincirinde mevcut olsa bile bağlantı engeller. 

Bir kayıt grubundaki tek bir cihaza kara listeye şu adımları izleyin:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynaklar listesinden kara listeye istediğiniz cihaz kayıt grubunu içeren sağlama hizmeti seçin.
3. Sağlama hizmetinizi seçin **kayıtlarını yönetme**ve ardından **tek tek kayıtları** sekmesi.
4. Seçin **Ekle** üstündeki düğmesi. 
5. Seçin **X.509** cihaz ve cihaz sertifika yükleyin kanıtlama mekanizması olarak. Bu cihazda yüklü imzalı son varlık sertifikasıdır. Cihaz kimlik doğrulama sertifikalarını oluşturmak için kullanır.
6. İçin **IOT Hub cihaz kimliği**, aygıtın Kimliğini girin. 
7. Seçin **devre dışı** üzerinde **etkinleştirmek girişi** geçiş yapın ve ardından **kaydetmek**. 

    [![Tek tek kayıt girişi portalında Grup kayıt aygıttan devre dışı bırakmak için devre dışı](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

Kaydınızı başarıyla oluşturduğunuzda görünür Cihazınızı görmelisiniz **tek tek kayıtları** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Disenrollment ayrıca büyük sağlamanın kaldırılması işlemini bir parçasıdır. Bir aygıt sağlamayı hem hizmet sağlama gelen disenrollment ve IOT hub'ından kaydı siliniyor içerir. Tam işlemi hakkında bilgi edinmek için [nasıl daha önce otomatik-sağlanan aygıtlar yetkisini kaldırma](how-to-unprovision-devices.md) 

