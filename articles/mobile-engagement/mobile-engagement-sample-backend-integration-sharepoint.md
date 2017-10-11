---
title: "Azure Mobile Engagement - arka uç tümleştirme"
description: "SharePoint'ten kampanyalar oluşturmak için SharePoint arka ucu ile Azure Mobile Engagement Bağlan"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d49f1094f4c3f170f3618f3e19e42266f9ae8858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile Engagement - API tümleştirme
Otomatik bir pazarlama sisteminde, oluşturma ve pazarlama kampanyaları etkinleştirme de otomatik olarak gerçekleşir. Bu amaçla - Azure Mobile Engagement API'lerini de kullanarak bu tür otomatik pazarlama kampanyaları oluşturma sağlar. 

Genellikle müşteriler kendi pazarlama kampanyaları bir parçası olarak Duyurular/yoklamalar vb. oluşturmak için Mobile Engagement ön uç arabirimi kullanın. Ancak pazarlama kampanyaları olgun dönüşürken, böylece tam otomatik bir işlem hattı, arka uç sistemlerden akan verilere dinamik olarak temel Mobile Engagement, kampanyalar oluşturan oluşturulabilir (örneğin, CRM sistem veya CMS sistemi SharePoint gibi) arka uç sistemlerindeki kilitli verilerden yararlanın gerek yoktur. 

![][5]

Bu öğretici burada bir SharePoint iş kullanıcı bir SharePoint listesi Pazarlama verilerle doldurur ve otomatik bir işlem listeden öğeler seçer ve bir pazarlama kampanyası SharePoint verileri oluşturmak için kullanılabilir REST API'lerini kullanarak Mobile Engagement sistemiyle bağlanır böyle bir senaryo geçer. 

> [!IMPORTANT]
> Genel olarak, iki unsur dayanan API'leri - kimlik doğrulama ve geçen parametreleri çağırma ayrıntıları gibi herhangi bir Mobile Engagement REST API'yi çağırmak nasıl anlamak için bu örnek bir başlangıç noktası olarak kullanabilirsiniz. 
> 
> 

## <a name="sharepoint-integration"></a>SharePoint Tümleştirmesi
1. İşte örnek SharePoint listesi gibi görünüyor. **Başlık**, **kategori**, **NotificationTitle**, **ileti** ve **URL** duyuru oluşturmak için kullanılır. Adlı bir sütun **IsProcessed** bir konsol programı biçiminde örnek otomasyon işlemi tarafından kullanılır. Böylece, zamanlayabilirsiniz bir Azure Web işi bu konsolu program ya da çalışma olabilir veya SharePoint iş akışı oluşturma ve SharePoint listesine bir öğe eklendiğinde duyuruyu etkinleştirme programa doğrudan kullanabilirsiniz. Bu örnekte, öğeler arasında SharePoint listesi ve bunların her biri için Azure Mobile Engagement duyuru Oluştur gider ve son olarak işaretler konsol programı kullanıyoruz **IsProcessed** başarılı duyuru oluşturulması doğru olması için bayrak.
   
    ![][1]
2. Örnek koddan kullanıyoruz *çevrimiçi istemci nesne modelini kullanarak SharePoint uzaktan kimlik doğrulama* [burada](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) SharePoint listesi ile kimlik doğrulaması için.
3. Kimlik doğrulaması yapıldıktan sonra Biz yeni oluşturulan tüm öğeleri bulmak için liste öğelerini döngü (olan **IsProcessed** = false). 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";
   
                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile Engagement tümleştirmesi
1. Biz işleme - gerektiren bir öğe bulduktan sonra biz liste öğesi ve çağrı duyuru oluşturmak için gereken bilgileri ayıklamak `CreateAzMECampaign` oluşturun ve daha sonra `ActivateAzMECampaign` etkinleştirmek için. Aslında Mobile Engagement arka ucuna çağırma REST API çağrıları bunlar. 
2. Mobile Engagement REST API'leri gerektiren bir **temel kimlik doğrulama düzeni yetkilendirme HTTP üstbilgisi** oluşan `ApplicationId` ve `ApiKey` hangi Azure portalından alın. Anahtarından kullandığınızdan emin olun **API anahtarları** bölüm ve *değil* gelen **sdk anahtarları** bölümü. 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. Duyuru oluşturmak için tür kampanya - başvurmak [belgelerine](https://msdn.microsoft.com/library/azure/mt683750.aspx). Kampanya belirtiyorsanız emin olmanız gerekir `kind` olarak *duyuru* ve [yükü](https://msdn.microsoft.com/library/azure/mt683751.aspx) ve FormUrlEncodedContent geçirme. 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. Oluşturulan duyuru olduktan sonra aşağıdakine benzer Mobile Engagement portalında görürsünüz (unutmayın durumu Taslak ve etkinleştirildi = = yok)
   
    ![][3]
5. `CreateAzMECampaign`Bir duyurunun kampanya oluşturur ve çağırana kimliğini döndürür. `ActivateAzMECampaign`Bu kimliği kampanyayı etkinleştirmek için parametre olarak gerektirir. 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. Etkinleştirilen duyuru olduktan sonra aşağıdakine benzer Mobile Engagement portalında görürsünüz:
   
    ![][4]
7. Kampanya etkinleştirilmiş almaz, kampanya ölçütü karşılayan tüm cihazlarda bildirimleri görmesini başlar. 
8. Liste öğesi IsProcessed ile işaretlenmiş göreceksiniz = false duyuru kampanya oluşturulduğunda True olarak ayarlandı.  

Bu örnek, çoğunlukla gerekli özellikleri belirtme basit duyuru kampanya oluşturulur. Bilgileri kullanarak portalından yapabilirsiniz kadar bu özelleştirebilirsiniz [burada](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



