---
title: Azure Service Fabric en iyi güvenlik yöntemleri | Microsoft Docs
description: Azure Service Fabric güvenliği için en iyi uygulamalar.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 3349abfb1b7cf85247b1bb5de8eb53fa09299b74
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65136479"
---
# <a name="azure-service-fabric-security"></a>Azure Service Fabric güvenliği 

Hakkında daha fazla bilgi için [en iyi güvenlik uygulamaları, Azure](https://docs.microsoft.com/azure/security/), gözden [en iyi güvenlik uygulamaları, Azure Service Fabric](https://docs.microsoft.com/azure/security/azure-service-fabric-security-best-practices)

## <a name="key-vault"></a>Key Vault

[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) önerilen gizli diziler için Azure Service Fabric uygulama ve kümelerin Yönetimi hizmetidir.
> [!NOTE]
> Sertifikaları/gizli bir anahtar Kasası'ndaki bir sanal makine ölçek kümesi sanal makine ölçek kümesi gizli dizi olarak dağıtılan, Key Vault ve sanal makine ölçek kümesi aynı konumda olmalıdır.

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>Service Fabric sertifikayı veren sertifika yetkilisi oluşturma

Bir Azure Key Vault sertifikası ya da oluşturulan veya bir anahtar kasasının içine aktarılır. Anahtar kasası sertifikası oluşturulduğunda, özel anahtarı içinde Key Vault oluşturdunuz ve hiç sertifika sahibine kullanıma sunulan. Anahtar Kasası'nda bir sertifika oluşturmak için kullanılabilecek yöntemler şunlardır:

- Genel-özel anahtar çifti oluşturma ve bir sertifika ile ilişkilendirmek için otomatik olarak imzalanan bir sertifika oluşturun. Sertifika kendi anahtarla imzalanacak. 
- El ile bir genel-özel anahtar çifti oluşturma ve bir X.509 sertifika imzalama isteği oluşturmak için yeni bir sertifika oluşturun. İmzalama isteği, kayıt yetkilisi ya da sertifika yetkilisi tarafından imzalanması. Anahtar Kasası'nda KV sertifika tamamlamak için sertifika bekleyen anahtar ile birleştirilebilir imzalı x509 eşleştirin. Bu yöntem daha fazla adım gerektirse de, özel anahtarı içinde oluşturulur ve anahtar Kasası'na sınırlı olduğundan, daha fazla güvenlik sağlayan verin. Bu, aşağıdaki diyagramda açıklanmıştır. 

Gözden geçirme [Azure anahtar kasası sertifikası oluşturma yöntemleri](https://docs.microsoft.com/azure/key-vault/create-certificate) bakın.

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>Service Fabric kümesi sanal makine ölçek kümeleri için Key Vault sertifikalarını dağıtma

Sanal makine ölçek kümesi sanal makine ölçek kümesi için birlikte bulunan bir keyvault sertifikaları dağıtmak için kullanın [osProfile](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile). Resource Manager şablonu özellikleri şunlardır:

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> Kasa Resource Manager şablon dağıtımı için etkinleştirilmesi gerekir.

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>Service Fabric kümeniz için sertifikanıza erişim denetimi listesi (ACL) uygulama

[Sanal makine ölçek kümesi uzantıları](https://docs.microsoft.com/cli/azure/vmss/extension?view=azure-cli-latest) yayımcı Microsoft.Azure.ServiceFabric düğümleri güvenlik yapılandırmak için kullanılır.
Sertifikalarınızı, Service Fabric kümesi işlemleri için bir ACL uygulamak için aşağıdaki Resource Manager şablonu özellikleri kullanın:

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>Güvenli bir Service Fabric küme sertifikası tarafından ortak adı

Service Fabric kümenizi sertifikası tarafından güvenli hale getirmek için `Common Name`, Resource Manager şablonu özelliğini kullanın [certificateCommonNames](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames)gibi:

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> Service Fabric kümeleri, ana bilgisayarınızın sertifika deposunda bulduğu ilk geçerli sertifikası kullanır. Windows üzerinde bu sertifikanın ortak adı ve verenin parmak iziyle eşleştiğinden süresi dolan son tarihi olan olacaktır.

Azure etki alanı gibi *\<bilgisayarınızı alt etki alanı\>. cloudapp.azure.com veya \<bilgisayarınızı alt etki alanı\>. trafficmanager.net Microsoft'a ait. Sertifika yetkililerini yetkisiz kullanıcılara etki alanları için sertifika veremez. Çoğu kullanıcı, bir etki alanı, bir kayıt şirketinden satın alınan ya da ortak bu ada sahip bir sertifika vermek üzere bir sertifika yetkilisi için bir yetkili etki alanı yöneticisi olmanız gerekir.

Etki alanınızın bir Microsoft IP adresine çözümlemek için DNS hizmeti yapılandırma hakkında ek ayrıntıları gözden geçirmek için nasıl yapılandırılacağını [etki alanınızı barındırmak için Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

> [!NOTE]
> Etki alanı ad sunucularınızın Azure DNS bölge adı sunucularınıza temsilciliği sonra aşağıdaki iki kayıtlarını DNS bölgenizi ekleyin:
> - Bir'bir ' etki alanı olmayan TEPESİNDE kayıt bir `Alias record set` tüm IP adresleri için özel etki alanınızı çözülecektir.
> - 'C' kaydı olmayan sağladığınız Microsoft alt etki alanları için bir `Alias record set`. Örneğin, Traffic Manager ya da yük dengeleyicinin DNS adı kullanabilirsiniz.

Service Fabric kümeniz için özel DNS adını görüntülemek için portal güncelleştirilecek `"managementEndpoint"`, izleme güncelleştirme Service Fabric Küme Kaynak Yöneticisi şablonu özellikleri:

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>Service Fabric paket gizli değerleri şifreleme

Azure Container Registry (ACR) kimlik bilgilerini, ortam değişkenleri, ayarları ve Azure toplu eklentisi depolama hesabı anahtarları Service Fabric paketlerinize şifrelenir ortak değerlerini içerir.

İçin [bir şifreleme sertifikası ayarlayın ve Windows Küme parolaları şifrelemek](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-windows):

Gizli şifreleme için otomatik olarak imzalanan bir sertifika oluşturun:

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

Yönergeleri kullanın [anahtar Kasası'nı dağıtma sertifikaları için Service Fabric kümesi sanal makine ölçek kümeleri](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) Service Fabric kümenizin sanal makine ölçek kümeleri için Key Vault Certificates dağıtılacak.

Aşağıdaki PowerShell komutunu kullanarak, şifrelemek ve ardından Service Fabric uygulama bildiriminizi şifrelenmiş değer ile güncelleştirin:

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

İçin [bir şifreleme sertifikası ayarlayın ve Linux kümelerinde parolaları şifrelemek](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-linux):

Gizli anahtarlarınız şifreleme için otomatik olarak imzalanan bir sertifika oluşturun:

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

Yönergeleri kullanın [anahtar Kasası'nı dağıtma sertifikaları için Service Fabric kümesi sanal makine ölçek kümeleri](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) Service Fabric kümenizin sanal makine ölçek kümeleri için.

Aşağıdaki komutları kullanarak, şifrelemek ve sonra Service Fabric uygulama bildirimi ile şifrelenmiş değer güncelleştirin:

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

Korumalı değerlerinizi şifreleme sonra [şifrelenmiş parolalar Service Fabric uygulamasında belirtin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application), ve [hizmet kodundan şifrelenmiş gizli dizilerin şifresini çözmek](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code).

## <a name="authenticate-service-fabric-applications-to-azure-resources-using-managed-service-identity-msi"></a>Service Fabric uygulamaları için Yönetilen hizmet kimliği (MSI) kullanarak Azure kaynaklarını kimlik doğrulaması

Azure kaynakları için yönetilen kimlikleri hakkında bilgi edinmek için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-does-it-work).
Azure Service Fabric kümeleri, sanal makine ölçek destekleyen kümelerinde, barındırıldığı [yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-managed-identities-for-azure-resources).
Bu MSI hizmetlerin bir listesini almak için görmek için kimlik doğrulaması için kullanılabilir [Azure Active Directory kimlik doğrulamasını destekleyen Azure Hizmetleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-azure-ad-authentication).


Sistem tarafından bir sanal makine ölçek kümesi veya var olan bir sanal makine ölçek kümesi oluşturma sırasında yönetilen kimlik atanan etkinleştirmek için aşağıdaki bildirmek `"Microsoft.Compute/virtualMachinesScaleSets"` özelliği:

```json
"identity": { 
    "type": "SystemAssigned"
}
```
Bkz: [Azure kaynakları için yönetilen kimlikleri nedir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vmss#system-assigned-managed-identity) daha fazla bilgi için.

Oluşturduysanız bir [yönetilen kullanıcı tarafından atanan kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm#create-a-user-assigned-managed-identity), aşağıdaki kaynak atamak için sanal makine ölçek kümesi şablonunuzda bildirin. Değiştirin `\<USERASSIGNEDIDENTITYNAME\>` kullanıcı tarafından atanan adı ile yönetilen oluşturduğunuz kimliği:

```json
"identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
        "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
    }
}
```

Önce uygulama yapabilir, Service Fabric yönetilen kimliğini kullanın, Azure ile kimlik doğrulaması yapmak için ihtiyaç duyduğu kaynaklara izni gerekir.
Aşağıdaki komutlar, bir Azure kaynağına erişim:

```bash
principalid=$(az resource show --id /subscriptions/<YOUR SUBSCRIPTON>/resourceGroups/<YOUR RG>/providers/Microsoft.Compute/virtualMachineScaleSets/<YOUR SCALE SET> --api-version 2018-06-01 | python -c "import sys, json; print(json.load(sys.stdin)['identity']['principalId'])")

az role assignment create --assignee $principalid --role 'Contributor' --scope "/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/<PROVIDER NAME>/<RESOURCE TYPE>/<RESOURCE NAME>"
```

Service Fabric uygulama kodunuzda [bir erişim belirteci edinmek](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#get-a-token-using-http) Azure kaynak aşağıdakine benzer bir REST yaparak Manager için:

```bash
access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true | python -c "import sys, json; print json.load(sys.stdin)['access_token']")

```

Service Fabric uygulamanızı, ardından Active Directory destekleyen Azure kaynaklarında kimlik doğrulamasını için erişim belirteci kullanabilirsiniz.
Aşağıdaki örnek, Cosmos DB kaynak için bunun nasıl yapılacağını gösterir:

```bash
cosmos_db_password=$(curl 'https://management.azure.com/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/Microsoft.DocumentDB/databaseAccounts/<YOUR ACCOUNT>/listKeys?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer $access_token" | python -c "import sys, json; print(json.load(sys.stdin)['primaryMasterKey'])")
```

## <a name="windows-defender"></a>Windows Defender 

Varsayılan olarak, Windows Defender virüsten koruma, Windows Server 2016 üzerinde yüklenir. Ayrıntılar için bkz [Windows Server 2016 üzerinde Windows Defender antivirüs'ü](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016). Kullanıcı arabirimi bazı sku'larda varsayılan olarak yüklenir, ancak gerekli değildir. Yükü Windows Defender tarafından gerçekleştirilen tüm performans etkisi ve kaynak tüketimini azaltmak ve işlemleri ve açık kaynaklı yazılım yolları hariç tutmak güvenlik ilkelerinizi izin verirseniz, aşağıdaki sanal makine ölçek kümesi uzantısı kaynağı bildirmek için Service Fabric kümenizi taramalarından dışlanacak Yöneticisi şablonu özellikleri:


```json
 {
    "name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
    "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
            "AntimalwareEnabled": "true",
            "Exclusions": {
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
            },
            "RealtimeProtectionEnabled": "true",
            "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
            }
        },
        "protectedSettings": null
    }
}
```

> [!NOTE]
> Windows Defender kullanmıyorsanız yapılandırma kuralları için kötü amaçlı yazılımdan koruma belgelerinize bakın. Windows Defender, Linux üzerinde desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda, bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md).
* Sanal makineleri veya Linux çalıştıran bilgisayarlar, bir küme oluşturun: [Bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md).
* Hakkında bilgi edinin [Service Fabric destek seçenekleri](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
