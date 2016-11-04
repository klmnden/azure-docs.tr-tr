---
title: Azure IoT Paketi ve Azure Active Directory | Microsoft Docs
description: Azure IoT Paketinin izinleri yönetmek için Azure Active Directory’i nasıl kullandığını açıklar.
services: ''
suite: iot-suite
documentationcenter: ''
author: aguilaaj
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2016
ms.author: araguila

---
# azureiotsuite.com sitesindeki izinler
## Oturum açtığınızda ne olur
[azureiotsuite.com][lnk-azureiotsuite] sayfasında ilk kez oturum açtığınızda o anda seçili olan Azure Active Directory (AAD) kiracısına ve Azure aboneliğine göre sahip olduğunuz izin düzeyleri site tarafından belirlenir.

1. Site, oturum açmış kullanıcı adınızın yanında görülen kiracılar listesini doldurmak üzere ilk olarak Azure’dan hangi AAD kiracılarına ait olduğunuzu öğrenir. Bu noktada site bir kiracı için aynı anda yalnızca bir kullanıcı belirteci edinebilir. Sonuç olarak, sağ üst köşedeki açılır menüyü kullanarak farklı bir kiracıya geçiş yaptığınızda site ilgili kiracının belirteçlerini almak için bu kiracıda oturumunuzu yeniden açar.
2. Site daha sonra, seçilen kiracı ile hangi aboneliğin ilişkilendirildiğini Azure’dan öğrenir. Önceden yapılandırılmış yeni bir çözüm oluşturduğunuzda kullanılabilir abonelikleri görürsünüz.
3. Site son olarak önceden yapılandırılmış çözümler olarak etiketlenmiş abonelikler ve kaynak gruplarındaki tüm kaynakları alır ve giriş sayfasındaki kutucukları doldurur.

Aşağıdaki bölümlerde önceden yapılandırılmış çözümlere erişimi denetleyen roller açıklanmıştır.

## AAD rolleri
AAD rolleri önceden yapılandırılmış çözümleri sağlama özelliğini denetler ve önceden yapılandırılmış bir çözümdeki kullanıcıları yönetir.

AAD’deki yönetici rolleri hakkında daha fazla bilgiyi [Azure AD’de yönetici rolleri atama][lnk-aad-admin] içinde bulabilirsiniz, ancak bu makale birincil olarak önceden yapılandırılmış çözümler tarafından kullanılan **Genel Yönetici** ve **Etki Alanı Kullanıcısı/Üyesi** rollerine odaklanmaktadır.

**Genel Yönetici:** AAD kiracısı başına çok sayıda genel yönetici olabilir. Bir AAD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel yöneticisi olursunuz. Genel yönetici önceden yapılandırılmış bir çözüm sağlayabilir ve AAD kiracılarının içindeki uygulama için kendisine **YÖNETİCİ** rolü atanabilir. Ancak, aynı AAD kiracısındaki başka bir kullanıcı bir uygulama oluşturduğunda genel yöneticinin varsayılan rolü **ÖRTÜK SALT OKUNUR** şeklindedir. Genel yöneticiler [Klasik Azure portalı][lnk-classic-portal] kullanarak uygulamalar için roller atayabilir.

**Etki Alanı Kullanıcısı/Üyesi:** Bir AAD kiracısı için çok sayıda etki alanı kullanıcısı/üyesi olabilir. Bir etki alanı kullanıcısı [azureiotsuite.com][lnk-azureiotsuite] sitesi üzerinden önceden yapılandırılmış bir çözüm sağlayabilir. Bu kullanıcılara, sağladıkları uygulama için verilen varsayılan rol **YÖNETİCİ**’dir. Kullanıcılar, [azure-iot-remote-monitoring][lnk-rm-github-repo] veya [azure-iot-predictive-maintenance][lnk-pm-github-repo] deposunda build.cmd komut dosyasını kullanarak bir uygulama oluşturabilirler, ancak rol atama izinleri olmadığı için kendilerine verilen varsayılan rol **ÖRTÜK SALT OKUNUR** rolüdür. AAD kiracısında başka bir kullanıcı bir uygulama oluşturduğunda bu kişilere o uygulama için **ÖRTÜK SALT OKUNUR** rolü varsayılan olarak atanır. Bunlar uygulamalar için rol atama özelliğine sahip değildir; bu nedenle, sağlayan kendileri olsa bile bir uygulama için kullanıcı ya da kullanıcı rolleri ekleyemezler.

**Konuk Kullanıcı/Konuk:** Bir AAD kiracısı için çok sayıda konuk kullanıcı/konuk olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, konuk kullanıcılar AAD kiracısında önceden yapılandırılmış bir çözüm sağlayamaz.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD’de kullanıcı oluşturma ve düzenleme][lnk-create-edit-users]
* [AAD’de Uygulama rolleri atama][lnk-assign-app-roles]

## Azure abonelik yöneticisi rolleri
Azure yöneticisi rolleri bir Azure aboneliğini bir AD kiracısıyla eşleme özelliğini denetler.

Azure Ortak Yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi rolleri hakkında daha fazla bilgi için [Azure Ortak Yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme][lnk-admin-roles] makalesine bakın.

## Uygulama rolleri
Uygulama rolleri önceden yapılandırılmış çözümünüzdeki cihazlara erişimi denetler.

Önceden yapılandırılmış bir çözüm sağladığınızda oluşturulan uygulamada iki tanımlanmış rol ve bir örtük rol mevcuttur.

