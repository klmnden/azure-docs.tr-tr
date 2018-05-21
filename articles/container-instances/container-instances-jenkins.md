---
title: Aracı kullanmak Azure kapsayıcı örnekleri bir Jenkins olarak derleme
description: Azure kapsayıcı örnekleri bir Jenkins yapı gibi aracı kullanmayı öğrenin.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/20/2018
ms.author: nepeters
ms.openlocfilehash: 4df230c8306a3876e94a5e9ada5e7408f134ba26
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Aracı kullanmak Azure kapsayıcı örnekleri bir Jenkins olarak derleme

Azure kapsayıcı örnekleri (ACI) kapsayıcılı iş yüklerini çalıştırmak için isteğe bağlı, burstable ve yalıtılmış bir ortam sağlar. Bu öznitelikler nedeniyle ACI büyük ölçekli olarak çalışan Jenkins derleme işi için harika bir platform sağlar. Bu makalede, dağıtma ve önceden yapılandırılmış bir Jenkins sunucusu ile ACI derleme hedefi olarak kullanarak size yol göstermektedir.

Azure kapsayıcı örnekleri hakkında daha fazla bilgi için bkz: [Azure kapsayıcı örnekleri hakkında][about-aci].

## <a name="deploy-a-jenkins-server"></a>Jenkins sunucusu dağıtma

1. Azure portalında seçin **kaynak oluşturma** arayın ve **Jenkins**. Jenkins teklifi bir yayımcısı ile seçin **Microsoft**ve ardından **oluşturma**.

2. Aşağıdaki bilgileri girin **Temelleri** oluşturmak ve ardından **Tamam**.

   - **Ad**: Jenkins dağıtımı için bir ad girin.
   - **Kullanıcı adı**: Jenkins sanal makinenin yönetici kullanıcı için bir ad girin.
   - **Kimlik doğrulama türü**: kimlik doğrulaması için bir SSH ortak anahtarı öneririz. Bu seçeneği belirlerseniz, Jenkins sanal makineye oturum açma için kullanılacak bir SSH ortak anahtarını yapıştırın.
   - **Abonelik**: Bir Azure aboneliği seçin.
   - **Kaynak grubu**: Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.
   - **Konum**: Jenkins sunucu için bir konum seçin.

   ![Jenkins portal dağıtımı için temel ayarları](./media/container-instances-jenkins/jenkins-portal-01.png)

3. Üzerinde **ek ayarlar** formunda, aşağıdaki öğeleri tamamlayın:

   - **Boyutu**: Jenkins sanal makineniz için uygun boyutlandırma seçeneği belirleyin.
   - **VM disk türü**: belirtin **HDD** (sabit disk sürücüsü) veya **SSD** (katı hal sürücüsü) Jenkins sunucusu.
   - **Sanal ağ**: varsayılan ayarlarını değiştirmek istiyorsanız oku seçin.
   - **Alt ağlar**: oku tıklatın, bilgileri doğrulayın ve seçin **Tamam**.
   - **Genel IP adresi**: genel IP adresini özel bir ad verin, SKU yapılandırmak ve atama yöntemi ayarlamak için ok seçin.
   - **Etki alanı adı etiketi**: Jenkins sanal makine için tam bir URL oluşturmak için bir değer belirtin.
   - **Jenkins yayın türü**: İstenen sürüm seçeneklerden birini seçin: **LTS**, **haftalık yapı**, veya **Azure doğrulandı**.

   ![Jenkins portal dağıtımı için ek ayarlar](./media/container-instances-jenkins/jenkins-portal-02.png)

4. Hizmet asıl tümleştirmesi seçin **Auto(MSI)** olmasını [Azure yönetilen hizmet kimliği] [ managed-service-identity] bir kimlik doğrulama kimliği Jenkins için otomatik olarak oluştur örneği. Seçin **el ile** kendi hizmet asıl kimlik bilgilerini sağlamak için.

5. Jenkins işleri oluşturmak için bulut aracıları bulut tabanlı bir platform yapılandırın. Bu makalede amacıyla seçin **ACI**. ACI bulut Aracısı ile bir kapsayıcı örneğinde her Jenkins derleme işi çalıştırılır.

   ![Bulut Tümleştirme ayarlarını Jenkins portal dağıtımı](./media/container-instances-jenkins/jenkins-portal-03.png)

6. İle tümleştirme ayarlarını tamamladığınızda seçin **Tamam**ve ardından **Tamam** yeniden üzerinde doğrulama özeti. Seçin **oluşturma** üzerinde **kullanım koşulları** Özet. Jenkins sunucusu dağıtmak için birkaç dakika sürer.

## <a name="configure-jenkins"></a>Jenkins’i yapılandırma

