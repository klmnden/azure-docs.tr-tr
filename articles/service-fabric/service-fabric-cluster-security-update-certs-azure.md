---
title: Bir Azure Service Fabric kümesindeki sertifikaları yönetme | Microsoft Docs
description: Yeni sertifikalar, geçiş sertifikası eklemek ve sertifika için veya bir Service Fabric kümesinden kaldırmak açıklar.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/23/2018
ms.author: chackdan
ms.openlocfilehash: 16758cc85b552e82d3daa63893558e1048bcefb8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207562"
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Ekleme veya Azure Service Fabric kümesi için sertifikaları kaldırın
Service Fabric'ın X.509 sertifikaları nasıl kullandığı tanımak ve hakkında bilgi sahibi olmanız önerilir [küme güvenlik senaryoları](service-fabric-cluster-security.md). Anlamanız gerekir hangi küme sertifika ve devam etmeden önce ne için kullanılır.

Service fabric, istemci sertifikalarını yanı sıra küme oluşturma sırasında sertifika güvenliği yapılandırdığınızda iki küme sertifikalar, birincil ve ikincil bir belirtmenize olanak sağlar. Başvurmak [Portalı aracılığıyla azure bir küme oluşturma](service-fabric-cluster-creation-via-portal.md) veya [Azure Resource Manager aracılığıyla azure bir küme oluşturma](service-fabric-cluster-creation-via-arm.md) oluşturma süresi sırasında ayarlama ile ilgili ayrıntılar için. Yalnızca bir küme sertifika belirtirseniz, zaman oluşturmak, sonra birincil sertifikası olarak kullanılır. Küme oluşturulduktan sonra ikincil olarak yeni bir sertifika ekleyebilirsiniz.

> [!NOTE]
> Güvenli bir küme için her zaman en az bir geçerli (değil iptal edilmiş ve süresi dolan değil) küme dağıtılan sertifika (birincil veya ikincil) (değilse, çalışan küme durur) gerekir. süre sonu, tüm geçerli sertifikaların düşmeden önce 90 gün sistem, bir uyarı izleme ve ayrıca bir uyarı sistem durumu olayı düğümde oluşturur. Şu anda hiçbir e-posta veya var. Bu makalede Service Fabric gönderir herhangi bir bildirim 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Portalı kullanarak bir ikincil küme sertifika Ekle
Azure Portalı aracılığıyla Azure powershell kullanma ikincil küme sertifika eklenemiyor. İşlem, bu belgenin sonraki bölümlerinde gösterilmiştir.

## <a name="swap-the-cluster-certificates-using-the-portal"></a>Portalı kullanarak küme sertifikaları değiştirme
Birincil ve ikincil değiştirmek istiyorsanız bir ikincil küme sertifika başarıyla dağıttıktan sonra güvenlik bölümüne gidin ve bağlam menüsünden birincil ikincil sertifikayla takas 'Birincil ile değiştirme' seçeneğini belirleyin Sertifika.

