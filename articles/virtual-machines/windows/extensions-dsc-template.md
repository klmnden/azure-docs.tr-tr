---
title: İstenen durum yapılandırması uzantısı Azure Resource Manager şablonları ile | Microsoft Docs
description: İstenen durum Yapılandırması'nı (DSC) uzantısı'nda Azure Resource Manager şablonu tanımı hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: mgreenegit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 03/22/2018
ms.author: migreene
ms.openlocfilehash: 095b0cba8f7d22920203e5e3c4bcd83666188023
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>İstenen durum yapılandırması uzantısı Azure Resource Manager şablonları ile

Bu makalede Azure Resource Manager şablonu [istenen durum Yapılandırması'nı (DSC) uzantısı işleyici](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Biraz farklı şema örnekler karşılaşabilirsiniz. Ekim 2016 sürümde şema değişikliği oluştu. Ayrıntılar için bkz [güncelleştirme önceki biçiminden](#update-from-the-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Bir Windows VM için şablon örneği

Aşağıdaki kod parçacığında gider **kaynak** şablon bölümünü.
DSC uzantı varsayılan uzantısı özellikleri devralır.
Daha fazla bilgi için bkz: [VirtualMachineExtension sınıfı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.75",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
            "Items": {
                        "registrationKeyPrivate": "registrationKey"
            }
            },
            "publicSettings": {
                "configurationArguments": [
                    {
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        },
                    },
                    {
                        "RegistrationUrl" : "registrationUrl",
                    },
                    {
                        "NodeConfigurationName" : "nodeConfigurationName"
                    }
                ]
            }
        },
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Şablon örneği Windows sanal makine ölçek için ayarlar

Bir sanal makine ölçek kümesi düğüm bir **özellikleri** sahip bölüm bir **VirtualMachineProfile, extensionProfile** özniteliği.
Altında **uzantıları**, DSC uzantısı ayrıntıları ekleyin.

DSC uzantı varsayılan uzantısı özellikleri devralır.
Daha fazla bilgi için bkz: [VirtualMachineScaleSetExtension sınıfı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
            "apiVersion": "2017-12-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.Powershell",
                "type": "DSC",
                "typeHandlerVersion": "2.75",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "protectedSettings": {
                    "Items": {
                                "registrationKeyPrivate": "registrationKey"
                    }
                    },
                    "publicSettings": {
                        "configurationArguments": [
                            {
                                "Name": "RegistrationKey",
                                "Value": {
                                    "UserName": "PLACEHOLDER_DONOTUSE",
                                    "Password": "PrivateSettingsRef:registrationKeyPrivate"
                                },
                            },
                            {
                                "RegistrationUrl" : "registrationUrl",
                            },
                            {
                                "NodeConfigurationName" : "nodeConfigurationName"
                            }
                        ]
                    }
                },
            }
        }
    ]
}
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarları bilgileri

Aşağıdaki şemada kullanmak **ayarları** Resource Manager şablonu Azure DSC uzantı bölümü.

