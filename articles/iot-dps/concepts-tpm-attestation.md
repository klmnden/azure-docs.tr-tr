---
title: Azure IOT Hub cihaz hizmeti - TPM kanıtı sağlama
description: Bu makale IOT cihaz sağlama hizmeti ile TPM kanıtlama akış kavramsal genel bakış sağlar.
services: iot-dps
keywords: ''
author: nberdy
ms.author: nberdy
ms.date: 04/23/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: dec024c5c23bf8c628457127af57b8d18800660e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tpm-attestation"></a>TPM kanıtlama

IOT Hub cihaz hizmeti sağlama, IOT Hub'de belirtilen bir IOT hub'ına sağlama zero touch Cihazınızı yapılandırmak üzere kullanmak için bir yardımcı hizmetidir. Cihaz sağlama hizmeti ile milyonlarca cihaza güvenli bir şekilde sağlayabilirsiniz.

Bu makalede kimlik kanıtlama işlemi kullanırken anlatılmaktadır bir [TPM](./concepts-device.md). TPM Güvenilir Platform Modülü için anlamına gelir ve bir donanım güvenlik modülü (HSM) türüdür. Bu makale, ayrık, üretici yazılımı kullanan veya TPM tümleşik varsayar. Yazılım benzetilmiş TPM'ler için prototipi oluşturulurken uymaktadır veya sınama, bunlar aynı düzeyde güvenlik ayrık bellenim olarak sağlamak için değil, veya tümleşik TPM'ler yapın. Üretim TPM'ler yazılımda kullanarak önermiyoruz. [Daha fazla bilgi edinin](http://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf) TPM'ler türleri hakkında.

