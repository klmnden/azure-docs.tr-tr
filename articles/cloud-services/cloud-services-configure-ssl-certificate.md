---
title: "Bir bulut hizmeti (Klasik) için SSL yapılandırma | Microsoft Docs"
description: "Web rolü için bir HTTPS uç nokta belirtme ve uygulamanızın güvenliğini sağlamak için bir SSL sertifikasını nasıl yükleyeceğiniz öğrenin."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 7df2371f64f0d8a9fabc0df37c17d4dfbc47cd7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Azure'da bir uygulama için SSL yapılandırma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Klasik Azure Portalı](cloud-services-configure-ssl-certificate.md)
> 
> 

Güvenli Yuva Katmanı (SSL) şifrelemesi, İnternet üzerinden gönderilen verilerin güvenliğini sağlamak için en yaygın kullanılan yöntemdir. Bu yaygın görev bir web rolü için HTTPS uç noktasının nasıl belirtileceğini ve uygulamanızın güvenliğini sağlamak için SSL sertifikasının nasıl yükleneceğini ele alır.

> [!NOTE]
> Bu görevde yordamlar Azure bulut Hizmetleri için geçerlidir; Uygulama hizmetleri için bkz: [bu](../app-service/app-service-web-tutorial-custom-ssl.md) makalesi.
> 
> 

Bu görev, bir üretim dağıtımı kullanır. Bu konunun sonunda hazırlama dağıtımı hakkında bilgi sağlanır.

Okuma [bu](cloud-services-how-to-create-deploy.md) henüz bir bulut hizmeti oluşturmadıysanız, ilk makalesi.

## <a name="step-1-get-an-ssl-certificate"></a>1. adım: bir SSL sertifikası alma
Bir uygulama için SSL yapılandırmak için önce bir sertifika yetkilisi (CA), kimin bu amaç için sertifikaları veren güvenilen bir üçüncü taraf imzalanmış bir SSL sertifikası almanız gerekir. Zaten bir yoksa, bir SSL sertifikalarını sattığı bir şirketten edinmeniz gerekir.

Sertifika Azure SSL sertifikaları için aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir olarak, anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın konu adı, bulut hizmetine erişmek için kullanılan etki alanını eşleşmesi gerekir. Cloudapp.net etki alanı için bir sertifika yetkilisinden (CA) bir SSL sertifikası elde edemiyor. Kullanılacak özel etki alanı adı almalıdır hizmetinize erişmek. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, uygulamaya erişmek için kullanılan özel etki alanı adı eşleşmelidir. Örneğin, özel etki alanı adınızı ise **contoso.com** CA'nız için bir sertifika istemesini ***. contoso.com** veya **www.contoso.com**.
* En az 2048 bit şifreleme sertifikası kullanması gerekir.

Test amaçları için yapabileceğiniz [oluşturma](cloud-services-certs-create.md) ve kendinden imzalı bir sertifika kullanın. Kendinden imzalı bir sertifika üzerinden bir CA kimliği doğrulanmamış ve cloudapp.net etki alanı Web sitesi URL'si olarak kullanabilirsiniz. Örneğin, aşağıdaki görev sertifikada kullanılan ortak ad (CN) olduğu otomatik olarak imzalanan bir sertifika kullanır **sslexample.cloudapp.net**.

Ardından, sertifika hakkındaki bilgiler, hizmet tanımı ve hizmet yapılandırma dosyalarını eklemeniz gerekir.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>2. adım: Hizmet tanım ve yapılandırma dosyalarını değiştirme
Uygulamanızı sertifikayı kullanmak üzere yapılandırılması gerekir ve bir HTTPS uç noktası eklenmelidir. Sonuç olarak, hizmet yapılandırma dosyalarını ve hizmet tanımı güncelleştirilmesi gerekir.

