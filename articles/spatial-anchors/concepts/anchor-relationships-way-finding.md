---
title: Bağlantı ilişkilerini ve Azure uzamsal yer işaretleri, yol bulma | Microsoft Docs
description: Bağlantı ilişkilerini arkasında kavramsal model hakkında bilgi edinin. Bir alanı içinde yer işaretleri bağlanmak için ve bir şekilde bulma senaryo karşılamak üzere yakında API'sini kullanmayı öğrenin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 008269a5883750dc8899d896c101c6a05bf7e814
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65969275"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Bağlantı ilişkilerini ve Azure uzamsal yer işaretleri, şekilde bulma

Bağlantı ilişkileri kullanarak bir alana bağlı bağlayıcılarını oluşturabilir ve sonra bunlar gibi sorular sorun:

* Yakındaki bağlantıların yer işaretleri var mı?
* Bunlar nasıl uzakta misiniz?

## <a name="examples"></a>Örnekler

Bu gibi durumlarda bağlı bağlayıcılarını kullanabilirsiniz:

* Bir çalışan bir endüstriyel fabrikası çeşitli konumlarda ziyaret içeren bir görevi tamamlamak gerekir. Fabrika her konumda uzamsal bağlantıları vardır. Çalışan bir konumdan diğerine Kılavuzu, HoloLens veya mobil uygulamanıza yardımcı olur. Uygulama, yakında uzamsal yer işaretleri için ilk ister ve sonraki konuma çalışan size yol gösterir. Uygulama, görsel olarak sonraki konuma uzaklık ve genel yönünü gösterir.

* Bir müze genel görüntüler uzamsal yer işaretleri oluşturur. Birlikte, bu yer işaretleri bir saatlik tura müze'nın temel genel görüntüler oluşturur. Genel bir görüntü mobil cihazlarında müze'nın karma gerçeklik uygulamasını ziyaretçiler açabilirsiniz. Ardından bunlar genel yönünü ve diğer genel görüntüler için daha fazla mesafe Turun görmek için alanı etrafında telefon kamerasına gelin. Bir kullanıcı doğru ortak bir ekran gösterilir gibi uygulama kullanıcının rehberlik edecek uzaklık ve genel yönünü güncelleştirir.

## <a name="set-up-way-finding"></a>Yedekleme yolu bulma ayarlama

Satır görüş yönünü ve rehberlik sunmak için yer işaretleri arasındaki uzaklığı kullanan bir uygulamayı kullanarak *şekilde bulma*. Yol-bulma Aç tarafından Aç Gezinti bölmesinden farklıdır. Bırakma tarafından Aç Gezinti bölmesinde kullanıcılar etrafında duvarlar, kapılar ve Katlar arasında gerçekleşir. Yol-bulma ile kullanıcı hedef genel yönünü konusunda ipuçları alır. Ancak çıkarımı veya bilgi alan hedef yapısı arasında gezinmek kullanıcı da yardımcı olur.

Bir şekilde bulma deneyimi oluşturmak için ilk deneyimine bir alanı hazırlamak ve kullanıcı etkileşim kuracağı bir uygulama geliştirmek. Kavramsal adımları şunlardır:

1. **Alanı planlama**: Alanı içinde hangi konumları şekilde bulma deneyiminin bir parçası olarak olacağına karar verin. Bizim senaryolarda Fabrika gözetmen veya müze turu Düzenleyici yolu bulma deneyimini dahil etmek için hangi konumların karar verebilirsiniz.
2. **Yer işaretleri bağlanma**: Uzamsal yer işaretleri oluşturmanız için seçilen konumlarını ziyaret edin. Son kullanıcı uygulamasının bir yönetici modunda veya farklı bir uygulama tamamen bunu yapabilirsiniz. Bağlanın veya her bağlantı için diğer ilgili. Hizmet, bu ilişkileri tutar.
3. **Son kullanıcı deneyimi Başlat**: Kullanıcılar seçilen konumlardan herhangi birinde olabilir bir yer işareti bulmak için uygulamayı çalıştırın. Genel tasarımınızı deneyimi girebileceğiniz konumları belirlemeniz gerekir.
4. **Bağlayıcılarını bulma**: Kullanıcı bir yer işareti bulduktan sonra uygulamanın yakındaki bağlantıların yer işaretleri isteyebilir. Bu yordam, cihaz ve bu yer işaretleri arasında bir poz döndürür.
5. **Kullanıcı Kılavuzu**: Uygulama, kullanıcının genel yönünü ve uzaklık hakkında rehberlik yapmak poz her bu bağlantıları kullanabilirsiniz. Örneğin, kamera akışını uygulamada bir simge ve aşağıda gösterildiği gibi her bir potansiyel hedefine temsil etmek için oka gösterebilir.
6. **Kılavuz İyileştir**: Kullanıcı gezer gibi uygulamayı düzenli olarak yeni bir poz arasında cihaz ve hedef bağlantı hesaplayabilirsiniz. Uygulama, hedefte geldiğinde kullanıcı Yardımı rehberi ipuçları iyileştirmek devam eder.

    ![İlişkin bir örnek bir uygulama yolu bulma Kılavuzu gösterebilirsiniz.](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Yer işaretleri bağlanma

Bir şekilde bulma deneyimi oluşturmak için önce seçilen konumda yer işaretlerini yerleştirmek gerekir. Bu bölümde, uygulamanın yönetici bu iş zaten bitmiş varsayacağız.

### <a name="connect-anchors-in-a-single-session"></a>Yer işaretleri, tek bir oturumda bağlanma

Yer işaretleri bağlanmak için:

1. İlk konuma izlemek ve bir CloudSpatialAnchorSession kullanarak bir bağlantı oluşturun.
2. İkinci bir konuma yol. Temel alınan MR/AR platformu hareketini izler.
3. Yer işareti B aynı CloudSpatialAnchorSession kullanarak oluşturun. A ve B bağlantıları artık bağlanır. Uzamsal bağlayıcılarını hizmeti, bu ilişkiyi korur.
4. Yordamı diğer bağlayıcıları için devam edin.

### <a name="connect-anchors-in-multiple-sessions"></a>Birden çok oturumlarda bağlayıcılarını bağlanma

Uzamsal bağlayıcılarını birden çok oturumu bağlanabilirsiniz. Bu yöntemi kullanarak, oluşturma ve bazı yer işaretleri tek seferde bağlanmak ve ardından daha sonra oluşturabilir ve daha fazla yer işaretleri bağlanın. 

Yer işaretleri birden çok oturumu bağlanmak için:

1. Uygulama içinde bir CloudSpatialAnchorSession bazı yer işaretleri oluşturur. 
2. Farklı bir zamanda yeni CloudSpatialAnchorSession kullanarak uygulamanın bu yer işaretleri (örneğin, bir yer işareti) birini bulur.
3. Yeni bir konuma yol. Temel alınan karma gerçeklik veya genişletilmiş gerçeklik platformu hareketini izler.
4. Yer işareti C aynı CloudSpatialAnchorSession kullanarak oluşturun. Yer işaretleri A, B ve C artık bağlanır. Uzamsal bağlayıcılarını hizmeti, bu ilişkiyi korur.

Zaman içinde daha fazla yer işaretleri ve daha fazla oturumları için bu yordamı devam edebilirsiniz.

### <a name="verify-anchor-connections"></a>Yer işareti bağlantıları doğrulama

Uygulamanın yakındaki bağlantıları için bir sorgu yayınlayarak iki çıpasının bağlandığınızdan emin doğrulayabilirsiniz. Sorgunun sonucu hedef bağlantı içeriyorsa, yer işareti bağlantısı doğrulanır. Yer işaretlerini bağlı değilseniz uygulamayı yeniden bağlanmayı deneyebilirsiniz. 

Neden bağlayıcılarını bağlanamayabilir bazı nedenler şunlardır:

* İzleme bağlayıcılarını bağlanma işlemi sırasında kaybedilen temel karma gerçeklik veya genişletilmiş gerçeklik platform.
* Yer işareti bağlantısı uzamsal bağlayıcılarını hizmet ile iletişim sırasında bir ağ hatası nedeniyle kalıcı uygulanamadı.

### <a name="find-sample-code"></a>Örnek kod bulma

Bağlayıcılarını bağlanmak ve sorgular yapmak için nasıl oluşturulduğunu gösteren örnek kodu bulmak için bkz [uzamsal bağlayıcılarını örnek uygulamalar](https://github.com/Azure/azure-spatial-anchors-samples).
