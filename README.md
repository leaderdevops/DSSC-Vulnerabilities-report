# Deep Security Smart check reporting module

[![License](https://img.shields.io/badge/License-Apache%202-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Maintained by ShunyEka Systems Pvt. Ltd.

This module shows how to use the Deep Security Smart Check API to retrieve the vulnerability findings from the last scan on an image.

See the [API reference documentation](https://deep-security.github.io/smartcheck-docs/api/) for more things you can do with the Deep Security Smart Check API.

## Get started

### Usage with docker plugin
```text
docker run -v <Directory to store report>:/root/app tshethp/dssc-vulnerability-report:v4 
            --smartcheck-host SMARTCHECK_HOST 
            --smartcheck-user SMARTCHECK_USER
            --smartcheck-password SMARTCHECK_PASSWORD
            --insecure-skip-tls-verify 
            --min-severity MIN_SEVERITY
            image
            
 example in jenkins pipeline: 
 docker run -v $WORKSPACE:/root/app tshethp/dssc-vulnerability-report:v4 --smartcheck-host abc.smartcheckurl.com --smartcheck-user administrator --smartcheck-password abc@pass --insecure-skip-tls-verify --min-severity low exampleregistry.com:5000/example-service-image:latest
 
 positional arguments:
  image                 The image to scan. Example:
                        registry.example.com/project/image:latest

optional arguments:
  -h, --help            show this help message and exit
  --smartcheck-host SMARTCHECK_HOST
                        The hostname of the Deep Security Smart Check
                        deployment. Example: smartcheck.example.com
  --smartcheck-user SMARTCHECK_USER
                        The userid for connecting to Deep Security Smart Check
  --smartcheck-password SMARTCHECK_PASSWORD
                        The password for connecting to Deep Security Smart
                        Check
  --insecure-skip-tls-verify
                        Ignore certificate errors when connecting to Deep
                        Security Smart Check
  --min-severity MIN_SEVERITY
                        The minimum severity of vulnerability to show.
                        Defaults to "high". Values:
                        [defcon1,critical,high,medium,low,negligible,unknown]
  --show-overridden     Show vulnerabilities that have been marked as
                        overridden
  --show-fixed          Show vulnerabilities that have been fixed by a later
                        layer
```

  If you want to modify reporting code then you can install bellow described dependencies to execute report.
### Install dependencies

You will need Python 3 and [pipenv](https://github.com/pypa/pipenv) to install the dependencies for this project.

```sh
$ pipenv install
Installing dependencies from Pipfile.lock (c53762)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 2/2 — 00:00:00
To activate this project's virtualenv, run the following:
 $ pipenv shell
```


### Usage

```text
usage: list-vulnerabilities.py [-h] [--smartcheck-host SMARTCHECK_HOST]
                               [--smartcheck-user SMARTCHECK_USER]
                               [--smartcheck-password SMARTCHECK_PASSWORD]
                               [--insecure-skip-tls-verify]
                               [--min-severity MIN_SEVERITY]
                               [--show-overridden] [--show-fixed]
                               image
 
 example: py list-vulnerabilities.py --smartcheck-host <DSSC server host name> --smartcheck-user administrator --smartcheck-password <password> --insecure-skip-tls-verify --min-severity high bryce.azurecr.io/bryce/java-struts2-cve-2018-11776:latest

List vulnerabilities found in scans

positional arguments:
  image                 The image to scan. Example:
                        registry.example.com/project/image:latest

optional arguments:
  -h, --help            show this help message and exit
  --smartcheck-host SMARTCHECK_HOST
                        The hostname of the Deep Security Smart Check
                        deployment. Example: smartcheck.example.com
  --smartcheck-user SMARTCHECK_USER
                        The userid for connecting to Deep Security Smart Check
  --smartcheck-password SMARTCHECK_PASSWORD
                        The password for connecting to Deep Security Smart
                        Check
  --insecure-skip-tls-verify
                        Ignore certificate errors when connecting to Deep
                        Security Smart Check
  --min-severity MIN_SEVERITY
                        The minimum severity of vulnerability to show.
                        Defaults to "high". Values:
                        [defcon1,critical,high,medium,low,negligible,unknown]
  --show-overridden     Show vulnerabilities that have been marked as
                        overridden
  --show-fixed          Show vulnerabilities that have been fixed by a later
                        layer
```


```text
usage: startscan.py [-h] [--smart_check_url="SMARTCHECK_HOST"]
                               [--smart_check_userid="SMARTCHECK_USER"]
                               [--smart_check_password="SMARTCHECK_PASSWORD"]
                               [--scan_registry="DOCKER_REGISTRY_SERVER:PORT"]
                               [--scan_repository="REPOSITORY/IMAGE"]
                               [--scan_tag="IMAGE_TAG"]
                               [--registry_user="DOCKER_REGISTRY_USER"]
                               [--registry_password="DOCKER_REGISTRY_PASSWORD"]
                               [--scan_aws="yes"]
                               [--aws_region="AWS_REGION_ID"]
                               [--aws_id="ACCESSKEY_ID"] 
                               [--aws_secret="SECRETKEY_ID]
                               
 
 AWS registry scan example: py -3 startscan.py --smart_check_url="dssc.example.com" --smart_check_userid="administrator" --smart_check_password="<DSSC Password>" --scan_registry="6503275734.dkr.ecr.us-east-1.amazonaws.com" --scan_repository="stage1-webapp" --scan_tag="111" --scan_aws="yes" --aws_region="us-east-1" --aws_id="<ACCESS_KEY>" --aws_secret="<SECRET KEY>"
 
 Generic registry scan example: py -3 startscan.py --smart_check_url="dssc.example.com" --smart_check_userid="administrator" --smart_check_password="<DSSC Passwrod>" --scan_registry="brme.azurecr.io" --scan_repository="brme/backed-app-build" --scan_tag="28" --scan_aws="no" --registry_user="brme" --registry_password="<Registry Password>""


List vulnerabilities found in scans

arguments:
  -h, --help            show this help message and exit
  --smart_check_url="SMARTCHECK_HOST"
                        The hostname of the Deep Security Smart Check
                        deployment. Example: smartcheck.example.com
  --smart_check_userid="SMARTCHECK_USER"
                        The userid for connecting to Deep Security Smart Check
  --smart_check_password="SMARTCHECK_PASSWORD"
                        The password for connecting to Deep Security Smart
                        Check
  --scan_registry="DOCKER_REGISTRY_SERVER:PORT"
                        dcker registry server URL with port number
  --scan_repository="REPOSITORY/IMAGE"
                        scan repository along with image name
                        
  --scan_tag="IMAGE_TAG"
                        repository image tag that need to be scanned
  --scan_aws="yes"
                        if set to yes then it will require AWS region, AWS access key and secret key

  --registry_user="DOCKER_REGISTRY_USER" [OPTIONAL]
                        docker registry username.
                        if --scan_aws="yes" given then this argument is optional 
  --registry_password="DOCKER_REGISTRY_PASSWORD"  [OPTIONAL]      
                        docker registry password.
                        if --scan_aws="yes" given then this argument is optional
  --aws_id="ACCESSKEY_ID" [OPTIONAL]
                        AWS ACR docker registry access key.
                        if --scan_aws="no" given then this argument is optional
  --aws_secret="SECRETKEY_ID [OPTIONAL]
                        AWS ACR docker registry password.
                        if --scan_aws="no" given then this argument is optional
```

## API flow

1. Start out by creating a session and getting the session token. We'll use a sample user with a not-very-complex password:

   <details open>
     <summary>"Create session" request</summary>

   ```text
   POST /api/sessions HTTP/1.1
   Content-Type: application/json
   X-Api-Version: 2018-05-01
   Accept: application/json

   { "user": { "userid": "scan-user", "password": "test" } }
   ```

   </details>

   <details open>
     <summary>"Create session" response</summary>

   ```text
   HTTP/1.1 201 Created
   Content-Type: application/json

   {
     ...
     "token": "bmljZSB0cnkK",
     "href": "{session_href}",
     ...
   }
   ```

   </details>

   We'll keep the `href` and `token` for later use.

2. Then we'll ask for the first scan that matches the image details:

   <details open>
     <summary>"List scans" request</summary>

   ```text
   GET /api/scans?registry=registry.example.com&repository=fake-image&tag=latest&exact=true&limit=1 HTTP/1.1
   X-Api-Version: 2018-05-01
   Authorization: Bearer bmljZSB0cnkK
   Accept: application/json
   ```

   </details>

   <details open>
     <summary>"List scans" response</summary>

   ```text
   HTTP/1.1 200 OK
   Content-Type: application/json
   Link: <{next_href}>;rel="next"

   {
     "scans": [ {
       ...
       "details": {
         ...
         "results": [ {
           ...
           "vulnerabilities": "{href}",
           ...
         } ]
       }
     } ],
     "next": "dGhpcyBpcyBhIGN1cnNvciBmb3Igc2NhbnMK"
   }
   ```

   </details>

3. We'll dig around in the scan details to get the layer vulnerabilities URL and then:

   <details open>
     <summary>"List scan layer vulnerabilities" request</summary>

   ```text
   GET {href} HTTP/1.1
   Authorization: Bearer bmljZSB0cnkK
   X-Api-Version: 2018-05-01
   Accept: application/json
   ```

   </details>

   <details open>
     <summary>"List scan layer vulnerabilities" response</summary>

   ```text
   HTTP/1.1 200 OK
   Content-Type: application/json
   Link: <{next_href}>;rel="next"

   {
     "vulnerabilities": [ ... ],
     "next": "dGhpcyBpcyBhIGN1cnNvciBmb3IgdnVsbnMK"
   }
   ```

   </details>

4. We'll process the first page of vulnerabilities, then we'll check if the `Link rel="next"` header exists, and it does, so we'll use it to get the next page of results:

   <details open>
     <summary>"List scan layer vulnerabilities" request #2</summary>

   ```text
   GET {next_href} HTTP/1.1
   Authorization: Bearer bmljZSB0cnkK
   X-Api-Version: 2018-05-01
   Accept: application/json
   ```

   </details>

   <details open>
     <summary>"List scan layer vulnerabilities" response #2</summary>

   ```text
   HTTP/1.1 200 OK
   Content-Type: application/json

   {
     "vulnerabilities": [ ... ]
   }
   ```

   </details>

5. We'll process the page of vulnerabilities that we got back, and look to see if there's a `Link rel="next"` header, and there is not, so we'll go back to step 3 to find the next layer with vulnerabilities in it.

6. When we're done with layers, we'll practice good hygiene and terminate our session:

   <details open>
     <summary>"Delete session" request</summary>

   ```text
   DELETE {session_href} HTTP/1.1
   Authorization: bmljZSB0cnkK
   ```

   </details>

   <details open>
     <summary>"Delete session" response</summary>

   ```text
   HTTP/1.1 204 No Content
   ```

   </details>

Done!
