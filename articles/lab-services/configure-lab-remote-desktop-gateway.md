---
title: Azure DevTest Labs'de Uzak Masaüstü Ağ geçidi kullanmak için bir laboratuvar yapılandırma | Microsoft Docs
description: RDP bağlantı noktasını kullanıma sunma gerek kalmadan Laboratuvar Vm'leri güvenli erişim sağlamak için Uzak Masaüstü Ağ geçidi ile Azure DevTest Labs'de bir laboratuvar yapılandırmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 430734878c01d10a4e7dd385dc75d8d502a2d82c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079008"
---
# <a name="configure-your-lab-in-azure-devtest-labs-to-use-a-remote-desktop-gateway"></a>Uzak Masaüstü Ağ geçidi kullanmak için Azure DevTest Labs Laboratuvarınızı yapılandırın
Azure DevTest Labs'de Laboratuvar sanal makineleri (VM'ler) güvenli erişim sağlamak laboratuvarınız için Uzak Masaüstü Ağ Geçidi RDP bağlantı noktasını kullanıma sunma gerek kalmadan yapılandırabilirsiniz. Laboratuvar Merkezi bir yerde görüntüleyin ve erişime sahip oldukları tüm sanal makinelere bağlanmak Laboratuvar kullanıcılarınız için sağlar. **Connect** düğmesini **sanal makine** sayfası makineye bağlanmak için açabileceğiniz bir makineye özgü RDP dosyası oluşturur. Daha fazla özelleştirebilir ve Uzak Masaüstü Ağ geçidi için Laboratuvarınızı bağlayarak güvenli RDP bağlantı. 

Laboratuvar kullanıcı doğrudan ağ geçidi makinesinde doğrular veya şirket kimlik bilgilerini kendi makinelerine bağlanmak için bir ağ geçidi etki alanına katılmış makinede kullanabilirsiniz için bu yaklaşım daha güvenlidir. Laboratuvar, kullanıcıların Laboratuvar sanal makinelerinde RDP bağlantı noktası internet'e açık gerek kalmadan bağlanmasına olanak sağlar ağ geçidi makinesine belirteci kimlik doğrulaması kullanarak da destekler. Bu makalede, laboratuar makinelerine bağlanmak için belirteç kimlik doğrulaması kullanan bir laboratuvarı ayarlama konusunda bir örneği açıklanmaktadır.

## <a name="architecture-of-the-solution"></a>Çözüm mimarisi

![Çözüm mimarisi](./media/configure-lab-remote-desktop-gateway/architecture.png)

1. [Alma RDP dosyasının içeriğini](/rest/api/dtl/virtualmachines/getrdpfilecontents) seçtiğinizde eylemini çağırdı **Connect** button.1. 
1. Alma RDP dosyası içeriği eylemi çağırır `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` bir kimlik doğrulama belirteci istemek için.
    1. `{gateway-hostname}` Belirtilen ağ geçidi ana bilgisayar adı olan **Laboratuvar ayarları** Azure portalında laboratuvarınız için sayfa. 
    1. `{lab-machine-name}` bağlanmaya çalıştığınız bilgisayarın adıdır.
    1. `{port-number}` bağlantı yapılması gereken bağlantı noktasıdır. Genellikle bu bağlantı noktası 3389 ' dir. Laboratuvar VM kullanıyorsa [IP paylaşılan](devtest-lab-shared-ip.md) DevTest Labs, bağlantı noktası özelliği, farklı olacaktır.
1. Uzak Masaüstü Ağ Geçidi çağrısından erteler `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` kimlik doğrulama belirteci oluşturmak için bir Azure işlevi için. DevTest Labs hizmeti, istek üstbilgisinde işlev anahtarı otomatik olarak içerir. Laboratuvar anahtar Kasası'nda kaydedilmesini işlevi anahtardır. Bu gizli dizi olarak gösterilecek adı **ağ geçidi belirteci gizli anahtarı** üzerinde **Laboratuvar ayarları** Laboratuvar için sayfa.
1. Belirteç kimlik sertifika tabanlı ağ geçidi makinesine doğrulamak için bir belirteç döndürecek şekilde Azure işlevini bekleniyor.  
1. Alma RDP dosyası, eylem içeriği sonra kimlik doğrulama bilgileri de dahil olmak üzere tam RDP dosyasını döndürür.
1. Tercih edilen RDP bağlantı programınızı kullanarak RDP dosyasını açın. Tüm RDP bağlantı programı belirteci kimlik doğrulaması desteklediğini unutmayın. Kimlik Doğrulama belirtecini bir sona erme tarihi vardır, işlev uygulaması ayarlayın. Belirtecin süresi dolmadan önce VM'yi laboratuvara bağlantısı.
1. Uzak Masaüstü Ağ Geçidi makinesinde RDP dosyası belirteç kimlik doğrulaması sonra bağlantı Laboratuvar makinenize iletilir.

