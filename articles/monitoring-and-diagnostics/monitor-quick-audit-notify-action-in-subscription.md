---
title: "Azure aboneliğinizdeki önemli eylemleri denetleme ve bunlar hakkında bildirimle alma | Microsoft Docs"
description: "Kaynak yönetiminin geçmişini, hizmet durumunu ve Etkinlik Günlüğü'ndeki diğer abonelik etkinliklerini anlayın, ardından aboneliğinizde üst düzeyde ayrıcalıklı bir işlem yapıldığında e-posta bildirimi almak için bir Etkinlik Günlüğü uyarısı kullanın."
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: b0a79f46788dc7efb588110dc50805c45c373a49
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="audit-and-receive-notifications-about-important-actions-in-your-azure-subscription"></a>Azure aboneliğinizdeki önemli eylemleri denetleme ve bunlar hakkında bildirimle alma

**Azure Etkinlik Günlüğü**, Azure'da abonelik düzeyindeki olayların geçmişini sağlar. *Kimin* *hangi* kaynakları oluşturduğu, güncelleştirdiği veya sildiği ve bu işlemleri *ne zaman* yaptığı hakkında bilgi sağlar. Uyarı koşullarınıza uyan bir etkinlik gerçekleştiğinde e-posta, SMS veya web kancası bildirimleri almak için bir **Etkinlik Günlüğü uyarısı** oluşturabilirsiniz. Bu Hızlı Başlangıç, basit bir ağ güvenlik grubu oluşturma, gerçekleşen eylemi anlamak için Etkinlik Günlüğü'nü gözden geçirme ve ardından, bundan sonra herhangi bir ağ güvenlik grubu oluşturulduğunda bildirim almak için Etkinlik Günlüğü uyarısı yazma işlemlerinde size yol gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

2. **Ağ**'ı ve **Ağ güvenlik grubu**’nu seçin.

3. **Ad** olarak "myNetworkSG" girin ve **myResourceGroup** adlı yeni bir kaynak grubu oluşturun. **Oluştur** düğmesine tıklayın.

    ![Portalda ağ güvenlik grubu oluşturma](./media/monitor-quick-audit-notify-action-in-subscription/create-network-security-group.png)

## <a name="browse-the-activity-log-in-the-portal"></a>Portalda Etkinlik Günlüğü'nü gözden geçirme

Artık Etkinlik Günlüğü'ne ağ güvenlik grubu oluşturmayı tanımlayan bir olay eklenmiştir. Bu olayı belirlemek için aşağıdaki yönergeleri kullanın.

1. Gezinti listesinin sol tarafında bulunan **İzleyici** düğmesine tıklayın. Etkinlik Günlüğü bölümü açılır. Bu bölümde, aboneliğinizdeki kaynaklar üzerinde kullanıcıların gerçekleştirdiği tüm eylemlerin geçmişi yer alır; bunlara, **Kaynak Grubu**, **Zaman Aralığı** ve **Kategori** gibi çeşitli özelliklere göre filtre uygulanabilir.

