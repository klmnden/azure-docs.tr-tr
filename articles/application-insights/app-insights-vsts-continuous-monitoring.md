---
title: DevOps yayın işlem hattınızı Azure DevOps ve Azure Application Insights ile sürekli izleme | Microsoft Docs
description: Application Insights ile sürekli izleme hızlıca ayarlamaya ilişkin yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ecda8621640223f1c27f32834f2e4a098da4aba6
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301642"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Yayın işlem hattınızı için sürekli izleme ekleme

DevOps yayın işlem hattınızı yazılım geliştirme yaşam döngüsü boyunca sürekli izlenmesini izin vermek için Azure Application Insights ile Azure DevOps hizmetleriyle tümleşir. 

Azure DevOps Hizmetleri artık yayın işlem hatlarını izleme verilerini Application Insights ve diğer Azure kaynakları birleştirebilirsiniz gerçekleştirilmesine sürekli izlenmesini de destekler. Bir Application Insights uyarı algılandığında, dağıtım Geçitli kalabilir veya uyarı geri çözülene kadar alınamaz. Tüm denetimlerden başarıyla, dağıtımları otomatik olarak test tüm üretim el ile müdahale gerekmeksizin geçebilirsiniz. 

## <a name="configure-continuous-monitoring"></a>Sürekli izlemeyi yapılandırma

1. Mevcut bir Azure DevOps Services projesi seçin.

2. Üzerine **derleme ve yayın** > seçin **yayınlar** > tıklayın **artı** > **Oluştur yayın tanımı** > Arama **izleme** > **sürekli izleme ile Azure App Service dağıtımı.**

   ![Azure DevOps Hizmetleri yeni yayın ardışık düzeni](.\media\app-insights-continuous-monitoring\001.png)

3. Tıklayın **uygulayın.**

4. Kırmızı ünlem yanındaki mavi metin seçin **ortam görevlerini görüntüle.**

   ![Ortam görevlerini görüntüle](.\media\app-insights-continuous-monitoring\002.png)

   Bir yapılandırma kutusu görünür; giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Parametre        | Değer |
   | ------------- |:-----|
   | **Ortam adı**      | Yayın işlem hattı ortamı tanımlayan adı |
   | **Azure aboneliği** | Azure DevOps Hizmetleri kuruluşa bağlı herhangi bir Azure aboneliği ile açılan doldurur.|
   | **App Service adı** | Yeni bir değer el ile giriş diğer seçimlere bağlı olarak bu alan için gerekli olabilir. |
   | **Kaynak Grubu**    | Aşağı açılan kullanılabilir kaynak grupları ile doldurur. |
   | **Application Insights kaynak adı** | Aşağı açılan daha önce seçilen kaynak grubuna karşılık gelen tüm Application Insights kaynakları ile doldurur.

5. Seçin **uyarıları Application ınsights'ı Yapılandır**

6. Varsayılan uyarı kuralları seçin **Kaydet** > açıklayıcı yorum girin > tıklatın **Tamam**

## <a name="modify-alert-rules"></a>Uyarı kurallarını değiştirme

1. Önceden tanımlanmış uyarı ayarlarını değiştirmek için kutuyu tıklatın **üç nokta...**  sağındaki **uyarı kuralları.**

   (Hazır-dört uyarı kuralları: kullanılabilirlik, başarısız olan istekleri, sunucu yanıt süresi, sunucu özel durumları.)

2. Aşağı açılan simgesi tıklayın **kullanılabilirlik.**

3. Kullanılabilirlik değiştirme **eşiği** hizmet düzeyi gereksinimlerinizi karşılayacak şekilde.

   ![Uyarı değiştirme](.\media\app-insights-continuous-monitoring\003.png)

4. Seçin **Tamam** > **Kaydet** > açıklayıcı yorum girin > tıklatın **Tamam.**

## <a name="add-deployment-conditions"></a>Dağıtım koşulları ekleme

1. Tıklayın **işlem hattı** > seçin **öncesi** veya **dağıtım sonrası koşulları** sembol sürekli izleme bir ağ geçidi gerekir aşama bağlı olarak.

   ![Dağıtım öncesi koşulları](.\media\app-insights-continuous-monitoring\004.png)

2. Ayarlama **kapılar** için **etkin** > **onay kapıları**> tıklatın **Ekle.**

3. Seçin **Azure İzleyici** (Bu seçenek, Azure İzleyici ve Application Insights erişim uyarılar hem sağlar)

    ![Azure İzleyici](.\media\app-insights-continuous-monitoring\005.png)

4. Girin bir **kapıların zaman aşımı** değeri.

5. Girin bir **örnekleme aralığı.**

## <a name="deployment-gate-status-logs"></a>Dağıtım geçit durumu günlükleri

Dağıtım kapıları ekledikten sonra önceden tanımlanmış bir eşiği aştığında Application ınsights uyarı istenmeyen sürüm yükseltme dağıtımınızdan korur. Uyarı çözümlendiğinde dağıtım otomatik olarak devam edebilirsiniz.

Bu davranışını gözlemlemek için seçin **yayınlar** > sağ yayın adı **açın** > **günlükleri.**

![Günlükler](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure işlem hatları hakkında daha fazla deneyin bu öğrenmek [hızlı başlangıçları.](https://docs.microsoft.com/azure/devops/pipelines)
