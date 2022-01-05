---
title: "Elastic Stack 구축"
excerpt: "ELK 오픈소스 프로그램을 활용하여 Logging Data Cluster를 구축하려고 한다."

categories:
  - Data

tags:
  - [Devlopment, Elastic, BigData]

toc: true
toc_sticky: true
 
last_modified_at: 2022-01-02


date: 2022-01-02

---
6233-4073
# Elastic Stack 소개
시스템 로그의 양이 많아지고 관리해야 할 서버가 많아지게 되면 서버 별로 Health Check 및 Application들의 로그 관리/분석이 어렵다.

그래서 흩어져있는 로그 Data들을 Logstash를 사용하여 수집하고 ElasticSearch에 Data를 저장하고 Kibana로 분석 및 시각화를 할 수 있다.
위와 같이 여러 서버의 다양한 로그들을 하나의 ES(Elasticsearch)에서 관리하게 되면 유지보수하기가 훨씬 용이해진다. 

회사 시스템의 로그 관리 개선이 필요하여 Elastic Stack Setup 관련된 정보들을 정리하려고 한다. 

구축 전에 간단히 개념을 학습하고 다음과 같이 몇가지 의문점이 생겼다...

1. 클러스터 구성 시 하나의 서버에 여러 Node를 실행하는 것이 좋을까?  
   
2. 사용시 사내 Data가 Elastic에 노출이 되진 않을까?  
   Kibana Setting에 Cluster Data 수집 허용이 default value가 true로 되어 있는데 다음 설정으로 수집을 못하게 할 수 있다. 수집되는 Data가 외부에서 수집되지 않아야 한다고 하면 이 설정은 필수인 것 같다. 
    ```
    kibana.yml
    # configure Telemetry
    telemetry.allowChangingOptInStatus: false
    #telemetry.optIn: false
    telemetry.enabled: false
    ```
3. 한 개의 Index에 Shard 개수와 최대 몇GB까지 관리하는 것이 적정한지?    
  1개의 Shard에 최대 50GB까지 관리하는 것이 적정 크기로 권고하고 있다. 
  그리고 Index에 최대 할당하는 Shard개수는 ES Node 개수만큼하는 것이 좋다고 하는데 부하 테스트를 통해 성능 점검을 해보면서 관련한 글들은 조금 더 찾아봐야겠다..


## Elasticsearch

---
1. elasticsearch.yml    
   아래 기능을 추가하면 계정 관리가 가능해진다.
    ```
    # 보안 기능 
    xpack.security.enabled: true   
    xpack.security.transport.ssl.enabled: true
    ```

2. Command
    ```
    #비밀번호 자동 생성
    ./bin/elasticsearch-setup-passwords auto
    #비밀번호 수동 생성 (추천)
    ./bin/elasticsearch-setup-passwords interactive
    #elastichsearch 실행
    ./bin/elasticsearch.bat
    ```
  

## Metricbeat
---
1. Command
    ```
    #Load Kibana Dashboard
    ./metricbeat setup -c metricbeat.yml
    
    # execute metricbeat
    ./metricbeat.exe -e -c .\metricbeat.yml
    ```
2. metricbeat.xml
    ```
    setup.template.settings:
      #metricbeat index size가 많은 경우 수정 필요     
      index.number_of_shards: 1
      index.codec: best_compression
      #_source.enabled: false
    ```
    metricbeat에서 생성되는 index 이름을 customizing을 하기 위해서는 아래와 같이 setup.xx 을 추가해줘야 한다. 
    일별로 인덱스 관리를 하기 위해서 다음과 같이 수정했다.

    ```
      output.elasticsearch:
      # Array of hosts to connect to.
      hosts: ["localhost:9200"]
      index: "metricbeat-%{[agent.version]}-%{+yyyy.MM.dd}"  
      #ilm.enabled: true
      # Optional protocol and basic auth credentials.
      #protocol: "https"
      username: "elastic"
      password: "0000"

      setup.template.name: "metricbeat-%{[agent.version]}"
      setup.template.pattern: "metricbeat-%{[agent.version]}-*"
      setup.ilm.enabled: false
    ```

## Logstash
---
 하나의 Logstash 인스턴스로 다양한 Input을 Handling하기 위해 Multi Pipeline으로 설정하려고 한다. 

1. Command
    ```
    #pipeline 실행     
    bin/logstash -f mypipeline.conf

    #multiple pipeline 실행     
    .\bin\logstash.bat

    ```
2. logstash.xml
    ```
    #logstash node/pipeline Health monitor 용도
    xpack.monitoring.enabled: true
    xpack.monitoring.elasticsearch.username: elastic
    xpack.monitoring.elasticsearch.password: mcsmgr
    xpack.monitoring.elasticsearch.hosts: ["http://localhost:9200"]
    ```  
3. pipeline1.conf   
   개인적으로 위 pipeline 설정하는데 삽질이 많았다...
   
   Kibana/Logstash/Elasticsearch의 Timezone 정보가 달라 Date Type의 정합성이 보장되지 않은 것을 보정하느라 오래걸렸다...ㅜㅜ 

   Logstash에서 indexname지정해주는 날짜는 Ruby를 사용해서 @timestamp의 날짜 정보를 parsing했다. 
   그 외에는 timezone을 default값 UTC에서 Asia/Seoul로 변경했다.
   
    ```
    input { 
        #stdin { }
        beats {
          port => "1234"      
          #host => "localhost"
        }  
    }

    filter {    
        grok {
            #match => { "message" => "%{TIMESTAMP_ISO8601:log-time} Log-level: %{LOGLEVEL:log-level} %{GREEDYDATA:log-msg}" }
            #pattern_definitions => { "SYSTIME" => "yyyy-MM-dd HH:mm:ss,SSS"
            match => { "message" => "%{TIMESTAMP_ISO8601:log.time}%{SPACE}\[%{LOGLEVEL:log.level}%{SPACE}\]%{GREEDYDATA:log.msg}" }
            remove_field => "message"        
        }
        date {
            #locale => "ko"        
            match => ["log.time", "yyyy-MM-dd HH:mm:ss,SSS"]
            timezone => "Asia/Seoul"
            #target => "log.time"
            #remove_field => ["log.time"]                                
            #locale => "ko"                    
        }

        ruby {
          code =>  "event.set('index_day', event.get('[@timestamp]').time.localtime.strftime('%y-%m-%d-%H'))"
        }
        mutate {
          remove_field => ["@version", "agent.id", "agent.ephemeral_id", "host.id"]
          #remove_tag => ["beats_input_codec_plain_applied"]
        }
    }

    output {
      elasticsearch {
        hosts => "localhost:9200"    
        user => elastic
        password => mcsmgr
        manage_template => false
        index => "applog-%{index_day}"     
        #index => "applog-%{+YYYY.MM.dd}-%{+HH}"     
        document_type => "%{[@metadata][type]}"
      }
      #stdout { codec => rubydebug }
    }
    ```
    
4. pipelines.yml
    ```
    - pipeline.id: pipeline1
      path.config: "config/pipeline1.conf"
    ```

# Filebeat
1. Command
    ```
    #pipeline 실행     
    ./filebeat -e -c ./filebeat.yml
    ```
2. filebeat.yml   
   로그가 길어질 경우 하나의 로그에 여러줄이 생기게 될경우 묶음으로 보내기 위해서 multiline 설정을 활용한다.
    ```        
    ### Multiline options
    multiline.pattern: '\d{4}\-\d{2}\-\d{2}\s\d{2}\:\d{2}\:\d{2}\,'
    multiline.negate: true
    multiline.match: after
    ```



    





 