### <a name="solution-requirements"></a>Çözüm gereksinimleri
DevTest Labs belirteci kimlik doğrulaması özelliği ile çalışmak için ağ geçidi makineler, etki alanı adı Hizmetleri (DNS) ve İşlevler için birkaç yapılandırma gereksinimleri vardır.

### <a name="requirements-for-remote-desktop-gateway-machines"></a>Uzak Masaüstü Ağ Geçidi makineler için gereksinimler
- HTTPS trafiğini işlemek için ağ geçidi makinesi üzerinde SSL sertifikası yüklenmelidir. Tek bir makineye varsa sertifikayı ağ geçidi grup veya makinenin FQDN için load balancer'ın tam etki alanı adı (FQDN) eşleşmelidir. Joker SSL sertifikaları çalışmaz.  
- Ağ geçidi makine üzerinde yüklü bir imzalama sertifikası. Kullanarak bir imzalama sertifikası oluşturma [Oluştur SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1) betiği.
- Yükleme [takılabilir kimlik doğrulama](https://code.msdn.microsoft.com/windowsdesktop/Remote-Desktop-Gateway-517d6273) modülü için Uzak Masaüstü Ağ Geçidi belirteci kimlik doğrulamasını destekler. Böyle bir modül biri `RDGatewayFedAuth.msi` gelen ile [System Center Virtual Machine Manager (VMM) görüntüleri](/system-center/vmm/install-console?view=sc-vmm-1807). System Center hakkında daha fazla bilgi için bkz: [System Center belgeleri](https://docs.microsoft.com/system-center/) ve [fiyatlandırma ayrıntıları](https://www.microsoft.com/cloud-platform/system-center-pricing).  
- Ağ Geçidi sunucusuna yapılan istekleri işleyebilir `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}`.

    Tek bir makineye varsa ağ geçidi-ağ geçidi grubunun yük dengeleyicinin FQDN'sini veya makinenin FQDN'si adıdır. `{lab-machine-name}` Bağlanmaya çalıştığınız Laboratuvar makinenin adıdır ve `{port-number}` bağlantı oluşturulacak bağlantı noktasıdır.  Varsayılan olarak, bu bağlantı noktası 3389 ' dir.  Ancak, sanal makine kullanıyorsa [IP paylaşılan](devtest-lab-shared-ip.md) DevTest Labs, bağlantı noktası özelliği, farklı olacaktır.
- [Uygulama isteği yönlendirme](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) modülü için Internet Information Server (IIS), yeniden yönlendirmek için kullanılabilir `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` isteklerine kimlik doğrulaması için bir belirteç almak için isteği işleyen bir azure işlev.


## <a name="requirements-for-azure-function"></a>Azure işlevi için gereksinimleri
Azure işlevi tanıtıcıları biçimi istekle `https://{function-app-uri}/app/host/{lab-machine-name}/port/{port-number}` ve aynı temel kimlik doğrulama belirtecini döndürür ağ geçidi makinelerde yüklü sertifika imzalama. `{function-app-uri}` Olan işlevine erişmek için kullanılan URI. İşlev anahtarı olduğundan otomatik olarak geçirilecek istek üst bilgisinde. Örnek işlevi için bkz: [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs). 


## <a name="requirements-for-network"></a>Ağ gereksinimleri

- DNS ağ geçidi makinelerde yüklü SSL sertifikası ile ilişkili FQDN için ağ geçidi makinesine ya da ağ geçidi makine grubunun yük dengeleyici trafik doğrudan gerekir.
- Bir laboratuvar makinesinden özel IP'ler kullanıyorsa, ağ geçidi makinesinden ya da aynı sanal ağ paylaşımı veya eşlenmiş sanal ağlarda kullanarak Laboratuvar makinesine bir ağ yolu olmalıdır.

## <a name="configure-the-lab-to-use-token-authentication"></a>Belirteç kimlik doğrulaması kullanmak için laboratuvar yapılandırma 
Bu bölümde, belirteci kimlik doğrulamasını destekleyen bir Uzak Masaüstü Ağ geçidi bir makineyi kullanmak için bir laboratuvar yapılandırma işlemi gösterilmektedir. Bu bölümde, bir Uzak Masaüstü Ağ Geçidi grubunu kendisini ayarlama ele alınmamıştır. Bu bilgi için bkz: [Uzak Masaüstü Ağ geçidi oluşturmak için örnek](#sample-to-create-a-remote-desktop-gateway) bu makalenin sonunda bölüm. 

Laboratuvar ayarları güncelleştirmeden önce başarıyla Laboratuvar anahtar Kasası'nda kimlik doğrulama belirteci işlevini yürütmek için gereken anahtar depolayın. İşlevi anahtar değer elde edebileceği **Yönet** Azure portalında işlev sayfası. Bir anahtar kasasında gizli dizi kaydetme hakkında daha fazla bilgi için bkz. [Key Vault'a gizli dizi ekleme](../key-vault/quick-create-portal.md#add-a-secret-to-key-vault). Gizli dizi adı daha sonra kullanmak için kaydedin.

Laboratuvar anahtar kasasının Kimliğini bulmak için aşağıdaki Azure CLI komutunu çalıştırın: 

```azurecli
az resource show --name {lab-name} --resource-type 'Microsoft.DevTestLab/labs' --resource-group {lab-resource-group-name} --query properties.vaultName
```

Aşağıdaki adımları kullanarak belirteci kimlik doğrulaması kullanmak üzere Laboratuvar yapılandırın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. Labs listesinden seçin, **Laboratuvar**.
1. Laboratuvar sayfasında seçin **yapılandırması ve ilkelerini**.
1. Soldaki menünün içinde **ayarları** bölümünden **Laboratuvar ayarları**.
1. İçinde **Uzak Masaüstü** bölümünde, tam etki alanı adı (FQDN) veya Uzak Masaüstü Hizmetleri Ağ Geçidi makinesinin IP adresini girin veya için grubu **ağ geçidi ana bilgisayar adı** alan. Bu değer ağ geçidi makinesi üzerinde kullanılan SSL sertifikası FQDN ile eşleşmesi gerekir.

    ![Uzak Masaüstü seçeneklerinde Laboratuvar ayarları](./media/configure-lab-remote-desktop-gateway/remote-desktop-options-in-lab-settings.png)
1. İçinde **Uzak Masaüstü** bölüm için **ağ geçidi belirteci** gizli dizi, daha önce oluşturduğunuz parolayı girin. Bu değer, işlev anahtarı, ancak işlev anahtarı tutan Laboratuvar anahtar kasasındaki gizli dizi adı değil.

    ![Ağ geçidi belirteci gizli anahtarı Laboratuvar ayarları](./media/configure-lab-remote-desktop-gateway/gateway-token-secret.png)
1. **Kaydet** değişiklikler.

    > [!NOTE] 
    > Tıklayarak **Kaydet**, ediyorsunuz [Uzak Masaüstü Ağ geçidinin Lisans Koşulları'nı](https://www.microsoft.com/licensing/product-licensing/products). Uzak ağ geçidi hakkında daha fazla bilgi için bkz: [Uzak Masaüstü Hizmetleri'ne Hoş Geldiniz](https://aka.ms/rds) ve [uzak masaüstü ortamınızı dağıtma](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).


Otomasyon aracılığıyla Laboratuvar yapılandırma tercih edilen olup olmadığını, [kümesi DevTestLabGateway.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Set-DevTestLabGateway.ps1) ayarlamak bir PowerShell komut dosyası **ağ geçidi ana bilgisayar adı** ve **ağ geçidi belirteci gizli anahtarı**ayarları. [Azure DevTest Labs GitHub deposu](https://github.com/Azure/azure-devtestlab) oluşturur veya güncelleştirir lab ile bir Azure Resource Manager şablonu da sağlar **ağ geçidi ana bilgisayar adı** ve **ağ geçidi belirteci gizli anahtarı**ayarları.

## <a name="configure-network-security-group"></a>Ağ güvenlik grubu yapılandırma
Daha fazla Laboratuvar güvenliğini sağlamak için laboratuvar sanal makineler tarafından kullanılan sanal ağ bir ağ güvenlik grubu (NSG) eklenebilir. Yönergeler için bir NSG kümesi nasıl [oluşturma, değiştirme veya silme bir ağ güvenlik grubu](../virtual-network/manage-network-security-group.md).

Yalnızca da ilk laboratuvarına ulaşmak için ağ geçidi üzerinden giden trafiğe izin veren bir NSG bir örnek aşağıda verilmiştir. Bu kuralın tek ağ geçidi makinesinin IP adresini veya IP adresini ağ geçidi makineleri önündeki yük dengeleyici kaynağıdır.

![Ağ güvenlik grubu - kuralları](./media/configure-lab-remote-desktop-gateway/network-security-group-rules.png)

## <a name="sample-to-create-a-remote-desktop-gateway"></a>Uzak Masaüstü Ağ geçidi oluşturmak için örnek

> [!NOTE] 
> Örnek şablonları kullanarak ediyorsunuz [Uzak Masaüstü Ağ geçidinin Lisans Koşulları'nı](https://www.microsoft.com/licensing/product-licensing/products). Uzak ağ geçidi hakkında daha fazla bilgi için bkz: [Uzak Masaüstü Hizmetleri'ne Hoş Geldiniz](https://aka.ms/rds) ve [uzak masaüstü ortamınızı dağıtma](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).

[Azure DevTest Labs GitHub deposu](https://github.com/Azure/azure-devtestlab) kurulum yardımcı olmak için birkaç örnekleri belirteci kimlik doğrulaması ve Uzak Masaüstü Ağ Geçidi DevTest Labs ile kullanmak için gereken kaynakları sağlar. Bu örnekler, ağ geçidi makinelerini, Laboratuvar ayarları ve işlev uygulaması için Azure Resource Manager şablonları içerir.

Örnek bir çözüm Uzak Masaüstü Ağ Geçidi grubu ayarlamak için aşağıdaki adımları izleyin.

1. İmzalama sertifikası oluşturun.  Çalıştırma [oluşturma SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1). Parmak izi, parola ve oluşturulan sertifikasını Base64 kodlaması kaydedin.
2. Bir SSL sertifikası alın. Denetim etki alanı için SSL sertifikası ile ilişkili FQDN olmalıdır. Parmak izi, parola ve bu sertifikayı Base64 kodlaması kaydedin. PowerShell kullanarak parmak izi almak için aşağıdaki komutları kullanın.

    ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate;
    $cer.Import(‘path-to-certificate’);
    $hash = $cer.GetCertHashString()
    ```

    PowerShell kullanarak Base64 kodlaması almak için aşağıdaki komutu kullanın.

    ```powershell
    [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(‘path-to-certificate’))
    ```
3. Dosyaların indirileceği [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway).

    Şablonu diğer birkaç Resource Manager şablonları ve ilgili kaynaklar aynı taban URI'sine erişim gerektirir. Tüm dosyaların kopyalanacağı [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway) ve bir depolama hesabındaki bir blob kapsayıcısını RDGatewayFedAuth.msi.  
4. Dağıtma **azuredeploy.json** gelen [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway). Şablon aşağıdaki parametreleri alır:
    - adminUsername – gerekli.  Ağ geçidi makineler için yönetici kullanıcı adı.
    - adminPassword – gerekli. Ağ geçidi makineler için yönetici hesabının parolası.
    - Instancecount – oluşturmak için ağ geçidi makine sayısı.  
    - alwaysOn – veya oluşturulan Azure işlev uygulaması normal bir durumda tutmak etkinleştirilip etkinleştirilmeyeceğini belirtir. Azure işlev uygulaması tutma gecikmeler kullanıcıların ilk laboratuvar sanal makinesi için bağlamayı deneyin ancak ücret ödenmesi gerekebilir kaçınır.  
    - tokenLifetime – oluşturulan belirtecin geçerli olacağı süre. Ss biçimidir.
    - sslCertificate – Base64 SSL sertifikası için ağ geçidi makinesine kodlaması.
    - sslCertificatePassword – ağ geçidi makinesi için SSL sertifika parolası.
    - sslCertificateThumbprint - SSL sertifikası yerel sertifika deposunda kimliği için sertifika parmak izi'ni tıklatın.
    - signCertificate – Base64 kodlaması için ağ geçidi makinesine sertifika imzalama için.
    - signCertificatePassword – imzalama sertifikası için ağ geçidi makinesi için parola.
    - signCertificateThumbprint - yerel sertifika deposunda imzalama sertifikasının kimliği için sertifika parmak izi'ni tıklatın.
    - _artifactsLocation – tüm destekleyici kaynakları bulunabileceği URI konumu. Bu değer, tam bir UIR, göreli bir yol olmalıdır.
    - _artifactsLocationSasToken – Azure depolama hesabı konumu ise, destekleyici kaynaklarına erişmek için kullanılan paylaşılan erişim imzası (SAS) belirteci.

    Aşağıdaki komutu kullanarak Azure CLI kullanarak şablonu dağıtılabilir:

    ```azurecli
    az group deployment create --resource-group {resource-group} --template-file azuredeploy.json --parameters @azuredeploy.parameters.json -–parameters _artifactsLocation=”{storage-account-endpoint}/{container-name}” -–parameters _artifactsLocationSasToken = “?{sas-token}”
    ```

    Parametre açıklamaları aşağıda verilmiştir:

    - {Depolama hesabı-bitiş noktası} çalıştırılarak alınabilir `az storage account show --name {storage-acct-name} --query primaryEndpoints.blob`.  {Depolama hesap-adı}, karşıya yüklediğiniz dosyalarının bulunduğu depolama hesabının adıdır.  
    - {Kapsayıcı-adı} karşıya yüklediğiniz dosyalarını tutan kapsayıcı {depolama hesap-adı} içindeki adıdır.  
    - {Sas belirteci-} çalıştırılarak alınabilir `az storage container generate-sas --name {container-name} --account-name {storage-acct-name} --https-only –permissions drlw –expiry {utc-expiration-date}`. 
        - {Depolama hesap-adı}, karşıya yüklediğiniz dosyalarının bulunduğu depolama hesabının adıdır.  
        - {Kapsayıcı-adı} karşıya yüklediğiniz dosyalarını tutan kapsayıcı {depolama hesap-adı} içindeki adıdır.  
        - {Utc-sona erme-date}, SAS belirteci sona erer ve bir SAS belirteci artık depolama hesabına erişmek için kullanılabilir UTC tarihtir.

    GatewayFQDN ve şablon dağıtımı çıktısından gatewayIP için değerleri kaydedin. Ayrıca bulunabilir yeni oluşturulan işlevin işlevi anahtarının değerini kaydetmeniz gerekir [işlev uygulaması ayarları](../azure-functions/functions-how-to-use-azure-function-app-settings.md) sekmesi.
5. Bu FQDN, SSL sertifikası önceki adımdaki gatewayIP IP adresine yönlendirir. Bu nedenle DNS yapılandırın.

    Uzak Masaüstü Ağ Geçidi grubu oluşturup uygun DNS güncelleştirmeler yapıldıktan sonra DevTest labs'deki bir laboratuvara tarafından kullanılmak üzere hazırdır. **Ağ geçidi ana bilgisayar adı** ve **ağ geçidi belirteci gizli anahtarı** ayarları dağıttığınız ağ geçidi makineleri kullanmak üzere yapılandırılması gerekir. 

    > [!NOTE]
    > Bir laboratuvar makinesinden özel IP'ler kullanıyorsa, ağ geçidi makinesinden ya da aynı sanal ağ paylaşımı veya eşlenmiş bir sanal ağ'ı kullanarak Laboratuvar makinesine bir ağ yolu olmalıdır.

    Ağ geçidi hem de Laboratuvar yapılandırıldıktan sonra bağlantı dosyasını oluşturulan Laboratuvar kullanıcı tıkladığında **Connect** belirteci kimlik doğrulaması kullanarak bağlanmak gereken bilgileri otomatik olarak dahil edilir.     

## <a name="next-steps"></a>Sonraki adımlar
Uzak Masaüstü Hizmetleri hakkında daha fazla bilgi için şu makaleye bakın: [Uzak Masaüstü Hizmetleri belgeleri](/windows-server/remote/remote-desktop-services/Welcome-to-rds)


