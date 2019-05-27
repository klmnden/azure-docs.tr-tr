---
title: Bir Azure Service Fabric kümesindeki sertifikaları yönetme | Microsoft Docs
description: Yeni sertifikalar, geçiş sertifikası ekleyin ve sertifika için veya bir Service Fabric kümesinden kaldırmak açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2018
ms.author: aljo
ms.openlocfilehash: 0038de621a02a2edf3198686e1f2fc88fb917d9c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66161918"
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Azure'da bir Service Fabric küme için sertifikaları kaldırın veya ekleyin
Service Fabric'ın X.509 sertifikaları nasıl kullandığı hakkında bilgilenmeli ve hakkında bilgi sahibi olmanız önerilir [küme güvenliği senaryoları](service-fabric-cluster-security.md). Anlamanız gerekir bir küme sertifikası ve devam etmeden önce ne için kullanılır.

Azure hizmeti dokularını SDK'ın varsayılan sertifika yük, davranıştır dağıtma ve tanımlanmış bir sertifika ile en uzak sona eren bir tarih geleceğe kullanma; bağımsız olarak, birincil veya ikincil yapılandırma tanımlarını. Klasik davranışa geri dönülüyor olmayan önerilen Gelişmiş eylem olduğundan ve "UseSecondaryIfNewer" ayarı parametre değeri Fabric.Code yapılandırmanızı içinde false olarak ayarlanması gerekir.

Service fabric küme oluşturma sırasında istemci sertifikalarının yanı sıra sertifika güvenliği yapılandırırken iki küme sertifikaları, birincil ve ikincil bir belirtmenize olanak sağlar. Başvurmak [portal aracılığıyla azure kümesine oluşturma](service-fabric-cluster-creation-via-portal.md) veya [Azure Resource Manager aracılığıyla azure kümesine oluşturma](service-fabric-cluster-creation-via-arm.md) oluşturma zamanı en ayarlama ile ilgili ayrıntılar için. Yalnızca bir küme sertifikası seçeneğini belirlerseniz zaman oluşturmak, sonra birincil sertifikası kullanılır. Küme oluşturulduktan sonra ikincil yeni bir sertifika ekleyebilirsiniz.

> [!NOTE]
> Güvenli bir küme için her zaman en az bir geçerli (iptal edilmiş değil ve süresinin dolmadığından) küme dağıtılan sertifikanın (birincil veya ikincil) (Aksi halde çalışmasını küme durdurur) gerekir. süre sonu, tüm geçerli sertifikaları ulaşmadan önce 90 gün sistem bir uyarı izleme ve ayrıca düğümde uyarı sistem durumu olayı oluşturur. Şu anda hiç e-posta veya var. Bu makalede Service Fabric gönderdiği herhangi bir bildirim 
> 
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Portalı kullanarak bir ikincil küme sertifikası ekleme
Azure portalı üzerinden kullanmak Azure powershell ikincil küme sertifikası eklenemiyor. İşlem, bu belgede daha sonra ana hatlarıyla açıklanmıştır.

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Portalı kullanarak bir küme sertifikası Kaldır
Güvenli bir küme için her zaman en az bir geçerli (iptal edilmiş değil ve süresinin dolmadığından) sertifikası gerekir. En sona eren tarihe içine ile dağıtılan sertifika kullanımda olacaktır ve kaldırma küme çalışmasını Durdur yapın; Yalnızca kaldırmak için sertifikanın süresi doldu veya süresi en yakın kullanılmayan bir sertifika emin olun.

İçin bir kullanılmayan küme güvenlik sertifikasının güvenlik bölümüne Git kaldırın ve kullanılmayan sertifikayı bağlam menüsünden 'Delete' seçeneğini belirleyin.