Bu makalede, yalnızca TPM 2.0 HMAC anahtar desteği ve onay anahtarları kullanarak cihazları için geçerlidir. X.509 sertifikaları kullanarak kimlik doğrulaması için cihazları için değil. TPM olan bir sektör çapında, ISO standart Trusted Computing Group ve daha fazla bilgiyi TPM hakkında [tam TPM 2.0 belirtimi](https://trustedcomputinggroup.org/tpm-library-specification/) veya [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html). Bu makalede, ayrıca ortak ve özel anahtar çiftlerini ve şifreleme için nasıl kullanıldığı hakkında bilgi sahibi olduğunu varsayar.

Aygıt hizmeti sağlama cihaz SDK'ları sizin için bu makalede açıklanan her şeyi işleyin. SDK'ları aygıtlarınızda kullanıyorsanız, ek bir şey uygulamak, gerek yoktur. Bu makalede, aygıt sağlar ve bunu neden olduğunu güvenli hale getirdiğinizde, TPM güvenlik yongası ile neler olup bittiğini kavramsal anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış

TPM'ler onay anahtarını (EK) adlı bir şey güven güvenli kök olarak kullanın. TPM'ye EK benzersizdir ve temelde değiştirme yeni bir cihaz değiştirir.

Başka türde bir depolama kök anahtarı (SRK) TPM'ler adlı olan anahtar yok. TPM sahipliğini gerçekleştirildikten sonra bir SRK TPM'nin sahibi tarafından oluşturulabilir. TPM sahipliğini bildiren, "birisi bir parola HSM'de ayarlar." TPM özgü yoludur TPM aygıt için yeni bir sahibi satılırsa, yeni sahibi yeni SRK oluşturmak için TPM sahipliğini alabilir. Önceki sahibi TPM kullanamazsınız yeni SRK oluşturulmasını sağlar. SRK TPM'nin sahibinin benzersiz olduğundan, SRK sahibi için TPM kendisini içine veri paketini mühürlemek için kullanılabilir. SRK sahibi kendi anahtarları depolamak için bir korumalı alan ve TPM ve cihaz satılırsa erişim revocability sağlar. Yeni bir ev taşıma gibi değil: sahipliği alma kapı kilitlendiğinde değiştirme ve önceki sahibe (SRK) kalan tüm Mobilya yok etme, ancak (EK) ev adresini değiştiremezsiniz.

Sonra bir aygıtı ayarlandı ve kullanıma hazır bir EK ve kullanılabilir bir SRK olması.

![TPM sahipliğini](./media/concepts-tpm-attestation/tpm-ownership.png)

TPM sahipliğini üzerinde bir Not: bir TPM sahipliğini TPM üretici, kullanılan TPM'nin araçlar kümesi ve cihaz işletim sistemine dahil olmak üzere pek çok üzerinde bağlıdır. Sahipliği almak için sisteminize ilgili yönergeleri izleyin.

Cihaz sağlama hizmeti (EK_pub) EK ortak parçası tanımlamak ve cihazları kaydetmek için kullanır. Cihaz satıcısından EK_pub üretim veya son sınama sırasında okuma ve böylece sağlamak bağlandığında cihaz tanınan EK_pub sağlama hizmete karşıya yükleme. Aygıt hizmeti sağlama SRK veya sahibi, böylece "TPM'yi temizleme" Müşteri verilerini siler ancak EK (ve diğer satıcı veriler) korunur ve sağlamak bağlandığında, cihaz yine cihaz sağlama hizmeti tarafından tanınması denetlemez.

## <a name="detailed-attestation-process"></a>Ayrıntılı kanıtlama işlemi

TPM sahip bir cihaz ilk sağlama aygıt hizmete bağlandığında, hizmet ilk sağlanan EK_pub kayıt listesinde depolanan EK_pub karşı denetler. EK_pubs eşleşmiyorsa, cihaz sağlamak için izin verilmiyor. EK_pubs eşleşiyorsa, bu hizmetin kimliğini kanıtlamak için kullanılan güvenli zor olduğu bir nonce challenge aracılığıyla EK özel bölümünü sahipliğini kanıtlamak için cihazın sonra gerektirir. Aygıt hizmeti sağlama nonce oluşturur ve SRK ve her ikisi de ilk kayıt çağrısı sırasında aygıt tarafından sağlanan EK_pub şifreler. TPM, EK özel bölümünü güvende tutar. Bu sahteciliği engeller ve SAS belirteci güvenli bir şekilde yetkili cihazları sağlanan sağlar.

Şimdi kanıtlama işlem ayrıntılı yol.

### <a name="device-requests-an-iot-hub-assignment"></a>Cihaz IOT hub'ı atama istekleri

İlk cihaz sağlama cihazı hizmetine bağlanır ve sağlamak ister. Bunu yaparken, cihaz kayıt kimliği, bir kimliği kapsamı EK_pub ve SRK_pub hizmetiyle TPM'nin sağlar. Hizmet geri cihaza şifrelenmiş nonce geçirir ve nonce şifresini çözmek ve yeniden bağlanma ve sağlama tamamlamak için bir SAS belirteci imzalamak için kullanan aygıta sorar.

![Aygıt isteklerini sağlama](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce sınama

Cihaz nonce alır ve nonce TPM'ye şifresini çözmek için EK ve SRK özel kısımları kullanır; Yeni sahibi TPM'nin sahipliğini sürerse değiştirebilirsiniz SRK için sabittir, EK nonce şifreleme temsilciler güven sırası.

![Nonce şifre çözme](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Nonce doğrulamak ve kimlik bilgilerini alma

Cihaz şifresi çözülmüş nonce kullanılarak bir SAS belirteci oturum ve cihaz sağlama imzalı SAS belirteci kullanarak hizmetine bağlantıyı yeniden kurmak. Tamamlanan Nonce sınama ile hizmet sağlamak için cihazın verir.

![Aygıt EK sahipliği doğrulamak için dağıtım noktaları için bağlantı yeniden kurar](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi cihaz IOT Hub'ına bağlanır ve aygıtlarınızın anahtarlarını güvenli bir şekilde saklanır bilgisinde güvenli tutun. Aygıt hizmeti sağlama güvenli bir şekilde TPM kullanarak bir cihazın kimliğini doğrular nasıl bildiğinize göre daha fazla bilgi için aşağıdaki makalelere denetleyin:

* [Otomatik sağlama içindeki tüm kavramları hakkında bilgi edinin](./concepts-auto-provisioning.md)
* [Otomatik sağlama kullanmaya başlamanıza](./quick-setup-auto-provision.md) akışını halletmeniz için SDK'ları kullanarak.
