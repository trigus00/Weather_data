
# WeatherPy
----

#### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
import pprint
import csv


# Import API key
from api_keys import api_key

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    # Replace spaces with %20 to create url correctly 
    city = city.replace(" ", "%20")
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)
pp = pprint.PrettyPrinter(indent=4)
pp.pprint(cities)


```

    [   'kudahuvadhoo',
        'olafsvik',
        'san%20quintin',
        'rikitea',
        'ushuaia',
        'sinnamary',
        'cherskiy',
        'honiara',
        'castro',
        'hobart',
        'port%20lincoln',
        'bethel',
        'soledade',
        'new%20norfolk',
        'mataura',
        'georgetown',
        'champerico',
        'naze',
        'hermanus',
        'atuona',
        'amderma',
        'busselton',
        'cap%20malheureux',
        'puerto%20baquerizo%20moreno',
        'asyut',
        'dikson',
        'kargil',
        'vaitupu',
        'tiksi',
        'komsomolskiy',
        'saint-philippe',
        'jieshi',
        'nikolskoye',
        'sao%20filipe',
        'barentsburg',
        'taolanaro',
        'hasaki',
        'tabiauea',
        'kamaishi',
        'pisco',
        'copperas%20cove',
        'chuy',
        'vaini',
        'bluff',
        'dubbo',
        'albany',
        'longyearbyen',
        'praia%20da%20vitoria',
        'arosa',
        'khipro',
        'guerrero%20negro',
        'vao',
        'avarua',
        'bognor%20regis',
        'jamestown',
        'talnakh',
        'kalmunai',
        'imbituba',
        'kaeo',
        'chokurdakh',
        'puerto%20escondido',
        'general%20roca',
        'illoqqortoormiut',
        'tasiilaq',
        'tazovskiy',
        'lebu',
        'hay%20river',
        'kodiak',
        'uyo',
        'ribeira%20grande',
        'vila%20velha',
        'aswan',
        'plouzane',
        'kahului',
        'lompoc',
        'tuatapere',
        'saint-georges',
        'barguzin',
        'boca%20do%20acre',
        'moose%20factory',
        'victoria',
        'martapura',
        'tricase',
        'bredasdorp',
        'kavieng',
        'roald',
        'ghanzi',
        'carnarvon',
        'sentyabrskiy',
        'mount%20isa',
        'gogrial',
        'alta%20floresta',
        'kapaa',
        'qabis',
        'knysna',
        'saint%20george',
        'saleaula',
        'torbay',
        'conceicao%20do%20araguaia',
        'merritt%20island',
        'pangnirtung',
        'dakar',
        'anloga',
        'khatanga',
        'samusu',
        'gat',
        'eskasem',
        'port%20alfred',
        'beringovskiy',
        'cabo%20san%20lucas',
        'morant%20bay',
        'manaus',
        'narsaq',
        'illintsi',
        'luderitz',
        'tuggurt',
        'dharchula',
        'hofn',
        'weyburn',
        'lashio',
        'punta%20arenas',
        'havoysund',
        'shenjiamen',
        'san%20pedro',
        'vila%20franca%20do%20campo',
        'tuktoyaktuk',
        'lichinga',
        'simao',
        'meulaboh',
        'tsihombe',
        'kavaratti',
        'port%20elizabeth',
        'cape%20town',
        'basco',
        'qaanaaq',
        'chany',
        'maldonado',
        'acapulco',
        'saldanha',
        'luanda',
        'provideniya',
        'husavik',
        'aklavik',
        'college',
        'birjand',
        'barrow',
        'etchoropo',
        'rawson',
        'faanui',
        'bontang',
        'lata',
        'point%20fortin',
        'geraldton',
        'sohag',
        'hilo',
        'rio%20grande',
        'upernavik',
        'koppal',
        'te%20anau',
        'garut',
        'hithadhoo',
        'ibra',
        'lima',
        'valparaiso',
        'ithaki',
        'sioux%20lookout',
        'risor',
        'hatod',
        'buariki',
        'aripuana',
        'richards%20bay',
        'souillac',
        'samarai',
        'maghama',
        'taltal',
        'krasnotorka',
        'ponta%20do%20sol',
        'poya',
        'shanhetun',
        'cidreira',
        'bure',
        'broome',
        'tomohon',
        'dien%20bien',
        'novyy%20urengoy',
        'isangel',
        'umzimvubu',
        'port%20macquarie',
        'beira',
        'selma',
        'kaitangata',
        'abha',
        'katsuura',
        'wad%20madani',
        'pontes%20e%20lacerda',
        'esperance',
        'chikwawa',
        'pevek',
        'tynda',
        'tumannyy',
        'cayenne',
        'san%20policarpo',
        'kupang',
        'kosa',
        'bodden%20town',
        'majene',
        'bengkulu',
        'butaritari',
        'nizhneyansk',
        'mundra',
        'sorland',
        'pouebo',
        'aykhal',
        'severo-kurilsk',
        'sorvag',
        'pokhara',
        'phan%20thiet',
        'correntina',
        'baykalsk',
        'keuruu',
        'kruisfontein',
        'vostok',
        'arica',
        'robe',
        'yellowknife',
        'saskylakh',
        'boyolangu',
        'las%20choapas',
        'cockburn%20town',
        'mar%20del%20plata',
        'mahebourg',
        'shache',
        'novyy%20urgal',
        'kieta',
        'navirai',
        'khani',
        'hami',
        'sao%20joao%20da%20barra',
        'mount%20gambier',
        'xiangfan',
        'arraial%20do%20cabo',
        'jumla',
        'santa%20marta',
        'karauzyak',
        'bambous%20virieux',
        'chirongui',
        'jalu',
        'sarangani',
        'attawapiskat',
        'the%20valley',
        'pochutla',
        'bolungarvik',
        'clyde%20river',
        'east%20london',
        'qaqortoq',
        'manaure',
        'marv%20dasht',
        'road%20town',
        'mkuranga',
        'adrar',
        'sharjah',
        'tshikapa',
        'buchanan',
        'babanusah',
        'hit',
        'matara',
        'hualmay',
        'bijar',
        'palivere',
        'zelenogorskiy',
        'bonthe',
        'manggar',
        'dembi%20dolo',
        'zhangjiakou',
        'yunjinghong',
        'jawhar',
        'comodoro%20rivadavia',
        'vidim',
        'khuzhir',
        'sassandra',
        'muzhi',
        'akyab',
        'puerto%20ayora',
        'tsuchiura',
        'tawkar',
        'yining',
        'rimbey',
        'leshukonskoye',
        'albury',
        'srandakan',
        'kjollefjord',
        'dalby',
        'garhakota',
        'mys%20shmidta',
        'gbadolite',
        'alofi',
        'bandarbeyla',
        'vite',
        'sakakah',
        'saint-pierre',
        'dong%20hoi',
        'lagoa',
        'suez',
        'kaberamaido',
        'nanortalik',
        'louga',
        'kisangani',
        'ixtapa',
        'marcona',
        'stornoway',
        'santa%20catalina',
        'roquetas%20de%20mar',
        'maple%20creek',
        'erdenet',
        'salinopolis',
        'roros',
        'borovskoy',
        'moron',
        'tete',
        'antonina',
        'san%20patricio',
        'mackay',
        'marystown',
        'ahipara',
        'luneville',
        'kyaikkami',
        'utiroa',
        'monte%20alegre',
        'bur%20gabo',
        'nouadhibou',
        'lurgan',
        'nanning',
        'pemangkat',
        'kasane',
        'cabedelo',
        'kadykchan',
        'leningradskiy',
        'sampit',
        'gizo',
        'belushya%20guba',
        'tytuvenai',
        'sawakin',
        'san%20cristobal',
        'dombarovskiy',
        'fortuna',
        'chapais',
        'zhengjiatun',
        'lavrentiya',
        'petropavlovsk-kamchatskiy',
        'victoria%20point',
        'karratha',
        'rakhya',
        'bengkalis',
        'awbari',
        'sidi%20bin%20nur',
        'quatre%20cocos',
        'vagur',
        'dmytrivka',
        'tura',
        'chokwe',
        'melfort',
        'sitka',
        'airai',
        'kotelnich',
        'halalo',
        'los%20llanos%20de%20aridane',
        'cheuskiny',
        'palembang',
        'yulara',
        'esso',
        'svetlaya',
        'kuala%20kedah',
        'awjilah',
        'wasilla',
        'lillesand',
        'yarim',
        'kapit',
        'viljandi',
        'araguaina',
        'areia',
        'maningrida',
        'tiffin',
        'payson',
        'preobrazheniye',
        'el%20balyana',
        'kidal',
        'borogontsy',
        'tenenkou',
        'sunyani',
        'tautira',
        'port%20blair',
        'staryy%20nadym',
        'taoudenni',
        'jinzhou',
        'yondo',
        'timra',
        'warqla',
        'ginda',
        'nguruka',
        'panguna',
        'vanavara',
        'vardo',
        'haapiti',
        'mareeba',
        'tanout',
        'shimoda',
        'myrtle%20beach',
        'mbandaka',
        'la%20ronge',
        'svetlogorsk',
        'grindavik',
        'margate',
        'pathein',
        'bac%20lieu',
        'mitu',
        'north%20platte',
        'ancud',
        'fare',
        'corrente',
        'riyadh',
        'sibolga',
        'port%20hedland',
        'ivanivka',
        'ondorhaan',
        'bima',
        'marrakesh',
        'ishigaki',
        'tessalit',
        'temozon',
        'kyzyl-mazhalyk',
        'pilar',
        'opuwo',
        'samoded',
        'middelburg',
        'wanning',
        'avera',
        'vicosa%20do%20ceara',
        'kawana%20waters',
        'perevoz',
        'grand%20centre',
        'shubarkuduk',
        'sabang',
        'termoli',
        'kutum',
        'fairbanks',
        'arman',
        'athabasca',
        'hirara',
        'lincoln',
        'manakara',
        'ilulissat',
        'kawalu',
        'roseburg',
        'grand%20river%20south%20east',
        'ndele',
        'namatanai',
        'seoul',
        'rafaela',
        'santa%20isabel',
        'zhireken',
        'yatou',
        'faya',
        'maun',
        'casian',
        'aksu',
        'azangaro',
        'kurilsk',
        'dunda',
        'paramonga',
        'mudgee',
        'cervo',
        'china',
        'macenta',
        'bela%20vista%20do%20paraiso',
        'puerto%20colombia',
        'ormara',
        'dzhusaly',
        'tarudant',
        'elko',
        'meyungs',
        'sabirabad',
        'caravelas',
        'vila',
        'grand%20gaube',
        'ngunguru',
        'hamilton',
        'labuhan',
        'iqaluit',
        'anlu',
        'nizwa',
        'dingle',
        'mayumba',
        'ayame',
        'acin',
        'dresden',
        'juba',
        'west%20wendover',
        'lasa',
        'karaul',
        'abu%20samrah',
        'linxia',
        'chagda',
        'itarema',
        'saint%20marys',
        'udachnyy',
        'koutsouras',
        'minbu',
        'alvor',
        'marsh%20harbour',
        'karamay',
        'kyren',
        'metkovic',
        'kemptville',
        'emerald',
        'omboue',
        'lorengau',
        'hambantota',
        'san%20rafael',
        'kisesa',
        'westport',
        'goderich',
        'banjarmasin',
        'kemijarvi',
        'constitucion',
        'amga',
        'poum',
        'elk',
        'kirakira',
        'ostrovnoy',
        'atar',
        'atambua',
        'iralaya',
        'encarnacion',
        'sidi%20qasim',
        'khrebtovaya',
        'krasnaya%20gora',
        'guelengdeng',
        'yar-sale',
        'klaksvik',
        'gladstone',
        'ouadda',
        'bridlington',
        'kibala',
        'mehamn',
        'kaabong',
        'ozernovskiy',
        'kholmogory',
        'zeya',
        'fraserburgh',
        'yerky',
        'buala',
        'half%20moon%20bay',
        'thompson',
        'vilyuysk',
        'sisimiut',
        'sehithwa',
        'tomaszow%20lubelski',
        'afmadu',
        'flinders',
        'progreso',
        'itaituba',
        'nome',
        'madinat%20sittah%20uktubar',
        'jacarei',
        'weligama',
        'kirkwall',
        'jagdalpur',
        'kuryk',
        'chulym',
        'pacific%20grove',
        'monrovia',
        'masterton',
        'alice%20springs',
        'bilma',
        'galesburg',
        'lapeer',
        'artur%20nogueira',
        'ankpa',
        'kwinana',
        'vertientes',
        'gushikawa',
        'codrington',
        'jasper',
        'stabat',
        'roanoke%20rapids',
        'noumea',
        'tsabong',
        'norman%20wells',
        'zhanatas',
        'necochea',
        'victor%20rosales',
        'sturgeon%20falls',
        'padang',
        'sabha',
        'kachug',
        'takaka',
        'tilichiki',
        'fort-shevchenko',
        'barawe',
        'price',
        'jinchang',
        'talara',
        'eyl',
        'havelock',
        'walvis%20bay',
        'verkhoyansk']


### Perform API Calls
* Perform a weather check on each city using a series of successive API calls.
* Include a print log of each city as it'sbeing processed (with the city number and city name).



```python

