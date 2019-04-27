---
title: Azure IOT Hub cihazı sağlama hizmeti içinde nasıl dağıtılacağıdır X.509 sertifikaları | Microsoft Docs
description: Cihaz sağlama hizmeti örneğinizi X.509 sertifikalarıyla sunma
author: wesmc7777
ms.author: wesmc
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 8cf5f262a758efe08ad73e2d8066ad4b736e76d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627037"
---
# <a name="how-to-roll-x509-device-certificates"></a>X.509 cihaz sertifikaları sunma

IOT çözümünüzü yaşam döngüsü boyunca, sertifikaları alma gerekecektir. İki sertifikaları çalışırken ana nedeni, güvenlik ihlali ve sertifika süre sonu olacaktır. 

Sertifikaları sıralı bir ihlal durumunda sisteminizde güvenli hale getirmek için bir güvenlik en iyi uygulamadır. Bir parçası olarak [varsayar ihlal Metodoloji](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf), Microsoft sorunlarınızda reaktif güvenlik işlemleri önleyici ölçüler yanı sıra yerinde olması gereksinimini. Cihaz sertifikalarınızı çalışırken bu güvenlik işlemleri bir parçası olarak dahil edilecek. Sertifikalarınızı geri alma sıklığı, çözümünüzün güvenlik gereksinimlerine bağlıdır. Çok gizli veriler içeren çözümler müşterilerle diğerleri sertifikalarını her birkaç yılda geri alma sırasında sertifika günlük, döküm.

Cihaz sertifikaları çalışırken cihaz ve IOT hub'ı üzerinde depolanan sertifika güncelleştirme içerir. Daha sonra cihaz normal kendini IOT hub'ı kullanarak yeniden sağlamak [otomatik sağlama](concepts-auto-provisioning.md) cihaz sağlama hizmeti ile.


## <a name="obtain-new-certificates"></a>Yeni sertifika alın

IOT cihazları için yeni sertifikalar almak için birçok yolu vardır. Kendi sertifikalarınızı oluşturma ve üçüncü taraf sahip sertifika oluşturma, yönetme, bu cihaz fabrikasından sertifikaları edinme içerir. 

