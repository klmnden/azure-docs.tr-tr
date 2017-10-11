---
title: "İstenen durum yapılandırması Resource Manager şablonu | Microsoft Docs"
description: "İstenen durum yapılandırması örnekleri içeren Azure ve sorun giderme için Resource Manager şablonu tanımı"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: 4292f9d8cd181073fdf0adff99fcb9624e0e9f55
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS ve Azure Resource Manager şablonları ile istenen durum yapılandırması
Bu makalede Resource Manager şablonu [istenen durum yapılandırması uzantısı işleyici](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="template-example-for-a-windows-vm"></a>Bir Windows VM için şablon örneği
Aşağıdaki kod parçacığında, şablonu kaynak bölüme gider.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a>Windows VMSS şablonu Örneğin
Bir VMSS düğümün "VirtualMachineProfile", "extensionProfile" özniteliği "Özellikler" bölümle vardır. DSC "Uzantılar altında" eklenir. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarları bilgileri
Bir Azure Resource Manager şablonu Azure DSC uzantı ayarları bölümü aşağıdaki şema içindir.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Ayrıntılar
| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| settings.wmfVersion |Dize |VM üzerinde yüklenmesi gereken Windows Management Framework'ün sürümünü belirtir. Bu özellik için 'Son' yükler WMF en güncel sürümünü ayarlama. Bu özellik için yalnızca geçerli olası değerler **'4.0', '5.0', ' 5.0PP' ve 'Son'**. Olası değerler tabi güncelleştirmelerdir. 'Son' varsayılan değerdir. |
| Settings.Configuration.URL |Dize |DSC yapılandırması zip dosyasını karşıdan yüklemek URL konumu belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir. Settings.Configuration.Script ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir. |
| Settings.Configuration.Script |Dize |DSC yapılandırması tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası configuration.url özelliği tarafından belirtilen URL'den indirilen ZIP dosyasının kök dizininde olması gerekir. Settings.Configuration.URL ve/veya settings.configuration.script tanımlanmışsa, bu özellik gereklidir. |
| Settings.Configuration.Function |Dize |DSC yapılandırması adını belirtir. Adlı yapılandırmasını configuration.script tarafından tanımlanan komut dosyasında yer almalıdır. Settings.Configuration.URL ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir. |
| settings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |Dize |DSC yapılandırmanız için giriş olarak kullanmak için yapılandırma verileri (.psd1) dosyanızı indirileceği URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationDataUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir. |
| settings.privacy.dataEnabled |Dize |Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik yalnızca olası değerler **'Etkinleştir', 'Devre dışı', '', veya $null**. Bu özellik boş ya da boş bırakarak telemetri sağlar. Varsayılan değer ''. [Daha fazla bilgi](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF indirileceği alternatif konumlar tanımlar. [Daha fazla bilgi](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |Dize |Configuration.URL tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |Dize |ConfigurationData.url tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |

## <a name="settings-vs-protectedsettings"></a>Ayarları vs. ProtectedSettings
Tüm ayarlar, VM'de ayarlarını metin dosyasına kaydedilir.
Ayarları metin dosyasında şifrelenmediğinden Özellikleri 'ayarlarında' ortak özelliklerdir.
Özellikleri 'protectedSettings' altında bir sertifika ile şifrelenir ve bu dosyayı VM düz metinde gösterilmez.

Yapılandırma kimlik bilgileri gerektiriyorsa, protectedSettings içinde eklenebilir:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Örnek
Aşağıdaki örnekte "Başlarken" bölümünden türetilen [DSC uzantısı işleyici genel bakış sayfasında](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Bu örnek uzantısını dağıtmak için cmdlet'leri yerine Resource Manager şablonları kullanır. "IisInstall.ps1" yapılandırmasını kaydetmek, yerleştirileceği bir. ZIP dosyası ve erişilebilir bir URL dosyayı karşıya yükleme. Bu örnek, Azure blob depolama kullanır, ancak karşıdan yüklemek mümkündür. Herhangi bir rastgele konumdan ZIP dosyaları.

Azure Resource Manager şablonunda VM doğru dosyasını indirin ve uygun PowerShell işlevi çalıştırmak için aşağıdaki kodu bildirir:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Önceki biçimden güncelleştiriliyor
Önceki biçiminde (ortak özellikler ModulesUrl, ConfigurationFunction, SasToken veya özellikler içeren) otomatik olarak herhangi bir ayarı geçerli biçimine uyum ve yalnızca bunlar önce yaptığınız gibi çalıştırın.

Aşağıdaki şema gibi Aranan hangi önceki ayarları şeması şöyledir:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Önceki biçimi geçerli biçimine nasıl uyum aşağıda verilmiştir:

| Özellik adı | Önceki şema eşdeğerine |
| --- | --- |
| settings.wmfVersion |Ayarlar. WMFVersion |
| Settings.Configuration.URL |Ayarlar. ModulesUrl |
| Settings.Configuration.Script |Ayarları ilk kısmı. ConfigurationFunction (önce '\\\\') |
| Settings.Configuration.Function |Ayarları ikinci bölümü. ConfigurationFunction (sonra '\\\\') |
| settings.configurationArguments |Ayarlar. Özellikleri |
| settings.configurationData.url |protectedSettings.DataBlobUri (olmadan, SAS belirteci) |
| settings.privacy.dataEnabled |Ayarlar. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |Ayarlar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |Ayarlar. SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS belirteci protectedSettings.DataBlobUri gelen |

## <a name="troubleshooting---error-code-1100"></a>Sorun giderme - 1100 hata kodu
Hata kodu 1100 DSC uzantısı için kullanıcı girişi ile ilgili bir sorun olduğunu gösterir.
Bu hataların metin değişkendir ve değişebilir.
İçine çalışabilir hataları ve bunları düzeltmek nasıl bazıları aşağıda verilmiştir.

### <a name="invalid-values"></a>Geçersiz değerler
"Privacy.dataCollection '{0}' dir. Yalnızca olası değerler şunlardır: '' 'Enable' ve 'Disable' ""WmfVersion olan '{0}'. Yalnızca olası değerler şunlardır:... ve 'Son' "

Sorun: Sağlanan değerine izin verilmez.

Çözüm: geçersiz değer geçerli bir değerle değiştirin. Ayrıntılar bölümündeki tabloya bakın.

### <a name="invalid-url"></a>Geçersiz URL
"ConfigurationData.url '{0}' dir. Bu geçerli bir URL değil""DataBlobUri olan '{0}'. Bu geçerli bir URL değil""Configuration.url olan '{0}'. Bu geçerli bir URL değil"

Sorun: Bir URL geçerli değil sağlanan.

Çözüm: sağlanan tüm URL'leri denetleyin. Tüm URL'leri geçerli konumlara uzantısı uzak makinede erişebilmesini çözmek emin olun.

### <a name="invalid-configurationargument-type"></a>Geçersiz ConfigurationArgument türü
"Geçersiz configurationArguments türü {0}"

Sorun: Bir Hashtable nesnesine ConfigurationArguments özelliği çözümlenemiyor. 

Çözüm: bir karma tablosu ConfigurationArguments özelliğinizi olun. Önceki örnekte sağlanan biçimi izler. Tırnak işareti, virgül ve küme ayraçları dikkat edin.

### <a name="duplicate-configurationarguments"></a>Yinelenen ConfigurationArguments
"Bağımsız değişken yinelenen '{0}' hem genel hem de korumalı configurationArguments bulunamadı"

Sorun: Genel Ayarları'nda ConfigurationArguments ve korumalı ayarlarında ConfigurationArguments aynı ada sahip özellikleri içerir.

Çözüm: Yinelenen özellikler birini kaldırın.

### <a name="missing-properties"></a>Özellikler eksik
"Configuration.function configuration.url veya configuration.module belirtilmesini gerektirir"

"Bu configuration.script belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen Configuration.script gerektirir"

"Bu configuration.function belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen ConfigurationUrlSasToken gerektirir"

"Bu configurationData.url belirtilen ConfigurationDataUrlSasToken gerektirir"

Sorun: Tanımlanmış bir özelliği eksik olan başka bir özellik olması gerekiyor.

Çözüm: 

* Eksik özellik sağlar.
* Eksik özellik olması gerekiyor özelliğini kaldırın.

## <a name="next-steps"></a>Sonraki Adımlar
DSC hakkında bilgi edinin ve sanal makine ölçek kümelerinin de [sanal makine ölçek kümeleri kullanma Azure DSC uzantısı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Daha fazla ayrıntı bulabilirsiniz [DSC'SİNİN güvenli kimlik bilgileri yönetimi](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Azure DSC uzantısı işleyici hakkında daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

PowerShell DSC hakkında daha fazla bilgi için [PowerShell Belge Merkezi ziyaret](https://msdn.microsoft.com/powershell/dsc/overview). 

