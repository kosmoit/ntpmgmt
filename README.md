# Selenium-Grid Updates
the Selenium Hub and Nodes are published in the mbms-dev repo, via the selenium-grid helm chart folder this file is in...
The images used are pulled from the official selenium repos, and then tagged, and pushed into nexus.  Currently our deployment is only using the HUB and Chrome-Node features/images.

# Updating Images
If requested to update the image in use to a newer one,  confirm from requester, which image is needed, and advise as of this current date (of this writing) the current image is:  
```
selenium/hub:4.3.0-20220628
&
selenium/node-chrome:4.3.0-20220628
```

**This is controlled in the values.yaml on first few lines with the imageTag & nodesImageTag variables.  (keep them the same)**

if you need to download and upload a new hub/chrome you would do the following  (example but different docker tag... and THEN update the values.yaml per above...)
```
docker pull selenium/hub:4.3.0-20220628
docker pull selenium/node-chrome:4.3.0-20220628

docker tag selenium/hub:4.3.0-20220628 container-registry.dev8.bip.va.gov/mbms/selenium/hub:4.3.0-20220628
docker tag selenium/node-chrome:4.3.0-20220628 container-registry.dev8.bip.va.gov/mbms/selenium/node-chrome:4.3.0-20220628

docker push container-registry.dev8.bip.va.gov/mbms/selenium/hub:4.3.0-20220628
docker push container-registry.dev8.bip.va.gov/mbms/selenium/node-chrome:4.3.0-20220628

UPDATE VALUES.yaml with new tag number

```

After updating the chart, github repo/etc.. below you will see a sample helm deployment..  current helm client version is use is 3.7.2

**Example Helm deployment, after updating yaml***
```
helm  upgrade --install selenium-grid .\selenium-grid\ -n mbms-dev
Release "selenium-grid" has been upgraded. Happy Helming!
NAME: selenium-grid
LAST DEPLOYED: Wed Oct  5 11:46:44 2022
NAMESPACE: mbms-dev
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
Selenium Grid Server deployed successfully.
1. Ingress is disabled. To access Selenium from outside of Kubernetes, simply run these commands:
    export POD_NAME=$(kubectl get pods -n mbms-dev -l "app.kubernetes.io/name=selenium-hub,app.kubernetes.io/instance=selenium-grid" -o jso
npath="{.items[0].metadata.name}")
    echo "Point your WebDriver tests to http://localhost:4444/wd/hub"
    kubectl -n mbms-dev port-forward $POD_NAME 4444:4444

2. Within Kubernetes cluster, you can use following Service endpoint:
        http://selenium-hub.mbms-dev.svc:4444
```