Otomatik olarak imzalanan sertifikalar birbiri tarafından bir kök CA sertifikası için güven zinciri oluşturmak için bir [yaprak sertifikayı](concepts-security.md#end-entity-leaf-certificate). İmzalama sertifikası yaprak sertifika güven zinciri sonunda imzalamak için kullanılan bir sertifikadır. İmzalama sertifikası, bir kök CA sertifikasını veya ara sertifika güven zinciri de olabilir. Daha fazla bilgi için [X.509 sertifikaları](concepts-security.md#x509-certificates).
 
İmzalama sertifikası almak için iki farklı yolu vardır. Üretim sistemleri için önerilen, ilk yol, bir kök sertifika yetkilisinden (CA) bir imza sertifikası satın almaktır. Bu şekilde güvenilir bir kaynaktan güvenlik bağlanır. 

İkinci yol OpenSSL gibi bir araç kullanarak kendi X.509 sertifikaları oluşturmaktır. Bu yaklaşım için test X.509 sertifikaları mükemmeldir ancak güvenlik etrafında birkaç garantileri sağlar. Yalnızca kendi CA sağlayıcısı olarak görev yapacak hazırladığınız sürece test etmek için bu yaklaşımı kullanmanız önerilir.
 

## <a name="roll-the-certificate-on-the-device"></a>Cihazda sertifika alma

Bir cihazdaki sertifikaları her zaman depolanması gibi güvenli bir yerde bir [donanım güvenlik modülü (HSM)](concepts-device.md#hardware-security-module). Cihaz sertifikaları alma şeklini nasıl oluşturulmuş ve cihazların ilk başta yüklü üzerinde bağlıdır. 

Üçüncü bir taraftan sertifikalarınızı geldiyseniz, bunlar sertifikalarını nasıl geri içine aramanız gerekir. İşlem, düzenlemeyle eklenebilir veya ayrı bir hizmet sunulup olabilir. 

Kendi cihaz sertifikaları yönetiyorsanız, sertifikalar güncelleştirmek için kendi işlem hattınızı gerekecektir. Her iki eski ve yeni bir yaprak sertifikanın ortak adı (CN) aynı olduğundan emin olun. Aynı CN sağlayarak, cihazın kendisi yinelenen bir kayıt oluşturmadan yeniden sağlamak. 


## <a name="roll-the-certificate-in-the-iot-hub"></a>IOT hub'ında sertifika alma

Cihaz sertifikasını el ile bir IOT hub'ına eklenebilir. Sertifika bir cihaz sağlama hizmeti örneği kullanarak otomatik olarak da yapılabilir. Bu makalede, bir cihaz sağlama hizmeti örneği otomatik sağlamayı desteklemek için kullanılıyor varsayacağız.

Ne zaman bir cihaz, başlangıçta otomatik sağlama yoluyla, onu başlatır-up, sağlanan ve sağlama hizmeti ile iletişim kurar. Sağlama hizmeti içinde kimlik bilgisi olarak cihazın yaprak sertifikayı kullanarak bir IOT hub'a bir cihaz kimliği oluşturmadan önce bir kimlik denetimi gerçekleştirerek yanıt verir. Sağlama hizmeti, cihazın hangi IOT hub'a atanır ve cihaz kimlik doğrulaması ve IOT hub'ına bağlanmak için kendi yaprak sertifikayı ardından kullanır. daha sonra bildirir. 

Yeni bir yaprak sertifikayı cihaza alındı sonra bağlanmak için yeni bir sertifika kullandığından, artık IOT hub'ına bağlanabilir. IOT hub'ı yalnızca eski sertifika cihazla tanır. Cihazın bağlantı denemesi sonucunu bir "yetkisiz" bağlantı hatası olacaktır. Bu hatayı gidermek için hesabı cihazın yeni Yaprak sertifikası için cihaz kayıt girişini güncelleştirmeniz gerekir. Daha sonra sağlama hizmeti, cihaz sağlama, gerektiği gibi IOT Hub cihaz kayıt defteri bilgilerini güncelleştirebilirsiniz. 

Bu bağlantı hatası için olası istisnalardan biri, burada oluşturduğunuz bir senaryo olacak bir [kayıt grubu](concepts-service.md#enrollment-group) cihaz sağlama hizmeti. Cihazın sertifikanın güven zincirinde kök veya ara sertifika sıralı değil, yeni sertifika kayıt grubundaki tanımlanan güven zinciri parçası ise bu durumda, ardından cihaz tanınır. Bir güvenlik ihlali için tepki olarak bu senaryo ortaya çıkarsa, ihlal olarak kabul edilir belirli cihaz sertifikaları grubunda en az bloke listeye almalıdır. Daha fazla bilgi için [kara belirli cihazlara bir kayıt grubundaki](https://docs.microsoft.com/azure/iot-dps/how-to-revoke-device-access-portal#blacklist-specific-devices-in-an-enrollment-group).

Toplu sertifika için kayıt girişlerini güncelleştirme üzerinde gerçekleştirilir **kayıtları Yönet** sayfası. Bu sayfaya erişmek için şu adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com) ve kayıt girdisi cihazınız için IOT Hub cihazı sağlama hizmeti örneğine gidin.

2. **Kayıtları yönetme**'ye tıklayın.

    ![Kayıtları yönetme](./media/how-to-roll-certificates/manage-enrollments-portal.png)


Nasıl kayıt girişi güncelleştirme işlemek, bireysel kayıtlar ya da grup kayıtları kullanmakta olduğunuz üzerinde bağlıdır. Ayrıca önerdiği yordamları olup, sertifikalar bir güvenlik ihlali ya da sertifika süre sonu nedeniyle bölgelerimizde bağlı olarak farklılık gösterir. Aşağıdaki bölümlerde, bu güncelleştirmeleri nasıl ele alınacağını açıklar.


## <a name="individual-enrollments-and-security-breaches"></a>Bireysel kayıtlar ve güvenlik ihlallerini

Yanıt olarak bir güvenlik ihlali sertifikaları bölgelerimizde, geçerli sertifika hemen silen aşağıdaki yaklaşımı kullanmanız gerekir:

1. Tıklayın **bireysel kayıtlar**, listesinde kayıt kimliği girişine tıklayın. 

2. Tıklayın **silme geçerli sertifika** düğmesine tıklayın ve ardından kayıt girişini yüklenmek üzere yeni bir sertifika seçmek için klasör simgesine tıklayın. Tıklayın **Kaydet** bittiğinde.

    Her ikisi de aşılırsa birincil ve ikincil sertifika için şu adımları tamamlanmalıdır.

    ![Bireysel kayıtlar yönetme](./media/how-to-roll-certificates/manage-individual-enrollments-portal.png)

3. Güvenliği aşılmış sertifikanın sağlama hizmetinden kaldırıldıktan sonra onun için bir cihaz kaydı var olduğu sürece IOT hub'ına cihaz bağlantılarını yapmak için sertifika hala kullanılabilir. Bu iki yolla karşılayabilirsiniz: 

    İlk yol, el ile IOT hub'ınıza gidin ve hemen güvenliği aşılmış sertifikayla ilişkili cihaz kaydını kaldırmak için olacaktır. Cihaz güncelleştirilmiş bir sertifika ile yeniden sağlar, yeni bir cihaz kaydı oluşturulur.     

    ![IOT hub cihaz kaydı Kaldır](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol aynı IOT hub'a cihazı yeniden sağlamak için destek çıkış kullanmak olabilir. Bu yaklaşım, IOT hub'ında cihaz kaydı için sertifikayı değiştirmek için kullanılabilir. Daha fazla bilgi için [cihazları yeniden sağlamak nasıl](how-to-reprovision.md).

## <a name="individual-enrollments-and-certificate-expiration"></a>Bireysel kayıtlar ve sertifika süre sonu

Sertifika süre sonu işlemek için sertifikaları bölgelerimizde, ikincil sertifika yapılandırması gibi sağlama girişiminde cihazlar için kapalı kalma süresini azaltmak için kullanmanız gerekir.

Daha sonra ikincil sertifika aynı zamanda, süre sonu yaklaştığında ve geri alınması gerekir, birincil yapılandırması kullanarak döndürebilirsiniz. Bu şekilde birincil ve ikincil sertifikaları arasında döndürme sağlama girişiminde cihazlar için kapalı kalma süresini azaltır.


1. Tıklayın **bireysel kayıtlar**, listesinde kayıt kimliği girişine tıklayın. 

2. Tıklayın **ikincil sertifika** ve ardından kayıt girişini yüklenmek üzere yeni bir sertifika seçmek için klasör simgesine tıklayın. **Kaydet**’e tıklayın.

    ![İkincil sertifika kullanılarak tek tek kayıtları Yönet](./media/how-to-roll-certificates/manage-individual-enrollments-secondary-portal.png)

3. Daha sonra birincil sertifikanın süresi dolduğunda, geri dönün ve birincil bu sertifikayı silmek **silme geçerli sertifika** düğmesi.

## <a name="enrollment-groups-and-security-breaches"></a>Kayıt grupları ve güvenlik ihlallerini

Yanıt olarak bir güvenlik ihlali bir grup kaydı güncelleştirmek için geçerli bir kök CA veya ara sertifika hemen siler aşağıdaki yaklaşımlardan birini kullanmanız gerekir.

#### <a name="update-compromised-root-ca-certificates"></a>Güvenliği aşılmış kök CA sertifikaları güncelleştirme

1. Tıklayın **sertifikaları** cihaz sağlama hizmeti örneğinizi için sekmesinde.

2. Güvenliği aşılmış sertifika listesinde tıklayın ve ardından **Sil** düğmesi. Sertifika adı girerek silme işlemini onaylamak ve tıklayın **Tamam**. Bu işlem, tüm riskli sertifikaları için yineleyin.

    ![Kök CA sertifikasını Sil](./media/how-to-roll-certificates/delete-root-cert.png)

3. Özetlenen adımları [doğrulanmış CA sertifikalarını yapılandırma](how-to-verify-certificates.md) ekleme ve yeni bir kök CA sertifika doğrulama.

4. Tıklayın **kayıtları Yönet** cihaz sağlama hizmeti örneğinizi için sekmesinde ve tıklayın **kayıt grupları** listesi. Kayıt grubu adınız listede tıklayın.

5. Tıklayın **CA sertifikası**ve yeni kök CA sertifikanızı seçin. Daha sonra **Kaydet**'e tıklayın. 

    ![Yeni kök CA sertifikasını seçin](./media/how-to-roll-certificates/select-new-root-cert.png)

6. Güvenliği aşılmış sertifikanın sağlama hizmetinden kaldırıldıktan sonra sertifikayı IOT hub'ına cihaz bağlantılarını cihaz kayıtları için mevcut olduğu sürece var. olmak için hala kullanılabilir. Bu iki yolla karşılayabilirsiniz: 

    İlk yol, el ile IOT hub'ınıza gidin ve hemen güvenliği aşılmış sertifikayla ilişkili cihaz kaydını kaldırmak için olacaktır. Cihazlarınızı tekrar güncelleştirilmiş sertifikalarla sağladığınızda, her biri için yeni bir cihaz kaydı oluşturulur.     

    ![IOT hub cihaz kaydı Kaldır](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol aynı IOT hub'ına cihazlarınızı yeniden sağlamak için destek çıkış kullanmak olabilir. Bu yaklaşım, IOT hub'ında cihaz kayıtları için sertifikaları değiştirmek için kullanılabilir. Daha fazla bilgi için [cihazları yeniden sağlamak nasıl](how-to-reprovision.md).



#### <a name="update-compromised-intermediate-certificates"></a>Güvenliği aşılmış Ara sertifikaları güncelleştirme

1. Tıklayın **kayıt grupları**ve ardından listedeki grubunun adına tıklayın. 

2. Tıklayın **ara sertifika**, ve **silme geçerli sertifika**. Kayıt grubu için yüklenmek üzere yeni Ara Sertifika gitmek için klasör simgesine tıklayın. Tıklayın **Kaydet** işiniz bittiğinde. Her ikisi de aşılırsa hem birincil ve ikincil sertifika için şu adımları tamamlanmalıdır.

    Bu yeni Ara Sertifika sağlama hizmeti zaten eklenmiş bir doğrulanmış kök CA sertifikası tarafından imzalanması gerekir. Daha fazla bilgi için [X.509 sertifikaları](concepts-security.md#x509-certificates).

    ![Bireysel kayıtlar yönetme](./media/how-to-roll-certificates/enrollment-group-delete-intermediate-cert.png)


3. Güvenliği aşılmış sertifikanın sağlama hizmetinden kaldırıldıktan sonra sertifikayı IOT hub'ına cihaz bağlantılarını cihaz kayıtları için mevcut olduğu sürece var. olmak için hala kullanılabilir. Bu iki yolla karşılayabilirsiniz: 

    İlk yol, el ile IOT hub'ınıza gidin ve hemen güvenliği aşılmış sertifikayla ilişkili cihaz kaydını kaldırmak için olacaktır. Cihazlarınızı tekrar güncelleştirilmiş sertifikalarla sağladığınızda, her biri için yeni bir cihaz kaydı oluşturulur.     

    ![IOT hub cihaz kaydı Kaldır](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    İkinci yol aynı IOT hub'ına cihazlarınızı yeniden sağlamak için destek çıkış kullanmak olabilir. Bu yaklaşım, IOT hub'ında cihaz kayıtları için sertifikaları değiştirmek için kullanılabilir. Daha fazla bilgi için [cihazları yeniden sağlamak nasıl](how-to-reprovision.md).


## <a name="enrollment-groups-and-certificate-expiration"></a>Kayıt grupları ve sertifika süre sonu

Sertifika süre sonu işlemek için sertifikaları sunuyoruz, ikincil sertifika yapılandırmasını gibi sağlama girişiminde cihazlar için kesinti süresi olmaksızın emin olmak için kullanmanız gerekir.

Daha sonra ikincil sertifika aynı zamanda, süre sonu yaklaştığında ve geri alınması gerekir, birincil yapılandırması kullanarak döndürebilirsiniz. Bu şekilde birincil ve ikincil sertifikaları arasında döndürme sağlama girişiminde cihazlar için kapalı kalma süresi sağlar. 

#### <a name="update-expiring-root-ca-certificates"></a>Süresi dolan kök CA sertifikaları güncelleştirme

1. Özetlenen adımları [doğrulanmış CA sertifikalarını yapılandırma](how-to-verify-certificates.md) ekleme ve yeni bir kök CA sertifika doğrulama.

2. Tıklayın **kayıtları Yönet** cihaz sağlama hizmeti örneğinizi için sekmesinde ve tıklayın **kayıt grupları** listesi. Kayıt grubu adınız listede tıklayın.

3. Tıklayın **CA sertifikası**ve altında yeni kök CA sertifikanızı seçin **ikincil sertifika** yapılandırma. Daha sonra **Kaydet**'e tıklayın. 

    ![Yeni kök CA sertifikasını seçin](./media/how-to-roll-certificates/select-new-root-secondary-cert.png)

4. Daha sonra birincil sertifikasının süresi doldu, tıklayın **sertifikaları** cihaz sağlama hizmeti örneğinizi için sekmesinde. Süresi dolan sertifikanın listesinde tıklayın ve ardından **Sil** düğmesi. Sertifika adı girerek silme işlemini onaylamak ve tıklayın **Tamam**.

    ![Kök CA sertifikasını Sil](./media/how-to-roll-certificates/delete-root-cert.png)



#### <a name="update-expiring-intermediate-certificates"></a>Süresi dolan Ara sertifikaları güncelleştirme


1. Tıklayın **kayıt grupları**ve listedeki grubunun adına tıklayın. 

2. Tıklayın **ikincil sertifika** ve ardından kayıt girişini yüklenmek üzere yeni bir sertifika seçmek için klasör simgesine tıklayın. **Kaydet**’e tıklayın.

    Bu yeni Ara Sertifika sağlama hizmeti zaten eklenmiş bir doğrulanmış kök CA sertifikası tarafından imzalanması gerekir. Daha fazla bilgi için [X.509 sertifikaları](concepts-security.md#x509-certificates).

   ![İkincil sertifika kullanılarak tek tek kayıtları Yönet](./media/how-to-roll-certificates/manage-enrollment-group-secondary-portal.png)

3. Daha sonra birincil sertifikanın süresi dolduğunda, geri dönün ve birincil bu sertifikayı silmek **silme geçerli sertifika** düğmesi.


## <a name="reprovision-the-device"></a>Cihaz yeniden hazırlayın

Hem cihaz hem de cihaz sağlama hizmeti sertifikası alınıyor sonra cihazın kendisi cihaz sağlama hizmeti iletişim kurarak yeniden sağlamak. 

Yeniden sağlamak için bir kolayca programlama cihazların cihaz IOT hub'a bağlanmaya çalışmasını "yetkisiz" hatası alırsa sağlama akışını gitmek için sağlama hizmetiyle bağlantı kurmak için cihaz programı için yoludur.

Eski ve yeni sertifikalar için kısa bir çakışma için geçerli ve cihazlara komut gönderme için IOT hub'ı kullanmak sağlama hizmetine yeniden kaydetmek IOT Hub bağlantı bilgilerini güncelleştirmek bunun başka bir yoludur. Her cihaz komutları farklı işleyebileceğinden komutu çağrılan ne yapılacağını bildiğiniz için cihaz programı gerekecektir. Cihazınızı IOT hub'ı aracılığıyla komutu birkaç yolu vardır ve kullanmanızı öneririz [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) veya [işleri](../iot-hub/iot-hub-devguide-jobs.md) işlemini başlatmak için.

Çıkış tamamlandıktan sonra cihazları yeni sertifikalarını kullanarak IOT hub'a bağlanmak mümkün olacaktır.


## <a name="blacklist-certificates"></a>Kara liste sertifikaları

Bir güvenlik ihlali yanıt olarak, bir cihaz sertifika kara gerekebilir. Bir cihaz sertifika kara listeye hedef cihaz/sertifika kayıt girişini devre dışı bırakın. Cihazları kara daha fazla bilgi için bkz. [Yönet kayıt silmeyi](how-to-revoke-device-access-portal.md) makalesi.

Başka bir kayıt girişi bir parçası olarak etkinleştirilse bile sertifikalar bölümü devre dışı kayıt girdisi, bir IOT hub'ı kullanarak kaydetme girişimleri başarısız olur gibi bir sertifika dahil olduktan sonra.
 



## <a name="next-steps"></a>Sonraki adımlar

- Cihaz sağlama hizmeti X.509 sertifikaları hakkında daha fazla bilgi için bkz: [güvenlik](concepts-security.md) 
- Kavram, elinde Azure IOT Hub cihazı sağlama hizmeti ile X.509 CA sertifikalarının yapma hakkında bilgi edinmek için [sertifikalarını doğrulama](how-to-verify-certificates.md)
- Kayıt grubu oluşturmak için portalı kullanma hakkında bilgi edinmek için [Azure portalı ile cihaz kayıtlarını yönetme](how-to-manage-enrollments.md).










