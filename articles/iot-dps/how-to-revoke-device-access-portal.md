---
title: Bir CİHAZDAN bir Azure IOT Hub cihazı sağlama hizmeti disenroll nasıl
description: Azure IOT Hub cihazı sağlama hizmeti sağlama önlemek için bir cihaz disenroll nasıl
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 0dadf0ec248dac01e5cc65779004477bf4afc823
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62113592"
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>Bir CİHAZDAN bir Azure IOT Hub cihazı sağlama hizmeti disenroll nasıl

Cihaz kimlik bilgilerinin doğru yönetim IOT çözümleri gibi yüksek profilli sistemler için önemlidir. Bu tür sistemleri için en iyi uygulama, cihazlar için erişimi iptal etmek için NET bir plana sahip olmaktır olduğunda, kimlik bilgilerini bir paylaşılan erişim imzaları (SAS) belirteci veya bir X.509 sertifikası ele geçirilebilir. 

Cihaz sağlama hizmeti, kayıt sağlayan bir cihazın olması [otomatik olarak sağlanan](concepts-auto-provisioning.md). Sağlanan bir cihaz IOT hub'da ilk almasına izin verme, kayıtlı bir olduğunu [cihaz ikizi](~/articles/iot-hub/iot-hub-devguide-device-twins.md) belirtin ve telemetri verilerini raporlamaya başlar. Bu makale, gelecekte yeniden sağlanan önleme sağlama hizmeti örneğinizi, bir CİHAZDAN disenroll açıklamaktadır.

> [!NOTE] 
> Yeniden deneme ilkesi erişimini iptal cihazların farkında olun. Örneğin, bir sonsuz yeniden deneme ilkesi olan bir cihaz sağlama Hizmeti'ne kaydolmak sürekli olarak deneyebilir. Bu durum hizmet kaynaklarını tüketir ve büyük olasılıkla performansı etkiler.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Bireysel kayıt girişi kullanarak kara liste cihazları