1. Azure portalında Jenkins kaynak grubuna gidin, Jenkins sanal makineyi seçin ve DNS adını not edin.

   ![Jenkins sanal makine ayrıntılarını DNS adı](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

2. Jenkins VM DNS adına göz atın ve döndürülen SSH dizesini kopyalayın.

   ![SSH dizesiyle Jenkins oturum açma yönergeleri](./media/container-instances-jenkins/jenkins-portal-04.png)

3. Geliştirme sisteminizde bir terminal oturumu açın ve son adımı SSH dizeden yapıştırın. Güncelleştirme `username` Jenkins sunucunun dağıttığınızda belirtilen kullanıcı adı için.

4. Oturumun ardından bağlı, ilk yönetici parolası almak için aşağıdaki komutu çalıştırın:

   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

5. SSH oturumu ve çalışan tünel bırakın ve Git http://localhost:8080 bir tarayıcıda. İlk yönetici parolası kutusuna yapıştırın ve ardından **devam**.

   ![Yönetici parolası kutusuyla "Jenkins kilidini" ekranı](./media/container-instances-jenkins/jenkins-portal-05.png)

6. Seçin **önerilen Eklentileri yüklemek** tüm yüklemek için Jenkins eklentileri önerilir.

   !["Jenkins özelleştirme" ekranla "önerilen eklentileri Yükle" seçeneğini](./media/container-instances-jenkins/jenkins-portal-06.png)

7. Bir yönetici kullanıcı hesabı oluşturun. Bu hesap, oturum açmayı ve Jenkins örneği ile çalışmak için kullanılır.

   ![Doldurulmuş kimlik bilgileriyle "İlk yönetici kullanıcı oluşturma" ekranı](./media/container-instances-jenkins/jenkins-portal-07.png)

8. Seçin **kaydedin ve son**ve ardından **Jenkins kullanmaya başlamak** yapılandırmayı tamamlamak için.

Jenkins yapılandırılmıştır ve yapı kod ve dağıtmak için hazır. Bu örnekte, basit bir Java uygulaması, Azure kapsayıcı örnekleri Jenkins yapıyı göstermek için kullanılır.

## <a name="create-a-build-job"></a>Derleme işi oluşturma

Kullanırken bir kapsayıcı görüntüsü bir Jenkins olarak hedef yapı, tüm başarılı bir derleme için gerekli araçları içeren bir görüntü belirtmeniz gerekir. Görüntüyü belirtmek için:

1. Seçin **yönetmek Jenkins** > **yapılandırma sistem** ve ekranı aşağı kaydırarak **bulut** bölümü. Bu örnekte, Docker görüntü değerine güncelleştirme **microsoft/java-üzerinde-azure-jenkins-ikincil**.

   İşiniz bittiğinde seçin **kaydetmek** Jenkins panoya dönmek için.

   ![Jenkins bulut yapılandırması](./media/container-instances-jenkins/jenkins-aci-image.png)

2. Şimdi Jenkins derleme işi oluşturun. Seçin **yeni öğe**, yapı projesi gibi bir ad verin **aci java demo**seçin **Serbest stilde proje**seçip **Tamam**.

   ![Derleme işi ve proje türleri listesi adı kutusu](./media/container-instances-jenkins/jenkins-new-job.png)

3. Altında **genel**, emin **burada bu proje çalıştırılabilir sınırla** seçilir. Girin **linux** etiket ifadesi için. Bu derleme işi ACI Bulutu üzerinde çalışan bu yapılandırmayı sağlar.

   !["Genel" sekmesindeki yapılandırma ayrıntıları ile](./media/container-instances-jenkins/jenkins-job-01.png)

4. Altında **kaynak kodu Yönetimi**seçin **Git** ve girin **https://github.com/spring-projects/spring-petclinic.git** depo URL'si için. Bu GitHub deposuna örnek uygulama kodunu içerir.

   ![Kaynak kodu bilgileriyle "Kaynak kodu Yönetimi" sekmesi](./media/container-instances-jenkins/jenkins-job-02.png)

5. Altında **yapı**seçin **Ekle derleme adımı** seçip **en üst düzey Maven hedefleri çağırma**. Girin **paket** derleme adımı hedef olarak.

   ![Derleme adımı seçimlerini "Oluştur" sekmesi](./media/container-instances-jenkins/jenkins-job-03.png)

6. **Kaydet**’i seçin.

## <a name="run-the-build-job"></a>Yapı işini çalıştır

Derleme işi sınamak ve Azure kapsayıcı örnekleri derleme platformu olarak izlemek için bir yapı el ile başlatın.

1. Seçin **şimdi yapı** bir derleme işi başlatmak için. Başlatılacak işi için birkaç dakika sürer. Aşağıdaki görüntüye benzer bir durum görmeniz gerekir:

   ![İş durumu "Geçmiş derleme" bilgilerle](./media/container-instances-jenkins/jenkins-job-status.png)

2. İş çalışırken, Azure Portalı'nı açın ve Jenkins kaynak grubu bakın. Bir kapsayıcı örneği oluşturuldu görmeniz gerekir. Bu örneği içinde Jenkins işi çalışıyor.

   ![Kaynak grubundaki kapsayıcı örneği](./media/container-instances-jenkins/jenkins-aci.png)

3. Jenkins Jenkins yürütücüler (varsayılan 2) yapılandırılmış sayıdan daha fazla iş çalışırken, birden çok kapsayıcı örneği oluşturulur.

   ![Yeni oluşturulan kapsayıcı örnekler](./media/container-instances-jenkins/jenkins-aci-multi.png)

4. Tüm yapı işleri tamamladıktan sonra kapsayıcı örnekleri kaldırılır.

   ![Kaynak grubu ile kapsayıcı örnekleri kaldırıldı](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde Jenkins hakkında daha fazla bilgi için bkz: [Azure ve Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md