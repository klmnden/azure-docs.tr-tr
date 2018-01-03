---
title: "Azure IOT Hub cihaz sağlama hizmeti için cihaz erişimi yönetme | Microsoft Docs"
description: "Azure Portalı'nda DPS hizmetinize cihaz erişimi iptal etme"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7985fba68ef2c6f651c64756f8c534928b573de5
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2017
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Azure Portalı'nda sağlama hizmetinize cihaz erişimi iptal etme

Cihaz kimlik bilgilerinin doğru yönetim IOT çözümleri gibi yüksek profilli sistemler için önemlidir. Bu sistemlere en iyi yöntem, kullanıcıların kimlik bilgilerini bir SAS belirteci ya da bir X.509 sertifikası burada tehlikeye girebilir durumlarda cihazlar için erişimi iptal etme NET bir plana sağlamaktır. Bu makalede, cihaz erişimi sağlama adımda iptal açıklar.

Cihaz sağlandıktan sonra bir IOT hub'ına cihaz erişimi iptal etme hakkında bilgi edinmek için. bkz: [aygıtları devre dışı](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Yeniden deneme ilkesi için erişimi iptal aygıtların farkında olun. Örneğin, bir sonsuz yeniden deneme ilkesi aygıtla hizmet kaynakları harcayan ve büyük olasılıkla performansı etkileyen sağlama Hizmeti'ne, kaydolmak sürekli olarak deneyebilirsiniz.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Bir tek tek kayıt girişi kara liste cihazları

Tek tek kayıtları tek bir aygıta uygulamak ve X.509 sertifikalarını veya SAS belirteçlerinde (gerçek veya sanal TPM) kanıtlama mekanizması olarak kullanabilir. (Bunların kanıtlama mekanizması yalnızca tek bir kayıt yoluyla sağlanabilir gibi SAS belirteci kullanan aygıtları.) Tek bir kayıt cihaza kara listeye devre dışı bırakın ya da kayıt girişi silmek: 

