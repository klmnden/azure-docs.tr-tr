---
title: "Azure Konum Tabanlı Hizmetler ile Etkileşimli Harita Araması | Microsoft Docs"
description: "Azure Hızlı Başlangıç - Azure Konum Tabanlı Hizmetler (önizleme) kullanarak bir demo etkileşimli harita araması başlatma"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: quickstart
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 0fcd25183617de879ada6d1f7d2a8fcf9551d6de
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="launch-a-demo-interactive-map-search-using-azure-location-based-services-preview"></a>Azure Konum Tabanlı Hizmetler (önizleme) kullanarak bir demo etkileşimli harita araması başlatma

Bu makalede Azure Haritalar kullanan etkileşimli bir arama ile Azure Konum Tabanlı Hizmetler (önizleme) veya kısaca LBS’nin özellikleri gösterilmektedir. Ayrıca kedi LBS hesabınızı oluşturma ve demo web uygulamasında kullanmak üzere hesap anahtarınızı alma adımları gösterilmektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-location-based-services-account-and-get-account-key"></a>Bir Konum Tabanlı Hizmetler hesabı oluşturup hesap anahtarını alma

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusunda, **konum tabanlı hizmetler** yazın.
3. *Sonuçlar* sayfasından, **Konum Tabanlı Hizmetler (önizleme)** seçeneğine tıklayın. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın. 
4. **Konum Tabanlı Hizmet Hesabı Oluştur** sayfasında, yeni hesabınız için bir *Ad* girin, kullanılacak *Abonelik*’i seçin ve yeni veya mevcut bir *Kaynak grubu*’nun adını girin. Kaynak grubunuzun konumunu seçin, *Önizleme Koşulları*’nı kabul edin ve **Oluştur**’a tıklayın.

    ![Portalda Konum Tabanlı Hizmetler Hesabı oluşturma](./media/quick-demo-map-app/create-lbs-account.png)

5. Hesabınız başarıyla oluşturulduğunda, hesabı açıp **AYARLAR**’a gidin. Hesabınızın birincil ve ikincil abonelik anahtarlarını almak için **Anahtarlar**’a tıklayın. **Birincil Anahtar** değerini sonraki bölümde kullanmak üzere yerel panonuza kopyalayın. 

## <a name="download-the-demo-application-for-azure-maps"></a>Azure Haritalar için demo uygulamasını indirme

1. [interactiveSearch.html](https://github.com/Azure-Samples/location-based-services-samples/blob/master/src/interactiveSearch.html) dosyasının içeriklerini indirin veya kopyalayın.
2. Bu dosyanın içeriklerini **AzureMapDemo.html** olarak yerel olarak kaydedin ve bir metin düzenleyicide açın.
3. **<insert-key>** dizesini arayın ve önceki bölümde aldığınız **Birincil Anahtar** değeriyle değiştirin. 


## <a name="launch-the-demo-application-for-azure-maps"></a>Azure Haritalar için demo uygulamasını başlatma

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Gösterilen Los Angeles haritasına bakın. Şehir, *AzureMapDemo.html* içinde **center** adlı JavaScript değişkenine verilen `[longitude, latitude]` çiftinin değeri tarafından belirlenir. Bu koordinatları istediğiniz herhangi bir şehrin koordinatlarıyla değiştirebilirsiniz. Örneğin, New York City’nin koordinatları *[-74.0060, 40.7128]*’dir.
3. Demo web uygulamasının sol üst köşesindeki arama kutusunda, aramak istediğiniz herhangi bir konum türü veya adresini girin. 
4. Farenizi arama kutusunun altında görünen adrs/konum listesinin üzerine getirdiğinizde haritalarda ilgili raptiye açılarak bu konum hakkında bilgileri görüntüler. Örneğin, bu uygulamanın örnek bir başlatması ve *restoranlar* için arama yapılması aşağıdaki şekilde sonuçlanır. Gerçek işletmelerin gizliliğini korumak için kurgusal ad ve adreslerin kullanıldığına dikkat edin. 

    ![Etkileşimli Arama web uygulaması](./media/quick-demo-map-app/lbs-interactive-search.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilerde hesabınız için Azure Konum Tabanlı Hizmetler’i kullanma ve yapılandırma ayrıntılı olarak açıklanmaktadır. Öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. **AzureMapDemo.html** web uygulamasını çalıştıran tarayıcıyı kapatın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından LBS hesabınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç’ta Azure LBS hesabınızı oluşturdunuz ve hesabınızı kullanarak bir demo uygulaması başlattınız. Azure Konum Tabanlı Hizmetler API’lerini kullanarak kendi uygulamanızı oluşturmayı öğrenmek için, sıradaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Azure Map ve Search öğreticisi](./tutorial-search-location.md)
