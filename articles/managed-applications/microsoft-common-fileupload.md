---
title: Azure dosya yükleme kullanıcı Arabirimi öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Common.FileUpload UI öğesi açıklar.
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
ms.date: 09/05/2018
ms.author: tomfitz
ms.openlocfilehash: 92a5f7c058904015cb22a239b7e7c4938ae1fdae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61044665"
---
# <a name="microsoftcommonfileupload-ui-element"></a>Microsoft.Common.FileUpload kullanıcı Arabirimi öğesi
Karşıya yüklenecek bir veya daha fazla dosyaları belirtmek bir kullanıcı olanak sağlayan bir denetimdir.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `constraints.accept` Tarayıcının dosya iletişim kutusunda gösterilen dosya türlerini belirtir. Bkz: [HTML5 belirtimi](https://www.w3.org/TR/html5/forms.html#attr-input-accept) için izin verilen değerler. Varsayılan değer **null**.
- Varsa `options.multiple` ayarlanır **true**, kullanıcının birden fazla dosya seçin tarayıcının dosya iletişim kutusunda izni. Varsayılan değer **false**.
- Bu öğenin değerine göre iki modda yükleme dosyalarını destekler. `options.uploadMode`. Varsa **dosya** belirtilirse, çıkış dosyasının BLOB içeriğini sahiptir. Varsa **url** belirtilmişse dosyayı geçici bir konuma yüklenir ve çıktıyı blobun URL'sini içerir. 24 saat sonra geçici blobları temizlenecek. Varsayılan değer **dosya**.
- Karşıya yüklenen bir dosya korunur. Çıkış URL'sini içeren bir [SAS belirteci](../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) dağıtım sırasında dosyasına erişim için.
- Değerini `options.openMode` nasıl dosya okunurken belirler. Dosya, düz metin olması bekleniyorsa, belirtin **metin**; başka belirtin **ikili**. Varsayılan değer **metin**.
- Varsa `options.uploadMode` ayarlanır **dosya** ve `options.openMode` ayarlanır **ikili**, base64 ile kodlanmış çıktı.
- `options.encoding` Dosya okunurken kullanılacak kodlamayı belirtir. Varsayılan değer **UTF-8**ve kullanılan yalnızca `options.openMode` ayarlanır **metin**.

## <a name="sample-output"></a>Örnek çıktı
Options.Multiple false ise ve options.uploadMode dosyasıdır, çıkış dosyasının içeriğini bir JSON dizesi sahiptir:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

Options.Multiple doğruysa and'options.uploadMode dosyasıdır ve çıktıda dosyaların içeriğini bir JSON dizisi olarak bulunur:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

Options.Multiple false ise ve options.uploadMode URL'dir. çıkış bir JSON dizesi bir URL vardır:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

Options.Multiple true ise ve options.uploadMode url çıktıda URL'lerin bir listesini bir JSON dizisi olarak bulunur:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

Bir CreateUiDefinition test ederken URL'leri tarayıcı konsolunu Microsoft.Common.FileUpload öğe tarafından oluşturulan bazı tarayıcılar (örneğin, Google Chrome) olacak şekilde kısaltın. Tam URL'leri kopyalamak için tek bağlantılar sağ gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
