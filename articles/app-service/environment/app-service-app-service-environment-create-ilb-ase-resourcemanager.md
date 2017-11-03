---
title: "Azure Resource Manager şablonları kullanarak bir ILB ana oluşturma | Microsoft Docs"
description: "Azure Resource Manager şablonları kullanarak bir iç yük dengeleyici ana oluşturmayı öğrenin."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.openlocfilehash: ea9407208f1bf555cf1a6d166825896dec89ec29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Azure Resource Manager Şablonlarını kullanarak ILB ASE oluşturma

> [!NOTE] 
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır. Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
>

## <a name="overview"></a>Genel Bakış
Uygulama hizmeti ortamları, genel VIP yerine bir sanal ağ iç adresiyle oluşturulabilir.  Bu iç adresi iç yük dengeleyiciye (ILB) adlı bir Azure bir bileşen tarafından sağlanır.  Azure Portalı'nı kullanarak bir ILB ana oluşturulabilir.  Bu, Azure Resource Manager şablonları yapmamanız otomasyonu kullanarak da oluşturulabilir.  Bu makalede Azure Resource Manager şablonları ile bir ILB ana oluşturmak için gereken sözdizimi ve adımları anlatılmaktadır.

Bir ILB ana oluşturulmasını otomatik hale getirmede ilgili üç adım vardır:

1. İlk temel ana, genel VIP yerine bir iç yük dengeleyici adresi kullanarak bir sanal ağ içinde oluşturulur.  Bu adım bir parçası olarak, bir kök etki alanı adı ILB ana atanır.
2. ILB ana oluşturulduktan sonra bir SSL sertifikası yüklenir.  
3. Karşıya yüklenen SSL sertifikası açıkça ILB ana "varsayılan" SSL sertifikasını atanmış.  SSL trafiği ILB ana uygulamaları için (örneğin https://someapp.mycustomrootcomain.com) ana için atanan ortak kök etki alanını kullanan uygulamalar ele olduğunda bu SSL sertifikasını kullanılacak

## <a name="creating-the-base-ilb-ase"></a>ILB ana temel oluşturma
Bir örnek Azure Resource Manager şablonunu ve ilişkili parametreler dosyası, Github'da bulunan [burada][quickstartilbasecreate].

İçindeki parametrelerden biri çoğu *azuredeploy.parameters.json* dosya hem ILB ASEs yanı sıra genel bir VIP bağlı ASEs oluşturmaya ortak.  Out Parametreleri özel notunun çağrıları aşağıdaki liste veya bir ILB ana oluştururken benzersiz şunlardır:

* *interalLoadBalancingMode*: Çoğu durumda bu kanal bağlantı noktası 80/443 numaralı bağlantı noktasındaki HTTP/HTTPS trafiğini hem denetim/verileri anlamına gelir 3 kulak için ana FTP hizmeti tarafından kümesi bağlı sanal ağ iç adresi ayrılmış bir ILB.  Bu özellik yerine 2 olarak ayarlanırsa, FTP hizmeti bir ILB adresine bağlantı noktaları (Denetim ve veri kanalı) HTTP/HTTPS trafiğini ortak VIP kalacak ancak bağlanacak ilgili.
* *Dnssuffıx*: Bu parametre için ana atanacak varsayılan kök etki alanını tanımlar.  Tüm web uygulamaları için Azure App Service ortak çeşitlemesi varsayılan kök etki alanıdır *azurewebsites.net*.  Ancak bir ILB Ana Müşteri'nin sanal ağa iç olduğundan, ortak hizmetin varsayılan kök etki alanını kullanmak için doesn't make Sense.  Bunun yerine, bir ILB ana şirketin iç sanal ağ içinde kullanmak için anlamlı bir varsayılan kök etki alanı olmalıdır.  Örneğin, bir varsayılan kök etki alanı kuramsal Contoso Corporation kullanabilirsiniz *contoso.com iç* uygulamalar için yalnızca çözümlenebilir ve Contoso sanal ağ içinde erişilebilir olacak şekilde tasarlanmıştır. 
* *ipSslAddressCount*: Bu parametre için 0 değerini otomatik olarak alınır *azuredeploy.json* ILB ASEs yalnızca tek bir ILB adresine sahip olmadığından dosya.  Hiçbir açık bir ILB ana IP SSL adresleri vardır ve bu nedenle bir ILB ana için IP SSL adres havuzu sıfır olarak ayarlanması gerekir, aksi takdirde sağlama hatası oluşur. 

Bir kez *azuredeploy.parameters.json* dosya doldurulmuş bir ILB ana için ILB ana sonra aşağıdaki Powershell kod parçacığını kullanılarak oluşturulabilir.  Dosya yolları Azure Resource Manager şablonu dosyaları makinenizde bulunduğu eşleşecek şekilde değiştirin.  Azure Resource Manager dağıtım adını ve kaynak grubu adı için kendi değerlerini sağlamak de unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Sonra Azure Resource Manager şablonu oluşturulacak ILB ana için birkaç saat sürecek gönderilir.  Oluşturma işlemi tamamlandıktan sonra ILB ana portalında UX dağıtım tetiklenen abonelik için uygulama hizmeti ortamları listesinde görünecektir.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Karşıya yükleme ve "Varsayılan" SSL sertifikası yapılandırma
ILB ana oluşturulduktan sonra uygulamalar için SSL bağlantısı kurmak için "varsayılan" SSL sertifikası kullanmak bir SSL sertifikası ana ile ilişkilendirilmiş olması gerekir.  DNS ana bilgisayarın varsayılan kuramsal Contoso Corporation örnekle devam edersek sonekidir *contoso.com iç*, ardından bağlantı *https://some-random-app.internal-contoso.com* için geçerli bir SSL sertifikası gerektirir **.internal contoso.com*. 

Bir iç CA'lar da dahil olmak üzere, bir dış veren bir sertifika satın alma ve kendinden imzalı bir sertifika kullanarak geçerli bir SSL sertifikası elde etmek için çeşitli yollar vardır.  SSL sertifikası kaynak bağımsız olarak, aşağıdaki sertifika öznitelikleri doğru şekilde yapılandırılmış olması gerekir:

* *Konu*: Bu öznitelik ayarlamak **kök etki alanı here.com .your*
* *Konu alternatif adı*: Bu öznitelik her ikisini de içermelidir **kök etki alanı here.com .your*, ve **.scm.your-kök-etki-here.com*.  SSL bağlantıları her uygulamayla ilişkili SCM/Kudu siteye biçiminde bir adresi kullanarak yapılacak ikinci giriş sebebi *your-app-name.scm.your-root-domain-here.com*.

Elle içinde geçerli bir SSL sertifikası ile iki ek hazırlık adımları gereklidir.  SSL sertifikası bir .pfx dosyası olarak dönüştürülen/kaydedilmiş olması gerekir.  .Pfx dosyasını tüm ara içerir ve sertifikaları kök gerekir ve ayrıca bir parola ile korunması gerekir unutmayın.

Ardından sonuç .pfx dosyasını SSL sertifikası bir Azure Resource Manager şablonu kullanarak karşıya yüklenecek çünkü bir base64 dizeye dönüştürülmesi gerekiyor.  Azure Resource Manager şablonları metin dosyaları olduğundan, .pfx dosyasını bir şablon parametresi olarak dahil edilebilir şekilde bir base64 dizeye dönüştürülmesi gerekir.

Aşağıdaki Powershell kod parçacığı otomatik olarak imzalanan sertifika oluşturma örneği gösterir, sertifika verme bir .pfx dosyası olarak dize kodlanmış .pfx dosyası bir base64 dönüştürme ve kodlanmış dize ayrı bir dosyaya base64 kaydetme.  Base64 kodlaması için Powershell kodu gelen uyarlanmıştır [Powershell komut dosyaları Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

SSL sertifikası başarıyla üretildi ve kodlanmış bir base64 dönüştürülen sonra dize, örneğin Azure Resource Manager şablonu için github'da [varsayılan SSL sertifikası yapılandırma] [ configuringDefaultSSLCertificate] kullanılabilir.

Parametrelerde *azuredeploy.parameters.json* dosya aşağıda listelenmiştir:

* *appServiceEnvironmentName*: ILB yapılandırılan ana adı.
* *existingAseLocation*: ILB ana burada dağıtılan Azure bölgesi içeren metin dizesi.  Örneğin: "Orta Güney ABD".
* *pfxBlobString*: based64 kodlanmış .pfx dosyası dize gösterimi.  Daha önce gösterilen kod parçacığını kullanarak "exportedcert.pfx.b64" içinde yer alan dizesini kopyalayın ve değeri olarak yapıştırmak *pfxBlobString* özniteliği.
* *Parola*: .pfx dosyasını güvenliğini sağlamak için kullanılan parola.
* *certificateThumbprint*: sertifikanın parmak izi.  Bu değer Powershell'den alırsanız (örneğin *$certificate. Parmak izi* önceki kod parçacığını gelen), değeri olarak kullanabileceğiniz-olduğu.  Windows sertifika iletişim kutusundan değeri kopyalarsanız, gereksiz boşluklar Şerit ancak unutmayın.  *CertificateThumbprint* gibi görünmelidir: AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*: kendi seçme kolay dize tanımlayıcı kimliği için kullanılan sertifikanın.  Azure Resource Manager tanımlayıcısı için bir parçası olarak kullanılan adı *Microsoft.Web/certificates* SSL sertifikası temsil eden varlık.  Adı **gerekir** aşağıdaki sonekiyle sona: \_yourASENameHere_InternalLoadBalancingASE.  Bu soneki portal tarafından sertifika ILB özellikli ana güvenliğini sağlamak için kullanılan bir göstergesi olarak kullanılır.

Kısaltılmış örneği *azuredeploy.parameters.json* aşağıda gösterilmiştir:

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

Bir kez *azuredeploy.parameters.json* dosya doldurulmuş, varsayılan SSL sertifikasının aşağıdaki Powershell kod parçacığını kullanılarak yapılandırılabilir.  Dosya yolları Azure Resource Manager şablonu dosyaları makinenizde bulunduğu eşleşecek şekilde değiştirin.  Azure Resource Manager dağıtım adını ve kaynak grubu adı için kendi değerlerini sağlamak de unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Azure Resource Manager şablonu, kabaca kırk dakika dakika başına ana ön uç değişikliği uygulamak için sürer gönderildikten sonra.  Örneğin, tamamlamak için iki ön uç kullanarak bir boyutta varsayılan ana ile şablonu yaklaşık bir saat ve yirmi dakika sürer.  Şablon çalışırken ana ölçeklendirilmiş mümkün olmaz.  

Şablon işlemi tamamlandıktan sonra ILB ana uygulamaları HTTPS üzerinden erişilebilir ve bağlantıları güvenli varsayılan SSL sertifikası.  Uygulama adı artı varsayılan konak adını kullanan ILB ana uygulamaları ele olduğunda varsayılan SSL sertifikası kullanılır.  Örneğin *https://mycustomapp.internal-contoso.com* varsayılan SSL sertifikası için kullanacağınız **.internal contoso.com*.

Ancak, ortak çok kiracılı hizmeti üzerinde çalışan yalnızca gibi uygulamalar, geliştiriciler özel ana bilgisayar adları tek tek uygulamalar için de yapılandırmanız ve benzersiz SNI SSL sertifikası bağlamaları tek tek uygulamalar için yapılandırın.  

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamı giriş](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

