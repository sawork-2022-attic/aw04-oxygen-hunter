# WebPOS

The demo shows a web POS system , which replaces the in-memory product db in aw03 with a one backed by 京东.


![](jdpos.png)

To run

```shell
mvn clean spring-boot:run
```

Currently, it creates a new session for each user and the session data is stored in an in-memory h2 db. 
And it also fetches a product list from jd.com every time a session begins.

1. Build a docker image for this application and performance a load testing against it.
2. Make this system horizontally scalable by using haproxy and performance a load testing against it.
3. Take care of the **cache missing** problem (you may cache the products from jd.com) and **session sharing** problem (you may use a standalone mysql db or a redis cluster). Performance load testings.

Please **write a report** on the performance differences you notices among the above tasks.

# scale up

running in one docker image, without cache, tested by Gatling

cpus=0.5

D:\Softwares\Gatling\gatling-charts-highcharts-bundle-3.7.6\results\gatlingtestsimulationonwebpos-20220329141614920\index.html

cpus=1

file:///D:/Softwares/Gatling/gatling-charts-highcharts-bundle-3.7.6/results/gatlingtestsimulationonwebpos-20220329141048198/index.html

cpus=2 

~~D:\Softwares\Gatling\gatling-charts-highcharts-bundle-3.7.6\results\gatlingtestsimulationonwebpos-20220329141740646\index.html  (seems baned by JD)~~

D:\Softwares\Gatling\gatling-charts-highcharts-bundle-3.7.6\results\gatlingtestsimulationonwebpos-20220329142819937\index.html

# scale out

running in 4 docker images, using haproxy http mode as load balancer, tested by Gatling

> if I don't use cache, 500 ERROR will happen when calling parseJD, and Gatling will fail about 50%~70%.
> if I use cache on JD::getProducts, 500 ERROR will happen less frequently, but Gatling will fail about 50%.

no cache: D:\Softwares\Gatling\gatling-charts-highcharts-bundle-3.7.6\results\gatlingtestsimulationonwebpos-20220330033306576\index.html

cache: D:\Softwares\Gatling\gatling-charts-highcharts-bundle-3.7.6\results\gatlingtestsimulationonwebpos-20220330035926166\index.html

# session sharing

