---
title: Xamarin nesne (Blob) depolama kullanma nasıl | Microsoft Docs
description: Xamarin için Azure depolama istemci kitaplığı, geliştiricilerin kendi yerel kullanıcı arabirimleri ile iOS, Android ve Windows Store apps oluşturmak sağlar. Bu öğreticide, Azure Blob Depolama kullanan bir uygulama oluşturmak için Xamarin kullanmayı gösterir.
services: storage
documentationcenter: xamarin
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: b9c707dcc1628f685661f88aaed29612465a5469
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098091"
---
# <a name="how-to-use-blob-storage-from-xamarin"></a>BLOB depolama alanından Xamarin kullanma

İOS, Android ve Windows Store apps ile kendi yerel kullanıcı arabirimleri oluşturmak için kod tabanı Xamarin etkinleştirir geliştiricilerin paylaşılan C# kullanın. Bu öğreticide bir Xamarin uygulaması ile Azure Blob Depolama kullanma gösterilmektedir. Kodlara başlamadan önce Azure depolama hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Microsoft Azure Storage'a giriş](../common/storage-introduction.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Yeni bir Xamarin uygulaması oluşturun
Bu öğreticide, biz Android, iOS ve Windows hedefleyen bir uygulama oluşturursunuz. Bu uygulama yalnızca bir kapsayıcı oluşturun ve bu kapsayıcıya bir blob karşıya yükleyin. Visual Studio Windows üzerinde kullanacağız, ancak aynı dersleri Xamarin Studio kullanarak Macos'ta bir uygulama oluştururken uygulanabilir.

Uygulamanızı oluşturmak için aşağıdaki adımları izleyin:

1. Henüz yapmadıysanız, indirme ve yükleme [Visual Studio için Xamarin](https://www.xamarin.com/download).
2. Visual Studio'yu açın ve bir boş uygulama (yerel taşınabilir) oluşturun: **Dosya > Yeni > Proje > platformlar arası > boş uygulama (yerel taşınabilir)**.
3. Çözüm Gezgini bölmesinde, çözümü sağ tıklayıp **çözüm için NuGet paketlerini Yönet**. Arama **WindowsAzure.Storage** ve çözümünüzdeki tüm projeler için en son kararlı sürümünü yükleyin.
4. Derleme ve projeyi çalıştırın.

Şimdi, bir sayaç artırılır bir düğmeye tıklayın olanak tanıyan bir uygulama olmalıdır.

## <a name="create-container-and-upload-blob"></a>Kapsayıcı oluşturma ve blob karşıya yükleme
Ardından, altında `(Portable)` projesi için kod ekleme `MyClass.cs`. Bu kod, bir kapsayıcı oluşturur ve bu kapsayıcıya bir blob yükler. `MyClass.cs` aşağıdaki gibi görünmelidir:

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

"Your_account_name_here" ve "your_account_key_here" gerçek hesap adınızı ve hesap anahtarı ile değiştirdiğinizden emin olun. 

İOS, Android ve Windows Phone projeleri tüm paylaşılan kodunuzun tamamını birinde yazabileceğiniz anlamına gelir, taşınabilir proje - başvurmuş yerleştirin ve tüm projeleriniz kullanın. Artık her proje için yararlanmaya başlamak için aşağıdaki kod satırını ekleyebilirsiniz: `MyClass.performBlobOperation()`

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
                // Release any cached data, images, etc. that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=391641

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
Bir Android veya Windows Phone öykünücüsü'nde artık bu uygulamayı çalıştırabilirsiniz. Bu uygulamayı bir iOS öykünücüde de çalıştırabilirsiniz, ancak bu bir Mac bilgisayar gerektirir Bunu yapmak özel yönergeler Lütfen okumak için belgelerine [Visual Studio'yu bir Mac bilgisayara bağlayarak](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Uygulamanızı çalıştırdıktan sonra kapsayıcı oluşturacak `mycontainer` depolama hesabınızda. Blob içermelidir `myblob`, metin olduğu `Hello, world!`. Kullanarak bunu doğrulayabilirsiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/).

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure depolama, Blob depolama alanındaki bir senaryo özel olarak odaklanan kullanan Xamarin platformlar arası uygulama oluşturma öğrendiniz. Ancak, çok fazla değil yalnızca Blob Depolama ile ancak tablo, dosya ve kuyruk depolama ile yapabilirsiniz. Lütfen daha fazla bilgi için aşağıdaki makalelere göz atın:

* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](storage-dotnet-how-to-use-blobs.md)
* [Azure dosyaları'na giriş](../files/storage-files-introduction.md)
* [.NET ile Azure Dosyaları için geliştirme](../files/storage-dotnet-how-to-use-files.md)
* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](../queues/storage-dotnet-how-to-use-queues.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]