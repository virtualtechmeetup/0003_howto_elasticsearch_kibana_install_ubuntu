# 0003_howto_elasticsearch_kibana_install_ubuntu
Instructions and configurations files for Elasticsearch and Kibana as discussed in https://youtu.be/gZb7HpVOges

## How To Install Elasticsearch and Kibana on Ubuntu Linux

The Elastic Stack is a great platform for various data analytics use-cases. But how do you get started? This video goes beyond a simple default installation of Elasticsearch and Kibana. It discusses real-world best practices for hardware sizing and configuration, providing production-level performance and reliability.

[![es_install_thumbnail_title](https://user-images.githubusercontent.com/10326954/75766188-0668a480-5d41-11ea-96d6-1c0e2aeba02b.png)](https://youtu.be/gZb7HpVOges)

## Rob's Hardware Recommendations

Environment | CPU cores | Memory | Storage
--- | ---:| ---:| ---
Local Development | 2 | 16GB | local SSD
Full-Time Lab | 4 | 32GB | local SSD
Production Data Nodes | 12 | 96GB | 6-8TB local SSDs

## 



## Install Elasticsearch

#### 1. Elastic signs all of its packages with the Elasticsearch PGP Signing Key. Add this key to your server.

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

#### 2. As the packages will be retrieved via HTTPS, you will need to install the apt-transport-https package.

```
sudo apt install apt-transport-https
```

#### 3. You will next need to add the Elastic repository.

There are different repositories for the standard distribution (X-Pack pre-bundled) and the Apache 2.0 licensed distribution. You will use the standard distribution in order to take advantage of the Security features of the X-Pack Basic tier.

```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

#### 4. You will now need to update the local cache of available packages and their versions.

```
sudo apt update
```

#### 5. You can now install Elasticsearch.

```
sudo apt install elasticsearch
```

#### 6. Configure JVM Heap.

If a JVM is started with unequal initial and max heap sizes, it may pause as the JVM heap is resized during system usage. For this reason it’s best to start the JVM with the initial and maximum heap sizes set to equal values.

Edit `/etc/elasticsearch/jvm.options` and set `-Xms` and `-Xmx` to about one third of the system memory, but do not exceed `31g`. For example...

```
# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms8g
-Xmx8g
```

#### 7. Increase System Limits.

You should specify system limits in a systemd configuration file for the elasticsearch service.

```
sudo mkdir /etc/systemd/system/elasticsearch.service.d
```

Copy the provided file `etc/systemd/system/elasticsearch.service.d/elasticsearch.conf` to `/etc/systemd/system/elasticsearch.service.d`

Additionally, copy the provided file `etc/sysctl.d/70-elasticsearch.conf` to `/etc/sysctl.d`. Reboot the system for these changes to take effect.

#### 8. Modify the Elasticsearch configuration.

Replace the default Elasticsearch configuration file with the provided configuration by copying `etc/elasticsearch/elasticsearch.yml` to `/etc/elasticsearch`

Modify this configuration as may be appropriate for your environment.

#### 9. Start Elasticsearch.

```
sudo systemctl daemon-reload
```

```
sudo systemctl start elasticsearch.service
```

#### 10. Enable Elasticsearch to start automatically when the system is started.

```
sudo systemctl enable elasticsearch.service
```

## Install Kibana

#### 1. Install Kibana.

```
sudo apt install kibana
```

#### 2. Modify the Kibana configuration.

Replace the default Kibana configuration file with the provided configuration by copying `etc/kibana/kibana.yml` to `/etc/kibana`

Modify this configuration as may be appropriate for your environment.

#### 3. Start Kibana.

```
sudo systemctl daemon-reload
```

```
sudo systemctl start kibana.service
```

Be patient. It may take a moment the first time as Kibana optimizes enabled applications.

#### 4. Enable Kibana to start automatically when the system is started.

```
sudo systemctl enable kibana.service
```
