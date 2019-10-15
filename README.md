# the-case-for-chaos-testing
Intro to Chaos Testing as a scientific method to increase confidence and security. This is a talk titled 'The Case for Chaos Testing' and the slides are available here: https://speakerdeck.com/peterlamar/chaostesting

References:

http://principlesofchaos.org/
https://news.ycombinator.com/item?id=20183590
https://news.ycombinator.com/item?id=16244586
https://outage.report/
https://github.com/dastergon/awesome-chaos-engineering
[CNCF Chaos Landscape](https://landscape.cncf.io/category=chaos-engineering&format=card-mode&grouping=category)

Demo 1:

[Repo/Demo](https://github.com/alexei-led/pumba)

``` 
docker run -it --rm --name tryme alpine sh -c "apk add --update iproute2 && ping www.example.com"
```
Notice the consistent pings to the url

In order to give Pumba access to the Docker daemon on the host machine, you will need to mount var/run/docker.sock unix socket.

Now open a new terminal window and run:
```
docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock gaiaadm/pumba --log-level debug --interval 20s netem --duration 10s delay tryme 
```

Demo 2:
[Repo/Demo](https://github.com/chaosblade-io/chaosblade)

```
docker pull registry.cn-hangzhou.aliyuncs.com/chaosblade/chaosblade-demo:latest
docker run -it registry.cn-hangzhou.aliyuncs.com/chaosblade/chaosblade-demo:latest
Less README.txt
```
Demo Delay
```
curl http://localhost:8080/dubbo/hello?name=dubbo
blade prepare jvm --process business
blade c dubbo delay --time 3000 --service com.example.service.DemoService --methodname sayHello --consumer
curl http://localhost:8080/dubbo/hello?name=dubbo
blade status 0611e66df1b4c498
blade destroy 0611e66df1b4c498
blade status 0611e66df1b4c498
curl http://localhost:8080/dubbo/hello?name=dubbo
```

Demo 3:

[Repo](https://github.com/litmuschaos/litmus)

Demo [Source](https://docs.litmuschaos.io/docs/next/example.html)


From litmus directory, with 'minikube start'

```
kubectl apply -f https://litmuschaos.github.io/pages/litmus-operator-latest.yaml
kubectl create -f https://raw.githubusercontent.com/litmuschaos/chaos-charts/master/charts/generic/experiments.yaml -n litmus
kubectl run myserver --image=nginx -n litmus
kubectl annotate deploy/myserver litmuschaos.io/chaos="true" -n litmus
kubectl create -f chaosengine.yaml
watch -n 1 kubectl get pods -n litmus
kubectl describe chaosresult engine-nginx-pod-delete -n litmus
```