```


```python

```


```python
#creating an empty list 

city_name = []
cloudiness = []
country = []
date = []
humidity = []
lat = []
lng = []
max_temp = []
wind_speed = []
units = "imperial"





url = "http://api.openweathermap.org/data/2.5/weather?"
record = 1 


print ("---Beginning Data Retrieval---")

for city in cities: 

    query_url = url +"appid="+api_key+"&q="+city+"&units="+ units     
   

    try:
        city_data = requests.get(query_url).json()
        
        city_name.append(city_data["name"])
        
        cloudiness.append(city_data["clouds"]["all"])            
        
        country.append(city_data["sys"]["country"])
            
        date.append(city_data["dt"])
        
        humidity.append(city_data["main"]["humidity"])
        
        max_temp.append(city_data["main"]["temp_max"])
            
        lat.append(city_data["coord"]["lat"])
        
        lng.append(city_data["coord"]["lon"])
        
        wind_speed.append(city_data["wind"]["speed"])
        
        city_record = city_data["name"]
        
        print(f"Processing Record {record}| {city_record}")
        print(f"{query_url}") 
        record = record +1 
        
        # Wait a second in loop to not over exceed rate limit of API
        time.sleep(1)
    
   

    except:
        print ("city not found ... skipping ")
    continue 

    
       
print ("---Data Retrieval Complete ---")       
          
       
        
        
        
    
      
        

