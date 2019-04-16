---
title: Azure IOT hub'ı X.509 güvenlik kavramları | Microsoft Docs
description: Kavram X.509 değeri anlama - yetkilisi sertifikaları IOT cihaz üretim ve kimlik doğrulama sertifikası.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 3c7e1167b3326620863d35cb2d4b07235cbd5517
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571354"
---
# <a name="conceptual-understanding-of-x509-ca-certificates-in-the-iot-industry"></a>X.509 CA sertifikalarını IOT sektördeki kavramsal anlayış

Bu makalede, IOT cihaz üretim ve IOT hub'ına kimlik doğrulaması X.509 sertifika yetkilisi (CA) sertifikaları kullanarak değerini açıklanır. Tedarik hakkında bilgi içeren zincir Kurulum ve avantajları vurgulayın.

Bu makalede açıklanır:

* X.509 CA sertifikalarını nedir ve nasıl alacağınız

* X.509 CA sertifikanız IOT hub'ına kaydetme

* Nasıl bir üretimi ayarlamak tedarik zinciri X.509 CA tabanlı kimlik doğrulaması

* Nasıl X.509 CA imzalı cihazları IOT hub'a bağlama

## <a name="overview"></a>Genel Bakış

X.509 sertifika yetkilisi (CA) kimlik doğrulaması, cihazlar IOT hub cihaz kimliği oluşturulmasını ve yaşam döngüsü yönetimi tedarik zincirinde önemli ölçüde basitleştiren bir yöntem kullanılarak kimlik doğrulaması için kullanılan bir yaklaşımdır.

X.509 CA kimlik doğrulamasının ayırt etmenin bir yolunun bir öznitelik bir CA sertifikası, aşağı akış cihazlarla sahip bir-çok ilişkidir. Bu ilişki, X.509 CA sertifikası kez kaydederek IOT Hub'ına çok sayıda cihazın kaydını sağlar, bir cihazın bağlanabilmesi için önce Aksi takdirde cihaz benzersiz sertifikaları için her bir cihaz önceden kayıtlı olması gerekir. Bu bire çok ilişkisi ayrıca cihaz sertifika yaşam döngüsü yönetimi işlemleri basitleştirir.

X.509 CA kimlik doğrulamasının başka bir önemli özelliği, tedarik zinciri Lojistik basitleştirme değil. Her cihaz güven temeli olarak benzersiz bir gizli dizi gibi bir anahtar tutan cihazların güvenli kimlik doğrulaması gerektirir. Sertifika tabanlı kimlik doğrulaması, bu gizli dizi özel bir anahtardır. Akış üretim tipik bir cihaz birden fazla adımları ve koruyucuları içerir. Güvenli bir şekilde cihaz özel anahtarları arasında birden çok koruyucuları yönetme ve güven Bakımı zor ve pahalı. Sertifika yetkililerini kullanarak bunları cihaza özel anahtarla entrusting yerine her Emanetçisi şifreleme bir güven zinciri içine imzalama bu sorunu çözer. Her Emanetçisi cihazları kendi ilgili işlem adımı üretim akışın sırayla imzalar. Genel bir en uygun tedarik zinciri ile yerleşik sorumluluk şifreleme güven zinciri aracılığıyla sonucudur. Bunu, cihazları benzersiz özel anahtarlarına koruduğunuzda bu işlem çoğu güvenlik verir hatalarının ayıklanabileceğini belirtmekte yarar. Bu amaçla, biz kullanımı, donanım güvenli modülleri (HSM) dahili olarak, hiçbir zaman gün ışığı göreceği özel anahtarları oluşturma yeteneğine bağlantısını okumanızı tavsiye.

Bu makalede, cihaz bağlantısı, tedarik zinciri kurulumundan X.509 CA kimlik doğrulamasını kullanarak bir uçtan uca görünümünü sunar yapma sırasında pekiştirilmesine anlamak için gerçek dünya örneği kullanın.

## <a name="introduction"></a>Giriş

X.509 CA sertifikası, sahibi diğer sertifikaları imzalamak dijital bir sertifikadır. Bu dijital sertifikayı X.509 IETF'ın RFC 5280 standardına göre belirlenen standart biçimlendirme bir sertifika uyumlu olduğundan ve onun sahibi diğer sertifikaları imzalamak bir sertifika yetkilisi (CA) olduğundan.

