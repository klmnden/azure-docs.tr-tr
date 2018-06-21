---
title: include dosyası
description: include dosyası
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 05/24/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: e431e7c45dc2cb41db5b9fa762abc908cc76b375
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665299"
---
## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma

CDN profili, CDN uç noktaları için bir kapsayıcı olup bir fiyatlandırma katmanını belirtir.

1. Azure portalında sol üst köşedeki **Kaynak oluştur** seçeneğini belirleyin. 
    
    **Yeni** bölmesi görüntülenir.
   
2. **Web ve Mobil** seçeneğini belirleyin ve **CDN**’yi seçin.
   
    ![CDN kaynağını seçin](./media/cdn-create-profile/cdn-new-resource.png)

    **CDN profili** bölmesi görüntülenir.

3. CDN profil ayarları için aşağıdaki tabloda belirtilen değerleri kullanın:
   
    | Ayar  | Değer |
    | -------- | ----- |
    | **Ad** | Profil adınız olarak *my-cdn-profile-123* girin. Bu ad küresel olarak benzersiz olmalıdır; zaten kullanılıyorsa farklı bir ad girebilirsiniz. |
    | **Abonelik** | Açılan listeden bir Azure aboneliği seçin. |
    | **Kaynak grubu** | **Yeni oluştur**’u seçin ve kaynak grubu adınız olarak *my-resource-group-123* girin. Bu adın küresel olarak benzersiz olması gerekir. Ad zaten kullanılıyorsa, farklı bir ad girebilir ya da **Var olanı kullan**'ı seçip açılan listeden **my-resource-group-123**'ü seçebilirsiniz. | 
    | **Kaynak grubu konumu** | Açılan listeden **Orta ABD**’yi seçin. |
    | **Fiyatlandırma katmanı** | Açılan listeden **Standart Verizon**’u seçin. |
    | **Şimdi yeni bir CDN uç noktası oluşturun** | Seçilmemiş şekilde bırakın. |  
   
    ![Yeni CDN profili](./media/cdn-create-profile/cdn-new-profile.png)

4. **Panoya sabitle**’yi seçerek profili oluşturulduktan sonra panonuza kaydedin.
    
5. Profili oluşturmak için **Oluştur**’u seçin. 

    **Microsoft’tan Azure CDN Standart** profilleri için tamamlanma işlemi genellikle iki saat sürer. 

