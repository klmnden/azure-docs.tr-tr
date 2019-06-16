---
title: Azure Resource Manager şablonu ile - App Service ortamı oluşturma
description: Resource Manager şablonu kullanarak bir dış veya ILB Azure App Service ortamı oluşturma açıklanır
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
ms.custom: seodec18
ms.openlocfilehash: bdf722ffa7a7c499ff256392886e0f229f27c7a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66137087"
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir ASE oluşturma

## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure App Service ortamları (ase), İnternet'ten erişilebilen bir uç nokta veya bir iç adresi bir Azure sanal ağı (VNet) içinde bir noktadaki ile oluşturulabilir. Bir iç uç nokta ile oluşturduğunuzda tarafından bir Azure iç yük dengeleyici (ILB) bileşeni olarak adlandırılan bu uç noktaya bulunur. Bir iç IP adresi üzerindeki ASE, ILB ASE olarak adlandırılır. ASE bir genel uç noktası ile dış ASE olarak adlandırılır. 

Azure portal veya Azure Resource Manager şablonu kullanarak bir ASE oluşturulabilir. Bu makalede Resource Manager şablonları ile bir dış ASE veya ILB ASE oluşturmak için ihtiyacınız olan sözdizimini ve adımları gösterilmektedir. Azure portalında bir ASE oluşturmayı öğrenmek için bkz: [dış ASE olun] [ MakeExternalASE] veya [ILB ASE olun][MakeILBASE].

Azure portalında bir ASE oluşturduğunuzda, sanal ağınızı aynı anda oluşturmak ya da uygulamasına dağıtmak için önceden var olan bir VNet seçin. Şablondan ASE oluşturma zaman ile başlaması gerekir: 

* A Resource Manager VNet.
* Bu sanal ağ içindeki alt ağ. Bir ASE alt ağ boyutunu öneririz `/24` gelecekteki büyümeye ve ölçekleme gereksinimlerine uyum sağlamak için 256 adreslerine sahip. ASE oluşturulduktan sonra boyutunu değiştiremezsiniz.
* Ağınızdan kaynak kimliği. Bu bilgiler, sanal ağ özellikleri altında Azure portalından alabilirsiniz.
* Uygulamasına dağıtmak istediğiniz abonelik.
* Uygulamasına dağıtmak istediğiniz konum.

ASE oluşturma işleminiz otomatik hale getirmek için:

1. Şablondan ASE oluşturma. Dış ASE oluşturma, bu adımdan sonra işiniz bittiğinde. ILB ASE oluşturma, yapmak için birkaç şey daha vardır.

2. ILB ASE'nizi oluşturulduktan sonra ILB ASE etki alanıyla eşleşen bir SSL sertifikası karşıya yüklendi.