X.509 CA'ın kullanımı somut bir örnek ile ilgili en iyi anlaşılmalıdır. Şirket X, bir oluşturucu akıllı-X-profesyonel yükleme için tasarlanan pencere öğeleri göz önünde bulundurun. Şirket X hem üretim hem de yükleme outsources. Bu akıllı-X-pencere öğeleri ve hizmet sağlayıcısı teknisyen Z yüklemek için üretmek için Fabrika-Y üretici daraltır. Şirket X istediği akıllı X Pencere doğrudan Fabrika-Y teknisyen-Z-yükleme için birlikte gelen olduğunu ve şirkete doğrudan bağlanan-daha fazla şirket X müdahalesi olmadan yükleme sonrasında X'lerin IOT Hub örneği. Bunu gerçekleştirmek için şirket X akıllı X Pencere hazırlamak için otomatik bağlantısı birkaç kez yapılan kurulum işlemleri tamamlamak gerekir. Uçtan uca senaryosu unutmayın, bu makalenin geri kalanında şu şekilde yapılandırılmıştır:

* X.509 CA sertifikası alın

* X.509 CA sertifikası IOT hub'ına kaydetme

* Bir sertifika güven zinciri içine oturum cihazlar

* Cihaz bağlantısı

## <a name="acquire-the-x509-ca-certificate"></a>X.509 CA sertifikası alın

Şirket X ortak kök sertifika yetkilisinden bir X.509 CA sertifikası satın almayı veya otomatik olarak imzalanan bir işlemle oluşturma seçeneğiniz vardır. Üzerinden uygulama senaryosuna bağlı olarak diğer bir seçenek en iyi olacaktır. Seçenek bağımsız olarak işlem bir ortak/özel anahtar çifti oluşturma ve ortak anahtar ile bir sertifika imzalama iki temel adımları kapsar.

![Akış X509CA sertifikaları oluşturmak için](./media/iot-hub-x509ca-concept/csr-flow.png)

Bu adımları gerçekleştirmek birleştiremiyorsa çeşitli hizmet sağlayıcılarıyla farklılık gösterir.

### <a name="purchasing-an-x509-ca-certificate"></a>X.509 CA sertifikası satın alma

Bir CA sertifikası satın alma, iyi bilinen bir kök CA Yasası bir güvenilen cihazları bağlandığında, IOT cihazları yasallığı için vouch için şirketlerin sahip avantajına sahiptir. Bunlar IOT hub'ına ilk bağlantı sonra üçüncü taraf ürünler veya hizmetler ile etkileşim kurmak için akıllı-X-pencere öğesi düşünüyorsanız, şirket X bu seçeneği seçersiniz.

X.509 CA sertifikası satın almak için şirket X bir kök Sertifika Hizmetleri Sağlayıcısı seçmelisiniz. ' % S'ifadesini 'Kök CA' için bir internet arama iyi müşteri adayları ortaya çıkarır. Kök CA ortak/özel anahtar çifti oluşturma ve bir sertifika imzalama isteği (CSR) için kendi hizmetlerini oluşturmayı, şirket X yönlendirecektir. Bir CSR bir sertifika yetkilisinden bir sertifika için uygulama resmi işlemidir. Bu satın alma sonucunu bir yetkilisi sertifikasına olarak kullanılmak üzere bir sertifikadır. X.509 sertifikaları yaygınlığını göz önünde bulundurulduğunda, sertifikayı standart olan RFC 5280 IETF'ın düzgün biçimlendirildiğini olasıdır.

### <a name="creating-a-self-signed-x509-ca-certificate"></a>İmzalanan X.509 CA sertifika oluşturma

İmzalanan X.509 CA sertifikası oluşturma işlemi, bir üçüncü taraf imzalayan kök sertifika yetkilisi gibi ilgili hariç olmak üzere satın alma için benzerdir. Bizim örneğimizde, şirket X bir kök sertifika yetkilisi yerine yetkilisi sertifikasını imzalar. Şirket X yetkilisi sertifikası satın almak hazır oluncaya kadar test etmek için bu seçeneği tercih edebilirsiniz. IOT hub'ı dışında herhangi bir üçüncü taraf hizmetine bağlanmak için akıllı X Pencere planlanmıyorsa şirket X otomatik olarak imzalanan X.509 CA sertifikası, üretimde de kullanabilirsiniz.

