---
title: Azure CredentialsCombo UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Compute.CredentialsCombo UI öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: 0412d55fe60524cde404e6a640723d3259e020e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60251420"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo kullanıcı Arabirimi öğesi
Windows ve Linux parola ve SSH ortak anahtarları için yerleşik doğrulama denetimleriyle grubudur.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi

Windows için bkz:

![Microsoft.Compute.CredentialsCombo Windows](./media/managed-application-elements/microsoft.compute.credentialscombo-windows.png)

Seçili parola ile Linux için kullanıcıları bakın:

![Microsoft.Compute.CredentialsCombo Linux parolası](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-password.png)

Seçili SSH ortak anahtarı ile Linux için kullanıcıları bakın:

![Microsoft.Compute.CredentialsCombo Linux anahtarı](./media/managed-application-elements/microsoft.compute.credentialscombo-linux-key.png)

## <a name="schema"></a>Şema
Windows için aşağıdaki şemayı kullanın:

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
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must contain at least 12 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

İçin **Linux**, aşağıdaki şemayı kullanın:

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
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must contain at least 12 characters, with at least 1 letter and 1 number."
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
- `osPlatform` belirtilmiş olmalı ve aşağıdakilerden biri olması **Windows** veya **Linux**.
- Varsa `constraints.required` ayarlanır **true**, sonra da parola veya SSH ortak anahtarı metin kutuları başarıyla doğrulamak için değerlere sahip olmalıdır. Varsayılan değer **true**.
- Varsa `options.hideConfirmation` ayarlanır **true**, sonra kullanıcı parolası onaylama için ikinci metin kutusunda gizlenir. Varsayılan değer **false**.
- Varsa `options.hidePassword` ayarlanır **true**, sonra da parola kimlik doğrulaması kullanma seçeneğini gizli. Kullanılmadan yalnızca `osPlatform` olduğu **Linux**. Varsayılan değer **false**.
- Kullanarak izin verilen parola ek kısıtlamalar uygulanabilir `customPasswordRegex` özelliği. Dizede `customValidationMessage` parola özel doğrulama başarısız olduğunda görüntülenir. Her iki özellik için varsayılan değerdir **null**.

## <a name="sample-output"></a>Örnek çıktı
Varsa `osPlatform` olan **Windows**, veya `osPlatform` olan **Linux** ve kullanıcı sağlanan parola yerine bir SSH ortak anahtarı, denetimi aşağıdaki çıkışı verir:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

Varsa `osPlatform` olduğu **Linux** ve kullanıcı sağlanan SSH ortak anahtarı, denetimi aşağıdaki çıktı döndürür:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
