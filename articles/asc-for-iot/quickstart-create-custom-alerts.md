---
title: IOT Önizleme için Azure Güvenlik Merkezi'nde özel uyarı oluşturma | Microsoft Docs
description: Oluşturun ve IOT için Azure Güvenlik Merkezi için özel cihaz uyarıları atayın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 12559af013d49e557ba0132bef24867867745c16
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618037"
---
# <a name="quickstart-create-custom-alerts"></a>Hızlı Başlangıç: Özel uyarılar oluşturabilirsiniz

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Özel güvenlik grupları ve Uyarıları kullanarak uçtan uca güvenlik bilgilerini ve IOT çözümünüzü arasında daha iyi güvenlik emin olmak için kategorik cihaz bilgi tüm avantajlarından yararlanın. 

## <a name="why-use-custom-alerts"></a>Özel uyarılar neden kullanmalısınız? 

IOT cihazlarınızı en iyi bildiğiniz.

Müşteriler, tam olarak beklenen cihaz davranışlarını anlamak için herhangi bir sapma uyarısında bekleniyordu, normal davranış ve IOT için Azure Güvenlik Merkezi (ASC) bu anlayış bir cihaz davranışı ilke Çevir sağlar.

## <a name="security-groups"></a>Güvenlik grupları

Güvenlik grupları, cihazların mantıksal gruplar tanımlamanıza olanak sağlar ve güvenlik durumunu merkezi bir biçimde yönetin.

Bu gruplar, cihazları için belirli donanımlarla cihazlarında dağıtılan belirli bir konuma ya da herhangi bir grup belirli ihtiyaçlarınıza uygun temsil edebilir.

Güvenlik grupları adlı bir güvenlik modül ikizi tag özelliği tarafından tanımlanan **IDAP**. Bir cihazın güvenlik grubunu değiştirmek için bu özelliğin değerini değiştirin.  

Varsayılan olarak, IOT Hub'ında her IOT çözümü adlı bir güvenlik grubu vardır. **varsayılan**.

Cihazlarınızı mantıksal kategoriler altında gruplandırmak için güvenlik grupları kullanın. Grup oluşturduktan sonra en etkili uçtan uca çözüm için tercih ettiğiniz özel uyarıları için doğru atayın. 

## <a name="customize-an-alert"></a>Bir uyarı özelleştirme

1. IOT Hub'ınızı açın. 
2. Tıklayın **özel uyarıları** içinde **güvenlik** bölümü. 
3. Özelleştirme için uygulanmasını istediğiniz bir güvenlik grubu seçin. 
4. Tıklayın **özel uyarı Ekle** 
5. Özel bir uyarı davranışı, açılır listeden seçin. 
6. Gerekli özellikleri Düzenle'ye tıklayın **Tamam**.
7. Tıkladığınızdan emin olun **Kaydet**. Yeni uyarı kaydetmeden uyarı IOT hub'ı kapatın başlatıldığında silinir.

 
## <a name="alerts-available-for-customization"></a>Uyarıları özelleştirme için kullanılabilir

Aşağıdaki tabloda, özelleştirme için kullanılabilir uyarıların bir özetini sağlar.

