---
title: Azure uygulama ağ geçidi geçişi ve Web uygulaması Güvenlik Duvarı'nı v1'den v2
description: Bu makalede, Azure Application Gateway ve Web uygulaması güvenlik duvarı v1'den v2'ye geçirmek nasıl gösterir
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 0fd605d7d502970dccd37da1f3f70fdadb1094a1
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550442"
---
# <a name="migrate-azure-application-gateway-and-web-application-firewall-from-v1-to-v2"></a>Azure uygulama ağ geçidi geçişi ve Web uygulaması Güvenlik Duvarı'nı v1'den v2

[Azure Application Gateway ve Web uygulaması Güvenlik Duvarı (WAF) v2](application-gateway-autoscaling-zone-redundant.md) otomatik ölçeklendirme ve kullanılabilirlik alanı yedeklilik gibi ek özellikler sunan kullanıma sunulmuştur. Ancak, mevcut v1 ağ geçitlerinin otomatik olarak v2'ye yükseltme değil. V1'den v2'ye geçirmek istiyorsanız, bu makaledeki adımları izleyin.

Geçiş işleminde iki aşaması vardır:

1. Yapılandırma geçişi
2. İstemci trafiğini geçirme

Bu makale, yapılandırma geçiş kapsar. İstemci trafiğini geçişi, ortamınıza bağlı olarak değişir. Ancak, bazı üst düzey, genel öneriler [sağlanan](#migrate-client-traffic).

## <a name="migration-overview"></a>Geçişe genel bakış

Azure PowerShell Betiği kullanılabilir aşağıdakileri yapar:

* Belirttiğiniz sanal ağ alt ağında yeni Standard_v2 veya WAF_v2 ağ geçidi oluşturur.
* Sorunsuz bir şekilde yeni oluşturulan Standard_V2 veya WAF_V2 ağ geçidine v1 standart veya WAF ağ geçidiyle ilişkili yapılandırmasını kopyalar.

### <a name="caveatslimitations"></a>Caveats\Limitations

* Yeni v2 ağ geçidi, yeni genel ve özel IP adresleri bulunur. V2 için sorunsuz bir şekilde mevcut v1 ağ geçidi ile ilişkili IP adreslerini taşınmasını mümkün değildir. Ancak, varolan (ayrılmamış) genel veya özel bir IP adresinin yeni v2 ağ geçidi için tahsis edebilirsiniz.
* V1 ağ geçidinizin bulunduğu sanal ağınızdaki bir IP adresi alanı için başka bir alt ağ sağlamanız gerekir. Betik v1 ağ geçidine sahip olan tüm mevcut alt ağlar v2 ağ geçidi oluşturulamıyor. Mevcut alt ağı zaten varsa, ancak yeterli IP adresi alanı yine de çalışabilir v2 ağ geçidi, sağlanan.
* Bir SSL yapılandırmasını geçirmek için v1 ağ geçidinde kullanılan SSL sertifikası belirtmeniz gerekir.
* FIPS modundayken V1 ağ geçidiniz için etkin varsa, yeni v2 geçidinize geçirilmeyecektir. FIPS modundayken, v2'de desteklenmez.
* IPv6 etkin v1 ağ geçidi geçişi olmayan şekilde v2 IPv6'yı desteklemez. Betik çalıştırırsanız, tamamlanmayabilir.
* V1 ağ geçidi yalnızca özel bir IP adresi varsa, betik, bir genel IP adresi ve yeni v2 ağ geçidi için özel bir IP adresi oluşturur. v2 ağ geçitleri yalnızca özel IP adresleri şu anda desteklenmiyor.

## <a name="download-the-script"></a>Betiği indirin

Geçiş betiği indirin [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureAppGWMigration).

## <a name="use-the-script"></a>Komut dosyası kullan

Yerel PowerShell ortam kurulumu ve tercihlerine bağlı olarak sizin için iki seçenek vardır:

* Yüklü olan Azure Az modülleri yok ya da Azure Az modülleri kaldırma sorun varsa, kullanılacak en iyi seçenek olduğu `Install-Script` betiği çalıştırmak için seçenek.
* Azure Az modülleri tutmak mı, betiği indirip doğrudan çalıştırmak için en iyi sonuç olur.

Azure Az modülleri yüklü olup olmadığını belirlemek için çalıştırma `Get-InstalledModule -Name az`. Az modülleri yüklü ve kullanabileceğiniz görmüyorsanız, `Install-Script` yöntemi.

### <a name="install-using-the-install-script-method"></a>Yükleme betiğini yöntemini kullanarak yükleyin

Bu seçeneği kullanmak için bilgisayarınızda yüklü Azure Az modülleri olmaması gerekir. Bunlar yüklü değilse, aşağıdaki komut, bir hata görüntüler. Azure Az modülleri kaldırma veya komut dosyasını el ile indirin ve çalıştırmak için başka bir seçeneği kullanabilirsiniz.
  
Betiği ile aşağıdaki komutu çalıştırın:

`Install-Script -Name AzureAppGWMigration`

Bu komut, ayrıca gerekli Az modüllerini yükler.  

### <a name="install-using-the-script-directly"></a>Doğrudan komut dosyası kullanarak yükleyin

Bazı Azure Az modülleri yüklü ve bunları kaldıramazsınız (veya bunları kaldırmak istemediğiniz varsa), el ile betiği kullanarak indirebilirsiniz **el ile indirin** sekmesinde betik indirme bağlantısı. Betik, bir ham nupkg dosya olarak indirilir. Betik bu nupkg dosyadan yüklemek için bkz [el ile indirme paketi](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download).

Betiği çalıştırmak için:

1. Kullanım `Connect-AzAccount` Azure'a bağlanmak için.

1. Kullanım `Import-Module Az` Az modülleri içeri aktarmak için.

1. Çalıştırma `Get-Help AzureAppGWMigration.ps1` gerekli parametreleri incelemek için:

   ```
   AzureAppGwMigration.ps1
    -resourceId <v1 application gateway Resource ID>
    -subnetAddressRange <subnet space you want to use>
    -appgwName <string to use to append>
    -sslCertificates <comma-separated SSLCert objects as above>
    -trustedRootCertificates <comma-separated Trusted Root Cert objects as above>
    -privateIpAddress <private IP string>
    -publicIpResourceName <public IP name string>
    -validateMigration -enableAutoScale
   ```

   Betik parametreleri için:
   * **ResourceId: [String]: Gerekli** -Azure kaynak kimliği için mevcut standart v1 veya v1. ağ geçidi WAF budur. Bu dize değerini bulmak için Azure portalına gidin, uygulama ağ geçidi veya WAF kaynak seçin ve tıklayın **özellikleri** ağ geçidi bağlantısı. Kaynak Kimliği, bu sayfada bulunur.

     Ayrıca, kaynak Kimliğini almak için aşağıdaki Azure PowerShell komutları çalıştırabilirsiniz:

     ```azurepowershell
     $appgw = Get-AzApplicationGateway -Name <v1 gateway name> -ResourceGroupName <resource group Name> 
     $appgw.Id
     ```

   * **subnetAddressRange: [String]:  Gerekli** -yeni v2 ağ geçidi içeren yeni bir alt ağ için ayırdığınızdan (veya atamak istediğiniz) IP adresi alanını budur. Bu CIDR gösteriminde belirtilmelidir. Örneğin: 10.0.0.0/24. Bu alt ağ önceden oluşturmanız gerekmez. Mevcut değilse komut sizin için oluşturur.
   * **appgwName: [String]: İsteğe bağlı**. Bu, yeni Standard_v2 veya WAF_v2 ağ geçidi adı olarak kullanmak için belirtin bir dizedir. Bu parametre belirtilirse, bu değilse mevcut v1 ağ geçidi adı soneki ile kullanılacak *_v2* eklenir.
   * **sslCertificates: [PSApplicationGatewaySslCertificate]: İsteğe bağlı**.  SSL sertifikaları v1 geçidinizden göstermek için oluşturabilir PSApplicationGatewaySslCertificate nesnelerin virgülle ayrılmış bir listesini yeni v2 ağ geçidine karşıya yüklenmelidir. Her standart v1 veya WAF v1 ağ geçidi için yapılandırılmış, SSL sertifikaları için yeni bir PSApplicationGatewaySslCertificate nesnesi aracılığıyla oluşturabilirsiniz `New-AzApplicationGatewaySslCertificate` burada gösterilen komutu. Yol, SSL sertifikası dosyanızı ve parola gerekir.

       Bu parametre yalnızca v1 ağ geçidi veya WAF için yapılandırdığınız HTTPS dinleyicileri yoksa isteğe bağlıdır. En az bir HTTPS dinleyicisi Kur varsa, bu parametre belirtmeniz gerekir.

      ```azurepowershell  
      $password = ConvertTo-SecureString <cert-password> -AsPlainText -Force
      $mySslCert1 = New-AzApplicationGatewaySslCertificate -Name "Cert01" `
        -CertificateFile <Cert-File-Path-1> `
        -Password $password 
      $mySslCert2 = New-AzApplicationGatewaySslCertificate -Name "Cert02" `
        -CertificateFile <Cert-File-Path-2> `
        -Password $password
      ```

      İçinde geçirebilirsiniz `$mySslCert1, $mySslCert2` (virgülle ayrılmış) önceki örnekte komut dosyasında bu parametre için değerler olarak.
   * **trustedRootCertificates: [PSApplicationGatewayTrustedRootCertificate]: İsteğe bağlı**. Virgülle ayrılmış listesini göstermek için oluşturabileceğiniz PSApplicationGatewayTrustedRootCertificate nesnelerin [güvenilir kök sertifikaları](ssl-overview.md) arka uç örneklerinizin, v2 ağ geçidinden kimlik doğrulaması için.  

      PSApplicationGatewayTrustedRootCertificate nesnelerin bir listesini oluşturmak için bkz [yeni AzApplicationGatewayTrustedRootCertificate](https://docs.microsoft.com/powershell/module/Az.Network/New-AzApplicationGatewayTrustedRootCertificate?view=azps-2.1.0&viewFallbackFrom=azps-2.0.0).
   * **Privateıpaddress: [String]: İsteğe bağlı**. Yeni v2 geçidinize ilişkilendirmek istediğiniz belirli bir özel IP adresi.  Bu, yeni v2 ağ geçidiniz için ayırdığınız aynı sanal ağda olması gerekir. Belirtilmezse, betik, v2 ağ geçidiniz için özel bir IP adresi ayırır.
    * **publicIpResourceId: [String]: İsteğe bağlı**. Genel IP adresi (standart SKU) kaynak aboneliğinizdeki yeni v2 ağ geçidine ayrılacak istediğiniz ResourceId. Belirtilmezse, betik ile aynı kaynak grubunda yeni bir genel IP ayırır. V2 ağ geçidinin adı ile adıdır *- IP* eklenir.
   * **validateMigration: [geçiş]: İsteğe bağlı**. V2 ağ geçidi oluşturma ve yapılandırma kopya sonra karşılaştırma doğrulamaları bazı temel yapılandırmasını yapmak için betik istiyorsanız bu parametreyi kullanın. Varsayılan olarak, doğrulama gerçekleştirilir.
   * **enableAutoScale: [geçiş]: İsteğe bağlı**. Komut dosyası oluşturulduktan sonra yeni v2 ağ geçidi üzerinde otomatik ölçeklendirmeyi etkinleştirmek istiyorsanız bu parametreyi kullanın. Varsayılan olarak, otomatik ölçeklendirmeyi devre dışıdır. Yeni oluşturulan v2 ağ geçidini daha sonra her zaman el ile etkinleştirebilirsiniz.

1. Uygun parametreleri kullanarak betiği çalıştırın. Bu beş ila yedi tamamlanması dakika sürebilir.

    **Örnek**

   ```azurepowershell
   AzureAppGWMigration.ps1 `
      -resourceId /subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/applicationGateways/myv1appgateway `
      -subnetAddressRange 10.0.0.0/24 `
      -appgwname "MynewV2gw" `
      -sslCertificates $Certs `
      -trustedRootCertificates $trustedCert `
      -privateIpAddress "10.0.0.1" `
      -publicIpResourceId "MyPublicIP" `
      -validateMigration -enableAutoScale
   ```

## <a name="migrate-client-traffic"></a>İstemci trafiğini geçirme

İlk olarak, çift başarıyla yeni bir v2 ağ geçidi tam yapılandırmasıyla oluşturulan betik üzerinden v1 geçidinizden geçirilmiş olduğunu kontrol edin. Bu işlemi Azure portalından doğrulayabilirsiniz.

Ayrıca, az miktarda bir trafik v2 ağ geçidi üzerinden el ile test gönderin.
  
Geçerli uygulama ağ geçidi (standart), istemci trafiğini ve her birine ilişkin önerilerimizle burada alabilirsiniz bazı senaryoları şunlardır:

* **(Bir A kaydı kullanarak) ön uç IP adresini işaret eden özel bir DNS bölgesi (contoso.com gibi), standart S1 veya WAF v1 ağ geçidi ile ilişkili**.

    DNS kaydınızı Standard_v2 application gateway'iniz ile ilişkili ön uç IP veya DNS etiketi işaret edecek şekilde güncelleştirebilirsiniz. DNS kaydınızı yapılandırılmış TTL bağlı olarak, bu, yeni v2 ağ geçidine geçirmek tüm istemci trafiğiniz için biraz zaman alabilir.
* **DNS etiketi işaret eden özel bir DNS bölgesi (örneğin, contoso.com) (örneğin: *myappgw.eastus.cloudapp.azure.com* bir CNAME kaydı kullanılarak) v1 geçidinizle ilişkili**.

   İki seçeneğiniz vardır:

  * Genel IP adresleri, uygulama ağ geçidinde kullanırsanız, denetimli, bunu yapabilirsiniz (Ağırlıklı trafik yönlendirme yöntemini) trafiği yeni v2 ağ geçidine artımlı olarak yönlendirmek için bir Traffic Manager profili kullanarak ayrıntılı geçiş.

    Bunu v1 ve v2 uygulama ağ geçitleri için DNS etiketleri ekleyerek yapabilirsiniz [Traffic Manager profili](../traffic-manager/traffic-manager-routing-methods.md#weighted-traffic-routing-method)ve CNAMEing Traffic Manager etki alanına (örneğin, özel DNS kaydı (örneğin, www.contoso.com) contoso.trafficmanager.NET).
  * Veya, özel etki alanı DNS kaydınızı yeni v2 uygulama ağ geçidinin DNS etiketi işaret edecek şekilde güncelleştirebilirsiniz. DNS kaydınızı yapılandırılmış TTL bağlı olarak, bu, yeni v2 ağ geçidine geçirmek tüm istemci trafiğiniz için biraz zaman alabilir.
* **İstemcilerinize ön uç IP adresi, application Gateway bağlanmak**.

   İstemcilerinizi yeni oluşturulan v2 uygulama ağ geçidiyle ilişkilendirilmiş IP adresleri kullanacak şekilde güncelleştirin. IP adresleri doğrudan kullanmamanızı öneririz. Kendi özel DNS bölgesi (contoso.com gibi) için CNAME olabilir, uygulama ağ geçidi ile ilişkili DNS ad etiketi (örneğin, yourgateway.eastus.cloudapp.azure.com) kullanmayı düşünün.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Yapılandırma v1'den v2'ye geçirmek için Azure PowerShell Betiği ile herhangi bir sınırlama var mıdır?

Evet. Bkz: [uyarılar/sınırlamaları](#caveatslimitations).

### <a name="is-this-article-and-the-azure-powershell-script-applicable-for-application-gateway-waf-product-as-well"></a>Bu makalede, Azure PowerShell Betiği ve Application Gateway WAF ürün için de geçerlidir? 

Evet.

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-v1-gateway-to-the-newly-created-v2-gateway"></a>Ayrıca, Azure PowerShell Betiği yeni oluşturulan v2 ağ geçidine v1 ağ trafiği üzerinde geçiş mu?

Hayır. Azure PowerShell Betiği, yalnızca yapılandırma geçirir. Gerçek trafiği geçiştir sizin sorumluluğunuzdadır ve denetim.

### <a name="is-the-new-v2-gateway-created-by-the-azure-powershell-script-sized-appropriately-to-handle-all-of-the-traffic-that-is-currently-served-by-my-v1-gateway"></a>Azure PowerShell betiği tarafından oluşturulan yeni v2 ağ geçidine tüm my v1 ağ geçidi tarafından şu anda sunulan trafiğini işlemek için uygun şekilde boyutlandırıldığından?

Azure PowerShell Betiği, mevcut v1 geçidinizde trafiğini işlemek için uygun bir boyut ile yeni bir v2 ağ geçidi oluşturur. Otomatik ölçeklendirme varsayılan olarak devre dışıdır, ancak komut dosyasını çalıştırırken otomatik ölçeklendirmeyi etkinleştirebilirsiniz.

### <a name="i-configured-my-v1-gateway--to-send-logs-to-azure-storage-does-the-script-replicate-this-configuration-for-v2-as-well"></a>Günlükleri göndermek için Azure depolama için v1 ağ yapılandırdım. Betik de v2 için bu yapılandırmayı çoğaltmak mu?

Hayır. Bu yapılandırma v2 için komut dosyasını kopyalamaz. Günlük yapılandırmasını ayrı olarak geçirilen v2 ağ geçidine eklemeniz gerekir.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Bu betik kullanarak bazı sorunlarla karşılaşıldı. Nasıl Yardım alabilirim?
  
Bir e-posta gönderebilir appgwmigrationsup@microsoft.com, Azure desteği ile bir destek talebi açabilir veya her ikisini birden yapın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hakkında bilgi edinin. ağ geçidi v2](application-gateway-autoscaling-zone-redundant.md)
