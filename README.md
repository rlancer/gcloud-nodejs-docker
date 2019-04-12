# AppEngine GitLab - CI / CD Base Image 

## Includes 
* **gcloud command-line tool** - https://cloud.google.com/sdk/gcloud/
* **gae-ayaml-env** - https://www.npmjs.com/package/gae-ayaml-env - app.yaml generator for populating enviorment varibles in the CI / CD process 
* **install me maybe** - https://www.npmjs.com/package/install-me-maybe, prevents having to install node modules twice if the previous version has been cached (currently GitLab's CI / CD GKE runner does not support caching)


## Example useage 

```yaml
deploy:
  image: "rlancer/gcloud-node:LTS-229"
  script:
    - install-me-maybe # or npm i
    - npm run build
    - gae-ayaml-env # to puplulate app.yaml with env vars
    - echo $GCLOUD_SERVICE > /tmp/$CI_PIPELINE_ID.json
    - gcloud auth activate-service-account --key-file /tmp/$CI_PIPELINE_ID.json
    - gcloud --quiet --project $GCLOUD_PROJECT_ID app deploy app.yaml
```
