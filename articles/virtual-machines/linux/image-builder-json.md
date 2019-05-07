---
title: Bir Azure Görüntü Oluşturucu şablonu (Önizleme) oluşturma
description: Azure Görüntü Oluşturucu ile kullanılacak bir şablon oluşturmayı öğrenin.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: b4646879eb7eeecf41852baab7ab64e4053b05e1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159608"
---
# <a name="preview-create-an-azure-image-builder-template"></a>Önizleme: Azure Görüntü Oluşturucu şablonu oluşturma 

Azure görüntü Oluşturucusu Görüntü Oluşturucu hizmette bilgi geçirmek için bir .json dosyası kullanır. Kendi oluşturmanıza yardımcı olacak bu makalede json dosyasının bölümlerini ele alacağız. Tam .json dosyaları örneklerini görmek için bkz: [Azure Görüntü Oluşturucu GitHub](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).

Bu temel şablonu biçimi şu şekildedir:

```json
 { 
    "type": "Microsoft.VirtualMachineImages/imageTemplates", 
    "apiVersion": "2019-05-01-preview", 
    "location": "<region>", 
    "tags": {
        "<name": "<value>",
        "<name>": "<value>"
             }
    "identity":{},           
    "dependsOn": [], 
    "properties": { 
        "<build timeout in minutes>": {}, 
        "build": {}, 
        "customize": {}, 
        "distribute": {} 
      } 
 } 
```



## <a name="type-and-api-version"></a>Türü ve API sürümü

`type` Olmalıdır kaynak türü `"Microsoft.VirtualMachineImages/imageTemplates"`. `apiVersion` API değişiklikleri zaman içinde değişir, ancak olmalıdır `"2019-05-01-preview"` Önizleme için.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2019-05-01-preview",
```

## <a name="location"></a>Location

Özel görüntü oluşturulacağı bölgeyi konumdur. Görüntü Oluşturucusu önizlemesi, aşağıdaki bölgelerde desteklenir:

- Doğu ABD
- Doğu ABD 2
- Batı Orta ABD
- Batı ABD
- Batı ABD 2


```json
    "location": "<region>",
```
    
## <a name="depends-on-optional"></a>(İsteğe bağlı) bağlıdır

Bu isteğe bağlı bir bölüm, devam etmeden önce bağımlılıkları tamamlandığından emin olmak için kullanılabilir. 

```json
    "dependsOn": [],
```

Daha fazla bilgi için [kaynak bağımlılıkları tanımlayın](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-define-dependencies#dependson).

## <a name="identity"></a>Kimlik
Varsayılan olarak, betik kullanarak veya GitHub'ı ve Azure depolama gibi birden fazla konumdan dosyaları kopyalama Görüntü Oluşturucu destekler. Bunları kullanmak için genel olarak erişilebilir olmalıdır.

Bir Azure User-Assigned yönetilen sizin tarafınızdan tanımlanan kimlik, kimlik 'Depolama Blob verileri okuyucu' en az Azure depolama hesabındaki verilmiş sürece Azure Depolama'ya Görüntü Oluşturucu erişim izin vermek için de kullanabilirsiniz. Başka bir deyişle, harici olarak erişilebilen depolama blobları veya Kurulum SAS belirteçlerini yapmak gerekmez.


```json
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<imgBuilderId>": {}
        }
        },