2. **Etkinlik Günlüğü** bölümünde **Kaynak Grubu** açılan listesine tıklayın ve **myResourceGroup**'u seçin. **Zaman aralığı** açılan listesinin değerini **Son 1 saat** olarak değiştirin. **Uygula**'ya tıklayın.

    ![Ekinlik Günlüğü'nü filtreleme](./media/monitor-quick-audit-notify-action-in-subscription/browse-activity-log.png)

3. Gösterilen olaylar tablosunda **Write NetworkSecurityGroups**'a tıklayın.

## <a name="browse-an-event-in-the-activity-log"></a>Etkinlik Günlüğü'nde bir olaya göz atma

Gösterilen bölümde, gerçekleştirilen işlemin ad, zaman damgası ve gerçekleştiren kullanıcı veya uygulama gibi temel ayrıntıları yer alır.

![Etkinlik Günlüğü'nde olay özetini görüntüleme](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

Olay ayrıntılarının tamamını görüntülemek için **JSON** sekmesine tıklayın. Bu sekme, kullanıcı veya uygulamanın işlemi gerçekleştirmek için nasıl yetkilendirildiği, olay kategorisi ve düzeyi ve işlemin durumu gibi ayrıntıları içerir.

![Etkinlik Günlüğü'nü olay ayrıntılarını görüntüleme](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-json.png)

## <a name="create-an-activity-log-alert"></a>Etkinlik Günlüğü uyarısı oluşturma

1. **Özet** sekmesine tıklayarak olay özetine dönün.

2. Gösterilen özet bölümünde **Etkinlik günlüğü uyarısı ekle**'ye tıklayın.

    ![Portalda ağ güvenlik grubu oluşturma](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

3. Gösterilen bölümde, Etkinlik Günlüğü uyarısına bir ad ve açıklama ekleyin.

4. **Ölçütler**'in altında **Olay kategorisi** olarak **Yönetim**, **Kaynak türü** olarak **Ağ güvenlik grupları**, **İşlem adı** olarak **Ağ Güvenlik Grubu Oluşturma veya Güncelleştirme**, **Durum** olarak **Başarılı** değerinin ayarlandığından ve diğer tüm ölçüt alanlarının boş bırakıldığından veya **Tümü** olarak ayarlandığından emin olun. Ölçütler, Etkinlik Günlüğü'nde yeni bir olay görüntülendiğinde uyarının etkinleştirilip etkinleştirilmeyeceğini saptamak için kullanılan kuralları tanımlar.

    ![Portalda ağ güvenlik grubu oluşturma](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-criteria.png)

5. **Şu şekilde bildir** alanında **Yeni** eylem grubunu seçin, ardından eylem grubu için **ad** ve **kısa ad** sağlayın. Eylem grubu, uyarı etkinleştirildiğinde (ölçütler yeni bir olayla eşleştiğinde) gerçekleştirilecek eylemler kümesini tanımlar.

6. **Eylemler**'in altında 1 veya daha çok eylem ekleyin; eylem için **Ad**, **Eylem türü** (örneğin, e-posta veya SMS) ve bu özel eylem türünün **Ayrıntılarını** sağlayın (örneğin, web kancası URL'si, e-posta adresi veya SMS numarası).

    ![Portalda ağ güvenlik grubu oluşturma](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-actions.png)

7. **Tamam**'a tıklayarak Etkinlik Günlüğü uyarısını kaydedin.

## <a name="test-the-activity-log-alert"></a>Etkinlik Günlüğü uyarısını test etme

> [!NOTE]
> Etkinlik Günlüğü uyarısının tümüyle etkinleştirilmesi yaklaşık 10 dakika sürer. Etkinlik Günlüğü uyarısı tümüyle etkinleştirilmeden önce gerçekleşen yeni olaylar bildirim üretmez.
>
>

Uyarıyı test etmek için, önceki bölümü tekrarlayarak **Ağ güvenlik grubu oluşturun**, ama bu ağ güvenlik grubuna farklı bir ad verin ve mevcut kaynak grubunu yeniden kullanın. Birkaç dakika içinde, ağ güvenlik grubunun oluşturulduğuna ilişkin bir bildirim alırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadıysa, kaynak grubunu ve ağ güvenlik grubunu silin. Bunu yapmak için, oluşturduğunuz kaynak grubunun adını portalın en üstündeki arama kutusuna yazın ve kaynak grubunun adına tıklayın. Görüntülenen bölümde, **Kaynak grubunu sil** düğmesine tıklayın, kaynak grubunun adını yazın ve **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Etkinlik Günlüğü olayı oluşturan bir işlem yaptınız ve gelecekte bu işlem yeniden yapıldığında size bildirilmesi için bir Etkinlik Günlüğü uyarısı oluşturdunuz. Sonra, o işlemi yeniden yaparak uyarıyı test ettiniz. Azure son 90 günün Etkinlik Günlüğü olaylarını kullanıma sunar. Olayları 90 günden daha uzun süre tutmanız gerekiyorsa, diğer izleme verilerinizle birlikte Etkinlik Günlüğü verilerinizi de arşivlemeyi deneyin.

> [!div class="nextstepaction"]
> [İzleme verilerini arşivleme](./monitor-tutorial-archive-monitoring-data.md)
