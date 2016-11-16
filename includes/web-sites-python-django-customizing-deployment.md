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

Django uygulamanız için statik dosya toplamayı atlamak istiyorsanız:

    \.skipDjango 

Dağıtım üzerinde daha fazla denetim için aşağıdaki dosyaları oluşturarak varsayılan dağıtım betiğini geçersiz kılabilirsiniz:

    \.deployment
    \deploy.cmd

Dosyaları oluşturmak için [Azure komut satırı arabirimi][Azure komut satırı arabirimi]’ni kullanabilirsiniz.  Bu komutu proje klasörünüzden kullanın:

    azure site deploymentscript --python

Bu dosyalar olmadığında, Azure geçici bir dağıtım betiği oluşturur ve bunu çalıştırır.  Yukarıdaki komutla oluşturduğunuzun aynısıdır.

[Azure komut satırı arabirimi]: http://azure.microsoft.com/downloads/


<!--HONumber=Nov16_HO2-->