Ardından amacınızla birincil olarak işaretlenmiş sertifikayı kaldırmak için ise, otomatik geçiş davranışını etkinleştirme süresi dolan tarihinden daha fazla ikincil sertifika birincil sertifika daha gelecekte dağıtma gerekecektir; Otomatik geçişi tamamlandıktan sonra birincil sertifikayı silin.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Resource Manager Powershell kullanarak bir ikincil sertifika Ekle
> [!TIP]
> Artık daha iyi ve daha kolay şekilde kullanarak bir ikincil sertifika eklemek [Ekle AzServiceFabricClusterCertificate](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet'i. Bu bölümdeki adımları izlemeden gerek yoktur.  Ayrıca, ilk olarak oluşturmak ve kullanırken kümeyi dağıtmak için kullanılan şablon gerekmeyen [Ekle AzServiceFabricClusterCertificate](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet'i.

Bu adımları Resource Manager nasıl çalışır ile ilgili bilgi sahibi olduğunuz ve Resource Manager şablonu kullanarak en az bir Service Fabric kümesi dağıttıysanız ve kullanışlı kümeyi oluşturmak için kullanılan şablonu varsayılır. JSON kullanarak memnun olduğunuz varsayılır.

> [!NOTE]
> Örnek şablonu ve boyunca veya bir başlangıç noktası olarak izlemek için kullanabileceğiniz parametreler için arıyorsanız, ardından bunu indirmek [git deposu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Resource Manager şablonunuzu düzenleyin

Aşağıdaki boyunca kolaylığı için örnek 5-VM-1-NodeType-Secure_Step2.JSON biz yapacak düzenlemeler içerir. Örnek kullanılabilir [git deposu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Tüm adımları izlediğinizden emin olun**

1. Resource Manager şablonu açın, küme dağıtmak için kullanılır. (Önceki depodan örnek indirdiyseniz, ardından güvenli bir kümeye dağıtmak için 5-VM-1-NodeType-Secure_Step1.JSON kullanın ve ardından bu şablonu açın).

2. Ekleme **iki yeni parametrenin** "secCertificateThumbprint" ve "secCertificateUrlValue", şablon parametre kısmına "dize" yazın. Aşağıdaki kod parçacığını kopyalayın ve şablona ekleyin. Şablonunuzun kaynağına bağlı olarak, bu tanımlı, bu nedenle sonraki adıma geçin zaten olabilir. 
 
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

3. Değişiklik yapma **Microsoft.ServiceFabric/clusters** kaynak - şablonunuzda "Microsoft.ServiceFabric/clusters" kaynak tanımı bulun. Bu tanımın özellikleri altında "Sertifika" JSON bulabilirsiniz etiketi aşağıdaki JSON kod parçacığı gibi görünmelidir:
   
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    "ThumbprintSecondary" yeni bir etiket ekleyin ve "[parameters('secCertificateThumbprint')]" bir değer verin.  

    Kaynak tanımı aşağıdaki gibi görünmelidir artık (şablon kaynağını bağlı olarak, bu tam olarak aşağıdaki kod parçacığında gibi olmayabilir). 

    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    İsterseniz **sertifika alma**, ardından yeni bir sertifika birincil olarak belirtin ve geçerli birincil ikincil olarak taşıma. Bu, geçerli birincil sertifikanızın geçişi içinde yeni bir dağıtım adımı sertifikada sonuçlanır.
    
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('secCertificateThumbprint')]",
              "thumbprintSecondary": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

4. Değişiklik yapma **tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımları - Microsoft.Compute/virtualMachineScaleSets kaynak tanımı bulun. "Publisher" kaydırın: "Microsoft.Azure.ServiceFabric", "virtualMachineProfile" altında.

    Service Fabric yayımcı Ayarları'nda şöyle bir şey görmeniz gerekir.
    
    ![Json_Pub_Setting1][Json_Pub_Setting1]
    
    Yeni sertifika girişler ekleyin
    
    ```json
                   "certificateSecondary": {
                        "thumbprint": "[parameters('secCertificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    
    ```

    Özellikleri artık şöyle görünmelidir
    
    ![Json_Pub_Setting2][Json_Pub_Setting2]
    
    İsterseniz **sertifika alma**, ardından yeni bir sertifika birincil olarak belirtin ve geçerli birincil ikincil olarak taşıma. Bu, geçerli sertifikanızın geçişi içinde yeni bir dağıtım adımı sertifikada sonuçlanır.     

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

    Özellikleri artık şöyle görünmelidir    
    ![Json_Pub_Setting3][Json_Pub_Setting3]

5. Değişiklik yapma **tüm** **Microsoft.Compute/virtualMachineScaleSets** kaynak tanımları - Microsoft.Compute/virtualMachineScaleSets kaynak tanımı bulun. "VaultCertificates" gidin:, "OSProfile" altında. şunun gibi görünmelidir.

    ![Json_Pub_Setting4][Json_Pub_Setting4]
    
    SecCertificateUrlValue ekleyin. Aşağıdaki kod parçacığını kullanın:
    
    ```json
                      {
                        "certificateStore": "[parameters('certificateStoreValue')]",
                        "certificateUrl": "[parameters('secCertificateUrlValue')]"
                      }
    
    ```
    Sonuçta elde edilen Json aşağıdakine benzer görünmelidir.
    ![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> 4 ve 5. adımları tüm Nodetypes/Microsoft.Compute/virtualMachineScaleSets kaynak tanımlarında şablonunuzda yinelenen olduğunu doğrulayın. Bunlardan biri kaçırmanıza sertifika üzerinde sanal makine ölçek kümesi ve kümenin (elde küme güvenlik için kullanabileceğiniz geçerli sertifika yok edersiniz; giderek dahil olmak üzere, kümenizdeki beklenmeyen sonuçlar doğuracağını yüklenmemiş Bu nedenle devam etmeden önce onay, çift.
> 
> 

### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Yukarıda eklediğiniz yeni parametreleri yansıtacak şekilde şablon dosyanızı düzenleyin
Örnekten kullanıyorsanız [git deposu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) örneği takip etmek için örnek 5-VM-1-NodeType-Secure.parameters_Step2.JSON içinde değişiklik başlatmak için 

Resource Manager şablonu parametreniz dosya düzenleme, secCertificateThumbprint ve secCertificateUrlValue için iki yeni parametreler eklendi. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>Şablonu Azure'a dağıtma

- Şablonunuzu Azure'da dağıtmak artık hazırsınız. Bir Azure PS 1 + sürümü komut istemi açın.
- Azure hesabınızda oturum açın ve belirli bir azure aboneliğini seçin. Bu, birden fazla azure aboneliğinize erişimi olan yeni başlayanlar için önemli bir adımdır.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionId <Subscription ID> 

```

Şablonu dağıtmadan önce test edin. Kümenizi şu anda dağıtılmış olan aynı kaynak grubunu kullanın.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Şablon, kaynak grubuna dağıtın. Kümenizi şu anda dağıtılmış olan aynı kaynak grubunu kullanın. New-AzResourceGroupDeployment komutu çalıştırın. Varsayılan değer olduğundan modu belirtmek gerekmez **artımlı**.

> [!NOTE]
> Modu için tam olarak ayarlarsanız, şablonunuzda bulunmayan kaynaklar yanlışlıkla silebilirsiniz. Bu nedenle bu senaryoda kullanmayın.
> 
> 

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Aynı PowerShell doldurulmuş bir örnek aşağıda verilmiştir.

```powershell
$ResourceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResourceGroup2

```

Dağıtım tamamlandıktan sonra yeni sertifikayı kullanarak kümenize bağlanın ve bazı sorgular gerçekleştirin. Yapmak mümkün olduğunda. Ardından, eski sertifika silebilirsiniz. 

Kendinden imzalı bir sertifika kullanıyorsanız, bunları yerel TrustedPeople sertifika deposuna içeri aktarmak unutmayın.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Hızlı başvuru için güvenli bir kümeye bağlanmak için komutu aşağıda verilmiştir 

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

## <a name="deploying-client-certificates-to-the-cluster"></a>İstemci sertifikaları, kümeye dağıtma.

Bir keyvault düğümlerine dağıtılmış sertifikaları sağlamak için önceki adımları 5'te belirtildiği gibi aynı adımları kullanabilirsiniz. Yalnızca tanımlanır ve farklı parametreler kullanın.


## <a name="adding-or-removing-client-certificates"></a>Ekleme veya istemci sertifikalarını kaldırma

Küme sertifikalarını ek olarak, bir Service Fabric kümesinde yönetim işlemlerini gerçekleştirmek için istemci sertifikaları ekleyebilirsiniz.

İki tür istemci sertifikaları - yönetici ekleyebilirsiniz veya salt okunur. Bunlar daha sonra yönetim işlemlerini ve sorgu işlemleri kümede erişimi denetlemek için kullanılabilir. Varsayılan olarak, küme sertifikaları izin verilen yönetici sertifikalar listesine eklenir.

İstemci sertifikaları herhangi bir sayıda belirtebilirsiniz. Her eklendiği/silindiği bir yapılandırmasını güncelleştirmesi için Service Fabric kümesi sonuçlanır.


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>İstemci sertifikaları - yönetici ekleme veya salt okunur Portalı aracılığıyla

1. Güvenlik bölümüne gidin ve '+ kimlik doğrulaması' güvenlik bölümü üzerine düğmesi.
2. 'Kimlik doğrulaması ekleme' bölümünü ' Kimlik doğrulaması türü' - 'Salt okunur istemci' veya 'Yönetici istemci' seçin
3. Artık yetkilendirme yöntemi seçin. Bu, Service Fabric'e bu sertifikayı konu adı veya parmak izi'ni kullanarak şuna olup olmadığını gösterir. Genel olarak, bu konu adının yetkilendirme yöntemi kullanmak için en iyi güvenlik yöntemi değildir. 

![İstemci sertifikası Ekle][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>İstemci sertifikaları - yönetici veya salt okunur portalı kullanarak silme

İçin küme güvenlik, güvenlik bölümüne Git için kullanılan ikincil sertifika kaldırın ve belirli bir sertifika bağlam menüsünden 'Delete' seçeneğini belirleyin.

## <a name="next-steps"></a>Sonraki adımlar
Küme yönetimi hakkında daha fazla bilgi için bu makaleleri okuyun:

* [Service Fabric kümesini yükseltme işlemi ve sizden beklentileri](service-fabric-cluster-upgrade.md)
* [İstemciler için rol tabanlı erişim Kurulumu](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