## <a name="register-the-x509-certificate-to-iot-hub"></a>IOT hub'ına bir X.509 sertifikası kaydetme

IOT hub'ın nerede bağlandıklarında akıllı X Pencere kimlik doğrulaması için kullanılacak X.509 CA kaydetmek şirket X gerekir. Bu kimlik doğrulaması ve herhangi bir sayıda akıllı X Pencere cihazları yönetme olanağı sağlayan bir tek seferlik bir işlemdir. Bu işlem, sertifika yetkilisi ve cihazlar arasında bir-çok ilişkisi nedeniyle tek seferlik ve X.509 CA kimlik doğrulama yöntemini kullanmanın başlıca avantajlarından biri de oluşturur. Böylece işletimsel maliyetleri ekleme her akıllı X Pencere cihazı için bireysel sertifika parmak izleri yüklenecek alternatiftir.

X.509 CA sertifikası kaydetme iki adımlı bir işlem, sertifika yükleme ve sertifika sağlama-ın-elinde içindir.

![X509CA sertifikayı kaydetme](./media/iot-hub-x509ca-concept/pop-flow.png)

### <a name="x509-ca-certificate-upload"></a>X.509 CA sertifika yükleme

X.509 CA sertifikası yalnızca bu işlemi karşıya yükleme, IOT Hub'ına CA sertifikasını karşıya yükleyin. IOT hub'ı, bir dosyadaki sertifika bekliyor. Şirket X yalnızca sertifika dosyasını karşıya yükler. Sertifika dosyası hiçbir koşulda gerekir, herhangi bir özel anahtarı içerir. Ortak anahtar altyapısı (PKI) yöneten standartları en iyi taahhütlerin şirketin bu Bilgi Bankası-X'lerin özel bu durumda, özel olarak şirket-X içinde bulunur.

### <a name="proof-of-possession-of-the-certificate"></a>Kavram, elinde sertifikanın

X.509 CA sertifikası gibi herhangi bir dijital sertifikayı gizlice için saldırılara açıktır genel bilgiler verilmiştir. Bu nedenle, bir gönderilebileceğinden sertifika kesebilen ve kendi olarak karşıya yüklemeyi yeniden deneyin. Bizim örneğimizde, IOT hub'ı şirket X karşıya CA sertifikası gerçekten şirket X ile ait olduğundan emin olun ister misiniz? Bunu yapar, bu nedenle, zorlu şirket-, sağlanmasında X bunlar aslında sertifikayı aracılığıyla sahip bir [elinde kavram (PoP) akış](https://tools.ietf.org/html/rfc5280#section-3.1). Şirket-sertifikanın özel anahtarını kullanarak X tarafından imzalanması için IOT Hub rastgele sayı oluşturma elinde kavram akışı kapsar. Şirket X PKI en iyi uygulamaları takip ve özel anahtarıyla korunan sonra yalnızca bunlar doğru elinde kavram sınaması için yanıt konumda olabilir. IOT hub'ı başarılı bir yanıt elinde kavram challenge öğesinin bağlı X.509 CA sertifika kaydetmek için devam eder.

IOT Hub'ından başarılı bir yanıt elinde kavram olarak X.509 CA kayıt işlemini tamamlar.

## <a name="sign-devices-into-a-certificate-chain-of-trust"></a>Bir sertifika güven zinciri içine oturum cihazlar

IOT, her cihazın benzersiz bir kimliğe sahip olmasını gerektirir. Sertifika tabanlı kimlik doğrulaması düzeni için form sertifikalar bu Kimlikleridir. Bizim örneğimizde, bu her akıllı-X-Pencere benzersiz cihaz sertifika sahip olduğu anlamına gelir. Şirket X bu tedarik zincirinde Kur nasıl?

Bu konuda gitmek için bir tedarik zinciri iş ortaklarıyla ilgili benzersiz cihaz özel anahtarları akıllı-X-pencere öğeleri ve entrusting bilgisine yönelik sertifikalarını önceden oluşturmak için yoludur. Şirket X için bu Fabrika Y ve Z teknisyen entrusting anlamına gelir. Bu geçerli bir yöntem olsa da, üstesinden gereken güven gibi emin olmak için sorunları ile birlikte gelir:

1. Tedarik zinciri ortakları PKI yoksayılıyor yanı sıra cihaz özel anahtarları paylaşmak zorunda kalmadan, hiçbir zaman özel anahtarlar, pahalı bir tedarik zinciri güven oluşturma yapar paylaşımı en iyi yöntemler. Ev cihazı özel anahtarları için güvenli odaları gibi büyük sistemler geldiğini ve düzenli güvenlik denetimleri gibi işlemleri yüklü olması gerekir. Her ikisi için tedarik zincirinin maliyeti ekleyin.

2. Tedarik zinciri cihazlar için güvenli bir şekilde muhasebe ve daha sonra bunları dağıtımda yönetme cihaz benzersiz sertifika (Bu nedenle özel anahtarı) oluşturma noktasından cihaz devre dışı bırakma her anahtar cihaz çifti için bire bir görev haline gelir. Bu cihaz Grup Yönetimi ışığının, sürece grupları kavramı açıkça işlemine şekilde oluşturulmuştur. Güvenli hesap ve cihaz yaşam döngüsü yönetimi, bu nedenle, yoğun işlem yükü olur. Bizim örneğimizde, şirket X bu yükü aittir.

X.509 CA sertifika kimlik doğrulaması kullanarak sertifika zincirleri zorlukları listelenen afore Zarif çözümleri sunar. Başka bir ara CA sırayla imzalar ve bu nedenle bir cihaz son ara CA imzalar kadar yanar ara CA imzalama bir CA'dan bir sertifika zinciri sonuçlanır. Bizim örneğimizde, sırayla son Akıllı X Pencere imzalayan teknisyen Z imzalar Fabrika-Y, şirket X imzalar.

![Sertifika zinciri hiyerarşisi](./media/iot-hub-x509ca-concept/cert-chain-hierarchy.png)

Sertifika zincirindeki cascade yukarıda mantıksal el alma yetki sunar. Birçok tedarik zinciri zincirindeki tüm yetkilisi sertifikaları ekleyebilir ve bu mantıksal el ile alınırken tüm Yukarı Akış CA sertifikaları zincirine her ara CA imzalı ve son oturum açtığı her bir cihaz son ara CA alma izleyin cihazda oturum. Bu, üretim yapmak için belirli bir Fabrika fabrikaları hiyerarşisini sözleşme üretim şirketine komisyon olduğunda yaygındır. Hiyerarşi düzeyleri birkaç olabilir ancak derin (örneğin, coğrafi konum/satır türü/Üretim ürüne göre), cihazla etkileşime geçmek yalnızca sonunda üreteci alır ancak zinciri hiyerarşinin üstünden korunur.

Alternatif zincirleri, CA cihazı ile etkileşim çalışması sertifika zinciri içeriğini bu noktada eklediği cihazla etkileşime geçmek için farklı ara CA olabilir. Bazı CA'ın yalnızca cihazın fiziksel etkileşim karma modeller de mümkün olduğu.

Bizim örneğimizde, Fabrika Y hem teknisyen Z akıllı-X-pencere öğesi ile etkileşim kurun. Şirket X akıllı X Pencere ait olsa da, aslında fiziksel olarak birlikte tüm tedarik zincirinde etkileşime girmez. Bu nedenle oluşturan şirket X akıllı X pencere için güven sertifikası zinciri hangi sırayla son imza akıllı-X-Pencere öğesine ardından sağlayan teknisyen Z imzalar Fabrika Y imzalama. Akıllı X Pencere öğesinin yükleme ve üretim fabrikası Y ve Z teknisyen her akıllı-X-pencere öğeleri oturum açmak için ilgili ara CA sertifikalarını kullanarak oluşturur. Tüm bu işlem son sonucunu akıllı-X-pencere öğeleri benzersiz cihaz sertifikaları ve daha şirket X CA sertifikasına güven sertifikası zinciri olduğu.

![Başka bir şirketin sertifikaları için bir şirket sertifikalardaki güven zinciri](./media/iot-hub-x509ca-concept/cert-mfr-chain.png)

