## Deploy to PCF

### Deploy a Spring microservice up and running on the Pivotal Platform.
To save time, we will be using Pivotal Web Services, an instance of Pivotal Platform hosted by Pivotal
We will use the cf CLI to perform all commands on apps deployed to the Pivotal Platform.

#### To install the CloudFoundry CLI:
```
https://github.com/cloudfoundry/cli#installers-and-compressed-binaries
```

Select the package manager for the MacOS
```
https://packages.cloudfoundry.org/stable?release=macosx64&source=github
```

To test that the CF CLI works, we can invoke the help command:
```
cf help
```

#### Deploy the sample app
```
git clone git@github.com:networknt/light-example-4j.git

Let's use the light-to-spring migration microservice
cd light-example-4j/springboot/servlet

To build a clean copy, let's run:
./mvnw clean install spring-boot:run
```

#### Check that the app is running and let's test the endpoints:
```
curl http://localhost:8081/spring/pets
curl http://localhost:8081/light/pets

curl -H "Authorization: Bearer eyJraWQiOiIxMDAiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJ1cm46Y29tOm5ldHdvcmtudDpvYXV0aDI6djEiLCJhdWQiOiJ1cm46Y29tLm5ldHdvcmtudCIsImV4cCI6MTc5NDg3MzA1MiwianRpIjoiSjFKdmR1bFFRMUF6cjhTNlJueHEwQSIsImlhdCI6MTQ3OTUxMzA1MiwibmJmIjoxNDc5NTEyOTMyLCJ2ZXJzaW9uIjoiMS4wIiwidXNlcl9pZCI6InN0ZXZlIiwidXNlcl90eXBlIjoiRU1QTE9ZRUUiLCJjbGllbnRfaWQiOiJmN2Q0MjM0OC1jNjQ3LTRlZmItYTUyZC00YzU3ODc0MjFlNzIiLCJzY29wZSI6WyJ3cml0ZTpwZXRzIiwicmVhZDpwZXRzIl19.gUcM-JxNBH7rtoRUlxmaK6P4xZdEOueEqIBNddAAx4SyWSy2sV7d7MjAog6k7bInXzV0PWOZZ-JdgTTSn6jTb4K3Je49BcGz1BRwzTslJIOwmvqyziF3lcg6aF5iWOTjmVEF0zXwMJtMc_IcF9FAA8iQi2s5l0DYgkMrjkQ3fBhWnopgfkzjbCuZU2mHDSQ6DJmomWpnE9hDxBp_lGjsQ73HWNNKN-XmBEzH-nz-K5-2wm_hiCq3d0BXm57VxuL7dxpnIwhOIMAYR04PvYHyz2S-Nu9dw6apenfyKe8-ydVt7KHnnWWmk1ErlFzCHmsDigJz0ku0QX59lM7xY5i4qA" http://localhost:8081/spring/pets | json_pp
```

##### Handy Reference:
http://cli.cloudfoundry.org/en-US/cf/

#### 1. Log in to PWS:
```cf login -a https://api.run.pivotal.io```

#### 2. List the apps running in the org:
```cf apps```

There is no spring-light application, and we want to deploy it

#### 3. Push the app to PWS
```cf push```

check with ```cf apps```

#### 4. Check the logs:
```
cf logs spring-light --recent
cf logs spring-light
```

#### 5. Check the health of the app:
Check the status of the app with: ```cf app spring-light```

```
Showing health and status for app spring-light in org dan.dobrin-org / space development as dan.dobrin@gmail.com...

name:              spring-light
requested state:   started
routes:            spring-light-persistent-lynx.cfapps.io
last uploaded:     Sat 19 Oct 11:16:49 EDT 2019
stack:             cflinuxfs3
buildpacks:        https://github.com/cloudfoundry/java-buildpack.git

type:           web
instances:      1/1
memory usage:   768M
     state     since                  cpu    memory           disk           details
#0   running   2019-10-19T15:17:05Z   0.1%   142.3M of 768M   129.2M of 1G
```