| Severity | Ad                                                                                                    | Veri Kaynağı | Açıklama                                                                                                                                     |
|----------|---------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Düşük      | Özel uyarı - cihaz iletilerini AMQP protokolünü de buluta sayısı izin verilen aralık içinde değil          | IoT Hub     | Bir zaman penceresinde cihaz iletileri (AMQP protokolünü) buluta miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Özel uyarı - reddedilen buluta AMQP protokolünü cihaza iletileriyle sayısı izin verilen aralık içinde değil | IoT Hub     | Bir zaman penceresinde cihaz tarafından reddedilen cihaz iletileri (AMQP protokolünü) buluta miktarını yapılandırılmış izin verilen aralığın değil |
| Düşük      | Özel uyarı - bulut iletileri AMQP protokolünü cihaz sayısı izin verilen aralık içinde değil          | IoT Hub     | Cihaz bulut iletilerini (AMQP protokolünü) bir zaman penceresinde miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Çağıran özel uyarı - doğrudan yöntem sayısı izin verilen aralık içinde değil                              | IoT Hub     | Doğrudan yöntem miktarı pencere yapılandırılmış izin verilen aralığın değil bir sürede çağırır                                                     |
| Düşük      | Özel uyarı - dosya yüklemeleri sayısı izin verilen aralık içinde değil                                       | IoT Hub     | Bir zaman penceresinde dosya yüklemeleri miktarını yapılandırılmış izin verilen aralığın değil                                                              |
| Düşük      | Özel uyarı - cihaz iletilerini HTTP protokolünde buluta sayısı izin verilen aralık içinde değil          | IoT Hub     | Bir zaman penceresinde cihaz iletileri (HTTP Protokolü) buluta miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Özel uyarı - cihaz iletilerini HTTP protokolünde reddedilen buluta sayısı izin verilen aralık içinde değil | IoT Hub     | Bir zaman penceresinde cihaz tarafından reddedilen cihaz iletileri (HTTP Protokolü) buluta miktarını yapılandırılmış izin verilen aralığın değil |
| Düşük      | Özel uyarı - bulut iletileri HTTP protokolü için cihaz sayısı izin verilen aralık içinde değil          | IoT Hub     | Cihaz bulut iletilerini (HTTP Protokolü), bir zaman penceresinde miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Özel uyarı - cihaz iletilerini MQTT protokolünü de buluta sayısı izin verilen aralık içinde değil          | IoT Hub     | Bir zaman penceresinde cihaz iletileri (MQTT protokolünü) buluta miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Özel uyarı - reddedilen buluta MQTT protokolünü cihaza iletileriyle sayısı izin verilen aralık içinde değil | IoT Hub     | Bir zaman penceresinde cihaz tarafından reddedilen cihaz iletileri (MQTT protokolünü) buluta miktarını yapılandırılmış izin verilen aralığın değil |
| Düşük      | Özel uyarı - bulut iletileri MQTT protokolünü cihaz sayısı izin verilen aralık içinde değil          | IoT Hub     | Cihaz bulut iletilerini (MQTT protokolünü) bir zaman penceresinde miktarını yapılandırılmış izin verilen aralığın değil                                  |
| Düşük      | Özel uyarı - komut kuyruğu temizler sayısı izin verilen aralık içinde değil                               | IoT Hub     | Komut kuyruğu miktarı pencere yapılandırılmış izin verilen aralığın değil bir sürede temizler                                                      |
| Düşük      | Özel uyarı - twin güncelleştirme sayısı izin verilen aralık içinde değil                                       | IoT Hub     | Bir zaman penceresinde ikizi güncelleştirmeleri miktarını yapılandırılmış izin verilen aralığın değil                                                              |
| Düşük      | Özel uyarı - yetkisiz işlemlerin sayısı izin verilen aralık içinde değil                            | IoT Hub     | Bir zaman penceresinde yetkisiz işlemlere miktarını yapılandırılmış izin verilen aralığın değil                                                   |
| Düşük      | Özel uyarı - izin verilen aralık içinde değil etkin bağlantı sayısı                                        | Aracı       | Etkin bağlantılar bir zaman penceresinde miktarını yapılandırılmış izin verilen aralığın değil                                                        |
| Düşük      | Özel uyarı - izin verilmeyen bir IP için giden bağlantı oluşturuldu                              | Aracı       | İzin verilmeyen bir IP için bir giden bağlantı oluşturuldu                                                                                  |
| Düşük      | Özel uyarı - yerel başarısız oturum açma sayısı izin verilen aralıkta değil.                                | Aracı       | Bir zaman penceresinde başarısız yerel oturum açma bilgileri miktarını yapılandırılmış izin verilen aralığın değil                                                       |
| Düşük      | Özel uyarı - izin verilmiyor bir kullanıcının oturum açma                                                      | Aracı       | Cihaza izin yerel bir kullanıcı oturum açanlar                                                                                        |
| Düşük      | Özel uyarı - izin verilmiyor bir işlemin yürütülmesi                                               | Aracı       | Cihazda verilmeyen bir işlemin yürütülmesi |          |

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik aracı dağıtma hakkında bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Güvenlik aracı dağıtma](how-to-deploy-agent.md)
