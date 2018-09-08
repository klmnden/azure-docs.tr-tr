---
title: 'Bing özel arama: bir özel arama web sayfası oluştur | Microsoft Docs'
description: Özel arama örneği yapılandırmak ve bir web sayfasına tümleştirme açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: 1f9b689ac6127bc2f7d1e810356ae9a23b8e0996
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162402"
---
# <a name="build-a-custom-search-web-page"></a>Özel Arama web sayfası derleme
Bing özel arama, önem verdiğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanıza olanak sağlar. Örneğin, bir arama deneyimi sağlayan Sanatları Web sitesi sahipseniz, etki alanları, siteler ve Bing arama, Web sayfaları belirtebilirsiniz. Kullanıcılarınızın, arama sonuçları ile ilgisiz içerik içerebilen genel arama sonuçlarını sayfalandırma yerine ilgilendiklerini içeriği uyarlanmış bakın. 

Bu öğreticide, bir özel arama örneği yapılandırın ve yeni bir web sayfasına tümleştirmek gösterilmektedir.

Kapsanan görevler şunlardır:

> [!div class="checklist"]
> - Özel arama örneği oluşturma
> - Etkin girişler ekleyin
> - Engellenen girişler ekleyin
> - Sabitlenmiş girişler ekleyin
> - Bir web sayfasına özel arama tümleştirin

## <a name="prerequisites"></a>Önkoşullar
- Eğitimle birlikte ilerlemek için Bing özel arama API'si için bir abonelik anahtarı gerekir.  Bir anahtarı almak için bkz. [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz.

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma
Bing özel arama örneği oluşturmak için:

1.  Bir internet tarayıcısı açın.
2.  Özel arama gidin [portalı](https://customsearch.ai).
3.  Bir Microsoft hesabı (MSA) kullanarak portalda oturum açın. Bir MSA yoksa tıklayın **Microsoft hesabı oluşturmanızda**. Portalı kullanarak ilk kez ise, verilerinize erişmek için gerekli izinleri sorar. **Evet**'e tıklayın.
4.  Oturum açtıktan sonra tıklayın **yeni özel arama**. İçinde **yeni bir özel arama örneği oluşturma** penceresinde, arama döndürür içerik türünü açıklar ve anlamlı bir ad girin. Herhangi bir zamanda adını değiştirebilirsiniz.
 
    ![Yeni özel arama örneği kutusu oluştur ekran görüntüsü](../media/newCustomSrch.png)

5.  Tamam'ı tıklatın, bir URL ve taban sayfasının alt eklenip eklenmeyeceğini belirtin:

    ![URL tanımı sayfasının ekran görüntüsü](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Etkin girişler ekleyin
Sonuçları belirli siteleri veya URL'leri dahil etmek için bunlara ekleyin **etkin** sekmesi.

1.  İçinde **tanımı Düzenleyicisi**, tıklayın **etkin** sekmesinde ve arama dahil etmek istediğiniz bir veya daha fazla site URL'sini girin.

    ![Tanımı Düzenleyicisi etkin sekmesinin Ekran görüntüsü](../media/customSrchEditor.png)

2.  Örneğiniz sonuçlar döndürüyor onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. Bing dizine genel siteler için sonuçları döndürür.

## <a name="add-blocked-entries"></a>Engellenen girişler ekleyin
Belirli siteleri veya URL'leri sonuçlarını tutmak için bunlara ekleyin **bloke** sekmesi.

1. İçinde **tanımı Düzenleyicisi**, tıklayın **bloke** sekmesini ve aramanızdan hariç tutmak istediğiniz bir veya daha fazla site URL'sini girin.

    ![Sekme tanımı Düzenleyicisi'nin ekran görüntüsü engellendi](../media/blockedCustomSrch.png)


2. Örneğiniz engellenen sitelerden sonuçlar döndürmüyor onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. 

## <a name="add-pinned-entries"></a>Sabitlenmiş girişler ekleyin
Üst arama sonuçları belirli bir Web sayfasına sabitlemek için Web sayfası ve sorgu terim ekleme **Pinned** sekmesi.  **Pinned** sekmesi, belirli bir sorgu için en iyi sonucu olarak görünen Web sayfasını belirtin Web sayfası ve sorgu terimi çiftlerinin listesini içerir. Kullanıcının sorgu terimine sabitlenmiş sorgu terimine dön sabitlenmelidir Web sayfasının tam olarak eşleşmelidir.

1. İçinde **tanımı Düzenleyicisi**, tıklayın **Pinned** sekmesini ve en iyi sonucu olarak döndürmelidir sayfasının Web sayfası ve sorgu terimi girin.

    ![Sekme tanımı Düzenleyicisi'nin ekran görüntüsü sabitlenmiş](../media/pinnedCustomSrch.png)

2. Örneğiniz belirtilen Web sayfasının üst sonuç olarak döndüren onaylamak için sağ tarafta önizleme bölmesinde sabitlediğiniz sorgu terimi girin.

## <a name="configure-hosted-ui"></a>Barındırılan kullanıcı Arabirimi yapılandırma
Özel arama, özel arama örneğinizin JSON yanıtı işlemek için barındırılan bir kullanıcı Arabirimi sağlar.  UI deneyiminizi tanımlamak için:

1. Tıklayın **barındırılan UI** sekmesi.
2. Bir düzen seçin.

    ![Barındırılan kullanıcı arabiriminin ekran görüntüsü düzeni adımını seçin.](./media/custom-search-hosted-ui-select-layout.png)

3. Bir renk temasını seçin.

    ![Barındırılan kullanıcı arabiriminin ekran görüntüsü düzeni adımını seçin.](./media/custom-search-hosted-ui-select-color-theme.png)

4. Ek yapılandırma seçeneklerini belirtin.

    ![Barındırılan UI ek yapılandırmalar adımında ekran görüntüsü](./media/custom-search-hosted-ui-additional-configurations.png)

5. Yapıştırma, **abonelik anahtarı**. Bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Barındırılan UI ek yapılandırmalar adımında ekran görüntüsü](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE [publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Barındırılan kullanıcı arabirimini kullanma
Barındırılan kullanıcı arabirimini kullanmak için iki yolu vardır.  

- 1. seçenek: sağlanan JavaScript kod parçacığını uygulamanızla tümleştirin.
- 2. seçenek: sağlanan HTML uç noktayı kullanın.

Bu öğreticinin geri kalanında gösterir **seçeneği 1: Javascript kod parçacığını**.  

## <a name="set-up-your-visual-studio-solution"></a>Visual Studio çözümünüzü ayarlama
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. İçinde **yeni proje** penceresinde **Visual C# / Web / ASP.NET Core Web uygulaması**, projenizi adlandırın ve ardından **Tamam**.
   
    ![Yeni Proje penceresinin ekran görüntüsü](./media/custom-search-new-project.png)

4. İçinde **yeni ASP.NET Core Web uygulaması** penceresinde **Web uygulaması** tıklatıp **Tamam**.
    
    ![Yeni Proje penceresinin ekran görüntüsü](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Index.cshtml Düzenle
1. İçinde **Çözüm Gezgini**, genişletme **sayfaları** ve çift **Index.cshtml** dosyayı açmak için.

    ![Genişletilmiş sayfalarını ve seçili Index.cshtml çözüm Gezgini'nin ekran görüntüsü](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. Index.cshtml içinde başlangıç 7 satırından ve altındaki her şeyi silin.
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Bir satır kesme öğe ve bir kapsayıcısı olarak görev yapacak bir div ekleyin.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Gelen **barındırılan UI** sekmesinde bölümüne kaydırın **kullanıcı arabirimini kullanan**. JavaScript kod parçacığını kopyalayın.

    ![Kaydet düğmesi barındırılan kullanıcı arabiriminin ekran görüntüsü](./media/custom-search-hosted-ui-consuming-ui.png)

5. Betik öğesi eklediğiniz kapsayıcıya yapıştırın.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch">
        <script type="text/javascript"
                id="bcs_js_snippet"
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">
        </script>
    </div>
    ```

6. İçinde **Çözüm Gezgini**, sağ tıklayın **wwwroot** tıklatıp **tarayıcıda görüntüle**.

    ![Çözüm Gezgini görünümü tarayıcıda wwwroot bağlam menüsünden seçme ekran görüntüsü](./media/custom-search-webapp-view-in-browser.png)

Yeni özel arama web sayfanız şuna benzer görünmelidir:

![Özel arama web sayfasının ekran görüntüsü](./media/custom-search-webapp-browse-index.png)

Arama yapmak için bu gibi sonuçları işler.

![Özel arama sonuçları ekran görüntüsü](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> - Oluşturulan bir özel arama örneği
> - Eklenen etkin girişleri
> - Engellenen girişleri
> - Sabitlenmiş girişleri
> - Bir web sayfasına tümleşik özel arama

Şimdi Bing özel arama uç noktaları program aracılığıyla arama geçebilirsiniz.

> [!div class="nextstepaction"]
> [Bing özel arama uç noktası çağrısı (C#)](../call-endpoint-csharp.md)