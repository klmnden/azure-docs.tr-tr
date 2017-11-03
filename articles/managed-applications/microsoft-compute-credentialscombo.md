---
title: "Azure yönetilen uygulama CredentialsCombo UI öğesi | Microsoft Docs"
description: "Azure yönetilen uygulamalar için Microsoft.Compute.CredentialsCombo kullanıcı Arabirimi öğesi açıklar"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: d8faa36aca762bc8d787d5750fcf7efdbaf986ea
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI öğesi
Windows ve Linux parolalar ve SSH ortak anahtarları için yerleşik doğrulama denetimleriyle grubudur. Bu öğe kullandığınız zaman [yönetilen bir Azure uygulama oluşturmaya](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Şema
Varsa `osPlatform` olan **Windows**, aşağıdaki şema kullanılır:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Varsa `osPlatform` olan **Linux**, aşağıdaki şema kullanılır:
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `osPlatform`belirtilmeli ve birini kullanabilir **Windows** veya **Linux**.
- Varsa `constraints.required` ayarlanır **doğru**, parola veya SSH ortak anahtarı metin kutuları başarıyla doğrulamak için değerleri içermesi gerekir. Varsayılan değer **doğru**.
- Varsa `options.hideConfirmation` ayarlanır **doğru**, sonra da kullanıcının parolasını onayladığınız için ikinci metin kutusu gizli. Varsayılan değer **false**.
- Varsa `options.hidePassword` ayarlanır **doğru**, parola kimlik doğrulaması kullanma seçeneğini gizli sonra. Kullanılabilmesi için yalnızca `osPlatform` olan **Linux**. Varsayılan değer **false**.
- İzin verilen parolalar ek kısıtlamalar kullanarak uygulanabilir `customPasswordRegex` özelliği. Dizede `customValidationMessage` parola özel doğrulama başarısız olduğunda görüntülenir. Her iki özellik için varsayılan değer **null**.

## <a name="sample-output"></a>Örnek çıktı
Varsa `osPlatform` olan **Windows**, veya kullanıcı parola yerine bir SSH ortak anahtarı sağlanan sonra aşağıdaki çıkış bekleniyor:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Kullanıcı SSH ortak anahtarı sağladıysanız, aşağıdaki çıkış bekleniyor:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](overview.md).
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
