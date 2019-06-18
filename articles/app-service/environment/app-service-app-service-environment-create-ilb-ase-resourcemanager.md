---
title: ILB ASE kullanarak Azure Resource Manager şablonları - App Service oluştur | Microsoft Docs
description: Azure Resource Manager şablonlarını kullanarak iç yük dengeleyici ASE oluşturmayı öğrenin.
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 35e0dc5dabaf1602b87ec6a8be86ed609f3ea12f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130764"
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Azure Resource Manager Şablonlarını kullanarak ILB ASE oluşturma

> [!NOTE] 
> Bu makale, App Service ortamı v1 hakkında yöneliktir. App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).
>

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
App Service ortamları, genel VIP yerine bir sanal ağ iç adresi ile oluşturulabilir.  Bu iç adresi, iç yük dengeleyici (ILB) adlı bir Azure bileşen tarafından sağlanır.  ILB ASE, Azure portalını kullanarak oluşturulabilir.  Bu, Azure Resource Manager şablonları ile Otomasyon kullanarak da oluşturulabilir.  Bu makalede Azure Resource Manager şablonları ile ILB ASE oluşturmak için gerekli sözdizimi ve adımları gösterilmektedir.

Bir ILB ASE oluşturma sürecinin otomatikleştirme ile ilgili üç adım vardır:

1. İlk temel ASE bir genel VIP yerine bir iç yük dengeleyici adresi kullanarak bir sanal ağ oluşturulur.  Bu adım bir parçası olarak, bir kök etki alanı adı için ILB ASE atanır.
2. ILB ASE oluşturulduktan sonra bir SSL sertifikası karşıya yüklendi.  
3. Karşıya yüklenmiş SSL sertifikasını, açıkça için ILB ASE, "varsayılan" SSL sertifikası olarak atanır.  ILB ASE üzerinde uygulamalar için SSL trafiği için (örn. ASE için atanan ortak kök etki alanını kullanan uygulamalar ele alınır, bu SSL sertifikasını kullanılacak https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>' % S'temeli ILB ASE oluşturma
Bir örnek Azure Resource Manager şablonunu ve ilişkili parametreler dosyası Github'da bulunur [burada][quickstartilbasecreate].

Parametrelerin çoğu *azuredeploy.parameters.json* hem ILB ase, hem de genel bir VIP için bağlı Ase'ler oluşturmak için yaygın dosya.  Out Parametreleri özel notun çağrıları aşağıdaki liste veya ILB ASE oluşturulurken benzersiz şunlardır:

* *Internalloadbalancingmode*:  Çoğu durumda kümesinde 80/443 numaralı bağlantı noktasındaki HTTP/HTTPS trafiğini hem ase'ye FTP hizmeti için bağlantı noktalarını dinledik denetim/veri kanalı anlamına gelir, 3, bu iç sanal ağ adres ayrılmış ILB bağlanacak.  Bu özellik, bunun yerine 2 olarak ayarlanır, yalnızca FTP hizmeti bir ILB adresini, bağlantı noktaları (Denetim hem de veri kanalı) HTTP/HTTPS trafiğini, genel VIP üzerinde kalır ancak bağlanacak ilgili.
* *Dnssuffıx*:  Bu parametre, ASE için atanan varsayılan kök etki alanı tanımlar.  Tüm web uygulamaları için Azure App Service'in genel varyasyonu varsayılan kök etki alanıdır *azurewebsites.net*.  ILB ASE, müşterinin sanal ağa iç olduğundan, ancak bu ortak hizmetin varsayılan kök etki alanını kullanmak için anlam ifade etmez.  Bunun yerine, ILB ASE şirketin iç sanal ağ içinde kullanmak için anlamlı varsayılan kök etki alanı olmalıdır.  Örneğin, bir kuramsal Contoso Corporation'ın bir varsayılan kök etki alanı kullanabilirsiniz *contoso.com iç* uygulamalar yalnızca çözümlenebilir ve Contoso'nun sanal ağdaki erişilebilir olacak şekilde tasarlanmıştır. 
* *ipSslAddressCount*:  Bu parametre 0 değerini otomatik olarak alınır *azuredeploy.json* ILB ase yalnızca tek bir ILB adresini olduğundan dosya.  Bir ILB ASE için açık bir IP SSL adresi yok ve bu nedenle bir ILB ASE için IP SSL adresi havuzu sıfır olarak ayarlanmalıdır, aksi takdirde bir sağlama hatası oluşur. 

Bir kez *azuredeploy.parameters.json* dosya doldurulur için ILB ASE, ILB ASE daha sonra aşağıdaki Powershell kod parçacığını kullanarak oluşturulabilir.  ' % S'dosya yolları, Azure Resource Manager şablon dosyaları makinenizde bulunduğu eşleşecek şekilde değiştirin.  Ayrıca Azure Resource Manager dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamanız unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Sonra Azure Resource Manager şablonu, ILB ASE, oluşturulmaya birkaç saat sürer gönderilir.  Oluşturma işlemi tamamlandıktan sonra App Service ortamları listesi için dağıtım tetiklenen abonelik portalında UX ILB ASE gösterilir.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Karşıya yükleme ve "Varsayılan" SSL sertifikası yapılandırma
ILB ASE oluşturulduktan sonra "varsayılan" SSL sertifikası uygulamalarına SSL bağlantıları kurmak için kullandığınız bir SSL sertifikası ASE ile ilişkili olmalıdır.  ASE'ın varsayılan DNS kuramsal Contoso Corporation örneğiyle devam etmesini sonekidir *contoso.com iç*, ardından bağlantı *https://some-random-app.internal-contoso.com* olan bir SSL sertifikası gerektirir Geçerli * *.internal contoso.com*. 

Bir iç CA'ları da dahil olmak üzere, harici bir verenden sertifika satın alma ve otomatik olarak imzalanan bir sertifika kullanarak geçerli bir SSL sertifikası almak için çeşitli yollar vardır.  SSL sertifikasının kaynağından bağımsız olarak, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

* *Konu*:  Bu öznitelik ayarlanmalıdır **kök etki alanı here.com .your*
* *Konu alternatif adı*:  Bu öznitelik her ikisini de içermelidir **kök etki alanı here.com .your*, ve * *.Your-kök-etki-here.com*.  Her bir uygulamayla ilişkili SCM/Kudu sitesiyle kurulan SSL bağlantılarını biçiminde bir adres kullanarak yapılacak ikinci girdi sebebi *your-app-name.scm.your-root-domain-here.com*.

Geçerli bir SSL sertifikası ile elle, iki ek hazırlık adımları gereklidir.  SSL sertifikasını .pfx dosyası olarak dönüştürülen/kaydedilmiş olması gerekir.  .Pfx dosyasını dahil tüm ara ve kök sertifikaları gerekiyor ve ayrıca bir parola ile korunması gerekir unutmayın.

Ardından sonuç .pfx dosyası için bir Azure Resource Manager şablonu kullanarak SSL sertifikasını karşıya yüklenecek base64 dizesine dönüştürülmesi gerekiyor.  Azure Resource Manager şablonları, metin dosyaları olduğundan, .pfx dosyası, bir şablon parametresi olarak eklenebilir böylece base64 dizesine dönüştürülmesi gerekiyor.

Aşağıdaki Powershell kod parçacığı otomatik olarak imzalanan bir sertifika oluşturma örneği gösterilmektedir, sertifikayı dışarı aktarma bir .pfx dosyası olarak .pfx dosyasını dönüştürme bir base64 kodlamalı dize ve kodlamalı dize ayrı bir dosyaya ardından base64 kaydediliyor.  Base64 kodlaması için Powershell kodu gelen uyarlanmış [Powershell betikleri Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

SSL sertifikası başarıyla oluşturuldu ve bir base64 kodlu dönüştürülen sonra dize, GitHub üzerindeki örnek Azure Resource Manager şablonu [varsayılan SSL sertifikasını yapılandırma] [ configuringDefaultSSLCertificate] kullanılabilir.

Parametrelerinde *azuredeploy.parameters.json* dosya aşağıda listelenmiştir:

* *appServiceEnvironmentName*:  Yapılandırılan ILB ASE adı.
* *existingAseLocation*:  ILB ASE dağıtıldığı Azure bölgesi içeren metin dizesi.  Örneğin:  "Güney Orta ABD".
* *pfxBlobString*:  Based64 ile kodlanmış .pfx dosyası dize gösterimini.  Daha önce gösterilen kod parçacığını kullanarak yer alan "içinde exportedcert.pfx.b64" dizesini kopyalayın ve değeri olarak yapıştırın *pfxBlobString* özniteliği.
* *Parola*:  .Pfx dosyasını güvenliğini sağlamak için kullanılan parola.
* *certificateThumbprint*:  Sertifikanın parmak izi.  Bu değer Powershell'den alıyorsanız (örneğin *$certificate. Parmak izi* önceki kod parçacığında), değeri olarak kullanabileceğiniz-olduğu.  Windows sertifika iletişim kutusundan değeri kopyalarsanız, ancak fazlalık alanları atmak unutmayın.  *CertificateThumbprint* gibi görünmelidir:  AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*:  Sertifika kimlik için kullanılan bir kolay dize tanımlayıcısı kendi seçme.  Ad, Azure Resource Manager için benzersiz tanımlayıcı bir parçası olarak kullanılıyor *Microsoft.Web/certificates* SSL sertifikası temsil eden varlık.  Adı **gerekir** bitiş şu sonekle: \_yourASENameHere_InternalLoadBalancingASE.  Bu sonek portal tarafından sertifikanın ILB özellikli bir ASE'nin güvenliğini sağlamak için kullanılan bir gösterge olarak kullanılır.

Kısaltılmış örneği *azuredeploy.parameters.json* aşağıda gösterilmiştir:

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

Bir kez *azuredeploy.parameters.json* dosya doldurulur, varsayılan SSL sertifikasını, aşağıdaki Powershell kod parçacığı kullanılarak yapılandırılabilir.  ' % S'dosya yolları, Azure Resource Manager şablon dosyaları makinenizde bulunduğu eşleşecek şekilde değiştirin.  Ayrıca Azure Resource Manager dağıtım adı ve kaynak grubu adı için kendi değerlerinizi sağlamanız unutmayın.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Sonra Azure Resource Manager şablonu, değişikliği uygulamak için ön uç ASE başına kabaca kırk dakika sürer gönderilir.  Örneğin, tamamlanması iki ön uç kullanarak, bir boyutta varsayılan ASE ile şablon yaklaşık bir saat ve 20 dakika sürer.  ASE, şablon çalışırken ölçeği mümkün olmayacaktır.  

Şablon tamamlandıktan sonra ILB ASE apps'de HTTPS üzerinden erişilebilir ve bağlantıları güvenli varsayılan SSL sertifikasını.  Varsayılan SSL sertifikasını ILB ASE apps'de yanı sıra varsayılan ana bilgisayar adı uygulama adının bir birleşimi kullanılarak gönderilen olduğunda kullanılır.  Örneğin *https://mycustomapp.internal-contoso.com* için varsayılan SSL sertifikasını kullanacağınız * *.internal contoso.com*.

Ancak, ortak bir çok kiracılı service üzerinde çalışan yeni uygulamalar gibi geliştiriciler de tek tek uygulamalar için özel konak adlarını yapılandırın ve sonra tek tek uygulamalar için benzersiz SNI SSL sertifikası bağlamaları yapılandırın.  

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [App Service Ortamı'na giriş](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

