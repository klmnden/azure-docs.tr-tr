---
title: "Azure Resource Manager şablonları ile durum yapılandırma uzantısı istenen | Microsoft Docs"
description: "İstenen durum yapılandırması uzantısı Azure Resource Manager şablonu tanımı"
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: f638d1530541526316f6e409f1efd44f136992a5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile istenen durum yapılandırma uzantısı

Bu makalede Azure Resource Manager şablonu [istenen durum yapılandırması uzantısı işleyici](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

*Not: biraz farklı şema örnekler karşılaşabilirsiniz. * 
 *Ekim 2016'şema değişikliği oluştu yayın.* 
 *Ayrıntıları bu sayfanın başlıklı bölümünde belirtilmiştir*
*[önceki biçiminden güncelleştirme](##Updating-from-the-Previous-Format)*.

## <a name="template-example-for-a-windows-vm"></a>Bir Windows VM için şablon örneği

Aşağıdaki kod parçacığında, şablonu kaynak bölüme gider.
DSC uzantı açıklandığı gibi varsayılan uzantısı özellikleri devralır [VirtualMachineExtension sınıfı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.)

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
                  "typeHandlerVersion": "2.72",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                    "configurationArguments": {
                        {
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        "RegistrationUrl" : "[parameters('registrationUrl1')]",
                        "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
```

## <a name="template-example-for-windows-vmss"></a>Windows VMSS şablonu Örneğin

Bir VMSS düğümün "VirtualMachineProfile", "extensionProfile" özniteliği "Özellikler" bölümle vardır. DSC "Uzantılar altında" eklenir.

DSC uzantı açıklandığı gibi varsayılan uzantısı özellikleri devralır [VirtualMachineScaleSetExtension sınıfı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.72",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configurationArguments": {
                                {
                                    "Name": "RegistrationKey",
                                    "Value": {
                                        "UserName": "PLACEHOLDER_DONOTUSE",
                                        "Password": "PrivateSettingsRef:registrationKeyPrivate"
                                    },
                                },
                                "RegistrationUrl" : "[parameters('registrationUrl1')]",
                                "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
                ]
            }
        }
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarları bilgileri

Bir Azure Resource Manager şablonu Azure DSC uzantı ayarları bölümü aşağıdaki şema içindir.

*Varsayılan yapılandırma komut dosyası için bağımsız bir listesi için*
*adlı aşağıda bölümüne bakın*
*[varsayılan yapılandırma komut dosyası](##Default-Configuration-Script) *.

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

| Özellik Adı | Tür | Açıklama |
| --- | --- | --- |
| settings.wmfVersion |string |VM üzerinde yüklenmesi gereken Windows Management Framework'ün sürümünü belirtir. Bu özellik için 'Son' yükler WMF en güncel sürümünü ayarlama. Bu özellik için yalnızca geçerli olası değerler **'4.0', '5.0', ' 5.0PP' ve 'Son'**. Olası değerler tabi güncelleştirmelerdir. 'Son' varsayılan değerdir. |
| settings.configuration.url |string |DSC yapılandırması zip dosyasını karşıdan yüklemek URL konumu belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir. Settings.Configuration.Script ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir. Bu özellikler için herhangi bir değer verilirse, uzantı LCM'yi meta veri kümesi varsayılan yapılandırması komut dosyasını çağırır ve bağımsız değişkenler sağlanması. |
| settings.configuration.script |string |DSC yapılandırması tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası configuration.url özelliği tarafından belirtilen URL'den indirilen ZIP dosyasının kök dizininde olması gerekir. Settings.Configuration.URL ve/veya settings.configuration.script tanımlanmışsa, bu özellik gereklidir. Bu özellikler için herhangi bir değer verilirse, uzantısı LCM'yi meta veri kümesi varsayılan yapılandırması komut dosyasını çağırır ve bağımsız değişkenler uygulanmalıdır. |
| settings.configuration.function |string |DSC yapılandırması adını belirtir. Adlı yapılandırmasını configuration.script tarafından tanımlanan komut dosyasında yer almalıdır. Settings.Configuration.URL ve/veya settings.configuration.function tanımlanmışsa, bu özellik gereklidir. Bu özellikler için herhangi bir değer verilirse, uzantı LCM'yi meta veri kümesi varsayılan yapılandırması komut dosyasını çağırır ve bağımsız değişkenler sağlanması. |
| settings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |string |DSC yapılandırmanız için giriş olarak kullanmak için yapılandırma verileri (.psd1) dosyanızı indirileceği URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, protectedSettings.configurationDataUrlSasToken özelliği, SAS belirteci değerine ayarlamanız gerekir. |
| settings.privacy.dataEnabled |string |Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik yalnızca olası değerler **'Etkinleştir', 'Devre dışı', '', veya $null**. Bu özellik boş ya da boş bırakarak telemetri sağlar. Varsayılan değer ''. [Daha fazla bilgi](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF indirileceği alternatif konumlar tanımlar. [Daha fazla bilgi](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |string |Configuration.URL tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |string |ConfigurationData.url tarafından tanımlanan URL'yi erişmek için SAS belirteci belirtir. Bu özellik şifrelenir. |

## <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Bu değerler hakkında daha fazla bilgi için bkz: belge sayfasının [yerel Configuration Manager temel ayarları](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig#basic-settings).
Aşağıdaki tabloda yalnızca LCM'yi özelliklerinde DSC uzantısı varsayılan yapılandırma betiği kullanılarak yapılandırılabilir.

| Özellik Adı | Tür | Açıklama |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Gerekli özellik. Azure Otomasyonu hizmeti ile bir PowerShell kimlik bilgisi nesnesi parola olarak kaydetmek için bir düğüm için kullanılan anahtarını belirtir. Bu değer, Automation hesabı karşı listkeys yöntemini kullanarak otomatik olarak bulunabilir ve korumalı ayarı olarak korunması. |
| settings.configurationArguments.RegistrationUrl |string |Gerekli özellik. Burada düğüm kaydolmayı deneyecek Azure Otomasyon uç noktası URL'sini belirtir. Bu değer, Automation hesabı karşı başvuru yöntemini kullanarak otomatik olarak bulunabilir. |
| settings.configurationArguments.NodeConfigurationName |string |Gerekli özellik. Düğüm yapılandırması düğüme atamak için Azure Automation hesabını belirtir. |
| settings.configurationArguments.ConfigurationMode |string |Yerel Configuration Manager için modu belirtir. Geçerli seçenekler "ApplyOnly", "ApplyandMonitor" ve "ApplyandAutoCorrect" içerir.  Varsayılan değer "ApplyandMonitor" dir. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Otomasyon hesabı için güncelleştirmeleri denetlemek LCM'yi ne sıklıkta atayacağını belirtir.  Varsayılan değer 30'dur.  En düşük değer 15'tir. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | LCM'yi geçerli yapılandırmasını doğrular ne sıklıkta belirtir.  Varsayılan değer 15'tir.  En düşük değer 15'tir. |
| settings.configurationArguments.RebootNodeIfNeeded | boole | DSC işlemi isterse bir düğümü otomatik olarak yeniden başlatılması olup olmadığını belirtir.  Varsayılan değer false şeklindedir. |
| settings.configurationArguments.ActionAfterReboot | string | Bir yeniden başlatmadan sonra bir yapılandırma uygularken olacağını belirtir. Geçerli seçenekler şunlardır: "ContinueConfiguration" ve "StopConfiguration". Varsayılan değer "ContinueConfiguration" şeklindedir. |
| settings.configurationArguments.AllowModuleOverwrite | boole | LCM'yi düğümde bulunan mevcut modüllerin üzerine yazıp yazmayacağını belirtir.  Varsayılan değer false şeklindedir. |

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

Aşağıdaki örnek, yerel Configuration Manager için meta veri ayarları sağlayın ve Azure Otomasyonu DSC hizmete kaydolmak DSC uzantısı varsayılan davranıştır.
Yapılandırma değişkenleri gereklidir ve LCM'yi meta verileri ayarlamak için varsayılan yapılandırma betiği geçirilir.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl" : "[parameters('registrationUrl1')]",
        "NodeConfigurationName" : "nodeConfigurationNameValue1"
  }
},
"protectedSettings": {
    "Items": {
                "registrationKeyPrivate": "[parameters('registrationKey1']"
            }
}
```

## <a name="example-using-configuration-script-in-azure-storage"></a>Yapılandırma komut dosyası Azure depolama alanında kullanan örnek

Aşağıdaki örnekte "Başlarken" bölümünden türetilen [DSC uzantısı işleyici genel bakış sayfasında](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Bu örnek uzantısını dağıtmak için cmdlet'leri yerine Resource Manager şablonları kullanır.
"IisInstall.ps1" yapılandırmasını kaydetmek, yerleştirileceği bir. ZIP dosyası ve erişilebilir bir URL dosyayı karşıya yükleme.
Bu örnek, Azure blob depolama kullanır, ancak karşıdan yüklemek mümkündür. Herhangi bir rastgele konumdan ZIP dosyaları.

Azure Resource Manager şablonunda VM doğru dosyasını indirin ve uygun PowerShell işlevi çalıştırmak için aşağıdaki kodu bildirir:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
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

| Özellik Adı | Önceki şema eşdeğerine |
| --- | --- |
| settings.wmfVersion |Ayarlar. WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Ayarları ilk kısmı. ConfigurationFunction (önce '\\\\') |
| settings.configuration.function |Ayarları ikinci bölümü. ConfigurationFunction (sonra '\\\\') |
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

"Privacy.dataCollection '{0}' dir.
Yalnızca olası değerler şunlardır: '' 'Enable' ve 'Disable' ".
"WmfVersion is '{0}'.
Yalnızca olası değerler şunlardır:... ve 'Son' ".

Sorun: Sağlanan değerine izin verilmez.

Çözüm: geçersiz değer geçerli bir değerle değiştirin.
Ayrıntılar bölümündeki tabloya bakın.

### <a name="invalid-url"></a>Geçersiz URL

"ConfigurationData.url '{0}' dir. Bu geçerli bir URL değil""DataBlobUri olan '{0}'. Bu geçerli bir URL değil""Configuration.url olan '{0}'. Bu geçerli bir URL değil"

Sorun: Bir URL geçerli değil sağlanan.

Çözüm: sağlanan tüm URL'leri denetleyin.
Tüm URL'leri geçerli konumlara uzantısı uzak makinede erişebilmesini çözmek emin olun.

### <a name="invalid-configurationargument-type"></a>Geçersiz ConfigurationArgument türü

"Geçersiz configurationArguments türü {0}"

Sorun: Bir Hashtable nesnesine ConfigurationArguments özelliği çözümlenemiyor.

Çözüm: bir karma tablosu ConfigurationArguments özelliğinizi olun.
Önceki örnekte sağlanan biçimi izler.
Tırnak işareti, virgül ve küme ayraçları dikkat edin.

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

- Eksik özellik sağlar.
- Eksik özellik olması gerekiyor özelliğini kaldırın.

## <a name="next-steps"></a>Sonraki Adımlar

DSC hakkında bilgi edinin ve sanal makine ölçek kümelerinin de [sanal makine ölçek kümeleri kullanma Azure DSC uzantısı ile](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).

Daha fazla ayrıntı bulabilirsiniz [DSC'SİNİN güvenli kimlik bilgileri yönetimi](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure DSC uzantısı işleyici hakkında daha fazla bilgi için bkz: [Azure istenen durum yapılandırması uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

PowerShell DSC hakkında daha fazla bilgi için [PowerShell Belge Merkezi ziyaret](https://msdn.microsoft.com/powershell/dsc/overview).
