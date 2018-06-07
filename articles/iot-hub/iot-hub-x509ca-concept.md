---
title: Azure IOT Hub X.509 güvenlik kavramları | Microsoft Docs
description: X.509 değeri anlama kavram - yetkilisi sertifikaları IOT cihaz üretim ve kimlik doğrulama sertifikası.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 1f7a02f66a8d87f33d7bac9068628dbd29e5bd7c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635704"
---
# <a name="conceptual-understanding-of-x509-ca-certificates-in-the-iot-industry"></a>Kavramsal anlayış IOT endüstri X.509 CA sertifikaları

Bu makalede X.509 sertifika yetkilisi (CA) sertifikaları IOT cihaz üretim ve IOT Hub'ına kimlik doğrulama kullanarak değerini açıklanmaktadır.  Kaynağı ile ilgili bilgi içeren zincir Kurulum ve avantajları vurgulayın.

Bu makalede açıklanır:

* X.509 CA sertifikaları nedir ve bunları alma
* IOT Hub'ına, X.509 CA sertifikanız kaydetme
* Bir üretimi ayarlamak nasıl tedarik zinciri X.509 CA tabanlı kimlik doğrulaması
* X.509 CA ile imzalanmış cihazlar IOT Hub'ına nasıl bağlanacağını

## <a name="overview"></a>Genel Bakış

X.509 sertifika yetkilisi (CA) kimlik doğrulama cihazlar IOT hub'ına cihaz kimliği oluşturma ve yaşam döngüsü yönetimi tedarik zinciri önemli ölçüde basitleştirir bir yöntem kullanarak kimlik doğrulaması için bir yaklaşımdır.

Bir ayırt X.509 CA kimlik doğrulamasının aşağı akış aygıtlarla bir CA sertifikasına sahip bir-çok ilişkisi özniteliğidir.  Bu ilişki, bir X.509 CA sertifikasını bir kez kaydederek IOT Hub'ına cihazlar herhangi bir sayıda kaydını sağlar, bir cihazın bağlanabilmesi için önce Aksi takdirde cihaz benzersiz sertifikaları her aygıt için önceden kaydedilmiş olmalıdır. Bu bir-çok ilişkisi ayrıca aygıt sertifika yaşam döngüsü yönetimi işlemlerini basitleştirir.

Başka bir önemli X.509 CA kimlik doğrulamasının tedarik zinciri Lojistik basitleştirme özniteliğidir.  Her cihaz güven için temel olarak benzersiz bir gizli bir anahtarı gibi tutan aygıtların güvenli kimlik doğrulaması gerektirir. Sertifika tabanlı kimlik doğrulaması, bu gizli özel bir anahtardır. Akış üretim tipik bir aygıt, birden çok adımları ve koruyucuları içerir.  Güvenli bir şekilde aygıt özel anahtarları arasında birden çok koruyucuları yönetme ve güvenini korumak zor ve pahalı bir işlemdir.  Sertifika yetkililerini kullanarak aygıt özel anahtarla entrusting yerine her koruyucu şifreleme bir güven zinciri içine imzalama tarafından bu sorunu çözer.  Her koruyucu sırayla kendi ilgili işlem adımının üretim akışının cihazları imzalar.  Genel en iyi tedarik zinciri güven şifreleme zinciri aracılığıyla yerleşik sorumluluk ile sonucudur.  Bu aygıtlar benzersiz özel anahtarlarını koruduğunuzda bu işlem çoğu güvenlik verir önemlidir.  Bu amaçla kullanma, donanım güvenli modülleri (HSM) dahili olarak hiçbir zaman günün ışık görürsünüz özel anahtarları oluşturma yeteneği yönlendirmeye.

Bu makalede cihaz bağlantısı için tedarik zinciri kurulumundan X.509 CA'ın kimlik doğrulaması kullanarak bir uçtan uca görünümünü sunar yapma çalışırken anlama pekiştirilmesine gerçek dünya örneği kullanın.

## <a name="introduction"></a>Giriş

X.509 CA sertifikasını, sahibi diğer sertifikaları imzalamak dijital bir sertifikadır.  Bu dijital sertifika X.509 IETF'ın RFC 5280 standardına göre belirlenen standart biçimlendirme bir sertifikaya uyumlu olduğundan ve onun sahibi diğer sertifikaları imzalamak bir sertifika yetkilisi (CA) olmasıdır.

