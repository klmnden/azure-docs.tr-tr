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
ms.openlocfilehash: dbe418db8fb3d73739a30b60703f15b3dc47b291
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Aracı kullanmak Azure kapsayıcı örnekleri bir Jenkins olarak derleme

Azure kapsayıcı örnekleri kapsayıcılı iş yükünü çalıştırmak için isteğe bağlı, burstable ve yalıtılmış bir ortam sağlar. Bu öznitelikler nedeniyle Azure kapsayıcı örnekleri büyük ölçekli olarak çalışan Jenkins derleme işi için harika bir platform haline. Bu belge, dağıtma ve yapı hedefi olarak ACI ile önceden yapılandırılmış bir Jenkins sunucusu kullanarak size yol göstermektedir.

Azure kapsayıcı örnekleri hakkında daha fazla bilgi için bkz: [Azure kapsayıcı örnekleri hakkında][about-aci].

## <a name="deploy-jenkins-server"></a>Jenkins sunucusu dağıtma

Azure portalında seçin **kaynak oluşturma** arayın ve **Jenkins**. Jenkins teklifi bir yayımcısı ile seçin **Microsoft** seçip **oluşturma**.

Temellerini forma aşağıdaki bilgileri girin ve tıklayın **Tamam** bittiğinde.

- **Ad** - Jenkins dağıtım adı.
- **Kullanıcı adı** -bu kullanıcı adı Jenkins sanal makine için yönetici kullanıcı olarak kullanılır.
- **Kimlik doğrulama türü** - SSH ortak anahtarı önerilir. Seçili olduğunda, Jenkins sanal makinede oturum açarken kullanılacak bir SSH ortak anahtarını kopyalayın.
- **Abonelik** -Azure aboneliğini seçin.
- **Kaynak grubu** - yeni oluşturun veya varolan bir kaynak grubu seçin.
- **Konum** -Jenkins sunucu için bir konum seçin.

![Jenkins portal dağıtımı temel ayarları](./media/container-instances-jenkins/jenkins-portal-01.png)

Ek ayarlar formunda, aşağıdaki öğeleri tamamlayın:

- **Boyutu** -Jenkins sanal makineniz için uygun boyutlandırma seçeneği belirleyin.
- **VM disk türü** -HDD (sabit disk sürücüsü) veya SSD (katı hal sürücüsü) için Jenkins sunucusu belirtin.
- **Sanal ağ** -varsayılan ayarları değiştirmek için seçin (isteğe bağlı) sanal ağ.
- **Alt ağlar** - seçin alt ağlar, bilgileri doğrulayın ve seçin **Tamam**.
- **Genel IP adresi** -genel IP adresi seçerek özel bir ad verin, SKU ve atama yöntemi yapılandırmak olanak tanır.
- **Etki alanı adı etiketi** -Jenkins sanal makine için tam bir URL oluşturmak için bir değer belirtin.
- **Jenkins yayın türü** -istenen sürüm seçeneklerden birini seçin: LTS, haftalık derleme veya Azure doğrulandı.

![Jenkins portal dağıtımı ek ayarlar](./media/container-instances-jenkins/jenkins-portal-02.png)

Hizmet asıl tümleştirmesi seçin **Auto(MSI)** olmasını [Azure yönetilen hizmet kimliği] [ managed-service-identity] otomatik olarak Jenkins örneği için bir kimlik doğrulama kimlik oluşturma. El ile sağlayıcı için kendi hizmet asıl kimlik bilgilerini seçin.

Jenkins işleri oluşturmak için bulut aracıları bulut tabanlı bir platform yapılandırın. Bu belge amacıyla ACI seçin. ACI bulut aracıyla her Jenkins derleme işi Azure kapsayıcı örneği çalıştırılır.

![Jenkins portal dağıtımı bulut tümleştirme ayarları](./media/container-instances-jenkins/jenkins-portal-03.png)

İle tümleştirme ayarlarını yaptıktan sonra tıklatın **Tamam**ve ardından **Tamam** yeniden üzerinde doğrulama özeti. Tıklatın **oluşturma** Kullanım Özeti koşullarını. Jenkins sunucusu dağıtmak için birkaç dakika sürer.

## <a name="configure-jenkins"></a>Jenkins’i yapılandırma

Azure portalında Jenkins kaynak grubuna gidin, Jenkins sanal makineyi seçin ve DNS adını not edin.

