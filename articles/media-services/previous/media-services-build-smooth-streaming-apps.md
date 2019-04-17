---
title: Kesintisiz akış Windows Store uygulaması Öğreticisi | Microsoft Docs
description: Oluşturmak için Azure Media Services'ı kullanmayı öğrenin bir C# kayıttan yürütme düzgün Stream içeriği için bir XML MediaElement denetimi ile Windows Store uygulaması.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 910c593c9277efeaf72dadc52740b1c918381e19
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59524778"
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Sorunsuz bir akış Windows Store uygulaması oluşturma  

Kesintisiz akış istemci SDK'sı Windows 8 için isteğe bağlı ve canlı kesintisiz akış içeriği oynatabilirsiniz Windows Store uygulamalar oluşturmalarını sağlar. Kesintisiz akış içeriği temel çalınmasını ek olarak, SDK'sı Microsoft PlayReady koruması, kalite düzeyi kısıtlama, DVR, geçiş, durum güncelleştirmeleri (örneğin, kalite düzeyi değişiklikler) için dinleme ses akışı Live gibi zengin özellikler de sağlar ve hata olayları ve benzeri. Desteklenen özellikler daha fazla bilgi için bkz. [sürüm notları](https://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Daha fazla bilgi için [Windows 8 için Player Framework](https://playerframework.codeplex.com/). 

Bu öğreticide, dört ders içerir:

1. Bir temel kesintisiz akış Store uygulaması oluşturma
2. Medya ilerleme durumunu denetlemek için bir kaydırıcı çubuğu Ekle
3. Kesintisiz akış akışları seçin
4. Kesintisiz akış parçaları seçin

## <a name="prerequisites"></a>Önkoşullar
> [!NOTE]
> Windows Store projeleri sürüm 8.1 ve önceki sürümler, Visual Studio 2017'de desteklenmez.  Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

* Windows 8, 32 bit veya 64-bit.
* Visual Studio 2012 ile bir 2015 sürümleri.
* [Microsoft kesintisiz akış istemci SDK'sı Windows 8 için tasarım](https://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home https://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

Tamamlanan çözümü her ders için MSDN geliştirici kodu örnekleri (kod Galerisi) ' indirilebilir: 

* [Ders 1](https://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - basit bir Windows 8 sorunsuz bir akış medya oynatıcı 
* [Ders 2](https://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - basit bir Windows 8 sorunsuz bir kaydırıcı çubuğun Media Player akış denetimi 
* [Ders 3](https://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Windows 8 sorunsuz bir akış Media Player ile Stream seçimi  
* [Ders 4](https://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Windows 8 sorunsuz bir parça seçimi Media Player akış.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>1. Ders: Bir temel kesintisiz akış Store uygulaması oluşturma

Bu derste, bir Windows Store uygulaması kesintisiz Stream yürütülecek MediaElement denetimi içerik oluşturacaksınız.  Çalışan uygulamayı şu şekilde görünür:

![Kesintisiz akış Windows Store uygulaması örneği][PlayerApplication]

Windows Store uygulaması geliştirme hakkında daha fazla bilgi için bkz. [geliştirme harika uygulamalar için Windows 8](https://msdn.microsoft.com/windows/apps/br229512.aspx). Bu ders, aşağıdaki yordamları içerir:

1. Windows Store projesi oluşturma
2. Tasarım kullanıcı arabirimi (XAML)
3. Dosyanın arkasındaki kodu değiştirin
4. Derleme ve uygulamayı test etme

### <a name="to-create-a-windows-store-project"></a>Windows Store projesi oluşturmak için

1. Visual Studio'yu çalıştırın; 2012 2015 sürümleri desteklenir.
1. **DOSYA** menüsünde **Yeni**’ye ve sonra **Proje**’ye tıklayın.
1. Yeni Proje iletişim kutusunda aşağıdaki değerleri seçin veya yazın:

    | Ad | Değer |
    | --- | --- |
    | Şablon grubu |Yüklü/Şablonlar/Visual C#Windows Store |
    | Şablon |Boş uygulama (XAML) |
    | Ad |SSPlayer |
    | Konum |C:\SSTutorials |
    | Çözüm adı |SSPlayer |
    | Çözüm için dizin oluştur |(Seçili) |

1. **Tamam** düğmesine tıklayın.

### <a name="to-add-a-reference-to-the-smooth-streaming-client-sdk"></a>Kesintisiz akış istemci SDK'sına bir başvuru eklemek için

1. Çözüm Gezgini'nden sağ **SSPlayer**ve ardından **Başvuru Ekle**.
1. Aşağıdaki değerleri yazın veya seçin:

    | Ad | Değer |
    | --- | --- |
    | Başvuru grubu |Windows ve uzantıları |
    | Başvuru |Microsoft kesintisiz akış istemci SDK'sı Windows 8 ve Microsoft Visual C++ çalışma zamanı paketi için seçin |

1. **Tamam** düğmesine tıklayın. 

Başvuru ekledikten sonra hedef Platformu (x64 veya x86) seçmeniz gerekir, ekleyerek başvuruları için herhangi bir CPU platform yapılandırması çalışmaz.  Çözüm Gezgini'nde, sarı bir uyarı işareti şu başvuru eklendi görürsünüz.

### <a name="to-design-the-player-user-interface"></a>Oynatıcı kullanıcı arabirimini tasarlamak için

1. Çözüm Gezgini'nde çift tıklayarak **MainPage.xaml** Tasarım Görünümü'nde açın.
2. Bulun **&lt;kılavuz&gt;** ve **&lt;/Grid&gt;** XAML dosya etiketleri ve iki etiketleri arasına aşağıdaki kodu yapıştırın:

   ```xml
         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="https://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   ```
   MediaElement denetimi medya kayıttan yürütme için kullanılır. Kaydırıcı denetimi sliderProgress adlı bir sonraki derste medya ilerleme durumunu denetlemek için kullanılır.
3. Tuşuna **CTRL + S** dosyayı kaydetmek için.

MediaElement denetimi, kesintisiz akış içeriği kullanıma hazır desteklemez. Kesintisiz akış desteğini etkinleştirmek için kesintisiz akış bayt akışı işleyici dosya adı uzantısı ve MIME türü tarafından kaydetmeniz gerekir.  Kaydetmek için Windows.Media ad alanının MediaExtensionManager.RegisterByteStreamHandler yöntemi kullanın.

Bu XAML dosyasında bazı olay işleyicileri denetimleri ile ilişkilendirilir.  Bu olay işleyicileri tanımlamanız gerekir.

### <a name="to-modify-the-code-behind-file"></a>Arka plan kod dosyasında değiştirmek için

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **kodu görüntüle**.
2. Dosyasının en üstüne aşağıdakileri ekleyin using deyimi:
   
        using Windows.Media;
3. Başında **MainPage** sınıfında, aşağıdaki veri üyesi ekleyin:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Sonunda **MainPage** oluşturucusu, aşağıdaki iki satırı ekleyin:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Sonunda **MainPage** sınıfında, aşağıdaki kodu yapıştırın:
   ```csharp
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion
   ```
   SliderProgress_PointerPressed olay işleyicisi burada tanımlanır.  Bu öğreticinin bir sonraki derste ele bunu çalıştırmak, için daha fazla works vardır.
6. Tuşuna **CTRL + S** dosyayı kaydetmek için.

Tamamlanmış arka plan kod dosyasında şöyle:

![Visual Studio, kesintisiz akış Windows Store uygulamasında Codeview][CodeViewPic]

### <a name="to-compile-and-test-the-application"></a>Derleme ve uygulamayı test etme

1. Gelen **derleme** menüsünde tıklatın **Configuration Manager**.
2. Değişiklik **etkin çözüm platformu** geliştirme platformunuz eşleştirilecek.
3. Tuşuna **F6** Projeyi derlemek için. 
4. Uygulamayı çalıştırmak için **F5**'e basın.
5. Uygulamanın üstünde, varsayılan kesintisiz akış URL'sini kullanın veya farklı bir tane girin. 
6. Tıklayın **kümesi kaynak**. Çünkü **otomatik yürütme** etkin varsayılan olarak, ortamı otomatik olarak çalıştırma.  Medya kullanarak denetleyebilirsiniz **Play**, **duraklatma** ve **Durdur** düğmeleri.  Dikey kaydırıcıyı kullanarak medya birimi denetleyebilirsiniz.  Ancak medya ilerleme durumunu denetleme için kaydırıcıyı yatay tamamen henüz uygulanmadı. 

Lesson1 tamamladınız.  Bu derste, kesintisiz akış içeriği kayıttan MediaElement denetimi kullanın.  Sonraki derste, kesintisiz akış içeriği, ilerleme durumunu denetlemek için bir kaydırıcı ekleyeceksiniz.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>2. Ders: Medya ilerleme durumunu denetlemek için bir kaydırıcı çubuğu Ekle

Ders 1'de, kayıttan yürütme kesintisiz akış medya içeriği için bir MediaElement XAML denetimi ile bir Windows Store uygulaması oluşturdunuz.  Bu, başlatma, durdurma ve duraklatma gibi bazı temel medya işlevleri birlikte gelir.  Bu derste, uygulamayı bir kaydırıcı çubuğu denetimi ekleyeceksiniz.

Bu öğreticide, MediaElement denetimi geçerli konumuna bağlı kaydırıcı konumu güncelleştirmek için bir zamanlayıcı kullanacağız.  Ayrıca kaydırıcı başlangıç ve bitiş durumunda Canlı içerik güncelleştirilmesi gerekiyor zaman.  Bu, daha iyi Uyarlamalı kaynak güncelleştirme olayda işlenebilir.

Medya kaynakları medya verileri üreten nesneleridir.  Kaynak Çözümleyici bir URL veya bayt akışı alır ve bu içerik için uygun bir medya kaynağı oluşturur.  Kaynak çözümleyici medya kaynakları oluşturmak uygulamalar standart yoludur. 

Bu ders, aşağıdaki yordamları içerir:

1. Kesintisiz akış işleyicisini kaydetmek 
2. Uyarlamalı kaynak yöneticisi düzeyinde olay işleyicileri ekleme
3. Uyarlamalı kaynak düzeyi olay işleyicileri ekleme
4. MediaElement olay işleyicileri ekleme
5. Kaydırıcı çubuğu ilgili kod ekleme
6. Derleme ve uygulamayı test etme

### <a name="to-register-the-smooth-streaming-byte-stream-handler-and-pass-the-propertyset"></a>Kesintisiz akış bayt akışı işleyicisi kaydetmek ve propertyset geçirmek için

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **kodu görüntüle**.
2. Dosyasının başında, aşağıdaki ekleyin using deyimi:

   ```csharp
        using Microsoft.Media.AdaptiveStreaming;
   ```
3. MainPage sınıfının başına aşağıdaki veri üyelerini ekleyin:

   ```csharp
         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
   ```
4. İçinde **MainPage** oluşturucusu, sonra aşağıdaki kodu ekleyin **bu. Components() başlatılamıyor;**  satır ve kayıt kodu bir önceki derste yazılan satır:

   ```csharp
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
   ```
5. İçinde **MainPage** oluşturucusunu eklemek için iki RegisterByteStreamHandler yöntemleri değiştirme İleri parametreleri:

   ```csharp
         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
   ```
6. Tuşuna **CTRL + S** dosyayı kaydetmek için.

### <a name="to-add-the-adaptive-source-manager-level-event-handler"></a>Uyarlamalı kaynak yöneticisi düzeyinde olay işleyicisi eklemek için

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **kodu görüntüle**.
2. İçinde **MainPage** sınıfında, aşağıdaki veri üyesi ekleyin:

   ```csharp
     private AdaptiveSource adaptiveSource = null;
   ```
3. Sonunda **MainPage** sınıfında, aşağıdaki olay işleyicisini ekleyin:

   ```csharp
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
   ```
4. Sonunda **MainPage** oluşturucusu, Uyarlamalı kaynak açık olaya abone olmak için aşağıdaki satırı ekleyin:

   ```csharp
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
   ```
5. Tuşuna **CTRL + S** dosyayı kaydetmek için.

### <a name="to-add-adaptive-source-level-event-handlers"></a>Uyarlamalı kaynak düzeyi olay işleyicileri eklemek için

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **kodu görüntüle**.
2. İçinde **MainPage** sınıfında, aşağıdaki veri üyesi ekleyin:

   ```csharp
     private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
     private Manifest manifestObject;
   ```
3. Sonunda **MainPage** sınıfında, aşağıdaki olay işleyicisini ekleyin:

   ```csharp
         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
   ```
4. Sonunda **mediaElement AdaptiveSourceOpened** yöntemi, olaylara abone olmak için aşağıdaki kodu ekleyin:

   ```csharp
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
   ```
5. Tuşuna **CTRL + S** dosyayı kaydetmek için.

Aynı olayları, uygulamadaki tüm ortam öğeleri için ortak işlevselliği işlemek için kullanılan Uyarlamalı kaynak yöneticisi düzeyinde de kullanılabilir. Her AdaptiveSource kendi olaylarını içerir ve tüm AdaptiveSource olayları altında AdaptiveSourceManager basamaklı.

### <a name="to-add-media-element-event-handlers"></a>Medya öğesi olay işleyicileri eklemek için

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **kodu görüntüle**.
2. Sonunda **MainPage** sınıfında, aşağıdaki olay işleyicisini ekleyin:

   ```csharp
         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
   ```
3. Sonunda **MainPage** oluşturucusu, alt simge olaylar için aşağıdaki kodu ekleyin:

   ```csharp
         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
   ```
4. Tuşuna **CTRL + S** dosyayı kaydetmek için.

### <a name="to-add-slider-bar-related-code"></a>İlgili kod kaydırıcı çubuğu Ekle

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **kodu görüntüle**.
2. Dosyasının başında, aşağıdaki ekleyin using deyimi:

   ```csharp
        using Windows.UI.Core;
   ```
3. İçinde **MainPage** sınıfında, aşağıdaki veri üyelerini ekleyin:

   ```csharp
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
   ```
4. Sonunda **MainPage** Oluşturucu aşağıdaki kodu ekleyin:

   ```csharp
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
   ```
5. Sonunda **MainPage** sınıfında, aşağıdaki kodu ekleyin:

   ```csharp
         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
   ```

   > [!NOTE]
   > CoreDispatcher olmayan UI iş parçacığından UI iş parçacığı için değişiklik yapmak için kullanılır. Dağıtıcı iş parçacığı üzerinde performans sorunu durumunda, geliştirici, dağıtıcı UI derse güncelleştirme amaçlayan öğesi tarafından sağlanan kullanmayı seçebilirsiniz.  Örneğin:

   ```csharp
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
   ```
6. Sonunda **mediaElement_AdaptiveSourceStatusUpdated** yöntemine aşağıdaki kodu ekleyin:

   ```csharp
         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
   ```
7. Sonunda **MediaOpened** yöntemine aşağıdaki kodu ekleyin:

   ```csharp
         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
   ```
8. Tuşuna **CTRL + S** dosyayı kaydetmek için.

### <a name="to-compile-and-test-the-application"></a>Derleme ve uygulamayı test etme

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üstünde, varsayılan kesintisiz akış URL'sini kullanın veya farklı bir tane girin. 
4. Tıklayın **kümesi kaynak**. 
5. Kaydırıcı çubuğunu test edin.

Ders 2 tamamladınız.  Bu derste bir kaydırıcı uygulamaya eklenir. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>3. Ders: Kesintisiz akış akışları seçin
Kesintisiz akış, içerik akışı görüntüleyicileri tarafından seçilebilir birden çok dil ses izleri ile yeteneğine sahiptir.  Bu derste, akışları seçilecek görüntüleyiciler olanak tanır. Bu ders, aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Dosyanın arkasındaki kodu değiştirin
3. Derleme ve uygulamayı test etme

### <a name="to-modify-the-xaml-file"></a>XAML dosyasını değiştirmek için

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **Görünüm Tasarımcısı**.
2. Bulun &lt;Grid.RowDefinitions&gt;ve RowDefinitions görünür gibi değiştirin:

   ```xml
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
   ```
3. İçinde &lt;kılavuz&gt;&lt;/Grid&gt; etiketler, böylece kullanıcılar kullanılabilir akışları listesini görmek ve akış'ı seçin, bir listbox denetimi tanımlamak için aşağıdaki kodu ekleyin:

   ```xml
         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
   ```
4. Tuşuna **CTRL + S** değişiklikleri kaydedin.

### <a name="to-modify-the-code-behind-file"></a>Arka plan kod dosyasında değiştirmek için

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **kodu görüntüle**.
2. SSPlayer ad alanı içinde yeni bir sınıf ekleyin:

   ```csharp
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
   ```
3. MainPage sınıfının başına aşağıdaki değişken tanımları ekleyin:

   ```csharp
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
   ```
4. MainPage sınıfının içinde şu bölge ekleyin:
   ```csharp
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the first video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the first audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
   ```
5. MediaElement_ManifestReady yöntemini bulun, işlevin sonuna aşağıdaki kodu ekleyin:
   ```csharp
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   ```
    Bu nedenle MediaElement bildirimi hazır olduğunda, kod kullanılabilir akışlarının listesini alır ve UI liste kutusu listesi ile doldurur.
6. MainPage sınıfının içinde kullanıcı Arabirimi bulun düğmeler olayları bölge'ye tıklayın ve ardından aşağıdaki işlevi ekleyin:
   ```csharp
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }
   ```

### <a name="to-compile-and-test-the-application"></a>Derleme ve uygulamayı test etme

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üstünde, varsayılan kesintisiz akış URL'sini kullanın veya farklı bir tane girin. 
4. Tıklayın **kümesi kaynak**. 
5. Audio_eng varsayılan dildir. Audio_es audio_eng arasında geçiş yapmak bu seçeneği deneyin. Yeni bir akışı seçtiğiniz her durumda, Gönder düğmesine tıklamanız gerekir.

Ders 3 tamamladınız.  Bu derste, akışları seçmek için işlevselliği ekleyin.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>4. Ders: Kesintisiz akış parçaları seçin

Birden fazla video dosyaları farklı kalite düzeylerine (bit hızlarında) ve çözümleri ile kodlanmış kesintisiz akış sunu içerebilir. Bu derste, parçalar seçmek kullanıcıların olanak sağlar. Bu ders, aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Dosyanın arkasındaki kodu değiştirin
3. Derleme ve uygulamayı test etme

### <a name="to-modify-the-xaml-file"></a>XAML dosyasını değiştirmek için

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **Görünüm Tasarımcısı**.
2. Bulun &lt;kılavuz&gt; etiket adıyla **gridStreamAndBitrateSelection**, etiket sonuna aşağıdaki kodu ekleyin:
   ```xml
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
   ```
3. Tuşuna **CTRL + S** he değişiklikleri kaydetmek için

### <a name="to-modify-the-code-behind-file"></a>Arka plan kod dosyasında değiştirmek için

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **kodu görüntüle**.
2. SSPlayer ad alanı içinde yeni bir sınıf ekleyin:
   ```csharp
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
   ```
3. MainPage sınıfının başına aşağıdaki değişken tanımları ekleyin:
   ```csharp
        private List<Track> availableTracks;
   ```
4. MainPage sınıfının içinde şu bölge ekleyin:
   ```csharp
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
   ```
5. MediaElement_ManifestReady yöntemini bulun, işlevin sonuna aşağıdaki kodu ekleyin:
   ```csharp
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
   ```
6. MainPage sınıfının içinde kullanıcı Arabirimi bulun düğmeler olayları bölge'ye tıklayın ve ardından aşağıdaki işlevi ekleyin:
   ```csharp
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }
   ```
   
### <a name="to-compile-and-test-the-application"></a>Derleme ve uygulamayı test etme

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üstünde, varsayılan kesintisiz akış URL'sini kullanın veya farklı bir tane girin. 
4. Tıklayın **kümesi kaynak**. 
5. Varsayılan olarak, tüm video akışı aşamalardan seçilir. Bit hızı değişiklikleri denemeler yapmak için en düşük bit hızını seçin ve ardından yüksek bit hızı seçin. Her bir değişiklikten sonra Gönder'i gerekir.  Video kalitesi değişiklikleri görebilirsiniz.

Ders 4 tamamladınız.  Bu derste, parçaları seçmek için işlevselliği ekleyin.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Diğer kaynaklar:
* [Gelişmiş Özellikler ile bir kesintisiz akış Windows 8 JavaScript uygulaması oluşturmayı öğrenin](https://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Kesintisiz akış teknik genel bakış](https://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