```

    ---Beginning Data Retrieval---
    Processing Record 1| Kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kudahuvadhoo&units=imperial
    city not found ... skipping 
    Processing Record 2| San Quintin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20quintin&units=imperial
    Processing Record 3| Rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=rikitea&units=imperial
    Processing Record 4| Ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ushuaia&units=imperial
    Processing Record 5| Sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sinnamary&units=imperial
    Processing Record 6| Cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cherskiy&units=imperial
    Processing Record 7| Honiara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=honiara&units=imperial
    Processing Record 8| Castro
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=castro&units=imperial
    Processing Record 9| Hobart
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hobart&units=imperial
    Processing Record 10| Port Lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20lincoln&units=imperial
    Processing Record 11| Bethel
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bethel&units=imperial
    Processing Record 12| Soledade
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=soledade&units=imperial
    Processing Record 13| New Norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=new%20norfolk&units=imperial
    Processing Record 14| Mataura
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mataura&units=imperial
    Processing Record 15| Georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=georgetown&units=imperial
    Processing Record 16| Champerico
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=champerico&units=imperial
    Processing Record 17| Naze
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=naze&units=imperial
    Processing Record 18| Hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hermanus&units=imperial
    Processing Record 19| Atuona
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=atuona&units=imperial
    city not found ... skipping 
    Processing Record 20| Busselton
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=busselton&units=imperial
    Processing Record 21| Cap Malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cap%20malheureux&units=imperial
    Processing Record 22| Puerto Baquerizo Moreno
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=puerto%20baquerizo%20moreno&units=imperial
    Processing Record 23| Asyut
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=asyut&units=imperial
    Processing Record 24| Dikson
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dikson&units=imperial
    Processing Record 25| Kargil
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kargil&units=imperial
    city not found ... skipping 
    Processing Record 26| Tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tiksi&units=imperial
    Processing Record 27| Komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=komsomolskiy&units=imperial
    Processing Record 28| Saint-Philippe
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saint-philippe&units=imperial
    Processing Record 29| Jieshi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jieshi&units=imperial
    Processing Record 30| Nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nikolskoye&units=imperial
    Processing Record 31| Sao Filipe
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sao%20filipe&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 32| Hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hasaki&units=imperial
    city not found ... skipping 
    Processing Record 33| Kamaishi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kamaishi&units=imperial
    Processing Record 34| Pisco
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pisco&units=imperial
    Processing Record 35| Copperas Cove
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=copperas%20cove&units=imperial
    Processing Record 36| Chuy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chuy&units=imperial
    Processing Record 37| Vaini
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vaini&units=imperial
    Processing Record 38| Bluff
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bluff&units=imperial
    Processing Record 39| Dubbo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dubbo&units=imperial
    Processing Record 40| Albany
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=albany&units=imperial
    Processing Record 41| Longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=longyearbyen&units=imperial
    Processing Record 42| Praia da Vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=praia%20da%20vitoria&units=imperial
    Processing Record 43| Arosa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=arosa&units=imperial
    Processing Record 44| Khipro
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=khipro&units=imperial
    Processing Record 45| Guerrero Negro
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=guerrero%20negro&units=imperial
    Processing Record 46| Vao
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vao&units=imperial
    Processing Record 47| Avarua
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=avarua&units=imperial
    Processing Record 48| Bognor Regis
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bognor%20regis&units=imperial
    Processing Record 49| Jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jamestown&units=imperial
    Processing Record 50| Talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=talnakh&units=imperial
    Processing Record 51| Kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kalmunai&units=imperial
    Processing Record 52| Imbituba
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=imbituba&units=imperial
    Processing Record 53| Kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kaeo&units=imperial
    Processing Record 54| Chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chokurdakh&units=imperial
    Processing Record 55| Puerto Escondido
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=puerto%20escondido&units=imperial
    Processing Record 56| General Roca
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=general%20roca&units=imperial
    city not found ... skipping 
    Processing Record 57| Tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tasiilaq&units=imperial
    Processing Record 58| Tazovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tazovskiy&units=imperial
    Processing Record 59| Lebu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lebu&units=imperial
    Processing Record 60| Hay River
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hay%20river&units=imperial
    Processing Record 61| Kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kodiak&units=imperial
    Processing Record 62| Uyo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=uyo&units=imperial
    Processing Record 63| Ribeira Grande
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ribeira%20grande&units=imperial
    Processing Record 64| Vila Velha
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vila%20velha&units=imperial
    Processing Record 65| Aswan
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=aswan&units=imperial
    Processing Record 66| Plouzane
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=plouzane&units=imperial
    Processing Record 67| Kahului
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kahului&units=imperial
    Processing Record 68| Lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lompoc&units=imperial
    Processing Record 69| Tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tuatapere&units=imperial
    Processing Record 70| Saint-Georges
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saint-georges&units=imperial
    Processing Record 71| Barguzin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=barguzin&units=imperial
    Processing Record 72| Boca do Acre
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=boca%20do%20acre&units=imperial
    Processing Record 73| Moose Factory
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=moose%20factory&units=imperial
    Processing Record 74| Victoria
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=victoria&units=imperial
    Processing Record 75| Martapura
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=martapura&units=imperial
    Processing Record 76| Tricase
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tricase&units=imperial
    Processing Record 77| Bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bredasdorp&units=imperial
    Processing Record 78| Kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kavieng&units=imperial
    Processing Record 79| Roald
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=roald&units=imperial
    Processing Record 80| Ghanzi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ghanzi&units=imperial
    Processing Record 81| Carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=carnarvon&units=imperial
    city not found ... skipping 
    Processing Record 82| Mount Isa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mount%20isa&units=imperial
    city not found ... skipping 
    Processing Record 83| Alta Floresta
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=alta%20floresta&units=imperial
    Processing Record 84| Kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kapaa&units=imperial
    city not found ... skipping 
    Processing Record 85| Knysna
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=knysna&units=imperial
    Processing Record 86| Saint George
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saint%20george&units=imperial
    city not found ... skipping 
    Processing Record 87| Torbay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=torbay&units=imperial
    Processing Record 88| Conceicao do Araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=conceicao%20do%20araguaia&units=imperial
    Processing Record 89| Merritt Island
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=merritt%20island&units=imperial
    Processing Record 90| Pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pangnirtung&units=imperial
    Processing Record 91| Dakar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dakar&units=imperial
    Processing Record 92| Anloga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=anloga&units=imperial
    Processing Record 93| Khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=khatanga&units=imperial
    city not found ... skipping 
    Processing Record 94| Gat
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=gat&units=imperial
    city not found ... skipping 
    Processing Record 95| Port Alfred
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20alfred&units=imperial
    Processing Record 96| Beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=beringovskiy&units=imperial
    Processing Record 97| Cabo San Lucas
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cabo%20san%20lucas&units=imperial
    Processing Record 98| Morant Bay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=morant%20bay&units=imperial
    Processing Record 99| Manaus
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=manaus&units=imperial
    Processing Record 100| Narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=narsaq&units=imperial
    Processing Record 101| Illintsi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=illintsi&units=imperial
    Processing Record 102| Luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=luderitz&units=imperial
    city not found ... skipping 
    Processing Record 103| Dharchula
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dharchula&units=imperial
    Processing Record 104| Hofn
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hofn&units=imperial
    Processing Record 105| Weyburn
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=weyburn&units=imperial
    Processing Record 106| Lashio
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lashio&units=imperial
    Processing Record 107| Punta Arenas
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=punta%20arenas&units=imperial
    Processing Record 108| Havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=havoysund&units=imperial
    Processing Record 109| Shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=shenjiamen&units=imperial
    Processing Record 110| San Pedro
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20pedro&units=imperial
    Processing Record 111| Vila Franca do Campo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vila%20franca%20do%20campo&units=imperial
    Processing Record 112| Tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tuktoyaktuk&units=imperial
    Processing Record 113| Lichinga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lichinga&units=imperial
    Processing Record 114| Simao
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=simao&units=imperial
    Processing Record 115| Meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=meulaboh&units=imperial
    city not found ... skipping 
    Processing Record 116| Kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kavaratti&units=imperial
    Processing Record 117| Port Elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20elizabeth&units=imperial
    Processing Record 118| Cape Town
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cape%20town&units=imperial
    Processing Record 119| Basco
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=basco&units=imperial
    Processing Record 120| Qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=qaanaaq&units=imperial
    Processing Record 121| Chany
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chany&units=imperial
    Processing Record 122| Maldonado
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=maldonado&units=imperial
    Processing Record 123| Acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=acapulco&units=imperial
    Processing Record 124| Saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saldanha&units=imperial
    Processing Record 125| Luanda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=luanda&units=imperial
    Processing Record 126| Provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=provideniya&units=imperial
    Processing Record 127| Husavik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=husavik&units=imperial
    Processing Record 128| Aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=aklavik&units=imperial
    Processing Record 129| College
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=college&units=imperial
    Processing Record 130| Birjand
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=birjand&units=imperial
    Processing Record 131| Barrow
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=barrow&units=imperial
    Processing Record 132| Etchoropo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=etchoropo&units=imperial
    Processing Record 133| Rawson
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=rawson&units=imperial
    Processing Record 134| Faanui
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=faanui&units=imperial
    Processing Record 135| Bontang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bontang&units=imperial
    Processing Record 136| Lata
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lata&units=imperial
    Processing Record 137| Point Fortin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=point%20fortin&units=imperial
    Processing Record 138| Geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=geraldton&units=imperial
    Processing Record 139| Sohag
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sohag&units=imperial
    Processing Record 140| Hilo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hilo&units=imperial
    Processing Record 141| Rio Grande
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=rio%20grande&units=imperial
    Processing Record 142| Upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=upernavik&units=imperial
    Processing Record 143| Koppal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=koppal&units=imperial
    Processing Record 144| Te Anau
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=te%20anau&units=imperial
    Processing Record 145| Garut
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=garut&units=imperial
    Processing Record 146| Hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hithadhoo&units=imperial
    Processing Record 147| Ibra
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ibra&units=imperial
    Processing Record 148| Lima
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lima&units=imperial
    Processing Record 149| Valparaiso
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=valparaiso&units=imperial
    Processing Record 150| Ithaki
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ithaki&units=imperial
    Processing Record 151| Sioux Lookout
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sioux%20lookout&units=imperial
    Processing Record 152| Risor
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=risor&units=imperial
    Processing Record 153| Hatod
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hatod&units=imperial
    city not found ... skipping 
    Processing Record 154| Aripuana
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=aripuana&units=imperial
    Processing Record 155| Richards Bay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=richards%20bay&units=imperial
    Processing Record 156| Souillac
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=souillac&units=imperial
    Processing Record 157| Samarai
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=samarai&units=imperial
    city not found ... skipping 
    Processing Record 158| Taltal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=taltal&units=imperial
    Processing Record 159| Krasnotorka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=krasnotorka&units=imperial
    Processing Record 160| Ponta do Sol
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ponta%20do%20sol&units=imperial
    Processing Record 161| Poya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=poya&units=imperial
    Processing Record 162| Shanhetun
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=shanhetun&units=imperial
    Processing Record 163| Cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cidreira&units=imperial
    Processing Record 164| Bure
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bure&units=imperial
    Processing Record 165| Broome
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=broome&units=imperial
    Processing Record 166| Tomohon
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tomohon&units=imperial
    city not found ... skipping 
    Processing Record 167| Novyy Urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=novyy%20urengoy&units=imperial
    Processing Record 168| Isangel
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=isangel&units=imperial
    city not found ... skipping 
    Processing Record 169| Port Macquarie
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20macquarie&units=imperial
    Processing Record 170| Beira
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=beira&units=imperial
    Processing Record 171| Selma
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=selma&units=imperial
    Processing Record 172| Kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kaitangata&units=imperial
    Processing Record 173| Abha
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=abha&units=imperial
    Processing Record 174| Katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=katsuura&units=imperial
    city not found ... skipping 
    Processing Record 175| Pontes e Lacerda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pontes%20e%20lacerda&units=imperial
    Processing Record 176| Esperance
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=esperance&units=imperial
    Processing Record 177| Chikwawa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chikwawa&units=imperial
    Processing Record 178| Pevek
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pevek&units=imperial
    Processing Record 179| Tynda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tynda&units=imperial
    city not found ... skipping 
    Processing Record 180| Cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cayenne&units=imperial
    Processing Record 181| San Policarpo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20policarpo&units=imperial
    Processing Record 182| Kupang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kupang&units=imperial
    Processing Record 183| Kosa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kosa&units=imperial
    Processing Record 184| Bodden Town
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bodden%20town&units=imperial
    Processing Record 185| Majene
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=majene&units=imperial
    city not found ... skipping 
    Processing Record 186| Butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=butaritari&units=imperial
    city not found ... skipping 
    Processing Record 187| Mundra
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mundra&units=imperial
    Processing Record 188| Sorland
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sorland&units=imperial
    Processing Record 189| Pouebo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pouebo&units=imperial
    Processing Record 190| Aykhal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=aykhal&units=imperial
    Processing Record 191| Severo-Kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=severo-kurilsk&units=imperial
    city not found ... skipping 
    Processing Record 192| Pokhara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pokhara&units=imperial
    Processing Record 193| Phan Thiet
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=phan%20thiet&units=imperial
    Processing Record 194| Correntina
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=correntina&units=imperial
    Processing Record 195| Baykalsk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=baykalsk&units=imperial
    Processing Record 196| Keuruu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=keuruu&units=imperial
    Processing Record 197| Kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kruisfontein&units=imperial
    Processing Record 198| Vostok
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vostok&units=imperial
    Processing Record 199| Arica
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=arica&units=imperial
    Processing Record 200| Robe
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=robe&units=imperial
    Processing Record 201| Yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yellowknife&units=imperial
    Processing Record 202| Saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saskylakh&units=imperial
    Processing Record 203| Boyolangu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=boyolangu&units=imperial
    Processing Record 204| Las Choapas
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=las%20choapas&units=imperial
    Processing Record 205| Cockburn Town
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cockburn%20town&units=imperial
    Processing Record 206| Mar del Plata
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mar%20del%20plata&units=imperial
    Processing Record 207| Mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mahebourg&units=imperial
    Processing Record 208| Shache
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=shache&units=imperial
    Processing Record 209| Novyy Urgal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=novyy%20urgal&units=imperial
    Processing Record 210| Kieta
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kieta&units=imperial
    Processing Record 211| Navirai
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=navirai&units=imperial
    Processing Record 212| Khani
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=khani&units=imperial
    Processing Record 213| Hami
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hami&units=imperial
    Processing Record 214| Sao Joao da Barra
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sao%20joao%20da%20barra&units=imperial
    Processing Record 215| Mount Gambier
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mount%20gambier&units=imperial
    city not found ... skipping 
    Processing Record 216| Arraial do Cabo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=arraial%20do%20cabo&units=imperial
    Processing Record 217| Jumla
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jumla&units=imperial
    Processing Record 218| Santa Marta
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=santa%20marta&units=imperial
    city not found ... skipping 
    Processing Record 219| Bambous Virieux
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bambous%20virieux&units=imperial
    Processing Record 220| Chirongui
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chirongui&units=imperial
    Processing Record 221| Jalu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jalu&units=imperial
    Processing Record 222| Sarangani
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sarangani&units=imperial
    city not found ... skipping 
    Processing Record 223| The Valley
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=the%20valley&units=imperial
    Processing Record 224| Pochutla
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pochutla&units=imperial
    city not found ... skipping 
    Processing Record 225| Clyde River
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=clyde%20river&units=imperial
    Processing Record 226| East London
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=east%20london&units=imperial
    Processing Record 227| Qaqortoq
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=qaqortoq&units=imperial
    Processing Record 228| Manaure
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=manaure&units=imperial
    city not found ... skipping 
    Processing Record 229| Road Town
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=road%20town&units=imperial
    Processing Record 230| Mkuranga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mkuranga&units=imperial
    Processing Record 231| Adrar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=adrar&units=imperial
    Processing Record 232| Sharjah
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sharjah&units=imperial
    Processing Record 233| Tshikapa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tshikapa&units=imperial
    Processing Record 234| Buchanan
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=buchanan&units=imperial
    city not found ... skipping 
    Processing Record 235| Hit
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hit&units=imperial
    Processing Record 236| Matara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=matara&units=imperial
    Processing Record 237| Hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hualmay&units=imperial
    Processing Record 238| Bijar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bijar&units=imperial
    Processing Record 239| Palivere
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=palivere&units=imperial
    Processing Record 240| Zelenogorskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=zelenogorskiy&units=imperial
    Processing Record 241| Bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bonthe&units=imperial
    Processing Record 242| Manggar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=manggar&units=imperial
    Processing Record 243| Dembi Dolo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dembi%20dolo&units=imperial
    Processing Record 244| Zhangjiakou
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=zhangjiakou&units=imperial
    city not found ... skipping 
    Processing Record 245| Jawhar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jawhar&units=imperial
    Processing Record 246| Comodoro Rivadavia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=comodoro%20rivadavia&units=imperial
    Processing Record 247| Vidim
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vidim&units=imperial
    Processing Record 248| Khuzhir
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=khuzhir&units=imperial
    Processing Record 249| Sassandra
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sassandra&units=imperial
    Processing Record 250| Muzhi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=muzhi&units=imperial
    city not found ... skipping 
    Processing Record 251| Puerto Ayora
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=puerto%20ayora&units=imperial
    Processing Record 252| Tsuchiura
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tsuchiura&units=imperial
    city not found ... skipping 
    Processing Record 253| Yining
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yining&units=imperial
    Processing Record 254| Rimbey
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=rimbey&units=imperial
    Processing Record 255| Leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=leshukonskoye&units=imperial
    Processing Record 256| Albury
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=albury&units=imperial
    Processing Record 257| Srandakan
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=srandakan&units=imperial
    Processing Record 258| Kjollefjord
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kjollefjord&units=imperial
    Processing Record 259| Dalby
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dalby&units=imperial
    Processing Record 260| Garhakota
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=garhakota&units=imperial
    city not found ... skipping 
    Processing Record 261| Gbadolite
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=gbadolite&units=imperial
    Processing Record 262| Alofi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=alofi&units=imperial
    Processing Record 263| Bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bandarbeyla&units=imperial
    Processing Record 264| Vite
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vite&units=imperial
    city not found ... skipping 
    Processing Record 265| Saint-Pierre
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saint-pierre&units=imperial
    Processing Record 266| Dong Hoi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dong%20hoi&units=imperial
    Processing Record 267| Lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lagoa&units=imperial
    Processing Record 268| Suez
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=suez&units=imperial
    Processing Record 269| Kaberamaido
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kaberamaido&units=imperial
    Processing Record 270| Nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nanortalik&units=imperial
    Processing Record 271| Louga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=louga&units=imperial
    Processing Record 272| Kisangani
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kisangani&units=imperial
    Processing Record 273| Ixtapa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ixtapa&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 274| Santa Catalina
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=santa%20catalina&units=imperial
    Processing Record 275| Roquetas de Mar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=roquetas%20de%20mar&units=imperial
    Processing Record 276| Maple Creek
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=maple%20creek&units=imperial
    Processing Record 277| Erdenet
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=erdenet&units=imperial
    Processing Record 278| Salinopolis
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=salinopolis&units=imperial
    Processing Record 279| Roros
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=roros&units=imperial
    Processing Record 280| Borovskoy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=borovskoy&units=imperial
    Processing Record 281| Moron
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=moron&units=imperial
    Processing Record 282| Tete
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tete&units=imperial
    Processing Record 283| Antonina
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=antonina&units=imperial
    Processing Record 284| San Patricio
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20patricio&units=imperial
    Processing Record 285| Mackay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mackay&units=imperial
    Processing Record 286| Marystown
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=marystown&units=imperial
    Processing Record 287| Ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ahipara&units=imperial
    Processing Record 288| Luneville
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=luneville&units=imperial
    Processing Record 289| Kyaikkami
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kyaikkami&units=imperial
    city not found ... skipping 
    Processing Record 290| Monte Alegre
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=monte%20alegre&units=imperial
    city not found ... skipping 
    Processing Record 291| Nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nouadhibou&units=imperial
    Processing Record 292| Lurgan
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lurgan&units=imperial
    Processing Record 293| Nanning
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nanning&units=imperial
    city not found ... skipping 
    Processing Record 294| Kasane
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kasane&units=imperial
    Processing Record 295| Cabedelo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cabedelo&units=imperial
    city not found ... skipping 
    Processing Record 296| Leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=leningradskiy&units=imperial
    Processing Record 297| Sampit
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sampit&units=imperial
    Processing Record 298| Gizo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=gizo&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 299| Sawakin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sawakin&units=imperial
    Processing Record 300| San Cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20cristobal&units=imperial
    Processing Record 301| Dombarovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dombarovskiy&units=imperial
    Processing Record 302| Fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=fortuna&units=imperial
    Processing Record 303| Chapais
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chapais&units=imperial
    Processing Record 304| Zhengjiatun
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=zhengjiatun&units=imperial
    Processing Record 305| Lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lavrentiya&units=imperial
    Processing Record 306| Petropavlovsk-Kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=petropavlovsk-kamchatskiy&units=imperial
    Processing Record 307| Victoria Point
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=victoria%20point&units=imperial
    Processing Record 308| Karratha
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=karratha&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 309| Awbari
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=awbari&units=imperial
    Processing Record 310| Sidi Bin Nur
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sidi%20bin%20nur&units=imperial
    Processing Record 311| Quatre Cocos
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=quatre%20cocos&units=imperial
    Processing Record 312| Vagur
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vagur&units=imperial
    Processing Record 313| Dmytrivka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dmytrivka&units=imperial
    Processing Record 314| Tura
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tura&units=imperial
    city not found ... skipping 
    Processing Record 315| Melfort
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=melfort&units=imperial
    Processing Record 316| Sitka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sitka&units=imperial
    Processing Record 317| Airai
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=airai&units=imperial
    Processing Record 318| Kotelnich
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kotelnich&units=imperial
    city not found ... skipping 
    Processing Record 319| Los Llanos de Aridane
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=los%20llanos%20de%20aridane&units=imperial
    city not found ... skipping 
    Processing Record 320| Palembang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=palembang&units=imperial
    Processing Record 321| Yulara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yulara&units=imperial
    Processing Record 322| Esso
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=esso&units=imperial
    Processing Record 323| Svetlaya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=svetlaya&units=imperial
    Processing Record 324| Kuala Kedah
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kuala%20kedah&units=imperial
    Processing Record 325| Awjilah
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=awjilah&units=imperial
    Processing Record 326| Wasilla
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=wasilla&units=imperial
    Processing Record 327| Lillesand
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lillesand&units=imperial
    Processing Record 328| Yarim
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yarim&units=imperial
    Processing Record 329| Kapit
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kapit&units=imperial
    Processing Record 330| Viljandi
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=viljandi&units=imperial
    Processing Record 331| Araguaina
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=araguaina&units=imperial
    Processing Record 332| Areia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=areia&units=imperial
    Processing Record 333| Maningrida
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=maningrida&units=imperial
    Processing Record 334| Tiffin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tiffin&units=imperial
    Processing Record 335| Payson
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=payson&units=imperial
    Processing Record 336| Preobrazheniye
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=preobrazheniye&units=imperial
    city not found ... skipping 
    Processing Record 337| Kidal
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kidal&units=imperial
    Processing Record 338| Borogontsy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=borogontsy&units=imperial
    Processing Record 339| Tenenkou
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tenenkou&units=imperial
    Processing Record 340| Sunyani
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sunyani&units=imperial
    Processing Record 341| Tautira
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tautira&units=imperial
    Processing Record 342| Port Blair
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20blair&units=imperial
    Processing Record 343| Staryy Nadym
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=staryy%20nadym&units=imperial
    Processing Record 344| Taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=taoudenni&units=imperial
    Processing Record 345| Jinzhou
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jinzhou&units=imperial
    Processing Record 346| Yondo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yondo&units=imperial
    Processing Record 347| Timra
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=timra&units=imperial
    city not found ... skipping 
    Processing Record 348| Ginda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ginda&units=imperial
    Processing Record 349| Nguruka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nguruka&units=imperial
    Processing Record 350| Panguna
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=panguna&units=imperial
    Processing Record 351| Vanavara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vanavara&units=imperial
    Processing Record 352| Vardo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vardo&units=imperial
    Processing Record 353| Haapiti
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=haapiti&units=imperial
    Processing Record 354| Mareeba
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mareeba&units=imperial
    Processing Record 355| Tanout
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tanout&units=imperial
    Processing Record 356| Shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=shimoda&units=imperial
    Processing Record 357| Myrtle Beach
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=myrtle%20beach&units=imperial
    Processing Record 358| Mbandaka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mbandaka&units=imperial
    Processing Record 359| La Ronge
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=la%20ronge&units=imperial
    Processing Record 360| Svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=svetlogorsk&units=imperial
    Processing Record 361| Grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=grindavik&units=imperial
    Processing Record 362| Margate
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=margate&units=imperial
    Processing Record 363| Pathein
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pathein&units=imperial
    city not found ... skipping 
    Processing Record 364| Mitu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mitu&units=imperial
    Processing Record 365| North Platte
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=north%20platte&units=imperial
    Processing Record 366| Ancud
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ancud&units=imperial
    Processing Record 367| Fare
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=fare&units=imperial
    Processing Record 368| Corrente
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=corrente&units=imperial
    Processing Record 369| Riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=riyadh&units=imperial
    Processing Record 370| Sibolga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sibolga&units=imperial
    Processing Record 371| Port Hedland
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=port%20hedland&units=imperial
    Processing Record 372| Ivanivka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ivanivka&units=imperial
    city not found ... skipping 
    Processing Record 373| Bima
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bima&units=imperial
    Processing Record 374| Marrakesh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=marrakesh&units=imperial
    Processing Record 375| Ishigaki
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ishigaki&units=imperial
    Processing Record 376| Tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tessalit&units=imperial
    Processing Record 377| Temozon
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=temozon&units=imperial
    Processing Record 378| Kyzyl-Mazhalyk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kyzyl-mazhalyk&units=imperial
    Processing Record 379| Pilar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pilar&units=imperial
    Processing Record 380| Opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=opuwo&units=imperial
    Processing Record 381| Samoded
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=samoded&units=imperial
    Processing Record 382| Middelburg
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=middelburg&units=imperial
    Processing Record 383| Wanning
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=wanning&units=imperial
    Processing Record 384| Avera
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=avera&units=imperial
    Processing Record 385| Vicosa do Ceara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vicosa%20do%20ceara&units=imperial
    city not found ... skipping 
    Processing Record 386| Perevoz
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=perevoz&units=imperial
    city not found ... skipping 
    Processing Record 387| Shubarkuduk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=shubarkuduk&units=imperial
    Processing Record 388| Sabang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sabang&units=imperial
    Processing Record 389| Termoli
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=termoli&units=imperial
    Processing Record 390| Kutum
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kutum&units=imperial
    Processing Record 391| Fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=fairbanks&units=imperial
    Processing Record 392| Arman
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=arman&units=imperial
    Processing Record 393| Athabasca
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=athabasca&units=imperial
    Processing Record 394| Hirara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hirara&units=imperial
    Processing Record 395| Lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lincoln&units=imperial
    Processing Record 396| Manakara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=manakara&units=imperial
    Processing Record 397| Ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ilulissat&units=imperial
    Processing Record 398| Kawalu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kawalu&units=imperial
    Processing Record 399| Roseburg
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=roseburg&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 400| Namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=namatanai&units=imperial
    Processing Record 401| Seoul
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=seoul&units=imperial
    Processing Record 402| Rafaela
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=rafaela&units=imperial
    Processing Record 403| Santa Isabel
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=santa%20isabel&units=imperial
    Processing Record 404| Zhireken
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=zhireken&units=imperial
    Processing Record 405| Yatou
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yatou&units=imperial
    Processing Record 406| Faya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=faya&units=imperial
    Processing Record 407| Maun
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=maun&units=imperial
    Processing Record 408| Casian
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=casian&units=imperial
    Processing Record 409| Aksu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=aksu&units=imperial
    Processing Record 410| Azangaro
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=azangaro&units=imperial
    Processing Record 411| Kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kurilsk&units=imperial
    Processing Record 412| Dunda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dunda&units=imperial
    Processing Record 413| Paramonga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=paramonga&units=imperial
    Processing Record 414| Mudgee
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mudgee&units=imperial
    Processing Record 415| Cervo
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=cervo&units=imperial
    Processing Record 416| China
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=china&units=imperial
    Processing Record 417| Macenta
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=macenta&units=imperial
    Processing Record 418| Bela Vista do Paraiso
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bela%20vista%20do%20paraiso&units=imperial
    Processing Record 419| Puerto Colombia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=puerto%20colombia&units=imperial
    Processing Record 420| Ormara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ormara&units=imperial
    city not found ... skipping 
    city not found ... skipping 
    Processing Record 421| Elko
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=elko&units=imperial
    city not found ... skipping 
    Processing Record 422| Sabirabad
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sabirabad&units=imperial
    Processing Record 423| Caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=caravelas&units=imperial
    Processing Record 424| Vila
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vila&units=imperial
    Processing Record 425| Grand Gaube
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=grand%20gaube&units=imperial
    Processing Record 426| Ngunguru
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ngunguru&units=imperial
    Processing Record 427| Hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hamilton&units=imperial
    Processing Record 428| Labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=labuhan&units=imperial
    Processing Record 429| Iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=iqaluit&units=imperial
    Processing Record 430| Anlu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=anlu&units=imperial
    Processing Record 431| Nizwa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nizwa&units=imperial
    Processing Record 432| Dingle
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dingle&units=imperial
    Processing Record 433| Mayumba
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mayumba&units=imperial
    Processing Record 434| Ayame
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ayame&units=imperial
    city not found ... skipping 
    Processing Record 435| Dresden
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=dresden&units=imperial
    Processing Record 436| Juba
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=juba&units=imperial
    Processing Record 437| West Wendover
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=west%20wendover&units=imperial
    Processing Record 438| Lasa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lasa&units=imperial
    city not found ... skipping 
    Processing Record 439| Abu Samrah
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=abu%20samrah&units=imperial
    Processing Record 440| Linxia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=linxia&units=imperial
    city not found ... skipping 
    Processing Record 441| Itarema
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=itarema&units=imperial
    Processing Record 442| Saint Marys
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=saint%20marys&units=imperial
    Processing Record 443| Udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=udachnyy&units=imperial
    Processing Record 444| Koutsouras
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=koutsouras&units=imperial
    Processing Record 445| Minbu
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=minbu&units=imperial
    Processing Record 446| Alvor
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=alvor&units=imperial
    Processing Record 447| Marsh Harbour
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=marsh%20harbour&units=imperial
    city not found ... skipping 
    Processing Record 448| Kyren
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kyren&units=imperial
    Processing Record 449| Metkovic
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=metkovic&units=imperial
    Processing Record 450| Kemptville
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kemptville&units=imperial
    Processing Record 451| Emerald
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=emerald&units=imperial
    Processing Record 452| Omboue
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=omboue&units=imperial
    Processing Record 453| Lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lorengau&units=imperial
    Processing Record 454| Hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=hambantota&units=imperial
    Processing Record 455| San Rafael
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=san%20rafael&units=imperial
    Processing Record 456| Kisesa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kisesa&units=imperial
    Processing Record 457| Westport
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=westport&units=imperial
    Processing Record 458| Goderich
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=goderich&units=imperial
    Processing Record 459| Banjarmasin
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=banjarmasin&units=imperial
    city not found ... skipping 
    Processing Record 460| Constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=constitucion&units=imperial
    Processing Record 461| Amga
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=amga&units=imperial
    Processing Record 462| Poum
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=poum&units=imperial
    Processing Record 463| Elk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=elk&units=imperial
    Processing Record 464| Kirakira
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kirakira&units=imperial
    Processing Record 465| Ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ostrovnoy&units=imperial
    Processing Record 466| Atar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=atar&units=imperial
    Processing Record 467| Atambua
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=atambua&units=imperial
    Processing Record 468| Iralaya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=iralaya&units=imperial
    Processing Record 469| Encarnacion
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=encarnacion&units=imperial
    city not found ... skipping 
    Processing Record 470| Khrebtovaya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=khrebtovaya&units=imperial
    Processing Record 471| Krasnaya Gora
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=krasnaya%20gora&units=imperial
    city not found ... skipping 
    Processing Record 472| Yar-Sale
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yar-sale&units=imperial
    Processing Record 473| Klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=klaksvik&units=imperial
    Processing Record 474| Gladstone
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=gladstone&units=imperial
    Processing Record 475| Ouadda
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ouadda&units=imperial
    Processing Record 476| Bridlington
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bridlington&units=imperial
    Processing Record 477| Kibala
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kibala&units=imperial
    Processing Record 478| Mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=mehamn&units=imperial
    Processing Record 479| Kaabong
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kaabong&units=imperial
    Processing Record 480| Ozernovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ozernovskiy&units=imperial
    Processing Record 481| Kholmogory
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kholmogory&units=imperial
    Processing Record 482| Zeya
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=zeya&units=imperial
    Processing Record 483| Fraserburgh
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=fraserburgh&units=imperial
    Processing Record 484| Yerky
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=yerky&units=imperial
    Processing Record 485| Buala
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=buala&units=imperial
    Processing Record 486| Half Moon Bay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=half%20moon%20bay&units=imperial
    Processing Record 487| Thompson
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=thompson&units=imperial
    Processing Record 488| Vilyuysk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vilyuysk&units=imperial
    Processing Record 489| Sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sisimiut&units=imperial
    Processing Record 490| Sehithwa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sehithwa&units=imperial
    Processing Record 491| Tomaszow Lubelski
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tomaszow%20lubelski&units=imperial
    city not found ... skipping 
    Processing Record 492| Flinders
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=flinders&units=imperial
    Processing Record 493| Progreso
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=progreso&units=imperial
    Processing Record 494| Itaituba
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=itaituba&units=imperial
    Processing Record 495| Nome
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=nome&units=imperial
    Processing Record 496| Madinat Sittah Uktubar
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=madinat%20sittah%20uktubar&units=imperial
    Processing Record 497| Jacarei
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jacarei&units=imperial
    Processing Record 498| Weligama
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=weligama&units=imperial
    Processing Record 499| Kirkwall
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kirkwall&units=imperial
    Processing Record 500| Jagdalpur
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jagdalpur&units=imperial
    Processing Record 501| Kuryk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kuryk&units=imperial
    Processing Record 502| Chulym
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=chulym&units=imperial
    Processing Record 503| Pacific Grove
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=pacific%20grove&units=imperial
    Processing Record 504| Monrovia
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=monrovia&units=imperial
    Processing Record 505| Masterton
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=masterton&units=imperial
    Processing Record 506| Alice Springs
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=alice%20springs&units=imperial
    Processing Record 507| Bilma
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=bilma&units=imperial
    Processing Record 508| Galesburg
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=galesburg&units=imperial
    Processing Record 509| Lapeer
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=lapeer&units=imperial
    Processing Record 510| Artur Nogueira
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=artur%20nogueira&units=imperial
    Processing Record 511| Ankpa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=ankpa&units=imperial
    Processing Record 512| Kwinana
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kwinana&units=imperial
    Processing Record 513| Vertientes
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=vertientes&units=imperial
    Processing Record 514| Gushikawa
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=gushikawa&units=imperial
    Processing Record 515| Codrington
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=codrington&units=imperial
    Processing Record 516| Jasper
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jasper&units=imperial
    Processing Record 517| Stabat
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=stabat&units=imperial
    Processing Record 518| Roanoke Rapids
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=roanoke%20rapids&units=imperial
    Processing Record 519| Noumea
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=noumea&units=imperial
    Processing Record 520| Tsabong
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tsabong&units=imperial
    Processing Record 521| Norman Wells
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=norman%20wells&units=imperial
    city not found ... skipping 
    Processing Record 522| Necochea
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=necochea&units=imperial
    city not found ... skipping 
    Processing Record 523| Sturgeon Falls
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sturgeon%20falls&units=imperial
    Processing Record 524| Padang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=padang&units=imperial
    Processing Record 525| Sabha
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=sabha&units=imperial
    Processing Record 526| Kachug
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=kachug&units=imperial
    Processing Record 527| Takaka
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=takaka&units=imperial
    Processing Record 528| Tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=tilichiki&units=imperial
    Processing Record 529| Fort-Shevchenko
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=fort-shevchenko&units=imperial
    city not found ... skipping 
    Processing Record 530| Price
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=price&units=imperial
    Processing Record 531| Jinchang
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=jinchang&units=imperial
    Processing Record 532| Talara
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=talara&units=imperial
    Processing Record 533| Eyl
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=eyl&units=imperial
    Processing Record 534| Havelock
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=havelock&units=imperial
    Processing Record 535| Walvis Bay
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=walvis%20bay&units=imperial
    Processing Record 536| Verkhoyansk
    http://api.openweathermap.org/data/2.5/weather?appid=cd80ea31f0dcfaa9dffb1cd446d9a99b&q=verkhoyansk&units=imperial
    ---Data Retrieval Complete ---


# Convert Raw Data to DataFrame
* Export the city data into a .csv.
* Display the DataFrame


```python
weatherpy_dict = {
    "City": city_name,
    "Cloudiness":cloudiness, 
    "Country":country,
    "Date":date, 
    "Humidity": humidity,
    "Lat":lat, 
    "Lng":lng, 
    "Max Temp": max_temp,
    "Wind Speed":wind_speed
}

