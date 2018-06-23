---
title: Varlık bağlama API'si ile çalışmaya başlama | Microsoft Docs
description: Metin ve Cognition Hizmetleri'nde varlık bağlama API kullanarak bir Bilgi Bankası'nda ilgili girişlere varlıklara adlı bağlantı analiz edin.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: entity-linking-intelligence
ms.topic: article
ms.date: 07/06/2016
ms.author: davl
ms.openlocfilehash: 54c4a3bbb3637c248bd7705ed291633368b542c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351772"
---
# <a name="get-started-with-entity-linking-api-in-c35"></a>C API bağlama varlık kullanmaya başlama&#35;

Microsoft'un varlık bağlama metin çözümlemek ve Bilgi Bankası'nda ilgili girişlere adlı varlıklar bağlamak için bir doğal dil işleme aracıdır. 

Bu öğretici bir NuGet paketi olarak varlık bağlama istemci kitaplığı kullanılarak bağlama varlık araştırır. 

### <a name="Prerequisites">Önkoşullar</a>

- Visual Studio 2015
- API anahtarı bir Microsoft Bilişsel hizmetler
- İstemci Kitaplığı ve örnek alın
- Microsoft varlık bağlama NuGet paketi

Varlık bağlama Intelligence hizmeti API'si istemci kitaplığı aracılığıyla karşıdan yüklenebilir [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows). Tercih ettiğiniz bir klasöre ayıklanacak indirilen ZIP dosyasının gerekir, çok sayıda kullanıcı Visual Studio 2015 klasörü seçin.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">1. adım: Varlık bağlama Intelligence hizmete abone olmak ve anahtarınızı alın</a>
Varlık yönetim bilgileri hizmeti bağlama kullanmadan önce bir API anahtarı için kaydolmanız gerekir. Bkz: [abonelikleri](https://www.microsoft.com/cognitive-services/en-us/sign-up). Bu öğreticide, hem birincil ve ikincil anahtar kullanılabilir.

### <a name="step-2-create-a-new-project-in-visual-studio"> 2. adım: Visual Studio'da yeni proje oluşturma</a>

Visual Studio'da yeni bir proje oluşturarak başlayalım. İlk olarak, Visual Studio 2015 Başlat menüsünden başlatın. Ardından, seçerek yeni bir proje oluşturun **→ şablonları → Visual C# → Windows Evrensel → boş uygulama yüklü** proje şablonu için:

 ![Açmayla Evrensel uygulama](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">3. adım: Varlık bağlama NuGet paketini projenize ekleyin.</a>

Varlık bağlama, Bilişsel hizmetler NuGet.org paket olarak yayımlanır ve kullanabilmeniz için önce yüklü olması gerekir.
Projenize eklemek için şu adrese gidin **Çözüm Gezgini** sekmesinde, projenize sağ tıklayın ve seçin **Nuget paketlerini Yönet**.

İlk olarak **NuGet Paket Yöneticisi** penceresinde, olarak select NuGet.org, **paket kaynağı** üst köşedeki. Seçin **Gözat** sol üst köşedeki ve arama kutusuna "ProjectOxford.EntityLinking" yazın. Seçin **Microsoft.ProjectOxford.EntityLinking** NuGet paketini ve tıklayın **yükleme**.

Ardından, Newtonsoft.Json ve yükleme işlemini arayın. Değişiklikleri gözden geçirmek için istenirse, tıklatın **Tamam**. EntityLinking lisans şartlarını sunulan tıklatmak **kabul ediyorum**.

Varlık bağlama şimdi uygulamanızı bir parçası olarak yüklenir. Bu, denetleyerek doğrulayabilirsiniz ** Microsoft.ProjectOxford.EntityLinking** başvuru varsa Çözüm Gezgini'nde projenizin bir parçası olarak.

 ![Proje dahil nuget kitaplığı](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">4. adım: bir giriş ekleyin ve uygulamanızın XAML metin bloğuna çıkış</a>
Gidin ** MainPage.xaml ** içinde **Çözüm Gezgini**, sonra da yeni bir pencerede açılır dosyasına çift tıklayın. Kolaylık olması için çift tıklayarak **XAML** düğmesini **Tasarımcısı** sekmesinde, bu Gizle **görsel tasarımcı** ve tüm kod görünümü alanı ayırın.

 ![Proje dahil nuget kitaplığı](./Images/UWPMainPage.png)
 
Bir metin hizmeti olarak işlevselliği görselleştirmek için en iyi yolu bir girdi ve çıktı metin bloğu oluşturuyor. Bunu yapmak için aşağıdaki XAML'de ekleyin **kılavuz**. Bu kod, üç bileşeni, bir giriş metin kutusu, çıktı metin bloğu ve Başlat düğmesi ekler.
 
 ```XAML
 <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <TextBox x:Name="inputBox" Grid.Row="0" TextWrapping="Wrap" Text="Enter a paragraph" Margin="10" AcceptsReturn="True" />
    <TextBlock x:Name="outputBlock" Grid.Row="1" TextWrapping="Wrap" Text="Result will be here" Margin="10" />
    <Button x:Name="button" Grid.Row="2" Content="Get Result" />
</Grid>
 ```
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">5. adım: Varlık yönetim bilgileri hizmeti bağlama eklemek devam</a>
 
Kullanıcı arabirimi şimdi oluşturulur. Varlık bağlama hizmet kullanmadan önce biz düğme tıklama işleyici eklemeniz gerekir. Açık **MainPage.xaml** gelen **Çözüm Gezgini**. Bir button_Click işleyici düğmesinin sonunda ekleyin.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Bir düğme tıklama işleyici kodu uygulanması gerekir. Açık **MainPage.xaml.cs** gelen **Çözüm Gezgini** düğme tıklama uygulamak için. EntityLinkingServiceClient varlık bağlama yanıtları almak için bir sarmalayıcı ' dir. EntityLinkingServiceClient oluşturucu bağımsız değişkeni Bilişsel hizmetler abonelik anahtardır. İçinde aldığınız abonelik anahtarında Yapıştır **1. adım** varlık bağlama hizmetini çağırmak için. 

"WikipediaId" varlık bağlama hizmetini kullanarak yanıta ekler örnek kod aşağıdadır. 
 
 ```csharp
 private async void button_Click(object sender, RoutedEventArgs e)
{
    var text = this.inputBox.Text;
    var client = new EntityLinkingServiceClient("Your subscription key","https://api.labs.cognitive.microsoft.com");
    var linkResponse = await client.LinkAsync(text);
    var result = string.Join(", ", linkResponse.Select(i => i.WikipediaID).ToList());
    this.outputBlock.Text = result;
}
 ```
 
Artık ilk doğal varlık bağlama uygulama işleme dilinizi çalıştırmak hazırsınız. Tuşuna **F5 tuşuna** derlemek ve uygulamayı başlatmak için. Metin parçacıkları veya paragrafları giriş kutusuna yapıştırın. "Get sonuç" düğmesine basın ve çıktı bloğu içinde tanımlanan varlıkları inceleyin.
 
 ![UWP örnek sonuç](./Images/DemoCodeResult.png)
 
### <a name="summary">Özet</a>
 
Bu öğreticide yalnızca birkaç satırlık C# ve XAML kodu ile varlık bağlama Intelligence hizmeti istemci kitaplığı yararlanmak için bir uygulama oluşturmak nasıl öğrendiniz. 

