---
title: Visual Studio bağlı hizmeti - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'si için Visual Studio bağlı hizmeti özelliğini kullanarak bir ASP.NET Core web uygulamasından bağlanın.
services: cognitive-services
author: ghogen
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: ghogen
ms.custom: seodec18
ms.openlocfilehash: 148f94410f6acb421d352b68b6f1ecb305a6b16a
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235953"
---
# <a name="use-connected-services-in-visual-studio-to-connect-to-the-computer-vision-api"></a>Visual Studio'daki Bağlı Hizmetler özelliğini kullanarak Görüntü İşleme API'sine bağlanma

Bu makalede ve beraberindeki destek makalelerinde, Bilişsel Hizmetler Görüntü İşleme API'si için Visual Studio Bağlı Hizmet özelliğinin kullanımına ilişkin ayrıntılar sağlanmaktadır. Özellik, Bilişsel Hizmetler uzantısının yüklendiği Visual Studio 2017 15.7 ve sonraki sürümlerde mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- **Web Geliştirme** iş yükünün yüklendiği **Visual Studio 2017 sürüm 15.7**. [Şimdi indir](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-cognitive-services-computer-vision-api"></a>Projenize Bilişsel Hizmetler Görüntü İşleme API'si desteği ekleme

1. Yeni bir ASP.NET Core web projesi oluşturun. Boş proje şablonunu kullanın. 

1. **Çözüm Gezgini**’nde **Ekle** > **Bağlı Hizmet** seçeneklerini belirleyin.
   Projenize ekleyebileceğiniz hizmetlerle birlikte Bağlı Hizmet sayfası görüntülenir.

   ![Visual Studio Proje menüsünde sağ tıklayın: Ekle > bağlı hizmeti](../media/vs-common/Connected-Service-Menu.PNG)

