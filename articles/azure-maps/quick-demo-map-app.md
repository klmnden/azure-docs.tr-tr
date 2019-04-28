---
title: Azure Haritalar ile etkileşimli harita araması | Microsoft Docs
description: Azure hızlı başlangıç - Azure haritalar'ı kullanarak bir demo etkileşimli harita araması oluşturma
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 6afe76aca388f1f6bd479f53eb4e18cc62c10584
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62116576"
---
# <a name="create-an-interactive-search-map-by-using-azure-maps"></a>Azure haritalar'ı kullanarak bir etkileşimli arama eşlemesi oluşturma

Bu makalede, Azure Haritalar’ın kullanıcılara etkileşimli arama deneyimi sunan bir harita oluşturmaya yönelik özellikler gösterilmektedir. Bu, şu temel adımlarda size yol gösterir:
* Kendi Azure haritalar hesabı oluşturun.
* Demo web uygulamasında kullanmak üzere hesap anahtarınızı alın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-an-account-and-get-your-key"></a>Hesap oluşturma ve anahtarınızı alma

1. Sol alt köşesindeki [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur**.
2. İçinde **markette Ara** kutusuna **haritalar**.
3. **Sonuçlar** içinden **Haritalar**’ı seçin. Seçin **Oluştur** haritanın altında görüntülenen düğme.
4. Üzerinde **Azure haritalar hesabı oluştur** sayfasında, aşağıdaki değerleri girin:
   - Yeni hesabınıza verilen **Ad**.
   - Bu hesap için kullanmak istediğiniz **Abonelik**.
   - Bu hesap için **Kaynak grubu**. Tercih edebileceğiniz **Yeni Oluştur** veya **var olanı kullan** kaynak grubu.
   - Seçin **fiyatlandırma katmanı** tercih ettiğiniz.
   - Okuma **lisans** ve **gizlilik bildirimi**. Koşulları kabul etmek için onay kutusunu işaretleyin.
   - Son olarak, seçin **Oluştur** düğmesi.

     ![Portalda bir Azure haritalar hesabı oluşturma](./media/quick-demo-map-app/create-account.png)

5. Hesabınız başarıyla oluşturulduktan sonra açın ve hesap menüsünden ayarları bölümünü bulun. Seçin **anahtarları** Azure haritalar hesabınız için birincil ve ikincil anahtarları görüntülemek için. **Birincil Anahtar** değerini sonraki bölümde kullanmak üzere yerel panonuza kopyalayın.

## <a name="download-the-application"></a>Uygulamayı indirme

1. [interactiveSearch.html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html) dosyasının içeriklerini indirin veya kopyalayın.
2. Bu dosyanın içeriği yerel kaydetmek **AzureMapDemo.html**. Bir metin düzenleyicisinde açın.
3. Arama dizesi için `<Your Azure Maps Key>`. Yerine konacak **birincil anahtar** değerini ise önceki bölümden.

## <a name="open-the-application"></a>Uygulamayı açma

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Şehir, Los Angeles, gösterilen haritasına bakın. Yakınlaştırma düzeyine bağlı olarak haritanın daha fazla veya daha az bilgiyle nasıl işlendiğini görmek için yakınlaştırma ve uzaklaştırma yapın. 
3. Haritanın varsayılan merkezini değiştirin. **AzureMapDemo.html** dosyasında, **center** adlı değişkeni arayın. Bu değişkenin boylam ve enlem çiftini, yeni **[-74.0060, 40.7128]** değerleriyle değiştirin. Dosyayı kaydedin ve tarayıcınızı yenileyin.
4. Etkileşimli arama deneyimini deneyin. Arama kutusuna demo web uygulamasının sol üst köşedeki arama **restoranlar**.
5. Adres ve arama kutusunun altında görünen konumları listesi üzerinde fareyi hareket ettirin. Bu konum hakkında bilgileri nasıl karşılık gelen PIN harita üzerinde yükseklikteki dikkat edin. Özel işletmelerin gizliliğini korumak için kurgusal ad ve adresler gösterilir.

    ![Etkileşimli arama web uygulaması](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticileri kullanın ve Azure haritalar hesabınız ile yapılandırma hakkında ayrıntılı olarak açıklanmaktadır. Bu hızlı başlangıçta oluşturulan kaynakları temizleyin, öğreticilere geçin planladığınız yok. Devam etmeyi planlamıyorsanız, kaynakları temizlemek için aşağıdaki adımları uygulayın:

1. Çalıştıran Tarayıcıyı kapatın **AzureMapDemo.html** web uygulaması.
2. Azure portalında sol menüden seçim yapın **tüm kaynakları**. Ardından, Azure haritalar hesabı seçin. Üst kısmındaki **tüm kaynakları** dikey penceresinde **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure haritalar hesabınızda oluşturduğunuz ve bir tanıtım uygulaması oluşturdunuz. Azure haritalar hakkında bilgi edinmek için aşağıdaki öğreticilere göz atın:

> [!div class="nextstepaction"]
> [Azure haritalar'ı kullanarak ilgi noktalarını arama](tutorial-search-location.md)

Daha fazla kod örnekleri ve etkileşimli bir kodlama deneyimi için bu kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Azure haritalar arama hizmetini kullanarak bir adres Bul](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Azure haritalar harita denetimini kullanma](how-to-use-map-control.md)