![Sertifika değiştirme][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Portalı kullanarak bir küme sertifikayı Kaldır
Güvenli bir küme için (dağıtılan en az bir geçerli değil iptal edilmiş ve süresi dolan değil) sertifika (birincil veya ikincil) her zaman gerekir aksi durumda, küme çalışmayı durdurur.

İçin küme güvenlik, güvenlik bölümüne Git için kullanılan ikincil bir sertifikayı kaldırın ve ikincil sertifika bağlam menüsünden 'Delete' seçeneğini belirleyin.

Maksadınızı birincil olarak işaretlenmiş sertifikayı kaldırmak için ise, ikincil kopya ilk değiştirme ve yükseltme tamamlandıktan sonra ikincil silmek gerekir.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Resource Manager Powershell kullanarak bir ikincil sertifika Ekle
> [!TIP]
> Daha iyi ve daha kolay şekilde kullanarak bir ikincil sertifika eklemek şimdi [Ekle AzureRmServiceFabricClusterCertificate](/powershell/module/azurerm.servicefabric/add-azurermservicefabricclustercertificate) cmdlet'i. Bu bölümdeki adımları izlemeden gerek yoktur.  Ayrıca, ilk olarak oluşturup kullanırken küme dağıtmak için kullanılan şablon gerekmez [Ekle AzureRmServiceFabricClusterCertificate](/powershell/module/azurerm.servicefabric/add-azurermservicefabricclustercertificate) cmdlet'i.

Bu adımları, Kaynak Yöneticisi'ni nasıl çalıştığını iyi ve en az bir Resource Manager şablonu kullanarak bir Service Fabric kümesi dağıttıysanız ve kullanışlı kümesi için kullanılan şablonu varsayalım. JSON kullanarak rahat olduğu da varsayılır.

> [!NOTE]
> Örnek bir şablon ve boyunca veya bir başlangıç noktası olarak izlemek için kullanabileceğiniz parametreler arıyorsanız sonra buradan indirin [git deposuna](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Kaynak Yöneticisi şablonunuzu düzenleyin

Aşağıdaki boyunca kolaylığı için örnek 5-VM-1-NodeTypes-Secure_Step2.JSON biz yapmadan tüm düzenlemeleri içerir. Örnek şu adresten edinilebilir [git deposuna](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Tüm adımları izlediğinizden emin olun**

1. Resource Manager şablonu açın, küme dağıtmak için kullanılır. (Önceki depoyu örnek indirdiyseniz, güvenli bir küme dağıtmak için 5-VM-1-NodeTypes-Secure_Step1.JSON kullanın ve bu şablonu açın).

2. Ekleme **iki yeni parametreler** "secCertificateThumbprint" ve "secCertificateUrlValue", şablonunuzun parametre bölümüne "dize" yazın. Aşağıdaki kod parçacığını kopyalayın ve şablonuna ekleyin. Şablonunuzu kaynak bağlı olarak bu, tanımlanan şekilde sonraki adımına geçmek zaten olabilir. 
 
    ```json
       "secCertificateThumbprint": {
          "type": "string",
          "metadata": {
            "description": "Certificate Thumbprint"
          }
        },
        "secCertificateUrlValue": {
          "type": "string",
          "metadata": {
            "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
          }
        },
    
    ```

3. Değişiklik yapma **Microsoft.ServiceFabric/clusters** kaynak - şablonunuzda "Microsoft.ServiceFabric/clusters" kaynak tanımı'nı bulun. Bu tanım özellikleri altında "Sertifika" JSON bulacaksınız aşağıdaki JSON parçacığı gibi görünmelidir etiketi:
   
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Yeni bir etiket "thumbprintSecondary" ekleyin ve "[parameters('secCertificateThumbprint')]" bir değer verin.  

    Kaynak tanımı aşağıdaki gibi görünmelidir artık bunu (kaynağınız şablona bağlı olarak, aşağıdaki parçacığı gibi tam olarak olmayabilir). 

    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    İsterseniz **sertifika alma**, ardından yeni sertifika birincil olarak belirtin ve geçerli birincil ikincil olarak taşıma. Bu, geçerli birincil sertifikanızı rollover içinde bir dağıtım adımı yeni sertifikayı sonuçlanır.
    
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('secCertificateThumbprint')]",
              "thumbprintSecondary": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

4. Değişiklik yapma **tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımları - Microsoft.Compute/virtualMachineScaleSets kaynak tanımı'nı bulun. "Publisher" gidin: "Microsoft.Azure.ServiceFabric" altında "virtualMachineProfile".

    Service Fabric yayımcı ayarlarında şöyle bir şey görmeniz gerekir.
    
    ![Json_Pub_Setting1][Json_Pub_Setting1]
    
    Yeni sertifika girişler ekleyin
    
    ```json
                   "certificateSecondary": {
                        "thumbprint": "[parameters('secCertificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    
    ```

    Özellikleri artık aşağıdaki gibi görünmelidir
    
    ![Json_Pub_Setting2][Json_Pub_Setting2]
    
    İsterseniz **sertifika alma**, ardından yeni sertifika birincil olarak belirtin ve geçerli birincil ikincil olarak taşıma. Bu, geçerli sertifikanızı rollover içinde bir dağıtım adımı yeni sertifikayı sonuçlanır.     

    ```json
                   "certificate": {
                       "thumbprint": "[parameters('secCertificateThumbprint')]",
                       "x509StoreName": "[parameters('certificateStoreValue')]"
                         },
                   "certificateSecondary": {
                        "thumbprint": "[parameters('certificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    ```

    Özellikleri artık aşağıdaki gibi görünmelidir    
    ![Json_Pub_Setting3][Json_Pub_Setting3]

5. Değişiklik yapma **tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımları - Microsoft.Compute/virtualMachineScaleSets kaynak tanımı'nı bulun. "VaultCertificates" gidin:, "OSProfile" altında. Bu gibi görünmelidir.

    ![Json_Pub_Setting4][Json_Pub_Setting4]
    
    SecCertificateUrlValue ekleyin. Aşağıdaki kod parçacığında kullanın:
    
    ```json
                      {
                        "certificateStore": "[parameters('certificateStoreValue')]",
                        "certificateUrl": "[parameters('secCertificateUrlValue')]"
                      }
    
    ```
    Sonuçta elde edilen Json aşağıdakine benzer görünmelidir.
    ![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> 4 ve 5 tüm Nodetypes/Microsoft.Compute/virtualMachineScaleSets kaynak tanımlarında şablonunuzda yinelenen olduğundan emin olun. Bunlardan birini kaçırılması durumunda, sertifika üzerinde sanal makine ölçek kümesi ve (, küme güvenlik için kullanabileceğiniz geçerli sertifika şunun; giderek küme de dahil olmak üzere kümenizdeki öngörülemeyen sonuçlara olacak yüklenmemiş Bu nedenle devam etmeden önce onay çift.
> 
> 

### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Şablon dosyanızın yukarıya eklenen yeni parametreleri yansıtacak şekilde düzenleyin
Örnekten kullanıyorsanız [git deposuna](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) izlemek için örnek 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON içinde değişiklik başlatmak için 

Resource Manager şablonu parametreniz dosya düzenleme, secCertificateThumbprint ve secCertificateUrlValue için iki yeni parametreleri ekleyin. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>Şablon Azure'a dağıtma

- Şimdi şablonunuzu Azure'da dağıtmak hazırsınız. Bir Azure PS sürüm 1 + komut istemi açın.
- Azure hesabınızda oturum açın ve belirli azure aboneliğini seçin. Bu, birden fazla azure aboneliği erişen çok kişi için önemli bir adımdır.

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Şablonu dağıtmadan önce test edin. Kümeniz için dağıtılan aynı kaynak grubunu kullanın.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Şablon, kaynak grubuna dağıtın. Kümeniz için dağıtılan aynı kaynak grubunu kullanın. New-AzureRmResourceGroupDeployment komutunu çalıştırın. Varsayılan değer olduğundan modunu belirtmek gerekmez **artımlı**.

> [!NOTE]
> Mod tamamlandı olarak ayarlarsanız, şablonunuzda olmayan kaynakları yanlışlıkla silebilirsiniz. Bu nedenle bu senaryoda kullanmayın.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Aynı powershell doldurulmuş bir örneği burada verilmiştir.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Dağıtım tamamlandıktan sonra yeni sertifikayı kullanarak, kümeye bağlanın ve bazı sorgular gerçekleştirebilir. Yapabileceklerinizi varsa. Sonra eski sertifika silebilirsiniz. 

Kendinden imzalı bir sertifika kullanıyorsanız, yerel TrustedPeople sertifika deponuza almayı unutmayın.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Hızlı başvuru için güvenli bir kümeye bağlanmak için komutu İşte 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Hızlı başvuru için küme durumu almak için komutu aşağıda verilmiştir

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-to-the-cluster"></a>Uygulama sertifikalarını kümeye dağıtma.

Keyvault düğümlere dağıtılan sertifikaları sağlamak için önceki adımları 5 özetlendiği gibi aynı adımları kullanabilirsiniz. yalnızca tanımlanır ve farklı parametrelerini kullanın.


## <a name="adding-or-removing-client-certificates"></a>Ekleme veya istemci sertifikaları kaldırma

Küme sertifikalara ek olarak, Service Fabric kümesi yönetim işlemlerini gerçekleştirmek için istemci sertifikaları ekleyebilirsiniz.

İstemci sertifikalarını - yönetici iki tür ekleyebilirsiniz veya salt okunur. Bunlar daha sonra küme üzerinde sorgu işlemleri ve yönetim işlemleri erişimi denetlemek için kullanılabilir. Varsayılan olarak, küme sertifikaları ve izin verilen yönetici sertifikalar listesine eklenir.

istemci sertifikalarını herhangi bir sayıda belirtebilirsiniz. Her eklendiği/silindiği için Service Fabric kümesi yapılandırma güncelleştirmede sonuçları


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>İstemci sertifikalarını - yönetici ekleme veya salt okunur Portalı aracılığıyla

1. Güvenlik kısmına gidin ve '+ kimlik doğrulama' düğmesini Güvenliği bölümü üstünde.
2. 'Kimlik doğrulama Ekle' bölümündeki ' kimlik doğrulama ' - 'Salt okunur istemci' veya 'Yönetici istemci' seçin
3. Şimdi yetkilendirme yöntemi seçin. Bu, Service Fabric, bu sertifikayı konu adı veya parmak izini kullanarak araması gerektiğini olup olmadığını gösterir. Genel olarak, bu konu adının yetkilendirme yöntemi kullanmak için en iyi güvenlik yöntemi değildir. 

![İstemci sertifikası ekleme][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>İstemci sertifikalarının - yönetici veya salt okunur Portalı'nı kullanarak silme

İçin küme güvenlik, güvenlik bölümüne Git için kullanılan ikincil bir sertifikayı kaldırın ve belirli bir sertifika bağlam menüsünden 'Delete' seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar
Küme yönetimi hakkında daha fazla bilgi için bu makaleler okuyun:

* [Service Fabric kümesi yükseltme işlemi ve sizden beklentileri](service-fabric-cluster-upgrade.md)
* [İstemciler için rol tabanlı erişim Kurulumu](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


