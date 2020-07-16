
### 1. Конфигурация Graylog кластера для сбора и обработки логов

Исходя из опыта и цифры 23 GB / день, то есть около 700 GB / месяц и срока хранения 2 месяца (1400 GB) понадобится:

* 3 сервера, на каждом:
    * 4-6 vcpu (должо хватить 4)
    * 12 GB RAM
    * Диск 2 TB (вполне подойдет и hdd, но если поиск нужен более быстрый, а бюджет позволяет, тогда ssd/nvme)
* Elasticsearch будет запущен в режиме кластера на 3 шардах с 2 репликами
* Heap 6GB - то есть половина от доступного RAM. Имеем схожие нагрузки на graylog в нашем проекте и все работает стабильно.
* Размер диска с учетом формата хранения в elasticsearch и его требования по наличию свободного места для аллокации


### 2. Graylog extractors

Для выделения данных из полей у нас уже настроены похожие паттерны и конвертеры:

* Remote Address: "(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})
* Request Timestamp:    regex_value: .+?\[(.+?)\], Converters:     date_format: dd/MMM/YYYY:HH:mm:ss Z
* Response Bytes:    regex_value: .+?HTTP/\S+" \d+ (\d+)
* Response Status: .+?HTTP/\S+" (\d+)
* Request Path: .+?"\S+ (\S+).+"
* Request Verb: .+\[.+\] "(\S+)

