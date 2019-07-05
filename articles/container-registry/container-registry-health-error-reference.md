---
title: Sistem durumu denetimi - Azure Container Registry için hata başvurusu
description: Hata kodları ve Azure Container Registry'de az acr denetimi durumu tanılama komutu çalıştırarak bulunan sorunların olası çözümleri
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: fc29b27cbb7eea983140c59529d981ad95c27ae8
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555117"
---
# <a name="health-check-error-reference"></a>Sistem durumu denetimi hata başvurusu

Tarafından döndürülen hata kodları hakkında ayrıntılar verilmiştir [az acr onay durumunu][az-acr-check-health] komutu. Her bir hata için olası çözümleri listelenmiştir.

## <a name="dockercommanderror"></a>DOCKER_COMMAND_ERROR

Bu hata, CLI için Docker istemcisinin bulunamadı anlamına gelir. Sonuç olarak, aşağıdaki ek denetimleri çalıştırılamaz: Docker sürüm Docker'ı değerlendirirken, arka plan programı durumu bulma ve çalıştıran bir Docker pull komutu.

*Olası çözümleri*: Docker İstemcisi'ni yükleme; Docker yolu için sistem değişkenlerini ekleyin.

## <a name="dockerdaemonerror"></a>DOCKER_DAEMON_ERROR

Bu hata, Docker cinini durumu kullanılamıyor veya bunu CLI kullanarak erişilemedi, anlamına gelir. Sonuç olarak, Docker işlemleri (gibi `docker login` ve `docker pull`) CLI aracılığıyla kullanılamaz.

*Olası çözümleri*: Docker Daemon programını yeniden başlatın veya düzgün şekilde yüklendiğini doğrulayın.

## <a name="dockerversionerror"></a>DOCKER_VERSION_ERROR

Bu hata, CLI komutunu çalıştırmak mümkün olmadığını anlamına gelir `docker --version`.

*Olası çözümleri*: Komut el ile çalıştırın, en yeni CLI sürümüne sahip ve hata iletisini inceleyin emin olun.

## <a name="dockerpullerror"></a>DOCKER_PULL_ERROR

Bu hata, CLI ortamınıza bir örnek görüntü çekme okuyamamış anlamına gelir.

*Olası çözümleri*: Görüntü çekmek gerekli tüm bileşenleri düzgün şekilde çalıştığını doğrulayın.

## <a name="helmcommanderror"></a>HELM_COMMAND_ERROR

Bu hata, Helm istemci diğer Helm işlemler ışığının CLI tarafından bulunamadı anlamına gelir.

*Olası çözümleri*: Helm istemci yüklendikten ve yolu için sistem ortam değişkenlerini eklendiğini doğrulayın.

## <a name="helmversionerror"></a>HELM_VERSION_ERROR

Bu hata, CLI'yı yüklü Helm sürümü belirlenemiyor anlamına gelir. Bu durum oluşabilir Azure CLI sürümünü (veya Helm sürümü) kullanılan kullanımdan kalkmıştır.

*Olası çözümleri*: En son Azure CLI sürümünü veya önerilen Helm sürüm için güncelleştirme; komutu el ile çalıştırın ve hata iletisini inceleyin.

## <a name="connectivitydnserror"></a>CONNECTIVITY_DNS_ERROR

Bu hata, belirli kayıt defteri oturum açma sunucusu için DNS işten ancak, kullanılamadığı anlamına gelir yanıt vermedi anlamına gelir. Bu, bazı bağlantı sorunlarını gösterebilir. Alternatif olarak, kayıt mevcut, kullanıcı (kendi oturum açma sunucusu düzgün bir şekilde almak için) kayıt defteri izinlere sahip olmayabilir veya Azure CLI içinde kullanılan olandan farklı bir bulutta hedef kayıt defteridir.

*Olası çözümleri*: Bağlantıyı doğrulamak; kayıt defteri yazımını doğrulayın ve bu kayıt defteri var; Kullanıcı doğru izinleri olduğunu ve kayıt defterinin bulut Azure CLI içinde kullandığınız aynı olduğunu doğrulayın.