X.509 CA'ın kullanımını somut bir örnek ile ilgili en iyi anlaşılmalıdır.  Şirket X, akıllı-X-profesyonel yükleme için tasarlanan pencere öğeleri oluşturucusunun göz önünde bulundurun. Şirket X hem üretim hem de yükleme outsources.  Üretici akıllı X Widgets ve hizmet sağlayıcısı teknisyen Z yüklemek için üretmek için Fabrika-Y sözleşme. Şirket X istediği akıllı X Pencere doğrudan Fabrika-Y teknisyen-Z yüklemesi için gönderilen olduğunu ve doğrudan şirket bağlanır-şirket X başka müdahalesi olmadan yükleme sonrasında X'lerin IOT Hub örneği. Şirket X Bunun gerçekleşmesi için akıllı X pencere otomatik bağlantı için hazırlamak için birkaç kerelik kurulum işlemlerini tamamlamak gerekir.  İle uçtan uca senaryoyu unutmayın, bu makalenin kalanında aşağıdaki gibi yapılandırılmış:

* X.509 CA sertifikası alın

* IOT Hub'ına X.509 CA sertifikasını Kaydet

* Bir sertifika güven zinciri oturum cihazlarının

* Cihaz bağlantısı

## <a name="acquire-the-x509-ca-certificate"></a>X.509 CA sertifikası alın

Şirket X bir X.509 CA sertifikasını bir ortak kök sertifika yetkilisinden satın alma ya da kendinden imzalı bir işlem aracılığıyla oluşturma seçeneğiniz vardır.  Üzerinden uygulama senaryosuna bağlı olarak diğer bir seçenek uygun olabilir.  Seçeneği seçerseniz, işlem bir genel/özel anahtar çifti oluşturma ve ortak anahtar imzalama sertifikası iki temel adımları kapsar.

![img csr akışı](./media/csr-flow.png)

Bu adımları gerçekleştirmek hakkında ayrıntılar çeşitli hizmet sağlayıcıları ile farklı.

### <a name="purchasing-an-x509-ca-certificate"></a>Bir X.509 CA sertifikası satın alma

Bir CA sertifikası satın alma, iyi bilinen bir kök CA act bir güvenilen cihazları bağlandığında, IOT cihazları meşru olup olmadığı için vouch için şirketlerin sahip avantajına sahiptir. Şirket X, IOT Hub'ına ilk bağlantı sonra üçüncü taraf ürünleri veya Hizmetleri ile etkileşim kurmak için akıllı-X-pencere öğesi istiyorsanız bu seçeneği seçmeniz gerekir.

Bir X.509 CA sertifikası satın almak için şirket X kök sertifikalar Hizmetler Sağlayıcısı seçmelisiniz. 'Kök CA' deyimi bir internet arama iyi müşteri adayları sunacak.  Kök CA ortak/özel anahtar çifti oluşturma ve bir sertifika imzalama isteği (CSR), hizmetlerini oluşturmak nasıl şirket X yol gösterecektir.  Bir CSR bir sertifika yetkilisinden bir sertifika için uygulama resmi işlemidir.  Bu satın alma sonucunu bir sertifika yetkilisi sertifikası olarak kullanmak için ' dir.  X.509 sertifikaları yaygınlığıyla verildiğinde, sertifikayı standart olan RFC 5280 IETF'ın doğru şekilde biçimlendirildiğini olasıdır.

### <a name="creating-a-self-signed-x509-ca-certificate"></a>X.509 CA otomatik olarak imzalanan sertifika oluşturma

Otomatik olarak imzalanan X.509 CA bir sertifika oluşturma işlemi, bir üçüncü taraf imzalayan kök sertifika yetkilisi gibi içeren dışında satın alma işleminde benzer. Bizim örneğimizde, şirket X bir kök sertifika yetkilisi yerine yetkilisi sertifikasını imzalar.  Şirket X yetkilisi sertifikası satın almaya hazır kadar test etmek için bu seçeneği tercih edebilirsiniz. Şirket X üretim, otomatik olarak imzalanan bir X.509 CA sertifikası akıllı X Pencere IOT hub'ı dışında herhangi bir üçüncü taraf hizmetlerine bağlanmak için tasarlanmamıştır de kullanabilir.