![Jenkins oturum açma yönergeleri](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

DNS adı Jenkins VM ve kopyalama döndürülen SSH dize tarayıcıya.

![Jenkins oturum açma yönergeleri](./media/container-instances-jenkins/jenkins-portal-04.png)

Geliştirme sisteminizde terminal oturumu açın ve son adımı SSH dizeden yapıştırın. 'Username' Jenkins sunucusu dağıtımı sırasında belirtilen kullanıcı adı için güncelleştirin.

Bağlantı kurulduktan sonra ilk yönetici parolası almak için aşağıdaki komutu çalıştırın.

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

SSH oturumu ve çalışan tünel bırakın ve gidin http://localhost:8080 bir tarayıcıda. Aşağıdaki resimde görüldüğü gibi alan ilk yönetici parolasını yapıştırın. Seçin **devam** bittiğinde.

![Jenkins kilidini aç](./media/container-instances-jenkins/jenkins-portal-05.png)

Seçin **önerilen Eklentileri yüklemek** tüm yüklemek için Jenkins eklentileri önerilir.

![Jenkins eklentisini yükleme](./media/container-instances-jenkins/jenkins-portal-06.png)

Yeni bir yönetici kullanıcı hesabı oluşturun. Bu hesap, oturum açma ve Jenkins örneği ile çalışmak için kullanılır.

![Jenkins kullanıcı oluştur](./media/container-instances-jenkins/jenkins-portal-07.png)

Seçin **kaydedin ve son** işiniz bittiğinde ve ardından **Jenkins kullanmaya başlamak** yapılandırmayı tamamlamak için.

Jenkins yapılandırılmıştır ve yapı kod ve dağıtmak için hazır. Bu örnekte, basit bir Java uygulaması, Azure kapsayıcı örnekleri Jenkins yapıyı göstermek için kullanılır.

## <a name="create-build-job"></a>Derleme işi oluşturma

Hedef Jenkins bir kapsayıcı görüntüsü kullanarak oluşturma sırasında tüm başarılı bir derleme için gerekli araçları içeren bir görüntü belirtmeniz gerekir. Görüntüyü belirtmek için işaretleyin **yönetmek Jenkins** > **yapılandırma sistem** ve ekranı aşağı kaydırarak **bulut** bölümü. Bu örnekte, Docker görüntü değerine güncelleştirme `microsoft/java-on-azure-jenkins-slave`.

Bunu yaptıktan sonra tıklatın **kaydetmek** Jenkins panoya dönmek için.

![Jenkins bulut yapılandırması](./media/container-instances-jenkins/jenkins-aci-image.png)

Şimdi Jenkins derleme işi oluşturun. Seçin **yeni öğe**, yapı projesi gibi bir ad verin `aci-java-demo`seçin **Serbest stilde proje**, tıklatıp **Tamam**.

![Jenkins işi oluşturma](./media/container-instances-jenkins/jenkins-new-job.png)

Altında **genel**, emin **burada bu proje çalıştırılabilir sınırla** seçilir. Girin `linux` etiket ifadesi için. Bu derleme işi ACI Bulutu üzerinde çalışan bu yapılandırmayı sağlar.

![Jenkins işi oluşturma](./media/container-instances-jenkins/jenkins-job-01.png)

Kaynak kodu Yönetimi altında seçin `Git` ve girin `https://github.com/spring-projects/spring-petclinic.git` depo URL'si için. Bu GitHub deposuna örnek uygulama kodunu içerir.

![Kaynak kodu Jenkins işe ekleme](./media/container-instances-jenkins/jenkins-job-02.png)

Yapı altında seçin **bir derleme adımı ekleme** seçip `Invoke top-level Maven targets`. Girin `package` derleme adımı hedef olarak.

![Jenkins derleme adımı ekleme](./media/container-instances-jenkins/jenkins-job-03.png)

Seçin **kaydetmek** bittiğinde.

## <a name="run-the-build-job"></a>Yapı işini çalıştır

Derleme işi sınamak ve Azure kapsayıcı örnekleri derleme platformu olarak izlemek için bir yapı el ile başlatın.

Seçin **şimdi yapı** bir derleme işi başlatmak için. İş çalışırken başlatmak birkaç dakika sürer, durumu aşağıdaki görüntülere benzer görmeniz gerekir.

![Jenkins derleme adımı ekleme](./media/container-instances-jenkins/jenkins-job-status.png)

İş çalışırken, Azure Portalı'nı açın ve Jenkins kaynak grubu bakın. Azure kapsayıcı örneği oluşturdu görmeniz gerekir. Jenkins işi çalışıyor Bu örneği içinde değil.

![ACI Jenkins derlemelerde](./media/container-instances-jenkins/jenkins-aci.png)

Jenkins Jenkins yürütücüler (varsayılan 2) yapılandırılmış sayıdan daha fazla iş çalışırken, birden çok Azure kapsayıcı örneği oluşturulur.

![ACI Jenkins derlemelerde](./media/container-instances-jenkins/jenkins-aci-multi.png)

Tüm yapı işleri tamamladıktan sonra Azure kapsayıcı örnekleri kaldırılır.

![ACI Jenkins derlemelerde](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure bkz Jenkins hakkında daha fazla bilgi edinmek için [Azure ve Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md