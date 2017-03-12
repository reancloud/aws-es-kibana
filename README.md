# AWS ES/Kibana Proxy

A proxy for signing requests to an [AWS Elasticsearch](https://aws.amazon.com/elasticsearch-service/) cluster with your IAM credentials.

This is the solution for accessing your cluster if you have [configured access policies](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html#es-createdomain-configure-access-policies) for your ES domain. This proxy service is particularly useful in scenarios where it is running on an EC2 instance whose instance role has been configured with the proper Elasticsearch access permissions.

## Usage

This proxy sits in between Kibana and your Elasticsearch service:

```
[ Kibana ] --> [ This Proxy ] --> [ AWS Elasticsearch ]
```

### Installation

Install the npm module

```bash
npm install -g git+https://github.com/reancloud/aws-es-kibana.git
```

### Examples

**Running running this proxy on an EC2 instance with an instance role that has already been granted the necessary access to the AWS Elasticsearch service:**

```bash
aws-es-kibana [AWS ELASTICSEARCH URL]
```

**Manually setting the access and secret access keys:**

```bash
export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXX
export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXX
aws-es-kibana [AWS ELASTICSEARCH URL]
```

**Using an AWS profile:**

```bash
AWS_PROFILE=myprofile aws-es-kibana [AWS ELASTICSEARCH URL]
```

**Using PM2**

(You can find more info on `pm2` [here](https://github.com/Unitech/pm2))

Specifying only an endpoint:

```bash
pm2 start /usr/bin/aws-es-kibana -- [AWS ELASTICSEARCH URL]
```

Binding to a different address and port:

```bash
pm2 start /usr/bin/aws-es-kibana -- -b 0.0.0.0 -p 9201 [AWS ELASTICSEARCH URL]
```

### Optional arguments

* `-b`, `--bind-address`:
    - _The ip address to bind to._
    - **Required:** False
    - **Default:** `127.0.0.1`
* `-p`, `--port`:
    - _The port to bind to._
    - **Required:** False
    - **Default:** `9200`
* `-r`, `--region`:
    - _The AWS region of the Elasticsearch cluster._
    - **Required:** False
    - **Default:** Determined via the `$REGION` environment variable, or parsed from Elasticsearch endpoint URL
* `-u`, `--user`:
    - _Set a basic auth username that will be required for all requests to the proxy._
    - **Required:** False
    - **Default:** Defaults to `$USER` if the flag is passed with no value, otherwise no username is required
* `-a`, `--password`:
    - _Set a basic auth password that will be required for all requests to the proxy._
    - **Required:** False
    - **Default:** Defaults to `$PASSWORD` if the flag is passed with no value, otherwise no password is required
* `-s`, `--silent`:
    - _Do not display figlet banner._
    - **Required:** False
    - **Default:** False; display the banner
* `o`, `--only-aggregates`:
    - _Queries with URLs matching the supplied regex must specify size: 0 or be rejected (HTTP 403)._
    - **Required:** False
    - **Default:** `""`
* `e`, `--exclude-pattern`:
    - _Queries with URLs NOT matching the supplied regex must specify size: 0 or be rejected (HTTP 403)._
    - **Required:** False
    - **Default:** `""`

## Credits

* This fork maintained by [@wilkystyle](https://github.com/wilkystyle)
* Forked from [santthosh/aws-es-kibana](https://github.com/santthosh/aws-es-kibana). Thanks [@santthosh](https://github.com/santthosh)