#### 6. Access the service
```
curl https://spring-light-demo.cfapps.io/spring/pets

curl -H "Authorization: Bearer eyJraWQiOiIxMDAiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJ1cm46Y29tOm5ldHdvcmtudDpvYXV0aDI6djEiLCJhdWQiOiJ1cm46Y29tLm5ldHdvcmtudCIsImV4cCI6MTc5NDg3MzA1MiwianRpIjoiSjFKdmR1bFFRMUF6cjhTNlJueHEwQSIsImlhdCI6MTQ3OTUxMzA1MiwibmJmIjoxNDc5NTEyOTMyLCJ2ZXJzaW9uIjoiMS4wIiwidXNlcl9pZCI6InN0ZXZlIiwidXNlcl90eXBlIjoiRU1QTE9ZRUUiLCJjbGllbnRfaWQiOiJmN2Q0MjM0OC1jNjQ3LTRlZmItYTUyZC00YzU3ODc0MjFlNzIiLCJzY29wZSI6WyJ3cml0ZTpwZXRzIiwicmVhZDpwZXRzIl19.gUcM-JxNBH7rtoRUlxmaK6P4xZdEOueEqIBNddAAx4SyWSy2sV7d7MjAog6k7bInXzV0PWOZZ-JdgTTSn6jTb4K3Je49BcGz1BRwzTslJIOwmvqyziF3lcg6aF5iWOTjmVEF0zXwMJtMc_IcF9FAA8iQi2s5l0DYgkMrjkQ3fBhWnopgfkzjbCuZU2mHDSQ6DJmomWpnE9hDxBp_lGjsQ73HWNNKN-XmBEzH-nz-K5-2wm_hiCq3d0BXm57VxuL7dxpnIwhOIMAYR04PvYHyz2S-Nu9dw6apenfyKe8-ydVt7KHnnWWmk1ErlFzCHmsDigJz0ku0QX59lM7xY5i4qA" https://spring-light-demo.cfapps.io/spring/pets | json_pp

curl -H "Authorization: Bearer eyJraWQiOiIxMDAiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJ1cm46Y29tOm5ldHdvcmtudDpvYXV0aDI6djEiLCJhdWQiOiJ1cm46Y29tLm5ldHdvcmtudCIsImV4cCI6MTc5NDg3MzA1MiwianRpIjoiSjFKdmR1bFFRMUF6cjhTNlJueHEwQSIsImlhdCI6MTQ3OTUxMzA1MiwibmJmIjoxNDc5NTEyOTMyLCJ2ZXJzaW9uIjoiMS4wIiwidXNlcl9pZCI6InN0ZXZlIiwidXNlcl90eXBlIjoiRU1QTE9ZRUUiLCJjbGllbnRfaWQiOiJmN2Q0MjM0OC1jNjQ3LTRlZmItYTUyZC00YzU3ODc0MjFlNzIiLCJzY29wZSI6WyJ3cml0ZTpwZXRzIiwicmVhZDpwZXRzIl19.gUcM-JxNBH7rtoRUlxmaK6P4xZdEOueEqIBNddAAx4SyWSy2sV7d7MjAog6k7bInXzV0PWOZZ-JdgTTSn6jTb4K3Je49BcGz1BRwzTslJIOwmvqyziF3lcg6aF5iWOTjmVEF0zXwMJtMc_IcF9FAA8iQi2s5l0DYgkMrjkQ3fBhWnopgfkzjbCuZU2mHDSQ6DJmomWpnE9hDxBp_lGjsQ73HWNNKN-XmBEzH-nz-K5-2wm_hiCq3d0BXm57VxuL7dxpnIwhOIMAYR04PvYHyz2S-Nu9dw6apenfyKe8-ydVt7KHnnWWmk1ErlFzCHmsDigJz0ku0QX59lM7xY5i4qA" https://spring-light-demo.cfapps.io/light/pets | json_pp
```

#### 7. Pivotal Services
Pivotal Platform enables administrators to provide a variety of services on the platform that can easily be consumed by applications.

Create a DB service.
**List all tiers available**
```cf marketplace -s elephantsql```

For demo purposes, create a service  using the free plan (turtle)
```cf create-service elephantsql turtle spring-light-db```

Bind the newly created service to the spring-ligtapp:
```cf bind-service spring-light spring-light-db```
Once a service is bound to an app, environment variables are stored that allow the app to connect to the service after a push, restage, or restart command

Tip: Use 'cf restage spring-light' to ensure your env variable changes take effect
```cf restage spring-light```

Check services and apps with:
```
cf services
cf app spring-light
```

See logs:
```
cf logs spring-light --recent
cf logs spring-light
```

#### 8. Scale the application:
Increasing the available disk space or memory can improve overall app performance.
Similarly, running additional instances of an app can allow an app to handle increases in user load and concurrent requests. These adjustments are called scaling.

a. Scaling up available disk space or memory is called vertical scaling.
b. Scaling your app horizontally adds or removes app instances. Adding more instances allows your application to handle increased traffic and demand.

Scale horizontally:
```
cf scale spring-light -i 2
```

Scale memory up - vertically:
```
cf scale spring-light -m 1G
```

Scale down disk space - vertically:
```
cf scale spring-light -k 512M
```

#### 9. Logout out PCF

```cf logout```