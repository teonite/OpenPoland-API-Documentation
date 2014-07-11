OpenPoland REST API
====================

Welcome to OpenPoland REST API documentation. Here you will find all informations you need for developmetn of systems and applications with this API.

We hope that this document is helpful for you, but if you have any questions or you've found mistake or inconsistency, please feel free to contact us at 

At this moment (beta release) API exposes methods for:

* listing assets
* searching
* metadata retrieval
* data retrieval


REST API tokens:
================

How to get a token
-------------------

Once you register at OpenPoland.net you can find your token in settings or at search page in example query.

REST API : search for assets
============================

How to find specyfic assets
---------------------------
Allowed method: GET

Context: /api/list

Query params:
	
	* q (string) : search phrase (optional)
	* page (integer) : page of results (optional)

Request search results:

::

    curl -i -XGET https://openpoland.net/api/list?q=ludność" -H "Authorization: Token __YOUR_TOKEN_HERE__"

Response (HTTP/1.0 200 OK):

::

	{
	   "has_next" : false,
	   "page" : "1",
	   "results" :
	      [
	         {
	            "id" : 1342,
	            "title" : "Ludność w wieku przedprodukcyjnym (17 lat i mniej), produkcyjnym i poprodukcyjnym wg płci (NTS-5, 1995-2012)"
	         },
	         {
	            "id" : 2137,
	            "title" : "Ludność wg grup wieku i płci (NTS-5, 1995-2012)"
	         },
	         {
	            "id" : 1336,
	            "title" : "Ludność wg miejsca zameldowania/zamieszkania i płci (NTS-5, 1995-2012)"
	         },
	         {
	            "id" : 2577,
	            "title" : "Ludność w wieku przedprodukcyjnym (14 lat i mniej), produkcyjnym i poprodukcyjnym wg płci (NTS-5, 1995-2012)"
	         },
	         {
	            "id" : 3361,
	            "title" : "Ludność w wieku przedprodukcyjnym (17 lat i mniej), produkcyjnym i poprodukcyjnym wg miejsca zamieszkania (NTS-5, 2012)"
	         },

	         ...

	      ]
	}

REST API : get asset metadata
==============================

For example if you want to find more about asset "Ludność zamieszkała w gospodarstwie domowym wg wieku (NTS-5, 1996)" with id **1891**, which you found throu search query, you will use metadata method to get more specyfic informations.

How to find specyfic assets
---------------------------
Allowed method: GET

Context: /api/assset/[asset_id]/meta

Parameters:
	
	asset_id : string

Example request

::

    curl -i -XGET -H "Authorization: Token __YOUR_TOKEN_HERE__" 'https://openpoland.net/api/asset/1891/meta/'
Response (HTTP/1.0 200 OK):

::

	{
	   "dims" :
	      [
	         {
	            "values" :
	               [
	                  {
	                     "id" : 3703,
	                     "name" : "z użytkownikiem gospodarstwa rolnego ponad 1 ha"
	                  },
	                  {
	                     "id" : 3704,
	                     "name" : "z użytkownikiem działki do 1 ha"
	                  }
	               ],
	            "name" : "Gospodarstwa"
	         },
	         {
	            "values" :
	               [
	                  {
	                     "id" : 3700,
	                     "name" : "ogółem"
	                  },
	                  {
	                     "id" : 3701,
	                     "name" : "poniżej 15 lat (urodzeni: 13.06.1981r.-12.06.1996r.)"
	                  },
	                  {
	                     "id" : 3702,
	                     "name" : "15 lat i więcej (urodzeni przed 13.06.1981r.)"
	                  }
	               ],
	            "name" : "Wiek"
	         },
	         {
	            "values" :
	               [
	                  {
	                     "id" : 528,
	                     "name" : "ogółem"
	                  },
	                  {
	                     "id" : 529,
	                     "name" : "mężczyźni"
	                  },
	                  {
	                     "id" : 530,
	                     "name" : "kobiety"
	                  }
	               ],
	            "name" : "Płeć"
	         }
	      ],
	   "years" :
	      [
	         1996
	      ],
	   "subKey" : 1891,
	   "title" : "Ludność zamieszkała w gospodarstwie domowym wg wieku (NTS-5, 1996)"
	}