Varsayılan yapılandırma komut dosyası için kullanılabilir olan bağımsız değişkenler listesi için bkz: [varsayılan yapılandırma komut dosyası](#default-configuration-script).

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
| settings.wmfVersion |string |VM üzerinde yüklenmesi gereken Windows Management Framework (WMF) sürümünü belirtir. Bu özelliği ayarlamak **son** WMF en son sürümünü yükler. Şu anda bu özellik için yalnızca olası değerler şunlardır: **4.0**, **5.0**, **5.0PP**, ve **son**. Olası değerler tabi güncelleştirmelerdir. Varsayılan değer **son**. |
| settings.configuration.url |string |DSC yapılandırması .zip dosyanızın indirileceği URL konumunu belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, ayarlamak **protectedSettings.configurationUrlSasToken** özelliğinin değeri, SAS belirteci. Bu özellik gereklidir **settings.configuration.script** veya **settings.configuration.function** tanımlanır. Bu özellikler için herhangi bir değer verilirse, uzantı konumu Configuration Manager (LCM'yi) meta veri ayarlamak için varsayılan yapılandırma komut dosyası çağırır ve bağımsız değişkenler sağlanması. |
| settings.configuration.script |string |DSC yapılandırması tanımını içeren komut dosyasının dosya adını belirtir. Bu komut dosyası tarafından belirtilen URL'den indirilen .zip dosyasının kök klasöründe olmalıdır **configuration.url** özelliği. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.script** tanımlanır. Bu özellikler için herhangi bir değer verilirse, uzantı LCM'yi meta verileri ayarlamak için varsayılan yapılandırma komut dosyası çağırır ve bağımsız değişkenler sağlanması. |
| settings.configuration.function |string |DSC yapılandırması adını belirtir. Adlı yapılandırma komut dosyasında yer almalıdır, **configuration.script** tanımlar. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.function** tanımlanır. Bu özellikler için herhangi bir değer verilirse, uzantı LCM'yi meta verileri ayarlamak için varsayılan yapılandırma komut dosyası çağırır ve bağımsız değişkenler sağlanması. |
| settings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |string |DSC yapılandırmanız için giriş olarak kullanmak için yapılandırma verileri (.psd1) dosyanızı indirileceği URL'yi belirtir. Sağlanan URL erişim için bir SAS belirteci gerektiriyorsa, ayarlamak **protectedSettings.configurationDataUrlSasToken** özelliğinin değeri, SAS belirteci. |
| settings.privacy.dataEnabled |string |Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik yalnızca olası değerler **etkinleştirmek**, **devre dışı**, **''**, veya **$null**. Bu özellik boş ya da boş bırakarak telemetri sağlar. Varsayılan değer **''**. Daha fazla bilgi için bkz: [Azure DSC uzantısı veri toplama](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF indirileceği alternatif konumlar tanımlar. Daha fazla bilgi için bkz: [Azure DSC uzantısı 2.8 ve kendi konuma uzantısı bağımlılıkları yüklemeleri eşlemeyi](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |string |SAS belirteci URL erişmek için kullanılacak belirtir, **configuration.url** tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |string |SAS belirteci URL erişmek için kullanılacak belirtir, **configurationData.url** tanımlar. Bu özellik şifrelenir. |

## <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Aşağıdaki değerleri hakkında daha fazla bilgi için bkz: [yerel Configuration Manager temel ayarları](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig#basic-settings).
DSC uzantı varsayılan yapılandırma komut dosyası aşağıdaki tabloda listelenen LCM'yi özelliklerini yapılandırmak için kullanabilirsiniz.

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Gerekli özellik. Azure Otomasyonu hizmeti ile bir PowerShell kimlik bilgisi nesnesi parola olarak kaydetmek için bir düğüm için kullanılan anahtarı belirtir. Bu değer kullanarak otomatik olarak bulunabileceğini **listkeys** Otomasyon hesabı karşı yöntemi. Değer bir korumalı ayarı olarak güvenli hale getirilmelidir. |
| settings.configurationArguments.RegistrationUrl |string |Gerekli özellik. Düğüm kaydetmek için girişimde bulunduğu Otomasyon uç noktası URL'sini belirtir. Bu değer kullanarak otomatik olarak bulunabileceğini **başvuru** Otomasyon hesabı karşı yöntemi. |
| settings.configurationArguments.NodeConfigurationName |string |Gerekli özellik. Düğüm yapılandırması düğüme atamak için Otomasyon hesabı belirtir. |
| settings.configurationArguments.ConfigurationMode |string |Modu için LCM'yi belirtir. Geçerli seçenekler şunlardır **ApplyOnly**, **ApplyandMonitor**, ve **ApplyandAutoCorrect**.  Varsayılan değer **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Otomasyon hesabı için güncelleştirmeleri denetlemek LCM'yi ne sıklıkta denediğini belirler.  Varsayılan değer **30**.  En düşük değer **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Ne sıklıkta LCM'yi geçerli yapılandırmasını doğrular belirtir. Varsayılan değer **15**. En düşük değer **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | boole | DSC işlemi isterse bir düğümü otomatik olarak başlatılması olup olmadığını belirtir. Varsayılan değer **false**. |
| settings.configurationArguments.ActionAfterReboot | string | Bir yeniden başlatmadan sonra bir yapılandırma uygularken olacağını belirtir. Geçerli seçenekler şunlardır: **ContinueConfiguration** ve **StopConfiguration**. Varsayılan değer **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | boole | LCM'yi düğümü üzerindeki var olan modülleri yazıp yazmayacağını belirtir. Varsayılan değer **false**. |

## <a name="settings-vs-protectedsettings"></a>Ayarları vs. ProtectedSettings

Tüm ayarlar, VM'de ayarlarını metin dosyasına kaydedilir.
Altında listelenen özellikler **ayarları** ortak özelliklerdir.
Ortak Özellikler ayarlarını metin dosyasına şifreli değildir.
Altında listelenen özellikler **protectedSettings** bir sertifika ile şifrelenir ve düz metin ayarları dosyası VM'de olarak gösterilmez.

Yapılandırma kimlik bilgileri gerektiriyorsa, kimlik bilgileri içerebilir **protectedSettings**:

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

## <a name="example-configuration-script"></a>Örnek yapılandırma betiği

Aşağıdaki örnek LCM'yi için meta veri ayarları sağlayın ve Automation DSC hizmete kaydolmak DSC uzantısı için varsayılan davranış gösterir.
Yapılandırma bağımsız değişkenler zorunludur.
Yapılandırma değişkenleri LCM'yi meta verileri ayarlamak için varsayılan yapılandırma betiği geçirilir.

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

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Yapılandırma komut dosyası Azure depolama alanında kullanan örnek

Aşağıdaki örnek kaynaklı [DSC uzantısı işleyici genel bakış](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Bu örnek uzantısını dağıtmak için cmdlet'leri yerine Resource Manager şablonları kullanır.
IisInstall.ps1 yapılandırmasını kaydetmek, bir .zip dosyasına yerleştirin ve erişilebilir bir URL dosyayı karşıya yükleyin.
Bu örnek, Azure Blob Depolama kullanır, ancak herhangi bir rastgele konumdan .zip dosyalarını indirebilirsiniz.

Resource Manager şablonu VM doğru dosyasını indirin ve ardından uygun PowerShell işlevi çalıştırmak için aşağıdaki kodu bildirir:

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

## <a name="update-from-a-previous-format"></a>Önceki bir biçimden güncelleştir

Uzantısı'nın önceki bir biçimde herhangi bir ayarı (ve ortak özellikleri olan **ModulesUrl**, **ConfigurationFunction**, **SasToken**, veya  **Özellikler**) uzantısı geçerli biçimine otomatik olarak uyum.
Bunlar yalnızca bunlar önce yaptığınız gibi çalıştırın.

Aşağıdaki şema gibi Aranan hangi önceki ayarları şeması gösterir:

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
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Ayarları ilk kısmı. ConfigurationFunction (önce \\ \\) |
| settings.configuration.function |Ayarları ikinci bölümü. ConfigurationFunction (sonra \\ \\) |
| settings.configurationArguments |Ayarlar. Özellikleri |
| settings.configurationData.url |protectedSettings.DataBlobUri (olmadan, SAS belirteci) |
| settings.privacy.dataEnabled |Ayarlar. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |Ayarlar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |Ayarlar. SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS belirteci protectedSettings.DataBlobUri gelen |

## <a name="troubleshooting---error-code-1100"></a>Sorun giderme - 1100 hata kodu

Hata kodu 1100 DSC uzantısı için kullanıcı girişi ile ilgili bir sorun gösterir.
Bu hataların metin değişir ve değişebilir.
İçine çalışabilir hataları bazıları aşağıda verilmiştir ve bunları nasıl düzeltebilirsiniz.

### <a name="invalid-values"></a>Geçersiz değerler

"Privacy.dataCollection '{0}' dir.
Yalnızca olası değerler şunlardır: '' 'Enable' ve 'Disable' ".
"WmfVersion is '{0}'.
Yalnızca olası değerler şunlardır:... ve 'Son' ".

**Sorun**: sağlanan değer verilmez.

**Çözüm**: geçersiz değer geçerli bir değerle değiştirin.
Daha fazla bilgi için bölümündeki tabloya bakın [ayrıntıları](#details).

### <a name="invalid-url"></a>Geçersiz URL

"ConfigurationData.url '{0}' dir. Bu geçerli bir URL değil""DataBlobUri olan '{0}'. Bu geçerli bir URL değil""Configuration.url olan '{0}'. Bu geçerli bir URL değil"

**Sorun**: A sağlanan URL geçerli değil.

**Çözüm**: tüm sağlanan URL'leri denetleyin.
Tüm URL'leri uzantısı uzak makinede erişebilmesini geçerli konumlara çözmek emin olun.

### <a name="invalid-configurationargument-type"></a>Geçersiz ConfigurationArgument türü

"Geçersiz configurationArguments türü {0}"

**Sorun**: *ConfigurationArguments* olamaz özelliğini çözümlemek için bir **Hashtable** nesnesi.

**Çözüm**: olun, *ConfigurationArguments* özelliği bir **Hashtable**.
Önceki örnekte sağlanan biçimi izler. Tırnak işareti, virgül ve küme ayraçları izleyin.

### <a name="duplicate-configurationarguments"></a>Yinelenen ConfigurationArguments

"Bağımsız değişken yinelenen '{0}' hem genel hem de korumalı configurationArguments bulunamadı"

**Sorun**: *ConfigurationArguments* genel ayarları ve *ConfigurationArguments* korumalı ayarlarında aynı ada sahip özelliklere sahiptir.

**Çözüm**: Yinelenen özellikler birini kaldırın.

### <a name="missing-properties"></a>Eksik özellikleri
"Configuration.function configuration.url veya configuration.module belirtilmesini gerektirir"

"Bu configuration.script belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen Configuration.script gerektirir"

"Bu configuration.function belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen ConfigurationUrlSasToken gerektirir"

"Bu configurationData.url belirtilen ConfigurationDataUrlSasToken gerektirir"

**Sorun**: tanımlanmış bir özellik eksik başka bir özellik olması gerekiyor.

**Çözümleri**:

- Eksik özellik sağlar.
- Eksik özellik olması gerekiyor özelliğini kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [ayarlar Azure DSC uzantısı ile sanal makine ölçek kullanarak](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
* İlgili diğer ayrıntıları bulmak [DSC'SİNİN güvenli kimlik bilgileri yönetimi](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Alma bir [Azure DSC uzantısı işleyici giriş](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](https://msdn.microsoft.com/powershell/dsc/overview).