* **YÖNETİCİ:** Cihaz ekleme, yönetme ve kaldırma üzerinde tam denetime sahiptir
* **SALT OKUNUR:** Cihazları görüntüleyebilir
* **ÖRTÜK SALT OKUNUR:** Salt Okunur ile aynıdır, ancak AAD kiracınızın tüm kullanıcılarına verilir. Dağıtım sırasında kolaylık sağlamak üzere yapılmıştır. [RolePermissions.cs][lnk-resource-cs] kaynak dosyasını değiştirerek bu rolü kaldırabilirsiniz.

### Bir kullanıcının uygulama rollerini değiştirme
Active Directory’de bir kullanıcıyı önceden yapılandırılmış çözümünüzün yöneticisi yapmak için aşağıdaki yordamı kullanabilirsiniz.

Bir kullanıcının rollerini değiştirmek için AAD genel yöneticisi olmanız gerekir:

1. [Klasik Azure portalına][lnk-classic-portal] gidin.
2. **Active Directory**’yi seçin.
3. AAD kiracınızın adına tıklayın (çözümünüzü sağlarken azureiotsuite.com üzerinde seçtiğiniz dizindir).
4. **Uygulamalar**'a tıklayın.
5. Önceden yapılandırılmış çözümünüzün adıyla eşleşen uygulamanın adına tıklayın. Uygulamanızı listede görmüyorsanız **Göster** açılır listesini **Şirketimin sahip olduğu uygulamalar** olarak değiştirin ve onay işaretine tıklayın.
6. **Kullanıcılar**’a tıklayın.
7. Rollerini değiştirmek istediğiniz kullanıcıyı seçin.
8. **Ata**’ya tıklayın ve kullanıcıya atamak istediğiniz rolü (**Yönetici** gibi) seçip onay işaretine tıklayın.

## SSS
### Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bunu nasıl yapabilirim?
1. [Klasik Azure portalına][lnk-classic-portal] gidin, sol taraftaki hizmet listesinde **Ayarlar**’a tıklayın.
2. Dizin eşlemesini değiştirmek istediğiniz aboneliği seçin.
3. **Dizini Düzenle**’ye tıklayın.
4. Açılır listede kullanmak istediğiniz **Dizin**’i seçin. İleri okuna tıklayın.
5. Dizin eşlemesini ve etkilenen ortak yöneticileri onaylayın. Başka bir dizinden taşıdığınız için özgün dizindeki tüm ortak yöneticiler kaldırılır.

### AAD kiracısı üzerinde etki alanı kullanıcısı/üyesiyim ve önceden yapılandırılmış bir çözüm oluşturdum. Uygulamam için bana nasıl rol atanır?
Kullanıcılara kendiniz rol atamak istiyorsanız AAD üzerindeki bir genel yöneticiden sizi genel yönetici olarak atamasını isteyin ya da bir genel yöneticiden size bir rol atamasını isteyin. Önceden yapılandırılmış çözümünüzün dağıtıldığı AAD kiracısını değiştirmek isterseniz sonraki soruya bakın.

### Uzaktan izlediğim önceden yapılandırılmış çözümümün ve uygulamamın atandığı AAD kiracısına nasıl geçiş yapabilirim?
<https://github.com/Azure/azure-iot-remote-monitoring> adresinden bir bulut dağıtımını yeniden çalıştırabilir ve yeni oluşturulan bir AAD kiracısı ile yeniden dağıtabilirsiniz. Yeni bir AAD kiracısı oluşturduğunuzda varsayılan olarak genel yönetici olduğunuz için, kullanıcı ekleme ve bu kullanıcılara roller atama erişiminiz olacaktır.

1. [Azure Yönetim portalında][lnk-classic-portal] yeni bir AAD dizini oluşturun.
2. <https://github.com/Azure/azure-iot-remote-monitoring> sayfasına gidin.
3. `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` komutunu çalıştırın (Örneğin, `build.cmd cloud debug myRMSolution`)
4. Sorulduğunda **tenantid** değerini önceki kiracınız yerine yeni oluşturduğunuz kiracı olarak ayarlayın.

### Şirket hesabıyla oturum açtığımda bir Hizmet Yöneticisini veya Ortak Yöneticiyi değiştirmek istiyorum
[Şirket hesabıyla oturum açıldığında Hizmet Yöneticisini ve Ortak Yöneticiyi Değiştirme][lnk-service-admins] başlıklı destek makalesine bakın.

### Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."
Aşağıdaki diyagrama göz atın:

![][img-flowchart]

> [!NOTE]
> AAD kiracısında genel yönetici ve abonelikte ortak yönetici olduğunuzu doğruladıktan sonra hatayı görmeye devam ederseniz hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atamasını isteyin: kullanıcı genel yönetici olarak ekleme ve ardından kullanıcıyı Azure aboneliğinde ortak yönetici olarak ekleme. Sorun devam ederse lütfen [Yardım ve Destek][lnk Yardım desteği] bölümüne başvurun.
> 
> 

**Bir Azure aboneliğim varken bu hatayı neden görüyorum?** *Önceden yapılandırılmış çözümler oluşturmak için bir Azure aboneliği gereklidir. Yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz.*

Bir Azure aboneliğinizin olduğundan eminseniz aboneliğinizin kiracı eşlemesini doğrulayın ve açılır menüden doğru kiracının seçildiğinden emin olun. İstenilen kiracının doğru olduğunu onayladıysanız yukarıdaki diyagramı izleyin ve aboneliğiniz ile bu AAD kiracısının eşlemesini doğrulayın.

## Sonraki adımlar
IoT Paketi hakkında bilgi almaya devam etmek için [önceden yapılandırılmış çözümü özelleştirme][lnk-customize] işlemine bakın.

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk Yardım desteği]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md


<!--HONumber=Aug16_HO1-->


