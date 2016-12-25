FreshmailBundle
===============

Łatwe odpytywanie usług FreshMail (http://freshmail.pl) w formie bundla dla symfonii 2

Biblioteczka bazuje na kodzie z:
https://github.com/FreshMail/REST-API

INSTALACJA
----------

Do połączen http bundle korzysta z Buzza (https://github.com/kriswallsmith/Buzz). Instalacja przez kompozytora:
  
    "kriswallsmith/buzz": "dev-master",
    "qbitz/freshmailbundle": "dev-master"

Dodanie bundla do AppKernel

    new Qbitz\FreshmailBundle\QbitzFreshmailBundle(),

KONFIGURACJA
------------

W config.yml wymagana jest konfiguracja dla bundla:

    qbitz_freshmail:
        buzz_client:          ~ # One of "curl"; "file_get_contents"
        buzz_timeout:         5
        api_key:              null # Required
        api_secret:           null # Required
        host:                 'https://app.freshmail.com/'
        prefix:               rest/

Tylko api_key i api_secret są wymagane, host i prefix lepiej nie zmieniać bo na razie tylko tak można się odwoływać.

Opcja buzz_client ustawia typ klienta dla Buzza, tylko Curl i file_get_contents, domyślnie Curl.

WYKORZYSTANIE
-------------

Do kontenera dodawana jest usługa 'qbitz.freshmail', która udostępnia tylko metodą doRequest()

  $arrayResponse = $container->get('qbitz.freshmail')->doRequest($url, $data);

gdzie url to url do usługi FreshMail, np.: 'ping', 'subscriber/add' (info na http://freshmail.pl/developer-api/autoryzacja/), a data to tablica zmiennych wysyłanych w żądaniu.

Przykłady:

    $browser->doRequest('ping');
    $browser->doRequest('subscriber/add', array( 'email'=>'kalapucka@example.com', 'list'=>'hashyhash', 'state'=>1 ));

KOMENDA W KONSOLI
-----------------

Używana tak samo jak metoda doRequest
  
    ./app/console qbitz:freshmail:doRequest ping
    ./app/console qbitz:freshmail:doRequest subscriber/add --vars="{\"email\"=>\"kalapucka@example.com\",\"list\"=>\"hashyhash\",\"state\"=>1}"