## <a name="register-the-x509-certificate-to-iot-hub"></a>IOT Hub'ına X.509 sertifikasını Kaydet

Şirket X IOT Hub'ın nerede bağlandıklarında akıllı X Widgets kimlik doğrulaması için kullanılacak X.509 CA'ya kaydetmeniz gerekir.  Bu kimlik doğrulaması ve herhangi bir sayıda akıllı X Pencere aygıtları yönetme olanağı sağlayan tek seferlik bir işlemdir.  Bu işlem yetkilisi sertifikasını ve cihazlar arasında bir-çok ilişkisi nedeniyle tek seferlik ve X.509 CA kimlik doğrulama yöntemini kullanmanın başlıca avantajlarından biri de bulunabilir.  Böylece işletim maliyetlerini ekleme her akıllı X Pencere cihaz için bağımsız sertifika parmak izleri karşıya yüklemek için kullanılan alternatiftir.

X.509 CA sertifikasını kaydetme iki adımlı bir işlem, sertifika yükleme ve sertifika kanıt-in-sahip olur.

![img pop akışı](./media/pop-flow.png)

### <a name="x509-ca-certificate-upload"></a>X.509 CA sertifika yükleme

X.509 CA sertifikasını yalnızca bu işlem karşıya yükleme, IOT Hub'ına CA sertifikasını yükleyin.  IOT hub'ı bir dosyada sertifika bekliyor. Şirket X yalnızca sertifika dosyası karşıya yükleme. Sertifika dosyası hiçbir koşulda gerekir herhangi bir özel anahtarı içerir.  Ortak anahtar altyapısı (PKI) yöneten standartları en iyi uygulamaları, şirket, bu bilgi olması zorunlu tutulmuştur-X'lerin özel bu durumda yalnızca şirket-X içinde yer alıyor.

### <a name="proof-of-possession-of-the-certificate"></a>Kanıt in elinde sertifikanın

Herhangi bir dijital sertifikayı gibi X.509 CA sertifikasını, gizlice dinlemeyi için açıktır ortak bilgilerdir.  Bu nedenle, dinleyenin bir sertifika müdahale ve kendi olarak karşıya yüklemeyi yeniden deneyin.  Bizim örneğimizde, IOT hub'ı şirket X karşıya CA sertifikasını gerçekten şirket-X-ait olduğundan emin olun ister misiniz? Bunu yapar, bu nedenle zor şirket-, Dene X tarafından bunlar aslında yoluyla sertifika sahip bir [elinde kanıtı (PoP) akış](https://tools.ietf.org/html/rfc5280#section-3.1). Elinde kanıt akış şirket-özel anahtarını kullanarak X tarafından imzalanması için IOT Hub bir rastgele sayı oluşturma kapsar.  Şirket X PKI en iyi yöntemler ve ardından ve kendi özel anahtar korumalı ise sonra yalnızca bunlar doğru elinde kanıt yanıtlaması konumda olabilir.  IOT hub'ı başarılı bir yanıt elinde kanıt Challenge bağlı X.509 CA sertifikasını kaydetmek için devam eder.

Başarılı yanıt elinde kanıtı olarak IOT hub'dan X.509 CA kayıt işlemini tamamlar.

## <a name="sign-devices-into-a-certificate-chain-of-trust"></a>Bir sertifika güven zinciri oturum cihazlarının

IOT her cihazın benzersiz bir kimliğe sahip olmasını gerektirir.  Bu kimlikleri sertifika tabanlı kimlik doğrulama şemasını form sertifikalarını alır.  Bizim örneğimizde, bu her akıllı-X-Pencere benzersiz cihaz bir sertifikaya sahip olmalıdır anlamına gelir.  Şirket X bu tedarik zincirinde Kur nasıl?

Bu konuda gitmek için bir akıllı X Widgets ve entrusting bilgi tedarik zinciri ortakları ile ilgili benzersiz cihaz özel anahtarların sertifikalarını önceden oluşturmak için yoludur.  Şirket X için bu Fabrika Y ve teknisyen Z entrusting anlamına gelir.  Bu geçerli bir yöntem olsa da, güven gibi emin olmak için aşmanız gerekir zorluklar gelir:

1. PKI yoksayılıyor yanı sıra tedarik zinciri ortakları aygıt özel anahtarları paylaşmasına gerek kalmadan, hiçbir zaman özel anahtarlar, pahalı tedarik zinciri güvende derleme yapar paylaşımı en iyi yöntemler.  Ev aygıt özel anahtarları için güvenli odaları gibi büyük sistemleri anlamına gelir ve düzenli güvenlik denetimleri gibi işlemleri yüklenmesi gerekmez.  Her ikisi de maliyet tedarik zinciri ekleyin.

2. Tedarik zinciri aygıtlar için güvenli bir şekilde hesap ve daha sonra bunları dağıtımda yönetme aygıt benzersiz sertifikası (Bu nedenle özel anahtarı) nesil noktasından cihaz devre dışı bırakma her anahtar cihaz çifti için bire bir görev haline gelir. Grupları kavramı açıkça işlemine şekilde oluşturulmuştur sürece bu aygıtların Grup Yönetimi olanağını engeller. Güvenli hesap ve cihaz yaşam döngüsü yönetimi, bu nedenle, yoğun işlemleri yük olur.  Bizim örneğimizde, şirket X bu yükünü hesaba katılmalıdır.

X.509 CA sertifikası kimlik doğrulaması, sertifika zincirleri kullanımıyla zorluklar listelenen afore Zarif çözümleri sunar.  Bir sertifika zinciri, başka bir ara CA sırayla imzalar ve dolayısıyla aygıt son ara CA imzalar kadar yanar ara CA imzalama bir CA'dan oluşur.  Bizim örneğimizde, sırayla son Akıllı X Pencere imzalar teknisyen-Z imzalar Fabrika-Y, şirket X imzalar.

![Sertifika zinciri hiyerarşi img](./media/cert-chain-hierarchy.png)

Sertifika zincirindeki cascade yukarıda mantıksal el dışı yetkilisinin gösterir.  Birçok tedarik zinciri bu mantıksal el yapabildiği her ara CA zincirine tüm Yukarı Akış CA sertifikaları alınırken imzalı ve son ara CA'ın son her aygıt imzalar dışı izleyin ve tüm yetkilisi sertifikalar zincirinden Ekle cihazda. Bu, üretim yapmak için belirli bir Fabrika oluşturucuları hiyerarşisini sözleşme üretim şirketle komisyon zaman yaygındır.  Hiyerarşi birkaç düzeyleri olsa da cihazla etkileşime geçmek derin (örneğin, coğrafi konum/satır türü/Üretim ürün tarafından), yalnızca son fabrikada alır ancak zinciri hiyerarşinin üstünden korunur.

Alternatif zincirleri, CA aygıt ile etkileşim durumda sertifika zinciri içeriği o noktada yerleştirir aygıt ile etkileşim farklı ara CA olabilir.  Bazı CA'ın sahip yalnızca cihazın fiziksel etkileşim karma modeller da burada mümkündür.

Bizim örneğimizde, Fabrika-Y ve teknisyen Z akıllı-X-Pencere öğesiyle etkileşim.  Şirket X akıllı X Pencere sahip olsa da, bunu gerçekten fiziksel olarak ile tüm tedarik zinciri etkileşime girmez.  Bu nedenle oluşturan şirket X akıllı X pencere için güven sertifikası zinciri hangi sırayla son imza için akıllı-X-Pencere sonra sağlayacak teknisyen Z imzalar Fabrika Y imzalama. Üretim ve akıllı X Pencere öğesinin yükleme Fabrika-Y ve teknisyen Z her akıllı-X-pencere öğeleri imzalamak için ilgili ara CA sertifikalarını kullanarak oluşturan. Son tüm işlem akıllı X Widgets benzersiz cihaz sertifikaları ve şirket X CA sertifikasını giderek güven sertifikası zinciri sonucudur.

![img cert üretici zinciri](./media/cert-mfr-chain.png)