## <a name="connectivityforbiddenerror"></a>CONNECTIVITY_FORBIDDEN_ERROR

Bu hata belirli bir kayıt sınaması uç nokta 403 Yasak HTTP durum koduyla yanıt anlamına gelir. Bu hata, bir sanal ağ yapılandırması nedeniyle büyük olasılıkla kayıt defterine kullanıcılar erişime sahip olmadığından anlamına gelir.

*Olası çözümleri*: Sanal ağ kuralları kaldırın veya geçerli istemci IP adresi izin verilenler listesine ekleyin.

## <a name="connectivitychallengeerror"></a>CONNECTIVITY_CHALLENGE_ERROR

Bu hata, hedef kayıt sınaması uç noktası bir sınama vermedi anlamına gelir.

*Olası çözümleri*: Süre sonra yeniden deneyin. Hata devam ederse, bir sorun açın https://aka.ms/acr/issues.

## <a name="connectivityaadloginerror"></a>CONNECTIVITY_AAD_LOGIN_ERROR

Bu hata, hedef kayıt sınaması uç noktası bir sınama verildi, ancak kayıt defterine Azure Active Directory kimlik doğrulamasını desteklemez anlamına gelir.

*Olası çözümleri*: Örneğin, yönetici kimlik bilgilerinizle kimliğinizi doğrulamanız farklı bir şekilde deneyin. Kullanıcılar Azure Active Directory'yi kullanarak kimlik doğrulaması gerekiyorsa, bir sorun açın https://aka.ms/acr/issues.

## <a name="connectivityrefreshtokenerror"></a>CONNECTIVITY_REFRESH_TOKEN_ERROR

Bu hata, hedef kayıt defterine erişim reddedildi için kayıt defteri oturum açma sunucusu bir yenileme belirteci ile yanıtlamadı anlamına gelir. Bu hata, kullanıcı kayıt defterini doğru izinlere sahip değilse veya kullanıcı kimlik bilgileri Azure CLI için eski olduğunda ortaya çıkabilir.

*Olası çözümleri*: Kullanıcının kayıt defterine doğru izinlere sahip olup olmadığını doğrulayın; çalıştırma `az login` izinleri, belirteçleri ve kimlik bilgilerini yenilemek için.

## <a name="connectivityaccesstokenerror"></a>CONNECTIVITY_ACCESS_TOKEN_ERROR

Hedef kayıt defterine erişim engellendi, böylece bu hata, kayıt defteri oturum açma sunucusu bir erişim belirteci ile yanıt vermedi anlamına gelir. Bu hata, kullanıcı kayıt defterini doğru izinlere sahip değilse veya kullanıcı kimlik bilgileri Azure CLI için eski olduğunda ortaya çıkabilir.

*Olası çözümleri*: Kullanıcının kayıt defterine doğru izinlere sahip olup olmadığını doğrulayın; çalıştırma `az login` izinleri, belirteçleri ve kimlik bilgilerini yenilemek için.

## <a name="loginservererror"></a>LOGIN_SERVER_ERROR

Bu hata CLI verilen kayıt defterinin oturum açma sunucusu bulamadı geçerli bulut için varsayılan sonek bulundu anlamına gelir. Kayıt defterindeki Bulut ve geçerli Azure CLI bulut eşleşmiyorsa, kullanıcı doğru izinlere kayıt yoksa, kayıt defteri, mevcut değilse veya Azure CLI sürümü eski ise bu hata oluşabilir.

*Olası çözümleri*: Yazım denetimi doğru olduğunu ve kayıt defteri var olduğunu doğrulayın; doğrulayın, kullanıcı kayıt defterini doğru izinlere sahip ve kayıt defteri CLI ortam ve bulut eşleşen; Azure CLI, en son sürüme güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Bir kayıt defteri durumunu denetlemek seçenekleri için bkz [Azure container registry durumunu denetleme](container-registry-check-health.md).

Bkz: [SSS](container-registry-faq.md) sık sorulan sorular ve Azure Container Registry hakkında bilinen diğer sorunlar için.





<!-- LINKS - internal -->
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
