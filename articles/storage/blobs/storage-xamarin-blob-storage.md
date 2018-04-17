---
title: Xamarin nesne (Blob) depodan kullanma | Microsoft Docs
description: Xamarin için Azure Storage istemci kitaplığı, geliştiricilerin kendi yerel kullanıcı arabirimleri ile iOS, Android ve Windows mağazası uygulamaları oluşturmalarına olanak sağlar. Bu öğretici Xamarin Azure Blob Depolama kullanan bir uygulama oluşturmak için nasıl kullanılacağını gösterir.
services: storage
documentationcenter: xamarin
author: michaelhauss
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 31dbaeb1dd998d8d27af5eff0fa293117ef7f471
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-use-blob-storage-from-xamarin"></a>BLOB depolama alanından Xamarin kullanma

Yerel kullanıcı arabirimlerini ile iOS, Android ve Windows mağazası uygulamaları oluşturmak için bir paylaşılan C# kullanmaya Xamarin etkinleştirir geliştiriciler codebase. Bu öğretici Xamarin uygulamasıyla Azure Blob storage kullanmayı gösterir. Bilgi edinmek istiyorsanız, kodlara başlamadan önce Azure Storage hakkında daha fazla bakın [Microsoft Azure Storage'a giriş](../common/storage-introduction.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Yeni bir Xamarin uygulaması oluşturma
Bu öğreticide, Android, iOS ve Windows hedefleyen bir uygulama oluşturuluyor. Bu uygulama yalnızca bir kapsayıcı oluşturun ve bu kapsayıcıya bir blob yükleyin. Biz Visual Studio Windows kullanmaya, ancak aynı learnings üzerinde macOS Xamarin Studio kullanarak bir uygulama oluştururken uygulanabilir.

Uygulamanızı oluşturmak için aşağıdaki adımları izleyin:

1. Henüz yapmadıysanız yükleyip [Visual Studio için Xamarin](https://www.xamarin.com/download).
2. Visual Studio'yu açın ve (yerel taşınabilir) boş bir uygulaması oluşturun: **Dosya > Yeni > Proje > platformlar arası > boş App(Native Portable)**.
3. Çözüm Gezgini bölmesinde Çözümünüze sağ tıklayın ve seçin **çözüm için NuGet paketlerini Yönet**. Arama **WindowsAzure.Storage** ve çözümünüzdeki tüm projeleri için en son kararlı sürümü yükleyin.
4. Derleme ve projenizi çalıştırma.

Şimdi bir sayaç artırılır bir düğmeye tıklayın olanak sağlayan bir uygulama olmalıdır.

## <a name="create-container-and-upload-blob"></a>Kapsayıcı oluşturun ve blob karşıya yükleme
Ardından, altında `(Portable)` projesi için bazı kod ekleyeceğiz `MyClass.cs`. Bu kod, bir kapsayıcı oluşturur ve bu kapsayıcıya bir blob yükler. `MyClass.cs` aşağıdaki gibi görünmelidir:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference to a previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create the container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference to a blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create the "myblob" blob with the text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

"Your_account_name_here" ve "your_account_key_here" gerçek hesap adı ve hesap anahtarı ile değiştirdiğinizden emin olun. 

İOS, Android ve Windows Phone projeleri tüm, tüm paylaşılan kodunuzu birinde yazabilirsiniz anlamına gelir, taşınabilir projenizi - başvuran yerleştirin ve tüm projeleriniz arasında kullanın. Aşağıdaki kod satırını faydalanarak başlatmak için her proje için artık ekleyebilirsiniz: `MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used to configure the page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Bu gibi durumlarda, bu uygulama artık bir Android veya Windows Phone öykünücüsü çalıştırabilirsiniz. Bu uygulamayı bir iOS öykünücüsünde de çalıştırabilirsiniz, ancak bu bir Mac bilgisayar gerektirir Bunu yapmak özel yönergeler Lütfen okumak için belgelerine [Visual Studio bir Mac bilgisayara bağlayarak](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Uygulamanızı çalıştırdıktan sonra kapsayıcı oluşturacak `mycontainer` depolama hesabınızdaki. Blob içermelidir `myblob`, metin olan `Hello, world!`. Bunu kullanarak doğrulamak [Microsoft Azure Storage Gezgini](http://storageexplorer.com/).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Blob depolama alanındaki bir senaryo özel olarak odaklanan depolama alanını, Xamarin platformlar arası uygulama oluşturmak nasıl öğrendiniz. Ancak, yalnızca Blob Storage ile aynı zamanda tablo, dosya ve kuyruk depolama ile çok daha fazla yapabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere gözden geçirin:

* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-blobs.md)
* [Azure dosyaları giriş](../files/storage-files-introduction.md)
* [.NET ile Azure Dosyaları için geliştirme](../files/storage-dotnet-how-to-use-files.md)
* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](../queues/storage-dotnet-how-to-use-queues.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]