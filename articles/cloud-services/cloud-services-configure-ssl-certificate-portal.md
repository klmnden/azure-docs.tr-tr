---
title: Bir bulut hizmeti için SSL yapılandırma | Microsoft Docs
description: Web rolü için bir HTTPS uç nokta belirtme ve uygulamanızın güvenliğini sağlamak için bir SSL sertifikasını nasıl yükleyeceğiniz öğrenin. Bu örnekler Azure Portalı'nı kullanın.
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: ''
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: 0e053ad7f1033317948b6ef0856984b21e56e425
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
ms.locfileid: "24859785"
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Azure'da bir uygulama için SSL yapılandırma

Güvenli Yuva Katmanı (SSL) şifrelemesi, İnternet üzerinden gönderilen verilerin güvenliğini sağlamak için en yaygın kullanılan yöntemdir. Bu yaygın görev bir web rolü için HTTPS uç noktasının nasıl belirtileceğini ve uygulamanızın güvenliğini sağlamak için SSL sertifikasının nasıl yükleneceğini ele alır.

> [!NOTE]
> Bu görevde yordamlar Azure bulut Hizmetleri için geçerlidir; Uygulama hizmetleri için bkz: [bu](../app-service/app-service-web-tutorial-custom-ssl.md).
>

Bu görev, bir üretim dağıtımı kullanır. Bu konunun sonunda hazırlama dağıtımı hakkında bilgi sağlanır.

Okuma [bu](cloud-services-how-to-create-deploy-portal.md) henüz bir bulut hizmeti oluşturmadıysanız, ilk.

## <a name="step-1-get-an-ssl-certificate"></a>1. adım: bir SSL sertifikası alma
Bir uygulama için SSL yapılandırmak için önce bir sertifika yetkilisi (CA), kimin bu amaç için sertifikaları veren güvenilen bir üçüncü taraf imzalanmış bir SSL sertifikası almanız gerekir. Zaten bir yoksa, bir SSL sertifikalarını sattığı bir şirketten edinmeniz gerekir.

Sertifika Azure SSL sertifikaları için aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika bir özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına dışarı aktarılabilir olarak, anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın konu adı, bulut hizmetine erişmek için kullanılan etki alanını eşleşmesi gerekir. Cloudapp.net etki alanı için bir sertifika yetkilisinden (CA) bir SSL sertifikası elde edemiyor. Kullanılacak özel etki alanı adı almalıdır hizmetinize erişmek. Bir CA'dan bir sertifika isteme, sertifikanın konu adı, uygulamaya erişmek için kullanılan özel etki alanı adı eşleşmelidir. Örneğin, özel etki alanı adınızı ise **contoso.com** CA'nız için bir sertifika istemesini ***. contoso.com** veya **www.contoso.com**.
* En az 2048 bit şifreleme sertifikası kullanması gerekir.

Test amaçları için yapabileceğiniz [oluşturma](cloud-services-certs-create.md) ve kendinden imzalı bir sertifika kullanın. Kendinden imzalı bir sertifika üzerinden bir CA kimliği doğrulanmamış ve cloudapp.net etki alanı Web sitesi URL'si olarak kullanabilirsiniz. Örneğin, aşağıdaki görev sertifikada kullanılan ortak ad (CN) olduğu otomatik olarak imzalanan bir sertifika kullanır **sslexample.cloudapp.net**.

Ardından, sertifika hakkındaki bilgiler, hizmet tanımı ve hizmet yapılandırma dosyalarını eklemeniz gerekir.

<a name="modify"> </a>

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

3. Hizmet tanımı dosyasında eklemek bir **bağlama** öğesi içinde **siteleri** bölümü. Bu öğe, sitenize uç noktaya eşlemek için bir HTTPS bağlaması ekler:

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

   Hizmet tanımı dosyası için gerekli tüm değişiklikleri tamamlandı; Ancak, yine sertifika bilgilerini service yapılandırma dosyasına eklemeniz gerekir.
4. Hizmet yapılandırma dosyanızdaki (CSCFG) ServiceConfiguration.Cloud.cscfg, ekleme bir **sertifikaları** , sertifikanızın değerle. Aşağıdaki kod örneği ayrıntılarını sunan **sertifikaları** bölümünde, parmak izi değeri dışında.

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

(Bu örnekte **sha1** parmak izi algoritması için. Sertifikanızın parmak izi algoritmanın uygun değeri belirtin.)

Hizmet tanımı ve hizmet yapılandırma dosyalarını güncelleştirildi, dağıtımınızı Azure'a yükleme paketi. Kullanıyorsanız **cspack**, kullanmayan **/generateConfigurationFile** yeni eklediğiniz sertifika bilgilerini üzerine yazar olarak bayrak.

## <a name="step-3-upload-a-certificate"></a>3. adım: bir sertifikayı karşıya yüklemek
Azure Portalı'na bağlanmak ve...

1. İçinde **tüm kaynakları** bölüm portalında, bulut hizmetinizi seçin.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Tıklatın **Sertifikalar**.

    ![Sertifikaları simgesine tıklayın](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Tıklatın **karşıya** sertifikaları alanının üstünde.

    ![Karşıya yükleme menü öğesini tıklatın](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. Sağlamak **dosya**, **parola**, ardından **karşıya** veri giriş alanı altındaki.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>4. adım: rol HTTPS kullanarak bağlanın
Dağıtımınızı Azure'da da çalışır durumda, HTTPS kullanarak bağlanabilir.

1. Tıklatın **Site URL'si** web tarayıcısını açın.

   ![Site URL'sini tıklatın](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. Web tarayıcınızda kullanmak üzere bağlantıyı değiştirmek **https** yerine **http**ve sayfasını ziyaret edin.

   > [!NOTE]
   > Kendinden imzalı bir sertifikayla ilişkili bir HTTPS uç noktası için göz attığınızda kendinden imzalı bir sertifika kullanıyorsanız, bir sertifika hatası tarayıcıda görebilirsiniz. Bir güvenilen sertifika yetkilisi tarafından imzalanmış bir sertifika kullanarak bu sorunları ortadan kaldırılır; Bu arada, hatayı yoksayabilirsiniz. (Başka bir kullanıcının güvenilen sertifika yetkilisi sertifika depolama alanına otomatik olarak imzalanan sertifika eklemek için bir seçenektir.)
   >
   >

   ![Site Önizleme](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Hazırlama dağıtımı yerine Üretim dağıtımı için SSL kullanmak istiyorsanız, önce hazırlama dağıtımı için kullanılan URL belirlemek gerekir. Bulut hizmetinizi dağıtıldıktan sonra hazırlık ortamı URL'si tarafından belirlenir **dağıtım kimliği** bu biçiminde GUID:`https://deployment-id.cloudapp.net/`  
   >
   > Bir sertifika ortak adı (CN) GUID tabanlı URL eşit oluşturun (örneğin, **328187776e774ceda8fc57609d404462.cloudapp.net**). Hazırlanan bulut hizmetinize sertifika eklemek için Portalı'nı kullanın. Ardından, sertifika bilgileri CSDEF ve CSCFG dosyalarınıza eklemek, uygulamanızın yeniden paketleyin ve yeni paketini kullanmak için hazırlanmış dağıtımınızı güncelleştirin.
   >

## <a name="next-steps"></a>Sonraki adımlar
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