```

Tam bir örnek için bkz. [ Azure depolamadaki dosyalara erişmek için bir Azure User-Assigned yönetilen kimliği kullanma](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

Görüntü Oluşturucusu User-Assigned kimlik desteği: • • özel etki alanı adlarını desteklemiyor yalnızca tek bir kimlik destekler

Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
Bu özellik dağıtma ile ilgili daha fazla bilgi için bkz: [yapılandırma kimliklerini Azure CLI kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

## <a name="properties-source"></a>Özellikler: kaynak

`source` Bölümü görüntü oluşturucusu tarafından kullanılan kaynak görüntü ile ilgili bilgiler içerir.

API 'görüntüsü derleme kaynağı tanımlayan bir SourceType' gerekiyorsa, şu anda üç tür vardır:
- ISO - kaynak RHEL ISO olduğunda bunu kullanın.
- PlatformImage - belirtilen bir Market görüntüsü kaynağı görüntüsüdür.
- ManagedImage - normal yönetilen bir görüntüden başlatırken bunu kullanın.
- SharedImageVersion - paylaşılan görüntü galerisinde görüntü sürümü kullanırken kaynağı olarak kullanılır.

### <a name="iso-source"></a>ISO kaynağı

Azure Görüntü Oluşturucu yalnızca yayımlanan Red Hat Enterprise Linux 7.x ikili DVD Iso'lar, Önizleme kullanarak destekler. Görüntü Oluşturucusu destekler:
- RHEL 7.3 
- RHEL 7.4 
- RHEL 7.5 
 
```json
"source": {
       "type": "ISO",
       "sourceURI": "<sourceURI from the download center>",
       "sha256Checksum": "<checksum associated with ISO>"
}
```

Almak için `sourceURI` ve `sha256Checksum` değerleri, Git `https://access.redhat.com/downloads` ürün seçip **Red Hat Enterprise Linux**ve desteklenen bir sürüm. 

Listesinde **yükleyiciler ve Red Hat Enterprise Linux sunucu görüntülerini**, Red Hat Enterprise Linux 7.x ikili DVD ve sağlama toplamı için bağlantıyı kopyalamanız gerekir.

> [!NOTE]
> Erişim belirteçleri bağlantılar sık aralıklarla yenilenir, şablon göndermek istediğiniz her seferinde, RH bağlarsanız işaretlemeniz gerekir böylece adresi değişti.
 
### <a name="platformimage-source"></a>PlatformImage kaynağı 
Azure Görüntü Oluşturucu aşağıdaki Azure Market görüntüleri destekler:
* Ubuntu 18.04
* Ubuntu 16.04
* RHEL 7.6
* CentOS 7.6
* Windows 2016
* Windows 2019

```json
        "source": {
            "type": "PlatformImage",
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "18.04-LTS",
                "version": "18.04.201903060"
        },
```


Burada Çalıştır AZ CLI kullanarak kullanılan sanal makinenin, oluşturmak için aynı özelliklerdir aşağıdaki özellikleri almak için: 
 
```azurecli-interactive
az vm image list -l westus -f UbuntuServer -p Canonical --output table –-all 
```

> [!NOTE]
> Sürüm 'Son' olamaz, sürüm numarasını almak için yukarıdaki komutu kullanmalısınız. 

### <a name="managedimage-source"></a>ManagedImage kaynağı

Kaynak görüntü mevcut yönetilen görüntüsü genelleştirilmiş bir VHD veya VM ayarlar. Kaynak yönetilen bir görüntü desteklenen bir işletim sistemi olması ve Azure Görüntü Oluşturucu şablonunuzu ile aynı bölgede olması gerekir. 

```json
        "source": { 
            "type": "ManagedImage", 
                "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
        }
```

`imageId` Yönetilen görüntünün ResourceId olmalıdır. Kullanım `az image list` kullanılabilir görüntülerini listelemek için.


### <a name="sharedimageversion-source"></a>SharedImageVersion kaynağı
Kaynak görüntü paylaşılan görüntü galerisinde mevcut olan bir görüntü sürümü ayarlar. Görüntü sürümü desteklenen bir işletim sistemi olmalıdır ve görüntüyü Azure Görüntü Oluşturucu şablonunuzu aynı bölgede çoğaltılması gerekir. 

```json
        "source": { 
            "type": "SharedImageVersion", 
            "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/p  roviders/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>" 
   } 
```

