---
title: Azure Resource Manager şablonları ile Desired State Configuration uzantısı
description: Azure Desired State Configuration ' nı (DSC) uzantısı için Resource Manager şablon tanımı hakkında bilgi edinin.
services: virtual-machines-windows
author: bobbytreed
manager: carmonm
tags: azure-resource-manager
keywords: DSC
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/05/2018
ms.author: robreed
ms.openlocfilehash: 1bcec37e7642ae0cb5bd68de1426c8cc62085d38
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61475533"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile Desired State Configuration uzantısı

Bu makalede Azure Resource Manager şablonu [Desired State Configuration ' nı (DSC) uzantısı işleyicisi](dsc-overview.md). Çok sayıda örnek **RegistrationURL** (dize olarak belirtilen) ve **RegistrationKey** (olarak sağlanan bir [PSCredential](/dotnet/api/system.management.automation.pscredential)) Azure Otomasyonu ile eklenecek. Bu değerleri edinme hakkında daha fazla ayrıntı için bkz. [makineleri Azure Otomasyon durum yapılandırması - güvenli kayıt tarafından Yönetim için hazırlama](/azure/automation/automation-dsc-onboarding#secure-registration).

> [!NOTE]
> Biraz farklı şema örneklerle karşılaşabilirsiniz. Şema değişikliği Ekim 2016 sürümündeki oluştu. Ayrıntılar için bkz [güncelleştirme Önceki biçimden](#update-from-a-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Bir Windows VM için şablon örneği

Aşağıdaki kod parçacığı gider **kaynak** şablon bölümü.
DSC uzantısı varsayılan uzantı özellikleri devralır.
Daha fazla bilgi için [VirtualMachineExtension sınıfı](/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet).

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-06-30",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "protectedSettings": {
      "Items": {
        "registrationKeyPrivate": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-06-30').Keys[0].value]"
      }
    },
    "settings": {
      "Properties": [
        {
          "Name": "RegistrationKey",
          "Value": {
            "UserName": "PLACEHOLDER_DONOTUSE",
            "Password": "PrivateSettingsRef:registrationKeyPrivate"
          },
          "TypeName": "System.Management.Automation.PSCredential"
        },
        {
          "Name": "RegistrationUrl",
          "Value": "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
          "TypeName": "System.String"
        },
        {
          "Name": "NodeConfigurationName",
          "Value": "[parameters('nodeConfigurationName')]",
          "TypeName": "System.String"
        }
      ]
    }
  }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Şablon örneği Windows sanal makine ölçek için ayarlar

Bir sanal makine ölçek kümesi düğümü olan bir **özellikleri** olan bölüm bir **VirtualMachineProfile, extensionprofile öğesine** özniteliği.
Altında **uzantıları**, DSC uzantısı ayrıntılarını ekleyin.

DSC uzantısı varsayılan uzantı özellikleri devralır.
Daha fazla bilgi için [VirtualMachineScaleSetExtension sınıfı](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [
      {
        "name": "Microsoft.Powershell.DSC",
        "properties": {
          "publisher": "Microsoft.Powershell",
          "type": "DSC",
          "typeHandlerVersion": "2.77",
          "autoUpgradeMinorVersion": true,
          "protectedSettings": {
            "Items": {
              "registrationKeyPrivate": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-06-30').Keys[0].value]"
            }
          },
          "settings": {
            "Properties": [
              {
                "Name": "RegistrationKey",
                "Value": {
                  "UserName": "PLACEHOLDER_DONOTUSE",
                  "Password": "PrivateSettingsRef:registrationKeyPrivate"
                },
                "TypeName": "System.Management.Automation.PSCredential"
              },
              {
                "Name": "RegistrationUrl",
                "Value": "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
                "TypeName": "System.String"
              },
              {
                "Name": "NodeConfigurationName",
                "Value": "[parameters('nodeConfigurationName')]",
                "TypeName": "System.String"
              }
            ]
          }
        }
      }
    ]
  }
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarları bilgileri

Aşağıdaki şemada kullanın **ayarları** Resource Manager şablonu Azure DSC uzantı bölümü.

