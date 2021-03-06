# Elastic-Job - distributed scheduled job solution

[![Total Lines](https://tokei.rs/b1/github/elasticjob/elastic-job-lite?category=lines)](https://github.com/elasticjob/elastic-job-lite)
[![Build Status](https://travis-ci.org/apache/shardingsphere-elastic-job-lite.svg?branch=master)](https://travis-ci.org/github/apache/shardingsphere-elastic-job-lite)
[![Maven Status](https://maven-badges.herokuapp.com/maven-central/elaticjob.shardingsphere.apache.org/elastic-job-lite/badge.svg)](https://maven-badges.herokuapp.com/maven-central/elaticjob.shardingsphere.apache.org/elastic-job-lite)
[![Gitter](https://badges.gitter.im/Elastic-JOB/elastic-job-lite.svg)](https://gitter.im/Elastic-JOB/elasticjob?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Coverage Status](https://coveralls.io/repos/elasticjob/elastic-job/badge.svg?branch=master&service=github)](https://coveralls.io/github/elasticjob/elastic-job?branch=master)
[![GitHub release](https://img.shields.io/github/release/elasticjob/elastic-job.svg)](https://github.com/elasticjob/elastic-job/releases)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

# [Homepage](http://shardingsphere.apache.org/elasticjob/)

# [中文主页](http://shardingsphere.apache.org/elasticjob/index_zh.html)

# Elastic-Job-Lite Console [![GitHub release](https://img.shields.io/badge/release-download-orange.svg)](https://elasticjob.io/dist/elastic-job-lite-console-2.1.5.tar.gz)

# Overview

Elastic-Job is a distributed scheduled job solution. Elastic-Job is composited from 2 independent sub projects: Elastic-Job-Lite and Elastic-Job-Cloud.

Elastic-Job-Lite is a centre-less solution, use lightweight jar to coordinate distributed jobs.

Elastic-Job-Lite and Elastic-Job-Cloud provide unified API. Developers only need code one time, then decide to deploy Lite or Cloud as you want.

# Features

* Distributed schedule job coordinate
* Elastic scale in and scale out supported
* Failover
* Misfired jobs refire
* Sharding consistently, same sharding item for a job only one running instance
* Self diagnose and recover when distribute environment unstable
* Parallel scheduling supported
* Job lifecycle operation
* Lavish job types
* Spring integrated and namespace supported
* Web console

# Architecture

## Elastic-Job-Lite

![Elastic-Job-Lite Architecture](docs/static/img/architecture/elastic_job_lite.png)


# [Release Notes](https://github.com/elasticjob/elastic-job/releases)

# [Roadmap](ROADMAP.md)

# Quick Start

## Add maven dependency

```xml
<!-- import elastic-job lite core -->
<dependency>
    <groupId>org.apache.shardingsphere.elasticjob</groupId>
    <artifactId>elastic-job-lite-core</artifactId>
    <version>${lasted.release.version}</version>
</dependency>

<!-- import other module if need -->
<dependency>
    <groupId>org.apache.shardingsphere.elasticjob</groupId>
    <artifactId>elastic-job-lite-spring</artifactId>
    <version>${lasted.release.version}</version>
</dependency>
```
## Job development

```java
public class MyElasticJob implements SimpleJob {
    
    @Override
    public void execute(ShardingContext context) {
        switch (context.getShardingItem()) {
            case 0: 
                // do something by sharding item 0
                break;
            case 1: 
                // do something by sharding item 1
                break;
            case 2: 
                // do something by sharding item 2
                break;
            // case n: ...
        }
    }
}
```

## Job configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:elasticjob="http://elasticjob.shardingsphere.apache.org/schema/elasticjob"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://elasticjob.shardingsphere.apache.org/schema/elasticjob
                           http://elasticjob.shardingsphere.apache.org/schema/elasticjob/elasticjob.xsd
                           ">
    <!--configure registry center -->
    <elasticjob:zookeeper id="regCenter" server-lists="yourhost:2181" namespace="elastic-job" base-sleep-time-milliseconds="1000" max-sleep-time-milliseconds="3000" max-retries="3" />

    <!--configure monitor -->
    <elasticjob:monitor id="monitor1" registry-center-ref="regCenter" monitor-port="9999"/>
    
    <!--configure job class -->
    <bean id="simpleJob" class="xxx.MyElasticJob" />
    
    <!--configure job -->
    <elasticjob:simple id="oneOffElasticJob" job-ref="simpleJob" registry-center-ref="regCenter" cron="0/10 * * * * ?"   sharding-total-count="3" sharding-item-parameters="0=A,1=B,2=C" />
</beans>
```
