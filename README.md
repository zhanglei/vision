# Vision

[![License GPL 3][badge-license]][COPYING]
![Version][badge-release]

## Description

[Vision][] is a system monitoring and log collector.

### Core

Powered by the following tools:

* [Docker][]: a portable, lightweight runtime and packaging tool.
* [HAProxy][]: a TCP/HTTP load balancer.
* [Consul][]: service discovering
* [Consul-template][]: populate values from Consul on your filesystem.
* [Registrator][]: automatically register/deregister Docker containers into Consul.

### Logging

Log collector service is provided using :

* [Elasticsearch][] (v1.4.2) web interface : `http://xxx:9200`
* [Kibana][] (v4.0.0-beta3) web interface : `http://xxx:5601`

Some [Elasticsearch][] plugins are available:
* [ElasticSearchHead][]: `http://xxx:9200/_plugin/head/`
* [ElasticHQ][]: `http://xxx:9200/_plugin/HQ/`
* [Kopf][]: `http://xxx:9200/_plugin/kopf/`

### Monitoring

Monitoring service is provided using :

* [Prometheus][]: An open-source service monitoring system and time series database.
* [Grafana][] (v1.9.0) web interface : `http://xxx:9090/`
* [InfluxDB][] (v0.8.7) web interface : `http://xxx:8083`
* [cAdvisor][] (v0.8.0) is used (`http://xxx:8081`) to monitoring containers.

## Deployment

### Local

* Download and install a release :

        $ curl https://github.com/nlamirault/vision/releases/download/x.y.z/vision-x.y.z-linux.tar.gz
        $ tar zxvf vision-x.y.z
        $ cd vision-x.y.z

* Start it :

        $ ./init.sh
        $ ./compose -d up

* Creates the [InfluxDB][] database:

        $ curl -X POST 'http://localhost:8086/db?u=root&p=root' \
            -d '{"name": "vision"}'

* Verify input datas from the InfluxDB UI (on 8083), using this query, after choosing `vision`
  database:

        select * from /.*/ limit 100


## Usage

### Monitoring

* You could use [sysinfo_influxdb][] to send metrics :

        $ sysinfo_influxdb -host 127.0.0.1:8086 -P vision -d vision -v=text -D


### Logging

* You could use [Heka][] and this configuration file [addons/hekad.toml][]
  to watch `local7.log` and send them to Elasticsearch:

        $ curl http://xx.xx.xx.xx:9200/hekad -X POST

* Using binary :

        $ sudo bin/hekad -config=addons/hekad.toml

* Into [Kibana][], set the default index and visualize logs:

        logfile-*


## Development

* Build the images :

        $ make build image=xxx

* Setup directories :

        $ make setup


## Support

Feel free to ask question or make suggestions in our [Issue Tracker][].


## License

Vision is free software: you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

Vision is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.  See the GNU General Public License for more details.

See [COPYING][] for the complete license.


## Changelog

A [ChangeLog.md][] is available.


## Contact

Nicolas Lamirault <nicolas.lamirault@gmail.com>



[Vision]: https://github.com/nlamirault/vision
[COPYING]: https://github.com/nlamirault/vision/blob/master/COPYING
[Issue tracker]: https://github.com/nlamirault/vision/issues
[badge-license]: https://img.shields.io/badge/license-GPL_3-green.svg
[badge-release]: https://img.shields.io/github/release/nlamirault/vision.svg

[Docker]: https://www.docker.io
[Docker documentation]: http://docs.docker.io

[Elasticsearch]: http://www.elasticsearch.org
[Grafana]: http://grafana.org/
[Kibana]: http://www.elasticsearch.org/overview/kibana/
[ElasticSearchHead]: http://mobz.github.io/elasticsearch-head
[ElasticHQ]: http://www.elastichq.org
[Kopf]: https://github.com/lmenezes/elasticsearch-kopf
[Fluentd]: http://fluentd.org/
[Heka]: http://hekad.readthedocs.org/en/latest/
[Supervisor]: http://supervisord.org
[sysinfo_influxdb]: https://github.com/novaquark/sysinfo_influxdb
[InfluxDB]: http://influxdb.com
[cAdvisor]: https://github.com/google/cadvisor
[HAProxy]: http://www.haproxy.org/
[Consul]: http://www.consul.io
[Consul-template]: https://github.com/hashicorp/consul-template
[Registrator]: https://github.com/gliderlabs/registrator
[Prometheus]: See: http://prometheus.io

[Virtualbox]: https://www.virtualbox.org
[Vagrant]: http://downloads.vagrantup.com
[SystemdD]: http://freedesktop.org/wiki/Software/systemd
