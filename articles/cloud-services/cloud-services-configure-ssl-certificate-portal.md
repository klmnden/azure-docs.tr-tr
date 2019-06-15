---
title: Bir bulut hizmeti için SSL'yi yapılandırma | Microsoft Docs
description: Bir web rolü için bir HTTPS uç noktasının nasıl belirtileceğini ve uygulamanızın güvenliğini sağlamak için bir SSL sertifikasının nasıl yükleneceğini öğrenin. Bu örnekler, Azure portalını kullanın.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeconnoc
ms.openlocfilehash: 2a9879ebc55a5f25c1a358e386697dce1c55ec90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61434101"
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Azure'daki uygulama için SSL'yi yapılandırma

Güvenli Yuva Katmanı (SSL) şifrelemesi, İnternet üzerinden gönderilen verilerin güvenliğini sağlamak için en yaygın kullanılan yöntemdir. Bu yaygın görev bir web rolü için HTTPS uç noktasının nasıl belirtileceğini ve uygulamanızın güvenliğini sağlamak için SSL sertifikasının nasıl yükleneceğini ele alır.

> [!NOTE]
> Yordamlar bu görevi, Azure bulut Hizmetleri için uygulanır. Uygulama hizmetleri için bkz: [bu](../app-service/app-service-web-tutorial-custom-ssl.md).
>

Bu görev, bir üretim dağıtımı kullanır. Bu konunun sonunda hazırlama dağıtımı hakkında bilgi sağlanır.

Okuma [bu](cloud-services-how-to-create-deploy-portal.md) bir bulut hizmeti henüz oluşturmadıysanız, ilk.

## <a name="step-1-get-an-ssl-certificate"></a>1\. adım: Bir SSL sertifikası alma
Bir uygulama için SSL'yi yapılandırmak için ilk olarak bir sertifika yetkilisi (CA) sertifika bu amaç için sorunları güvenilen bir üçüncü taraf imzalanmış bir SSL sertifikası almanız gerekir. Zaten bir yoksa, bir SSL sertifikaları satan bir şirketten edinmeniz gerekir.

Sertifika, azure'da SSL sertifikaları için aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifikanın konu adı, bulut hizmetine erişmek için kullanılan etki alanı eşleşmesi gerekir. Cloudapp.net etki alanı için bir sertifika yetkilisinden (CA) bir SSL sertifikası alınamıyor. Kullanılacak özel etki alanı adı edinmeniz gerekir hizmetinize erişim. Sertifikanın konu adı, bir CA'dan bir sertifika talep ettiğinizde, uygulamanıza erişmek için kullanılan özel etki alanı adı eşleşmelidir. Örneğin, özel etki alanı adınızı ise **contoso.com** Sertifika yetkilinizden için bir sertifika isteği * **. contoso.com** veya **www\.contoso.com**.
* Sertifikanın en az 2048 bit şifreleme kullanmanız gerekir.

Test amaçları için yapabilecekleriniz [oluşturma](cloud-services-certs-create.md) ve otomatik olarak imzalanan bir sertifika kullanın. Kendinden imzalı bir sertifika bir CA ile kimlik doğrulaması yapılamıyor ve cloudapp.net etki alanı Web sitesi URL'si olarak kullanabilirsiniz. Örneğin, aşağıdaki görev sertifikada kullanılan ortak ad (CN) olan otomatik olarak imzalanan bir sertifika kullanır **sslexample.cloudapp.net**.

Ardından, hizmet tanımı ve hizmet yapılandırma dosyalarını sertifikayla ilgili bilgileri içermelidir.

<a name="modify"></a>

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>2\. adım: Hizmet tanım ve yapılandırma dosyalarını değiştirme
Uygulamanız sertifika kullanacak şekilde yapılandırılması gerekir ve bir HTTPS uç noktası eklenmelidir. Sonuç olarak, güncelleştirilecek hizmet yapılandırma dosyalarını ve hizmet tanımı gerekir.

1. Geliştirme ortamınızda Hizmet tanım dosyası (CSDEF) açın, ekleme bir **sertifikaları** içinde bölümünde **WebRole** bölümünde ve sertifikası hakkında aşağıdaki bilgileri ekleyin (ve Ara sertifikalar için):

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

   **Sertifikaları** bölümü, sertifika, konumunu ve bunu bulunduğu depo adını adını tanımlar.

   İzinler (`permissionLevel` özniteliği) aşağıdaki değerlerden birine ayarlayın:

   | İzin değeri | Açıklama |
   | --- | --- |
   | limitedOrElevated |**(Varsayılan)**  Tüm rol işlemler özel anahtara erişebilir. |
   | yükseltilmiş |Yalnızca yükseltilmiş işlemleri özel anahtara erişebilir. |

