Materiały do wpisu: 

## Srodowisko

sudo docker-compose up -d

## CURL

$ (echo -n '{"filename":"U2.docx", "data": "'; base64 ./U2.docx; echo '"}') | curl -H "Content-Type: application/json" -d @-  http://192.168.114.128:9200/songs/_doc/1?pipeline=attachment

## Kibana Dev Tool

PUT _ingest/pipeline/attachment
{
  "description" : "What did you hide in this file? (¬‿¬)",
  "processors" : [
    {
      "attachment" : {
        "field" : "data"
      }
    }
  ]
}


GET /songs/_doc/1

POST /songs/_search
{
  "query":{
    "term":{
      "attachment.content": "kissed"
    }
  }
}

POST /songs/_search
{
  "query": {
    "query_string": {
      "default_field": "attachment.content", 
      "query": "kiss"
    }
  }
}

DELETE /songs

PUT /songs
{
  "mappings": {
    "properties": {
      "attachment.content": {
        "type": "text",
        "analyzer": "english"
      }
    }
  }
}

PUT _ingest/pipeline/better_attachment
{
  "description" : "What did you hide in this file? better version (¬‿¬)",
  "processors" : [
    {
      "attachment" : {
        "field" : "data",
        "properties": [ "content", "title" ],
        "indexed_chars" : 20,
        "indexed_chars_field" : "max_size"
      }
    }
  ]
}

PUT _ingest/pipeline/even_better_attachment
{
  "description" : "What did you hide in this file? even better version (¬‿¬)",
  "processors" : [
    {
      "attachment" : {
        "field" : "data",
        "properties": [ "content", "title" ],
        "indexed_chars" : 20,
        "indexed_chars_field" : "max_size"
      },
      "remove":{
        "field":"data"
      }
    }
  ]
}