1. Kullanılabilir hizmetler menüsünde **Bilişsel Hizmetler Görüntü İşleme API'sini** seçin.

   ![Bağlı hizmetler menüsü: Görüntüleri analiz edin... ana hatlarıyla açıklanmıştır](./media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-0.PNG)

   Visual Studio’da oturum açtıysanız ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, aboneliklerinizi içeren bir açılır listenin yer aldığı bir sayfa görüntülenir.

   ![Bilgisayar işleme API'si penceresiyle vurgulanmış abonelik açılır](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-1.PNG)

1. Kullanmak istediğiniz aboneliği seçin ve sonra Görüntü İşleme API'si için bir ad seçin veya otomatik olarak oluşturulan adı değiştirmek için Düzenle bağlantısını seçin, kaynak grubunu ve Fiyatlandırma Katmanını seçin.

   ![Bağlı hizmet ayrıntılarını düzenleme](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-2.PNG)

   Fiyatlandırma katmanları ile ilgili ayrıntılar için bağlantıyı izleyin.

1. Ekle’yi seçerek, Bağlı Hizmet için destek ekleyin.
   Visual Studio; Görüntü İşleme API'si bağlantısını desteklemek üzere NuGet paketlerini, yapılandırma dosyası girdilerini ve diğer değişiklikleri eklemek için projenizi değiştirir. Çıkış Penceresi’nde projenizde olup bitenlerin günlüğü gösterilir. Aşağıdakine benzer bir şey görmeniz gerekir:

   ```output
   [4/26/2018 5:15:31.664 PM] Adding Computer Vision API to the project.
   [4/26/2018 5:15:32.084 PM] Creating new ComputerVision...
   [4/26/2018 5:15:32.153 PM] Creating new Resource Group...
   [4/26/2018 5:15:40.286 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' version 2.1.0.
   [4/26/2018 5:15:44.117 PM] Retrieving keys...
   [4/26/2018 5:15:45.602 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceKey=<service key>
   [4/26/2018 5:15:45.606 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceEndPoint=https://australiaeast.api.cognitive.microsoft.com/vision/v2.0
   [4/26/2018 5:15:45.609 PM] Changing appsettings.json setting: ComputerVisionAPI_Name=WebApplication-Core-ComputerVision_ComputerVisionAPI
   [4/26/2018 5:15:46.747 PM] Successfully added Computer Vision API to the project.
   ```
 
## <a name="use-the-computer-vision-api-to-detect-attributes-of-an-image"></a>Bir görüntüdeki öznitelikleri algılamak için Görüntü İşleme API'sini kullanma

1. Startup.cs’de deyimleri kullanarak aşağıdakileri ekleyin.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Bir yapılandırma alanı ekleyin ve `Startup` sınıfında yapılandırma alanını başlatan bir oluşturucu ekleyerek programınızda yapılandırmayı etkinleştirin.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Projenizdeki wwwroot klasörüne bir görüntüler klasörü ekleyin ve wwwroot klasörünüz için bir görüntü dosyası ekleyin. Örneğin, bu [Görüntü İşleme API'si sayfasındaki](https://azure.microsoft.com/services/cognitive-services/computer-vision/) görüntülerden birini kullanabilirsiniz. Görüntülerden birini sağ tıklayın, Çözüm Gezgini'nde sonra yerel sabit sürücünüze kaydedin, görüntüleri klasörü sağ tıklatın ve seçin **Ekle** > **var olan öğe** projenize eklemek için. Projeniz Çözüm Gezgini’nde aşağıdakine benzer şekilde görünmelidir: 
  
   ![Çözüm Gezgini görünümü seçili bir resim dosyası ile ekran görüntüsü](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-3.PNG) 

1. Görüntü dosyasına sağ tıklayın, Özellikler’i seçin ve ardından **Yeniyse kopyala** seçeneğini belirleyin. 

   ![Görüntü Özellikleri penceresinde; Yeniyse Kopyala ayarlamak çıkış dizinine Kopyala](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-5.PNG) 
 
1. Görüntü İşleme API'sine erişmek ve bir görüntüyü test etmek için aşağıdaki kod ile Yapılandırma yöntemini değiştirin.

   ```csharp
    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        // TODO: Change this to your image's path on your site. 
        string imagePath = @"images/subway.png";

        // Enable static files such as image files. 
        app.UseStaticFiles();

        string visionApiKey = this.configuration["ComputerVisionAPI_ServiceKey"];
        string visionApiEndPoint = this.configuration["ComputerVisionAPI_ServiceEndPoint"];

        HttpClient client = new HttpClient();

        // Request headers.
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", visionApiKey);

        // Request parameters. A third optional parameter is "details".
        string requestParameters = "visualFeatures=Categories,Description,Color&language=en";

        // Assemble the URI for the REST API Call.
        string uri = visionApiEndPoint + "/analyze" + "?" + requestParameters;

        HttpResponseMessage response;

        // Request body. Posts an image you've added to your site's images folder. 
        var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
        byte[] byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

        string contentString = string.Empty;
        using (ByteArrayContent content = new ByteArrayContent(byteData))
        {
            // This example uses content type "application/octet-stream".
            // The other content types you can use are "application/json" and "multipart/form-data".
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

            // Execute the REST API call.
            response = client.PostAsync(uri, content).Result;

            // Get the JSON response.
            contentString = response.Content.ReadAsStringAsync().Result;
        }

        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.Run(async (context) =>
        {
            await context.Response.WriteAsync("<h1>Cognitive Services Demo</h1>");
            await context.Response.WriteAsync($"<p><b>Test Image:</b></p>");
            await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
            await context.Response.WriteAsync($"<p><b>Computer Vision API results:</b></p>");
            await context.Response.WriteAsync("<p>");
            await context.Response.WriteAsync(JsonPrettyPrint(contentString));
            await context.Response.WriteAsync("<p>");
        });
    }
   ```

    Buradaki kod Görüntü İşleme API'sine bir çağrı göndermek için URI'ye ve ikili içerik olarak görüntüye sahip olan bir HTTP isteği oluşturur.

1. GetImageAsByteArray ve JsonPrettyPrint yardımcı işlevlerini ekleyin.

   ```csharp
    /// <summary>
    /// Returns the contents of the specified file as a byte array.
    /// </summary>
    /// <param name="imageFilePath">The image file to read.</param>
    /// <returns>The byte array of the image data.</returns>
    static byte[] GetImageAsByteArray(string imageFilePath)
    {
        FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
        BinaryReader binaryReader = new BinaryReader(fileStream);
        return binaryReader.ReadBytes((int)fileStream.Length);
    }

    /// <summary>
    /// Formats the given JSON string by adding line breaks and indents.
    /// </summary>
    /// <param name="json">The raw JSON string to format.</param>
    /// <returns>The formatted JSON string.</returns>
    static string JsonPrettyPrint(string json)
    {
        if (string.IsNullOrEmpty(json))
            return string.Empty;

        json = json.Replace(Environment.NewLine, "").Replace("\t", "");

        string INDENT_STRING = "    ";
        var indent = 0;
        var quoted = false;
        var sb = new StringBuilder();
        for (var i = 0; i < json.Length; i++)
        {
            var ch = json[i];
            switch (ch)
            {
                case '{':
                case '[':
                    sb.Append(ch);
                    if (!quoted)
                    {
                        sb.AppendLine();
                    }
                    break;
                case '}':
                case ']':
                    if (!quoted)
                    {
                        sb.AppendLine();
                    }
                    sb.Append(ch);
                    break;
                case '"':
                    sb.Append(ch);
                    bool escaped = false;
                    var index = i;
                    while (index > 0 && json[--index] == '\\')
                        escaped = !escaped;
                    if (!escaped)
                        quoted = !quoted;
                    break;
                case ',':
                    sb.Append(ch);
                    if (!quoted)
                    {
                        sb.AppendLine();
                    }
                    break;
                case ':':
                    sb.Append(ch);
                    if (!quoted)
                        sb.Append(" ");
                    break;
                default:
                    sb.Append(ch);
                    break;
            }
        }
        return sb.ToString();
    }
   ```

1. Web uygulamasını çalıştırın ve Görüntü İşleme API'sinin görüntünüzde ne bulduğunu görün.

   ![Görüntü İşleme API'si görüntü ve biçimlendirilmiş sonuçlar](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-4.PNG)  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubunu silin. Böylece bilişsel hizmet ve ilgili kaynaklar silinir. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü işleme API'si hakkında daha fazla bilgi edinmek [görüntü işleme API'si belgeleri](Home.md).