REST API : get asset data
=========================

Limits and paging:

	The volume of some assets data may be as large as 400MB or even larger. Thus API limits the size of returned data for each query and data is paged, you can request next page of results using query parameter. If you need higher data limits, please contact us at contact@openpoland.net.


How to get **all** data for selected asset
------------------------------------------
Allowed method: GET

Context: /api/assset/[asset_id]/data

Parameters: 

	* asset_id : string

Response structure:
	Record of 'results' array has the following structure:
	
	* first field is always NTS id of territorial unit
	* second field is always common name of territorial unit
	* then N (from 1 to 5) fields follow containing values for dimensions specified by metadata
	* last four fields are always (in order) year, measure unit, value, data attribute 

Example request

::

    curl -i 'https://openpoland.net/api/asset/1891/data/' -XGET -H "Authorization: Token __YOUR_TOKEN_HERE__"
Response (HTTP/1.0 200 OK):

::

	{
	   "asset_id" : "1891",
	   "has_next" : true,
	   "page" : 0,
	   "results" :
	      [
	         [
	            "1101506022",
	            "Andrespol (2)",
	            "z użytkownikiem gospodarstwa rolnego ponad 1 ha",
	            "ogółem",
	            "ogółem",
	            "1996",
	            "osoba",
	            802.0,
	            "B"
	         ],
	         [
	            "1101506022",
	            "Andrespol (2)",
	            "z użytkownikiem gospodarstwa rolnego ponad 1 ha",
	            "ogółem",
	            "mężczyźni",
	            "1996",
	            "osoba",
	            386.0,
	            "B"
	         ],
	         [
	            "1101506022",
	            "Andrespol (2)",
	            "z użytkownikiem gospodarstwa rolnego ponad 1 ha",
	            "ogółem",
	            "kobiety",
	            "1996",
	            "osoba",
	            416.0,
	            "B"
	         ],
	         [
	            "1101506022",
	            "Andrespol (2)",
	            "z użytkownikiem gospodarstwa rolnego ponad 1 ha",
	            "poniżej 15 lat (urodzeni: 13.06.1981r.-12.06.1996r.)",
	            "ogółem",
	            "1996",
	            "osoba",
	            153.0,
	            "B"
	         ],
	         [
	            "1101506022",
	            "Andrespol (2)",
	            "z użytkownikiem gospodarstwa rolnego ponad 1 ha",
	            "poniżej 15 lat (urodzeni: 13.06.1981r.-12.06.1996r.)",
	            "mężczyźni",
	            "1996",
	            "osoba",
	            72.0,
	            "B"
	         ]
	    ]
    }

How to get **filtered** data for selected asset
-----------------------------------------------

Allowed method: POST

Context: /api/assset/[asset_id]/data_filter

Data format (json): 
	
Example request

::

   curl -XPOST 'https://openpoland.net/api/asset/2888/data_filter'\
 	-d '{
    	"query":{
         	"time" :[1998],
           	"nts" : [4326562000]
    	}
	} ' \
 	-H "Content-Type: application/json"\
 	-H 'Authorization: Token __YOUR_TOKEN_HERE__'

Response (HTTP/1.0 200 OK):
::

	{
	   "asset_id" : "2577",
	   "has_next" : true,
	   "page" : 15,
	   "results" :
	      [
	         [
	            "2245068000",
	            "Powiat m.Jaworzno",
	            "w wieku poprodukcyjnym",
	            "kobiety",
	            "2008",
	            "osoba",
	            10843.0,
	            "B"
	         ],
	         [
	            "2245075000",
	            "Powiat m.Sosnowiec",
	            "ogółem",
	            "ogółem",
	            "2008",
	            "osoba",
	            221259.0,
	            "B"
	         ],
	         [
	            "2245075000",
	            "Powiat m.Sosnowiec",
	            "ogółem",
	            "mężczyźni",
	            "2008",
	            "osoba",
	            104983.0,
	            "B"
	         ],
	         [
	            "2245075000",
	            "Powiat m.Sosnowiec",
	            "ogółem",
	            "kobiety",
	            "2008",
	            "osoba",
	            116276.0,
	            "B"
	         ]
	      ]
	}



