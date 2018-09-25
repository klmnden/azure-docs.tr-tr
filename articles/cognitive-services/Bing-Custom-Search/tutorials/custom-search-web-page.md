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
ms.openlocfilehash: 8bc1520325afc256ac62cc1f1dfaf24c53da4b83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980007"
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

1. Bir internet tarayıcısı açın.  
  
2. Özel arama gidin [portalı](https://customsearch.ai).  
  
3. Bir Microsoft hesabı (MSA) kullanarak portalda oturum açın. Bir MSA yoksa tıklayın **Microsoft hesabı oluşturmanızda**. Portalı kullanarak ilk kez ise, verilerinize erişmek için gerekli izinleri sorar. **Evet**'e tıklayın.  
  
4. Oturum açtıktan sonra tıklayın **yeni özel arama**. İçinde **yeni bir özel arama örneği oluşturma** penceresinde, arama döndürür içerik türünü açıklar ve anlamlı bir ad girin. Herhangi bir zamanda adını değiştirebilirsiniz.  
  
  ![Yeni özel arama örneği kutusu oluştur ekran görüntüsü](../media/newCustomSrch.png)  
  
5. Tamam'ı tıklatın, bir URL ve alt URL'sinin eklenip eklenmeyeceğini belirtin.  
  
  ![Tanım Sayfası URL'sini ekran görüntüsü](../media/newCustomSrch1-a.png)  


## <a name="add-active-entries"></a>Etkin girişler ekleyin

Sonuçları belirli Web siteleri veya URL'leri dahil etmek için bunlara ekleyin **etkin** sekmesi.

1.  Üzerinde **yapılandırma** sayfasında **etkin** sekmesinde ve arama dahil etmek istediğiniz bir veya daha fazla Web sitesi URL'sini girin.

    ![Tanımı Düzenleyicisi etkin sekmesinin Ekran görüntüsü](../media/customSrchEditor.png)

2.  Örneğiniz sonuçlar döndürüyor onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. Bing dizine genel Web siteleri için yalnızca sonuçları döndürür.

## <a name="add-blocked-entries"></a>Engellenen girişler ekleyin

Belirli Web siteleri veya URL'leri sonuçlarını tutmak için bunlara ekleyin **bloke** sekmesi.

1. Üzerinde **yapılandırma** sayfasında **bloke** sekmesini ve aramanızdan hariç tutmak istediğiniz bir veya daha fazla Web sitesi URL'sini girin.

    ![Tanımı Düzenleyicisi engellenen sekmesinin Ekran görüntüsü](../media/blockedCustomSrch.png)


2. Örneğiniz Engellenen Web sitelerinden sonuçlar döndürmüyor onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. 

## <a name="add-pinned-entries"></a>Sabitlenmiş girişler ekleyin

Üst arama sonuçları belirli bir Web sayfasına sabitlemek için Web sayfası ve sorgu terim ekleme **Pinned** sekmesi. **Pinned** sekmesi, belirli bir sorgu için en iyi sonucu olarak görünen Web sayfasını belirtin Web sayfası ve sorgu terimi çiftlerinin listesini içerir. Web sayfası, yalnızca kullanıcının sorgu dizesi PIN'in eşleşme koşulu temel alarak PIN'in sorgu dizesi eşleşiyorsa sabitlenir. [Daha fazla bilgi edinin](../define-your-custom-view.md#pin-to-top).

1. Üzerinde **yapılandırma** sayfasında **Pinned** sekmesini ve en iyi sonucu olarak döndürülmesini istediğiniz Web sayfasının Web sayfası ve sorgu terimi girin.  
  
2. Varsayılan olarak, kullanıcının sorgu dizesi için en iyi sonucu olarak Web sayfasına geri dönmek, Bing, PIN'in sorgu dizesi tam olarak eşleşmelidir. Eşleşme koşulu değiştirmek için PIN Düzenle (Kalem simgesine tıklayın), tam olarak tıklayın **sorgu eşleşme koşulu** sütun ve uygulamanız için doğru eşleşme koşulu seçin.  
  
    ![Sekme tanımı Düzenleyicisi'nin ekran görüntüsü sabitlenmiş](../media/pinnedCustomSrch.png)
  
3. Örneğiniz belirtilen Web sayfasının üst sonuç olarak döndüren onaylamak için sağ tarafta önizleme bölmesinde sabitlediğiniz sorgu terimi girin.

## <a name="configure-hosted-ui"></a>Barındırılan kullanıcı Arabirimi yapılandırma

Özel arama, özel arama örneğinizin JSON yanıtı işlemek için barındırılan bir kullanıcı Arabirimi sağlar. UI deneyiminizi tanımlamak için:

1. Tıklayın **barındırılan UI** sekmesi.  
  
2. Bir düzen seçin.  
  
  ![Barındırılan kullanıcı arabiriminin ekran düzeni adımını seçin](./media/custom-search-hosted-ui-select-layout.png)  
  
3. Bir renk temasını seçin.  
  
  ![Barındırılan kullanıcı arabiriminin ekran renk temasını seçin](./media/custom-search-hosted-ui-select-color-theme.png)  

  Renk teması daha iyi web uygulamanızla tümleştirmek için ince ayar yapmak gerekiyorsa **Özelleştir tema**. Tüm renk yapılandırmaları tüm düzen temaları için geçerlidir. Bir rengini değiştirmek için ilgili metin kutusunda rengin RGB ONALTILIK değerini (örneğin, #366eb8) girin. Veya, renk düğmesini tıklatın ve sizin için uygun Gölge'ye tıklayın. Her zaman renkleri seçerken erişilebilirlik hakkında düşünün.
  
  ![Renk teması barındırılan kullanıcı arabiriminin ekran özelleştirme](./media/custom-search-hosted-ui-customize-color-theme.png)  

  
4. Ek yapılandırma seçeneklerini belirtin.  
  
  ![Barındırılan UI ek yapılandırmalar adımında ekran görüntüsü](./media/custom-search-hosted-ui-additional-configurations.png)  
  
  Gelişmiş yapılandırmaları için Yardım düğmesini tıklatın **Gelişmiş yapılandırmaları Show**. Bu yapılandırmalar gibi ekler *bağlantı hedefi* Web arama seçenekleri için *etkinleştirme filtreleri* görüntü ve Video seçenekleri ve *arama kutusu metni yer tutucusu* çeşitli için Seçenekler.

  ![Barındırılan UI gelişmiş yapılandırmalar adımında ekran görüntüsü](./media/custom-search-hosted-ui-advanced-configurations.png)  
  
5. Abonelik anahtarlarınızın açılan listelerinden seçin. Ya da abonelik anahtarını el ile girebilirsiniz. Anahtarlarını alma hakkında daha fazla bilgi için bkz: [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).  
  
  ![Barındırılan UI ek yapılandırmalar adımında ekran görüntüsü](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

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
  
  ```razor
  @page
  @model IndexModel
  @{
      ViewData["Title"] = "Home page";
  }    
  ```  
  
3. Bir satır kesme öğe ve bir kapsayıcısı olarak görev yapacak bir div ekleyin.  
  
  ```html
  @page
  @model IndexModel
  @{
      ViewData["Title"] = "Home page";
  }
  <br />
  <div id="customSearch"></div>
  ```  
  
4. İçinde **barındırılan UI** sayfasında bölümüne kaydırın **kullanıcı arabirimini kullanan**. Tıklayın *uç noktaları* JavaScript kod parçacığını erişmek için. Kod parçacığını tıklayarak da edinebilirsiniz **üretim** ardından **barındırılan UI** sekmesi.
  
  <!-- Get new screenshot after prod gets new bits
  ![Screenshot of the Hosted UI save button](./media/custom-search-hosted-ui-consuming-ui.png)  
  -->
  
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
          src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
      </script>
  </div>
  ```  
  
6. İçinde **Çözüm Gezgini**, sağ tıklayın **wwwroot** tıklatıp **tarayıcıda görüntüle**.  
  
  ![Çözüm Gezgini görünümü tarayıcıda wwwroot bağlam menüsünden seçme ekran görüntüsü](./media/custom-search-webapp-view-in-browser.png)  

Yeni özel arama web sayfanız şuna benzer görünmelidir:

![Özel arama web sayfasının ekran görüntüsü](./media/custom-search-webapp-browse-index.png)

Arama yapmak için sonuçlar şöyle işlenir:

![Özel arama sonuçları ekran görüntüsü](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing özel arama uç noktası çağrısı (C#)](../call-endpoint-csharp.md)