Bireysel kayıtlar, tek bir cihaz olarak uygulanır ve kanıtlama mekanizması olarak X.509 sertifikalarını veya SAS belirteçlerini (gerçek ya da sanal TPM'de) kullanabilir. (Kendi kanıtlama mekanizması yalnızca bireysel kayıt sağlanabilir gibi SAS belirteçlerini kullanma cihazlar.) Bireysel kayıt olan bir cihazda kara listeye devre dışı bırakın veya kayıt girişini silin. 

Geçici olarak cihaz kayıt girdisini devre dışı bırakarak veya kara listeye: 

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde, cihazınızın kara listeye istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizi seçin **kayıtları Yönet**ve ardından **bireysel kayıtlar** sekmesi.
4. Kara listeye istediğiniz cihaz kayıt girişini seçin. 

    ![Bireysel kayıt seçin](./media/how-to-revoke-device-access-portal/select-individual-enrollment.png)

5. Kayıt sayfanızda altına ve seçin **devre dışı** için **etkinleştirme giriş** geçiş yapın ve ardından **Kaydet**.  

   ![Bireysel kayıt girişi portalda devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Kalıcı olarak cihaz kayıt girdisini silerek kara listeye:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde, cihazınızın kara listeye istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizi seçin **kayıtları Yönet**ve ardından **bireysel kayıtlar** sekmesi.
4. Kara listeye istediğiniz cihaz kayıt girişinin yanındaki onay kutusunu seçin. 
5. Seçin **Sil** penceresi tıklayın ve ardından üst kısmındaki **Evet** kayıt kaldırmak istediğinizi onaylayın. 

   ![Bireysel kayıt girişi portalında Sil](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)


Yordamı tamamladığınızda, bireysel kayıtlar listesinden kaldırılsa giriş görmeniz gerekir.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Kayıt grubu kullanarak bir X.509 ara veya kök CA sertifikası kara

X.509 sertifikaları, genellikle bir güven sertifikası zinciri düzenlenir. Bir sertifika zinciri her aşamasında güvenliği tehlikeye girdiğinde, güven bozulur. Cihaz sağlama hizmeti, sertifikayı içeren herhangi bir zincirde aşağı yönde sağlama cihazları engellemek için sertifika kara listede gerekir. X.509 sertifikaları ve sağlama hizmetiyle nasıl kullanıldığı hakkında daha fazla bilgi için bkz: [X.509 sertifikaları](./concepts-security.md#x509-certificates). 

Kayıt grubu, kök CA veya aynı tarafından ara imzalı X.509 sertifikalarının ortak bir kanıtlama mekanizmasını paylaşan cihazlar için bir girdidir. Kayıt grubu girdisini ara ile ilişkili X.509 sertifikası ile yapılandırılmış veya kök CA. Giriş, ayrıca, sertifika zincirlerinde Bu sertifika ile cihazlar tarafından paylaşılan tüm yapılandırma değerleri, ikizi durumu ve IOT hub bağlantı gibi ile yapılandırılır. Sertifika kara listeye devre dışı veya kendi kayıt grubunu silin.

Geçici olarak sertifika kayıt grubu devre dışı bırakarak veya kara listeye: 

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde, imzalama sertifikası kara listeye istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizi seçin **kayıtları Yönet**ve ardından **kayıt grupları** sekmesi.
4. Kara listeye istediğiniz sertifikayı kullanarak kayıt grubunu seçin.
5. Seçin **devre dışı** üzerinde **etkinleştirme giriş** geçiş yapın ve ardından **Kaydet**.  

   ![Portalda kayıt grubu girdisini devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Kalıcı olarak sertifika, kayıt grubunu silerek kara listeye:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinde, cihazınızın kara listeye istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizi seçin **kayıtları Yönet**ve ardından **kayıt grupları** sekmesi.
4. Kara listeye istediğiniz sertifika için kayıt grubunun yanındaki onay kutusunu seçin. 
5. Seçin **Sil** penceresi tıklayın ve ardından üst kısmındaki **Evet** kayıt grubu kaldırmak istediğinizi onaylayın. 

   ![Portalda kayıt grubu girdisini Sil](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Yordamı tamamladıktan sonra kayıt grupları listesinden kaldırılsa giriş görmeniz gerekir.  

> [!NOTE]
> Bir sertifika için bir kayıt grubunu silerseniz, sertifika, sertifika zincirine sahip cihazları hala etkin kayıt grubu, kök sertifika veya başka bir ara sertifika kendi sertifikasında yukarıya kaydetme olabilir zinciri var.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Kara liste belirli cihazlara bir kayıt grubundaki

X.509 kanıtlama mekanizması uygulamak cihazların kimliğini doğrulamak için cihazın sertifika zinciri ve özel anahtarı kullanın. Bir cihaz bağlar ve cihaz sağlama hizmeti ile kimlik doğrulaması, hizmet önce cihazın kimlik eşleştiren bireysel kayıt için arar. Hizmet, ardından cihaz sağlanan olup olmadığını belirlemek için kayıt grupları arar. Hizmet cihazı için devre dışı bireysel bir kayıt bulursa, cihaza bağlanmasını engeller. Bir etkin kayıt grubu için bir ara veya kök CA cihazın sertifika zincirinde mevcut olsa bile hizmetin bağlantı engeller. 

Tek bir cihaza bir kayıt grubundaki Engellenenler listesine şu adımları izleyin:

1. Azure portal ve Seç'i açın **tüm kaynakları** sol menüden.
2. Kaynak listesinden kara listeye istediğiniz cihaz kayıt grubu içeren sağlama hizmetini seçin.
3. Sağlama hizmetinizi seçin **kayıtları Yönet**ve ardından **bireysel kayıtlar** sekmesi.
4. Seçin **Ekle bireysel kayıt** üstünde düğme. 
5. Üzerinde **kayıt ekleme** sayfasında **X.509** kanıtlama olarak **mekanizması** aygıt için.

    Cihaz sertifikayı karşıya yüklemek ve kara listede için cihazın cihaz kimliği girin. Sertifika için cihazda yüklü imzalı son varlık sertifikası kullanın. Cihaz kimlik doğrulaması için imzalı son varlık sertifikası kullanır.

    ![Kara listeye alınan cihaz için cihaz özelliklerini ayarlama](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group-1.png)

6. Listenin sonuna kaydırın **kayıt ekleme** sayfasından seçim yapıp **devre dışı** üzerinde **etkinleştirme giriş** geçiş yapın ve ardından **Kaydet**. 

    [![Bireysel kayıt girişi portalında Grup kaydı CİHAZDAN devre dışı bırakmak için devre dışı](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

Kaydınız başarılı bir şekilde oluşturduğunuzda, listelenen devre dışı bırakılmış bir cihaz kaydı sorunlarınızı görmelisiniz **bireysel kayıtlar** sekmesi. 

## <a name="next-steps"></a>Sonraki adımlar

Kayıt silmeyi ayrıca daha büyük işlemini bir parçasıdır. Bir cihaz sağlamayı kaldırma, sağlama hizmeti kayıt silmeyi hem IOT hub'ından SDK'ya içerir. Tam işlemleri hakkında bilgi edinmek için [nasıl daha önce otomatik olarak sağlanan cihazları sağlamasını kaldırmak](how-to-unprovision-devices.md) 

