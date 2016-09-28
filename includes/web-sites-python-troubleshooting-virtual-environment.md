Uyumlu sanal ortamın zaten olduğunu algılarsa, dağıtım betiği Azure’da sanal ortamın oluşturulmasını atlar.  Böylece dağıtım önemli ölçüde hızlandırılabilir.  Zaten yüklü olan paketler pip tarafından atlanır.

Belirli durumlarda, sanal ortamın silinmesini zorlayabilirsiniz.  Sanal ortamı deponuzun bir parçası olarak eklemeye karar verirseniz bunu yapmak istersiniz.  Bu işlemi, bazı paketlerden uzak kalmanız veya requirements.txt dosyasındaki değişiklikleri sınamanız gerektiğinde de yapmak isteyebilirsiniz.

Azure’da var olan sanal ortamı yönetmek için birkaç seçenek vardır:

### Seçenek 1: FTP Kullanma

FTP istemcisiyle sunucuya bağlanın; env klasörünü artık silebileceksiniz.  Bazı FTP istemcilerinin (örneğin, web tarayıcıları) salt okunur olabileceğini ve bu klasörleri silmenize izin verilmeyeceğini unutmayın; bu beceriye sahip FTP istemcisi kullandığınızdan emin olmak istersiniz.  FTP konak adı ve kullanıcısı [Azure portalında](https://portal.azure.com), web uygulamanızın dikey penceresinde görüntülenir.

### Seçenek 2: Çalışma zamanını değiştirme

Burada, Python’un istenen sürümüyle eşleşmediğinde dağıtım betiğinin env klasörünü sileceği olgusunun avantajlarını kazanan bir alternatif bulunmaktadır.  Böylece var olan ortam etkin olarak silinir ve yeni bir tane oluşturulur.

1. Python’un farklı bir sürümüne geçin (runtime.txt veya Azure Portal’da **Uygulama Ayarları** dikey penceresi aracılığıyla)
1. git bazı değişiklikleri gönderir (varsa, pip yükleme hatalarını yoksayın)
1. Python’un ilk sürümüne geri dönün
1. Git bazı değişiklikleri yeniden gönderir

### Seçenek 3: Dağıtım betiğini özelleştirme

Dağıtım betiğini özelleştirdiyseniz, env klasörünü silmesi amacıyla zorlamak için deploy.cmd dosyasında kodu değiştirebilirsiniz.


<!--HONumber=Sep16_HO3-->


