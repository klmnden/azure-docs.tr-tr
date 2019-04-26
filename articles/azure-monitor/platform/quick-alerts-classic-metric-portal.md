---
title: Ölçüm değeri bir koşula uyduğunda bildirim alma
description: Kullanıcıların bir Mantıksal Uygulama için ölçüm oluşturmasına yardımcı olmaya yönelik bir hızlı başlangıç kılavuzu
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: quickstart
ms.date: 02/08/2018
ms.author: ancav
ms.custom: mvc
ms.subservice: alerts
ms.openlocfilehash: f8b9db47684a6dd78302f094d8e670da4a61ab2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60453400"
---
# <a name="receive-a-notification-when-a-metric-value-meets-a-condition"></a>Ölçüm değeri bir koşula uyduğunda bildirim alma

Azure İzleyici birçok Azure kaynağı için ölçümler sağlar. Bu ölçümler söz konusu kaynakların performansını ve durumunu gösterir. Birçok durumda, ölçüm değerleri kaynakta sorunlu bir noktaya işaret edebilir. Anormal davranışları izlemek için ölçüm uyarıları oluşturabilir ve böyle bir durumla karşılaşıldığında bildirim alabilirsiniz. Bu Hızlı Başlangıçta Mantıksal Uygulama oluşturma, iş oluşturma ve Mantıksal Uygulamanın ölçümlerini görselleştirme işlemlerinin adımları gösterilir. Ardından uyarı oluşturmaya ve Mantıksal Uygulama kaynağının ölçümüyle ilgili bildirim almaya geçilir.

