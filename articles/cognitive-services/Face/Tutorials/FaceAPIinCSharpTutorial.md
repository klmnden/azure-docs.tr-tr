---
title: "Öğretici: Bir görüntüdeki yüzleri algılama ve çerçeveleme - Yüz Tanıma API'si, C#"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide bir görüntüdeki yüzleri algılamak ve çerçevelemek için Yüz Tanıma API'sini kullanan bir Windows uygulaması oluşturacaksınız.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: nolachar
ms.openlocfilehash: 657c471761c36de5095763623210909308f55c2a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162621"
---
# <a name="tutorial-create-a-wpf-app-to-detect-and-frame-faces-in-an-image"></a>Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için WPF uygulaması oluşturma

Bu öğreticide, .NET istemci kitaplığı aracılığıyla Yüz Tanıma hizmetini kullanan bir Windows Presentation Framework (WPF) uygulaması oluşturursunuz. Uygulama resimdeki yüzleri algılar, her yüzün çevresine bir çerçeve çizer ve durum çubuğunda yüzün açıklamasını görüntüler. Örnek kodun tamamı GitHub'da [Windows'da resimdeki yüzleri algılama ve çerçeve içine alma](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample).

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - WPF uygulaması oluşturma
> - Yüz Tanıma hizmeti istemci kitaplığını yükleme
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme
> - Durum çubuğunda yüzün açıklamasını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

- Örneği çalıştırmanız için bir abonelik anahtarınız olmalıdır. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz deneme abonelik anahtarları alabilirsiniz.
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü. Visual Studio 2017 için, .NET Desktop uygulama geliştirme iş yükü gereklidir. Bu öğreticide Visual Studio 2017 Community Edition kullanılır.
- [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview) istemci kitaplığı NuGet paketi. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="create-the-visual-studio-solution"></a>Visual Studio çözümünü oluşturma

Windows WPF uygulama projesini oluşturmak için bu adımları izleyin.

1. Visual Studio’yu açın ve **Dosya** menüsünde **Yeni**’ye, ardından **Proje**’ye tıklayın.
   - Visual Studio 2017'de, **Yüklü** öğesini ve ardından **Diğer Diller**'i genişletin. **Visual C#** seçeneğini, sonra da **WPF Uygulaması (.NET Framework)** öğesini seçin.
   - Visual Studio 2015'te, **Yüklü** öğesini ve ardından **Şablonlar**'ı genişletin. **Visual C#** seçeneğini, sonra da **WPF Uygulaması** öğesini seçin.
1. Uygulamaya **FaceTutorial** adını verin ve **Tamam**'a tıklayın.

## <a name="install-the-face-service-client-library"></a>Yüz Tanıma hizmeti istemci kitaplığını yükleme

İstemci kitaplığını yüklemek için şu yönergeleri izleyin.

1. **Araçlar** menüsünden **NuGet Paket Yöneticisi**’ni ve ardından **Paket Yöneticisi Konsolu**’nu seçin.
1. **Paket Yöneticisi Konsolu**'nda aşağıdakini yapıştırın ve **Enter** tuşuna basın.

    `Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.2.0-preview`

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

### <a name="mainwindowxaml"></a>MainWindow.xaml

*MainWindow.xaml* dosyasını açın (ipucu: bölmeleri **yukarı/aşağı ok simgesini** kullanarak değiştirin) ve içeriği aşağıdaki kodla değiştirin. Bu xaml kodu, kullanıcı arabirimi penceresini oluşturmak için kullanılır. `FacePhoto_MouseMove` ve `BrowseButton_Click` olay işleyicilerine dikkat edin.

