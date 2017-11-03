Azure, uygulamanızın **bu koşulların her ikisi de doğruysa** Python kullandığını saptayacaktır:

* requirements.txt dosyası kök klasöründe
* .py dosyaları VEYA python belirten runtime.txt dosyası kök klasöründe

Durum böyle olduğunda, aşağıdaki ek Python işlemlerinin yanı sıra dosyaların standart eşitlemesini gerçekleştiren Python’a özel bir dağıtım betiğini de kullanacaktır:

* Sanal ortamın otomatik yönetimi
* PIP kullanarak requirements.txt dosyasında listelenen paketlerin yüklenmesi
* Seçili Python sürümü temelinde uygun web.config oluşturun.
* Django uygulamaları için statik dosyaları toplama

Betiği özelleştirmek zorunda kalmadan, varsayılan dağıtım adımlarının bazı yönlerini denetleyebilirsiniz.

Python’a özel dağıtım adımlarını atlamak istiyorsanız bu boş dosyayı oluşturabilirsiniz:

    \.skipPythonDeployment

Dağıtım üzerinde daha fazla denetim için aşağıdaki dosyaları oluşturarak varsayılan dağıtım betiğini geçersiz kılabilirsiniz:

    \.deployment
    \deploy.cmd

Kullanabileceğiniz [Azure komut satırı arabirimi] [ Azure command-line interface] dosyaları oluşturmak için.  Bu komutu proje klasörünüzden kullanın:

    azure site deploymentscript --python

Bu dosyalar olmadığında, Azure geçici bir dağıtım betiği oluşturur ve bunu çalıştırır.  Yukarıdaki komutla oluşturduğunuzun aynısıdır.

[Azure command-line interface]: http://azure.microsoft.com/downloads/
