---
title: include dosyası
description: include dosyası
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 04/13/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: 0db5f571324694f0518ffc4e92af985e5125d755
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma

CDN profili, CDN uç noktaları için bir kapsayıcı olup bir fiyatlandırma katmanını belirtir.

1. Azure portalında sol üst köşedeki **Kaynak oluştur** seçeneğini belirleyin.
    
    **Yeni** bölmesi görüntülenir.
   
2. **Web ve Mobil** seçeneğini belirleyin ve **CDN**’yi seçin.
   
    ![CDN kaynağını seçin](./media/cdn-create-profile/cdn-new-resource.png)

    **CDN profili** bölmesi görüntülenir.

    Görüntünün altındaki tabloda belirtilen ayarları kullanın.
   
    ![Yeni CDN profili](./media/cdn-create-profile/cdn-new-profile.png)

    | Ayar  | Değer |
    | -------- | ----- |
    | **Ad** | Profil adınız olarak *my-cdn-profile-123* girin. Bu ad genel olarak benzersiz olmalıdır; halihazırda kullanılıyorsa farklı bir ad girebilirsiniz. |
    | **Abonelik** | Açılan listeden bir Azure aboneliği seçin.|
    | **Kaynak grubu** | **Yeni oluştur**’u seçin ve kaynak grubu adınız olarak *my-resource-group-123* girin. Bu ad genel olarak benzersiz olmalıdır; halihazırda kullanılıyorsa farklı bir ad girebilirsiniz. | 
    | **Kaynak grubu konumu** | Açılan listeden **Orta ABD**’yi seçin. |
    | **Fiyatlandırma katmanı** | Açılan listeden **Standart Verizon**’u seçin. |
    | **Şimdi yeni bir CDN uç noktası oluşturun** | Seçilmemiş şekilde bırakın. |  
   
3. **Panoya sabitle**’yi seçerek profili oluşturulduktan sonra panonuza kaydedin.
    
4. Profili oluşturmak için **Oluştur**’u seçin. 