Ölçümler ve ölçüm uyarıları hakkında daha fazla bilgi için bkz. [Azure İzleyici ölçümlerine genel bakış](data-platform.md) ve [Azure İzleyici uyarılarına genel bakış](alerts-overview.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-logic-app"></a>Mantıksal Uygulama oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Mantıksal Uygulama**'yı arayıp bulun ve seçin. **Oluştur** düğmesine tıklayın.

3. myLogicApp adını ve myResourceGroup Kaynak Grubunu girin. Aboneliğinizi kullanın.  Varsayılan konumu kullanın. **Panoya Sabitle** seçeneğini işaretleyin.  Tamamlandığında **Oluştur**’a tıklayın. 

    ![Portalda mantıksal uygulamanızla ilgili temel bilgileri girin](./media/quick-alerts-classic-metric-portal/create-logic-app-portal.png)  


4. Mantıksal uygulama panonuza sabitlenmiş olmalıdır. Mantıksal uygulamanın üzerine tıklayarak bu uygulamaya gidin.

5. Mantıksal Uygulama panelinde **Mantıksal Uygulama Tasarımcısı**'nı seçin.

     ![Portal panelindeki mantıksal uygulama tasarımcısında bir yinelenme tetikleyicisi oluşturulmuş](./media/quick-alerts-classic-metric-portal/logic-app-designer.png)  

6. Aşağıdaki diyagramda görüldüğü gibi değerlerinizi ayarlayın.

    ![Portal panelinde mantıksal uygulama tetikleyicisini yapılandırın](./media/quick-alerts-classic-metric-portal/create-logic-app-triggers.png) 

7. Tasarımcıda **Yinelenme** tetikleyicisini seçin.

8. Mantıksal uygulamanızın her 20 saniyede bir tetiklenmesini sağlamak için aralık değeri olarak 20 ve sıklık olarak saniye ayarlayın.

9. **Yeni Adım** düğmesine tıklayın ve **Eylem ekle**'yi seçin.

10. **HTTP** seçeneğini belirtin ve **HTTP-HTTP**'yi seçin.

11. **Yöntem** olarak POST ve **Uri** olarak dilediğiniz bir web adresini ayarlayın.

12. **Kaydet**’e tıklayın.

13. Mantıksal uygulama çalıştırma eylemlerinin gerçekleşmesi 5 dakikayı bulabilir.  

## <a name="view-metrics-for-your-logic-app"></a>Mantıksal uygulamanızın ölçümlerini görüntüleme

1. Gezinti bölmesinin sol tarafındaki **İzleyici** seçeneğine tıklayın.

2. **Ölçümler** sekmesinde mantıksal uygulamanızın **Abonelik**, **Kaynak Grubu**, **Kaynak Türü** ve **Kaynak** bilgilerini doldurun.

3. Ölçüm listesinde **Başarısız Çalıştırmalar**’ı seçin.

4. Geçtiğimiz saatin verilerini görüntülemek için grafiğin **Zaman aralığı**'nı değiştirin.

5. Artık geçtiğimiz saat içinde mantıksal uygulamanızın başlatmış olduğu toplam çalıştırma sayısının çizildiği grafiği görmelisiniz. Görmüyorsanız, yukarıdaki adımdan sonra en az 5 dakika beklediğinizden emin olun. Ardından tarayıcınızı yenileyin. 

    ![Mantıksal uygulama kaynağı için bir ölçüm grafiği çizin](./media/quick-alerts-classic-metric-portal/logic-app-metric-chart.png)

## <a name="create-a-metric-alert-for-your-logic-app"></a>Mantıksal uygulamanız için ölçüm uyarısı oluşturma

1.  Ölçümler panelinin sağ üst kısmındaki **Ölçüm uyarısı ekle** düğmesine tıklayın.

2. Ölçüm uyarınızı 'myLogicAppAlert' olarak adlandırın ve uyarının kısa bir açıklamasını sağlayın.

3. Ölçüm uyarısına **Koşul** olarak 'Büyüktür' ayarlayın, **Eşik** olarak '10' ayarlayın ve **Dönem** olarak da 'Son 5 dakika boyunca' seçeneğini ayarlayın.

4. Son olarak, **Ek yönetici e-postaları**'nın altına e-posta adresinizi girin. Bu uyarı, mantıksal uygulamanızda 5 dakikalık bir dönemde 10'dan fazla başarısız çalıştırma olması durumunda size bir e-posta gönderilmesini güvence altına alır.

    ![Portal panelinde mantıksal uygulama uyarısını yapılandırın](./media/quick-alerts-classic-metric-portal/logic-app-metrics-alert-portal.png)

## <a name="receive-metric-alert-notifications-for-your-logic-app"></a>Mantıksal uygulamanız için ölçüm uyarısı bildirimleri alma
1. Birkaç saniye içinde 'Microsoft Azure Uyarılar' tarafından gönderilen ve size uyarının 'etkinleştirildiğini' bildiren bir e-posta almalısınız.

2. Mantıksal uygulamanıza geri dönün, aralık 1 ve sıklık saat olacak şekilde yinelenme tetikleyicisini değiştirin.

3. Birkaç dakika içinde 'Microsoft Azure Uyarılar' tarafından gönderilen ve size uyarının 'çözüldüğünü' bildiren bir e-posta almalısınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları silmeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure Portal'da sol taraftaki menüden **İzleyici**'ye tıklayın.

2. **Uyarılar** sekmesini seçin, bu hızlı başlangıç kılavuzunda oluşturduğunuz uyarıyı bulun ve tıklayın.

3. Ölçüm uyarı panelinde **Sil**'e tıklayın.

4. Azure Portal'da sol taraftaki menüde **Mantıksal Uygulama**'yı arayın ve ardından **Mantıksal uygulamalar**'a tıklayın.

5. Panelde, metin kutusunda bu hızlı başlangıçta oluşturduğunuz mantıksal uygulamaya tıklayın ve sonra da **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, kaynaklarınız için ölçüm uyarısı oluşturmayı öğrendiniz. Ölçüm uyarıları hakkında daha fazla bilgi için, uyarılarla ilgili genel bakış bilgilerimize tıklayarak gezinin.

> [!div class="nextstepaction"]
> [Azure İzleyici abonelik eylemi uyarıları](./../../azure-monitor/platform/quick-audit-notify-action-subscription.md )