Varsayılan yapılandırma betiğini için kullanılabilir olan bağımsız değişkenler listesi için bkz. [varsayılan yapılandırma betiğini](#default-configuration-script).

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
| settings.wmfVersion |string |Sanal makinenizde yüklü Windows Management Framework (WMF) sürümünü belirtir. Bu özelliği ayarlamak **son** WMF'nin en son sürümünü yükler. Şu anda bu özellik için yalnızca olası değerler şunlardır: **4.0**, **5.0**, **5.1**, ve **son**. Bu olası değerler şunlardır: güncelleştirmeleri tabidir. Varsayılan değer **son**. |
| Settings.Configuration.URL |string |DSC yapılandırması .zip dosyanızın indirileceği URL konumu belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, ayarlama **protectedSettings.configurationUrlSasToken** değeriniz SAS belirtecinizle değere. Bu özellik gereklidir **settings.configuration.script** veya **settings.configuration.function** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı konumu Configuration Manager'ı (LCM) meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenleri iletilmelidir. |
| Settings.Configuration.Script |string |DSC yapılandırma tanımı içeren komut dosyasının dosya adını belirtir. Bu betik tarafından belirtilen URL'den indirilen .zip dosyasının kök klasöründe olmalıdır **settings.configuration.url** özelliği. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.script** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenler iletilmelidir. |
| Settings.Configuration.Function |string |DSC yapılandırma adını belirtir. Betikte adlı yapılandırmanın eklenmelidir, **settings.configuration.script** tanımlar. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.function** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenler iletilmelidir. |
| settings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |string |DSC yapılandırma için giriş olarak kullanmak için yapılandırma verileri (.psd1) dosyasını indirileceği URL'sini belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, ayarlama **protectedSettings.configurationDataUrlSasToken** değeriniz SAS belirtecinizle değere. |
| settings.privacy.dataCollection |string |Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik için yalnızca olası değerler şunlardır: **etkinleştirme**, **devre dışı**, **''** , veya **$null**. Bu özellik boş ya da boş bırakarak telemetri sağlar. Varsayılan değer **''** . Daha fazla bilgi için [Azure DSC uzantı veri koleksiyonu](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF indirileceği alternatif konumlar tanımlar. Daha fazla bilgi için [Azure DSC uzantı 2.8 ve yüklemeleri uzantısı bağımlılıkları kendi konumuyla eşleşen nasıl](https://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |string |URL erişmek için SAS belirteci belirtir, **settings.configuration.url** tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |string |URL erişmek için SAS belirteci belirtir, **settings.configurationData.url** tanımlar. Bu özellik şifrelenir. |

## <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Aşağıdaki değerleri hakkında daha fazla bilgi için bkz: [yerel Configuration Manager temel ayarları](/powershell/dsc/metaconfig#basic-settings).
DSC uzantısı varsayılan yapılandırma betiği, aşağıdaki tabloda listelenen LCM özellikleri yapılandırmak için kullanabilirsiniz.

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| protectedSettings.configurationArguments.RegistrationKey |PSCredential |Gerekli özellik. Bir düğüm için Azure Otomasyon hizmeti ile bir PowerShell kimlik bilgisi nesnesi bir parola olarak kaydetmek için kullanılan anahtarını belirtir. Bu değeri kullanarak otomatik olarak bulunabileceğini **listkeys'i** Otomasyon hesabına karşı yöntemi.  Bkz: [örnek](#example-using-referenced-azure-automation-registration-values). |
| settings.configurationArguments.RegistrationUrl |string |Gerekli özellik. Düğümü kaydetmek için girişimde bulunduğu Otomasyon uç noktası URL'sini belirtir. Bu değeri kullanarak otomatik olarak bulunabileceğini **başvuru** Otomasyon hesabına karşı yöntemi. |
| settings.configurationArguments.NodeConfigurationName |string |Gerekli özellik. Düğüm yapılandırması düğüme atamak için Otomasyon hesabı belirtir. |
| settings.configurationArguments.ConfigurationMode |string |LCM için modu belirtir. Geçerli seçenekler şunlardır **ApplyOnly**, **ApplyandMonitor**, ve **ApplyandAutoCorrect**.  Varsayılan değer **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Güncelleştirmeler için Otomasyon hesabı ile denetlemek LCM'ne sıklıkta denediğini belirler.  Varsayılan değer **30**.  Minimum değer **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Ne sıklıkta LCM geçerli yapılandırmasını doğrular belirtir. Varsayılan değer **15**. Minimum değer **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | boole | DSC işlemi isterse bir düğümü otomatik olarak yeniden başlatılabilir olup olmadığını belirtir. Varsayılan değer **false**. |
| settings.configurationArguments.ActionAfterReboot | string | Bir yeniden başlatmadan sonra bir yapılandırma uygulanırken olacağını belirtir. Geçerli seçenekler **ContinueConfiguration** ve **StopConfiguration**. Varsayılan değer **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | boole | LCM düğümünde mevcut modüllerini yazıp yazmayacağını belirtir. Varsayılan değer **false**. |

## <a name="settings-vs-protectedsettings"></a>ayarları protectedSettings karşılaştırması

Tüm ayarlar, sanal makine ayarlarını metin dosyasına kaydedilir.
Altında listelenen özellikleri **ayarları** ortak özellikleri.
Genel Özellikler ayarlarını metin dosyasına şifreli değildir.
Altında listelenen özellikleri **protectedSettings** bir sertifika ile şifrelenir ve sanal makine ayarları dosyasına düz metin olarak gösterilmez.

Yapılandırma, kimlik bilgileri gerekiyorsa, kimlik bilgileri içerebilir **protectedSettings**:

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

Aşağıdaki örnek, LCM için meta veri ayarları sağlayın ve Automation DSC hizmetiyle kaydetmek için DSC uzantısı için varsayılan davranış gösterir.
Yapılandırma bağımsız değişkenleri gereklidir.
Yapılandırma değişkenleri LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini geçirilir.

```json
"settings": {
    "RegistrationUrl" : "[parameters('registrationUrl1')]",
    "NodeConfigurationName" : "nodeConfigurationNameValue1"
},
"protectedSettings": {
    "configurationArguments": {
        "RegistrationKey": {
            "userName": "NOT_USED",
            "Password": "registrationKey"
        }
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Yapılandırma betiği Azure depolama kullanan örnek

Aşağıdaki örnek dandır [DSC uzantısı işleyicisine genel bakış](dsc-overview.md).
Bu örnek, uzantıyı dağıtmak için cmdlet'leri yerine Resource Manager şablonları kullanır.
Iısınstall.ps1 yapılandırmayı kaydedin, bir .zip dosyasına girin (örnek: `iisinstall.zip`) ve erişilebilir bir URL içinde dosyası yükleyin.
Bu örnek, Azure Blob Depolama kullanır, ancak herhangi bir rastgele konumdan .zip dosyalarını indirebilirsiniz.

Resource Manager şablonunda VM doğru dosyayı indirin ve ardından uygun PowerShell işlevi çalıştırmak için aşağıdaki kodu bildirir:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/iisinstall.zip",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="example-using-referenced-azure-automation-registration-values"></a>Azure Otomasyonu kayıt değerlerini başvurulan örnek kullanma

Aşağıdaki örnekte **RegistrationUrl** ve **RegistrationKey** Azure Otomasyon hesabı özellikleri başvuran ve kullanarak **listkeys'i** yöntemi Birincil anahtarı (0) alın.  Bu örnekte, parametreler **automationAccountName** ve **NodeConfigName** şablona sağlandı.

```json
"settings": {
    "RegistrationUrl" : "[reference(concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))).registrationUrl]",
    "NodeConfigurationName" : "[parameters('NodeConfigName')]"
},
"protectedSettings": {
    "configurationArguments": {
        "RegistrationKey": {
            "userName": "NOT_USED",
            "Password": "[listKeys(resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName')), '2018-01-15').Keys[0].value]"
        }
    }
}
```

## <a name="update-from-a-previous-format"></a>Önceki bir biçimden güncelleştir

Herhangi bir ayarı uzantısı'nın önceki bir biçimde (ve ortak özelliklerine sahip **ModulesUrl**, **ModuleSource**, **ModuleVersion**,  **ConfigurationFunction**, **SasToken**, veya **özellikleri**) uzantısının geçerli biçime otomatik olarak uyum sağlar.
Yalnızca bunlar önce yaptığınız gibi bunlar çalıştırın.

Aşağıdaki şema gibi görünüyordu hangi önceki ayarları şeması gösterir:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
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

Önceki biçimden geçerli biçime nasıl uyum sağlayan şu şekildedir:

| Geçerli özellik adı | Önceki şema eşdeğer |
| --- | --- |
| settings.wmfVersion |Ayarlar. WMFVersion |
| Settings.Configuration.URL |Ayarlar. ModulesUrl |
| Settings.Configuration.Script |Ayarlarının ilk bölümü. ConfigurationFunction (önce \\ \\) |
| Settings.Configuration.Function |Ayarları ikinci bölümü. ConfigurationFunction (sonra \\ \\) |
| Settings.Configuration.Module.Name | Ayarlar. ModuleSource |
| Settings.Configuration.Module.Version | Ayarlar. Moduleversıon |
| settings.configurationArguments |Ayarlar. Özellikleri |
| settings.configurationData.url |protectedSettings.DataBlobUri (olmadan bir SAS belirteci) |
| settings.privacy.dataCollection |settings.Privacy.dataCollection |
| settings.advancedOptions.downloadMappings |Ayarlar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS belirteci protectedSettings.DataBlobUri gelen |

## <a name="troubleshooting"></a>Sorun giderme

Karşılaşabileceğiniz hatalar ve bunları nasıl düzeltebilirsiniz, bazıları aşağıda verilmiştir.

### <a name="invalid-values"></a>Geçersiz değerler

"Privacy.dataCollection olan '{0}'.
Yalnızca olası değerler şunlardır: '' 'Enable' ve 'Disable' ".
"WmfVersion olan '{0}'.
Yalnızca olası değerler şunlardır:... ' Son ' ".

**Sorun**: Belirtilen değer izin verilmez.

**Çözüm**: Geçersiz bir değer geçerli bir değerle değiştirin.
Daha fazla bilgi için bkz: tablodaki [ayrıntıları](#details).

### <a name="invalid-url"></a>Geçersiz URL

"ConfigurationData.url olan '{0}'. Bu geçerli bir URL değil"" DataBlobUri olan '{0}'. Bu geçerli bir URL değil"" Configuration.url olan '{0}'. Bu geçerli bir URL değil"

**Sorun**: Sağlanan URL geçerli değil.

**Çözüm**: Sağlanan tüm URL'leri denetleyin.
Tüm URL'leri uzantısı uzak makinede erişebilirsiniz geçerli konumlara çözmek emin olun.

### <a name="invalid-registrationkey-type"></a>RegistrationKey türü geçersiz

"Parametresi için geçersiz tür RegistrationKey PSCredential türü."

**Sorun**: *RegistrationKey* protectedSettings.configurationArguments değerinde bir PSCredential dışında bir türde sağlanan olamaz.

**Çözüm**: ProtectedSettings.configurationArguments giriş RegistrationKey için aşağıdaki biçimi kullanarak bir PSCredential türü değiştirin:

```json
"configurationArguments": {
    "RegistrationKey": {
        "userName": "NOT_USED",
        "Password": "RegistrationKey"
    }
}
```

### <a name="invalid-configurationargument-type"></a>ConfigurationArgument türü geçersiz

"Geçersiz configurationArguments türü {0}"

**Sorun**: *ConfigurationArguments* özelliği çözümleyemiyor bir **karma tablo** nesne.

**Çözüm**: Olun, *ConfigurationArguments* özelliği bir **karma tablo**.
Yukarıdaki örneklerde sağlanan biçim izleyin. Tırnak işareti, virgül ve küme ayraçları izleyin.

### <a name="duplicate-configurationarguments"></a>Yinelenen ConfigurationArguments

"Yinelenen bağımsız bulunan{0}' hem genel hem de korumalı configurationArguments içinde"

**Sorun**: *ConfigurationArguments* genel ayarları ve *ConfigurationArguments* korumalı ayarlarında özellikler ile aynı ada sahip.

**Çözüm**: Yinelenen özelliklerinden birini kaldırın.

### <a name="missing-properties"></a>Eksik özellikleri

"ayarlar. Configuration.Function settings.configuration.url veya settings.configuration.module belirtilmesini gerektirir"

"ayarlar. Bu settings.configuration.script belirtilen Configuration.URL gerektirir"

"ayarlar. Bu settings.configuration.url belirtilen Configuration.Script gerektirir"

"ayarlar. Bu settings.configuration.function belirtilen Configuration.URL gerektirir"

"Bu settings.configuration.url belirtilen protectedSettings.ConfigurationUrlSasToken gerektirir"

"Bu settings.configurationData.url belirtilen protectedSettings.ConfigurationDataUrlSasToken gerektirir"

**Sorun**: Tanımlanan bir özellik eksik başka bir özellik olması gerekiyor.

**Çözümleri**:

- Eksik özellik sağlar.
- Eksik özellik olması gerekiyor özelliğini kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [sanal makine ölçek kümeleri ile Azure DSC uzantı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Hakkında daha fazla ayrıntı bulmak [DSC'ın güvenli kimlik bilgileri yönetimi](dsc-credentials.md).
- Alma bir [giriş Azure DSC uzantısı işleyicisine](dsc-overview.md).
- PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](/powershell/dsc/overview).
