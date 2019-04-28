---
title: Azure portalı ile cihaz kayıtlarını yönetme | Microsoft Docs
description: Azure portalında cihaz sağlama hizmeti için cihaz kayıtlarını yönetme
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 51b072bfd0827528a5504133dff8c1cdd7a7ca86
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122786"
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Azure portalı ile cihaz kayıtlarını yönetme

A *cihaz kaydı* tek bir cihaz veya bir grubun belirli bir noktada Azure IOT Hub cihazı sağlama hizmeti ile kaydedebilir cihazların bir kayıt oluşturur. Kayıt için istenen IOT hub'ı dahil olmak üzere, bu kayıt işleminin bir parçası olarak cihaz ilk istenen yapılandırmayı içerir. Bu makalede, sağlama hizmetinizin cihaz kayıtlarını yönetme işlemini göstermektedir.


## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

Sağlama hizmeti ile cihazlarınızı kaydedebilirsiniz iki yolu vardır:

* Bir **kayıt grubu** olabilen aynı imza sertifikası tarafından imzalanmış X.509 Sertifika, ortak bir kanıtlama mekanizmasını paylaşan cihazlar grubu için bir giriş [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)fiziksel cihazda cihaz sertifikası oluşturmak için kullanılır. Aynı kiracıya giden tüm cihazlar veya çok sayıda istenen bir ilk yapılandırmayı paylaşan cihazlar için kayıt grubu kullanılması önerilir. Yalnızca X.509 kanıtlama mekanizması olarak kullanan cihazları kaydedebilmeniz için Not *kayıt grupları*. 

    Kayıt grubu portalında aşağıdaki adımları kullanarak cihazları bir grup oluşturabilirsiniz:

  1. Azure portalında oturum açın ve tıklayın **tüm kaynakları** sol taraftaki menüden.  
  2. Cihazınıza kaynakları listesinden kaydetmek istediğiniz cihaz sağlama hizmetine tıklayın.  
  3. Sağlama hizmetinize:  
     a. Tıklayın **kayıtları Yönet**, ardından **kayıt grupları** sekmesi.  
     b. Üstteki **Ekle** düğmesine tıklayın.  
     c. "Kayıt grubu Ekle" panelini göründüğünde, kayıt listesi girişi bilgilerini girin.  **Grup adı** gereklidir. Ayrıca "CA veya Ara" seçin **sertifika türü**ve kök karşıya **birincil sertifika** cihaz grubu için.  
     d. **Kaydet**’e tıklayın. Oluşturma başarılı kayıt grubunuzun, grup adı altında gösterildiğini görmelisiniz **kayıt grupları** sekmesi.  

     [![Portalda kayıt grubu](./media/how-to-manage-enrollments/group-enrollment.png)](./media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* Bir **bireysel kayıt** kaydedebilir, tek bir cihaz için bir giriş. Bireysel kayıtlar, ya da x509 kullanabilir sertifikalarını veya SAS belirteçlerini (bir fiziksel veya sanal'nden TPM) kanıtlama mekanizması olarak. Benzersiz ilk yapılandırma gerektiren cihazlar için veya kanıtlama mekanizması olarak TPM ya da sanal TPM aracılığıyla SAS belirteçleri yalnızca kullanabileceğiniz cihazlar için bireysel kayıtların kullanılmasını öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Bireysel kayıt portalında aşağıdaki adımları izleyerek oluşturabilirsiniz:

    1. Azure portalında oturum açın ve tıklayın **tüm kaynakları** sol taraftaki menüden.
    1. Cihazınıza kaynakları listesinden kaydetmek istediğiniz cihaz sağlama hizmetine tıklayın.
    1. Sağlama hizmetinize:  
       a. Tıklayın **kayıtları Yönet**, ardından **bireysel kayıtlar** sekmesi.  
       b. Üstteki **Ekle** düğmesine tıklayın.   
       c. "Kayıt Ekle" panelini göründüğünde, kayıt listesi girişi bilgilerini girin. İlk kanıtlama seçin **mekanizması** cihazın (X.509 veya TPM). X.509 kanıtlama yaprak karşıya gerektirir **birincil sertifika** aygıt için. TPM girmenizi gerektirir **kanıtlama anahtarı** ve **kayıt kimliği** aygıt için.  
       d. **Kaydet**’e tıklayın. Oluşturma başarılı kayıt grubunuzun, Cihazınızı altında gösterildiğini görmelisiniz **bireysel kayıtlar** sekmesi.  

       [![Bireysel kayıt Portalı'nda](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Güncelleştirme kayıt girişi
Portalında aşağıdaki adımları kullanarak mevcut bir kayıt girişi güncelleştirebilirsiniz:

1. Azure portalında cihaz sağlama hizmetinizi açın ve tıklayın **kayıtları yönetme**. 
1. Değiştirmek istediğiniz kayıt girişine gidin. Bir özet bilgilerini cihaz kaydı sorunlarınızı açılır girdiye tıklayın. 
1. Bu sayfadaki öğeleri dışındaki güvenlik türü değiştirebilirsiniz ve IOT hub cihazı gibi bir kimlik, cihaz kimliğini yanı sıra bağlantılı olması gerekir Ayrıca ilk cihaz ikizi durumu değiştirebilir. 
1. Tamamlandığında, tıklayın **Kaydet** cihaz kaydı sorunlarınızı güncelleştirilecek. 

    ![Kayıt Portalı'nda güncelleştirme](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>Cihaz kaydı Kaldır
Burada yandan cihazlarınıza düzenlenen tüm IOT hub'ına sağlanması gerekmez durumlarda, ilgili kayıt girişi portalında aşağıdaki adımları kullanarak kaldırabilirsiniz:

1. Azure portalında cihaz sağlama hizmetinizi açın ve tıklayın **kayıtları yönetme**. 
1. Gidin ve kaldırmak istediğiniz kayıt girişi seçin. 
1. Tıklayın **Sil** üstünde düğmesine ve ardından **Evet** onaylamanız istendiğinde. 
1. İşlem tamamlandıktan sonra cihaz kayıtlarını listesinden kaldırılsa giriş görürsünüz. 
 
    ![Kayıt Portalı'nda Kaldır](./media/how-to-manage-enrollments/remove-enrollment.png)