X.509 CA yöntemi değerini gözden geçirmek iyi bir noktadır.  Önceden oluşturma ve sertifikaları tedarik zinciri her akıllı-X-pencere öğesi için teslim etmeden yerine, şirket X yalnızca el Fabrika-Y kez imzalamak için.  Aygıtları yaşam döngüsü boyunca her bir aygıtı izlemek gerek kalmadan yerine, şirket X değil izleyebilir ve doğal olarak tedarik zinciri işleminden sonra bazı yılın Temmuz teknisyen-Z tarafından yüklü aygıtlar Örneğin, ortaya grupları aracılığıyla cihazları yönetebilirsiniz.

Son ancak, kimlik doğrulama CA yöntemi tedarik zinciri üretim cihazda güvenli sorumluluk infuses. Sertifika zinciri işlemi nedeniyle eylemleri zincirindeki her bir üyenin, şifreli olarak kaydedilen ve doğrulanabilir.

Bu işlem, bütünlük açısından ortaya çıkması bazı varsayımlarda güvenir.  Cihaz benzersiz ortak/özel anahtar çifti bağımsız oluşturulmasını ve özel anahtarı içindeki aygıt korunması gerekir.  Neyse ki, güvenli Silikon yongaları formunda, donanım güvenli modülleri (HSM) dahili olarak anahtarlar oluşturma ve özel anahtarlarını koruma özelliği mevcut.  Şirket X yalnızca gereken böyle yongaları birini Akıllı-X-pencere öğesi'nin bileşeni ürün reçetesi ekleyin.

## <a name="device-connection"></a>Cihaz bağlantısı

Önceki bölümlerde yukarıdaki cihaz bağlantı oluşturmayı.  IOT Hub'ına bir X.509 CA sertifikasını kaydederek bir saat, nasıl potansiyel olarak milyonlarca cihaza bağlanma almak ve ilk kez kimliği doğrulanmış?  Basit; aynı sertifikayı karşıya yükleme ve elinde kanıt akış aracılığıyla X.509 CA sertifikasını kaydetme ile daha önce karşılaştı.

Cihaz benzersiz sertifikaları ve sertifika zincirinden kendi ilgili X.509 CA'ın kimlik doğrulaması donatılmış için üretilen aygıtların tedarik zinciri üretim.  İlk kez için bile aygıt bağlantısı gerçekleşir iki adımlı bir işlem: sertifika zinciri karşıya yükleme ve elinde sağlama.

Sertifika zinciri karşıya yükleme sırasında aygıt aygıt benzersiz sertifikasını IOT Hub'ına içinde yüklü sertifika zinciri ile birlikte yükler.  IOT Hub, önceden kaydedilmiş X.509 CA sertifikasını kullanarak, şifreli olarak karşıya yüklenen sertifika zinciri dahili olarak tutarlı olduğunu ve zinciri X.509 CA sertifikasının geçerli sahibi tarafından kaynaklanan şunları doğrulayabilirsiniz.  X.509 CA kayıt işlemine yalnızca oluştu, IOT Hub, onaylaması için bir elinde kanıt sınama yanıtı işlemini zinciri başlatmak ve dolayısıyla aygıt sertifika bunu karşıya cihaz gerçekte ait.  Bunu IOT Hub tarafından doğrulama için özel anahtarını kullanarak aygıt tarafından imzalanması için rastgele bir sınama oluşturarak yapar.   Başarılı yanıt IOT Hub'ı cihaz özgün olarak kabul etmek ve bağlantı vermek için tetikler.

Bizim örneğimizde, her akıllı-X-Pencere aygıt benzersiz sertifikasını Fabrika-Y ve teknisyen Z X.509 CA sertifikaları ile birlikte karşıya yükleme ve IOT Hub'ından elinde kanıt sınaması için yanıt vermek.

![img aygıt pop akış](./media/device-pop-flow.png)

Güven foundation aygıt özel anahtarları dahil olmak üzere özel anahtarları koruma ye dayanıyorsa dikkat edin.  Biz bu nedenle stres olamaz yeterli güvenli Silikon önemini aygıt özel anahtarları ve hiçbir zaman herhangi bir özel anahtara paylaşımı genel en iyi uygulama olarak korumak için donanım güvenli modülleri (HSM) biçiminde CHIPS ister başka bir işlemle entrusting bir Fabrika kendi özel anahtarı.

