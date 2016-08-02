<properties
    pageTitle="Mantıksal Uygulama Oluşturma | Microsoft Azure"
    description="SaaS hizmetlerine bağlanan Mantıksal Uygulamalar oluşturma hakkında bilgi edinin"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/16/2016"
    ms.author="stepsic"/>

# SaaS hizmetlerine bağlanan yeni bir mantıksal uygulama oluşturma

| Hızlı Başvuru |
| --------------- |
| [Logic Apps Tanımlama Dili](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Logic Apps Bağlayıcı Belgeleri](../connectors/apis-list.md) |
| [Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Bu konuda [App Service Logic Apps](app-service-logic-what-are-logic-apps.md)’i kullanmaya birkaç dakika içinde nasıl başlayabileceğiniz gösterilmektedir. İlginizi çeken bir dizi Tweet’i bir posta kutusuna iletmenize imkan tanıyan bir iş akışı gösterilecektir.

Bu senaryoyu kullanmak için aşağıdakiler gereklidir:

- Bir Azure aboneliği
- Bir Twitter hesabı
- Bir Office 365 hesabı

## Tweet’leri e-posta ile göndermek için yeni bir mantıksal uygulama oluşturma

1. Azure portal panosunda **Market**’i seçin. 
2. Her şey menüsünde 'mantıksal uygulamalar' araması yapın ve ardından **Logic App (önizleme)** seçeneğine tıklayın. Ayrıca **Yeni**, **Web + Mobil** ve **Mantıksal Uygulama (önizleme)**’yi seçebilirsiniz. 
3. Mantıksal uygulamanız için bir ad girin, uygulama hizmeti planı seçin ve **Oluştur**’u seçin.  
    Bu adımda bir uygulama hizmeti planınızın olduğu ve gerekli özellikleri bildiğiniz varsayılır. Bunlar geçerli değilse endişelenmeyin, [Azure App Service planlarına ayrıntılı genel bakış](azure-web-sites-web-hosting-plans-in-depth-overview.md) bölümünden başlayabilirsiniz. 

4. Mantıksal uygulama ilk kez açıldığında bir tetikleyici gereklidir. Tetikleyici arama kutusunda **twitter** araması yapın ve seçin.

7. Twitter’da aramak istediğiniz anahtar sözcüğünü yazmanız gerekir. 
    ![Twitter araması](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. Artı işaretini seçin ve ardından **Eylem ekle** veya **Koşul ekle** öğesini seçin:  
    ![Artı](./media/app-service-logic-create-a-logic-app/plus.png)
6. **Eylem Ekle**’yi seçtiğinizde kullanılabilir eylemleriyle birlikte tüm bağlayıcılar listelenir. Bundan sonra mantıksal uygulamanıza hangi bağlayıcı ve eylemin ekleneceğini seçebilirsiniz. Örneğin, **Office 365 - E-posta Gönder** öğesini ve daha fazla Office 365 eylemini seçebilirsiniz:  
    ![Eylemler](./media/app-service-logic-create-a-logic-app/actions.png)

7. Bundan sonra istediğiniz e-posta için parametreleri doldurmanız gerekir:  ![Parametreler](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Son olarak, mantıksal uygulamanızı canlı hale getirmek için **Kaydet**’i seçebilirsiniz.

## Oluşturduktan sonra mantıksal uygulamanızı yönetme

Mantıksal uygulamanız artık çalışır durumdadır. Zamanlanmış iş akışı her çalıştığında belirli bir hashtag’i içeren tweet’ler denetlenir. Eşleşen bir tweet bulduğunda bunu Dropbox’ınıza yerleştirir. Son olarak, uygulamayı devre dışı bırakmayı veya nasıl çalıştığını görürsünüz.

1. Ekranın sol tarafındaki **Gözat**’a tıklayın ve **Logic Apps**’ı seçin.

2. Geçerli durumunu ve genel bilgileri görmek için oluşturduğunuz yeni mantıksal uygulamaya tıklayın.

3. Yeni mantıksal uygulamanızı düzenlemek için **Tetikleyiciler ve Eylemler** öğesine tıklayın.

5. Uygulamayı kapatmak için komut çubuğunda **Devre Dışı Bırak**’a tıklayın.

5 dakikadan daha kısa bir süre içinde bulutta çalışan basit bir mantıksal uygulama ayarlayabildiniz. Logic Apps özelliklerini kullanma hakkında daha fazla bilgi için bkz. [Logic Apps özelliklerini kullanma]. Mantıksal Uygulama tanımları hakkında bilgi edinmek için bkz. [Mantıksal Uygulama tanımları yazma](app-service-logic-author-definitions.md).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Logic Apps özelliklerini kullanma]: app-service-logic-create-a-logic-app.md



<!---HONumber=Jun16_HO2-->


