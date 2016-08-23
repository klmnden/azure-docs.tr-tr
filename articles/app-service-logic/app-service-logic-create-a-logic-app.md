<properties
    pageTitle="Mantıksal Uygulama Oluşturma | Microsoft Azure"
    description="SaaS hizmetlerine bağlanan Mantıksal Uygulamalar oluşturma hakkında bilgi edinin"
    authors="jeffhollan"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/16/2016"
    ms.author="jehollan"/>

# SaaS hizmetlerine bağlanan yeni bir mantıksal uygulama oluşturma

Bu konuda [Azure Logic Apps](app-service-logic-what-are-logic-apps.md)’i kullanmaya birkaç dakika içinde nasıl başlayabileceğiniz gösterilmektedir. E-postanıza ilgi çekici tweet’ler göndermenize imkan tanıyan basit bir iş akışı gösterilecektir.

Bu senaryoyu kullanmak için aşağıdakiler gereklidir:

- Bir Azure aboneliği
- Bir Twitter hesabı
- Outlook.com veya barındırılan Office 365 posta kutusu

## Tweet’leri e-posta ile göndermek için yeni bir mantıksal uygulama oluşturma

1. [Azure portal panosunda](https://portal.azure.com) **Yeni**’yi seçin. 
2. Arama çubuğunda 'logic' araması yapın ve ardından **Logic App** öğesini seçin. Ayrıca **Yeni**, **Web + Mobil** ve **Mantıksal Uygulama**’yi seçebilirsiniz. 
3. Mantıksal uygulamanız için bir ad girin, bir konum, kaynak grubu ve **Oluştur**’u seçin.  **Panoya Sabitle**’yi seçerseniz mantıksal uygulama dağıtıldıktan sonra otomatik olarak açılır.  
4. Mantıksal uygulamanızı ilk kez açtıktan sonra başlamak için bir şablondan seçim yapabilirsiniz.  Bunu sıfırdan oluşturmak için şimdilik **Boş Mantıksal Uygulama**’ya tıklayın. 
1. Tetikleyici oluşturmanız için gereken ilk öğedir.  Mantıksal uygulamanızı başlatacak olay budur.  Tetikleyici arama kutusunda **twitter** araması yapın ve seçin.
7. Şimdi tetiklenecek bir arama terimi yazın.  **Sıklık** ve **Aralık**, mantıksal uygulamanızın tweet’leri ne sıklıkla denetleyeceğini (ve bu süre boyunca tüm tweet’leri döndüreceğini) belirler.
    ![Twitter araması](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. **Yeni adım** düğmesini ve ardından **Eylem ekle** veya **Koşul ekle** öğesini seçin
6. **Eylem Ekle** öğesini seçtiğinizde bir eylem seçmek üzere [kullanılabilir bağlayıcılar](../connectors/apis-list.md) içinden arama yapabilirsiniz. Örneğin, outlook.com adresinden posta göndermek için **Outlook.com - E-posta Gönder**’i seçebilirsiniz:  
    ![Eylemler](./media/app-service-logic-create-a-logic-app/actions.png)

7. Bundan sonra istediğiniz e-posta için parametreleri doldurmanız gerekir:  ![Parametreler](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Son olarak, mantıksal uygulamanızı canlı hale getirmek için **Kaydet**’i seçebilirsiniz.

## Oluşturduktan sonra mantıksal uygulamanızı yönetme

Mantıksal uygulamanız artık çalışır durumdadır. Girilen arama terimini içeren tweet’ler düzenli olarak denetlenir. Eşleşen bir tweet bulunduğunda size bir e-posta gönderilir. Son olarak, uygulamayı devre dışı bırakmayı veya nasıl çalıştığını görürsünüz.

1. [Azure Portal](https://portal.azure.com)’a gidin

1. Ekranın sol tarafındaki **Gözat**’a tıklayın ve **Logic Apps**’ı seçin.

2. Geçerli durumunu ve genel bilgileri görmek için oluşturduğunuz yeni mantıksal uygulamaya tıklayın.

3. Yeni mantıksal uygulamanızı düzenlemek için **Düzenle** öğesine tıklayın.

5. Uygulamayı kapatmak için komut çubuğunda **Devre Dışı Bırak**’a tıklayın.

1. Mantık uygulamanızın ne zaman çalışacağını izlemek için çalışma ve tetikleme geçmişlerini görüntüleyin.  En son verileri görmek için **Yenile**’ye tıklayabilirsiniz.

5 dakikadan daha kısa bir süre içinde bulutta çalışan basit bir mantıksal uygulama ayarlayabildiniz. Logic Apps özelliklerini kullanma hakkında daha fazla bilgi için bkz. [Logic Apps özelliklerini kullanma]. Mantıksal Uygulama tanımları hakkında bilgi edinmek için bkz. [Mantıksal Uygulama tanımları yazma](app-service-logic-author-definitions.md).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Logic Apps özelliklerini kullanma]: app-service-logic-create-a-logic-app.md



<!--HONumber=Aug16_HO1-->