3. Karşıya yüklenmiş SSL sertifikasını, ILB ASE için "varsayılan" SSL sertifikasını atanır.  ASE için atanan ortak kök etki alanı kullanıldığında, bu sertifika uygulamalara ILB ASE üzerinde SSL trafiği için kullanılır (örneğin, https://someapp.mycustomrootdomain.com).


## <a name="create-the-ase"></a>ASE oluşturma
Bir ASE ve ilişkili parametreler dosyası oluşturan Resource Manager şablonu kullanılabilir [bir örnekte] [ quickstartasev2create] GitHub üzerinde.

ILB ASE yapmak istiyorsanız, bu Resource Manager şablonu kullanma [örnekler][quickstartilbasecreate]. Bunlar için kullanım örneği yararlanılır. Parametrelerin çoğu *azuredeploy.parameters.json* dosya dış ase, ILB ase oluşturma için ortak. Aşağıdaki listede out Parametreleri özel notun çağırır veya ILB ASE oluşturma zaman benzersiz olan:

* *Internalloadbalancingmode*: Çoğu durumda, 80/443 numaralı bağlantı noktasındaki HTTP/HTTPS trafiğini hem denetim/veri kanalı bağlantı noktaları anlamına gelir ve 3 Bu ASE üzerinde FTP hizmeti tarafından dinledik için kümesi bağlı bir ILB ayrılan sanal ağ iç adresi. Bu özelliği 2 olarak ayarlanırsa yalnızca FTP hizmeti ile ilgili bağlantı noktaları (Denetim hem de veri kanalı) için bir ILB adresini bağlıdır. HTTP/HTTPS trafiğini, genel VIP üzerinde kalır.
* *Dnssuffıx*: Bu parametre, ASE için atanan varsayılan kök etki alanı tanımlar. Tüm web uygulamaları için Azure App Service'in genel varyasyonu varsayılan kök etki alanıdır *azurewebsites.net*. ILB ASE, müşterinin sanal ağa iç olduğundan, bu ortak hizmetin varsayılan kök etki alanını kullanmak için anlam ifade etmez. Bunun yerine, ILB ASE şirketin iç sanal ağ içinde kullanmak için anlamlı varsayılan kök etki alanı olmalıdır. Örneğin, Contoso Corporation bir varsayılan kök etki alanını kullanabilir *contoso.com iç* çözümlenebilir ve yalnızca Contoso sanal ağ içinde erişilebilir olacak şekilde tasarlanmıştır uygulamalar için. 
* *ipSslAddressCount*: Bu parametre, 0 değerini otomatik olarak varsayılan *azuredeploy.json* ILB ase yalnızca tek bir ILB adresini olduğundan dosya. ILB ASE için açık IP SSL adres yok. Bu nedenle, ILB ASE için IP SSL adresi havuzu, sıfır olarak ayarlanmalıdır. Aksi takdirde, bir sağlama hatası oluşur. 

Sonra *azuredeploy.parameters.json* PowerShell kod parçacığını kullanarak ASE oluşturma, dosya doldurulur. Dosya yolları, Resource Manager şablon dosyası konumları makinenizde eşleşecek şekilde değiştirin. Resource Manager dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamanız unutmayın:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

Bu işlem yaklaşık bir saat ASE, oluşturulmaya götürür. Ardından ASE Ase'ler listesi için dağıtım tetiklenen abonelik portalında gösterilir.

## <a name="upload-and-configure-the-default-ssl-certificate"></a>Karşıya yükleme ve "varsayılan" SSL sertifikası yapılandırma
Uygulamalar için SSL bağlantıları kurmak için kullanılan "varsayılan" SSL sertifikası olarak bir SSL sertifikası ASE ile ilişkilendirilmiş olması gerekir. ASE'ın varsayılan DNS son eki ise *contoso.com iç*, bağlantı https://some-random-app.internal-contoso.com için geçerli bir SSL sertifikası gerektirir * *.internal contoso.com*. 

İç sertifika yetkililerini kullanarak harici bir verenden sertifika satın alma veya otomatik olarak imzalanan bir sertifika kullanarak geçerli bir SSL sertifikası alın. SSL sertifikası kaynağı ne olursa olsun, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

* **Konu**: Bu öznitelik ayarlanmalıdır **kök etki alanı here.com .your*.
* **Konu alternatif adı**: Bu öznitelik her ikisini de içermelidir **kök etki alanı here.com .your* ve * *.Your-kök-etki-here.com*. Her bir uygulamayla ilişkili SCM/Kudu sitesiyle kurulan SSL bağlantılarını biçiminde bir adres kullanın *your-app-name.scm.your-root-domain-here.com*.

Geçerli bir SSL sertifikası ile elle, iki ek hazırlık adımları gereklidir. SSL sertifikasını .pfx dosyası olarak dönüştürün/kaydedin. .Pfx dosyası olmalıdır tüm ara ve kök sertifikaları içermelidir olduğunu unutmayın. Bir parola ile güvenli hale getirin.

.Pfx dosyası, SSL sertifikasını bir Resource Manager şablonu kullanarak yüklendiği base64 dizesine dönüştürülmesi gerekiyor. Resource Manager şablonları, metin dosyaları olduğundan, .pfx dosyasını base64 dizesine dönüştürülmesi gerekir. Bu şekilde bir şablon parametresi olarak dahil edilebilir.

Aşağıdaki PowerShell kod parçacığını kullanın:

* Otomatik olarak imzalanan bir sertifika oluşturun.
* Sertifika .pfx dosyası olarak dışarı aktarın.
* .Pfx dosyası base64 ile kodlanmış dizeye Dönüştür.
* Base64 ile kodlanmış dize ayrı bir dosyaya kaydedin. 

Bu PowerShell kodu base64 kodlaması için gelen uyarlanmış [PowerShell betikleri blog][examplebase64encoding]:

```powershell
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.pfx"
Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

$fileContentBytes = get-content -encoding byte $fileName
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$fileContentEncoded | set-content ($fileName + ".b64")
```

SSL sertifikası başarıyla oluşturuldu ve base64 ile kodlanmış dizeye dönüştürülür sonra örnek Resource Manager şablonu kullanma [varsayılan SSL sertifikasını yapılandırma] [ quickstartconfiguressl] GitHub üzerinde. 

Parametrelerinde *azuredeploy.parameters.json* dosya burada listelenir:

* *appServiceEnvironmentName*: Yapılandırılan ILB ASE adı.
* *existingAseLocation*: ILB ASE dağıtıldığı Azure bölgesi içeren metin dizesi.  Örneğin: "Güney Orta ABD".
* *pfxBlobString*: .Pfx dosyasını based64 kodlu bir dize gösterimi. Daha önce gösterilen kod parçacığını kullanın ve yer alan "içinde exportedcert.pfx.b64" dizesini kopyalayın. Değeri olarak yapıştırın *pfxBlobString* özniteliği.
* *Parola*: .Pfx dosyasını güvenliğini sağlamak için kullanılan parola.
* *certificateThumbprint*: Sertifikanın parmak izi. Bu değer Powershell'den alıyorsanız (örneğin, *$certificate. Parmak izi* önceki kod parçacığında), değeri olduğu gibi kullanabilirsiniz. Fazla boşlukları atmak Windows sertifika iletişim kutusundan değeri kopyalarsanız, unutmayın. *CertificateThumbprint* AF3143EB61D43F6727842115BB7F17BBCECAECAE gibi görünmelidir.
* *certificateName*: Sertifika kimlik için kullanılan bir kolay dize tanımlayıcısı kendi seçme. Ad, kaynak yöneticisi tanımlayıcısı için bir parçası olarak kullanılıyor *Microsoft.Web/certificates* SSL sertifikası temsil eden varlık. Adı *gerekir* bitiş şu sonekle: \_yourASENameHere_InternalLoadBalancingASE. Azure portalında bu son ek bir gösterge kullanır. sertifikanın ILB özellikli bir ASE'nin güvenliğini sağlamak için kullanılır.

Kısaltılmış örneği *azuredeploy.parameters.json* şurada gösterilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
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
```

Sonra *azuredeploy.parameters.json* dosya doldurulur, PowerShell kod parçacığını kullanarak varsayılan SSL sertifikasını yapılandırın. Dosya yolları, Resource Manager şablonu dosyalarını makinenizde bulunduğu eşleşecek şekilde değiştirin. Resource Manager dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamanız unutmayın:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

Değişikliği uygulamak için ön uç ASE başına yaklaşık 40 dakika sürer. Örneğin, iki ön uçlar kullanan bir varsayılan boyutlu ASE'yi için tamamlanması yaklaşık bir saat ve 20 dakika şablonunu alır. Şablon çalışırken, ASE ölçeklendirilemez.  

Şablon tamamlandıktan sonra ILB ASE apps'de HTTPS üzerinden erişilebilir. Bağlantılar, varsayılan SSL sertifikasını kullanarak güvenli hale getirilir. Uygulama adı ve varsayılan ana bilgisayar adını bir bileşimini kullanarak ILB ASE apps'de ele varsayılan SSL sertifikasını kullanılır. Örneğin, https://mycustomapp.internal-contoso.com için varsayılan SSL sertifikasını kullanan * *.internal contoso.com*.

Ancak, genel çok kiracılı bir hizmet üzerinde çalışan yeni uygulamalar gibi geliştiriciler tek tek uygulamalar için özel bir ana bilgisayar adları yapılandırabilirsiniz. Bunlar, benzersiz SNI SSL sertifikası bağlamaları tek tek uygulamalar için de yapılandırabilirsiniz.

## <a name="app-service-environment-v1"></a>App Service Ortamı v1 ##
App Service ortamı iki sürümü vardır: ASEv1 ve ASEv2. Yukarıdaki bilgiler ASEv2’yi temel alır. Bu bölümde ASEv1 ile ASEv2 arasındaki farklar gösterilmektedir.

ASEv1'de, tüm kaynakları el ile yönetin. Buna ön uçlar, çalışanlar ve IP tabanlı SSL için kullanılan IP adresleri dahildir. App Service planınızın ölçeğini önce sitemi barındırmak istediğiniz çalışan havuzu Ölçeklendirmesi gerekir.

ASEv1, ASEv2’den farklı bir fiyatlandırma modeli kullanır. ASEv1’de ayrılmış her vCPU için ücret ödersiniz. Ön uçlar veya herhangi bir iş yükünü çalışanlar için kullanılan Vcpu'lar dahildir. ASEv1’de bir ASE’nin varsayılan en büyük ölçek boyutu toplam 55 konaktır. Buna çalışanlar ve ön uçlar dahildir. ASEv1’in bir avantajı, klasik bir sanal ağa ve bir Resource Manager sanal ağına dağıtılabilmesidir. ASEv1 hakkında daha fazla bilgi için bkz. [App Service Ortamı v1’e giriş][ASEv1Intro].

Resource Manager şablonu kullanarak bir ASEv1 oluşturma için bkz: [Resource Manager şablonu ile bir ILB ASE v1 oluşturmak][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service/web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
