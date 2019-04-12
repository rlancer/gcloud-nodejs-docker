# AppEngine GitLab - CI / CD Base Image 

View on Docker Hub - https://hub.docker.com/r/rlancer/gcloud-node

## Included in tag 1.0.2
* **NodeJS @ 10.15.3- NPM @ 6.9.0**  
* **gcloud command-line tool @ 241.0.0** - https://cloud.google.com/sdk/gcloud/
* **gae-ayaml-env @ 0.0.19** - https://www.npmjs.com/package/gae-ayaml-env - app.yaml generator for populating enviorment varibles in the CI / CD process 
* **install-me-maybe @ 0.0.3** - https://www.npmjs.com/package/install-me-maybe, prevents having to install node modules twice if the previous version has been cached (currently GitLab's CI / CD GKE runner does not support caching)


## Example useage 

```yaml
deploy:
  image: "rlancer/gcloud-node:1.0.1"
  script:
    - install-me-maybe # or npm i
    - npm run build
    - gae-ayaml-env # to puplulate app.yaml with env vars
    - echo $GCLOUD_SERVICE > /tmp/$CI_PIPELINE_ID.json
    - gcloud auth activate-service-account --key-file /tmp/$CI_PIPELINE_ID.json
    - gcloud --quiet --project $GCLOUD_PROJECT_ID app deploy app.yaml
```