- Geçici olarak cihaz kara listeye kayıt girdisini devre dışı bırakabilirsiniz. 

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
    2. Cihazınızı kaynaklar listesinde kara listeye istediğiniz sağlama hizmete tıklayın.
    3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **tek tek kayıtları** sekmesi.
    4. Açmak için kara listeye istediğiniz cihaz kayıt girişini tıklatın. 
    5. Tıklatın **devre dışı** kayıt liste girdisi alt kısmındaki ardından **kaydetmek**.  

        ![Tek tek kayıt girişi portalında devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Kalıcı olarak cihaz kara listeye kayıt girdisini silebilirsiniz.

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
    2. Cihazınızı kaynaklar listesinde kara listeye istediğiniz sağlama hizmete tıklayın.
    3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **tek tek kayıtları** sekmesi.
    4. Kara listeye istediğiniz cihaz kayıt girişi yanındaki kutuyu işaretleyin. 
    5. Tıklatın **silmek** pencerenin üst kısmında, ardından **Evet** kayıt kaldırmak istediğinizi onaylamak için. 

        ![Tek tek kayıt girişi portalında Sil](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Eylem tamamlandıktan sonra tek tek kayıtları listesinden kaldırılır, giriş görürsünüz.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Bir X.509 ara veya bir kayıt grubunu kullanarak kök CA sertifikasını kara

X.509 sertifikaları genellikle bir güven sertifikası zinciri düzenlenir. Bir Sertifika zincirindeki herhangi bir aşamada güvenliği tehlikeye girdiğinde, güven bozuluyor ve sertifika cihazların cihaz sağlama hizmeti tarafından sağlanacak den bu sertifikayı içeren zincir akışında aşağı önlemek için kara listede gerekir. X.509 sertifikaları sağlama hizmeti ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [X.509 sertifikalarını](./concepts-security.md#x509-certificates). 

Bir giriş aynı tarafından ara imzalı X.509 sertifikalarının ortak bir kanıtlama mekanizması paylaşma veya kök CA cihazlar için bir kayıt grubudur. Kayıt Grup giriş ara veya kök CA'ın yanı sıra, kendi sertifika bu sertifika ile cihazları tarafından paylaşılan tüm yapılandırma değerlerini, twin durumu ve IOT hub bağlantısı gibi ilişkili X.509 sertifikası ile yapılandırılmış Zincir. Sertifika kara listeye devre dışı bırakın ya kayıt grubu silme:

- Geçici sertifika kara listeye kendi kayıt grubuna devre dışı bırakabilirsiniz. 

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
    2. Kaynaklar listesinde imzalama sertifikası kara listeye istediğiniz sağlama hizmete tıklayın.
    3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **kayıt grupları** sekmesi.
    4. Açmak için kara listeye istediğiniz sertifika için kayıt grubunu tıklatın.
    5. Tıklatın **grubunu Düzenle** kayıt Grup girişinin üst sol.
    6. Kayıt girişi devre dışı bırakmak için seçin **devre dışı** üzerinde **etkinleştirmek girişi** geçiş yapın ve ardından **kaydetmek**.  

        ![Kayıt Portalı'nda grup giriş devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Kalıcı olarak sertifika kara listeye kendi kayıt grubu silebilirsiniz.

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
    2. Cihazınızı kaynaklar listesinde kara listeye istediğiniz sağlama hizmete tıklayın.
    3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **kayıt grupları** sekmesi.
    4. Kara listeye istediğiniz sertifika için kayıt grubu yanındaki kutuyu işaretleyin. 
    5. Tıklatın **silmek** pencerenin üst kısmında, ardından **Evet** kayıt grubunu kaldırmak istediğinizi onaylamak için. 

        ![Kayıt Portalı'nda grup giriş silme](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. İşlem tamamlandığında, kayıt gruplar listesinden kaldırılır, giriş görürsünüz.  

> [!NOTE]
> Bir sertifika için bir kayıt grubunu silerseniz, bu sertifika, sertifika zincirinde cihazları hala etkin kayıt grubu kök sertifikası veya başka bir ara sertifika kendi sertifikasında yukarıya için kaydol mümkün olabilir zinciri var.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Bir kayıt grubundaki kara liste belirli cihazlar

X.509 kanıtlama mekanizması uygulayan aygıtlar, kimlik doğrulaması için cihazın sertifika zinciri ve özel anahtarı kullanın. Bir aygıt bağlar ve cihaz sağlama hizmeti ile kimlik doğrulaması, hizmet cihaz sağlanan olup olmadığını belirlemek için kayıt grupları arama yapmadan önce cihazın kimlik eşleşen tek bir kayıt ilk arar. Hizmet cihaz devre dışı bırakılmış bir bireysel kaydını bulursa, bir etkin kayıt grubu için bir ara veya kök CA aygıtın sertifika zincirinde mevcut olsa bile, aygıt bağlanmasını, önler. Bir kayıt grubundaki tek bir cihaza kara listeye şu adımları izleyin:

1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
2. Kaynaklar listesinden kara listeye istediğiniz cihaz kayıt grubu içeren sağlama hizmete tıklayın.
3. Sağlama hizmetinizi tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **tek tek kayıtları** sekmesi.
4. Tıklatın **Ekle** üstündeki düğmesi. 
5. Seçin **X.509** aygıt sertifika yükleyin ve cihaz için güvenlik mekanizması olarak. Kimlik doğrulama sertifikalarını oluşturmak için kullandığı cihaz yüklü imzalı son varlık sertifikası budur.
6. Girin **IOT Hub cihaz kimliği** cihaz için. 
7. Kayıt girişi devre dışı bırakmak için seçin **devre dışı** üzerinde **etkinleştirmek girişi** geçin. 
8. **Kaydet**’e tıklayın. Kaydınızı başarılı oluşturulmasını üzerinde altında göründüğünü Cihazınızı görmelisiniz **tek tek kayıtları** sekmesi. 

    ![Tek tek kayıt girişi portalında devre dışı bırak](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




