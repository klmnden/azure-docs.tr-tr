---
title: Azure IOT Hub cihazı sağlama hizmeti - TPM kanıtlama
description: Bu makalede, IOT cihaz sağlama Hizmeti'ni kullanarak TPM kanıtlama flow kavramsal bir genel bakış sağlar.
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 07c5dbce0b98d1c197164f4fc77682f78ede57f0
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59048886"
---
# <a name="tpm-attestation"></a>TPM kanıtlama

IOT Hub cihazı sağlama hizmeti, sıfır dokunma cihaz sağlama belirtilen bir IOT hub'ı yapılandırmak için kullandığınız bir IOT Hub için bir yardımcı hizmettir. Cihaz sağlama hizmeti ile güvenli bir şekilde milyonlarca cihaz sağlayabilirsiniz.

Kimlik kanıtlama işlemini, kullanırken bu makalede açıklanır bir [TPM](./concepts-device.md). TPM için Güvenilir Platform Modülü'anlamına gelir ve bir donanım güvenlik modülü (HSM) türüdür. Bu makalede, ayrık bir üretici yazılımı kullanan veya TPM tümleşik varsayılır. Yazılım benzetilmiş TPM'ler prototip oluşturma için çok uygundur, bunlar test aynı güvenlik düzeyine sahip ayrık, üretici yazılımı sağlamak için değil veya veya tümleşik TPM'ler yapın. Üretim ortamında yazılım TPM'ler kullanarak önermiyoruz. Tpm'lerin türleri hakkında daha fazla bilgi için bkz. [TPM kısa bir giriş](https://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf).

