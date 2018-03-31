---
title: Azure Mobile Engagement hizmet API'leri erişmek için .NET SDK kullanarak
description: Mobile Engagement .NET SDK'sı Azure Mobile Engagement hizmet API'leri erişmek için nasıl kullanılacağını açıklar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: c07728aa-43f2-4238-8b4a-c9eddf9d838b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 99595eb1f9a1eab1db51796632d58df35bf45be6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Azure Mobile Engagement hizmet API'leri erişmek için .NET SDK kullanarak
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Azure Mobile Engagement kullanıma sunan bir dizi API aygıtları yönetebilmeniz için Reach/anında iletme kampanyalarını vb. Bu API'leri ile etkileşim kurmak için de size sağladığımız bir [Swagger dosyası](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) için tercih ettiğiniz dili SDK'ları oluşturmak için Araçlar ile kullanabilirsiniz. Kullanmanızı öneririz [AutoRest](https://github.com/Azure/AutoRest) aracı, SDK bizim Swagger dosyasından oluşturun.

> [!NOTE]
> Azure Mobile Engagement hizmeti, Mart 2018’de devre dışı bırakılacaktır. Şu anda yalnızca mevcut müşteriler tarafından kullanılabilmektedir. Daha fazla bilgi için bkz. [Mobile Engagement nedir?](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Bu API'leri ile etkileşime olanak tanıyan bir C# sarmalayıcı kullanarak benzer şekilde bir .NET SDK'sı oluşturduk ve kimlik doğrulama belirteci anlaşma yapın ve kendiniz yenilemek yok.  

Bu örnek, .NET SDK'yı kullanmak için izlemeniz gereken adımlar kümesi üzerinden geçer:

1. Öncelikle, kimlik doğrulaması açıklandığı gibi Azure Active Directory'yi kullanarak, API için Kurulum gerekir [burada](mobile-engagement-api-authentication.md#authentication). Bu adımları sonunda, geçerli bir olmalıdır **Subscriptionıd**, **Tenantıd**, **ApplicationId** ve **gizli**. 
2. .NET SDK'sı bir duyuru kampanya oluşturma senaryo ile çalışma göstermek için basit bir Windows konsol uygulaması kullanacağız. Bu nedenle Visual Studio'yu açın ve oluşturma bir **konsol uygulaması**.   
3. Olarak kullanılabilir olan .NET SDK'yı yüklemek gereken sonraki **Microsoft Azure katılım Yönetimi Kitaplığı** Nuget galerisinde [burada](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
   Nuget Visual Studio'dan yüklüyorsanız, onay işaretli olduğundan emin olmak gereken **INCLUDE yayın öncesi** paketi için arama seçeneği:
   
    ![][1]
4. İçinde `Program.cs` dosyasında, şu ad alanlarından ekleyin:
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. Sonraki kimlik doğrulaması ve Mobile Engagement uygulaması duyuru kampanya oluşturmakta olduğunuz etkileşim kullanacağız aşağıdaki sabitleri tanımlamanız gerekir:
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";
6. Tanımlamak `EngagementManagementClient` Mobile Engagement SDK'sı yöntemleri çağırmak için kullanacağı değişkeni:
   
        static EngagementManagementClient engagementClient; 
7. Aşağıdakileri ekleyin, `Main` yöntemi:
   
        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. Başlatma mvc'deki aşağıdaki yöntemi tanımlayın `EngagementManagementClient` ilk kimlik doğrulaması ve kendisini içinde planladığınız duyuru kampanya oluşturmak Mobile Engagement uygulamayla ilişkilendirme:
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > Kullanmanız gereken Not **uygulama kaynak adı** AppName parametresi için Azure Yönetim Portalı'nda tanımlanan. 
   > 
   > 
9. Son olarak, hangi ele basit bir oluşturmak için daha önce başlatılmış EngagementClient kullanmanın önemli CreateCampaign yöntemi tanımlayın **dilediğiniz zaman** & **yalnızca bildirim** bir başlık ve ileti kampanyayla: 
   
        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. Konsol uygulamasını çalıştırın ve aşağıdaki kampanya başarılı oluşturulmasını görürsünüz:
    
    **Kampanya Kimliği '159' başarıyla oluşturuldu ve 'Taslak' durumda**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
