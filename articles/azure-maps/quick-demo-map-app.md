---
title: Azure Haritalar ile etkileşimli Harita Arama | Microsoft Docs
description: Azure Hızlı Başlangıç - Azure Haritalar’ı kullanarak bir demo etkileşimli harita araması başlatma
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: quickstart
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 8dedaf95289d9637f5f3d1e80a763b5fb400c617
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="launch-an-interactive-search-map-using-azure-maps"></a>Azure Haritalar’ı kullanarak bir etkileşimli arama haritası başlatma

Bu makalede, Azure Haritalar’ın kullanıcılara etkileşimli arama deneyimi sunan bir harita oluşturmaya yönelik özellikler gösterilmektedir. Kendi Haritalar hesabınızı oluşturmaya ve demo web uygulamasında kullanmak üzere hesap anahtarınızı almaya yönelik temel adımlar gösterilmektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-account-and-get-your-key"></a>Hesap oluşturma ve anahtarınızı alma

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusuna **Haritalar** yazın.
3. *Sonuçlar* içinden **Haritalar**’ı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın. 
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    - Yeni hesabınıza verilen *Ad*. 
    - Bu hesap için kullanmak istediğiniz *Abonelik*.
    - Bu hesap için *Kaynak grubu*. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    - *Kaynak grubu konumu*nu seçin.
    - *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin. 
    - Son olarak **Oluştur** düğmesine tıklayın.

    ![Portalda Haritalar hesabı oluşturma](./media/quick-demo-map-app/create-account.png)

5. Hesabınız başarıyla oluşturulduktan sonra hesabı açıp hesap menüsünün ayarlar bölümünü bulun. Azure Haritalar hesabınızın birincil ve ikincil anahtarlarını görüntülemek için **Anahtarlar**’a tıklayın. **Birincil Anahtar** değerini sonraki bölümde kullanmak üzere yerel panonuza kopyalayın. 

## <a name="download-the-application"></a>Uygulamayı indirme

1. [interactiveSearch.html](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/interactiveSearch.html) dosyasının içeriklerini indirin veya kopyalayın.
2. Bu dosyanın içeriklerini **AzureMapDemo.html** olarak yerel olarak kaydedin ve bir metin düzenleyicide açın.
3. `<insert-key>` dizesini arayın ve önceki bölümde aldığınız **Birincil Anahtar** değeriyle değiştirin. 


## <a name="launch-the-application"></a>Uygulamayı başlatma

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Gösterilen Los Angeles haritasına bakın. Yakınlaştırma düzeyine bağlı olarak haritanın daha fazla veya daha az bilgiyle nasıl işlendiğini görmek için yakınlaştırma ve uzaklaştırma yapın. 
3. Haritanın varsayılan merkezini değiştirin. **AzureMapDemo.html** dosyasında, **center** adlı değişkeni arayın. Bu değişkenin boylam ve enlem çiftini, yeni **[-74.0060, 40.7128]** değerleriyle değiştirin. Dosyayı kaydedin ve tarayıcınızı yenileyin. 
3. Etkileşimli arama deneyimini deneyin. Demo web uygulamasının sol üst köşesindeki arama kutusunda **restoranlar** ifadesini arayın. 
4. Farenizi arama kutusunun altında görünen adrs/konum listesinin üzerine getirdiğinizde haritalarda ilgili raptiye açılarak bu konum hakkında bilgileri görüntüler. Özel işletmelerin gizliliğini korumak için kurgusal ad ve adresler gösterilir. 

    ![Etkileşimli Arama web uygulaması](./media/quick-demo-map-app/interactive-search.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilerde, hesabınızla Haritalar’ın nasıl kullanılacağı ve yapılandırılacağı ayrıntılı şekilde açıklanmaktadır. Öğreticilere devam etmeyi planlıyorsanız, bu Hızlı Başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. **AzureMapDemo.html** web uygulamasını çalıştıran tarayıcıyı kapatın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Haritalar hesabınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıçta Haritalar hesabınızı oluşturdunuz ve bir demo uygulaması başlattınız. Haritalar API’lerini kullanarak kendi uygulamanızı oluşturmayı öğrenmek için, sıradaki öğretici ile devam edin.

> [!div class="nextstepaction"]
> [Haritalar ile ilgi çekici nokta arama](./tutorial-search-location.md)
