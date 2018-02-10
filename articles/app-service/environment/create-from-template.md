---
title: "Resource Manager şablonu kullanarak bir Azure uygulama hizmeti ortamı oluşturma"
description: "Bir Resource Manager şablonu kullanarak bir dış veya ILB Azure uygulama hizmeti ortamı oluşturma açıklanmaktadır"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 015bf031aea6b79fcca0a416253e9aa47bb245b6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir ana oluşturma

## <a name="overview"></a>Genel Bakış
Azure uygulama hizmeti ortamları (ASEs), internet'ten erişilebilen bir uç nokta veya bir Azure sanal ağı (VNet) bir iç adresi noktadaki oluşturulabilir. Bir Azure tarafından bir iç yük dengeleyici (ILB) bileşeni olarak adlandırılan bir iç uç nokta oluşturulduktan sonra Bu uç sağlanır. ANA iç IP adresi üzerinde bir ILB ana adı verilir. Genel bir uç nokta ile ana bir dış ana adı verilir. 

Azure portal veya bir Azure Resource Manager şablonu kullanarak bir ana oluşturulabilir. Bu makalede bir dış ana veya ILB ana Resource Manager şablonları oluşturmak için gereken sözdizimi ve adımları anlatılmaktadır. Azure portalında bir ana oluşturmayı öğrenmek için bkz: [bir dış ana olun] [ MakeExternalASE] veya [bir ILB ana olun][MakeILBASE].

Azure portalında bir ana oluşturduğunuzda, sanal ağınızı aynı anda oluşturmak veya uygulamasına dağıtmak için önceden var olan bir sanal ağı seçin. Bir şablondan bir ana oluşturduğunuzda ile başlamanız gerekir: 

* Resource Manager Vnet'i.
* Bu VNet içindeki bir alt ağ. Bir ana alt ağ boyutunu öneririz `/25` karşılık Gelecekteki büyümeyi barındırmak için 128 adreslerine sahip. Ana oluşturulduktan sonra boyutu değiştiremezsiniz.
* Sanal ağınızı kaynak kimliği. Sanal ağ özellikleri altında Azure portalından, bu bilgiler alabilirsiniz.
* Uygulamasına dağıtmak istediğiniz aboneliği.
* Uygulamasına dağıtmak istediğiniz konum.

Ana oluşturmayı otomatikleştirmek için:

1. ANA şablon oluşturun. Bir dış ana oluşturursanız, bu adımdan sonra tamamlandı. Bir ILB ana oluşturursanız, yapmak için birkaç şey daha vardır.

2. ILB ana oluşturulduktan sonra ILB ana etki alanı ile eşleşen bir SSL sertifikası yüklenir.