Bu, X.509 CA yöntemi değerini incelemek için iyi bir noktadır. Önceden oluşturma ve devre dışı sertifikalar her akıllı-X-pencere öğesi için tedarik zincirine gönderdikten yerine, şirket X Fabrika Y bir kez oturum açmak yalnızca gerekiyordu. Cihaz yaşam döngüsü boyunca her bir cihaz izlemek zorunda kalmak yerine şirket X şimdi izlemek ve doğal olarak tedarik zinciri işleminden sonra bazı yılın Temmuz teknisyen Z tarafından yüklü cihazlar örneğin kitaplığınızı grupları aracılığıyla cihazları yönetme.

Son ancak, CA kimlik doğrulama yöntemi, tedarik zinciri üretim cihazında güvenli sorumluluk infuses. Sertifika zinciri işlemi nedeniyle, zincirdeki her üyesinin eylemleri şifreli olarak kaydedilmiş ve doğrulanabilir.

Bu işlem, bütünlük açısından ortaya çıkması bazı varsayımlarda bağımlıdır. Cihazın benzersiz ortak/özel anahtar çifti oluşturma bağımsız ve özel anahtarı cihazı korunması gerekir. Neyse ki, donanım güvenli modülleri (HSM) dahili olarak anahtarlar oluşturma ve özel anahtarları koruma özelliğine sahip biçiminde güvenli Silikon yongaları mevcut. Şirket X yalnızca bir tür yongaları akıllı-X-Pencere öğesinin bileşen reçetesi eklemeniz gerekir.

## <a name="device-connection"></a>Cihaz Bağlantısı

Önceki bölümlerde yukarıdaki cihaz bağlantı oluşturmayı. IOT hub'ına bir X.509 CA sertifikası kaydederek bir kez nasıl potansiyel olarak milyonlarca cihazı bağlayın ve ilk kez kimliği doğrulanmış?  Basit; aynı sertifikayı karşıya yükleme ve elinde kavram akış aracılığıyla X.509 CA sertifikası kaydetme ile daha önce karşılaştık.

Benzersiz cihaz sertifikaları ve sertifika zincirinden kendi ilgili X.509 CA kimlik doğrulama özelliği için üretilen cihazlar, tedarik zinciri üretim. İlk kez için bile, cihaz bağlantısı olur iki adımlı bir işlem: sertifika zinciri karşıya yükleme ve elinde kanıt.

Sertifika zinciri karşıya yükleme sırasında cihaz, cihaz benzersiz sertifika içinde IOT Hub'ına yüklü sertifika zinciri ile birlikte yükler. IOT hub'ı önceden kayıtlı X.509 CA sertifikası kullanarak, birkaç şey, karşıya yüklenen sertifika zinciri dahili olarak tutarlıdır ve X.509 CA sertifikası geçerli sahibi tarafından zinciri oturumun açıldığı şifreli olarak doğrulayabilirsiniz. X.509 CA kayıt işlemine yalnızca oluştu, IOT Hub, belirlemek için bir elinde kavram sınama-yanıt işlemini zinciri başlatmak ve bu nedenle cihaz sertifikası karşıya cihaza gerçekten ait. Bunu IOT Hub tarafından doğrulama için özel anahtarını kullanarak cihaz tarafından imzalanması için rastgele bir sınama oluşturarak yapar. IOT Hub'ı cihaz özgün olarak kabul et ve bağlantı vermek için başarılı bir yanıt tetikler.

Bizim örneğimizde her akıllı-X-Pencere Fabrika Y ve Z teknisyen X.509 CA sertifikalarını birlikte benzersiz cihaz sertifikasını karşıya yükleyin ve ardından IOT Hub'ından elinde kavram sınaması için yanıt.

![Hub'ından başka pop sınaması bir sertifikasından akışı](./media/iot-hub-x509ca-concept/device-pop-flow.png)

Cihaz özel anahtarları dahil olmak üzere özel anahtarları koruma konusunda güven foundation ye dayanıyorsa dikkat edin. Biz bu nedenle stres olamaz yeterli güvenli Silikon önemini yongalarından birini donanım güvenli modülleri (HSM) biçiminde cihaz özel anahtarları ve hiçbir zaman herhangi bir özel anahtara paylaşımı genel en iyi uygulama olarak korumak için bir Fabrika başka entrusting ister kendi özel anahtar.