---
title: 'Bing özel arama: özel arama web sayfası oluşturma | Microsoft Docs'
description: Özel arama örneği yapılandırmak ve bir web sayfasına tümleştirmek açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: c1431ec852cab943e00d3933ef4f0500a4fdb151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352895"
---
# <a name="build-a-custom-search-web-page"></a>Derleme özel arama web sayfası
Bing özel arama uyarlanmış arama deneyimleri hakkında önemli konular için oluşturmanıza olanak sağlar. Örneğin, bir arama deneyimi sağlayan bir savaş Sanatları Web sitesi sahibiyseniz, etki alanları, siteler ve Bing arar Web sayfaları belirtebilirsiniz. Kullanıcılarınızın, disk belleği ilgisiz içerebilecek genel arama sonuçları aracılığıyla yerine ilgilendiğiniz içerik uyarlanmış arama sonuçları bakın. 

Bu öğretici, bir özel arama örneği yapılandırın ve yeni bir web sayfasına tümleştirmek gösterilmiştir.

Kapsanan görevleri şunlardır:

> [!div class="checklist"]
> - Özel arama örneği oluşturma
> - Etkin girişleri ekleme
> - Engellenen girişleri ekleme
> - Sabitlenmiş girişleri ekleme
> - Bir web sayfasına özel arama tümleştirme

## <a name="prerequisites"></a>Önkoşullar
- Birlikte öğreticiyi izlemek için Bing özel arama API için bir abonelik anahtarı gerekir.  Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz.

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma
Bing özel arama örneği oluşturmak için:

1.  Bir Internet tarayıcısı açın.
2.  Özel arama gidin [portal](https://customsearch.ai).
3.  Bir Microsoft hesabı (MSA) kullanarak portalında oturum açın. Bir MSA yoksa tıklatın **bir Microsoft hesabı oluşturmayı**. Portalı kullanarak ilk kez varsa, veri erişim izinlerini sorar. **Evet**'e tıklayın.
4.  Oturum açtıktan sonra tıklatın **yeni özel arama**. İçinde **yeni bir özel arama örneği oluşturmayı** penceresinde, anlamlı ve arama döndürür içerik türünü açıklayan bir ad girin. Herhangi bir zamanda adını değiştirebilirsiniz.
 
    ![Ekran görüntüsü yeni bir özel arama örneği kutusu oluşturma](../media/newCustomSrch.png)

5.  Tamam'ı tıklatın, bir URL ve temel sayfasının alt sayfalar dahil edilip edilmeyeceğini belirtin:

    ![URL tanımı sayfasının ekran görüntüsü](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Etkin girişleri ekleme
Belirli sitelerin veya URL'leri sonuçlarını dahil etmek için bunları Ekle **etkin** sekmesi.

1.  İçinde **tanımı Düzenleyicisi**, tıklatın **etkin** sekmesinde ve aramanızda dahil etmek istediğiniz bir veya daha fazla site URL'sini girin.

    ![Tanımı Düzenleyicisi etkin sekmesinin Ekran görüntüsü](../media/customSrchEditor.png)

2.  Örneğinizi sonuçlar döndürür onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. Bing dizine genel sitelere için sonuçları döndürür.

## <a name="add-blocked-entries"></a>Engellenen girişleri ekleme
Sonuçları belirli sitelerin veya URL'leri dışlamak için bunları Ekle **bloke** sekmesi.

1. İçinde **tanımı Düzenleyicisi**, tıklatın **bloke** sekmesinde ve aramanıza hariç tutmak istediğiniz bir veya daha fazla site URL'sini girin.

    ![Ekran görüntüsü tanımı Düzenleyicisi'nin sekmesini engellendi](../media/blockedCustomSrch.png)


2. Örneğinizi engellenen siteler sonuçlar döndürmez onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. 

## <a name="add-pinned-entries"></a>Sabitlenmiş girişleri ekleme
Web sayfası ve sorgu Terime belirli bir Web sayfası arama sonuçlarının en üstüne Sabitle ekleyin **Pinned** sekmesi.  **Pinned** sekmesi belirli bir sorgu için üst sonucunda görüntülenen Web belirtin Web sayfası ve sorgu Terime çiftlerinin listesini içerir. Kullanıcının sorgu Terime sabitlenmiş sorgu terimine dön sabitlenmelidir Web sayfasının tam olarak eşleşmelidir.

1. İçinde **tanımı Düzenleyicisi**, tıklatın **Pinned** sekmesinde ve üst sonucu olarak döndürmelidir Web sayfası Web sayfası ve sorgu Terime girin.

    ![Ekran görüntüsü tanımı Düzenleyicisi'nin sabitlenmiş sekmesi](../media/pinnedCustomSrch.png)

2. Örneğinizi belirtilen Web sayfasının üst sonuç olarak döndüren onaylamak için sabitlenmiş sorgu terimine sağda önizleme bölmesinde girin.

## <a name="configure-hosted-ui"></a>Barındırılan UI yapılandırın
Özel arama özel arama örneğinizi JSON yanıtı işlemek için barındırılan bir kullanıcı Arabirimi sağlar.  UI deneyiminizi tanımlamak için:

1. Tıklatın **barındırılan UI** sekmesi.
2. Bir düzen seçin.

    ![Düzen adım barındırılan kullanıcı Arabirimi ekran görüntüsü seçin](./media/custom-search-hosted-ui-select-layout.png)

3. Renk temasını seçin.

    ![Düzen adım barındırılan kullanıcı Arabirimi ekran görüntüsü seçin](./media/custom-search-hosted-ui-select-color-theme.png)

4. Ek yapılandırma seçeneklerini belirtin.

    ![Barındırılan UI ek yapılandırmalar adım ekran görüntüsü](./media/custom-search-hosted-ui-additional-configurations.png)

5. Yapıştırma, **abonelik anahtarı**. Bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Barındırılan UI ek yapılandırmalar adım ekran görüntüsü](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Barındırılan kullanıcı Arabirimi kullanma
Barındırılan UI kullanmak için iki yolu vardır.  

- Seçenek 1: sağlanan JavaScript kod parçacığı uygulamanıza tümleştirin.
- Seçenek 2: sağlanan HTML uç noktasını kullanın.

Bu öğreticinin geri kalanında gösterilmektedir **seçenek 1: Javascript kod parçacığı**.  

## <a name="set-up-your-visual-studio-solution"></a>Visual Studio çözümünüzü ayarlayın
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. İçinde **yeni proje** penceresinde, seçin **Visual C# / Web / ASP.NET çekirdek Web uygulaması**, projenizi adlandırın ve ardından **Tamam**.
   
    ![Yeni Proje penceresinin ekran görüntüsü](./media/custom-search-new-project.png)

4. İçinde **çekirdek yeni bir ASP.NET Web uygulaması** penceresinde, seçin **Web uygulaması** tıklatıp **Tamam**.
    
    ![Yeni Proje penceresinin ekran görüntüsü](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Index.cshtml Düzenle
1. İçinde **Çözüm Gezgini**, genişletin **sayfaları** çift tıklayın ve **Index.cshtml** dosyası açılamıyor.

    ![Genişletilmiş sayfalarını ve seçili Index.cshtml çözüm Gezgini'nin ekran görüntüsü](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. Index.cshtml içinde başlangıç 7 satırından ve aşağıda her şeyi silin.
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Bir satır kesme öğe ve bir kapsayıcı olarak hareket edecek bir div ekleyin.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Gelen **barındırılan UI** sekmesi, bölümüne kaydırın **kullanıcı arabirimini kullanma**. JavaScript kod parçacığı kopyalayın.

    ![Kaydet düğmesi barındırılan kullanıcı Arabirimi ekran görüntüsü](./media/custom-search-hosted-ui-consuming-ui.png)

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

    ![Çözüm Gezgini tarayıcıda wwwroot bağlam menüsünden görünümü seçme ekran görüntüsü](./media/custom-search-webapp-view-in-browser.png)

Yeni özel arama web sayfanız şuna benzer görünmelidir:

![Özel arama web sayfasının ekran görüntüsü](./media/custom-search-webapp-browse-index.png)

Aramadan şuna benzer sonuçlar getirir.

![Özel arama sonuçları ekran görüntüsü](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> - Özel arama örneği
> - Eklenen etkin girişleri
> - Engellenen girişleri
> - Sabitlenmiş girişleri
> - Bir web sayfasına tümleşik özel arama

Şimdi, Bing özel arama uç noktaları program aracılığıyla arama devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Çağrı Bing özel arama uç noktası (C#)](../call-endpoint-csharp.md)