1. Geliştirme ortamınızı Hizmet tanım dosyası (CSDEF) açın, ekleme bir **sertifikaları** içinde bölümünde **WebRole** bölümünde ve sertifikası (ve Ara sertifikaları) hakkında aşağıdaki bilgileri içerir:
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                        storeLocation="LocalMachine" 
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by the CA root, you
            must include all the intermediate certificates
            here. You must list them here, even if they are
            not bound to any endpoints. Failing to list any of
            the intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```
   
   **Sertifikaları** bölümü bizim sertifika, konumunu ve onu bulunduğu deposunun adını adını tanımlar.
   
   İzinler (`permisionLevel` özniteliği) aşağıdaki değerlerden birine ayarlayın:
   
   | İzni değeri | Açıklama |
   | --- | --- |
   | limitedOrElevated |**(Varsayılan)**  Tüm rol işlemler özel anahtara erişebilir. |
   | yükseltilmiş |Yükseltilmiş işlemleri yalnızca özel anahtara erişebilir. |
2. Hizmet tanımı dosyasında eklemek bir **Inputendpoint** öğesi içinde **uç noktaları** bölüm HTTPS'yi etkinleştirmek için:
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. Hizmet tanımı dosyasında eklemek bir **bağlama** öğesi içinde **siteleri** bölümü. Bu bölümde, sitenize uç noktaya eşlemek için bir HTTPS bağlaması ekler:
   
    ```xml   
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```
   
   Hizmet tanımı dosyası için gerekli tüm değişiklikleri tamamlandı ancak hala service yapılandırma dosyasına sertifika bilgilerini eklemeniz gerekir.
4. Hizmet yapılandırma dosyanızdaki (CSCFG) ServiceConfiguration.Cloud.cscfg, ekleme bir **sertifikaları** içinde bölümünde **rol** ile Aşağıda örnek parmak izi değerini değiştirerek bölümünde, sertifikanızı:
   
    ```xml   
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(Önceki örnekte kullanan **sha1** parmak izi algoritması için. Sertifikanızın parmak izi algoritmanın uygun değeri belirtin.)

Hizmet tanımı ve hizmet yapılandırma dosyalarını güncelleştirildi, dağıtımınızı Azure'a yükleme paketi. Kullanıyorsanız **cspack**, kullanmayan **/generateConfigurationFile** , eklediğiniz sertifika bilgilerini üzerine yazar olarak bayrak.

## <a name="step-3-upload-a-certificate"></a>3. adım: bir sertifikayı karşıya yüklemek
Dağıtım paketi sertifikayı kullanmak üzere güncelleştirilmiş ve bir HTTPS uç nokta eklenir. Artık Azure Klasik portalı ile Azure için paket ve sertifika yükleyebilirsiniz.

1. Oturum [Klasik Azure portalı][Azure classic portal]. 
2. Tıklatın **bulut Hizmetleri** sol taraftaki gezinti bölmesi.
3. İstenen bulut hizmeti'ı tıklatın.
4. Tıklatın **sertifikaları** sekmesi.
   
    ![Sertifikalar sekmesini tıklatın](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. **Karşıya Yükle** düğmesine tıklayın.
   
    ![Karşıya Yükle](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Sağlamak **dosya**, **parola**, ardından **tam** (onay işaretine).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>4. adım: rol HTTPS kullanarak bağlanın
Dağıtımınızı Azure'da da çalışır durumda, HTTPS kullanarak bağlanabilir.

1. Azure Klasik portalında dağıtımınızı seçin, ardından altında bağlantıyı **Site URL'si**.
   
   ![Site URL'si belirleme][2]
2. Web tarayıcınızda kullanmak üzere bağlantıyı değiştirmek **https** yerine **http**ve sayfasını ziyaret edin.
   
   > [!NOTE]
   > Kendinden imzalı bir sertifikayla ilişkili bir HTTPS uç noktası için göz attığınızda kendinden imzalı bir sertifika kullanıyorsanız, bir sertifika hatası tarayıcıda görebilirsiniz. Bir güvenilen sertifika yetkilisi tarafından imzalanmış bir sertifika kullanarak bu sorunları ortadan kaldırılır; Bu arada, hatayı yoksayabilirsiniz. (Başka bir kullanıcının güvenilen sertifika yetkilisi sertifika depolama alanına otomatik olarak imzalanan sertifika eklemek için bir seçenektir.)
   > 
   > 
   
   ![SSL örnek web sitesi][3]

Hazırlama dağıtımı yerine Üretim dağıtımı için SSL kullanmak istiyorsanız, önce hazırlama dağıtımı için kullanılan URL belirlemeniz gerekir. Bulut hizmeti, bir sertifika veya sertifika bilgileri dahil etmeden hazırlama ortamına dağıtın. Uygulama dağıtıldıktan sonra Azure Klasik portalın içinde listelenen GUID tabanlı URL belirleyebilirsiniz **Site URL'si** alan. Bir sertifika ortak adı (CN) GUID tabanlı URL eşit oluşturun (örneğin, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Hazırlanan bulut hizmetinize sertifika eklemek için Klasik Azure portalını kullanın. Ardından, sertifika bilgileri CSDEF ve CSCFG dosyalarınıza eklemek, uygulamanızın yeniden paketleyin ve yeni paketini kullanmak için hazırlanmış dağıtımınızı güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