```xml
<Window x:Class="FaceTutorial.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                    Content="Browse..."
                    Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

*MainWindow.xaml* öğesini genişletin, sonra *MainWindow.xaml.cs* dosyasını açın ve içeriği aşağıdaki kodla değiştirin. Dalgalı kırmızı alt çizgileri yoksayın; ilk derleme sonrasında bunlar gözden kaybolacaktır.

İlk iki satır, istemci kitaplığı ad alanlarını içeri aktarır. Ardından `FaceClient` oluşturulur ve abonelik anahtarı geçirilir; bu arada `MainWindow` oluşturucusunda Azure bölgesi ayarlanır. İki yöntem (`BrowseButton_Click` ve `FacePhoto_MouseMove`), *MainWindow.xaml* dosyasında bildirilen olay işleyicilerine karşılık gelir.

`BrowseButton_Click`, kullanıcının jpg resmi seçmesine olanak tanıyan bir `OpenFileDialog` oluşturur. Resim okunur ve ana pencerede görüntülenir. `BrowseButton_Click` kodunun kalan bölümü ve `FacePhoto_MouseMove` kodu, izleyen adımlarda eklenir.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;

namespace FaceTutorial
{
    public partial class MainWindow : Window
    {
        // Replace <SubscriptionKey> with your valid subscription key.
        // For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // Replace or verify the region.
        //
        // You must use the same region as you used to obtain your subscription
        // keys. For example, if you obtained your subscription keys from the
        // westus region, replace "westcentralus" with "westus".
        //
        // NOTE: Free trial subscription keys are generated in the westcentralus
        // region, so if you are using a free trial subscription key, you should
        // not need to change this region.
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });

        IList<DetectedFace> faceList;   // The list of detected faces.
        String[] faceDescriptions;      // The list of descriptions for the detected faces.
        double resizeFactor;            // The resize factor for the displayed image.

        public MainWindow()
        {
            InitializeComponent();

            if (Uri.IsWellFormedUriString(faceEndpoint, UriKind.Absolute))
            {
                faceClient.Endpoint = faceEndpoint;
            }
            else
            {
                MessageBox.Show(faceEndpoint,
                    "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
                Environment.Exit(0);
            }
        }

        // Displays the image and calls UploadAndDetectFaces.
        private async void BrowseButton_Click(object sender, RoutedEventArgs e)
        {
            // Get the image file to scan from the user.
            var openDlg = new Microsoft.Win32.OpenFileDialog();

            openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
            bool? result = openDlg.ShowDialog(this);

            // Return if canceled.
            if (!(bool)result)
            {
                return;
            }

            // Display the image file.
            string filePath = openDlg.FileName;

            Uri fileUri = new Uri(filePath);
            BitmapImage bitmapSource = new BitmapImage();

            bitmapSource.BeginInit();
            bitmapSource.CacheOption = BitmapCacheOption.None;
            bitmapSource.UriSource = fileUri;
            bitmapSource.EndInit();

            FacePhoto.Source = bitmapSource;
        }

        // Displays the face description when the mouse is over a face rectangle.
        private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
        {
        }
    }
}
```

### <a name="insert-your-subscription-key-and-verify-or-change-the-region"></a>Abonelik anahtarınızı ekleme ve bölgeyi doğrulama veya değiştirme

- *MainWindow.xaml.cs* dosyasında aşağıdaki satırı bulun ve `<Subscription Key>` değerini Yüz Tanıma API'si abonelik anahtarınızla değiştirin:

    ```csharp
    private const string subscriptionKey = "<SubscriptionKey>";
    ```

- *MainWindow.xaml.cs* dosyasında aşağıdaki satırı bulun ve abonelik anahtarınızla ilişkilendirilmiş Azure bölgesini değiştirin veya doğrulayın:

    ```csharp
    private const string Endpoint =
        "https://westcentralus.api.cognitive.microsoft.com";
    ```

    Konumun, abonelik anahtarlarınızı aldığınız yerle aynı olduğundan emin olun. Örneğin abonelik anahtarlarınızı **westus** bölgesinden aldıysanız, `Westcentralus` değerini `Westus` ile değiştirin.

    Abonelik anahtarlarınızı ücretsiz denemeyi kullanarak aldıysanız, anahtarlarınızın bölgesi **westcentralus**'tur ve bu nedenle hiçbir değişiklik gerekmez.

### <a name="test-the-app"></a>Uygulamayı test edin

Uygulamanızı test etmek için menüde **Başlat**'a basın. Pencere açıldığında, sol alt köşedeki **Gözat**'a tıklayın. Fotoğraflara göz atıp daha sonra pencerede görüntülenecek olan fotoğrafı seçebileceğiniz **Dosya Aç** iletişim kutusu görüntülenir.

![Yüzlerin değiştirilmemiş resmini gösteren ekran görüntüsü](../Images/getting-started-cs-ui.png)

## <a name="upload-an-image-to-detect-faces"></a>Yüzlerin algılanması için resmi karşıya yükleme

