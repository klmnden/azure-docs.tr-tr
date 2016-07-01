<properties 
    pageTitle="Beş dakikada Azure Storage’a başlayın | Microsoft Azure" 
    description="Azure Storage Hızlı Başlangıç, Visual Studio ve Azure Storage öykünücüsü kullanarak Microsoft Azure Blob’ları, Tablo ve Kuyruklarını hızla kullanmaya başlayın. İlk Azure Storage uygulamanızı beş dakikada çalıştırın." 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="05/23/2016"
    ms.author="tamram"/>

# Beş dakikada Azure Storage’a başlayın 

## Genel Bakış

Azure Storage ile geliştirmeye başlamak kolaydır. Bu öğretici bir Azure Storage uygulamasını nasıl hızla çalıştırabileceğinizi gösterir. .NET için Azure SDK ile birlikte gelen Hızlı Başlangıç şablonlarını kullanacaksınız. Bu Hızlı Başlangıçlar Azure Storage ile bazı temel programlama senaryolarını gösteren çalıştırılmaya hazır kodlar içerir. 

Kodlara başlamadan önce Azure Storage hakkında daha fazla bilgi edinmek için bkz. [Sonraki Adımlar](#next-steps).

## Ön koşullar

Başlamadan önce aşağıdaki ön koşulları yerine getirmeniz gerekir:

1. Bir uygulama derlemek ve oluşturmak için bilgisayarınızda [Visual Studio](https://www.visualstudio.com/)’nun bir sürümünün yüklü olması gerekir. 

2. [.NET için Azure SDK](https://azure.microsoft.com/downloads/)’nin en yeni sürümünü yükleyin. SDK Azure QuickStart örnek projelerini, Azure Storage öykünücüsünü ve [.NET için Azure Storage İstemcisi](https://msdn.microsoft.com/library/azure/dn261237.aspx)ni içerir.

3. Bu öğretide kullanacağımız Azure Hızlı Başlangıç örnek projeler için gerekli olduğundan [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653)’in bilgisayarınızda yüklü olduğundan emin olun. 

    Bilgisayarınızda yüklü .NET Framework’ün sürümünü bilmiyorsanız, bkz. [Nasıl Yapılır: Hangi .NET Framework Sürümlerinin Yüklendiğini Belirleme](https://msdn.microsoft.com/vstudio/hh925568.aspx). Veya **Başlat** düğmesine ya da Windows tuşuna basın, **Denetim Paneli** yazın. Ardından **Programlar** > **Programlar ve Özellikler**’e tıklayarak yüklü programlar arasında .NET Framework 4.5’in yüklü olup olmadığını görün.

4. Bir Azure aboneliği ve Azure Storage hesabına ihtiyaç duyacaksınız.

    - Bir Azure aboneliği edinmek için bkz. [Ücretiz Deneme](https://azure.microsoft.com/pricing/free-trial/), [Satın Alma Seçenekleri](https://azure.microsoft.com/pricing/purchase-options/) ve [Üye Teklifleri](https://azure.microsoft.com/pricing/member-offers/) (MSDN, Microsoft İş Ortağı Ağı, BizSpark ve diğer Microsoft programları üyeleri).
    - Azure’da bir depolama hesabı oluşturmak için bkz. [Depolama hesabı oluşturma](storage-create-storage-account.md#create-a-storage-account).

## İlk Azure Storage uygulamanızı bulutta Azure Storage’a karşı çalıştırma

Hesabınızı edindikten sonra Visual Studio’da Azure Hızlı Başlangıç örnek projelerinden birini kullanarak basit bir Azure Storage uygulaması oluşturabilirsiniz. Bu öğreti şu Azure Storage örnek projelerine odaklanır : **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** ve **Azure Storage: Tablolar**:

1. Visual Studio’yu çalıştırın.
2. **Dosya** menüsünden **Yeni Proje**’ye tıklayın.
3. **Yeni Proje** iletişim kutusundan **Yüklenen** > **Şablonlar** > **Visual C#** > **Bulut** > **QuickStarts** > **Veri Hizmetleri**’ne tıklayın.
    a. Aşağıdaki şablonlardan birini seçin: **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** veya **Azure Storage: Tablolar**.
    b. Hedef altyapı olarak **.NET Framework 4.5**’in seçildiğinden emin olun.
    - 3.c. Aşağıda gösterildiği şekilde projeniz için bir ad belirleyin ve yeni Visual Studio çözümünü oluşturun:
    
    ![Azure Hızlı Başlangıçlar][Image1]

Uygulamayı çalıştırmadan önce kaynak kodunu incelemek isteyebilirsiniz. Kodu incelemek için Visual Studio’da **Görüntüle** menüsünde **Çözüm Gezgini**’ni seçin. Ardından Program.cs dosyasına çift tıklayın. 

Ardından örnek uygulamayı çalıştırın:

1.  Visual Studio’da **Görünüm** menüsünde **Çözüm Gezgini**’ni seçin. App.config dosyasını açın ve Azure Storage öykünücüsü için bağlantı dizesine açıklama yerleştirin.

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Azure Storage Hizmeti için bağlantı dizesinin açıklamasını kaldırın ve App.config dosyasında depolama hesabı adı ile erişim tuşunu sağlayın: `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Depolama hesabı erişim tuşunuzı almak için bkz. [Depolama erişim tuşlarınızı yönetme](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  App.config dosyasında depolama hesap adı ve erişim tuşunu sağladıktan sonra **Dosya** menüsünde, **Tümünü Kaydet**’e tıklayarak tüm proje dosyalarını kaydedin.
4.  **Yapı** menüsünde **Yapı Çözümü**’ne tıklayın.
5.  **Hata Ayıklama** menüsünde **F11**’e basarak adım adım çözümü çalıştırın veya **F5**’e basarak çözümü çalıştırın.


## İlk Azure Storage uygulamanızı Azure Storage Öykünücüsüne karşı çalıştırma

[Azure Storage Öykünücüsü](storage-use-emulator.md) geliştirme amaçlı olarak Azure Blob, Kuyruk ve Tablo hizmetlerine öykünen yerel bir ortam sağlar. Bir Azure aboneliği veya depolama hesabı oluşturmadan ve herhangi bir masraf yapmadan depolama uygulamanızı yerel olarak test etmek üzere depolama öykünücüsünü kullanabilirsiniz.

Denemek için Visual Studio’da Azure Hızlı Başlangıç örnek projelerinden birini kullanarak basit bir Azure Storage uygulaması oluşturalım. Bu öğretici **Azure Blob Storage**, **Azure Table Storage** ve **Azure Kuyruk Depolama** örnek projelerine odaklanır:

1. Visual Studio’yu çalıştırın.
2. **Dosya** menüsünden **Yeni Proje**’ye tıklayın.
3. **Yeni Proje** iletişim kutusundan **Yüklenen** > **Şablonlar** > **Visual C#** > **Bulut** > **QuickStarts** > **Veri Hizmetleri**’ne tıklayın.
   a. Aşağıdaki şablonlardan birini seçin: **Azure Storage: Blob’lar**, **Azure Storage: Dosyalar**, **Azure Storage: Kuyruklar** veya **Azure Storage: Tablolar**.
   b. Hedef altyapı olarak **.NET Framework 4.5**’in seçildiğinden emin olun.   
    c. Aşağıda gösterildiği şekilde projeniz için bir ad belirleyin ve yeni Visual Studio çözümünü oluşturun:
    
    ![Azure Hızlı Başlangıçlar][Image1]

4.  Visual Studio’da **Görünüm** menüsünde **Çözüm Gezgini**’ni seçin. Zaten bir eklediyseniz App.config dosyası ve yorum, Azure Storage hesabı bağlantı dizesi kullanıma açar. Ardından bağlantı dizesi için Azure Storage öykünücüsü açıklamadan çıkarın:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Uygulamayı çalıştırmadan önce kaynak kodunu incelemek isteyebilirsiniz. Kodu incelemek için Visual Studio’da **Görüntüle** menüsünde **Çözüm Gezgini**’ni seçin. Ardından Program.cs dosyasına çift tıklayın. 

Ardından Azure Storage Öykünücüsünde örnek uygulamayı çalıştırın:

1.  **Başlat** düğmesine veya Windows tuşuna basın ve *Microsoft Azure Storage öykünücüsü*nü arayın ve uygulamayı başlatın. Öykünücü başlatıldığında Windows Görev Görünüm alanında bir simge ve bir bildirim göreceksiniz.
2.  Visual Studio’da **Yapı** menüsünde **Yapı Çözümü**’ne tıklayın. 
3.  **Hata Ayıklama** menüsünde **F11**’e basarak adım adım çözümü çalıştırın veya **F5**’e basarak çözümü başından sonuna kadar çalıştırın.

## Sonraki Adımlar

Azure Storage ile ilgili daha fazla bilgi edinmek için şu kaynaklara bakın:

* [Microsoft Azure Storage’a Giriş](storage-introduction.md)
* [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md)
* [.NET kullanarak Azure Table Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-tables.md)
* [.NET kullanarak Azure Kuyruk Depolamaya başlayın](storage-dotnet-how-to-use-queues.md)
* [.NET kullanarak Azure File Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-files.md)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure Storage Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
* [.NET için Microsoft Azure Storage İstemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
 



<!--HONumber=Jun16_HO2-->