# Create a data frame from dictionary
weather_data = pd.DataFrame(weatherpy_dict)

# Display count of weather data values 
weather_data.count()
```




    City          536
    Cloudiness    536
    Country       536
    Date          536
    Humidity      536
    Lat           536
    Lng           536
    Max Temp      536
    Wind Speed    536
    dtype: int64




```python
weather_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kudahuvadhoo</td>
      <td>100</td>
      <td>MV</td>
      <td>1563856116</td>
      <td>73</td>
      <td>2.67</td>
      <td>72.89</td>
      <td>83.92</td>
      <td>11.95</td>
    </tr>
    <tr>
      <th>1</th>
      <td>San Quintin</td>
      <td>0</td>
      <td>PH</td>
      <td>1563855741</td>
      <td>67</td>
      <td>17.54</td>
      <td>120.52</td>
      <td>86.08</td>
      <td>4.90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rikitea</td>
      <td>33</td>
      <td>PF</td>
      <td>1563855704</td>
      <td>71</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>68.80</td>
      <td>19.93</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ushuaia</td>
      <td>75</td>
      <td>AR</td>
      <td>1563855473</td>
      <td>97</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>32.00</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sinnamary</td>
      <td>69</td>
      <td>GF</td>
      <td>1563855888</td>
      <td>77</td>
      <td>5.38</td>
      <td>-52.96</td>
      <td>80.32</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Cherskiy</td>
      <td>100</td>
      <td>RU</td>
      <td>1563856121</td>
      <td>95</td>
      <td>68.75</td>
      <td>161.30</td>
      <td>37.30</td>
      <td>10.09</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Honiara</td>
      <td>40</td>
      <td>SB</td>
      <td>1563855795</td>
      <td>62</td>
      <td>-9.43</td>
      <td>159.96</td>
      <td>89.60</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Castro</td>
      <td>40</td>
      <td>CL</td>
      <td>1563856123</td>
      <td>100</td>
      <td>-42.48</td>
      <td>-73.76</td>
      <td>33.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Hobart</td>
      <td>75</td>
      <td>AU</td>
      <td>1563855572</td>
      <td>54</td>
      <td>-42.88</td>
      <td>147.33</td>
      <td>53.60</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Port Lincoln</td>
      <td>51</td>
      <td>AU</td>
      <td>1563855997</td>
      <td>74</td>
      <td>-34.72</td>
      <td>135.86</td>
      <td>59.08</td>
      <td>37.07</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Bethel</td>
      <td>90</td>
      <td>US</td>
      <td>1563855702</td>
      <td>64</td>
      <td>60.79</td>
      <td>-161.76</td>
      <td>69.80</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Soledade</td>
      <td>75</td>
      <td>BR</td>
      <td>1563856128</td>
      <td>100</td>
      <td>-7.06</td>
      <td>-36.37</td>
      <td>68.00</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>12</th>
      <td>New Norfolk</td>
      <td>75</td>
      <td>AU</td>
      <td>1563855691</td>
      <td>54</td>
      <td>-42.78</td>
      <td>147.06</td>
      <td>53.60</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Mataura</td>
      <td>41</td>
      <td>NZ</td>
      <td>1563856130</td>
      <td>93</td>
      <td>-46.19</td>
      <td>168.86</td>
      <td>46.99</td>
      <td>1.99</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Georgetown</td>
      <td>20</td>
      <td>GY</td>
      <td>1563855662</td>
      <td>100</td>
      <td>6.80</td>
      <td>-58.16</td>
      <td>75.20</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Champerico</td>
      <td>40</td>
      <td>MX</td>
      <td>1563856133</td>
      <td>88</td>
      <td>16.38</td>
      <td>-93.60</td>
      <td>73.40</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Naze</td>
      <td>92</td>
      <td>NG</td>
      <td>1563856134</td>
      <td>96</td>
      <td>5.43</td>
      <td>7.07</td>
      <td>72.94</td>
      <td>3.89</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Hermanus</td>
      <td>96</td>
      <td>ZA</td>
      <td>1563855752</td>
      <td>92</td>
      <td>-34.42</td>
      <td>19.24</td>
      <td>55.00</td>
      <td>18.01</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Atuona</td>
      <td>2</td>
      <td>PF</td>
      <td>1563856136</td>
      <td>75</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>79.60</td>
      <td>14.05</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Busselton</td>
      <td>100</td>
      <td>AU</td>
      <td>1563855691</td>
      <td>84</td>
      <td>-33.64</td>
      <td>115.35</td>
      <td>60.01</td>
      <td>8.72</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Cap Malheureux</td>
      <td>75</td>
      <td>MU</td>
      <td>1563855777</td>
      <td>68</td>
      <td>-19.98</td>
      <td>57.61</td>
      <td>71.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Puerto Baquerizo Moreno</td>
      <td>100</td>
      <td>EC</td>
      <td>1563856140</td>
      <td>85</td>
      <td>-0.90</td>
      <td>-89.60</td>
      <td>69.52</td>
      <td>11.18</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Asyut</td>
      <td>0</td>
      <td>EG</td>
      <td>1563856141</td>
      <td>42</td>
      <td>27.18</td>
      <td>31.19</td>
      <td>80.60</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Dikson</td>
      <td>100</td>
      <td>RU</td>
      <td>1563855700</td>
      <td>91</td>
      <td>73.51</td>
      <td>80.55</td>
      <td>34.96</td>
      <td>8.32</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Kargil</td>
      <td>0</td>
      <td>PK</td>
      <td>1563856143</td>
      <td>42</td>
      <td>34.56</td>
      <td>76.13</td>
      <td>57.82</td>
      <td>1.19</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Tiksi</td>
      <td>36</td>
      <td>RU</td>
      <td>1563855800</td>
      <td>32</td>
      <td>71.64</td>
      <td>128.87</td>
      <td>71.14</td>
      <td>25.75</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Komsomolskiy</td>
      <td>98</td>
      <td>RU</td>
      <td>1563856146</td>
      <td>61</td>
      <td>67.55</td>
      <td>63.78</td>
      <td>62.68</td>
      <td>6.76</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Saint-Philippe</td>
      <td>75</td>
      <td>CA</td>
      <td>1563855693</td>
      <td>93</td>
      <td>45.36</td>
      <td>-73.48</td>
      <td>68.00</td>
      <td>4.79</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jieshi</td>
      <td>38</td>
      <td>CN</td>
      <td>1563856148</td>
      <td>79</td>
      <td>22.82</td>
      <td>115.83</td>
      <td>86.00</td>
      <td>1.01</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Nikolskoye</td>
      <td>0</td>
      <td>RU</td>
      <td>1563855708</td>
      <td>82</td>
      <td>59.70</td>
      <td>30.79</td>
      <td>57.20</td>
      <td>3.56</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>506</th>
      <td>Bilma</td>
      <td>9</td>
      <td>NE</td>
      <td>1563856734</td>
      <td>36</td>
      <td>18.69</td>
      <td>12.92</td>
      <td>88.49</td>
      <td>4.61</td>
    </tr>
    <tr>
      <th>507</th>
      <td>Galesburg</td>
      <td>1</td>
      <td>US</td>
      <td>1563856736</td>
      <td>87</td>
      <td>40.95</td>
      <td>-90.37</td>
      <td>68.00</td>
      <td>7.02</td>
    </tr>
    <tr>
      <th>508</th>
      <td>Lapeer</td>
      <td>1</td>
      <td>US</td>
      <td>1563856737</td>
      <td>87</td>
      <td>43.05</td>
      <td>-83.32</td>
      <td>62.60</td>
      <td>6.20</td>
    </tr>
    <tr>
      <th>509</th>
      <td>Artur Nogueira</td>
      <td>0</td>
      <td>BR</td>
      <td>1563856738</td>
      <td>77</td>
      <td>-22.57</td>
      <td>-47.17</td>
      <td>60.80</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>510</th>
      <td>Ankpa</td>
      <td>97</td>
      <td>NG</td>
      <td>1563856739</td>
      <td>98</td>
      <td>7.37</td>
      <td>7.63</td>
      <td>69.77</td>
      <td>5.73</td>
    </tr>
    <tr>
      <th>511</th>
      <td>Kwinana</td>
      <td>75</td>
      <td>AU</td>
      <td>1563856740</td>
      <td>67</td>
      <td>-32.25</td>
      <td>115.77</td>
      <td>66.99</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>512</th>
      <td>Vertientes</td>
      <td>40</td>
      <td>CU</td>
      <td>1563856742</td>
      <td>94</td>
      <td>21.26</td>
      <td>-78.15</td>
      <td>77.00</td>
      <td>1.12</td>
    </tr>
    <tr>
      <th>513</th>
      <td>Gushikawa</td>
      <td>75</td>
      <td>JP</td>
      <td>1563856743</td>
      <td>70</td>
      <td>26.35</td>
      <td>127.87</td>
      <td>87.80</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>514</th>
      <td>Codrington</td>
      <td>0</td>
      <td>AU</td>
      <td>1563856744</td>
      <td>36</td>
      <td>-28.95</td>
      <td>153.24</td>
      <td>78.80</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>515</th>
      <td>Jasper</td>
      <td>75</td>
      <td>US</td>
      <td>1563856482</td>
      <td>100</td>
      <td>33.83</td>
      <td>-87.28</td>
      <td>75.99</td>
      <td>1.43</td>
    </tr>
    <tr>
      <th>516</th>
      <td>Stabat</td>
      <td>99</td>
      <td>ID</td>
      <td>1563856747</td>
      <td>84</td>
      <td>3.76</td>
      <td>98.46</td>
      <td>77.33</td>
      <td>1.72</td>
    </tr>
    <tr>
      <th>517</th>
      <td>Roanoke Rapids</td>
      <td>75</td>
      <td>US</td>
      <td>1563856748</td>
      <td>100</td>
      <td>36.46</td>
      <td>-77.65</td>
      <td>75.00</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>518</th>
      <td>Noumea</td>
      <td>0</td>
      <td>NC</td>
      <td>1563856749</td>
      <td>56</td>
      <td>-22.28</td>
      <td>166.46</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>519</th>
      <td>Tsabong</td>
      <td>0</td>
      <td>BW</td>
      <td>1563856750</td>
      <td>34</td>
      <td>-26.02</td>
      <td>22.40</td>
      <td>49.79</td>
      <td>14.41</td>
    </tr>
    <tr>
      <th>520</th>
      <td>Norman Wells</td>
      <td>40</td>
      <td>CA</td>
      <td>1563856751</td>
      <td>64</td>
      <td>65.28</td>
      <td>-126.83</td>
      <td>69.80</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>521</th>
      <td>Necochea</td>
      <td>0</td>
      <td>AR</td>
      <td>1563856753</td>
      <td>96</td>
      <td>-38.55</td>
      <td>-58.74</td>
      <td>35.01</td>
      <td>6.42</td>
    </tr>
    <tr>
      <th>522</th>
      <td>Sturgeon Falls</td>
      <td>40</td>
      <td>CA</td>
      <td>1563856736</td>
      <td>87</td>
      <td>46.36</td>
      <td>-79.93</td>
      <td>60.01</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>523</th>
      <td>Padang</td>
      <td>50</td>
      <td>ID</td>
      <td>1563856755</td>
      <td>64</td>
      <td>-0.92</td>
      <td>100.36</td>
      <td>83.09</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>524</th>
      <td>Sabha</td>
      <td>0</td>
      <td>LY</td>
      <td>1563856756</td>
      <td>29</td>
      <td>27.03</td>
      <td>14.43</td>
      <td>76.97</td>
      <td>11.63</td>
    </tr>
    <tr>
      <th>525</th>
      <td>Kachug</td>
      <td>100</td>
      <td>RU</td>
      <td>1563856757</td>
      <td>39</td>
      <td>53.96</td>
      <td>105.89</td>
      <td>79.13</td>
      <td>6.40</td>
    </tr>
    <tr>
      <th>526</th>
      <td>Takaka</td>
      <td>98</td>
      <td>NZ</td>
      <td>1563856759</td>
      <td>74</td>
      <td>-40.86</td>
      <td>172.81</td>
      <td>54.00</td>
      <td>1.01</td>
    </tr>
    <tr>
      <th>527</th>
      <td>Tilichiki</td>
      <td>100</td>
      <td>RU</td>
      <td>1563856760</td>
      <td>72</td>
      <td>60.47</td>
      <td>166.10</td>
      <td>54.29</td>
      <td>6.22</td>
    </tr>
    <tr>
      <th>528</th>
      <td>Fort-Shevchenko</td>
      <td>83</td>
      <td>KZ</td>
      <td>1563856761</td>
      <td>79</td>
      <td>44.51</td>
      <td>50.26</td>
      <td>76.07</td>
      <td>18.25</td>
    </tr>
    <tr>
      <th>529</th>
      <td>Price</td>
      <td>1</td>
      <td>US</td>
      <td>1563856762</td>
      <td>19</td>
      <td>39.60</td>
      <td>-110.81</td>
      <td>80.60</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>530</th>
      <td>Jinchang</td>
      <td>97</td>
      <td>CN</td>
      <td>1563856764</td>
      <td>28</td>
      <td>38.52</td>
      <td>102.19</td>
      <td>72.11</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>531</th>
      <td>Talara</td>
      <td>7</td>
      <td>PE</td>
      <td>1563856765</td>
      <td>88</td>
      <td>-4.58</td>
      <td>-81.27</td>
      <td>64.91</td>
      <td>18.48</td>
    </tr>
    <tr>
      <th>532</th>
      <td>Eyl</td>
      <td>100</td>
      <td>SO</td>
      <td>1563856766</td>
      <td>37</td>
      <td>7.98</td>
      <td>49.82</td>
      <td>83.27</td>
      <td>33.38</td>
    </tr>
    <tr>
      <th>533</th>
      <td>Havelock</td>
      <td>1</td>
      <td>US</td>
      <td>1563856767</td>
      <td>74</td>
      <td>34.88</td>
      <td>-76.90</td>
      <td>82.99</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>534</th>
      <td>Walvis Bay</td>
      <td>10</td>
      <td>NA</td>
      <td>1563856768</td>
      <td>96</td>
      <td>-22.95</td>
      <td>14.51</td>
      <td>49.79</td>
      <td>1.99</td>
    </tr>
    <tr>
      <th>535</th>
      <td>Verkhoyansk</td>
      <td>0</td>
      <td>RU</td>
      <td>1563856770</td>
      <td>26</td>
      <td>67.55</td>
      <td>133.39</td>
      <td>70.31</td>
      <td>8.03</td>
    </tr>
  </tbody>
