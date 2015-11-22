## this is file nagivator in browser 

## Requirements :
   vuejs >= 1 & vue-resource (0.1.16) 
   
the current featutes :
1) Folder navigation ( backward is also possible )
2) folder creation 
3) file upload

this is json file format expected, 

the ur

```
[  
   {  
      "name":"uploads",
      "type":"folder",
      "path":"uploads",
      "items":[  
         {  
            "name":"file.txt",
            "type":"file",
            "path":"uploads/file.txt",
            "size":8
         },
         {  
            "name":"hotel",
            "type":"folder",
            "path":"uploads/hotel",
            "items":[  
               {  
                  "name":"kamal.jpg",
                  "type":"file",
                  "path":"uploads/hotel/kamal.jpg",
                  "size":34017
               },
               {  
                  "name":"visa.jpg",
                  "type":"file",
                  "path":"uploads/hotel/visa.jpg",
                  "size":561074
               },
               {  
                  "name":"new",
                  "type":"folder",
                  "path":"uploads/hotel/new",
                  "items":[  
                     {  
                        "name":"test",
                        "type":"folder",
                        "path":"uploads/hotel/new/test",
                        "items":[  
                           {  
                              "name":"Name",
                              "type":"folder",
                              "path":"uploads/hotel/new/test/Name",
                              "items":[  

                              ]
                           }
                        ]
                     }
                  ]
               }
            ]
         }
      ]
   }
]
```

## Things To Do
---------------
0) URL hard coding has to be removed
1) Delete folder/file
2) Rename folder/file
3) upload the folder 
4) Icons on file according to file MIME type  