Bu makalede, yalnızca HMAC anahtar desteği ve onay anahtarları ile TPM 2.0 kullanan cihazlar için geçerlidir. X.509 sertifikaları kullanarak kimlik doğrulaması için cihazlar için değildir. TPM, bir endüstri genelinde, ISO standart Trusted Computing Group ve daha fazla bilgi edinebilirsiniz TPM hakkında [tam TPM 2.0 belirtimi](https://trustedcomputinggroup.org/tpm-library-specification/) veya [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html). Bu makalede ayrıca genel ve özel anahtar çiftleri ve şifreleme için nasıl kullanıldığı hakkında bilgi sahibi olduğunuz varsayılır.

Cihaz sağlama hizmeti cihaz SDK'ları, bu makalede açıklanan her şeyi işleyin. Cihazlarınıza SDK'ları kullanıyorsanız ek bir şey uygulamak gerek yoktur. Bu makalede, kavramsal olarak, cihaz sağlar ve bunu neden olduğu güvenli hale getirdiğinizde TPM güvenlik yonganız ile neler olduğunu anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış

TPM'ler onay anahtarını (EK) adlı bir şey güvenli güven kökü olarak kullanın. TPM'ye EK benzersizdir ve temelde değiştirerek yeni bir cihaz değiştirir.

Başka türde bir TPM'ler depolama kök anahtarı (SRK) adlı olan anahtar yoktur. TPM sahipliğini gerçekleştirildikten sonra TPM'nin sahibi tarafından bir SRK oluşturulabilir. TPM sahipliğini deyiş, "biri bir parola HSM'de ayarlar." TPM özgü yoludur Bir TPM cihazı için yeni bir sahip satılırsa yeni sahip yeni bir SRK oluşturmak için TPM sahipliğini alabilir. Önceki dosya sahibi, TPM kullanamazsınız yeni SRK oluşturmayı sağlar. SRK'nin TPM sahibine benzersiz olduğu için veri sahibi için TPM kendisini mühürlenecek SRK'nin kullanılabilir. SRK'nin sahibi kendi anahtarlarını depolamak bir korumalı alan ve TPM ve cihaz satılırsa revocability erişim sağlar. Yeni bir Eve taşıma gibi olduğu: sahipliği alma kapılar yer alan kilitlerin değiştirmek ve tüm mobilyası önceki sahipleri (SRK) sol yok etme, ancak (EK) ev adresi değiştiremez.

Bir cihaz ayarlama ve kullanılmaya hazır ayarlandıktan sonra bir EK hem de bir SRK kullanılmaya hazır olacaktır.

![TPM sahipliğini](./media/concepts-tpm-attestation/tpm-ownership.png)

TPM sahipliğini hakkında bir Not: TPM sahipliğini TPM üretici, kullanılan TPM araç kümesini ve cihaz işletim sistemi dahil olmak üzere pek çok üzerinde bağlıdır. Sahipliği sisteminize ilgili yönergeleri izleyin.

Cihaz sağlama hizmeti (EK_pub) EK ortak bölümünü tanımlamak ve cihaz kaydetmek için kullanır. Cihaz satıcısı EK_pub son bir test veya üretim sırasında okuyabilir ve böylece cihaz sağlamayı bağlandığında tanınması EK_pub sağlama hizmetine yükleyin. Cihaz sağlama hizmeti SRK ya da sahibi "TPM temizleme" Müşteri verilerini siler ancak EK (ve diğer satıcı verileri) korunur ve sağlamayı bağlandığında, cihazın hala cihaz sağlama hizmeti tarafından tanınması için kontrol yapmaz.

## <a name="detailed-attestation-process"></a>Ayrıntılı kanıtlama işlem

Bir cihazla bir TPM cihazı sağlama hizmeti için ilk kez bağlandığında, hizmet ilk sağlanan EK_pub kayıt listesinde depolanan EK_pub karşı denetler. EK_pubs eşleşmiyorsa, cihaz sağlama için izin verilmiyor. EK_pubs eşleşiyorsa, bu hizmetin kimliğini kanıtlamak için kullanılan güvenli bir güçlük nonce zor aracılığıyla EK özel bölümünü sahipliğini kanıtlamak için cihaz ardından gerektirir. Cihaz sağlama hizmeti nonce oluşturur ve SRK'nin ve ikisi için de ilk kayıt araması sırasında cihaz tarafından sağlanan EK_pub şifreler. TPM, EK özel bölümünü güvende tutar. Bu sahteciliği engeller ve güvenli bir şekilde yetkili cihazları için sağlanan SAS belirteçleri sağlar.

Şimdi kanıtlama işlemini ayrıntılı yol.

### <a name="device-requests-an-iot-hub-assignment"></a>Cihaz IOT hub'ı atama istekleri

İlk cihaz için cihaz sağlama hizmeti bağlanır ve sağlamayı ister. Böylece, cihaz kayıt Kimliğini, bir kimlik kapsamı ve EK_pub ve SRK_pub hizmetiyle TPM sağlar. Hizmet, şifrelenmiş nonce cihaza geri gönderir ve nonce şifresini çözmek ve yeniden bağlanın ve sağlamayı tamamlamak için bir SAS belirteci oturum kullanan cihazın ister.

![Cihaz isteklerini sağlama](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce sınaması

Cihaz nonce alır ve SRK ve EK özel kısımlarına nonce TPM'ye şifresini çözmek için kullanır; Yeni bir sahip TPM sahipliğini sürerse değiştirebilirsiniz SRK için sabittir, EK nonce şifreleme temsilciler güven sırası.

![Nonce şifresini çözme](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Geçici öğeyi doğrulayabilirsiniz ve kimlik bilgilerini alma

Cihaz şifresi nonce kullanarak bir SAS belirteci oturum açın ve yeniden imzalı SAS belirteci kullanarak cihaz sağlama hizmeti için bağlantı. Tamamlanan Nonce sınama ile cihaz sağlama hizmeti sağlar.

![Cihaz bağlantı EK sahipliğini doğrulamak için cihaz sağlama Hizmeti'ne yeniden kurar.](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz IOT Hub'ına bağlanır ve cihazlarınızın anahtarları güvenli şekilde depolanan Bilgi Bankası'nda güvenli tutun. Artık cihaz sağlama hizmeti güvenli bir şekilde TPM kullanarak bir cihazın kimliğini doğrular nasıl bildiğinize göre daha fazla bilgi için aşağıdaki makalelere göz atın:

* [Otomatik sağlama içindeki tüm kavramlar hakkında bilgi edinin](./concepts-auto-provisioning.md)
* [Otomatik sağlama kullanmaya başlama](./quick-setup-auto-provision.md) akışını halletmeniz için SDK'ları kullanarak.
