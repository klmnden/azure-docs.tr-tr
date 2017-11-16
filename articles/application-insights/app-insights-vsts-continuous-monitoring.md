---
title: "DevOps yayın ardışık VSTS ve Azure Application Insights ile sürekli izleme | Microsoft Docs"
description: "Application Insights ile sürekli izleme hızlı bir şekilde ayarlamak için yönergeler sağlar"
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6ced5941d06c02a2576bf7f561bd535810e7ffd9
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Yayın hattınızı sürekli izleme Ekle

Visual Studio Team Services (VSTS) DevOps yayın hattınızı yazılım geliştirme yaşam döngüsü boyunca sürekli izlenmesini izin vermek için Azure Application Insights ile tümleşir. 

VSTS şimdi yayın işlem hatları Application Insights ve diğer Azure kaynaklarına izleme verilerini dahil edebilirsiniz aslına sürekli izlenmesini destekler. Application Insights uyarı algılandığında, dağıtım Geçitli kalabilir veya uyarı geri çözülene kadar geri alınması. Tüm denetimler başarılı olursa dağıtımları otomatik olarak sınamadan tüm üretim el ile müdahale gerek kalmadan geçebilirsiniz. 

## <a name="configure-continuous-monitoring"></a>Sürekli İzlemeyi Yapılandır

1. Varolan VSTS projesini seçin.

2. Üzerine gelerek **derleme ve sürüm** > seçin **sürümleri** > tıklatın **artı** > **oluşturma yayın tanımı** > Arama **izleme** > **sürekli izleme ile Azure uygulama hizmeti dağıtım.**

   ![Yeni VSTS yayın tanımı](.\media\app-insights-continuous-monitoring\001.png)

3. Tıklatın **uygulayın.**

4. Kırmızı bir ünlem işareti yanındaki mavi için metni seçin **ortam görevleri görüntüleyin.**

   ![Ortam görevleri görüntüle](.\media\app-insights-continuous-monitoring\002.png)

   Bir yapılandırma kutusu görünür, giriş alanları doldurun için aşağıdaki tabloyu kullanın.

    | Parametre        | Değer |
   | ------------- |:-----|
   | **Ortam adı**      | Yayın tanımı ortamı tanımlayan adı |
   | **Azure aboneliği** | Aşağı açılan VSTS hesaba bağlantılı tüm Azure aboneliklerini doldurur|
   | **Uygulama hizmeti adı** | Diğer seçimleri bağlı olarak bu alan için yeni bir değer elle girilmesi gerekebilir |
   | **Kaynak Grubu**    | Kullanılabilir kaynak gruplarıyla açılan doldurur |
   | **Uygulama Öngörüler kaynak adı** | Daha önce seçilen kaynak grubuna karşılık gelen tüm Application Insights kaynaklarla açılan doldurur.

5. Seçin **yapılandırma Application Insights uyarıları**

6. Varsayılan uyarı kuralları seçin **kaydetmek** > tanımlayıcı bir açıklama girin > tıklatın **Tamam**

## <a name="modify-alert-rules"></a>Uyarı kuralları değiştirme

1. Önceden tanımlanmış uyarı ayarlarını değiştirmek için kutuyu tıklatın **elipsler...**  sağındaki **uyarı kuralları.**

   (Out-of-box dört uyarı kuralları: kullanılabilirlik, başarısız olan istekleri, sunucu yanıt süresi, sunucu özel durumları.)

2. Aşağı açılan simgeyi tıklatın **kullanılabilirlik.**

3. Kullanılabilirlik değiştirme **eşik** hizmet düzeyi gereksinimlerinizi karşılayacak şekilde.

   ![Uyarıyı Değiştir](.\media\app-insights-continuous-monitoring\003.png)

4. Seçin **Tamam** > **kaydetmek** > tanımlayıcı bir açıklama girin > tıklatın **Tamam.**

## <a name="add-deployment-conditions"></a>Dağıtım koşulları ekleme

1. Tıklatın **ardışık düzen** > seçin **öncesi** veya **dağıtım sonrası koşullar** sürekli izleme ağ geçidi gerekir aşama bağlı olarak simge.

   ![Dağıtım öncesi koşulları](.\media\app-insights-continuous-monitoring\004.png)

2. Ayarlama **ağ geçitleri** için **etkin** > **onay geçitleri**> tıklatın **Ekle.**

3. Seçin **Azure İzleyici** (Bu seçenek, Azure İzleyici ve Application Insights erişim uyarılar hem sağlar)

    ![Azure İzleyici](.\media\app-insights-continuous-monitoring\005.png)

4. Girin bir **ağ geçitleri zaman aşımı** değeri.

5. Girin bir **örnekleme aralığı.**

## <a name="deployment-gate-status-logs"></a>Dağıtım kapısı durum günlükleri

Dağıtım geçitleri ekledikten sonra bir uyarı, önceden tanımlanmış, Eşiği aşan Application ınsights'ta istenmeyen sürüm yükseltme dağıtımınızdan korur. Uyarı çözüldükten sonra dağıtım otomatik olarak devam edebilirsiniz.

Bu davranış izlemek için seçin **sürümleri** > sağ yayın adı **açmak** > **günlükleri.**

![Günlükler](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla hakkında VSTS derleme ve sürüm deneyin bu öğrenmek için [quickstarts.](https://docs.microsoft.com/en-us/vsts/build-release/)
