---
title: Azure portal ile cihaz kayıtlarını yönetme | Microsoft Docs
description: Azure Portalı'nda DPS hizmetiniz için cihaz kayıtlarını yönetme
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/05/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 55486d9a37968351f5313c708e9ef26e5b89063c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Azure Portal ile cihaz kayıtlarını yönetme

A *cihaz kaydı* tek bir cihazı veya grubun belirli bir noktada Azure IOT Hub cihaz sağlama Hizmeti'ne kaydedebilir cihazların bir kayıt oluşturur. İlk istenen yapılandırma istenen IOT hub'ı dahil, kaydın parçası olarak aygıt kaydı içerir. Bu makalede sağlama hizmetiniz için cihaz kayıtlarını yönetme gösterilmektedir.


## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

Aygıtlarınızı sağlama hizmeti ile kaydetmek için iki yolu vardır:

* Bir **kayıt grup** olabilen aynı imzalama sertifikası tarafından imzalanmış X.509 sertifikalarının ortak bir kanıtlama mekanizması paylaşan aygıtları bir grup için bir giriş [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate), fiziksel aygıttaki aygıt sertifika üretmek için kullanılan. Çok sayıda istenen ilk yapılandırmasını paylaşan cihazlar için veya cihazlar için bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz. Yalnızca X.509 kanıtlama mekanizması olarak kullanan cihazları kaydedebilmeniz için Not *kayıt grupları*. 

    Portalında aşağıdaki adımları kullanarak cihazları bir grup için bir kayıt grubu oluşturun:

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.  
    2. Cihazınızı için kaynaklar listesi kaydetmek istediğiniz cihaz sağlamayı hizmete tıklayın.  
    3. Sağlama hizmetinizi:  
       a. Tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **kayıt grupları** sekmesi.  
       b. Üstteki **Ekle** düğmesine tıklayın.  
       c. "Kayıt grubu Ekle" paneli göründüğünde, kayıt liste girdisi için bilgileri girin.  **Grup adı** gereklidir. Ayrıca "CA veya Ara" seçin **sertifika türü**ve kök karşıya **birincil sertifika** aygıtları grubu için.  
       d. **Kaydet**’e tıklayın. Üzerinde başarılı oluşturma kayıt grubunuzun altında göründüğünü grup adını görmeniz gerekir **kayıt grupları** sekmesi.  

       [![Portal kayıt grubunda](./media/how-to-manage-enrollments/group-enrollment.png)] (. / media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* Bir **tek tek kayıt** kaydedebilir tek bir cihaz için bir giriş. Tek tek kayıtları ya da x509 kullanabilir sertifikalar veya SAS belirteçler (bir fiziksel veya sanal'nden TPM) kanıtlama mekanizmaları. Benzersiz başlangıç yapılandırmasını gerektiren cihazlar için veya yalnızca TPM ya da sanal TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak kullanabileceğiniz cihazlar için tek tek kayıtları kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Aşağıdaki adımları kullanarak portalda tek tek bir kayıt oluşturabilirsiniz:

    1. Azure portalında oturum açın ve tıklatın **tüm kaynakları** sol taraftaki menüden.
    2. Cihazınızı için kaynaklar listesi kaydetmek istediğiniz cihaz sağlamayı hizmete tıklayın.
    3. Sağlama hizmetinizi:  
       a. Tıklatın **kayıtlarını yönetme**seçeneğini belirleyip **tek tek kayıtları** sekmesi.  
       b. Üstteki **Ekle** düğmesine tıklayın.   
       c. "Kaydı Ekle" paneli göründüğünde, kayıt liste girdisi için bilgileri girin. İlk kanıtlama seçin **mekanizması** aygıtın (X.509 veya TPM). X.509 kanıtlama yaprak karşıya gerektirir **birincil sertifika** cihaz için. TPM girmenizi gerektirir **kanıtlama anahtar** ve **kayıt kimliği** cihaz için.  
       d. **Kaydet**’e tıklayın. Üzerinde başarılı oluşturma kayıt grubunuzun altında göründüğünü Cihazınızı görmeniz gerekir **tek tek kayıtları** sekmesi.  

       [![Tek tek kayıt Portalı'nda](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Güncelleştirme kayıt girişi
Varolan bir kayıt girişi portalında aşağıdaki adımları kullanarak güncelleştirebilirsiniz:

1. Azure portalında hizmeti cihaz sağlama açın ve tıklatın **kayıtlarını yönetme**. 
2. Değişiklik yapmak istediğiniz kayıt girişine gidin. Cihaz kaydı ile ilgili özet bilgiler açar girdiyi tıklatın. 
3. Bu sayfadaki öğeler güvenlik türü dışında değiştirebilir ve IOT hub cihaz gibi kimlik bilgileri, hem de cihaz kimliği bağlantılı olması gerekir Ayrıca ilk cihaz çifti durumunu değiştirebilir. 
4. Tamamlandığında, tıklatın **kaydetmek** , cihaz kaydını güncelleştirmek için. 

    ![Kayıt Portalı'nda güncelleştirme](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>Cihaz kaydı Kaldır
Burada aygıtlarınızın tüm IOT hub'ına sağlanması gerekmez durumlarda, ilgili kayıt girişi portalında aşağıdaki adımları kullanarak kaldırabilirsiniz:

1. Azure portalında hizmeti cihaz sağlama açın ve tıklatın **kayıtlarını yönetme**. 
2. Gidin ve kaldırmak istediğiniz kayıt girişi seçin. 
3. Tıklatın **silmek** en üstünde düğmesine tıklayın ve ardından **Evet** onaylamanız istendiğinde. 
5. Eylem tamamlandıktan sonra cihaz kayıtlarını listesinden kaldırılır, giriş görürsünüz. 
 
    ![Kayıt Portalı'nda Kaldır](./media/how-to-manage-enrollments/remove-enrollment.png)


