---
title: Bilgisayar işleme C# Öğreticisi | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler görüntü işleme için bir ASP.NET Core web uygulamasından bağlanın.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 76ca1215144a5caa40971e1eda23f6462f7bf27b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665258"
---
# <a name="connecting-to-cognitive-services-computer-vision-api-by-using-connected-services-in-visual-studio"></a>Bilişsel hizmetler görüntü işleme API'si, Visual Studio bağlı Hizmetler'i kullanarak bağlanma

Bilişsel hizmetler görüntü işleme API'sini kullanarak, görsel verilerin işlenmesi ve kategorilere ayırma için zengin bilgiler ayıklayın ve görüntüleri hizmetlerinizi oluşturmanıza yardımcı olması için makine Yardımlı resim denetimi gerçekleştirin.

Bu makalede ve yardımcı makaleleri için Bilişsel hizmetler görüntü işleme API'si Visual Studio bağlı hizmeti özelliğini kullanarak için ayrıntıları sağlayın. Özellik, her iki Visual Studio 2017 15.7 veya sonraki sürümlerinde, Bilişsel hizmetler uzantısı yüklü yöneliktir.

## <a name="prerequisites"></a>Önkoşullar

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- **Visual Studio 2017 sürüm 15.7** ile **Web geliştirme** iş yükü yüklenmiş. [Hemen indirin](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-cognitive-services-computer-vision-api"></a>Bilişsel hizmetler görüntü işleme API'si için projenize desteği eklendi

1. Yeni bir ASP.NET Core web projesi oluşturun. Boş proje şablonu kullanın. 

1. İçinde **Çözüm Gezgini**, seçin **Ekle** > **bağlı hizmet**.
   Projenize eklediğiniz Hizmetleri ile bağlı hizmet sayfasında görünür.

   ![Bağlı hizmet menü öğesi ekleme](../media/vs-common/Connected-Service-Menu.PNG)

1. Kullanılabilir hizmetler menüsünde **Bilişsel hizmetler görüntü işleme API'si**.

   ![Bağlanmak için bir hizmet seçin](./media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-0.PNG)

   Visual Studio'da oturum açıldıktan ve hesabınızla ilişkili bir Azure aboneliğiniz varsa, aboneliklerinizin ile bir açılan listedeki bir sayfa görünür.

   ![Aboneliğinizi seçme](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-1.PNG)

1. Görüntü işleme API'si için bir ad seçin ve istediğiniz aboneliği seçin veya otomatik olarak oluşturulan adı değiştirmek, kaynak grubunu ve fiyatlandırma katmanı seçin düzenleme bağlantısını seçin.

   ![Bağlı hizmet ayrıntılarını Düzenle](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-2.PNG)

   Fiyatlandırma katmanları hakkında ayrıntıları bağlantısını izleyin.

1. Desteklenen için bağlı hizmet eklemek için Ekle'yi seçin.
   Visual Studio projenizin NuGet paketlerini, yapılandırma dosyası girdisi ve bir bağlantı görüntü işleme API'sini destekleyen diğer değişiklikler eklemek için değiştirir. Çıkış penceresinde günlük projenize olup bitenleri gösterir. Aşağıdaki gibi görmeniz gerekir:

   ```output
   [4/26/2018 5:15:31.664 PM] Adding Computer Vision API to the project.
   [4/26/2018 5:15:32.084 PM] Creating new ComputerVision...
   [4/26/2018 5:15:32.153 PM] Creating new Resource Group...
   [4/26/2018 5:15:40.286 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' version 1.0.2-preview.
   [4/26/2018 5:15:44.117 PM] Retrieving keys...
   [4/26/2018 5:15:45.602 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceKey=<service key>
   [4/26/2018 5:15:45.606 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceEndPoint=https://australiaeast.api.cognitive.microsoft.com/vision/v2.0
   [4/26/2018 5:15:45.609 PM] Changing appsettings.json setting: ComputerVisionAPI_Name=WebApplication-Core-ComputerVision_ComputerVisionAPI
   [4/26/2018 5:15:46.747 PM] Successfully added Computer Vision API to the project.
   ```
 
## <a name="use-the-computer-vision-api-to-detect-attributes-of-an-image"></a>Görüntü öznitelikleri algılamak için görüntü işleme API'sini kullanın.

1. Aşağıdaki using deyimlerini Startup.cs içinde.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Yapılandırma alan ekleme ve yapılandırma alanı başlatan bir oluşturucu ekleyin `Startup` sınıfı programınızda yapılandırmasını etkinleştirmek için.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Wwwroot klasörü, projenizdeki bir görüntü klasörü Ekle ve, wwwroot klasörü için bir resim dosyası ekleyin. Örneğin, görüntülerden birini bu kullanabileceğiniz [görüntü işleme API'si sayfa](https://azure.microsoft.com/services/cognitive-services/computer-vision/). Görüntülerden birini sağ tıklayın, kaydetme, yerel sabit diske ve ardından Çözüm Gezgini'nde, görüntü klasörü, choosee sağ tıklayıp **Ekle** > **var olan öğe** projenize eklemek için. Çözüm Gezgini'nde projenize aşağıdakine benzer görünmelidir: 
  
   ![Resimler klasöründeki ile görüntü dosyası](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-3.PNG) 

1. Görüntü dosyasını sağ tıklayın, Özellikler'i seçin ve ardından **yeniyse Kopyala**. 

   ![Yeniyse Kopyala](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-5.PNG) 
 
1. Yapılandırma yöntemi, görüntü işleme API'si erişim ve görüntü test etmek için aşağıdaki kod ile değiştirin.

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
    Kodu buraya, görüntü işleme REST API'si çağrısı için ikili içerik olarak URI ve görüntü ile bir HTTP isteği oluşturur.

1. Yardımcı işlevleri GetImageAsByteArray ve JsonPrettyPrint ekleyin.

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

1. Web uygulamasını çalıştırmak ve görüntünüzü görüntü işleme API'si bulduğu bakın.

   ![Bilgisayar işleme API'si görüntü ve biçimlendirilmiş sonuç](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-4.PNG)  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu silin. Bu bilişsel hizmet ve ilişkili kaynakları siler. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu Hızlı Başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü işleme API'si hakkında daha fazla bilgi edinmek [görüntü işleme API'si belgeleri](Home.md).
