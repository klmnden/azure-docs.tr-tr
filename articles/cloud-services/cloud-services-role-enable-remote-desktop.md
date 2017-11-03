---
title: "Bir Azure bulut hizmetindeki, Uzak Masaüstü'nü etkinleştirme | Microsoft Docs"
description: "Azure bulut hizmeti uygulamanız Uzak Masaüstü bağlantılarına izin verecek şekilde yapılandırma"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Azure bulut Hizmetleri rolünde için Uzak Masaüstü Bağlantısı etkinleştir

> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klasik Azure Portalı](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Uzak Masaüstü Uzak Masaüstü uzantısı aracılığıyla etkinleştirmeyi seçebilirsiniz veya hizmet tanımında Uzak Masaüstü modüller ekleyerek geliştirme sırasında rolünüze Uzak Masaüstü Bağlantısı etkinleştirebilirsiniz. Tercih edilen yaklaşım uygulamanızı yeniden dağıtmak zorunda kalmadan uygulama bile dağıtıldıktan sonra Uzak Masaüstü'nü etkinleştirme gibi Uzak Masaüstü uzantısı kullanmaktır.

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Klasik Azure portalından Uzak Masaüstü'nü yapılandırma
Klasik Azure portalı, Uzak Masaüstü uzantısı yaklaşımı kullanır, bu nedenle bile uygulama dağıtıldıktan sonra Uzak Masaüstü etkinleştirebilirsiniz. **Yapılandırma** sayfası bulut hizmetiniz için Uzak Masaüstü'nü etkinleştirmek için sanal makinelere bağlanmak için kullanılan yerel yönetici hesabını değiştirirseniz sağlar, sertifika kimlik doğrulamasında kullanılan ve sona erme tarihini ayarlayın.

1. Tıklatın **bulut Hizmetleri**, bulut hizmeti adına tıklayın ve ardından **yapılandırma**.
2. Tıklatın **uzak** altındaki düğmesini.

    ![Uzak bulut Hizmetleri](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > İlk olarak Uzak Masaüstü'nü etkinleştirin ve (onay işareti) Tamam'ı tıklatın, tüm rol örneklerini yeniden başlatılır. Yeniden başlatma önlemek için parolayı şifrelemek için kullanılan sertifikanın rolünün yüklenmesi gerekir. Bir yeniden başlatmayı engellemek için [bulut hizmeti için bir sertifika karşıya](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) ve bu iletişim kutusuna dönün.

3. İçinde **rolleri**, güncelleştirme veya seçmek istediğiniz rolü seçin **tüm** tüm rolleri için.
4. Aşağıdaki değişiklikleri yapın:

   * Uzak Masaüstü'nü etkinleştirmek için seçin **Uzak Masaüstü'nü etkinleştirme** onay kutusu. Uzak Masaüstü devre dışı bırakmak için onay kutusunu temizleyin.
   * Rol örneği Uzak Masaüstü bağlantılarını kullanmak için bir hesap oluşturun.
   * Var olan hesap parolasını güncelleştirin.
   * Kimlik doğrulaması için kullanılacak karşıya yüklenen bir sertifika seçin (kullanarak sertifikayı karşıya **karşıya** üzerinde **sertifikaları** sayfa) veya yeni bir sertifika oluşturun.
   * Uzak Masaüstü yapılandırmasının sona erme tarihini değiştirin.

5. Tamamladığınızda, yapılandırmayı güncelleştirmelerinizi tıklatın **Tamam** (onay işareti).

## <a name="remote-into-role-instances"></a>Uzaktan rol örnekleri içine
Uzak Masaüstü roller üzerinde etkinleştirildikten sonra bir rol örneği çeşitli araçları aracılığıyla içine uzaktan yapabilirsiniz.

Klasik Azure portalından bir rol örneğine bağlanmak için:

1. Tıklatın **örnekleri** açmak için **örnekleri** sayfası.
2. Yapılandırılmış Uzak Masaüstü sahip rol örneği seçin.
3. Tıklatın **Bağlan**ve Masaüstü açmak için yönergeleri izleyin.
4. Tıklatın **açık** ve ardından **Bağlan** Uzak Masaüstü bağlantısı başlatmak için.

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Visual Studio rol örneği uzak kullanın
Visual Studio'da, Sunucu Gezgini:

1. Genişletme **Azure** > **bulut Hizmetleri** > **[bulut hizmet adı]** düğümü.
2. Genişletin **hazırlama** veya **üretim**.
3. Tek tek rol genişletin.
4. Rol örnekleri birine sağ tıklayın, **Uzak Masaüstü kullanarak bağlan...** ve ardından kullanıcı adını ve parolayı girin.

![Sunucu Gezgini Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a>RDP dosyasını almak için PowerShell kullanın
Kullanabileceğiniz [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) RDP dosyasını almak üzere. Ardından, bulut hizmetine erişmek için Uzak Masaüstü Bağlantısı ile RDP dosyasını kullanabilirsiniz.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Program aracılığıyla Hizmet Yönetimi REST API'si aracılığıyla RDP dosyasını indir
Kullanabileceğiniz [RDP dosyasını karşıdan](https://msdn.microsoft.com/library/jj157183.aspx) REST işlemini RDP dosyası indirilemedi.

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Hizmet tanımı dosyasında Uzak Masaüstü'nü yapılandırmak için
Bu yöntem, uygulama geliştirme sırasında için Uzak Masaüstü etkinleştirmenize izin verir. Bu yaklaşım şifrelenmiş parolalar gerektirir saklanmasını uygulama çözümünüzün yeniden dağıtımını hizmeti yapılandırmanızı dosya ve uzak masaüstü yapılandırması herhangi bir güncelleştirme gerektirir. Bu downsides önlemek istiyorsanız, yukarıda açıklanan tabanlı uzak masaüstü uzantısı yaklaşımı kullanmalısınız.  

Visual Studio için kullanabileceğiniz [bir Uzak Masaüstü Bağlantısı etkinleştirmek](../vs-azure-tools-remote-desktop-roles.md) hizmet tanımı dosyası yaklaşımı kullanarak.  
Aşağıdaki adımlar için hizmet modeli dosyalarından Uzak Masaüstü'nü etkinleştirmek için gereken değişiklikleri açıklar. Visual Studio otomatik olarak yapar bu değişiklikleri yayımlarken.

### <a name="set-up-the-connection-in-the-service-model"></a>Hizmet modelinde bağlantı kurma
Kullanım **içeri aktarmalar** almak için öğesi **RemoteAccess** modülü ve **RemoteForwarder** modülüne [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) dosya.

Hizmet tanımı dosyası ile aşağıdaki örneğe benzemelidir `<Imports>` öğe eklendi.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
[ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) dosyasını aşağıdaki örneğe benzer, Not `<ConfigurationSettings>` ve `<Certificates>` öğeleri. Belirtilen sertifika olmalıdır [bulut hizmetine karşıya](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Ek Kaynaklar
[Bulut hizmetleri yapılandırmak için nasıl](cloud-services-how-to-configure.md)
[bulut SSS - Uzak Masaüstü Hizmetleri](cloud-services-faq.md)
