---
title: Geri bildirim döngüsü - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu içerik kişiselleştirme C# Personalizer Service hızlı başlangıç.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: f0aca3e387d675064cf798b4efdeb66cfe906520
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153542"
---
# <a name="quickstart-personalize-content-using-c"></a>Hızlı Başlangıç: İçerik kullanarak kişiselleştirmeC# 

Bu kişiselleştirilmiş içerik görüntüleme C# Personalizer Service hızlı başlangıç.

Bu örnek için kişiselleştirme istemci kitaplığının nasıl kullanılacağını gösterir C# aşağıdaki eylemleri gerçekleştirmek için: 

 * Kişiselleştirme için eylemlerin bir listesini Sırala.
 * Belirtilen olay için kullanıcı seçimine dayalı olarak eylem sıralanmış üst ayırmak için ödül bildirin.

Kişiselleştirme ile çalışmaya başlama, aşağıdaki adımları içerir:

1. SDK'ya başvurma 
1. Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod yazma,
1. Döngü eğitmek için ödül göndermek için kod yazma.

## <a name="prerequisites"></a>Önkoşullar

* Gereksinim duyduğunuz bir [Personalizer hizmet](how-to-settings.md) aboneliğinizi almak için anahtar ve belirteci veren url hizmeti. 
* [Visual Studio 2015 veya 2017](https://visualstudio.microsoft.com/downloads/).
* Microsoft.Azure.CognitiveServices.Personalization SDK'sı NuGet paketi. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Yeni bir konsol uygulaması oluşturma ve Personalizer SDK'ya başvurma 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Kişiselleştirme istemci kitaplığı NuGet paketini yükleyin. Menüsünde **Araçları**seçin **Nuget Paket Yöneticisi**, ardından **çözüm için NuGet paketlerini Yönet**.
1. Seçin **Gözat** sekmesinde ve **arama** kutusuna `Microsoft.Azure.CognitiveServices.Personalization`.
1. Seçin **Microsoft.Azure.CognitiveServices.Personalization** zaman görüntüler.
1. Projenizin adına yanındaki onay kutusunu seçip **yükleme**.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Kod ekleyip Personalizer ve Azure anahtarlarınızı yerleştirin

1. Program.cs içeriğini şu kodla değiştirin. 
1. Değiştirin `serviceKey` geçerli Personalizer abonelik anahtarınız ile değeri.
1. Değiştirin `serviceEndpoint` , hizmet uç noktası ile. `https://westus2.api.cognitive.microsoft.com/` bunun bir örneğidir.
1. Programı çalıştırın.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod ekleyin

Aşağıdaki C# kodudur kullanıcı bilgileri, _features ve içeriğinizi hakkında bilgi iletmek için yapılandırılabilip _eylemleri_, SDK'sını kullanarak Personalizer için. Eylem, kullanıcıya göstermek için sıralanmış üst personalizer döndürür.  

```csharp
using Microsoft.Azure.CognitiveServices.Personalization;
using Microsoft.Azure.CognitiveServices.Personalization.Models;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;

namespace PersonalizationExample
{
    class Program
    {
        // The key specific to your personalization service instance; e.g. "0123456789abcdef0123456789ABCDEF"
        private const string serviceKey = "";

        // The endpoint specific to your personalization service instance; e.g. https://westus2.api.cognitive.microsoft.com/
        private const string serviceEndpoint = "";

        static void Main(string[] args)
        {
            int iteration = 1;
            bool runLoop = true;

            Uri url = new Uri(serviceEndpoint);

            // Get the actions list to choose from personalization with their features.
            IList<RankableAction> actions = GetActions();

            // Initialize Personalization client.
            PersonalizationClient client = InitializePersonalizationClient(url);

            do
            {
                Console.WriteLine("\nIteration: " + iteration++);

                // Get context information from the user.
                string timeOfDayFeature = GetUsersTimeOfDay();
                string tasteFeature = GetUsersTastePreference();

                // Create current context from user specified data.
                IList<object> currentContext = new List<object>() {
                    new { time = timeOfDayFeature },
                    new { taste = tasteFeature }
                };

                // Exclude an action for personalization ranking. This action will be held at its current position.
                IList<string> excludeActions = new List<string> { "juice" };

                // Generate an ID to associate with the request.
                string eventId = Guid.NewGuid().ToString();

                // Rank the actions
                var request = new RankRequest(actions, currentContext, excludeActions, eventId);
                RankResponse response = client.Rank(request);

                Console.WriteLine("\nPersonalization service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

                float reward = 0.0f;
                string answer = GetKey();

                if (answer == "Y")
                {
                    reward = 1;
                    Console.WriteLine("\nGreat! Enjoy your food.");
                }
                else if (answer == "N")
                {
                    reward = 0;
                    Console.WriteLine("\nYou didn't like the recommended food choice.");
                }
                else
                {
                    Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended food choice.");
                }

                Console.WriteLine("\nPersonalization service ranked the actions with the probabilities as below:");
                foreach (var rankedResponse in response.Ranking)
                {
                    Console.WriteLine(rankedResponse.Id + " " + rankedResponse.Probability);
                }

                // Send the reward for the action based on user response.
                client.Reward(response.EventId, new RewardRequest(reward));

                Console.WriteLine("\nPress q to break, any other key to continue:");
                runLoop = !(GetKey() == "Q");

            } while (runLoop);
        }

        /// <summary>
        /// Initializes the personalization client.
        /// </summary>
        /// <param name="url">Azure endpoint</param>
        /// <returns>Personalization client instance</returns>
        static PersonalizationClient InitializePersonalizationClient(Uri url)
        {
            PersonalizationClient client = new PersonalizationClient(url,
            new ApiKeyServiceClientCredentials(serviceKey),
            new DelegatingHandler[] { });

            return client;
        }

        /// <summary>
        /// Get users time of the day context.
        /// </summary>
        /// <returns>Time of day feature selected by the user.</returns>
        static string GetUsersTimeOfDay()
        {
            string[] timeOfDayFeatures = new string[] { "morning", "afternoon", "evening", "night" };

            Console.WriteLine("\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night");
            if (!int.TryParse(GetKey(), out int timeIndex) || timeIndex < 1 || timeIndex > timeOfDayFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + timeOfDayFeatures[0] + ".");
                timeIndex = 1;
            }

            return timeOfDayFeatures[timeIndex - 1];
        }

        /// <summary>
        /// Gets user food preference.
        /// </summary>
        /// <returns>Food taste feature selected by the user.</returns>
        static string GetUsersTastePreference()
        {
            string[] tasteFeatures = new string[] { "salty", "sweet" };

            Console.WriteLine("\nWhat type of food would you prefer (enter number)? 1. salty 2. sweet");
            if (!int.TryParse(GetKey(), out int tasteIndex) || tasteIndex < 1 || tasteIndex > tasteFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + tasteFeatures[0] + ".");
                tasteIndex = 1;
            }

            return tasteFeatures[tasteIndex - 1];
        }

        /// <summary>
        /// Creates personalization actions feature list.
        /// </summary>
        /// <returns>List of actions for personalization.</returns>
        static IList<RankableAction> GetActions()
        {
            IList<RankableAction> actions = new List<RankableAction>
            {
                new RankableAction
                {
                    Id = "pasta",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "medium" }, new { nutritionLevel = 5, cuisine = "italian" } }
                },

                new RankableAction
                {
                    Id = "ice cream",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionalLevel = 2 } }
                },

                new RankableAction
                {
                    Id = "juice",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionLevel = 5 }, new { drink = true } }
                },

                new RankableAction
                {
                    Id = "salad",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "low" }, new { nutritionLevel = 8 } }
                }
            };

            return actions;
        }

        private static string GetKey()
        {
            return Console.ReadKey().Key.ToString().Last().ToString().ToUpper();
        }
    }
}
```

## <a name="run-the-program"></a>Programı çalıştırma

Programı derleyin ve çalıştırın. Hızlı Başlangıç programı birkaç soru özellikleri bilinen kullanıcı tercihleri toplamak için ardından üst eylem sağlar ister.

![Hızlı Başlangıç programı birkaç soru özellikleri bilinen kullanıcı tercihleri toplamak için ardından üst eylem sağlar ister.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

[Personalizer nasıl çalışır?](how-personalizer-works.md)


