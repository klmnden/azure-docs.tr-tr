---
title: Azure CDN profili ve uç noktası oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, yeni bir CDN profili ve uç noktası oluşturularak Azure CDN’nin nasıl etkinleştirileceği gösterilmektedir.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/13/2018
ms.author: mazha
ms.custom: mvc
ms.openlocfilehash: 6237b47be878217115849b87ebcd3d980665643a
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint"></a>Hızlı Başlangıç: Azure CDN profili ve uç noktası oluşturma
Bu hızlı başlangıçta, yeni bir CDN profili ve CDN uç noktası oluşturarak Azure Content Delivery Network’ü (CDN) etkinleştireceksiniz. Bir profil ve uç nokta oluşturduktan sonra müşterilerinize içerik sunmaya başlayabilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıç amaçları doğrultusunda, kaynak konak adı için kullandığınız *mystorageacct123* adlı bir depolama hesabı oluşturmuş olmanız gerekir. Daha fazla bilgi için bkz. [Azure CDN ile bir Azure depolama hesabını tümleştirme](cdn-create-a-storage-account-with-cdn.md)

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma
Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Yeni bir CDN uç noktası oluşturma

CDN profili oluşturduktan sonra bir uç nokta oluşturmak için bunu kullanabilirsiniz.

1. Azure portalında, panonuzda oluşturduğunuz CDN profilini seçin. Bulamazsanız, **Tüm hizmetler**’i ve sonra **CDN profilleri**’ni seçin. **CDN profilleri** sayfasında, kullanmak istediğiniz profili seçin. 
   
    CDN profili sayfası görüntülenir.

2. **Uç nokta**’yı seçin.
   
    ![CDN profili](./media/cdn-create-new-endpoint/cdn-select-endpoint.png)
   
    **Uç nokta ekleyin** sayfası görüntülenir.

    Görüntünün altındaki tabloda belirtilen ayarları kullanın.
   
    ![Uç nokta ekleyin bölmesi](./media/cdn-create-new-endpoint/cdn-add-endpoint.png)

    | Ayar | Değer |
    | ------- | ----- |
    | **Ad** | Uç nokta konak adınız için *my-endpoint-123* değerini girin. Bu ad genel olarak benzersiz olmalıdır; halihazırda kullanılıyorsa farklı bir ad girebilirsiniz. Bu ad, _&lt;uç nokta adı&gt;_.azureedge.net etki alanındaki önbelleğe alınmış kaynaklarınıza erişmek için kullanılır.|
    | **Kaynak türü** | **Depolama**’yı seçin. | 
    | **Kaynak konak adı** | Konak adınız için *mystorageacct123.blob.core.windows.net* değerini girin. Bu ad genel olarak benzersiz olmalıdır; halihazırda kullanılıyorsa farklı bir ad girebilirsiniz |
    | **Kaynak yolu** | Boş bırakın. |
    | **Kaynak barındırma üst bilgisi** | Varsayılan olarak oluşturulan değeri değiştirmeyin. |  
    | **Protokol** | Varsayılan **HTTP** ve **HTTPS** seçeneklerini seçili şekilde bırakın. |
    | **Kaynak bağlantı noktası** | Varsayılan bağlantı noktası değerlerini değiştirmeyin. | 
    | **Şunun için iyileştirildi:** | Varsayılan **Genel web teslimatı** seçimini değiştirmeyin. |
    
3. Yeni uç nokta oluşturmak için **Ekle**’yi seçin.
   
   Uç nokta oluşturulduktan sonra, profile yönelik uç noktalar listesinde görünür.
    
   ![CDN uç noktası](./media/cdn-create-new-endpoint/cdn-endpoint-success.png)
    
   Kaydın yayılması zaman alacağından, uç nokta hemen kullanılabilir olmaz. 


## <a name="clean-up-resources"></a>Kaynakları temizleme
Önceki adımlarda, bir kaynak grubunda CDN profili ve bir uç nokta oluşturdunuz. [Sonraki adımlara](#next-steps) gidip uç noktanıza nasıl özel etki alanı ekleneceğini öğrenmek istiyorsanız bu kaynakları kaydedin. Ancak ileride bu kaynakları kullanmayı düşünmüyorsanız, kaynak grubunu silerek bunları silebilir, böylece ek ücretleri önleyebilirsiniz:

1. Azure portalındaki sol menüden **Kaynak grupları**’nı seçin ve sonra **my-resource-group-123** seçeneğini belirleyin.

2. **Kaynak grubu** sayfasında **Kaynak grubunu sil**’i seçin, metin kutusuna *my-resource-group-123* değerini girin ve **Sil**’i seçin.

    Bu işlem, bu hızlı başlangıçta oluşturduğunuz kaynak grubunu, profili ve uç noktayı siler.

## <a name="next-steps"></a>Sonraki adımlar
CDN uç noktanıza özel etki alanı ekleme hakkında bilgi edinmek için aşağıdaki öğreticiye bakın:

> [!div class="nextstepaction"]
> [Özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)


