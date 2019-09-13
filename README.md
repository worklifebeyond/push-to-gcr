# push-to-gcr
Minimal Github workflow action to push a docker image to Google Cloud Registry

Add `service-account.json` to Github secret as `GCLOUD_CREDENTIALS`

[push-to-gcr.yaml](https://github.com/worklifebeyond/push-to-gcr/blob/master/push-to-gcr.yml)

```yaml
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Build the Docker image
      run: |
        echo $GCLOUD_CREDENTIALS > keyfile.json
        cat keyfile.json | docker login -u _json_key --password-stdin https://gcr.io
        docker build . --file Dockerfile --tag gcr.io/${{ secrets.CLOUDSDK_CORE_PROJECT }}/test:v1
        docker push gcr.io/${{ secrets.CLOUDSDK_CORE_PROJECT }}/test:v1
      env:
        GCLOUD_CREDENTIALS: ${{ secrets.GCLOUD_CREDENTIALS }}
        CLOUDSDK_CORE_PROJECT: ${{ secrets.CLOUDSDK_CORE_PROJECT }}
```