`imageVersionId` ResourceId görüntü sürümü olması gerekir. Kullanım [az sig görüntü sürüm listesi](/cli/azure/sig/image-version#az-sig-image-version-list) listesi görüntüsü sürümleri.

## <a name="properties-customize"></a>Özellikler: özelleştirme


Görüntü Oluşturucusu birden çok 'özelleştiricilerin' destekler. Özelleştiriciler betikleri çalıştırma ya da sunucular yeniden gibi görüntünüzü özelleştirmek için kullanılan işlevlerdir. 

Kullanırken `customize`: 
- Birden çok özelleştiricilerin kullanabilirsiniz, ancak bir benzersiz olması gerekir `name`.
- Şablonda belirtilen sırada özelleştiricilerin yürütün.
- Tüm özelleştirme bileşen başarısız olur ve bir hata geri rapor bir Özelleştirici, başarısız olursa.
- Görüntü derleme gerektirir ve Görüntü Oluşturucu tamamlamak için yeterli zamana izin vermek için 'buildTimeoutInMinutes' özelliği ayarlamak, ne kadar süre göz önünde bulundurun.
- Betik bir şablonda kullanmadan önce sınamanız önerilir. Kendi sanal makinesinde bir komut dosyası hata ayıklaması daha kolay olacaktır.
- Hassas verileri betiklerde koymayın. 
- Komut dosyası konumları, kullanmakta olduğunuz sürece genel olarak erişilebilir olmasına gerek [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

```json
        "customize": [
            {
                "type": "Shell",
                "name": "<name>",
                "scriptUri": "<path to script>"
            },
            {
                "type": "Shell",
                "name": "<name>",
                "inline": [
                    "<command to run inline>",
                ]
            }

        ],
```     

 
Özelleştirme bölümünde bir dizidir. Azure görüntü Oluşturucusu aracılığıyla özelleştiricilerin sırayla çalıştırılır. Tüm Özelleştirici herhangi bir hata, yapı işlemi başarısız olur. 
 
 
### <a name="shell-customizer"></a>Kabuk Özelleştirici

Kabuk betikleri çalıştırma Kabuk Özelleştirici destekler; bunlar IB bunlara erişmek genel olarak erişilebilir olmalıdır.

```json
    "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "scriptUri": "<link to script>"        
        }, 
    ], 
        "customize": [ 
        { 
            "type": "Shell", 
            "name": "<name>", 
            "inline": "<commands to run>"
        }, 
    ], 
```

İşletim sistemi desteği: Linux 
 
Özellikleri Özelleştir:

- **tür** – Kabuğu 
- **ad** - özelleştirme izleme adı 
- **scriptUri** -URI dosyasının konumu 
- **Satır içi** -Kabuk komutları, virgülle ayrılmış bir dizi.
 
> [!NOTE]
> Kabuk Özelleştirici RHEL ISO kaynağı ile çalışırken, herhangi bir özelleştirme gerçekleşmeden önce bir Red Hat yetkilendirme sunucusu ile kayıt, ilk özelleştirme Kabuk tanıtıcıları emin olmanız gerekir. Özelleştirme tamamlandıktan sonra komut dosyasını yetkilendirme sunucusu ile kaydını kaldırmanız gerekir.

### <a name="windows-restart-customizer"></a>Özelleştirici Windows yeniden başlatma 
Yeniden başlatma Özelleştirici Windows VM'yi yeniden başlatın ve bekleyin yeniden çevrimiçi sağlar, bu, yeniden başlatma gerektiren yazılım yüklemenize olanak tanır.  

```json 
     "customize": [ 
            "type{ ": "WindowsRestart", 
            "restartCommand": "shutdown /r /f /t 0 /c", 
            "restartCheckCommand": "echo Azure-Image-Builder-Restarted-the-VM  > buildArtifacts/azureImageBuilderRestart.txt",
            "restartTimeout": "5m"
         }],
```

İşletim sistemi desteği: Windows
 
Özellikleri Özelleştir:
- **Tür**: WindowsRestart
- **restartCommand** -(isteğe bağlı) yeniden çalıştırılacak komutu. Varsayılan değer: `'shutdown /r /f /t 0 /c \"packer restart\"'`.
- **restartCheckCommand** – yeniden başlatma (isteğe bağlı) başarılı olup olmadığını denetlemek için komutu. 
- **restartTimeout** -magnitude ve birimi bir dize olarak belirtilen zaman aşımı'ı yeniden başlatın. Örneğin, `5m` (5 dakika) veya `2h` (2 saat). Varsayılan değer: ' 5 milyon '


### <a name="powershell-customizer"></a>PowerShell Özelleştirici 
PowerShell betikleri ve satır içi komut çalıştıran Kabuk Özelleştirici desteklediği komut IB bunlara erişmek genel olarak erişilebilir olması gerekir.

```json 
     "customize": [
        { 
             "type": "PowerShell",
             "name":   "<name>",  
             "scriptUri": "<path to script>" 
        },  
        { 
             "type": "PowerShell", 
             "name": "<name>", 
             "inline": "<PowerShell syntax to run>", 
             "valid_exit_codes": "<exit code>" 
         } 
    ], 
```

İşletim sistemi desteği: Windows ve Linux

Özellikleri Özelleştir:

- **tür** – PowerShell.
- **scriptUri** -PowerShell betik dosyasının konumunu URI. 
- **Satır içi** – virgüllerle ayrılmış şekilde çalıştırılması için satır içi komutları.
- **valid_exit_codes** : isteğe bağlı, betik/satır içi döndürülen geçerli kodları komutu, bu komut dosyası/satır içi komut bildirilen hata engel olacak.

### <a name="file-customizer"></a>Özelleştirici dosya

Dosya Özelleştirici GitHub ya da Azure depolama, dosya indirme görüntü Oluşturucusu sağlar. Üzerinde derleme yapıtları dayanan bir görüntü derleme işlem hattı varsa, derleme paylaşımından indirmek için dosya Özelleştirici ayarlayabilir ve yapıtları görüntüye taşıyın.  

```json
     "customize": [ 
         { 
            "type": "File", 
             "name": "<name>", 
             "sourceUri": "<source location>",
             "destination": "<destination>" 
         }
     ]
```

İşletim sistemi desteği: Linux ve Windows 

Dosya Özelleştirici özellikleri:

- **sourceUri** -bir erişilebilir depolama uç noktası bu GitHub ya da Azure depolama olabilir. Yalnızca bir dosya, tüm bir dizin değil de indirebilirsiniz. Bir dizine indirmek gerekiyorsa, sıkıştırılmış bir dosya kullanın ve sonra kabuğu veya PowerShell özelleştiricilerin kullanarak sıkıştırmasını açın. 
- **Hedef** – tam hedef yolu ve dosya adı budur. Başvurulan herhangi bir yol ve alt dizinleri gerekir var, bunlar önceden ayarlamak için kabuğu veya PowerShell özelleştiricilerin'ı kullanın. Betik özelleştiricilerin yolu oluşturmak için kullanabilirsiniz. 

Bu dizinler Windows ve Linux yolları tarafından desteklenir, ancak bazı farklar vardır: 
- Linux işletim sisteminin – yalnızca yolun Görüntü Oluşturucu yazabilirsiniz/tmp ' dir.
- Windows – yolu kısıtlama, ancak yolun mevcut olması gerekir.
 
 
Dosyayı indirin ve belirtilen dizine yerleştirin çalışılırken bir hata varsa Özelleştir adımı başarısız olur ve bu içinde customization.log olacaktır.

Dosya Özelleştirici dosyaları Azure Storage'dan indirilebilir kullanarak [MSI](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage).

### <a name="generalize"></a>Genelleştir 
Varsayılan olarak, Azure Görüntü Oluşturucu 'görüntüyü Genelleştirme için ' her görüntü özelleştirme aşamasına sonunda kod 'sağlamasını kaldırma' da çalışır. Genelleştirme, birden çok VM oluşturmak için tekrar kullanılabilmesi için görüntüyü ayarlandığı oluşturan bir işlemdir. Windows VM'ler için Azure Görüntü Oluşturucu Sysprep kullanır. Linux için Azure Görüntü Oluşturucu çalıştıran ' waagent-sağlamayı kaldırma '. 

Azure Görüntü Oluşturucu gerekirse bu komut özelleştirmenize olanak tanıyacak şekilde genelleştirmek için komutları Görüntü Oluşturucu kullanıcıları her durum için uygun olmayabilir. 

Mevcut özelleştirme geçiriyorsanız ve farklı Sysprep/waagent komutları kullanıyorsanız, Görüntü Oluşturucu genel komutları kullanarak yapabilirsiniz ve VM oluşturma başarısız olursa, kendi Sysprep veya waagent komutları kullanın.

Azure Görüntü Oluşturucu özel bir Windows görüntüsü başarıyla oluşturur ve onu seçip VM oluşturma başarısız veya başarılı bir şekilde tamamlanmazsa Bul VM oluşturma, Windows Server Sysprep belgelerine gözden geçirmek veya bir destek isteği ile yükseltmek gerekir Sorun giderme ve doğru Sysprep kullanım bildirmek Windows Server Sysprep Müşteri Hizmetleri desteği takım.


#### <a name="default-sysprep-command"></a>Varsayılan Sysprep komutu
```powershell
echo '>>> Waiting for GA to start ...'
while ((Get-Service RdAgent).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureTelemetryService).Status -ne 'Running') { Start-Sleep -s 5 }
while ((Get-Service WindowsAzureGuestAgent).Status -ne 'Running') { Start-Sleep -s 5 }
echo '>>> Sysprepping VM ...'
if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force} & $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit
while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 5  } else { break } }
```
#### <a name="default-linux-deprovision-command"></a>Varsayılan Linux sağlamayı kaldırma komutu

```bash
/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync
```

#### <a name="overriding-the-commands"></a>Komutların geçersiz kılma
Komutların geçersiz kılmak için tam dosya adı ile komut dosyaları oluşturmak için PowerShell veya kabuk betiği provisioners kullanın ve bunları doğru dizinler yerleştirin:

* Windows: c:\DeprovisioningScript.ps1
* Linux: /tmp/DeprovisioningScript.sh

Bu komutlar okuyup Görüntü Oluşturucu, bu AIB günlüklere yazılır 'customization.log'. Bkz: [sorun giderme](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md#collecting-and-reviewing-aib-logs) günlükleri toplamak nasıl.
 
## <a name="properties-distribute"></a>Özellikler: dağıtma

Azure görüntü Oluşturucusu üç dağıtım hedeflerini destekler: 

- **managedImage** - yönetilen görüntüsü.
- **sharedImage** -paylaşılan görüntü Galerisi.
- **VHD** -bir depolama hesabında VHD.

Hedef türleri yapılandırmadaki bir görüntüsünü dağıtmak, lütfen bkz [örnekler](https://github.com/danielsollondon/azvmimagebuilder/blob/7f3d8c01eb3bf960d8b6df20ecd5c244988d13b6/armTemplates/azplatform_image_deploy_sigmdi.json#L80).

Dağıtımın yapılacağı birden fazla hedef olabileceğinden, Görüntü Oluşturucu sorgulayarak erişilebilir her dağıtım hedefi için bir durum tutar `runOutputName`.  `runOutputName` Sorgulayabilir, bir nesne bu dağıtım hakkında bilgi için dağıtım sonrası olan. Örneğin, VHD veya görüntü sürümü için burada çoğaltılmış bölgeleri konumunu sorgulayabilirsiniz. Bu, her dağıtım hedefi özelliğidir. `runOutputName` Her dağıtım hedefi için benzersiz olmalıdır.
 
### <a name="distribute-managedimage"></a>Dağıt: managedImage

Görüntü çıktısını bir yönetilen bir görüntü kaynağı olacaktır.

```json
"distribute": [
        {
"type":"managedImage",
       "imageId": "<resource ID>",
       "location": "<region>",
       "runOutputName": "<name>",
       "artifactTags": {
            "<name": "<value>",
             "<name>": "<value>"
               }
         }]
```
 
Özellikler dağıtın:
- **tür** – managedImage 
- **imageId** – hedef görüntü kaynak kimliği beklenen biçim: /subscriptions/<subscriptionId>/resourceGroups/<destinationResourceGroupName>/providers/Microsoft.Compute/images/<imageName>
- **Konum** -yönetilen bir görüntü konumu.  
- **runOutputName** – benzersiz dağıtım tanımlamak için ad.  
- **artifactTags** -isteğe bağlı kullanıcı tanımlı anahtar değer çifti etiketler.
 
 
> [!NOTE]
> Hedef kaynak grubunun mevcut olması gerekir.
> Farklı bir bölgeye dağıtılmış görüntü istiyorsanız, dağıtım süresini artırır. 

### <a name="distribute-sharedimage"></a>Dağıt: sharedImage 
Azure paylaşılan görüntü Galerisi görüntüsünü bölgeye çoğaltmasını yönetme sağlayan yeni bir görüntü yönetimi hizmeti olan sürüm oluşturma ve paylaşma özel görüntüler. Azure görüntü Oluşturucusu paylaşılan resim galerileri tarafından desteklenen bölgeler görüntülerini dağıtabilirsiniz, böylece bu hizmetle dağıtmak destekler. 
 
Paylaşılan görüntü Galerisi oluşur: 
 
- Galeri - birden çok paylaşılan görüntüleri için kapsayıcı. Bir galeri, tek bir bölgede dağıtılır.
- Görüntü tanımları: kavramsal bir gruplandırma görüntüler. 
- Yansıma sürümü - VM veya ölçek kümesi dağıtmak için kullanılan bir görüntü türü budur. Yansıma sürümü nerede VM dağıtılması gerektiğini diğer bölgelere çoğaltılabilir.
 
Görüntü Galerisine dağıtabilmeniz için önce bir galeri oluşturup bir görüntü tanımı gerekir, bkz: [görüntüleri paylaşılan](shared-images.md). 

```json
{
     "type": "sharedImage",
     "galleryImageId": “<resource ID>”,
     "runOutputName": "<name>",
     "artifactTags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
     "replicationRegions": [
        "<region where the gallery is deployed>",
        "<region>"
    ]}
``` 

Paylaşılan resim galerileri özelliklerini dağıtın:

- **tür** -sharedImage  
- **galleryImageId** – paylaşılan görüntü Galerisi kimliği. Biçim: /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageGalleryName>.
- **runOutputName** – benzersiz dağıtım tanımlamak için ad.  
- **artifactTags** -isteğe bağlı kullanıcı tanımlı anahtar değer çifti etiketler.
- **replicationRegions** -bölgeler çoğaltma için bir dizi. Bölgelerinden birini galeri dağıtıldığı bölgede olması gerekir.
 
> [!NOTE]
> Galeri için farklı bir bölgede Azure Görüntü Oluşturucu kullanabilirsiniz ancak bu daha uzun sürer ve Azure Görüntü Oluşturucu hizmeti görüntünün veri merkezleri arasında aktarmak ihtiyaç duyacaksınız. Görüntü Oluşturucusu olacak otomatik olarak monoton bir tamsayı üzerinde temel görüntünün sürümü, şu anda belirtemezsiniz. 

### <a name="distribute-vhd"></a>Dağıtın: VHD  
Bir VHD'ye çıkış sağlayabilir. VHD'yi kopyalayın ve bunu Azure Marketi'nde içerik yayımlamak veya Azure Stack ile kullanmak için kullanabilirsiniz.  

```json
 { 
     "type": "VHD",
     "runOutputName": "<VHD name>",
     "tags": {
          "<name": "<value>",
           "<name>": "<value>"
             }
 }
```
 
İşletim sistemi desteği: Windows ve Linux

VHD parametreleri dağıtın:

- **tür** -VHD.
- **runOutputName** – benzersiz dağıtım tanımlamak için ad.  
- **etiketleri** -isteğe bağlı kullanıcı tanımlı anahtar değer çifti etiketler.
 
Azure Görüntü Oluşturucu, kullanıcı bir depolama hesabı konumu belirtmek izin vermez ancak durumunu sorgulayabilirsiniz `runOutputs` konumu alınamıyor.  

```azurecli-interactive
az resource show \
   --ids "/subscriptions/$subscriptionId/resourcegroups/<imageResourceGroup>/providers/Microsoft.VirtualMachineImages/imageTemplates/<imageTemplateName>/runOutputs/<runOutputName>"  | grep artifactUri 
```

> [!NOTE]
> VHD oluşturulduktan sonra olabildiğince çabuk farklı bir konuma kopyalayın. VHD görüntüsü şablonu Azure Görüntü Oluşturucu hizmetine gönderildiğinde oluşturulan geçici kaynak grubunda bir depolama hesabında depolanır. Görüntü şablonunu silerseniz, VHD kaybedersiniz. 
 
## <a name="next-steps"></a>Sonraki adımlar

Farklı senaryolar için örnek .json dosyaları vardır [Azure Görüntü Oluşturucu GitHub](https://github.com/danielsollondon/azvmimagebuilder).
 
 
 
 
 
 
 
 
 
 
