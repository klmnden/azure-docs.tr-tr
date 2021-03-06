---
title: 'Hızlı Başlangıç: Bir geri bildirim döngüsü - Personalizer oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu içerik kişiselleştirme C# Personalizer Service hızlı başlangıç.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 06/11/2019
ms.author: edjez
ms.openlocfilehash: 0b856b8d134cc160b8bb759fce0408204cf0ba61
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722445"
---
# <a name="quickstart-personalize-content-using-c"></a>Hızlı Başlangıç: İçerik kullanarak kişiselleştirmeC# 

Bu kişiselleştirilmiş içerik görüntüleme C# Personalizer Service hızlı başlangıç.

Bu örnek için Personalizer istemci kitaplığının nasıl kullanılacağını gösterir C# aşağıdaki eylemleri gerçekleştirmek için: 

 * Kişiselleştirme için eylemlerin bir listesini Sırala.
 * Belirtilen olay için kullanıcı seçimine dayalı olarak eylem sıralanmış üst ayırmak için ödül bildirin.

Personalizer ile çalışmaya başlama, aşağıdaki adımları içerir:

1. SDK'ya başvurma 
1. Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod yazma,
1. Döngü eğitmek için ödül göndermek için kod yazma.

## <a name="prerequisites"></a>Önkoşullar

* Gereksinim duyduğunuz bir [Personalizer hizmet](how-to-settings.md) abonelik anahtarını ve uç noktasını hizmet URL'sini alabilirsiniz. 
* [Visual Studio 2015 veya 2017](https://visualstudio.microsoft.com/downloads/).
* [Microsoft.Azure.CognitiveServices.Personalizer](https://go.microsoft.com/fwlink/?linkid=2092272) SDK'sı NuGet paketi. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="change-the-model-update-frequency"></a>Model güncelleştirme sıklığını değiştirme

Azure portalında Personalizer kaynakta değişiklik **modeli güncelleştirme sıklığını** 10 saniyedir. Bu hizmet hızlı bir şekilde, üst eylem her yineleme için nasıl değiştiğini görmek sağlayarak eğitme.

Personalizer döngü ilk örneği olduğunda hiçbir model olmuştur beri gelen eğitmek için herhangi bir ödül API çağrısı. Derece çağrıları eşit olasılıklar her öğe için döndürür. Uygulamanızı yine de her zaman RewardActionId çıktısını kullanarak içerik rank.

![Model güncelleştirme sıklığını değiştirme](./media/settings/configure-model-update-frequency-settings.png)

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Yeni bir konsol uygulaması oluşturma ve Personalizer SDK'ya başvurma 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Personalizer istemci kitaplığı NuGet paketini yükleyin. Menüsünde **Araçları**seçin **Nuget Paket Yöneticisi**, ardından **çözüm için NuGet paketlerini Yönet**.
1. Denetleme **ön sürümü dahil et**.
1. Seçin **Gözat** sekmesinde ve **arama** kutusuna `Microsoft.Azure.CognitiveServices.Personalizer`.
1. Seçin **Microsoft.Azure.CognitiveServices.Personalizer** zaman görüntüler.
1. Projenizin adına yanındaki onay kutusunu seçip **yükleme**.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Kod ekleyip Personalizer ve Azure anahtarlarınızı yerleştirin

1. Program.cs içeriğini şu kodla değiştirin. 
1. Değiştirin `serviceKey` geçerli Personalizer abonelik anahtarınız ile değeri.
1. Değiştirin `serviceEndpoint` , hizmet uç noktası ile. `https://westus2.api.cognitive.microsoft.com/` bunun bir örneğidir.
1. Programı çalıştırın.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod ekleyin

Aşağıdaki C# kodudur kullanıcı bilgileri, _features ve içeriğinizi hakkında bilgi iletmek için yapılandırılabilip _eylemleri_, SDK'sını kullanarak Personalizer için. Eylem, kullanıcıya göstermek için sıralanmış üst personalizer döndürür.  

```csharp
using Microsoft.Azure.CognitiveServices.Personalizer;
using Microsoft.Azure.CognitiveServices.Personalizer.Models;
using System;
using System.Collections.Generic;
using System.Linq;

namespace PersonalizerExample
{
    class Program
    {
        // The key specific to your personalizer service instance; e.g. "0123456789abcdef0123456789ABCDEF"
        private const string ApiKey = "";

        // The endpoint specific to your personalizer service instance; e.g. https://westus2.api.cognitive.microsoft.com/
        private const string ServiceEndpoint = "";

        static void Main(string[] args)
        {
            int iteration = 1;
            bool runLoop = true;

            // Get the actions list to choose from personalizer with their features.
            IList<RankableAction> actions = GetActions();

            // Initialize Personalizer client.
            PersonalizerClient client = InitializePersonalizerClient(ServiceEndpoint);

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

                // Exclude an action for personalizer ranking. This action will be held at its current position.
                IList<string> excludeActions = new List<string> { "juice" };

                // Generate an ID to associate with the request.
                string eventId = Guid.NewGuid().ToString();

                // Rank the actions
                var request = new RankRequest(actions, currentContext, excludeActions, eventId);
                RankResponse response = client.Rank(request);

                Console.WriteLine("\nPersonalizer service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

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

                Console.WriteLine("\nPersonalizer service ranked the actions with the probabilities as below:");
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
        /// Initializes the personalizer client.
        /// </summary>
        /// <param name="url">Azure endpoint</param>
        /// <returns>Personalizer client instance</returns>
        static PersonalizerClient InitializePersonalizerClient(string url)
        {
            PersonalizerClient client = new PersonalizerClient(
                new ApiKeyServiceClientCredentials(ApiKey)) {Endpoint = url};

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
        /// Creates personalizer actions feature list.
        /// </summary>
        /// <returns>List of actions for personalizer.</returns>
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