2. Hizmet tanımı dosyasında ekleme bir **Inputendpoint** öğesiyle **uç noktaları** bölüm HTTPS'yi etkinleştirmek için:

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

3. Hizmet tanımı dosyasında ekleme bir **bağlama** öğesiyle **siteleri** bölümü. Bu öğe, sitenize uç noktaya eşlemek için bir HTTPS bağlamasına ekler:

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

   Hizmet tanım dosyası için gerekli tüm değişiklikleri tamamladınız; Ancak, yine sertifika bilgileri service yapılandırma dosyasına eklemeniz gerekir.
4. Hizmet yapılandırma dosyasında (CSCFG) ServiceConfiguration.Cloud.cscfg, ekleme bir **sertifikaları** sertifikanızın, ile değeri. Aşağıdaki kod örneği ayrıntılarını sunan **sertifikaları** parmak izi değeri dışında bir bölüm.

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

(Bu örnekte **sha1** parmak izi algoritması için. Sertifikanızın parmak izi algoritması için uygun değeri belirtin.)

Hizmet yapılandırma dosyalarını ve hizmet tanımı güncelleştirildi, dağıtımınızı Azure'a yükleme paketi. Kullanıyorsanız **cspack**, kullanmayın **/generateConfigurationFile** , yeni eklediğiniz sertifika bilgileri üzerine yazılacağından, bayrak.

## <a name="step-3-upload-a-certificate"></a>3\. adım: Sertifikayı karşıya yükleyin
Azure Portalı'na bağlanmak ve...

1. İçinde **tüm kaynakları** bölüm portalın bulut hizmetinizi seçin.

    ![Bulut hizmetinizi yayımlayın](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Tıklayın **sertifikaları**.

    ![Sertifikaları simgesine tıklayın](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Tıklayın **karşıya** sertifikaları alanının üstünde.

    ![Karşıya yükleme menü öğesini tıklatın](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. Sağlamak **dosya**, **parola**, ardından **karşıya** veri giriş alanı alt kısmındaki.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>4\. Adım: HTTPS kullanarak rol örneğine bağlanın
Dağıtımınızı Azure'da hazır ve çalışır durumda, HTTPS üzerinden ona bağlanabilirsiniz.

1. Tıklayın **Site URL'si** web tarayıcısını açın.

   ![Site URL'si tıklayın](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. Web tarayıcınızda kullanmak üzere bağlantıyı değiştirin **https** yerine **http**ve ardından sayfasını ziyaret edin.

   > [!NOTE]
   > Otomatik olarak imzalanan sertifikayla ilişkili bir HTTPS uç noktasına göz atarken otomatik olarak imzalanan bir sertifika kullanıyorsanız, bir sertifika hatası tarayıcıda görebilirsiniz. Bir güvenilen sertifika yetkilisi tarafından imzalanmış bir sertifika kullanarak bu sorunu ortadan kaldırır; Bu arada, hatayı yoksayabilirsiniz. (Otomatik olarak imzalanan sertifikayı kullanıcının güvenilen sertifika yetkilisi sertifika deposuna eklemek için başka bir seçenek içindir.)
   >
   >

   ![Site Önizleme](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Hazırlama dağıtımı yerine Üretim dağıtımı için SSL kullanmak istiyorsanız, önce hazırlama dağıtımı için kullanılan URL belirlemek gerekir. Bulut hizmetinizin dağıtıldıktan sonra hazırlık ortamına URL tarafından belirlenir **dağıtım kimliği** GUID şu biçimde: `https://deployment-id.cloudapp.net/`  
   >
   > Ortak ad (CN) GUID tabanlı URL eşit bir sertifika oluşturun (örneğin, **328187776e774ceda8fc57609d404462.cloudapp.net**). Portal, sertifikayı, hazırlanmış bir bulut hizmetine eklemek için kullanın. Ardından, sertifika bilgileri CSDEF ve CSCFG dosyalarınıza eklemek, uygulamanızı yeniden paketleyin ve yeni paketi kullanmak için hazırlanan dağıtımınızı güncelleştirin.
   >

## <a name="next-steps"></a>Sonraki adımlar
* [Bulut hizmetinizin genel yapılandırma](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmeti dağıtma](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma bir [özel etki alanı adı](cloud-services-custom-domain-name-portal.md).
* [Bulut hizmetinizi yönetme](cloud-services-how-to-manage-portal.md).