Yüzleri algılamanın en kolay yolu`FaceClient.Face.DetectWithStreamAsync` yöntemini çağırmaktır. Bu yöntem, yerel resmin karşıya yüklenmesi için [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API yöntemini sarmalar.

Aşağıdaki yöntemi `MainWindow` sınıfında `FacePhoto_MouseMove` yönteminin altına ekleyin.

Analiz etmek için yüz özniteliklerinin listesi oluşturulur ve gönderilen resim dosyası bir `Stream` içinde okunur. Her ikisi de `DetectWithStreamAsync` çağrısına geçirilir.

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-each-face"></a>Her yüzün çevresine dikdörtgenler çizme

Resimde algılanan her yüzün çevresine bir dikdörtgen çizmek için kodu ekleyin.

*MainWindow.xaml.cs* dosyasında, `BrowseButton_Click` yöntemine `async` değiştiricisini ekleyin.

```csharp
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
```

Aşağıdaki kodu `BrowseButton_Click` yönteminin sonuna, `FacePhoto.Source = bitmapSource` satırının ardına ekleyin.

Algılanan yüz listesi, `UploadAndDetectFaces` çağrısıyla doldurulur. Ardından, her yüzün çevresine bir dikdörtgen çizilir ve değiştirilen resim ana pencerede görüntülenir.

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    resizeFactor = (dpi > 0) ? 96 / dpi : 1;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text =
        "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="describe-the-faces-in-the-image"></a>Resimdeki yüzleri açıklama

Aşağıdaki yöntemi `MainWindow` sınıfına, `UploadAndDetectFaces` yönteminin altına ekleyin.

Yöntem, yüz özniteliklerini yüzü açıklayan bir dizeye dönüştürür. Fare işaretçisi yüz dikdörtgeninin üzerine geldiğinde dize görüntülenir.

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f)
        sb.Append(String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f)
        sb.Append(String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f)
        sb.Append(String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f)
        sb.Append(String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f)
        sb.Append(String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f)
        sb.Append(String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f)
        sb.Append(String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f)
        sb.Append(String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>Yüz açıklamasını görüntüleme

`FacePhoto_MouseMove` yöntemini aşağıdaki kodla değiştirin.

Bu olay işleyicisi, fare işaretçisi yüz dikdörtgeninin üzerine geldiğinde yüz açıklaması dizesini görüntüler.

```csharp
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
    // If the REST call has not completed, return.
    if (faceList == null)
        return;

    // Find the mouse position relative to the image.
    Point mouseXY = e.GetPosition(FacePhoto);

    ImageSource imageSource = FacePhoto.Source;
    BitmapSource bitmapSource = (BitmapSource)imageSource;

    // Scale adjustment between the actual size and displayed size.
    var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

    // Check if this mouse position is over a face rectangle.
    bool mouseOverFace = false;

    for (int i = 0; i < faceList.Count; ++i)
    {
        FaceRectangle fr = faceList[i].FaceRectangle;
        double left = fr.Left * scale;
        double top = fr.Top * scale;
        double width = fr.Width * scale;
        double height = fr.Height * scale;

        // Display the face description if the mouse is over this face rectangle.
        if (mouseXY.X >= left && mouseXY.X <= left + width &&
            mouseXY.Y >= top  && mouseXY.Y <= top + height)
        {
            faceDescriptionStatusBar.Text = faceDescriptions[i];
            mouseOverFace = true;
            break;
        }
    }

    // String to display when the mouse is not over a face rectangle.
    if (!mouseOverFace)
        faceDescriptionStatusBar.Text =
            "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüz içeren bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Bundan sonra, resimdeki yüzlerin üzerinde kırmızı bir dikdörtgen göreceksiniz. Fareyi yüz dikdörtgeninin üzerine getirdiğinizde, durum çubuğunda o yüzün açıklaması gösterilir.

![Algılanan yüzlerin dikdörtgenlerle çerçeve içine alındığını gösteren ekran görüntüsü](../Images/getting-started-cs-detected.png)

## <a name="summary"></a>Özet

Bu öğreticide, Yüz Tanıma hizmeti istemci kitaplığını kullanmanın temel sürecini öğrendiniz ve resimde yüzleri görüntüleyen ve çerçeve içine alan bir uygulama oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Yüz işaretlerini algılama ve kullanma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