3. Karşıya yüklenen SSL sertifikası ILB ana "varsayılan" SSL sertifikasını atanır.  (Örneğin, https://someapp.mycustomrootdomain.com) ana için atanan ortak kök etki alanı kullanırken bu sertifikayı SSL trafiği ILB ana uygulamalar için kullanılır.


## <a name="create-the-ase"></a>Ana oluşturma
Bir ana ve onun ilişkili parametreler dosyası oluşturur bir Resource Manager şablonu kullanılabilir [bir örnekte] [ quickstartasev2create] github'da.

Bir ILB ana yapmak istiyorsanız, bu Resource Manager şablonu kullanmak [örnekler][quickstartilbasecreate]. Bunlar için kullanım gereksinimini karşılamak. İçindeki parametrelerden biri çoğu *azuredeploy.parameters.json* dosyasının oluşturulmasını ILB ASEs ve dış ASEs için ortak. Aşağıdaki listede out özel not parametreleri çağıran veya bir ILB ana oluşturduğunuzda, benzersiz:

* *internalLoadBalancingMode*: Çoğu durumda, bu kanal bağlantı noktası 80/443 numaralı bağlantı noktasındaki HTTP/HTTPS trafiğini hem denetim/verileri anlamına gelir 3 kulak için ana FTP hizmeti tarafından kümesi bağlı bir ILB ayrılan sanal ağa iç adresi. Bu özellik 2 olarak ayarlanırsa, FTP hizmeti ile ilgili bağlantı noktaları (Denetim ve veri kanalı) bir ILB adresine bağlanır. HTTP/HTTPS trafiğini ortak VIP kalır.
* *Dnssuffıx*: Bu parametre için ana atanmış varsayılan kök etki alanını tanımlar. Tüm web uygulamaları için Azure App Service ortak çeşitlemesi varsayılan kök etki alanıdır *azurewebsites.net*. Bir ILB Ana Müşteri'nin sanal ağa iç olduğundan, ortak hizmetin varsayılan kök etki alanını kullanmak için doesn't make Sense. Bunun yerine, bir ILB ana şirketin iç sanal ağ içinde kullanmak için anlamlı bir varsayılan kök etki alanı olmalıdır. Örneğin, Contoso Corporation bir varsayılan kök etki alanı kullanabilirsiniz *contoso.com iç* uygulamalar için çözümlenebilir ve yalnızca Contoso sanal ağ içinde erişilebilir olacak şekilde tasarlanmıştır. 
* *ipSslAddressCount*: Bu parametre için 0 değerini otomatik olarak varsayılan *azuredeploy.json* ILB ASEs yalnızca tek bir ILB adresine sahip olmadığından dosya. Hiçbir açık bir ILB ana IP SSL adresleri vardır. Bu nedenle, bir ILB ana için IP SSL adres havuzu sıfır olarak ayarlanması gerekir. Aksi halde, bir sağlama hatası oluşur. 

Sonra *azuredeploy.parameters.json* dosya doldurulur, ana PowerShell kod parçacığını kullanarak oluşturun. Dosya yolları makinenizde Resource Manager şablonu dosya konumlarını eşleşecek şekilde değiştirin. Resource Manager dağıtım adını ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın:

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Yaklaşık bir saat oluşturulması ana alır. Ardından ana ASEs listesi dağıtım tetiklenen abonelik için portalda görüntülenir.

## <a name="upload-and-configure-the-default-ssl-certificate"></a>Karşıya yükleme ve "varsayılan" SSL sertifikası yapılandırma
Uygulamalar için SSL bağlantıları kurmak için kullanılan "varsayılan" SSL sertifikası bir SSL sertifikası ana ile ilişkilendirilmiş olması gerekir. Ana'nın varsayılan DNS son eki ise *contoso.com iç*, https://some-random-app.internal-contoso.com bağlantı için geçerli bir SSL sertifikası gerektirir **.internal contoso.com*. 

İç sertifika yetkilileri kullanarak, bir dış veren bir sertifika satın veya otomatik olarak imzalanan bir sertifika kullanarak geçerli bir SSL sertifikası alın. SSL sertifikası kaynak bağımsız olarak, aşağıdaki sertifika öznitelikleri düzgün şekilde yapılandırılmalıdır:

* **Konu**: Bu öznitelik ayarlamak **kök etki alanı here.com .your*.
* **Konu alternatif adı**: Bu öznitelik her ikisini de içermelidir **kök etki alanı here.com .your* ve **.scm.your-kök-etki-here.com*. SSL bağlantıları her uygulamayla ilişkili SCM/Kudu siteye kullanma biçiminde bir adresi *your-app-name.scm.your-root-domain-here.com*.

Elle içinde geçerli bir SSL sertifikası ile iki ek hazırlık adımları gereklidir. SSL sertifikasını .pfx dosyası olarak dönüştürün/kaydedin. .Pfx dosyasını gerekir ve dahil tüm ara sertifikaların kök unutmayın. Bir parola ile güvenli hale getirin.

.Pfx dosyasını SSL sertifikası bir Resource Manager şablonu kullanarak yüklendiği bir base64 dizeye dönüştürülmesi gerekiyor. Resource Manager şablonları metin dosyaları olduğundan, .pfx dosyasını bir base64 dizeye dönüştürülmesi gerekir. Bu şekilde bir şablon parametresi olarak dahil edilebilir.

Aşağıdaki PowerShell kod parçacığını kullanın:

* Otomatik olarak imzalanan bir sertifika oluşturun.
* Sertifika bir .pfx dosyası olarak dışarı aktarın.
* .Pfx dosyası base64 ile kodlanmış bir dizeye dönüştürün.
* Base64 ile kodlanmış dize ayrı bir dosyaya kaydedin. 

Base64 kodlaması için bu PowerShell kodu gelen uyarlanmıştır [PowerShell komut dosyası blog][examplebase64encoding]:

        $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

        $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
        $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

        $fileName = "exportedcert.pfx"
        Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

        $fileContentBytes = get-content -encoding byte $fileName
        $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
        $fileContentEncoded | set-content ($fileName + ".b64")

SSL sertifikası başarıyla oluşturulur ve Base64 ile kodlanmış bir dize olarak dönüştürülen sonra örnek Resource Manager şablonu kullanmak [varsayılan SSL sertifikası yapılandırma] [ quickstartconfiguressl] github'da. 

Parametrelerde *azuredeploy.parameters.json* dosya burada listelenir:

* *appServiceEnvironmentName*: ILB yapılandırılan ana adı.
* *existingAseLocation*: ILB ana burada dağıtılan Azure bölgesi içeren metin dizesi.  Örneğin: "Orta Güney ABD".
* *pfxBlobString*: .pfx dosyasını based64 kodlanmış bir dize gösterimi. Daha önce gösterilen kod parçacığını kullanın ve "exportedcert.pfx.b64" içinde yer alan dizesini kopyalayın. Değeri olarak yapıştırmak *pfxBlobString* özniteliği.
* *Parola*: .pfx dosyasını güvenliğini sağlamak için kullanılan parola.
* *certificateThumbprint*: sertifikanın parmak izi. Bu değer Powershell'den alırsanız (örneğin, *$certificate. Parmak izi* önceki kod parçacığını gelen), değeri olarak kullanabilirsiniz. Windows sertifika iletişim kutusundan değeri kopyalarsanız, gereksiz boşluklar Şerit unutmayın. *CertificateThumbprint* AF3143EB61D43F6727842115BB7F17BBCECAECAE gibi görünmelidir.
* *certificateName*: kendi seçme kolay dize tanımlayıcı kimliği için kullanılan sertifikanın. Resource Manager tanımlayıcısı için bir parçası olarak kullanılan adı *Microsoft.Web/certificates* SSL sertifikası temsil eden varlık. Adı *gerekir* aşağıdaki sonekiyle sona: \_yourASENameHere_InternalLoadBalancingASE. Azure portal bu soneki bir göstergesi olarak kullanır. sertifika ILB özellikli ana güvenliğini sağlamak için kullanılır.

Kısaltılmış örneği *azuredeploy.parameters.json* burada gösterilir:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Sonra *azuredeploy.parameters.json* dosya doldurulur, PowerShell kod parçacığını kullanarak varsayılan SSL sertifikası yapılandırın. Dosya yolları Resource Manager şablonu dosyaları makinenizde bulunduğu eşleşecek şekilde değiştirin. Resource Manager dağıtım adını ve kaynak grubu adı için kendi değerlerinizi sağlamayı unutmayın:

     $templatePath="PATH\azuredeploy.json"
     $parameterPath="PATH\azuredeploy.parameters.json"

     New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Değişikliği uygulamak için ana ön uç başına kabaca 40 dakika sürer. Örneğin, iki ön uçlar kullanan bir varsayılan boyutlu ana için tamamlanması yaklaşık bir saat ve 20 dakika şablonunu alır. Şablon çalışırken, ana ölçeklendirme olamaz.  

Şablon tamamlandıktan sonra ILB ana uygulamaları HTTPS üzerinden erişilebilir. Bağlantıları varsayılan SSL sertifikası kullanılarak güvenli hale getirilir. Uygulama adına ve varsayılan ana bilgisayar adı bir bileşimini kullanarak ILB ana uygulamaları ele varsayılan SSL sertifikası kullanılır. Örneğin, https://mycustomapp.internal-contoso.com için varsayılan SSL sertifikası kullanır **.internal contoso.com*.

Ancak, ortak çok müşterili hizmeti Çalıştır yalnızca gibi uygulamalar, geliştiriciler özel ana bilgisayar adları tek tek uygulamalar için yapılandırabilirsiniz. Bunlar benzersiz SNI SSL sertifikası bağlamaları tek tek uygulamalar için de yapılandırabilirsiniz.

## <a name="app-service-environment-v1"></a>App Service Ortamı v1 ##
App Service Ortamının iki sürümü vardır: ASEv1 ve ASEv2. Yukarıdaki bilgiler ASEv2’yi temel alır. Bu bölümde ASEv1 ile ASEv2 arasındaki farklar gösterilmektedir.

ASEv1 içinde tüm kaynakları el ile yönetin. Buna ön uçlar, çalışanlar ve IP tabanlı SSL için kullanılan IP adresleri dahildir. Uygulama hizmeti planınızı ölçeklendirebilirsiniz önce da barındırmak istediğiniz çalışan havuzunda Ölçeklendirmesi gerekir.

ASEv1, ASEv2’den farklı bir fiyatlandırma modeli kullanır. ASEv1’de ayrılmış her vCPU için ücret ödersiniz. Ön Uçları veya tüm iş yükleri barındıran olmayan çalışanlar için kullanılan Vcpu'lar dahildir. ASEv1’de bir ASE’nin varsayılan en büyük ölçek boyutu toplam 55 konaktır. Buna çalışanlar ve ön uçlar dahildir. ASEv1’in bir avantajı, klasik bir sanal ağa ve bir Resource Manager sanal ağına dağıtılabilmesidir. ASEv1 hakkında daha fazla bilgi için bkz. [App Service Ortamı v1’e giriş][ASEv1Intro].

Resource Manager şablonu kullanarak bir ASEv1 oluşturmak için bkz: [Resource Manager şablonu ile bir ILB ana v1 oluşturma][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