</table>
<p>536 rows × 9 columns</p>
</div>



### Plotting the Data
* Use proper labeling of the plots using plot titles (including date of analysis) and axes labels.
* Save the plotted figures as .pngs.

#### Latitude vs. Temperature Plot


```python
plt.scatter(weather_data["Lat"], weather_data["Max Temp"], marker="o", s=10)

# Incorporate the other graph properties
plt.title("City Latitude vs. Max Temperature")
plt.ylabel("Max. Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Max_Temp_vs_Latitude.png")


# Show plot
plt.show()
```


![png](output_13_0.png)


#### Latitude vs. Humidity Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Humidity"], marker="o", s=10)

# Incorporate the other graph properties
plt.title("City Latitude vs. Humidity")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Humidity_vs_Latitude.png")
# Show plot
plt.show()
```


![png](output_15_0.png)


#### Latitude vs. Cloudiness Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Cloudiness"], marker="o", s=10)

# Incorporate the other graph properties
plt.title("City Latitude vs. Cloudiness")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Cloudiness_vs_Latitude.png")


# Show plot
plt.show()
```


![png](output_17_0.png)


#### Latitude vs. Wind Speed Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Lat"], weather_data["Wind Speed"], marker="o", s=10)

# Incorporate the other graph properties
plt.title("City Latitude vs. Wind Speed")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("Wind_Speed_vs_Latitude.png")

# Show plot
plt.show()
```


![png](output_19_0.png)



```python
  
```
