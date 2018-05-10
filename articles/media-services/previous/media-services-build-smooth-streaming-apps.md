---
title: Kesintisiz akış Windows mağazası uygulaması Öğreticisi | Microsoft Docs
description: Bir C# Windows mağazası uygulaması, kesintisiz akışı kayıttan yürütme için XML MediaElement denetimi ile içerik oluşturmak için Azure Media Services kullanmayı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: juliako
ms.openlocfilehash: f55e8aa652d65ea751a77742fa5823b09d9ee87b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Windows mağazası uygulama akışı düzgün oluşturma

Kesintisiz akış istemci SDK Windows 8 için isteğe bağlı ve canlı kesintisiz akış içeriği yürütmek Windows mağazası uygulamaları geliştiricilerin oluşturmalarını sağlar. Ek olarak temel kayıttan kesintisiz akış içeriğinin SDK Microsoft PlayReady koruma, kalite düzeyi kısıtlama, Canlı DVR, geçiş, durum güncelleştirmeleri (kalite düzeyi değişiklikleri gibi) için dinleme ses akışı gibi zengin özellikleri de sağlar ve hata olayları ve benzeri. Desteklenen özellikler daha fazla bilgi için bkz: [sürüm notları](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Daha fazla bilgi için bkz: [Windows 8 için Player Framework](http://playerframework.codeplex.com/). 

Bu öğretici dört dersleri içerir:

1. Bir temel kesintisiz akış depolama uygulaması oluştur
2. Medya ilerleme durumunu denetlemek için bir kaydırıcı çubuğu ekleme
3. Kesintisiz akış akışları seçin
4. Kesintisiz akış parçaları seçin

## <a name="prerequisites"></a>Önkoşullar
> [!NOTE]
> Windows mağazası projeleri sürüm 8.1 ve önceki sürümleri Visual Studio 2017 desteklenmiyor.  Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

* Windows 8 32 bit veya 64-bit.
* Visual Studio sürümleri 2015 aracılığıyla 2012.
* [Microsoft Windows 8 için İstemci SDK akış kesintisiz](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

Tamamlanan çözümü her ders için MSDN Geliştirici kod örneklerini (kod Galerisi) indirilebilir: 

* [Ders 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - Media Player, akış basit Windows 8 kesintisiz 
* [Ders 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - basit Windows 8 düzgün Media Player kaydırıcı çubuğun ile akış denetimi 
* [Ders 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - bir Windows 8 kesintisiz akış seçimi ile Media Player akış  
* [Ders 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Media Player izleme seçimi ile akış Windows 8 düzgün.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Ders 1: temel bir kesintisiz akış mağazası uygulaması oluşturma

Bu ders içinde bir Windows mağazası uygulaması kesintisiz akış yürütmek için MediaElement denetimi içerik oluşturur.  Çalışan uygulama şuna benzer:

![Kesintisiz akış Windows mağazası uygulama örneği][PlayerApplication]

Windows mağazası uygulaması geliştirme hakkında daha fazla bilgi için bkz: [geliştirmek harika uygulamaları Windows 8 için](http://msdn.microsoft.com/windows/apps/br229512.aspx). Bu ders aşağıdaki yordamları içerir:

1. Bir Windows mağazası projesi oluşturma
2. Tasarım kullanıcı arabirimi (XAML)
3. Dosyanın arkasındaki kodu değiştirin
4. Derleme ve uygulamayı test etme

**Bir Windows mağazası projesi oluşturmak için**

1. Visual Studio çalıştırın; sürümleri 2012 2015 aracılığıyla desteklenir.
2. **DOSYA** menüsünde **Yeni**’ye ve sonra **Proje**’ye tıklayın.
3. Yeni Proje iletişim kutusundan yazın veya aşağıdaki değerleri seçin:

| Ad | Değer |
| --- | --- |
| Şablon grubu |Yüklü/Şablonlar/Visual C# / Windows Mağazası |
| Şablon |Boş uygulama (XAML) |
| Ad |SSPlayer |
| Konum |C:\SSTutorials |
| Çözüm adı |SSPlayer |
| Çözüm için dizin oluştur |(seçili) |

1. **Tamam**’a tıklayın.

**Kesintisiz akış istemci SDK'sına bir başvuru eklemek için**

1. Çözüm Gezgini'nden sağ **SSPlayer**ve ardından **Başvuru Ekle**.
2. Aşağıdaki değerleri yazın veya seçin:

| Ad | Değer |
| --- | --- |
| Başvuru grubu |Windows/uzantıları |
| Başvuru |Microsoft Windows 8 ve Microsoft Visual C++ çalışma zamanı paketi için İstemci SDK akış kesintisiz seçin |

1. **Tamam**’a tıklayın. 

Başvuruları ekledikten sonra hedeflenen bir platform (x64 veya x86) seçmeniz gerekir, ekleyerek başvuruları için herhangi bir CPU platform yapılandırması çalışmaz.  Çözüm Gezgini'nde, sarı renkli uyarı işareti bu başvurular eklenen görürsünüz.

**Tasarım player kullanıcı arabirimi**

1. Çözüm Gezgini'nde, çift tıklayarak **MainPage.xaml** Tasarım görünümünde açın.
2. Bulun **&lt;kılavuz&gt;** ve **&lt;/Grid&gt;** XAML dosyası etiketleri ve iki etiketleri arasına aşağıdaki kodu yapıştırın:

         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
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
   
   MediaElement denetimi kayıttan yürütme ortamı için kullanılır. SliderProgress adlı kaydırıcı denetimi sonraki Ders medya ilerleme durumunu denetlemek için kullanılır.
3. Tuşuna **CTRL + S** dosyayı kaydetmek için.

MediaElement denetimi kesintisiz akış içerik out-of-box desteklemiyor. Kesintisiz akış desteğini etkinleştirmek için dosya adı uzantısı ve MIME türüne göre kesintisiz akış bayt akışı işleyici kaydetmeniz gerekir.  Kaydetmek için Windows.Media ad MediaExtensionManager.RegisterByteStremHandler yöntemi kullanın.

Bu XAML dosyası içinde bazı olay işleyicileri denetimleri ile ilişkilendirilmiş.  Bu olay işleyicileri tanımlamanız gerekir.

**Dosyanın arkasındaki kod değiştirmek için**

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **görünümü kodu**.
2. Dosyanın üst kısmında, aşağıdaki ekleme deyimini kullanarak:
   
        using Windows.Media;
3. Başında **MainPage** sınıfı, aşağıdaki veri üyesi ekleyin:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Sonunda **MainPage** oluşturucusu, aşağıdaki iki satırı ekleyin:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Sonunda **MainPage** sınıfında, aşağıdaki kodu yapıştırın:
   
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

SliderProgress_PointerPressed olay işleyicisi burada belirtilir.  Bu öğreticinin sonraki Ders içinde ele çalışma edinilir yapmak için daha fazla works vardır.
6. Tuşuna **CTRL + S** dosyayı kaydetmek için.

Tamamlanmış dosyanın arkasındaki kod şöyle:

![Visual Studio, kesintisiz akış Windows mağazası uygulamasında Codeview][CodeViewPic]

**Derleme ve uygulamayı test etme**

1. Gelen **yapı** menüsünde tıklatın **Configuration Manager**.
2. Değişiklik **etkin çözüm platformu** geliştirme platformu eşleşecek şekilde.
3. Tuşuna **F6** Projeyi derlemek için. 
4. Uygulamayı çalıştırmak için **F5**'e basın.
5. Uygulama üst kısmında, varsayılan kesintisiz akış URL'sini kullanabilir veya farklı bir tane girin. 
6. Tıklatın **ayarlamak kaynak**. Çünkü **Otomatik Yürüt** etkin varsayılan olarak, medyayı otomatik olarak yürütmek.  Medya kullanarak denetleyebilirsiniz **Yürüt**, **duraklatma** ve **durdurmak** düğmeler.  Dikey kaydırıcıyı kullanarak medya birimi kontrol edebilirsiniz.  Ancak medya ilerleme durumunu denetlemek için yatay kaydırıcı tam henüz uygulanmadı. 

Lesson1 tamamladınız.  Bu ders, kesintisiz akış içeriği kayıttan yürütme için MediaElement denetimi kullanın.  Sonraki Ders kesintisiz akış içeriğinin ilerleme durumunu denetlemek için kaydırıcıyı ekleyeceksiniz.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Ders 2: Media ilerleme durumunu denetlemek için bir kaydırıcı çubuğun ekleme

Ders 1'de, kesintisiz akış medya içeriği kayıttan yürütme MediaElement XAML denetimine sahip bir Windows mağazası uygulaması oluşturuldu.  Başlatma, durdurma ve duraklatma gibi bazı temel media işlevleri gelir.  Bu ders, uygulamaya kaydırıcı çubuğu denetimi ekleyeceksiniz.

Bu öğreticide, MediaElement denetimi geçerli konumuna bağlı kaydırıcı konumunu güncelleştirmek için bir zamanlayıcı kullanacağız.  Kaydırıcı başlangıç ve bitiş de canlı içerik durumunda güncelleştirilmesi gerekiyor zaman.  Bu, daha iyi Uyarlamalı kaynak güncelleştirme olayda işlenebilir.

Medya kaynaklarına medya veri üreten nesneleridir.  Kaynak Çözümleyici bir URL veya bayt akış alır ve bu içerik için uygun medya kaynağı oluşturur.  Kaynak çözümleyici medya kaynaklarına oluşturmak üzere uygulamalar için standart yoludur. 

Bu ders aşağıdaki yordamları içerir:

1. Kesintisiz akış işleyici kaydetme 
2. Uyarlamalı Kaynak Yöneticisi düzeyi olay işleyicileri ekleme
3. Uyarlamalı kaynak düzeyi olay işleyicileri ekleme
4. MediaElement olay işleyicileri ekleme
5. Kaydırma çubuğu ilgili kod ekleme
6. Derleme ve uygulamayı test etme

**Kesintisiz akış bayt akışı işleyici kaydetmek ve propertyset geçirmek için**

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **görünümü kodu**.
2. Dosyanın başına aşağıdaki Ekle deyimi kullanarak:

        using Microsoft.Media.AdaptiveStreaming;
3. MainPage sınıfı başına aşağıdaki veri üyeleri Ekle:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. İçinde **MainPage** oluşturucusu, sonra aşağıdaki kodu ekleyin **bu. Components() başlatılamadı;**  satır ve kayıt kod önceki Ders yazılmış satırları:

        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. İçinde **MainPage** oluşturucusunu eklemek için iki RegisterByteStreamHandler yöntem Değiştir İleri parametreleri:

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
6. Tuşuna **CTRL + S** dosyayı kaydetmek için.

**Uyarlamalı Kaynak Yöneticisi düzeyi olay işleyicisi eklemek için**

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **görünümü kodu**.
2. İçinde **MainPage** sınıfı, aşağıdaki veri üyesi ekleyin:
   
     Özel AdaptiveSource adaptiveSource = null;
3. Sonunda **MainPage** sınıfı, aşağıdaki olay işleyicisini ekleyin:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. Sonunda **MainPage** oluşturucusu, Uyarlamalı kaynak açık olaya abone olmak için aşağıdaki satırı ekleyin:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Tuşuna **CTRL + S** dosyayı kaydetmek için.

**Uyarlamalı kaynak düzeyi olay işleyicileri eklemek için**

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **görünümü kodu**.
2. İçinde **MainPage** sınıfı, aşağıdaki veri üyesi ekleyin:
   
     Özel AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   Özel bildirim manifestObject;
3. Sonunda **MainPage** sınıfı, şu olay işleyicileri ekleyin:

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
4. Sonunda **mediaElement AdaptiveSourceOpened** yöntemi, olaylara abone olmak için aşağıdaki kodu ekleyin:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Tuşuna **CTRL + S** dosyayı kaydetmek için.

Aynı olaylar, uygulamadaki tüm ortam öğeleri için ortak işlevselliği işlemek için kullanılan Uyarlamalı kaynak yöneticisi düzeyinde de kullanılabilir. Tüm AdaptiveSource olayları AdaptiveSourceManager altında basamaklı ve her AdaptiveSource, kendi olaylarını içerir.

**Ortam öğesi olay işleyicileri ekleme**

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **görünümü kodu**.
2. Sonunda **MainPage** sınıfı, şu olay işleyicileri ekleyin:

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
3. Sonunda **MainPage** oluşturucusu, alt simge olayları için aşağıdaki kodu ekleyin:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Tuşuna **CTRL + S** dosyayı kaydetmek için.

**Kaydırıcı çubuğun eklemek için kodu ilgili**

1. Çözüm Gezgini'nden sağ tıklayın **MainPage.xaml**ve ardından **görünümü kodu**.
2. Dosyanın başına aşağıdaki Ekle deyimi kullanarak:
      
        using Windows.UI.Core;
3. İçinde **MainPage** sınıfı, aşağıdaki veri üyeleri Ekle:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. Sonunda **MainPage** oluşturucusu, aşağıdaki kodu ekleyin:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. Sonunda **MainPage** sınıfında, aşağıdaki kodu ekleyin:

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
      
>[!NOTE]
>CoreDispatcher dışı kullanıcı Arabirimi iş parçacığından için kullanıcı Arabirimi iş parçacığı değişiklik yapmak için kullanılır. Dağıtıcı iş parçacığı engeli durumunda, geliştirici UI klasöründe güncelleştirme amaçlayan öğesi tarafından sağlanan dağıtıcısı kullanmayı seçebilirsiniz.  Örneğin:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. Sonunda **mediaElement_AdaptiveSourceStatusUpdated** yöntemi, aşağıdaki kodu ekleyin:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. Sonunda **MediaOpened** yöntemi, aşağıdaki kodu ekleyin:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Tuşuna **CTRL + S** dosyayı kaydetmek için.

**Derleme ve uygulamayı test etme**

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulama üst kısmında, varsayılan kesintisiz akış URL'sini kullanabilir veya farklı bir tane girin. 
4. Tıklatın **ayarlamak kaynak**. 
5. Kaydırıcı çubuğun sınayın.

Ders 2 tamamladınız.  Bu ders uygulamaya kaydırıcıyı eklendi. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Ders 3: Kesintisiz akış akışları seçin
Kesintisiz akış görüntüleyicileri tarafından seçilebilen birden çok dil ses izleri ile içerik akışı sağlamak yeteneğine sahiptir.  Bu alıştırmanın ilerisinde akışları seçmek görüntüleyiciler olanak sağlar. Bu ders aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Kod behand dosyasını değiştirme
3. Derleme ve uygulamayı test etme

**XAML dosyasını değiştirmek için**

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **Görünüm Tasarımcısı**.
2. Bulun &lt;Grid.RowDefinitions&gt;ve RowDefinitions görünür gibi değiştirin:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. İçinde &lt;kılavuz&gt;&lt;/Grid&gt; etiketler, böylece kullanıcılar kullanılabilir akışları listesini görmek ve seçin akışlar bir listbox denetimini tanımlamak için aşağıdaki kodu ekleyin:

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
4. Tuşuna **CTRL + S** değişiklikleri kaydedin.

**Dosyanın arkasındaki kod değiştirmek için**

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **görünümü kodu**.
2. SSPlayer ad alanı içinde yeni bir sınıf ekleyin:
   
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
3. MainPage sınıfı başlangıcında, aşağıdaki değişken tanımları ekleyin:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. MainPage sınıfında, aşağıdaki bölge ekleyin:
   
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
   
            // Select the frist video stream from the list if no video stream is selected
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
   
            // Select the frist audio stream from the list if no audio steam is selected.
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
5. MediaElement_ManifestReady yöntemini bulun, işlevi sonuna aşağıdaki kodu ekleyin:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Bu nedenle MediaElement bildirimi hazır olduğunda, kod kullanılabilir akışları listesini alır ve UI liste kutusu listesi ile doldurur.
6. Kullanıcı arabirimini MainPage sınıfı içinde bulun düğmeleri olayları bölge'ye tıklayın ve ardından aşağıdaki işlevi ekleyin:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Derleme ve uygulamayı test etme**

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulama üst kısmında, varsayılan kesintisiz akış URL'sini kullanabilir veya farklı bir tane girin. 
4. Tıklatın **ayarlamak kaynak**. 
5. Varsayılan audio_eng dilidir. Audio_es audio_eng arasında geçiş yapmak deneyin. Her, yeni bir akış seçin, Gönder düğmesine tıklamanız gerekir.

Ders 3 tamamladınız.  Bu alıştırmanın ilerisinde akışları seçmek için işlevsellik ekleyin.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Ders 4: Kesintisiz akış parçaları seçin
Kesintisiz akış sunu farklı kalite düzeyleri (bit hızları) ve çözümlemeleri ile kodlanmış birden çok video dosyaları içerebilir. Bu ders, kullanıcıların parçaları seçmesine olanak sağlar. Bu ders aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Kod behand dosyasını değiştirme
3. Derleme ve uygulamayı test etme

**XAML dosyasını değiştirmek için**

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **Görünüm Tasarımcısı**.
2. Bulun &lt;kılavuz&gt; etiket adıyla **gridStreamAndBitrateSelection**, etiket sonuna aşağıdaki kodu ekleyin:
   
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
3. Tuşuna **CTRL + S** he değişiklikleri kaydetmek için

**Dosyanın arkasındaki kod değiştirmek için**

1. Çözüm Gezgini'nden sağ **MainPage.xaml**ve ardından **görünümü kodu**.
2. SSPlayer ad alanı içinde yeni bir sınıf ekleyin:
   
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
3. MainPage sınıfı başlangıcında, aşağıdaki değişken tanımları ekleyin:
   
        private List<Track> availableTracks;
4. MainPage sınıfında, aşağıdaki bölge ekleyin:
   
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
5. MediaElement_ManifestReady yöntemini bulun, işlevi sonuna aşağıdaki kodu ekleyin:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Kullanıcı arabirimini MainPage sınıfı içinde bulun düğmeleri olayları bölge'ye tıklayın ve ardından aşağıdaki işlevi ekleyin:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }

**Derleme ve uygulamayı test etme**

1. Tuşuna **F6** Projeyi derlemek için. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulama üst kısmında, varsayılan kesintisiz akış URL'sini kullanabilir veya farklı bir tane girin. 
4. Tıklatın **ayarlamak kaynak**. 
5. Varsayılan olarak, tüm video akışına parçaları seçilir. Bit hızı değişiklikleri denemek için en düşük bit hızı seçin ve ardından kullanılabilir en yüksek bit hızı seçin. Her bir değişiklikten sonra Gönder'i tıklatmalısınız.  Video kalitesini değişiklikleri görebilirsiniz.

Ders 4 tamamladınız.  Bu alıştırmanın ilerisinde parçaları seçmek için işlevsellik ekleyin.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Diğer kaynaklar:
* [Gelişmiş özelliklere sahip bir kesintisiz akış Windows 8 JavaScript uygulaması oluşturma](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Kesintisiz akış teknik genel bakış](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

